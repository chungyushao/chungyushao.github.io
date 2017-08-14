---
layout: post
title: "C++ Mutex in container"
excerpt: ""
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---
> * [stackoverflow](https://stackoverflow.com/questions/7557179/move-constructor-for-stdmutex)


# Scenario

* I am trying to write a base DS that has a mutex. The DS will be put into container and used by different thread. I don't want to simply lock the whole outside container, rather, I want to lock on the level of each element.


## First problem encounter: mutex don't have copy constructor

```cpp
//This code can not compile
#include <mutex>
#include <vector>
using namespace std;

struct MutexObj {
	mutex mutex_;
	int i;
};

int main() {
	vector<MutexObj> vec;
	vec.push_back(MutexObj());
}
```

* The code can't compile: `error: field of type 'std::__1::mutex' has private copy constructor`
  * Why?
> mutex is a short name of mutual exclusion i.e you want to make sure that, when there are multiple threads you dont want them to change/modify the value in parallel. You want to serialize the access or modification/read so that the value read is valid.

  * The construction of mutex is not atomic itself: Accessing the object while being constructed may initiate a data race.
