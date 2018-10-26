---
title: "Counting and mining with the shell"
teaching: 60
exercises: 30
questions:
- "How can I count data?"
- "How can I find data within files?"
- "How can I combine existing commands to do new things?"
objectives:
- "Demonstrate counting lines, words, and characters with the shell command wc and appropriate flags"
- "Use strings to mine files and extract matched lines with the shell"
- "Create complex single line commands by combining shell commands and regular expressions to mine files"
- "Redirect a command's output to a file."
- "Process a file instead of keyboard input using redirection."
- "Construct command pipelines with two or more stages."
- "Explain Unix's 'small pieces, loosely joined' philosophy."
keypoints:
- "The shell can be used to count elements of documents"
- "The shell can be used to search for patterns within files"
- "Commands can be used to count and mine any number of files"
- "Commands and flags can be combined to build complex queries specific to your work"

---
##  Counting and mining data

Now that you know how to navigate the shell, we will move onto
learning how to count and mine data using a few of the standard shell commands.
While these commands are unlikely to revolutionise your work by themselves,
they're very versatile and will add to your foundation for working in the shell
and for learning to program with python later on.  

## Counting and sorting

*(this could be an opportunity to talk about why counting and sorting can be useful for text analysis?)* 

We will begin by counting the contents of files using the Unix shell.
We can use the Unix shell to quickly generate counts from across files,
something that is tricky to achieve using the graphical user interfaces of standard office suites.

Let's start by navigating to the directory that contains a small sample of our data using the
`cd` command:

~~~
$ cd YourNameTestFolder
~~~
{: .bash}

Remember, if at any time you are not sure where you are in your directory structure,
use the `pwd` command to find out:

~~~
$ pwd
~~~
{: .bash}
~~~
../cephfs/YourNameTestFolder$
~~~
{: .output}

And let's just check what files are in the directory and how large they
are with `ls -lh`:

~~~
$ ls -lh
~~~
{: .bash}
~~~
total 1.0M
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

<!--(NOT SURE WHERE TO PUT TSV STUFF) In this episode we'll focus on the dataset `2014-01_JA.tsv`, that contains
journal article metadata, and the three `.tsv` files derived from the original
dataset. Each of these three .tsv files includes all data where a keyword such
as `africa` or `america` appears in the 'Title' field of `2014-01_JA.tsv`.

<!-- ## CSV and TSV Files
> CSV (Comma-separated values) is a common plain text format for storing tabular
> data, where each record occupies one line and the values are separated by commas.
> TSV (Tab-separated values) is just the same except that values are separated by
> tabs rather than commas. Confusingly, CSV is sometimes used to refer to both CSV,
> TSV and variations of them. The simplicity of the formats make them great for
> exchange and archival. They are not bound to a specific program (unlike Excel
> files, say, there is no `CSV` program, just lots and lots of programs that
> support the format, including Excel by the way.), and you wouldn't have any
> problems opening a 40 year old file today if you came across one.
{: .callout}
<!-- hm, reminds me of MARC -->

`wc` is the "word count" command: it counts the number of lines, words, bytes
and characters in files. Since we love the wildcard operator, let's run the command
`wc *.txt` to get counts for all the `.txt` files in the current directory
(it takes a little time to complete):

~~~~
$ wc *.txt
~~~~
{: .bash}
~~~
   2186    9129   55310 1967-05-26_.txt
   2476    9915   60589 1967-09-24_.txt
   3293   15283   91904 1967-10-13_.txt
   6264   29446  175313 1967-10-27_.txt
   2575   11909   69381 1967-11-10_.txt
   3459   14994   90410 1967-11-22_.txt
   3072   17048   97212 1967-12-06_.txt
   2881   14093   85859 2014-11-24_.txt
   2745   13343   80843 2014-12-01_.txt
   2720   12391   74060 2014-12-04_.txt
   2352   12028   72877 2014-12-08_.txt
   2656   12027   72189 2014-12-11_.txt
  36679  171606 1025947 total
~~~
{: .output}

The first three columns contains the number of lines, words, and bytes
(to show number characters you have to use a flag).

If we only have a handful of files to compare, it might be faster or more convenient
to just check with Microsoft Excel, OpenRefine or your favourite text editor, but
when we have tens, hundreds or thousands of documents, the Unix shell has a clear
speed advantage. The real power of the shell comes from being able to combine commands
and automate tasks, though. We will touch upon this slightly.

