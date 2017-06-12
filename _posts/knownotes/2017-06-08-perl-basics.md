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

# Procedure variables
* Technically, procedure variables have the prefix `&,` although the prefix is **not always necessary** in modern Perl:

```perl
sub foo {
 print "hello" ;
} ;

&foo() ;  # prints hello
foo() ;   # also prints hello
```

# Typeglobs

* The rarely used sixth variable “type,” the typeglob (sigil `*`), represents the **entire symbol table entry for a variable.**
* Typeglobs can **create aliases in the symbol table** and expose this implementation detail:

```perl
$same = 42 ;
@same = (1, 2, 3) ;
%same = (foo => 1, bar => 2) ;
sub same { print "foo" } ;

print $same ;  # prints 42
print @same ;  # prints 123
print %same ;  # prints bar2foo1
same() ;      # prints foo

*different = *same ;

print $different ;  # prints 42
print @different ;  # prints 123
print %different ;  # prints bar2foo1
different() ;       # prints foo

$different[1] = -2 ;

print @same ;       # prints 1-23
```

* The assignment `*different = same` has the same effect as `*different = *same`.

> Modern Perl has proper references, so these sorts of tricks are mostly unnecessary.

# Contexts
* In Perl, there are three contexts in which an expression may be evaluated:
  * scalar: When an expression is evaluated in a scalar context, it will become a scalar (and there are implicit coercions for each value type).
  * list: When an expression is evaluated in a list context, it will become a list (with another set of implicit coercions).
  * void: In a void context, the value of the expression is ignored.
* Using an array in a context that expects a scalar will yield the size of the array:

```perl
@bar = ("foo","bar","baz") ;
$barsize = @bar ;             # @bar turns into list before assignment
print $barsize ;              # prints 3
print scalar @bar ;           # prints 3
print $bar ;                  # prints nothing
```

* Using a scalar in a context that expects a list will create a single-element list with just that value:

```perl
@bar = 42 ;          # acts like @bar = (42) ;
print $bar[0] ;      # prints 42
print scalar @bar ;  # prints 1
```

* When an array is assigned a list, it constructs an array with the same elements as the list.


* `@bar = (10,20,30) ;`:
  * the expression `(10,20,30)` gets evaluated in list context, which produces a list with three elements: 10, 20 and 30.
  * That list is then immediately assigned to the array `@bar`, which converts it to an array with three elements: 10, 20 and 30.

* In Perl, the comma operator , has very different interpretations under scalar and list contexts.
* In a list context, **the comma operator appends its two arguments together** (each being evaluated in the list context):

```perl
@bar = ("foo", "baz") ;
print @bar ;             # prints foo, then baz
```
  * Technically, in the above, "foo" is evaluated as a list, which creates a singleton list containing just "foo", then the same happens to "bar", and then these singleton lists are appended.

* This leads to counterintuitive behavior, as **inner lists are flattened into the outer list**:

```perl
@bar = (10,20) ;
print scalar @bar ;    # prints 2 -- the length of @bar

@bar = (10,(20,30)) ;  # (10,(20,30)) became (10,20,30)
print scalar @bar ;    # prints 3 -- the length of @bar
```


* In a scalar context, the comma operator, evaluates the left operand, discards the result and then returns the right operand:

```perl
$bar = ("foo","baz") ;
print $bar ;             # prints baz (not 2)
```

  * The left-hand side of an assignment determines the context of the right-hand side.
  * When multiple values are assigned, the right-hand side is in list context:

```perl
@coords = (10,20,30) ;
($x,$y,$z) = @coords ;
print $x, $y, $z ;      # prints 10, then 20, then 30
```

* The first non-scalar destination in an assignment captures **the remainder of the incoming list**:

```perl
@long = (1,2,3,4,5,6) ;
($x,@rest) = @long ;
print $x ;              # prints 1
print @rest ;           # prints 23456

#/----------------

($x,@rest,@oops) = @long ;
print $x ;              # prints 1
print @rest ;           # prints 23456
print @oops ;           # prints nothing
```


