---
layout: post
title: "OfflineIMAP Notifications"
date: 2013-11-21
categories: posts
---

Before we begin, I will assume that anyone reading this already has a working 
OfflineIMAP setup. OfflineIMAP is not a hard requirement though. Any mail sync 
software that uses the Maildir format should work with little to no changes. 

If you by chance do not have such a setup yet, and are interested, 
go ahead and check out [this blog post][offlineimapblog] or the Arch Wiki's 
articles on [OfflineIMAP][offlineimap] and [Mutt][mutt], but ignore
instructions for setting up a cron job, as that is mainly what I am covering 
in this post. 

Mutt in combination with OfflineIMAP and Msmtp is easily the best mail setup
I've used, but because of it's simplicity, it's missing one thing that I 
like to have: visual notifications for new mail. In my [bspwm][bspwm] 
and [dwm][dwm] setups, I like to keep mutt running in a terminal window, 
in a dedicated workspace or tag respectively. Short of obsessively switching 
back and forth to the mutt terminal, I don't have a clean and efficient 
way to know immediately when I revieve a new email. 
So I figured a good solution  was create a bash script to 
check my maildirs after each time OfflineIMAP runs. With some googling and 
some help from the fine people over on the Arch Linux forums, I got this 
working with few issues. [[1]](#aside)

Now lets get started!

Make sure that you have your OS' version of `libnotify` installed 
before you begin. You will need a notification daemon for `notify-send` 
to communicate with. I personally use [dunst][dunst] because it's 
lightweight and simple to setup, but there are [many other choices][notify] 
that also work.

We can now put the following in a file called `offlineimap-notify.sh`: 

```bash
#!/usr/bin/bash

#run OfflineIMAP once, with quiet interface
offlineimap -o -q -u quiet

#count new mail
maildirnew="$HOME/Mail/*/*/new/"
new="$(find $maildirnew -type f | wc -l)"

#count old mail
maildirold="$HOME/Mail/*/*/cur/"
old="$(find $maildirold -type f | wc -l)"

if [ $new -gt 0 ] 
then
    export DISPLAY=:0; export XAUTHORITY=~/.Xauthority; 
    notify-send -a "OfflineIMAP" "New mail!\nNew: $new Old: $old"
fi
```

Make sure the script is executable, and put it in your `$PATH` if you wish.

It will count all of the files in the `~/Mail` directory and send a notification 
if there new ones. You may need to modify the counting section to your needs, depending
on the location of your Maildirs.

The `export DISPLAY=:0; export XAUTHORITY=~/.Xauthority;` part is neccessary, 
in my case, when running the script from a cron job, since cron will normally 
not have access to the environmental display variables. 
If I am wrong about this or there is a better solution, I would be glad to 
know about it.

We can run the script every 3 minutes with cron:

```
*/3 * * * * ~/Scripts/offlineimap-notify.sh
```

Now you should have a lovely little notification every time you get a new 
email! Simple as that.

[Click for example!][muttimg] (1024x600 728K)

```
Notes: 

If your OS keeps bash in /bin, then please change the shebang line 
to reflect that. Arch Linux keeps all binaries in /usr/bin but most other 
OS' don't.

Regarding my use of `find`: 
I am aware that in general parsing the output of `find` and `ls` can be problematic 
in certain circumstances. However, OfflineIMAP should not produce 
file names that are problematic, so ideally I should not see these issues. 
Any suggestions are more than welcome as I am still fairly new to bash
scripting myself.
```

<a name="aside">[[1]: Arch Linux forums scripting thread][aside]</a>

[offlineimapblog]: http://pbrisbin.com/posts/mutt_gmail_offlineimap/
[offlineimap]: https://wiki.archlinux.org/index.php/OfflineIMAP
[mutt]: https://wiki.archlinux.org/index.php/Mutt
[bspwm]: https://bbs.archlinux.org/viewtopic.php?id=149444
[dwm]: http://dwm.suckless.org/
[dunst]: https://www.archlinux.org/packages/community/x86_64/dunst/
[notify]: https://wiki.archlinux.org/index.php/Libnotify#Builtin_servers
[aside]: https://bbs.archlinux.org/viewtopic.php?pid=1346862#p1346862
[muttimg]: /mutt.png
