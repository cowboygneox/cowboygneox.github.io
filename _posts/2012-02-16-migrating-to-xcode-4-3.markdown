---
author: cowboygneox
comments: true
date: 2012-02-16 16:00:31+00:00
layout: post
link: https://seanfreitag.wordpress.com/2012/02/16/migrating-to-xcode-4-3/
slug: migrating-to-xcode-4-3
title: Migrating to Xcode 4.3
wordpress_id: 154
categories:
- iOS
---

While drinking my cup of coffee and clean installing Mac OS X Lion on my MacBook Pro this morning, I came across an unexpected inconvenience: Xcode had been changed on the App Store. Not just updated, though, Apple completely changed the location of major files that I use daily. (ex. git) Frustrating for me, but for you the reader, I will get you back online the way that you expect.

To start, you'll notice that the `/Developer` folder no longer exists, and better yet, the PATH variable isn't set to anything new, so anything developer is not set on your behalf. Not to fret, though, since the Developer folder has been moved to `/Applications/Xcode.app/Contents/Developer`. It seems that only the location has changed, and that the Developer folder's layout stayed the same, which is a nice bonus.

If you are like me and use [JetBrain's AppCode](http://www.jetbrains.com/objc/), or some other tool that makes external use of Xcode's libraries, then you'll need to tell those applications which hole Xcode has decided to hide in. From the command line, type the following:

[sourcecode language="bash" light="true" wraplines="false"]
sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer
[/sourcecode]

I was able to get `git` back by doing the following in Xcode:

Go to the menu `Xcode > Preferences... > Downloads` tab

[![](http://seanfreitag.files.wordpress.com/2012/02/screen-shot-2012-02-16-at-9-38-24-am1.png?w=300)](http://seanfreitag.files.wordpress.com/2012/02/screen-shot-2012-02-16-at-9-38-24-am1.png)

You'll notice that there is a new "Command Line Tools" option that hasn't been seen in the past. This will install applications like `git` into your `/usr/bin` folder.

Once Xcode is finished installing the "Command Line Tools", confirm a successful install by firing up the Terminal and typing `git`. You should see a series of options.

Let me know if this helps anyone, or if I can improve this post!
