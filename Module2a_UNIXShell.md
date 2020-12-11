---
title: "Introduction to UNIX Shell"
author: "UM Bioinformatics Core"
date: "2020-12-11"
output:
    html_document:
        theme: readable
        toc: true
        toc_depth: 4
        toc_float: true
        number_sections: true
        fig_caption: true
        keep_md: true
---

<!---
library(rmarkdown)
render('Module2a_UNIXShell.Rmd', knit_root_dir = 'markdown')
--->

<!--- Allow the page to be wider --->
<style>
    body .main-container {
        max-width: 1200px;
    }
</style>

> # Objectives
> *
> *
> *
> *
>

# Background

We interact with computers in myriad ways: with keyboard and mouse, with our fingers in touch interfaces, or with our voice. The **graphical user interface** (GUI) is the most widely used way to interact. A GUI let's us interact with elements on a screen to accomplish various tasks.

A GUI is relatively intuitive to learn, but it doesn't scale very well for repetitive tasks. To illustrate this -- and since we're here to learn how to do RNA-seq analysis -- let's imagine that we have 20 samples fresh off the sequencer, and we'd like to know which genes are differentially expressed among various groupings of the samples. Using a GUI we'd likely be clicking through the repetitive steps for each sample for hours, and the potential for making a mistake would be high.

The Unix shell can help us avoid such drudgery. The Unix shell is both a **command-line interface** (CLI) and a scripting language, allowing such repetitive tasks to be done automatically and quickly. With the proper commands, the shell can repeat tasks with or without modification as many times as needed. Using the shell, the analysis of the RNA-seq data would require much less of our time. Moreover, the best practices for such an analysis consists of tools which usually *require* the use of the CLI.

# The shell

The shell is a program where we type commands. It enables us to invoke, often with one line of code, complex programs like sequence aligners or simple commands that list the contents of a directory. The most popular Unix shell is Bash, and is the default on most modern versions of Unix and packages providing Unix-like tools for Windows (e.g. gitbash). With macOS Catalina and later, Apple has changed the default shell to zsh, but the behavior is similar enough for our purposes.

Using the shell will take some getting used to. While a GUI naturally presents us with choices to select from, a CLI does not so we must learn commands in a manner not dissimilar from learning a new language. We will cover some essential commands during this module.

The grammar of a shell allows us to combine existing tools into powerful pipelines handling large volumes of data automatically. Sequences of commands can be written in a script, improving the reproducibility of workflows. This is *exactly* what we need for our RNA-seq analysis.

Moreover, the command line is typically the only way to interact with remote machines and high-perforance computing clusters (like Great Lakes). The personal computers that we use on a day-to-day basis simply don't have enough storage, memory, or compute power to analyze an RNA-seq experiment in a reasonable amount of time.

When we open a new shell, we are taken to a **prompt**, indicating the shel is waiting for input.

```
$
```

The shell typically uses `$ ` as the prompt, but may use a different symbol (e.g. macOS uses `% `). Going forward, when typing commands, from this lesson or other sources, *do not type the prompt*, only the commands that follow it.

# Navigating files and directories

The **file system** is the part of the operating system that manages files and directories. You can imagine it like a tree which organizes files and folders.

There are several commands frequently used to create, inspect, rename, and delete files and directories. Let's go back to our open shell window.

By default the shell drops you somewhere in the file system to begin. To find out where, we can use a command called `pwd`, meaning 'print working directory'. Directories are **places** in the file system, and wherever you are at any particular time is called the **current working directory**. So when we open a shell, where are we? What is the current working directory?

```
$ pwd
/Users/nelle
```

Here, the response is `/Users/nelle`, which is the **path**, or location, of Nelle's **home directory**. Your path will be different depending on the operating system you're using and your user name, but your shell will typically start off in your home directory. Let's continue with the example of Nelle's computer for the moment.

On Nelle's computer, the file system looks like this;

<center>
![An example of a simple file system with the root directory and sub-directories](images/filesystem.svg)
</center>

At the top is the **root directory** which holds everything else. It is referred to with a slash character, `/`, on its own. This is the leading slash in the path to Nelle's home directory `/Users/nelle`.

Inside `/` are several other directories:

- `bin`, a place for built-in programs
- `data` for miscellaneous data files
- `Users` where users' personal directories are located, and
- `tmp` for temporary files.

