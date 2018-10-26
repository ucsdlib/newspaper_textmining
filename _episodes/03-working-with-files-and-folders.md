---
title: "Working with files and directories"
teaching: 20
exercises: 10
questions:
- "What is the shell?"
- "What is the command line?"
- "Why should I use it?"
objectives:
- "Create files and directories from the command line"
- "Use tab completion to limit typing"
- "Look at all or part of a file using the command line"
- "Moving and copying files"
keypoints:
- "The shell can be used to copy, move, and combine multiple files"
---
## Working with files and folders

As well as navigating directories, we can interact with files on the command line:
we can read them, open them, run them, and even edit them. In fact, there's really
no limit to what we *can* do in the shell, but even experienced shell users still switch to
graphical user interfaces (GUIs) for many tasks, such as editing formatted text
documents (Word or OpenOffice), browsing the web, editing images, etc. But if we
wanted to make the same crop on hundreds of images, say, the pages of a scanned book,
then we could automate that cropping work by using shell commands.

We will try a few basic ways to interact with files. Let's first move into the
`TestFolder` directory on your desktop.

~~~
$ cd
$ cd cephfs/TestFolder/
$ pwd
~~~
{: .bash}
~~~
/home/jovyan/cephfs/TestFolder
~~~
{: .output}

Here, we will create a new directory and move into it:

~~~
$ mkdir firstdir
$ cd firstdir
~~~
{: .bash}

Here we used the `mkdir` command (meaning 'make directories') to create a directory
named 'firstdir'. Then we moved into that directory using the `cd` command.

But wait! There's a trick to make things a bit quicker. Let's go up one directory.

~~~
$ cd ..
~~~
{: .bash}

Instead of typing `cd firstdir`, let's try to type `cd f` and then hit the Tab key.
We notice that the shell completes the line to `cd firstdir/`.

> ## Tab for Auto-complete
> Hitting tab at any time within the shell will prompt it to attempt to auto-complete
> the line based on the files or sub-directories in the current directory.
> Where two or more files have the same characters, the auto-complete will only fill up to the
> first point of difference, after which we can add more characters, and
> try using tab again. We would encourage using this method throughout
> today to see how it behaves (as it saves loads of time and effort!).
{: .callout}

### Reading files

If you are in `firstdir`, use `cd ..` to get back to the `shell-lesson` directory.

Here we have twelve issues of The Guardian from 1967 and 2014. We will use this smaller directory to practice, before diving into the larger directory that contains 2,915 issues. Let's take a lok. 

~~~
$ ls -lh
~~~
{: .bash}
~~~
total 1020K
-rw-r--r-- 1 jovyan users  55K Sep 21 21:42 1967-05-26.txt
-rw-r--r-- 1 jovyan users  60K Sep 21 21:42 1967-09-24.txt
-rw-r--r-- 1 jovyan users  90K Sep 21 21:42 1967-10-13.txt
-rw-r--r-- 1 jovyan users 172K Sep 21 21:42 1967-10-27.txt
-rw-r--r-- 1 jovyan users  68K Sep 21 21:42 1967-11-10.txt
-rw-r--r-- 1 jovyan users  89K Sep 21 21:42 1967-11-22.txt
-rw-r--r-- 1 jovyan users  95K Sep 21 21:42 1967-12-06.txt
-rw-r--r-- 1 jovyan users  84K Sep 21 22:02 2014-11-24.txt
-rw-r--r-- 1 jovyan users  79K Sep 21 22:02 2014-12-01.txt
-rw-r--r-- 1 jovyan users  73K Sep 21 22:02 2014-12-04.txt
-rw-r--r-- 1 jovyan users  72K Sep 21 22:02 2014-12-08.txt
-rw-r--r-- 1 jovyan users  71K Sep 21 22:02 2014-12-11.txt
~~~
{: .output}

How `cat` command let's us read these files. 

~~~
$ cat 1967-05-26.txt
~~~
{: .bash}

The terminal window erupts and the entire issue cascades by (it is printed to
your terminal). Great, but we can't really make any sense of that amount of text.

> ## Canceling Commands
> To cancel this print of `1967-05-26.txt`, or indeed any ongoing processes in the Unix shell, hit `Ctrl+C`
{: .callout}

Often we just want a quick glimpse of the first or the last part of a file to
get an idea about what the file is about. To let us do that, the Unix shell
provides us with the commands `head` and `tail`.

