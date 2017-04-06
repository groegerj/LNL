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
and LineageOS 14.1. LineageOS is the successor of Cyanogenmod,
which has been abandoned.

http://lineageos.org

Stock firmware or other Android ROMs should
work as well but might require some additional tuning.
In any case, the following is needed:

*  A rooted Android with su binary.
*  An Android version that allows setting selinux to moderate.
*  A busybox binary in /system/xbin.
*  A terminal emulator app.

## su command

You can check whether you have the su command available
but starting your favourite terminal app (see below) and type

    su

su is strictly needed for LNL to work.

### Have su

Cyanogenmod comes with su, you just has to activate it (see below).
LineageOS does not. If you install LineageOS from a ROM zip file,
install addonsu-arm-signed.zip in addition, which is available from

https://download.lineageos.org/extras

If you build LineageOS from source on your own, run

  export WITH_SU=true

before building. See

http://lineageos.org/Update-and-Build-Prep

### Activate su

On Cyanogenmod/LineageOS, su has to be activated.
In the settings, go to "About phone/table",
and there tap several times on the "build number" item.
This makes the developer settings available. Go there, and activate
root access for apps. Then open the terminal again and type su.
You will be asked if you grant root to the terminal app.
Mark your decision as permanent and say yes.

## selinux

TODO explanation, OK on Cyanogenmod/LineageOS

## busybox

Busybox combines tiny versions of many common Unix utilities such as
the mount command used by LNL. On Android systems, there has been
a similar such program called Toybox, which also provides a mount
command. However (as of Cyanogenmod 13), Toybox seems not to support
recursive bind mounts needed by LNL, which is why Busybox is needed.

You can check whether you have the su command available
but starting your favourite terminal app (see below) and type

    busybox

### Install Busybox - Manual Way

Busybox is available from

https://busybox.net

You may download the source code and compile busybox on your own
or use an executable binary file provided there.
In any case, make sure the busybox binary is compiled for the ARM
platform and statically linked, i.e. without further system dependencies.

In the following, I assume that you have a working busybox file
available and access to a root shell on your device, be it from a
terminal app with su command or through adb. Run the following commands.

    mount -o remount,rw /system
    cp PATHTO/busybox /system/xbin/busybox
    (or adb push PATHTO/busybox /system/xbin/busybox)
    chmod 755 /system/xbin/busybox

### Include Busybox as Part of a Custom ROM

As with any additional software to be included,
the busybox stuff has to go to its own subdirectory with Android.mk makefile.
You can choose between putting the source code there, to be compiled along with
the other stuff when you build the ROM, or a precompiled busybox binary to be
installed as in the manual way described above, but by the build system.
Let us assume that everything lives in

  external/busybox

(within the source tree), although the precise choice of subdirectory does
not matter too much, and there is an according makefile

  external/busybox/Android.mk

For the precomiled option,
create that directory, copy a precomiled busybox binary there and copy
the file Android_busybox.mk (which comes with LNL) renamed to Android.mk there.
Busybox binaries are also available from

https://busybox.net

but of course feel free to compile it on your own.
Just make sure the architecture matches your target system (usually ARM something),
and you build the binary without dependencies on dynamic libraries (static build).
When you compile, busybox offers tons of settings. You need not change the default options,
besides these two things (ARM and static).

In Android.mk, look for the line

    LOCAL_MODULE := busybox

As the name suggest, this is the module's name which must be made known to
the build system, that is to any build makefile used for the build.
For example, add 'busybox' to the 'PRODUCT_PACKAGES +=' command in the file

  build/target/product/embedded.mk

If everything is done correctly, your new ROM will come with busybox.

As an alternative for the first step (that busybox directory), you may
try to use the busybox module optionally shipped with CyanogenMod/LineageOS.
Instead of populating the directory on your own, use the one from github:
Add the line

    <project name="CyanogenMod/android_external_busybox" path="external/busybox" remote="github" revision="cm-13.0" />

or

    <project name="LineageOS/android_external_busybox" path="external/busybox" remote="github" revision="cm-14.1" />

(or whatever with the intended name and revision) to your

  roomservice.xml

and, of course, do not forget to add 'busybox' to embedded.mk (or whatever).
That way of including busybox has been my preferred one on CyanogenMod 13.
For some reason, it does not work for me on LineageOS 14.1. To be more precise,
the module gets included, busybox is there, and can be started as an executable.
But behaves extremely weird and does not work at all as intended.
Compared to CM 13, nothing essential has changed, so I guess there might have
been changes in the bionic library which broke busybox(?)
For now, I gave up on this and are happy with the first method (precompiled binary).

## Terminal

For best user experience, jackpal's Terminal app
and Hacker's keyboard, both available through F-Droid.

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

