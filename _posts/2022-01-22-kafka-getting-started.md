---
layout: post
title:  "Kafka - getting started"
description: "Quick reference for getting started with Kafka" 
categories: [Tools]
tags: [Kafka, Apache Kafka]
date: 2022-01-22 10:15:09 +0530
---

Quick notes and reference commands to get started with Kafka on a local machine.

Getting started => [Apache Kafka Quick Start](https://kafka.apache.org/quickstart)

**Important files:**
* `config/zookeeper.properties`
* `config/server.properties`

**Quick commands:**

Start Kafka:

* Start zookeeper server first: `$ bin/zookeeper-server-start.sh config/zookeeper.properties`
* Start Kafka server next: `$ bin/kafka-server-start.sh config/server.properties`

Create a topic:

Topic name: mytopic1, Number of partitions of the topic: 3, Replication Factor: 1

`$ bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic mytopic1 --partitions 3 --replication-factor 1`

Note: 
* In Kafka 3.0.0 `bootstrap-server localhost:9092` has replaced `zookeeper localhost:2181` for the command syntax. Hence, this won't work: `$ bin/kafka-topics.sh --zookeeper localhost:2181 --create --topic mytopic1 --partitions 3 --replication-factor 1`
* Kafka bootstrap-server runs on port 9092 by default

List topics:

`$ bin/kafka-topics.sh --list --bootstrap-server localhost:9092`

View topic details:

`$ bin/kafka-topics.sh --describe --topic mytopic1 --bootstrap-server localhost:9092`

Write events into the topic:

`$ bin/kafka-console-producer.sh --topic mytopic1 --bootstrap-server localhost:9092`

Read the events:

`$ bin/kafka-console-consumer.sh --topic mytopic1 --bootstrap-server localhost:9092` (read current incoming events but not from beginning of the queue)

`$ bin/kafka-console-consumer.sh --topic mytopic1 --from-beginning --bootstrap-server localhost:9092` (read all events from beginning of the queue)


To delete a topic:

`$ bin/kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic mytopic1`

To delete any data of local Kafka environment including any events:

`$ rm -rf /tmp/kafka-logs /tmp/zookeeper`

To stop Kafka:

`$ bin/kafka-server-stop.sh`

`$ bin/zookeeper-server-stop.sh`


Kafka core APIs:

* Producer API: For an application to publish a stream of records to one or more Kafka topics.
* Consumer API: For an application to subscribe to one or more topics.
* Streams API: For an application to act as a *stream processor*. 
    * Consumes from an input stream from one or more topics.
    * Produces an output stream to one or more output topics.
    * Helps to transform input streams to output streams.
* Connector API: For connecting Kafka topics to existing applications.

Installing kafka-python: `pip install kafka-python`












