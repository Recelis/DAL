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

Types declaraed with one or more \*_type parameters_ that serve as placeholder for actual type _concrete type_.

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

### private

Only accessible within the body of the class or struct when they are declared.

```csharp
class Employee2
{
    private readonly string _name = "FirstName, LastName";
    private readonly double _salary = 100.0;

    public string GetName()
    {
        // uses getters to return the private values
        return _name;
    }

    public double Salary
    {
        get { return _salary; }
    }
}
```

### abstract

The abstract modifier indicates that the thing has missing or incomplete information. Can be used on classes, methods, `indexers`, and `events`.

#### abstract classes

```C#
abstract class Shape
{
    public abstract int GetArea();
}

class Square : Shape
{
    private int _side;

    public Square(int n) => _side = n;

    // GetArea method is required to avoid a compile-time error.
    public override int GetArea() => _side * _side;

    static void Main()
    {
        var sq = new Square(12);
        Console.WriteLine($"Area of the square = {sq.GetArea()}");
    }
}
// Output: Area of the square = 144
```

In this example, the abstract class Shape has an abstract method GetArea. The Square class inherits from Shape and has to give an implementation of the GetArea method.

An abstract class cannot be instantiated.
It can contain abstract methods and accessors.
You cannot modifier an abstract class with a `sealed` modifier because the sealed modifier is the opposite of an abstract class.
A non-abstract class has to include implementations of the inherited abstract methods and accessors.

##### abstract methods

Abstract methods are a `virtual method`. They can only be permitted in abstract classes.

```C#
public abstract void MyMethod();
```

The implementation must be an `override`.

### virtual

The virtual modifier is used on a property or a method on a base class which allows that property or method to be overridden in the inheriting class.

If the inheriting class does not override it, then it'll call the original member.

You cannot use the virtual modifier with the `static`, `abstract`, `private`, or `override` modifiers.

The use of virtual is an exmaple of `Polymorphism`.

```C#
class MyBaseClass
{
    // virtual auto-implemented property. Overrides can only
    // provide specialized behavior if they implement get and set accessors.
    public virtual string Name { get; set; }

    // ordinary virtual property with backing field
    private int _num;
    public virtual int Number
    {
        get { return _num; }
        set { _num = value; }
    }
}

class MyDerivedClass : MyBaseClass
{
    private string _name;

    // Override auto-implemented property with ordinary property
    // to provide specialized accessor behavior.
    public override string Name
    {
        get
        {
            return _name;
        }
        set
        {
            if (!string.IsNullOrEmpty(value))
            {
                _name = value;
            }
            else
            {
                _name = "Unknown";
            }
        }
    }
}
```

### sealed

[docs](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/sealed)

A sealed class is one that cannot be inherited. You cannot use the sealed modifier on an abstract class because the modifiers are in direct opposition to each other (abstract classes MUST be inherited by another class).

They can also be used on a method or property that overrides a `virtual` method or property on a base class to prevent them being overridden.

```C#
class X
{
    protected virtual void F() { Console.WriteLine("X.F"); }
    protected virtual void F2() { Console.WriteLine("X.F2"); }
}

class Y : X
{
    sealed protected override void F() { Console.WriteLine("Y.F"); }
    protected override void F2() { Console.WriteLine("Y.F2"); }
}

class Z : Y
{
    // Attempting to override F causes compiler error CS0239.
    // protected override void F() { Console.WriteLine("Z.F"); }

    // Overriding F2 is allowed.
    protected override void F2() { Console.WriteLine("Z.F2"); }
}
```

### static
[docs](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/static)

Static modifier allows the member to below to the type instead od a specific object. A class can be made static which makes all of its members and properties static. It also means that the class is automatically `sealed`.

When the static modifier is placed on a member or a property, then that member or property cannot be referenced through the instance.

It can only be referenced by the type name.

```csharp
public class MyBaseC
{
    public struct MyStruct
    {
        public static int x = 100;
    }
}
Console.WriteLine(MyBaseC.MyStruct.x);
```

The `this` keyword cannot be used on a static method or property accessors.

# dyanmic

[docs](https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/interop/using-type-dynamic)
The dynamic type is one where the compiler assumes the element supports any operation so there will not be any errors at compilation but a run-time exception will be caused if the operation is not valid.

```csharp
static void Main(string[] args)
{
    ExampleClass ec = new ExampleClass();
    // The following call to exampleMethod1 causes a compiler error
    // if exampleMethod1 has only one parameter. Uncomment the line
    // to see the error.
    //ec.exampleMethod1(10, 4);

    dynamic dynamic_ec = new ExampleClass();
    // The following line is not identified as an error by the
    // compiler, but it causes a run-time exception.
    dynamic_ec.exampleMethod1(10, 4);

    // The following calls also do not cause compiler errors, whether
    // appropriate methods exist or not.
    dynamic_ec.someMethod("some argument", 7, null);
    dynamic_ec.nonexistentMethod();
}

class ExampleClass
{
    public ExampleClass() { }
    public ExampleClass(int v) { }

    public void exampleMethod1(int i) { }

    public void exampleMethod2(string str) { }
}
```

In the example above, exampleMethod only takes in 1 argument, so this will throw a run-time exception.

Any operation involving a dynamic will usually result in a dynamic unless you are conversing something from dynamic to another type or a constructor takes in arguments of type dynamic.

```C#
dynamic d = 1;
var testSum = d + 3;
// Rest the mouse pointer over testSum in the following statement.
System.Console.WriteLine(testSum);
```

testSum is a dynamic.

## Conversions

You can easily convert any value to dynamic.

```csharp
dynamic d1 = 7;
```

Or converting away from dynamic:

```csharp
int i = d1;
```
