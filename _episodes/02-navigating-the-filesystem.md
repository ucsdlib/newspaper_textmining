---
title: "Navigating the filesystem"
teaching: 20
exercises: 10
questions:
- "How do you move around the filesystem in the shell?"
objectives:
- "Use shell commands to work with directories and files"
- "Use shell commands to find and manipulate data"
keypoints:
- "Knowing where you are in your directory structure is key to working with the shell"
---

## Getting started 

*(Note that this lesson needs to be modified slightly to reflect the virtual machine and file directory used during the lesson.)*

To simplify set up, we will be using the shell on a **virtual machine**. A virtual machine is just like your computer, but you access it remotely with your computer. Virtual machines can have many benefits, such as providing larger amounts of computing power than you have on your personal computer. For our workshop, it also ensures we are all working in the same environment with access to the same files. 

Our virtual machine is hosted on the Jupyterhub, a pilot project at UC San Diego. To access it, navigate to this link: https://jupyterhub.nautilus.optiputer.net/. Select UC San Diego, and login with your Active Directory credentials. When you come to the page that says "Spawner Options," select "student newspapers" and click continue. 

You should now be connected to a virtual machine. It may take a few minutes. 

## Navigating the shell

We will now begin with the basics of navigating the Unix shell.

Let’s start by opening the shell. We will use the shell through a program called the "terminal." While you can use a terminal on your personal computer, we will be using a terminal located on our virtual machine. 

To access a terminal from the Juptyerhub, click on "File" (in the upper left navigation bar), and select "New" and "terminal." 

This likely results in seeing a black window with a cursor flashing next to a dollar sign.
This is our command line, and the `$` is the command **prompt** to show that the system is ready for our input.
The appearance of the prompt will vary from system to system, depending on how the set up has been configured,
but it usually ends with a `$`.

The first thing we will do is input a command that will give you access to the student newspaper dataset. Type `ln -s /nfs` and hit enter. You should see a folder called "nfs" appear in the left panel. The left panel is a graphical representation of our file system. The shell, however, will allow us to view, manage, and manipulate our file system through text based commands.

When working in the shell, you are always *somewhere* in the computer's
file system, in some folder (directory). We will therefore start by finding out
where we are by using the `pwd` command, which you can use whenever you are unsure
about where you are. It stands for "print working directory" and the result of the
command is printed to your standard output, which is the screen.

Let's type `pwd` and hit enter to execute the command:
(The `$` sign is used to indicate a command to be typed on the command prompt,
 but we never type the `$` sign itself, just what follows after it.)

~~~
$ pwd
~~~
{: .bash}
~~~
/home/jovyan
~~~
{: .output}

The output will be a path to the home directory of your virtual machine. 

Let's check if we recognise it
by listing the contents of the directory. To do that, we use the `ls` command:

~~~
$ ls
~~~
{: .bash}
~~~
nfs 
~~~
{: .output}

The output shows you what files are in your home directory. There is not a lot here, so lets go inside the **nfs** directory to see what is there. 

To change directories, we use the `cd` or Change Directory command:

~~~
$ cd nfs
~~~
{: .bash}

Notice that the command didn't output anything. This means that it was carried
out successfully. Let's check by using `pwd`:

~~~
$ pwd
~~~
{: .bash}
~~~
/home/jovyan/nfs
~~~
{: .output}

Let's see what is inside the nfs directory by using the `ls` command:

~~~
$ ls
~~~
{: .bash}
~~~
GuardianNewspapersByDate TestFolder Masha
~~~
{: .output}

We can see that are three directories in the current directory nfs. 

To reverse the sort, use the command `ls -r`.

~~~
$ ls -r
~~~
{: .bash}
~~~
Masha TestFolder GuardianNewspapersByDate 
~~~
{: .output}

> ## Errors 

What if we  tried to change to a directory that didn't exist? 
If something had gone wrong, the command would have told you. Let's
test that by trying to move into a non-existentdirectory:

