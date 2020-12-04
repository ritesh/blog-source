---
author: admin
categories:
- linux
comments: true
date: 2012-09-15T21:36:28Z
slug: giving-up-on-native-linux
summary: In which I whine about Linux not working for me. Being a sucker for punishment,
  I'm typing this from a laptop running Linux. Stockholm syndrome, perhaps?
title: Giving up on Linux desktop
url: /2012/09/15/giving-up-on-native-linux/
---

I recently acquired a shiny new-ish desktop from a friend. Intel Core-i7 with 8 cores and a whopping 12 GB of RAM with a high-end ATI Radeon graphics card. It came with Windows 7 pre-installed and I decided to install Ubuntu 12.04 amd64 as a dual boot option. Initially, I had some trouble getting it to install on the inbuilt SSD (which should have been the first warning sign in retrospect). Having gotten it to install on the slower 500GB HDD, I was quite happy with it until I ran into the first few glitches. These issues were small to begin with but they irked me enough to get rid of Ubuntu altogether from the system.

  * The audio was glitchy. At first I thought this had something to do with Adobe Flash which has historically been rubbish on Linux and _especially_ on 64 bit Linux. I removed amd64 and replaced it with the 32 bits release instead. It turned out that audio was glitchy even for other applications (Rhythmbox, Spotify) regardless of the bitness. By glitchy I mean that it stuttered randomly if I so much as scrolled inside a browser window. As a responsible netizen, I've reported a bug but I don't expect it to be fixed anytime soon. 

	
  * Suspend to RAM was not available as an option. Suspend to disk worked sometimes but the computer often refused to "wake up" after going to sleep.

	
  * I tried to like Unity, I really did. It didn't really work for my workflow which involves running multiple terminals. There might be an easier way to smoothly switch between windows of the same application but I could not find it. OSX, for example, has this nice feature that lets you use number keys to switch between terminals (Cmd+1, Cmd+2, etc.).

	
  * Compiz was also glitchy for me, even with the proprietary fglrx drivers.



I love the power of having a bash shell at hand but using Linux as a full time desktop OS is _still_ a nightmare. I want to just boot my system and get shit done and I don't want to have to fiddle around with pulseaudio or any other crap. I have an older laptop running the old Ubuntu LTS just fine; I think that if you have old or fully supported hardware (mostly anything but ATI graphics), it _just works_. In any case, I suspect I'll probably try again when the next Ubuntu LTS release is out. Until then I am grudgingly obliged to run Ubuntu as a virtual machine on the Windows system. 

A quick tip to get rid of grub from within Windows 7. Use [MbrFix](http://www.sysint.no/nedlasting/mbrfix.htm). You might want to be careful though, the wrong command could wreck your hard disk(s). 


