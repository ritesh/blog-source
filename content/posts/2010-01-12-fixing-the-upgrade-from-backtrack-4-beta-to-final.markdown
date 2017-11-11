---
date: 2010-01-12 12:09:00+00:00
slug: fixing-the-upgrade-from-backtrack-4-beta-to-final
title: Upgrade from Backtrack 4 Beta to final
wordpress_id: 32
---

Backtrack 4 was released today. I had just completed downloading the   
beta version only yesterday and finished installing it today. Since   
BT4 has now moved to Ubuntu, I'd assumed that upgrading it to the   
final would be easy as apt-get pie. I did and pretty much hosed KDE. A   
little research showed that KDE is now in a different path (`/opt/kde3`)   
than it was before (`/usr/kde3`).   

The quickest hack to fix this is to add `/usr/kde3` to the `PATH` line in   
your `.bashrc` . This still doesn't start KDE when you type startx   
however. I resorted to another (sigh) hack to make this run as I badly   
needed BT4 for work. echo "`exec startkde`" > `~/.xinitrc` fixes that   
issue.   
This is cheap hackery. I'm sure there are better ways to fix this.

