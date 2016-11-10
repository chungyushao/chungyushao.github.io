---
layout: post
title: "Python Regular Expression"
excerpt: "sampling distribution"
categories: knownotes
tags: [python, regular expression]
comments: true
share: true
author: chungyu
---
> Mainly from
* [Python Document: 7.2. re — Regular expression operations](https://docs.python.org/2/library/re.html)
* [Regular Expression HOWTO](https://docs.python.org/2/howto/regex.html#regex-howto)
* [Regular Expression Info](http://www.regular-expressions.info/)


## [What is regular expression?](https://en.wikipedia.org/wiki/Regular_expression)
> a regular expression (sometimes called a rational expression) is a sequence of characters that define a search pattern, mainly for use in pattern matching with strings, or string matching, i.e. **"find and replace"-like operations**.

So, what kind of find and replace"-like operations python `re` module support?
* `re.search(pattern, string, flags=0)`
    * Checks for a match **anywhere in the string**
* `re.match(pattern, string, flags=0)`
    * Checks for a match only **at the beginning of the string**
* `re.split(pattern, string, maxsplit=0, flags=0)`
    * Split string by the occurrences of pattern.   
* `re.sub(pattern, repl, string, count=0, flags=0)`
    * Return the string obtained by replacing the leftmost non-overlapping occurrences of pattern in string by the replacement repl.
* `re.findall(pattern, string, flags=0)`, `re.finditer(pattern, string, flags=0)`

```python
import re
string = "abcdba"
abMatch = re.match(r"ab", string)
noMatch = re.match(r"cd", string)
cdMatch = re.search(r"cd", string)
print "ab match:{}\nno match: {}\ncd match:{}".format(abMatch, noMatch, cdMatch)
# ab match:<_sre.SRE_Match object at 0x109543988>
# no match: None
# cd match:<_sre.SRE_Match object at 0x1095655e0>

pattern = re.compile("ab")
abCompileMatch = pattern.match(string)
print "pattern:{}\nab compile match:{}".format(pattern, abCompileMatch)
# pattern:<_sre.SRE_Pattern object at 0x108d91c10>
# ab compile match:<_sre.SRE_Match object at 0x10953dbf8>

print "type of r\'ab\':{}\ntype of `pattern`: {}".format(type(r"ab"), type(pattern))
# type of r'ab':<type 'str'>
# type of `pattern`: <type '_sre.SRE_Pattern'>
```

## `Match` object
* Match object is **what we got from the provided find-like operations**, so what would the user like to have as a result?
    * **The string(s) that matche the pattern(s)**
    * **The index(es) of each matched substring**

### Retrievnig string from `Match` object
* The concept of **group(s)**:
    * We can concate different subpattern using parenthesis to a large pattern to match a string. Each sub-pattern matching is return in different *group*, with the access function call as below snippet.
        * `group([group1, ...])`
            * Returns one or more subgroups of the match.
        * `groups([default])`
            * Return a tuple containing all the subgroups of the match, from 1 up to however many groups are in the pattern. The default argument is used for groups that did not participate in the match; it defaults to None.


### Retrieving Index from `Match` object
* `start([group])` and `end([group])`
    * Return the indices of the start and end of the substring matched by group;
* `span([group])`
    * For MatchObject m, return the 2-tuple (m.start(group), m.end(group)).
    * If group did not contribute to the match, this is (-1, -1).            



## How to write pattern?
> regular expression (sometimes called a rational expression) is **a sequence of characters** that define a search pattern

Remembered what we lernt from the wikipedia page? Regular expression is composed by a sequence of characters. To understand how to write the pattern, we need to know the **characters** first. Generally speaking, regular expressions, regardless of the language, all separates characters to ordinary characters and special characters.

Most of characters are **"ordinary characters"**, which **simply match themselves**. However, there are **special characters** which either
* (1) stand for classes of ordinary characters, or
* (2) affect how the regular expressions around them are interpreted.
    * Among them, I mainly catagorized the functionalities to be
        * (1) setting anchor
        * (2) repetitions
        * (3) building logic


#### stand for classes of ordinary characters:
* `'.'`: In the default mode, dot `.` matches any character except a newline.
* `'[]'`: Used to indicate a set of characters
    * Special characters lose their special meaning inside sets. For example, `'[(+*)]'` will match any of the literal characters `'(', '+', '*'`, or `')'`.
* `'[^]'`: Complementing the set. If the first character of the set `'[]'` is `'^'`, all the characters that are not in the set will be matched.
* Character sets

|       | represents      |
|    ---|              ---|
| `\w`  | `[a-zA-Z0-9_]`  |
| `\W`  | `[^a-zA-Z0-9_]` |
| `\s`  | `[ \n\r\t\f\v]` |
| `\S`  | `[^ \n\r\t\f\v]`|
| `\d`  | `[0-9] `        |
| `\D`  | `[^0-9]`        |


#### Setting anchor
* `'^'`: Matches the start of the string.
* `'$'`: Matches the end of the string or just before the newline at the end of the string.
* `'(...)'`: Matches whatever regular expression is inside the parentheses, and indicates the start and end of a group
* `'\A'`: Matches only at the start of the string.
* `'\Z'`: Matches only at the end of the string.
* `'\b'`: Matches the empty string, but only at the beginning or end of a word. => Read [Advanced discussion: \b and \B]
* `'\B'`: Matches the empty string, but only when it is not at the beginning or end of a word. => Read [Advanced discussion: \b and \B]()

#### Repetitions
* `'*'`: Match 0 or more repetitions of the preceding RE, as many repetitions as are possible.
* `'+'`: Match 1 or more repetitions of the preceding RE
* `'?'`: Match 0 or 1 repetitions of the preceding RE
* `'{m}'`: Specifies that exactly m copies of the previous RE should be matched
* `'{m,n}'`: Causes the resulting RE to match from m to n (inclusive) repetitions of the preceding RE

#### Building Logic
* `'|'`: `A|B`, where A and B can be arbitrary REs, creates a regular expression that will match either A or B.


### Advanced discussion 1: `\b` and `\B`
#### `\b`
* Matches the empty string, but only at the beginning or end of a word.
* formally, `\b` is defined as the boundary between a `\w` and a `\W` character (or vice versa), or between \w and the beginning/end of the string.
* **This match is zero-length.**
* `\w` means `[a-zA-Z0-9_]` in Python


```Python
print re.search(r"\bfoo\b", "foo").group() #\w and the beginning/end of the string
print re.search(r"\bfoo\b", "foo.").group() #`.` is in the \W set!
print re.search(r"\bfoo\b", "(foo)").group() # `(` and `)` is in the \W set!
print re.search(r"\bfoo\b", "bar foo baz").group() # ` ` is in the \W set!
print re.search(r"\bfoo\b", "foobar") # the right \b can't be matched, since b is in \W set
print re.search(r"\bfoo\b", "foo3") #  3 can't be matched

# foo
# foo
# foo
# foo
# None
# None
```
#### `\B`
* Matches the empty string, but only when it is not at the beginning or end of a word.
* `\B` is the negated version of `\b`.
* `\B` matches at every position where `\b` does not.
* Effectively, `\B` matches at **any position between two word characters** as well as at any position between two non-word characters.

```python
print re.search(r"py\B", "python").group() #the empty string was mathced between p"yt"hon
print re.search(r"py\B", "py3").group() #the empty string was mathced between p"y3"
print re.search(r"py\B", "py") # y's next is the end, so we can't match
print re.search(r"py\B", "py.") # y's next is in \W, so we can't match
print re.search(r"py\B", "py!") # y's next is in \W, so we can't match

# py
# py
# None
# None
# None
```

### Advanced discussion 2: Raw string notation (r"text")
Remembered we mentioned there are two styles of function calls:

```python
abMatch = re.match(r"ab", string)
# or
pattern = re.compile("ab")
abCompileMatch = pattern.match(string)    
```

So, what's the difference and why do we need the raw string notation (r"text")? It's actually caused by the **Backslash character (`'\'`)**. `'\'` indicate special forms or to allow special characters to be normal in regular expression. However, this conflicts with the string literal, which makes it simply to be a escape sign. In short, matching **a** literal backslash `'\'` in a string, one has to write **"\\\\"** as the RE string, because the regular expression must be \\, and each backslash must be expressed as \\ inside a Python string literal. The `r` at the start of the pattern string designates a python "raw" string which **passes through backslashes without change.**

Take a look for the following examples, noted that in the first parameters in the `re.match`, it should be a pattern string, whereas in the second parameters, it's a string. `"\\"` in a string means the single `"\"`


```python
print re.match("\\\\", "\\").group()
print re.match(r"\\", "\\").group()
print re.match(r"\\", r"\\").group()
print re.match("\\\\", r"\\").group()
print re.match("\\\\w", "\\w").group()
print re.match(r"\\w", "\\w").group()
print re.match("\\\\section", "\\section").group()
print re.match(r"\\section", "\\section").group()

# \
# \
# \
# \
# \w
# \w
# \section
# \section
```

### Advanced discussion 3: The "greedy" match concept

What is the "greedy" means in regular expression?

```python
print re.match("ab*", "a").group()
print re.match("ab*", "ab").group()
print re.match("ab*", "abb").group()
print re.match("ab*", "abbbbbbbbbbb").group()

# a
# ab
# abb
# abbbbbbbbbbb
```
From the example, we can see that `'*'` causes the resulting RE to match 0 or more repetitions of the preceding RE, **as many repetitions as are possible**. This is the so-called greedy in regular expression. However, sometimes greedy is not what we want.

```python
print "greedy: " + re.match(r"p.*q", "pbq c pdq").group() # what if we only want the <a> to be matched?
```

In the above example, we just want the "pbq", however, the greedy `.*` will give us the whole "pbq c pdq". Followings list the methods/usages to escape from the greedy operations.

#### (1) `*?`, `+?`, `??`

```python
print "non greedy: " + re.match(r"p.*?q", "pbq c pdq").group()
print "---"
print "greedy: " + re.match(r"p.+q", "pbq c pdq").group()
print "non greedy: " + re.match(r"p.+?q", "pbq c pdq").group()
print "---"
print "greedy: " + re.match(r"p?q", "pq").group()
print "non greedy: " + re.match(r"p??q", "pq").group()
print "greedy: " + re.match(r"p?q", "q").group()
print "non greedy: " + re.match(r"p??q", "q").group()
```

The last case `??` and `?`, though has same result, it is actually different in how the regular engine runs.
For the greedy match of `re.match("p?q", "pq")`, it will actually check "pq" first, then check "q". However, if you use `re.match("p??q", "pq")`, it will check "q" first, then check "pq". This concepts also apply on the `'|'`, OR operator.

As the target string is scanned, REs separated by `'|'` are tried from left to right. When one pattern completely matches, that branch is accepted. This means that once A matches, B will not be tested further, even if it would produce a longer overall match. Therefore, the document said the **`'|'` operator is never greedy**.

#### (2) `{m,n}?`

```python
print "greedy: " + re.match(r"a{3,5}", "aaa").group()
print "non greedy: " + re.match(r"a{3,5}?", "aaa").group()
print "greedy: " + re.match(r"a{3,5}", "aaaaa").group()
print "non greedy: " + re.match(r"a{3,5}?", "aaaaa").group()

# greedy: aaa
# non greedy: aaa
# greedy: aaaaa
# non greedy: aaa
```

### Advanced discussion 4:  `(?...)`, the extension notation
`(?...)` is an extension notation. The **first character after the `'?'` determines** what the meaning and further syntax of the construct is.

#### (1) `(?:...)`
* A non-capturing version of regular parentheses.

```python
print re.match(r"(ab)(cd)(e)", "abcde").group()
print re.match(r"(ab)(cd)(e)", "abcde").groups()
print re.match(r"(ab)(cd)(e)", "abcde").group()
print re.match(r"(ab)(?:cd)(e)", "abcde").groups()

# abcde
# ('ab', 'cd', 'e')
# abcde
# ('ab', 'e')
```

#### (2) `(?P<name>...)` and `(?P=name)`
 `(?P<name>...)`
* the substring matched by the group is accessible via the symbolic group name `name`.
`(?P=name)`
* A backreference to a named group; it matches whatever text was matched by the earlier group named name.
* There is another way to do the backreference: `\number`, which will be shown below

```python
# example 1: set and access the symbolic group name `foo` and `bar`
match = re.match(r"(?P<foo>ab)(c)(?P<bar>de)", "abcde")
print match.group()
print match.groupdict()
print match.groupdict()['foo']
print match.groupdict()['bar']
print match.group(1)
print match.group(2)
print match.group(3)

# abcde
# {'foo': 'ab', 'bar': 'de'}
# ab
# de
# ab
# c
# de

# example 2: use the back reference by symbolic group name
print re.match(r"(?P<foo>ab)(c)(?P=foo)", "abcde")
print "---"
match = re.match(r"(?P<foo>ab)(c)(?P=foo)", "abcab")
print match.groupdict()
print match.groups() # the later group that been referenced by the symbolic name "foo" will not be shown
print match.span(1)

# None
# ---
# {'foo': 'ab'}
# ('ab', 'c')
# (0, 2)
```
