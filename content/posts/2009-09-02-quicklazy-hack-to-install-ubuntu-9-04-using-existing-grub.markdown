---
date: 2009-09-02 18:23:00+00:00
slug: quicklazy-hack-to-install-ubuntu-9-04-using-existing-grub
title: Quick/Lazy hack to install Ubuntu 9.04 using existing GRUB
wordpress_id: 64
categories:
- grub
- lazy
- ubuntu
---

Prior to using any Debian derivatives, I was a RedHat fan and hence   
heavily favoured Fedora/CentOS/RHEL over other distributions. Over the   
last few releases Fedora has been a disappointment and I was stuck   
between using Gentoo/Ubuntu. Hell I even gave FreeBSD a shot (sucked   
for me). Gentoo was my favoured distribution about 2-3 years ago but   
the excessive kernel recompilation was really getting to me. To go the   
more _stable_ path I decided to give Debian a shot. Debian lenny is an   
amazing release, except it sucked as a desktop OS for me. Random sound   
issues, flash taking the whole system down and poor 3D acceleration   
really ruined it for me (couldn't play quake live even with   
proprietary drivers). 

I decided to finally switch to Ubuntu Jaunty (9.04). Being too lazy to   
connect an optical drive or buying blank CDs, I decided to install it   
using my existing GRUB setup (Debian). Turns out it's easier than   
Lindsay Lohan excessively inebriated. 

 A few preliminary notes: This has been tested using the Jaunty mini   
iso, not sure if other versions work as designed. Did not try   
installing using the wireless network (that would be stupid). Need a   
decent broadband connection (say 512kbps upward) for the installation   
to finish within a reasonable time (leave it overnight only after   
you're done selecting packages). Will hose your system if power goes   
out and you've crossed the partitioning stage. Take like a backup,   
man. 

 Step 1: Get the mini.iso from here :   
[https://help.ubuntu.com/community/Installation/MinimalCD](https://help.ubuntu.com/community/Installation/MinimalCD)   
Step 2: Extract the mini.iso to get two files, linux and initrd.gz   
Step 3: Rename those files for easy reference (eg linux_ubuntu &   
initrd_ubuntu.gz)   
Step 4: Move them to your boot directory   
  #sudo mv linux_ubuntu initrd_ubuntu.gz /boot/   
Step 5: Edit /boot/grub/menu.lst using your favourite editor and add   
the lines to point to these   
```
  title		Ubuntu 9.04 installer   
  root (hd0,0)   
  kernel		/linux_ubuntu   
  initrd /initrd_ubuntu.gz
``` 

 Remember the above setting should be _exactly_ the same as the other   
lines on your existing linux install (If your current linux uses UUIDs   
then the root line would be replaced with a suitable UUID, the "root"   
parameter might work but I'm not sure) except for the last two lines   
(kernel and initrd) 

 Step 6: Reboot select Ubuntu installer, proceed as usual, select your   
mirror, repartition your disks (even the partition you booted from can   
be destroyed; is that cool or what?) and let the installer do its   
thing. 

 I live in an area where powercuts are fairly common (even after a   
lying politician promised there would be no more), so i went with the   
minimal install during the initial setup (so that I'd have something   
to fall back on as soon as possible) . After this I was able to reboot   
to a desktop-less OS. Doing an `sudo apt-get install ubuntu-desktop`   
got the latest version of GNOME and associated stuff fresh from the   
repositories. Jaunty works great for me, even though I'm afflicted   
with an ATI card (on board) and a faulty USB controller (disabled,   
using a PCI addon USB card instead). 

Time taken to do the initial install: Approx an hour (including download time)   
Time taken to download the ubuntu-desktop packages: About 1 hr 30 minutes   
Time taken to install/configure the desktop package: About 15 minutes 

 My broadband connection is allegedly 1Mbps (ADSL) but it usually hangs   
somewhere between 512 and 768 and peaks at 1Mbps sometimes. 

 Hope this helps someone.
