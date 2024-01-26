---
title: "Operating Systems"
date: 2022-11-18T15:00:00+01:00
tags: ['os','userland','kernel','linux','windows','arch-linux','linux-mint','ubuntu','gentoo-linux','windows-xp','windows-7','windows-10','windows-11']
---

*WORK IN PROGRESS*

Your OS (operating system) is just like other programs you use, it just has different privileges, OS consists of kernel and userland, kernel can directly manage your computer's hardware and userland are basic applications you need for system to do anything, function. You (the user) use applications that talk to the operating system, which talks to the hardware.

## Linux
Linux is kernel but is also common name for Unix-like operating systems such as GNU/Linux which uses GNU utilities and is by far the most popular way of using Linux kernel, there is also busybox/Linux which uses lightweight busybox utils which are just a single binary file, popular OS designed for containers called Alpine Linux uses busybox and musl libc in an attempt to be as minimal as possible, Android operating system is heavily modified version of busybox/Linux. Notable Linux distributions are:
* **Arch Linux** - GNU/Linux, independent (not based on anything), uses systemd init system, but thanks to Artix Linux, which is a fork that makes it easier to use other init systems with Arch specific tools, you can use other init systems with ease, mostly used by experienced Linux users for its KISS (Keep it simple, stupid!) principle, base install doesn't come with DE, but you can install any DE or WM (window manager) you like through its package manager called pacman, AUR (Arch User Repository) or manually.
* **Ubuntu** - GNU/Linux, based on Debian, uses apt package manager, GNOME DE (desktop environment), systemd init system, made by Canonical Ltd, has several flavors that use different DEs like KDE, LXQT, XFCE and many more.
* **Linux Mint** - GNU/Linux, based on Ubuntu, uses Cinamon as DE but has XFCE flavour for older machines, it's a really great way to get into Linux, it has a Windows-like UI and is polished to give beginners the best out-of-the-box experience.
* **Gentoo Linux** - can be made into GNU/Linux, busybox/Linux or even sbase/Linux - you can use whatever userland you want, you can use OpenRC, Runit or systemd - you can use whatever init system you want, you can make Gentoo into anything you want, but it takes more time than with other Linux distributions, It uses a source-based package manager called Portage, which means that the package manager helps you get, manage and compile the source code of the packages you want to use, it doesn't have prebuilt binaries (but you can use tools like sisyphys which comes with Redcore Linux to download prebuilt binaries).

## Windows
Windows is by far the most popular family of desktop operating systems. The most iconic versions are:
* **Windows XP** - known for being lightweight.
* **Windows 7** - still used in many PC althogh being EOL (End Of Life), meaning it won't receive any updates.
* **Windows 10** - the most popular operating system - currently has a market share of around 71.27%.
* **Windows 11** - the latest version of Microsoft Windows.

