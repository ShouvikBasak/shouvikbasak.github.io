---
layout: post
title:  "Apache Hadoop - getting started"
description: "Quick reference for getting started with Apache Hadoop on a Mac" 
categories: [Tools]
tags: [Hadoop, Apache Hadoop, Install Hadoop on a Mac]
date: 2022-01-30 15:39:09 +0530
---

Pre-requisite: Java should be installed

* Install Java on Mac: (https://www.java.com/en/download/help/mac_install.html)[https://www.java.com/en/download/help/mac_install.html]

* finding JAVA_HOME: `java -XshowSettings:properties -version`
```
...
java.home = /Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre
...
java version "1.8.0_121"
Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)
```

* Update Java PATH and JAVA_HOME in .zshrc file
```
export PATH="$PATH:/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/bin"
export JAVA_HOME="/Library/Java/JavaV`irtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre"```









