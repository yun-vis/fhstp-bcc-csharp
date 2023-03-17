---
# permalink: /about/
layout: single
title: "Inheritance"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2023-03-17
---

# Other Properties of Methods
## Types of the Methods
* **Instance methods** are actions that an object does to itself.
  * A mom cat procreates a kitty by taking a dad cat as an input.
  * The mom cat cannot produce kitties from multiple dad cats.
* **Static methods** are actions the type does.
  * The owner of the cats encourages them to procreate a kitty.
  * The owner can pair many cat couples at the same time.

```csharp
using System;
using System.Collections.Generic;
using Animals;

// namespace
namespace MyBusiness
{
    // main program
    internal class Program
    {
        static void Main(string[] args)
        {
            // Create 3 cats and initialize them
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
            Cat coffee = new Cat("Coffee", new DateTime(2019, 6, 20));
            Cat kiwi = new Cat("Kiwi", new DateTime(2018, 11, 19));

            // Print out the present content
            nana.WriteToConsole();
            coffee.WriteToConsole();
            kiwi.WriteToConsole();

            // Call instance method
            Cat kitty1 = nana.ProduceKittyWith(coffee);
            kitty1.Name = "Naffee";
            // Call static method
            Cat kitty2 = Cat.ProduceKitty(kiwi, coffee);
            // The following statement functions as the above one
            // Cat kitty2 = kiwi * coffee;
            kitty1.Name = "Kiffee";

            // Print out the kitty name
            Console.WriteLine($"{nana.Name} has {nana.Children.Count} kitty.");
            Console.WriteLine($"{coffee.Name} has {coffee.Children.Count} kitty.");
            Console.WriteLine($"{kiwi.Name} has {kiwi.Children.Count} kitty.");
            Console.WriteLine(
            format: "{0}'s first kitty is named \"{1}\".",
            arg0: coffee.Name,
            arg1: coffee.Children[0].Name);
        }
    }
}

/*
The animal namespace
*/
namespace Animals
{
    public class Cat
    {
        /*
        Class field, including different variables
        they can be organized by similar characteristics
        */

        // The name of the cat
        public string Name;
        // The birthday of the cat
        public DateTime DateOfBirth;
        // The children of the cat
        public List<Cat> Children = new List<Cat>();

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

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }

        // Static method to "multiply"
        public static Cat ProduceKitty(Cat cat1, Cat cat2)
        {
            Cat kitty = new Cat
            {
                Name = $"Baby of {cat1.Name} and {cat2.Name}"
            };
            cat1.Children.Add(kitty);
            cat2.Children.Add(kitty);
            return kitty;
        }

        // Use operators instead of the above method
        public static Cat operator * (Cat cat1, Cat cat2)
        {
            return Cat.ProduceKitty(cat1, cat2);
        }

        // Instance method to "multiply"
        public Cat ProduceKittyWith(Cat partner)
        {
            return ProduceKitty(this, partner);
        }
    }
}
```
```bash
$ Nana was born on a Monday.
$ Coffee was born on a Thursday.
$ Kiwi was born on a Monday.
$ Nana has 1 kitty.
$ Coffee has 2 kitty.
$ Kiwi has 1 kitty.
$ Coffee\'s first kitty is named "Kiffee".
```

## Local (Nested/Inner) Functions

**Local functions** are private methods of a type that are nested in another member.

```csharp
public Cat ProduceKittyWith(Cat partner)
{
    // This is a local function
    bool testIfInLove(Cat partner)
    {
        return true;
    }

    // Test if the cat is in love with the partner.
    testIfInLove( partner);

    return ProduceKitty(this, partner);
}
```

---
# Splitting Files to Organize Them [Doc](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new)
```bash
// Create a folder named MyBusiness
CRC_CSD-05> mkdir MyBusiness
// Enter the folder MyBusiness
CRC_CSD-05> cd MyBusiness
// Set up a console application
CRC_CSD-05/MyBusiness> $ dotnet new console
// Leave the folder MyBusiness
CRC_CSD-05/MyBusiness> $ cd ../
// Set up a class library called PetLibrary
CRC_CSD-05> $ dotnet new classlib --name PetLibrary
// Change the folder to PetLibrary
CRC_CSD-05> $ cd PetLibrary
// Change the file name and add content to Class Cat
CRC_CSD-05/PetLibrary> $ mv Class1.cs Cat.cs
// Go back to the folder MyBusiness and run the program
CRC_CSD-05/PetLibrary> $ cd ../MyBusiness
CRC_CSD-05/MyBusiness> $ dotnet run
```

