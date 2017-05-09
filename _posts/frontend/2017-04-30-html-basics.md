---
layout: post
title: "HTML CSS basics"
excerpt: ""
categories: frontend
tags: [htmlcss]
comments: true
share: true
author: chungyu
---
> * [Interactive Data Visualization for the Web](http://shop.oreilly.com/product/0636920026938.do?cmp=af-strata-books-videos-product_cj_9781449339739_%25zp)


# Concepts

* HTML is a tool for specifying **semantic structure**, or attaching hierarchy, relationships, and meaning to content.
* HTML doesn’t address the visual representation of a document’s structure—that’s CSS’ job.


# Marking up
* “Marking up” is the process of adding tags to create elements.
* Tags usually occur in pairs, in which case adding an opening and closing pair of tags creates a new element in the document structure.
* Some tags never occur in pairs, such as the img element, which references an image file.
  * Although HTML no longer requires it, you will sometimes see such tags written in self-closing fashion, with a trailing slash before the closing bracket: `<img src="photo.jpg" />`
  * As of HTML5, the self-closing slash is optional: `<img src="photo.jpg">`
* Some unfamiliar tags:
  * `span`: An arbitrary span of text, typically within a larger containing element like `p`.
  * `div`: An arbitrary division within the document. Used for grouping and containing related elements.

# Attributes
* All HTML elements can be assigned attributes by including property/value pairs in the opening tag `<tagname property="value"></tagname>`
* `class` and `id` are extremely useful attributes, as they can be referenced later to identify specific pieces of content.
  * Your CSS and JavaScript code will rely heavily on classes and IDs to identify elements.
  * Class and ID names cannot begin with numerals; they **must begin with alphabetic characters**

```html
<p class="uplifting">Brilliant paragraph</p>
<p class="uplifting">Insightful paragraph</p>
<p class="uplifting awesome">Awe-inspiring paragraph</p>
```
* all three paragraphs are uplifting, but only the last one is both uplifting and awesome.

```html
<div id="content">
  <div id="visualization"></div>
  <div id="button"></div>
</div>
```
* There can be **only one** `id` per element, and each ID value can be used only once on the page.
* [thread about not unique id](http://stackoverflow.com/questions/5611963/can-multiple-different-html-elements-have-the-same-id-if-theyre-different-eleme)
  * Should IDs be unique? YES.
    *
  * Must IDs be unique? NO, at least IE and FireFox allow multiple elements to have the same ID.

> As a general rule, if there will be only one such element on the page, you can use an `id`. Otherwise, use a `class`.

# Rendering and the Box Model
* Rendering is the process by which browsers, after parsing the HTML and generating the DOM, apply visual rules to the DOM contents and draw those pixels to the screen.
* The most important thing to keep in mind when considering how browsers render content is this: **to a browser, everything is a box.**
  * Paragraphs, divs, spans—all are boxes in the sense that they are two-dimensional rectangles, with properties that any rectangle can have, such as width, height, and positions in space.
  * You can see these boxes with the help of the web inspector.


# CSS
* CSS styles consist of `selectors` and `properties`. Selectors are followed by properties, grouped in curly brackets.
* The same properties can be applied to multiple selectors at once by separating the selectors with a comma
* why are they called Cascading Style Sheets? It’s because selector matches cascade from the top down. When more than one selector applies to an element, the later rule generally overrides the earlier one

```css

selectorA, selectorB, selectorC {
        property: value;
        property: value;
        property: value;
}
```
* Selectors identify specific elements to which styles will be applied.
* Descendant selectors: These match elements that are contained by (or “descended from”) another element.
  * `h1 em`: Selects `em` elements contained in an `h1`
  * `div p`: Selects `p` elements contained in a `div`
* Class selectors
  * `.caption`: Selects elements with class "caption"
  * Because elements can have more than one class, you can target elements with multiple classes by stringing the classes together
    * `.bar.highlight`: Could target highlighted bars
    * `.axis.x`: Could target an x-axis
    * `.axis.y`: Could target a y-axis
      * `.axis` could be used to apply styles to both axes
* ID selectors
  * These match the single element with a given ID. (Remember, IDs can be used only once each in the DOM.)
  * IDs are preceded with a `#` mark
    * `#header`: Selects element with ID "header"
  * You can string selectors together to get very specific results.  
    * `div.sidebar`: Selects `divs` with class `sidebar`, but not other elements with that class
    * `#button.on`: Selects element with ID `button`, but only when the class `on` is applied

> Because the DOM is dynamic, classes and IDs can be added and removed, so you might have CSS rules that apply only in certain scenarios.    

###### CSS overrides
* Later rules generally override earlier ones, but not always.
* The true logic has to do with the **specificity** of each selector.
  * If two selectors have the same specificity, then the later one will be applied.

> To save yourself headaches later, keep your selectors clear and easy to read. Start with general selectors on top, and work your way down to more specific ones
