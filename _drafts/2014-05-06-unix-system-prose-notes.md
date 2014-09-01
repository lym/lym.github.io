---
layout: post
title: "Unix System prose notes"
description: ""
category: "System Administration" 
tags: ["Unix", "Linux", "System Administration", "Dev ops", "shell"]
---
{% include JB/setup %}

*Symbolic links* were created to overcome the two disadvantages of
hard links:
 - Hard links cannot span physical devices, and
 - hard links cannot reference directories, only files.

Symbolic links are a special type of file that contains a text
pointer to the target file or directory.

### For when you're lost!

    help [command] => works for shell built-ins e.g help cp

    [command]--help => provides usage information for compiled
        programs such as are found in /usr/bin. eg `ruby --help`

    man [command | program] => display a program's manual Page. e.g man ruby


### Invoking commands

    [command] [-options] [arguments]

Options modify the behaviour of commands

Arguments are the items upon which the command acts.


### Man Page Organization
Section : Contents
1 => User commands
2 => Programming Interfaces for kernel system calls
3 => Programming interfaces for the c library
4 => Special files such as device nodes and drivers
5 => File formats
6 => Games and amusements such as screen savers
7 => Miscellaneous
8 => System Administration tools

    man [section] [search_term]  # to go to a specific section of the
        manual page e.g:
    man 1 ruby

### Display Appropriate information
    apropos => Display Appropriate information, note that man with the -k option does exactly the same thing.
e.g. 
    apropos floppy.

### Find out a little about a command
    whatis => Display a brief description of a command.
e.g
    whatis ls

    info => Displays a Program's Info entry.
e.g
    info ls

### Verbose
Most of the command-line programs we have discussed so far are part
of the GNU Project’s coreutils package, so you can find more information
about them by typing

    $ info coreutils

The `gzip` package includes a special version of less called zless,
which will display the contents of gzip-compressed text files.
e.g. 
    $ zless /usr/share/doc/less/changelog.Debian.gz 

### Create alias
Notice the structure of this command:
	alias name='string'
e.g. alias foo='cd /usr; ls; cd -'

After the command alias we give the alias a name followed immediately
(no whitespace allowed) by an equal sign, which is followed immediately by a
quoted string containing the meaning to be assigned to the name. After we
define our alias, it can be used anywhere the shell would expect a command.

We can also use the `type` command again to see our alias:

To remove an alias, the unalias command is used, like so:

    $ unalias foo

