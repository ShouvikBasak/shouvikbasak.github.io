---
layout: post
title:  "Kafka notes"
description: "Quick reference for getting started with Kafka" 
categories: [Apache Tools]
tags: [Kafka, Apache Kafka]
date: 2022-01-22 10:15:09 +0530
---

Kafka Quick Start: https://kafka.apache.org/quickstart

**Quick commands:**

Start Kafka:

`$ bin/zookeeper-server-start.sh config/zookeeper.properties`
`$ bin/kafka-server-start.sh config/server.properties`

Create a topic:

`$ bin/kafka-topics.sh --create --partitions 1 --replication-factor 1 --topic quickstart-events --bootstrap-server localhost:9092`

Note: 
* In Kafka 3.0.0 –bootstrap-server localhost:9092 has replaced –zookeeper localhost:2181 
`$ bin/kafka-topics.sh --zookeeper localhost:2181 --create --topic first_topic --replication-factor 1 --partitions 3`(this does not work)

* instead use:
`$ bin/kafka-topics.sh --create --topic first_topic --replication-factor 1 --partitions 3 --bootstrap-server localhost:9092`


List topics:
`bin/kafka-topics.sh --list --bootstrap-server localhost:9092`
`bin/kafka-topics.sh --list --bootstrap-server localhost:9092`
Note: 
`kafka-topics.sh --list --zookeeper localhost:2181` (this does not work)


View topic details:

`$ bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092`


Write events into the topic:

`$ bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092`
`$ bin/kafka-console-producer.sh --topic first_topic --broker-list localhost:9092`

Read the events:

`$ bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092`
`$ bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic first_topic --from-beginning`

To delete any data of local Kafka environment including any events:

`$ rm -rf /tmp/kafka-logs /tmp/zookeeper`

To delete a topic:

`$ bin/kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic first_topic`
