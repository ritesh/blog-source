---
date: 2011-07-16 12:38:00+00:00
slug: installing-rubygems-in-your-home-folder
title: Installing rubygems in your home folder
wordpress_id: 7
categories:
- programming
- ruby
---

No root? No problem! Obtain a copy of rubygems from ruby forge.   
```   
$ tar xzvf rubygems-1.4.2   
$ cd rubygems-1.4.2   
$ ruby setup.rb --prefix=/your/home/folder/local   
$ echo "RUBYLIB=$HOME:/same/path/as/above/lib" >> ~/.bash_profile   
$ echo "export RUBYLIB" >> ~/.bash_profile   
$ source ~/.bash_profile   
```

This should give you a working install of rubygems. Check out the environment   
```   
$ gem env   
```

Install something   
```   
$ gem install rake   
```