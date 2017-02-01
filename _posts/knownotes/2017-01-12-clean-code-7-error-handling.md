---
layout: post
title: "Clean Code: Chapter 7 - Error Handling"
excerpt: ""
categories: knownotes
tags: [SE]
comments: true
share: true
author: chungyu
---

> From Clean Code

##### Use Exceptions Rather Than Return Codes

##### Write Your Try-Catch-Finally Statement First
> This helps you define what the user of that code should expect, no matter what goes wrong with the code that is executed in the try.

* When you execute code in the try portion of a `try-catch-finally` statement, you are stating that execution can abort at any point and then resume at the `catch`.
  * `try` blocks are like transactions. Your `catch` has to leave your program in a consistent state, no matter what happens in the `try`.
* Try to write tests that force exceptions, and then add behavior to your handler to satisfy your tests.
  * This will cause you to build the transaction scope of the try block first and will help you maintain the transaction nature of that scope.

##### Use Unchecked Exceptions
* Checked exceptions can sometimes be useful if you are writing a critical library: You must catch them. But in general application development the dependency costs outweigh the benefits.
> Consider the calling hierarchy of a large system. Functions at the top call functions below them, which call more functions below them, ad infinitum. Now let’s say one of the lowest level functions is modified in such a way that it must throw an exception. If that exception is checked, then the function signature must add a throws clause. But this means that every function that calls our modified function must also be modified either to catch the new exception or to append the appropriate throws clause to its signature. Ad infinitum. The net result is a cascade of changes that work their way from the lowest levels of the software to the highest! Encapsulation is broken because all functions in the path of a throw must know about details of that low-level exception.

##### Provide Context with Exceptions
* Each exception that you throw should provide enough context to determine the source and location of an error.
  * a stack trace can’t tell you the intent of the operation that failed.

##### Define Exception Classes in Terms of a Caller’s Needs
> When we define exception classes in an application, our most important concern should be **how they are caught**.


* Wrapping third-party APIs is a best practice.
  * When you wrap a third-party API, you minimize your dependencies upon it: You can choose to move to a different library in the future without much penalty.

```java
//bad
ACMEPort port = new ACMEPort(12);
try {
  port.open();
} catch (DeviceResponseException e) {
  reportPortError(e);
  logger.log("Device response exception", e);
} catch (ATM1212UnlockedException e) {
  reportPortError(e);
  logger.log("Unlock exception", e);
} catch (GMXError e) {
  reportPortError(e);
  logger.log("Device response exception");
} finally {
  ...
}
```

```java
//much better

LocalPort port = new LocalPort(12);
try {
  port.open();
} catch (PortDeviceFailure e) {
  reportError(e);
  logger.log(e.getMessage(), e);
} finally {
  ...
}


public class LocalPort {
  private ACMEPort innerPort;
  public LocalPort(int portNumber) {
    innerPort = new ACMEPort(portNumber);
  }
  public void open() {
    try {
      innerPort.open();
    } catch (DeviceResponseException e) {
      throw new PortDeviceFailure(e);
    } catch (ATM1212UnlockedException e) {
      throw new PortDeviceFailure(e);
    } catch (GMXError e) {
      throw new PortDeviceFailure(e);
    }
  }
...
}  
```

##### Define the Normal Flow
* There are some times when you may not want to abort.
* **[TODO]** Need to dig in for the SPECIAL CASE PATTERN

##### Don’t Return Null
*  When we return null, we are essentially creating work for ourselves and foisting problems upon our callers.
  * All it takes is one missing null check to send an application spinning out of control.
* If you are tempted to return null from a method, consider throwing an exception or returning a SPECIAL CASE object instead.
* If you are calling a null-returning method from a third-party API, consider wrapping that method with a method that either throws an exception or returns a special case object.  

```java
//bad
List<Employee> employees = getEmployees();
if (employees != null) {
  for(Employee e : employees) {
    totalPay += e.getPay();
  }
}
```

```java
//better, emptyList is regarded as special case object example here!
List<Employee> employees = getEmployees();
for(Employee e : employees) {
  totalPay += e.getPay();
}


public List<Employee> getEmployees() {
  if( .. there are no employees .. )
    return Collections.emptyList();
}
```

##### Don’t Pass Null
* Returning null from methods is bad, but passing null into methods is worse.
* In most programming languages there is no good way to deal with a null that is passed by a caller accidentally. 
