---
layout: post
title: The Termite Terminal Emulator
date:
categories: posts linux
---

In much of the \*nix world, the terminal is a very important part of everyday
activies, so naturally a pleasant terminal experience is an absolute
requirement. As such, there is no dearth of terminal emulators available,
however [xterm][xterm] and [rxvt-unicode][urxvt] (urxvt) have generally
remained the most popular. I was drawn to urxvt due to it's simplicity and
extensibility, and because I wanted a terminal that was as lightweight as the
tiling window managers I had started using.

Throughout my experiences with urxvt I was happy and content with what it
provided, save for some minor inital usibility hiccups that were quickly solved with
the many extensions available. However, I began to hear about another new terminal
emulator on the Arch Linux forums, called Termite.

Termite is a keyboard driven VTE-based (yes the same library that GNOME uses)
terminal that offered same simplicity of urxvt, with even simpler configuration, 
and many nice features baked in. For a while I ignored it because I was happy
with urxvt, but it wasn't until I started using anti-aliased infinality-patched
fonts in my Terminal. While the urxvt package on Arch has Xft-support enabled I
was not completely happy with the quality and style of the rendering.

After seeing some posts suggesting Termite, I made the switch, and was
pleasantly surprised. 

[comparison screenshot]

Since Termite is written using the VTE libraries, it has the added advantage of
using the Pango for it's font rendering, as opposed to just Xft like what urxvt
uses. In my cases, Pango has much cleaner rending, and does not have the
strange spacing that urxvt seems to have. However, also due to the fact that
Termite is VTE-based, it depends on GTK3, so anyone looking for a
toolkit independent terminal emulator should look elsewhere.

[xterm]: http://invisible-island.net/xterm/
[urxvt]: http://software.schmorp.de/pkg/rxvt-unicode.html
