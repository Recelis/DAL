+++
title = 'System Runtime DLL'
date = 2024-03-18T09:50:39+10:00
draft = true
+++

# System Runtime DLL

## Lazy<T> Class
This is a class supports lazy initialization. This will create an object but then only later initialize it when it is needed.

```csharp
lazyLargeObject = new Lazy<LargeObject>(InitLargeObject);

...
static LargeObject InitLargeObject()
{
    LargeObject large = new LargeObject(Thread.CurrentThread.ManagedThreadId);
    // Perform additional initialization here.
    return large;
}
...
// using lazyLargeObject
LargeObject large = lazyLargeObject.Value;

// IMPORTANT: Lazy initialization is thread-safe, but it doesn't protect the
//            object after creation. You must lock the object before accessing it,
//            unless the type is thread safe. (LargeObject is not thread safe.)
lock(large)
{
    large.Data[0] = Thread.CurrentThread.ManagedThreadId;
    Console.WriteLine("Initialized by thread {0}; last used by thread {1}.",
        large.InitializedBy, large.Data[0]);
}
```
