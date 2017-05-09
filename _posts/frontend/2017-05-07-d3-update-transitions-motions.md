---
layout: post
title: "D3: update, transitions, motions"
excerpt: ""
categories: frontend
tags: [d3]
comments: true
share: true
author: chungyu
---
> * [Interactive Data Visualization for the Web by Scott Murray](http://shop.oreilly.com/product/0636920026938.do?cmp=af-strata-books-videos-product_cj_9781449339739_%25zp)

# Intro
* you might want your visualization to reflect data that almost always changes over time.
* In D3 terms, those changes are handled by `updates`.
* The visual adjustments are made pretty with `transitions`, which can employ motion for perceptual benefit.


# `update`

* To make sure we can observe the change as it happens, we will separate our update code from everything else.
* We will need a “trigger,” something that happens after page load to apply the updates.

### Interaction via Event Listeners
* In JavaScript, events are happening all the time.
* If someone is listening, then the event will be heard, and can go down in posterity, or at least trigger some sort of DOM interaction.

```js
<p>Update</p>

d3.select("p")
.on("click", function() {
//...
});
```


# `transition`
* Without transition(), D3 evaluates every attr() statement immediately, so the changes in height and fill happen right away. When you add transition(), D3 introduces the element of time.
*  D3 interpolates between the old values and the new values, meaning it normalizes the beginning and ending values, and calculates all their in-between states.
  * interpolated over time: default is 250 milliseconds
  * you can control how much time is spent on any transition by `.duration(milliseconds)`

# `ease`
* The quality of motion used for a transition is called **easing**.
* In animation terms, we think about elements easing into place, moving from here to there.
* With D3, you can specify different kinds of easing by using `ease()`.


# `enter`
* When we changed our data values, but not the length of the whole data set, we didn’t have to worry about an update selection—we simply rebound the data, and transitioned to new attribute values.
* The genius of the `update` selection is that it contains within it references to `enter` and `exit` subselections.
  * Entering elements are those that are new to the scene.
* Whenever there are more data values than corresponding DOM elements, the `enter` selection contains references to those elements that do not yet exist. 
