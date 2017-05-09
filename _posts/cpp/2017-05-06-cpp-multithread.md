---
layout: post
title: "C++ multithreads"
excerpt: "join, detach"
categories: cpp
tags: [C++, multi-thread]
comments: true
share: true
author: chungyu
---
> * [cppreference](http://en.cppreference.com/w/cpp/thread/thread)
> * [cplusplus.com](http://www.cplusplus.com/reference/thread/thread)
> * [stackoverflow](http://stackoverflow.com/questions/22803600/when-should-i-use-stdthreaddetach)

# `join`

* Blocks the current thread until the thread identified by `*this` finishes its execution.
* The completion of the thread identified by `*this` synchronizes with the corresponding successful return from `join()`
* This synchronizes the moment this function returns with the completion of all the operations in the thread: This **blocks the execution of the thread that calls this function** until the function called on construction returns (if it hasn't yet).
* After a call to this function, the thread object becomes non-joinable and can be destroyed safely.

# `detach`
* Separates the thread of execution from the thread object, allowing execution to continue independently.
* Any allocated resources will be freed once the thread exits.
* After calling detach `*this` no longer owns any thread.
* Detaches the thread represented by **the object from the calling thread**, allowing them to execute independently from each other.
* Both threads continue without blocking nor synchronizing in any way. Note that **when either one ends execution, its resources are released.**
* After a call to this function, the thread object becomes non-joinable and can be destroyed safely.

# When to call `detach`
* In the destructor of `std::thread`, `std::terminate` is called if:
  * the thread was not joined (with `t.join()`) and was not detached either (with `t.detach()`)

> Thus, you should always either join or detach a thread before the flows of execution reaches the destructor.

* When a program terminates (ie, main returns) the remaining detached threads executing in the background are not waited upon; instead **their execution is suspended and their thread-local objects destructed.**

* Crucially, this means that the stack of those threads is not unwound and thus **some destructors are not executed.**
* Depending on the actions those destructors were supposed to undertake, this might be as bad a situation as if the program had crashed or had been killed. Hopefully the OS will release the locks on files, etc... but you could have corrupted shared memory, half-written files, and the like.
* So, should you use `join` or `detach` ? **Use `join`**
  * Unless you need to have more flexibility AND are willing to provide a synchronization mechanism to wait for the thread completion on your own, in which case you may use `detach`
* `detach` is mainly useful when you have a task that need to be done in background, **but you don't care about it's execution**. This is usually a case for some libraries. They may create a background worker thread and detach it so you won't even know that.

# Why `lock`
* Both processes and threads are independent sequences of execution.
* The typical difference is that threads (of the same process) run in a **shared memory space**, while processes run in separate memory spaces.
* To ensure safely read/write shared memory, we need lock (mutex).

# Why `lock_guard`, `unique_lock`
* If the function between `lock` and `unlock` throw exception, the `unlock` might not be called, and all the other threads are keep waiting for the `unlock`...
* The class `lock_guard` is a mutex wrapper that provides a convenient RAII-style mechanism for owning a mutex for the duration of a scoped block.
  * When a `lock_guard` object is created, it attempts to take ownership of the `mutex` it is given. When control leaves the scope in which the `lock_guard` object was created, the `lock_guard` is destructed and the mutex is released.

# Why conditional variable?
* The `condition_variable` class is a synchronization primitive that can be used to block a thread, **or multiple threads at the same time**, until another thread both modifies a shared variable (the condition), and notifies the `condition_variable`.
* The thread that intends to modify the variable has to
  * (1) acquire a `std::mutex` (typically via std::lock_guard)
  * (2) perform the modification while the lock is held
  * (3) execute `notify_one` or `notify_all` on the std::condition_variable

> **(the lock does not need to be held for notification)**

* Any thread that intends to wait on `condition_variable` has to
  * (1) acquire a `std::unique_lock<std::mutex>`, on the **same mutex** as used to protect the shared variable
  * (2) execute `wait`, `wait_for`, or `wait_until`. **The wait operations atomically release the mutex and suspend the execution of the thread. ==> THREAD IS NOT KEEP RUNNING IN THE WHILE LOOP**
* When the condition variable is notified, a timeout expires, or a spurious wakeup occurs, the thread is awakened, and the mutex is atomically reacquired. The thread should then check the condition and resume waiting if the wake up was spurious.

### Consumer-producer model
### Read Write lock
