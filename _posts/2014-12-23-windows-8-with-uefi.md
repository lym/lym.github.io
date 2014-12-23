---
layout: post
title: "Windows 8 with uefi"
description: ""
category: File Systems
tags: [Windows, UEFI, Secure Boot, DOS, file systems, Boot]
---
{% include JB/setup %}
# Installing New Operating System on a pc that shipped with Secure boot enabled.

Today morning I was faced with an on-the-surface trivial task of installing a
new Operating System on a netbook. The netbook shipped with Windows 7 but the
owner needed for it to be wiped and have Windows 8 put on instead.

I had a bootable flash drive with Win 8 on the fly so I plugged it in and hit
F12 on power on. For some reason the boot sequence wouldn't pause long enough for
me to hit F12 and select a different boot medium.

So after a couple minutes of shouting out tasty words, I figured out I had
to first go into the Acer's setup and and see what was going on. I noticed I
had to change the boot mode form UEFI to legacy, pure hunch.

To get to this option, you hit <kbd>F2</kbd> then go over to the Boot tab.
After making and saving the change, you'll be able to press <kbd>F12</kbd>
after the power button and have the boot device options show up. Here you
can select your USB HDD and it will run the win 8 installer.

Now here comes another hoop to jump over. When you try to install Windows on any
of the partitions it will refuse to install to GPT partitions since, am asuming
individual who did the initial install chose GPT over MBR. Either that or its
secure boot policy, I don't know how that got there but it was like that.

So to get over this issue you have to drop down to good old DOS. You drop to a DOS
prompt by hitting SHIFT F10. At the prompt you enter the following commands.

	DISKPART  // Enter disk partition mode
	LIST DISK  // List disks online
	SELECT DISK [DISK NUMBER] // No of disk to install to
	CLEAN
	CONVERT MBR
	CREATE PARTITION PRIMARY SIZE=[SIZE IN MB]
	FORMAT FS NTFS LABEL "SYSTEM" QUICK
	EXIT		// Close prompt

Refresh the disks in the GUI area and be on your way. 

hints from:

- [Super User](http://superuser.com/questions/515891/how-to-reformat-hd-from-gpt-partitions-to-ntfs-partitions-for-windows-7-install)
- [Computer Hope](http://www.computerhope.com/issues/ch000192.htm)
- [msdn](http://msdn.microsoft.com/en-us/library/windows/hardware/dn640535(v=vs.85).aspx)
- [GPT wikipedia wiki](http://en.wikipedia.org/wiki/GUID_Partition_Table)
- [How to geek](http://www.howtogeek.com/193669/whats-the-difference-between-gpt-and-mbr-when-partitioning-a-drive/)

And prior filesystem nerdery ;-)
