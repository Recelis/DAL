# OOP

## 3 pillars of Object Oriented Programming
These are `encapsulation`, `inheritance` and `polymorphism`.

### Polymorphism
[docs](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/polymorphism)

There are two points to polymorphism.
- At run time, derived class objects can be treated as objects of the base class for method parameters and collections or arrays. This may mean the object's declared type is not identical to the run-time type.
e.g.
if you have a base class Animal, and a derived class Dog. Any method that takes in an Animal can also take in a Dog (because Dog has all the public methods of an Animal)

```python
def make_animal_speak(animal: Animal):
    animal.speak()

dog = Dog()
make_animal_speak(dog)
```

- the second point is that base classes can define and implement `virtual` methods which can be `override` by the derived classes with their own definition and implementation. During the run-time, there will be a look up on the run-time type of the object, so even though they may all be given the base class type, the run-time will use the derived class.

```csharp
public class Shape
{
    // A few example members
    public int X { get; private set; }
    public int Y { get; private set; }
    public int Height { get; set; }
    public int Width { get; set; }

    // Virtual method
    public virtual void Draw()
    {
        Console.WriteLine("Performing base class drawing tasks");
    }
}

public class Circle : Shape
{
    public override void Draw()
    {
        // Code to draw a circle...
        Console.WriteLine("Drawing a circle");
        base.Draw();
    }
}
public class Rectangle : Shape
{
    public override void Draw()
    {
        // Code to draw a rectangle...
        Console.WriteLine("Drawing a rectangle");
        base.Draw();
    }
}
public class Triangle : Shape
{
    public override void Draw()
    {
        // Code to draw a triangle...
        Console.WriteLine("Drawing a triangle");
        base.Draw();
    }
}

var shapes = new List<Shape>
{
    new Rectangle(),
    new Triangle(),
    new Circle()
};

// Polymorphism at work #2: the virtual method Draw is
// invoked on each of the derived classes, not the base class.
foreach (var shape in shapes)
{
    shape.Draw();
}
```


## abstract classes
[docs](https://stackoverflow.com/a/1913647)
Abstract classes are those that do not have objects created from them. Instead they act as a base class for other classes to derive from. They can have everything a normal class has and also a method stub, which is a method without a body.



## SOLID Principles

### Single Responsibility