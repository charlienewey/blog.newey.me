---
layout: post
title: Wake-on-LAN is Super Useful
date: 2015-05-28 00:39:56.000000000 +01:00
---

I was sitting in my kitchen this evening, working on my laptop and I suddenly
needed to access a particular file that resides on my desktop. Normally this
isn't an issue and I'll just get up, walk over to my desktop computer and boot
it up.

<!-- more -->

Today, however, I was struck by a sudden and desperate need to... not move
anywhere. And so, in the spirit of future laziness, I set about finding a way
to turn on my computer remotely. Wake-on-LAN seemed like the simplest option
(you can essentially tell your computer to boot up just by sending a "magic
packet" to the network card), and luckily my motherboard supports it.

Installing the `ethtool` Arch package was a doddle, and then it was simply a
case of editing a configuration file and creating a new systemd unit. Starting
my computer over the network is as easy as plugging my MAC address into the
`wakeonlan` tool. All I have to do now is configure and start an SSH service
and *hooray*, I can access my desktop computer remotely - even if it's switched
off to start!

If I want to share files, all I have to do is set up an SMB or NFS service and
mount that remotely. The best part is that using wake-on-LAN is more
environmentally friendly - now I can turn my desktop computer off without
having to worry about turning it on again :)