~~~
$ head 1967-05-26.txt
~~~
{: .bash}
~~~
I
( ,

From left to rtcbt: New AS Pres. Rkbard AJleabotI aod AS Vice-Pres. lien Sweetwood are sbown
receivtog tbe pyel 01 authorlty from Jim HefUD IDI Rlebard lloaereHf at the IpsbJlaUOD of Ofticers.
Tbe ceremony was held at Torrey PIoes IDD OIl May 19.

e"." • ..,. '0 ".w L.ft
Climaxin& a year Of orpai-
~~~
{: .output}

Ooof, that is some messy text!!!! Let's look at a more recent issue.

~~~
$ head 2014-12-11.txt
~~~
{: .bash}
~~~
VOLUME 48, ISSUE 20  THURSDAY, DECEMBER 11, 2015 WWW.UCSDGUARDIAN.ORG

WINTER MOVIE
PREVIEWS

With so many films hitting
theaters over the holidays,
check out our winter movie
guide to find out what flops
~~~
{: .output}

Much cleaner! Anyone have thoughts about why there is a difference between the two files? The quality of the text presents some interesting challenges that we will consider that later. 

In the meantime, it is useful to know that `head` provides a view of the first ten lines, whereas `tail filename.txt` provides a perspective on the last ten lines:

~~~
$ tail 2014-12-11.txt
~~~
{: .bash}
~~~
VS Cal Poly Pomona
VS Cal Poly Pomona
AT San Diego State

UPCOMING

UCSD
GAMES
~~~
{: .output}

If ten lines is not enough (or too much), we can specify the number of lines to get using the `-n` arguement. 
For example, `head -n 20` will print 20 lines.

Another way to navigate files is to view the contents by scrolling through them.
Type `more 2014-12-11.txt` to see the first screen, `enter` to scroll down, and `q` to quit (return to the command prompt).

~~~
$ more 2014-12-11.txt
~~~
{: .bash}

Is this your new favorite way of reading *The Guardian*?

Like many other shell commands, the commands `cat`, `head`, `tail` and `more`
can take any number of arguments (they can work with any number of files).
We will see how we can get the first lines of several files at once.
To save some typing, we introduce a very useful trick first.

> ## Re-using commands
> On a blank command prompt, hit the up arrow key and notice that the previous
> command you typed appears before your cursor. We can continue pressing the
> up arrow to cycle through your previous commands. The down arrow cycles back
> toward your most recent command. This is another important labour-saving
> function and something we'll use a lot.
{: .callout}

Hit the up arrow until you get to the `head 2014-12-11.txt` command. Add a space
and then `1967-05-26.txt` (Remember your friend Tab? Type `1` followed by Tab to
get `33504-0.txt`), to produce the following command:

~~~
$ head 2014-12-11.txt 1967-05-26.txt
~~~
{: .bash}
~~~
==> 2014-12-11.txt <==

VOLUME 48, ISSUE 20  THURSDAY, DECEMBER 11, 2015 WWW.UCSDGUARDIAN.ORG

WINTER MOVIE
PREVIEWS

With so many films hitting
theaters over the holidays,
check out our winter movie
guide to find out what flops

==> 1967-05-26.txt <==

I
( ,

From left to rtcbt: New AS Pres. Rkbard AJleabotI aod AS Vice-Pres. lien Sweetwood are sbown
receivtog tbe pyel 01 authorlty from Jim HefUD IDI Rlebard lloaereHf at the IpsbJlaUOD of Ofticers.
Tbe ceremony was held at Torrey PIoes IDD OIl May 19.

e"." • ..,. '0 ".w L.ft
Climaxin& a year Of orpai-
~~~
{: .output}

All good so far, but what if we want to read lots of issues? It would be tedious to enter
all the filenames. Luckily the shell supports wildcards! The `?` (matches exactly
one character) and `*` (matches zero or more characters) are probably familiar
from library search systems. We can use the `*` wildcard to write the above `head`
command in a more compact way:

~~~
$ head *.txt
~~~
{: .bash}

> ## More on wildcards
> Wildcards are a feature of the shell and will therefore work with *any* command.
> The shell will expand wildcards to a list of files and/or directories before
> the command is executed, and the command will never see the wildcards.
> As an exception, if a wildcard expression does not match any file, Bash
> will pass the expression as a parameter to the command as it is. For example
> typing `ls *.pdf` results in an error message that there is no file called *.pdf.
{: .callout}


<!-- Timing: Spent 75 minutes to get here -->

### Moving, copying and deleting files

We may also want to change the file name to something more descriptive.
We can **move** it to a new name by using the `mv` or move command,
giving it the old name as the first argument and the new name as the second
argument:

~~~
$ mv 1967-05-26.txt FirstNewspaper.txt
~~~
{: .bash}

This is equivalent to the 'rename file' function.

Afterwards, when we perform a `ls` command, we will see that it is now called `gulliver.txt`:

~~~
$ ls
~~~
{: .bash}
~~~
FirstNewspaper.txt  1967-10-13.txt  1967-11-10.txt  1967-12-06.txt  2014-12-01.txt  2014-12-08.txt
1967-09-24.txt  1967-10-27.txt  1967-11-22.txt  2014-11-24.txt  2014-12-04.txt  2014-12-11.txt
~~~
{: .output}

> ## Copying a file
>
> Instead of *moving* a file, you might want to *copy* a file (make a duplicate),
> for instance to make a backup before modifying a file.
> Just like the `mv` command, the `cp` command takes two arguments: the old name
> and the new name. How would you make a copy of the file `FirstNewspaper.txt` called
> `FirstNewspaper-backup.txt`? Try it!
>
> > ## Answer
> > ~~~
> > cp FirstNewspaper.txt FirstNewspaper-backup.txt
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Renaming a directory
>
> Renaming a directory works in the same way as renaming a file. Try using the
> `mv` command to rename the `firstdir` directory to `backup`.
>
> > ## Answer
> > ~~~
> > mv firstdir backup
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Moving a file into a directory
>
> If the last argument you give to the `mv` command is a directory, not a file,
> the file given in the first argument will be moved to that directory. Try
> using the `mv` command to move the file `FirstNewspaper-backup.txt` into the
> `backup` folder.
>
> > ## Answer
> > ~~~
> > mv FirstNewspaper-backup.txt backup
> > ~~~
> > {: .bash}
> >
> > This would also work:
> >
> > ~~~
> > mv FirstNewspaper-backup.txt backup/FirstNewspaper-backup.txt
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## The wildcards and regular expressions
>
> The `?` wildcard matches one character. The `*` wildcard matches zero or
> more characters. 

> ## Using `history`
> Use the `history` command to see a list of all the commands
> you've entered during the current session. You can also use `Ctrl + r` to do
> a reverse lookup. Hit `Ctrl + r`, then start typing any part of the command you're
> looking for. The past command will autocomplete. Hit `enter` to run the command again,
> or press the arrow keys to start editing the command. If you can't find what you're
> looking for in the reverse lookup, use `Ctrl + c` to return to the prompt. If you want to save your history, maybe to
> extract some commands from which to build a script later on, you can do that with `history > history.txt`.
> This will output all history to
> a text file called `history.txt` that you can later edit. To recall a command from history, enter `history`. Note the command number, e.g. 2045. Recall the command by entering `!2045`. This will execute the command.
{: .challenge}

> ## Using the `echo` command
> The `echo` command simply prints out a text you specify. Try it out: `echo "The shell is awesome"`.
> Interesting, isn't it?
>
> You can also specify a variable, for instance `NAME=` followed by your name.
> Then type `echo "$NAME is a shell pro"`. What happens?
>
> You can combine both text and normal shell commands using `echo`, for example the
> `pwd` command you have learned earlier today. You do this by enclosing a shell
> command in `$(` and `)`, for instance `$(pwd)`. Now, try out the following:
> `echo "Finally, it is nice and sunny on" $(date)`.
> Note that the output of the `date` command is printed together with the text
> you specified. You can try the same with some of the other commands you have learned so far.
>
> **Why do you think the echo command is actually quite important in the shell environment?**
>
> > ## Answer
> > You may think there is not much value in such a basic command like `echo`. However, from the moment you
> > start writing automated shell scripts, it becomes very useful. For instance, you often need
> > to output text to the screen, such as the current status of a script.
> >
> > Moreover, you just used a shell variable for the first time, which can be used to temporarily store information,
> > that you can reuse later on. It will give many opportunities from the moment you start writing automated scripts.
> {: .solution}
{: .challenge}

Finally, onto deleting. We won't use it now, but if you do want to delete a file,
for whatever reason, the command is `rm`, or remove.

Using wildcards, we can even delete lots of files. And adding the `-r` flag we
can delete folders with all their content.

**Unlike deleting from within our graphical user interface, there is *no* warning,
*no* recycling bin from which you can get the files back and no other undo options!**
For that reason, please be very careful with `rm` and extremely careful with `rm -r`.
