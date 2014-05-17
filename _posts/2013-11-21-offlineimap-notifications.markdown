---
layout: post
title: "Simple OfflineIMAP Notifications with libnotify"
date: 2013-11-21
categories: posts linux
---
 
Mutt in combination with OfflineIMAP and Msmtp is easily the least sucky mail setup
I've used, but because of it's simplicity, it's missing one thing that I 
like to have: visual notifications for new mail. In my [bspwm][bspwm] 
and [dwm][dwm] setups, I like to keep mutt running in a terminal window, 
in a dedicated workspace or tag respectively. Short of obsessively switching 
back and forth to the mutt terminal, I don't have a clean and efficient 
way to know immediately when I revieve a new email.

I found some scripts that can accomplish this on the Arch Linux forums, but
they were more complex than what I wanted. Instead I wrote a short bash script 
to run OfflineIMAP and display the number of new emails, if any. With some googling and 
some help from the fine people over on the Arch Linux forums, I got this working 
with few issues.

The entire process was fairly straightforward, so here's a little how-to: 

Make sure to have your OS' version of `libnotify` installed 
before you begin, and a notification daemon for `notify-send` 
to communicate with. I personally use [dunst][dunst] because it's 
lightweight and simple to setup, but there are [many other choices][notify] 
that also work.

We can put the following in a file called `offlineimap-notify.sh`: 

```bash
#!/usr/bin/bash

#run OfflineIMAP once, with quiet interface
offlineimap -o -q -u quiet

#count new mail for every maildir
maildirnew="$HOME/Mail/*/*/new/"
new="$(find $maildirnew -type f | wc -l)"

#count old mail for every maildir
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
not have access to the environmental display variables. I also tried to use
this script with a systemd timer unit, in a systemd User instance, but I 
was unable to get it to display the notifcations, even though it was syncing
my mail properly. If I can find a solution to this I will create a follow-up
post with my findings.

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

[bspwm]: https://bbs.archlinux.org/viewtopic.php?id=149444
[dwm]: http://dwm.suckless.org/
[dunst]: https://www.archlinux.org/packages/community/x86_64/dunst/
[notify]: https://wiki.archlinux.org/index.php/Libnotify#Builtin_servers
[aside]: https://bbs.archlinux.org/viewtopic.php?pid=1346862#p1346862
[muttimg]: /img/mutt.png
