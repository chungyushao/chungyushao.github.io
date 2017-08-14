---
layout: post
title: "Perl regular expression and substitution"
excerpt: ""
categories: scriptlang
tags: [perl]
comments: true
share: true
author: chungyu
---
> [A guide to Perl: By experiment](http://matt.might.net/articles/perl-by-example/)

# Quote operators: Strings and regex

* Perl is filled with quote forms and quote-like operators.
* Singly-quoted strings are literal strings, **with no interpolation**:
  * `print 'This is a $string' ;  # prints This is a $string`

* There is an “quote operator” form for non-interpolated strings: `q`:
  * A quote operator takes a delimiter character, and then it looks for a matching delimiter.
  * Most characters act as their own matching delimiter, but the balanced delimiters `<`, `(`, `{` and `[` match with `>`, `)`, `}` and `]` respectively.

```perl
print q(This is a $string.) ;   # prints This is a $string.
print q{This is a $string.} ;   # prints This is a $string.
print q|This is a $string.| ;   # prints This is a $string.
print q<This is a $string.> ;   # prints This is a $string.
print q/This is a $string./ ;   # prints This is a $string.
print q#This is a $string.# ;   # prints This is a $string.
print q"This is a $string." ;   # prints This is a $string.
print q zThis is a $string.z ;  # prints This is a $string.
```

* If a balanced delimiter is in use, it **all internal uses of those balanced delimiters must be balanced**:
  * `print q(This (and this) run.) ;  # prints This (and this) run.`
  * `print q(This (and this fails.) ; # error`


* The advantage of quote operators is that `'` and `"` do not have to be escaped within them (unless of course, they were chosen as the delimiter character):

```perl
print 'I don\'t like escaping.' ; # prints I don't like escaping.
print q(I don't like esacping.) ; # prints I don't like escaping.
```

# Strings and interpolation

* Double-quoted strings are literal strings, but with interpolation:

```perl
$pi = 3.14 ;
@a = ("of","a","circle") ;
print "$pi is the circumference @a over its diameter." ;
 # prints 3.14 is the circumference of a circle over its diameter.
```

* When an array variable appears within an string, it expands, by default, with single spaces between its elements.
* But, it is possible to change the separator by assigning it to the special variable `$"`:

```perl
@array = (1,2,3) ;

{
  local $" = '::' ;
  print "@array" ;    # prints 1::2::3
}

print "@array" ;
```

* The quote operator form of double quotes is `qq`:

```perl
$string = "dog" ;
print qq(This is a $string.) ;   # prints This is a dog.
print qq{This is a $string.} ;   # prints This is a dog.
print qq[This is a $string.] ;   # prints This is a dog.
print qq<This is a $string.> ;   # prints This is a dog.
print qq|This is a $string.| ;   # prints This is a dog.
print qq/This is a $string./ ;   # prints This is a dog.
print qq#This is a $string.# ;   # prints This is a dog.
print qq"This is a $string." ;   # prints This is a dog.
print qq zThis is a $string.z ;  # prints This is a dog.
```

* If necessary for differentiation, interpolated variables can be delineated with `{}`:

```perl
$prefix = "bi" ;

print "$prefixmodal" ;   # prints nothing, $prefixmodal not a var
print "${prefix}modal" ; # prints bimodal
```

* String interpolation even attempts array and hash indexing:

```perl
@registries = (42,1701) ;

print "NCC-$registries[1]" ;  # prints NCC-1701
```

# Backtick: Shell process expansion

* Backtick quotes \`, work like they do in bash: they execute the shell command, and evaluate to its output as a Perl value:

```perl
print `ls` ; # prints all files in the current directory
```

* In an list context, it splits the output along newlines, unless the variable `$/` is set to a different separator:

```perl
@files = `ls` ;
foreach $file (@files) {
  chomp $file ;            # remove newline from end of $file
  print "file: $file" ;
} # prints each file, but with file: first


{
  local $/ = ':';
  @last_user = `tail -1 /etc/passwd` ;
  print "@last_user" ; # prints passwd entry for the last user,
                       # with space after each :

  # looks like:
  # robot: *: 239: 239: robot: /var/empty: /usr/bin/false
}
```

* The quote operator form of backtick expansion is `qx`:

```perl
@files = qx|ls| ;
foreach $file (@files) {
  chomp $file ;            # remove newline from end of $file
  print "file: $file" ;
} # prints each file, but with file: first
```

* As with doubly-quoted strings, interpolation works on backtick expanded quote forms as well:

```perl
$passwd = '/etc/passwd';
$password_file = `cat $passwd` ;
print $password_file ;            # Prints contents of /etc/passwd
```

* But, using the `qx` quote operator with `'` as a delimiter will not interpolate:

```perl
$user = `echo $USER` ;     # runs echo with no args
print $user ;              # prints nothing

$user = qx'echo $USER' ;   
print $user ;              # prints value of shell var $USER
```

* Quote operators that allow interpolation (except `qq` itself) disable interpolation when the delimiter is the single quote, `'`.

### Quote words

* For quickly creating an array of whitespace-separated words, the quote operator `qw` is convenient:

```perl
@names = qw(Harry     Larry
 Moe) ;
print "@names" ; # prints Harry Larrry Moe
```

# Regular expressions

* The default quote for a matching regular expression operation is `/`.
* A regular expression, by default, attempts to match against the contents of the default variable, `$_`.

```perl
$_ = "foobar" ;
print "yes" if /foo/ ; # prints yes
print "yes" if /bar/ ; # prints yes
print "no"  if /baz/ ; # does not print
```

* The matching operator `=~` allows testing against a specific variable:

```perl
$fb = "facebook" ;
print "yes" if $fb =~ /face/ ;  # prints yes
print "yes" if $fb =~ /book/ ;  # prints yes
print "no"  if $fb =~ /apple/ ; # does not print
```

* If the right-hand side is not a regular expression quote, then run-time value of the expression is dynamically interpreted as a regular expression:

```perl
$fb = "facebook" ;
$face = "face" ;
$book = "bo+k" ;
$apple = "ap*le" ;
print "yes" if $fb =~ $face ;   # prints yes
print "yes" if $fb =~ $book ;   # prints yes => dynamically interpreted as a regular expression
print "no"  if $fb =~ $apple ;  # does not print => dynamically interpreted as a regular expression
```


* The matching quote operator `m` allows one to change the quotes on a matching regular expression:

```perl
$fb = "facebook" ;
print "yes" if $fb =~ m(face) ;  # prints yes
print "yes" if $fb =~ m|book| ;  # prints yes
print "no"  if $fb =~ m"apple" ; # does not print
```

* To quote (and potentially invest time optimizing) a regular expression for later use (rather than match with it immediately), use the `qr` quote operator:

```perl
$fb = "facebook" ;
$face = qr{face} ;
$book = qr|bo+k| ;
print "yes" if $fb =~ $face ;      # prints yes
print "yes" if $fb =~ $book ;      # prints yes
print "no"  if $fb =~ qr/ap*le/ ;  # does not print
```


* With regular expression quotes, interpolation of variables is allowed, as it is in doubly-quoted stings:

```perl
$rx = "foo|bar" ;

print "match" if "foobar" =~ /$rx/ ;     # prints match
print "match" if "foobar" =~ /x${rx}x/ ; # prints nothing
                                         # tries to match xfoo|barx
```

### Extracting matches from regular expressions

Directly after a successful match, Perl binds variables to submatches.

Parentheses do more than dictate precedence; they indicate submatches.

The nth leftmost parenthesis denotes the nth submatch, and the variable $n holds the nth submatch:

$words = "foo bar baz qux" ;
$words =~ /(\w+) (\w+) (\w+) (\w+)/ ;

print $1 ; # prints foo
print $2 ; # prints bar
print $3 ; # prints baz
print $4 ; # prints qux


$head = "## This is a title [ref] ##" ;
if ($head =~ /^##[ ]*((\w|\s)+)\s*\[(\w*)\][ ]*##$/) {
  print $1 ; # prints This is a title
  print $3 ; # prints ref
} else {
  print "No match!"
}
The entire matched segment is availale in the variable $&:

$in = "foobarrrrrrrrrrrrbaz" ;
$in =~ /bar*/ ;
print $& ; # prints barrrrrrrrrrrr
Regular expression modifiers

Regular expression quotes may be directly followed flags that modify both the parsing and the behavior of the regular expressions.

The multiline modifier m modifies the behavior of the anchors ^ and $ so that each can match where a linebreak happens:

$in = "foo\nbar\nbaz" ;

print "no"  if $in =~ /^bar$/ ;  # prints nothing
print "yes" if $in =~ /^bar$/m ; # prints yes
The “single line” modifier s changes the behavior of . so that it can match newline:

$in = "foo\nbar" ;
print "no"  if $in =~ /foo.bar/ ;  # prints nothing
print "yes" if $in =~ /foo.bar/s ; # prints yes
The p modifier copies the string prior to the match, the matched string and the string after the match into ${^PREMATCH}, ${^MATCH} and ${^POSTMATCH} respectively:

$in = "This is foo." ;
$in =~ /foo/p;
print ${^PREMATCH} ;   # prints This is
print ${^MATCH} ;      # prints foo
print ${^POSTMATCH} ;  # prints .
The case-insensitive modifier i ignores case according to the current locale:

$in = "fooBAR" ;
print "no"  if $in =~ /foobar/ ;  # prints nothing
print "yes" if $in =~ /foobar/i ; # prints yes
The modifier x ignores whitespace and comments in the pattern:

$in = "foobar" ;
print "no"  if $in =~ /foo bar/ ;  # prints nothing
print "yes" if $in =~ /foo bar/x ; # prints yes
This permits nicely document regular expressions:

$ipchunk = qr{(
    [0-9]        # 0   - 9
 |  [1-9][0-9]   # 10  - 99
 |  1[0-9][0-9]  # 100 - 199
 |  2[0-4][0-9]  # 200 - 249
 |  25[0-5]      # 250 - 255
)}x ;

print "no"  if "256" =~ /^$ipchunk$/ ; # prints nothing
print "yes" if "255" =~ /^$ipchunk$/ ; # prints yes
In an list context, the “global” modifier g returns an array of all matches within the search string:

$in = "123,456,789";
@allmatches = ($in =~ /\d+/g) ;
print $allmatches[0] ; # prints 123
print $allmatches[1] ; # prints 456
print $allmatches[2] ; # prints 789
In a scalar context, the modifier g causes the match operator to remember matches, and return each one successively for each evaluation:

$in = "123,456,789";
while ($in =~ /(\d+)/g) {
  print $1 ;
} # prints 123, then 456, then 789
The special pattern \G matches the last match point on a per-string basis. The current procedure pos yields the current match point for string:

$in = "123,456,789";
print pos $in ;   # prints nothing
$in =~ /(\d+)/g ;
print pos $in ;   # prints 3
$in =~ /(\d+)/g ;
print pos $in ;   # prints 7
$in =~ /(\d+)/g ;
print pos $in ;   # prints 11
The procedure pos can also set the last match point for a string:

$in = "123,456,789";
$in =~ /(\d+)/g ;
print $1;            # prints 123
$in =~ /(\d+)/g ;
print $1 ;           # prints 456
pos($in) = 3 ;
$in =~ /(\d+)/g ;
print $1      ;      # prints 456
Caution must be taken, because while the last-match position is held per-string copying the string will reset it:

$in = "123,456,789";
$in =~ /(\d+)/g ;
print pos $in ;       # prints 3
$inref = \$in ;
print pos ${$inref} ; # prints 3
$in2 = $in ;
print pos $in2 ;      # prints nothing!
The modifier c, in conjunction with g, does not reset the match position for the string on a failed match.

This makes it easy to build lightweight lexical analyzers:

$in = "if (cond) { print ; }" ;

$in =~ /^/g ;
while (1) {
  print "IF"    if $in =~ /\Gif/gc ;
  print ""      if $in =~ /\G\s+/gc ;
  print "PRINT" if $in =~ /\Gprint/gc ;
  print "ID"    if $in =~ /\G\w+/gc ;
  print "LP"    if $in =~ /\G\(/gc ;
  print "RP"    if $in =~ /\G\)/gc ;
  print "LB"    if $in =~ /\G\{/gc;
  print "RB"    if $in =~ /\G\}/gc ;
  print "SEMI"  if $in =~ /\G;/gc ;
  last if $in =~ /\G$/gc ;
}

# prints
# IF
#
# LP
# ID
# RP
#
# LB
#
# PRINT
#
# SEMI
#
# RB
Substitution operators

Perl borrows and significantly extends sed’s substitution quote operator, s.

The general form for substitution is s/pattern/replacement/modifiers.

The balanced delimiter forms must balance pattern and replacement:

$in = "foo" ;
$in =~ s{foo}{bar} ;
print $in ; # prints bar
The s operator returns true if a substitution succeeds, and false otherwise:

As with the match operator m, it operatos on $_ by default, and it places the result in $_ by default, but the =~ operator can force it to operator on a different variable:

$_ = "foo" ;
s/foo/bar/ ;
print $_ ;          # prints bar

$in = "foo" ;
$in =~ s/foo/bar/ ;
print $in ;         # prints bar
The same flags that apply to the match quote operator m also work with substitution:

$in = "This is a foo foo." ;
$in =~ s/foo/bar/ ;
print $in ; # prints This is a bar foo.

$in = "This is a foo foo." ;
$in =~ s/foo/bar/g ;
print $in ; # prints This is a bar bar.
The submatch variables $n are visible in the replacement:

$in = "Triple: this" ;
$in =~ s/(Triple: )(\w+)/$1$2$2$2/ ;
print $in ; # prints "Triple: thisthisthis"
Since the s operator destroys its target string by default, it also accepts an r modifier which causes it to (non-destructively) return the result instead:

$in = "foo" ;
$out = ($in =~ s/foo/bar/r) ;
print $in ;   # prints foo
print $out ;  # prints bar
