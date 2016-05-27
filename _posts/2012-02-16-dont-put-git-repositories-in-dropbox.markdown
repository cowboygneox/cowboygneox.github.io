---
author: cowboygneox
comments: true
date: 2012-02-16 17:11:51+00:00
layout: post
link: https://seanfreitag.wordpress.com/2012/02/16/dont-put-git-repositories-in-dropbox/
slug: dont-put-git-repositories-in-dropbox
title: Don't Put Git Repositories in Dropbox
wordpress_id: 178
categories:
- Git
tags:
- Dropbox
- Git
---

Recently, my wife's laptop started to become excruciatingly slow. The CPU fans were blaring and applications like [Firefox](http://www.mozilla.org/en-US/firefox/new/) took ages to do anything. After investigating the CPU spikes, I came to find that it was the trusty [Dropbox](https://www.dropbox.com/) that we share, and it was making her computer have a heart attack due to all of the files it had to index and deal with.

Long story short, Git, Mercurial, and other source control applications often have thousands of files on top of the source that you are maintaining. If you put a repository in a Dropbox, it is basically source controlling the source control, and the implications of that cause a lot of wasted CPU cycles and bandwidth consumption.

I do not blame Dropbox for not being able to effectively work with source control repositories, since the algorithmic complexity of managing hundreds of thousands of individual files is a daunting task, and I applaud their efforts. However, if you still choose to put repositories in your Dropbox, I recommend you configure it as so:



	
  1. Place all repositories in a separate folder

	
  2. On a server computer, configure Dropbox to monitor updates on the repository folder (and any other folders you care about)

	
  3. On all client computers, make sure to configure Dropbox to NOT monitor updates on the repository folder


The client machines should not directly touch the repositories in the Dropbox; leave that for the server to do. Pushing updates to the server is more Git-centric anyway.

I have used the configuration above with success, but I have yet to find any real reason to put my repositories in my Dropbox, so I will host my source controlled code on a remote repository (such as [GitHub](https://github.com/)) and I will keep local repositories out of my Dropbox; my computers will thank me.
