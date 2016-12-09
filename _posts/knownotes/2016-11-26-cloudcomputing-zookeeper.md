---
layout: post
title: "Apache Zookeeper"
excerpt: "Stream Processing"
categories: knownotes
tags: [cloud computing]
comments: true
share: true
author: chungyu
---

> mainly from
  * [Apache Kafka: Next Generation Distributed Messaging System](https://www.infoq.com/articles/apache-kafka)
  *

# Problem
* One common problem with all these distributed systems is **how would you determine which servers are alive and operating at any given point of time?**
  * Most importantly, how would you do these things reliably in the face of the difficulties of distributed computing such as network failures, bandwidth limitations, variable latency connections, security concerns, and anything else that can go wrong in a networked environment, perhaps even across multiple data centers?
* These types of questions are the focus of Apache ZooKeeper

# ZooKeeper
> a distributed, hierarchical file system that facilitates loose coupling between clients and provides an eventually consistent view of its znodes, which are like files and directories in a traditional file system.

* It provides basic operations such as creating, deleting, and checking existence of znodes.
* It provides an event-driven model in which clients can watch for changes to specific znodes, for example if a new child is added to an existing znode.
* ZooKeeper achieves high availability by running multiple ZooKeeper servers, called an **ensemble**, with each server holding an in-memory copy of the distributed file system to service client read requests.
  * Typical ZooKeeper ensemble have one server acting as a leader while the rest are followers.
  * On start of ensemble leader is elected first and all followers replicate their state with leader.
  * All write requests are routed through leader and changes are broadcast to all followers.
    * Change broadcast is termed as atomic broadcast.
![from ref 1]({{site.url}}/images/cc/zookeeper-ensambles.png)  
