---
title: "Jericho - Week 8"
layout: post
---

> Waves upon waves.

We are one step closer to getting the demo going. The focus of week 8 has been on setting up game rules and game state management. This makes it play like an actual game, rather than a one-off test.

Here are the changes.

### Extra animations

Slimes have a few new animations to improve the general aesthetic.

{% include image.html img="/assets/images/week8/slime_death2.gif" title="Slime death" caption="Poor slime. He had so much ahead of him." %}

The player also received a death animation.

{% include image.html img="/assets/images/week8/player_death3.gif" title="Player death" caption="Time slows down when the player dies." %}


### Waves and spawners

The game management is comprised of three main pieces.

A **Spawner** are a basic object that know how to emit a number of enemies over a duration.

{% include image.html img="/assets/images/week8/spawner.gif" title="Spawner" caption="A quick spawner test using five slimes." %}

A **Wave Definition** is a mapping that tells which spawners to emit, what to emit, and how quickly.

{% include image.html img="/assets/images/week8/wave_definition.png" title="Wave Definition" caption="Pretty sore on the eyes, but it works." %}

The **Wave State Manager** is a finite-state machine that handles general game state. This makes it simple to separate concerns between game states.

{% include image.html img="/assets/images/week8/wave_fsm.png" title="Wave FSM" caption="The FSM for state management." %}

The *Show Next Wave State* is a good example of the separation of concerns. When the state is active, is simply shows the UI text for the wave number. It also chooses to transition the music depending on the upcoming wave. When done, it transitions off to the *Playing Wave State*. No other states need to know how to show the UI or manage music, it is totally isolated in its own area.

Similarly, the *Playing Wave State* monitors every *EnemyKilledMessage* and will trigger a check on whether the wave is done or not. When done, it transitions off to the *Wave Complete State*. This keeps everything nice and tidy.

### Victory surprise

I managed to add in a prize when completing the final wave. I won't show it here, so you'll have to beat the demo!

### Upgrade to Unity 5.1

The engine has been updated to 5.1, and all the resulting bugs have (hopefully) been squashed.

With Unity 5.1 comes the new networking library. Once the Jericho demo is out, I will likely switch over to the networking evaluation project alluded to in the [Week 7 post]({% post_url 2015-06-03-jericho---week-7 %}).

### Next week

Before the demo is ready, I plan to make the following changes within Week 9.

1. Health pickups found in chests.
1. Chest spawning logic.
1. Improve the UI to show the current wave number.
1. Begin creating the actual wave definitions to use in the demo.

Thanks for reading!
