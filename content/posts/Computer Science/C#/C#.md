+++
title = 'C#'
date = 2024-03-18T09:50:39+10:00
draft = true
+++

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

### Strings

#### Character Escape

[docs](https://learn.microsoft.com/en-gb/training/modules/csharp-basic-formatting/2-exercise-character-escape-sequences)

Character escapes in C# begin with a \ inside a string.

```C#
Console.WriteLine("Hello\nWorld!");
Console.WriteLine("Hello\tWorld!");
```

You can escape a quotation mark too.

```C#
Console.WritelINE("hELLO \"World\"");
```

And you can even escape an escape.

```C#
Console.WriteLine("c:\\source\\repos");
```

#### Verbatim string literal

A verbatim string literal keeps the formatting of the string.

```C#
Console.WriteLine(@"    c:\source\repos
        (this is where your code goes)");
```

#### String Concatenation

```C#
    string firstName = "Bob";
    string message = "Hello " + firstName;
```

#### String Interpolation

```C#
string message = $"{greeting} {firstName}!"
```

### Numbers

C# deals with numbers similar to most other languages. It does allow `implicit type conversion` which allows numbers to be implicitly converted to a string during concatenation.

```C#
string firstName = "Bob";
int widgetsSold = 7;
Console.WriteLine(firstName + " sold " + widgetsSold + " widgets.");
```

#### Decimals

C# also requires a different type to deal with decimals.

```C#
decimal decimalQuotient = 7.0m / 5;
```

For a decimal operation to work, you'll need at least one of the numbers to be a decimal.

## Type System
[docs](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/)

## Value Types
Value types derive from `System.Valuetype` which derived from `System.Object`. Value types variables directly contain their values and not in a separate heap allocation or garbage collection needed. The two types are:
`struct` and `enum`. They are **sealed** in that you can't drive a type from any value type.


## Reference Types
A reference type can be a `class`, `record`, `delegate`, `array`, or `interface`. On declaration, value is null until it is assigned instance of type or you create one using `new` operator.

```C#
MyClass myClass = new MyClass();
MyClass myClass2 = myClass;
```

There are built-in reference types which are `dynamic`, `object` or `string`.

### Namespaces
[docs](https://learn.microsoft.com/en-gb/dotnet/csharp/fundamentals/program-structure/)

Each file will contain a a namespace which will contain **class**, **struct**, **interface**, **enumeration**, **delegates**, or other namespaces. A namespace here is sort of like a module.

### Classes

### Records
[docs](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/record)
A `record` modifier is a given to a `class` or a `struct` to indicate that this is primarily to hold data.

A `record` typically refers to `record class` which means that they are allocated on the heap. However, a `record struct` does exist and are allocated on the stack.

```C#
// A record class
public record Person(string FirstName, string LastName);

// A record struct
public readonly record struct Point(double X, double Y, double Z);
```

When the `primary constructor` are declared, the parameters are referred to as `positional parameters`. The compiler will create these `positional properties` to mirror the primary constructor.

i.e. in the Person example, the class will have a property FirstName and another property LastName without you needing to define this.

A `record class` are supposed to be `immutable` but they can have mutable fields and properties.

```C#
public record Person
{
    public required string FirstName { get; set; }
    public required string LastName { get; set; }
};
```





### Structs

### Interfaces

### Enumeration

### Delegates

## Types of literal values
A literal values also have types from the compiler. E.g. a numeric literal should be typed by appending letter to end of number. `4.56f`, if no letter is appended, compiler will infer type.

## Generic types
Types declaraed with one or more **type parameters* that serve as placeholder for actual type *concrete type*.

```C#
List<string> stringList = new List<string>();
```

Allows reuse of same class without converting each element to an object.

# Attributes
[docs](https://learn.microsoft.com/en-us/dotnet/standard/attributes/)
[docs](https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/reflection-and-attributes/)
Attributes are a way to associate metadata, or declarative info with code. Once associated, it can be queried at run time using **reflection**. They can be created by declaring instances of special classes derived from **System.Attribute** and are created when the compiler turns the code into **common intermediate language (CIL)** and placed in a **portable executable (PE)**.

## Reflection
Reflection looks into the metadata of a compiled code and gets the attributes defined in your code (type, method, properties...) You can dynamically created an instance of a type, bind type to an existing object, or get type from an existing object and invoke methods or access fields or properties.

Need to be add `using System;` and `using System.Reflection;` at top of `.cs` file.

Examples of getting type using `GetType()` method.
```C#
int i = 42;
Type type = i.GetType();
Console.WriteLine(type);
```

## Using attributes
You can give it apply a characteristic to a class (sort of like typing it)
```C#
[Serializable]
public class SampleClass
{
    // Objects of this type can be serialized.
}
```

# Keywords

## Access Modifiers
[docs](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/access-modifiers)

### public
No restriction on data.
```csharp
class PointTest
{
    public int x;
    public int y;
}

class Program
{
    static void Main()
    {
        var p = new PointTest();
        // Direct access to public members.
        p.x = 10;
        p.y = 15;
        Console.WriteLine($"x = {p.x}, y = {p.y}");
    }
}
// Output: x = 10, y = 15

```

### protected
Access is limited to containing class or types derived from containing class. They cannot even be accessed from the class that they are a member of.

```csharp
class A
{
    protected int x = 123;
}

class B : A
{
    static void Main()
    {
        var a = new A();
        var b = new B();

        // Error CS1540, because x can only be accessed by
        // classes derived from A.
        // a.x = 10;

        // OK, because this class derives from A.
        b.x = 10;
    }
}
```

### internal
An internal type or members are only accessible within files in the same `assembly`.

```csharp
public class BaseClass
{  
    // Only accessible within the same assembly.
    internal static int x = 0;
}  
```
