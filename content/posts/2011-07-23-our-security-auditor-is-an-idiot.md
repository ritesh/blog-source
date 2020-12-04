---
date: 2011-07-23 10:34:00+00:00
slug: our-security-auditor-is-an-idiot
title: Our security auditor is an idiot
wordpress_id: 6
categories:
- audit
- security
---

  


> A security auditor for our servers has demanded the following within two weeks:  
> 
>  * A list of current usernames and plain-text passwords for all user accounts on all servers
>  * A list of all password changes for the past six months, again in plain-text
>  * A list of "every file added to the server from remote devices" in the past six months
>  * The public and private keys of any SSH keys
>  * An email sent to him every time a user changes their password, containing the plain text password
>  
>  We're running Red Hat Linux 5/6 and CentOS 5 boxes with LDAP authentication.  

>  As far as I'm aware, everything on that list is ether impossible or incredibly difficult to get, but if I don't provide this information we loose access to our payments platform, and any income we might have got while we move away. Any suggestions for how I can solve or fake this information?  
>    
>  The only way I can think to get all the plain text passwords, is to get everyone to reset their password and make a note of what they set it to. That doesn't solve the problem of the past six months of password changes, because I can't retroactively log that sort of stuff, the same goes for logging all the remote files.  
>    
>  Getting the public and private parts of the SSH keys is possible, but annoying as we have a few users with a few computers, all with their own SSH keys. Unless I've missed an easier way to do that?  
>    
>  I have explained to him many times the things he's asking for are impossible, he responded in an email:  
>    
>  I have over 10 years experience in security auditing and a full  
>  understanding of the redhat security methods, so I suggest you check  
>  your facts about what is and isn't possible. You say no company could  
>  possibly have this information but I have performed hundreds of audits  
>  where this information has been readily available. All [generic credit  
>  card processing provider] clients are required to conform with our new  
>  security policies and this audit is intended to ensure those policies  
>  have been implemented* correctly.


via [serverfault.com](http://serverfault.com/questions/293217/our-security-auditor-is-an-idiot-how-do-i-give-him-the-information-he-wants)

  
What the what? This is probably the funniest thing I've read in a while. How does this auditor still have a job?