> List context appears in more places than one might expect, which means that **many commas are not part of the syntax of the construct, but are really just operators.**

* For instance, the index in a slice is actually a list context:

```perl
@indices = (2,4) ;
@values = (0,10,20,30,40,50,60) ;
@slice = @values[1,@indices] ;     # grabs indices 1,2,4

print @slice ;                     # prints 10, then 20, then 40
```


# References


* A reference is a **scalar** value that contains the memory address of an object.
* In Perl, it is possible to create references to scalars, arrays and hashes (and even to procedures and typeglobs).
* To take a reference to a named value, use the reference operator `\`:

```perl
$s = "I'm a scalar." ;
@a = ("A", "Hash") ;
%h = (foo => 42, bar => 1702) ;

$sref = \$s ;
$aref = \@a ;
$href = \%h ;

print $sref ; # prints SCALAR(0xAddr)
print $aref ; # prints ARRAY(0xAddr)
print $href ; # prints HASH(0xAddr)
```

* The hexadecimal value that prints next to the type of the reference is the memory address of the referenced value.
* To dereference a reference, prefix it with `$`, `@` or `%` (or wrap it with `${}`, `@{}` or `%{}`) depending on the type of the reference:

```perl
print $$sref ; # prints I'm a scalar.
print @$aref ; # prints AHash
print %$href ; # prints bar1702foo42

print ${$sref} ; # prints I'm a scalar.
print @{$aref} ; # prints AHash
print %{$href} ; # prints bar1702foo42


print ${$aref}[1] ;     # prints Hash
print ${$href}{"foo"} ; # prints 42
```


### anonymous references

* Perl can also create anonymous references, references for which **the referenced value does not correspond to a named variable.**

* The bracket notation `[]` creates an anonymous array:

```perl
$b = [1,2,3] ;
print $b ;         # prints ARRAY(0xAddr)
print $$b[1] ;     # prints 2
print ${$b}[1] ;   # prints 2
```

* The braces notation `{}` creates an anonymous hash:

```perl
$h = { foo => 1, bar => 2 } ;
print $h ;            # print HASH(0xAddr)
print $$h{"bar"} ;    # print 2
print ${$h}{"bar"} ;  # print 2
```

* The argument supplied to both `[]` and `{}` are actually in list context, which means that the usual rules for expansion into a list apply:

```perl
@foo = ("a" => 10, "b" => 20) ;
$href = {@foo} ;       # @foo expands to "a",10,"b",20
print $href->{"a"} ;   # prints 10
```

* Since references are scalars, it is possible to have arrays that contain arrays:

```perl
$matrix = [ [ 1, 0, 0 ],
            [ 0, 1, 0 ],
            [ 0, 0, 1 ] ] ;

print ${${$mtrix}[1]}[1] ; # prints 1
```


### Dereference operator `->`

* The dereference operator `->` works on arrays, hashes and references:


```perl
@array = (10,20,30) ;
$aref = \@array ;

print @array->[1] ; # prints 20
print $aref->[1] ;  # prints 20

@array->[2] = 40 ;  # prints 40
print $aref->[2] ;  # prints 40

%hash = (foo => 100, bar => 200) ;
$href = \%hash ;

print %hash->{"foo"} ;  # prints 100
print $href->{"foo"} ;  # prints 100

%hash->{"bar"} = 300 ;  # prints 300
print $href->{"bar"} ;  # prints 300
```

### Typeglob references

* A reference to a typeglob essentially creates a first-class symbol table entry:

```perl
$tgref = \*foo ;

print $tgref ;       # prints GLOB(0xAddr)

*baz = *$tgref ;

$baz = 100 ;
@baz = (2,3) ;

print $foo ;         # prints 100
print @foo ;         # prints 2, then 3
```
