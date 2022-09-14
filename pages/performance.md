---
# permalink: /about/
layout: single
title: "Performance"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2022-03-13
---

## .NET Processes

Any process have three default streams:

- **Standard input (stdin)**: This is the only input stream for all terminal or console applications. When you call Console.ReadLine or Console.Read the result is taken from this stream.

- **Standard Output (stdout)**: When you call output-related commands in the console singleton class, all data goes here. For example the WriteLine or the Write methods.

- **Standard error (stderr)**: This is a dedicated stream to print error-related content to the console. This is dedicated because some applications and scripting solutions want to hide these messages.

```csharp
using System;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            string ?name = "";

            // Check if parameters are passed
            // If no, ask for the name
            if (args.Length == 0)
            {
                Console.WriteLine("Hello, please tell me your name:");
                // standard input
                name = Console.ReadLine();
            }
            else
            {
                name = args[0];
            }

            // standard output
            Console.WriteLine("stdout: Hello {0} ", name);
            // standard error
            Console.Error.WriteLine("stderr: Hello {0} ", name);
        }
    }
}
```
Type the following commands at the terminal,
```bash
$ dotnet run
```
```bash
$ dotnet run Yun
```
```bash
// Redirect standard output to a file called stdout.txt
$ dotnet run > stdout.txt
```
```bash
// Redirect standard error to a file called stderr.txt
$ dotnet run 2> stderr.txt
```

## try-catch-finally statement [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/try-catch)

The try-catch statement consists of a try block followed by one or more catch clauses, which specify handlers for different exceptions.

When an exception is thrown, the common language runtime (CLR) looks for the **catch** statement that handles this exception.

```csharp
using System;
using System.IO;
using static System.IO.Path;
using static System.Environment;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            string file = Combine(CurrentDirectory, "mydata.txt");
            Console.WriteLine($"my file = {file}");
            StreamReader? textReader = null;

            // try-catch-finally statement
            try
            {
                textReader = File.OpenText(file);
                // output all the contents of the file
                Console.WriteLine(textReader.ReadToEnd());
            }
            catch (Exception ex)
            {
                Console.WriteLine($"{ex.GetType()} says {ex.Message}");
            }
            finally
            {
                // if textReader is not null, we close it
                if (textReader != null)
                {
                    textReader.Close();
                }
            }
            Console.WriteLine("End of the program!");
        }
    }
}
```
When you have mydata.txt under the folder
```bash
my file = /Users/yun/Dropbox/FH/BCC/C-Sharp/Codes/CRC_CSD-10/MyBusiness/mydata.txt
Hello yun
End of the program!
```
When you don't have mydata.txt under the folder
```bash
my file = /Users/yun/Dropbox/FH/BCC/C-Sharp/Codes/CRC_CSD-10/MyBusiness/mydata.txt
System.IO.FileNotFoundException says Could not find file '/Users/yun/Dropbox/FH/BCC/C-Sharp/Codes/CRC_CSD-10/MyBusiness/mydata.txt'.
End of the program!
```

## Define Exception for Your Own Class

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
            Cat coffee = new Cat("Coffee", new DateTime(2022, 3, 20));

            try
            {
                coffee.getPregnant(new DateTime(2023, 3, 31));
                coffee.getPregnant(new DateTime(2022, 4, 25));
            }
            catch (CatException ex)
            {
                Console.WriteLine(ex.Message);
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

        // method
        public void getPregnant(DateTime day)
        {
            if ((day.Year - DateOfBirth.Year) < 1)
            {
                // It is a handler
                throw new CatException("The cat is too young for producing a kitty!");
            }
            else
            {
                Console.WriteLine($"Ok in {day.Year}!");
            }
        }
    }

    public class CatException : Exception
    {
        // Unlike ordinary methods, constructors are not inherited,
        // so we must explicitly declare and explicitly call the base constructor implementations in System.Exception
        public CatException() : base() { }
        public CatException(string message) : base(message) { }
        public CatException(
          string message, Exception innerException)
          : base(message, innerException) {
           }
    }
}
```
```bash
$ Ok in 2023!
$ The cat is too young for producing a kitty!
```

## Nested Exception

```csharp
// Nested exception
try
{
    try
    {
        var num = int.Parse("abc"); // Throws FormatException               
    }
    catch (FormatException fe)
    {
        try
        {
            coffee.getPregnant(new DateTime(2022, 4, 25));
        }
        catch (CatException ex)
        {
            throw new CatException(ex.Message, fe);
        }
    }
}
catch (CatException oex)
{
    string inMes = "", outMes = "";
    if (oex.InnerException != null)
    {
        // Inner exception (FormatException) message
        inMes = oex.InnerException.Message;
    }
    outMes = oex.Message;
    Console.WriteLine($"Inner Exception:\n\t{inMes}");
    Console.WriteLine($"Outter Exception:\n\t{outMes}");
}
```
```bash
Inner Exception:
        Input string was not in a correct format.
