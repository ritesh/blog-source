+++
date = "2017-03-07T22:38:48Z"
title = "Header files voodoo"
+++
Trying to build a Qt project on Linux today. Turns out, (some) Qt programmers are case insensitive when it comes to including heAdEr fiLEs in C++ source. This is fine on Windows and macOS, but on an OS with a case sensitive filesystem like `ext4`, this causes a whole lot of problems. `gcc` and `g++` try and fail to find `coolHeader.h` if it's called `CoolHeader.h`. I'm sure that this isn't an uncommon problem, but I couldn't find a solution that didn't involve serious `sed`-fu.


There's an easier hack, if you're not ready to use lowercase header filenames in your C++ code like Bjarne Stroustrup probably intended. Create an empty file large enough to contain both your source and build artifacts, format it as a FAT32 volume and mount it as a loop device. Use this loop device to build your code.

```
dd if=/dev/zero of=/fakedisk bs=1024 count=200000 
losetup /dev/loop0 /fakedisk 
mkfs.vfat /dev/loop0 # Make it a VFAT filesystem
mkdir /mnt/faker
mount -t vfat /dev/loop0 /mnt/faker
cp -Rv ~/your/QtCode /mnt/faker/
cd /mnt/faker/QtCode/ && qmake -makefile
make # Job done, off to the pub 
```
But hey, if `sed` works to refactor your codebase, more power to you!
