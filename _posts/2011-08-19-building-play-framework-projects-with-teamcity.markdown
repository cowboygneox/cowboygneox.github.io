---
author: cowboygneox
comments: true
date: 2011-08-19 05:20:47+00:00
layout: post
link: https://seanfreitag.wordpress.com/2011/08/18/building-play-framework-projects-with-teamcity/
slug: building-play-framework-projects-with-teamcity
title: Building Play Framework Projects with TeamCity
wordpress_id: 45
categories:
- JVM
- Play Framework
tags:
- Maven
- Play Framework
- TeamCity
---

In my recent exploration of the [Play Framework](http://www.playframework.org/), I thought it would be practical to test its integration capabilities with a continuous integration system. Since I have background with JetBrain's [TeamCity](http://www.jetbrains.com/teamcity/) and it has been more than capable to meet my CI needs, I decided to figure out how to make it build Play projects.

For this tutorial, I am going to assume the following parameters:



	
  * TeamCity 6.5.3 is installed and configured

	
  * Play 1.2.2

	
  * Git 1.7.6


_Note: Other versions may work, but I listed the versions I was using when I wrote this article._

**Configuring a TeamCity agent for building Play projects**
Configuring the agent is not a difficult task, it just requires that Play be downloaded and installed.



	
  1. [Download the Play Framework](http://www.playframework.org/download) on the agent computer

	
  2. Follow the instructions and install Play to a location of your choice

	
  3. Optionally: For Scala support, be sure to execute the following on the agent:  
`play install scala`

	
  4. When Play tests fail, the runner returns an exit code of 0. Because of this, TeamCity will assume that the build succeeds, even though tests fail. To address this issue, there needs to be a step that checks the test status. Play has documented that in the test-result folder, it will contain a result.passed or result.failed file. If we check for that file, we can translate it into an exit code and feed that to TeamCity.
Make a script in the play home folder that contains the following:

_Windows (in a batch file, ex. checktest.bat):_
[sourcecode language="powershell" light="true" wraplines="false"]
@ECHO OFF
IF NOT EXIST test-result\result.passed EXIT 1
[/sourcecode]

_Linux/Unix/Mac OS X (in a script, ex. checktest.sh):_
[sourcecode language="bash" light="true" wraplines="false"]
#!/bin/bash
if [ -f test-result/result.passed ];
then
  exit 0
else
  exit 1
fi
[/sourcecode]
_Note: In Linux/Unix/Mac OS X, be sure to run _`chmod +x checktest.sh`_ to make your script executable_



**Creating a Play build configuration in TeamCity**
TeamCity offers the ability to build Maven, Ant, MSBuild, and other build tools, but it also offers a simple command line build step option. Using the command line, we can run the build and tests, and then check the result.



	
  1. Create a Project for the Play project in TeamCity if you haven't already

	
  2. Choose your project and select "Create build configuration".

	
  3. Name the configuration, and make sure "build process exit code is not zero" is checked.
Continue on to VCS settings when ready.

	
  4. Select the VCS of your choice, or click "Create and attach new VCS root" if you do not have one

	
  5. Choose Git, and set the Fetch URL to the Play project repository.
_Note: On my machine, I had my Git repository on my TeamCity server, so my URL was as follows:_
`file:///${path-to-repository}/${git-repository}`

	
  6. Change any other settings that are relevant to your VCS, then choose "Test connection". If everything checks out, click "Save".

	
  7. On the "Version Control Settings" page, adjust the settings to your liking (if needed) and click "Add Build Step".
_Note: I always recommend that "Clean all files before build" is checked, since it will ensure your repository is always fresh with no tainted files._

	
  8. There are three command line build steps to add (in order):

	
    1. Make the Play project resolve its dependencies (because they are not supposed to be in the repository)
Command executable: `${play-home}\play.[bat|sh]`
Command parameters: `dependencies`

	
    2. Run the tests using Play's test runner
Command executable: `${play-home}\play.[bat|sh]`
Command parameters: `auto-test`

	
    3. Run the checktest script shown above (which will tell TeamCity whether or not tests passed/failed)
Command executable: `${play-home}\checktest.[bat|sh]`

_Note: play.[bat|sh] represents the statement "Either play.bat or play.sh" depending on your operating system._
  9. Touch up the configuration as needed, but there are no more mandatory changes


If all is good, run the new build and watch it process your tests and report a passed or failed build. The build logs on TeamCity will report the console output exactly if it had been run locally, so use this to see passed/failed tests and other information.

**TODO:**
TeamCity uses artifacts to make it easy to navigate and manage passed/failed tests, but with the above approach, no artifacts are used. I would like to find a way to make artifacts work with TeamCity when using Play.

**Final Thoughts**
Play also supports Maven (`play install maven`) and Ant (`play install antify`). I would expect that TeamCity could use the Maven/Ant configurations generated by these plugins, but I haven't tested them myself. I would rather have TeamCity run the tests using Play's native runner and not have to manage external configurations.
