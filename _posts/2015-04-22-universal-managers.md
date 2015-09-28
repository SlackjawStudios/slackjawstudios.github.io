---
title: "Universal Manager Prefabs"
layout: post
---

> A how-to on maintaining manager objects throughout a project.

Maintaining universal manager objects in Unity can be a bit annoying. You typically need a startup scene that loads all your managers. You also want to be able to jump into any scene, so you put your managers there. Now they're everywhere. You can't even combine them into a single prefab very easily, since nested prefabs are only scheduled to release between now and my death.

Inspired by [this](http://forum.unity3d.com/threads/a-different-way-to-make-globals-singletons-c.267060/#post-1764894) post on the Unity forums, it seemed like a solid way to handle managers. The idea is that you can mark certain objects as a "manager" object, and the Universe will ensure it is loaded. Even if you are jumping straight into a specific scene.

I went ahead and coded it up before I eventually found that LightStriker, who had that original post on the Unity forums, actually posted his Universe approach to the [Asset Store](https://www.assetstore.unity3d.com/en/#!/content/24227). I'll have to check that out soon to see how it compares to my approach.

The way I implemented the Universe concept in Jericho was initially to use an editor script that hooked into the editor's Play button. When entering play mode, the correct objects were spawned, so they were available before the game started. When stopping play, it would remove the objects. This worked well, except changing scenes would cause duplicate objects, since the objects were technically in the scene file (similar to loading your startup scene multiple times).

Now, I just load them at startup using a UniverseController. This means I do need to have a Universe prefab in each scene, but that's pretty easy compared to making sure I have every manager in every scene.

The UniverseController looks like the following:

{% highlight csharp %}
using System;
using UnityEngine;

namespace Jericho.Universe
{
  public class UniverseController : MonoBehaviour
  {
    private static UniverseController instance;

    private void OnEnable()
    {
      if (instance == null)
      {
        instance = this;
      }
      if (instance != null && instance != this)
      {
        // Instance was already loaded, and this is not it.
        //Debug.LogWarning("Instance already loaded!");
        DestroyImmediate(this.gameObject);
        return;
      }

      DontDestroyOnLoad(this.gameObject);

      UniverseObject[] objs = Resources.LoadAll<UniverseObject>("");

      Array.Sort(objs,
        delegate(UniverseObject x, UniverseObject y) { return y.Priority.CompareTo(x.Priority); });

        foreach (UniverseObject obj in objs)
        {
          GameObject child = GameObject.Instantiate(obj.gameObject);
          child.transform.parent = this.transform;
        }
      }
    }
  }
  {% endhighlight %}

When this script is loaded, it will look in the Resources folder for any prefab that has a UniverseObject component. Then, it loads all of the prefabs, ordered by their priority. Priority is quite helpful when you need certain manager objects to be available before others. For example, I want my EventSystem manager to be loaded before my Dev Console, since the Dev Console relies on it.

  The UniverseObject component is simple:

  {% highlight csharp %}
  using UnityEngine;

  namespace Jericho.Universe
  {
    public class UniverseObject : MonoBehaviour
    {
      public float Priority = 0f;
    }
  }
  {% endhighlight %}

And that's really all it takes. Pretty simple, but it has made it a lot easier to ensure managers exist across scenes, no matter where I start my game from.

{% include twitch.html %}
