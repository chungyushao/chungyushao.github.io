---
layout: post
title: "`pickle`, Python object serialization"
excerpt: "Series"
categories: knownotes
tags: [python, serialization]
comments: true
share: true
author: chungyu
---

> mainly from
> * [pickle — Python object serialization](https://docs.python.org/2.7/library/pickle.html#data-stream-format)

> * Wiki: serialization is the process of translating data structures or object state into a format that can be stored and reconstructed later in the same or another computer environment.
> * It is very useful when you want to transmit one object data across the network


#### Data stream format: Protocols

The data format used by `pickle` is Python-specific.
  * This has the advantage that there are no restrictions imposed by external standards such as XDR (which can’t represent pointer sharing);
  * however it means that non-Python programs may not be able to reconstruct pickled Python objects.

Protocol
* Protocol version 0 is the original ASCII protocol and is backwards compatible with earlier versions of Python.
* Protocol version 1 is the old binary format which is also compatible with earlier versions of Python.
* Protocol version 2 was introduced in Python 2.3. It provides much more efficient pickling of new-style classes.


#### Usage
`pickle.dump(obj, file[, protocol])`:
* Write a pickled representation of obj to the open file object file. This is equivalent to `Pickler(file, protocol).dump(obj)`.

`pickle.dumps(obj[, protocol])`:
Return the pickled representation of the object as a string, instead of writing it to a file.


`pickle.load(file)`:
* Read a string from the open file object file and interpret it as a pickle data stream, reconstructing and returning the original object hierarchy. This is equivalent to `Unpickler(file).load()`.

`pickle.loads(string)`:
Read a pickled object hierarchy from a string. Characters in the string past the pickled object’s representation are ignored.
