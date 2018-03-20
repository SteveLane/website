---
date: 2016-07-10
title: emacs hanging
description: emacs is hanging for some reason
tags: ["emacs", "r"]
comments: true
slug: "emacs-hanging"
---

For some reason `emacs` has been hanging on me. I've noticed this happening only when I've been running on battery (I'm on a MacBook Pro, early 2015 model).

When it's happened, I've generally been in fullscreen mode, and often using R and ESS. I thought it may be down to ESS and ElDoc giving me function hints, but I'm not too sure. It could also be due to the native fullscreen mode being sent to a new workspace on the mac.

For the moment, I've turned off native fullscreen. This is pretty simple to do, e.g. in your `init.el` put something like:

```
;; Get rid of OSX native fullscreen (use f11 or M-x toggle-frame-fullscreen)
(setq ns-use-native-fullscreen nil)
```

We'll see what happens!

### Update!

It's definitely happening in R when ESS is making use of ElDoc to provide argument hints on the echo area. No idea why it only seems to happen when I'm running off battery though.
