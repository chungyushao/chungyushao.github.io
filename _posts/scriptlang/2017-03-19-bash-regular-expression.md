---
layout: post
title: "Bash Scripts Regex"
excerpt: ""
categories: scriptlang
tags: [bash,regular expression]
comments: true
share: true
author: chungyu
---
> [A Brief Introduction to Regular Expressions](http://www.tldp.org/LDP/abs/html/x17129.html)

# Elements of regex
A Regular Expression contains one or more of the following:
* A character set: The characters retaining their literal meaning.
* An anchor: These designate (anchor) the position in the line of text that the RE is to match. For example, `^,` and `$` are anchors.
* Modifiers: These expand or narrow (modify) the range of text the RE is to match. Modifiers include the asterisk `*`, brackets `[...]`, and the backslash `\`.


# Anchor
* `*`: matches any number of repeats of the character string or RE preceding it, including zero instances.
* `.`: matches any one character, except a newline.

# Modifiers
* `^`: matches the beginning of a line, but sometimes, depending on context, negates the meaning of a set of characters in an RE.
* `$`: at the end of an RE matches the end of a line.
  * `"^$"`: matches blank lines.
* `"[...]"`: enclose a set of characters to match in a single RE.   
* `"[^b-d]"`: matches any character except those in the range b to d.
* `\`:  scapes a special character, which means that character gets interpreted literally
* Escaped "angle brackets" -- `"\<...\>"`: mark word boundaries.

# Extended REs
* Additional metacharacters added to the basic set. Used in `egrep`, `awk`, and `Perl`.
* `?`: matches zero or one of the previous RE. It is generally used for matching single characters.
* `+`: matches one or more of the previous RE. It serves a role similar to the `*`, but does not match zero occurrences.
* `()`: enclose a group of REs.
* `|`: "or" RE operator matches any of a set of alternate characters.
* Escaped "curly brackets" -- `"\{ \}"`: indicate the number of occurrences of a preceding RE to match.
  * `"[0-9]\{5\}"` matches exactly five digits

> * Curly brackets are not available as an RE in the "classic" (non-POSIX compliant) version of `awk`. However, the GNU extended version of awk, `gawk`, has the `--re-interval` option that permits them (without being escaped).
> * `Perl` and some `egrep` versions do not require escaping the curly brackets.
