---
layout: post
title: "Undertow tuning"
excerpt: "specific tunning for the performance boost"
categories: devnote
tags: [undertow, server]
modified: 2016-10-23
comments: true
share: true
author: chungyu
---

Observer the usage of your server `htop`, if the CPU hasn't been fully utilized, you can...
```java
server = Undertow.builder()
        .addHttpListener(80, "0.0.0.0")
        .setHandler(path)
        .setIoThreads(64)
        .setWorkerThreads(100)
        .setDirectBuffers(true)
        .build();
```

> * Mainly from [Entering Undertow Web server](https://www.javacodegeeks.com/2014/01/entering-undertow-web-server.html)

> Other reference
* [reference 2](http://undertow.io/undertow-docs/undertow-docs-1.2.0/listeners.html)
* [reference 3](http://undertow.io/javadoc/1.2.x/io/undertow/Undertow.Builder.html#setDirectBuffers-boolean-)


# Buffer Caches
> * A Buffer is essentially a block of memory into which you can write data, which you can then later read again. 
> * Buffer memory access is much faster than physical access.

# Task Keepalive
> * The Task keepalive (default 60) controls the number of seconds to wait for the next request from the same client on the same connection. 
> * With Keep-Alives the browser gets to eliminate a full round trip for every request after the first, usually cutting full page load times in half.

# IO Threads
> * The number of Io threads corresponds to the number of Web server Threads available.
> * The Task Max Threads has control on the max number of concurrent requests.

# Buffer Pool
> * Undertow is based on the Java NIO API 
> 	* In the NIO library, all data is handled with Buffers. When data is read, it is read directly into a buffer. When data is written, it is written into a buffer.

# Direct buffers
> * direct buffers are allocated outside the Java heap; therefore, once allocated, their memory address is fixed for the lifetime of the buffer. 
> * Having a fixed memory address in turn causes that the kernel can safely access them directly and, hence, direct buffers can be used more efficiently in I/O operations

# Buffer size
> * Provided that direct buffers are being used, the default 16kb buffers are optimal if maximum performance is required

