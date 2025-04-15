# C++

## Pointers

[tutorial](https://www3.ntu.edu.sg/home/ehchua/programming/cpp/cp4_PointerReference.html)
Computer memory stores _contents_ at an _address_. Each address typically holds 8-bit (1 byte) of data. C introduces _variables_ which is a named location that can store a value of a particular type (`int`, `double`, `char`).

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

Name: pNumber (int\*)
address: 0xcanbeanything
value: 0x22ccec (&number)

Name: number (int)
address: 0x22ccec (&number)
value: 88

### Indirection or Dereferencing Operator (\*)

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
int * iPtr; // declared pointer
// there needs to be a initialization of the pointer to the address e.g. iPtr = &value; i.e. pointer = address of value
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

## References

A reference is an 'alias' to an existing variable. It allows you to read and update the original value which makes them very powerful when it is used as a function parameter where instead of `passing-by-value`, you pass by reference.

As such it is very similar to a pointer. Although references tend to have nicer syntax and are easier to use.

The syntax is:

```cpp
    type &newName = existingName;
```

An example is:

```cpp
int number = 99;
int & refNumber = number;
refNumber ++;
cout << refNumber << endl; // should be 100
cout << number << endl; // should also be 100
```

The value of the reference is the address of the variable.

e.g.
name: refNumber (int&)
address: 0xcanbeanything
value: 0x22ccec (&number) // address of number

Name: number (int)
address: 0x22ccec (&number)
value: 88

## References vs Pointers

Use references when possible, pointers when needed [stackoverflow](https://stackoverflow.com/a/18147466).

A reference is a named constant for an address. There are two main differences:

1. a reference needs to be initiased during declaration. e.g. `int & iRef; //errors out` It also cannot be changed once it has been established, while a pointer can.
2. To get the value pointed to by a pointer, you use a dereferencing operator \*.
   But it is not necessary for a reference.
   i.e.

```cpp
// for pointer
int variable = 5;
int * ptr = &variable;
cout << *ptr << endl; // 5

// for reference
int & reference = variable;
cout << reference << endl; // 5
```

### Pass-by-reference into Functions with Reference Arguments vs Pointer Arguments

For standard pass by value:

```cpp
int main() {
   int number = 8;
   cout <<  "In main(): " << &number << endl;  // 0x22ff1c
   cout << number << endl;         // 8
   cout << square(number) << endl; // 64
   cout << number << endl;         // 8 - no change
}

int square(int n) {  // non-const
   cout <<  "In square(): " << &n << endl;  // 0x22ff00
   n *= n;           // clone modified inside the function
   return n;
}
```

For pass by reference with Pointer arguments

```cpp
int main() {
   int number = 8;
   cout <<  "In main(): " << &number << endl;  // 0x22ff1c
   cout << number << endl;   // 8
   square(&number);          // Explicit referencing to pass an address
   cout << number << endl;   // 64
}

void square(int * pNumber) {  // Function takes an int pointer (non-const)
   cout <<  "In square(): " << pNumber << endl;  // 0x22ff1c
   *pNumber *= *pNumber;      // Explicit de-referencing to get the value pointed-to
}
```

Pass by reference with Reference arguments.

```cpp
int main() {
   int number = 8;
   cout <<  "In main(): " << &number << endl;  // 0x22ff1c
   cout << number << endl;  // 8
   square(number);          // Implicit referencing (without '&')
   cout << number << endl;  // 64
}

void square(int & rNumber) {  // Function takes an int reference (non-const)
   cout <<  "In square(): " << &rNumber << endl;  // 0x22ff1c
   rNumber *= rNumber;        // Implicit de-referencing (without '*')
}
```

## Use const whenever possible

A `const` function formal parameter cannot be modified inside the function. This is a good tip to remember to avoid accidentally modifying variables inside functions. A `const` function parameter can receive `const` and non-`const` arguments. While a non-const function reference or pointer can only receive a non-const argument.

```cpp
int squareConst(const int number) {
   // number *= number;  // error: assignment of read-only parameter
   return number * number;
}
```

## Passing Function's Return Value

You can pass the return value as a reference or pointer.

```cpp
int & squareRef(int & rNumber) {
   cout <<  "In squareRef(): " << &rNumber << endl;  // 0x22ff14
   rNumber *= rNumber;
   return rNumber;
}

int * squarePtr(int * pNumber) {
   cout <<  "In squarePtr(): " << pNumber << endl;  // 0x22ff10
   *pNumber *= *pNumber;
   return pNumber;
}
```

BUT! you should not return a reference to a local variable in a function because that local variable will be cleaned up when the function ends.

```cpp
int * squarePtr(int number) {
   int localResult = number * number;
   return &localResult;
      // warning: address of local variable 'localResult' returned
}
```

However, you can do this with `dynamic memory allocation`.

```cpp

int main() {
   int number = 8;
   cout << number << endl;  // 8
   int *ptrResult = &squarePtr(number);
   cout << ptrResult << endl;  // 64
   delete ptrResult; // needs to be cleaned up

   int & refResult = squareRef(number);
   cout << refResult << endl;   // 64
   // no way to delete this memory because delete should only be used on a pointer.
   // delete &refResult; might work but you don't truly know if it is in the heap.
}

int * squarePtr(int number) {
   int * dynamicAllocatedResult = new int(number * number);
   return dynamicAllocatedResult;
}

// this is REALLY DANGEROUS/BAD CODE!
int & squareRef(int number) {
   int * dynamicAllocatedResult = new int(number * number);
   return *dynamicAllocatedResult;
}
```

## Memory Management

[docs](https://stackoverflow.com/a/5836571)

### stack

The stack is where local variables are stored and are used to hold parameters that are passed to functions. In other words, parameters that exist solely in a function live in the stack.

```cpp
int myFunction(int param) {
    // param lives in stack
    return param; // this is copied by value before being returned.
}
```

### heap

The heap on the other hand is memory that is defined with the `new` keyword or `malloc`. It is program defined memory that usually needs to be manually freed.

## Macros

A macro is basically a form of text substitution that occurs before compilation begins. The most common example is `define`. It is possible to do function macros too but it isn't recommended.

```cpp
#define SQUARE(x) ((x) * (x))
// instead use constexpr for type safety
constexpr int square(int x) {
    return x * x;
}
```

### define

```cpp
#define PI 3.14159

int main() {
    double r = 2.0;
    double area = PI * r * r; // PI is replaced with 3.14159
}
```

### include guards

Macros that prevent header files from being included more than once.

#### ifndef, #define and endif

These are `include guards` which are macros to prevent header files from being included more than once.

```cpp
#ifndef MATH_H // if not defined
#define MATH_H // define it

#endif // end of MATH_H
```

### pragma once

Use this modern macro instead as your `include guards`.

```cpp
#pragma once
```

## Template

[docs](https://learn.microsoft.com/en-us/cpp/cpp/templates-cpp?view=msvc-170)

These are things that allow you to write classes and functions that can take any type. Sort of like a generic in TypeScript.

```cpp
template <typename T>
T minimum(const T& lhs, const T& rhs)
{
    return lhs < rhs ? lhs : rhs;
}
```

It uses two keywords, `template` and `typename`. T is the `template parameter` while the `typename` acts as a placeholder for the type to be used.

To use this:

```cpp
int a = get_a();
int b = get_b();
int i = minimum(a, b);
```

The compiler will deduce this as an `int` and create a corresponding int function.

## inheritance

[docs](https://learn.microsoft.com/en-us/cpp/cpp/inheritance-cpp?view=msvc-170)

In C++, classes can be derived from another. The class being derived from is the `base class`, The syntax is:

```cpp
class DerivedSingleBase : [virtual] [access-specifier] Base
{
    // member list
};

class DerivedMultipleBases : [virtual] [access-specifier] Base1,
    [virtual] [access-specifier] Base2, ...
{
    // member list
};
```

There are three important parts here: `virtual inheritance`, the `access specifier` which forms the `member access control`, and `single vs multiple inheritance`. The virtual inheritance for a class is somewhat different from the virtual used in a function.

### virtual inheritance

#### virtual function

[docs](https://learn.microsoft.com/en-us/cpp/cpp/virtual-functions?view=msvc-170)

This basically means that the function in the base class can be overwritten.

```cpp
class Base {
public:
    virtual void sayHi() {
        std::cout << "Base says hi\n";
    }
};

class Derived : public Base {
public:
    void sayHi() override {
        std::cout << "Derived says hi\n";
    }
};
```

##### calling by pointer or reference

#### override keyword

The `override` keyword is actually optional but is recommended to use in modern C++ (C++ 11 and newer) to make it explicit that this function is virtual.

### pure virtual

### virtual destructors

recommended

```

```

### member access control

### single vs multiple inheritance

## guarding init() functions

## F()

## std library

### Smart Pointers

std::unique_ptr

## size_t

## enum

### KEY_COUNT technique

## Class Member

Class::member
