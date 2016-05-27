---
author: cowboygneox
comments: true
date: 2011-07-27 14:15:15+00:00
layout: post
link: https://seanfreitag.wordpress.com/2011/07/27/the-search-for-a-practical-rails-variant-on-the-jvm/
slug: the-search-for-a-practical-rails-variant-on-the-jvm
title: The search for a practical Rails variant on the JVM
wordpress_id: 27
categories:
- JVM
- Play Framework
tags:
- Grails
- Groovy
- Java
- Play Framework
- Ruby
- Ruby on Rails
- Scala
- Spring Roo
---

I am a huge fan of the Ruby on Rails implementation of convention over configuration. While I was exploring the Rails framework, I was shocked at how quickly I could set up a project, configure a database, build a template, and have a functional website prototype. It really seemed like it could have been enough to stray me away from the JVM. 

However, after a few days of playing around with it, I remembered that I am not a fan of dynamic languages when writing production level code. Perhaps I am old fashioned, but I do not like to have to test every line of my code to ensure that it will function as expected, and I certainly do not like what I call "invisible methods" on Ruby objects that prevent me from seeing all of the available functions attached to said object. I have spent a year in Python prior to moving to the JVM and, in my opinion, statically typed languages provide more functionality and promote faster, more robust code than dynamic languages (quite the generalization I know).

Statically typed languages have their faults as well. Java and C# suffer strongly from a lot of boiler-plate code and built in libraries that promote bad patterns. To compensate for this, other frameworks (such as Spring) have become popular to help improve the productivity in Java/C#, but using such frameworks, I feel like I spend all of my time managing dependencies in Maven and configuring the project, instead of writing the core of the code.

Because of how much time it takes me to configure a new webapp using Java, Spring, Maven, etc. (and maybe I'm just slow), I decided to seek out a Rails variant for the JVM and see if it can increase the speed in which I produce a webapp while allowing me more time to write the code I really want to write.

The three major JVM Rails variants I encountered are:



	
  1. [Grails](http://grails.org/)

	
  2. [Spring Roo](http://www.springsource.org/roo)

	
  3. [Play Framework](http://www.playframework.org/)



I have spent about 3 days of my side time on all 3 frameworks, so here is a quick summary of my findings:

**Grails**
I have a prejudice for dynamic languages (a fault that I will someday come to terms with), so using Groovy is not fun to me, therefore defeating the purpose of using Grails. Grails was not a good fit for me. Regardless, some strengths of Grails:



	
  * Uses Groovy

	
  * Now owned by SpringSource, so it is unlikely to lose support any time soon

	
  * Strong IDE tooling support

	
  * Very large adoption; cannot argue with mass numbers


**Spring Roo**
Spring has made a great effort at the convention over configuration, but then they introduced Roo only annotations and configuration options, completely leaving their popular and robust web-mvc framework in the dust. If Roo helped configure web-mvc Spring projects, I would have been sold, but I guess I wasn't impressed enough with all of its other bells and whistles. Also, since SpringSource owns both Grails and Roo, I expect that Grails will be the champion and Roo will be left behind. Some strengths of Roo:



	
  * Uses Java

	
  * Full IDE support

	
  * Rails-like command line tools for generating code


**Play Framework**
The Play Framework has where I have been investing my time in learning and mastering. I feel it has adopted many of Rails' strengths, and also recognizes Rails faults and attempts to improve on them. Some of Play's core strengths:



	
  * Java and Scala 2.8.1 support

	
  * No need for a web container (like Tomcat)

	
  * Excellent Documentation

        
  * Auto-recompiles at page-load time, so no need to build EVER


**Final thoughts on the search for Rails on the JVM**
If I were not on a Scala kick at this moment in time, I would probably investigate Grails more. It certainly has much greater support than the other two, and many more resources for developing and getting started.

With that said, given the initial exposure I have had with Grails, I would say that I personally think Play is more intuitive. I have briefly looked over the complete documentation for both Grails and Play, and have found that I am able to do more with Play with very little effort.

Because I find Play is the easiest framework to accomplish my goals, I am going to invest my time into Play and try to implement some webapp ideas that I have. Once I resolve my distaste for dynamic languages, I will try and circle back around to Grails for perspective, and see if Play truly stands a chance against Grails or if I'm just fighting the curve.

