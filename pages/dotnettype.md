---
# permalink: /about/
layout: single
title: ".Net Type"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2022-03-23
---

# How to improve code readability?

# Lambda Expression [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-expressions)

You use a lambda expression to create an anonymous function. Use the lambda declaration operator **=>** to separate the lambda's parameter list from its body. A lambda expression can be of any of the following two forms:

* **Expression lambda** that has an expression as its body:

```csharp
(input-parameters) => expression
```

* **Statement lambda** that has a statement block as its body:

```csharp
(input-parameters) => { <sequence-of-statements> }
```

```csharp
// Lambda Expression
using System;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            // Local function
            bool compareMethod(int a, int b)
            {
                return a == b;
            }

            // Expression lambda
            bool compareLambda(int a, int b) => (a == b);
            Console.WriteLine(compareMethod(3, 4));
            // Console.WriteLine(compareLambda(3, 4));

            // Print a Fibonacci sequence
            for (int i = 0; i < 10; i++)
            {
                Console.WriteLine("The {0} term of the Fibonacci sequence is {1:N0}.",
                  arg0: i + 1,
                  arg1: FibLambda(term: i));
                //   arg1: FibMethod(term: i));
            }
        }

        // Fibonacci sequence
        static int FibMethod(int term)
        {
            switch (term)
            {
                case 0:
                    return 0;
                case 1:
                    return 1;
                default:
                    return FibMethod(term - 1) + FibMethod(term - 2);
            }
        }

        // Fibonacci sequence
        // Statement lambda
        static int FibLambda(int term) => term switch
        {
            0 => 0,
            1 => 1,
            _ => FibLambda(term - 1) + FibLambda(term - 2)
        };
    }
}
```


# Delegates [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/delegates/)

## Basic Delegates

A delegate is a type that represents references to methods with a particular parameter list and return type. When you instantiate a delegate, you can associate its instance with any method with a compatible signature (the method name, and the type and order of parameters) and return type. You can invoke (or call) the method through the delegate instance.

```csharp
using System;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            GenerateMyNumbers generateRandom = new GenerateMyNumbers(GetRandomNumber);
            GenerateMyNumbers generateOrdered = new GenerateMyNumbers(GetOrderedNumber);

            // int[] numbers = generateRandom(10, 3);
            int[] numbers = generateOrdered(10, 3);

            foreach (int n in numbers)
            {
                Console.Write(n + " ");
            }
            Console.WriteLine();
        }

        // My first delegate
        public delegate int[] GenerateMyNumbers(int x, int y);
        // Create an arry with size amount and assign a random value 0-maxNum in this array
        public static int[] GetRandomNumber(int maxNum, int amount)
        {
            Random random = new Random();
            int[] nums = new int[amount];

            for (int i = 0; i < amount; i++)
            {
                // 0 ~ maxNum-1
                nums[i] = random.Next(0, maxNum);
            }
            return nums;
        }
        // Get an ordered integer sequence from min to max
        public static int[] GetOrderedNumber(int max, int min)
        {
            // Avoid when max is smaller than min
            if (max < min)
            {
                int[] noNum = { 0 };
                return noNum;
            }

            // Create an ordered sequence
            int[] nums = new int[max - min + 1];
            for (int i = 0; i <= max - min; i++)
            {
                nums[i] = min + i;
            }
            return nums;
        }
    }
}
```
```bash
$ 3 4 5 6 7 8 9 10
```

* Delegates are used to pass methods as arguments to other methods.
* Delegates can be used to define callback methods.
* Delegates can be chained together; for example, multiple methods can be called on a single event.
* Lambda expressions (in certain contexts) are compiled to delegate types.

```csharp
using System;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            var array = new int[] { 1, 2, 3, 4, 5, 6, 7, 8, 9 };

            // string.Join: method that lets you concatenate each element in an
            // object array without explicitly converting its elements to strings
            Console.WriteLine(string.Join(",", Change1(array)));
            Console.WriteLine(string.Join(",", Change2(array)));
            Console.WriteLine(string.Join(",", Change3(array)));
        }

        // Take out each elemnt in an array and +1
        public static int[] Change1(int[] _array)
        {
            var array = new int[_array.Length];
            for (int i = 0, c = _array.Length; i < c; i++)
            {
                array[i] = _array[i] + 1;
            }
            return array;
        }
        // Take out each elemnt in an array and *2
        public static int[] Change2(int[] _array)
        {
            var array = new int[_array.Length];
            for (int i = 0, c = _array.Length; i < c; i++)
            {
                array[i] = _array[i] * 2;
            }
            return array;
        }
        // Take out each elemnt in an array and square it
        public static int[] Change3(int[] _array)
        {
            var array = new int[_array.Length];
            for (int i = 0, c = _array.Length; i < c; i++)
            {
                array[i] = _array[i] * _array[i];
            }
            return array;
        }
    }
}
```
```bash
$ 2,3,4,5,6,7,8,9,10
$ 2,4,6,8,10,12,14,16,18
$ 1,4,9,16,25,36,49,64,81
```

