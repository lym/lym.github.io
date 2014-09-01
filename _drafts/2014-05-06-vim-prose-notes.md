---
layout: post
title: "Vim prose notes"
description: "Another unfiltered dump of notes"
category: "Vim"
tags: ["Vim", "Tools", "Programming", "Text Editors"]
---
{% include JB/setup %}

# vim = vi Improved

Note: If you read the vim documentation, you will notice that
(confusingly) command mode is called normal mode and ex commands are
called command mode. Beware.

Moving the Cursor Around:

    0 (zero) : To the beginning of the current line

    SHIFT-6 (^) : To the first non-whitespace character on the current line.

    SHIFT-4 ($) : To the end of the current line.

    W : To the beginning of the next word or punctuation character.

    `<CTR>W s`  : split window horizontally; same as :split

    `<CTR>W v`  : split window horizontally : same as :vsplit

    `<CTR>W n`  : open a new buffer (empty) : same as :new alternate is :vnew



    `SHIFT-W (W)` : To the beginning of the next word, ignoring punctu
    ation characters.

    B : To the beginning of the previous word or punctuation character.

    J : Remove a line-break. Concatenate two lines.

    u : undo most recent action.

    a : append text after current cursor posn. kinda like the opposite of
    'i' 

    e : move to next end of word
    CTRL + R: redo.

    CTRL + ] : follow a hyperlink, or more accurately a tag

    SHIFT-B ( B) : To the beginning of the previous word, ignoring
    punctuation characters.

    CTRL-F or PAGE DOWN : Down one page.

    CTRL-B or PAGE UP : Up one page.

    CTRL-W L : Move to the window Right

    CTRL-W H : Move to the window Left 

    CTRL-W K : Move to the window top

    CTRL-W J : Move to the window down

    number-SHIFT-G : To line number (for example, 1G moves to the first line
    of the file)

    SHIFT-G (G) : To the last line of the file

    gg : jump to beginning of file.

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

## Opening a Line:
Another way we can insert text is by “opening” a line. This inserts a blank
line between two existing lines and enters insert mode. This has two
variants, as shown in Table 12-2.
`o` : The line below the current line
`O` : The line above the current line

## Deleting Text:
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

## Text Deletion Commands:
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

## Cutting, Copying, and Pasting Text:
The d command not only deletes text, it also "cuts" text. Each time we use
the d command, the deletion is copied into a paste buffer (think clipboard)
that we can later recall with the p command to paste the contents of the buf-
fer after the cursor or with the P command to paste the contents before the
cursor.

The y command is used to "yank" (copy) text in much the same way the
d command is used to cut text. Table 12-4 lists some examples combining
the y command with various movement commands.

## Yanking Commands
    yy : The current line
    5yy : The current line and the next four lines
    yW : From the current cursor location to the beginning of the next word.
    y$ : From the current cursor location to the end of the current line.
    y0 : From the current cursor location to the beginning of the current line.
    y^ : From the current cursor location to the first non-whitespace character in the line.
    yG : From the current line to the end of the file
    y20G : From the current line to the 20th line of the file.

## Joining Lines:
vi is rather strict about its idea of a line. Normally, it is not possible to move
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

## Search and Replace
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

Place the cursor on the first line of the file. Type `/Line`

followed by the ENTER key. The cursor will move to line 2. Next, type n,
and the cursor will move to line 3. Repeating the n command will move the
cursor down the file until it runs out of matches. While we have so far used
only words and phrases for our search patterns, vi allows the use of regular
expressions, a powerful method of expressing complex text patterns. 

## Global Search and Replace:
vi uses an ex command to perform search-and-replace operations (called
substitution in vi) over a range of lines or the entire file. To change the word
Line to line for the entire file, we would enter the following command:

    :%s/Line/line/g

Let’s break this command down into separate items and see what each
one does
An Example of Global Search-and-Replace Syntax
`:` The colon caharacter starts an ex command.
`%` : Specifies the range of lines for the operation. % is a shortcut
meaning from the first line to the last line. Alternatively, the
range could have been specified 1,5 (because our file is five lines
long), or 1,$, which means from line 1 to the last line in the file.
If the range of lines is omitted, the operation is performed only on
the current line.
`s` : Specifies the operation - in this case, substitution (search and replace)
`/Line/line` : The search pattern and the replacement text.
`g` : This means global, in the sense that the substitution is
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