The path `/Users/nelle` tells us that Nelle's home directory is stored inside `/Users` and that `/Users` is stored inside the root directory `/`. From now on, you'll be exploring your own file system, which will have a similar structure, but will not be identical.

## The list command

Let's next learn the command which shows us the contents of the file system. The listing command `ls` does this:

```
$ ls
Desktop		Downloads	Mounts		Music		Projects
Documents	Library		Movies		Pictures	Public
```

Your results might be a little different depending on your operating system and what's been put into your home directory since you started using your computer.

`ls` prints the names of the files and directories in the current working directory. We can get a little more information by using the `-F` **option** (also known as a **switch** or **flag**), which tells `ls` to classify the output by adding a marker to file and directory names indicating what they are:

- a trailing `/` means directory
- `@` means a link
- `*` means an executable

Sometimes the default options will color the output to indicate this.

```
$ ls -F
Desktop/	Downloads/	Mounts/		Music/		Projects/
Documents/	Library/	Movies/		Pictures/	Public/
```

Here the home directory only contains **sub-directories**. Any names in your output without a classifying symbol are plain **files**.

## Syntax of a command

Let's break down the simple command we just used to learn some more terminology:

```
$ ls -F /
```

- `ls` is the **command**,
- `-F` is an **option**, **switch**, or **flag**, and
- `/` is an **argument**.

An option or flag can start either with a single dash (`-`) or two dashes (`--`) and they change the behavior of a command. Arguments tell the command what to operate on (e.g. files and directories). Sometimes options and arguments are called **parameters**. Commands can be called with more than one option and more than one argument, but a command doesn't always require them.

Each part is separated by spaces: omitting thje space between `ls` and `-F` is telling the shell to look for a command `ls-F`, which doesn't exist. Capitalization can also be important. For example, `ls -s` displays the size of files and directories alongside their names:

```
$ ls -s
0 Desktop	0 Downloads	0 Mounts	0 Music		0 Projects
0 Documents	0 Library	0 Movies	0 Pictures	0 Public
```

While `ls -S` sorts the files and directories by their sizes:

```
$ ls -S
Library		Projects	Desktop		Music		Documents
Downloads	Pictures	Mounts		Public		Movies
```

## Getting help

`ls` has many other **options**. Two common ways to find out how to use a command and what options it accepts are:

1. Pass the `--help` option to the command, such as:

```
$ ls --help
```

2. Read the manual with `man`, such as:

```
$ man ls
```

A **note**, depending on your environment it may be that only one of these works. For example, `man ls` works on macOS but `ls --help` works for gitbash.

### The `--help` option

Many bash commands, and programs written by others, support the `--help` option to show more information on how to use the command or program.

