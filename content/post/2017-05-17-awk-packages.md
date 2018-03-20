---
date: 2017-05-17
title: "What bl&^$y packages have I used?"
comments: true
tags: ["r", "cmdline", "awk"]
slug: "awk-packages"
---

> UPDATE: 2017-06-21. I've updated the script so that it now sorts the package list, and removes duplicates.

Sometimes you may be working on an analysis package that's spread into a wide array of scripts, each using various libraries, and you may have lost track of all those libraries your analysis requires.

When actually developing an R package, you list the packages your package needs in order to work in the `DESCRIPTION` file (see [here](http://r-pkgs.had.co.nz/description.html#dependencies)).

If you haven't already listed what packages are required for your analysis, it's a good thing to do. It makes all other users of your work explicitly aware of what those dependencies are. It's pretty standard to use `library()` or `require()` to attach your packages to your workspace, so we can work with that using the standard commandline tool [`awk`](https://en.wikipedia.org/wiki/AWK).

Consider a single file `analysis.R`; we can find (and strip) all package refs:

```bash
$ awk -F '[(]|[)]' '/^library|^require/{print $2;}' analysis.R >> installs.txt
```

What `awk` does by default is to break each line in the input file (here, `analysis.R`) into fields separated by whitespace; each field can be referenced by `$1`, `$2` and so on. You can also separate each line into fields by a regular expression which is what we've done above: `-F '[(]|[)]'` tells `awk` to split each line into fields at either opening or closing parentheses.

The second part of the command gives the *pattern-action* statement to be applied. So in our example, `'/^library|^require/{print $2;}'` takes lines that begin (`^`) with the patterns **library** or **require**, and then prints the second field (`$2`).

So, what's happening is that `awk` scans the file `analysis.R` for lines beginning with either **library** or **require**; for matching lines, it separates them into fields, splitting at either open or close parentheses.

An example will help; consider the following `analysis.R`:

```r
## This is a pretty standard analysis script.
## I need mgcv to do some work, so I attach it
library(mgcv)
## I then read in my data and do other stuff...
```

It's pretty standard for R scripts to put `library()` and `require()` calls on separate lines. So, our `awk` command will scan the file until the third line, where it finds **library** at the beginning of the line. It'll then split the line into two non-empty fields, `$1=library` and `$2=mgcv`. It prints `$2` (in this case, the string "mgcv") into the file `installs.txt`.

Done! Except, what if we have lots of scripts? A simple bash script suffices:

```bash
#!/bin/bash
# Strips libraries from .R/.r scripts and creates a text file with a list of them.
# Requires first argument as folder where scripts are stored, and second as the
# text file it is stored in.
folder=$1
inst=$2
if [ -f $inst ] ; then
    rm $inst
fi
for filename in $folder/*.R; do
    if [ -f $filename ]; then
	awk -F '[(]|[)]' '/^library|^require/{print $2;}' $filename >> $inst
    fi
done
for filename in $folder/*.r; do
    if [ -f $filename ]; then
	awk -F '[(]|[)]' '/^library|^require/{print $2;}' $filename >> $inst
    fi
done
sort < $inst > $inst.bk
uniq < $inst.bk > $inst
rm $inst.bk
```

Make the file executable (`chmod u+x strip-libs.sh`, if you call the file `strip-libs.sh`). Assuming your scripts are stored in the folder `R`, and you want to store the package list in the file `installs.txt`, pass those as arguments to the executable:

```bash
$ ./strip-libs.sh R/ installs.txt
```

The final three lines of the script are a simple hack to sort and remove duplicates in the package list. If you're on a *nix-like, `sort` and `uniq` should be available to you.
