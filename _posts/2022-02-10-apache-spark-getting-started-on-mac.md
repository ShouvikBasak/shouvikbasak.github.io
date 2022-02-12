---
layout: post
title:  "Apache Spark - getting started on Mac"
description: "Quick reference for getting started with Apache Spark" 
categories: [Apache]
tags: [Spark, Apache Spark]
date: 2022-02-10 06:15:09 +0530
---

Quick reference for running Apache Spark on Mac.

Please note that these are working notes for quick reference.

* Quick Start documentation: [https://spark.apache.org/docs/latest/quick-start.html](https://spark.apache.org/docs/latest/quick-start.html)
* RDD Programming Guide: [https://spark.apache.org/docs/latest/rdd-programming-guide.html](https://spark.apache.org/docs/latest/rdd-programming-guide.html)

* Download: [https://spark.apache.org/downloads.html](https://spark.apache.org/downloads.html)
  - Older versions of Spark can be downloaded from [https://archive.apache.org/dist/spark/](https://archive.apache.org/dist/spark/)
* Note:
  - Spark uses Hadoop’s client libraries for HDFS and YARN. Downloads are pre-packaged with Hadoop versions. 
  - “Hadoop free” binary downloads are also available to run Spark with any Hadoop version by augmenting Spark’s classpath.

* Pre-requisite:
  - JDK (recommended JDK 8 or JDK 11) needs to be installed with JAVA_HOME set
  - Check existing installation of Java and java_home `$ usr/libexec/java_home`

      `/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home` as an example

  - Remove any old version of JDK if required by removing the directory and the associated paths in configuration files, if any
  - Install JDK 11
    - Gets installed at: `/Library/Java/JavaVirtualMachines/jdk-11.0.14.jdk/Contents/Home` (Java 11)  
                         `/Library/java/JavaVirtualMachines/jdk1.8.0_321.jdk/Contents/Home` (Java 8)
  - Update ~/.zshrc:

      `export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk-11.0.14.jdk/Contents/Home"`  (Java 11)  
      `export JAVA_HOME="/Library/java/JavaVirtualMachines/jdk1.8.0_321.jdk/Contents/Home"` (Java 8)

  - `$ java -version` (Java 11)

        java version "11.0.14" 2022-01-18 LTS
        Java(TM) SE Runtime Environment 18.9 (build 11.0.14+8-LTS-263)
        Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.14+8-LTS-263, mixed mode)

 - `$ java -version` (Java 8)

        java version "1.8.0_321"
        Java(TM) SE Runtime Environment (build 1.8.0_321-b07)
        Java HotSpot(TM) 64-Bit Server VM (build 25.321-b07, mixed mode)


* Extract and install: `tar -xvzf spark-3.2.0-bin-hadoop3.2.tgz`
* In `~/.zshrc` file include:

    `export SPARK_HOME="/Users/shouvik/opt/spark-3.2.0-bin-hadoop3.2"`  
    `export PATH="$PATH:$SPARK_HOME/bin"`  
    `export PYSPARK_PYTHON=python3`

* Following libraries are built-in: 
    - SQL and DataFrames
    - Spark Streaming
    - MLlib (machine learning)
    - GraphX (graph)

* Alternative pyspark install option: `$ pip install pyspark`

* Verify which version of pyspark will run:`$ which pyspark`

    /Users/shouvik/opt/spark-3.2.1-bin-hadoop3.2/bin/pyspark

* Run Spark Python Shell: `$ bin/pyspark`

  Python 3.7.7 (default, Mar 23 2020, 17:31:31) 
  [Clang 4.0.1 (tags/RELEASE_401/final)] :: Anaconda, Inc. on darwin
  ...
  ...
  Welcome to Spark version 3.2.1  
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

* Starts a Spark Cluster without a third-party cluster manager (like YARN for example)

* Start Standalone Master: `$ sbin/start-master.sh`

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

    `$ sbin/start-slave.sh spark://Shouviks-MacBook-Pro.local:7077` (for Spark 2.4.6) 

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
  - Start a Worker instance: `sbin/start-worker.sh <master-spark-URL>`
  - Stop a Master: `sbin/stop-master.sh`
  - Stop all Worker instances: `sbin/stop-worker.sh`

* Reference guide for running Spark in Standalone Mode: [https://spark.apache.org/docs/latest/spark-standalone.html](https://spark.apache.org/docs/latest/spark-standalone.html)


* For running Spark from Jupyter notebook
  - `pip install findspark`
  - In Jupyter Notebooks use
      `import findspark`  
      `findspark.init()`  


### To use Spark 2.4.6

Follow above process

**Note:**
* Spark 2.4.6 worked with JDK8 and Python 3.7.12
* Spark 2.4.6 is not compatible with 3.9 (or possibly 3.7 and above)

* Create Anaconda environment with Python 3.7
/opt/anaconda3> `conda create -n env_python3.7 python=3.7`

environment location: `/opt/anaconda3/envs/env_python3.7`

Activate environment: `conda activate env_python3.7`
To deactivate an active environment: `conda deactivate`

`python --version`  
Python 3.7.12

`which pyspark`  
/Users/shouvik/opt/spark-2.4.6-bin-hadoop2.6/bin/pyspark

`jupyter notebook` runs  
Open a jupyter notebook and run in a shell:  
`!python --version`  
Output: Python 3.7.12  

By default opening Terminal:  
`python --version`
Python 3.9.7  
(Default Anaconda env)  

Activate environment: `conda activate env_python3.7`

`conda activate env_python3.7`                    

`pyspark`
Python 3.7.12 | packaged by conda-forge | (default, Oct 26 2021, 05:59:23) 
[Clang 11.1.0 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
....
....
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Welcome to Spark version 2.4.6
Using Python version 3.7.12 (default, Oct 26 2021 05:59:23)
SparkSession available as 'spark'.
>>> 

No errors observed




