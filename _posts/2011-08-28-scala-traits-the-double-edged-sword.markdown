---
author: cowboygneox
comments: true
date: 2011-08-28 19:41:25+00:00
layout: post
link: https://seanfreitag.wordpress.com/2011/08/28/scala-traits-the-double-edged-sword/
slug: scala-traits-the-double-edged-sword
title: 'Scala Traits: The Double Edged Sword'
wordpress_id: 126
categories:
- JVM
tags:
- Scala
---

Traits seem to be one of Scala's strongest selling points: the ability to make an interface that comes with implementation attached. Multiple inheritance comes with many strengths, but I have been thinking it may be a double-edged sword.

I had a thought that it may be possible to optimize trait compilation in Scala so that classes that implement the trait only acquire the fields and methods that it actually uses. I asked such a question on [Stack Overflow](http://stackoverflow.com), and I learned a lot, which I will summarize here.

_Note: [Here is the question I asked on Stack Overflow](http://stackoverflow.com/q/7219098/916128)_

**The Summary of What I Learned**



	
  * Traits bring everything into a class that implements them, regardless if they use it or not. This is to satisfy a compiler contract so that external classes can use the traits fields/methods

	
  * using `val` in a trait compiles to `private final`, but still takes memory in each implementing class

	
  * `private val`'s that are in the trait but not used are still compiled into their implementing classes. I guess the developer should be sure to only write code that he/she actually needs

	
  * Fields (`var/val`) will be copied to each instance of the implemented trait, and the methods (`def`) will be in a method table that applies to all implementing classes.

	
  * However, just because a method is universal, does not mean that it shares its memory. Placing `def obj = new DumbObject` in a trait will create a new `DumbObject` for each instance of the implementing class. To truly share memory, a companion object must be used with the trait.



**My Conclusion**
Using traits effectively means writing many concise, simple traits instead of "catch all" traits that contain too much bulk. It also means making use of `def`'s when possible and introducing `val/var`'s only when needed. Finally, if traits are to provide references of the same objects to all implementing classes, ex. for dependency injection, a companion object must be used.

Since traits can be mixed in easily (especially when using a cake pattern), the developer must be conscious of what is being mixed. A class that implements a trait will receive all of the benefits, but also all of the bulk, of a trait. I again reiterate that using traits are like wielding double-edged swords: powerful if used correctly, but deadly if not.