Outter Exception:
        The cat is too young for producing a kitty!
```

# Patterns

Patterns are distilled commonality that you find in programs.

- **Design Patterns:** solves reoccurring problems in software construction.

- **Architectural Patterns:** fundamental structural organization for software systems. Architectural patterns are seen as commonality at higher level than design patterns.

# Event-Driven Architecture Pattern

**Event-driven architecture (EDA)** The event-driven architecture pattern is a popular distributed **asynchronous** architecture pattern used to produce highly scalable applications.

# Raising and handling events

- Event [Doc](https://docs.microsoft.com/en-us/dotnet/standard/events/)
- Event Handler [Doc](https://docs.microsoft.com/en-us/dotnet/api/system.eventhandler-1?view=net-6.0)

## Built-in EventHandler Delegate

In Program.cs
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
            // Create a cat and initialize it
            Cat coffee = new Cat("Coffee", new DateTime(2019, 6, 20));

            // coffee.Shout is an event handler
            coffee.Shout += Cat_Shout; // register with an event
            coffee.Poke();
            coffee.Poke();
            coffee.Poke();
            coffee.Poke();
        }

        // This is a method outside of the Main() method
        private static void Cat_Shout(object sender, EventArgs e)
        {
            Cat cat = (Cat)sender;
            Console.WriteLine($"{cat.Name} is at angry level: {cat.AngerLevel}.");
        }
    }
}
```
In Animals.cs
```csharp
using System;
using System.Collections.Generic;

/*
The animal namespace
*/
namespace Animals
{
    // Reuse IComparable<Cat>
    public class Cat
    {
        /*
        Class field, including different variables
        they can be organized by similar characteristics
        */

        // The name of the cat
        public string Name { get; set; }
        public DateTime DateOfBirth { get; set; }
        public int AngerLevel { get; set; }

        /*
        Class methods, where functions should be implemented
        */

        // Constructors
        // Default constructor. It will be called by default
        public Cat()
        {
            Name = "";
        }
        // Parameterized Constructor
        public Cat(string name, DateTime dateOfBirth)
        {
            this.Name = name;
        }

        // event delegate field
        public event EventHandler? Shout;

        // method
        public void Poke()
        {
            AngerLevel++;
            if (AngerLevel >= 3)
            {
                // if something is listening...
                if (Shout != null)
                {
                    // ...then call the delegate
                    Shout.Invoke(this, EventArgs.Empty);
                }
            }
        }
    }
}
```
```bash
Coffee is at angry level: 3.
Coffee is at angry level: 4.
```

# Understanding processes, threads, and tasks

- **Process:** A process, with one example being each of the console applications we have created has resources like memory and threads allocated to it.

- **Thread:** A thread executes your code, statement by statement. By default, each process only has one thread, and this can cause problems when we need to do more than one **task** at the same time.

## Running multiple actions synchronously

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using System.Diagnostics;

