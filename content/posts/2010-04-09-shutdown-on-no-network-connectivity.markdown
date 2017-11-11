---
date: 2010-04-09 06:50:00+00:00


slug: shutdown-on-no-network-connectivity
title: Shutdown on no network connectivity
wordpress_id: 26
categories:
- bash
- shutdown
---

``` 
#!/bin/bash  
#Lame attempt at automated shutdown during power outages  
#Author: Ritesh Sinha  
#Email: riteshkumarsinha # GMAIL.com  
#http://ritesh.posterous.com  
#License: WTFPL http://sam.zoy.org/wtfpl/COPYING  
#Checks for network connectivity, if none, wait for ten minutes then shutdown  
#TODO: Add email notification (email sent at next boot, requires MTA reconfiguration)  
#NOTETOSELF: Use gmail as smarthost?  
#TODO: Figure out a better way to pull this off  
  
ETHTOOL=/usr/sbin/ethtool  
IF=/dev/eth0  
GREP=/bin/grep  
AWK=/usr/bin/awk  
LOG=/var/log/outages  
SDOWN=/sbin/poweroff  
function checklink()  
{  
if [ "$($ETHTOOL eth0 | $GREP Link | $AWK {'print $3'})" == 'yes' ]  
then  
return 0   
else  
return 1  
fi  
}  
  
# call checklink  
echo "Checking if link is up"  
checklink  
if [ $? -eq 0 ]  
then  
echo "Link is up, all good"  
else  
echo "Link seems to be down"  
echo "Waiting for ten minutes before checking again"  
#Will put longer sleep after testing  
sleep 600  
checklink  
if [ $? -gt 0 ]  
then  
echo "Link is still down"  
echo "Shutting down"  
echo "SHUTDOWN:$(date)" >> $LOG  
$SDOWN  
fi  
fi  
  
```  
Bugs? Gosh, I hope not!