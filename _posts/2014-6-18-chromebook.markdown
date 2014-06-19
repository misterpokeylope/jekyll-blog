--- 
layout: post
title: "Experiences with a Chromebook"
date: 2014-06-18
categories: posts linux
---

For the past few years my main mobile workhorse has been a circa 2010 Asus
eeePC netbook, running Arch Linux. I purchased it used a few years back after a
increasing need to bring my computer with me to school, and the genuine
discomfort of toting around the 17-inch laptop I use at home. While it's a
wonderful machine, courtesy of Lenovo, it wasn't the most forward thinking of
purchases.

Forward to this summer, where I am currently taking Summer Semester classes,
and where two of which involve developing Android applications. In the
loosest of terms, Eclipse does **run** on this netbook, but its single core Intel 
Atom processor, and 1GB of RAM make it a less pleasant experience that I would
prefer to have. Unlike most people, I actually don't mind the smaller size of
netbooks, but in a "post-netbook" market of Ultrabooks and Tablets, it's not
incredibly easy to find a light, cheap, and small laptop.

So, what kind of laptops are light, cheap, and small? Searching would usually
always lead me to the same conclusion; Chromebooks. Since their introduction I
had usually passed them off as worthless, due to their cloud-based nature, and
inability to run anything other than web-related software. However in my search 
I discovered that most Chromebooks can run full-blown Linux installs with 
relative ease, a fact that completely reversed my impressions of them.

Among the current crop of Chromebooks, the ones that seemed to consistently 
stand out were the Acer C720 and the HP Chromebook 11. Despite the praise
the HP Chromebook 11 has recieved, the fact that it uses an ARM based processor
means it's incompatible with my class workload. So what about the Acer C720?

For around $200-$250 (less than what I paid for my netbook), one gets a
Haswell-based dual-core Celeron processor, 2GB of RAM, 16GB/32GB SSD, and a 
11.6-inch 1366x786 matte(!!) display. While even 32 GB is a little tight on 
space, the rest of those specs are more than enough for the kind of work I need
to do while I'm not at home.

I ended up going with the 32GB model since I need all the storage I can get. The
Android SDK alone takes up more than 5GB on my current Arch Linux install, but the
rest of the storage can be supplemented with a [SanDisk Cruzer Fit][sandisk] flash
drive. While it means I can't take all 200+ GB of the classic Doctor Who
episodes I have with me on the go, I don't expect storage space to be much of
an issue.

As of writing this I don't have Arch Linux installed yet, due to a
busy week of summer classes, but it's given me time to see what it's like to 
use Chrome OS for my daily computing needs.

[![Chrome OS Desktop](/img/crosdesktopthumb.png)](/img/crosdesktop.png)

Since it's release Chrome OS has come a long way, from just being a Chrome
browser window, to having the hardware accelerated window manager it uses
today. The only applications it has are the Google Chrome browser, a file
manager, and a video player, which can be a boon, and a curse. As a result,
it's incredibly fast and light, but new functionality only comes from
installing web apps from Google's [Chrome Web Store][webstore].

Fortunately, Chrome OS is really just a Linux distro with a stripped down
userland and fancy GUI. It even includes it's own homegrown terminal called
crosh, which can be opened with the `Ctrl+Alt+t` keybinding. From there one
can SSH into other machines, and do other basic network diagnostics. But upon
enabling Developer mode, one can access a standard Bash shell, by entering in 
'shell' in crosh. 

This Linux underpinning has lead to some interesting projects, one of the most
notable being the [crouton][crouton] project. Without regurgitating
what the link already says, crouton installs a full blown Linux distro of your
choice into a [chroot environment][chroot], effectively allowing the user to 
run Chrome OS and Linux side by side, sharing the system's resources.

[![Chrome OS Chroot](/img/croschrootthumb.png)](/img/croschroot.png)

As a stopgap until I have more time to install Arch, I've installed a Debian
Sid chroot with e17 (for any graphical tasks I might need to do).
Thanks to this I can be using Chrome for internet browsing, open up a
crosh window, chroot into Debain Sid, and then work on my Python programming
lab for class in a much more complete command-line environment than what's
possible in Chrome OS' Developer mode. Then if I need to, I can even
install Eclipse and the Android SDK into the chroot, and pop into e17!

[![Debian Sid](/img/debiansidthumb.png)](/img/debiansid.png)

While I would vastly prefer a full Arch install I have found that this a very
comfortable medium between Chrome OS and a full Linux install. The benefit of
using a chroot is that it gets access to the Chrome OS Kernel and drivers,
meaning that the hardware will always work. That is contrasted to the standard
Linux Kernel, where the C720's touchpad driver still has to be pathed in at
each new major release of the kernel.

Once I get around to installing Arch in the next few weeks or so, I will
probably make another post chronicling those adventures, but in the meantime,
my initial impressions of Chromebooks and Chrome OS have been much more
positive than I expected. If I didn't need to any programming, I would probably 
not even need to replace Chrome OS. It's a somewhat grim reminder of how much I 
rely on web browers as a part of my workflow.

[sandisk]: http://amzn.com/B00812F7O8
[webstore]: https://chrome.google.com/webstore/category/apps
[crouton]: https://github.com/dnschneid/crouton
[chroot]: http://en.wikipedia.org/wiki/Chroot
