---
layout: post
title:  "Apache Hadoop - getting started on Mac"
description: "Quick reference for getting started with Apache Hadoop on a Mac" 
categories: [Tools]
tags: [Hadoop, Apache Hadoop, Install Hadoop on a Mac]
date: 2022-01-30 15:39:09 +0530
---

Quick reference for getting started with Apache Hadoop on a Mac.

**Pre-requisites:**

* `$ssh localhost` should work without a passphrase
* Java should be installed
    * Install Java on Mac: [https://www.java.com/en/download/help/mac_install.html](https://www.java.com/en/download/help/mac_install.html)
    * finding JAVA_HOME: `$java -XshowSettings:properties -version`

```
...
java.home = /Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre
...
...
java version "1.8.0_121"
Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)
```

* Update Java PATH and JAVA_HOME in `.zshrc` file

```
export PATH="$PATH:/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/bin"
export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre"
```


Reference document: [https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html)

**Installation and Configuration:**


Download Hadoop from here: [https://www.apache.org/dyn/closer.cgi/hadoop/common/](https://www.apache.org/dyn/closer.cgi/hadoop/common/)

Install Hadoop: `$tar -xvzf hadoop-3.3.1.tar.gz`

Installation / Extraction directory: `/Users/shouvik/opt/kafka_2.13-3.0.0`

Below commands are executed from the location: `/Users/shouvik/opt/kafka_2.13-3.0.0`

Verify Hadoop has been installed properly `$ bin/hadoop`

**Test for Standalone Operation:**

(Following the steps in the reference document)

```
  $ mkdir input
  $ cp etc/hadoop/*.xml input
  $ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.1.jar grep input output 'dfs[a-z.]+'
  $ cat output/*
  ```
**Configure for Pseudo-Distributed Operation:**
* Update `etc/hadoop/core-site.xml`

```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

* Update `etc/hadoop/hdfs-site.xml`

```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

* Format the filesystem

`$ bin/hdfs namenode -format`

* Start NameNode daemon and DataNode daemons

`$ sbin/start-dfs.sh`

* Check in browser whether `http://localhost:9870/` is accessible. This is the Namenode view.

* Create HDFS directories

`$ bin/hdfs dfs -mkdir /user`

`$ bin/hdfs dfs -mkdir /user/shouvik`

All MapReduce files will be available under `/user/shouvik` in HDFS

Run a sample MapReduce job (using the getting started guide)

`$ bin/hdfs dfs -mkdir input`

`$ bin/hdfs dfs -put etc/hadoop/*.xml input` (copy from local file system to HDFS)

`$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.1.jar grep input output 'dfs[a-z.]+'` (run the job)

`$ bin/hdfs dfs -cat output/*` (check the output of the job)

`$ bin/hdfs dfs -get output output` (to copy the file from HDFS to local file system)


**Some useful notes:**

* To check the running processes :

`$jps`
```
23105 NameNode
23205 DataNode
23339 SecondaryNameNode
32126 Jps
```

* YARN need not be configured for running the MapReduce jobs


























