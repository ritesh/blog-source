---
date: 2015-04-05T19:35:03Z
categories: [xubuntu, linux]
summary: Have a laptop that refuses to wake up after falling asleep? Try this!
title: Inhibiting Sleep on Xubuntu 14.04
url: /2015/04/05/inhibiting-sleep-on-xubuntu-14-04/
---

My work laptop ships with proprietary encryption software that encrypts the disk drive (a self-encrypting drive) when turned off or in sleep mode. This works perfectly fine on Windows 7, but having rid my laptop of Windows in favour of Xubuntu, I soon realised that a day-to-day Linux desktop is for masochists. 

When the computer is set to sleep mode, the disk drive locks and Xubuntu doesn't know how to unlock it when it wakes back up. Instead of the disk unlocking, I get a ton of ATA error messages about a missing disk, the laptop locks up and needs to be power cycled. To save me from myself, I removed the "Sleep" and "Hibernate" (can't remember if this was already disabled) options from the logout menu, but there was still a chance that I might close the lid and lose hours of work. 

To disable the lid-close event from doing anything, you can make the following changes to `/etc/systemd/logind.conf`. 

    HandleSuspendKey=ignore
    HandleHibernateKey=ignore
    HandleLidSwitch=ignore
    #Ignore ALL THE THINGS

This tip is courtesy StackOverflow user [John Törnblom](http://askubuntu.com/users/205327/john-t%C3%B6rnblom) and this answer for [Ubuntu 13.04](http://askubuntu.com/questions/362667/xubuntu-13-10-disabling-suspend-on-lid-being-closed). 

Apart from the die-when-wake-up-from-sleep issue, I can highly recommend Xubuntu 14.04 as a daily desktop operating system for work. Heck, even PulseAudio does the right things with Bluetooth speakers & headsets and works as you'd expect.



