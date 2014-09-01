---
layout: post
title: "Git prose notes"
description: "Unorganized memory dump"
category: "Version Control"
tags: ["Development tracing", "Version control", "Project Management"]
---
{% include JB/setup %}

A *Version control system* is one that records changes to a file or set
of files over time so that you can recall specific versions later.

# Git
Git has three main states that your files can reside in: 
- committed, 
- modified and 
- staged.

*Committed* means that the data is safely stored in your local
database. 

*Modified* means that you have changed the file but have not
committed it to your database yet. 

*Staged* means that you have marked a modified file in its current
version to go into your next commit snapshot.

This leads us to the three main sections of a Git project: the Git
directory, the working directory and the staging area.

The basic Git workflow goes something like this:
1. You modify files in your working directory.
2. You stage the files, adding snapshots of them to your staging area.
3. You do a commit, which takes the files as they are in the staging
area and stores that snapshot permanently to your Git directory.

## git command sample usecases
    $ git config --list  => to check your git settings

    $ git config user.name => to get a specific key

    $ git gui

    $ gitk

    $ git init => to initialize a git repo. this creates a new subdirectory named .git in your projects folder.

    $ git add [filename] => to add the files for tracking

    $ git commit -m "message" => to commit the changes.

    $ git reset   # to rollback a commit

    $ git merge --abort   # run after a merge attempt has resulted into a
                        conflict

    $ git clone [url]. 

Below are various ways to get helpful information

    $ git help <verb>
    $ git <verb> --help
    $ man git-<verb>

For example, if you want to clone the Ruby Git library called Grit,
you can do so like this:

    $ git clone git://github.com/schacon/grit.git

That creates a directory named "grit", initializes a .git directory
inside it, pulls down all the data for that repository, and checks
out a working copy of the latest version. 

Git clone will pull down the entire git repo together will the all
the history of commits. It literally "clones" the remote repo.

If you want to clone the repository into a directory named something
other than grit, you can specify that as the next command-line option:

    $ git clone git://github.com/schacon/grit.git mygrit

That command does the same thing as the previous one, but the target
directory is called mygrit.

Git has a number of different transfer protocols you can use. The
previous example uses the git:// protocol, but you may also see
http(s):// or user@server:/path.git, which uses the SSH transfer
protocol.

Remember that each file in your working directory can be in one of
two states: tracked or untracked. Tracked files are files that were
in the last snapshot; they can be unmodified, modified or staged.
Untracked files are everything else - any files in your working
directory that were not in your last snapshot and are not in your
staging area. When you first clone a repository, all your files will
be tracked and un-modified because you just checked them out and
havent edited anything.

    $ git status

The main tool you use to determine which files are in which state is
the git status command.

the `git add` command (it’s a multipurpose command — you use it to
begin tracking new files, to stage files, and to do other things like
marking merge-conflicted files as resolved)

## Note-to-self:
    $ git commit
takes the staged files as arguments and you stage files by hitting
    $ git add [filename]

    $ git diff => to see "exactly" what changes you made but not yet
staged. Notice here git diff has no arguments.
That command compares what is in your working directory with what is
in your staging area. The result tells you the changes you’ve made
that you haven’t yet staged.

If you want to see what you’ve staged that will go into your next
commit, you can use git diff -- cached. 
(In Git versions 1.6.1 and later, you can also use git diff --staged,
which may be easier to remember.) This command compares your staged
changes to your last commit:

It’s important to note that git diff by itself doesn’t show all
changes made since your last commit — only changes that are still
unstaged. This can be confusing, because if you’ve staged all of your
changes, git diff will give you no output.

Providing the -a option to the `git commit` command makes Git
automatically stage every file that is already tracked before doing
the commit, letting you skip the git
add part:

    git commit -a -m 'added new benchmarks'

`$ git branch [-r | -a]` : by default all local branches are listed. -r
option shows remote tracking branches. -a option shows both local and
remote tracking branches.

`$ git rm --cached readme.txt` to remove a file from git (being tracked)
but keep it in your staging area. That's to say, remove it from git but
keep it on your hard-drive.

You can pass files, directories, and file-glob patterns to the git rm
command. That means you can do such things as:

    $ git rm log/\*.log

This command removes all files that have the .log extension in the log
directory.

### $ git log
to view commit history. by default, with no arguments, git logs lists
the commits made in reverse chronological order. That is, the most
recent commits show up first. As you can see this command lists each
commit with its SHA-1 checksum, the author's name and e-mail, the date
written, and the commit message.

### $ git log -p -2
the -p option shows the diff introduced in each commit and the -2 limits
the output to only the last two entries.

$ git log --stat    # to see some abbreviated stats for each commit.

