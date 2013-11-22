---
layout: post
title: "OfflineIMAP Notifications"
date: 2013-11-21
categories: posts
---



Before we begin, I will assume that anyone reading this already has a working Mutt setup,
and specifically one that uses OfflineIMAP to fetch mail and 
store in a local maildir (if you're using some sort of mbox setup, stop 
reading here because all of the following will likely not work for you).
If you by chance do not have such a setup yet, and are interested, go ahead 
and check out [this blog post][offlineimapblog] or the Arch Wiki's articles on 
[OfflineIMAP][offlineimap] and [Mutt][mutt], all while ignoring instructions 
for setting up a cron job, as that is mainly what I am covering in this post.


Mutt is easily the best mail client I've used, but because of it's 
simplicity, it's missing one thing that I like to have: visual notifications 
for new mail. In my [bspwm][bspwm] and [dwm][dwm] setups, I like to keep mutt 
running in a terminal window, in a dedicated workspace or tag respectively. 
Short of obsessively switching back and forth to the mutt terminal, I don't 
have a clean and efficient way to know immediately when I revieve a new email. 
So I figured the best solution to the problem was create a bash script to 
check my maildirs after each time OfflineIMAP runs. With some googling and 
some help from the fine people over on the Arch Linux forums, I got this 
working with few issues.

Now lets get started!

...but before we begin fore real this time, I will assume basic competence with 
the command line. If you're a bit shaky on creating files, changing 
permissions, or using cron from the command line, then some quick reading 
will be in order.

Make sure that you also have your distro's version of `libnotify` installed 
before you being, as none of this will work without it. You will need a 
notification daemon for `notify-send` to communicate with. I personally use 
[dunst][dunst] because it is light-weight and simple to setup, but there are 
[many other choices][notify] that should also work.

We will be making only a single bash script to get this running let's make a 
file called `offlineimap-notify.sh` (or whatever you want), which will be the 
example used, make it executable, and be sure that the script is in your $PATH.

In this file we will want to put the following: 

{% highlight bash %}
#!/usr/bin/bash

offlineimap -o -q -u quiet

maildirnew="$HOME/Mail/*/*/new/"
new="$(find $maildirnew -type f | wc -l)"

maildirold="$HOME/Mail/*/*/cur/"
old="$(find $maildirold -type f | wc -l)"

if [ $new -gt 0 ] 
then
    export DISPLAY=:0; export XAUTHORITY=~/.Xauthority; 
    notify-send -a "OfflineIMAP" "New mail!\nNew: $new Old: $old"
fi
{% endhighlight %}

`Note: if your OS keeps bash in /bin, then please change the shebang line 
to reflect that. Arch Linux keeps all binaries in /usr/bin but most other 
OS' don't.

In case recieve any emails, regarding my use of find in this manner, I am
aware that in general parsing the output of find and ls can be problematic 
in certain circumstances. However, OfflineIMAP should generally not produce 
file names that are problematic. I don't have huge amounts of time or energy 
to devote to this, so my scripts will not always be ideal. [[see]][aside]. Any
suggestions are more than welcome as I am still fairly new to bash
scripting myself.`

The script should be pretty simple to understand. It will count all of the 
files in your `~/Mail` directory and send a notification if there new ones, 
which will reside in the `$HOME/Mail/*/*/new/` for each account. You may need 
to modify this section to your needs.

The `export DISPLAY=:0; export XAUTHORITY=~/.Xauthority;` part is neccessary, 
in my case, when running the script from a cron job, since cron will normally 
not have access to DBUS or the display variables. If I am wrong about this 
or there is a better solution, I would be glad to know about it.

We can run the script every 3 minutes, as a cron job with `crontab -e`:

{% highlight bash %}

*/3 * * * * ~/Scripts/offlineimap-notify.sh

{% endhighlight %}

Now you should have a lovely little notification every time you get a new 
email!

[Click for example!][muttimg]

[offlineimapblog]: http://pbrisbin.com/posts/mutt_gmail_offlineimap/
[offlineimap]: https://wiki.archlinux.org/index.php/OfflineIMAP
[mutt]: https://wiki.archlinux.org/index.php/Mutt
[bspwm]: https://bbs.archlinux.org/viewtopic.php?id=149444
[dwm]: http://dwm.suckless.org/
[dunst]: https://www.archlinux.org/packages/community/x86_64/dunst/
[notify]: https://wiki.archlinux.org/index.php/Libnotify#Builtin_servers
[aside]: https://bbs.archlinux.org/viewtopic.php?pid=1346862#p1346862
[muttimg]: /mutt.png