## Editing Multiple Files:
It’s often useful to edit more than one file at a time. You might need to
make changes to multiple files, or you may need to copy content from one
file into another. With vi we can open multiple files for editing by specifying
them on the command line:

    vi file1 file2 file3...

To switch from one file to the next, use this ex command: `:n`
To move back to the previous file, use: `:N`

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


The more common command to save a file is :w, however, this always
saves the file, even if it hasn't changed. 

    :up preserves timestamps and preserves needless disk access.

    :bw [buffer No.] : to close one buffer(file)

    " : starts a comment.

    set tabstop=4 : my tabstop


For practical purposes there are four modes:

 - Insert mode
Use only for typing; not moving around or editing. Stay in this mode for as short a time
as possible.

 - Normal mode
Use this for editing: moving around the file, changing text, and rearranging structure. Dip
in and out of Insert mode when needed.

 - Visual mode
Use this for visually selecting text so that you can cut, copy, or format it.
Command-Line mode
Use this for entering commands, e.g. :set number


Use `<Ctrl>+o` in Insert mode to switch to Normal mode for one command, then
return to Insert mode. For example, `<Ctrl>+o` gqas enters Normal mode, reformats the
current sentence,1 then returns you to Insert mode.


    <Ctrl>+F to scroll downwards

caw deletes the current word and puts you into Insert mode to change
it. Once you have done so, hit `<Esc>` again to return to Normal mode.


The traditional approach is to use the arrow keys to move up, down, left, and right. Vim
supports that style of navigation but also offers a more efficient alternative:

    Key : Movement
    h 	: Left
    l 	: Right
    k 	: Up a line
    j 	: Down a line
    0 	: Start of line
    ^ 	: First printing character of line
    $ 	: End of line
    G   : End of file


Another benefit is that you can prefix these shortcuts with counts
(as you can with many Vim commands) which specify how many times they
should be executed. For instance, 2k moves up two lines. Once you've
become used to these keys, take a look at motions and text objects in
Selecting Text with Motions to make the humble combination of h, l,
k, and j more powerful still.

    :%y+  : to yank all lines to the system clipboard.

    "+p or "*p to paste from clipboard

    dw = delete word

    w = beginning of next word

    A = move to the end and start appending (move to end and enter insert mode)

    b = backtract word by word from behind. Opposite of w

    ^ move to next non whitespace character.

    n move to next match, used in combination with / while searching.

    e = move to the end of a word

    b = move to the beginning of a word

    4yy = Copy the current line and the 3 following it.

    c = [change] deletes then enters insert mode.

    daw/caw = delete/change entire word irrespective of the cursor position in th word.
    :registers  to see the contents of your registers.

    das/cas = delete/chenge entire sentence.

    dap : delete a paragraph

    vim -p [filename1, filename2,...,filenamen] to open the specified
    files in their own tabs in the same window. Navigate the different
    tabs using :tabNext and tabprevious, among other commands.

    vim -O [filename1, filename2,...,filenamen] to open the specified
    files in vertically split windows. Use -o for the default,
    horizontally split windows.

To paste text from the system clipboard use Shift+Ins in Insert mode
or `"*` in Normal mode. Conversely, "+y yanks the current selection
to the system clipboard.

Note: The solution above uses the concept of a single clipboard, much
like some operating systems do. Vim can work this way, as you can
see, but also supports 'named registers'. These are effectively,
multiple independent clipboards. Registers are named with a "
character followed by a single lowercase letter, e.g. "a.

To yank/delete/put using a named register, simply prefix the command
with the register name. So, to yank the current line to register "b
use "byy. To paste it use "bp.


