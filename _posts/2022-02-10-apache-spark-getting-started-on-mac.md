---
layout: post
title:  "Apache Spark - getting started on Mac"
description: "Quick reference for getting started with Apache Spark" 
categories: [Apache]
tags: [Spark, Apache Spark]
date: 2022-02-10 06:15:09 +0530
---

Quick reference for running Apache Spark on Mac.

* Quick Start documentation: [https://spark.apache.org/docs/latest/quick-start.html](https://spark.apache.org/docs/latest/quick-start.html)
* RDD Programming Guide: [https://spark.apache.org/docs/latest/rdd-programming-guide.html](https://spark.apache.org/docs/latest/rdd-programming-guide.html)

* Download: https://spark.apache.org/downloads.html
* Note:
  - Spark uses Hadoop’s client libraries for HDFS and YARN. Downloads are pre-packaged with Hadoop versions. 
  - “Hadoop free” binary downloads are also available to run Spark with any Hadoop version by augmenting Spark’s classpath.

* Pre-requisites:
  - Java needs to be installed with PATH and JAVA_HOME set

    #Java configs in ~/.zshrc:
    `export PATH="$PATH:/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/bin"`
    `export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home/jre"`

* Extract and install: `tar -xvzf spark-3.2.0-bin-hadoop3.2.tgz`
* In `~/.zshrc` file include:

    `export SPARK_HOME="/Users/shouvik/opt/spark-3.2.0-bin-hadoop3.2"`
    `export PATH="$PATH:$SPARK_HOME/bin"`

* Following libraries are built-in: 
    - SQL and DataFrames
    - Spark Streaming
    - MLlib (machine learning)
    - GraphX (graph)

* Alternative pyspark install option: `$ pip install pyspark` 

* Verify which version of pyspark will run:`$ which pyspark`

  /Users/shouvik/opt/spark-3.2.1-bin-hadoop3.2/bin/pyspark

* Interactive Python Shell: `$ bin/pyspark`

* Run Spark Python Shell: `$ bin/pyspark`

  Python 3.7.7 (default, Mar 23 2020, 17:31:31) 
  [Clang 4.0.1 (tags/RELEASE_401/final)] :: Anaconda, Inc. on darwin
  ...
  ...
  Welcome to
        ____              __
      / __/__  ___ _____/ /__
      _\ \/ _ \/ _ `/ __/  '_/
    /__ / .__/\_,_/_/ /_/\_\   version 3.2.1
        /_/

  Using Python version 3.7.7 (default, Mar 23 2020 17:31:31)
  Spark context Web UI available at http://192.168.29.131:4040
  Spark context available as 'sc' (master = local[*], app id = local-1644452442329).
  SparkSession available as 'spark'.
  >>> 

* Additional installs:

  `% pip install pyspark.streaming.kafka`

* Submit Spark jobs using `bin/spark-submit`. Example application provided in Python

  `bin/spark-submit examples/src/main/python/pi.py 10`












