---
layout: post
title: "Perl Basics"
excerpt: ""
categories: knownotes
tags: [perl]
comments: true
share: true
author: chungyu
---
> [A guide to Perl: By experiment](http://matt.might.net/articles/perl-by-example/)

# The syntax and semantics of Perl are complex
* Perl’s syntax is inherently ambiguous and cannot be resolved without information available at run-time:
  * there are at least 6 different valid ways to parse the expression foo bar 1, 2, 3.
* Perl relies on implicitly assigned variables.
  * To program without surprise in Perl, one must know all the cases in which their values change.
* The parameter-passing mechanism is Byzantine, and in some cases, dangerously counter-intuitive.
* “Context” determines how expressions evaluate, yet expressions also determine context.
* There are four distinct scoping mechanisms for variables: lexical, dynamic, global and package.


# What is a Perl program?
* At a high level, a Perl program is a sequence of statements and declarations.
  * Statements are commands that conduct computation, side effects and I/O:
  * Declarations may import packages and define procedures.

>
  [At the very least, you should add:
  `use strict;`
  `use warnings;`
  to any code that might go into production.]

# Comments
* A code comment in Perl begins with a hash `#` and extends to the end of the line.
* multi-line comment: POD documentation format

```perl
=begin comment
print "This does not print" ;
It is a comment.
=end comment
=cut
print "This is not a comment" ; # This prints.
```

# Variables
* Perl seems to have five common variable “types”: “constants,” scalars, arrays, hashes and procedures.
  * (A sixth type, the typeglob, is not common in modern Perl.)
  * Scalar variables hold basic values like numbers and strings (and references to other, possibly complex, values).
  * Constants are barewords that should evaluate to a single value.
  * Array variables hold multiple scalar values in order.
  * Hashes map keys (strings) to scalar values.
  * Procedures (called subroutines) accept arguments, perform computation and return results.
* Each variable type in Perl is associated with a prefix called a sigil.

# Scalar variables
* The `$` prefix accesses a variable as a scalar:

```perl
$foo=3 ;
$string="hello" :
```

# Constants
* Constants have no prefix, and they should only be defined once, with the form `use constant-name => value ;`

```perl
use constant PI => 3.14 ;
print PI ;
```

* Pitfall!! Because constants are resolved at compile-time, they take effect even if the block in which they are defined fails to execute:
  * constants aren’t even really constants: they’re functions that take no arguments. PI() and &PI both work

```perl
if (0) {
  use constant E => 2.17 ;
}
print E ;                    # prints 2.17
print E();                   # prints 2.17
print &E;                    # prints 2.17
```

# Array variables
* Arrays use the prefix `@`, and arrays contain sequences of scalar values:
* The familiar `[]` subscript notation accesses and modifies array elements, but with the prefix `$`

```perl
@arr = ("foo","bar","baz");
print $arr[1] ;              # prints bar
$arr[1] = "-" ;
print @arr ;                 # prints foo-baz
```

> array variables “contain” the entire array, not a pointer or reference to the array. As a result, **copying one array variable into another copies the entire array**:

```perl
@a = (1, 2, 3);
@b = @a ;
$b[1] = -2 ;
print @a ;  # prints 123
print @b ;  # prints 1-23
```

# Hash variables
* The prefix `%` denotes a hash variable, but hashes must be indexed with braces `{}` instead of brackets `[]`

```perl
%hash = ("foo", 1, "bar", 2) ;

print $hash["foo"] ;             # prints nothing
print $hash{"foo"} ;             # prints 1
$hash{"bar"} = 20 ;
print $hash{"bar"} ;             # prints 20 ;
```

* Hash variables expect **a list for initialization**.
* To make it cleaner to write these initializers, the => operator acts kind of like the comma operator:

```perl
%days = ("mon" => 0,
         "tue" => 1,
         "wed" => 2,
         "thu" => 3,
         "fri" => 4,
         "sat" => 5,
         "sun" => 6);
print $days{"fri"} ;     # prints 4
```

* With the `=>` operator, if the left-hand operand is a bare identifier, it gets treated as a `string`:

```perl
%days = (mon => 0,
         tue => 1,
         wed => 2,
         thu => 3,
         fri => 4,
         sat => 5,
         sun => 6);
print $days{"fri"} ;     # prints 4
```

* Since **keys to hashes must be strings**, barewords supplied as hash keys will be turned into strings even in the index position:

```perl
%a = (foo => 10) ;
print $a{foo} ;     # prints 10
```

> Hashes are copied on assignment as well:

```perl
%a = (foo => 10);
%b = %a ;

$b{"foo"} = 20 ;

print %a ;  # prints foo10
print %b ;  # prints foo20  
```


# Slices

* Arrays can be sliced by giving them a list of indices:

```perl
@foo = (0,10,20,30,40,50) ;
@bar = @foo[2,3] ;
print @bar ;               # prints 20, 30

@foo[2,5] = (-20,-50) ;
print @foo ;               # prints 0, 10, -20, 30, 40, -50
```

* Hashes can also be sliced:

```perl
%foo = (alpha => 10,
        beta  => 20,
        gamma => 30) ;

@alphabet = @foo{alpha,beta} ;
print @alphabet;

@foo{alpha,gamma} = (-10, -30) ;
print $foo{alpha} ;                 # prints -10
print $foo{beta} ;                  # prints 20
print $foo{gamma} ;                 # prints -30
```