```
$ ls --help
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'always' (default
                               if omitted), 'auto', or 'never'; more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
      --file-type            likewise, except do not append '*'
      --format=WORD          across -x, commas -m, horizontal -x, long -l,
                               single-column -1, verbose -l, vertical -C
      --full-time            like -l --time-style=full-iso
  -g                         like -l, but do not list owner
      --group-directories-first
                             group directories before files;
                               can be augmented with a --sort option, but any
                               use of --sort=none (-U) disables grouping
  -G, --no-group             in a long listing, don't print group names
  -h, --human-readable       with -l and/or -s, print human readable sizes
                               (e.g., 1K 234M 2G)
      --si                   likewise, but use powers of 1000 not 1024
  -H, --dereference-command-line
                             follow symbolic links listed on the command line
      --dereference-command-line-symlink-to-dir
                             follow each command line symbolic link
                               that points to a directory
      --hide=PATTERN         do not list implied entries matching shell PATTERN
                               (overridden by -a or -A)
      --indicator-style=WORD  append indicator with style WORD to entry names:
                               none (default), slash (-p),
                               file-type (--file-type), classify (-F)
  -i, --inode                print the index number of each file
  -I, --ignore=PATTERN       do not list implied entries matching shell PATTERN
  -k, --kibibytes            default to 1024-byte blocks for disk usage
  -l                         use a long listing format
  -L, --dereference          when showing file information for a symbolic
                               link, show information for the file the link
                               references rather than for the link itself
  -m                         fill width with a comma separated list of entries
  -n, --numeric-uid-gid      like -l, but list numeric user and group IDs
  -N, --literal              print raw entry names (don't treat e.g. control
                               characters specially)
  -o                         like -l, but do not list group information
  -p, --indicator-style=slash
                             append / indicator to directories
  -q, --hide-control-chars   print ? instead of nongraphic characters
      --show-control-chars   show nongraphic characters as-is (the default,
                               unless program is 'ls' and output is a terminal)
  -Q, --quote-name           enclose entry names in double quotes
      --quoting-style=WORD   use quoting style WORD for entry names:
                               literal, locale, shell, shell-always,
                               shell-escape, shell-escape-always, c, escape
  -r, --reverse              reverse order while sorting
  -R, --recursive            list subdirectories recursively
  -s, --size                 print the allocated size of each file, in blocks
  -S                         sort by file size, largest first
      --sort=WORD            sort by WORD instead of name: none (-U), size (-S),
                               time (-t), version (-v), extension (-X)
      --time=WORD            with -l, show time as WORD instead of default
                               modification time: atime or access or use (-u);
                               ctime or status (-c); also use specified time
                               as sort key if --sort=time (newest first)
      --time-style=STYLE     with -l, show times using style STYLE:
                               full-iso, long-iso, iso, locale, or +FORMAT;
                               FORMAT is interpreted like in 'date'; if FORMAT
                               is FORMAT1<newline>FORMAT2, then FORMAT1 applies
                               to non-recent files and FORMAT2 to recent files;
                               if STYLE is prefixed with 'posix-', STYLE
                               takes effect only outside the POSIX locale
  -t                         sort by modification time, newest first
  -T, --tabsize=COLS         assume tab stops at each COLS instead of 8
  -u                         with -lt: sort by, and show, access time;
                               with -l: show access time and sort by name;
                               otherwise: sort by access time, newest first
  -U                         do not sort; list entries in directory order
  -v                         natural sort of (version) numbers within text
  -w, --width=COLS           set output width to COLS.  0 means no limit
  -x                         list entries by lines instead of by columns
  -X                         sort alphabetically by entry extension
  -Z, --context              print any security context of each file
  -1                         list one file per line.  Avoid '\n' with -q or -b
      --help     display this help and exit
      --version  output version information and exit

The SIZE argument is an integer and optional unit (example: 10K is 10*1024).
Units are K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,... (powers of 1000).

Using color to distinguish file types is disabled both by default and
with --color=never.  With --color=auto, ls emits color codes only when
standard output is connected to a terminal.  The LS_COLORS environment
variable can change the settings.  Use the dircolors command to set it.

Exit status:
 0  if OK,
 1  if minor problems (e.g., cannot access subdirectory),
 2  if serious trouble (e.g., cannot access command-line argument).

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
Full documentation at: <http://www.gnu.org/software/coreutils/ls>
or available locally via: info '(coreutils) ls invocation'
```

### The `man` command

The other way to learn about `ls` is to type:

```
$ man ls
```

This turns the terminal into a page with a description of the `ls` command and its options.

To navigate a `man` page, use the up/down arrow keys to move line-by-line or the PageUp/PageDown keys to skip up and down by a full page. To quit the `man` page, press `Q`.