In MyBusiness.csproj, you see the configuration of the program
```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <RootNamespace>MyBusiness</RootNamespace>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

<!-- Set environment variable -->
  <ItemGroup>
    <ProjectReference
      Include="../PetLibrary/PetLibrary.csproj" />
      <!-- The slash and backslash presentation used to describe a path were not
      standardized. It causes a lot of pain.
      Some morden editors automatically convert them. -->
  </ItemGroup>

</Project>
```

In MyBusiness/Program.cs,
```csharp
using System;
using PetLibrary;

// namespace
namespace MyBusiness
{
    // main program
    public class Program
    {
        static void Main(string[] args)
        {
            // Create a cat
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
            nana.WriteToConsole();

            OwnPets myPets = new OwnPets();
            myPets.WriteToConsole();
        }
    }
}
```

In PetLibrary/Cat.cs,
```csharp
// The same as namespace PetLibrary{}. We use ";" just to save indent spac
namespace PetLibrary;

public class Cat
{
    /*
    Class field, including different variables
    they can be organized by similar characteristics
    */

    // The name of the cat
    public string Name;
    // The birthday of the cat
    public DateTime DateOfBirth;
    // The children of the cat
    public List<Cat> Children = new List<Cat>();

    /*
    Class methods, where functions should be implemented
    */

    // Constructors
    // Default constructor. It will be called by default
    public Cat()
    {
        Name = "UnknownCat";
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

    // Print out method
    public void WriteToConsole()
    {
        Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
    }

    // Static method to "multiply"
    public static Cat ProduceKitty(Cat cat1, Cat cat2)
    {
        Cat kitty = new Cat
        {
            Name = $"Baby of {cat1.Name} and {cat2.Name}"
        };
        cat1.Children.Add(kitty);
        cat2.Children.Add(kitty);
        return kitty;
    }

    // Use operators instead of the above method
    public static Cat operator *(Cat cat1, Cat cat2)
    {
        return Cat.ProduceKitty(cat1, cat2);
    }

    // Instance method to "multiply"
    public Cat ProduceKittyWith(Cat partner)
    {
        return ProduceKitty(this, partner);
    }
}
```

In PetLibrary/Dog.cs,
```csharp
namespace PetLibrary;

// Access modifier internal can be only used inside the project
internal class Dog
{
    // Fields
    // The name of the cat
    public string Name;

    // Constructors
    // Default constructor. It will be called by default
    public Dog()
    {
        Name = "UnknownDog";
    }
}
```

In PetLibrary/OwnPets.cs,
```csharp
namespace PetLibrary;

public class OwnPets
{
    // Fields
    Cat houseCat;
    Dog houseDog;

    // Constructors
    public OwnPets()
    {
        houseCat = new Cat();
        houseDog = new Dog();
    }

    // Finalizers

    // Methods
    // Print out method
    public void WriteToConsole()
    {
        Console.WriteLine($"{houseCat}'s name is  {houseCat.Name}.");
        Console.WriteLine($"{houseDog}'s name is  {houseDog.Name}.");
    }
}
```

---
# Key OOP Concept

It is like a phylogenetic tree. The higher hierarchy you climb, the more abstract concept about the object you get.

* **Objects:** instances of classes. E.g., my cat.

* **Class:** the blueprint for the data type (variables) and available methods for a given type or class of object. E.g., cat. Something that is cute, fluffy, with two eyes, four legs and a tail.

---
* **Encapsulation:** the combination of the data and actions that are related to an object. Achieve by using access modifiers, which is a way to ensure security. E.g., a cat can access its toilet.

* **Composition:** is about what an object is made of. E.g., A cat has two eyes, four legs and a tail.

* **Aggregation:** is about what can be combined with an object. E.g., a cat does not have a horn. You cannot ask a cat to fly like a bird, but it can walk like a quadruped.

* **Inheritance:** reuse code by having a subclass derive from a base or superclass. All functionality in the base class is inherited by and becomes available in the derived class. E.g., quadruped. A cat inherits the function of a quadruped.

* **Polymorphism:** allow a derived class to override an inherited action to provide custom behavior. E.g., animal. Animals like dogs speak "Woof!", but cat speaks "Meow!".

