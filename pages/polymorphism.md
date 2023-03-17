---
# permalink: /about/
layout: single
title: "Polymorphism"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2022-03-17
---

# Understanding Polymorphism

**Polymorphism:** allow a derived class to override an inherited action to provide custom behavior. E.g., animal. Animals like dogs speak "Woof!", but cats speak "Meow!".


# Generics [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/types/generics)

Generics introduces the concept of type parameters to .NET, which make it possible to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code.

## Generic Method

Convert.ChangeType [Doc](https://docs.microsoft.com/en-us/dotnet/api/system.convert.changetype?view=net-6.0)

```csharp
namespace CRC_CSD_06;

class Program
{
    static void Main(string[] args)
    {
        int r = 2;
        // Function overloading
        int newR = DoubleMyResource(r);
        Console.WriteLine($"My old resource is {r} and the new one is {newR}");
        float f = 2.5F;
        float newF = DoubleMyResource(f);
        Console.WriteLine($"My old resource is {f} and the new one is {newF}");

        // Generic functions
        int gr = 2;
        int newGR = DoubleMyResource<int>(gr);
        Console.WriteLine($"My old resource is {gr} and the new one is {newGR}");

        float gf = 2.5F;
        float newGF = DoubleMyResource<float>(gf);
        Console.WriteLine($"My old resource is {gf} and the new one is {newGF}");

    }

    public static int DoubleMyResource(int resource)
    {
        return 2*resource;
    }

    // Function overloading
    public static float DoubleMyResource(float resource)
    {
        return 2.0F*resource;
    }

    // Generic function
    public static T DoubleMyResource<T>(T resource) where T : IConvertible
    {
        double d = resource.ToDouble(Thread.CurrentThread.CurrentCulture);
        // ToDouble: A system method to convert a type to a double. We just use it for now.
        // Thread.CurrentThread.CurrentCulture: ToDouble requires a parameter that implements IFormatProvider to understand the format of numbers for a language and region. We can pass the CurrentCulture property of the current thread to specify the language and region used by your computer. 

        d *= 2.0; // r = r * 2.0M

        return (T) Convert.ChangeType(d, typeof(T));
    }
}
```

## Generic Class

In MyBusiness/Program.cs,
```csharp
using System;
using Animals;

// namespace
namespace MyBusiness
{
    // main program
    internal class Program
    {
        static void Main(string[] args)
        {
            // Cat is currently flexible, because any type can be set for the food field and input parameter.
            // But there is no type checking, so inside the CheckFood method,
            // we cannot safely do much and the results are sometimes not what you might expect
            var t1 = new Cat();
            t1.food = 5;
            Console.WriteLine($"Cat food with an integer: {t1.CheckFood(5)}");
            var t2 = new Cat();
            t2.food = "fish";
            Console.WriteLine($"Cat food with a string: {t2.CheckFood("fish")}");

            // The type is defined at allocation
            var gt1 = new GenericCat<int>();
            gt1.food = 5;
            Console.WriteLine($"Cat food with an integer: {gt1.CheckFood(5)}");
            var gt2 = new GenericCat<string>();
            gt2.food = "fish";
            Console.WriteLine($"Cat food with a string: {gt2.CheckFood("fish")}");

            // generic methods
            string food1 = "4";
            Console.WriteLine("Double cat food {0} to {1}",
              arg0: food1,
              arg1: GenericMethodCat.DoubleMyFood<string>(food1));
            byte food2 = 3;
            Console.WriteLine("Double cat food {0} to {1}",
              arg0: food2,
              arg1: GenericMethodCat.DoubleMyFood(food2));
        }
    }
}
```

In PetLibrary/Animals.cs,

```csharp

public class Cat
{
    public object? food = default(object);
    // object: Supports all classes in the .NET class hierarchy and provides low-level services to derived classes. This is the ultimate base class of all .NET classes; it is the root of the type hierarchy.
    // default: A default value expression produces the default value of a type

    public string CheckFood(object input)
    {
        if (food == null)
        {
            return "Expected food is empty.";
        }
        else if (food == input)
        // else if (food.Equals(input)) // Here, you explicitly compare the value not the address.
        // Cat is currently flexible, because any type can be set for the food field and input parameter.
        // But there is no type checking, so inside the Process method, we cannot safely do much and the results are sometimes not what you might expect; for example, when passing int values into an object parameter!
        // This is because the value 5 stored in the food is stored in a different memory address to the value 5 passed as a parameter and when comparing reference types like any value stored in object, they are only equal if they are stored at the same memory address, that is, the same object, even if their values are equal. We can solve this problem by using generics.
        {
            return "Expected food and input are the same.";
        }
        else
        {
            return "Expected food and input are NOT the same.";
        }
    }
}

public class GenericCat<T> where T : IComparable
{
    public T? food = default(T?);
    // A default value expression produces the default value of a type

    public string CheckFood(T input)
    {
        if (food == null)
        {
            return "Expected food is empty.";
        }
        else if (food.CompareTo(input) == 0)
        {
            return "Expected food and input are the same.";
        }
        else
        {
            return "Expected food and input are NOT the same.";
        }
    }
}

public class GenericMethodCat
{
    public static double DoubleMyFood<T>(T input) where T : IConvertible
    {
        double d = input.ToDouble(Thread.CurrentThread.CurrentCulture);
        // ToDouble: A system method to convert a type to a double. We just use it for now.
        // Thread.CurrentThread.CurrentCulture: ToDouble requires a parameter that implements IFormatProvider to understand the format of numbers for a language and region. We can pass the CurrentCulture property of the current thread to specify the language and region used by your computer. 

        return 2.0 * d;
    }
}
```
```bash
$ Cat food with an integer: Expected food and input are NOT the same.
$ Cat food with a string: Expected food and input are the same.
$ Cat food with an integer: Expected food and input are the same.
$ Cat food with a string: Expected food and input are the same.
$ Double cat food 4 to 8
$ Double cat food 3 to 6
```

## Thread.CurrentUICulture Property [Doc](https://docs.microsoft.com/en-us/dotnet/api/system.threading.thread.currentuiculture?view=net-6.0)

<!-- ## in (Generic Modifier) [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/in-generic-modifier) -->


You have now seen two ways to change the behavior of an inherited method. We can hide it using the **new** keyword (known as non-polymorphic inheritance), or we can **override** it (known as polymorphic inheritance).


---
# Selected Theory

## Useful Advanced C# Data Structure

* Collections [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/collections). We will introduce one-by-one later.
