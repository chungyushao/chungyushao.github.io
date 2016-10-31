---
layout: post
title: "Bloom Filter in HBase"
excerpt: "Bloom Filter intro"
categories: knownotes
tags: [hbase]
comments: true
share: true
author: chungyu
---

> [Julian Dontcheff's Database Blog](https://juliandontcheff.wordpress.com/2012/08/28/bloom-filters-for-dbas/)

A Bloom filter is a probabilistic algorithm for doing **existence tests** in less memory than a full list of keys would require. In other words, a Bloom filter is a method for representing a set of n elements (also called keys) to support membership queries.

> [Commiter's response in quora](https://www.quora.com/How-are-bloom-filters-used-in-HBase)

* When a HFile is opened, typically when a region is deployed to a RegionServer, the bloom filter is loaded into memory and used to determine if a given key is in that store file. They can be scoped on a row key or column key level, where the latter needs more space as it has to store many more keys compared to just using the row keys
* Keep in mind that HBase only has a block index per file, which is rather coarse grained and tells the reader that a key *may* be in the file because it falls into a start and end key range in the block index. But if the key is actually present can only be determined by loading that block and scanning it.
*  If you are doing Row scans, then Row+Col doesn't buy you anything.  **A row bloom can filter on a Row+Col get, but not the other way around.**  However, the problem with Row blooms is that you may have a ton of column-level puts and end up with a scenario where a Row is present in every StoreFile.  Then the bloom filter is wasteful because it's always positive.
* Since the reads are in parallel, you don't see much perf gain on an individual get, which is dominated by disk read latency.  If later, we allow a serial get option (beneficial for data that is overwritten), then you'll see bloom filters have immediate benefit on read latency.

> [hbase scan and bloom filter](http://xitong.iteye.com/blog/1797420)

* scan是一个范围，如果是row的bloomfilter不命中只能说明该rowkey不在此storefile中，但next rowkey可能在。而rowcol的bloomfilter就不一样了，如果rowcol的bloomfilter没有命中表明该qualifiy不在这个storefile中，因此这次scan就不需要scan此storefile了！
