---
layout: post
title:  "C Pointer Review"
date:   2025-01-06 00:08:00 -0800
categories: c-fundamentals
---
# Pointer Exercises

## 1. Swap Two Integers
*Implement a function that swaps two integers.*

In the function below, I take in two pointers `int *a` and `int *b` as arguments, then create a temporary variable to store the value in `a`. Next, I store the value of `b` in `a`, then store `tmp` in `b`.

```
void swapInts(int *a, int *b){
    int tmp = *a;
    *a = *b;
    *b = tmp;
}
```

## 2. Swap Two Integers without a `tmp`Variable
*Implement a function that swaps two integers without using a third `tmp` variable. Try to use addition/subtraction and multiplication/division.*

**Addition/Subtraction**

```
void swapIntsNoThird1(int *a, int *b){
    *a = *a + *b;
    *b = *a - *b;
    *a = *a - *b;
}
```

**Multiplication/Division**

```
void swapIntsNoThird2(int *a, int *b){
    *a = *a * *b;
    *b = *a / *b;
    *a = *a / *b;
}
```

## 3. Generic Swap Function

*Implement a function that swaps the values of two unknown data types, given the size of the data type.*

The function below takes in two generic  `void *` pointers as arguments as well as the size of the data type. `malloc()` is used to dynamically allocate memory of `size` for variable `tmp`. The `memcpy()` function provided by the `string.h` library copies the value in `a` to `tmp`, `b` to `a`, and `tmp` to `b`. Finally, the dynamic memory is deallocated/released at the end of the function in order to prevent potential memory leaks.

```
void genericSwap(void *a, void *b, int size){
    void *tmp = malloc(size);
    memcpy(tmp, a, size);
    memcpy(a, b, size);
    memcpy(b, tmp, size);
    free(tmp);
}
```

- Note to self: How would I solve this without memcpy?

## 4. Search if an Element is in a Given Array

*Implement a function that returns true if it exists in a given array, otherwise return false.*

This is a fairly straight forward question, but I included it in my list of pointer exercises as a reminder that an ***array degrades to a pointer*** to the first element of the array. An alternative way of writing the function signature is:

```
bool search(int arr[], int element, int size);
```

In the function I've implemented below, I pass in 1) a pointer to the first element in `arr`, 2) the `element` I want to search for, and finally, 3) the number of elements (or `size`) of the array. I simply loop through all elements in the array and check to see if the current element is equal to `element` - if it is, then I `return true.` If the element is not present in the array, then I `return false`. 

```
bool search(int *arr, int element, int size){
    for(int i = 0; i < size; i++){
        if(arr[i] == element){
            return true;
        }
    }
    return false;
}
```

## 5. Swap Two Arrays of the Same Size

*Implement a function that swaps the values of two arrays of the same (known) size.*

- **Solved in O(n) Time:**
There are multiple ways to solve this! This can actually be solved using the `genericSwap()` function in section (3) above. Another way is to use `swapInts()` implemented above in section (1) and pass each element by reference into the function I've implemented below `swapArrayOn().

```
void swapArrOn(int *arr1, int *arr2){
    for(int i = 0; i < SIZE; i++){
        swapInts(&arr1[i], &arr2[i]);
    }
}
```

- **Solved in O(1) Time:**
Another way to solve this problem is by utilizing dynamically allocated arrays and multiple indirection. In the snippet of code below, `swapArrO1()` takes in `void **a` and `void **b` as arguments. The function 1) stores the address of `a` in the temporary `void*` variable `tmp`, 2) assigns the address of `b` to `a`, and finally 3) assigns the address of `tmp` to `b`. The addresses of `a` and `b` are swapped, essentially swapping the array values.

I provide an example of how the function can be used: `int *a` and `int *b` are dynamically allocated arrays that are later initialized in the `for` loop. A key concept to keep in mind is that the dynamically allocated arrays store the pointer to the allocated memory (on the heap) in variables `int *a` and `int *b`. Swapping the pointers stored in `int *a` and `int *b` changes where `a` and `b` point to without changing the underlying data. A statically allocated array is stored in a fixed location in memory (in the stack) -- its base address is immutable and cannot be modified. Thus, swapping two pointers that point to statically allocated arrays is not allowed.

Although `a` and `b` are of type `int*`, `&a` and `&b` are of type `int**`. I simply cast `&a` and `&b` to the generic pointer to pointer `void**` in order to comply with the `swapArrO1()` function signature.

```
void swapArrO1(void **a, void **b){
    void *tmp = *a;
    *a = *b;
    *b = tmp;
}

int main() {
    int *a = (int*)malloc(SIZE * sizeof(int));
    int *b = (int*)malloc(SIZE * sizeof(int));

	// Initialize array a and array b
    for(int i = 0; i < SIZE; i++){
        a[i] = i;
        b[i] = i * 10;
    }
    
    swapArrO1((void**)&a, (void**)&b);
    return 0;
}
```

```
array a: 0 10 20 30 40 
array b: 0 1 2 3 4 


=== Code Execution Successful ===
```