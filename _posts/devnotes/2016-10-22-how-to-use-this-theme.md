---
layout: post
title: "Quick notes for using the so-simple theme"
excerpt: "as titile!"
categories: devnote
tags: [jekyll]
modified: 2016-10-22
comments: true
share: true
author: chungyu
---

## 1. Don't modify the files under `_site`, it's self-generated.

## 2. How to add the navigation bar:

* Add entry in `{home}/_data/navigation.yml`
* Put the cooresponding folder under `{home}/_posts/new_category`
* Put the index.md folder under `{home}/new_category`
  * modify `... site.categories.{new_category}`

## 3. How to modify author properties:

* Add entry in `{home}/_data/authors.yml`