Note: gVim reads vimrc then a gvimrc file located in the same place an vimrc.

    :vsplit : open another file in a vertically split window. :vs =>
    shorthand

    :x : another way to quit vim

    :version, :echo $VIM

    "*for some reason this text gets highlighted in my txt's"*

    >,< : tab left or right according to shiftwidth.

    :![command_name] : to run other programs without exiting vim
    e.g; :!ruby -w some_file.rb
    The concept is that Vim suspends itself, asks your system to execute the
    command, shows you it's output, then once the user presses <Enter> ,
    returns you to Vim.

    % references the current file

    :Ex [directory_name] : to view the file contents of a directory kinda
    like what nerdtree does.

    :set title titlestring=Mytitle : to set the title of the editor to
    something more custom.

    :vimgrep /pattern/ file1 file2 file3 .... filen : to search for a
    pattern in any given number of files.


vimgrep jumps to the first match it finds. To jump to the next match use :cn;
use :cN for the previous.

    :Sex [directory_name]
    :Vex [directory_name]
    :Tex [directory_name]

to see the file listings on the left side, in an upper split or in a new
tab respectively.


    ["x]d{motion} : delete text that "motion" moves over into register x.
                    Retrieve/paste into different are by hitting <shift> + "
                    + x + p
                    The x can be any alpha from a-z or A-Z. It represents a
                    named register.
    ["x]d$ : deletes, into register x, till the end of the line. same as D.

    :[range] d[elete] [x] {count} : delete provided range into register x.

    R : Enter replace mode, each character you type replaces a character
    under the cursor. Under replace mode the backspace key replaces the
    previous text, if any.

    r{char} : replace character under cursor with char. For quick changes.

    g~~ : switch case of current line.

    gU{motion} : make motion text uppercase.

    gu{motion} : make motion text lower case.

    gUU : make current line Upper case.

    guu : make current line lower text.

    gf : (go to file) open file whose name is under the cursor.

If your filename is followed by a line number, e.g. foo.txt:10 you can
jump to the given line with gF.

    Vim help 29%

    vimdiff file1 file2 : to checkout the differences between two files.

    [action] [movement] eg; >3j: tab four lines below
    "." : to repeat the command.

    ! : the override command

The override command modifier is needed because Vim is reluctant to throw
away changes. If you were to just type ":q", Vim would display an error
message and refuse to exit:
E37: No write since last change (use ! to override) ~
By specifying the override, you are in effect telling Vim, "I know that what
I'm doing looks stupid, but I'm a big boy and really want to do this."
If you want to continue editing with Vim: The ":e!" command reloads the
original version of the file.

As you read the help text, you will notice some text enclosed in vertical bars
(for example, |help|). This indicates a hyperlink. If you position the

cursor anywhere between the bars and press CTRL−] (jump to tag), the help
system takes you to the indicated subject. (For reasons not discussed here,
the Vim terminology for a hyperlink is tag. So CTRL−] jumps to the location
of the tag given by the word under the cursor.)
After a few jumps, you might want to go back. CTRL−T (pop tag) takes you
back to the preceding position. CTRL−O (jump to older position) also works
nicely here.


    d) : delete entire sentence.

Write a privileged file with 
    :SudoW or 
    :SudoWrite, 
it will prompt for sudo password when writing


