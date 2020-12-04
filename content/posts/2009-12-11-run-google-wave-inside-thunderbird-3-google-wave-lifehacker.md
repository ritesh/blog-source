---
date: 2009-12-11 13:22:00+00:00
slug: run-google-wave-inside-thunderbird-3-google-wave-lifehacker
title: Run Google Wave Inside Thunderbird 3 
wordpress_id: 41
categories:
- google
- thunderbird
- wave
---

  
     

> Google's Wave tool suffers from a "just another inbox" problem. So it's great to see Wave running inside a tab in Thunderbird, right next to your email inbox, with a single JavaScript line.
> 
>   
	  
							
> 
> Among Thunderbird 3's new features is [Content Tabs](https://developer.mozilla.org/en/Thunderbird/Content_Tabs), a way to monitor other web sites and web-based messaging from inside Thunderbird. The Quetzalcoatal blog hacked together a little code that opens Wave cleanly and conveniently inside one of these tabs, and even keeps it open after you restart Thunderbird.
> 
>   

> 
> To load it up, head to the Tools menu, select Error Console, and enter this line (be sure to select all of it):
> 
>   

> 
> `Components.classes['@mozilla.org/appshell/window-mediator;1'].getService(Components.interfaces.nsIWindowMediator).i`
`getMostRecentWindow("mail:3pane").`
`document.getElementById("tabmail").openTab("contentTab", {contentPage: "https://wave.google.com/wave/?nouacheck"});`
> 
>   

> 
> Hit the "Evaluate" button when you're done, and you'll see a new tab asking for your Wave login. Log in, click "Remember" to have Thunderbird remember your password, and now it's just like having a little browser tab opened on Wave.
> 
>   

> 
> [Google Wave in Thunderbird 3](http://quetzalcoatal.blogspot.com/2009/12/google-wave-in-thunderbird-3.html) [Quetzalcoatal via [Kabatology](http://www.kabatology.com/12/09/a-line-of-code-that-opens-google-wave-inside-thunderbird-3)]
> 
> 

  


via [lifehacker.com](http://lifehacker.com/5423317/run-google-wave-inside-thunderbird-3)
Nifty trick to run Google Wave with thunderbird
