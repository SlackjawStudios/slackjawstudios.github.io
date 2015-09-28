---
title: "Jericho - Week 9"
layout: post
---

> Nothing but love.

One week closer to a demo.

As I started to design actual waves, I realized that the game is pretty tough. The player slowly accumulates damage, and it is difficult (and annoying) to try to complete all the waves. So this week was an attempt to fix that.

### Health pickups

Similar to gems, the player can now pick up hearts during gameplay. Doing so will heal the player, and bigger hearts restore more health.

{% include image.html img="/assets/images/week9/heart_sizes.png" title="Hearts" caption="Hearts come in all sorts of sizes." %}

I considered putting hearts in chests, or having enemies drop them. Ultimately, I wasn't fond of the idea for random chance. Instead, hearts will spawn before specific waves.

{% include image.html img="/assets/images/week9/hearts.gif" title="Hearts" caption="So refreshing." %}

The spawning animation could use some love (pun not intended<sup>but still awesome</sup>), but you get the idea.

### New wave definitions

Last week showed how I am defining waves, but it was pretty lame. It was placed directly on a MonoBehaviour, only because I was having difficulty making it work as a Datablock ScriptableObject.

With that fixed, waves are defined as actual assets with a slick new editor.

{% include image.html img="/assets/images/week9/wave_editor1.png" title="Wave editor" caption="Waves can be managed all in one place." %}

{% include image.html img="/assets/images/week9/wave_editor2.png" title="Wave editor" caption="Editing a wave is pretty easy." %}

### Code architecture

A lot of thinking went into code architecture this week. Compile times are ~15 seconds right now, which makes it pretty annoying when trying to make rapid changes.

I began refactoring much of the code, both my own and third-party code, to allow for building the code into DLLs. I plan to do a follow-up post in the future to show my exact workflow, so stay tuned.

### Next week

That's it for week 9. Look to week 10 for the following:

1. Chest spawning logic.
1. Improve the UI to show the current wave number.

Thanks for reading!