Leader key : Args : Macros
gvim theme : Updated > dark > Neverness


    :vimgrep /term/ **/*.ext : search for files containing "term".

    <leader> key = \


*set indentation to 8 for c code*
    :set tabstop=8
    :set softtabstop=8
    :set shiftwidth=8
    :set noexpandtab
    :retab!           # to activate your settings in current session.

*shiftwidth* controls how many spaces are inserted when using the
`<</<<`
technique, or the automatic indenting used with source code.

*softtabstop* specifies how many columns Vim uses when Tab is hit in
insert mode. If it's less than *tabstop*, and Vim's not expanding tabs
(:set noexpandtab), Vim indents with tabs, padding with spaces where
necessary.

    $ vim [file_name] +[line_number]	# opens file and places cursor
                                          at line_number

    echo g:colors_name 
    :echo g:[variable_name]

## Navigating Source Code
    Key		| Move To
    %			| End of construct
    [[	  | Backwards to the beginning of the current function.
    ][		| Forwards to the beginning of the current function.
    ]}		| Beginning of the current block.
    [{		| End of current block.
    }[		| Beginning of current comment block
    }]		| End of current comment block.
    gd		| First usage of current variable name. (go to definition)
    gD		| Go to the first usage of the current variable name.

## Navigating the viewport
    H  | Top of the screen (Home)
    L  | Bottom of the screen (Lower)
    M  | Middle of the screen. (Middle)
    gg | Top of file
    G  |Bottom of file

*tabs* are used for grouping related files in a source project.

    set foldmethod=syntax
    set foldmethod=indent

## Folding
Fold commands start with z. 

    Command		| Action
    zf				| Fold the selected text
    zf#j			| Create a fold from the cursor down # lines
    zf/string | Create a fold from the cursor to string
    zfaB			| Fold the current block delimited by bracket `B`

`B` can be any of `()` `[]` `{}` `<>`. This feature understands nested blocks,
too, so will usually do the right thing.

zf4j : create a fold from the current line to the forth line down.

    Command | Action
    zc			| Close the current fold.
    zo			| Open the current fold.
    zM			| Close all folds.
    zr			| Open one level of folds.
    zR			| Open all folds.
    zj			| Move to the next fold.
    zk			| Move to the previous fold.
    zm			| Close one level of folds.
    zn			| Disable folding.
    zN			| Re-enable folding.

By default, folds are forgotten when you edit another file. To save them
use `:mkview`. Then , to restore them, use `:loadview`

## Navigating Marks
You want to bookmark specific points in a file so you can easily jump to
them from elsewhere.

Use Vim's `marks` feature. A mark is a character in the range
`a-zA-Z0-9`. Its represented in the examples below as M.

    Command	| Action
    mM			| Mark the current position as M.
    'M			| Jump to the first character of the line containing M
    `M			| Jump to the position of mark M.
    ``			| return to previous position.

    :marks  | show a list of marks set.

Marks 0-9 are mainly for Vim's internal use, so ignore them. Marks a-z
are only available in the current file, and are deleted when it is
closed. Marks A-Z are available across multiple files. If your .viminfo
file is available, they will persist across sessions.

# GVim
## Maximising Screen Space
The toolbar, menubar and other GUI artifacts take up too much of your
screen; you may want to hide them.

Solution
Modify the `guioptions` variable. Gvim decides which elements of the GUI
to display baased on the value of `guioptions`. This is a series of
letters, each of which refer to some specific element. Some examples
follow:

+ m : Display a menu bar
+ T : Display a toolbar
+ r : Always display the right-hand scrollbar.
+ R : Display the right-hand scrollbar if the window is split
	vertically.
+ l : Always display a left-hand scroll.
+ L : Display the left-hand scrollbar if the window is split vertically.
+ b : Display the horizontal scrollbar.

So to hide the menu bar, toolbar and scrollbars you could use 

	:set guioptions=mTrlb

To display a hidden element use `+=` instead, e.g.

	:set guioptions+=T


## Creating Menus and Toolbar Buttons
You want to add your own commands to Gvim's menus or toolbar for quick
access.

For example you've written a function that does something neat but you
are not willing to use it if you have to type its name everytime; you
want to invoke it by selecting a menu option.

Use `set amenu <menu> <command>` to map a menu item to a command. This
is the GUI equivalent of `:map`.

For example,

	:amenu Help.Op&tions :help options<cr>

adds a new item called "Options" to the <i>Help</i> menu which invokes
`:help options`. The ampersand (&) signifies that the character it
prefixes can be used as a keyboard shortcut, so in this case `<Alt>+h+t`
selects this command.

You are not restricted to adding items to existing menus; you can create
a new top-level menu simply by specifying a name not currently in use.
For example:

	:amenu <silent>&Vim\.org :!xdg-open http://www.vim.org/<cr>

will create a new top-level menu called "Vim" with the shortcut key "V".
It will contain one entry named <i>vim.org</i> (we escape the . because
otherwise it would create a vim entry which in turn contains an org
item). When invoked it will open the Vim website on systems adhering to
the Free Desktop Specification. The `<silent>` prefix prevents the command
from being echoed on the command-line.

If you want to add a dashed separator line between menu items use a menu
item named `-SEP-` and an empty command, e.g; 

	:amenu Help.-SEP- :.


To control where a top-level menu appears relative to its neighbors you
need to prefix `amenu` with a numeric priority: the lower the number the
further right the menu's position.

For example,

	:5amenu First.first :echo 'first'<cr>

creates a top-level menu named <i>First</i> that appears before all of the 
others.

You can also use `:amenu` to add a new toolbar icon:

	:amenu icon=image-path Toolbar.item-name
	command

For example:

	:amenu icon=optiions.png ToolBar.OptionsHelp :help<cr>

if the `image-path` consists only of a filename, as above, Vim prepends
$VIMRUNTIME/bitmaps/ to it.

## Looking up Documentation for the Keyword under the Cursor
You want to invoke an external command to lookup documentation for the
keyword underneath the cursor. For example, Linux users may like to read the
manual for the named command with the `man` utility.

Use `:set keywordprg=program`, then hit K while hovering over the word.

This recipe calls the command specified with `:set keywordprg`, passing
the current word as an argument. Thus, if `keywordprg = man`, then
hovering over the word `ls`	and hitting `K` would display the
documentation for Linux's `ls` command.

When used with `man`, Vim translates a count for the K command into a
section number. So `7K` over glob invokes `man 7 glob` to display
section 7 of the `glob` documentation.

The Ruby programming language has a utility called `ri` that displays
documentation about the given Ruby method. The Perl programming language
has a similar command called `perldoc`. By setting `keywordprg`
appropriately, you can make context-sensitive documentaion lookup
trivial.


## Changing The Window Title
You want to change the title of a Vim window to make it more
descriptive.

Solution:
Assign a value to the `titlestring` option and set the `title` option.
For example:

	:set title titlestring=My\ Title

When working with multiple instances of Vim, it can be difficult to
remember what task each window corresponds to. You can avoid this
problem by customizing each window's title. The window title can also
function similarly to the status line, reminding the user about an
important aspect of the current file.

The default window title contains the current filename, followed by a
character indicating the state of this file, followed by the name of its
directory. The state character is one of the following:

	- File can't be modified

	+ File has been modified

	= File is read-only

	=+ File is read-only and has been modified.

This is already quite a descriptive title, but we can customize it
further using a format string, as described in the "Changing the Status
Line" recipe. The Vim documentation (:help titlestring) gives the
following example:

	:auto BufEnter * let &titlestring = hostname() . "/" .
	expand("%:p")

Here the window title is reset when the user enters a new buffer. It
contains the hostname, a forward slash, then the full path of the
current file.

Another example is to display the value of an environment variable in
the window title along with the filename. For instance, Ruby on Rails
developers could prefix the filename with the value of `RAILS_ENV`,
which indicates whether the application is in development, production,
staging or testing mode:

	let &titlestring=expand($RAILS_ENV) . ": " .
	expand("%:t")

One last trick is to embed the value of an external command in the
window title using the `%{system('command')}` syntax. This could be used
to display the name of the current branch, if using a version control
system, or indicate whether the project's unit teste are passing or
failing.


## Mapping commands
    :vmap		Visual mode mapping
                    e.g		:vmap Q gq

    :imap		Insert mode only

    :cmap		Command-line only

    :map		Normal, Visual and Operator-pending
                    e.g		:map <Space> <PageDown>

It is generally recommended to map the function keys (`<F1>-<F12>`), as
well as their shifted counterparts (e.g. `<Shift>-<F3>`) because they are
not used by Vim.

Before creating a mapping you might like to check what, if anything,
it's currently being used for. You can do this by executing 

	`:help` <i>key</i>

e.g. :help `<F1>` will show that Vim maps it to :help. If you wan to see
the user-defined mappings (whether set by you or a plugin) call the
`:map` command with no arguments. This works with the mode-specific map
commands outlined above, too, so :imap will show Insert mode mappings.


## Changing the colour scheme
You don't like the colors Vim uses; you want to change them.

For example, you've found a colour scheme you like better, so want to
instruct Vim to use it. Or you find that the current color scheme makes
text hard to read so you want to find a more suitable one.

To browse existing colour schemes enter `:colourscheme`, then hit
`<Tab>`
to cycle through the installed schemes. If you find one that you like
hit `<Enter>` to apply it.

A color scheme is a set of rules controlling how different elements of
teh interface appear. Vim is distributed with a selection of color
schemes, but one can also download new ones.

### Installing Color schemes
1. Browse the available color schemes at "Vim.org" and download any that
	 you like.
2. Create a $VIM/colors, e.g. mkdir -p ~/.vim/colors on POSIX systems.
3. Copy the .vim file you downloaded in step one to the colors directory
	 you just created.
4. Open vim then execute :colorscheme name, where name is that of the
	 file you downloaded without the .vim extension.
5. If you want to use this color scheme permanently add colorscheme name
	 to you vimrc; otherwise repeate these steps with a different color
scheme.


## Creating Command-line Commands
You want to create your own `:command` command.

Use the `command` command like so

	command name <command>

where name is the command you're creating and `<command>` the command name
should execute. (name must start with an initial capital)

For example, 

	:command Ls !ls -all % lets you use :Ls to view the long listing for
the current file on POSIX systems, thus showing the permissions, owner,
group, etc.

The command can be anything you could enter at the : prompt.

You can modify how the command is defined by supplying :command with a
list of arguments with the syntax

	:command arg1, arg2, ... , argN name <command>

These are not to be confused with arguments passed to the `<command>,`
itself, however.

To create a command that accepts arguments you use the syntax

	:command -nargs=spec name command

where spec is:

    1	: One argument

    * : Any number of arguments

    ? : Zero or one argument

    + : One or more arguments.

You reference the arguments in `<command>` with the `<args>` placeholder.
The `<q-args>` quotes special characters in the argument. Foe example you
could use
	
	:command -nargs=1 Ci !cd %:h && git commit %:t -m <q-args>

to quickly change to the directory containing the current file (%:h is
the current pathname with the last component removed) and commit the
current file (%:t is the last component of the current pathname) to a
Git repository by typing

	:Ci message without worrying about using quotation marks and the like.

To create a command that accepts a count you use the -count=default
argument, then reference the count in command as `<count>`

To create a command that accepts a range you use the -range=spec
argument. If you don't supply a spec (i.e -range), the range defaults
to the current line. A spec of % means that the range defaults to the
whole file. You can reference the range in the command with the
placeholders `<line1>` and `<line2>` which denote the first and last line of
the given range, respectively.


## Extending Vim With Scripts and Plugins
You want to add functionality to Vim, preferably without having to write
it yourself.

Browse (Vim Scripts)[http://www.vim.org/scripts/] to find a script that
meets your needs. Its 'type' should be *utility* or *ftplugin*. Download
the latest version to your computer. If the plugin comes with its own
installation instructions, use those; otherwise, read on.

If the file you've downloaded has a name ending with `.vim`, you usually
just need to save it in the right directory and then its ready to use.
For scripts labelled *utility* also known as *global plugins*, this
directory is $VIMHOME/plugin; for those labelled *ftplugin*, also known
as *filetype plugins*, the last component of this path is *ftplugin*
instead. If this directory does not already exist you need to create it.

Plugins can either be global or filetype-specific. Global plugins are
loaded for every file you open; filetype-specific plugins are only
loaded for certain filetypes.

Vim 7 added support for a new plugin installation method called
*vimball*. Vimballs make plugin installation and configuration easier
and are a slight improvement over the previous methods. They are not in
wide use yet, but if you find a plugin distributed in this way (they
have a .vba extension), try the steps below:
1. Download the .vba file
2. Open it with Vim, e.g. vim something.vba.
3. Use :VimballList to verify its contents.
4. Install it by sourcing :source %.

Side note
By Unix/Linux convention filenames which begin with a period are
designated hidden. By default file browsers and other utilities ignore
these files unless explicitly commanded not to.



# FILETYPE DETECTION
Scenario:
You want to write language specific rules for the Vim Editor. For
example you're hacking on the kernel but you also write Ruby code when
you're not working on kernel source.

Possible solution:
Write a c.vim file in the Vim language
place this script in `$VIMRUNTIME/ftplugin/`
Tell Vim how to detect this particular filetype by entering:

    au BufNewFile,BufRead *.foo				set filetype=foofoo

Write this single-line file as "ftdetect/foofoo.vim" in the first
directory that appears in 'runtimepath'. For Unix that would be

    "~/.vim/ftdetect/foofoo.vim" The convetion is to use the name of hte

filetype for the script name.
