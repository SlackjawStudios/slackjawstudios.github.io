---
title: "Jericho - Week 4"
layout: post
---

> Happy little slimes.

One month in! Given how little time I am able to spend on the project, I'm pleased with the progress so far.

Here's the quick rundown of changes made during week 4.

### Improved Jumping

All of the old code for player jumping has been removed, and it now uses a new system backed by a finite-state machine.

{% include image.html img="/assets/images/week4/jump_fsm_2.gif" title="Jump FSM" caption="Jumping is controlled by a FSM and can be visualized at runtime. It is built using NodeCanvas." %}

The *PlayerJumpGroundedState* is active when the player has collisions below.

The *PlayerJumpPoweringState* is active while the player is beginning to jump, and it continues to be active while the player is holding jump. This allows players to jump higher if holding the key longer. This state continuously applies jump force to the player, using a falloff curve based on how long the key has been held.

Once the jump key is up, or if enough time has passed, if moves to the *PlayerJumpingState*. This doesn't do much, but will eventually go back to the grounded state when hitting the floor.

{% include image.html img="/assets/images/week4/player_settings.png" title="Player settings" caption="Player settings are all in one place for easy tuning." %}

Most importantly, this new system makes it much easier to tweak settings and physics, so that I can begin fine-tuning how the player controls feel.

### Slime AI

The slimes now contain a [Behavior Tree](http://en.wikipedia.org/wiki/Behavior_Trees) to control basic decision making.

{% include image.html img="/assets/images/week4/slime_ai_tree_3.gif" title="Slime AI Tree" caption="The slime decision making process. It can be visualized at runtime. This is built using NodeCanvas." url="/assets/images/week4/slime_ai_tree_3.gif" %}

At the root, the tree uses a sequencer to make an initial decision. It puts priority on being hit, in which case the slime gets knocked back.

If not being hit, it will attempt to jump. This does several checks, such as being able to jump (based on a time interval), and if it is on the ground. If it is able to jump, it goes down the *Perform Jump* branch, which actually calculates and performs the jump.

If the jump branch fails at any point, the top-level will move on to the Idle branch. This one doesn't do anything at the moment, so the slime just hangs out and waits.

### UI and Sounds

When you get slightly burned out on writing code, it can be refreshing to work on more visual aspects.

While not final, I added some placeholder sounds to jumps and dashes, as the audio feedback is helpful for the player to understand what is happening.

{% include image.html img="/assets/images/week4/dash_cooldown_2.gif" title="Dash cooldown" caption="A basic UI for representing an ability cooldown. In this case, it shows the player's dash ability." %}

Some recent playtests showed that some players were frustrated with the time you have to wait between dashes. I don't necessarily want to lower the cooldown time, since I think it is in a good spot.

Instead, I believe the frustration is coming from mashing the dash key without any affect. To remedy this frustration, I added a visual cooldown icon that shows the status of the dash ability. Even though it isn't final, I kind of like the radial style.

{% include image.html img="/assets/images/week4/dash_cooldown.gif" title="Dash cooldown" caption="The cooldown indicator in action." %}

### Next Week

That's it for week 4. While it isn't as much progress as I would have liked, it certainly is moving in the right direction.

Here's what to expect next week:

1. Slime health, death, and spawning
1. Player health and death
1. Beginning of game rules and flow for the Wave Defense minigame.

{% include twitch.html %}
