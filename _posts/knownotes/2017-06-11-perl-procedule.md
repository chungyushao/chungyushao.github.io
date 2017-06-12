---
layout: post
title: "Perl Procedure"
excerpt: ""
categories: knownotes
tags: [perl]
comments: true
share: true
author: chungyu
---
> [A guide to Perl: By experiment](http://matt.might.net/articles/perl-by-example/)

# Procedures

* Perl calls procedures **subroutines**.

```perl
sub my_procedure {
  print "I'm a procedure!" ;
}
```

* There are many ways to call a procedure:

```perl
my_procedure;             # prints I'm a procedure!
&my_procedure;            # prints I'm a procedure!
my_procedure();           # prints I'm a procedure!
&my_procedure();          # prints I'm a procedure!
my_procedure 1, 2, 3;     # prints I'm a procedure!
my_procedure (1, 2, 3);   # prints I'm a procedure!
&my_procedure (1, 2, 3);  # prints I'm a procedure!
&my_procedure 1, 2, 3;    # error, parens required with &
```


* Arguments to procedures arrive implicitly via the `@_` array:

```perl
sub foo {
  print "foo: @_" ;
}

foo 1, 2, 3 ; # prints foo: 1 2 3
foo (1,2,3) ; # prints foo: 1 2 3
foo ;         # prints foo:
```

* Keep in mind that individual arguments are accessed as `$_[n]`:

```perl
sub bar {
  print $_[1] ;
}

bar 1, 2, 3 ; # prints 2
bar (1,2,3) ; # prints 2
bar ;         # prints nothing
```

* By default, the arguments to a procedure are in the list context, which means that arrays passed as arguments will be flattened (by the comma operator, actually):

```perl
@a = ((1,2),3) ;   # Internally, @a becomes (1,2,3)
@c = (6,7) ;       
@b = (5,@c) ;      # Internally, @b becomes (5,6,7)

sub print9 {
  print $_[0] ;
  print $_[1] ;
  print $_[2] ;
  print $_[3] ;
  print $_[4] ;
  print $_[5] ;
  print $_[6] ;
  print $_[7] ;
  print $_[8] ;
}

print9 0,@a,4,@b,8 ; # prints 0 through 8, one on each line.
print $b[1];         # prints 6
print $b[2];         # prints 7
```

* Understanding this automatic appending and flattening behavior is critical to understanding procedures calls.
* The comma operator (,) can mean cons, append, flatten all at once.
* To reiterate and to be clear, given this procedure definition:

```perl
sub print3 {
  print $_[0], $_[1], $_[2] ;
}
```

* all of the following are equivalent procedure calls:

```perl
# Each of the following prints 123:
print3 1, 2, 3 ;
print3 (1,2,3) ;
&print3 (1,2,3) ;

@args = (1,2,3) ;
print3 @args ;
print3 (@args) ;

@arglets = (1,2) ;
print3 @args,3 ;
print3 (@args,3) ;
```

* Actually, `print3()` and `&print3()` could differ; `&print3()` would ignore the prototype, if there is one, as discussed below.

### Return
* The `return` keyword exits the current procedure and returns the value it received:

```perl
sub one {
  return 1 ;
}

print one ;  # prints 1
```

* Otherwise, the value of the last expression gets returned:

```perl
sub two {
  2
}

print two ;  # prints 2
```


### Arguments to procedures

* If a procedure is called by `&` with no arguments, then it implicitly receives the current `@_` as its own arguments:

```perl
sub print_args {
  print @_ ;
}

sub call_print_args {
  &print_args ;
}

call_print_args "hello, world!" ; # prints hello, world!
```


* (Procedures called with `&` also ignore the prototype, as explained below.)
* The convention for naming arguments is to assign the immediately:

```perl
sub sum {
  my ($a, $b) = @_ ;
  return $a + $b ;
}
```


* Perl novices often don’t realize that **arguments in Perl are implicitly passed by alias: modifications to the inputs to a procedure will be seen by the caller of that procedure.**

That is, the arguments array `@_` contains aliases to the input values:

```perl
$x = 3 ;
@a = (4,5,6) ;

sub mod_args {
  $_[0] = 42 ;
  $_[2] = 17 ;
}

mod_args $x, @a ;

print "$x : @a" ;  # prints 42 : 4 17 6
```

* Remarkably, it’s possible to modify arrays and hashes this way:

```perl
sub mod_args {
  $_[0] = 42 ;
  $_[2] = 17 ;
}

@a = (7,8,9) ;
%h = ( "foo" => 42 ) ;

mod_args $a[1], 0, $h{"bar"} ;

print "@a" ;       # prints 7 42 9
print $h{"bar"} ;  # prints 17
```

# References to procedures and anonymous procedures

* Perl allows references to procedures:

```perl
sub sum {
  return $_[0] + $_[1] ;
}

$mysum = \&sum ;         # the & is necessary

print $mysum ;           # prints CODE(0xAddr)

print &{$mysum}(10,20) ; # prints 30

print &$mysum(10,20) ;   # prints 30
```


* Perl also permits the creation of anonymous procedures (more precisely, closures):

```perl
$myprod = sub {
  return $_[0] * $_[1] ;
} ;

print $myprod ;            # prints CODE(0xAddr)
print &{$myprod}(10,20) ;  # prints 200
```

* The `->` operator provides a more convenient syntax for invoking anonymous procedures:

```perl
$anon = sub {
  print $_[0] ;
} ;

$anon->(1701) ;  # prints 1701
```

# Procedures and context

* Surprisingly, context can even change how a procedure call is parsed.

* `foo (bar 1, 2 , 3)` could parse to:
  * `foo((bar(1, 2, 3)))`
  * `foo((bar(1)), 2, 3)`
  * depending on the context for the arguments of bar.

* Before Perl can evaluate (or sometimes even parse) an expression, it must know the contexts of that expression.
* This leads to two questions for procedure calls:
  * What are the contexts of the arguments to a procedure?
  * For instance, given a call: `foo(bar())`, What is the context of `bar()`?
  * How does the procedure know the context of its return value?
  * For instance, given a call: `localtime()`: Is `localtime()` returning a scalar, or a list?


# Argument context and prototypes

* When declaring a procedure, **each procedure may specify a prototype, which specifies contexts for arguments.**
* The prototype precedes the body block; the declaration form for a procedure with a prototype is
  * `sub procedure-name ( prototype ) { body }`

* If a procedure is used (syntactically) before its definition, it is possible to predeclare it with
  * `sub procedure-name ( prototype ) ;`

* It is necessary to predeclare so that the Perl parser can correctly parse calls to this procedure.
* The prototype is **a sequence of specifiers**, where the basic specifiers are:
  * `$` for scalar context;
  * `@` for list context;
  * `&` for a code reference;
  * `+` for scalar context, unless named hash or array; and
  * `*` for typeglob (mostly for passing archaic bareword filehandles).

* To make some arguments optional, place a semicolon `;` between the mandatory and optional arguments specifiers.
* Any basic specifier may be preceded by `\` to forcibly capture a reference to the incoming argument.
* Finally, it is possible to specify more than one mode for an argument by wrapping them in brackets:
  * `\[abc…]` which will accept \a or \b or \c or …

* Unfortunately, these specifiers don’t behave according to what one’s intuition might expect. We’ll try out each one to see what it does. Let’s try creating a procedure that accepts one scalar:

```perl
sub scalarg ($) {
  print $_[0] ;
}

@a = ("foo", "bar", "baz") ;
%h = ("foo" => 42 ) ;
$x = 1701 ;

scalarg $x ;  # prints 1701
scalarg @a ;  # prints 3 (length of @a)
scalarg %h ;  # prints 1/8   # WTF?

scalarg (1,2) ; # error: too many arguments
```

* So, it seems `$` forces scalarity for its argument. Let’s try creating a procedure that accepts an array:

```perl
@a = ("foo", "bar", "baz") ;
%h = ("foo" => 42 ) ;
$x = 1701 ;

sub arrarg (@) {
  print $_[0], ",", $_[1], ":", "@_" ;
}

arrarg $x ;     # prints 1701,:1701
arrarg @a ;     # prints foo,bar:foo bar baz
arrarg %h ;     # prints foo,42:foo 42

arrarg $x,@a ;  # prints 1701,foo:1701 foo bar baz
```

* It seems that the procedure call still flattened out the arrays (and hashes) when making the call.
* The prototype specifier `@` doesn’t seem to do what one might expect.
  * The real purpose of `@` is to allow a variable number of arguments: if it appears as the last parameter in a prototype, then the procedure accepts any number of arguments.


* In fact, last is the only sensible position for this specifier. Let’s try creating a procedure with the hash specifier:

```perl
@a = ("foo", "bar", "baz") ;
%h = ("foo" => 42 ) ;
$x = 1701 ;

sub hasharg (%) {
  print "$_[0]", "::", "@_";
}

hasharg $x ;      # prints 1701::1701
hasharg @a ;      # prints foo::foo bar baz
hasharg %h ;      # prints foo::foo 42
```

So, `%` doesn’t mean that argument is going to be a hash. It seems to behave identically to `@`.
  * (And, in the absence of a reference modifier, that’s exactly what it does.)

* Trying to use that argument as a hash, or even the whole input as a hash, will not work:

```perl
sub use_hash (%) {
  print $_[0]{"foo"} ;  # nope
  print $_{"foo"} ;     # nope
}

use_hash ("foo" => 1701) ;  # prints nothing
```

* To use the provided list as a hash, one must re-interpret the arguments in `@_` as a hash:

```perl
sub use_hash (%) {
  %hash = @_ ;
  print $hash{"foo"} ;
}

use_hash ("foo" => 42) ;    # prints 42
```

* Of course, it is also possible to drop `@_` into an anonymous hash and then dereference it directly:


```perl
sub use_hash (%) {
  print ${{@_}}{"foo"} ;
}

use_hash ("foo" => 1701) ;  # prints foo twice
```

* The specifier `&` expects to receive a reference to code, but if the first argument is a literal block of code, it creates an anonymous procedure for it on the fly:

```perl
sub take_block (&) {
  &{$_[0]}() ; # run it once
  &{$_[0]}() ; # run it twice
}

take_block { print "hello" } ; # prints hello twice

sub print_me {
  print "me" ;
}

take_block sub { print_me } ;  # prints me twice

take_block \&print_me ;        # prints me twice

take_block print_me ;          # error: must be block or code ref
```

* Unfortunately, code blocks (withouth sub) are only accepted as the very first parameter:

```perl
sub take_block_second ($&) {
  if ($_[0]) {
    &{$_[0]} ;
  }
}

take_block_second 10, { print "second" } ;  # error: { } treated as hash
```

* The `+` specifier has an interesting effect:

```perl
@a = ("foo", "bar", "baz") ;
%h = ("foo" => 42 ) ;
$x = 1701 ;

sub what_is (+) {
  print $_[0] ;
}

what_is $x ;  # prints 1701
what_is @a ;  # prints ARRAY(0xAddr)  
what_is %h ;  # prints HASH(0xAddr)
```


* Suddenly, instead of allowing the argumens to flatten, the `+` specifier captured a reference to the array or hash that was passed in.

Now, it’s possible to take two separate arrays as arguments:

```perl
sub what_are (++) {
  print $_[0], " ", $_[1] ;
}

@a1 = (1,2,3) ;
@b1 = (4,5,6) ;

what_are ((1,2),(3,4)) # scalar context! prints 2, then 4
what_are @a1,@b1 ;     # prints ARRAY(0xAddr) ARRAY(0xAddr)
```

* Except that it prefers to interpret items a scalars when possible.
* To be clear, the `\@` specifier can force an argument to be a referenceable array:

```perl
sub take_array (\@) {
  print $_[0] ;
}

take_array @a1 ;                 # prints ARRAY(0xAddr)

sub take_two_arrs (\@\@) {
  print $_[0], $_[1] ;
}

take_two_arrs @a1, @b1 ;         # prints ARRAY(0xAddr) ARRAY(0xAddr)
take_two_arrs ((1,2),(3,4)) ;    # error: arrays must be named
```

This is a little strange. The specifier `\@` seems to accept both (addressable) arrays and pointers to arrays.

To accept a reference to one of several specifiers, Perl accepts a grouped `\[ specifiers ]` form:

```perl
sub array_or_hash (\[@%]) {
  print $_[0] ;
}

$scalar = 3 ;
@array = (10,20,30) ;
%hash = ("foo" => 42, "bar" => 13) ;

array_or_hash @array ;    # prints ARRAY(0xAddr)
array_or_hash %hash ;     # prints HASH(0xAddr)
array_or_hash $scalar ;   # compilation error
```

# Return context

* When inside a procedure, the oddly-titled primitive `wantarray` determines if the context to which the procedure is returning expects a scalar, a list or nothing at all:


```perl
sub print_context () {
  if (wantarray()) {    
    print "list context";
  }

  elsif (defined wantarray()) {
    print "scalar context";
  }

  else {
    print "void context";
  }
}

$x = print_context ;  # prints scalar context
@x = print_context ;  # prints list context
print_context ;       # prints void context
```

This is how procedures like `localtime` can decide whether to return an array or a scalar:

```perl
@a = localtime ;

$x = localtime ;

print "@a" ; # prints a 9-element array, e.g.:
             # 29 49 15 28 0 114 2 27 0
print "$x" ; # prints the time as string, e.g.:
             # Tue Jan 28 15:49:29 2014
```


* In fact, procedures must be aware of their invocation context.
* If they could not determine this, then it would be impossible to correctly evaluate return statements.
* The context of the expressions in return statements is the context in which the procedure was invoked:

```perl
sub foo {
  return (4,5,6) ;
}

$x = foo() ;
@a = foo() ;

print $x ;     # evaluates (4,5,6) in scalar context
               # prints 6
print @a ;     # evaluates (4,5,6) in list context
               # prints 4, 5, then 6
```

# Ignoring prototypes

* If procedure gets invoked with `&`, then its prototype is ignored:

```perl
sub f ($$) {
  print @_ ;
}

f ((1,2),(3,4)) ;    # prints 2, then 4

&f ((1,2),(3,4)) ;   # prints 1, 2, 3 ,4
```

* While it is possible to provide prototypes to anonymous procedures, these are also ignored:

```perl
$f = sub ($$) {
  print @_ ;
};

$f->((1,2),(3,4)) ;   # prints 1, 2, 3, 4
```

* From this test, it seems that prototype information is not stored with the procedure itself, but rather, it is information associated with a specific procedure name, and available only during parsing.
