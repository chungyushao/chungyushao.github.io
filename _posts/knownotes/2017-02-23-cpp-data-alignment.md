---
layout: post
title: "C++ Data Alignment"
excerpt: ""
categories: knownotes
tags: [C++]
comments: true
share: true
author: chungyu
---

> * [cppreference.com](http://en.cppreference.com/w/cpp/language/object)
> * [Coding for Performance: Data alignment and structures By Sumedh N. (Intel)](https://software.intel.com/en-us/articles/coding-for-performance-data-alignment-and-structures)
> * TOREAD:  [geeksforgeeks](http://www.geeksforgeeks.org/structure-member-alignment-padding-and-data-packing/)

# Alignment Requirement
Every data type has an alignment associated with it which is mandated by the processor architecture rather than the language itself. Aligning data elements allows the processor to fetch data from memory in an efficient manner and thereby improves performance. The compiler tries to maintain these alignments for data elements to provide optimal performance.

* Every object type has the property called `alignment requirement`, which is an integer value (of type `std::size_t`, always a power of 2) **representing the number of bytes between successive addresses at which objects of this type can be allocated**.
* The alignment requirement of a type can be queried with alignof or `std::alignment_of`.
* The pointer alignment function `std::align` can be used to obtain a suitably-aligned pointer within some buffer, and `std::aligned_storage` can be used to obtain suitably-aligned storage.
* Each object type imposes its alignment requirement on every object of that type
  * In order to satisfy alignment requirements of all non-static members of a class, **padding** may be inserted after some of its members.
  * Padding: For structures that generally contain data elements of different types, the compiler tries to maintain proper alignment of data elements by inserting unused memory between elements.
  * The compiler may also increase the size of structure, if necessary, to make it a multiple of the alignment by adding padding at the end of the structure. This is known as 'Tail Padding'.
* The compiler aligns the entire structure to its most strictly aligned member.  
* stricter alignment (with larger alignment requirement) can be requested using `alignas`.
* The weakest alignment (the smallest alignment requirement) is the alignment of `char`, `signed char`, and `unsigned char`, which equals 1
* the largest fundamental alignment of any type is the alignment of `std::max_align_t`.

# Padding in 64-bit machines

Data type | Size of Type | Padding Bytes
--- | ---
char | 1 | 1
short | 2 | 2
int | 4 | 4
float | 4 | 4
double | 8 | 8
long | 8 | 8
long long | 8 | 8
any type's pointer | 8 | 8
long double | 16 | 16

# BKM: Minimizing memory wastage
* One can try to minimize this memory wastage by ordering the structure elements such that the **widest (largest) element comes first, followed by the second widest, and so on**.

```cpp
// Size: 32
struct s1 {
  char a;
  short a1;
  char b1;
  float b;
  int c;
  char e;
  double f;
};
```

```cpp
// Size: 24
struct s2 {
  double f;
  float b;
  int c;
  short a1;
  char a, b1, e;
};
```
###### `S1`: Size is 32 (Waste 11 bytes for padding)
inclusive address | contents
--- | ---
0 | `char a`
1 | 1 byte padding to make `short a1` on the alignment of 2 bytes
2-3 | `short a1`
4 | `char b1`
5-7 | 3 bytes padding to make `float b` on the alignment of 4 bytes
8-11 | `float b`
12-15 | `int c`
16 | `char e`
17-23 | 7 bytes padding to make `double f` on the alignment of 8 bytes
24-31 | `double f`

###### `S2`: Size is 24 (Waste only three bytes)
inclusive address | contents
--- | ---
0-7 | `double f`
8-11 | `float b`
12-15 | `int c`
16-17 | `short a1`
18 | `char a`
19 | `char b1`
20 | `char e`
21-23 | padding to make the struct on the 4 bytes of alignment

* An exception to this ordering of elements in structures is if your structures are bigger than a cache line, which is 64 bytes in case of Intel Xeon Phi coprocessor, and some loops or kernels touch only a part of the structure. In this case, it may be beneficial to keep the parts of the structure that are touched together in memory, which may lead to improved cache locality.

* Your structures are larger than a cache line with some loops or kernels touching only a part of the structure then you may consider reorganizing the data by splitting the large structure into multiple smaller structures which are stored as separate arrays. This potentially improves the density of touched data and thereby improves cache locality.

* You can also use the `__declpsec(align(num_bytes))` attribute to direct the compiler to align data more strictly than it otherwise would.

```cpp
__declspec(align(32)) struct S3 {
  char a2;
  int b2;
};

struct S4 {
  char a1;
  struct S3 s3;
};
```
CAN'T COMPILE ????