~~~
$ cd "Evil plan to destroy the world"
~~~
{: .bash}
~~~
bash: cd: Evil plan to destroy the world: No such file or directory
~~~
{: .output}

Notice that we surrounded the name by quotation marks. The *arguments* given
to any shell command are separated by spaces, so a way to let them know that
we mean 'one single thing called "Evil plan to destroy the world"', not
'six different things', is to use (single or double) quotation marks.

We've now seen how we can go 'down' through our directory structure
(as in into more nested directories). If we want to go back, we can type `cd ..`.
This moves us 'up' one directory, putting us back where we started.
**If we ever get completely lost, the command `cd` without any arguments will bring
us right back to the home directory, the place where we started.**

> ## Previous Directory
> To switch back and forth between two directories use `cd -`.
{: .callout}

> ## Getting more information about files and directories.

We may want more information than just a list of files and directories.
We can get this by specifying various **flags** (also known as `options`, `parameters`, or, most frequently,
`arguments`) to go with our basic commands.
Arguments modify the workings of the command by telling the computer what sort of output or manipulation we want.

Let's try this out inside our nfs directory. Navigate to the nfs directory using the `cd` command.

If we type `ls -l` and hit enter, the computer returns a list that contains information about folderes and directories in the working directory.

~~~
$ ls -l
~~~
{: .bash}
~~~
total 140
drwxr-xr-x 3 jovyan users 98304 Sep 20 16:55 GuardianNewspapersByDate
drwxr-xr-x 2 jovyan users     6 Sep 20 22:53 Masha
drwxr-xr-x 2 jovyan users     6 Sep 21 18:27 TestFolder
~~~
{: .output}


Each list item contains the name of the file or directory on the far right, e.g. "GuardianNewspaperByDate." Moving left from the name, you then see the date and time of when the item was created or last modified, e.g. "Sep 20 16:55." The next item gives information about the size of the file or directory (in bytes), e.g. "98304." The next item gives information about the owner and group of the file or directory, e.g. "joyvan users." And the item on the left, gives information about permissions for reading, writing, and executing those files. 

We will not go into permissions yet at this stage. If you want to read more about them, see: http://www.yourownlinux.com/2014/01/linux-ls-command-tutorial-with-examples.html. 

In everyday usage we are more used to units of measurement like kilobytes, megabytes, and gigabytes.
Luckily, there's another flag `-h` that when used with the -l option, use unit suffixes:
Byte, Kilobyte, Megabyte, Gigabyte, Terabyte and Petabyte in order to reduce the
number of digits to three or less using base 2 for sizes.

Now `ls -h` won't work on its own. When we want to combine two flags,
we can just run them together. So, by typing `ls -lh` and hitting
enter we receive an output in a human-readable format (note: the order here doesn't matter).

~~~
$ ls -lh
~~~
{: .bash}
~~~
total 140K
drwxr-xr-x 3 jovyan users 96K Sep 20 16:55 GuardianNewspapersByDate
drwxr-xr-x 2 jovyan users   6 Sep 20 22:53 Masha
drwxr-xr-x 2 jovyan users   6 Sep 21 18:27 TestFolder
~~~
{: .output}

Now move into the GuardianNewspaperByDate directory using the `cd` command. 

Try using the `ls -lh` command in the GuardianNewspaperByDate directory. What do you see?

To reverse the sort, use the command `ls -lhr`. 

To sort by file size, use the `ls -lhS` command. Now what do you see? 

What command would you use to reverse the sort for file size? 


> ## Try exploring
>
> Move around the computer, get used to moving in and out of directories,
> see how different file types appear in the Unix shell. Be sure to use the `pwd` and
> `cd` commands, and the different flags for the `ls` command you learned so far.

----

>
{: .challenge}


Being able to navigate the file system is very important for using the Unix shell effectively.
As we become more comfortable, we can get very quickly to the directory that we want.


