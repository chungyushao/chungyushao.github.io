---
layout: post
title: "HBase Column Attribute"
excerpt: "Compression,  Data Block Encoding"
categories: knownotes
tags: [hbase]
modified: 2016-10-22
comments: true
share: true
author: chungyu
---

> reference: [ Official Documents](https://archive.cloudera.com/cdh5/cdh/5/hbase-0.98.6-cdh5.2.0/book/compression.html)

# Data block encoding
* attempts to limit duplication of information in keys, taking advantage of some of the fundamental designs and patterns of HBase, such as sorted row keys and the schema of a given table.
* **Prefix**:
	* If you have long keys (compared to the values) or many columns, use a prefix encoder.
* **Diff**: 
	* Diff encoding is disabled by default because writing and scanning are slower but more data is cached.
* **Fast Diff**:
	* Fast Diff works similar to Diff, but uses a faster implementation.
	* Fast Diff is the recommended codec to use if you have long keys or many columns.
* **Prefix Tree**:
	* provides faster random access at a cost of slower encoding speed.
	* FAST_DIFF is recommended, as more testing is needed for Prefix Tree encoding.

# Compressors
> If the values are large (and not precompressed, such as images), use a data block compressor.

* reduce the size of large, opaque byte arrays in cells, and can significantly reduce the storage space needed to store uncompressed data.
* In most cases, enabling Snappy or LZO by default is a good choice, because they have a low performance overhead and provide space savings.
	* none
	* Snappy
		* Snappy or LZO for hot data, which is accessed frequently. 
	* LZO
	* LZ4
	* GZ
		* GZIP for cold data, which is accessed infrequently.




# Enabling Compression on a ColumnFamily of an Existing Table using HBase Shell
```bash
hbase> disable 'test'
hbase> alter 'test', {NAME => 'cf', COMPRESSION => 'GZ'}
hbase> enable 'test'
```
* Changes Take Effect Upon Compaction. If you change compression or encoding for a ColumnFamily, the changes take effect during compaction.