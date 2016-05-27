---
author: cowboygneox
comments: true
date: 2012-10-10 17:14:12+00:00
layout: post
link: https://seanfreitag.wordpress.com/2012/10/10/running-ui-tests-on-an-ios-device-in-a-continuous-integration-environment/
slug: running-ui-tests-on-an-ios-device-in-a-continuous-integration-environment
title: Running UI Tests on an iOS Device in a Continuous Integration Environment
wordpress_id: 221
categories:
- iOS
---

In testing, there are many different ways to stress your code. In a [previous post](http://seanfreitag.wordpress.com/2012/06/04/continuous-integration-for-ios/), I discussed running unit tests in a continuous integration environment. However, no matter how hard you try, in a Model-View-Controller pattern codebase, unit tests can really only tackle the model, and maybe a little bit of the controller. A higher level of testing is required in order to ensure the visual elements exist and function as expected.





## Enter KIF





KIF is a test framework by the iOS team at [Square](https://squareup.com/). The UI testing that Apple provides is Javascript based, and near to impossible to automate. Square decided to create a testing framework driven by Objective-C, and runnable via the command line, instead of proprietary software. This is awesome, because it allows iOS developers to be able to write UI tests in their native language, and test via the iPhone Simulator from command line.







  * Original Source: [square/KIF](https://github.com/square/KIF)


  * My Updated Source:  [cowboygneox/KIF](https://github.com/cowboygneox/KIF)






## KIF for iPhone Simulator Tests





I have found that the documentation on [square/KIF](https://github.com/square/KIF) is effective for enabling and writing tests with KIF on the iPhone Simulator. They even have a section for running the KIF tests on the simulator from the command line.





###### Changes I would recommend with the instructions







  * My updated [cowboygneox/KIF](https://github.com/cowboygneox/KIF) contains a build script to make a universal libKIF.a. If you take the universal library and the header files for KIF, you don't have to bring in the KIF project and further obfuscate your project files.


  * For running the KIF tests via the command line, use my fixed [cowboygneox/Waxsim](https://github.com/cowboygneox/WaxSim). WaxSim was not honoring the sdk provided, so I fixed it.






## KIF for Device Tests





If your experience in iOS is anything like mine, you'll find that the simulator does a very poor job of emulating the hardware it simulates. I have found many instances where the UI I designed functioned as expected in the simulator, but had major flaws when put on a device; flaws so large that had the same UI tests been run on the device, the UI tests would have failed. Thus, I decided that KIF is only valuable if it can be run on the device from the command line.





In my research, I could not find any public information about running iOS UI tests on the device in a continuous integration environment, so I sought out to do it myself. I needed a way to install and debug a compiled application on the device, and with a quick Google search, I found Fruitstrap.





#### Fruitstrap





Fruitstrap is a command-line utility intended to install and debug an iOS application on a device. It requires files that come with Xcode, but doesn't directly require Xcode to be present or running. Although this project is no longer maintained, I decided to pickup the source as it was, clean it up a bit, and add the required mechanisms to work with KIF.







  * Original Source: [ghughes/fruitstrap](https://github.com/ghughes/fruitstrap)


  * My Updated Source: [cowboygneox/fruitstrap](https://github.com/cowboygneox/fruitstrap)






##### Installing Fruitstrap





I would recommend using the instructions I have written on [cowboygneox/fruitstrap](https://github.com/cowboygneox/fruitstrap).





##### Using Fruitstrap with KIF Tests





The script below is what I use in my continuous integration environment (specifically [Jenkins](http://jenkins-ci.org/)) for running KIF tests on a device. Here are some notes about the script:







  * The script is run in the project root


  * Fruitstrap has been compiled and placed in the project root


  * There is an Xcode project named `UI Tests` which compiles and runs the KIF tests.


  * The script is expected to have an argument of the 40 character UUID of the device used for testing


  * The script writes to a file `/tmp/KIF-$$.out` and to `stdout`


  * The script will end with a nonzero exit code if it cannot find '…0 failures'.


  


    
    <code>#!/bin/bash
    
    # Exit on errors
    set -e
    
    # Build KIF for use on the device
    xcodebuild -target "UI Tests" -sdk iphoneos clean build CONFIGURATION_BUILD_DIR=/tmp/build
    
    # Use fruitstrap as a means to install and run the app
    ./fruitstrap -d -b /tmp/build/UI\ Tests.app -i $1 2>&1 | tee /tmp/KIF-$$.out
    
    # Check the log to see if testing finished correctly
    grep -q "TESTING FINISHED: 0 failures" /tmp/KIF-$$.out
    </code>





## Wrap Up





Combining together the unit test runner in my [previous post](http://seanfreitag.wordpress.com/2012/06/04/continuous-integration-for-ios/), the KIF simulator test runner documented [here](https://github.com/square/KIF), and the KIF device test runner shown above creates a pretty solid testing environment for iOS codebases. The mobile-sphere is quickly growing to have serious code bases, and the consumer expectation of quality in mobile apps is growing even faster. Using this test-bed combination allows us as developers to have strong confidence in our iOS products, and helps to eliminate the potential of any dumb bugs that will give our app a 1-star rating on the App Store.
