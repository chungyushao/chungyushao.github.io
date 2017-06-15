---
layout: post
title: "Perl IO"
excerpt: ""
categories: scriptlang
tags: [perl]
comments: true
share: true
author: chungyu
---
> [A guide to Perl: By experiment](http://matt.might.net/articles/perl-by-example/)

# Input and output

* In Perl, input and output operations are associated with filehandles.
* `STDIN` is an input filehandle available by default, and it refers to user input coming from the console.
* `STDOUT` is an output filehandle available by default, and writing to it will send output ot the console.

* To **read from** a filehandle, wrap it with `<>`:
  * `print <STDIN> ;`:  prints the first line of user input
* Every read from a filehandle implicitly assigns the input to the default variable, `$_`.
  * Consequently, the following statement: `<STDIN> ;` has the same effect as the statement: `$_ = <STDIN> ;`
  * So, the following program also works:

```perl
<STDIN> ;
print $_ ;  # prints the first line of user input
```


* In fact, `print` uses the default `$_` if no arguments are given, so the following program works as well:

```perl
<STDIN> ; # puts the first line in $_
print ;   # prints the value in $_
```

* The `open` operator can establish new filehandles; `close` closes them.

> Oddly, filehandles can be barewords:

```perl
open F, "<io.pl";  # opens io.pl for reading as F

while (<F>) {
  print ;
}                  # prints contents of io.pl

close F ;          # closes filehandle F
```

* Filehandles can also be stored in scalar variables:

```perl
open $fh, "<tmp.txt" ;

while (<$fh>) {
  print ;
} # prints contents of tmp.txt

close $fh ;
```


* This certainly makes it more convenient to pass a filehandle to a procedure.
* Bareword filehandles must be treated as `typeglobs`:

```perl
sub pass_handle {
  print "file handle: " . $_[0] . "\n" ;
}

open F, "<tmp.txt" ;
pass_handle *F ;         # prints file handle: *main::F
pass_handle F ;          # error
close F ;
```

* But, scalar filehandles need no special treatment, as expected:

```perl
open $fh, "<tmp.txt" ;
pass_handle $fh;         # prints file handle: GLOB(0xAddr)
close $fh ;
```

* To accept a truly bareword filehandle as an argument, it becomes necessary to use the rarely used `*` prototype specifier, which creates an alias to the bareword filehandle in the symbol table:

```perl
sub pass_handle2 (*) {
  print "file handle: " . $_[0] . "\n" ;

  *FH = $_[0] ;
  while (<FH>) {
    print ;
  }
}

open F, "<tmp.txt" ;
pass_handle2 F;      # prints file handle: F, contents of tmp.txt
close F ;
```


* The print command is special, in that it can take a filehandle before it takes any parameters:

```perl
open $tmp, ">tmp.txt" ; # opens tmp.txt for writing
print $tmp "Testing" ;  # writes Testing to tmp.txt
close $tmp ;            # closes the filehandle


open $tmp, "<tmp.txt" ; # opens tmp.txt for reading

while (<$tmp>) {
  print STDOUT $_ ;             
}                      # prints Testing to STDOUT

close $tmp ;            # closes the filehandle
```


* By default, `print` and `write` send to `STDOUT`, but you can change the default with select:

```perl
open $tmp, ">>tmp.txt" ;
select $tmp ;
print " and such.\n" ; # appends and such to tmp.txt
close $tmp ;
```

* When opening a file, the second argument to open determines the mode in which it is opened:
  * `open FH, "<file"`: opens a file for reading.
  * `open FH, ">file"`: opens a file for writing, and will replace contents.
  * `open FH, ">>file"`: opens a file for appending.

* This is not meant as a tutorial on the library, but open also has a three-argument form:
  * `open FH, "<", "file"` opens a file for reading.
  * `open FH, ">", "file"` opens a file for writing, and will replace contents.
  * `open FH, ">>", "file"` opens a file for appending.

> Seasoned Perl programmers tell me that three-argument open is preferred, and that bareword filehandles are strongly discouraged in favor of scalars.
