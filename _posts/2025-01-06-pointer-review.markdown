---
layout: post
title:  "C Pointer Review"
date:   2025-01-06 00:08:00 -0800
categories: 365-days-of-code c-fundamentals
---
# Day 6:  Pointer Review
#c-fundamentals 
## `sizeof()` Operator
 `sizeof()` is a ***compile-time operator*** in C that determines the size of a data type, variable, or object. It is ***not*** a function and it is ***not*** part of a library! The `sizeof()` operator is built into the C language, and the compiler translates `sizeof()` into the data type or variable size passed into the operator during compilation.

The `sizeof()` operator can be useful when dynamically allocating memory based on the size of a data type. For example, the following line of code may be used to dynamically allocate memory for an array of 10 integers:

```
int *arr = malloc(10 * sizeof(int));
```

#### Examples:
Examples (1)-(3) may be fairly straight forward, but (4) & (5) can be a bit tricky.
```
(1) printf("(1) size = %d\n", sizeof(int));
(2) printf("(2) size = %d\n", sizeof(double));
(3) printf("(3) size = %d\n", sizeof(char));
(4) printf("(4) size = %d\n", sizeof('a'));
(5) printf("(5) size = %d\n", sizeof("a"));
```

After running the example code, I get the output below. Here, an `int` is of size 4 (bytes), `double` is 8 bytes,  and `char` is 1 byte.  What may be surprising to some, is that **(3)** `sizeof('a')` is 4 bytes and **(4)** `sizeof("a")` is 2 bytes. 

```
(1) size = 4
(2) size = 8
(3) size = 1
(4) size = 4
(5) size = 2
```

(3) `sizeof('a')` is 4 bytes because `'a'` is considered a ***character literal***, which in C/C++, has the type `int` ***not*** `char`. 

(4) `sizeof("a")` is 2 bytes because `"a"` is considered a ***string literal***, which in C/C++, is represented as an array of `char`. Thus, `sizeof("a")` evaluates to 2 bytes because the string literal contains both character `a` and `\0`.

## Multiple Indirection
***Multiple indirection*** refers to pointers that point to other pointers, aka "pointers to pointers". (1) is an example of a single indirection, while (2) is a pointer to a pointer that holds the memory address of another pointer.

```
int a = 55;
(1) int *ptr1 = &a;
(2) int **ptr2 = &ptr1;
```

## General Pointer
### What is `void*`?
`void*` is a universal pointer used in the development of general purpose functions where  pointers can point to any data type (e.g. integer, float, double, etc).  For example:

```
int a = 10;
void *ptr = &a;
```

#### Casting and Dereferencing:
The `void*` pointer can be used to hold the address of any data type, but cannot be dereferenced without casting to a specified type. Attempting to dereference a `void*` pointer will result in a compilation error.   
`
```
printf("value of *ptr: %d\n", *ptr); // Wrong, compiler error
```

```
ERROR!
/tmp/MBJKsMGZpU/main.c: In function 'main':
/tmp/MBJKsMGZpU/main.c:13:35: warning: dereferencing 'void *' pointer
   13 |     printf("value of *ptr: %d\n", *ptr);
      |                                   ^~~~
/tmp/MBJKsMGZpU/main.c:13:35: error: invalid use of void expression


=== Code Exited With Errors ===
```

The **correct** syntax is:

```
printf("value of *ptr: %d\n", *(int*)ptr); // Correct!
```

```
value of *ptr: 10


=== Code Execution Successful ===
```