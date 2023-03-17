---
# permalink: /about/
layout: single
title: "C# Basics"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2023-03-16
---

![C# Logo](/assets/images/c-sharp.png)

# C# Environment Setup

Windows:
  * Visual Studio Code: .Net 6, support PUML Class Diagram

MacOS:
  * Visual Studio Code: .Net 6, support PUML Class Diagram (tested on Mojave)

Linux:
  * Visual Studio Code: .Net 6, support PUML Class Diagram (tested Unbuntu 22.04)

## Install Visual Studio Code and .NET 6 SDKs

  * [Visual Studio Code](https://code.visualstudio.com/)
  * [.Net 6 SDKs](https://dotnet.microsoft.com/en-us/download/dotnet)

### Install Visual Studio Code Extension

  * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
  * [CSharp to PlantUML](https://marketplace.visualstudio.com/items?itemName=pierre3.csharp-to-plantuml)
  * [PlantUML](http://www.plantuml.com/plantuml)
  * [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)
  
### Additional Settings (Optional)
  * Turn on Word Wrap in the Setting

## CSharp to PlantUML via Extension
  * Usage: Ctrl+Shift+P to enable the vscode Command Palette and run the command "csharp2plantuml.classDiagram".

## CSharp to PlantUML (Optional, in case you don't find the extension through Visual Studio Code)

  * [CSharp to PlantUML](https://github.com/pierre3/PlantUmlClassDiagramGenerator)
  * Install
  ```bash
  $ dotnet tool install --global PlantUmlClassDiagramGenerator
  ```
  * Use
  ```bash
  $ puml-gen ./Program.cs -public
  ```
  * Copy the text from *.puml and visualize using [PUML Viewer](https://www.planttext.com)

## PlantUML via Extension 
  * PlantUML is an Extension for viewing *.puml files.
  * Prerequisite: 
    * Windows: [Java runtime](https://www.java.com/en/download/)
    * Mac OS: [Java runtime](https://www.java.com/en/download/) + graphviz ($ brew install graphviz) 
  * Usage: Alt+D to enable the preview function

## Markdown All in One via Extension
  * Usage: Ctrl K+V to preview the file


## First Console Program

```bash
$ dotnet --info // check the version installed in your system
$ dotnet new console // Use top-level statements
$ dotnet new console --use-program-main // Skip top-level statements and include Main()
```

In Program.cs [Doc](https://aka.ms/new-console-template)
```csharp
// See https://aka.ms/new-console-template for more information
Console.WriteLine("Hello, World!");
```

```csharp
using System;

namespace CRC_CSD_01
{
    class Program
    {
        static void Main(string[] args) // Where the application begins
        {
            Console.WriteLine("Hello World!");
            // Equal to System.Console.WriteLine("Hello World!");
        }
    }
}
```

In CRC_CSD-00.csproj [Doc](https://docs.microsoft.com/en-us/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file)
```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <RootNamespace>CRC_CSD_01</RootNamespace>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>
```

## How to Check the .Net Version

```bash
$ dotnet --version
```

## How to Build the Program

```bash
$ dotnet build
```

## How to Run the Program (Including Build)

```bash
$ dotnet run
```

## Namespace
```csharp
using System;         // System namespace (defined by C#)
                      // The "using" Directive

namespace CRC_CSD_01  // Application namespace (defined by the programmer)
{
    class Program
    {
        // static: shared method of all instances by the class
        // void: return "nothing" in the method
        // string[] args: parameters passed to the main function.
        // The parameters can be taken when lauching the application.
        static void Main(string[] args) // Where the application begins
        {
            Console.WriteLine("Hello World!");  // Console is a system class
        }
    }
}
```

```csharp
using static System.Console;  // Console is a static system class
/*
A static class is basically the same as a non-static class,
but there is one difference: a static class cannot be instantiated.
*/

namespace CRC_CSD_01  // Application namespace (defined by the programmer)
{
    class Program
    {
        // static: shared method of all instances by the class
        // void: return "nothing" in the method
        // string[] args: parameters passed to the main function.
        // The parameters can be taken when lauching the application.
        static void Main(string[] args) // Where the application begins
        {
            WriteLine("Hello World!");
        }
    }
}
```

```csharp
using System;

namespace CRC_CSD_01
{
    class Program
    {
        // string[] args: parameters passed to the main function.
        static void Main(string[] args) // Where the application begins
        {
            Console.WriteLine("The length of the arguments: " + args.Length);
            for( int i = 0; i < args.Length; i++ ){
                Console.WriteLine(args[i]);
            }
        }
    }
}
```

## Introduction to Markdown language
  * [Markdown Official Website](https://daringfireball.net/projects/markdown/)
  * [Markdown Guide](https://www.markdownguide.org/)


## Writing a README.mk file

  * [Best README Template](https://github.com/othneildrew/Best-README-Template)

  * About The Project
    * Built With
  * Getting Started: How to install and set up your app.
    * Prerequisites
    * Installation
  * Usage: Show useful examples of how a project can be used.
  * Roadmap: What have been implemented and what are the planed features.
  * Contributing: Encourage people to work on your project.
  * License: Your project license
  * Contact
  * Acknowledgments


---
# Selected Theory

## Naming Convention [Doc](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)

### Pascal Case
Use pascal casing ("PascalCasing") when naming a class, record, or struct. When naming public members of types, such as fields, properties, events, methods, and local functions, use pascal casing. When naming an interface, use pascal casing in addition to prefixing the name with an I. This clearly indicates to consumers that it's an interface.

### Camel Case
Use camel casing ("camelCasing") when naming private or internal fields, and prefix them with _.

## Comments
```csharp
// This is a single line comment

/*
This is a multi-line comment
and continues until the end
of comment symbol is reached
*/

/*
The following codes show how I often add comments to my programs.
This is a my application namespace, called My Business.
*/
namespace CRC_CSD_01
{
    /*
    The class of my main program
    */
    class Program
    {
        /*
        Main: The class of my main program, where the application begins.
        Input:
          args: input parameters
        Output:
          none
        */
        static void Main(string[] args)
        {
            System.Console.WriteLine("Hello World!");
        }
    }
}
```

---
# External Resources
