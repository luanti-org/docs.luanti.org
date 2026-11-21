---
title: LuantiEDU
aliases:
  - /MinetestEDU
  - /LuantiEDU
  - /luanti-edu
---

> [!NOTE]
> This page is a work-in-progress, and some detailed guides are unavailable.

![](/images/LuantiEDU-Logo.png)

The page is intended to provide information for teachers, youth and social workers of all kinds who want to use Luanti in an educational sense together with children and young people.

There should be information for the Luanti beginner, both for the teacher and the student. What possibilities Luanti offers and where it has limits. How to use Luanti on your own and extend it with mods. Or how to set up a server on the Internet and protect it from unwanted access. Particularly for the work with children, care is taken to ensure that the children can be adequately supervised in order to facilitate orderly cooperation.

Teachers who are willing to translate the texts into other languages can of course do so. It is also possible to create new pages in your own language, which can then be translated by others into their own language. As a basis, however, all information should also be available in English.

## General information about Luanti

Luanti is a free open world game engine for almost all operating systems. In randomly generated worlds consisting of blocks, the player can extract different resources, combine them with each other and shape the world according to his or her liking. Luanti is very similar to other sandbox computer games like Infiniminer and Minecraft. The game doesn't actually have a goal or an end, but the focus is on constructing buildings from blocks. However, it is also possible to construct a world in such a way that certain goals must be achieved, depending on what is desired. Technically, the Luanti engine is particularly focused on two goals. The game content should be easily modifiable and extensible by mods and should also work on older and new machines.

### Why Luanti and not Minecraft?

Minecraft is much better known and many children and teenagers are already playing it. So why use Luanti? Here are a few arguments:

**Contra:**

- Getting a server on the Internet is not as easy as it is for Minecraft. There are only providers for this and you have to administrate your own server by hand, because there are no graphical interfaces, as is the case with Minecraft.

**Pro:**

- It's free because it's open-source and it's going to stay open.
- It runs on almost all operating systems and devices ... _except iOS systems_.
- From all versions (also from the tablet) you can usually visit the same servers.

**Why switching from Minecraft is so easy:**

- What you can do in Minecraft is generally the same way you can do it in Luanti _(sometimes better or more simple)_.
- The operation is almost identical to Minecraft, so even changers shouldn't have any problems.
- There are tools to convert Minecraft worlds into Luanti worlds.

### Possible errors that might occur for Minecraft users

The biggest mistake you can make with Luanti is to consider only the naked system you get with the installation. Because Luanti is (almost) nothing without the extensions ([mods](/for-players/mods)) and games. The main developers are very reluctant to stick to fixed features. But extending Luanti is much easier than it is with Minecraft if you're not an absolute computer layman.

On this page we have therefore gathered information to help you learn about important enhancements that make Luanti a fully-fledged replacement for Minecraft. Surely it will take some time before a Luanti variant will be released, which will make it easy for teachers with little personal experience to manage larger numbers of students. The fact that there is no company behind Luanti will certainly have an impact. That's how community members develop what they like ... sometimes better than in Minecraft... but here and there there is a lack of details.

However, this collection of information here is intended to enable technically-interested teachers to use Luanti by assembling all necessary information. You are welcome to add your own information. Everyone is invited to attend.

## Download, Install, First Game

There are two ways to get a first impression of Luanti:

- you create a local singleplayer world and try and play around with it
- you visit one of the servers specified in the public server list

The first variation is probably better for beginners, as they can play alone in peace and quiet. The servers show examples of heavily expanded Luanti variants and are therefore especially suitable for those who already know Minecraft and have certain expectations.

We are working to expand this page with the following sections:

- First use of Luanti in a singleplayer world
  - How to have a special map for your game
  - Installation of a tutorial game to learn how to use Luanti
  - Navigating a Luanti world
  - How to use [texture packs](/for-players/texture-packs)
- Luanti [servers](/for-players/servers)
  - Short tutorial for students to visit a server
  - What is allowed and what is not ([privileges](/for-players/privileges))
- How to improve the Luanti performance

Luanti offers the possibility to simply create a local server that can be used with other users that are in the same LAN/WLAN. This is very useful for schools or if you meet with others in a foreign location.

- How to use Luanti as a local server

## Installation of Extensions (Mods)

Anything that Luanti doesn't seem to offer in comparison to Minecraft can be inserted into Luanti via an extension or modification, called a mod. It may seem difficult at first, but many mods are available. You just have to find the right mod. You can install a [mod](/for-players/mods) for a singleplayer world, or for all players on your [server](/for-players/servers).

### Types of Mods

Hundreds of mods are available via the official [ContentDB](https://content.luanti.org/), but anyone can create a mod and publish it anywhere. The effect of the mods may be classified as following:

- **User interface** changes or other "machines" _(like the oven)_ are offered which can replace the simple ones that are included.
- New **blocks** are made available for constructions.
- New **materials** _(rock types, minerals, etc.)_ are added during the creation of the world.
- When the world is created, more or other plants and **biomes** _(a local ecosystem with special dirt and plants)_ are created. Sometime you can use this new material to build blocks or items.
- New items like **tools** to dig, hoe, chop or new weapons are added.
- [**Mobs**](/for-players/mobs) _(self-acting creatures, in humans, animals or monster form)_ that can interact with the player are supplemented.
- Change the environment completely _(e.g. play on the planet Mars)_
- Prepared **buildings** will be added _(villages with inhabitants, abandoned mines, pyramids, ...)_

A lot of mods offer combinations of these possibilities.

## Setting up a Luanti server

If you want to work with a group of children it is a good idea to have your own server.

So far there are not yet any good providers where you can quickly order and set up a Luanti server. One reason for this is that the installation of mods is usually necessary and you have to access the remote computer. While Minecraft now has graphical user interfaces, Luanti requires everything to be done from the console.

- What kind of server do I need?
- Installation of Luanti on a Linux server
- Installation of a mod for the server
- Copy a singleplayer world to a server

If you have successfully installed the Luanti server and can also reach it with the local Luanti client, you should make some settings which are mainly about protecting the server from unauthorized access. This can be done not only by mods but also by settings in a special file.

- [`minetest.conf`](/for-players/minetest-conf)
- [Make the server visible in the public server list](/for-server-hosts)
- [Define the texture pack used on your server](/for-players/texture-packs)

If the server is set up correctly, it can start. If you share the server address or point to the server in the public server list, other users will come to the server and so it may happen that you have to intervene as an administrator. So here are some important commands and options you have as admin:

- Server commands for the admin
- Protect areas on the server
- Enable automatic transports on the map

Now the server is hopefully perfectly set up and all players and the admin are pleased. Surely there is always something to do, many of those things can be done when you are on the Luanti server yourself. But some things can only be done on the console and if you automate them, you can save yourself a lot of work.

- Automatic backup
- Automatic restart after the server crashed
- Start scripts to run different worlds with one installation
- Have several worlds on the same server running simultaneously