For now, we'll see how we can build a simple pipeline to find the shortest file
in terms of number of lines. We start by adding the `-l` flag to get only the
number of lines, not the number of words and bytes:

~~~~
$ wc -l *.txt
~~~~
{: .bash}
~~~
   2186 1967-05-26_.txt
   2476 1967-09-24_.txt
   3293 1967-10-13_.txt
   6264 1967-10-27_.txt
   2575 1967-11-10_.txt
   3459 1967-11-22_.txt
   3072 1967-12-06_.txt
   2881 2014-11-24_.txt
   2745 2014-12-01_.txt
   2720 2014-12-04_.txt
   2352 2014-12-08_.txt
   2656 2014-12-11_.txt
  36679 total
~~~
{: .output}

The `wc` command itself doesn't have a flag to sort the output, but as we'll
see, we can combine three different shell commands to get what we want.

First, we have the `wc -l *.txt` command. We will save the output from this
command in a new file. To do that, we *redirect* the output from the command
to a file using the ‘greater than’ sign (>), like so:

~~~
$ wc -l *.tsv > lengths.txt
~~~
{: .bash}

There's no output now since the output went into the file `lengths.txt`, but
we can check that the output indeed ended up in the file using `cat` or `less`
(or Notepad or any text editor).

~~~~
$ cat lengths.txt
~~~~
{: .bash}
~~~
   2186 1967-05-26_.txt
   2476 1967-09-24_.txt
   3293 1967-10-13_.txt
   6264 1967-10-27_.txt
   2575 1967-11-10_.txt
   3459 1967-11-22_.txt
   3072 1967-12-06_.txt
   2881 2014-11-24_.txt
   2745 2014-12-01_.txt
   2720 2014-12-04_.txt
   2352 2014-12-08_.txt
   2656 2014-12-11_.txt
   36691 total
~~~
{: .bash}

Next, there is the `sort` command. We'll use the `-n` flag to specify that we
want numerical sorting, not lexical sorting, we output the results into
yet another file, and we use `cat` to check the results:

~~~~
$ sort -n lengths.txt > sorted-lengths.txt
$ cat sorted-lengths.txt
~~~~
{: .bash}
~~~
   2186 1967-05-26_.txt
   2352 2014-12-08_.txt
   2476 1967-09-24_.txt
   2575 1967-11-10_.txt
   2656 2014-12-11_.txt
   2720 2014-12-04_.txt
   2745 2014-12-01_.txt
   2881 2014-11-24_.txt
   3072 1967-12-06_.txt
   3293 1967-10-13_.txt
   3459 1967-11-22_.txt
   6264 1967-10-27_.txt
  36679 total
~~~
{: .output}

Finally we have our old friend `head`, that we can use to get the first line
of the `sorted-lengths.txt`:

~~~~
$ head -n 1 sorted-lengths.txt
~~~~
{: .bash}
~~~
  2186 1967-05-26_.txt
~~~
{: .output}

