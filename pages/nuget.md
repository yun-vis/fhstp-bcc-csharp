---
# permalink: /about/
layout: single
title: ".Net NAUI"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"
last_modified_at: 2025-02-04
---

# [An Introduction to NuGet](https://learn.microsoft.com/en-us/nuget/what-is-nuget)
An essential tool for any modern development platform is a mechanism through which developers can create, share, and consume useful code. Often such code is bundled into "packages" that contain compiled code (as DLLs) along with other content needed in the projects that consume these packages.

For .NET (including .NET Core), the Microsoft-supported mechanism for sharing code is NuGet, which defines how packages for .NET are created, hosted, and consumed, and provides the tools for each of those roles.

More packges to install? Please visit [https://www.nuget.org/](https://www.nuget.org/)

```bash
// Check installed NuGet version
$ dotnet nuget --version
```
# Install [SkiaSharp](https://www.nuget.org/packages/SkiaSharp/) and [SkiaSharp.Views.Maui.Controls](https://www.nuget.org/packages/SkiaSharp.Views.Maui.Controls/3.118.0-preview.2.3)
SkiaSharp is a cross-platform 2D graphics API for .NET platforms based on Google's Skia Graphics Library. 
```bash
$ dotnet add package SkiaSharp.Views.Maui.Controls --version 3.118.0-preview.2.3
$ dotnet add package SkiaSharp --version 3.116.1
$ dotnet add package SkiaSharp.Views.Forms --version 3.116.1
```

# Install [QuickGraph](https://www.nuget.org/packages/QuikGraph)
```bash
// core
$ dotnet add package QuikGraph --version 2.5.0
// file IO
$ dotnet add package QuikGraph.Serialization --version 2.5.0
// visualization
$ dotnet add package QuikGraph.Graphviz --version 2.5.0
```
# Install [Gurobi Optimizer](https://www.nuget.org/packages/Gurobi.Optimizer)
```bash
$ dotnet add package Gurobi.Optimizer --version 12.0.1
```

# Remove a NuGet Package
```bash
$ dotnet list package
$ dotnet remove package Package_Name
```

# Solution
What is a solution?

## Project Management

## Create a new solution
```bash
$ dotnet new solution
```

## Add project to a Solution

```bash
$ dotnet sln add YourProject.csproj
```
