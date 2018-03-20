---
date: 2017-08-16
title: "Running R on a Docker container through Emacs"
author: "Steve Lane"
slug: "docker-emacs"
tags: ["r", "docker", "emacs"]
categories: blog
comments: true
---

I perform all my analyses using [Emacs+ESS](http://ess.r-project.org/Manual/ess.html). I'm used to it, I like certain things it does that RStudio doesn't do, so I'm sticking with it.

However, I'm also using a mac. This has caused me headaches in trying to get graphics working from within a docker container. What I want to be able to do is this:

{{< tweet 897337191919042560 >}}

That is, I want to run a (local) containerised version of R, interact with it from Emacs running on my mac, and be able to plot graphics on the fly, rather than saving them to disk and then viewing.

The first thing I tried was something along these lines: [https://blog.jessfraz.com/post/r-containers-for-data-science/](https://blog.jessfraz.com/post/r-containers-for-data-science/). After a huge amount of searching, I still couldn't get the display to work; I was always getting errors telling me that `X11cairo` wasn't available. So, my mac's version of `XQuartz` wasn't communicating with the docker container (at least, that's what I think was the issue).

My solution, which is a massive hack, is to SSH into the running container (using X forwarding), and send code that way. I tried [`tramp`](https://www.gnu.org/software/tramp/) within Emacs, but it too wasn't passing my `DISPLAY` properly. Here's what I've done to get it working:

- Make sure your container has SSH up and working. A basic Dockerfile (using the `rocker/verse`) image may look like:

```
FROM rocker/verse:3.4.1

## Update and install extra packages
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    openssh-server \
    libx11-dev \
    x11proto-core-dev \
    xauth \
    xfonts-base \
    xvfb \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/

RUN mkdir /var/run/sshd

RUN echo 'root:root' | chpasswd

RUN sed -ri 's/^PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/X11Forwarding no/X11Forwarding yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/#X11UseLocalhost yes/X11UseLocalhost no/g' /etc/ssh/sshd_config

EXPOSE 22

CMD    ["/usr/sbin/sshd", "-D"]
```
	
- Build the container. From within the directory of your Dockerfile:

```bash
$ docker build -t ssh-container .
```

- Run the container (mapping the current directory to /home/rstudio/ and exposing SSH via the localhost port 42222):

```bash
$ docker run -d -v $(pwd):/home/rstudio/ -p 127.0.0.1:42222:22 \
  ssh-container
```

- Fire up Emacs, and open a shell (`M-x shell`); then SSH into the running container and start R:

```bash
$ ssh -Y rstudio@localhost -p 42222
$ R
```

- Whilst in the buffer that the R process is in, associate this with ESS: `M-x ess-remote RET R RET`.

- Open up your R script and tell ESS to use the R session on the container: `C-c C-s *shell*`.

That got me up and running, but as I said, it really is a hack!
