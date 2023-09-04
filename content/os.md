---
title: "Operating Systems"
date: 2022-11-18T15:00:00+01:00
tags: ['wip','os','linux','windows']
---

*WORK IN PROGRESS*

# Your OS (operating system) is just like other programs you use, it just has different privileges, OS consists of kernel and userland, kernel can directlly manage your computer's hardware and userland are basic applications you need for system to do anything, function. You (the user) use apps that talk to operating system that talks to hardware.

## Linux
### Linux is kernel but is also common name for Unix-like operating systems like GNU/Linux which uses GNU utilities and is by far the most popular way of using Linux kernel, there is also busybox/Linux which uses lightweight busybox utils that are just single binary file, popular OS designed for containers called Alpine Linux uses busybox and musl libc in attempt to be as minimal as possible, Android operating system is heavily modified version of busybox/Linux. Notable Linux distributions are:
* **Arch Linux - GNU/Linux, independent (not based on anything), uses systemd init system but thanks to Artix Linux which is fork that makes it easier to use other init systems with Arch specific tools you can use other init systems with ease, mostly used among experienced Linux users for its KISS (Keep it simple, stupid!) principle, base install doesn't come with DE but you can install any DE or WM (window manager) you like through its package manager called `pacman`, AUR (Arch User Repository) or manually.**
* **Ubuntu - GNU/Linux, based on Debian, uses `apt` package manager, GNOME DE (desktop enviroment), systemd init system, made by Canonical Ltd., has multiple flavors that use different DEs like KDE, LXQT, XFCE and many more.**
* **Linux Mint - GNU/Linux, based on Ubuntu, uses Cinamon as DE but has XFCE flavor for older machines, it's really great way to get into Linux, it has Windows like UI and is polished to give begginners best out of the box expirience.**
* **Gentoo Linux - Can be made GNU/Linux, busybox/Linux or even sbase/Linux - you can use whatever userland you want, can use OpenRC, Runit or systemd - you can use whatever init system you want, you can turn Gentoo into anything you want, but it take more time than with other Linux distributions, it uses source based package manager called `portage` meaning that the package manager asists you with getting, managing and compiling source code of packages you want to use, it doesn't have prebuild binaries (but you can use tools like `sisyphys` which comes with Redcore Linux to download prebuild binaries).**

## Windows
### Windows is by far the most popular desktop operating system's family. The most iconic versions are:
* **Windows XP - known for being lightweight.**
* **Windows 7 - still used in many PC althogh being EOL (End Of Life), meaning it won't receive any updates.**
* **Windows 10 - the most popular operating system, currently has about 71.27% market share.**
* **Windows 11 - the latest release of Microsoft Windows.**

