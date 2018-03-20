---
title: "Reviewer Responses"
description: "In which we have our cake and eat it too."
date: 2018-03-20T20:42:18+11:00
publishDate: 2018-03-21
tags: ["git", "cmdline"]
categories: blog
---

Clearly one of the immensely joyful experiences of being in academia, is being on the receiving end of reviewer comments.

Responding to the reviewer comments can be tedious --- if you're a word (shudder) user, then no doubt you'd use track changes. But generally, you'd also respond to the requested changes in some form of a response letter. (At least, that's what I do.)

Those of us using latex/markdown and some form of version control can make this process really easy --- you just need to set up your git comments in a structured manner.

I respond to one comment at a time. When I've made changes, I commit them, and use the commit message to track these changes. I have the header of the commit message which reviewer I'm responding to, then use marks to signify the reviewer comment, and my response. Something like this:

```
Reviewer 1

** Figure 2: you should include ...

*** We do not agree with the reviewer on this point. This is because ...
```

One all the reviews are done, it's really easy to scrape them all out using `git log` and some grep-fu. For example, you can list out all the comments by Reviewer 1 like so:

```sh
$ git log --grep="Reviewer 1" --pretty=format:"%s%n%n%b"
```

This formats the log using `%s`, the subject (header) and `%b` the body, of the commit message.

Of course, it's much more efficient if you extract all these changes, for each reviewer you have, and export to word (shudder) using [pandoc](https://pandoc.org/):

```sh
#!/bin/bash

# Script to retrieve responses to reviewers from git commits.
# Must be run in a git repository (should check...)

# Exit if no arguments were provided.
[ $# -eq 0 ] && { echo "Usage: $0 [number of reviewers] [output name]"; exit 1; }

# Arguments are: number of reviewers, name of output file

echo "# Responses to Reviewers Comments
" > $2.md

for (( n=1; n<=$1; n++ ))
do
    # First, extract the comments.
    echo "Extracting responses to Reviewer $n"
    echo "
## Reviewer $n
" >> $2.md
    git log --reverse --grep="Reviewer $n" --pretty=format:"%s%n%n%b" >> $2.md

    # Now, note that reviewer comments start with 'Reviewer X', so delete those
    sed -i.bak '/^Reviewer/ d' $2.md

    # Now, the way I structure the git comments is to have * in place of #
    # This is because # in git commit messages gets ignored.
    # Replace * with # (doing the hard way in case there's extra of these characters floating around)
    sed -i.bak 's/^\*\*\* /\#\#\# /g' $2.md
    sed -i.bak 's/^\*\*\*\* //g' $2.md

    # Finally, convert the markdown to docx
    pandoc -o $2.docx $2.md
done

# Remove backup
rm $2.md.bak
```