But we're really just interested in the end result, not the intermediate
results now stored in `lengths.txt` and `sorted-lengths.txt`. What if we could
send the results from the first command (`wc -l *.txt`) directly to the next
command (`sort -n`) and then the output from that command to `head -n 1`? 
Luckily we can, using a concept called pipes. On the command line, you make a
pipe with the vertical bar character |. (Note, we are adding "`_`" to the command `*.txt`
to distinguish our newspaper text files from the new files `lenghts.txt` and `sorted-lengths.txt`
that we have just made.  

Let's try with one pipe first:

~~~~
$ wc -l *_.txt | sort -n
~~~~
{: .bash}
~~~
   2186 1967-05-26_.txt
   2352 2014-12-08_.txt
   2476 1967-09-24_.txt
   2575 1967-11-10_.txt
   2656 2014-12-11_.txt
   2720 2014-12-04_.txt
   2745 2014-12-01_.txt
   2881 2014-11-24_.txt
   3072 1967-12-06_.txt
   3293 1967-10-13_.txt
   3459 1967-11-22_.txt
   6264 1967-10-27_.txt
  36679 total
~~~
{: .output}

Notice that this is exactly the same output that ended up in our `sorted-lengths.txt`
earlier. Let's add another pipe:

~~~~
$ wc -l *.tsv | sort -n | head -n 1
~~~~
{: .bash}
~~~
     2186 1967-05-26_.txt
~~~
{: .output}

It can take some time to fully grasp pipes and use them efficiently, but it's a
very powerful concept that you will find not only in the shell, but also in most
programming languages.

![Redirects and Pipes](../fig/redirects-and-pipes.png)

> ## Pipes and Filters
> This simple idea is why Unix has been so successful. Instead of creating enormous
> programs that try to do many different things, Unix programmers focus on creating
> lots of simple tools that each do one job well, and that work well with each other.
> This programming model is called “pipes and filters”. We’ve already seen pipes; a
> filter is a program like `wc` or `sort` that transforms a stream of input into a
> stream of output. Almost all of the standard Unix tools can work this way: unless
> told to do otherwise, they read from standard input, do something with what they’ve
> read, and write to standard output.
>
> The key is that any program that reads lines of text from standard input and writes
> lines of text to standard output can be combined with every other program that
> behaves this way as well. You can and should write your programs this way so that
> you and other people can put those programs into pipes to multiply their power.
{: .callout}
<!-- Copied from https://swcarpentry.github.io/shell-novice/04-pipefilter/ -->

> ## Adding another pipe
> We have our `wc -l *_.txt | sort -n | head -n 1` pipeline. What would happen
> if you piped this into `cat`? Try it!
>
> > ## Solution
> > The `cat` command just outputs whatever it gets as input, so you get exactly
> > the same output from
> >
> > ~~~
> > $ wc -l *_.txt | sort -n | head -n 1
> > ~~~
> > {: .bash}
> >
> > and
> >
> > ~~~
> > $ wc -l *_.txt | sort -n | head -n 1 | cat
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Count, sort and print (faded example)
>To count the total lines in every `txt` file, sorting the results and then print the first line of the file we use the following:
>
>~~~
>wc -l *_.txt | sort | head -n 1
>~~~
>{: .bash}
>
>
>Now let's change the scenario. Imagine that we have a directory containing 100 `txt` files. We want to know the 10 files that contain the most words. Fill in the blanks below to count the words for each file, put them into order, and then make an output of the 10 files with the most words (Hint: The sort command sorts in ascending order by default).
>
>~~~
>__ -w *.txt | sort | ____
>~~~
>{: .bash}
>
> > ## Solution
> >
> > Here we use the `wc` command with the `-w` (word) flag on all `txt` files, `sort` them and then output the last 10 lines using the `tail` command.
> >~~~
> > wc -w *.txt | sort | tail -n 10
> >~~~
> {: .solution}
>{: .bash}
{: .challenge}


> ## Counting number of files, part I
> Let's make a different pipeline. You want to find out how many files and
> directories there are in the current directory. Try to see if you can pipe
> the output from `ls` into `wc` to find the answer, or something close to the
> answer.
>
> > ## Solution
> > You get close with
> >
> > ~~~
> > $ ls -l | wc -l
> > ~~~~
> > {: .bash}
> >
> > but the count will be one too high, since the "total" line from `ls`
> > is included in the count. We'll get back to a way to fix that later
> > when we've learned about the `grep` command.
> {: .solution}
{: .challenge}

> ## Writing to files
> The `date` command outputs the current date and time. Can you write the
> current date and time to a new file called `logfile.txt`? Then check
> the contents of the file.
>
> > ## Solution
> > ~~~
> > $ date > logfile.txt
> > $ cat logfile.txt
> > ~~~~
> > {: .bash}
> > To check the contents, you could also use `more` or many other commands.
> >
> > Beware that `>` will happily overwrite an existing file without warning you,
> > so please be careful.
> {: .solution}
{: .challenge}

> ## Appending to a file
> While `>` writes to a file, `>>` appends something to a file. Try to append the
> current date and time to the file `logfile.txt`?
>
> > ## Solution
> > ~~~
> > $ date >> logfile.txt
> > $ cat logfile.txt
> > ~~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Counting the number of words
>
> Check the manual for the `wc` command (using `wc --help`)
> to see if you can find out what flag to use to print out the number of words
> (but not the number of lines and bytes). Try it with the `_.txt` files.
>
> If you have time, you can also try to sort the results by piping it to `sort`.
> And/or explore the other flags of `wc`.
>
> > ## Solution
> >
> > From `wc --help`, you will see that there is a `-w` flag to print the number of
> > words:
> >
> > ~~~
> >      -w      The number of words in each input file is written to the standard
> >              output.
> > ~~~
> > {: .output}
> >
> > So to print the word counts of the `_.txt` files:
> >
> > ~~~
> > $ wc -w *_.txt
> > ~~~
> > {: .bash}
> > ~~~
> >   9129 1967-05-26_.txt
> >   9915 1967-09-24_.txt
> >  15283 1967-10-13_.txt
> >  29446 1967-10-27_.txt
> >  11909 1967-11-10_.txt
> >  14994 1967-11-22_.txt
> >  17048 1967-12-06_.txt
> >  14093 2014-11-24_.txt
> >  13343 2014-12-01_.txt
> >  12391 2014-12-04_.txt
> >  12028 2014-12-08_.txt
> >  12027 2014-12-11_.txt
> > 171606 total
> > ~~~
> > {: .output}
> >
> > And to sort the lines numerically:
> >
> > ~~~
> > $ wc -w *_.txt | sort -n
> > ~~~
> > {: .bash}
> > ~~~
> >    9129 1967-05-26_.txt
> >    9915 1967-09-24_.txt
> >   11909 1967-11-10_.txt
> >   12027 2014-12-11_.txt
> >  12028 2014-12-08_.txt
> >  12391 2014-12-04_.txt
> >  13343 2014-12-01_.txt
> >  14093 2014-11-24_.txt
> >  14994 1967-11-22_.txt
> >  15283 1967-10-13_.txt
> >  17048 1967-12-06_.txt
> >  29446 1967-10-27_.txt
> > 171606 total
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

## Mining or searching

Searching for something in one or more files is something we'll often need to do,
so let's introduce a command for doing that: `grep` (short for **global regular
expression print**). A regular expression is a special text string for describing 
a search pattern. As its name suggests, `grep` supports regular expressions and
is therefore only limited by your imagination, the shape of your data, and - when
working with thousands or millions of files - the processing power at your disposal.

