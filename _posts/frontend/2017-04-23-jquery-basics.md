---
layout: post
title: "JQuery Basics 1"
excerpt: ""
categories: frontend
tags: [jquery]
comments: true
share: true
author: chungyu
---
> [learn.jquery.com](http://learn.jquery.com/about-jquery/how-jquery-works/)

# `$`

* The jQuery library exposes its methods and properties via two properties of the `window` object called `jQuery` and `$`.
  * `$` is simply an alias for jQuery and it's often employed because it's shorter and faster to write.

# Ready event  
* To run code as soon as the document is ready to be manipulated, jQuery has a statement known as the ready even

```js
$( document ).ready(function() {

    // Your code here.

});
```

# Callbacks and Functions
* JavaScript enables you to freely pass functions around **to be executed at a later time**.
* A callback is **a function that is passed as an argument to another function and is executed after its parent function has completed.**
* Callbacks are special because they patiently **wait to execute until their parent finishes.**  
  * Meanwhile, the browser can be executing other functions or doing all sorts of other work.

###### Callback without Arguments

* `$.get( "myhtmlpage.html", myCallBack ); `

###### Callback with arguments

* `$.get( "myhtmlpage.html", myCallBack( param1, param2 ) );`: Wrong way
  * `myCallBack( param1, param2 )` immediately and then passes `myCallBack()`'s return value as the second parameter to `$.get()`
* Right way:

```js
$.get( "myhtmlpage.html", function() {

    myCallBack( param1, param2 );

});
```
  * anonymous function does exactly one thing: calls `myCallBack()`, with the values of `param1` and `param2`
  * When `$.get()` finishes getting the page myhtmlpage.html, it executes the anonymous function, which executes `myCallBack( param1, param2 )`.

# `$.fnc1()` v.s. `$(xxx).fnc2()`

* `$(xxx).fnc2()`: a method can be called on a jQuery selection
  * Most jQuery methods are called on **jQuery objects** as `$(xxx)`;
  * these methods are said to be part of the `$.fn` namespace, or the "jQuery prototype," and are best thought of as **jQuery object methods**.
  * automatically receive and return the selection as `this`.
* `$.fnc1()`: a method that isn't called on a selection
  * However, there are several methods that do not act on a selection;
  * these methods are said to be part of the `jQuery` namespace, and are best thought of as core **jQuery methods**.
  * they are not automatically passed any arguments, and their return value will vary.
