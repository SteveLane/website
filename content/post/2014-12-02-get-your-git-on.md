---
date: 2014-12-02
title: "Get your Git on"
comments: true
tags: ["reproducible research", "git", "markdown"]
slug: "get-your-git-on"
---

Git? Who you calling a Git, git?

I mentioned in this
[previous post]({%post_url 2014-11-24-tools-of-the-trade%}) that one of the
tools I use daily is git ([http://git-scm.com/](http://git-scm.com/)). Git is a
distributed version control system.

What does that mean exactly? Without getting technical, it means that you can
store versions of your documents and code for analyses, across various
systems/computers, and even with other people ([GitHub](https://github.com/) is
an example of the sharing part).

So is there a catch (yes, sort of), and what is it? Well, you'll have to learn
some git commands, and if you want to version control your reports and
manuscripts, you may well want to consider writing them in a flat file format. A
flat file format is a simple text document, without all the formatting that you
would get in, for example, a Word document. This is generally the biggest
barrier that people have when moving to writing in a different format.

Why do we need to bother with changing the format in which we write reports
and/or manuscripts? It's to do with the way that git does version control. Put
simply, git stores incremental changes to your file, rather than *whole*
versions. And herein lies part of its power! No longer do you have to store a
whole sequence of files, with stupidly incremented names such as
`report-draft-02-12-2014.docx`, which when you want to work on in a couple of
days time, you'll save a new copy and rename so that you can go back to the
previous version if required. That's totally inefficient!

Now, if you only generally have a small number of versioned drafts, then
sticking to Word documents might suffice, as they're generally small in size,
and these days, storage is cheap. But what about collaborating? Word has *track
changes*, which displays each of your collaborators edits in a different colour,
along with the edited text, but how then to combine all those edits, when
everyone has saved different versions of the document? Word also has a *combine*
feature, which shows and compares the edits in two documents, but I've generally
found this to be messy. You still need to keep some form of control over who
sees the document and when, otherwise it becomes too hard to manage.

Using `git diff` to look at changes between different *versions* of a file is a
much easier way (IMO) to track changes both by myself, but also with
collaborators. But there's the rub: you also need to convince your collaborators
that it's worthwhile changing their workflow, and old habits die hard!

Finally, using git is a great way to backup your work. You can commit your data
files, reports, etc. to a git repository on your personal/organisation network
share simply, which means you'll hopefully never lose your work, and it's also
accessible wherever your network is accessible - brilliant!

I'm going to discuss writing reports in
[markdown](http://daringfireball.net/projects/markdown/) (which works
fantastically with git, and with a few tweaks, produces some really nice html
output) in another post, so for now:

```{bash}
$ git init
```
