---
author: cowboygneox
comments: true
date: 2012-10-26 20:11:10+00:00
layout: post
link: https://seanfreitag.wordpress.com/2012/10/26/giving-the-crash-reporter-time-to-do-its-work/
slug: giving-the-crash-reporter-time-to-do-its-work
title: Giving The Crash Reporter Time To Do Its Work
wordpress_id: 227
categories:
- iOS
---

At my place of business, I've been having many issues with corrupt data crashing my apps. Often times, when my app crashes, it will continue to crash on startup, forcing the user to have to uninstall and reinstall my app. The biggest issue with this entire scenario is that the crash reporting framework is unable to submit the crash report because of the immediate nature of the crash, and thus I never receive any crash reports, and never solve the problem. Obviously, an application that cannot submit its crash reports is not a suitable production app.





I use [HockeyApp](http://www.hockeyapp.net/) for our AdHoc deployment, and [QuincyKit](http://quincykit.net/) for AdHoc and App Store crash reports. QuincyKit does a fantastic job of submitting crash reports (when the application allows it to). I needed to engineer a way to ensure that QuincyKit received the time it required to submit crash reports before trying to reinitialize the app (and likely crash it again), and so I came up with this simple application delegate solution below using `NSRunLoop`:




    
    <code>// This solution assumes usage of both HockeyApp and QuincyKit
    // Changing the source to work with a custom QuincyKit configuration should be trivial
    
    @interface SimpleAppDelegate : NSObject <UIApplicationDelegate, BWQuincyManagerDelegate>
    
    @property (nonatomic, retain) UIWindow *window;
    @property (nonatomic) BOOL waitForCrashReportToSend;
    
    @end
    
    @implementation SimpleAppDelegate
    
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        [self installCrashHandlers];
    
        // initialize any other frameworks and your app
    
        return YES;
    }
    
    - (void)installCrashHandlers {
        // Configure QuincyKit
        [[BWQuincyManager sharedQuincyManager] setAppIdentifier:[self hockeyappAppId]];
        [[BWQuincyManager sharedQuincyManager] setDelegate:self];
        [[BWQuincyManager sharedQuincyManager] setAutoSubmitCrashReport:YES];
    
        // Only wait for crashes to be submitted if there are crashes to submit
        self.waitForCrashReportToSend = [[BWQuincyManager sharedQuincyManager] didCrashInLastSession];
    
        // Block this thread, but allow the run loop to continue until QuincyKit is finished
        while (self.waitForCrashReportToSend) {
            [[NSRunLoop currentRunLoop] runUntilDate:[[NSDate date] dateByAddingTimeInterval:0.1]];
        }
    
        NSLog(@"Crash handlers are installed");
    }
    
    // From BWQuincyManagerDelegate, called when QuincyKit is finished summitting crash reports
    - (void)connectionClosed {
        self.waitForCrashReportToSend = NO;
    }
    
    - (NSString *)hockeyappAppId {
        // Return the hockeyapp app id
    }
    
    @end
    </code>





Using the above source, I have been very successful in receiving the crashes from our users. It does not solve my primary issue of data corruption, but it ensures that I have as much information about the problem as possible.





Please feel free to ask questions or make comments!