the `--stat` option prints below each commit entry a list of modified files, how
many files were changed, and how many lines in those files were added and removed. It also puts a
summary of the information at the end.

    $ git log --since=2.weeks   # show list of commits made in the last two weeks.

This command works with lots of formats — you can specify a specific date (“2008-01-15”) or a
relative date such as “2 years 1 day 3 minutes ago”.

You can also filter the list to commits that match some search criteria. The --author option allows
you to filter on a specific author, and the --grep option lets you search for keywords in the commit
messages. (Note that if you want to specify both author and grep options, you have to add --all-
match or the command will match commits with either.)

if you want to see which commits modifying test files in the Git source code history
were committed by Junio Hamano and were not merges in the month of October 2008, you can run
something like this:
    $ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \
        --before="2008-11-01" --no-merges -- t/

    $ git commit --amend

If you’ve made no changes since
your last commit (for instance, you run this command immediately after your previous commit), then
your snapshot will look exactly the same and all you’ll change is your commit message.

Remember, anything that is committed in Git can almost always be recovered. Even commits that
were on branches that were deleted or commits that were overwritten with an --amend commit can be
recovered (see Chapter 9 for data recovery). However, anything you lose that was never committed is
likely never to be seen again.


## Working with Remotes:

    $ git remote : to see the shortnames of your remotes. to see the remotes
        you have configured.
    $ git remote -v : to see your short names of your remotes expanded

    $ git remote add [short_name] [remote_url]
    $ git remote add pb git://github.com/paulboone/ticgit.git

    $ git fetch pb : to fetch all the info that pb has but that you don't
yet have in your repo.
pb's master branch is accessible locally as pb/master - you can merge it
into your branches, or you can check out a local branch at that point if
you want to inspect it.

## Fetching and Pulling from Your Remotes:
As you just saw, to get data from your remote projects, you can run:
    $ git fetch [remote_name]
The command goes out to that remote project and pulls down all the data
from that remote project that you don't have yet. After you do this,
you should have references to all the branches from that remote, which
you can merge in or inspect at any point.
If you clone a repo, the command automatically adds that repository
under the name origin. So git fetch origin fetches any new work that has
been pushed to that server since you cloned (or last fetched from) it.
It's important to note that the fetch command pulls the data to your
local repository - it doesn't automatically merge it with any of your
work or modify what you're currently working on. You have to merge it
manually into your work when you're ready.

If you have a branch set up to track a remote branch, you can use the
git pull command to automatically fetch and then merge a remote branch
into your current branch. This may be an easier or more comfortable
workflow for you; and by default the git clone command automatically
sets up your local master branch to track the remote master branch on
the server you cloned from (assuming the remote has a master branch).
Running git pull generally fetches data from the server you originally
cloned and automatically tries to merge it into the code your currently
working on.

    git clone : to bring a project down for the first time, git fetch : to
        bring down the changes since you last cloned, 
    git pull to fetch and then merge into the current branch.
    git push: to push up your changes to the remote
    git remote : to see the remotes you have configured.
    git remote -v : to see the shortnames of the remotes expanded.
    git remote add [short_name] [remote_url] to add a remote.
    git push [remote_name] [branch_name] : to push to a remote.

e.g $ git push origin master
## Note to self
the remote is the same, it's the actions that change

## Pushing to your Remotes:
When you have your project at a point that you want to share, you have
to push it upstream. The command for this is simple:

    git push [remote-name] [branch-name]. If you want to push your master

branch to your origin server (again, cloning generally sets up both of
those names for you automatically), then you can run this to push your
work back up to the server:

    git push origin master

This command works only if you cloned from a server to which you have
write access and if nobody has pushed in the meantime. If you and some
one else clone at the same time and they push upstream and then you push
upstream, your push will rightly be rejected. You'll have to pull down
their work first and incorporate it into yours before you'll be allowed
to push.

Inspecting a Remote:
If you want to see more information about a particular remote, you can
use the git remote show [remote-name] command. If you run this command
with a particular short name, such as origin, you get sth like this:
    $ git remote show origin

    * remote origin
    URL: git://github.com/schacon/ticgit.git
    Remote branch merged with 'git pull' while on branch master
    master
    Tracked remote branches
      master
      ticgit

It lists the URL for the remote repository as well as the tracking
branch information. The command helpfully tells you that if you're on
the master branch and you run git pull, it'll automatically merge in the
master branch on the remote after it fetches all the remote references.
It also lists all the remote refernces it has pulled down.

That is a simple example you're likely to encounter. When you're using
Git more heavily, however, you may see much more information from git
remote show:

    $ git remote show origin

    * remote origin
    URL: git@github.com:defunkt/github.git
    Remote branch merged with 'git pull' while on branch issues
      issues
    Remote branch merged with 'git pull' while on branch master
      master
    New remote branches (next fetch will store in remotes/origin)
      caching
    Stale tracking branches (use 'git remote prune')
      libwalker
      walker2
    Tracked remote branches
      acl
      apiv2
      dashboard2
      issues
      master
      postgres
    Local branch pushed with 'git push'
      master:master

