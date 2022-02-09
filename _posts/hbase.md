---
layout: post
title:  "Apache HBase - getting started on Mac"
description: "Quick reference for getting started with Apache HBase on a Mac" 
categories: [Apache]
tags: [HBase, Apache HBase]
date: 2022-02-09 08:15:09 +0530
---

Quick reference for a Standalone HBase installation on a Mac.

Apache HBase Reference Guide: https://hbase.apache.org/book.html

A standalone instance of HBase has all the HBase daemons included:
* Master
* RegionServers
* ZooKeeper 

All the 3 daemons run in a single JVM persisting to the local filesystem.

**Prerequisite:** JDK needs to be installed

**Download Apache HBase:**
https://hbase.apache.org/downloads.html => (download the bin for the stable version) = > download from https://www.apache.org/dyn/closer.lua/hbase/2.4.9/hbase-2.4.9-bin.tar.gz

**Extract and install:** `tar -xvzf hbase-2.4.9-bin.tar.gz`

**Configure:**

Check Java_HOME: `echo $JAVA_HOME`
    `$/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre`

Update `conf/hbase-env.sh` in HBase installation directory with:

    `export export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre`

Update ~/.zshrc file:

    `export PATH="$PATH:</install path>/hbase-2.4.9/bin"`


**Start HBase:** `start-hbase.sh`

Received the following warnings:
    ```
    SLF4J: Class path contains multiple SLF4J bindings.
    SLF4J: Found binding in [jar:file:/...../hadoop-3.3.1/share/hadoop/common/lib/slf4j-log4j12-1.7.30.jar!/org/slf4j/impl/StaticLoggerBinder.class]
    SLF4J: Found binding in [jar:file:/...../hbase-2.4.9/lib/client-facing-thirdparty/slf4j-log4j12-1.7.30.jar!/org/slf4j/impl/StaticLoggerBinder.class]
    SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
    SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
    running master, logging to /..../hbase-2.4.9/bin/../logs/hbase-.....local.out
    ```

Verify that there is _one_ running process `HMaster`. In standalone mode HBase runs all three daemons (`HMaster`, `HRegionServer` and `ZooKeeper`) within the single JVM. 

`jps`
    ```
    80356 Jps
    80156 HMaster
    ```
HBase Web UI at http://localhost:16010 was not accessible

**Solved above warnings/error:**

In `~/.zshrc` commented 
#export PATH="$PATH:/Users/shouvik/opt/hadoop-3.3.1/bin"

Start HBase: `bin/start-hbase.sh`

    ```
    running master, logging to /Users/shouvik/opt/hbase-2.4.9/bin/../logs/hbase-.......local.out
    ```

`jps`
    81571 HMaster
    81732 Jps

HBase Web UI: http://localhost:16010 is now accessible => http://localhost:16010/master-status


**Connect to HBase**

Run HBase shell: 
    `hbase shell`
    Version 2.4.9, ........
    Took 0.0031 seconds                                                                                                          
    hbase:001:0> 

Works!!

**Exploring the HBase shell commands:**






