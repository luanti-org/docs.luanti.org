---
title: Overview of Luanti forks
aliases:
  - /Overview_of_Luanti_forks
  - /Overview_of_Minetest_forks
  - /overview-of-luanti-forks
---

# Luanti forks

A software is said to be a **fork** if it is a derivation of a copy of another software; that is, if it is based on its source code and other related data. Luanti has seen some forks in its history. This article gives an overview of some of the known forks of Luanti.

## What is a Luanti fork?

As said, above, a fork is another copy of software that is edited. For Luanti, it is the same. Except a few mods put together is not called a fork, that would be a modpack. A fork is a Core engine change(s) that are not official and will not be released as a version of Luanti unless submitted and accepted in a pull request.

## List of projects

The following projects are known Luanti forks, which may or may not be active at the moment.

### Voxelands

**Voxelands** was started under the name “Minetest Classic” on the 15th of April 2013 by darkrose as a fork of the latest stable release of the 0.3 series of Minetest-c55 (“Minetest-c55” was the earlier name of Luanti). There was a later short-lived revival effort started in October 2022 by rudzik8, but neither project is currently maintained[^vl-1].

The fork was motivated by a dissatisfaction of Luanti becoming more and more a game engine rather than a game. Voxelands developers also claim that with the start of 0.4 series and the introduction of the [Lua modding API](/for-players/mods), performance was initially hurt, which was another key motivation for Voxelands. Voxelands is incompatible with Luanti and is quite different.

Key goals of Voxelands were keeping the game at least as performant as the Minetest-c55 0.3 series, adding new content, maintaining balanced gameplay with a focus of in-world functionality, backporting bugfixes and some features from the 0.4 series and maintaining backwards compatibility to the 0.3 series at the network protocol level.

[^vl-1]: The last commit on [rudzik8's fork](https://git.minetest.land/oerkki/voxelands) is dated April 6th, 2024; the last commit for [the original project](https://gitlab.com/voxelands/voxelands) is dated March 17th, 2018.


### Blocklife

[Blocklife](https://forum.luanti.org/viewtopic.php?f=14&t=12021) was first announced on the Luanti forum on Thu Apr 30, 2015 by gibucsoft. It started with only few noticeable changes to version 0.4.7 and came with a game that included some more mods than Minetest Game. The most interesting feature was the use of two hands that could each be used separately. On 23th May 2018 gibucsoft announced to once more start developing. This time the block size might get reduced to a tenth in side length. The goal would be to have a more realistic shaped “round” world. This fork is not currently maintained.

### Freeminer

**[Freeminer](http://freeminer.org/)** is a fork of Luanti maintained by proller, which was started by a group of developers in November 2013 as a result of a dissatisfaction with the direction Luanti and Minetest Game were taking at the time[^fm-1]. The project currently serves as proller's personal testing ground for experimental features, being semi-regularly rebased to stable upstream versions.

The project goals have been vaguely described as something along the lines of “To create a fun and playable game”. It is packaged with [a mod soup based on Minetest Game](https://github.com/freeminer/default) that includes mobs, armor and other minor additions.

[Freeminer is also available for Android](https://f-droid.org/packages/org.freeminer.freeminer/).

[^fm-1]: https://forum.luanti.org/viewtopic.php?p=120032#p120032

### Minetest-delta

[Minetest-delta](https://github.com/erlehmann/minetest-delta) was a fork of Luanti, maintained in mid-2011, with the goal of adding more experimental features to Luanti. Some contributions like papyrus, cacti and jungles have since been merged in Luanti.

### Minetest-M13

[Minetest-M13](https://minetestm13.weebly.com/) is an ancient fork. No activity recorded since 2012.

### Various forks of the Android version

There are a lot of Android forks. Sadly, there are a lot of low-quality proprietary Luanti forks. Many of them are known to have extremely annoying advertisements (which often make the game near-unplayable) or even in-app-purchases. **Those are not part of official Luanti!**

The _official_ Android versions can be found here:

- [Luanti on F-Droid](https://f-droid.org/packages/net.minetest.minetest/)
- [Luanti on Google Play](https://play.google.com/store/apps/details?id=net.minetest.minetest)

**Important: Ads, in-app purchases or other anti-features are not, and will never be part of Luanti!** If you see ads, double-check if you have _really_ installed Luanti, and not something else. Luanti is a community-driven free software project and we are way too proud of ourselves to ever need something like ads or in-app-purchases. :D

Many of these Luanti forks come and go rather quickly. Here are some known names in Google Play (because it changes so quickly, this list ls likely outdated and incomplete):

{{% comment %}} cspell:disable {{% /comment %}}

- Worldcraft: Exploration Lite
- WorldCraft 2 : Pocket Edition
- PixelCraft — 3D Survival!
- ► MultiCraft ― Free Miner!
- Crazy Craft on Castle World PE
- MultiCraft - Minetest France
- Cartoon Craft: Castle World PE
- Voxel Craft : Castle Build PE
- World Craft 3D
- Squeake Craft PLUS
  {{% comment %}} cspell:enable {{% /comment %}}

[Discussion thread](https://forum.luanti.org/viewtopic.php?f=3&t=16707).

Not much is known about these forks so far (feel free to edit this wiki page!). But we do know all these forks are illegitimate proprietary forks and/or they contain anti-features such as ads. For most of these, there's no source code provided, there's a proprietary license or no license at all. None of these problems exist in Luanti.

Note we are absolutely not against unofficial forks of Luanti and creating awesome new things with it (that's the whole point of being free software!). But for Android, we simply have not been impressed by the results so far. However, if you think you found a great Luanti fork for Android, please tell us in the [forums](https://forum.luanti.org/viewtopic.php?f=3&t=16707).

If you want to avoid proprietary forks with annoying anti-features altogether, we recommend you start to use [F-Droid](https://f-droid.org/) instead of Google Play. F-Droid software is a repository maintained by free software zealots like us in which fishy proprietary software is not allowed in the first place. :-)

### Various forks of the iOS version

Just like with Android, we also had people forking Luanti for iOS. The similar problems apply here.

Here's a list (we can't recommend any of these):

{{% comment %}} cspell:disable {{% /comment %}}

- [Buildcraft](https://itunes.apple.com/us/app/buildcraft-multiplayer-block-game/id740372768?mt=8)
- [Worldcraft 2](https://itunes.apple.com/us/app/worldcraft-survival-2/id900353132?mt=8)
- [Worldcraft Pocket Edition](https://itunes.apple.com/us/app/worldcraft-pocket-edition/id796349324?mt=8)
- [Exploration](https://itunes.apple.com/us/app/exploration-mind-world-of-craft-survival-game/id880642020?mt=8)
- FreeCraft (No iTunes Store link at the moment)

None of these are free software either. Buildcraft even forces you to perform an in-app purchases to make its ads go away.
{{% comment %}} cspell:enable {{% /comment %}}

[Discussion thread](https://forum.luanti.org/viewtopic.php?f=3&t=16707).
