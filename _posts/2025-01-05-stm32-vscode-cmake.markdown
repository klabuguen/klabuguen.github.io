---
layout: post
title:  "Setting up the STM32 with VSCode and CMake"
date:   2025-01-05 00:08:00 -0800
categories: 365-days-of-code workspace
---
I've been rusty in regards to C pointer fundamentals and decided to review pointer arithmetic. One can easily be thrown off by pointer arithmetic because adding 1 to a ***pointer*** is not as simple as adding two numbers in ***standard arithmetic***. For example:

We know that in ***standard arithmetic***:

$$ 1 + 1 = 2 $$

$$ 1 + 2 = 3 $$

However, ***pointer arithmetic*** may not be as intuitive. 

Say that we have an array, `arr`, holding 3 elements `10`, `20`, and `30` :

```
uint32_t arr[3] = {10, 20, 30};
uint32_t *arrPtr = arr;
printf("The array starts at address %p\n", arrPtr);
printf("The second value in the array starts at address %p\n", arrPtr + 1);
printf("The third value in the array starts at address %p\n", arrPtr + 2);
```


**Question:** What do you think would be printed out given the diagram below?
###### Memory Layout (Starting at Address 0x5000)
```
0x5000  +--------------------+
0x5001   |	    Element 1     |
0x5002   |         10         |
0x5003   |      (4 bytes)     |
0x5004  +--------------------+
0x5005   |	    Element 2     |
0x5006   |         20         |
0x5007   |      (4 bytes)     |
0x5008  +--------------------+
0x5009   |	    Element 3     |
0x500A   |         30         |
0x500B   |      (4 bytes)     |
0x500C  +--------------------+
```


If you guessed: 
```
The array starts at address 0x5000
The second value in the array starts at address 0x5004
The third value in the array starts at address 0x5008
```

...then you are correct!

Addition and subtraction in pointer arithmetic is done in chunks of the size of the data type that the pointer is pointing to. Because each element is *32 bits*, or *4 bytes*, the pointer address increases by **4 bytes** every time 1 is added.

Thus, anytime you add or subtract from a pointer address:
$$ ptrAddr = ptrAddr \pm a \times sizeof(ptrType) $$
Where `a` is the value that you want to increment or decrement by, ptrAddr is the current pointer address, and ptrType is the data type that the pointer is pointing to. 

### Exercises:
```
int32_t num = 55;
int *ptr = &num;

(1) printf("%d", num);
(2) printf("%p", &num);
(3) printf("%p", ptr);

*ptr = 20;

(4) printf("%d", num);
```