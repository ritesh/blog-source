---
date: 2010-08-26 18:14:00+00:00
slug: using-mutt-with-gmail
title: Using mutt with Gmail
wordpress_id: 17
categories:
- cli
- command
- email
- mutt
---

Why bother? I can't think of a reason other than because. My muttrc   
(single gmail account) copied from various muttrcs around the   
internet.   
```   
set imap_user = "EMAILID@gmail.com"   
set imap_pass = "Password"   
set smtp_url = "smtp://EMAILID@smtp.gmail.com:587/"   
set smtp_pass = ""   
set from = "EMAILID"   
set realname = "Ritesh Sinha"   
set folder = "imaps://imap.gmail.com:993"   
set spoolfile = "+INBOX"   
set postponed="+[Gmail]/Drafts"   
set record = "+[Gmail]/Sent Mail"   
set header_cache=~/.mutt/cache/headers   
set message_cachedir=~/.mutt/cache/bodies   
set certificate_file=~/.mutt/certificates   
set move = no   
set include   
set sort = 'threads'   
set sort_aux = 'reverse-last-date-received'   
set auto_tag = yes   
set imap_check_subscribed   
set hostname = gmail.com   
ignore headers *   
unignore headers from to subject date cc   
hdr_order Date From To Cc   
color attachment brightmagenta black   
color error brightwhite red # errors yell at you in red   
color hdrdefault white black # headers   
color indicator default magenta # currently selected message   
color markers brightcyan black # the + for wrapped pager lines   
color message brightcyan black # informational messages, not mail   
color normal white black # plain text   
color quoted green black # quoted text   
color search brightgreen black # hilite search patterns in the pager   
color signature green black # signature (after "-- ") is red   
color status brightyellow blue # status bar is yellow *on blue*   
color tilde blue black # ~'s after message body   
color tree red black # thread tree in index menu is magenta   
color underline yellow black   
color header brightwhite black ^(From|Subject): # Important headers   
color body magenta black "(ftp|http)://[^ ]+" # picks up URLs   
color body magenta black [-a-z_0-9.]+@[-a-z_0-9.]+   
color index brightgreen black ~N   
source "/usr/share/doc/mutt/examples/gpg.rc" # This is for GPG   
set pgp_sign_as=0xB8914BE6   
set pgp_autosign=no   
auto_view text/html   
set sort=reverse-date-sent   
set query_command="goobook query '%s'"   
bind editor  complete-query   
bind editor  noop   
# Gmail-style keyboard shortcuts   
macro index,pager y "unset trashn "   
"Gmail archive message"   
macro index,pager d "set   
trash="imaps://imap.googlemail.com/[GMail]/Bin"n "   
"Gmail delete message"   
macro index,pager gi "=INBOX" "Go to inbox"   
macro index,pager ga "=[Gmail]/All Mail" "Go to all mail"   
macro index,pager gs "=[Gmail]/Starred" "Go to   
starred messages"   
macro index,pager gd "=[Gmail]/Drafts" "Go to drafts"   
```