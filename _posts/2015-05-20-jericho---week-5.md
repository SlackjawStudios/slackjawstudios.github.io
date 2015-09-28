---
title: "Jericho - Week 5"
layout: post
---

> Getting real.

With week 5 coming to a close, Jericho is finally starting to feel playable. It's not *quite* there yet, but I can see the light.

You can see the roadmap for the project on the [Projects](/projects) page. Week 5 has brought Jericho much closer to the V0.1 release, and I expect to have a playable build ready for distribution by week 7.

Here is a peek at the progress made during week 5.

### Gameplay and Physics Tuning

I continued to refine the mechanics and control of the game, and the gameplay feels much more "sticky" now<sup>(That's a weird word for it.)</sup>

But sticky is a good thing! It means you can control the character how you want, move how you need, and stick to the targets you are attacking. Previously, everything felt paper-thin, and it was not responsive.

#### Attack Velocity Reduction

When attacking, the player slows velocity by 95%. This means you can hover while attacking midair or stop a dash suddenly, but you can no longer continuously attack while running.

{% include image.html img="/assets/images/week5/attack_velocity.gif" title="Attack velocity" caption="Hovering for a sweet air combo." %}

#### Combos and Responsive Attacks

The original intent of the combo system was to allow the player to chain attacks together in a satisfying manner. The problem was that enemies would get knocked back too easily.

Now, enemies will not be knocked back or go invincible for a certain number of frames after the attack. This makes them stunned in place, which provides a window for more attacks. If the enemy is not attacked during this window, then it will be knocked back and become invincible.

{% include image.html img="/assets/images/week5/slime_combo.gif" title="Slime combos" caption="A 3-hit combo before the slime is knocked back." %}

As shown in the GIF, enemies also now flash white when being attacked, Metal Slug style. This is done using a modified sprite shader, which may be the subject of an upcoming post.

### Health and Score HUD

The HUD received some additions, and it now shows the player's health and score. It is not really a final implementation, but it is a good temporary solution.

{% include image.html img="/assets/images/week5/health2.gif" title="Health bar" caption="The health bar goes down as the player takes damage." %}

Enemies use the MessageBus (see below) to publish events on death, and the ScoreKeeper listens to them to update the player's score.

{% include image.html img="/assets/images/week5/score.png" title="Score" caption="New high score." %}

### Splitting Slimes

I can't call this a game if big slimes don't split into smaller slimes. So now they do.

{% include image.html img="/assets/images/week5/slime_split.gif" title="Slime split" caption="How baby slimes are formed." %}

### Wizard Enemy

The second enemy type has been added, though it is still rough around the edges. This enemy, a flying wizard, will teleport around the arena. With each teleport, it will shoot a few fireballs at the player.

{% include image.html img="/assets/images/week5/wizard1.gif" title="Wizard" caption="The fireballs hurt quite a bit." %}

Hitting the wizard while it is attacking will cancel its attack routine, but the fireballs will do a lot of damage if you are hit.

{% include image.html img="/assets/images/week5/wizard2.gif" title="Wizard Death" caption="Sweet pixely death." %}

{% include image.html img="/assets/images/week5/teleportDirection2.gif" title="Teleport direction" caption="The wizard calculating where to teleport to." %}

### Super Jump

I'm not sure the Super Jump will stay, but it has produced some awesome moments of jumping high up to finish off a wizard.

{% include image.html img="/assets/images/week5/superjump1.gif" title="Super jump" caption="Now with 200% more dunks." %}

### Pause Menu

Work on the pause menu has started, but it is not functional yet. I don't really want to implement pause logic everywhere, so I may let the game simulation continue in the background. Pause at your own risk!

{% include image.html img="/assets/images/week5/menu2.gif" title="Pause menu" caption="Don't click the red button." %}

### Audio Overhaul

I have implemented [InAudio 2](https://www.assetstore.unity3d.com/en/#!/content/15609) into Jericho, and it is an awesome (and free!) asset.

It is currently being used to play music and all SFX. It also handles varying the SFX. For example, the sword swing receives a random pitch every time it plays.

{% include image.html img="/assets/images/week5/inaudio.png" title="InAudio" caption="Audio is stored as nodes. The RandomGenericHit node plays a random hit sound with a varying pitch." %}

### MessageBus

To prevent cluttering this update with boring code, the MessageBus details are covered in a separate post that you can find [here]({% post_url 2015-05-19-messagebus-for-unity3d %}).

The tl;dr of the MessageBus is that I now have a system in place that lets me loosely couple game logic, while still allowing different sections of the game to work together.

For example, an enemy will publish a message when it dies, and the ScoreKeeper may increment the player's score. Or the player will publish a message on death, and another subscriber may choose to restart the level.

{% include image.html img="/assets/images/week5/subscriber.png" title="MessageBus subscriber" caption="The score keeper subscribes to enemy death events." %}

Code architecture is very important to me, and the MessageBus allows for cleaner code.

### Next week

1. More combat responsiveness and juicyness.
1. Implement game rules, including wave spawners.

Thanks for reading!

{% include twitch.html %}
