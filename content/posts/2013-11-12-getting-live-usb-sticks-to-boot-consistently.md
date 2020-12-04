---
author: admin
categories:
- linux
comments: true
date: 2013-11-12T20:53:18Z
slug: getting-live-usb-sticks-to-boot-consistently
summary: Pro-tip for booting Linux via a USB stick for computers that don't allow
  native USB booting
title: Getting Live USB sticks to boot consistently
url: /2013/11/12/getting-live-usb-sticks-to-boot-consistently/
---

Occasionally I like to boot my desktop running Windows from a USB stick to try out the latest version of a Linux distribution on hardware -- as opposed to running it in VirtualBox. I remember that this used to be straightforward on my old Acer laptop, but it's hit and miss on my desktop. I've not quite figured out why boot-from-USB fails occasionally, but I've found great way around it.

Enter the [plop boot manager](http://www.plop.at/en/bootmanager/index.html). Install this nifty little tool and it gives you a boot menu before windows starts up. You can now select your USB stick and it should boot, provided that unetbootin or a similar tool did it's job.
