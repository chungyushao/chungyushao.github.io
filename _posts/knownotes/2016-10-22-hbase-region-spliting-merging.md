---
layout: post
title: "HBase region spliting and merging"
excerpt: "region spliting and merging"
categories: knownotes
tags: [hbase]
modified: 2016-10-22
comments: true
share: true
author: chungyu
---

> reference: [ article from Enis Soztutar](http://hortonworks.com/blog/apache-hbase-region-splitting-and-merging/)


# Background

* HBase stores rows of data in tables. 
* Tables are split into chunks of rows called “regions”. 
	* Those regions are distributed across the cluster, hosted and made available to client processes by the RegionServer process. 
* A region is a continuous range within the key space, meaning all rows in the table that sort between the region’s start key and end key are stored in the same region. 
* Regions are non-overlapping, i.e. a single row key belongs to exactly one region at any point in time. 
* A region is only served by a single region server at any point in time, which is how HBase guarantees strong consistency within a single row#.
* A Region in turn, consists of many “Stores”, which correspond to column families. 
* A store contains one memstore and zero or more store files. The data for each column family is stored and accessed separately.
* A table typically consists of many regions, which are in turn hosted by many region servers. 
	* Thus, regions are the physical mechanism used to distribute the write and query load across region servers.
* When a table is first created, HBase, by default, will allocate only one region for the table.
* This means that initially, all requests will go to a single region server, regardless of the number of region servers. 
* This is the primary reason why initial phases of loading data into an empty table cannot utilize the whole capacity of the cluster.


# The split mechanism

* As write requests are handled by the region server, they accumulate in an in-memory storage system called the “memstore”. 
* Once the memstore fills, its content are written to disk as additional store files. This event is called a **“memstore flush”**. 
* As store files accumulate, the RegionServer will **“compact”** them into combined, larger files. 
* After each flush or compaction finishes, a region split request is enqueued if the RegionSplitPolicy decides that the region should be split into two. 
	* Since all data files in HBase are immutable, when a split happens, the newly created daughter regions will not rewrite all the data into new files. 
	* Instead, they will create  small sym-link like files, named Reference files, which point to either top or bottom part of the parent store file according to the split point. 
	* The reference file will be used just like a regular data file, but only half of the records. 
	* The region can only be split if there are no more references to the immutable data files of the parent region. * Those reference files are cleaned gradually by compactions, so that the region will stop referring to its parents files, and can be split further.






# PRE-SPLITTING
* pre-splitting will ensure that the initial load is more evenly distributed throughout the cluster
* pre-splitting also has a risk of creating regions, that do not truly distribute the load evenly
* If the initial set of region split points is chosen poorly, you may end up with heterogeneous load distribution, which will in turn limit your clusters performance.



