---
title: "Jericho - Week 3"
layout: post
---

> Prepare for combat.

So, last week I listed some things that I'd do the next week. And I did pretty much none of them.

That's not to say I didn't do anything. I switched gears a little bit based on some recommendations from others who have played the prototype, and I am now prioritizing making the character controller "feel right" before I go HAM on the actual base building and card system.

### Training Room

Enter the training room.

This is intended to be a small prototype within the larger game, and it focuses entirely on improving character controls. It is to be used as a test bed for tuning physics and combat.

{% include image.html img="/assets/images/week3/slimes_idle.gif" title="Slimes!" caption="Slimes!" %}

The goal will be to kill as many slimes as you can, while more slimes keep spawning. This allows me to run tests with different settings, and to measure the following:

1. How far are people able to get?
1. What comments do they have on how things "feel"?
1. What can I do to make combat fun?

{% include image.html img="/assets/images/week3/attacking2.gif" title="Attacking slimes" caption="Some initial hit detection on slimes." %}

This demo will also be a small enough slice of the game to consider distributing it for a wider playtest. After I get it working, of course...

### Character Controller

The focus to make character controls feel right have been on getting proper move speeds and jump height. It's not quite there yet, but feeling okay so far.

{% include image.html img="/assets/images/week3/jump_higher.gif" title="Hold to jump higher" caption="You can hold Jump to jump higher." %}

The character controller does not use Unity's physics. It instead uses basic raycasting and velocity calculations.

{% include image.html img="/assets/images/week3/no_slide.gif" title="No slide" caption="Entities don't slide down slopes like a RigidBody would." %}

The player uses a PlayerMovement component to calculate velocity based on gravity and input, and then tells the 2D Controller to move.

The slimes also use the same 2D Controller setup. Rather than having a PlayerMovement component, it uses a 2D Motor component. This motor component implements generic gravity and velocity, and allows other components to update it. This is quite similar to a RigidBody2D, but it does so with an oversimplification on physics.

### Three-Attack Combo

Combos are fun. Well, usually. Mine aren't.

{% include image.html img="/assets/images/week3/combo2.gif" title="Combo" caption="Downwards, upwards, and jab attacks." %}

I'm not quite happy with how the combo system turned out, but I think the direction could work. The idea is to prevent spamming attack, and to ultimately make them feel more responsive. Rapid attacks will yield a combo, while waiting between attacks will only use the first attack.

{% include image.html img="/assets/images/week3/combo_sprites.png" title="Combo sprites" caption="Temporary sprites for attack hitboxes." %}

### FPS Graph

Lastly, I am using [FPS Graph - Performance Analyzer](https://www.assetstore.unity3d.com/en/#!/content/6513) from the Asset Store to show my FPS in-game. While I don't have any FPS issues now, I feel it will be helpful in the long run.

{% include image.html img="/assets/images/week3/fps_graph.gif" title="FPS Graph" caption="You can toggle the FPS chart by pressing F1." %}

### Next Week

* Further tweaks on the character controller. Specifically around dash velocity and jump height.
* Basic animations for enemy damage and death.
* Slime AI.

{% include twitch.html %}
