---
layout: post
title: "C++ Special Lock"
excerpt: "std::defer_lock, std::try_to_lock, std::adopt_lock, for std::lock_guard, std::unique_lock and std::shared_lock."
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---
> * [cppreference](http://en.cppreference.com/w/cpp/thread/lock_tag)
> * [medium post](https://medium.com/@asitdhal/c-11-locking-strategy-adopt-lock-and-defer-lock-eeedf76a2689)

* `std::defer_lock`, `std::try_to_lock` and `std::adopt_lock` are instances of empty struct tag types `std::defer_lock_t`, `std::try_to_lock_t` and `std::adopt_lock_t` respectively.
* **They are used to specify locking strategies for `std::lock_guard`, `std::unique_lock` and `std::shared_lock`.**


# `std::adopt_lock` and `std::defer_lock`
In some situations, the thread needs to hold two locks simultaneously and release them after accessing the shared data. When a thread has more than one lock, there is a chance of deadlock.
* To avoid this C++11 provides `std::adopt_lock` and `std::defer_lock`


### `std::adopt_lock`
* std::adopt_lock assumes that **the calling thread already owns the lock**. The wrapper should adopt the ownership of the mutex and **release it when control goes out of scope.**

```cpp
std::lock(m1, m2); // calling thread locks the mutex
std::lock_guard<std::mutex> lock1(m1, std::adopt_lock);    
std::lock_guard<std::mutex> lock2(m2, std::adopt_lock);
// access shared data protected by the m1 and m2
```

### `std::defer_lock`
* `std::defer_lock` doesn’t acquire ownership of the mutex and assumes that the calling thread is going to call lock to acquire the mutex. The wrapper is going to release the lock when control goes out of scope.

```cpp
std::unique_lock<std::mutex> lock1(fromAcc->m, std::defer_lock);
std::unique_lock<std::mutex> lock2(toAcc->m, std::defer_lock);
std::lock(lock1, lock2);
// access shared data protected by the m1 and m2
```

### difference between `std::lock_guard` and `std::unique_lock`
* The above two code snippets has same effect, but they are using different orders.
* `std::lock_guard` with `std::adopt_lock` strategy assumes the mutex is already acquired.
	* Pros: `std::lock_guard` is cheaper than std::unique_lock.
	* Cons: if you forget one of the lock_guard statements, compiler will not show any error, but there will be deadlock.

```cpp
std::lock(m1, m2); // calling thread locks the mutex
std::lock_guard<std::mutex> lock1(m1, std::adopt_lock);    
std::lock_guard<std::mutex> lock2(m2, std::adopt_lock);
// access shared data protected by the m1 and m2
```
	* two `lock_guard` instances are constructed, one for each mutex
	* `std::adopt_lock` parameter specifies that mutexes are already locked, and they should just adopt the ownership  of the existin lock on the mutex rather than attempt to lock the mutex in the `std::lock_guard` constructor.

* `std::unique_lock` with `std::defer_lock` strategy assumes the mutex is not acquired on construction, rather than explicitly going to be locked.
	* Pros: compiler will catch the error if you forget to define one of the unique_lock statements.

```cpp
std::unique_lock<std::mutex> lock1(fromAcc->m, std::defer_lock);
std::unique_lock<std::mutex> lock2(toAcc->m, std::defer_lock);
std::lock(lock1, lock2);
// access shared data protected by the m1 and m2
```
	* `std::unique_lock` doesn't always own the mutex, it's associated with.
	* The 2nd argument `std::defer_lock` indiactes that mutex should remain unlocked on construction.
	* The lock can then be acquired later by calling `lock()` on `std::unique_lock` object.


# List of `std::unique_lock` and strategy
* `unique_lock( mutex_type& m, std::defer_lock_t t ) noexcept;`
	* Does not lock the associated mutex.
* `unique_lock( mutex_type& m, std::try_to_lock_t t );`
	* Tries to lock the associated mutex without blocking by calling m.try_lock(). The behavior is undefined if the current thread already owns the mutex except when the mutex is recursive.
* `unique_lock( mutex_type& m, std::adopt_lock_t t );`
	* Assumes the calling thread already owns m.
* Check code here: [cppreference](http://en.cppreference.com/w/cpp/thread/unique_lock/unique_lock)