This command shows which branch is automatically pushed when you run git
push on certain branches. It also shows you which remote branches on the
server you don't yet have, which remote branches you have that have been
removed from the server, and multiple branches that are automatically
merged when you run git pull.

Removing and Renaming Remotes:
If you want to rename a reference, in newer versions of Git you can run
git remote rename to change a remote's short name. For instance, of you
want to rename pb to paul, you can do so with git remote:

    git remote rename pb paul
  
It's worth mentioning that this changes your remote branches too. What
used to be referencd as pb/master is now paul/master.

If you want to remove a reference for some reason - you've moved the
server or are no longer using a particular mirror, or perhaps a
contributor isn't contributing anymore - you can use git remote rm:

    git remote rm paul


## Tagging:
Like most VCSs, Git has the ability to tag specific points in history as
being important. Generally people use this functionality to mark release
points (v1.0, and so on). In this section, you'll learn how to list the
available tags, how to create new tags, and what the different tags are
for.

### Listing your Tags:
Listing the available tags in Git is straight forward. Just type git
tag:

    git tag
    v1.0
    v1.0

This command lists the tags in alphabetical order; the order in which
they appear has no real importance. You can also search for tags with a
particular pattern. The Git source repo, for instance, contains more
than 240 tags. If you're only interested in looking at the 1.4.2 series,
you can run this:

    git tag -l 'v1.4.2.*'

    v1.4.2.1
    v1.4.2.2
    v1.4.2.3
    v1.4.2.4                          

## Creating Tags:
Git usees two main types of tags: lightweight and annotated. A
lightweight tag is very much like a branch that doesn't change - It's
just a pointer to a specific commit. Annotated tags, however, are stored
as full objects in the Git database. They are checksummed; contain the
tagger name, email, and date; have a tagging message: and can be signed
and verified with GNU Privacy Guard (GPG). It's generally recommeded
that you create annotated tags so you can have all this information; but
if you want a temporary tag or for some reason don't want to keep the
other information, lightweight tags are available too.

### Annotated Tags
Creating an annotated tag in Git is simple. The easiest way is to
specify -a when you run the tag command:
    git tag -a v1.4 -m 'my version 1.4'
    git tag

    v0.1
    v1.3
    v1.4

The `-m` specifies the tagging message, which is stored with the tag. If
you don't specify a message for an annotated tag, Git launches your
editor so you can enter it.

You can see the tag data along with the commit that was tagged by using
the `git show` command:
    git show v1.4

    tag v1.4
    Tagger: Scott Chacon <schacon@gee-mail.com>
    Date:
    Mon Feb 9 14:45:11 2009 -0800
    my version 1.4
    commit 15027957951b64cf874c3557a0f3547bd83b3ff6
    Merge: 4a447f7... a6b4c97...
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:
    Sun Feb 8 19:02:46 2009 -0800
    Merge branch 'experiment'

That shows the tagger information, the date the commit was tagged, and
the annotated message before showing the commit information.

## Signed Tags:
You can also sign your tags with GPG, assuming you have a private key.
All you have to do is use -s instead of -a:
    git tag -s v1.5 -m 'my signed 1.5 tag'

    You need a passphrase to unlock the secret key for
    user: "Scott Chacon <schacon@gee-mail.com>"
    1024-bit DSA key, ID F721C45A, created 2009-02-09

If you run git show on that tag, you can see your GPG signature attached
to it:
    $ git show v1.5

    tag v1.5
    Tagger: Scott Chacon <schacon@gee-mail.com>
    Date:
    Mon Feb 9 15:22:20 2009 -0800
    my signed 1.5 tag
    -----BEGIN PGP SIGNATURE-----
    Version: GnuPG v1.4.8 (Darwin)
    iEYEABECAAYFAkmQurIACgkQON3DxfchxFr5cACeIMN+ZxLKggJQf0QYiQBwgySN
    Ki0An2JeAVUCAiJ7Ox6ZEtK+NvZAj82/
    =WryJ
    -----END PGP SIGNATURE-----
    commit 15027957951b64cf874c3557a0f3547bd83b3ff6
    Merge: 4a447f7... a6b4c97...
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:
    Sun Feb 8 19:02:46 2009 -0800
    Merge branch 'experiment'

### Lightweight Tags:
Another way to tag commits is with a lightweight tag. This is basically
the commit checksum stored in a file - no other inflormation is kept. To
create a lightweight tag, don't supply the -a, -s or -m option:
    git tag v1.4-lw
    git tag

    v0.1
    v1.3
    v1.4
    v1.4-lw
    v1.5

