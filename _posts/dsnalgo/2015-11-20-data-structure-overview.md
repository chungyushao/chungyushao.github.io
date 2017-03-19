---
layout: post
title: "Data Structure Overview"
excerpt: ""
categories: dsnalgo
tags: [data structure]
comments: true
share: true
author: chungyu
---

> From

|  Data Structure | Advantage  | Disadvantage|Java Library|
|---              |         ---|          ---||
| Array |  Quick insertion|Slow search||
||   very fast access if index known. |slow deletion||
||                                    |fixed size||
|Ordered array| Quicker search than unsorted array|Slow insertion and deletion||
|||fixed size.||
|Stack|last-in, first-out access|Slow access to other items||
|Queue|first-in, first-out access|Slow access to other items||
|Linked list|Quick insertion|Slow search||
||quick deletion|||
|Binary tree|Quick search, insertion|Deletion algorithm is complex||
||qucik deletion (if tree remains balanced)|||
|Red-black tree|Quick search, insertion, deletion|Complex||
||Tree always balanced.|||
|2-3-4 tree|Quick search, insertion, deletion.|Complex||
||Tree always balanced.|||
||Similar trees good for disk storage.|||
|Hash table|Very fast access if key known.|Slow deletion| `HashMap<K, V> A = new HashMap<K,V>()`;|
|||access slow if key not known|`HashSet<V> A =new HashSet<V>();`|
|||inefficient memory usage.||
|Heap|Fast insertion, deletion, access to largest item.|Slow access to other items||
|Graph|Models real-world situations|Some algorithms are slow and complex.|||