To begin using `grep`, first navigate to the `TestFolder` directory if not already
there. Then create a new directory "results":

~~~
$ mkdir results
~~~
{: .bash}


Now let's try our first search:

~~~
$ grep parking *_.txt
~~~
{: .bash}

~~~
1967-10-27_.txt:for parking In unposted lots, and tben were Informed afterwards
1967-10-27_.txt:cooveolent parking faclllUes, If only on a temporary basis.
1967-11-10_.txt:did an outstanding job of sparking
1967-11-22_.txt:parking lot through the second
1967-12-06_.txt:knocking the tops oft parking me-
2014-12-04_.txt:brief struggle to find parking in the
~~~
{: .output}

Here we see every line that uses the word "parking." Not very many cases. Try using the command
to search for other words like "student," "science," "humanities," or a word of your choice.
Remember that the shell will expand `*_.txt` to a list of all the .txt files in the
directory. `grep` will then search these for instances of the string "parking" and
print the matching lines.

### Using spaces

Try searching for the word "she"

~~~
$ grep she *_.txt
~~~
{: .bash}

Read the results. Anything look fishy? `grep` will find *all* instances of "she", even when it
is inside another word. To isolate the word, use quotation marks to specifiy spaces on either side:.

~~~
$ grep " she " *_.txt
~~~
{: .bash}

Now try using pipes to count the number of times " she " is used verse " he ". 

~~~
grep " she " *_.txt | wc -l
~~~
{: .bash}

~~~
grep " he " *_.txt | wc -l
~~~
{: .bash}

Interesting? What other searches would you do on the entire data set?  

> ## Strings
> A string is a sequence of characters, or "a piece of text".
{: .callout}

You can count the number of times a word appears in a file by adding the `-c`flag
to `grep`. 

~~~
$ grep -c " she " *_.txt
~~~
{: .bash}
~~~
1967-05-26_.txt:0
1967-09-24_.txt:0
1967-10-13_.txt:4
1967-10-27_.txt:1
1967-11-10_.txt:3
1967-11-22_.txt:1
1967-12-06_.txt:0
2014-11-24_.txt:3
2014-12-01_.txt:2
2014-12-04_.txt:8
2014-12-08_.txt:0
2014-12-11_.txt:8
~~~
{: .output}

The shell now prints the number of times the string " she " appeared in each file.
If you look at the output from the previous command, this tends to refer to the
date field for each journal article.

We will try another search:

~~~
$ grep -c " he " *_.txt
~~~
{: .bash}
~~~
1967-05-26_.txt:7
1967-09-24_.txt:19
1967-10-13_.txt:6
1967-10-27_.txt:40
1967-11-10_.txt:16
1967-11-22_.txt:29
1967-12-06_.txt:42
2014-11-24_.txt:7
2014-12-01_.txt:14
2014-12-04_.txt:2
2014-12-08_.txt:10
2014-12-11_.txt:16
~~~
{: .output}