// namespace
namespace MyBusiness
{
    // main program
    internal class Program
    {
        static void Main(string[] args)
        {
            var timer = Stopwatch.StartNew();
            Console.WriteLine("Running methods synchronously on one thread.");
            MethodA();
            MethodB();
            MethodC();
            Console.WriteLine($"{timer.ElapsedMilliseconds:#,##0}ms elapsed.");
        }
        static void MethodA()
        {
            Console.WriteLine("Starting Method A...");
            Thread.Sleep(3000); // simulate three seconds of work
            Console.WriteLine("Finished Method A.");
        }
        static void MethodB()
        {
            Console.WriteLine("Starting Method B...");
            Thread.Sleep(2000); // simulate two seconds of work
            Console.WriteLine("Finished Method B.");
        }
        static void MethodC()
        {
            Console.WriteLine("Starting Method C...");
            Thread.Sleep(1000); // simulate one second of work
            Console.WriteLine("Finished Method C.");
        }
    }
}
```
```bash
$ Running methods synchronously on one thread.
$ Starting Method A...
$ Finished Method A.
$ Starting Method B...
$ Finished Method B.
$ Starting Method C...
$ Finished Method C.
$ 6,033ms elapsed.
```

## Running tasks asynchronously

```csharp
static void Main(string[] args)
{
    var timer = Stopwatch.StartNew();
    // WriteLine("Running methods synchronously on one thread.");
    // MethodA();
    // MethodB();
    // MethodC();

    Console.WriteLine("Running methods asynchronously on multiple threads.");
    Task taskA = new Task(MethodA);
    taskA.Start();
    Task taskB = Task.Factory.StartNew(MethodB);
    Task taskC = Task.Run(new Action(MethodC));
    Console.WriteLine($"{timer.ElapsedMilliseconds:#,##0}ms elapsed.");
}
```
```bash
Running methods asynchronously on multiple threads.
$ Starting Method A...
$ Starting Method C...
$ Starting Method B...
$ 22ms elapsed.
```

**[IMPORTANT]** It is even possible that the console app will end before one or more of the tasks have a chance to start and write to the console!

## Waiting for tasks

```csharp
Task[] tasks = { taskA, taskB, taskC };
Task.WaitAll(tasks);
```
```bash
$ Running methods asynchronously on multiple threads.
$ Starting Method C...
$ Starting Method A...
$ Starting Method B...
$ Finished Method C.
$ Finished Method B.
$ Finished Method A.
3,024ms elapsed.
```

## Accessing a resource from multiple threads

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using System.Diagnostics;

// namespace
namespace MyBusiness
{
    // main program
    internal class Program
    {
        static Random r = new Random();
        static string Message = ""; // a shared resource

        static void Main(string[] args)
        {
            Console.WriteLine("Please wait for the tasks to complete.");
            Stopwatch watch = Stopwatch.StartNew();

            Task a = Task.Factory.StartNew(MethodA);
            Task b = Task.Factory.StartNew(MethodB);

            // Wait until all tasks are finished
            Task.WaitAll(new Task[] { a, b });
            Console.WriteLine();

            Console.WriteLine($"Results: {Message}.");
            Console.WriteLine($"{watch.ElapsedMilliseconds:#,##0} elapsed milliseconds.");
        }
        static void MethodA()
        {
            // add 5 times char A in the message
            for (int i = 0; i < 5; i++)
            {
                Thread.Sleep(r.Next(2000));
                Message += "A";
                Console.Write(".");
            }
        }
        static void MethodB()
        {
            // add 5 times char B in the message
            for (int i = 0; i < 5; i++)
            {
                Thread.Sleep(r.Next(2000));
                Message += "B";
                Console.Write(".");
            }
        }
    }
}
```
```bash
$ Please wait for the tasks to complete.
$ ..........
$ Results: BABABABAAB.
$ 8,677 elapsed milliseconds.
```

**[IMPORTANT]** This shows that both threads were modifying the message concurrently. In an actual application, this could be a problem.

## Applying a mutually exclusive lock to a resource

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;
using System.Diagnostics;

// namespace
namespace MyBusiness
{
    // main program
    internal class Program
    {
        static Random r = new Random();
        static string Message = ""; // a shared resource

        static void Main(string[] args)
        {
            Console.WriteLine("Please wait for the tasks to complete.");
            Stopwatch watch = Stopwatch.StartNew();

            Task a = Task.Factory.StartNew(MethodA);
            Task b = Task.Factory.StartNew(MethodB);

            // Wait until all tasks are finished
            Task.WaitAll(new Task[] { a, b });
            Console.WriteLine();

            Console.WriteLine($"Results: {Message}.");
            Console.WriteLine($"{watch.ElapsedMilliseconds:#,##0} elapsed milliseconds.");
        }
        static object conch = new object();
        static void MethodA()
        {
            // add 5 times char A in the message
            lock (conch)
            {
                for (int i = 0; i < 5; i++)
                {
                    Thread.Sleep(r.Next(2000));
                    Message += "A";
                    Console.Write(".");
                }
            }
        }
        static void MethodB()
        {
            // add 5 times char B in the message
            lock (conch)
            {
                for (int i = 0; i < 5; i++)
                {
                    Thread.Sleep(r.Next(2000));
                    Message += "B";
                    Console.Write(".");
                }
            }
        }
    }
}
```
```bash
$ Please wait for the tasks to complete.
$ ..........
$ Results: AAAAABBBBB.
$ 7,701 elapsed milliseconds.
```

## Understanding the lock statement

```csharp
lock (conch)
{
    // work with shared resource
}
```
```csharp
try
{
    Monitor.Enter(conch);
    // work with shared resource
}
finally
{
    Monitor.Exit(conch);
}
```

## Avoiding deadlocks

Replace the MethodA() to the following content by giving a time to avoid potential deadlock.

```csharp
static void MethodA()
{
    // add 5 times char A in the message
    try
    {
        // Add a timer to avoid potential deadlock
        if (Monitor.TryEnter(conch, TimeSpan.FromSeconds(15)))
        {
            for (int i = 0; i < 5; i++)
            {
                Thread.Sleep(r.Next(2000));
                Message += "A";
                Console.Write(".");
            }
        }
        else
        {
            Console.WriteLine("Method A failed to enter a monitor lock.");
        }
    }
    finally
    {
        Monitor.Exit(conch);
    }
}
```
```bash
$ Please wait for the tasks to complete.
$ ..........
$ Results: BBBBBAAAAA.
$ 9,263 elapsed milliseconds.
```

---
# Selected Theory

## Deadlock