* **Abstraction:** is about capturing the core idea of an object and ignoring the details or specifics. C# has an abstract keyword which formalizes the concept. If a class is not explicitly abstract, then it can be described as being concrete. E.g., define a abstract class called **Animal** (a live creature that moves), but implement its derived class **Cat** by giving actual behaviors of a cat.


---
# Inheriting Classes
In C#, classes are used to create custom types. Inheritance is the process by which one class inherits the members of another class.

# Self-Defined Interface [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface)
An **interface** defines a contract. Any class or struct that implements that contract must provide an implementation of the members defined in the interface. Note that you cannot apply access modifiers to interface members.

The basic difference is that a **class** has both a definition and an implementation whereas an **interface** only has a definition.

**Interfaces** don't contain fields because fields represent a specific implementation of data representation, and exposing them would break encapsulation.

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
            // Create a cat
            Cat nana = new Cat("Nana", new DateTime(2019, 12, 9));
            double speed = nana.SpeedUp(2);
            Console.WriteLine($"{nana.Name} is running at speed {speed} km/hr."); ;
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
    public class Cat : IRun
    // ":" operator allows Cat class to reuse what has been defined in IRun.
    // Naming convention: add I for an interface name.
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
            Speed = 0.0;
            Name = "Unknown";
            DateOfBirth = DateTime.Today;
        }
        // Parameterized Constructor
        public Cat(string name, DateTime dateOfBirth)
        {
            Speed = 0.0;
            this.Name = name;
            this.DateOfBirth = dateOfBirth;
        }
        // Finalizer
        ~Cat()
        {
        }

        // Implementation of interface IRun
        public double Speed { get; set; }
        public int Distance { get; }
        public double SpeedUp(double velocity)
        {
            Speed += velocity;
            return Speed;
        }
    }

    // IRun is an interface
    // You have variables and methods, but you don't implement them.
    // No access modifier is allowed.
    interface IRun
    {
        // Instance field
        // double Velocity; // compile error
        // Property
        double Speed { get; set; }
        int Distance { get; }

        double SpeedUp(double velocity);
    }
}
```
```bash
$ Nana is running at speed 2 km/hr.
```

# System-Defined Interface [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interface)

In MyBusiness/Program.cs,
```csharp
using System;
using Animals;

// namespace
namespace MyBusiness
{
    // main program
    class Program
    {
        static void Main(string[] args)
        {
            // Create a cat array
            Cat[] cats =
            {
                new Cat("Nana", new DateTime(2019, 12, 9)),
                new Cat("Coffee", new DateTime(2019, 6, 20)),
                new Cat("Kiwi", new DateTime(2018, 11, 19))
            };

            // Print the array
            for (int i = 0; i < cats.Length; i++)
            {
                Console.WriteLine($"{cats[i].Name}");
            }

            // Sort the array
            Array.Sort(cats);

            // Print the array again
            Console.WriteLine("Use Cat's IComparable implementation to sort the cat instance:");
            for (int i = 0; i < cats.Length; i++)
            {
                Console.WriteLine($"{cats[i].Name}");
            }
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
    public class Cat : IComparable<Cat>
    // You can check the definition of IComparible
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

        // Print out method
        public void WriteToConsole()
        {
            Console.WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
        }

        // Implementation of interface IComparible
        // Also check the class "public class Cat : IComparable<Cat>"
        public int CompareTo(Cat? anotherCat)
        {
            if (anotherCat != null)
                return Name.CompareTo(anotherCat.Name);
            else
                return 0;
        }
    }
}
```
```bash
$ Nana
$ Coffee
$ Kiwi
$ Use Cat\'s IComparable implementation to sort the cat instance:
$ Coffee
$ Kiwi
$ Nana
```

# Class Inheritance [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/inheritance)

**Inheritance**, together with encapsulation and polymorphism, is one of the three primary characteristics of object-oriented programming.

In PetLibrary/Animals.cs, add another class
```csharp
public class WildCat : Cat
{
    public string? CountryCode { get; set; }
    public DateTime FoundDate { get; set; }

    // overridden methods
    public override string ToString()
    {
        return $"{Name}'s code is {CountryCode}";
    }

