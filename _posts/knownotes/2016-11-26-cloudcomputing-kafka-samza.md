---
layout: post
title: "Apache Kafka, Samza"
excerpt: "Stream Processing"
categories: knownotes
tags: [cloud computing]
comments: true
share: true
author: chungyu
---

> mainly from
  * Cloud Computing writeup
  * [Apache Kafka: Next Generation Distributed Messaging System](https://www.infoq.com/articles/apache-kafka)
  * [Apache Samza, LinkedIn’s Framework for Stream Processing](http://thenewstack.io/apache-samza-linkedins-framework-for-stream-processing/)


# [Publish Subscribe Pattern](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern)
* publish–subscribe is a **messaging pattern**
  * Publish–subscribe is a sibling of the message queue paradigm
  * This pattern provides greater network scalability and a more dynamic network topology, with a resulting decreased flexibility to modify the publisher and the structure of the published data.
* publishers: senders of messages
* subscribers: receivers
  * publishers characterize published messages into classes without knowledge of which subscribers, if any, there may be.
  * subscribers express interest in one or more classes and only receive messages that are of interest, without knowledge of which publishers, if any, there are.

# Apache Kafka
* Kafka is a distributed publish-subscribe messaging system.
* You can think of Kafka as being a streaming data source.
* Kafka does not perform any processing by itself, it just provides a way to store and categorize a stream of data.

##### Terminology
* Producers
  * Producers are processes that publish messages to one or more topics in the Kafka cluster.

* Consumers
  * Consumers are processes that subscribe to topics and read messages from the Kafka cluster.

* Topics
  * A stream of messages of a particular type is defined as a topic.
    * A Message is defined as a payload of bytes and a Topic is a category or feed name to which messages are published (a user-defined category to which messages are published).
  * Topics are generally maintained as a partitioned log.

* Partitions
  * Topics are divided into partitions.
  * A partition represents the unit of parallelism in Kafka. In general, a higher number of partitions means higher throughput.
  * Within each partition each message has a specific offset that consumers use to keep track of how much they have progressed through the stream.

* Brokers
  * The published messages are stored at a set of servers called Brokers or Kafka Cluster.
  * Brokers are responsible for message persistence and replication.
  * Producers talk to brokers to publish messages to the Kafka cluster and consumers talk to brokers to consume messages from the Kafka cluster.

##### Delivery model
* Kafka supports both the point-to-point delivery model and publish-subscribe model.
* Point-to-point delivery model:
  * multiple consumers jointly consume a single copy of message in a queue
* Publish-subscribe model
  * multiple consumers retrieve its own copy of the message.

##### Kafka Storage
* Each partition of a topic corresponds to a **logical log**.
  * Physically, a log is implemented as a set of **segment files of equal sizes**.
* Every time a producer publishes a message to a partition, the broker simply appends the message to the last segment file.
* Segment file is flushed to disk after configurable numbers of messages have been published or after a certain amount of time elapsed.
* Messages are exposed to consumer **after it gets flushed**.
  * Messages are exposed by the logical offset in the log.
* Consumer always consumes messages **from a particular partition sequentially**
  * if the consumer acknowledges particular message offset, it implies that the consumer has consumed all prior messages.
* Consumer issues asynchronous pull request to the broker to have a buffer of bytes ready to consume.
  * Each asynchronous pull request contains the offset of the message to consume.

##### Kafka Broker
* A message is automatically deleted if it has been retained in the broker longer than a certain period.
  * It is very tricky to delete message from the broker as broker doesn't know whether consumer consumed the message or not.
  * Consumer can deliberately rewind back to an old offset and re-consume data.
    * (This violates the common contract of a queue)