This time if you run git show on the tag, you don't see the extra tag
information. The command just shows the commit:

    $ git show v1.4-lw

    commit 15027957951b64cf874c3557a0f3547bd83b3ff6
    Merge: 4a447f7... a6b4c97...
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:
    Sun Feb 8 19:02:46 2009 -0800
    Merge branch 'experiment'

## Verifying Tags:
To verify a signed tag, you use git tag -v [tag-name]. This cammand uses
GPG to verify the signature. You need the signer's public key in your
keyring for this to work properly:
    $ git tag -v v1.4.2.1

    object 883653babd8ee7ea23e6a5c392bb739348b1eb61
    type commit
    tag v1.4.2.1
    tagger Junio C Hamano <junkio@cox.net> 1158138501 -0700
    GIT 1.4.2.1
    Minor fixes since 1.4.2, including git-mv and git-http with alternates.
    gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
    gpg: Good signature from "Junio C Hamano <junkio@cox.net>"
    gpg:
    aka "[jpeg image of size 1513]"
    Primary key fingerprint: 3565 2A26 2040 E066 C9A7 4A7D C0C6 D9A4 F311 9B9A

If you don't have the signer's public key, you get something like this
instead:
<samp>
    gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
    gpg: Can't check signature: public key not found
    error: could not verify the tag 'v1.4.2.1'
</samp>

## Tagging Later:
You can also tag commits after you've moved past them. Suppose your
commit history looks like this:
    $ git log --pretty=oneline

<samp>
    15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
    a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
    0d52aaab4479697da7686c15f77a3d64d9165190 one more thing
    6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
    0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function
    4682c3261057305bdd616e23b64b0857d832627b added a todo file
    166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
    9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
    964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
    8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme
</samp>

Now suppose you forgot to tag the project at v1.2, which was at the
"updated rakefile" commit. You can accomplish this. To tag that commit,
you specify the commit checksum (or part of it) at the end of the
command:

    $ git tag -a v1.2 9fceb02

Sharing Tags:
By default git push does not transfer tags to remote servers. You will
have to explicitly push tags to a shared server after you have created
them. This process is just like sharing remote branches - you can run
`git push origin [tagname]`

    $ git push origin v1.5
<samp>
    Counting objects: 50, done.
    Compressing objects: 100% (38/38), done.
    Writing objects: 100% (44/44), 4.56 KiB, done.
    Total 44 (delta 18), reused 8 (delta 1)
    To git@github.com:schacon/simplegit.git
    * [new tag]   v1.5 -> v1.5
</samp>

If you have lots of tags you wish to push up at once, you can use the
--tags option to the git push command. This will transfer all of your
tags to the remote server that are not already there.

    $ git push origin --tags
<samp>
    Counting objects: 50, done.
    Compressing objects: 100% (38/38), done.
    Writing objects: 100% (44/44), 4.56 KiB, done.
    Total 44 (delta 18), reused 8 (delta 1)
    To git@github.com:schacon/simplegit.git
    * [new tag]   v0.1 -> v0.1
    * [new tag]   v1.2 -> v1.2
    * [new tag]   v1.4 -> v1.4
    * [new tag]   v1.4-lw -> v1.4-lw
    * [new tag]   v1.5 -> v1.5
</samp>

Now, when someone else clones or pulls from your repository, they will
get all your tags as well.


## Git Branching:

Basic new brach merge workflow
 - `git branch [new_branch_name]` # creates new branch but doesn't check it
out. Also `git -b [branch_name]`
 - `git checkout [new_branch_name]` # new branch becomes the working branch,
   gets the HEAD label
 - `git commit [your changes]`
 - `git checkout [branch to merge to]`
 - `git merge [older branch] [new_branch]`

Shorthand for creating a new branch and then switching to it
immediately:
    $ git checkout -b [new branch name]

    $ git branch -d hotfix # to delete an already merged branch because
        technically you don't  need it any more.


If you want to use a graphical tool to resolve these issues, you can
run git mergetool, which fires up an appropriate visual merge tool
and walks you through the conflicts:

    $ git mergetool

sample output of the git mergetool comand
<samp>
    merge tool candidates: kdiff3 tkdiff xxdiff meld gvimdiff opendiff
    emerge vimdiff
    Merging the files: index.html
    Normal merge conflict for 'index.html':
    {local}: modified
    {remote}: modified
    Hit return to start merge resolution tool (opendiff):
</samp>

If you want to use a merge tool other than the default (Git chose
opendiff for me in this case because I ran the command on a Mac), you
can see all the supported tools listed at the top after “merge tool
candidates”. Type the name of the tool you’d rather use.


The `git branch` command does more than just create and delete
branches. If you run it with no arguments, you get a simple listing
of your current branches:

    $ git branch
<samp>
    iss53
    * master
    testing
</samp>


