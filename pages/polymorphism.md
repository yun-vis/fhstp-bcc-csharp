---
# permalink: /about/
layout: single
title: "Polymorphism"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2022-03-15
---

# Understanding Polymorphism

**Polymorphism:** allow a derived class to override an inherited action to provide custom behavior. E.g., animal. Animals like dogs speak "Woof!", but cats speak "Meow!".


# Generics [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/types/generics)

Generics introduces the concept of type parameters to .NET, which make it possible to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code.

## Generic Method

Convert.ChangeType [Doc](https://docs.microsoft.com/en-us/dotnet/api/system.convert.changetype?view=net-6.0)

```csharp
using System;

// namespace
namespace MyBusiness
{
    // main program
    class Program
    {
        // internal is the default if no access modifier is specified.
        static void Main(string[] args)
        {
            /*
            Function overloading
            */
            // int
            int or = 2;
            int newOR = DoubleMyResourc(or);

            Console.WriteLine($"My old resource is {or} and my new resource is {newOR}");

            // float
            float of = 2.5F;
            float newOF = DoubleMyResourc(of);

            Console.WriteLine($"My old resource is {of} and my new resource is {newOF}");

            /*
            Generics
            */
            // int
            int gr = 2;
            int newGR = DoubleMyResourc<int>(gr);

            Console.WriteLine($"My old resource is {gr} and my new resource is {newGR}");

            // float
            float gf = 2.5F;
            float newGF = DoubleMyResourc<float>(gf);

            Console.WriteLine($"My old resource is {gf} and my new resource is {newGF}");
        }

        public static int DoubleMyResourc(int resource)
        {
            return 2*resource;
        }

        public static float DoubleMyResourc(float resource)
        {
            return 2.0F*resource;
        }

        // Compile error! The type conversion is unknown to the compiler.
        // static T DoubleMyResource<T>(T resource)
        // {
        //     return  2 * resource;
        // }

        public static T DoubleMyResourc<T>(T resource)
        {
            // Type conversion is also called "casting"!
            decimal r = (decimal) Convert.ChangeType(resource, typeof(decimal));
            r *= 2.0M; // r = r * 2.0

            return (T)Convert.ChangeType(r, typeof(T));
        }
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
/*
The animal namespace
*/
namespace Animals
{
    // Working with non-generic types
    public class Cat
    {
        public object? food = default(object);
        public string CheckFood(object input)
        {
            if (food == input)
            {
                return "Expected food and input are the same.";
            }
            else
            {
                return "Expected food and input are NOT the same.";
            }
        }
    }

    // Working with generic types
    // One can consider T as a kind of template
    public class GenericCat<T> where T : IComparable
    {
        // A default() returns the default value of type parameter. Here is used for initialization.
        public T? food = default(T?);

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

    // Working with generic methods
    // One can consider T as a kind of template
    public class GenericMethodCat
    {
        public static double DoubleMyFood<T>(T input)
        where T : IConvertible
        {
            // convert using the current culture
            double d = input.ToDouble(
                Thread.CurrentThread.CurrentCulture);   
                // A system method to convert a string to a double, we simply use it for now.
            return 2 * d;
        }
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

## Override and New Keywords [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/knowing-when-to-use-override-and-new-keywords)

In **method overriding** (using the keyword **override**), when base class reference variable pointing to the object of the derived class, then it will call the overridden method in the derived class.

In the **method hiding** (using the keyword **new**), when base class reference variable pointing to the object of the derived class, then it will call the hidden method in the base class.

E.g., when a method is hidden with new, the compiler is not smart enough to know that the object is an WildCat, so it calls the WriteToConsole method in Cat.


## Single Inheritance

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
            // Declare a dog
            Dog shiba = new Dog();
            shiba.Speak();

            // Declare a cat and a wild cat
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
            WildCat leopard = new WildCat
            {
                Name = "Alice",
                CountryCode = "Taiwan"
            };

            nana.Speak();
            leopard.Speak();
        }
    }
}
```

In PetLibrary/Animals.cs,
```csharp
/*
The animal namespace
*/
namespace Animals
{
    public class Animal
    {
        public virtual void Speak()
        {
            Console.WriteLine("");
        }
    }

    public class Dog : Animal
    {
        public override void Speak()
        {
            Console.WriteLine("Woof!");
        }
    }

    public class Cat : Animal
    {
        /*
        Class field, including different variables
        they can be organized by similar characteristics
        */

        // The name of the cat
        public string Name;
        // The birthday of the cat
        public DateTime DateOfBirth;

        /*
        Class methods, where functions should be implemented
        */

        // Constructors
        // Default constructor. It will be called by default
        public Cat()
        {
            Name = "Unknown";
            DateOfBirth = DateTime.Today;
        }
        // Parameterized Constructor
        public Cat(string name, DateTime dateOfBirth)
        {
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
        }
        // Finalizer
        ~Cat()
        {
        }

        public override void Speak()
        {
            Console.WriteLine("Meow!");
        }

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }
    }

    public class WildCat : Cat
    {
        public string? CountryCode { get; set; }
        public DateTime FoundDate { get; set; }

        // Default Constructor
        public WildCat()
        {
        }

        public override void Speak()
        {
            Console.WriteLine("WildMeow!");
        }

        // hidden methods
        // based on the keyword new
        public new void WriteToConsole()
        {
            base.WriteToConsole();
            Console.WriteLine(format:
              "{0} was born on {1:dd/MM/yy} and found on {2:dd/MM/yy}",
              arg0: Name,
              arg1: DateOfBirth,
              arg2: FoundDate);
        }
    }
}
```

## Nested Inheritance [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/inheritance)

Note that a class can derive from only a single direct base class. But what if I need information from multiple classes? Use interface.

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
            // Declare a dog
            Dog shiba = new Dog();
            shiba.Speak();

            // Declare a cat and a wild cat
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
            WildCat leopard = new WildCat
            {
                Name = "Alice",
                CountryCode = "Taiwan"
            };

            nana.Speak();
            leopard.Speak();
        }
    }
}
```

In PetLibrary/Animals.cs
```csharp
/*
The animal namespace
*/
namespace Animals
{
    public class Animal
    {
        public virtual void Speak()
        {
            Console.WriteLine("");
        }
    }

