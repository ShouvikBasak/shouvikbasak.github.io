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
        "____              __  "
      "/ __/__  ___ _____/ /__  "
      "_\ \/ _ \/ _ `/ __/  '_/  "
    "/__ / .__/\_,_/_/ /_/\_\   version 3.2.1  "
       " /_/  "

  Using Python version 3.7.7 (default, Mar 23 2020 17:31:31)
  Spark context Web UI available at http://192.168.29.131:4040
  Spark context available as 'sc' (master = local[*], app id = local-1644452442329).
  SparkSession available as 'spark'.
  >>> 

* Additional installs:

  `% pip install pyspark.streaming.kafka`

* Submit Spark jobs using `bin/spark-submit`. Example application provided in Python

  `bin/spark-submit examples/src/main/python/pi.py 10`

### Launching Spark Cluster in Standalone Deploy Mode

* Start Standalone Master: `$ sbin/start-master.sh`

  % sbin/start-master.sh
  starting org.apache.spark.deploy.master.Master, logging to /Users/shouvik/opt/spark-3.2.1-bin-hadoop3.2/logs/spark-shouvik-org.apache.spark.deploy.master.Master-1-Shouviks-MacBook-Pro.local.out

  - Check log file: `/Users/shouvik/opt/spark-3.2.1-bin-hadoop3.2/logs/spark-shouvik-org.apache.spark.deploy.master.Master-1-Shouviks-MacBook-Pro.local.out` and note the following

    INFO Utils: Successfully started service 'sparkMaster' on port 7077.
    INFO Master: Starting Spark master at spark://Shouviks-MacBook-Pro.local:7077
    INFO Master: Running Spark version 3.2.1
    INFO Utils: Successfully started service 'MasterUI' on port 8080.
    INFO MasterWebUI: Bound MasterWebUI to 0.0.0.0, and started at http://192.168.29.131:8080
    INFO Master: I have been elected leader! New state: ALIVE

* Master Web UI: `http://localhost:8080` (default)

* From the Master Web UI note the `master-spark-URL` required for starting `Worker instances` for :spark://Shouviks-MacBook-Pro.local:7077

* Start one or more Workers: `sbin/start-worker.sh <master-spark-URL>`
  - Get the `<master-spark-URL>` from the information provided when starting the Master or from the Master Web UI

    `$ sbin/start-worker.sh spark://Shouviks-MacBook-Pro.local:7077`
    starting org.apache.spark.deploy.worker.Worker, logging to /Users/shouvik/opt/spark-3.2.1-bin-hadoop3.2/logs/spark-shouvik-org.apache.spark.deploy.worker.Worker-1-Shouviks-MacBook-Pro.local.out

  - Check the log `/Users/shouvik/opt/spark-3.2.1-bin-hadoop3.2/logs/spark-shouvik-org.apache.spark.deploy.worker.Worker-1-Shouviks-MacBook-Pro.local.out`

    INFO Utils: Successfully started service 'sparkWorker' on port 51194.
    INFO Worker: Starting Spark worker 192.168.29.131:51194 with 4 cores, 7.0 GiB RAM
    INFO Worker: Running Spark version 3.2.1
    INFO Worker: Spark home: /Users/shouvik/opt/spark-3.2.1-bin-hadoop3.2
    INFO Utils: Successfully started service 'WorkerUI' on port 8081.
    INFO WorkerWebUI: Bound WorkerWebUI to 0.0.0.0, and started at http://192.168.29.131:8081
    INFO Worker: Connecting to master Shouviks-MacBook-Pro.local:7077...
    INFO TransportClientFactory: Successfully created connection to Shouviks-MacBook-Pro.local/127.0.0.1:7077 after 56 ms (0 ms spent in bootstraps)
    INFO Worker: Successfully registered with master spark://Shouviks-MacBook-Pro.local:7077

  - Check the Worker Web UI: `http://192.168.29.131:8081`
  - Verify the Master Web UI: Worker instance should be listed in `Workers`

* Note:
  - `conf/workers` needs to be created in Spark directory containing the host names of all the Spark worker nodes.
  - If `conf/workers` does not exist, the launch scripts defaults to a single machine (localhost).
  - Master machine accesses each of the worker machines via ssh. Hence communication between teh Master and Worker nodes should work password-less SSH configuration.

* Quick set of commands:
  - Start a Master: `sbin/start-master.sh`
  - Start a Worker instance: `sbin/start-worker.sh`
  - Stop a Master: `sbin/stop-master.sh`
  - Stop all Worker instances: `sbin/stop-worker.sh`

* Reference guide for running Spark in Standalone Mode: [https://spark.apache.org/docs/latest/spark-standalone.html](https://spark.apache.org/docs/latest/spark-standalone.html)





