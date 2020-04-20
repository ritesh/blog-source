---
date: 2008-07-09 11:13:00+00:00


slug: installing-truecrypt-on-fedora-9-howto
title: Installing TrueCrypt on Fedora 9 HOWTO
wordpress_id: 97
categories:
- fedora
- howto
- install
- truecrypt
---

On the 4th of July, the TrueCrypt team released version 6.0 of its amazing product. I am generally quite excited by new software releases and can't wait to try them out as soon as possible. From the web-site, some of the new features in the TrueCrypt version 6.0 are:

Parallelized encryption/decryption on multi-core processors (or multi-processor systems). Increase in encryption/decryption speed is directly proportional to the number of cores and/or processors.

Ability to create and run an encrypted hidden operating system whose existence is impossible to prove (provided that certain guidelines are followed). 

And many more [killer features](http://www.truecrypt.org/docs/?s=version-history)

I was disappointed to see that there were no ready-made packages available for Fedora 9 so I decided to compile from source. Here are the quick steps you could follow to install TrueCrypt on your Fedora Machine. I assume that you are familiar with TrueCrypt and its features, so I will not explain any of that. Be sure to read the [TrueCrypt documentation ](http://www.truecrypt.org/docs/)

**Obligatory Warning: I do not claim that this is the best way to go about installing TrueCrypt on your system and I am not liable for any damages caused **

With that out of the way here is what you need:

0. Ensure that your box is up to date  
  $ sudo yum -y update

1. Install fuse and development packages  
   $ sudo yum install fuse fuse-devel

2. Install gtk2 development libraries  
   $ sudo yum -y install gtk2-devel

3. Obtain a copy of the wxWidgets source version 2.8 from SourceForge  
   $ wget http://prdownloads.sourceforge.net/wxwindows/wxWidgets-2.8.7.tar.gz

4. Download the latest TrueCrypt source package from their [website ](http://www.truecrypt.org/downloads2.php)(As of writing this, it was version 6.0a)

5. Untar the wxWidgets source into a directory of your choice. I chose /usr/src   
  $ tar xzvf wxWidgets-2.8.7.tar.gz -C /usr/src

6. Untar the TrueCrypt package to the temporary directory  
  $ tar xzvf TrueCrypt 6.0a Source.tar.gz -C /tmp

7. Change to the truecrypt directory to build the application  
  $ cd /tmp/truecrypt-6.0a-source/  
  $ make WX_ROOT=/usr/src/wxWidgets-2.8.7 wxbuild ##change accordingly to point to the wxWidgets source  
  $ make

8. If all goes well, you should have a truecrypt binary in the Main folder in the truecrypt source folder (/tmp/truecrypt-6.0a-source/Main/truecrypt). Copy this to /usr/bin (As root of course)

9. The next part involves creating a group called truecrypt, adding users who are permitted to use truecrypt to it and ensuring that no password is required for them to run truecrypt. This is explained in a very user friendly manner by Oliver Meyer on howtoforge. Follow *ONLY* the first section of [this guide](http://www.howtoforge.com/encrypting-file-systems-with-truecrypt-on-fedora8) . Be sure to check out the other easy to follow and highly informative tutorials on [howtoforge](http://www.howtoforge.com/).

When I get a little more time, I'll clean this up and put it on howtoforge. I doubt that would be required though, since someone will eventually [package it for Fedora 9.](http://www.lfarkas.org/linux/packages/fedora/9/i386/)

   
Resources:

http://www.howtoforge.com/encrypting-file-systems-with-truecrypt-on-fedora8  
[http://meandubuntu.wordpress.com/2008/02/07/my-first-compile-truecrypt-50/](http://meandubuntu.wordpress.com/2008/02/07/my-first-compile-truecrypt-50/)

  
Hope this helps.