Notice the `*` character that prefixes the master branch: it
indicates the branch that you currently have checked out. This means
that if you commit at this point, the master branch will be moved
forward with your new work. To see the last commit on each branch,
you can run `git branch -v`:

    $ git branch -v

    iss53
    93b412c fix javascript issue
    * master 7a98805 Merge branch 'iss53'
    testing 782fd34 add scott to the author list in the readmes


    $ git branch --merged # to see branches already merged in

    iss53
    * master

    $ git branch --no-merged # to see branches not merged in
testing

## Note to self
Git branches except master are always based on other branches, the ones
that were active just before the new branch was created and checked out.
To make sure a branch is based on a particular branch, check out that
particular branch and then check out the new branch


It’s important to remember when you’re doing all this that these
branches are completely local. When you’re branching and merging,
everything is being done only in your Git repository — no server
communication is happening.


To synchronize your work, you run a git fetch origin command. This
command looks up which server origin is
(in this case, it’s git.ourcompany.com), fetches any data from it
that you don’t yet have, and updates your local database, moving your
origin/master pointer to its new, more up-to-date position


It’s important to note that when you do a fetch that brings down new remote branches, you don’t
automatically have local, editable copies of them. In other words, in this case, you don’t have a new
serverfix branch — you only have an origin/serverfix pointer that you can’t modify.
To merge this work into your current working branch, you can run git merge origin/serverfix. If
you want your own serverfix branch that you can work on, you can base it off your remote branch:
$ git checkout -b serverfix origin/serverfix
Branch serverfix set up to track remote branch refs/remotes/origin/serverfix.
Switched to a new branch "serverfix"
This gives you a local branch that you can work on that starts where origin/serverfix is.


4.5.2. Tracking Branches
Note to self:
  what's the difference between cloning, fetching and pulling?
Cloning is the very first one you do to get a remote git repo.
Fetching brings the remote git repo down but does not merge the working
branches.
Pull, brings the remote repo over and automatically does the merging
=end note

Checking out a local branch from a remote branch automatically creates what is called a tracking
branch. Tracking branches are local branches that have a direct relationship to a remote branch. If
you’re on a tracking branch and type git push, Git automatically knows which server and branch to
push to. Also, running git pull while on one of these branches fetches all the remote references and
then automatically merges in the corresponding remote branch.

When you clone a repository, it generally automatically creates a
master branch that tracks origin/master. That’s why git push and git
pull work out of the box with no other arguments. However, you can
set up other tracking branches if you wish — ones that don’t track
branches on origin and don’t track the master branch. The simple case
is the example you just saw, running
    git checkout -b [branch] [remotename]/[branch]. If you have Git
version 1.6.2 or later, you can also use the --track shorthand:

    $ git checkout --track origin/serverfix

Branch serverfix set up to track remote branch refs/remotes/origin/serverfix.
Switched to a new branch "serverfix"
To set up a local branch with a different name than the remote
branch, you can easily use the first version with a different local
branch name:

    $ git checkout -b sf origin/serverfix

Branch sf set up to track remote branch refs/remotes/origin/serverfix.
Switched to a new branch "sf"
Now, your local branch sf will automatically push to and pull from origin/serverfix.


## The Protocols
Git can use four major protocols to transfer data:
 - Local
 - Secure Shell (SSH)
 - Git
 - HTTP

It's important to note that with the exception of HTTP protocols, all of
these require Git to be installed and working on the server.

Git assumes SSH if you aren’t explicit:
    $ git clone user@server:project.git

You can also not specify a user, and Git assumes the user you’re currently logged in as.

If you want to allow anonymous read-only access to your projects, you’ll have to set up SSH for you to push over but
something else for others to pull over.

## The Git Protocol
Next is the Git protocol. This is a special daemon that comes packaged with Git; it listens on a
dedicated port (9418) that provides a service similar to the SSH protocol, but with absolutely no
authentication. In order for a repository to be served over the Git protocol, you must create the git-
export-daemon-ok file — the daemon won’t serve a repository without that file in it — but other
than that there is no security. Either the Git repository is available for everyone to clone or it isn’t.
This means that there is generally no pushing over this protocol. You can enable push access; but
given the lack of authentication, if you turn on push access, anyone on the internet who finds your
project’s URL could push to your project. 

# The Pros
The Git protocol is the fastest transfer protocol available. If you’re serving a lot of traffic for a public
project or serving a very large project that doesn’t require user authentication for read access, it’s
likely that you’ll want to set up a Git daemon to serve your project. It uses the same data-transfer
mechanism as the SSH protocol but without the encryption and authentication overhead.

