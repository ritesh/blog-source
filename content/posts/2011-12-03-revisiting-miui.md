---
author: admin
categories:
- android
comments: true
date: 2011-12-03T11:28:59Z
slug: revisiting-miui
summary: Some thoughts on using the MIUI ROM for Android. I'm currently on Cyanogenmod
  and quite happy with it.
title: Revisiting MIUI
url: /2011/12/03/revisiting-miui/
---

After encountering an annoying bug on CyanogenMod, I decided to give MIUI another go. For some reason, my phone constantly vibrated in call, possibly due to an obscure proximity sensor setting. Having tried a number of ways to switch it off, I finally gave up and decided it was time to try something different. My SO got an iPhone 4s and I was very close to getting one for myself. I'm usually reluctant to switch Android ROMs as I've switched to using 2 factor authentication for my Google account and having to reset the authenticator etc is annoying.

The last time that I used MIUI I was happy for a few days until the random crashes started. These were usually triggered by the camera application, which was frustrating as I was travelling at the time and needed the camera more than the phone. However I gave in and decided to get the latest version of MIUI and see if things had improved. The original ROM is created by a Chinese company called xiaomi. It is available repackaged through a number of other distributors who usually add a language pack for other languages. There's even a Scottish distribution which I was tempted to try but wasn't too sure about.

I obtained a copy from miui.us and quickly realised that it ships only with Chinese and US English. Which isn't too much of a problem until the dialer started borking my phone numbers US style. A UK version is available from miui android, which worked perfectly for me. Some of the English words are a bit dodgy at times (machine translation?) but otherwise it's really well done. The MIUI ROM is smaller in size (about 88 MB compared to 100+ MB) than the CM7 ROM, which is strange because apart from being derived from CM7, it adds a few extras of its own. I suppose many of the applications that come with CM7 have been stripped out (Advanced Call Settings etc).

The first thing one notices after a fresh install is how the UI borrows from vendor customisations for Android like TouchWiz. It also feels like it's a lawsuit away from being decimated by a well known fruit company. The default font is different from what ships with AOSP or CyanogenMod, I'm not sure what it is but it looks great. Most of the applications are the same as on CyanogenMod with one glaring exception. The stock browser has been replaced with a custom 'MIUI browser' which offers tabbed browsing and ostensible speed improvements. I'm used to the stock browser and was a little disappointed with it and am planning to revert to the stock browser when I have the time (this is relatively annoying process). A 'MiTalk' application is provided which is probably similar to WhatsApp/Kik but exclusive to MIUI. I don't have any use for it and couldn't find an easy way to get rid of it as it is a system application.

The ROM ships with apps2sd built-in, which allows you to move applications and the dalvik cache to your SD card. This feature is really important for phones like mine (HTC Desire) which ship with a tiny partition for applications. I had an ext3 partition ready on my SD card from my previous install of CM7 and was able to move the dalvik cache to the SD card. There isn't a GUI option to do this but you can use a terminal emulator (or adb) to run apps2sd.

`# su - 
# a2sd
`
Another little useful feature was a birthday reminder notification. I'm not sure if this is available on CyanogenMod but it is quite useful in nagging you to wish people on their birthday or buy them gifts. The notifications show up in the top notification bar every day after midnight. Presumably this data is derived from the Google profiles of the people you have on your contacts list. 

After about 2 weeks of use, I can say I'm really happy with this ROM and feel that it works really well for me. I haven't run into any deal breaking issues or encountered any force-close bugs. If you're looking for a no-nonsense ROM I would still recommend CM7 as it is closest to the stock Android experience. I doubt there will ever be an Ice Cream Sandwich ROM for my phone so I'll stick to this until things change.

[gallery link="file"]
