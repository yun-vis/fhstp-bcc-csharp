---
# permalink: /about/
layout: single
title: "File IO"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2022-03-30
---

# Program Deployment

## Understanding .NET components

- **Deployment:** Software deployment is all of the activities that make a software system available for use.

- **Language Compilers:** These turn your source code written with languages C# into intermediate language (IL) code stored in assemblies.
With C# 6.0 and later, Microsoft switched to an open source rewritten compiler known as **Roslyn** that is also used by Visual Basic.

- **Assembly:** Assemblies form the fundamental units of deployment, version control, reuse, activation scoping, and security permissions for .NET-based applications. An assembly is a collection of types and resources that are built to work together and form a logical unit of functionality. Assemblies take the form of executable (.exe) or dynamic link library (.dll) files, and are the building blocks of .NET applications. [Doc](https://docs.microsoft.com/en-us/dotnet/standard/assembly/)

- **Common Language Runtime (CLR):** This runtime loads assemblies, compiles
the IL code stored in them into native code instructions for your computer's CPU, and
executes the code within an environment that manages resources such as threads and
memory.

- **Package (Class Library) Manager:** A package manager or package-management system is a collection of software tools that automates the process of installing, upgrading, configuring, and removing computer programs for a computer in a consistent manner.

  - Packages can ship on their own schedule.
  - Packages can be tested independently of other packages.
  - Packages can support different OSes and CPUs by including multiple versions of the same assembly built for different OSes and CPUs.
  - Packages can have dependencies specific to only one library.
  - Apps are smaller because unreferenced packages aren't part of the distribution.

- **NuGet Packages:** NuGet is the package manager for .NET. Note that .NET is split into a set of packages, distributed using a Microsoft-supported package management technology named NuGet. [Doc](https://www.nuget.org/ )

- **Base Class Library (BCL):** The BCL provides the most foundational types and utility functionality and is the base of all other .NET class libraries. [Doc](https://docs.microsoft.com/en-us/dotnet/standard/framework-libraries)

- **Namespace:** A namespace is the address of a type. Namespaces are a mechanism to uniquely identify a type by requiring a full address rather than just a short name.

- **Dependency:** A dependency in programming is an essential functionality, library or piece of code that is essential for a different part of the code to work.

## Understanding the Microsoft .NET project SDKs

By default, console applications have a dependency reference on the Microsoft .NET SDK. This platform contains thousands of types in NuGet packages that almost all applications would need, such as the int and string types.

- **Software Development Kit (SDK):** A software development kit (SDK) is a collection of software development tools in one installable package.

- **Application Programming Interface (API):** An application programming interface is a connection between computers or between computer programs.

## Understanding Frameworks

There is a two-way relationship between frameworks and packages. Packages define the APIs, while frameworks group packages. A framework without any packages would not define
any APIs.

```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>
```
```bash
$ cd C:\Program Files\dotnet\sdk // Windows 10
$ cd /usr/local/share/dotnet/sdk // MacOS
```

```csharp
using System;
// using System.Xml.Linq;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            // XDocument doc = new XDocument();
            Stack<int> myStack = new Stack<int>();
        }
    }
}
```

# Publishing your Applications for Deployment

- Framework-dependent Deployment (FDD).
- Framework-dependent Executables (FDEs).
- Self-contained.

```csharp
using System;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("I can run everywhere!");
        }
    }
}
```
```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
    <RuntimeIdentifiers>
      win10-x64;osx-x64;rhel.7.4-x64
    </RuntimeIdentifiers>
  </PropertyGroup>
</Project>
```

- The win10-x64 RID value means Windows 10 or Windows Server 2016.
- The osx-x64 RID value means macOS High Sierra 10.13 or later.
- The rhel.7.4-x64 RID value means Red Hat Enterprise Linux (RHEL) 7.4 or later.

You can find the currently supported Runtime Identifier (RID) values from [Doc](https://docs.microsoft.com/en-us/dotnet/articles/core/rid-catalog).

## Understanding dotnet Commands

- dotnet restore: This downloads dependencies for the project.
- dotnet build: This compiles the project.
- dotnet test: This runs unit tests on the project.
- dotnet run: This runs the project.
- dotnet pack: This creates a NuGet package for the project.
- dotnet publish: This compiles and then publishes the project, either with dependencies
or as a self-contained application.
- dotnet add package: This adds a reference to a package or class library to the project.
- dotnet remove package: This removes a reference to a package or class library from the project.
- dotnet list package: This lists the package or class library references for the project.

```bash
$ dotnet list package
$ dotnet add package Newtonsoft.Json --version 13.0.1
```
```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="13.0.1" />
  </ItemGroup>
</Project>
```

## Publishing a Self-Contained App

- **Debug Mode:** Debug includes debugging information in the compiled files (allowing easy debugging).
- **Release Mode:** Release usually has optimizations enabled.

```bash
$ dotnet publish -c Release -r osx-x64
```

## Publishing a Single-File App

```bash
$ dotnet publish -r osx-x64 -c Release --self-contained=false -p:PublishSingleFile=true
// The pdb is a program database file that stores debugging information.
```

## Packaging a Library for NuGet
```bash
$ dotnet build -c Release
$ dotnet pack -c Release
```

# Managing the filesystem

## Handling cross-platform environments and filesystems

```csharp
using System.IO; // types for managing the filesystem
using static System.Console;
using static System.IO.Directory;
using static System.IO.Path;
using static System.Environment;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            OutputFileSystemInfo();
        }
        static void OutputFileSystemInfo()
        {
            WriteLine("{0,-33} {1}", "Path.PathSeparator", PathSeparator);
            WriteLine("{0,-33} {1}", "Path.DirectorySeparatorChar",
              DirectorySeparatorChar);
            WriteLine("{0,-33} {1}", "Directory.GetCurrentDirectory()",
              GetCurrentDirectory());
            WriteLine("{0,-33} {1}", "Environment.CurrentDirectory",
              CurrentDirectory);
            WriteLine("{0,-33} {1}", "Environment.SystemDirectory",
              SystemDirectory);
            WriteLine("{0,-33} {1}", "Path.GetTempPath()", GetTempPath());
            WriteLine("GetFolderPath(SpecialFolder");
            WriteLine("{0,-33} {1}", " .System)",
              GetFolderPath(SpecialFolder.System));
            WriteLine("{0,-33} {1}", " .ApplicationData)",
              GetFolderPath(SpecialFolder.ApplicationData));
            WriteLine("{0,-33} {1}", " .MyDocuments)",
              GetFolderPath(SpecialFolder.MyDocuments));
            WriteLine("{0,-33} {1}", " .Personal)",
              GetFolderPath(SpecialFolder.Personal));
        }
    }
}
```
On Windows
```bash
$ Path.PathSeparator                ;
$ Path.DirectorySeparatorChar       \
$ Directory.GetCurrentDirectory()   C:\Users\lbwu\Dropbox\FH\BCC\C-Sharp\Codes\CRC_CSD-09\MyBusiness
$ Environment.CurrentDirectory      C:\Users\lbwu\Dropbox\FH\BCC\C-Sharp\Codes\CRC_CSD-09\MyBusiness
$ Environment.SystemDirectory       C:\Windows\system32
$ Path.GetTempPath()                C:\Users\lbwu\AppData\Local\Temp\
$ GetFolderPath(SpecialFolder
$  .System)                         C:\Windows\system32
$  .ApplicationData)                C:\Users\lbwu\AppData\Roaming
$  .MyDocuments)                    C:\Users\lbwu\Documents
$  .Personal)                       C:\Users\lbwu\Documents
```
On MacOS
```bash
$ Path.PathSeparator                :
$ Path.DirectorySeparatorChar       /
$ Directory.GetCurrentDirectory()   /Users/yun/Dropbox/FH/BCC/C-Sharp/Codes/CRC_CSD-09/MyBusiness
$ Environment.CurrentDirectory      /Users/yun/Dropbox/FH/BCC/C-Sharp/Codes/CRC_CSD-09/MyBusiness
$ Environment.SystemDirectory       /System
$ Path.GetTempPath()                /var/folders/y_/jv3nd6kn7_v88vmf3378p6p40000gn/T/
$ GetFolderPath(SpecialFolder
$  .System)                         /System
$  .ApplicationData)                /Users/yun/.config
$  .MyDocuments)                    /Users/yun
$  .Personal)                       /Users/yun
```

## Managing drives

```csharp
using System.IO; // types for managing the filesystem
using static System.Console;
using static System.IO.Directory;
using static System.IO.Path;
using static System.Environment;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            WorkWithDrives();
        }
        static void WorkWithDrives()
        {
            WriteLine("{0,-30} | {1,-10} | {2,-7} | {3,18} | {4,18}",
              "NAME", "TYPE", "FORMAT", "SIZE (BYTES)", "FREE SPACE");
            foreach (DriveInfo drive in DriveInfo.GetDrives())
            {
                if (drive.IsReady)
                {
                    WriteLine(
                            "{0,-30} | {1,-10} | {2,-7} | {3,18:N0} | {4,18:N0}",
                            drive.Name, drive.DriveType, drive.DriveFormat,
                            drive.TotalSize, drive.AvailableFreeSpace);
                }
                else
                {
                    WriteLine("{0,-30} | {1,-10}", drive.Name, drive.DriveType);
                }
            }
        }
    }
}
```
On Windows
```bash
$ NAME                           | TYPE       | FORMAT  |       SIZE (BYTES) |         FREE SPACE
$ C:\                            | Fixed      | NTFS    |  1,023,551,021,056 |    776,903,106,560
$ H:\                            | Network    | NTFS    |      2,147,483,648 |      2,147,479,552
$ I:\                            | Network    | NTFS    |  6,596,932,399,104 |    260,713,881,600
$ M:\                            | Network    | NTFS    |     53,687,091,200 |     53,687,091,200
$ P:\                            | Network    | NTFS    |  2,199,005,425,664 |    834,980,163,584
$ S:\                            | Network    | NTFS    |  6,596,932,399,104 |    260,713,881,600
```
On MacOS
```bash
$ NAME                           | TYPE       | FORMAT  |       SIZE (BYTES) |         FREE SPACE
$ /                              | Fixed      | apfs    |    500,068,036,608 |      6,828,941,312
$ /dev                           | Ram        | devfs   |            194,048 |                  0
$ /private/var/vm                | Fixed      | apfs    |    500,068,036,608 |      6,828,941,312
$ /net                           | Network    | autofs  |                  0 |                  0
$ /home                          | Network    | autofs  |                  0 |                  0
$ /Volumes/firmwaresyncd.J9xOfG  | Fixed      | msdos   |        206,472,192 |        182,210,048
```

## Managing directories

```csharp
using System.IO; // types for managing the filesystem
using static System.Console;
using static System.IO.Directory;
using static System.IO.Path;
using static System.Environment;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            WorkWithDirectories();
        }
        static void WorkWithDirectories()
        {
            // define a directory path for a new folder // starting in the user's folder
            var newFolder = Combine(
                GetFolderPath(SpecialFolder.Personal),
                "Desktop", "MyNewFolder");
            WriteLine($"Working with: {newFolder}"); // check if it exists
            WriteLine($"Does it exist? {Exists(newFolder)}");
            // create directory
            WriteLine("Creating it...");
            CreateDirectory(newFolder);
            WriteLine($"Does it exist? {Exists(newFolder)}");
            Write("Confirm the directory exists, and then press ENTER: ");
            ReadLine();
            // delete directory
            WriteLine("Deleting it...");
            Delete(newFolder, recursive: true);
            WriteLine($"Does it exist? {Exists(newFolder)}");
        }
    }
}
```
```bash
$ Working with: /Users/yun/Desktop/MyNewFolder
$ Does it exist? False
$ Creating it...
$ Does it exist? True
$ Confirm the directory exists, and then press ENTER:
$ Deleting it...
$ Does it exist? False
```

## Managing files

```csharp
using System.IO; // types for managing the filesystem
using static System.Console;
using static System.IO.Directory;
using static System.IO.Path;
using static System.Environment;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            WorkWithFiles();
        }
        static void WorkWithFiles()
        {
            // define a directory path to output files // starting in the user's folder
            var dir = Combine(
                GetFolderPath(SpecialFolder.Personal),
                "Desktop", "MyNewFiles");
            CreateDirectory(dir);
            // define file paths
            string textFile = Combine(dir, "Dummy.txt");
            string backupFile = Combine(dir, "Dummy.bak");
            WriteLine($"Working with: {textFile}");
            // check if a file exists
            WriteLine($"Does it exist? {File.Exists(textFile)}");
            // create a new text file and write a line to it
            StreamWriter textWriter = File.CreateText(textFile);
            textWriter.WriteLine("Hello, C#!");
            textWriter.Close(); // close file and release resources
            WriteLine($"Does it exist? {File.Exists(textFile)}");
                                // copy the file, and overwrite if it already exists
            File.Copy(sourceFileName: textFile,
              destFileName: backupFile, overwrite: true);
            WriteLine(
              $"Does {backupFile} exist? {File.Exists(backupFile)}");
            Write("Confirm the files exist, and then press ENTER: ");
            ReadLine();
            // delete file
            File.Delete(textFile);
            WriteLine($"Does it exist? {File.Exists(textFile)}");
            // read from the text file backup
            WriteLine($"Reading contents of {backupFile}:");
            StreamReader textReader = File.OpenText(backupFile);
            WriteLine(textReader.ReadToEnd());
            textReader.Close();
        }
    }
}
```
```bash
$ Working with: /Users/yun/Desktop/MyNewFiles/Dummy.txt
$ Does it exist? False
$ Does /Users/yun/Desktop/MyNewFiles/Dummy.bak exist? True
$ Confirm the files exist, and then press ENTER:
$ Does it exist? False
$ Reading contents of /Users/yun/Desktop/MyNewFiles/Dummy.bak:
$ Hello, C#!
```

## Managing paths

```csharp
// Managing paths
WriteLine($"Folder Name: {GetDirectoryName(textFile)}");
WriteLine($"File Name: {GetFileName(textFile)}");
WriteLine("File Name without Extension: {0}",
  GetFileNameWithoutExtension(textFile));
WriteLine($"File Extension: {GetExtension(textFile)}");
WriteLine($"Random File Name: {GetRandomFileName()}");
WriteLine($"Temporary File Name: {GetTempFileName()}");
```
```bash
$ Folder Name: /Users/yun/Desktop/MyNewFiles
$ File Name: Dummy.txt
$ File Name without Extension: Dummy
$ File Extension: .txt
$ Random File Name: qh4j5vy2.kgn
$ Temporary File Name: /var/folders/y_/jv3nd6kn7_v88vmf3378p6p40000gn/T/tmpfQQNS0.tmp
```

## Getting file information

```csharp
var info = new FileInfo(backupFile);
WriteLine($"{backupFile}:");
WriteLine($"Contains {info.Length} bytes");
WriteLine($"Last accessed {info.LastAccessTime}");
WriteLine($"Has readonly set to {info.IsReadOnly}")
```
```bash
$ /Users/yun/Desktop/MyNewFiles/Dummy.bak:
$ Contains 11 bytes
$ Last accessed 3/30/2022 8:44:26 AM
$ Has readonly set to False
```

## Reading and writing with streams

```csharp
using System.IO; // types for managing the filesystem
using static System.Console;
using static System.IO.Directory;
using static System.IO.Path;
using static System.Environment;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            WorkWithText();
        }
        // define an array of Viper pilot call signs
        static string[] callsigns = new string[] {
            "Husker", "Starbuck", "Apollo", "Boomer",
            "Bulldog", "Athena", "Helo", "Racetrack" };
        static void WorkWithText()
        {
            // define a file to write to
            string textFile = Combine(CurrentDirectory, "streams.txt");
            // create a text file and return a helper writer
            StreamWriter text = File.CreateText(textFile);
            // enumerate the strings, writing each one
            // to the stream on a separate line
            foreach (string item in callsigns)
            {
                text.WriteLine(item);
            }
            text.Close(); // release resources
                          // output the contents of the file
            WriteLine("{0} contains {1:N0} bytes.",
              arg0: textFile,
              arg1: new FileInfo(textFile).Length);
            WriteLine(File.ReadAllText(textFile));
        }
    }
}
```
```bash
/Users/yun/Dropbox/FH/BCC/C-Sharp/Codes/CRC_CSD-09/MyBusiness/streams.txt contains 60 bytes.
$ Husker
$ Starbuck
$ Apollo
$ Boomer
$ Bulldog
$ Athena
$ Helo
$ Racetrack
```

## Writing to XML streams

```csharp
using System.IO; // types for managing the filesystem
using static System.Console;
using static System.IO.Path;
using static System.Environment;
using System.Xml;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            WorkWithXml();
        }
        // define an array of Viper pilot call signs
        static string[] callsigns = new string[] {
            "Husker", "Starbuck", "Apollo", "Boomer",
            "Bulldog", "Athena", "Helo", "Racetrack" };
        static void WorkWithXml()
        {
            // define a file to write to
            string xmlFile = Combine(CurrentDirectory, "streams.xml");
            // create a file stream
            FileStream xmlFileStream = File.Create(xmlFile);
            // wrap the file stream in an XML writer helper
            // and automatically indent nested elements
            XmlWriter xml = XmlWriter.Create(xmlFileStream,
            new XmlWriterSettings { Indent = true });
            // write the XML declaration
            xml.WriteStartDocument();
            // write a root element
            xml.WriteStartElement("callsigns");
            // enumerate the strings writing each one to the stream
            foreach (string item in callsigns)
            {
                xml.WriteElementString("callsign", item);
            }
            // write the close root element
            xml.WriteEndElement();
            // close helper and stream
            xml.Close();
            xmlFileStream.Close();
            // output all the contents of the file
            WriteLine("{0} contains {1:N0} bytes.",
              arg0: xmlFile,
              arg1: new FileInfo(xmlFile).Length);
            WriteLine(File.ReadAllText(xmlFile));
        }
    }
}
```
```bash
$ /Users/yun/Dropbox/FH/BCC/C-Sharp/Codes/CRC_CSD-09/MyBusiness/streams.xml contains 310 bytes.
$ <?xml version="1.0" encoding="utf-8"?>
$ <callsigns>
$   <callsign>Husker</callsign>
$   <callsign>Starbuck</callsign>
$   <callsign>Apollo</callsign>
$   <callsign>Boomer</callsign>
$   <callsign>Bulldog</callsign>
$   <callsign>Athena</callsign>
$   <callsign>Helo</callsign>
$   <callsign>Racetrack</callsign>
$ </callsigns>
```

## Disposing of file resources

When you open a file to read or write to it, you are using resources outside of .NET. These are called unmanaged resources and must be disposed of when you are done working with them.

```csharp
using System.IO; // types for managing the filesystem
using static System.Console;
using static System.IO.Path;
using static System.Environment;
using System.Xml;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            WorkWithXml();
        }
        // define an array of Viper pilot call signs
        static string[] callsigns = new string[] {
            "Husker", "Starbuck", "Apollo", "Boomer",
            "Bulldog", "Athena", "Helo", "Racetrack" };
        static void WorkWithXml()
        {
            FileStream? xmlFileStream = null;
            XmlWriter? xml = null;
            try
            {
                // define a file to write to
                string xmlFile = Combine(CurrentDirectory, "streams.xml");
                // create a file stream
                xmlFileStream = File.Create(xmlFile);
                // wrap the file stream in an XML writer helper
                // and automatically indent nested elements
                xml = XmlWriter.Create(xmlFileStream,
                new XmlWriterSettings { Indent = true });
                // write the XML declaration
                xml.WriteStartDocument();
                // write a root element
                xml.WriteStartElement("callsigns");
                // enumerate the strings writing each one to the stream
                foreach (string item in callsigns)
                {
                    xml.WriteElementString("callsign", item);
                }
                // write the close root element
                xml.WriteEndElement();
                // close helper and stream
                xml.Close();
                xmlFileStream.Close();
                // output all the contents of the file
                WriteLine($"{0} contains {1:N0} bytes.",
                  arg0: xmlFile,
                  arg1: new FileInfo(xmlFile).Length);
                WriteLine(File.ReadAllText(xmlFile));
            }
            catch (Exception ex)
            {
                WriteLine($"{ex.GetType()} says {ex.Message}");
            }
            finally
            {
                if (xml != null)
                {
                    xml.Dispose();
                    WriteLine("The XML writer's unmanaged resources have been disposed.");
                }
                if (xmlFileStream != null)
                {
                    xmlFileStream.Dispose();
                    WriteLine("The file stream's unmanaged resources have been disposed.");
                }
            }
        }
    }
}
```
```bash
$ 0 contains 1 bytes.
$ <?xml version="1.0" encoding="utf-8"?>
$ <callsigns>
$   <callsign>Husker</callsign>
$   <callsign>Starbuck</callsign>
$   <callsign>Apollo</callsign>
$   <callsign>Boomer</callsign>
$   <callsign>Bulldog</callsign>
$   <callsign>Athena</callsign>
$   <callsign>Helo</callsign>
$   <callsign>Racetrack</callsign>
$ </callsigns>
$ The XML writer\'s unmanaged resources have been disposed.
$ The file stream\'s unmanaged resources have been disposed.
```

## Encoding strings as byte arrays

```csharp
using static System.Console;
using System.Text;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            WorkWithText();
        }
        static void WorkWithText()
        {
            WriteLine("Encodings");
            WriteLine("[1] ASCII");
            WriteLine("[2] UTF-7");
            WriteLine("[3] UTF-8");
            WriteLine("[4] UTF-16 (Unicode)");
            WriteLine("[5] UTF-32");
            WriteLine("[any other key] Default");
            // choose an encoding
            Write("Press a number to choose an encoding: ");
            ConsoleKey number = ReadKey(intercept: false).Key;
            WriteLine();
            WriteLine();
            Encoding encoder = number switch
            {
                ConsoleKey.D1 => Encoding.ASCII,
                ConsoleKey.D2 => Encoding.UTF7,
                ConsoleKey.D3 => Encoding.UTF8,
                ConsoleKey.D4 => Encoding.Unicode,
                ConsoleKey.D5 => Encoding.UTF32,
                _ => Encoding.Default
            };
            // define a string to encode
            string message = "A pint of milk is £1.99";
            // encode the string into a byte array
            byte[] encoded = encoder.GetBytes(message);
            // check how many bytes the encoding needed
            WriteLine("{0} uses {1:N0} bytes.",
              encoder.GetType().Name, encoded.Length);
            // enumerate each byte
            WriteLine($"BYTE HEX CHAR");
            foreach (byte b in encoded)
            {
                WriteLine($"{b,4} {b.ToString("X"),4} {(char)b,5}");
            }
            // decode the byte array back into a string and display it
            string decoded = encoder.GetString(encoded);
            WriteLine(decoded);
        }
    }
}
```
```bash
$ Encodings
$ [1] ASCII
$ [2] UTF-7
$ [3] UTF-8
$ [4] UTF-16 (Unicode)
$ [5] UTF-32
$ [any other key] Default
$ Press a number to choose an encoding: 1

$ ASCIIEncodingSealed uses 23 bytes.
$ BYTE HEX CHAR
$   65   41     A
$   32   20      
$  112   70     p
$  105   69     i
$  110   6E     n
$  116   74     t
$   32   20      
$  111   6F     o
$  102   66     f
$   32   20      
$  109   6D     m
$  105   69     i
$  108   6C     l
$  107   6B     k
$   32   20      
$  105   69     i
$  115   73     s
$   32   20      
$   63   3F     ?
$   49   31     1
$   46   2E     .
$   57   39     9
$   57   39     9
$ A pint of milk is ?1.99
```

## Serializing Objects

- **Serialization:** is the process of converting a live object into a sequence of bytes using a specified format.
- **Deserialization:** is the reverse process of **Serialization:**.

There are dozens of formats you can specify, but the two most common ones are **eXtensible Markup Language (XML)** and **JavaScript Object Notation (JSON)**.

## Serializing as XML

In Program.cs,
```csharp
using System; // DateTime
using System.Collections.Generic; // List<T>, HashSet<T>
using System.Xml.Serialization; // XmlSerializer
using System.Xml;
using static System.Console;
using static System.Environment;
using static System.IO.Path;
using Animals;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            // create an object graph
            var people = new List<Person> {
            new Person(30000M) { FirstName = "Alice",
                LastName = "Smith",
                DateOfBirth = new DateTime(1974, 3, 14) },
            new Person(40000M) { FirstName = "Bob",
                LastName = "Jones",
                DateOfBirth = new DateTime(1969, 11, 23) },
            new Person(20000M) { FirstName = "Charlie",
                LastName = "Cox",
                DateOfBirth = new DateTime(1984, 5, 4),
                Children = new HashSet<Person> {
                    new Person(0M) { FirstName = "Sally",
                    LastName = "Cox",
                    DateOfBirth = new DateTime(2000, 7, 12) } } }
            };
            // create object that will format a List of Persons as XML
            var xs = new XmlSerializer(typeof(List<Person>));
            // create a file to write to
            string path = Combine(CurrentDirectory, "people.xml");
            using (FileStream stream = File.Create(path))
            {
                // serialize the object graph to the stream
                xs.Serialize(stream, people);
            }
            WriteLine("Written {0:N0} bytes of XML to {1}",
              arg0: new FileInfo(path).Length,
              arg1: path);
            WriteLine();
            // Display the serialized object graph
            WriteLine(File.ReadAllText(path));
        }
    }
}
```
In Animals.cs,
```csharp
using System;
using System.Collections.Generic;

/*
The animal namespace
*/
namespace Animals
{
    public class Person
    {
        public Person()
        {
        }
        public Person(decimal initialSalary)
        {
            Salary = initialSalary;
        }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime DateOfBirth { get; set; }
        public HashSet<Person> Children { get; set; }
        protected decimal Salary { get; set; }
    }
}
```
```bash
$ <?xml version="1.0" encoding="utf-8"?><ArrayOfPerson xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"><Person><FirstName>Alice</FirstName><LastName>Smith</LastName><DateOfBirth>1974-03-14T00:00:00</DateOfBirth></Person><Person><FirstName>Bob</FirstName><LastName>Jones</LastName><DateOfBirth>1969-11-23T00:00:00</DateOfBirth></Person><Person><FirstName>Charlie</FirstName><LastName>Cox</LastName><DateOfBirth>1984-05-04T00:00:00</DateOfBirth><Children><Person><FirstName>Sally</FirstName><LastName>Cox</LastName><DateOfBirth>2000-07-12T00:00:00</DateOfBirth></Person></Children></Person></ArrayOfPerson>
```

## Deserializing XML files

```csharp
using (FileStream xmlLoad = File.Open(path, FileMode.Open))
{
    // deserialize and cast the object graph into a List of Person
    var loadedPeople = (List<Person>)xs.Deserialize(xmlLoad);
    foreach (var item in loadedPeople)
    {
        WriteLine("{0} has {1} children.",
          item.LastName, item.Children.Count);
    }
}
```
```bash
$ Smith has 0 children.
$ Jones has 0 children.
$ Cox has 1 children.
```

## Taking Care of System-Dependent Methods [Doc](https://docs.microsoft.com/en-us/dotnet/api/system.environment.specialfolder?view=net-6.0)

```csharp
using System;

namespace MyBusiness
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.Write("My operating system (OS) is ");
            Console.WriteLine(Environment.OSVersion);
            Console.Write("Is my OS a Mac? ");            
            Console.WriteLine(OperatingSystem.IsMacOS());
            // Console.WriteLine(OperatingSystem.IsWindows());            
            // Console.WriteLine(OperatingSystem.IsLinux());
            Console.Write("Environment.SpecialFolder.Personal = ");
            Console.WriteLine(Environment.GetFolderPath(Environment.SpecialFolder.Personal));
            Console.Write("Environment.SpecialFolder.UserProfile = ");
            Console.WriteLine(Environment.GetFolderPath(Environment.SpecialFolder.UserProfile));
        }
    }
}
```
On Windows
```bash
My operating system (OS) is Microsoft Windows NT 10.0.19044.0
Is my OS a Mac? False
Environment.SpecialFolder.Personal = C:\Users\lbwu\Documents
Environment.SpecialFolder.UserProfile = C:\Users\lbwu
```
On MacOS
```bash
My operating system (OS) is Unix 10.14.6
Is my OS a Mac? True
Environment.SpecialFolder.Personal = /Users/yun
Environment.SpecialFolder.UserProfile = /Users/yun
```

---
# Selected Theory

# File System
