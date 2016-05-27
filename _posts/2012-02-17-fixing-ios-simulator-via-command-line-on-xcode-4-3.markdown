---
author: cowboygneox
comments: true
date: 2012-02-17 15:42:52+00:00
layout: post
link: https://seanfreitag.wordpress.com/2012/02/17/fixing-ios-simulator-via-command-line-on-xcode-4-3/
slug: fixing-ios-simulator-via-command-line-on-xcode-4-3
title: Fixing iOS Simulator Launching via Command Line on Xcode 4.3
wordpress_id: 188
categories:
- iOS
tags:
- iOS
- Xcode
---

I, like many others, use the command line to start the iOS simulator for tests and other automated reasons. However, since Xcode 4.3, Apple has broken all variations of command line invoking of the iOS Simulator.

Apple decided to remove the `/Developer` folder as the installation path, and now Xcode 4.3 can be found in `/Applications/Xcode.app`. Many compiled libraries require the `/Developer` at run time, so lucky enough for us, that's an easy fix:
[sourcecode language="bash" light="true" wraplines="false"]
sudo ln -s /Applications/Xcode.app/Contents/Developer/ /Developer
[/sourcecode]

When running a command line launcher for the simulator, you may run into the following irritating exception:
[sourcecode language="bash" light="true" wraplines="false"]
dyld: Library not loaded: @rpath/DevToolsFoundation.framework/Versions/A/DevToolsFoundation
[/sourcecode]

Apple moved the DevTools frameworks from `/Developer/Library/PrivateFrameworks/` to `/Applications/Xcode.app/Contents/OtherFrameworks/`. This is an odd change, but it can be speculated that Apple did not want developers using those particular frameworks any more. Fixing this is also trivial:
[sourcecode language="bash" light="true" wraplines="false"]
sudo ln -s /Applications/Xcode.app/Contents/OtherFrameworks/DevToolsCore.framework /Developer/Library/PrivateFrameworks/
sudo ln -s /Applications/Xcode.app/Contents/OtherFrameworks/DevToolsCParsing.framework /Developer/Library/PrivateFrameworks/
sudo ln -s /Applications/Xcode.app/Contents/OtherFrameworks/DevToolsFoundation.framework /Developer/Library/PrivateFrameworks/
sudo ln -s /Applications/Xcode.app/Contents/OtherFrameworks/DevToolsInterface.framework /Developer/Library/PrivateFrameworks/
sudo ln -s /Applications/Xcode.app/Contents/OtherFrameworks/DevToolsKit.framework /Developer/Library/PrivateFrameworks/
sudo ln -s /Applications/Xcode.app/Contents/OtherFrameworks/DevToolsRemoteClient.framework /Developer/Library/PrivateFrameworks/
sudo ln -s /Applications/Xcode.app/Contents/OtherFrameworks/DevToolsSupport.framework /Developer/Library/PrivateFrameworks/
[/sourcecode]

Note: I did try to recompile the source for [WaxSim](https://github.com/square/WaxSim) and [ios-sim](https://github.com/Fingertips/ios-sim), trying to get the build to be compatible with both Xcode 4.2 and 4.3, but I could not create a build that worked on both. If someone figures it out, let me know, because using an Xcode 4.3 binary would be the ideal situation.

Comments are always welcome :)
