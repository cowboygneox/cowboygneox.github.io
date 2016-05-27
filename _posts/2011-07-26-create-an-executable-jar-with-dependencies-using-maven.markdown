---
author: cowboygneox
comments: true
date: 2011-07-26 01:36:45+00:00
layout: post
link: https://seanfreitag.wordpress.com/2011/07/25/create-an-executable-jar-with-dependencies-using-maven/
slug: create-an-executable-jar-with-dependencies-using-maven
title: Create an executable jar with dependencies using Maven
wordpress_id: 5
categories:
- JVM
tags:
- Maven
---

I recently ran into a need to create an executable jar, but did not know where to begin. I had the following requirements:



	
  1. It had to be executable (ex. java -jar runnable.jar)

	
  2. Included all dependencies (so that I did not have to tote them around otherwise)

        
  3. Played nicely with the Spring framework

        
  4. Maven compatible


As it turns out, Maven had a solution that accomplished just that. The [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/) allows creation of the fabled "UberJar", the magical jar that contains the source and dependencies combined together in perfect harmony.

While the documentation is pretty good, I figured I would post the configuration that worked for the above requirements:

**Pom source:**
[sourcecode language="xml" light="true" wraplines="false"]
<project>
  ...
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>1.4</version>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
            <configuration>
              <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                  <manifestEntries>
                    <Main-Class>org.sonatype.haven.ExodusCli</Main-Class>
                    <Build-Number>123</Build-Number>
                  </manifestEntries>
                </transformer>
                <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                  <resource>META-INF/spring.handlers</resource>
                </transformer>
                <transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                  <resource>META-INF/spring.schemas</resource>
                </transformer>
              </transformers>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
  ...
</project>
[/sourcecode]
**Using Maven Shade**
Technically, it executes automatically during the `mvn:package` plugin, but I typically run `mvn clean install` (because package happens before install).

**Explanation**
The "transformers" the are meat and potatoes of the plugin. With the above transformers, I can provide a manifest (configurable to anything, which is awesome), as well as AppendingTransformers, which prevent Spring schemas from being overwritten.
