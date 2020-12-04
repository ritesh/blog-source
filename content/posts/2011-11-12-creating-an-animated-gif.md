---
author: admin
categories:
- linux
- photos
comments: true
date: 2011-11-12T13:32:09Z
slug: creating-an-animated-gif
title: Creating an Animated GIF
url: /2011/11/12/creating-an-animated-gif/
wordpress_id: 24
---

There are many ways to do this. You could use Windows Movie Maker, iMovie or professional movie editing applications. If you're looking for something quick and dirty from the command line, especially if you have all images in one directory and they don't require any post processing, [ImageMagick](http://imagemagick.org) can help.

ImageMagick is available for for Windows, Linux and MacOS X. The convert utility that comes with it is probably one of the most useful utilities I've ever used. Take for example, the task of converting a BMP image to JPEG.

    
    
    $ convert mypic001.bmp mypic001.jpg
    


Similarly, to convert a directory full of JPEG images to an animated GIF do this:


    
    
    $ convert -loop 0 -delay 100 -size 200x200 *.jpg animated.gif
    


Delay represents the time in milliseconds between each frame, and loop 0 ensures that the animation runs forever. The files in the directory are numbered incrementally, so they should be added in the correct order. Ensure that the dimensions of the resulting GIF are not too large, anything over 300x300 made my browser choke. For more information, check out the [documentation](http://www.imagemagick.org/Usage/anim_basics/).
