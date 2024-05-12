+++
title = 'C#'
date = 2024-03-18T09:50:39+10:00
draft = true
+++

Understanding C# requires knowing basic syntax which is easy to understand. But also means that I have to get my hands on OOP concepts which is not that easy. More difficult that knowing what something is, is when to use them and when not to use them.

## Syntax

### Printing new line

```C#
    Console.WriteLine('Hello this will print a new line');
    Console.Write('This will print something');
```

### Variable

```C#
    string variableName = 'my variable'; // this is undefined;
```

Variables should be camelCase and case-sensitive.

#### Declaring and Defining a Variable

```C#
string firstName; // declaring a variable
firstName = "Bob"; // defining a variable
```

#### Improperly assigning a variable of the incorrect data type

```C#
int firstName;
firstName = "Bob";
// (2,13): error CS0029: Cannot implicitly convert type 'string' to 'int'
```

#### Implicitly typed local variables

Anything using the `var` keyword.

```C#
var message = "Hello world!";
```

Message will implicitly be inferred to be a string. It can be modified.

```C#
var message = "Hello world!";
message = "Hello world changed";
```

However, this cannot be changed to a different type.

```C#
var message = "Hello world!";
message = 1;
// (2,11): error CS0029: Cannot implicitly convert type 'int' to 'string'
```

`var variables` must be defined on initialisation.

```C#
var message;
// (1,5): error CS0818: Implicitly-typed variables must be initialized
```

### Namespaces

Each file will contain a a namespace which will contain **class**, **struct**, **interface**, **enumeration**, **delegates**, or other namespaces. A namespace here is sort of like a module. https://learn.microsoft.com/en-gb/dotnet/csharp/fundamentals/program-structure/

### Classes

### Structs

### Interfaces

### Enumeration

### Delegates

## .Net

```

```