Manual pages can also be found on the internet, for example [https://www.explainshell.com](https://explainshell.com).

### Mishaps are okay!

We all hit the wrong key now and then. The shell will often alert you to this fact, sometimes with helpful messages.

```
$ ks -F
command not found: ks
```

Here we might have had `ls` in mind, but the shell simply tells us that `ks` is not a valid command. If we try to use a flag that doesn't exist we get something a bit more helpful:

```
$ ls -j
ls: illegal option -- j
usage: ls [-@ABCFGHLOPRSTUWabcdefghiklmnopqrstuwx1%] [file ...]
```

## Exploring other directories

`ls` can be used to look at more than just the current working directory. Let's take a look at what's in the `Desktop` folder with `ls -F Desktop`. That is, the **command** `ls` with **option** `-F` and **argument** `Desktop`. In particular, the argument `Desktop` indicates we want the listing of something other than the current working directory:

```
$ ls -F Desktop
data-shell/
```

Your output should be the list of all files and sub-directories in the Desktop directory, including the `data-shell` directory you downloaded as part of the [setup]() for these lessons (TODO: Update link). On many systems, the command line `Desktop` is the same as your GUI Desktop. Have a look to confirm this.

Knowing that `data-shell` is in our Desktop directory, we can do a couple things.

First, we can look at its contents:

```
$ ls -F Desktop/data-shell
creatures/		molecules/		notes.txt		solar.pdf
data/			north-pacific-gyre/	pizza.cfg		writing/
```

Second, we can change our location to a different directory, moving out of our home directory.

The command to do this is `cd`, or 'change directory.' You can think of `cd` as the same as double clicking a folder in a GUI to move into a folder. If we want to move into the `data` directory we saw in the previous call to `ls -F Desktop` we can use the following commands to get there:

```
$ cd Desktop
$ cd data-shell
$ cd data
```

This moves us into `Desktop`, then into `data-shell`, then in `data`. Notice each time that `cd` didn't print anything to the shell; this is normal, and many shell commands don't output anything when successfully executed.

Now if we run `pwd`, we'll see the change in directory:

```
$ pwd
/Users/rcavalca/Desktop/data-shell/data
```

And we can use `ls -F` without giving the directory because we are already there:

```
$ ls -F
amino-acids.txt		elements/		planets.txt
animal-counts/		morse.txt		salmon.txt
animals.txt		pdb/			sunspot.txt
```

We have gone down the directory tree by moving through sub-directories, but how do we move back up? One might think that because `data-shell` is above `data` we could:

```
$ cd data-shell
cd: no such file or directory: data-shell
```

But that doesn't seem to work. It turns out there is a shortcut in the shell to move up one directory level; that looks like:

```
$ cd ..
```

`..` is a special directory meaning "the directory containing this one" or the **parent** of the current directory. If we run `pwd` we see that we have gone up a directory:

```
$ pwd
/Users/rcavalca/Desktop/data-shell
```

You may have noticed the special directory `..` didn't show up when we ran `ls` previously. If we want to display it, we have to use the `-a` (show all) flag to `ls -F`:

```
$ ls -F -a
./			creatures/		north-pacific-gyre/	solar.pdf
../			data/			notes.txt		writing/
.bash_profile		molecules/		pizza.cfg
```

We see `..`, which refers to the parent of this directory, i.e. `/Users/rcavalca/Desktop`, but we also wee `.`. That refers to the current working directory.

Note that we called `ls -F -a` to use two options, but in many command line tools, multiple options can be combined with a single `-` and no spaces, so `ls -Fa` is equivalent.

The basic commands for navigating the file system in a shell on your computer are `pwd`, `ls`, and `cd`. Let's quickly explore some variations on these commands to explore some further shell concepts. What happens if we call `cd` without giving a directory?

```
$ cd
```

We know that `pwd` will tell us where we are, so calling it tells us:

```
$ pwd
/Users/rcavalca
```

So `cd` with no arguments takes us to our home directory. Let's return to the `data` directory from before. Last time, we used three commands, but we can do it in one step:

```
$ cd Desktop/data-shell/data
```

You can check that we're in the right place by doing `pwd` and `ls -F`. We discussed the use of `..` to move up directories, but there is another way to move around the file system.

When we've been giving directory names, or even a path (like `Desktop/data-shell/data`), these have been **relative paths**. That is to say, they were relative to the current working directory.

We could use the **absolute path** to the directory by including the entire path from the root directory. An absolute path always refers to exactly one directory, no matter our working directoy when running the command.

So to go up to the `data-shell` directory we could:

```
$ cd /Users/rcavalca/Desktop/data-shell
```

Note, your path will depend on your user name and where you home directory is.

# Altering files and directories

## Creating directories

We've just learned how to explore files and directories, but how do we create them? A good place to start is to see where we are and what we have.

Going back to `data-shell`:

```
$ cd ~/Desktop/data-shell
$ ls -F
.			notes.txt		north-pacific-gyre	data
..			solar.pdf		creatures		writing
molecules		pizza.cfg		.bash_profile
```

Let's make a new directory called `thesis` using the command `mkdir thesis` (this is another command that doesn't print a result to the shell).

```
$ mkdir thesis
```

The `mkdir` command means 'make directory'. Since we gave a relative path, it was created in the working directory:

```
$ ls -F
creatures/		north-pacific-gyre/	solar.pdf
data/			notes.txt		thesis/
molecules/		pizza.cfg		writing/
```

Since we just made it, there's nothing in it:

```
$ ls -F thesis
```

The `mkdir` command is not limited to creating single directories at a time. We can use the `-p` option to create a directory with any number of nested sub-directories, as in:

```
$ mkdir -p thesis/chapter_1/section_1
```

The `-R` option to `ls` lists all nested subdirectories within a directory:

```
$ ls -FR thesis
chapter_1/

thesis/chapter_1:
section_1/

thesis/chapter_1/section_1:
```

> **A note on names**
>
> Complicated names can cause difficulty later on by making it difficult to remember and difficult to type the names of directories or files. Some tips for naming files and directories:
>
> 1. Don't use spaces. Instead use `-` or `_` to separate words, e.g. `north-pacific-gyre/`.
>
> 2. Don't begin names with `-` because commands treat such names as options.
>
> 3. Stick with letters, numbers, `.`s, `-`s, and `_`s. Many characters have special meanings, so it's best to avoid them.
>
> If you inherit files or directories with spaces or other special characters, surround the name in quotes (`""`).

## Moving files and directories

Let's say that we want to move the file `notes.txt` into the `thesis` folder we just created. We can do that with the move command `mv`:

```
$ mv notes.txt thesis/notes.txt
```

The first argument tells `mv` what we're moving (the source) and the second is where it should go (the target). If we check with `ls`, we'll see that the file has been moved:

```
$ ls thesis
chapter_1	notes.txt
```

We should be careful when specifying the target file name because `mv` silently overwrites any existing file with the same name, which could cause data loss. The `-i` option (`mv -i` or `mv --interactive`), will ask for confirmation before moving.

Note, `mv` also works on directories. Say we want to change the name of the directory `creatures` to `mythical_creatures`. We could do that with:

```
$ mv creatures mythical_creatures
```

After moving `notes.txt` to `thesis/notes.txt`, we decide we'd rather have it back in `data-shell`. Here we need to use the shortcut that came up earlier `.` that represents the current working directory:

```
$ mv thesis/notes.txt .
$ ls -F
data/			north-pacific-gyre/	solar.pdf
molecules/		notes.txt		thesis/
mythical_creatures/	pizza.cfg		writing/
```

## Copying files and directories

The `mv` command allowed us to move things around, but there is a copy command `cp`, which allows us to duplicate items. Let's copy `notes.txt` into the `thesis/` directory.

```
$ cp notes.txt thesis/notes.txt
$ ls thesis
chapter_1	notes.txt
```

It is also possible to copy a directory and all its contents using the recursive option `-r`. This can help us create a back up of a directory:

```
$ cp -r thesis thesis_backup
$ ls thesis thesis_backup
thesis:
chapter_1	notes.txt

thesis_backup:
chapter_1	notes.txt
```

## Removing files and directories

Going back to the `data-shell` directory, let's remove the `solar.pdf` file. The command for this is `rm`, short for 'remove':

```
$ rm solar.pdf
```

We can confirm the file is gone with `ls`:

```
$ ls solar.pdf
ls: solar.pdf: No such file or directory
```

> **Deletion is forever**
> Unlike Windows or macOS, the Unix shell doesn't have a trash bin where deleted files can be recovered (though most Unix GUIs do). Instead, when files are deleted, they are unlinked from the file system so the space can be recycled.

Since deletion is forever, let's learn how to use `rm` safely with the `-i` flag:

```
$ rm -i notes.txt
remove notes.txt? n
$ ls
data			north-pacific-gyre	thesis
molecules		notes.txt		thesis_backup
mythical_creatures	pizza.cfg		writing
```

So we see that after deciding not to remove `notes.txt`, it's still there.

Finally, what if we want to remove a directory? If we wanted to remove the `thesis` directory, we might try:

```
$ rm thesis
rm: thesis: is a directory
```

This happens because `rm` works only works on files by default, not directories. This is sort of a safety feature. We can use the recursive option `-r` to remove a directory and all its contents.

```
$ rm -r thesis
```

Given that we can't retrieve those files using the shell (except we did cleverly back up that folder), `rm -r` should be used with **great caution**. If the directory doesn't contain too many files, consider `rm -r -i`.

# Summary

In this lesson we've learned:

- What a shell is.
- Why it is useful.
- How to navigate the file system.
- How to manipulate files and folders.

We have only scratched the surface of what is possible with the commands available in the Unix shell.