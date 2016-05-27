---
author: cowboygneox
comments: true
date: 2014-02-05 06:09:03+00:00
layout: post
link: https://seanfreitag.wordpress.com/2014/02/05/xcode-fails-to-run-when-switching-between-versions/
slug: xcode-fails-to-run-when-switching-between-versions
title: Xcode fails to run when switching between versions
wordpress_id: 233
categories:
- iOS
tags:
- iOS versions
- Xcode
---

I haven't posted in a while, but I'd really like to start getting back into it. I figured I would start with something easy.

I was trying to switch between iOS 7 and iOS 7 beta 5 today, and I struggled to get the simulator to start on the iOS 7 beta 5. I rebooted and it worked, but then I couldn't get iOS 7 to work.

Recap, to change between iOS versions:

{% highlight bash %}
# /Applications/Xcode.app is the default, but you can use another
sudo xcode-select --switch /Applications/Xcode.app/
{% endhighlight %}

But sometimes, even when you close Xcode, its processes can hang around and interfere with the other version that you want to run. Use this command to take care of that:

{% highlight bash %}
ps -ea | grep 'Xcode' | grep -v 'grep' | cut -d' ' -f 1 | xargs kill
{% endhighlight %}

Have fun switching between stable and non-stable Xcode's!
