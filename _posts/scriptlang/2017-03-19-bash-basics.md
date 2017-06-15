---
layout: post
title: "Bash Scripts Basics"
excerpt: ""
categories: scriptlang
tags: [bash]
comments: true
share: true
author: chungyu
---
> [Bash Guide for Beginners by Machtelt Garrels](http://tldp.org/LDP/Bash-Beginners-Guide/html/)

# Data
###### Variable
* Normally, `var=123; var="start"` is okay for bash
* When you want to specify type of variables: `declare OPTION(s) VARIABLE=value`
* OPTIOINs:
  * `-a`: variable is Arrays
  * `-f`: function names
  * `-i`: integer
  * `-r`: variable become read only
  * ...
* Constants can also be declared by `readonly OPTION VARIABLE(s)`
* `${#VAR}` syntax will calculate the number of characters in a variable.

##### Array Variable
* Any variable may be used as an array.
* There is no maximum limit to the size of an array
* nor any requirement that member variables be indexed or assigned contiguously.
* `ARRAY[INDEXNR]=value` or `declare -a ARRAYNAME`
  * Attributes to the array may be specified using the `declare` and `readonly` built-ins
  * Attributes apply to all variables in the array
* `ARRAY=(one two ... three)`
* `echo ${ARRAY[*]}`: `one two three`
* `echo ${ARRAY[2]}`: `three`
* `ARRAY[3]=four`: becomes `(one two four)`
* `unset ARRAY[1]`: becomes `(one three four)`
* `unset ARRAY`
* `echo ${#ARRAY}`: `3`

# Operation
##### Setting default value
* `${VAR:-WORD}`: If `VAR` is not defined or null, make the value to be `WORD`
  * `[ -z "${COLUMNS:-}" ] && COLUMNS=80` is a shorter notation for

```bash
if [ -z "${COLUMNS:-}" ]; then
   COLUMNS=80
fi
```
* `${VAR1:=$VAR2}`: If `VAR` is not defined or null, make the value to be `$VAR2`

##### Removing substrings
* `${VAR:OFFSET[:LENGTH]}`: If `LENGTH` is omitted, the remainder of the variable content is taken

```bash
export STRING="chungtest"
echo ${STRING:5} #test
echo ${STRING:0:5} #chung
```
* `${VAR#PATTERN}` and `${VAR##PATTERN}`: deleting the pattern matching the expansion of `PATTERN` in `VAR`.
  * `#`: the shortest matching pattern
  * `##`: the longest matching pattern.

##### Replacing parts of variable names
* `${VAR/PATTERN/STRING}`: replaces only the first match
* `${VAR//PATTERN/STRING}`: replaces all matches of PATTERN with STRING



# Control
#### Sequential
#### Branch
#### Loop
# IO
