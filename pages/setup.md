---
# permalink: /about/
layout: single
title: "C# Basics"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-02-03
---

<!-- ![C# Logo](/fhstp-bcc-csharp/assets/images/c-sharp.png) -->

# C# Environment Setup

Windows:
  * Visual Studio Code: .Net 8 (tested on Windows 10)

MacOS:
  * Visual Studio Code: .Net 8 (tested on ??)

Linux:
  * Visual Studio Code: .Net 8, support PUML Class Diagram (tested on Unbuntu 24.04)

## Install Visual Studio Code and .NET 8 SDKs

  * [Visual Studio Code](https://code.visualstudio.com/)
  * [.Net 8 SDKs (Long Term Support)](https://dotnet.microsoft.com/en-us/download/dotnet)

### Install Visual Studio Code Extension

  * [.NET MAUI (Optional)](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit): official Microsoft extension for MAUI developers.
    * The following two extensions should come automatically.
    * [C# Dev Kit (Mandatory)](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit)
      * Using C# Dev Kit requires you to sign in with a Microsoft account that has an active Visual Studio subscription. Visual Studio Community, for example.
    * [C# (Mandatory)](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
  * [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one): Extension for Markdown documents.
  * [CSharp to PlantUML](https://marketplace.visualstudio.com/items?itemName=pierre3.csharp-to-plantuml): Extension for converting Calss relationship in C# to PUML format. 
  * [PlantUML](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml): Extension for visualizing PUML format in an image.
  
### Additional Settings (Optional)
  * Turn on Word Wrap in the Setting if preferred.

## CSharp to PlantUML via Extension of VSCode
  * Usage: Ctrl+Shift+P to enable the vscode Command Palette and run the command "csharp2plantuml.classDiagram".

<!-- ## CSharp to PlantUML (Optional, in case you don't find the extension through Visual Studio Code)

  * [CSharp to PlantUML](https://github.com/pierre3/PlantUmlClassDiagramGenerator)
  * Install
  ```bash
  $ dotnet tool install --global PlantUmlClassDiagramGenerator
  ```
  * Use
  ```bash
  $ puml-gen ./Program.cs -public
  ```
  * Copy the text from *.puml and visualize using [PUML Viewer](https://www.planttext.com) -->

## PlantUML via Extension of VSCode
  * PlantUML is an Extension for viewing *.puml files.
  * Prerequisite maybe needed (check the documentation of the extension): 
    * Windows: [Java runtime](https://www.java.com/en/download/)
    * Mac OS: [Java runtime](https://www.java.com/en/download/) + graphviz 
  * Usage: Alt+D (Windows)/Option+D (Mac OS) to enable the preview function

## Markdown All in One via Extension
  * Usage: Ctrl K+V to preview the file

## First Console Program

```bash
// check the installed .Net in your system
$ dotnet --info 
// check the .Net Version
$ dotnet --version
// Use top-level statements
$ dotnet new console 
// Skip top-level statements and include Main()
$ dotnet new console --use-program-main 
```
or call .NET New Project in the [VSCode Command Palette](https://code.visualstudio.com/docs/getstarted/userinterface#:~:text=The%20most%20important%20key%20combination,for%20the%20most%20common%20operations.)

In Program.cs [Doc](https://aka.ms/new-console-template)
### Use top-level statements
```csharp
// See https://aka.ms/new-console-template for more information
Console.WriteLine("Hello, World!");
```

### Skip top-level statements and include Main
```csharp
namespace CRC_CSD_01;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
    }
}
```

In CRC_CSD-00.csproj [Doc](https://docs.microsoft.com/en-us/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file)
```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net8.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>
```

## How to Run the Program

```bash
// Build the program
$ dotnet build
// Run the program (Including Build, so you can skip the above command) 
$ dotnet run
```

## Namespace in C#
```csharp
using System;         // System namespace (defined by C#)
                      // The "using" Directive
// or
// using static System.Console;  // Console is a static system class
/*
A static class is basically the same as a non-static class,
but there is one difference: a static class cannot be instantiated.
*/

namespace CRC_CSD_01; // Application namespace (defined by the programmer)

class Program
{
    // static: shared method of all instances by the class
    // void: return "nothing" in the method
    // string[] args: parameters passed to the main function.
    // The parameters can be taken when lauching the application.
    static void Main(string[] args)  // Where the application begins
    {
        Console.WriteLine("Hello, World!");   // Console is a system class
    }
}
```

## Console Program with Accompanying Parameters
```csharp
using System;

namespace CRC_CSD_01;
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
```

## Introduction to Markdown language
  * [Markdown Official Website](https://daringfireball.net/projects/markdown/)
  * [Markdown Guide](https://www.markdownguide.org/)


## Example for a README.md file in your Project

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

## Comments, an Example
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
namespace CRC_CSD_01;
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
```

# Potential Errors

* Explorer in VSCode is missing. Ctrl+Shift+P to enable the vscode Command Palette and run the command "View: Reset View Locations".


---
# External Resources

* Additional IDEs
  * [Visual Studio 2022](https://visualstudio.microsoft.com/vs/)
    * [Install Visual Studio](https://learn.microsoft.com/en-us/visualstudio/install/install-visual-studio?view=vs-2022)
  * [JetBrain Ridar](https://www.jetbrains.com/rider/)
* [Microsoft Learn](https://learn.microsoft.com/en-us/)
* [Debugging in VS Code](https://code.visualstudio.com/docs/csharp/debugging)
* [Announcing the .NET MAUI extension for Visual Studio Code](https://devblogs.microsoft.com/visualstudio/announcing-the-dotnet-maui-extension-for-visual-studio-code/) 
* [Creating C4 and UML Diagrams Using PlantUML with VSCode Extension](https://medium.com/@robertdennyson/creating-c4-and-uml-diagrams-using-plantuml-with-vscode-extension-90032a21ec43)