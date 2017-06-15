---
layout: post
title: "Perl statements and expression"
excerpt: ""
categories: scriptlang
tags: [perl]
comments: true
share: true
author: chungyu
---
> [A guide to Perl: By experiment](http://matt.might.net/articles/perl-by-example/)

# Statements

* The simplest statements in Perl are expressions statements, which consist of an expression followed by a semicolon: `print "foo" ;`


# if statements

* In Perl, if statements must use parentheses around the conditional, and they must use braces around the body:

```perl
if foo { print "foo"; }  # error: should be (foo)
if (bar) print "bar" ;   # error: should be { print "bar" ; }
```

* In Perl, `elsif` should be used instead of else if:

```perl
$count = 19 ;

if ($count < 10) {
  print "count < 10" ;
} elsif ($count < 20) {
  print "10 < count < 20" ;  # this prints
} else {
  print "count >= 20" ;
}
```

* For stylistic reasons, conditionals may be placed after statements:

```perl
$foo = 20 ;
print "big" if $foo > 10 ;     # prints big
print "small" if $foo <= 10 ;  # prints nothing
```

* Perl also supports an `unless` variant of if which negates the condition:

```perl
$age = 22 ;

unless ($age >= 25) { print "You cannot rent a car." ; }
# prints You cannot rent a car.

die() unless $age >= 21 ;  # does nothing
```

# while statements

```perl
$count = 10 ;
while ($count > 0) {
  print $count ;
  $count = $count - 1 ;
} # prints 10 through 1
```

* The expression `next` will advance to the next iteration of the innermost loop:

```perl
$count = 10 ;
while (--$count > 0) {
  next if $count % 2 == 0 ;
  print $count ;
} # prints 9 7 5 3 1 ;
```

* The expression `last` will exit the innermost loop
* In this sense, last is like `break` in Java or C

```perl
$count = 0 ;
while (1) {
  print $count ;
  $count++ ;
  last if $count == 10 ;
} # prints 0 1 2 3 4 5 6 7 8 9
```

* The expression `redo` will restart the current innermost loop but **without re-evaluating the condition**:

```perl
$count = 1 ;
while ($count > 0) {
  if ($count <= 0) {
    print "impossible?" ;
    last ;
  }
  $count-- ;
  print $count ;
  redo ;
} # prints 0, then impossible?
```


* In Perl, while blocks can have a `continue` block attached.
  * When followed by a BLOCK, `continue` is actually a flow control statement rather than a function.
  * If there is a `continue` BLOCK attached to a BLOCK (typically in a while or foreach ), it is always executed just before the conditional is about to be evaluated again,

```perl
$count = 10 ;
while ($count > 0) {
  next if $count % 2 == 0 ;
  print $count ;
} continue {
  $count-- ;
} # prints 9 7 5 3 1
```

* A `next` expression will jump into the continue block; a `redo` will not.
* It is possible to label a loop in Perl, so that `next`, `last` and `redo` can choose to which loop they refer:

```perl
$i = 0;
OUTER: while ($i < 6) {                   
  $j = 0 ;
  INNER: while ($j < 6) {
    next OUTER if $i % 2 == 0 ;
    next INNER if $j % 3 == 0 ;
    print "$i:$j" ;
  } continue {
    $j++ ;
  }
} continue {
  $i++ ;
} ;
prints:

1:1
1:2
1:4
1:5
3:1
3:2
3:4
3:5
5:1
5:2
5:4
5:5
```


* Naturally, Perl allows `while` to follow a statement:

```perl
$i = 0;
print $i while ($i++ < 4) ; # prints 1 through 4
```

* If the intent is to run the block once before testing the condition, then the `do-while` form applies:

```perl
$i = 0 ;
do { print $i } while ($i++ < 4) ; # prints 0 through 4
```

# Blocks

* Perl also has compound statements (also known as **block statements**), formed with braces, `{}`:

```perl
{ print "hello" ; print "goodbye" ; } # prints hello; then goodbye
```

* Finally, `next` and `last` will actually work in any block, not just a while body:

```perl
{
  print "This prints." ;
  next ;
  print "But this doesn't." ;
} # prints This prints.

{
  print "This prints." ;
  last ;
  print "But this doesn't." ;
} # prints This prints.
```

* They seem to have identical behavior, except that regular blocks can have continue blocks as well:

```perl
{
  next ;
  print "I won't print." ;
} continue {
  print "But, I will." ;
} # prints But, I will.

{
  last ;
  print "I won't print." ;
} continue {
  print "Neither will this." ;
} # prints nothing
```

* Of course, blocks can labelled:

```perl
OUTER: {
  INNER: {
    next OUTER ;
  } continue {
    print "This won't print." ;
  }
} # prints nothing
```

# for statements

* Perl has traditional C-style for statements of the form for `( initializer ; test ; increment ) { body }`:

```perl
for ($i = 0; $i < 4; $i++) {
  print $i ;
} # prints 0 through 3
```

# foreach statements

* The `foreach` form in Perl allows iteration over individual elements in arrays:

```perl
@array = ("foo","bar","baz") ;

foreach $element (@array) {
  print $element ;
} # prints foo, bar and then baz
```

* In Perl, the keywords `for` and `foreach` may be used interchangeably.
* As it turns out, the iteration variable is an alias to the elements:

```perl
@array = ("foo","bar","baz") ;

foreach $element (@array) {
  $element = "$element:x" ;
}

print "@array" ; # prints foo:x bar:x baz:x
```

* Leaving off the variable for iteration will bind each element to the default variable, `$_`:

```perl
@array = (4,5,6) ;

foreach (@array) {
  print $_ ;
} # prints 4 through 6
```

* The default binding version may follow a statement:

```perl
print $_ for (1..3) ;         # prints 1; then 2; then 3
print $a for my $a (1..3) ;   # error
```

# Labelled statements and goto

* Any statement in Perl can be labelled, and `goto` can branch to any label:

```perl
$n = 4 ;
$a = 1 ;
TOP:
$a = $a * $n ;
$n = $n - 1 ;
goto TOP if $n >= 1 ;
print $a ; # prints 24
```

* Labels are resolved at run-time in Perl, which means that computed strings can be used to jump off to a label:

```perl
$fi = "FI" ;
$rst = "RST" ;
$first = "$fi$rst";
goto $first ;
FIRST: print "foo" ; goto DONE ;
SECOND: print "bar" ;
DONE: {} ;
# Program prints foo
```

* The `goto` form in Perl is also used to perform tail call jumps.
* The expression `goto &proc` will (effectively) return from the current procedure and immediately invoke `proc` in its place:

```perl
sub proc1 {
  goto &proc2 ; # tail call to proc2
}

sub proc2 {
  return 42 ;
}

print proc1 ;  # prints 42
```
