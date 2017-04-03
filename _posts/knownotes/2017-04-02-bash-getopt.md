---
layout: post
title: "Bash Scripts arguments `getopts`"
excerpt: ""
categories: knownotes
tags: [bash]
comments: true
share: true
author: chungyu
---
> [Small getopts tutorial](http://wiki.bash-hackers.org/howto/getopts_tutorial)


# Option without arguments

```bash
#!/bin/bash

while getopts ":a" opt; do
  case $opt in
    a)
      echo "-a was triggered!" >&2
      ;;
    \?)
      echo "Invalid option: -$OPTARG" >&2
      ;;
  esac
done
```

* **invalid options don't stop the processing**: If you want to stop the script, you have to do it yourself (exit in the right place)
* **multiple identical options are possible**: If you want to disallow these, you have to check manually (e.g. by setting a variable or so)


# Option with arguments

```bash
#!/bin/bash

while getopts ":a:" opt; do
  case $opt in
    a)
      echo "-a was triggered, Parameter: $OPTARG" >&2
      ;;
    \?)
      echo "Invalid option: -$OPTARG" >&2
      exit 1
      ;;
    :)
      echo "Option -$OPTARG requires an argument." >&2
      exit 1
      ;;
  esac
done
```

# Syntax
* `getopts OPTSTRING VARNAME [ARGS...]`
  * `OPTSTRING`	tells `getopts` which options to expect and where to expect arguments
  * `VARNAME`	tells `getopts` which shell-variable to use for option reporting
  * `ARGS`	tells `getopts` to parse these optional words instead of the positional parameters

* Every option character is simply named as is: `getopts fAx VARNAME` means scripts takes `-f`, `-A`, `-x`
* When you want getopts to expect an argument for an option, just place a `:` (colon) after the proper option flag.
  * `getopts fA:x VARNAME`: need `-A SOMETHING`
