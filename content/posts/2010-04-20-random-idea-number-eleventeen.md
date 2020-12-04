---
date: 2010-04-20 13:48:00+00:00


slug: random-idea-number-eleventeen
title: Random idea number eleventeen
wordpress_id: 25
---

On a tech forum I visit often, we were discussing the various ways to   
power down a computer when power goes out (apart from the boring way   
of making your UPS automatically doing it for you). Now that I think   
about it, I'm really obsessed with shutting computers down. I had an   
idea long ago which was quite stupid but I'm gonna note it down anyway   
so that I can laugh about it later. Also, I want to ensure that   
Shampoo doesn't patent it. 

What if I could shut down my computer by making it listen to the   
annoying beeps that UPSes usually make? All the cheap ones don't have   
an option to turn that off, so I could probably use it as signal to   
power down. Considering that linux has finally implemented some sort   
of "sound framework" that doesn't (theoretically at least) lock sound   
devices when a process accesses it, it should be quite trivial to   
create a lightweight daemon that samples from the system microphone   
and checks for the annoying beep waveform. If it matches, kill the   
system. 

Two or three questions come to mind. The "Why do this?" can be   
answered with "whatevs, becoz i can". The other more pertinent   
questions are:   

* How difficult would this be to pull off?   
*  Wouldn't it be annoying to have a microphone connected to your   
system all the time?   
* Would it not be resource intensive to continuously poll the mic?   
* Are there any libraries which would make it easy for me to pull   
this off without resorting to hacks?   
* And finally, if i ran around with a beeping thingamajig, would it   
not be possible to start a shutdown? How can one protect against this? 

I hope to try it out this weekend, let's see if it can be done.