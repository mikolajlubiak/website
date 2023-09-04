---
title: "Source Engine"
date: 2023-06-22T13:00:00+02:00
tags: ['source','gamedev']
---

# Source Engine

## Source is a 3D game engine developed by Valve Corporation. Debuted in Counter-Strike in June 2004.

### Half-Life 2 followed shortly after Source and has been in active development ever since. Unusually for game engines, Source does not have a meaningful version numbering scheme. Instead, it will be developed in continuous incremental updates.
### Source is designed to support first-person shooters, but is also used professionally to develop role-playing games, side-scrolling games, puzzles, MMORPGs, top-down shooters, and real-time strategy games.

# Outstanding technology
* Direct3D rendering on Microsoft Windows PC, Xbox and Xbox 360; OpenGL rendering on Mac OS X, Linux and PlayStation 3. High Dynamic Range Rendering (HDR).
* Delay-compensated client/server network model
* Network-aware and bandwidth-efficient physics engine (derived from Havok).
* Scalable multiprocessor support
* Precomputed radiosity lighting and dynamic shadow maps. Deferred lighting is supported on consoles.
* Facial animation system. Lip-syncing with the system is automatically generated and localizable.
* Mixed skeleton animation system including inverse kinematics.
* water flow effect
* dynamic 3D scars
* start report
* Map logic script using Squirrel.
* Access to source code that is important for mod teams
* distributed map compiler
* fabric simulation

# Modularity and Notable Upgrades
### Source is designed to evolve incrementally as technology advances, unlike competitors' "version jumps" that prevent backward compatibility. Steam allows Valve to distribute automatic updates with new versions of the engine to large numbers of users. However, in practice there have been occasional breaks in this compatibility chain.
### Both Episode One and Orange Box introduced new versions of the engine that could not be used to run older games or his mods unless the developer upgraded the code and possibly the content. However, in both cases, version updates require significantly less effort than competing engines. This was demonstrated in 2010 when Valve updated all core source games to the latest engine version.
### Since the release of his Source in 2004, major architectural changes have been made, including:
* High Dynamic Range Rendering (2005, Lost Day:
* sauce)
* Ability to simulate camera bezels and the effects of brightness levels beyond the actual range of a computer monitor. I had to rewrite all the shaders in the game.
* "Soft" Particles (2007, Orange Box)
* An artist-controlled threaded particle system replaces the previously hard-coded effects.
* Hardware Facial Animation (2007, The Orange Box)
* The latest graphics cards provide hardware acceleration for "movie and TV show" quality.
* Multiprocessor support (2007, The Orange Box)
* A major redesign of the code allowed the Source engine to take advantage of multiple CPU cores on his PC, Xbox 360 and PlayStation 3. On PC, support was experimental and unstable until Left 4 Dead was released. Multiprocessor support was later backported to Team Fortress 2 and Day of Defeat.
* sauce.
* Xbox 360 support (2007, The Orange Box)
* Valve is developing his Xbox 360 version of The Orange Box in-house, and console support is fully integrated into the main engine codeline, as opposed to PlayStation 3 support. This includes asset converters, cross-platform games, and Xbox Live integration. Code can be ported from PC to Xbox 360 by simply recompiling.
* PlayStation 3 support (2007, The Orange Box)
* Sauce first appeared on the PlayStation 3 in his 2007, but the engine ports were made external and had a lot of problems. Valve took the Portal 2 issue internally and combined it with Steamworks integration to create what they call "the best console version of the game."
* Mac OS X support (2010, multiple games)
* In April 2010, Valve released all major Source games on Mac OS X. All future Valve games will be released simultaneously on Windows and Mac. The game uses only Direct3D on Windows and OpenGL only on other platforms.

# Future technology

## Linux support
* On July 16, 2012, Valve announced that they were in the process of porting their Source Engine game Left 4 Dead 2 to Ubuntu Linux version 12.04. The promised port of Left 4 Dead 2 didn't make it to the closed beta and was instead replaced with a fully functional port of Team Fortress 2.

## New authoring tools
* One of Valve's biggest projects (as of May 2011) is developing a new content authoring tool for Source. They replace tools that are now outdated and enable faster and more efficient content creation. Studio head Gabe Newell said creating content with the engine's current toolset was "very tedious" and "slow".
* The in-process tools framework was created in 2007 and is currently used by the engine and Source Filmmaker (see below). source engine 2

## Source Engine 2
* In August 2012, Valve fansite Valve Time announced that Valve may be working on a "Source Engine 2" based on programming by Source Filmmaker, who drove the next version of the technology. In November, Gabe Newell confirmed that Source Engine 2 was in development and that Valve was "waiting for a game with it to come out."

## Image based rendering
* Image-based rendering technology was in development for Half-Life 2, but was removed from the engine prior to release. This was mentioned again by Gabe Newell in 2006 as a technology he wanted to add to Source to implement support for scenes much larger than would be possible with strictly polygonal objects.

## Origin
* The source has been stripped from his GoldSrc engine, which itself is a heavily modified version by John D. Carmack Quake engine.

# Criticism
## Tool set
* Valve is currently creating a number of new tools.
* Source SDK tools have been criticized for being outdated and hard to use. Many tools, including those for texture and model compilation, require some level of text editor scripting by the user before they can be run on the command line. Sometimes with long console commands. Spearheading this silliness was at the University of London, where after a short stint at Source, the computer explored professional architectural visualization in his games. It's time to move on to the engine. A third party tool provides a GUI but is not supported by Valve.
* The user interface of Valve's Hammer Editor, the SDK's world-building tool, hasn't changed much since it was first released in 1998 for GoldSrc and the original Half-Life.

# Valve Developer Community
### On June 28th, 2005, Valve opened a wiki. VDC has replaced Valve's static Source SDK documentation with a complete community site powered by MediaWiki. Within days, Valve reported that "the number of useful articles nearly doubled." These new articles cover previously undocumented Counter-Strike.
### Source Bot (added by bot author Mike Booth), Valve's NPC AI, advice to his mod team on setting up source control, and more articles. paper

# Papers
### Valve employees occasionally contribute to various events and publications that discuss various aspects of Source's development, including SIGGRAPH, Game Developer Magazine and Game Developers Conference. They are aimed at experts and often discuss complex concepts.
