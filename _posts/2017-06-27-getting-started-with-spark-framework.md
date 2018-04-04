---
layout: post
title: "Getting Started with Spark Framework"
description: "Setting up a sample project using Spark Framework with Maven"
category: Developer
draft: false
tags:
- development
- side-project
- java
- maven
- spark-framework
- github
---

Starting from [Spark Framework Tutorials](https://sparktutorials.github.io/), my first step was to follow [Setting up Spark with Maven](https://sparktutorials.github.io/2015/04/02/setting-up-a-spark-project-with-maven.html).

I use Eclipse as my favorite IDE and when I was at the end of the tutorial I run the app.

This is the first error I came across:

```bash
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

that I solved thanks to this [Stackoverflow answer](https://stackoverflow.com/a/21787813/2768802) adding to `pom.xml` the following:

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-api</artifactId>
  <version>1.7.5</version>
</dependency>
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>1.7.5</version>
</dependency>
```

Then there was this warning:

```bash
log4j:WARN No appenders could be found for logger (dao.hsqlmanager).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
```

that I solved following this post: [Fix for log4j WARN No appenders could be found for logger, Please initialize the log4j system properly](http://www.journaldev.com/10721/log4j-warn-no-appenders-could-be-found-for-logger-please-initialize-the-log4j-system-properly):

adding `log4j.xml` to the project:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
 
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/" debug="false">
 
  <!-- console appender -->
  <appender name="console" class="org.apache.log4j.ConsoleAppender">
    <param name="Target" value="System.out" />
    <layout class="org.apache.log4j.PatternLayout">
      <param name="ConversionPattern" value="%-5p %c{1} - %m%n" />
    </layout>
  </appender>
     
  <!-- rolling file appender -->
  <appender name="file" class="org.apache.log4j.RollingFileAppender">
    <param name="File" value="logs/main.log" />
    <param name="Append" value="true" />
    <param name="ImmediateFlush" value="true" />
    <param name="MaxFileSize" value="10MB" />
    <param name="MaxBackupIndex" value="5" />
 
    <layout class="org.apache.log4j.PatternLayout">
      <param name="ConversionPattern" value="%d %d{Z} [%t] %-5p (%F:%L) - %m%n" />
    </layout>
  </appender>
 
  <root>
    <priority value="DEBUG" />
    <appender-ref ref="file" />
    <appender-ref ref="console" />
  </root>
 
</log4j:configuration>
```

and thanks to this post: [Log4j Tutorial](http://www.journaldev.com/10689/log4j-tutorial) addin the `init()` method to the `MainApp.java` file:

```java
package com.example.web.app;
import static spark.Spark.get;

import java.text.SimpleDateFormat;
import java.util.Date;

import org.apache.log4j.xml.DOMConfigurator;

public class MainApp {
	
  static {
    init();
  }

  public static void main(String[] args) {
    get("/hello", (req, res) -> {
      SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
      String message = String.format("Hello World [%s]", sdf.format(new Date()));
      return message;
    });
  }
	
  /**
  * method to init log4j configurations
  */
  private static void init() {
    DOMConfigurator.configure("log4j.xml");
  }
}
```

Finally I was able to call my basic endpoint `http://localhost:4567/hello` from [Postman](https://www.getpostman.com/).


> Thanks to [Chiu-Ki Chan](https://twitter.com/chiuki) for her advices about blogging in her video [How to be an Android Expert](https://www.youtube.com/watch?v=IMSY1uH4nT8) from [Android Summit 2015](https://www.youtube.com/playlist?list=PLzJZrgVJE8Bbjjpj7Sc3CpldvIiGnJFDg)