# The Cons
The downside of the Git protocol is the lack of authentication. It’s generally undesirable for the Git
protocol to be the only access to your project. Generally, you’ll pair it with SSH access for the few
developers who have push (write) access and have everyone else use git:// for read-only access. It’s
also probably the most difficult protocol to set up. It must run its own daemon, which is custom
it requires xinetd configuration
or the like, which isn’t always a walk in the park. It also requires firewall access to port 9418, which
isn’t a standard port that corporate firewalls always allow. Behind big corporate firewalls, this obscure
port is commonly blocked.

## The HTTP/S Protocol
Last we have the HTTP protocol. The beauty of the HTTP or HTTPS protocol is the simplicity of
setting it up. Basically, all you have to do is put the bare Git repository under your HTTP document
root and set up a specific post-update hook, and you’re done
At that point, anyone who can access the web server under which you put the repository can
also clone your repository. To allow read access to your repository over HTTP, do something like this:
    $ cd /var/www/htdocs/
    $ git clone --bare /path/to/git_project gitproject.git
    $ cd gitproject.git
    $ mv hooks/post-update.sample hooks/post-update
    $ chmod a+x hooks/post-update
That’s all. The post-update hook that comes with Git by default runs the appropriate command (git
update-server-info) to make HTTP fetching and cloning work properly. This command is run
when you push to this repository over SSH; then, other people can clone via something like
$ git clone http://example.com/gitproject.git
In this particular case, we’re using the /var/www/htdocs path that is common for Apache setups, but
you can use any static web server — just put the bare repository in its path. The Git data is served as
basic static files

It’s possible to make Git push over HTTP as well, although that technique isn’t as widely used and
requires you to set up complex WebDAV requirements.

# The Pros
The upside of using the HTTP protocol is that it’s easy to set up. Running the handful of required
commands gives you a simple way to give the world read access to your Git repository. It takes only
a few minutes to do. The HTTP protocol also isn’t very resource intensive on your server. Because it
generally uses a static HTTP server to serve all the data, a normal Apache server can serve thousands
of files per second on average — it’s difficult to overload even a small server.
You can also serve your repositories read-only over HTTPS, which means you can encrypt the content
transfer; or you can go so far as to make the clients use specific signed SSL certificates. Generally, if
you’re going to these lengths, it’s easier to use SSH public keys; but it may be a better solution in your
specific case to use signed SSL certificates or other HTTP-based authentication methods for read-only
access over HTTPS.
Another nice thing is that HTTP is such a commonly used protocol that corporate firewalls are often
set up to allow traffic through this port.

# The Cons
The downside of serving your repository over HTTP is that it’s relatively inefficient for the client.
It generally takes a lot longer to clone or fetch from the repository, and you often have a lot more
network overhead and transfer volume over HTTP than with any of the other network protocols.
Because it’s not as intelligent about transferring only the data you need — there is no dynamic work
on the part of the server in these transactions — the HTTP protocol is often referred to as a dumb
protocol.

clone is basically a git init and a git fetch.

Generating Your SSH Public Key
That being said, many Git servers authenticate using SSH public keys. In order to provide a public
key, each user in your system must generate one if they don’t already have one. This process is similar
across all operating systems. First, you should check to make sure you don’t already have a key. By
default, a user’s SSH keys are stored in that user’s ~/.ssh directory. You can easily check to see if
you have a key already by going to that directory and listing the contents:
    $ cd ~/.ssh
    $ ls
<samp>
    authorized_keys2
    config  id_dsa
    id_dsa.pub  known_hosts
</samp>

You’re looking for a pair of files named something and something.pub, where the something is
usually `id_dsa` or `id_rsa`. The .pub file is your public key, and the other file is your private key. If
you don’t have these files (or you don’t even have a .ssh directory), you can create them by running
a program called ssh-keygen, which is provided with the SSH package on Linux/Mac systems and
comes with the MSysGit package on Windows:
    $ ssh-keygen
