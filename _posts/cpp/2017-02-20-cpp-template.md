---
layout: post
title: "C++ Template"
excerpt: "Templated class, Templated functions"
categories: cpp
tags: [C++]
comments: true
share: true
author: chungyu
---

> * [Templates and Template Classes in C++ By Alex Allain](http://www.cprogramming.com/tutorial/templates.html)
> * [Templated Functions By Alex Allain](http://www.cprogramming.com/tutorial/templated_functions.html)
> * [@isocpp](https://isocpp.org/wiki/faq/templates)

# Templated Class
* Templates are a way of making your classes more abstract by letting you **define the behavior of the class without actually knowing what datatype will be handled by the operations of the class**.
* The ability to have a single class that can handle several different datatypes means the code is easier to maintain, and it makes classes more reusable.

##### Declaring templated class
`template <typename T> class a_class {...};`

##### Declaring member function of templated class
`template<typename T> void a_class<T>::a_function(){...}`

##### Declaring an instance of a templated class
`a_class<int> an_example_class;`


##### Using `template <class T>` or `template <typename T>`?
> [stackoverflow](http://stackoverflow.com/questions/213121/use-class-or-typename-for-template-parameters)
> * The committee introduced a new keyword `typename` to resolve syntactic ambiguity.
> * For backward compatibility, `class` kept its overloaded meaning.


# Templated Functions

`template <typename type> type func_name(type arg1, ...);`

```c++
template <typename type> type add(type a, type b) {
    return a + b;
}

int x = add<int>(1, 2);
```

```c++
template <typename rettype, typename argtype> rettype cast(argtype x) {
    return (rettype)x;
}

double d_num = cast<double>(10);
```

# Templated Classes with Templated Functions

```c++
template <typename type1> class TClass {
    template <typename type2> type2 myFunc(type2 arg);
};
//
template <typename type>  // For the class
template <typename type2>  // For the function
type2 TClass<type>::myFunc(type2 arg) {
    // code
}
```

# [Splitting templated C++ classes into .hpp/.cpp files--IS NOT POSSIBLE](http://stackoverflow.com/questions/1724036/splitting-templated-c-classes-into-hpp-cpp-files-is-it-possible)
> from [reply of Sharjith N., Michael Wildermuth](): It is not possible to write the implementation of a template class in a separate cpp file and compile. All the ways to do so, if anyone claims, are workarounds to mimic the usage of separate cpp file but practically if you intend to write a template class library and distribute it with header and lib files to hide the implementation, it is simply not possible.

* The header files are never compiled. They are only preprocessed.

##### First, why separating interface from implementation?
* The main objective: hiding implementation details in binary form. Why?
  * This is where you must separate the data structures and algorithms. So?

* Your template classes must represent only data structures not the algorithms. This enables you to hide more valuable implementation details in separate non-templatized class libraries, the classes inside which would work on the template classes or just use them to hold data.
* The template class would actually contain less code to assign, get and set data. Rest of the work would be done by the algorithm classes.


##### So why not?
  * The preprocessed code is then clubbed with the cpp file which is actually compiled.
* If the compiler has to generate the appropriate memory layout for the object it needs to know the data type of the template class.
  * It must be understood that template class is not a class at all but a template for a class
  * the declaration and definition of this templated class should be generated by the compiler at compile time after getting the information of the data type from the argument.
* As long as the memory layout cannot be created, the instructions for the method definition cannot be generated.
  * All class methods are converted into individual methods with name mangling and the first parameter as the object which it operates on. The `this` argument is which actually tells about size of the object
  * template class is unavailable for the compiler unless the user instantiates the object with a valid type argument
* if you put the method definitions in a separate cpp file and try to compile it the object file itself will not be generated with the class information. The compilation will not fail, it would generate the object file but it won't generate any code for the template class in the object file. This is the reason why the linker is unable to find the symbols in the object files and the build fails.  