    // hidden methods
    public new void WriteToConsole()
    {
        Console.WriteLine(format:
          "{0} was born on {1:dd/MM/yy} and found on {2:dd/MM/yy}",
          arg0: Name,
          arg1: DateOfBirth,
          arg2: FoundDate);
    }
}
```

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
            Cat[] cats =
            {
                new Cat("Nana", new DateTime(2019, 12, 9)),
                new Cat("Coffee", new DateTime(2019, 6, 20)),
                new Cat("Kiwi", new DateTime(2018, 11, 19))
            };

            // Initialize objects by using an object initializer
            // Cat: base class or parent class
            // Wild: derived class or child class
            WildCat leopard = new WildCat
            {
                Name = "Alice",
                CountryCode = "Taiwan"
            };
            Cat petCat = leopard;

            leopard.WriteToConsole();
            petCat.WriteToConsole();
            Console.WriteLine(leopard.GetName());
            Console.WriteLine(petCat.GetName());        
            // It is calling the method from the derived class. Strange?
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
    public class Cat
    {
        /*
        Class field, including different variables
        they can be organized by similar characteristics
        */

        // The name of the cat
        public string Name;
        // The birthday of the cat
        public DateTime DateOfBirth;
        // The children of the cat
        public List<Cat> Children = new List<Cat>();

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

        // Must should have existed from the base class, from System.Object in this case
        // ToString has been defined in the namespace System
        public override string ToString()
        {
            return Name;
        }

        public virtual string GetName()
        {
            return Name;
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

        // overridden methods
        // based on the keyword override
        public override string ToString()
        {
            return $"{Name}: from {CountryCode}";
        }

        public override string GetName()
        {
            return $"{Name}: from {CountryCode}";
        }

        // hidden methods
        // based on the keyword new
        public new void WriteToConsole()
        {
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
$ Alice was born on 15/03/22 and found on 01/01/01
$ Alice was born on a Tuesday.
$ Alice: from Taiwan
$ Alice: from Taiwan
```

## Initialize a Derived Class

You can create a constructor and pass the base object during the declaration.

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
            WildCat nanaQ = new WildCat(nana);

            nana.WriteToConsole();
            leopard.WriteToConsole();
            nanaQ.WriteToConsole();
        }
    }
}
```

In PetLibrary/Animals.cs,
```csharp
public class WildCat : Cat
{
    public string? CountryCode { get; set; }
    public DateTime FoundDate { get; set; }

    // Default Constructor
    public WildCat()
    {
    }

    // Parameterized Constructor
    public WildCat(Cat cat)
    {
        this.Name = cat.Name;
        this.DateOfBirth = cat.DateOfBirth;
    }

    // hidden methods
    // based on the keyword new
    public new void WriteToConsole()
    {
        // base.WriteToConsole();
        Console.WriteLine(format:
          "{0} was born on {1:dd/MM/yy} and found on {2:dd/MM/yy}",
          arg0: Name,
          arg1: DateOfBirth,
          arg2: FoundDate);
    }
}
```

## base Keyword [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/base)

The base keyword is used to access members of the base class from within a derived class.



## Virtual Function [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/virtual)

By default, methods are non-virtual. You cannot override a non-virtual method.


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
namespace CRC_CSD_06;

class Program
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
        nana.Move();
        leopard.Speak();
        leopard.Move();
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
        public virtual void Move()
        {
            Console.WriteLine("Move like an animal!");
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

        // Constructors
        // Default constructor. It will be called by default
        public Cat()
        {
            Name = "UnknownCat";
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

        /*
        Class methods, where functions should be implemented
        */
        public override void Speak()
        {
            Console.WriteLine("Meow!");
        }

        public override void Move()
        {
            Console.WriteLine("Move like a cat!");
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

## Preventing Inheritance and Overriding

Using the keyword sealed.

In PetLibrary/Animals.cs, add
```csharp
public class WildCat : Cat
{
    public sealed override string GetName()
    {
        return $"{Name}: from {CountryCode}";
    }  
}

public class MonsterCat : WildCat
{
    public override string GetName()
    {
        return "I am a monster.";
    }
}
```
```bash
$ cannot override inherited member because it is sealed
```

---
# Selected Theory

## Useful Advanced C# Data Structure

* Collections [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/collections). We will introduce one-by-one later.

## File Management

* Create and publish a package with the dotnet CLI [Doc](https://learn.microsoft.com/en-us/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli)

* Visual Studio Code Tips and Tricks [Doc](https://code.visualstudio.com/docs/getstarted/tips-and-tricks)