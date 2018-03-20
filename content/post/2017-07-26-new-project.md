---
date: 2017-07-26
title: "Creating a new project directory"
comments: true
tags: ["r", "cmdline"]
slug: "new-project"
---

Create a new project directory easily!

> Make all the things.

I was starting a new data analysis project today, and came to the realisation I've been doing things stupidly. When I start a new project, I've been manually adding directories, copying over previously used `Makefile`'s and other scripts, and generally doing things slow.

So, I realised I should streamline this. I wrote a `bash` script to set up a new project directory (how I like it, ymmv). It also does some other neat stuff like creating a basic `Makefile`.

Check it out at [https://github.com/SteveLane/new-project-sh](https://github.com/SteveLane/new-project-sh).
