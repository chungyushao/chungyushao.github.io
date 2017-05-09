---
layout: post
title: "D3: scales and axis"
excerpt: "scales, axis, group, transformation, ticks"
categories: frontend
tags: [d3]
comments: true
share: true
author: chungyu
---
> * [Interactive Data Visualization for the Web by Scott Murray](http://shop.oreilly.com/product/0636920026938.do?cmp=af-strata-books-videos-product_cj_9781449339739_%25zp)


# Scales
* Scales are functions that map from an input domain to an output range.
* D3 scales are functions with parameters that you define. Once they are created, you call the scale function* , pass it a data value, and it nicely returns a scaled output value. You can define and use as many scales as you like.
* Tick marks are part of an axis, which is a visual representation of a scale. A scale is a mathematical relationship, with no direct visual output.


###### Terminology
* Input! Domain!
* Output! Range!

```js
//Dummy example
var scale = d3.scale.linear()
                      .domain([100, 500])
                      .range([10, 350]);

scale(100); //Returns 10
scale(300); //Returns 180
scale(500); //Returns 350
```

###### Scaling the Scatterplot

```js
var dataset = [
	[5, 20], [480, 90], [250, 50], [100, 33], [330, 95],
	[410, 12], [475, 44], [25, 67], [85, 21], [220, 88]
];

var xScale = d3.scale.linear()
              .domain([0, d3.max(dataset, function(d) { return d[0]; })])
              .range([0, w]);
var yScale = d3.scale.linear()
              .domain([0, d3.max(dataset, function(d) { return d[1]; })])
              .range([0, h]);

// Then you can use
.attr("cx", function(d) { return xScale(d[0]); })
.attr("cy", function(d) { return yScale(d[1]); })
```

###### Refining the Plot

```js
//put the larger y on the top of the top
.range([h, 0]);
```


# Axis
* Much like its scales, D3’s axes are actually **functions** whose parameters you define.
* Unlike scales, when an axis function is called, it doesn’t return a value, but generates the **visual elements** of the axis, including lines, labels, and ticks.
* axis functions are **SVG-specific**, as they generate SVG elements.
* axes are intended for use with **quantitative scales** (as opposed to ordinal ones).
* Each axis also needs to be told on what scale to operate

```js
var xAxis = d3.svg.axis();
xAxis.scale(xScale);
```

* We can also specify where the labels should appear relative to the axis itself.
  * `xAxis.orient("bottom");`
  * horizontal axes are `top` and `bottom`. For vertical axes, use `left` and `right`:
* To actually generate the axis and insert all those little lines and labels into our SVG, we must call the xAxis function.

> axis function actually draws something to the screen (by appending SVG elements to the DOM), we need to specify **where in the DOM it should place those new elements.**

```js
svg //first reference svg, the SVG image in the DOM.
  .append("g") //append() a new g element to the end of the SVG.
  .attr("transform", "translate(0," + (h - padding) + ")")
  .call(xAxis);
  //hands off g to the xAxis function, so our axis is generated within g.
```

###### Concept: "g" group
* A `g` element is a group element.
* Group elements are invisible, unlike `line`, `rect`, and `circle`, and they have **no visual presence themselves**.
* `g` elements can be used to contain (or “group”) other elements, which keeps our code nice and tidy.
* We can apply **transformations** to `g` elements, which affects how visual element**s within that group** (such as lines, rects, and circles) are rendered.

###### Concept: `call`
* D3’s `call()` function takes the incoming selection and hands that selection off to any function.
* In this case, the selection is our new g group element.
* Although the g isn’t strictly necessary, we are using it because the axis function is about to generate lots of crazy lines and numbers, and it’s nice to contain all those elements within a single group object.

###### Concept: transformation
* By adding one line of code, we can transform the entire axis group
* Translation transforms are specified with the easy syntax of `translate(x,y)`, where `x` and `y` are, obviously, the number of horizontal and vertical **pixels by which to translate the element**.

### Adding style to axis
* The axes themselves are made up of `path`, `line`, and `text` elements, so those are the three elements that we target in our CSS.
* The paths and lines can be styled with the same rules, and text gets its own rules around font and font size.

```css
.axis path,
.axis line {
    fill: none;
    stroke: black;
    shape-rendering: crispEdges;
}
.axis text {
    font-family: sans-serif;
    font-size: 11px;
}
```

### Ticks
```js
var xAxis = d3.svg.axis()
              .scale(xScale)
              .orient("bottom")
              .ticks(5); //Set rough # of ticks
```
* D3 inteprets the `ticks()` value as merely a **suggestion** and will override your suggestion with what it determines to be the most clean and human-readable values

###### Formatting Tick Labels
* `tickFormat()`, which enables you to specify how your numbers should be formatted.

```js
// For example, you might want to include three places after the decimal point,
// or display values as percentages, or both.
var formatAsPercentage = d3.format(".1%");
xAxis.tickFormat(formatAsPercentage);
```


### Ordinal Scales
* Ordinal scales are typically used for ordinal data, typically categories with some inherent order to them
* `d3.scale.ordinal()` is it supports **range banding**.
  * Instead of returning a continuous range, as any quantitative scale (like `d3.scale.linear()`) would, ordinal scales use discrete ranges, meaning the output values are determined in advance, and could be numeric or not.
* We could use `range()` like everyone else, or we can be smooth and use `rangeBands()`
  * which takes a low and high value, and automatically divides it into even chunks or “bands,” based on the length of the domain.
* It is also possible to include a second parameter, which includes a bit of spacing between each band.

```js
.rangeBands([0, w], 0.2)
// meaning that 20 percent of the width of each band will be used for spacing
// in between bands:
```
* `rangeRoundBands()`: the same as range Bands(), except that the output values are rounded to the nearest whole pixel

Here, I’ve used 0.2,
