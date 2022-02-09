---
layout: post
title:  "Apache HBase - getting started on Mac"
description: "Quick reference for getting started with Apache HBase on a Mac" 
categories: [Apache]
tags: [HBase, Apache HBase]
date: 2022-02-09 08:15:09 +0530
---

Quick reference for a Standalone HBase installation on a Mac.

Apache HBase Reference Guide: [https://hbase.apache.org/book.html](https://hbase.apache.org/book.html)

**Prerequisite:** JDK needs to be installed

**Download Apache HBase:**
[https://hbase.apache.org/downloads.html](https://hbase.apache.org/downloads.html) => (download the bin for the stable version) = > download from [https://www.apache.org/dyn/closer.lua/hbase/2.4.9/hbase-2.4.9-bin.tar.gz](https://www.apache.org/dyn/closer.lua/hbase/2.4.9/hbase-2.4.9-bin.tar.gz)

**Extract and install:** `tar -xvzf hbase-2.4.9-bin.tar.gz`

## Standalone mode using local filesystem

A standalone instance of HBase has all the HBase daemons included:
* Master
* RegionServers
* ZooKeeper 

All the 3 daemons run in a single JVM persisting to the local filesystem.

**Configure:**

Check Java_HOME: `echo $JAVA_HOME`

    `$/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre`

Update `conf/hbase-env.sh` in HBase installation directory with:

    export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre

Update ~/.zshrc file:

    export PATH="$PATH:</install path>/hbase-2.4.9/bin"


**Start HBase:** `start-hbase.sh`

Received the following warnings:


    SLF4J: Class path contains multiple SLF4J bindings.
    SLF4J: Found binding in [jar:file:/...../hadoop-3.3.1/share/hadoop/common/lib/slf4j-log4j12-1.7.30.jar!/org/slf4j/impl/StaticLoggerBinder.class]
    SLF4J: Found binding in [jar:file:/...../hbase-2.4.9/lib/client-facing-thirdparty/slf4j-log4j12-1.7.30.jar!/org/slf4j/impl/StaticLoggerBinder.class]
    SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
    SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
    running master, logging to /..../hbase-2.4.9/bin/../logs/hbase-.....local.out

Verify that there is _one_ running process `HMaster`. In standalone mode HBase runs all three daemons (`HMaster`, `HRegionServer` and `ZooKeeper`) within the single JVM. 

`jps`

    80356 Jps
    80156 HMaster

HBase Web UI at http://localhost:16010 was not accessible

**Solved above warnings/error:**

In `~/.zshrc` commented 
#export PATH="$PATH:/Users/shouvik/opt/hadoop-3.3.1/bin"

Start HBase: `bin/start-hbase.sh`

The warning is gone!

    ...
    running master, logging to /Users/shouvik/opt/hbase-2.4.9/bin/../logs/hbase-.......local.out
    ...


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

Quick commands to get started from the reference guide.

Create table - must provide one column family along with table name

hbase:003:0> `create 'test', 'cf'`

    Created table test
    Took 1.2568 seconds                                                                                                          
    => Hbase::Table - test
    hbase:004:0> list 'test'
    TABLE                                                                                                                        
    test                                                                                                                         
    1 row(s)
    Took 0.0358 seconds                                                                                                          
    => ["test"]

hbase:005:0> `describe 'test'`

    Table test is ENABLED                                                                                                        
    test                                                                                                                         
    COLUMN FAMILIES DESCRIPTION                                                                                                  
    {NAME => 'cf', BLOOMFILTER => 'ROW', IN_MEMORY => 'false', VERSIONS => '1', KEEP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODIN
    G => 'NONE', COMPRESSION => 'NONE', TTL => 'FOREVER', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLIC
    ATION_SCOPE => '0'}                                                                                                          

    1 row(s)
    Quota is disabled
    Took 0.1286 seconds                                                                                                          
    hbase:006:0> put 'test', 'row1', 'cf:a', 'value1'
    Took 0.1017 seconds                                                                                                          
    hbase:007:0> put 'test', 'row2', 'cf:b', 'value2'
    Took 0.0069 seconds                                                                                                          
    hbase:008:0> put 'test', 'row3', 'cf:c', 'value3'
    Took 0.0150 seconds                                                                                                          


hbase:009:0> `scan 'test'`

    ROW                              COLUMN+CELL                                                                                 
    row1                            column=cf:a, timestamp=2022-02-09T15:53:39.281, value=value1                                
    row2                            column=cf:b, timestamp=2022-02-09T15:54:02.022, value=value2                                
    row3                            column=cf:c, timestamp=2022-02-09T15:54:14.169, value=value3                                
    3 row(s)
    Took 0.0546 seconds                                                                                                          
    hbase:010:0> get 'test', 'row1'
    COLUMN                           CELL                                                                                        
    cf:a                            timestamp=2022-02-09T15:53:39.281, value=value1                                             
    1 row(s)
    Took 0.0136 seconds                                                                                                          

hbase:011:0> `disable 'test'`

    Took 0.3587 seconds                                                                                                          

hbase:012:0> `list 'test'`

    TABLE                                                                                                                        
    test                                                                                                                         
    1 row(s)
    Took 0.0049 seconds                                                                                                          
    => ["test"]

hbase:013:0> `put 'test', 'row4', 'cf:d', 'value4'`

    ERROR: Table test is disabled!

    For usage try 'help "put"'

    Took 0.4446 seconds                                                                                                          

hbase:014:0> `enable 'test'`

    Took 0.6702 seconds                                                                                                          
    hbase:015:0> put 'test', 'row4', 'cf:d', 'value4'
    Took 0.0094 seconds                                                                                                          
    hbase:016:0> scan 'test'
    ROW                              COLUMN+CELL                                                                                 
    row1                            column=cf:a, timestamp=2022-02-09T15:53:39.281, value=value1                                
    row2                            column=cf:b, timestamp=2022-02-09T15:54:02.022, value=value2                                
    row3                            column=cf:c, timestamp=2022-02-09T15:54:14.169, value=value3                                
    row4                            column=cf:d, timestamp=2022-02-09T15:56:33.403, value=value4                                
    4 row(s)
    Took 0.0160 seconds                                                                                                          

hbase:017:0> `disable 'test'`

    Took 0.3560 seconds                                                                                                          
    hbase:018:0> drop test
    Traceback (most recent call last):
    ArgumentError (wrong number of arguments (given 0, expected 2..3))

hbase:019:0> `drop 'test'`

    Took 0.1380 seconds                                                                                                          

hbase:020:0> `list`

    TABLE                                                                                                                        
    0 row(s)
    Took 0.0116 seconds                                                                                                          
    => []


## Standalone mode with HBase over HDFS

**To run in standalone mode and use HDFS instead of local filesystem**

Check `jps` to check nothing is running. Stop HBase if running.

`stop-hbase.sh`

`stopping hbase..............

`jps`

    86473 Jps

To configure this standalone variant, edit your hbase-site.xml setting hbase.rootdir to point at a directory in your HDFS instance but then set hbase.cluster.distributed to false. 

Update `conf/hbase-site.xml`

    <property>
        <name>hbase.cluster.distributed</name>
        <value>false</value>
    </property>

    <property>
        <name>hbase.rootdir</name>
        <value>hdfs://localhost:9000/hbase</value>
    </property>

    # Remove existing configuration for hbase.tmp.dir and hbase.unsafe.stream.capability.enforce  

    <property>
        <name>hbase.tmp.dir</name>
        <value>./tmp</value>
    </property>
    
    <property>
        <name>hbase.unsafe.stream.capability.enforce</name>
        <value>false</value>
    </property>


**Start Hadoop**

`sbin/start-dfs.sh`

    Starting namenodes on [localhost]
    Starting datanodes
    Starting secondary namenodes [Shouviks-MacBook-Pro.local]

`jps`

    87649 NameNode
    87761 DataNode
    87991 Jps
    87898 SecondaryNameNode

Verify [http://localhost:9870/dfshealth.html#tab-overview](http://localhost:9870/dfshealth.html#tab-overview)

**Start HBase**

`bin/start-hbase.sh`

    The authenticity of host '127.0.0.1 (127.0.0.1)' can't be established.
    ECDSA key fingerprint is SHA256:.....
    Are you sure you want to continue connecting (yes/no/[fingerprint])? y
    Please type 'yes', 'no' or the fingerprint: yes
    127.0.0.1: Warning: Permanently added '127.0.0.1' (ECDSA) to the list of known hosts.
    127.0.0.1: running zookeeper, logging to /..../opt/hbase-2.4.9/bin/../logs/hbase-shouvik-zookeeper-....local.out
    running master, logging to /..../opt/hbase-2.4.9/bin/../logs/hbase-shouvik-master-....local.out
    : running regionserver, logging to /..../opt/hbase-2.4.9/bin/../logs/hbase-shouvik-....local.out

**Issue faced:**

`jps`

    91968 HQuorumPeer
    92208 Jps
    91715 SecondaryNameNode
    92132 HRegionServer
    91574 DataNode
    91470 NameNode

jps command should show the HMaster and HRegionServer processes running. HMaster was not running

Solution:

Cleaned up Hadoop configuration - stopped dfs, deleted /tmp/hadoop-dir, name node format, start hadoop

`bin/start-hbase.sh`

    127.0.0.1: running zookeeper, logging to /..../opt/hbase-2.4.9/bin/../logs/hbase-shouvik-zookeeper-.....local.out
    running master, logging to /..../opt/hbase-2.4.9/bin/../logs/hbase-shouvik-master-.....local.out
    : running regionserver, logging to /..../opt/hbase-2.4.9/bin/../logs/hbase-shouvik-regionserver-......local.out

`jps`

    4017 SecondaryNameNode
    4482 HRegionServer
    4388 HMaster
    3879 DataNode
    4312 HQuorumPeer
    4541 Jps
    3773 NameNode

HMaster and HRegionServer processes are both running after the cleanup






























