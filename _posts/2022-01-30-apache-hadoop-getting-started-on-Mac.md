---
layout: post
title:  "Apache Hadoop - getting started on Mac"
description: "Quick reference for getting started with Apache Hadoop on a Mac" 
categories: [Data Engineering]
tags: [Hadoop, Apache Hadoop, Install Hadoop on a Mac]
date: 2022-01-30 15:39:09 +0530
---

Quick reference for getting started with Apache Hadoop on a Mac.

**Pre-requisites:**

* `$ssh localhost` should work without a passphrase. If it does not work follow the below steps.

```
  $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  $ chmod 0600 ~/.ssh/authorized_keys
```
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

Installation / Extraction directory: `/Users/shouvik/opt/hadoop-3.3.1`

Below commands are executed from the location: `/Users/shouvik/opt/hadoop-3.3.1`

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

`$ jps`

    3075 DataNode
    2964 NameNode
    3303 Jps
    3214 SecondaryNameNode

* Check in browser whether `http://localhost:9870/` is accessible. This is the Namenode view.

* Create HDFS directories

`$ bin/hdfs dfs -mkdir /user`

`$ bin/hdfs dfs -mkdir /user/shouvik`

All MapReduce files will be available under `/user/shouvik` in HDFS

Run a sample MapReduce job (using the getting started guide)

`$ bin/hdfs dfs -mkdir input` (make a directory)

`$ bin/hdfs dfs -ls` (to check the listings in home directory)
`$ bin/hdfs dfs -ls /` (to check the listings from root)


`$ bin/hdfs dfs -put etc/hadoop/*.xml input` (copy from local file system to HDFS)

`$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.1.jar grep input output 'dfs[a-z.]+'` (run the job)

`$ bin/hdfs dfs -ls output` (check the output file name)

`$ bin/hdfs dfs -cat output/<output_file_name>` (check the output of the job)

`$ bin/hdfs dfs -get output output` (to copy the file from HDFS to local file system)


**Some useful notes and commands:**
* YARN need not be configured for running the MapReduce jobs

* To check the running processes:

`$ jps`
```
23105 NameNode
23205 DataNode
23339 SecondaryNameNode
32126 Jps
```
* Some handy commands:
`$ hdfs dfs -put file.txt mydata/myfolder` (transfer a file to HDFS)
`$ hdfs dfs -rm mydata/myfolder/file.txt` (delete a file in HDFS)
`$ hdfs dfs -cat mydata/myfolder/file.txt` (read a file in HDFS)
`$ hdfs dfs -copyFromLocal file.txt mydata/myfolder/file.txt` (copy a file from local file ystem to HDFS)
`$ hdfs dfs -copyToLocal mydata/myfolder/file.txt .` (copy a file from HDFS to local filesytem)

* HDFS Command Guide: (https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HDFSCommands.html)[https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HDFSCommands.html]

* Hadoop Streaming Guide: (https://hadoop.apache.org/docs/r1.2.1/streaming.html)[https://hadoop.apache.org/docs/r1.2.1/streaming.html]
    * Used for running non-java code for MapReduce programs like Python. Use the following command:
    
