We got back the counts of the instances of the string ` he ` within the files.
Now, amend the above command to the below and observe how the output of each is different:

~~~
$ grep -ci " he " *.txt
~~~
{: .bash}
~~~
1967-05-26_.txt:12
1967-09-24_.txt:28
1967-10-13_.txt:8
1967-10-27_.txt:53
1967-11-10_.txt:20
1967-11-22_.txt:34
1967-12-06_.txt:53
2014-11-24_.txt:9
2014-12-01_.txt:19
2014-12-04_.txt:3
2014-12-08_.txt:15
2014-12-11_.txt:16
~~~
{: .output}

This repeats the query, but prints a case
insensitive count (including instances of both ` he ` and ` He ` and other case variants).
Note how the count has increased. As before, cycling back and
adding `> results/`, followed by a filename (ideally in .txt format), will save the results to a data file.

So far we have counted strings in file and printed to the shell or to
file those counts. But the real power of `grep` comes in that you can
also use it to create subsets of tabulated data (or indeed any data)
from one or multiple files.  

~~~
$ grep -i " she " *_.txt
~~~
{: .bash}

<!--This script looks in the defined files and prints any lines containing ` she `
(without regard to case) to the shell. We add today's date to the filename using 
[ISO format](https://en.wikipedia.org/wiki/ISO_8601) of `YYYY-MM-DD`.

~~~
<!-- $ grep -i she *_.txt > results/2016-07-19_JAi-revolution.tsv
~~~
{: .bash}

<!--This saves the subsetted data to file.

Instead of using quotation marks to search for single words, we can also the the `-w` flag,
which indicates to only search for whole words. We can save these results to a .tsv file named with today's
date. First, let's make a new directory 'results.'

~~~
$ mkdir results
~~~
{: .bash}

~~~
$ grep -iw she *_.txt > results/2018-09-19_sheIW.tsv
~~~
{: .bash}

This script looks in the defined files and
exports any lines containing the whole word `she` (without regard to case)
to the specified .tsv file.

Try running the same script for 'he.' 

~~~
$ grep -iw he *_.txt > results/2018-09-19_heIW.tsv
~~~
{: .bash}

We can show the difference between the files we created.

~~~
$ wc -l results/*.tsv
~~~
{: .bash}
~~~
    346 results/2018-09-19_heIW.tsv
   62 results/2018-09-19_sheIW.tsv
  408 total
~~~
{: .output}
 

Finally, we'll use the **regular expression syntax** covered earlier to search for similar words.

> ## Basic and extended regular expressions
> There is unfortunately both ["basic" and "extended" regular expressions](https://www.gnu.org/software/grep/manual/html_node/Basic-vs-Extended.html).
> This is a common cause of confusion, since most tutorials, including ours, teach
> extended regular expression, but `grep` uses basic by default.
> Unles you want to remember the details, make your life easy by always using
> extended regular expressions (`-E` flag) when doing something more complex
> than searching for a plain string.
{: .callout}

The regular expression 'fr[ae]nc[eh]' will match "france", "french", but also "frence" and "franch".
It's generally a good idea to enclose the expression in single quotation marks, since
that ensures the shell sends it directly to grep without any processing (such as trying to
expand the wildcard operator *).

~~~
$ grep -iwE 'fr[ae]nc[eh]' *.txt
~~~
{: .bash}

The shell will print out each matching line.

We include the `-o` flag to print only the matching part of the lines e.g.
(handy for isolating/checking results):

~~~
$ grep -iwEo 'fr[ae]nc[eh]' *.txt
~~~
{: .bash}


Pair up with your neighbor and work on these exercies:

> ## Case sensitive search
> Search for all case sensitive instances of
> a word you choose in all four derived tsv files in this directory.
> Print your results to the shell.
>
> > ## Solution
> > ~~~
> > $ grep Africa *_.txt
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Case sensitive search in select files
> Search for all case sensitive instances of a word you choose in issues appearing in the 1960s. 
>
> > ## Solution
> > ~~~
> > $ grep student 196*.txt
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Count words (case insensitive)
> Count all case sensitive instances of a word you choose in
> issues from 2000 to 2009  in this directory.
> Print your results to the shell.
>
> > ## Solution
> > ~~~
> > $ grep -c professor 200*.txt
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Count words (case insensitive)
> Count all case insensitive instances of that word in the files
> in this directory. Print your results to the shell.
>
> > ## Solution
> > ~~~
> > $ grep i student *_.txt
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Case insensitive search in select files
> Search for all case insensitive instances of that
> word in the files in this directory. Print your results to  a file `results/new.tsv`.
>
> > ## Solution
> > ~~~
> > $ grep -i student *_.txt > results/new.tsv
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Case insensitive search in select files (whole word)
> Search for all case insensitive instances of that whole word
> in the files in this directory. Print your results to a file `results/new2.tsv`.
>
> > ## Solution
> > ~~~
> > $ grep -iw student *a.tsv > results/new2.tsv
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

<!--> ## Searching with regular expressions
> Use regular expressions to find all ISSN numbers
> (four digits followed by hyphen followed by four digits)
> in `2014-01_JA.tsv` and print the results to a file `results/issns.tsv`.
> Note that you might have to use the `-E` flag (or `-P` with some versions
> of `grep`, e.g. with Git Bash on Windows.).
>
> > ## Solution
> > ~~~
> > $ grep -E '\d{4}-\d{4}' 2014-01_JA.tsv > issns.tsv
> > ~~~
> > {: .bash}
> >
> > or
> >
> > ~~~
> > $ grep -P '\d{4}-\d{4}' 2014-01_JA.tsv > issns.tsv
> > ~~~
> > {: .bash}
> >
> > If you came up with something more advanced, perhaps including word boundaries,
> > please share your result on the Etherpad and give yourself a pat on the shoulder.
> >
> > {: .bash}
> {: .solution}
{: .challenge}

<!-- could be good, just not right now... 
> ## Finding unique values
> If you pipe something to the `uniq` command, it will filter out duplicate lines
> and only return unique ones. Try piping the output from the command in the last exercise
> to `uniq` and then to `wc -l` to count the number of unique ISSN values.
> Note: This exercise requires the `-o` flag. See the callout box "Invalid option -- o?"
> above.
>
> > ## Solution
> > ~~~
> > $ grep -Eo '\d{4}-\d{4}' 2014-01_JA.tsv | uniq | wc -l
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Counting number of files, part II
> In the earlier counting exercise in this episode, you tried counting the number
> of files and directories in the current directory.
>
> * Recall that the command `ls -l | wc -l` took us quite far, but the result was one
>   too high because it included the "total" line in the line count.
> * With the knowledge of `grep`, can you figure out how to exclude the "total"
>   line from the `ls -l` output?
> * Hint: You want to exclude any line *starting*
>   with the text "total". The hat character (^) is used
>   in regular expressions to indicate the start of a line.
>
> > ## Solution
> > To find any lines starting with "total", we would use:
> >
> > ~~~
> > $ ls -l | grep -E '^total'
> > ~~~
> > {: .bash}
> >
> > To *exclude those lines, we add the `-v` flag:
> >
> > ~~~
> > $ ls -l | grep -v -E '^total'
> > ~~~
> > {: .bash}
> >
> > The grand finale is to pipe this into `wc -l`:
> >
> > ~~~
> > $ ls -l | grep -v -E '^total' | wc -l
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

### Using a Loop to Count Words

We will now use a loop to automate the counting of certain words within a document.

Let's rename a copy of a newspaper file to something easy to type by using the copy command: `cp`.


~~~
$ cp 2014-12-11_.txt newspaper.txt
~~~

This will make a copy of the file and name it "newspaper.txt."

Now let's create our loop. In the loop, we will ask the computer to go through the text, looking for the words "he," "she," "student," "professor,"and count the number of times each of them appear. The results will print to the screen.

~~~
$ for name in "he" "she" "student" "professor"
> do
>    echo $name
>    grep -wi $name newspaper.txt | wc -l
> done
~~~

{: .bash}

~~~
he
22
she
12
student
8
professor
1
~~~
{: .output}

What is happening the the loop?  
+ `echo $name` is printing the current value of `$name`
+ `grep $name newspaper.txt` finds each line that contains the value stored in `$name`
+ The output from the `grep` command is redirected with the pipe, `|` (without the pipe and the rest of the line, the output from `grep` would print directly to the screen)
+ `wc -l` counts the number of _lines_ (because we used the `-l` flag) sent from `grep`. Because `grep` only returned lines that contained the value stored in `$name`, `wc -l` corresponds to the number of occurances of each girl's name.

## Experiment 

Talk with your partner about what sort of word searches might be interesting. Do you want to compare frequencies of different words? Do you want to see how word frequencies change over different times? What do these results suggest about representation of topics, interest, and voices withint the student newspaper? 

<!-- How would you then repeat this for every file? And then put into a tsv file? For visualization? 
