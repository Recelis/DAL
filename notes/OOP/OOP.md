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

## abstract classes
[docs](https://stackoverflow.com/a/1913647)
Abstract classes are those that do not have objects created from them. Instead they act as a base class for other classes to derive from. They can have everything a normal class has and also a method stub, which is a method without a body.



## SOLID Principles

### Single Responsibility