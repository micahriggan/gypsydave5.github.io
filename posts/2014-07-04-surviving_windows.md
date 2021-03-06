---
layout: post
title:  "Surviving Windows"
date:   2014-07-04 11:35:15
tags:
    - Tools
    - Windows
published: true
---

I've worked within Windows and Microsoft OSs for... well, most of my computing
life now I think about it. I'm pretty sure that most people are the same. And
like most people I've always had my little moans about it. Too many to mention.

This has come to a head since I [switched to Linux][LinuxSwitch] a few months
ago. Not that Linux (Ubuntu in my case) doesn't have it's own long list of
problems, but they all feel more like problems I should be fixing myself
- rather than things I'll just moan about and put up with. Yes, Ubuntu (and the
Unity desktop specfically) has 'spoiled' me. I'd rather be working in that
environment than any earlier version of Windows. Woo-hoo. Hear the whoop from camp
Linux, joy shall be in heaven over one sinner that repenteth, etc etc usw.

But there's still so much of my life that has to be done in Windows. Work
spreadsheet has VBA macros? Work machine in Vista? [Netflix requires
Silverlight][Netflix]? It's inevitable.

So instead of moaning I've been trying to fix Windows to make it usable like
I want it to be. And I think I've found some good options. The first thing to
note is that I've been so used to putting up with Windows that I was under the
impression that there was very little that could be done to it. Lay that thought
to rest - there's hundreds of hacks, options, fixes, and bits of software we can
use to make Window's a happier place to be. Or, if not actively happy, then at
least survivable...

## Winsplit Revolution##
OK, not the greatest name. What I missed most about Ubuntu when stuck on Vista
was the way I could throw windows around the desktop using the numpad. This
feauture is neatly emulated with [Winsplit Revolution][WSRev], which is small
and, well, works. You can customize the shortcuts too. (Looks like the original
website is no longer around so I've linked to the CNET page).

## Put the taskbar on the side##
Oh this sounds dumb I know, but nobody even thinks of doing it on Windows
because it's a change. Totally inspired by the Unity set up, it just makes more
sense. If most of what I'm doing involves reading down a page on the screen
- code, text, etc - then I really want optimize the vertical space on the
screen. So slam the taskbar to the side and get yourself a couple of centimeters
for free. If you were going to try one new thing this week make it this.

##GVim##
All that Vim goodness - [now on your Windows][GVimWin]. And you can use the same
config files. And you can set the config depending on the environment (Windows,
Linux, OSX). And you could save all those files onto Dropbox, make symlinks to
them... OK - too far. But get GVim for Windows.

##Autohotkey##
I'll write something about the what effect of removing the caps lock button from
my keyboard has been (no, not physically) at a later date. But for fun keyboard
hacks like this and more I've been enjoying [Autohotkey][AHkey], which has its
own simple scripting language to allow you to remap and rewite your keyboard to
your hearts content.

##Get GNUy##
I've tried [Cygwin][Cygwin] before - part of working using a Linux only font
tool. But I found it... big. Powerful, yes - but big. So big I didn't use it.
Second time around I've been using [Gow][Gow] - which brings all the \*nix-y
command line goodness to your `cmd` shell in Windows.

##Chocolatey##
Missing `apt-get` or similar package management utilities on Windows?
[Chocolatey][Chocolatey] to the rescue. Install Git! Install Node! Install
everything listed above and more, and get them all updated from the command
line. It's brilliant.

Ah, that's it for now. All I'd finish off by saying is that the only thing
holding me back from improving my experience of Windows was... ignorance. And
indolence. GNU/Linux makes you change things to fit the way you work, and as soon as
you've learned how to do that it's easier to avoide complacence with other OSes.
. A good lesson.

[Gow]: https://github.com/bmatzelle/gow/wiki
[Chocolatey]: http://chocolatey.org/
[WSRev]: http://download.cnet.com/WinSplit-Revolution/3000-2072_4-10971915.html
[GVimWin]: http://www.vim.org/download.php#pc
[AHkey]: http://www.autohotkey.com/
[Netflix]: https://launchpad.net/pipelight
[Cygwin]: https://www.cygwin.com/