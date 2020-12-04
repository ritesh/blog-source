---
author: admin
categories:
- linux
comments: true
date: 2012-05-05T15:53:46Z
slug: prevent-accidental-rm
summary: Pro-tip to prevent accidently hosing your home directory. If you're too smart
  for your own good, this may not be sufficient.
title: Prevent accidental rm
url: /2012/05/05/prevent-accidental-rm/
---

If you're anything like me, you might casually type `rm -rf *` in your home directory when you really meant to delete the contents of another directory. You should, of course, always have a backup of the contents of your home directory just in case this happens. 

There is simple hack to force `rm` to prompt you to think about the files that are going to be deleted. Create a file called '-i' in the directory whose contents you want to protect from accidental deletion.

An easy way to do this is to run `touch ./-i ` which gives you:

    
    
    ritesh@viridian:/var/tmp/aa$ ls -a
    -i      .       ..      100.txt 200.txt l.txt   seq.txt
    


If you now try and run rm -rf * now, this is what happens instead of everything getting nuked all at once.

    
    
    ritesh@viridian:/var/tmp/aa$ rm -rf *
    remove 100.txt? 
    


I've tried this on bash, I'm not sure if this works with other shells (zsh, ksh). Sure, you could always remember to use ` rm -rfi ` but who can remember that whole extra letter? 


