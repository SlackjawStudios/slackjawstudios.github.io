---
title: "Jericho - Week 1"
layout: post
date: 2015-04-22 18:30
---

> Lions, and tigers, and infrastructure, oh my!

Quickly prototyping is a great practice. You know what else is a good practice? Taking time to do things right.

To help myself do things right, I am working to setup a solid base upon which to build upon. As other game studios have said:

> Before we built the game, we built the tools.

I don't want to reinvent every tool that's out there already, so I am making sure to reuse what I can.

To this end, I am working on getting the following pieces setup.

### Basic Character

Until I get around to making a character, I am using [this](https://www.assetstore.unity3d.com/en/#!/content/29304) character from the asset store.


{% include image.html img="/assets/images/week1/jump.gif" title="Jump!" header="Jump!" %}

{% include image.html img="/assets/images/week1/doublejump.gif" title="Double Jump!" header="Double Jump!" %}

{% include image.html img="/assets/images/week1/dash.gif" title="Dash!" header="Dash!" %}

{% include image.html img="/assets/images/week1/attack.gif" title="Attack!" header="Attack!" %}

I am also using [Rewired](https://www.assetstore.unity3d.com/en/#!/content/21676) for all of my controls, since using a controller would be pretty sweet.

### Dev Console

An awesome dev console for Unity is [UConsole](https://github.com/thebeardphantom/UConsole/) (which is a fork of Wenzil's [Unity Console](https://github.com/Wenzil/UnityConsole)). It makes it straightforward to have a dev console open up via the backtick (`) key, and to be able to register your own commands.

![UConsole](/assets/images/uconsole.png)

It even goes on to support modules in which to extend the console, along with the ability to select gameobjects while the console is open. This means you can select objects and run commands directly against those entities, such as renaming, destroying, or any custom command you create.

![UConsole Entity](/assets/images/uconsole_player.png)

### Universe

Maintaining universal manager objects in Unity can be a bit annoying. You typically need a startup scene that loads all your managers. So I took a stab at creating a Universe concept, inspired by [this](http://forum.unity3d.com/threads/a-different-way-to-make-globals-singletons-c.267060/#post-1764894) post on the Unity forums. You can see a writeup of my implementation in [this post]({% post_url 2015-04-22-universal-managers %}).

So now it's pretty easy to maintain my managers all across the project.

![Universe](/assets/images/universe_hierarchy.png)

### Card Database

One of the prerequisites I must complete is how I define cards. This will be needed for most parts of the game, so it seemed like a good thing to nail down early.

I contemplated defining cards via a JSON file that is loaded at startup. However, I ultimately went with a ScriptableObject, since it makes it easy to link to other assets, such as a Sprite to use as a card's image. [This](http://burgzergarcade.net/scriptableobjects-as-databases-in-unity/) resource was very helpful for getting a custom editor off the ground.

![Card DB](/assets/images/card_db.png)

I also setup most of the classes I need to define cards. With all of this setup, I am able to start coding against it to start generating bases, and eventually to have players build bases.

### Random outdoor tidbits

I don't really want to focus on the outdoor pieces quite yet, but I did want to get a sample scene together just to convey an idea.

Here's a basic outdoor scene, where the player can enter "base building" mode. This shows which cards the player has, and will eventually let them start placing cards onto their base. And might even not look so horrible someday.

Bonus points for helpful signs.

[![Sign](/assets/images/sign.gif "Sweet sign") ](/assets/images/sign.gif "Sweet sign")

### Next week

* Create classes for base configurations and base layouts
* Begin generating bases from configurations

{% include twitch.html %}
