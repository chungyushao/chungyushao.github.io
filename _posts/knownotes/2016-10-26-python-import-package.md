---
layout: post
title: "Python import related"
excerpt: "module, import, __main__, package"
categories: knownotes
tags: [python]
modified: 2016-10-24
comments: true
share: true
author: chungyu
---

> mainly from
> * [Python 2.7 doc, 6. Modules](https://docs.python.org/2.7/tutorial/modules.html#packages)


# Basics

* A module is a file containing Python definitions and statements.
* The file name is the module name with the suffix `.py` appended. 
* A module can contain executable statements as well as function definitions. 
  * These statements are intended to initialize the module. 
  * They are executed **only the first time the module name is encountered in an import statement.** 
* Each module has its own private symbol table, which is used as the global symbol table by all functions defined in the module.
* The imported module names are placed in the importing module’s global symbol table.

# `import`
> * For efficiency reasons, **each module is only imported once per interpreter session.** 
>   * if you change your modules, you must restart the interpreter

`from fibo import fib, fib2`
  * This does not introduce the module name from which the imports are taken in the local symbol table 
  * In the example, `fibo` is not defined.

`from fibo import *`
  * This imports all names except those beginning with an underscore `_`.
  * In general the practice of `import *` from a module or package is not recommened.


# `__name__`
```python
# module file: fibo.py
import fibo
print fibo.__name__ # 'fibo'
```

# Executing modules as scripts

if you defined
```python
if __name__ == "__main__":
    import sys
    fib(int(sys.argv[1]))
``` 
at the end of your module, you can make the file usable as a script as well as an importable module,
because **the code that parses the command line only runs if the module is executed as the “main” file.**

So you can do
`python fibo.py 50` to make the file usable as a script as well as `import fibo` 
  * If the module is imported, the code is not run 


# The Module Search Path

`import spam`

1. The interpreter first searches for a built-in module with that name. 
2. If not found, it then searches for a file named `spam.py` in a list of directories given by the variable `sys.path`.

# import package

`from package import item`
	
* the item can be either a submodule (or subpackage) of the package, or some other name defined in the package, like a function, class or variable. 
* The import statement 
	* first tests whether the item is defined in the package; 
	* if not, it assumes it is a module and attempts to load it. 
	* If it fails to find it, an ImportError exception is raised.

`import item.subitem.subsubitem`
	
* each item except for the last must be a package; 
* the last item can be a module or a package but can’t be a class or function or variable defined in the previous item.


```bash
sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```

* The __init__.py files are required to make Python treat the directories as containing packages
	* In the simplest case, __init__.py can just be an empty file, but it can also execute initialization code for the package or set the __all__ variable	
* `import sound.effects.echo`
	* It must be referenced with its full name: `sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)`	
* `from sound.effects import echo`
	* This also loads the submodule echo, and makes it available without its package prefix
	* `echo.echofilter(input, output, delay=0.7, atten=4)`
* `from sound.effects.echo import echofilter`
	* `echofilter(input, output, delay=0.7, atten=4)`

#  Importing * From a Package

 * if a package’s `__init__.py` code defines a list named `__all__`, it is taken to be the list of module names that should be imported when `from package import *` is encountered. 
 * It is up to the package author to keep this list up-to-date when a new version of the package is released. * 

 * For example, in the file sound/effects/__init__.py 
`__all__ = ["echo", "surround", "reverse"]`
	* This would mean that `from sound.effects import *` would import the three named submodules of the sound package.