To see all the aliases defined in the environment, use the alias com-
mand without arguments.

    $ alias

    alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo \
        terminal || echo error)" "$(history|tail -n1|sed            \
        '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
    alias egrep='egrep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias grep='grep --color=auto'
    alias l='ls -CF'
    alias la='ls -A'
    alias ll='ls -alF'
    alias ls='ls --color=auto'
    alias rvm-restart='rvm_reload_flag=1 source '\''/home/lym/.rvm/scripts/rvm'\'''

There is one tiny problem with defining aliases on the command line.
They vanish when your shell session ends. 

### Handy utilities
cat — Concatenate files.
sort — Sort lines of text.
uniq — Report or omit repeated lines.
wc — Print newline, word, and byte counts for each file.
grep — Print lines matching a pattern.
head — Output the first part of a file.
tail — Output the last part of a file.
tee — Read from standard input and write to standard output and files.

### I/O
I/O is divided into 3:
 - Standard input (connected to the keyboard)
 - Standard output (connected to the screen)
 - Standard Error

    $ > ls-output.txt

Simply using the redirection operator with no command preceding it
will truncate an existing file or create a new, empty file.
So, how can we append redirected output to a file instead of overwriting
the file fr

    $ ls -l /usr/bin >> ls-output.txt

Using the >> operator will result in the output being appended to the
file. If the file does not already exist, it is created just as
though the > operator had been used.

To redirect standard error we must refer to its file descriptor. A pro-
gram can produce output on any of several numbered file streams. While
we have referred to the first three of these file streams as standard input,
output, and error, the shell references them internally as file descriptors
0, 1, and 2, respectively. The shell provides a notation for redirecting files
using the file descriptor number. Since standard error is the same as file
descriptor 2, we can redirect standard error with this notation:
$ ls -l /bin/usr 2> ls-error.txt

The file descriptor 2 is placed immediately before the redirection operator to perform 
the redirection of standard error to the file ls-error.txt.

0 = standard input
1 = standand output
2 = standard error

$ ls -l /bin/usr > ls-output.txt 2>&1  => redirect standard error.

use `&>` to redirect both 1 and 2 i.e standard output and error.

Sometimes silence really is golden, and we don’t want output from a com-
mand—we just want to throw it away. This applies particularly to error and
status messages. The system provides a way to do this by redirecting output
to a special file called /dev/null. This file is a system device
called a bit bucket, which accepts input and does nothing with it.

    $ ls -l /bin/usr 2> /dev/null

### apt
    $ sudo apt-get remove [package_name] : to delete a package
e.g:
    $ sudo apt-get remove couchdb

### cat
    cat [file...]

The cat command reads one or more files and copies them to standard output.

In most cases, you can think of cat as being analogous to the TYPE
command in DOS. You can use it to display files without paging.
 
Note: cat lists displays file content and it exits while less stays.

Since cat can accept more than one file as an argument, it can
also be used to join files together. Say we have downloaded a large
file that has been split into multiple parts (multimedia files are
often split this way on Usenet), and we want to join them back
together. If the files were named 
`movie.mpeg.001 movie.mpeg.002 ... movie.mpeg.099`
we could rejoin them with this command:

    $ cat movie.mpeg.0* > movie.mpeg

Since wildcards always expand in sorted order, the arguments will be
arranged in the correct order.

`ctr + D` ends cat session i.e when cat is entered without arguments.

The ability of commands to read data from standard input and send to
standard output is utilized by a shell feature called pipelines. Using the pipe
operator | (vertical bar), the standard output of one command can be piped into the standard input of another.
command | command

We can use less to display, page by page, the output of
any command that sends its results to standard output:

    $ ls -l /usr/bin | less

Pipelines are often used to perform complex operations on data. It is
possible to put several commands together into a pipeline.
Frequently, the commands used this way are referred to as filters.
Filters take input, change it somehow, and then output it.

The first one we will try is sort. Imagine we
want to make a combined list of all of the executable programs in /bin and
/usr/bin, put them in sorted order, and then view the list:

    $ ls /bin /usr/bin | sort | less

Since we specified two directories (/bin and /usr/bin), the output of
`ls` would have consisted of two sorted lists, one for each
directory. By including sort in our pipeline, we changed the data to
produce a single, sorted list.

The uniq command is often used in conjunction with sort. uniq accepts
a sorted list of data from either standard input or a single
filename argument (see the uniq man page for details) and, by
default, removes any duplicates from the list. 

    $ ls /bin /usr/bin | sort | uniq | less  #remove duplicates

    $ ls /bin /usr/bin | sort | uniq -d | less  #include just the duplicates

The `wc` (word count) command is used to display the number of lines,
words, and bytes contained in files. For example:
$ wc ls-output.txt

    $ ls /bin /usr/bin | sort | uniq | wc -l  #just show the number of lines from the unique sorted list.

grep is a powerful program used to find text patterns within files, like this:

    grep pattern [file...]

Let’s say we want to find all the files in our list of programs that
have the word zip in the name. Such a search might give us an idea of
which programs on our system have something to do with file
compression. We would do this:

    $ ls /bin /usr/bin | sort | uniq | grep zip

There are a couple of handy options for grep: -i, which causes grep
to ignore case when performing the search (normally searches are case
sensitive) and -v, which tells grep to print only lines that do not
match the pattern.

The head command prints the first 10 lines of a file, and the tail
command prints the last 10 lines. By default, both commands print 10
lines of text, but this can be adjusted with the -n option:

    $ head -n 5 ls-output.txt

    $ tail -n 5 ls-output.txt

These can be used in pipelines as well:

    $ ls /usr/bin | tail -n 5

`tail` has an option that allows you to view files in real time. This
is useful for watching the progress of log files as they are being
written. In the following example, we will look at the messages file
in `/var/log`. Superuser privileges are required to do this on some
Linux distributions, because the /var/log/messages file may contain
security information.

    $ tail -f /var/log/messages

Using the -f option, tail continues to monitor the file and when new
lines are appended, they immediately appear on the display. This
continues until you type CTRL-C.

Linux provides a command called `tee` which creates a “T” fitting on
our pipe. The tee program reads standard input and copies it to both
standard output (allowing the data to continue down the pipeline) and
to one or more files. This is useful for capturing a pipeline’s
contents at an intermediate stage of processing

    $ ls /usr/bin | tee ls.txt | grep zip  #put a copy of the output in ls.txt, then grep the other copy.

When the <kbd>ENTER</kbd> key is pressed, the shell automatically
expands any qualifying characters on the commandline before the
command is carried out.

The mechanism by which wildcards work is called *pathname expansion*.

    $ ls -d .[!.]?*

This pattern expands into every filename that begins with a period,
does not include a second period, contains at least one additional
character, and may be followed by any other characters.

Arithmetic expansion uses the following form:

    $((expression))

    $ echo $((2 + 2))

Arithmetic expansion supports only integers (whole numbers, no
decimals) but can perform quite a number of different operations.

    +, -, *, /, %, **

    netstat => check running ports

    netstat -l => show only listening ports. these are omitted by default.

Perhaps the strangest expansion is called brace expansion. With it,
you can create multiple text strings from a pattern containing
braces. Here’s an example:

    $ echo Front-{A,B,C}-Back
    > Front-A-Back Front-B-Back Front-C-Back

    $ echo w{ord,hat,eird,ack}
    > word what weird wack

Patterns to be brace expanded may contain a leading portion called a
preamble and a trailing portion called a postscript. The brace
expression itself may contain either a comma-separated list of
strings or a range of integers or single characters. The pattern may
not contain embedded whitespace.

    $ echo Number_{1..5}0
    > Number_10 Number_20 Number_30 Number_40 Number_50

    $ echo {Z..A}
    > Z Y X W V U T S R Q P O N M L K J I H G F E D C B A

    $ echo a{A{1,2},B{3,4}}b
    > aA1b aA2b aB3b aB4b

    $ echo $USER   #The $USER variable contains the current user's name
    > lym

To see a list of available variables, try this:

    $ printenv | less

Note: the `$` sign preceeds variables.

    $ file $(ls /usr/bin/* | grep zip)

In this example, the results of the pipeline became the argument list
of the file command. There is an alternative syntax for command
substitution in older shell programs that is also supported in bash.
It uses back quotes instead of the dollar sign and parentheses:

    $ ls -l `which cp`

Examples of Expansions:
 - pathname expansion
the mechanism by which wildcards work.
e.g. `ls`, `echo D*`, `echo *s`, `echo [[:upper:]]*`
 - tilde expansion
When used at the beginning of a word, it expands into the name of the
home directory of the named user or, if no user is named, the home
directory of the current user:
e.g. 
    $ echo ~ 
    > /home/me

    $ echo ~foo
    > /home/foo

 - Arithmetic Expansion
The shell allows arithmetic to be performed by expansion. This allows us to use the shell prompt as a calculator:
e.g.
    $ echo $((2 + 2))

- Brace expansion
    $ echo Front-{A,B,C}-Back

 - Parameter expansion
e.g.
    $ echo $USER, echo $PATH

Command substitution allows us to use the output of a command as an
expansion:
    $ file $(ls /usr/bin/* | grep zip)

If you place text inside double quotes, all the special characters
used by the shell lose their special meaning and are treated as
ordinary characters.

The exceptions are $ (dollar sign), \ (backslash), and  <code> ` </code> (back tick).
This means that word splitting, pathname expansion, tilde expansion,
and brace expansion are suppressed, but parameter expansion,
arithmetic expansion, and command substitution are still carried out.

If we need to suppress all expansions, we use single quotes. Here is
a comparison of unquoted, double quotes, and single quotes:

    $ echo text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER
    > text /home/me/ls-output.txt a b foo 4 me

    $ echo "text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER"
    > text ~/*.txt {a,b} foo 4 me

    $ echo 'text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER'
    > text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER

Sometimes we want to quote only a single character. To do this, we
can precede a character with a backslash, which in this context is
called the escape character. Often this is done inside double quotes
to selectively prevent an expansion.

    $ echo "The balance for user $USER is: \$5.00"
    > The balance for user me is: $5.00

It is also common to use escaping to eliminate the special meaning of
a character in a filename. For example, it is possible to use
characters in filenames that normally have special meaning to the
shell. These would include $, !, &, (a space), and others. To include
a special character in a filename, you can do this:

    $ mv bad\&filename good_filename

To allow a backslash character to appear, escape it by typing \\.
Note that within single quotes, the backslash loses its special
meaning and is treated as an ordinary character.

In addition to its role as the escape character, the backslash is
also used as part of a notation to represent certain special
characters called control codes. The first 32 characters in the ASCII
coding scheme are used to transmit commands to teletype-like devices.
Some of these codes are familiar (tab, backspace, line-feed, and
carriage return), while others are not (null, end-of-transmission,
and acknowledge), as shown in Table 7-2.

Escape Sequence | Meaning
    \a				 Bell ("alert"- causes the computer to beep)
    \b				 BAckspace
    \n				 Newline (on Unix-like systems, this = linefeed)
    \r				 Cariage Return
    \t				 Tab

This table lists some of the common backslash escape sequences. The
idea behind using the backslash originated in the C programming
language and has been adopted by many others, including the shell.
Adding the -e option to echo will enable interpretation of escape
sequences. You may also place them inside $' '. Here, using the sleep
command, a simple program that just waits for the specified number of
seconds and then exits, we can create a primitive countdown timer.

    sleep 10; echo -e "Time's up\a"

We could also do this:

    sleep 10; echo "Time's up" $'\a'

hard links are in the form:
    $ ln [file_to_be_referenced] [refrensing_file]

To check if two files are the same (linked, hard or soft), we check for the inode number:

    $ ls -li [directories_to_be_checked]

    $ ln -s ../Orders.txt ./New\ folder/ord.txt
When creating symbolic links, the 1st argument(file to be linked to),
should be provided with an absolute pathname or pathname relative to
the linking file as shown above. ord.txt is the link so order.txt is
provided as ../Orders.txt (reletave to ord.txt).
 
    lrwxrwxrwx 1 root root 11 2012-08-11 07:34 libc.so.6 -> libc-2.6.so

    $ history => to see a list of your recent terminal entries.

The Readline documentation uses the terms killing and yanking to
refer to what we would commonly call cutting and pasting. Table 8-3
lists the commands for cutting and pasting. Items that are cut are
 stored in a buffer called the kill-ring.

    $ set | less

As we discovered in Chapter 1, bash maintains a history of commands
that have been entered. This list of commands is kept in your home
directory in a file called .bash_history. 

By default, bash stores the last 500 commands you have entered.

Let’s say we want to find the commands we used to list /usr/bin. Here
is one way we could do this:

    $ history | grep /usr/bin

If say, the number 88 is the line number of the command in the
history list, we could use this immediately with another type of
expansion called history expansion. To use our discovered line, we
could do this:

    $ !88

bash will expand `!88` into the contents of the 88th line in the
history list.

bash will expand !88 into the contents of the 88th line in the
history list. We will cover other forms of history expansion a little
later. bash also provides the ability to search the history list
incrementally. This means that we can tell bash to search the history
list as we enter characters, with each additional character further
refining our search. To start an incremental search, enter CTRL-R
followed by the text you are looking for. When you find it, you can
either press ENTER to execute the command or press CTRL-J to copy the
line from the history list to the current command line.
To find the next occurrence of the text (moving “up” the history
list), press CTRL-R again. To quit searching, press either CTRL-G or
`CTRL-C`.

The prompt changes to indicate that we are performing a reverse
incremental search. It is “reverse” because we are searching from
"now" to sometime in the past. Next, we start typing our search text.

In addition to the command history feature in bash, most Linux
distributions include a program called script, which can be used to
record an entire shell session and store it in a file. The basic
syntax of the command is

	script [file]

where file is the name of the file used for storing the recording. If no file is specified, the file typescript is used. 

To find out information about your identity, use the `id` command:
    $ id
    > uid=1000(lym) gid=1000(lym) groups=1000(lym),4(adm),20(dialout),24(cdrom),46(plugdev),116(lpadmin),118(admin),124(sambashare)

Fedora starts its numbering of regular user accounts at 500, while
Ubuntu starts at 1000. We can also see that the Ubuntu user belongs
to a lot more groups. This has to do with the way Ubuntu manages
privileges for system devices and services.

So where does this information come from? Like so many things in
Linux, it comes from a couple of text files. User accounts are
defined in the `/etc/passwd` file, and groups are defined in the
`/etc/group` file. When user accounts and groups are created, these
files are modified along with `/etc/shadow`, which holds information
about the user’s password. For each user account, the `/etc/passwd`
file defines the user (login) name, the uid, the gid, the account’s
real name, the home directory, and the login shell.
If you examine the contents of /etc/passwd and /etc/group, you will
notice that besides the regular user accounts there are accounts for
the superuser (uid 0) and various other system users.
In Chapter 10, when we cover processes, you will see that some of
these other “users” are, in fact, quite busy.
While many Unix-like systems assign regular users to a common group
such as users, modern Linux practice is to create a unique,
single-member group with the same name as the user. This makes
certain types of permission assignment easier.

Access rights to files and directories are defined in terms of read
access, write access, and axecution access. If we look at the output
of the ls command, we can get some clue as to how this is implemented.

    $ > foo.txt
    $ ls -l foo.txt
    -rw-rw-r-- 1 me	  me 0 2012-03-06 14:52 foo.txt

The first 10 characters of the listing are
the file attributes (see Figure 9-1). The first
of these characters is the file type. Table 9-1
lists the file types you are most likely to see
(there are other, less common types too).
The remaining nine characters of the
file attributes, called the file mode, represent
the read, write, and execute permissions
for the file’s owner, the file’s group owner,
and everybody else.

When set, the r, w and x mode attributes have certain effects on files and directories:

    Attribute	|	Filetype
    -----------------------------------------
     - 				A regular file          |
     d				A directory             |
     l 				A symbolic link         |
     c				A special character file|
     b				A block special file    |
    -----------------------------------------

Notice that with symbolic links, the remaining file
attributes are always rwxrwxrwx and are dummy values. The real
file attributes are those of the file the symbolic link points to.

A character special file refers to a device that handles data as a
stream of bytes, such as a terminal or modem.

A block special file refers to a device that handles data in blocks,
such as a hard drive or CD-ROM drive.

Table 9-2: Permission attributes

Owner - Owner group - Everybody else
the order is always rwx e.g. -wx, r-x, rw-, ---,


    Octal | Binary | Filemode
    0		000		    ---
    1		001			--x
    2		010			-w-
    3		011			-wx
    4		100			r--
    5		101			r-x
    6		110			rw-
    7		111			rwx


### chmod — Change File Mode:

To change the mode (permissions) of a file or directory, the chmod
command is used. Be aware that only the file’s owner or the superuser
can change the mode of a file or directory. chmod supports two
distinct ways of specifying mode changes: octal number representation
and symbolic representation. 

rwx, ---,rw-, r--, r-x, -wx, --x,-w-

Octal Representation:
By using three octal digits, we can set the file mode for the owner, group owner, and world.

    $ > foo.txt
    $ ls -l foo.txt
    -rw-rw-r-- 1 lym lym 0 2012-05-25 15:52 foo.txt
    $ chmod 600 foo.txt 
    $ ls -l foo.txt 
    -rw------- 1 lym lym 0 2012-05-25 15:52 foo.txt

By passing the argument 600, we were able to set the permissions
of the owner to read and write while removing all permissions from
the group owner and world. Though remembering the octal-to-binary
mapping may seem inconvenient, you will usually have to use only a
few common ones:

    7 (rwx), 6 (rw-), 5 (r-x), 4 (r--), and 0 (---).

Symbolic Representation: 
`chmod` also supports a symbolic notation for specifying file modes.
Symbolic notation is divided into three parts: 
 -whom the change will affect,
 -which operation will be performed, 
 -and which permission will be set. 

To specify who is affected, a combination of the characters u, g, o,
and a is used, as shown in Table 9-5.

    Symbol	|	Meaning
    u			Short for user(means file or directory owner)
    g			Group owner
    o			Short for others but means world
    a			Short for all(combination of u,g and o)

If no character is specified, all will be assumed. The operation may
be a + indicating that a permission is to be added, a - indicating
that a permission is to be taken away, or a = indicating that only
the specified permissions are to be applied and that all others are
to be removed. Permissions are specified with the r, w, and x
characters. Table 9-6 lists some examples of symbolic notation.

    Notation	|	Meaning
    u + x			Add execute permission for the owner
    u-x				Remove execute permission from the owner
    +x				Add execute permission for all
    o-rw			Remove read/write permissions from world
    go=rw			Set the group owner and anyone besides the owner to have

read and write permission. If either the group owner or world
previously had execute permissions, remove them.

u+x,go=rx		Add execute permission for the owner and set the permissions
for the group and others to read and execute. Multiple speci-
fications may be separated by commas.

Symbolic notation does offer the advantage of allowing you to set a
single attribute without disturbing any of the others.

A word of caution regarding the --recursive option: It acts on both
files and directories, so it’s not as useful as one would hope
because we rarely want files and directories to have the same
permissions.

umask — Set Default Permissions
The umask command controls the default
permissions given to a file when it is
created. It uses octal notation to express
a mask of bits to be removed from a file’s
mode attributes.

    $ rm -f foo.txt
    $ umask
    0002
    $ > foo.txt
    $ ls -l foo.txt
    -rw-rw-r-- 1 lym lym 0 2012-05-25 16:41 foo.txt

We first removed any existing copy of foo.txt to make sure we were
starting fresh. Next, we ran the umask command without an argument to
see the current value. It responded with the value 0002 (the value
0022 is another common default value), which is the octal
representation of our mask. We then created a new instance of the
file foo.txt and observed its permissions. We can see that both the
owner and group get read and write permissions, while everyone else
gets only read permission. World does not have write permission
because of the value of the mask. Let’s repeat our example, this time
setting the mask ourselves:

    $ rm foo.txt
    $ umask 0000
    $ > foo.txt
    $ ls -l foo.txt
    -rw-rw-rw- 1 me  me 0 2012-03-06 14:58 foo.txt

When we set the mask to 0000 (effectively turning it off), we see
that the file is now world writable.


Changing Identities:
There are three ways to take on an
alternate identity:
 - Log out and log back in as the alternate user. 
 - Use the su command. 
 - Use the sudo command.

From within your own shell session, the su command allows you to
assume the identity of another user and either start a new shell
session with that user’s ID or issue a single command as that user.
The sudo command allows an administrator to set up a configuration
file called /etc/sudoers and define specific commands that particular
users are permitted to execute under an assumed identity. The choice
of which command to use is largely determined by which Linux
distribution you use.
Your distribution probably includes both commands, but its
configuration will favor either one or the other. We’ll start with su.

su — Run a Shell with Substitute User and Group IDs:
The su command is used to start a shell as another user. The command syntax looks like this:
    su [-[l]] [user]

If the -l option is included, the resulting shell session is a login
shell for the specified user. This means that the user’s environment
is loaded and the working directory is changed to the user’s home
directory. This is usually what we want. If the user is not
specified, the superuser is assumed. Notice that (strangely) the -l
may be abbreviated as -, which is how it is most often used. To start
a shell for the superuser, we would do this:

	$ su -

When finished, enter exit to return to the previous shell:

    # exit

sudo — Execute a Command as Another User:

The sudo command is like su in many ways but has some important
additional capabilities. The administrator can configure sudo to
allow an ordinary user to execute commands as a different user
(usually the superuser) in a very controlled way. In particular, a
user may be restricted to one or more specific commands and no
others. Another important difference is that the use of sudo does not
require access to the superuser’s password. To authenticate using
sudo, the user enters his own password. Let’s say, for example, that
sudo has been configured to allow us to run a fictitious backup
program called backup_script, which requires superuser privileges.

With `sudo` it would be done like this:

    $ sudo backup_script
    Password:
    System Backup Starting...

chown — Change File Owner and Group:

The `chown` command is used to change the owner and group owner of a
file or directory. Superuser privileges are required to use this
command. The syntax of chown looks like this:

    chown [owner][:[group]] file...

chown can change the file owner and/or the file group owner depending
on the first argument of the command. Table 9-7 lists some examples.

    Argument	| 	Result
    bob				changes ownership of the file from its current
                    user to user bob

    bob:users		Changes the ownership of the file from its
                    current owner to user bob and changes the file
                    group owner to group users.

    :admins			Changes the group owner to the group admins. The
                    file owner is unchanged.

    bob:			Changes the file owner from the current owner to
                    user bob and chnages the group owner to the login
                    group of user bob.

Let’s say that we have two users: janet, who has access to superuser
privileges, and tony, who does not. User janet wants to copy a file
from her home directory to the home directory of user tony. Since
user janet wants tony to be able to edit the file, janet changes the
ownership of the copied file from janet to tony:

    $ sudo cp myfile.txt ~tony
    Password:
    $ sudo ls -l ~tony/myfile.txt
    -rw-r--r-- 1 root root 8031 2012-03-20 14:30 /home/tony/myfile.txt
    $ sudo chown tony: ~tony/myfile.txt
    $ sudo ls -l ~tony/myfile.txt
    -rw-r--r-- 1 tony tony 8031 2012-03-20 14:30 /home/tony/myfile.txt

Here we see user janet copy the file from her directory to the home
directory of user tony. Next, janet changes the ownership of the file
from root (a result of using sudo) to tony. Using the trailing colon
in the first argument, janet also changed the group ownership of the
file to the login group of tony, which happens to be group tony.
Notice that after the first use of sudo, janet was not prompted for
her password? This is because sudo, in most configurations, "trusts"
you for several minutes (until its timer runs out).

chgrp — Change Group Ownership:
In older versions of Unix, the chown command changed only file
ownership, not group ownership. For that purpose a separate command,
chgrp, was used. It works much the same way as chown, except for
being more limited.

Exercising Your Privileges:

Changing Your Password:
The last topic we’ll cover in this chapter is setting passwords for
yourself (and for other users if you have access to superuser
privileges). To set or change a password, the passwd command is used.
The command syntax looks like this:

    passwd [user]

To change your password, just enter the passwd command. You will be
prompted for your old password and your new password:

    $ passwd
    (current) UNIX password:
    New UNIX password:

The passwd command will try to enforce use of “strong” passwords.
This means it will refuse to accept passwords that are too short, are
too similar to previous passwords, are dictionary words, or are too
easily guessed:

    $ passwd
    (current) UNIX password:
    New UNIX password:
    BAD PASSWORD: is too similar to the old one
    New UNIX password:
    BAD PASSWORD: it is WAY too short
    New UNIX password:
    BAD PASSWORD: it is based on a dictionary word

If you have superuser privileges, you can specify a username as an
argument to the passwd command to set the password for another user.
Other options are available to the superuser to allow account locking,
password expiration, and so on.

As a general guideline,
Passwords should consist of 6 to 8 characters including one or more
characters from each of the following sets:

 · lower case alphabetics

 · digits 0 thru 9

 · punctuation marks

Care must be taken not to include the system default erase or kill
characters.  passwd will reject any password which is not suitably
complex.

Note that the -l option or locking an account's password does not
deactivate the account as the user can still login using other
authentication tokens for example ssh.

To disable the account, administrators should use usermod
           --expiredate 1 (this set the account's expire date to Jan 2, 1970).

    $ passwd -S
    -S, --status
    Display account status information. The status information consists
    of 7 fields. The first field is the user's login name. The second
    field indicates if the user account has a locked password (L), has
    no password (NP), or has a usable password (P). The third field
    gives the date of the last password change. The next four fields
    are the minimum age, maximum age, warning period, and inactivity
    period for the password. These ages are expressed in days.


FILES
    /etc/passwd
       User account information.

    /etc/shadow
       Secure user account information.

    /etc/pam.d/passwd
       PAM configuration for passwd.



Modern operating systems are usually multitasking,
meaning that they create the illusion of doing more
than one thing at once by rapidly switching from one
executing program to another. The Linux kernel
manages this through the use of processes. Processes
are how Linux organizes the different programs wait-
ing for their turn at the CPU.

ps — Report a snapshot of current processes.
top — Display tasks.
jobs — List active jobs.
bg — Place a job in the background.
fg — Place a job in the foreground.
kill — Send a signal to a process.
killall — Kill processes by name.
shutdown — Shut down or reboot the system.

Daemon programs are those that just sit in the background and do
their own thing without userface. So even if we are not logged in,
the system is at least a little busy performing routine stuff.

PIDs are assigned in ascending order with init always getting PID 1. 

Viewing Processes with ps:
*TTY* is short for teletype and refers to the controlling terminal
for the process. 

    $ ps  

By default ps doesn’t show us very much, just the processes
associated with the current terminal session. 

    $ ps x

Adding the x option (note that there is no leading dash) tells ps to show all of our processes regardless of what terminal (if any) they are controlled by. The presence of a ? in the TTY column indicates no controlling terminal. Using this option, we see a list of every process that we own.

A new column titled STAT has been added to the output. STAT is short for state and reveals the current status of the process, as shown in Table 10-1

Process States:
R	: Running. The process is running or ready to run
S	: Sleeping, The the process is not running; rather, it's 	waiting for an event, such as a keystroke or network packet.
D	: Uninteruptible sleep. Process is waiting for I/O such as a disk drive.
T	: Stopped. Process has been instructed to stop.
Z	: A defunct or "zombie" process. This is a child process that has terminated but has not been cleaned up by its parent.
<	: A high priority process. It's possible to grant more importance to a process, giving it more time on the CPU. This property of a process is called "niceness" A process with high priority is said to be less nice because its taking more of the CPU's time, which leaves less for everybody else.
N	: A low-priority process. A process with low priority (a nice process) will get process time only after other processes with higher priority have been serviced.

Another popular set of options is aux (without a leading dash). This gives us even more information:

$ ps aux

While the ps command can reveal a lot about what the machine is doing, it provides only a snapshot of the machine’s state at the moment the ps command is executed. To see a more dynamic view of the machine’s activity, we use the top command:

$ top

The top program displays a continuously updating (by default, every
3 seconds) display of the system processes listed in order of process activity.

Its name comes from the fact that the top program is used to see the “top” processes on the system. The top display consists of two parts: a system summary at the top of the display, followed by a table of processes sorted by CPU activity:

page 131 => sample top results and explanation

The xlogo program is a sample program supplied with the
X Window System (the underlying engine that makes the graphics on our display go), which simply displays a resizable window containing the X logo.

To launch a program so that it is immediately placed in the back-
ground, we follow the command with an ampersand character (&):

$ xlogo &

After the command was entered, the xlogo window appeared and the
shell prompt returned, but some funny numbers were printed too. This message is part of a shell feature called job control. With this message, the shell is telling us that we have started job number 1 ([1]) and that it has PID 28236. 

The shell’s job control facility also gives us a way to list the jobs that have been launched from our terminal. Using the jobs command, we can see the following list:

$ jobs

The results show that we have one job, numbered 1, that it is running, and that the command was xlogo &.

A process in the background is immune from keyboard input, including any attempt to interrupt it with a CTRL-C. To return a process to the foreground, use the fg command, as in this example:

$ fg %1

Note_2_self:
Start a job in the background by calling it with &. To bring it back to the foreground, chack for its job number using the "job" command then hit $ fg %[job_no] e.g. fg %1

Sometimes we’ll want to stop a process without terminating it. This is often done to allow a foreground process to be moved to the background. To stop a foreground process, type CTRL-Z.

We can either restore the program to the foreground, using the fg
command, or move the program to the background with the bg command:
$ bg %1

Moving a process from the foreground to the background is handy if we launch a graphical program from the command but forget to place it in the background by appending the trailing &.

kill [-signal] PID...

if no signal is specified on the command line, then the TERM (Terminate) signal is sent by default.


While this is all very straightforward, there is more to it. The kill command doesn’t exactly “kill” processes; rather it sends them signals. Signals are one of several ways that the operating system communicates with programs. We have already seen signals in action with the use of CTRL-C and CTRL-Z. When the terminal receives one of these keystrokes, it sends a signal to the program in the foreground. In the case of CTRL-C, a signal called INT
(Interrupt) issent; with CTRL-Z, a signal called TSTP (Terminal Stop) is sent. Programs, in turn, “listen” for signals and may act upon them as they are received. The fact that a program can listen and act upon signals allows it to do things like save work in progress when it is sent a termination signal.

page 136 => kill options

Note that signals may be specified either by number or by name,
including the name prefixed with the letters SIG :

$ kill -1 [PID]

$ kill -HUP [pid]

$ kill -SIGHUP [pid]

You can also use jobspecs(with % sign) in place of pids.

Processes, like files, have owners, and you must be the owner of a process (or the superuser) in order to send it signals with kill.

Due to shell aliases and built-in `kill' command, using an unadorned `kill' interactively or in a script may get you different functionality than that described here.  Invoke it via `env' (i.e., `env kill ...') to avoid interference from the shell.


a complete list of signals can be seen with the following
command:
$ kill -l

t’s also possible to send signals to multiple processes matching a specified program or username by using the killall command. Here is the syntax:
killall [-u user] [-signal] name...

pstree: Outputs a process list arranged in a tree-like pattern showing the parent/child relationships between processes.

vmstat: Outputs a snapshot of system resource usage including
memory, swap, and disk I/O. To see a continuous display,
follow the command with a time delay (in seconds) for updates
(e.g., vmstat 5). Terminate the output with CTRL-C.

xload: A graphical program that draws a graph showing system load
over time.

tload: Similar to the xload program, but draws the graph in the
terminal. Terminate the output with CTRL-C.



Note_2_self: ps takes a snapshot of the system at the time "ps" is called while top shows a continuously changing list (refreshing every 3 seconds by default)

/proc => where ps reads from.




The shell maintains a body of information during our shell session called the environment. Data stored in the environment is used by programs to determine facts about our configuration.
While most programs use config files to store program settings, some programs will also look for values stored in the environment to adjust their behaviour.
Knowing this, we can use the envirinment to customize our shell experience.

We'll be looking at the following commands:
 - printenv : Print part or all the environment.
 - set : Set shell options
 - export : Export environment to subsequently executed programs.
 - alias : Create an alias for a command.

The shell stores two basic types of data in the environment, although, with
bash, the types are largely indistinguishable. They are environment variables
and shell variables. Shell variables are bits of data placed there by bash, and
environment variables are basically everything else. In addition to variables,
the shell also stores some programmatic data, namely aliases and shell func-
tions.

Examining the Environment:
To see what is stored in the environment, we can use either the set built in
bash or the printenv program. The set command will show both the shell and
environment variables, while printenv will display only the latter. Since the
list of environment contents will be fairly long, it is best to pipe the output
of either command into less:

    $ printenv | less

Note 2 self
we pipe large program outputs to less b'se less makes navigation way easier as it neatly displays stuff page by page.

The printenv command can also list the value of a specific variable.
$ printenv USER

Note_2_self:
$ printenv USER == $echo $USER
ttyl == teletype
The set command, when used without options or arguments, will dis-
play both the shell and environment variables, as well as any defined shell
functions.

$ set | less

Unlike printenv, its output is courteously sorted in alphabetical order.
One element of the environment that neither set nor printenv displays is
aliases. To see them, enter the alias command without arguments:

$ alias

Some Interesting Variables
The environment contains quite a few variables, and though your environ-
ment may differ from the one presented here, you will likely see the vari-
ables shown in Table 11-1 in your environment.

DISPLAY: The name of your display if you are running a graphical
environment. Usually this is :0, meaning the first display
generated by the X server.

EDITOR: The name of the program to be used for text editing.

SHELL: The name of your shell program(bash for ubuntu 11.10)

HOME: The pathname of your home directory.

LANG: Defines the character set and collation order of your language.

OLD_PWD: The previous working directory.

PAGER: The name of the program to be used for paging output. This is often set to /usr/bin/less

PATH: A colon-separated list of directories that are searched when you enter the name of an executable program.

PS1: Prompt String 1. This defines the contents of your shell prompt.
As we will later see, this can be extensively customized.

PWD: The current working directory

TERM: The name of your terminal type. Unix-like systems support many terminal protocols; this variable sets the protocol to be used with your terminal emulator.

TZ: Specifies your time-zone. Most Unix-like systems maintain your Computer's internal clock in Coordinated Universal Time (UTC) and then display the local time by applying an offset specified by this variable.

USER: Your username(current logged-in user).

Note: the above values varry by definition.

How Is the Environment Established?
When we log on to the system, the bash program starts and reads a series
of configuration scripts called startup files, which define the default envi-
ronment shared by all users. This is followed by more startup files in our
home directory that define our personal environment. The exact sequence
depends on the type of shell session being started.

Login and Non-login Shells:
There are two kinds of shell sessions: a login shell session and a non-login
shell session.
A login shell session is one in which we are prompted for our username and
password; for example, when we start a virtual console session. A non-login
shell session typically occurs when we launch a terminal session in the GUI.
Login shells read one or more startup files, as shown in Table 11-2.

/etc/profile : A global configuration script that applies to all users.

~/.bash_profile : A user's personal startup file. Can be used to extend or override settings in the global configuration script.

~/.bash_login : If ~/.bash_profile is not found, bash attempts to read this script.

~/.profile : If neither ~/.bash_profile nor ~/.bash_login is found, bash attempts to read this file. This is the default in debian-based distributions, such as Ubuntu.

Non-login shell sessions read the startup files as shown in Table 11-3.(below)

/etc/bash.bashrc : A global configuration script that applies to all users.
~/.bashrc : A user's personal startup file. Can be used to extend ir override settings in the global configuration script.

In addition to reading the startup files above, non-login shells inherit
the environment from their parent process, usually a login shell.

The ~/.bashrc file is probably the most important startup file from the
ordinary user’s point of view, since it is almost always read. Non-login shells
read it by default, and most startup files for login shells are written in such a
way as to read the ~/.bashrc file as well.

The PATH variable is often (but not always, depending on the distribu-
tion) set by the /etc/profile startup file and with this code:
PATH=$PATH:$HOME/bin


export PATH
The export command tells the shell to make the contents of PATH avail-
able to child processes of this shell.


As a general rule, to add directories to your PATH or define additional envi-
ronment variables, place those changes in .bash_profile (or equivalent,
according to your distribution—for example, Ubuntu uses .profile). For
everything else, place the changes in .bashrc. Unless you are the system
administrator and need to change the defaults for all users of the system,
restrict your modifications to the files in your home directory. It is certainly
possible to change the files in /etc such as profile, and in many cases it would
be sensible to do so, but for now let’s play it safe.

Text editors fall into two basic categories: graphical and text based.
GNOME and KDE both include some popular graphical editors. GNOME
ships with an editor called gedit, which is usually called Text Editor in
the GNOME menu. KDE usually ships with three, which are (in order of
increasing complexity) kedit, kwrite, and kate.

There are many text-based editors. The popular ones you will encounter
are nano, vi, and emacs. The nano editor is a simple, easy-to-use editor designed
as a replacement for the pico editor supplied with the PINE email suite. The
vi editor (on most Linux systems replaced by a program named vim, which is
short for Vi IMproved) is the traditional editor for Unix-like systems.

Whenever we edit an important configura-
tion file, it is always a good idea to create a backup copy of the file first. This
protects us in case we mess the file up while editing. To create a backup of
the .bashrc file, do this:
$ cp .bashrc .bashrc.bak

The extensions .bak, .sav, .old, and .orig are all popular ways of
indicating a backup file. Oh, and remember that cp will overwrite existing files
silently.

In nano ^X stands for Ctr + x

The screen consists of a header at the top, the text of the file being
edited in the middle, and a menu of commands at the bottom. Since nano
was designed to replace the text editor supplied with an email client, it is
rather short on editing features.

The notation ^X means CTRL-X. This is
a common notation for the control characters used by many programs.

In Nano we press Ctr + O to save.

umask 0002
export HISTCONTROL=ignoredups
export HISTSIZE=1000
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'

Explanation:
Umask 0002 : Sets the umask to solve the problem with
shared directories we discussed in
Chapter 9.

export HISTCONTROL=ignoredups : Causes the shell’s history recording
 feature to ignore a command if the same
command was just recorded.

export HISTSIZE=1000 : Increases the size of the command history
from the default of 500 lines to 1000
lines.

alias l.='ls -d .* --color=auto' : Creates a new command called l.,
which displays all directory entries that
begin with a dot.

alias ll='ls -l –color=auto' : Creates a new command called ll,
which displays a long-format directory
listing.

As we can see, many of our additions are not intuitively obvious, so it
would be a good idea to add some comments to our .bashrc file to help
explain things to the humans. Using the editor, change our additions to
look like this:

# Change umask to make directory sharing easier
umask 0002

# Ignore duplicates in command history and increase
# history size to 1000 lines
export HISTCONTROL=ignoredups
export HISTSIZE=1000

# Add some helpful aliases
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'


Activating Our Changes:
The changes we have made to our .bashrc will not take effect until we close
our terminal session and start a new one, because the .bashrc file is only read
at the beginning of a session. However, we can force bash to reread the mod-
ified .bashrc file with the following command:
$ source .bashrc

Shell scripts and bash startup files use a # symbol to begin a comment.
Other configuration files may use other symbols. Most configuration files will
have comments. Use them as a guide.

as we read man pages for commands, take
note of the environment variables that commands support. There may be a
gem or two. 



note 2 self:
Graphical text editors accomodate the use of a mouse, text based
editors like nano, vi and emacs do NOT.

POSIX, a standard for program compatibility on Unix systems,
requires that vi be present.

In the discussions that follow, we will assume that we have a program
called vi that is really vim.

vim = vi Improved

vi was written by Billy joy in 1976, later went on to found Sun Micro Systems
vim is done by Bran Moolenaar

We quit vi by hitting:
    :q
The shell prompt should return. If, for some reason, vi will not quit
(usually because we made a change to a file that has not yet been saved),
we can tell vi that we really mean it by adding an exclamation point to the
command:
    :q!

Note: If you get "lost" in vi, try pressing the ESC key twice to find
your way again.

The leading tilde characters (~) indicate that no text exists on that line.
This shows that we have an empty file.
The second most important thing to learn about vi (after learning how
to exit) is that vi is a modal editor. When vi starts up, it begins in command
mode. In this mode, almost every key is a command, so if we were to start typ-
ing, vi would basically go crazy and make a big mess.

note 2 self:
vi modes:
 - command mode (default)
 - insert mode (press i to enter)
 - 

Entering Insert Mode:
In order to add some text to our file, we must first enter insert mode. To do
this, we press the I key (i). Afterward, we should see the following at the bot-
tom of the screen if vim is running in its usual enhanced mode (this will not
appear in vi-compatible mode):
--INSERT--

To exit insert mode and return to command mode, press the ESC key.

Saving Our Work:
To save the change we just made to our file, we must enter an ex command
while in command mode. This is easily done by pressing the : key. After
doing this, a colon character should appear at the bottom of the screen:
:

To write our modified file, we follow the colon with a w, then ENTER:
:w

The file will be written to the hard drive, and we should get a confirma-
tion message at the bottom of the screen

Note: If you read the vim documentation, you will notice that 
(confusingly) command mode is called normal mode and ex commands are
called command mode. Beware.

COMPATIBILITY MODE:
In the example startup screen shown at the beginning of this section (taken
from Ubuntu 8.04), we see the text Running in Vi compatible mode. This means
that vim will run in a mode that is closer to the normal behavior of vi rather
than the enhanced behavior of vim. For purposes of this chapter, we will want
to run vim with its enhanced behavior. To do this, you have a couple of options:
Try running vim instead of vi (if that works, consider adding alias vi='vim'
to your .bashrc file).
Use this command to add a line to your vim configuration file:
echo "set nocp" >> ~/.vimrc
Different Linux distributions package vim in different ways. Some distribu-
tions install a minimal version of vim by default that supports only a limited set
of vim features. While performing the lessons that follow, you may encounter
missing features. If this is the case, install the full version of vim.

Moving the Cursor Around:
While it is in command mode, vi offers a large number of movement com-
mands, some of which it shares with less. Table 12-1 lists a subset.

L or right arrow : Right one character
H or left arrow : Left one character
J or down arrow : Down one line
K or up arrow : Up one line
0 (zero) : To the beginning of the current line
SHIFT-6 (^) : To the first non-whitespace character on the current line.
SHIFT-4 ($) : To the end of the current line.
W : To the beginning of the next word or punctuation character.
SHIFT-W (W) : To the beginning of the next word, ignoring punctu
ation characters.
B : To the beginning of the previous word or punctuation character.
SHIFT-B ( B) : To the beginning of the previous word, ignoring punctuation characters.
CTRL-F or PAGE DOWN : Down one page.
CTRL-B or PAGE UP : Up one page.
number-SHIFT-G : To line number (for example, 1G moves to the first line
of the file)
SHIFT-G (G) : To the last line of the file
 
Why are the H, J, K, and L keys used for cursor movement? Because
when vi was originally written, not all video terminals had arrow keys, and
skilled typists could use regular keyboard keys to move the cursor without
ever having to lift their fingers from the keyboard.
Many commands in vi can be prefixed with a number, as with the G
command listed in Table 12-1. By prefixing a command with a number,
we may specify the number of times a command is to be carried out. For
example, the command 5j causes vi to move the cursor down five lines.

If we press the U key while in command mode, vi will undo
the last change that you made. 

If we wanted to add some text to the end of this sentence, we would dis-
cover that the i command will not do it, because we can’t move the cursor
beyond the end of the line. vi provides a command to append text, the sens-
ibly named a command. If we move the cursor to the end of the line and
type a, the cursor will move past the end of the line, and vi will enter insert
mode.

vi offers a shortcut to move to the end of the current line and start appending.
It’s the A command. 

First, we’ll move the cursor to the beginning of the line using the 0
(zero) command.

Opening a Line:
Another way we can insert text is by “opening” a line. This inserts a blank
line between two existing lines and enters insert mode. This has two
variants, as shown in Table 12-2.
o : The line below the current line
O : The line above the current line

Deleting Text:
As we might expect, vi offers a variety of ways to delete text, all of which
contain one of two keystrokes. First, the X key will delete a character at the
cursor location. x may be preceded by a number specifying how many char-
acters are to be deleted. The D key is more general purpose. Like x, it may
be preceded by a number specifying the number of times the deletion is
to be performed. In addition, d is always followed by a movement command
that controls the size of the deletion. Table 12-3 lists some examples.
Place the cursor on the word It on the first line of our text. Type x
repeatedly until the rest of the sentence is deleted. Next, type u repeatedly
until the deletion is undone.

Note: Real vi supports only a single level of undo. vim supports multiple levels.

Text Deletion Commands:
x : The current character
3x : The current character and the next two characters.
dd : The current line
5dd : The current line and the next four lines.
dW : From the current cursor location to the beginning of the
next word
d$ : From the current cursor location to the end of the current line
d0 : From the current cursor location to the beginning of the line
d^ : From the current cursor location to the first non-whitespace
character in the line
dG : From the current line to the end of the file
d20G : From the current line to the 20th line of the file.

Cutting, Copying, and Pasting Text:
The d command not only deletes text, it also “cuts” text. Each time we use
the d command, the deletion is copied into a paste buffer (think clipboard)
that we can later recall with the p command to paste the contents of the buf-
fer after the cursor or with the P command to paste the contents before the
cursor.

The y command is used to “yank” (copy) text in much the same way the
d command is used to cut text. Table 12-4 lists some examples combining
the y command with various movement commands.

Yanking Commands
yy : The current line
5yy : The current line and the next four lines
yW : From the current cursor location to the beginning of the next word.
y$ : From the current cursor location to the end of the current line.
y0 : From the current cursor location to the beginning of the current line.
y^ : From the current cursor location to the first non-whitespace character
in the line.
yG : From the current line to the end of the file
y20G : From the current line to the 20th line of the file.

Joining Lines:
vi is rather strict about its idea of a line. Normally, it is not
possible to move
the cursor to the end of a line and delete the end-of-line character to join
one line with the one below it. Because of this, vi provides a specific com-
mand, J (not to be confused with j, which is for cursor movement), to join
lines together.
If we place the cursor on line 3 and type the J command, here’s what
happens:

The quick brown fox jumped over the lazy dog. It was cool.
Line 2
Line 3 Line 4
Line 5

Search and Replace
vi has the ability to move the cursor to locations based on searches. It can
do this on either a single line or over an entire file. It can also perform text
replacements with or without confirmation from the user.
Searching Within a Line
The f command searches a line and moves the cursor to the next instance
of a specified character. For example, the command fa would move the
cursor to the next occurrence of the character a within the current line.
After performing a character search within a line, the search may be
repeated by typing a semicolon.
Searching the Entire File
To move the cursor to the next occurrence of a word or phrase, the / com-
mand is used. This works the same way as in the less program we covered in
Chapter 3. When you type the / command, a forward slash will appear at the
bottom of the screen. Next, type the word or phrase to be searched for, fol-
lowed by the ENTER key. The cursor will move to the next location contain-
ing the search string. A search may be repeated using the previous search
string with the n command. Here’s an example:

The quick brown fox jumped over the lazy dog. It was cool.
Line 2
Line 3
Line 4
Line 5

Place the cursor on the first line of the file. Type
/Line

followed by the ENTER key. The cursor will move to line 2. Next, type n,
and the cursor will move to line 3. Repeating the n command will move the
cursor down the file until it runs out of matches. While we have so far used
only words and phrases for our search patterns, vi allows the use of regular
expressions, a powerful method of expressing complex text patterns. 

Global Search and Replace:
vi uses an ex command to perform search-and-replace operations (called
substitution in vi) over a range of lines or the entire file. To change the word
Line to line for the entire file, we would enter the following command:
:%s/Line/line/g

Let’s break this command down into separate items and see what each
one does
An Example of Global Search-and-Replace Syntax
: The colon caharacter starts an ex command.
% : Specifies the range of lines for the operation. % is a shortcut
meaning from the first line to the last line. Alternatively, the range
could have been specified 1,5 (because our file is five lines long),
or 1,$, which means from line 1 to the last line in the file. If the
range of lines is omitted, the operation is performed only on the
current line.
s : Specifies the operation - in this case, substitution (search and replace)
/Line/line : The search pattern and the replacement text.
g : This means global, in the sense that the substitution is
performed on every instance of the search string in each line. If g
is omitted, only the first instance of the search string on each line
is replaced.

After executing our search-and-replace command, our file looks like this:
The quick brown fox jumped over the lazy dog. It was cool.
line 2
line 3
line 4
line 5

We can also specify a substitution command with user confirmation.
This is done by adding a c to the end of the command. For example:

:%s/line/Line/gc

This command will change our file back to its previous form; however,
before each substitution, vi stops and asks us to confirm the substitution
with this message:
replace with Line (y/n/a/q/l/^E/^Y)?
Each of the characters within the parentheses is a possible response:
y : Perform the substitution.
n : Skip this instance of the pattern
a : Perform the substitution on this and all subsequent instances of the pattern.
q or ESC : Quit substituting.
l : perform this substitution and then quit. Short for last.
CTR-E, CTR-Y : Scroll down and scroll up, respectively. Useful for
viewing the context of the proposed substitution.

Editing Multiple Files:
It’s often useful to edit more than one file at a time. You might need to
make changes to multiple files, or you may need to copy content from one
file into another. With vi we can open multiple files for editing by specifying
them on the command line:
vi file1 file2 file3...

To switch from one file to the next, use this ex command:
:n

To move back to the previous file, use:
:N

While we can move from one file to another, vi enforces a policy that
prevents us from switching files if the current file has unsaved changes. To
force vi to switch files and abandon your changes, add an exclamation point
(!) to the command.
In addition to the switching method described above, vim (and some
versions of vi) provides some ex commands that make multiple files easier
to manage. We can view a list of files being edited with the :buffers com-
mand. Doing so will display a list of the files at the bottom of the display:

:buffers
1 %a "foo.txt"
line 1
2
"ls-output.txt"
line 0
Press ENTER or type command to continue

To switch to another buffer (file), type :buffer followed by the number
of the buffer you wish to edit. For example, to switch from buffer 1, which
contains the file foo.txt, to buffer 2, which contains the file ls-output.txt, we
would type this:
:buffer 2

and our screen now displays the second file.

It’s also possible to add files to our current editing session. The ex com-
mand :e (short for edit) followed by a filename will open an additional file.

Note: You cannot switch to files loaded with the :e command using
either the :n or :N command. To switch files, use the :buffer command
followed by the buffer number.

Copying Content from One File into Another
Often while editing multiple files, we will want to copy a portion of one file
into another file that we are editing. This is easily done using the usual yank
and paste commands we used earlier.

Inserting an Entire File into Another:
It’s also possible to insert an entire file into one that we are editing.

:r foo.txt
The :r command (short for read) inserts the specified file before the
cursor position. Our screen should now look like this:

Saving Our Work:
Like everything else in vi, there are several ways to save our edited files. We
have already covered the ex command :w, but there are some others we may
also find helpful.
In command mode, typing ZZ will save the current file and exit vi. Like-
wise, the ex command :wq will combine the :w and :q commands into one
that will both save the file and exit.
The :w command may also specify an optional filename. This acts like a
Save As command. For example, if we were editing foo.txt and wanted to save
an alternative version called foo1.txt, we would enter the following:

:w foo1.txt

Note: While this saves the file under a new name, it does not change the name of the file you
are editing. As you continue to edit, you will still be editing foo.txt, not foo1.txt.


Anatomy of a Prompt
Our default prompt looks something like this:
	lym@salym-dev-box:~$

Notice that it contains our username, our hostname, and our current
working directory, but how did we get that anyway? Very simply, it
turns out that the prompt is defined by an environment variable named
PS1 (prompt String 1). We can view the contents of PS1 with the echo
command:
    lym@salym-dev-box:~$ echo $PS1
    > \[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\u@\h:\w\$

Every Linux distribution defines the prompt string a little
differently, some quite exotically.

From the results, we can see that PS1 contains a few of the characters we
see in our prompt, such as the square brackets, the @ sign, and the dollar
sign, but the rest are a mystery. Table 13-1 is a partial list of the
characters that the shell treats specially in the prompt string.

    Table 13-1: Escape Codes Used in Shell Prompts
    \a : ASCII bell. This makes the computer beep when it's encountered.

    \d : Current date in day, month, date format; for example "Mon May 26"

    \h : Hostname of the local muchine minus the trailing domain name.

    \H : Full Hostname

    \j : Number of jobs running in the current shell session.

    \l : Name of the current terminal service.

    \n : A newline character.

    \r : A carriage return.

    \s : Name of the shell program.

    \t : Current time in 24-hour, hours:minutes:seconds format.

    \T : Current time in 12-hour format.

    \@ : Current time in 12-hour AM/PM format.

    \A : Current time in 24-hour, hours:minutes format

    \u : Username of the current user.

    \v : Version Number of the shell.

    \V : Version and release numbers of the shell.

    \w : Name of the current working directory.

    \W : Last part of the current working-directory name.

    \! : History number of the current command.

    \# : Number of commands entered during this shell session.

    \$ : This displays a "$" character unless you have super-user privileges. In that case it displays a '#' instead.

    \[ : This signals the start of a series of one or more
        non-printing characters. Its used to embed non-printing
        control characters that minipulate the terminal emulator in some way, such as moving the cursor or changing text colors.

    \] : This signals the end of a non-printing character sequence.

Trying Some Alternative Prompt Designs:

With this list of special characters, we can change the prompt to see the
effect. First, we’ll back up the existing string so we can restore it later. To
do this, we will copy the existing string into another shell variable that we
create ourselves:

    $ ps1_old="$PS1"

We create a new variable called `ps1_old` and assign the value of PS1 to it.
We can verify that the string has been copied by using the echo command:

    $ echo $ps1_old
    > \[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\u@\h:\w\$

We can restore the original prompt at any time during our terminal ses-
sion by simply reversing the process:

    $ PS1="$ps1_old"

Now that we are ready to proceed, let’s see what happens if we have an
empty prompt string:
    $PS1= 

If we assign nothing to the prompt string, we get nothing. No prompt
string at all! The prompt is still there but displays nothing, just
as we asked it to. Since this is kind of disconcerting to look at,
we’ll replace it with a minimal prompt:

    PS1="\$ "

Notice the trailing space within the double quotes. This provides the
space between the dollar sign and the cursor when the prompt is
displayed. Let’s add a bell to our prompt:

	$ PS1="\a\$ "

Now we should hear a beep each time the prompt is displayed. This
could get annoying, but it might be useful if we needed notification when
an especially long-running command has been executed.
Next, let’s try to make an informative prompt with some hostname and
time-of-day information:

    $ PS1="\A \h \$ "

Adding Color
Most terminal emulator programs respond to certain non-printing character
sequences to control such things as character attributes (like color, bold
text, and the dreaded blinking text) and cursor position. 

TERMINAL CONFUSION
Back in ancient times, when terminals were hooked to remote computers,
there were many competing brands of terminals and they all worked differ-
ently. They had different keyboards, and they all had different ways of inter-
preting control information. Unix and Unix-like systems have two rather
complex subsystems (called termcap and terminfo) to deal with the babel of ter-
minal control. If you look into the deepest recesses of your terminal emulator
settings, you may find a setting for the type of terminal emulation.
In an effort to make terminals speak some sort of common language,
the American National Standards Institute (ANSI) developed a standard set
of character sequences to control video terminals. Old-time DOS users will
remember the ANSI.SYS file that was used to enable interpretation of these
codes.

Character color is controlled by sending the terminal emulator an ANSI
escape code embedded in the stream of characters to be displayed. The con-
trol code does not “print out” on the display; rather it is interpreted by the
terminal as an instruction. As we saw in Table 13-1, the `\[` and `\]` sequences
are used to encapsulate non-printing characters. An ANSI escape code begins
with an octal 033 (the code generated by the ESC key), followed by an optional
character attribute, followed by an instruction. For example, the code to set
the text color to normal (attribute = 0) black text is `\033[0;30m`.
Table 13-2 lists available text colors. Notice that the colors are divided
into two groups, differentiated by the application of the bold character
attribute (1), which creates the appearance of "light" colors.
e.g.

    PS1="\[\033[0;41m\]<\u@\h \W>\$\[\033[0m\] "

Note: Besides the normal (0) and bold (1) character attributes, text
may also be given underscore (4), blinking (5), and inverse (7)
attributes. In the interests of good taste, many terminal emulators
refuse to honor the blinking attribute.

Moving the Cursor:
Escape codes can be used to position the cursor. This is commonly used
to provide a clock or some other kind of information at a different location
on the screen, such as an upper corner, each time the prompt is drawn.
Table 13-4 lists the escape codes that position the cursor.


Using these codes, we’ll construct a prompt that draws a red bar at the
top of the screen containing a clock (rendered in yellow text) each time the
prompt is displayed. The code for the prompt is this formidable looking
string:

    PS1="\[\033[s\033[0;0H\033[0;41m\033[K\033[1;33m\t\033[0m\033[u\]<\u@\h \W>\$ "


Saving the Prompt:
Obviously, we don’t want to be typing that monster all the time, so we’ll
want to store our prompt someplace. We can make the prompt permanent
by adding it to our .bashrc file. To do so, add these two lines to the file:

    PS1="\[\033[s\033[0;0H\033[0;41m\033[K\033[1;33m\t\033[0m\033[u\]<\u@\h \W>\$ "
    export PS1



## Packaging Systems
Different distributions use different packaging systems, and as a general rule
a package intended for one distribution is not compatible with another dis-
tribution. Most distributions fall into one of two camps of packaging techno-
logies: the Debian .deb camp and the Red Hat .rpm camp. There are some
important exceptions, such as Gentoo, Slackware, and Foresight, but most
others use one of the two basic systems shown in Table 14-1.

Debian style (.deb) : Debian, Ubuntu, Xandros, Linspire
Red Hat style (.rpm) : Fedora, CentOS, Red Hat Enterprise Linux, openSUSE,
Mandriva, PCLinuxOS

## Package Files:
The basic unit of software in a packaging system is the package file. A package
file is a compressed collection of files that comprise the software package.
A package may consist of numerous programs and data files that support
the programs. In addition to the files to be installed, the package file also
includes metadata about the package, such as a text description of the
package and its contents. Additionally, many packages contain pre- and
post-installation scripts that perform configuration tasks before and after
the package installation.
Package files are created by a person known as a package maintainer,
often (but not always) an employee of the distribution vendor. The package
maintainer gets the software in source code form from the upstream provider
(the author of the program), compiles it, and creates the package metadata
and any necessary installation scripts. Often, the package maintainer will
apply modifications to the original source code to improve the program’s
integration with the other parts of the Linux distribution.

## Repositories:
While some software projects choose to perform their own packaging and
distribution, most packages today are created by the distribution vendors
and interested third parties. Packages are made available to the users of a
distribution in central repositories, which may contain many thousands of
packages, each specially built and maintained for the distribution.
A distribution may maintain several different repositories for different
stages of the software development life cycle. For example, there will usually
be a testing repository, which contains packages that have just been built and
are intended for use by brave souls who are looking for bugs before the pack-
ages are released for general distribution. A distribution will often have a
development repository where work-in-progress packages destined for inclusion
in the distribution’s next major release are kept.
A distribution may also have related third-party repositories. These
are often needed to supply software that, for legal reasons such as patents
or Digital Rights Management (DRM) anticircumvention issues, cannot
be included with the distribution. Perhaps the best-known case is that of
encrypted DVD support, which is not legal in the United States. The third-
party repositories operate in countries where software patents and anti-
circumvention laws do not apply. These repositories are usually wholly
independent of the distribution they sup-port, and to use them one must
know about them and manually include them in the configuration files for
the package management system.

## Dependencies:
Programs seldom stand alone; rather, they rely on the presence of other
software components to get their work done. Common activities, such as
input/output for example, are handled by routines shared by many programs.
These routines are stored in what are called shared libraries, which provide
essential services to more than one program. If a package requires a shared
resource such as a shared library, it is said to have a dependency. Modern
package management systems all provide some method of dependency resolu-
tion to ensure that when a package is installed, all of its dependencies are
installed, too.

## High- and Low-Level Package Tools:
Package management systems usually consist of two types of tools: low-level
tools that handle tasks such as installing and removing package files, and
high-level tools that perform metadata searching and dependency resolu-
tion. In this chapter, we will look at the tools supplied with Debian-style sys-
tems (such as Ubuntu and many others) and those used by recent Red Hat
products. While all Red Hat–style distributions rely on the same low-level
program (rpm), they use different high-level tools. For our discussion, we
will cover the high-level program yum, used by Fedora, Red Hat Enterprise
Linux, and CentOS. Other Red Hat–style distributions provide high-level
tools with comparable features (see Table 14-2).
Table14-2: Packaging System Tools:

    Distributions | Low-Level Tools | High-Level Tools
    --------------------------------------------------
    Debian style  | dpkg			| apt-get, aptitude
    --------------------------------------------------
    Fedora, RedHat| rpm				| yum
    Enterprise    |
    Linux, CentOS |
    ---------------------------------------------------
 
## Common Package Management Tasks:
Many operations can be performed with the command-line package man-
agement tools. We will look at the most common. Be aware that the low-
level tools also support creation of package files, an activity outside the
scope of this book.
In the following discussion, the term `package_name` refers to the actual
name of a package, as opposed to `package_file`, which is the name of the file
that contains the package.

## Finding a Package in a Repository:
By using the high-level tools to search repository metadata, one can locate a
package based on its name or description (see Table 14-3).
    Table 14-3: Package Search Commands:
    Style		|	Commands
    -----------------------------------------------
    Debian      |   apt-get update
                    apt-cache search [searchstring]
    Red Hat		| yum search [searchstring]
    -----------------------------------------------

## Installing a Package from a Repository:
High-level tools permit a package to be downloaded from a repository and
installed with full dependency resolution (see Table 14-4).
    Style		|	Commands
    ----------------------------------------------
    Debian		|	apt-get update
                    apt-get install [package_name]
    Red Hat		|	yum install package_name
    ----------------------------------------------

## Installing a Package from a Package File:
If a package file has been downloaded from a source other than a reposit-
ory, it can be installed directly (though without dependency resolution)
using a low-level tool (see Table 14-5).

Table 14-5: Low-Level Package Installation Commands
    Style		|	Command
    ------------------------------------------------
    Debian		|	dpkg --install [package_file]
    Red Hat		|	rpm -i [package_file]
    ------------------------------------------------

Example: If the emacs-22.1-7.fc7-i386.rpm package file has been down-
loaded from a non-repository site, install it on a Red Hat system this way:

    $ rpm -i emacs-22.1-7.fc7-i386.rpm

Note: Since this technique uses the low-level rpm program to perform
the installation, no dependency resolution is performed. If rpm
discovers a missing dependency, rpm will
exit with an error.

## Removing a Package:
Packages can be uninstalled using either the high-level or low-level tools.
The high-level tools are shown in Table 14-6.

Table14-6: Package Removal Commands
    Style		|	Command
    ----------------------------------------------
    Debian		|	apt-get remove [package_name]
    Red Hat		|	yum erase [package_name]
    ----------------------------------------------

Example: Uninstall the emacs package from a Debian-style system:

    $ apt-get remove emacs

## Updating Packages from a Repository:
The most common package management task is keeping the system up-to-
date with the latest packages. The high-level tools can perform this vital task
in one single step (see Table 14-7).

Table 14-7: Package Update Commands

    Style		|	Command
    -------------------------------------------------
    Debian		|	apt-get update; apt-get upgrade
    Red Hat		|	yum update
    -------------------------------------------------

Example: Apply any available updates to the installed packages on a
Debian-style system:

    apt-get update; apt-get upgrade

## Upgrading a Package from a Package File:
If an updated version of a package has been downloaded from a non-
repository source, it can be installed, replacing the previous version (see
Table 14-8).

Table 14-8: Low-Level Package Upgrade Commands
    Style		|	Command
    ---------------------------------------------
    Debian		|	dpkg --install [package_file]
    Red Hat		|	rpm -U [package_file]
    ---------------------------------------------

Example: Update an existing installation of emacs to the version con-
tained in the package file emacs-22.1-7.fc7-i386.rpm on a Red Hat system:
    rpm -U emacs-22.1-7.fc7-i386.rpm

Note: dpkg does not have a specific option for upgrading a package versus installing one, as rpm does.

## Listing Installed Packages:
The commands shown in Table 14-9 can be used to display a list of all the
packages installed on the system.

Table 14-9: Package Listing Commands
    Style		|	Command
    ---------------------------------
    Debian		|	dpkg --list
    Red Hat		|	rpm -qa
    ---------------------------------

## Determining Whether a Package Is Installed:
The low-level tools shown in Table 14-10 can be used to display whether a
specified package is installed.

Table 14-10: Package Status Commands
    Style		|	Command
    -----------------------------------------------
    Debian		|	dpkg --status [package_name]
    Red Hat		|	rpm -q [package_name]
    ----------------------------------------------

Example: Determine whether the emacs package is installed on a Debian-
style system:

    $ dpkg --status emacs

Note 2 self:
dpkg = debian packager

## Displaying Information About an Installed Package
If the name of an installed package is known, the commands shown in
Table 14-11 can be used to display a description of the package.

Table 14-11: Package Information Commands:
    Style		|	Command
    ---------------------------------------------
    Debian		|	apt-cache show [package_name]
    Red Hat		|	yum info [package_name]
    ---------------------------------------------

Example: See a description of the emacs package on a Debian-style
system:

    $ apt-cache show emacs

Finding Which Package Installed a File:
To determine which package is responsible for the installation of a particu-
lar file, the commands shown in Table 14-12 can be used.

Table 14-12: Package File Identification Commands
    Style		|	Command
    ------------------------------------------------
    Debian		|	dpkg --search [file_name]
    Red Hat		|	rpm -qf [file_name]
    ------------------------------------------------

Example: See which package installed the /usr/bin/vim file on a Red Hat
system:

    rpm -qf /usr/bin/vim


Linux has amazing capabilities for handling storage devices, whether
physical storage such as hard disks, network storage, or virtual
storage devices like RAID (redundant array of independent disks) and
LVM (logical volume manager).

We will look at the following commands:
 - mount : Mount a filesystem
 - umount : UNmount a filesystem
 - fdisk : Partition table manipulator
 - fsck : Check and repair a filesystem.
 - fdformat : Format a floppy disk
 - mkfs : Create a filesystem.
 - dd : Write block-oriented data directly to the device.
 - genisoimage (mkisofs) : Create an ISO 9660 image file.
 - wodim (cdrecord) : Write data to optical storage media.
 - md5sum - Calculate an MD5 checksum.

As we recall from Chapter 2, Unix-like
operating systems, like Linux, maintain a single filesystem tree with devices
attached at various points. This contrasts with other operating systems such
as MS-DOS and Windows that maintain separate trees for each device (for
example C:\, D:\, etc.).
A file named /etc/fstab lists the devices (typically hard disk partitions)
that are to be mounted at boot time.

Viewing a List of Mounted Filesystems:
The mount command is used to mount filesystems. Entering the command
without arguments will display a list of the filesystems currently mounted:

    $ mount
    /dev/sda6 on / type ext4 (rw,errors=remount-ro,commit=0)
    proc on /proc type proc (rw,noexec,nosuid,nodev)
    sysfs on /sys type sysfs (rw,noexec,nosuid,nodev)
    fusectl on /sys/fs/fuse/connections type fusectl (rw)
    none on /sys/kernel/debug type debugfs (rw)
    none on /sys/kernel/security type securityfs (rw)
    udev on /dev type devtmpfs (rw,mode=0755)
    devpts on /dev/pts type devpts (rw,noexec,nosuid,gid=5,mode=0620)
    tmpfs on /run type tmpfs (rw,noexec,nosuid,size=10%,mode=0755)
    none on /run/lock type tmpfs (rw,noexec,nosuid,nodev,size=5242880)
    none on /run/shm type tmpfs (rw,nosuid,nodev)
    binfmt_misc on /proc/sys/fs/binfmt_misc type binfmt_misc (rw,noexec,nosuid,nodev)
    gvfs-fuse-daemon on /home/lym/.gvfs type fuse.gvfs-fuse-daemon (rw,nosuid,nodev,user=lym)
    /dev/sda3 on /media/Sick type fuseblk (rw,nosuid,nodev,allow_other,blksize=4096,default_permissions)


The format of the listing is:
    [device] on [mount_point] type [filesystem_type] (options). 

note that audio CDs are not the same as CD-ROMs. Audio CDs do not
contain filesystems and thus cannot be mounted in the usual sense.

If we list the contents of the /dev directory (where all devices
live), we can see that there are lots and lots of devices:

 $ ls /dev

Table 15-2: Linux Storage Device Names
    /dev/fd* : Floppy disk drives

    /dev/hd* : IDE (PATA) disks on older systems. Typical motherboards
    contain two IDE connectors, or channels, each with a cable
    with two attachment points for drives. The first drive on
    the cable is called the master device and the second is
    called the slave device. The device names are ordered
    such that /dev/hda refers to the master device on the first
    channel, /dev/hdb is the slave device on the first channel;
    /dev/hdc, the master device on the second channel, and
    so on. A trailing digit indicates the partition number on the
    device. For example, /dev/hda1 refers to the first partition
    on the first hard drive on the system while /dev/hda refers to
    the entire drive.

    /dev/lp* : Printers

    /dev/sd* : SCSI disks. On recent Linux systems, the kernel treats all
    disk-like devices (including PATA/SATA hard disks, flash
    drives, and USB mass storage devices such as portable music
    players and digital cameras) as SCSI disks. The rest of the
    naming system is similar to the older /dev/hd* naming
    scheme described above.

    /dev/sr* : Optical drives (CD/DVD readers and burners)

In addition, we often see symbolic links such as /dev/cdrom, /dev/dvd, and
/dev/floppy, which point to the actual device files, provided as a convenience.
If you are working on a system that does not automatically mount
removable devices, you can use the following technique to determine how
the removable device is named when it is attached. First, start a real-time
view of the /var/log/messages file (you may require superuser privileges
for this):
$ sudo tail -f /var/log/messages

Note: Using the tail -f /var/log/messages technique is a great way to watch what the system is doing in near realtime.

With our device name in hand, we can now mount the flash drive:
$ sudo mkdir /mnt/flash
$ sudo mount /dev/sdb1 /mnt/flash
$ df

The device name will remain the same as long as it remains physically
attached to the computer and the computer is not rebooted.
Creating New FIlesystems: Let's say that we want to reformat the flash drive with a Linux native filesystem, rather than the FAT32 system it has now, This involves two steps: first, (optionally) creating a new partition layout if the existing one is not to our liking, and second, creating a new, empty filesystem on the drive.  



Networking:

This chapter will cover the following:
- ping - Send an ICMP `ECHO_REQUEST` to network hosts.
- traceroute - Print the route packets trace to a network host.
- netstat - Print network connections, routing tables, interface statistics, masquarade connections, and multicast memberships.
- ftp - Internet file transfer program.
- lftp - An improved Internet file transfer program.
- wget - Non-interactive network downloader.
- ssh - OpenSSH SSH client (remote login program)
- scp - Secure copy (remote file copy program)
- sftp - Secure file transfer program.


ping - Send a special packet to a Network Host
The most common network command is "ping". The ping command sends a
special network packet called an IMCP `ECHO_REQUEST` to a specified host.
Most network devices receiving this packet will reply to it, allowing the network connection to be verified.

Note: It's possible to configure most network devices (including Linux Hosts) to ignore these packets. This is usually done for security reasons, to partially obscure a host from a pontential attacker. It's also common for firewalls to be configured to block IMCP traffic.




xchm => Compiled HTML reader for X
evince => pdf reader for x
irb => interactive Ruby
apt = Advanced PAckage Tool

Type vimtutor into a shell to go through a brief interactive tutorial inside VIM.

Note 2 self:
If you make this source file executable (using, for instance, chmod +x myprog.rb), Unix lets
you run the file as a program.
    % ./myprog.rb
Hello, world!
You can do something similar under Microsoft Windows using file associations, and you
can run Ruby GUI applications by double-clicking their names in Explorer.
    #!/usr/local/bin/ruby -w
    puts "Hello, world!"


If you make this source file executable (using, for instance, chmod +x myprog.rb), Unix lets
you run the file as a program.
% ./myprog.rb
Hello, world!
You can do something similar under Microsoft Windows using file associations, and you
can run Ruby GUI applications by double-clicking their names in Explorer.
=end

traceroute—Trace the Path of a Network Packet
The traceroute program (some systems use the similar tracepath program
instead) displays a listing of all the “hops” network traffic takes to get from
the local system to a specified host. For example, to see the route taken to
reach http://www.slashdot.org/, we would do this:
$ tracepath slashdot.org

In the output, we can see that connecting from our test system to http://
www.slashdot.org/ requires traversing 16 routers. For routers that provide
identifying information, we see their hostnames, IP addresses, and perform-
ance data, which include three samples of round-trip time from the local
system to the router. For routers that do not provide identifying information
(because of router configuration, network congestion, firewalls, etc.), we see
asterisks as in the output.


netstat — Examine Network Settings and Statistics
The netstat program is used to examine various network settings and statis-
tics. Through the use of its many options, we can look at a variety of features
in our network setup. Using the -ie option, we can examine the network
interfaces in our system:

$ netstat -ie

In the example above, we see that our test system has two network inter-
faces. The first, called eth0, is the Ethernet interface; the second, called lo, is
the loopback interface, a virtual interface that the system uses to “talk to itself.”
When performing causal network diagnostics, the important things to
look for are the presence of the word UP at the beginning of the fourth line
for each interface, indicating that the network interface is enabled, and the
presence of a valid IP address in the inet addr field on the second line. For
systems using Dynamic Host Configuration Protocol (DHCP), a valid IP
address in this field will verify that the DHCP is working.


Using the -r option will display the kernel’s network routing table.
This shows how the network is configured to send packets from network
to network:

    $netstat -i

In this simple example, we see a typical routing table for a client machine
on a local area network (LAN) behind a firewall/router. The first line of the
listing shows the destination 192.168.1.0. IP addresses that end in zero refer
to networks rather than individual hosts, so this destination means any host
on the LAN. The next field, Gateway, is the name or IP address of the gateway
(router) used to go from the current host to the destination network. An
asterisk in this field indicates that no gateway is needed.
The last line contains the destination default. This means any traffic
destined for a network that is not otherwise listed in the table. In our example,
we see that the gateway is defined as a router with the address of 192.168.1.1,
which presumably knows what to do with the destination traffic.
The netstat program has many options, and we have looked at only a
couple. Check out the netstat man page for a complete list.


The locate database is created by another program named updatedb.
Usually it's run periodically as a cron job

A cron-job is a task run at regular intervals by the cron daemon.

Most systems equiped with locate run update db once a day.

you will notice that very recent files
do not show up when using locate. To overcome this, it’s possible to run the
updatedb program manually by becoming the superuser and running updatedb
at the prompt.

The beauty of `find` is that it can be used to
identify files that meet specific criteria. It does this through the (slightly
strange) application of tests, actions, and options.

    $ find Enya -type f | wc -l

Note that find returns path names

    $ find [path_name] -[type | name | size] [option] 

e.g.

    $ find /media/Sick/Media/Musique/ -name "*.mp3" | less
        # to recursively look for all mp3 files from the given path and pipe the result to less

    $ find /media/Sick/Media/Musique/ -name "*.mp3" | wc -l

        # to find the count of all mp3 files, since each path represents an individual file.

    $ find ~ -type f -name "*.JPG" -size +1M | wc -l

When using the size option
    Character | Unit
    b         | 512-byte blocks (the default if no unit is specified)
    c         | Bytes
    w         | 2-byte words
    k         | Kilobytes (units of 1024 bytes)
    M         | Megabytes (units of 1,048,576 bytes)
    G         | Gigabytes (units of 1,073,741,824 bytes)



    $df : show the amount of diskspace used and available on the file
    system.

    $file : determine the filetype.

    $ man errno : To see a list of available System errors


Archiving and Backup

$ gzip [filename]   # replace the supplied file with a compressed one
$ gunzip [compressed file]  # uncompress a compressed file

Archiving Files
Archiving Files
A common file-management task used in conjunction with compression is
archiving. Archiving is the process of gathering up many files and bundling
them into a single large file. Archiving is often done as a part of system
backups. It is also used when old data is moved from a system to some type
of long-term storage.


$ tar cf playground.tar playground # to make tar called playground.tar from contents of playground directory.

$ tar tf plaground.tar  # to see the contents of a tar

For a more detailed listing, we can add the v (verbose) option:
$ tar tvf playground

Now, let’s extract the playground in a new location. We will do this by
creating a new directory named foo, changing the directory, and extracting
the tar archive:
$ mkdir foo
$ cd foo
$ tar xf ../playground.tar
$ ls  #=> playground

There is one caveat, however: Unless you are operating as the super-
user, files and directories extracted from archives take on the ownership
of the user performing the restoration, rather than the original owner.

When extracting an archive, it’s possible to limit what is extracted. For
example, if we wanted to extract a single file from an archive, it could be
done like this:
$ tar xf archive.tar pathname


REGULAR EXPRESSIONS

grep — Search Through Text

grep = Global Regular expression Print

In essence, grep searches text files for the occurence of a specified regular 
expression and outputs any line containing a match to the standard output.

    $ grep [options] regex [file...]

    $ grep bzip dirlist*.txt

In this example, grep searches all of the listed files for the string bzip and
finds two matches, both in the file dirlist-bin.txt. If we were interested in only
the files that contained matches rather than the matches themselves, we
could specify the -l option:

    grep -l bzip dirlist*.txt

Conversely, if we wanted to see a list of only the files that did not con-
tain a match, we could do this:

    $ grep -L bzip dirlist*.txt

Regular expression
metacharacters consist of the following:

    ^ $ . [ ] { } - ? * + ( ) | \

Note: As we can see, many of the regular-expression metacharacters are also characters that
have meaning to the shell when expansion is performed. When we pass regular expres-
sions containing metacharacters on the command line, it is vital that they be enclosed
in quotes to prevent the shell from attempting to expand them.

    $ grep -h '.zip' dirlist*.txt

Anchors
The caret (^) and dollar sign ($) characters are treated as anchors in regular
expressions. This means that they cause the match to occur only if the regular
expression is found at the beginning of the line (^) or at the end of the line ($).

Note that the
regular expression ^$ (a beginning and an end with nothing in between)
will match blank lines.

    $ grep -i '^..j.r$' /usr/share/dict/words


In ASCII, the first 32 characters
(numbers 0–31) are control codes (things like tabs, backspaces, and car-
riage returns). The next 32 (32–63) contain printable characters, including
most punctuation characters and the numerals zero through nine. The next
32 (numbers 64–95) contain the uppercase letters and a few more punctu-
ation symbols. The final 31 (numbers 96–127) contain the lowercase letters
and yet more punctuation symbols. Based on this arrangement, systems
using ASCII used a collation order that looked like this:
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
This differs from proper dictionary order, which is like this:
aAbBcCdDeEfFgGhHiIjJkKlLmMnNoOpPqQrRsStTuUvVwWxXyYzZ
As the popularity of Unix spread beyond the United States, there grew
a need to support characters not found in US English. The ASCII table was
expanded to use a full 8 bits, adding character numbers 128–255, which
accommodated many more languages. To support this ability, the POSIX
standards introduced a concept called a locale, which could be adjusted to
select the character set needed for a particular location. We can see the lan-
guage setting of our system using this command:
  $ echo $LANG

With this setting, POSIX-compliant applications will use a dictionary
collation order rather than ASCII order. This explains the behavior of the
commands above. A character range of [A-Z], when interpreted in dictionary
order, includes all of the alphabetic characters except the lowercase a—
hence our results.

To partially work around this problem, the POSIX standard includes
a number of character classes, which provide useful ranges of characters.
They are described in Table 19-2.
Table 19-2: POSIX Character Classes

    Character Class Description
    [:alnum:] The alphanumeric characters; in ASCII, equivalent to
    [A-Za-z0-9]
    [:word:] The same as [:alnum:], with the addition of the underscore
            character (_)
    [:alpha:] The alphabetic characters; in ASCII, equivalent to [A-Za-z]
    [:blank:] Includes the space and tab characters
    [:cntrl:] The ASCII control codes; includes the ASCII characters 0
             through 31 and 127
    [:digit:] The numerals 0 through 9
    [:graph:] The visible characters; in ASCII, includes characters 33
             through 126
    [:lower:] The lowercase letters
    [:punct:] The punctuation characters; in ASCII, equivalent to
    [-!"#$%&'()*+,./:;<=>?@[\\\]_`{|}~]
    [:print:] The printable characters; all the characters in [:graph:]
             plus the space character
    [:space:] The whitespace characters including space, tab, carriage
             return, newline, vertical tab, and form feed; in ASCII,
            equivalent to [ \t\r\n\v\f]
    [:upper:] The uppercase characters
    [:xdigit:] Characters used to express hexadecimal numbers; in ASCII,
              equivalent to [0-9A-Fa-f]


Remember, however, that this is not an example of a regular expres-
sion; rather it is the shell performing pathname expansion. We show it here
because POSIX character classes can be used for both.


REVERTING TO TRADITIONAL COLLATION ORDER
You can opt to have your system use the traditional (ASCII) collation order by
changing the value of the LANG environment variable. As we saw in the previous
section, the LANG variable contains the name of the language and character set
used in your locale. This value was originally determined when you selected an
installation language as your Linux was installed.
To see the locale settings, use the locale command:
    $ locale
    LANG=en_US.UTF-8
    LC_CTYPE="en_US.UTF-8"
    LC_NUMERIC="en_US.UTF-8"
    LC_TIME="en_US.UTF-8"
    LC_COLLATE="en_US.UTF-8"
    LC_MONETARY="en_US.UTF-8"
    LC_MESSAGES="en_US.UTF-8"
    LC_PAPER="en_US.UTF-8"
    LC_NAME="en_US.UTF-8"
    LC_ADDRESS="en_US.UTF-8"
    LC_TELEPHONE="en_US.UTF-8"
    LC_MEASUREMENT="en_US.UTF-8"
    LC_IDENTIFICATION="en_US.UTF-8"
    LC_ALL=
To change the locale to use the traditional Unix behaviors, set the LANG
variable to POSIX:

    $ export LANG=POSIX

Note that this change converts the system to use US English (more spe-
cifically, ASCII) for its character set, so be sure this is really what you want.
You can make this change permanent by adding this line to your .bashrc file:
export LANG=POSIX

$ locale  # to see the locale settings

POSIX Basic vs. Extended Regular Expressions
Just when we thought this couldn’t get any more confusing, we discover that
POSIX also splits regular expression implementations into two kinds: basic
regular expressions (BRE) and extended regular expressions (ERE). The features we
have covered so far are supported by any application that is POSIX compli-
ant and implements BRE. Our grep program is one such program.
What’s the difference between BRE and ERE? It’s a matter of metachar-
acters. With BRE, the following metacharacters are recognized: ^ $ . [ ] *
All other characters are considered literals. With ERE, the following meta-
characters (and their associated functions) are added: ( ) { } ? + |

However (and this is the fun part), the characters () {} are treated as
metacharacters in BRE if they are escaped with a backslash, whereas with
ERE, preceding any metacharacter with a backslash causes it to be treated
as a literal.
Since the features we are going to discuss next are part of ERE, we are
going to need to use a different grep. Traditionally, this has been performed
by the egrep program, but the GNU version of grep also supports extended
regular expressions when the -E option is used.


POSIX
During the 1980s, Unix became a very popular commercial operating system,
but by 1988, the Unix world was in turmoil. Many computer manufacturers had
licensed the Unix source code from its creators AT&T, and were supplying vari-
ous versions of the operating system with their systems. However, in their efforts
to create product differentiation, each manufacturer added proprietary changes
and extensions. This started to limit the compatibility of the software. As always
with proprietary vendors, each was trying to play a winning game of “lock-in” with
their customers. This dark time in the history of Unix is known today as the
Balkanization.
Enter the IEEE (Institute of Electrical and Electronics Engineers). In the
mid-1980s, the IEEE began developing a set of standards that would define how
Unix (and Unix-like) systems would perform. These standards, formally known
as IEEE 1003, define the application programming interfaces (APIs), the shell and
utilities that are to be found on a standard Unix-like system. The name POSIX,
which stands for Portable Operating System Interface (with the X added to the end
for extra snappiness), was suggested by Richard Stallman (yes, that Richard
Stallman) and was adopted by the IEEE.


Alternation
The first of the extended regular expression features we will discuss is called
alternation, which is the facility that allows a match to occur from among a
set of expressions. Just as a bracket expression allows a single character to
match from a set of specified characters, alternation allows matches from a
set of strings or other regular expressions.

    $ echo "AAA" | grep AAA
    AAA

    $ echo "AAA" | grep -E 'AAA|BBB'
    AAA

    echo "BBB" | grep -E 'AAA|BBB'
    BBB

Alternation is not limited to two choices:

    $ echo "AAA" | grep -E 'AAA|BBB|CCC'
    AAA

To combine alternation with other regular-expression elements, we can
use () to separate the alternation:

    $ grep -Eh '^(bz|gz|zip)' dirlist*.txt

This expression will match the filenames in our lists that start with
either bz, gz, or zip. If we leave off the parentheses, the meaning of this
regular expression changes to match any filename that begins with bz or
contains gz or contains zip:

    $ grep -Eh '^bz|gz|zip' dirlist*.txt

Quantifiers[start from here]

    $ cat -A [file_name]   #show the non-printing characters in the text.

    ^I = "CTRL + I"

## MS-DOS TEXT VS. UNIX TEXT
One of the reasons you may want to use cat to look for non-printing characters
in text is to spot hidden carriage returns. Where do hidden carriage returns
come from? DOS and Windows! Unix and DOS don’t define the end of a line
the same way in text files. Unix ends a line with a linefeed character (ASCII 10),
while MS-DOS and its derivatives use the sequence carriage return (ASCII 13)
and linefeed to terminate each line of text.
There are a several ways to convert files from DOS to Unix format. On
many Linux systems, programs called dos2unix and unix2dos can convert text files
to and from DOS format. However, if you don’t have dos2unix on your system,
don’t worry. The process of converting text from DOS to Unix format is very
simple; it simply involves the removal of the offending carriage returns. That is
easily accomplished by a couple of the programs discussed later in this chapter.

cat also has options that are used to modify text. The two most promin-
ent are -n, which numbers lines, and -s, which suppresses the output of mul-
tiple blank lines.
e.g; $cat -ns filename

sort—Sort Lines of Text Files
$ sort [options] input 

$ ls -l /usr/bin | sort -nr -k 5 | head # sort output by size.

$ cut # exract particular parts of a file.

For uniq to actually do its job, the input must be sorted first:
sort foo.txt | uniq
This is because uniq only removes duplicate lines that are adjacent to
each other.

Slicing and Dicing
The next three programs we will discuss are used to peel columns of text
out of files and recombine them in useful ways.

cut - Remove sections from each line of files
The cut program is used to extract a section of text from a line and output
the extracted section to standard output. It can accept multiple file argu-
ments or input from standard input.

paste—Merge Lines of Files
The paste command does the opposite of cut. Rather than extracting a
column of text from a file, it adds one or more columns of text to a file.
It does this by reading multiple files and combining the fields found in
each file into a single stream of standard output. Like cut, paste accepts
multiple file arguments and/or standard input.


join—Join Lines of Two Files on a Common Field
In some ways, join is like paste in that it adds columns to a file, but it does so
in a unique way. A join is an operation usually associated with relational data-
bases where data from multiple tables with a shared key field is combined to
form a desired result. The join program performs the same operation. It
joins data from multiple files based on a shared key field.

It is important to point out that the files must be sorted on the key field for join
to work properly.

$ join distros-key-names.txt distros-key-vernums.txt | head

Note also that, by default, join uses whitespace as the input field delim-
iter and a single space as the output field delimiter. This behavior can be
modified by specifying options. 


comm—Compare Two Sorted Files Line by Line
The comm program compares two text files, displaying the lines that are
unique to each one and the lines they have in common. To demonstrate,
we will create two nearly identical text files using cat:


diff—Compare Files Line by Line
Like the comm program, diff is used to detect the differences between files.
However, diff is a much more complex tool, supporting many output for-
mats and the ability to process large collections of text files at once. diff is
often used by software developers to examine changes between different
versions of program source code because it has the ability to recursively
examine directories of source code, often referred to as source trees. One
common use for diff is the creation of diff files or patches that are used by
programs such as patch (which we’ll discuss shortly) to convert one version
of a file (or files) to another version.

    $ diff file1.txt file2.txt
    1d0
    < a
    4a4
    > e

When viewed using the context format (the -c option), the output looks
like this:
    $ diff -c file1.txt file2.txt
    *** file1.txt
    2012-12-23 06:40:13.000000000 -0500
    --- file2.txt
    2012-12-23 06:40:34.000000000 -0500
    ***************
    *** 1,4 ****
    - a
    b
    c
    d
    --- 1,4 ----
    b
    c
    d
    + e

The output begins with the names of the two files and their timestamps.
The first file is marked with asterisks, and the second file is marked with dashes.
Throughout the remainder of the listing, these markers will signify their
respective files. Next, we see groups of changes, including the default num-
ber of surrounding context lines. In the first group, we see `*** 1,4 ****`, which
indicates lines 1 through 4 in the first file. Later we see --- 1,4 ----, which indi-
cates lines 1 through 4 in the second file.

Table 20-5: diff Context-Format Change Indicators
Indicator Meaning
(none) A line shown for context. It does not indicate a difference
      between the two files.
-      A line deleted. This line will appear in the first file but not in the
       second file.
+   A line added. This line will appear in the second file but not in
    the first file.
!   A line changed. The two versions of the line will be displayed,
   each in its respective section of the change group.


The unified format is similar to the context format but is more concise.
It is specified with the -u option:
    $ diff -u file1.txt file2.txt
    --- file1.txt
    2012-12-23 06:40:13.000000000 -0500
    +++ file2.txt
    2012-12-23 06:40:34.000000000 -0500
    @@ -1,4 +1,4 @@
    -a
    b
    c
    d
    +e


## patch — Apply a diff to an Original
The patch program is used to apply changes to text files. It accepts output
from diff and is generally used to convert older version of files into newer
versions. Let’s consider a famous example. The Linux kernel is developed
by a large, loosely organized team of contributors who submit a constant
stream of small changes to the source code. The Linux kernel consists of
several million lines of code, while the changes that are made by one con-
tributor at one time are quite small. It makes no sense for a contributor to
send each developer an entire kernel source tree each time a small change
is made. Instead, a diff file is submitted. The diff file contains the change
from the previous version of the kernel to the new version with the contrib-
utor’s changes. The receiver then uses the patch program to apply the change
to his own source tree. Using diff/patch offers two significant advantages:
The diff file is very small, compared to the full size of the source tree.
The diff file concisely shows the change being made, allowing reviewers
of the patch to quickly evaluate it.
Of course, diff/patch will work on any text file, not just source code. It
would be equally applicable to configuration files or any other text.
To prepare a diff file for use with patch, the GNU documentation sug-
gests using diff as follows:

    diff -Naur old_file new_file > diff_file

where `old_file` and `new_file` are either single files or
directories containing files. The r option supports recursion of a
directory tree. Once the diff file has been created, we can apply it
to patch the old file into the new file:

    patch < diff_file

We’ll demonstrate with our test file:

    $ diff -Naur file1.txt file2.txt > patchfile.txt
    $ patch < patchfile.txt
    patching file file1.txt
    cat file1.txt
    b
    c
    d
    e


In this example, we created a diff file named patchfile.txt and then used
the patch program to apply the patch. Note that we did not have to specify a
target file to patch, as the diff file (in unified format) already contains the
filenames in the header. Once the patch is applied, we can see that file1.txt
now matches file2.txt.
patch has a large number of options, and additional utility programs
can be used to analyze and edit patches.

Editing on the Fly
tr—Transliterate or Delete Characters
The tr program is used to transliterate characters. We can think of this as a
sort of character-based search-and-replace operation. Transliteration is the
process of changing characters from one alphabet to another. For example,
converting characters from lowercase to uppercase is transliteration. We can
perform such a conversion with tr as follows:

$ echo "lowercase letters" | tr a-z A-Z
LOWERCASE LETTERS

$ echo "lowercase letters" | tr [:lower:] A
AAAAAAAAA AAAAAAA

In addition to transliteration, tr allows characters to simply be deleted
from the input stream. Earlier in this chapter, we discussed the problem of
converting MS-DOS text files to Unix-style text. To perform this conversion,
carriage return characters need to be removed from the end of each line.
This can be performed with tr as follows:
    tr -d '\r' < dos_file > unix_file

where `dos_file` is the file to be converted and `unix_file` is the result. This
form of the command uses the escape sequence `\r` to represent the carriage
return character. 


ROT13: THE NOT-SO-SECRET DECODER RING
One amusing use of tr is to perform ROT13 encoding of text. ROT13 is a trivial
type of encryption based on a simple substitution cipher. Calling ROT13 encryp-
tion is being generous; text obfuscation is more accurate. It is used sometimes on
text to obscure potentially offensive content. The method simply moves each
character 13 places up the alphabet. Since this is halfway up the possible 26
characters, performing the algorithm a second time on the text restores it to
its original form. To perform this encoding with tr:
echo "secret text" | tr a-zA-Z n-za-mN-ZA-M
frperg grkg
Performing the same procedure a second time results in the translation:
echo "frperg grkg" | tr a-zA-Z n-za-mN-ZA-M
secret text
A number of email programs and Usenet news readers support ROT13
encoding. Wikipedia contains a good article on the subject: http://en.wikipedia
.org/wiki/ROT13.


sed—Stream Editor for Filtering and Transforming Text
The name sed is short for stream editor. It performs text editing on a stream
of text, either a set of specified files or standard input.

    $ echo "front" | sed 's/front/back/'

    aspell—Interactive Spell Checker


    $ printf "<html>\n\t<head>\n\t\t<title>%s</title>\n\t</head>
    \n\t<body>\n\t\t<p>%s</p>\n\t</body>\n</html>\n" "Page Title" "Page Content"
    <html>
    <head>
    <title>Page Title</title>
    </head>
    <body>
    <p>Page Content</p>
    </body>
    </html>


PostScript is a page-description language that is used to describe the
contents of a printed page to a typesetter-like device. We can take the out-
put of our command and store it to a file (assuming that we are using a
graphical desktop with a Desktop directory):

    $ zcat /usr/share/man/man1/ls.1.gz | groff -mandoc > ~/Desktop
    /foo.ps

What we see is a nicely typeset man page for ls! In fact, it’s possible to
convert the PostScript file into a PDF (Portable Document Format) file with this
command:
  $ ps2pdf ~/Desktop/foo.ps ~/Desktop/ls.pdf

    Note: Linux systems often include many command line-programs for file-format conversion.
    They are often named using the convention format2format. Try using the command
    ls /usr/bin/*[[:alpha:]]2[[:alpha:]]* to identify them. Also try searching for pro-
    grams named formattoformat.

There are some rules about variable names:
 - Variable names may consist of alphanumeric characters (letters and
numbers) and underscore characters.
 - The first character of a variable name must be either a letter or an
underscore.
 - Spaces and punctuation symbols are not allowed.

Here Documents
We’ve looked at two different methods of outputting our text, both using
the echo command. There is a third way called a here document or here script. A
here document is an additional form of I/O redirection in which we embed
a body of text into our script and feed it into the standard input of a com-
mand. It works like this:
command << token
text
token
where command is the name of a command that accepts standard input and
token is a string used to indicate the end of the embedded text.

Note that the token must
appear alone and that there must not be trailing spaces on the line.

Here documents can be used with any command that accepts standard
input. In this example, we use a here document to pass a series of commands
to the ftp program in order to retrieve a file from a remote FTP server:
    #!/bin/bash
    # Script to retrieve a file via FTP
    FTP_SERVER=ftp.nl.debian.org
    FTP_PATH=/debian/dists/lenny/main/installer-i386/current/images/cdrom
    REMOTE_FILE=debian-cd_info.tar.gz
    ftp -n << _EOF_
    open $FTP_SERVER
    user anonymous me@linuxbox
    cd $FTP_PATH
    hash
    get $REMOTE_FILE
    bye
    _EOF_
    ls -l $REMOTE_FILE

If we change the redirection operator from << to <<-, the shell will
ignore leading tab characters in the here document. This allows a here
document to be indented, which can improve readability:
    #!/bin/bash
    # Script to retrieve a file via FTP
    FTP_SERVER=ftp.nl.debian.org
    FTP_PATH=/debian/dists/lenny/main/installer-i386/current/images/cdrom
    REMOTE_FILE=debian-cd_info.tar.gz
    ftp -n <<- _EOF_
    open $FTP_SERVER
    user anonymous me@linuxbox
    cd $FTP_PATH
    hash
    get $REMOTE_FILE
    bye
    _EOF_
    ls -l $REMOTE_FILE


function name {
commands
return
}

alternatively
name () {
commands
return
}

Both forms are equivalent and may be used interchangeably.

Note that in order for function calls to
be recognized as shell functions and not interpreted as the names of external
programs, shell function definitions must appear in the script before they
are called.

Shell-function names follow the same rules as variables. A function must
contain at least one command. The return command (which is optional) sat-
isfies the requirement.


SHELL FUNCTIONS IN YOUR .BASHRC FILE
Shell functions make excellent replacements for aliases, and they are actually
the preferred method of creating small commands for personal use. Aliases are
very limited in the kind of commands and shell features they support, whereas
shell functions allow anything that can be scripted. For example, if we liked the
`report_disk_space` shell function that we developed for our script, we could cre-
ate a similar function named ds for our .bashrc file:
ds () {
echo “Disk Space Utilization For $HOSTNAME”
df -h
}


The if statement has the following syntax:
    if commands; then
    commands
    [elif commands; then
    commands...]
    [else
    commands]
    fi
where commands is a list of commands.

Exit Status
Commands (including the scripts and shell functions we write) issue a value
to the system when they terminate, called an exit status. This value, which is
an integer in the range of 0 to 255, indicates the success or failure of the
command’s execution. By convention, a value of 0 indicates success, and
any other value indicates failure. The shell provides a parameter that we can
use to examine the exit status. Here we see it in action:
    $echo $?
    0

Note to self: the most recent exit status is stored in $?

The shell provides two extremely simple built-in commands that do
nothing except terminate with either a 0 or 1 exit status. The true com-
mand always executes successfully, and the false command always executes
unsuccessfully:
$ true
$ echo $?
0

$ false
$ echo $?
1

We can use these commands to see how the if statement works. What
the if statement really does is evaluate the success or failure of commands:
[me@linuxbox ~]$ if true; then echo "It's true."; fi
It's true.

    $ if false; then echo "It's true."; fi

    $
The command echo "It's true." is executed when the command follow-
ing if executes successfully, and it is not executed when the command fol-
lowing if does not execute successfully. If a list of commands follows if, the
last command in the list is evaluated:

    $ if false; true; then echo "It's true."; fi
    It's true.
    $ if true; false; then echo "It's true."; fi
    $


Using test
By far, the command used most frequently with if is test. The test com-
mand performs a variety of checks and comparisons. It has two equivalent
forms:
test expression
and the more popular
`[ expression ]`
where expression is an expression that is evaluated as either true or false.
The test command returns an exit status of 0 when the expression is true
and a status of 1 when the expression is false.


File Expressions
The expressions in Table 27-1 are used to evaluate the status of files.

Expression  : Is true if . . .
file1 -ef file2: file1 and file2 have the same inode numbers (the two
                filenames refer to the same file by hard linking).
file1 -nt file2: file1 is newer than file2.
file1 -ot file2: file1 is older than file2.
-b file: exists and is a block-special (device) file.
-c file: file exists and is a character-special (device) file.
-d file: file exists and is a directory.
-e file: file exists.
-f file: file exists and is a regular file.
-g file: file exists and is set-group-ID.
-G file: file exists and is owned by the effective group ID.
-k file: file exists and has its “sticky bit” set.
-L file: file exists and is a symbolic link.
-O file: file exists and is owned by the effective user ID.
-p file: file exists and is a named pipe.
-r file: file exists and is readable (has readable permission for
         the effective user).
-s file: file exists and has a length greater than zero. 
-S file: file exists and is a network socket.
-t fd:   fd is a file descriptor directed to/from the terminal. This
         can be used to determine whether standard input/output/
         error is being redirected.
-u file: file exists and is setuid.
-w file: file exists and is writable (has write permission for the
         effective user).
-x file: file exists and is executable (has execute/search per-
         mission for the effective user).


String Expressions
The expressions in Table 27-2 are used to evaluate strings.
Expression string   : Is true if
string:               String is not null
-n string:            The length of the string is greater than zero
-z string:            The length of the string is zero
string1 = string2:    string1 and string2 are equal. SIngle or double
string1 == string2:   equal signs may be used, but the use of double equal signs is greatly preferred.
string1 != string2:   string1 and string2 are not equal
string1 > string2:    string1 sortd after string2
string1 < string2:    string1 sorts before string2

Warning: The > and < expression operators must be quoted (or escaped with a backslash) when
    used with test. If they are not, they will be interpreted by the shell as redirection oper-
    ators, with potentially destructive results. Also note that while the bash documentation
    states that the sorting order conforms to the collation order of the current locale, it does
    not. ASCII (POSIX) order is used in versions of bash up to and including 4.0.


Integer Expressions
The expressions in Table 27-3 are used with integers.
Expression:     Is true if
integer1 -eq integer2:  integer1 is equal to integer2
integer1 -ne integer2:  integer1 is not equal to integer2.
integer1 -le integer2:  integer1 is less than or equal to integer2
integer1 -lt integer2:  integer1 is less than integer2
integer1 -ge integer2:  integer1 is greater than or equal to integer2
integer1 -gt integer2: integer1 is greater than integer2.


Control Operators: Another Way to Branch
bash provides two control operators that can perform branching. The &&
(AND) and || (OR) operators work like the logical operators in the [[ ]]
compound command. This is the syntax:
command1 && command2
and
command1 || command2
It is important to understand the behavior of these. With the && oper-
ator, command1 is executed and command2 is executed if, and only if, command1 is
successful. With the || operator, command1 is executed and command2 is exe-
cuted if, and only if, command1 is unsuccessful.

In practical terms, it means that we can do something like this:
[me@linuxbox ~]$ mkdir temp && cd temp
This will create a directory named temp, and if it succeeds, the current
working directory will be changed to temp. The second command is attempted
only if the mkdir command is successful. Likewise, a command like
[me@linuxbox ~]$ [ -d temp ] || mkdir temp
will test for the existence of the directory temp, and only if the test fails will
the directory be created. This type of construct is very handy for handling
errors in scripts, a subject we will discuss more in later chapters. For
example, we could do this in a script:
[ -d temp ] || exit 1


read—Read Values from Standard Input
The read built-in command is used to read a single line of standard input.
This command can be used to read keyboard input or, when redirection is
employed, a line of data from a file. The command has the following syntax:
    read [-options] [variable...]
where options is one or more of the available options listed in Table 28-1 and
variable is the name of one or more variables used to hold the input value.
If no variable name is supplied, the shell variable REPLY contains the line
of data.

Table 28-1: read Options
Option:     Description
-a array    Assign the input to array, starting with index zero. 
-d delimeter: The first character in the string delimiter is used to indicate end of input, rather than a new line character.
-e          Use Readline to handle input. This permits input editing in the same manner as the commandline.
-n num      Read num characters of input, rather than an entire line.
-p prompt   Display a prompt for inut using the string prompt.
-r          Raw mode. Do not interprete backslash characters as escapes.
-s          Silent mode. Do not echo characters to the display as they are typed. This is useful when inputing passwords and other confidential information.
-t seconds  Timeout. Terminate input after seconds. read returns a non-zero status if an input times out.
-u fd       Use input from file descriptor fd, rather than standard input.


Separating Input Fields with IFS
Normally, the shell performs word splitting on the input provided to read.
As we have seen, this means that multiple words separated by one or more
spaces become separate items on the input line and are assigned to separate
variables by read. This behavior is configured by a shell variable named IFS
(for Internal Field Separator). The default value of IFS contains a space, a
tab, and a newline character, each of which will separate items from one
another.
We can adjust the value of IFS to control the separation of fields input to
read. For example, the /etc/passwd file contains lines of data that use the colon
character as a field separator. By changing the value of IFS to a single colon,
we can use read to input the contents of /etc/passwd and successfully separate
fields into different variables. Here we have a script that does just that:
    #!/bin/bash
    # read-ifs: read fields from a file
    FILE=/etc/passwd
    read -p "Enter a username > " user_name
    file_info=$(grep "^$user_name:" $FILE)
    if [ -n "$file_info" ]; then
      IFS=":" read user pw uid gid name home shell <<< "$file_info"
      echo "User =
      '$user'"
      echo "UID =
      '$uid'"
      echo "GID =
      '$gid'"
      echo "Full Name = '$name'"
      echo "Home Dir. = '$home'"
      echo "Shell =
      '$shell'"
    else
      echo "No such user '$user_name'" >&2
      exit 1
    fi


The <<< operator indicates a here string. A here string is like a here doc-
ument, only shorter, consisting of a single string. In our example, the line
of data from the /etc/passwd file is fed to the standard input of the read
command. We might wonder why this rather oblique method was chosen
rather than

    echo "$file_info" | IFS=":" read user pw uid gid name home shell

Well, there’s a reason . . .

    YOU CAN’T PIPE READ
    While the read command normally takes input from standard input, you cannot
    do this:
    echo "foo" | read
    We would expect this to work, but it does not. The command will appear
    to succeed, but the REPLY variable will always be empty. Why is this?
    The explanation has to do with the way the shell handles pipelines. In bash
    (and other shells such as sh), pipelines create subshells. These are copies of the
    shell and its environment that are used to execute the command in the pipe-
    line. In our previous example, read is executed in a subshell.
    Subshells in Unix-like systems create copies of the environment for the
    processes to use while they execute. When the processes finish, the copy of the
    environment is destroyed. This means that a subshell can never alter the environ-
    ment of its parent process. read assigns variables, which then become part of the
    environment. In the example above, read assigns the value foo to the variable
    REPLY in its subshell’s environment, but when the command exits, the subshell
    and its environment are destroyed, and the effect of the assignment is lost.
    Using here strings is one way to work around this behavior. Another
    method is discussed in Chapter 36.


FLOW CONTROL: LOOPING WITH WHILE AND UNTIL

The syntax of the while command is:
while commands; do commands; done

    #!/bin/bash
    # while-count: display a series of numbers
    count=1
    while [ $count -le 5 ]; do
    echo $count
    count=$((count + 1))
    done
    echo "Finished."

Like if, while evaluates the exit status of a list of commands. As long as
the exit status is 0, it performs the commands inside the loop.

The break command immediately terminates a loop, and
program control resumes with the next statement following the loop.

The continue command causes the remainder of the loop to be skipped, and pro-
gram control resumes with the next iteration of the loop.

until
The until command is much like while, except instead of exiting a loop
when a non-zero exit status is encountered, it does the opposite. An until
loop continues until it receives a 0 exit status.

    #!/bin/bash
    # until-count: display a series of numbers
    count=1
    until [ $count -gt 5 ]; do
    echo $count
    count=$((count + 1))
    done
    echo "Finished."

Deciding whether to use the while or until
loop is usually a matter of choosing the one that allows the clearest test
to be written.

Side note
      “Off by one” errors. When coding loops that employ counters, it is pos-
      sible to overlook that the loop may require that the counting start with
      0, rather than 1, for the count to conclude at the correct point. These
      kinds of errors result in either a loop “going off the end” by counting
      too far, or else missing the last iteration of the loop by terminating one
      iteration too soon.


Tracing
Bugs are often cases of unexpected logical flow within a script. That is, por-
tions of the script are either never executed or are executed in the wrong
order or at the wrong time. To view the actual flow of the program, we use a
technique called tracing.
One tracing method involves placing informative messages in a script
that display the location of execution. We can add messages to our code
fragment:
    echo "preparing to delete files" >&2
    if [[ -d $dir_name ]]; then
    if cd $dir_name; then
    echo "deleting files" >&2
    rm *
    else
    echo "cannot cd to '$dir_name'" >&2
    exit 1
    fi
    else
    echo "no such directory: '$dir_name'" >&2
    exit 1
    fi
    echo "file deletion complete" >&2

bash also provides a method of tracing, implemented by the -x option
and the set command with the -x option. 
    #!/bin/bash -x
Alternatively:
You can provide the -x option as a commandline option to bash i.i:
    $bash -x [executable_file_name]

With tracing enabled, we see the commands performed with expansions
applied. The leading plus signs indicate the display of the trace to distinguish
them from lines of regular output. The plus sign is the default character for
trace output. It is contained in the PS4 (prompt string 4) shell variable.

To perform a trace on a selected portion of a script, rather than the
entire script, we can use the set command with the -x option:
    #!/bin/bash
    # trouble: script to demonstrate common errors
number=1
set -x # Turn on tracing
if [ $number = 1 ]; then
echo "Number is equal to 1."
else
echo "Number is not equal to 1."
fi
set +x # Turn off tracing
We use the set command with the -x option to activate tracing and the
+x option to deactivate tracing. This technique can be used to examine mul-
tiple portions of a troublesome script.


case
The bash multiple-choice compound command is called case. It has the fol-
lowing syntax:
  case word in
    [pattern [| pattern]...) commands ;;]...
  esac

The case command looks at the value of word—in our example, the value
of the REPLY variable—and then attempts to match it against one of the speci-
fied patterns. When a match is found, the commands associated with the spe-
cified pattern are executed. After a match is found, no further matches are
attempted.

Patterns
The patterns used by case are the same as those used by pathname expan-
sion. Patterns are terminated with a ) character. Table 31-1 shows some valid
patterns.

Table 31-1 case Patern examples
a) Matches if word equals a.

[[:alpha:]]) Matches if word is a single alphabetic character.

???) Matches if word is exactly three characters long.

`*.txt)` Matches if word ends with the characters .txt.

`*)` Matches any value of word. It is good practice to include
  this as the last pattern in a case command to catch any
 values of word that did not match a previous pattern; that
is, to catch any possible invalid values.


Combining Multiple Patterns
It is also possible to combine multiple patterns using the vertical pipe charac-
ter as a separator. This creates an “or” conditional pattern. This is useful for
such things as handling both upper- and lowercase characters.

Even when no arguments are provided, $0 will always contain the first
item appearing on the command line, which is the pathname of the pro-
gram being executed.

Note: You can actually access more than nine parameters using parameter expansion. To
specify a number greater than nine, surround the number in braces; for example,
${10}, ${55}, ${211}, and so on.

The shell also provides a variable, $#, that yields the number of arguments
on the command line.

The * and @ Special Parameters
Parameter Description
`$*` Expands into the list of positional parameters, starting with 1.
   When surrounded by double quotes, it expands into a double-
   quoted string containing all the positional parameters, each
   separated by the first character of the IFS shell variable (by
   default a space character).
`$@` Expands into the list of positional parameters, starting with 1.
   When surrounded by double quotes, it expands each posi-
   tional parameter into a separate word surrounded by double
   quotes.


LOOPING WITH FOR
The for loop differs from the while and until loops in that it provides a means of processing sequences during a loop.

A for loop is implemented, naturally enough, with the for command. In
modern versions of bash, for is available in two forms.

for: Traditional Shell Form
The original for command’s syntax is as follows:
  for variable [in words]; do
    commands
  done

    where variable is the name of a variable that will increment during the exe-
    cution of the loop, words is an optional list of items that will be sequentially
    assigned to variable, and commands are the commands that are to be executed
    on each iteration of the loop.

$ for i in A B C D; do echo $i; done

$ for i in {A..D}; do echo $i; done
# range specification

    for i in distros*.txt; do echo $i; done
# pathname expansion

If the optional in words portion of the for command is omitted, for
defaults to processing the positional parameters.


for: C Language Form
Recent versions of bash have added a second form of for-command syntax,
one that resembles the form found in the C programming language. Many
other languages support this form, as well.
for (( expression1; expression2; expression3 )); do
  commands
done

where expression1, expression2, and expression3 are arithmetic expressions
and commands are the commands to be performed during each iteration of
the loop.
In terms of behavior, this form is equivalent to the following construct:
    (( expression1 ))
  while (( expression2 )); do
    commands
    (( expression3 ))
  done
expression1 is used to initialize conditions for the loop, expression2 is used
to determine when the loop is finished, and expression3 is carried out at the
end of each iteration of the loop.


Expansions to Manage Empty Variables
Several parameter expansions deal with nonexistent and empty variables.
These expansions are handy for handling missing positional parameters
and assigning default values to parameters. Here is one such expansion:
${parameter:-word}
If parameter is unset (i.e., does not exist) or is empty, this expansion
results in the value of word. If parameter is not empty, the expansion results
in the value of parameter.

    lym@salym-dev-box:~/bin$ foo=
    lym@salym-dev-box:~/bin$ echo ${foo:-"substitute value if unset"}
    substitute value if unset
    lym@salym-dev-box:~/bin$ echo $foo

    lym@salym-dev-box:~/bin$ foo=bar
    lym@salym-dev-box:~/bin$ echo ${foo:-"substitute value if unset"}
    bar
    lym@salym-dev-box:~/bin$ echo $foo
    bar

Here is another expansion, in which we use the equal sign instead of
a dash:
  ${parameter:=word}
If parameter is unset or empty, this expansion results in the value of word.
In addition, the value of word is assigned to parameter. If parameter is not empty,
the expansion results in the value of parameter.

    lym@salym-dev-box:~/bin$ foo=
    lym@salym-dev-box:~/bin$ echo ${foo:="default value if unset"}
    default value if unset
    lym@salym-dev-box:~/bin$ echo $foo
    default value if unset
    lym@salym-dev-box:~/bin$ foo=bar
    lym@salym-dev-box:~/bin$ echo ${foo:="default value if unset"}
    bar
    lym@salym-dev-box:~/bin$ echo $foo
    bar

Note: Positional and other special parameters cannot be assigned this way.

Here we use a question mark:
${parameter:?word}
If parameter is unset or empty, this expansion causes the script to exit
with an error, and the contents of word are sent to standard error. If parameter
is not empty, the expansion results in the value of parameter.

    lym@salym-dev-box:~/bin$ foo=
    lym@salym-dev-box:~/bin$ echo ${foo:?"parameter is empty"}
    bash: foo: parameter is empty
    lym@salym-dev-box:~/bin$ echo $?
    1
    lym@salym-dev-box:~/bin$ foo=bar
    lym@salym-dev-box:~/bin$ echo ${foo:?"parameter is empty"}
    bar
    lym@salym-dev-box:~/bin$ echo $?
    0

Here we use a plus sign:
${parameter:+word}
If parameter is unset or empty, the expansion results in nothing. If
parameter is not empty, the value of word is substituted for parameter; however,
the value of parameter is not changed.

    lym@salym-dev-box:~/bin$ foo=
    lym@salym-dev-box:~/bin$ echo ${foo:+"substitute value if set"}

    lym@salym-dev-box:~/bin$ foo=bar
    lym@salym-dev-box:~/bin$ echo ${foo:+"substitute value if set"}
    substitute value if set
    lym@salym-dev-box:~/bin$ echo $foo
    bar

    ${!prefix*}
    ${!prefix@}

This expansion returns the names of existing variables with names
beginning with prefix. According to the bash documentation, both forms
of the expansion perform identically. Here, we list all the variables in the
environment with names that begin with BASH:

    echo ${!BASH*}
    BASH BASHOPTS BASHPID BASH_ALIASES BASH_ARGC BASH_ARGV BASH_CMDS
    BASH_COMMAND BASH_COMPLETION BASH_COMPLETION_COMPAT_DIR
    BASH_COMPLETION_DIR BASH_LINENO BASH_REMATCH BASH_SOURCE BASH_SUBSHELL BASH_VERSINFO BASH_VERSION

String Operations
There is a large set of expansions that can be used to operate on strings. Many
of these expansions are particularly well suited for operations on pathnames.
The expansion

    ${#parameter}

expands into the length of the string contained by parameter. Normally,
parameter is a string; however, if parameter is either `@` or `*`, then the expansion
results in the number of positional parameters.

    ${parameter:offset}
    ${parameter:offset:length}

This expansion is used to extract a portion of the string contained in
parameter. The extraction begins at offset characters from the beginning of the
string and continues until the end of the string, unless the length is specified.

If the value of offset is negative, it is taken to mean it starts from the
end of the string rather than the beginning. Note that negative values must
be preceded by a space to prevent confusion with the ${parameter:-word}
expansion. length, if present, must not be less than 0.
If parameter is @, the result of the expansion is length positional paramet-
ers, starting at offset.

    ${parameter#pattern}
    ${parameter##pattern}

These expansions remove a leading portion of the string contained in
parameter defined by pattern. pattern is a wildcard pattern like those used in
pathname expansion. The difference in the two forms is that the # form
removes the shortest match, while the ## form removes the longest match.

${parameter%pattern}
${parameter%%pattern}
These expansions are the same as the # and ## expansions above, except
they remove text from the end of the string contained in parameter rather
than from the beginning.

    ${parameter/pattern/string}
    ${parameter//pattern/string}
    ${parameter/#pattern/string}
    ${parameter/%pattern/string}

This expansion performs a search and replace upon the contents of
parameter. If text is found matching wildcard pattern, it is replaced with the
contents of string. In the normal form, only the first occurrence of pattern is
replaced. In the // form, all occurrences are replaced. The /# form requires
that the match occur at the beginning of the string, and the /% form requires
the match to occur at the end of the string. /string may be omitted, which
causes the text matched by pattern to be deleted.

Parameter expansion is a good thing to know. The string-manipulation
expansions can be used as substitutes for other common commands such as
sed and cut. Expansions improve the efficiency of scripts by eliminating the
use of external programs.

Note: It is important to remember the exact meaning of the = in the expression above. A single
= performs assignment: foo = 5 says, “Make foo equal to 5.” A double == evaluates
equivalence: foo == 5 says, “Does foo equal 5?” This can be very confusing because
the test command accepts a single = for string equivalence. This is yet another reason
to use the more modern [[ ]] and (( )) compound commands in place of test.

Table 34-3: Assignment Operators

      Notation Description
      parameter = value Simple assignment. Assigns value to parameter.
      parameter += value Addition. Equivalent to parameter = parameter +
                        value.
      parameter -= value Subtraction. Equivalent to parameter = parameter –
                          value.
      parameter *= value Multiplication. Equivalent to parameter = parameter ×
                         value.
      parameter /= value Integer division. Equivalent to parameter = parameter ÷
                         value.
      parameter %= value Modulo. Equivalent to parameter = parameter % value.
      parameter++ Variable post-increment. Equivalent to parameter =
                 parameter + 1. (However, see the following
                discussion.)
      parameter-- Variable post-decrement. Equivalent to parameter =
                 parameter - 1.
      ++parameter Variable pre-increment. Equivalent to parameter =
                 parameter + 1.
      --parameter Variable pre-decrement. Equivalent to parameter =
                 parameter - 1.

While they both either increment or decrement the parameter by 1, the
two placements have a subtle difference. If placed at the front of the param-
eter, the parameter is incremented (or decremented) before the parameter
is returned. If placed after, the operation is performed after the parameter is
returned.

Bit Operations
One class of operators manipulates numbers in an unusual way. These oper-
ators work at the bit level. They are used for certain kinds of low-level tasks,
often involving setting or reading bit flags. Table 34-4 lists the bit operators.

    Operator Description
    ~ Bitwise negation. Negate all the bits in a number.
    << Left bitwise shift. Shift all the bits in a number to the left.
    >> Right bitwise shift. Shift all the bits in a number to the right.
    & Bitwise AND. Perform an AND operation on all the bits in two
    numbers.
    | Bitwise OR. Perform an OR operation on all the bits in two numbers.
    ^ Bitwise XOR. Perform an exclusive OR operation on all the bits in
    two numbers.

Note that there are also corresponding assignment operators (for
example, <<=) for all but bitwise negation.

Here we will demonstrate producing a list of powers of 2, using the left
bitwise shift operator:

    $ for ((i=0;i<8;++i)); do echo $((1<<i)); done
    1
    2
    4
    8
    16
    32
    64
    128

Logic
As we discovered in Chapter 27, the (( )) compound command supports a
variety of comparison operators. There are a few more that can be used to
evaluate logic. Table 34-5 shows the complete list.

Table 34-5: Comparison Operators

    Operator Description
    <= Less than or equal to
    >= Greater than or equal to
    < Less than
    > Greater than
    == Equal to
    != Not equal to
    && Logical AND
    || Logical OR

expr1?expr2:expr3 Comparison (ternary) operator. If expression expr1
                 evaluates to be non-zero (arithmetic true) then expr2,
                else expr3.

When used for logical operations, expressions follow the rules of arith-
metic logic; that is, expressions that evaluate as 0 are considered false, while
non-zero expressions are considered true. 

Please note that performing assignment within the expressions is not
straightforward. When this is attempted, bash will declare an error:

    $ a=0
    $ ((a<1?a+=1:a-=1))
    bash: ((: a<1?a+=1:a-=1: attempted assignment to non-variable (error token is
    "-=1")

This problem can be mitigated by surrounding the assignment expres-
sion with parentheses:

    $ ((a<1?(a+=1):(a-=1)))


bc — An Arbitrary-Precision Calculator Language
We have seen that the shell can handle all types of integer arithmetic, but
what if we need to perform higher math or even just use floating-point num-
bers? The answer is, we can’t. At least not directly with the shell. To do this,
we need to use an external program.

One approach is to use a specialized calculator program. One such
program found on most Linux systems is called bc.

`quit` exits the bc intepreter

It is also possible to pass a script to bc via standard input:

    $ bc < foo.bc
    4

The ability to take standard input means that we can use here docu-
ments, here strings, and pipes to pass scripts. This is a here string example:

    $ bc <<< "2+2"
    4

Note: Scalar Variables: variables that contain a single value e.g; integers, floats, strings.

Arrays contain elements and are accessed using indices.

Arrays in bash are limited to a single dimension.

Creating an Array
Array variables are named just like other bash variables and are created auto-
matically when they are accessed. Here is an example:

    $ a[1]=foo
    $ echo ${a[1]}
    foo

An array can also be created with the declare command:

    $ declare -a a

Assigning Values to an Array
Values may be assigned in one of two ways. Single values may be assigned
using the following syntax:
name[subscript]=value
where name is the name of the array and subscript is an integer (or arith-
metic expression) greater than or equal to 0. Note that the first element of
an array is subscript 0, not 1. value is a string or integer assigned to the array
element.
Multiple values may be assigned using the following syntax:

    name=(value1 value2 ...)

where name is the name of the array and value1 value2 ... are values assigned
sequentially to elements of the array, starting with element 0.

    $ days=(Sun Mon Tue Wed Thu Fri Sat)

It is also possible to assign values to a specific element by specifying a
subscript for each value:

    $ days=([0]=Sun [1]=Mon [2]=Tue [3]=Wed [4]=Thu [5]=Fri [6]=Sat)

When you try to access an array variable, only the first one arr[0] is returned.

Note to self:
A file can be thought of as an array of lines with line numbers acting as indices.

    -d [filename] : test if filename is a directory.
    -f [filename] : test if filename is a regular file.

    $ stat -c %y ~/my_notes/regex.txt | cut -c 12-13 #=> extract the hour portion of the file stat.

Array Operations

Outoutting the entire contents of an Array:
The subscripts * and @ can be used to access every element in an array. As
with positional parameters, the @ notation is the more useful of the two.
Here is a demonstration:

    $ animals=("a dog" "a cat" "a fish")
    $ for i in ${animals[*]}; do echo $i; done
    a
    dog
    a
    cat
    a
    fish
    lym@salym-dev-box:~$ for i in ${animals[@]}; do echo $i; done
    a
    dog
    a
    cat
    a
    fish
    lym@salym-dev-box:~$ for i in "${animals[*]}"; do echo $i; done
    a dog a cat a fish
    lym@salym-dev-box:~$ for i in "${animals[@]}"; do echo $i; done
    a dog
    a cat
    a fish

Determining the Number of Array Elements
Using parameter expansion, we can determine the number of elements in
an array in much the same way as finding the length of a string. Here is an
example:

    $ a[100]=foo
    $ echo ${#a[@]} # number of array elements
    1
    $ echo ${#a[100]} # length of element 100
    3

Finding the Subscripts Used by an Array
As bash allows arrays to contain “gaps” in the assignment of subscripts, it is
sometimes useful to determine which elements actually exist. This can be
done with a parameter expansion using the following forms:

    ${!array[*]}
    ${!array[@]}
    where array is the name of an array variable. Like the other expansions that
    use * and @, the @ form enclosed in quotes is the most useful, as it expands
    into separate words:
    $ foo=([2]=a [4]=b [6]=c)
    $ for i in "${foo[@]}"; do echo $i; done
    a
    b
    c
    $ for i in "${!foo[@]}"; do echo $i; done
    2
    4

Adding Elements to the End of an Array
Knowing the number of elements in an array is no help if we need to append
values to the end of an array, since the values returned by the * and @ nota-
tions do not tell us the maximum array index in use. Fortunately, the shell
provides us with a solution. By using the += assignment operator, we can
automatically append values to the end of an array. Here, we assign three
values to the array foo, and then append three more.

    $ foo=(a b c)
    $ echo ${foo[@]}
    a b c
    $ foo+=(d e f)
    $ echo ${foo[@]}
    a b c d e f


Deleting an Array
To delete an array, use the unset command:

    $ foo=(a b c d e f)
    $ echo ${foo[@]}
    a b c d e f
    $ unset foo
    $ echo ${foo[@]}
    $
    unset may also be used to delete single array elements:
    $ foo=(a b c d e f)
    $ echo ${foo[@]}
    a b c d e f
    $ unset 'foo[2]'
    $ echo ${foo[@]}
    a b d e f


Interestingly, the assignment of an empty value to an array does not
empty its contents:

    $ foo=(a b c d e f)
    $ foo=
    $ echo ${foo[@]}
    b c d e f

Any reference to an array variable without a subscript refers to element 0
of the array:

    $ foo=(a b c d e f)
    $ echo ${foo[@]}
    a b c d e f
    $ foo=A
    $ echo ${foo[@]}
    A b c d e f


Group Commands and Subshells
Group command:

    { command1; command2; [command3; ...] }

Subshell:
    (command1; command2; [command3;...])

    ls -l > output.txt
    echo "Listing of foo.txt" >> output.txt
    cat foo.txt >> output.txt

the above 3 lines can be summarized as:

    { ls -l; echo "Listing of foo.txt"; cat foo.txt; } > output.txt

or (using a subshell)

    (ls -l; echo "Listing of foo.txt"; cat foo.txt) > output.txt

We can also use group commands in pipes

    { ls -l; echo "Listing of foo.txt"; cat foo.txt; } | lpr


Process Substitution
While they look similar and can both be used to combine streams for
redirection, there is an important difference between group commands
and subshells. Whereas a group command executes all of its commands
in the current shell, a subshell (as the name suggests) executes its com-
mands in a child copy of the current shell. This means that the environ-
ment is copied and given to a new instance of the shell. When the subshell
exits, the copy of the environment is lost, so any changes made to the
subshell’s environment (including variable assignment) are lost as well.
Therefore, in most cases, unless a script requires a subshell, group com-
mands are preferable to subshells. Group commands are both faster and
require less memory.

Process substitution is expressed in two ways: for processes that produce
standard output:

    <(list)

or for processes that intake standard input:

    >(list)

where list is a list of commands.
To solve our problem with read, we can employ process substitution
like this:

    read < <(echo "foo")
    echo $REPLY


### Note:
When we design a large, complicated script, it is important to consider
what happens if the user logs off or shuts down the computer while the
script is running. When such an event occurs, a signal will be sent to all
affected processes. In turn, the programs representing those processes can
perform actions to ensure a proper and orderly termination of the program.


bash provides a mechanism for this purpose known as a trap. Traps are
implemented with the appropriately named built-in command trap. trap
uses the following syntax:
trap argument signal [signal...]
where argument is a string that will be read and treated as a command, and
signal is the specification of a signal that will trigger the execution of the
interpreted command.

Asynchronous Execution
bash has a built-in command to help manage asynchronous execution such
as this. The wait command causes a parent script to pause until a specified
process (i.e., the child script) finishes.

Note: $! contains the pid of the last job to be put in the background.


Named pipes behave like files but actually form first-in, first-out (FIFO)
buffers. As with ordinary (unnamed) pipes, data goes in one end and emerges
out the other. With named pipes, it is possible to set up something like this:

    process1 > named_pipe

and

    process2 < named_pipe

and it will behave as if

    process1 | process2

    $ locale
    $ history


Quick tip: Use the following tools to terminate connection such as SSH tunnels 
or VPNs left by your own users or kill high bandwidth consuming connection such 
as P2P on Linux based router.

a) Use tcpkill command to kill specified in-progress TCP connections. To kill 
all outgoing ftp (21) connection type:

    # tcpkill -i eth0 port 21

To kill all all packets arriving at or departing from host 192.168.1.100, enter:

    # tcpkill host 192.168.1.2

b) Use cutter command to cut all connections from 192.168.1.5 to the server type

    # cutter 192.168.1.5

To Cut all ssh connection from 192.168.1.5 to the server enter: 

    # cutter 192.168.1.5 22

    /etc/init.d/mysql start
    service [service_name] start
        # to start stop services like mysql

Which command is the best command to show all processors (CPU) related 
statistics on Linux operating system?

1. vmstat
2. mpstat
3. iostat
4. sysstat


$ mkdir -p app/controllers/api/v1 : create directory even if parent does not 
exist, set the parent's permission bits to the current defaults.


    $ notify-send "message" : output notifications.

    $ ubuntu-support-status

    /etc/apt/sources.list

    sudo apt-key add rabbitmq-signing-key-public.asc

    grep [OPTION]... PATTERN [FILE]...

e.g:

    grep port my_notes/unix





If you create a symbolic link (symlink/ softlink) that points to a program,
everytime a new version of the program is installed, the symlink has to be
updated to point to the new version.

### Handy Commands
    $ rm -rf [directory_name]

    $ df -h   # inquire about disk space status

    $ ps -A  => View all running processes.

    $ ps -ejH # print process tree

    $ ps aux # list all processes owned by user x e.g.,

    $ ps -aulym

    $ info coreutils    # centralized information on utilities packaged
                           in coreutils

    $uname		# Kernel information

    $ dpkg-reconfigure  # reconfigure already installed package.

    $ debconf-show	  # to just look at the configurations.

    $ for i in ~/my_notes/ldd3/examples/misc-modules/*; do cp $i .; done
        # copy all files in misc-modules to current folder.

