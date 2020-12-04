---
author: admin
categories:
- linux
- sh
comments: true
date: 2011-12-04T11:24:19Z
slug: do-batch-things-well
summary: Some tips on doing batch things well. I will follow this up some day with
  my daily workflow which involves being as lazy as possible and letting the computer
  do all the work.
title: Do batch things well
url: /2011/12/04/do-batch-things-well/
---

I sometimes forget this really easy one liner for doing things in a batch on Linux/MacOS.

    
    
    # for file in `find . -type f`; do somescript.py $file; done
    


If you're relying on the output of find it is probably best to use the `xargs` option to `find` like so:

    
    
    # find . -type f -print0 | xargs -0 somescript.py
    


Even better, you can avoid using `xargs`, instead use the `exec` option to find. Although I think this is available only on newer versions of `find`.

    
    
    # find . -type f -exec somescript.py '{}' +
    


Sure, I could've written the batch file handling feature as part of my script but why waste my time when there are existing tools that already [do one thing well](http://en.wikipedia.org/wiki/Unix_philosophy#McIlroy:_A_Quarter_Century_of_Unix)?
