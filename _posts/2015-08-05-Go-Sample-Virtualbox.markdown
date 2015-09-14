---
layout: post
title: Sample GoCD Virtualbox based environment
status: public
type: post
published: true
author: Ken Mugrage
---

If you're interested in checking out GoCD but don't want to spend the time automating your
own system, this might be a great option for you. We've created an environment using Vagrant and Virtualbox.
Once it's up, you'll have a full GoCD installation including several example pipelines.

__Note: This is an update to a blog post last year. The demo has been modified enough to warrant a new post.__

The original version of this box used very simple PHP applications with some also simple tests. Neither these applications
nor the tests were "real" enough to be representative of what you're actually doing, and the need to keep the demo box self
contained made the real applications more of a distraction and source of problems than anything of real value.

This version uses Rake with empty targets and simple /bin/echo tasks instead of real deployments.

### System Requirements

__In order to run this you'll need [Virtualbox](https://www.virtualbox.org/) 
and [Vagrant](https://www.vagrantup.com/).__ Both of these are available for most operating 
systems.

### Using the box
To get started, open a command prompt in an empty directory and type...

<blockquote>
vagrant init gocd/gocd-demo
</blockquote>

This will create a file called Vagrantfile in your current directory. 

Next, type...

<blockquote>
vagrant up
</blockquote>

Completion of this (especially the first time) will take quite a while, depending on your
bandwidth. Vagrant will be downloading the full box image (about 1GB) from Vagrantcloud
while you wait.

__Note:__ If you have an existing GoCD installation on the same machine as this virtual machine
you may get a port conflict.

After a few minutes, you should be able to navigate to http://localhost:8153/go/pipelines on your local
machine and see the following...

![](/images/blog/sample-virtualbox/pipelines-v2.png)

These pipelines are all related, as shown in the following value stream map screenshot...

![](/images/blog/sample-virtualbox/vsm-v2.png)

Feel free to play around with the installation and see how everything works. You can always
reset the box to it's orginal state if you need to!

### What's on the machine?

The box will be updated as new things come out, but as of this writing...

* GoCD 15.2 Server
* GoCD 15.2 Agent
* Pipelines which are simulated using empty Rake targets and /bin/echo commands

<br>

As always, GoCD questions can be asked at [https://groups.google.com/forum/#!forum/go-cd](https://groups.google.com/forum/#!forum/go-cd)




