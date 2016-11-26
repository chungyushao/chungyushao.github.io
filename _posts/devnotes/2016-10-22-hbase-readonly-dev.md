---
layout: post
title: "Read Only HBase Developments Notes"
excerpt: "HBase 1.2.2 on EMR 5.0.0"
categories: devnote
tags: [hbase]
modified: 2016-10-22
comments: true
share: false
author: chungyu
---

# Where can I find the hbase monitor of EMR?
* not in the original port 60000 or 60010, instead
	* [EMR HBase Configuration: ](http://docs.aws.amazon.com/ElasticMapReduce/latest/ReleaseGuide/emr-hbase-configure.html)
	* HMaster UI: port `16010` using HTTP


# Load/requests balance is extremly important
![Split region in Web UI]({{site.url }}/images/hbase-webui-split.png)


# MapReduce Scan Caching

* [official reference](https://archive.cloudera.com/cdh5/cdh/5/hbase-0.98.6-cdh5.2.0/book/ch07s02.html)
* [reference threads by @b4hand](http://stackoverflow.com/questions/22528859/hbase-scan-performance)

##### `Scan.setCaching`
* `setCaching` actually specifies how many rows will be transmitted per RPC to the regionserver.
* `setCaching(1)`: every time you call `next()` you pay the cost of a round-trip to the regionserver.
* `setCaching(someLargeNum)`: you pay for extra memory in the client, and potentially, you are fetching rows that you won't use, for example, if you stop scanning after reaching a certain number of rows or after you've found a specific value.

##### `Scan.setCacheBlocks`
* `setBlockCache`: basically instructs the regionserver to not pull any data from this Scan into the HBase BlockCache which is a separate pool of memory from the MemStore
* MemStores are used for writing and **BlockCache is used for reading**, and these two pieces of memory are completely separate.
* HBase currently does not use the BlockCache as a write-back cache. You can control the size of the block cache with the hfile.block.cache.size config setting in hbase-site.xml.
* Similarly you can control the total pool size of the MemStore via the hbase.regionserver.global.memstore.size setting.
* You might want to use `setCacheBlocks(false)` if you are doing a full table scan, and you don't want to flush your current working set in the block cache. Otherwise, if you are scanning data that is being used frequently, it would probably be better to leave the setBlockCache alone.
* when you manually set `setCacheBlocks(false)` then , it will stop caching the rows it reads from hdfs.
* **`setCaching` affects client-side behavior while `setCacheBlocks` affects regionserver-side behavior**

# Scan with `startkey`, `endkey`

* endkey is excluseive, so the trick is not adding the value to the endkey, instead

```java
String exclusiveEndkey = endkey + "0";
//or
String exclusiveEndkey = endkey + "~";
```
# If you are doing Row scans, then Row+Col (Bloomfilter) doesn't buy you anything.