    public class Dog : Animal, IRun
    {
        public override void Speak()
        {
            Console.WriteLine("Woof!");
        }

        // Implementation of interface IRun
        public double Speed { get; set; }
        public int Distance { get; }
        public double SpeedUp(double velocity)
        {
            Speed += 2.0 * velocity;
            return Speed;
        }
    }

    interface IRun
    {
        // Instance field
        // double Velocity; // compile error
        // Property
        double Speed { get; set; }
        int Distance { get; }

        double SpeedUp(double velocity);
    }

    public class Cat : Animal
    {
        /*
        Class field, including different variables
        they can be organized by similar characteristics
        */

        // The name of the cat
        public string Name;
        // The birthday of the cat
        public DateTime DateOfBirth;

        /*
        Class methods, where functions should be implemented
        */

        // Constructors
        // Default constructor. It will be called by default
        public Cat()
        {
            Name = "Unknown";
            DateOfBirth = DateTime.Today;
        }
        // Parameterized Constructor
        public Cat(string name, DateTime dateOfBirth)
        {
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
        }
        // Finalizer
        ~Cat()
        {
        }

        public override void Speak()
        {
            Console.WriteLine("Meow!");
        }

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }
    }

    public class WildCat : Cat
    {
        public string? CountryCode { get; set; }
        public DateTime FoundDate { get; set; }

        // Default Constructor
        public WildCat()
        {
        }

        public override void Speak()
        {
            Console.WriteLine("WildMeow!");
        }

        // hidden methods
        // based on the keyword new
        public new void WriteToConsole()
        {
            base.WriteToConsole();
            Console.WriteLine(format:
              "{0} was born on {1:dd/MM/yy} and found on {2:dd/MM/yy}",
              arg0: Name,
              arg1: DateOfBirth,
              arg2: FoundDate);
        }
    }
}
```
```bash
$ Woof!
$ Meow!
$ WildMeow!
```


## Abstract Class [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/abstract)

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
            // Create a cat array
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
            WildCat leopard = new WildCat
            {
                Name = "Alice",
                CountryCode = "Taiwan"
            };

            nana.Speak();
            leopard.Speak();
        }
    }
}
```

In PetLibrary/Animals.cs,
```csharp
/*
The animal namespace
*/
namespace Animals
{
    public abstract class Animal
    {
        public abstract void Speak();
    }

    public class Cat : Animal
    {
        /*
        Class field, including different variables
        they can be organized by similar characteristics
        */

        // The name of the cat
        public string Name;
        // The birthday of the cat
        public DateTime DateOfBirth;

        /*
        Class methods, where functions should be implemented
        */

        // Constructors
        // Default constructor. It will be called by default
        public Cat()
        {
            Name = "Unknown";
            DateOfBirth = DateTime.Today;
        }
        // Parameterized Constructor
        public Cat(string name, DateTime dateOfBirth)
        {
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
        }
        // Finalizer
        ~Cat()
        {
        }

        public override void Speak()
        {
            Console.WriteLine("Meow!");
        }

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }
    }

    public class WildCat : Cat
    {
        public string? CountryCode { get; set; }
        public DateTime FoundDate { get; set; }

        // Default Constructor
        public WildCat()
        {
        }

        public override void Speak()
        {
            Console.WriteLine("WildMeow!");
        }

        // hidden methods
        // based on the keyword new
        public new void WriteToConsole()
        {
            base.WriteToConsole();
            Console.WriteLine(format:
              "{0} was born on {1:dd/MM/yy} and found on {2:dd/MM/yy}",
              arg0: Name,
              arg1: DateOfBirth,
              arg2: FoundDate);
        }
    }
}
```



---
# Selected Theory

## Useful Advanced C# Data Structure

* Collections [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/collections). We will introduce one-by-one later.
