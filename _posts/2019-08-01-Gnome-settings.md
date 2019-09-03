---
layout: post
title: Gnome settings won't start.
color: rebeccapurple
tags: [Gnome, settings, ubuntu, linux]
---

I have been having an issue with my settings program not starting.
I would search for settings or look for it my list of apps and attempt to launch it. However, all I would get is a spinning wheel and then nothing. It would fail in the background without any warning or  error dialog. This is inherently bad design. Anyway I am kind of used to such shortcomings in open source software.

Curious about what was happening in the background, I fired up a bionic terminal and typed gnome-control-center and there it was, the error message meant for terminal-eyes only.

```bash
ERROR:../shell/cc-panel-list.c:926:cc_panel_list_set_active_panel: assertion failed: (data != NULL)
[1] 7043 abort (core dumped) gnome-control-center
```

It made little sense to me so I took the liberty of executing gnome-control-center
`$ gdb gnome-control-center` then followed by `run`

This time I received some helpful information;

```bash
~ gdb gnome-control-center
GNU gdb (Ubuntu 8.1-0ubuntu3) 8.1.0.20180409-git
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from gnome-control-center...(no debugging symbols found)...done.
(gdb) n
The program is not being run.
(gdb) run
Starting program: /usr/bin/gnome-control-center 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[New Thread 0x7fffcc836700 (LWP 7400)]
[New Thread 0x7fffcbb4a700 (LWP 7401)]
[New Thread 0x7fffcb349700 (LWP 7402)]
[New Thread 0x7fffcab48700 (LWP 7403)]
**
ERROR:../shell/cc-panel-list.c:926:cc_panel_list_set_active_panel: assertion failed: (data != NULL)

Thread 1 "gnome-control-c" received signal SIGABRT, Aborted.
__GI_raise (sig=sig@entry=6) at ../sysdeps/unix/sysv/linux/raise.c:51
51	../sysdeps/unix/sysv/linux/raise.c: No such file or directory.
(gdb) q
```

My best guess was that there was a corrupted thread running loose somewhere in the program.

So the best I could do was to force a reset of the gnome control center.

```bash
$ dconf reset -f /org/gnome/control-center/
$ gnome-control-center
```