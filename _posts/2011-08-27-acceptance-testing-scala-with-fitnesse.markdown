---
author: cowboygneox
comments: true
date: 2011-08-27 22:01:01+00:00
layout: post
link: https://seanfreitag.wordpress.com/2011/08/27/acceptance-testing-scala-with-fitnesse/
slug: acceptance-testing-scala-with-fitnesse
title: 'Acceptance Testing: Scala with FitNesse'
wordpress_id: 107
categories:
- JVM
tags:
- FitNesse
- Scala
---

In my current job, my team has been chosen to be the team that evaluates the possible integration of [Scala](http://www.scala-lang.org) into our Java-shop. We have been writing a simple webapp with about 4 months of work (between a team of 4 people) and using technologies like [Spring Framework](http://www.springsource.org), [Hibernate](http://www.hibernate.org), [Maven](http://maven.apache.org), etc. Since our department has chosen [FitNesse](http://fitnesse.org) as our acceptance test framework, we have been using it on our Scala webapp, and have encountered one fundamental issue.

**A Quick Explanation of FitNesse**
FitNesse is a wiki tool that takes written business requirements and turns them into acceptance tests. It uses the Fit library with JVM languages to exercise code and assert behavior from a very high abstraction. It parses the wiki pages and resolves them to "Fixtures", which are components that attach to code. This allows easy communication and testing of high-level business requirements.

_Note: This article assumes experience with Scala and FitNesse._

**The Fundamental Issue**
While we have not had a lot of issues using FitNesse with Scala, some of the fixtures require public fields, which Scala, by design, does not have.

Given the following source:
[sourcecode language="scala" light="true" wraplines="false"]
class TestClass {
  val someProperty: String
}
[/sourcecode]

It compiles into this:
[sourcecode language="java" light="true" wraplines="false"]
public class TestClass {
  private String someProperty;
  public String someProperty() {
    return someProperty;
  }
  public void someProperty_$(String someProperty) {
    this.someProperty = someProperty
  }
}
[/sourcecode]

Apparently, the ability to be able to access private fields in FitNesse fixtures has been a community request for a while now. Until I encountered the above scenario, I personally did not have any problem changing my fixtures to use public fields (it was annoying, yes, but not a deal breaker), but the design choices in FitNesse were not compatible with Scala's architecture.

**The Solution**
Luckily, my coworker and I figured out a solution and submitted it on [GitHub](https://github.com). Hopefully it will be accepted into the trunk.
[My FitNesse repository](https://github.com/gneoxsolutions/fitnesse)
[The pull request](https://github.com/unclebob/fitnesse/pull/44)

If the above changes are accepted into the trunk, then I no longer see any barriers to using Scala with FitNesse. +1 to Scala :)
