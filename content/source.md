---
title: "Source Engine"
date: 2023-06-22T13:00:00+02:00
tags: ['source','gamedev']
---

## Source is a 3D game engine developed by Valve Corporation. Debuted in Counter-Strike in June 2004. Half-Life 2 followed shortly after Source and has been in active development ever since. Unusually for game engines, Source does not have a meaningful version numbering scheme. Instead, it will be developed in continuous incremental updates.
## Source is designed to support first-person shooters, but is also used professionally to develop role-playing games, side-scrolling games, puzzles, MMORPGs, top-down shooters, and real-time strategy games.

{{< center >}}
# Technology
{{< /center >}}
* Direct3D rendering on Microsoft Windows PC, Xbox and Xbox 360; OpenGL rendering on Mac OS X, Linux and PlayStation 3.
* High Dynamic Range Rendering (HDR).
* Lag-compensated client/server network model
* Network-aware and bandwidth-efficient physics engine (derived from Havok).
* Scalable multiprocessor support
* Precomputed radiosity lighting and dynamic shadow maps. Deferred lighting is supported on consoles.
* Facial animation system. Lip-syncing with the system is automatically generated and localizable.
* Mixed skeleton animation system including inverse kinematics.
* Water flow effect
* Dynamic 3D scars
* Alpha to coverage
* Map logic script using Squirrel.
* Access to source code that is important for mod teams
* Distributed map compiler
* Fabric simulation

{{< center >}}
# Modularity and notable upgrades
{{< /center >}}
### Source is designed to evolve incrementally as technology advances, unlike competitors' "version jumps" that prevent backward compatibility. Steam allows Valve to distribute automatic updates with new versions of the engine to large numbers of users. However, in practice there have been occasional breaks in this compatibility chain.
### Both Episode One and Orange Box introduced new versions of the engine that could not be used to run older games or his mods unless the developer upgraded the code and possibly the content. However, in both cases, version updates require significantly less effort than competing engines. This was demonstrated in 2010 when Valve updated all core source games to the latest engine version.
### Since the release of his Source in 2004, major architectural changes have been made, including:
#### High dynamic range rendering (2005, Day of Defeat: Source)
* Simulation of a camera aperture and the ability to fake the effects of brightness values beyond computer monitors' actual range. Required all of the game's shaders to be rewritten.
#### "Soft" particles (2007, The Orange Box)
* An artist-driven, threaded particle system replaced previously hard-coded effects.
#### Hardware facial animation (2007, The Orange Box)
* Hardware accelerated on modern video cards for "feature film and broadcast television" quality.
#### Multiprocessor support (2007, The Orange Box)
* A large code refactoring allowed the Source engine to take advantage of multiple CPU cores on the PC, Xbox 360 and PlayStation 3. On the PC, support was experimental and unstable until the release of Left 4 Dead. Multiprocessor support was later backported to Team Fortress 2 and Day of Defeat: Source.
#### Xbox 360 support (2007, The Orange Box)
* Valve created the Xbox 360 release of The Orange Box in-house, and support for the console, unlike support for the PlayStation 3, is fully integrated into the main engine codeline. It includes asset converters, cross-platform play and Xbox Live integration. Program code can be ported from PC to Xbox 360 simply by recompiling it.
#### PlayStation 3 support (2007, The Orange Box)
* Source first appeared on the PlayStation 3 in 2007, but with an engine port that was created externally and which was plagued with issues. Valve took the problem in-house for Portal 2, and in combination with Steamworks integration created what they called "the best console version of the game".
#### Mac OS X support (2010, multiple games)
* In April 2010 Valve released all of their major Source games on Mac OS X. All future Valve games will be released simultaneously for Windows and Mac. Games will only use Direct3D on Windows, and only OpenGL on the other platforms.
#### Linux support (2012, multiple games)
* The first of Valve's games to support Linux was Team Fortress 2, the port released in October 2012 along with the closed beta of the Linux version of Steam.

{{< center >}}
# Future technology
{{< /center >}}
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

{{< center >}}
# Criticism
{{< /center >}}
## Tool set
* Valve is currently creating a number of new tools.
* Source SDK tools have been criticized for being outdated and hard to use. Many tools, including those for texture and model compilation, require some level of text editor scripting by the user before they can be run on the command line. Sometimes with long console commands. Spearheading this silliness was at the University of London, where after a short stint at Source, the computer explored professional architectural visualization in his games. It's time to move on to the engine. A third party tool provides a GUI but is not supported by Valve.
* The user interface of Valve's Hammer Editor, the SDK's world-building tool, hasn't changed much since it was first released in 1998 for GoldSrc and the original Half-Life.

{{< center >}}
# Valve Developer Community
{{< /center >}}
### On June 28th, 2005, Valve opened a wiki. VDC has replaced Valve's static Source SDK documentation with a complete community site powered by MediaWiki. Within days, Valve reported that "the number of useful articles nearly doubled." These new articles cover previously undocumented Counter-Strike.
### Source Bot (added by bot author Mike Booth), Valve's NPC AI, advice to his mod team on setting up source control, and more articles. paper

{{< center >}}
# Papers
{{< /center >}}
### Valve employees occasionally contribute to various events and publications that discuss various aspects of Source's development, including SIGGRAPH, Game Developer Magazine and Game Developers Conference. They are aimed at experts and often discuss complex concepts.
