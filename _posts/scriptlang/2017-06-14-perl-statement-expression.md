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


# Expressions

* The simplest expressions in Perl are literals:
  * string constants like 'foo' and numeric constants like 3 or 3.14.

* Many complex expressions in Perl are procedure or primitive calls of some kind.
* Most other expressions types are constructed from binary, unary or ternary operators.

* Perl supports the usual arithmetic operators:

```perl
print 10 + 20 ;  # prints 30
print 10 - 20 ;  # prints -10
print 10 / 20 ;  # prints 0.5
print 10 * 20 ;  # prints 200

print 2 ** 3 ;   # prints 8 (exponentiation)
```

### C-style in/decrement

* Perl also supports unary increment and decrement:

```perl
$foo = 13 ;
print $foo++ ;   # prints 13
print $foo   ;   # prints 14
print ++$foo ;   # prints 15
print $foo   ;   # prints 15

print $foo-- ;   # prints 15
print $foo   ;   # prints 14
print --$foo ;   # prints 13
print $foo   ;   # prints 13
```


### Boolean operators

* Perl also supports the common Boolean operators:

```perl
print "and: ", (20 && 10) ;  # prints 10
print "and: ", (0 && 20) ;   # prints 0

print "or: ", (1 || 2) ;     # prints 1
print "or: ", (0 || 1) ;     # prints 1

print "not: ", !0 ;          # prints 1
print "not: ", !42 ;         # prints nothing


print "and: ", (20 and 10) ;  # prints 10
print "and: ", (0 and 20) ;   # prints 0

print "or: ", (1 or 2) ;      # prints 1
print "or: ", (0 or 1) ;      # prints 1

print "not: ", not 0 ;        # prints 1
print "not: ", not 42 ;       # prints nothing
```

* The word forms of each operator act identically, but have the lowest possible precedence.

### Number comparison

* Perl supports the common comparison operators as well:

```perl
if (10 == 10) { print "true" }   # prints true
if (20 == 10) { print "false" }  # prints nothing
if (10 <= 20) { print "true" }   # prints true
if (10 > 20) { print "false" }   # prints nothing
```

### String comparison

* But, Perl requires named operators for string comparisons:

```perl
if (10 lt 20)  { print "true" }       # prints true
if (101 lt 20) { print "yikes!" }     # prints yikes!
if ("101" lt "20") { print "true" }   # prints true

if ("cat" lt "hat") { print "true" }  # prints true
if ("cat" gt "bat") { print "false" } # prints nothing
if ("cat" le "cat") { print "true" }  # prints true
if ("cat" ge "cat") { print "true" }  # prints true

if ("alice" == "bob") { print "uh-oh!" } # prints uh-oh!
if ("alice" eq "bob") { print "false" }  # prints nothing
if ("cat" eq "cat") { print "true" }     # prints true
```

### Bitwise operator

* Perl allows C-like bitwise and bitshift operators `&,` `|`, `~`, `<<` and `>>` – as well, but caution should be taken when using them, since their **interpretation changes depending on whether use integer or use bigint** are in effect.

* To a get a sense of how these operators work, we can use printf to print binary:

```perl
$a = 23 ;
$b = 71 ;
printf "%b\n", $a ;       # prints   10111
printf "%b\n", $b ;       # prints 1000111

printf "%b\n", $a & $b ;  # prints     111
printf "%b\n", $a | $b ;  # prints 1010111
printf "%b\n", ~$a ;      # prints
 # 1111111111111111111111111111111111111111111111111111111111101000
```

### One line if else (ternary)

* Perl supports a C-style ternary operator for conditionals too:

```perl
$name = "Alice" ;
print ($name eq "Alice" ? 1 : 2) ;  # prints 1
```

### `.` operator

* Some Perl operators are different, or have no counterpart in other languages.
* The dot operator `.` is concatenation:

```perl
$foo = "Hello, " ;
$bar = "world!" ;
$foobar = $foo . $bar ;
print $foobar ;          # prints Hello, world!
```

### `x` operator

* The repetition operator `x` repeats a scalar (coerced to a list) or a list, depending on context:

```perl
$rrr = "r" x 10 ;
print $rrr ;       # prints rrrrrrrrrr

@rrr = ("r") x 10 ;
print "@rrr" ;     # prints r r r r r r r r r r

@rrr = "r" x 10 ;
print "@rrr" ;     # prints rrrrrrrrrr

@nums = (1,2) x 5 ;
print "@nums" ;    # prints 1 2 1 2 1 2 1 2 1 2
```

### `~~` operator

* The parentheses on the left-hand side are required to force the list context.
* The operator `~~` is “smartmatch,” which has “smart” comparison behavior:

```perl
@arr1 = (1,2,3) ;
@arr2 = (2,3) ;

@keys = ("foo") ;
%hash = (foo => 42, bar => 1701) ;

print "match" if @arr1 ~~ \@arr1 ; # prints match
print "match" if @arr2 ~~ @arr1 ;  # prints nothing

print "match" if @keys ~~ %hash ; # prints match
```

* To predict the behavior of smartmatch, consult the offical Perl docs.


### `...` operator

* In a list context, the range operator `...` produces an array starting with the left-hand side and going up to the right-hand side:

```perl
@range = 3...6 ;
print "@range" ;  # prints 3 4 5 6
```

* In scalar context, the `...` operator has a very different interpretation; the scalar `...` operator is meant to emulate the range behavior of `awk` and `sed`.

* In a scalar context, `lhs ... rhs` will be false until lhs evaluates to true. Then, it will be true until after rhs evaluates to false. Then, it will evaluate to false and wait for lhs to be true again:

```perl
$i = 0 ;
while ($i < 10) {
  print $i if ($i == 3) ... ($i == 7) ;
  $i++ ;
} # prints 3 through 7
```


* By introducing a toggle variable, the above code could be rewritten:

```perl
$toggle = true ;
$toggle = 1 ;
$i = 0 ;
while ($i < 10) {
  print $i if  ($toggle ? (($i == 3) ? !($toggle = 0) : 0)
                        : (($i == 7) ?  ($toggle = 1) : 1)) ;
  $i++ ;
} # prints 3 through 7
```

* Of course, one naturally wonders in which block this implicit `$toggle` variable lives.
* It could be the nearest enclosing block, or it could be the nearest enclosing procedure.

> It appears that they are **lexically scoped to the nearest procedure**, which leads to suprises:

```perl
sub proc {  

  my @a = (1,2) ;
  my @b = (1,2,3,4) ;

  foreach my $a (@a) {  # execute this loop twice
    foreach my $b (@b) {

      my $flip = ($b == 3) ... ($b == 11) ;

      if ($flip) {
        print "\$b: 3 <= $b <= 11" ;
      }
    }
  }
}

proc() ;  # prints:

# $b: 3 <= 3 <= 11  # OK
# $b: 3 <= 4 <= 11  # OK
# $b: 3 <= 1 <= 11  # uh-oh!
# $b: 3 <= 2 <= 11  # uh-oh!
# $b: 3 <= 3 <= 11  # looks OK, but it's not
# $b: 3 <= 4 <= 11  # looks OK, but it's not
```


* Constants have a special interpretation if they appear in a `...` operator.
* A constant implicitly compares for equality against the current line number of the input (stored in the variable `$.`).

* The following program will print first 10 lines of input:

```perl
while (<>) {
 print if 1 ... 10 ;
}
```
  * because it is equivalent to:

```perl
while (<>) {
 print if ($. == 1) ... ($. == 10) ;
}
```

### `..` operator v.s `...` operator

* The `..` operator is an alternative to `...` with a minor difference:
  * in the scalar context, `..` will test the right-hand side when the left-hand side matches.
  * If both match, then the `..` evaluates to true once, and then resumes evaluating to false:

```perl
$i = 0 ;
while ($i < 10) {
  print $i if ($i == 3) .. ($i == 3) ;
  $i++ ;
} # prints only 3


$i = 0 ;
while ($i < 10) {
  print $i if ($i == 3) ... ($i == 3) ;
  $i++ ;
} # prints 3 through 9
```
