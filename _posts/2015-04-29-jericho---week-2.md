---
title: "Jericho - Week 2"
layout: post
---

> One step at a time.

While week 1 was mostly focused on defining cards and getting a basic card UI going, week 2 was more focused on the internals of a player base. Since this is where a large amount of the gameplay will be, I want to get started on it sooner rather than later, giving me time to iron out any design issues with it, and to ultimately work on the Funability™ of it.

Here's a quick rundown of the changes.

### Base Layouts

Most of the work for week 2 has been coding up the bases. Before allowing players to customize bases, I want to get all the code in place to assemble bases together. For now, I'll create a hard-coded base configuration and use that to generate a base that I can walk around in.

I also started implementing the "hallway" room. Right now, it's just a static room. I want to allow for some variations though. I'm either going to create multiple hallway prefabs, or create multiple portions of a hallway and stich them together at runtime.

{% include image.html img="/assets/images/week2/hallway.png" title="The beginning of the hallway" caption="The feng shui is a bit off." %}

### Card DB V2

It turns out [Unity Datablocks](http://unitydatablocks.com/) is pretty cool. I was already using ScriptableObjects to define my cards, but this makes it a much nicer experience. It has a pretty slick editor UI, and it makes it easy to inherit properties from parent datablocks. This means I can define a lot of properties at an abstract level, such as a "room card", and then define specific settings for specific rooms, such as a hallway. Nice!

{% include image.html img="/assets/images/week2/card_db.png" title="Card DB V2" caption="Card DB V2" %}
{% include image.html img="/assets/images/week2/card_db_2.png" title="Card DB V2" caption="Card DB Inspector" %}

You can also see I created some datablocks for PlayerSettings. I have a default settings, and I have an override for a "fast moving player". At runtime, I can swap out which settings I'm using. This has been great for tuning settings, as well as just switching to the "fast moving player" settings when I want to quickly run around my levels.

{% include image.html img="/assets/images/week2/fast.gif" title="Fast movement" caption="Easy switching to faster settings. I can jump super high for some dunks." %}

### Text Mesh Pro

The UI for the cards had some pretty horrible fonts. The default Unity text components really don't work too well for pixel perfect fonts.

So I picked up TextMesh Pro, and now my fonts are much more crisp! Well, except for this sign, which has some weird artifacts going on.

{% include image.html img="/assets/images/week2/tmp_sign.png" title="Sweet sign" caption="Sweet sign." %}

I also had a designer/marketing/UI friend tell me how horrible it is to use stylized pixel fonts for all text in a game. So I swapped some out for some OpenSans. Thanks for dumping on my pixels.

### Attack Swings

I just got started on some attack stuff. The placeholder character sprite really doesn't have the animations I want, and the attack animation is super weak looking.

Rather than using the animations it comes with (as shown in the Week 1 update), I am instead just going to show arc trails to simulate a sword swing. This means I can start tuning attacks according to the hitboxes I want, while still having a slight visual to go along with it.

{% include image.html img="/assets/images/week2/swing_anim.gif" title="My lousy arc trail" caption="My lousy arc trail." %}

{% include image.html img="/assets/images/week2/swing.png" title="My lousy arc trail" caption="A mid-swing screenshot of said lousy arc trail." %}

### Next Week

* Add entrances/exits to the hallway room.
* Generate our first base, likely just using connected hallways.
* Implement loading between rooms.
* Hopeful: Implement a three-attack combo system for the sword swings, and making attacks feel responsive.

{% include twitch.html %}
