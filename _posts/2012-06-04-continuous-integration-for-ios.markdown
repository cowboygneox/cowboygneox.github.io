---
author: cowboygneox
comments: true
date: 2012-06-04 12:54:23+00:00
layout: post
link: https://seanfreitag.wordpress.com/2012/06/04/continuous-integration-for-ios/
slug: continuous-integration-for-ios
title: Continuous Integration for iOS
wordpress_id: 199
categories:
- iOS
---

In my opinion, a programming language is completely useless to me if I cannot create a test and build process that is consistent and reproducible on many different machines, and sometimes, different platforms; iOS/Objective-C is certainly no exception. Assuming that you, the reader, agree with me and just want to see how I have implemented CI with my iOS projects, then here we go!

**Assumptions:**




  * Xcode 4.x (Xcode 3.x will likely work, but I cannot confirm.)


  * The project/target is named FunProject, and tests are FunProjectTests


  * You can build for either device or simulator using `-sdk iphoneos` or `-sdk iphonesimulator`, but I will assume iphonesimulator


  * The configuration will be Debug unless specified, but I will use AdHoc


**AdHoc build from commandline**
To start, you can build a target for AdHoc deployments from the command line:
[sourcecode language="bash" light="true" wraplines="false"]
/usr/bin/xcodebuild -target FunProject -sdk iphoneos -configuration AdHoc clean build
[/sourcecode]
A breakdown of options:




  * -target: not needed if you want the primary target, but its good to be specific


  * -sdk: specify the SDK. You can be specific (ex. iphonesimulator4.3) or assume version (ex. iphonesimulator).


  * -configuration: I believe it defaults to Debug, but best to be explicit.


  * clean build: Cleans the project, then builds it. I generally recommend cleaning the project before building.



**Tests from the command line**
[sourcecode language="bash" light="true" wraplines="false"]
/usr/bin/xcodebuild -target FunProjectTests -configuration Debug -sdk iphonesimulator clean build TEST_AFTER_BUILD=YES
[/sourcecode]
More options:




  * TEST_AFTER_BUILD=YES: Without this, the tests won't actually run.



**Script that pulls it all together**
In a future article, I will outline packaging the application for use on HockeyApp or TestFlight, but for simple builds in a continuous integration environment, a simple bash script looks like this:
[sourcecode language="bash" light="true" wraplines="false"]
#!/bin/bash

# End execution of this script if any execution returns a bad exit code
set -e

# Run the tests
/usr/bin/xcodebuild -target FunProjectTests -configuration Debug -sdk iphonesimulator clean build TEST_AFTER_BUILD=YES

# Build the AdHoc product
/usr/bin/xcodebuild -target FunProject -sdk iphoneos -configuration AdHoc clean build
[/sourcecode]

Simple and effective. As always, I welcome feedback and questions, so please let me know if I can help you, the reader, make your iOS experience more effective.
