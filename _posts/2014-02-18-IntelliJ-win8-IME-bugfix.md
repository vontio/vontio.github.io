---
layout: post
tagline: "IntelliJ Win8 IME bugfix"
category : IDE
tags : [IDE,IntelliJ]
---
{% include JB/setup %}

IntelliJ Win8 IME bugfix
============================

## why

IntelliJ with Win8 IME will delete word after current caret,use `jdk6` to fix it.

## fix

- install `jdk6` download [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk6downloads-1902814.html)
- add envirment variable `IDEA_JDK` = `path/to/jdk6` 
- run `path/to/intelliJ/bin/idea.bat`