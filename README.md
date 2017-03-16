# LNL - LNL not Linux

Copyright (C) 2014-2017 Josua Groeger.
groegerj at thp.uni-koeln dot de

LNL stands for "LNL not Linux". It consists of several shell scripts
to establish a chroot Debian-Linux environment on an Android system.

# LICENSE

LNL is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

LNL is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with LNL.  If not, see <http://www.gnu.org/licenses/>.

# PURPOSE AND HISTORY

Run Debian-Linux in a chroot environment on an Android device.
The main idea has been around for some time, see e.g.

http://www.saurik.com/id/10

https://wiki.debian.org/ChrootOnAndroid

for an incomplete list.
LNL is my own implementation to automatise the setup
of embedding the Linux environment nicely into Android.
There are other solutions serving a similar purpose or
using similar ideas, such as

http://sven-ola.commando.de/repo/debian-kit-en.html

Debian Kit is older than LNL.
When I was new to the subject, I failed to install it
for some reason and soon after more or less forgot about it.
Only recently, I rediscovered Debian Kit.
While LNL works pretty well by now, it would be a good idea
to make a thorough comparision on commonalities, differences
and mutual advantages.

More explanation to come soon (explain directory blabla,
home dir solution, resolv.conf etc.)

# PREREQUISITES

LNL has been developed and tested mainly on Cyanogenmod 13
and LineageOS 14.1. Stock firmware or other Android ROMs should
work as well but might require some additional tuning.
In any case, the following is needed:

*  A rooted Android with su binary.
*  A busybox binary in /system/xbin.
*  An Android version that allows setting selinux to moderate.
*  A terminal emulator app.

In addition, you need a terminal emulator app.

*  For best user experience, jackpal's Terminal app
   and Hacker's keyboard, both available through F-Droid.

Prerequisites to be explained in more detail...

# USAGE

Scripts work fine provided one has a suitably configured Debian system
installed in /data/debian...

Documentation to be written...

lnl-debootstrap:
create a small Debian system and compress to an archive.
To be run on a Debian system. You need the following packages there:
apt-get install qemu qemu-user-static debootstrap
Status: works ok.
Minimal requirement for additional software: inotify-tools and dialog.

Steps to do for installation:

*  copy archive file (result of lnl-debootstrap) to Android device
*  On Android, extract to /data/debian
*  On Android, copy lnl and lnl-init to /system/xbin/
*  Now, invoking "lnl" on a terminal app should start the real thing :-)

X11 usage to be documented...