<samp>
    Generating public/private rsa key pair.
    Enter file in which to save the key (/Users/schacon/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /Users/schacon/.ssh/id_rsa.
    Your public key has been saved in /Users/schacon/.ssh/id_rsa.pub.
    The key fingerprint is:
    43:c5:5b:5f:b1:f1:50:43:ad:20:a6:92:6a:1f:9a:3a schacon@agadorlaptop.local
</samp>

First it confirms where you want to save the key (`.ssh/id_rsa`), and then it asks twice for a
passphrase, which you can leave empty if you don’t want to type a password when you use the key.
Now, each user that does this has to send their public key to you or whoever is administrating the Git
server (assuming you’re using an SSH server setup that requires public keys). All they have
to do is
copy the contents of the .pub file and e-mail it. The public keys look something like this:

    $ cat ~/.ssh/id_rsa.pub

<samp>
  ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAklOUpkDHrfHY17SbrmTIpNLTGK9Tjom/BWDSU
  GPl+nafzlHDTYW7hdI4yZ5ew18JH4JW9jbhUFrviQzM7xlELEVf4h9lFX5QVkbPppSwg0cda3
  Pbv7kOdJ/MTyBlWXFCR+HAo3FXRitBqxiX1nKhXpHAZsMciLq8V6RjsNAQwdsdMFvSlVK/7XA
  t3FaoJoAsncM1Q9x5+3V0Ww68/eIFmb1zuUFljQJKprrX88XypNDvjYNby6vw/Pb0rwert/En
  mZ+AW4OZPnTPI89ZPmVMLuayrD2cE86Z/il8b+gw3r3+1nKatmIkjn2so1d01QraTlMqVSsbx
  NrRFi9wrf+M7Q== schacon@agadorlaptop.local
</samp>

For a more in-depth tutorial on creating an SSH key on multiple operating systems, see the GitHub
guide on SSH keys at http://github.com/guides/providing-your-ssh-key.

    git instaweb --httpd=webrick  # in a directory that has got a git repo.

## GitWeb
Now that you have basic read/write and read-only access to your project, you may want to set up a
simple web-based visualizer. Git comes with a CGI script called GitWeb that is commonly used for
this.
If you want to check out what GitWeb would look like for your project, Git comes with a command
to fire up a temporary instance if you have a lightweight server on your system like lighttpd or
webrick. On Linux machines, lighttpd is often installed, so you may be able to get it to run by
typing git instaweb in your project directory. If you’re running a Mac, Leopard comes preinstalled
with Ruby, so webrick may be your best bet. To start instaweb with a non-lighttpd handler, you can
run it with the --httpd option.

    $ git instaweb --httpd=webrick

    [2009-02-21 10:02:21] INFO WEBrick 1.3.1
    [2009-02-21 10:02:21] INFO ruby 1.8.6 (2008-03-03) [universal-darwin9.0]

That starts up an HTTPD server on port 1234 and then automatically starts a web browser that opens
on that page. It’s pretty easy on your part. When you’re done and want to shut down the server, you
can run the same command with the --stop option:

    $ git instaweb --httpd=webrick --stop

If you want to run the web interface on a server all the time for your team or for an open source
project you’re hosting, you’ll need to set up the CGI script to be served by your normal web server.


## Side Note
    $ find .  # finds all hidden files in the specied directory.

    $ git rebase --interactive  # Re-write commit history. Be careful
though.

    $ git ls-files


`git diff [--options] [--] [<path>...]`
This form is to view the changes you made relative to the index
(staging area for the next commit). In other words, the
differences are what you could tell git to further add to the
index but you still haven’t. You can stage these changes by using
`git-add(1)`.

If exactly two paths are given and at least one points outside the
current repository, git diff will compare the two files /
directories. This behavior can be forced by --no-index.

`git-stash` - Stash the changes in a dirty working directory away

*SYNOPSIS*

    git stash list [<options>]
    git stash show [<stash>]
    git stash drop [-q|--quiet] [<stash>]
    git stash ( pop | apply ) [--index] [-q|--quiet] [<stash>]
    git stash branch <branchname> [<stash>]
    git stash [save [-p|--patch] [-k|--[no-]keep-index] [-q|--quiet] [<message>]]
    git stash clear
    git stash create

## Note
Working directory == before $git add
index == after git add
HEAD == most recent commit || current branch

`@@ -47,8 +47,8 @@`
`@@` are sort of like change information delimiters
`-` represents first file
`+` represents second file
first numbers, 47 represent the starting line, of changes.
second numbers, 8 represent the number of affected lines.


## SHA : Secure Hash Algorithm.
A SHA creates an identifier of fixed length that uniquely identifies a specific
piece of content. SHA-1 succeeded SHA-0.

## Git Object Types
Git *objects* are the actual data of Git, the main thing that the repository 
is made up of.

There are four main object types in Git, the first three being the most 
important to really understand the main functions of Git.

All of these types of objects are stored in the Git `Object Database` which
is kept in the `Git Directory`. Each object is compressed (with Zlib) and 
referenced by the SHA-1 value of its contents plus a small header. (The SHA-1
string is 40 characters long).

## The Blob
In Git, the contents of files are stored as `blobs`.

It is important to note that it is the `contents` that are stored, not the 
files. The names and modes of the files are not stored with the blob, just the
contents.
This means that if you have two files anywhere in your project that are 
exactly the same, even if they have different names, Git will only store the 
blob once. This also means that during repository transfers, such as clones or
fetches, Git will only transfer the blob once, then expand it out into multiple
files upon checkout.

## The Tree
Directories in Git basically correspond to `trees`.
A tree is a simple list of trees and blobs that the tree contains, along with
the names and modes of those trees and blobs. The contents section of a tree
object consists of a very simple text file that lists the mode, type, name and
SHA of each entry.

## The Commit
So, now that we can store arbitrary trees of content in Git, where does the 
'history' part of 'tree history storage system' come in? The answer is the 
`commit` object.

The commit is very simple, much like the tree. It simply points to a tree and
keeps:
  - an `author`, 
  - `committer`, 
  - `message` and 
  - any `parent` commits that directly preceded it.

Most times a commit will only have a single parent but if you merge two 
branches, the next commit will point to both of them.

## The Tag
The final type of object you'll find in a Git database is the `tag`. This is an
object that provides a parmanent shorthand name for a particular commit. It 
contains:
  - An object
  - type
  - tag
  - tagger and
  - a message

Normally the type is `commit` and the object is the SHA-1 of the commit you're 
tagging. The tag can also be GPG signed, providing cryptographic integrity to a
release version.

We'll talk a little bit more about the tags and how they differ from branches
(which also point to commits, but are not stored as objects) in the next 
section, where we'll put all of this together into how all these objects relate
to each other conceptually.


## The Git Data Model
In computer science speak, the Git object data is a `directed acyclic
graph`. That is, starting at any commit you can traverse it's parents in
one direction and there's no chain that begins and ends with the same
object.

All commit objects point to a tree and optionally to previous commits.
All trees point to one or many blobs and/or trees. Given this simple
model, we can store and retrieve vast histories of complex trees of
arbitrarily changing content quickly and efficiently.

 
## References
In addition to the Git objects, which are immutable - that is, they
cannot be changed, there are references also stored in Git. Unlike the
objects, references can constantly change. They are simple pointers to a
particular commit, something like a tag, but easily movable.

Examples of references are branches and remotes. A branch in Git is
nothing more than a file in the `.git/refs/heads/` directory that
contains the SHA-1 of the most recent commit of that branch. To branch
that line of development, all Git does is create a new file in that
directory that points to the same SHA-1. As you continue to commmit, one
of the branches will keep changing to point to the new commit SHA-1s
while the other can stay where it was.


## Traversal
So, what do all the arrows in these illustrations really mean? How does
Git atually retrieve these objects in practice? Well, it gets the
initial SHA-1 of the starting commit by looking in `.git/refs/`
directory for the branch, tag or remote you specify. Then it traverses
the objects by walking the trees one by one, checking out the blobs
under the names listed.

    $ git checkout <tag_name | branch_name | remote>


## Branching and Merging
Here we come to one of the real strengths of Git, cheap inline
branching. This is a feature that truly sets it apart and will likely
change the way you think about developing code once you get used to it.

When we are working on code in Git, storing trees in any state and
keeping pointers to them is very simple, as we've seen. In fact, in Git
the act of creating a new branch is simply writing a file in the
`.git/refs/heads` directory that has the SHA-1 of the last commit for
that branch.

## Note
Creating a branch is nothing more than just writing 40 characters to a
file

Switching to that branch simply means having Git make your working
directory look like the tree that SHA-1 points to and updating the HEAD
file so each commit from that point on moves that branch pointer forward
(in other words, it changes the 40 characters in
`.git/refs/heads/[current_branch_name]` be the SHA-1 of your last
commit).


## Remotes
Now let's take a look at remotes. Remotes are basically pointers to
branches in other people's copies of the same repository, often on other
computers. If you got your repository by cloning it, rather than
initializing it, you should have a remote branch of where you copied it
from automatically added as origin by default. Which means the tree that
was checked out during your initial clone would be referenced as
origin/master, which means "the master branch of the origin remote."

Let's say you clone someone's repository and make a few changes. You
would have two references, one to origin/master which points to where
the master branch was on the person's repository you cloned from when
you did so, and a master branch that points to the most recent local
commit.

Now let's say you run a `fetch`. A `fetch` pulls all the refs and
objects that you don't alread have from the remote repository you
specify. By default, it is origin, but you can name your remotes
anything, and you can have more than one. Suppose we fetch from the
repository that we originally cloned from and they had been doing some
work. They have now commited a few times on their master branch, and
they also branched off at one point to try an idea, andt hey named the
branch *idea* locally, then they pushed that branch. We now have access
to those changes as origin/idea.
We look at the idea branch and like where they are going with it, but we
also want the changes they've made on the master branch, so we do a
3-way merge of their branches and our master. We don't know how well
this is going to work, so we make a *tryidea* branch first and  then do
the merge there.

Now we can run our tests and merge everything back to our master branch
of we want. Then we can tell our friend we cloned from to fetch from our
repository, where we've merged their two branches for them and
integrated some of our changes as well. They can choose to accept or
reject that patch.


    $ git citool

## Getting a Git Repository
There are two major ways you will get a Git repository:
 1) clone or
 2) initialize a new one.


## Note:
Pay extra attention to the first line of your commit message - it
will often be the only thing people see when they are looking
through your commit history.