```csharp
using System;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            var array = new int[] { 1, 2, 3, 4, 5, 6, 7, 8, 9 };

            // Create three delegate objects that take differentmethods as parameters
            MyDelegate myDelegate1 = new MyDelegate(AddOne);
            MyDelegate myDelegate2 = new MyDelegate(MultipleTwo);
            MyDelegate myDelegate3 = new MyDelegate(Square);
            // Syntactic sugar
            // syntactic sugar is syntax within a programming language that is
            // designed to make things easier to read or to express.
            // The following is also a valid syntax;
            // MyDelegate myDelegate3 = Square;

            // string.Join: method that lets you concatenate each element in an
            // object array without explicitly converting its elements to strings
            Console.WriteLine(string.Join(",", Change(array, myDelegate1)));
            Console.WriteLine(string.Join(",", Change(array, myDelegate2)));
            Console.WriteLine(string.Join(",", Change(array, myDelegate3)));
        }

        // Declaration of the delegete
        public delegate int MyDelegate(int x);

        public static int AddOne(int number)
        {
            return number+1;
        }
        public static int MultipleTwo(int number)
        {
            return number*2;
        }
        public static int Square(int number)
        {
            return number * number;
        }

        // A method Change that takes another method as the input parameters
        // In this case, the method (as a parameter) should be defined as a delegate
        public static int[] Change(int[] _array, MyDelegate myDelegate)
        {
            var array = new int[_array.Length];
            for (int i = 0, c = _array.Length; i < c; i++)
            {
                array[i] = myDelegate(_array[i]);
            }
            return array;
        }
    }
}
```
```bash
$ 2,3,4,5,6,7,8,9,10
$ 2,4,6,8,10,12,14,16,18
$ 1,4,9,16,25,36,49,64,81
```

## Lambda Expression with Delegates

```csharp
public static int Square(int number) => number * number;
```

## Func, Action and Predicate Delegates

* **Action Delegate:** Encapsulates a method that has a single parameter and does not return a value. [Doc](https://docs.microsoft.com/en-us/dotnet/api/system.action-1?view=net-6.0)

```csharp
using System;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            Action<int, int> Addition = new Action<int, int>(AddNumbers);
            // Alternative
            // Action<int, int> Addition = AddNumbers;  
            Addition(10, 20);
        }

        // add param1 and param2 and return the sum
        private static void AddNumbers(int param1, int param2)
        {
            int result = param1 + param2;
            Console.WriteLine($"Addition = {result}");
        }
    }
}
```
```bash
$ Addition = 30
```

* **Func<T,TResult> Delegate:** Encapsulates a method that has one parameter and returns a value of the type specified by the TResult parameter. [Doc](https://docs.microsoft.com/en-us/dotnet/api/system.func-2?view=net-6.0)

```csharp
using System;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            // Declare a Func delegate
            Func<int, int, int> Addition = new Func<int, int, int>(AddNumbers);
            // Func<int, int, int> Addition = AddNumbers;
            // Using Lambda Expression
            // Func<int, int, int> Addition = (param1, param2) => param1 + param2;  

            int result = Addition(10, 20);
            Console.WriteLine($"Addition = {result}");
        }

        // add param1 and param2 and return the sum
        private static int AddNumbers(int param1, int param2)
        {
            return param1 + param2;
        }
    }
}
```
```bash
$ Addition = 30
```

* Advantages of Action and Func Delegates
  + Easy and quick to define delegates.
  + Makes code short.
  + Compatible type throughout the application.

* **Predicate Delegate:** Represents the method that defines a set of criteria and determines whether the specified object meets those criteria. [Doc](https://docs.microsoft.com/en-us/dotnet/api/system.predicate-1?view=net-6.0)

Syntax difference between predicate & func is that here in predicate, you don't specify a return type because it is always a **bool**.

```csharp
using System;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            Predicate<string> CheckIfApple = new Predicate<string>(IsApple);
            // Predicate<string> CheckIfApple = IsApple;
            bool result = IsApple("I Phone X");
            if (result){
                Console.WriteLine("It's an IPhone");
            }
        }

        private static bool IsApple(string modelName)
        {
            // Check if the model name an iPhone
            if (modelName == "I Phone X")
                return true;
            else
                return false;
        }
    }
}
```
```bash
$ It\'s an IPhone
```

# Type-testing Operators and Cast Expression

## Implicit Casting

```csharp
WildCat leopard = new WildCat();
Cat petCat = leopard;
// compile error
// WildCat wCat = petCat;  
```

## Explicit Casting

```csharp
WildCat wCat = (WildCat)petCat;
```

## Avoiding Casting Exceptions

```csharp
// WildCat wCat = (WildCat)petCat;
if (petCat is WildCat)
{
    Console.WriteLine($"{nameof(petCat)} IS an WildCat");
    WildCat wCat = (WildCat)petCat;
    // safely do something with wCat
}
```

## is Operator

The **is** operator will check if the run-time type of an expression is compatible with a given type. The **as** operator considers only reference, nullable, boxing, and unboxing conversions.

```csharp
if (petCat is WildCat)
{
}
```

## as Operator

The **as** operator is used to explicitly convert an expression to a given type if its run-time type is compatible with that type. The as operator returns null if the conversion is not possible.

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
            // object is a base class of all derived classes in C#
            object[] myObjects = new object[4];
            myObjects[0] = new Dog();
            myObjects[1] = new Cat();
            myObjects[2] = "Dog";
            myObjects[3] = "Cat";

            for (int i = 0; i < myObjects.Length; i++)
            {
                // Convert an element in the myobjects array to a string element
                string? s = myObjects[i] as string;
                // Print out the current element
                Console.Write($"Inspecting element: {myObjects[i]}");

                // If converted successfully, s will be a string, otherwise return null.
                if (s == null)
                {
                    Console.Write(" --> Incompatible type");
                }
                else
                {
                    Console.Write(" --> Compatible type");
                }
                Console.WriteLine(", with string!");
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
        public override void Speak()
        {
            Console.WriteLine("Meow!");
        }
    }
}
```

---
# Environment.SpecialFolder Enum [Doc](https://docs.microsoft.com/en-us/dotnet/api/system.environment.specialfolder?view=net-6.0)

# Other Interesting Types

* System.Numerics (BigInteger, Complex, Quaternion)

---
# Selected Theory
