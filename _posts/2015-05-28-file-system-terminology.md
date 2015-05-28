---
layout: post
title: "File System Terminology"
description: ""
category: 
tags: []
---
{% include JB/setup %}

When talking about file systems it doesn't hurt to be familiar with the
following terminology.

## Disk
A permanent storage medium of a certain size. A disk also has a
<i>sector</i> or <i>block size</i>, which is the minimum unit that the
disk can read or write. The block size of most modern hard disks is 512
bytes.

## Block
The smallest unit writable by a disk or file system. Everything a file
system does is composed of operations done on blocks. A file system
block is always the same size as, or larger (in integer multiples) than
the disk block size.

## Partition
Any subset of the total number of blocks on a disk. A disk can therefore
have several partitions.

## Volume
The name we give to a collection of blocks on some storage medium (i.e.,
disk). That is, a volume may be all of the blocks on a single disk, some
portion of the total number of blocks on a disk, or it may span multiple
disks and be all of the blocks on several disks.

The term <i>volume</i>
is used to refer to a disk or partition that has been inititialized with
a file system.

## Superblock
The area of a volume where a file system stores its critical volumewide
information. A superblock usually contains information such as:
- how large a volume is,
- the name of the volume,
- and so on.

## Metadata
A general term refering to information that is about something but not
directly part of it. For example the size of a file is very important
information about a file, but it is not part of the data in the file.

## Journaling
A method of ensuring the correctness of file system metadata even in the
presence of power failures or unexpected reboots.

## I-node
The place where a file system stores all the necessary meta data about a
file and any other data associated with the file. The term <i>i-node</i>
is historical and originated in Unix. An i-node is also known as a
<i>file control block</i> (FCB) or <i>file record</i>.

## Extent
A starting block number and a length of successive blocks on a disk. For
example an extent might start at block 1000 and continue for 150 blocks.
Extents are always contiguous. Extents are also known as <i>block
runs</i>

## Attribute
A name (as a text string) and value associated with the name. The value
may have a defined type (string, integer, etc.), or it may just be
arbitrary data.
