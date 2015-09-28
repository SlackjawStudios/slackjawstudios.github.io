---
title: "MessageBus for Unity3D"
layout: post
useExcerpt: true
---

Code architecture is a funny thing in game development. On one hand, you want to just get things working, and it is best to just crank out code while optimizing later. On the other hand, you want to write clean code, so that iterative development is easier.

There are many packages in the Unity3D world that address code architecture, though they are mostly MVC-oriented. For example, [StrangeIOC](http://strangeioc.github.io/strangeioc/) and [Zenject](https://github.com/modesttree/Zenject) are pretty awesome. However, having used both, I feel they are a bit heavy on... everything.

While I don't think MVC fits into Unity3D, I do fully support loose coupling of components. This post is an overview of a system being used in my game development, which allows for easy messaging between systems.

### Why do it?

Consider a scoring system in which the player can kill enemies, and doing so will increment the player's score.

Great. So we update our Enemy.cs component, and have it increment the user's score. Something like this:

{% highlight csharp %}

void OnDeath()
{
  GameObject.FindObjectOfType<ScoreKeeper>().Score += 1;
}

{% endhighlight %}

Next, we want to spawn an additional enemy to replace this one. So let's do that.

{% highlight csharp %}

void OnDeath()
{
  GameObject.FindObjectOfType<ScoreKeeper>().Score += 1;
  GameObject.FindObjectOfType<Spawner>().SpawnAnother();
}

{% endhighlight %}

Unfortunately, we are now tied to the concept of score keeping and spawning logic. Ideally, our enemy just handles its own death sequence, and other systems can choose to react to it.

So let's instead change our script to be more like this:

{% highlight csharp %}

void OnDeath()
{
  GlobalMessageBus.Instance.Publish(new EnemyDeathMessage(this));
}

{% endhighlight %}

Great! All we do is fire an event, and not worry about the details beyond it. Elsewhere, systems can subscribe to these events and choose what to do. This makes it much easier to separate code, yielding faster development times, fewer bugs, and more flexibility when changing game logic.

### Goals

For our MessageBus, we will set the following goals:

1. It should be type-safe. We don't want to use something flimsy, like strings, to define what to listen to.
1. It should allow arbitrary parameters to be sent with messages.
1. It should be able to clean up subscribers that have been destroyed.

### MessageBus implementation

This MessageBus implementation is heavily based on the [Event Aggregator](http://www.codeproject.com/Articles/812461/Event-Aggregator-Pattern) project, but is has been adapted slightly for Unity.

First, let's define what a MessageBus can do via an interface.

{% highlight csharp %}

using System;

namespace MessageBus
{
  public interface IMessageBus
  {
    void PublishEvent<TEventType>(TEventType evt);
    void Subscribe(Object subscriber);
  }
}

{% endhighlight %}

Our MessageBus interface allows us to either publish an event, or to register a subscriber.

Notice that the Subscribe call does not say what to subscribe to. Instead, we will use the object itself to find which events it has listeners for. Before the witty remarks come in, the answer is yes. This will use reflection. I have yet to add caching to the reflection calls, but I will not do so until performance shows up as an issue. After all, it is only done when registering subscribers, not when publishing or consuming messages.

Next, let's define an interface for what a subscriber will look like.

{% highlight csharp %}
namespace MessageBus
{
  public interface ISubscriber<TEventType>
  {
    void OnEvent(TEventType evt);
  }
}
{% endhighlight %}

Simple enough. We will use generics to provide the message type we want to listen on, providing us with type-safety.

For the actual MessageBus itself, we will look for any implemented ISubscriber<> classes on the registered subscriber object, and we will route messages to that.

Next up, we will implement our MessageBus.

{% highlight csharp %}

using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;

namespace MessageBus
{
  public class MessageBus : IMessageBus
  {
    private readonly Dictionary<Type, List<WeakReference>> _subscribers =
    new Dictionary<Type, List<WeakReference>>();

    private readonly object _lockObj = new object();

    public void PublishEvent<TEventType>(TEventType evt)
    {
      var subscriberType = typeof (ISubscriber<>).MakeGenericType(typeof (TEventType));

      var subscribers = GetSubscriberList(subscriberType);

      List<WeakReference> subsToRemove = new List<WeakReference>();

      foreach (var weakSubscriber in subscribers)
      {
        if (weakSubscriber.IsAlive)
        {
          var subscriber = (ISubscriber<TEventType>)weakSubscriber.Target;
          InvokeSubscriberEvent(evt, subscriber);
        }
        else
        {
          subsToRemove.Add(weakSubscriber);
        }
      }

      // Remove any dead subscribers.
      if (subsToRemove.Count > 0)
      {
        lock (_lockObj)
        {
          foreach (var remove in subsToRemove)
          {
            subscribers.Remove(remove);
          }
        }
      }
    }

    public void Subscribe(object subscriber)
    {
      lock (_lockObj)
      {
        var subscriberTypes =
        subscriber.GetType()
        .GetInterfaces()
        .Where(i => i.IsGenericType && i.GetGenericTypeDefinition() == typeof (ISubscriber<>));

        WeakReference weakRef = new WeakReference(subscriber);

        foreach (var subscriberType in subscriberTypes)
        {
          List<WeakReference> subscribers = GetSubscriberList(subscriberType);
          subscribers.Add(weakRef);
        }
      }
    }

    private void InvokeSubscriberEvent<TEventType>(TEventType evt, ISubscriber<TEventType> subscriber)
    {
      subscriber.OnEvent(evt);
    }

    private List<WeakReference> GetSubscriberList(Type subscriberType)
    {
      List<WeakReference> subscribersList = null;

      lock (_lockObj)
      {
        bool found = _subscribers.TryGetValue(subscriberType, out subscribersList);

        if (!found)
        {
          // Create the list.
          subscribersList = new List<WeakReference>();
          _subscribers.Add(subscriberType, subscribersList);
        }
      }

      return subscribersList;
    }
  }
}

{% endhighlight %}

That's it! Now all we need is some way to access the message bus from the rest of our code. This can be done lots of different ways. One such way may be to use a Singleton:

{% highlight csharp %}
public class GlobalMessageBus
{
  private static MessageBus _instance;

  public static MessageBus Instance
  {
    get { return _instance ?? (_instance = new MessageBus()); }
  }
}
{% endhighlight %}

Whenever we want to send a message, we can simply do:

{% highlight csharp %}
GlobalMessageBus.Instance.PublishEvent(new EnemyKilledEvent(enemyThatWasKilled));
{% endhighlight %}

Our event class (in this case, EnemyKilledEvent) is just a regular class. It doesn't need to implement any special interface. Additionally, it can provide whatever fields it needs. In the above example, we pass in the enemy that was killed, so that a subscriber can use it.

I hope this MessageBus implementation helps. If time allows, I may upload a unityPackage file that contains the full source.
