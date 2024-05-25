+++
title = 'C++'
date = 2024-03-18T09:50:39+10:00
draft = true
+++

# C++

## Pointers
[tutorial](https://www3.ntu.edu.sg/home/ehchua/programming/cpp/cp4_PointerReference.html)
Computer memory stores *contents* at an  *address*. Each address typically holds 8-bit (1 byte) of data. C introduces *variables* which is a named location that can store a value of a particular type (`int`, `double`, `char`).

This means that a 32-bit system will require 32/8 = 4 memory locations to store an int.

### Pointer Variables
A pointer variable is a variable that stores the memory address of some data.

### Declaring Pointers
You need to declare a pointer before they can be used.
```cpp
type *ptr
type* ptr
type * ptr
```

### Initializing Pointers via Address-Of Operator (&)
Once you've declared the pointer, you can then point it to a valid address. Prior to initializing it'll be pointing to "somewhere" which might not be a valid location.
The `address-of operator (&)` gets the address of the variable so you can use it to get the address of a variable and assign this address to a pointer variable.
```cpp
int number = 99;
int * pNumber;
pNumber = &number;
```

### Indirection or Dereferencing Operator (*)
After initializing, you can now use the return the value stored at the pointer using the `indirection operator (dereferencing operator) (*)`.
```cpp
int number = 00;
int * pNumber = &number; // you can do the declaration and initialization in the same line
cout << *pNumber << endl;
*pNumber = 99; // you can reassign the value of number using the deference of the pointer
```

### Pointer Types
Pointers are given types on declaration. This constraints them to only holding the address of that type of initialization and not any other type.
```cpp
int i = 88;
double j = 33.33;
int * iPtr = &i;
*iPtr = j; // THIS WILL ERROR!
iPtr = &J; // THIS WILL ERROR TOO!;
iPtr = i; // THIS WILL ERROR AS WELL BECAUSE iPtr can only hold the address of an int!
```  

### Uninitialized Pointers
Pointers that have not been initialized will have a serial logical error (might be caught at compile time but if not then highly dangerous!).
```cpp
int * iPtr;
*iPtr = 55;
cout << *iPtr << endl;
```
The 55 is saved "somewhere" which is an unknown location that could corrupt existing data/cause a memory leak/be a vulnerability.

### Null Pointers
It is good practice to initialise a pointer to null during declaration if you do not have the variable to assign it to yet.
```cpp
int * iPtr = 0;
int * p = NULL;
```
Either 0 or NULL will work. You can also use `nullptr` to do this with C++11. This helps because NULL is 0 and an integer which can lead to function calling ambiguity. [stackoverflow](https://stackoverflow.com/questions/1282295/what-is-the-nullptr-keyword-and-why-is-it-better-than-null)
```cpp
int *ptr = nullptr;                
```