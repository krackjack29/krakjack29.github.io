---
title: Creating a Console Application using Visual Studio Code and DotNetCore
date: 2016/07/06 16:49:00
layout: single
categories: 
   - .net
tags:
   - .net
   - csharp
   - howto
   - unittest
---

Visual Studio code is a simple code editor from Microsoft which is multi-platform, light weight and supports multiple languages such as NodeJs, C#, Python etc. You can download the latest version [here](https://code.visualstudio.com/).

Microsoft recently announced the [.Net Core 1.0](https://dotnet.github.io/) (open source, cross platform version of Dotnet framework) available for download along with Visual Studio Update 3.  After installing .Net core you can use Visual Studio Code to develop .Net application, in this article we will see how to create a simple console application which takes two numbers and gives the sum back (Yeah hello world is too cliched )

Open up bash/command prompt and create a directory, initialize dot net core

```bash
mkdir samplecalculator
cd samplecalculator
dotnet new
dotnet restore
code .
```
The 3rd and the 4th command are to initialize the new dotnet framework in the samplecalulator folder. Last command would open Visual Studio code with Samplecalculator folder as working directory.

![step1](/assets/images/vsstep1.png)

there may or may not be a notification saying “Required assets to build and debug are missing from your project. Add Them”, if you find them click on Yes. It would create two files launch.json and tasks.json within .vscode folder.

Press F1 to install C# extension (from omnisharp) for visual studio code, without this you wont get the intellisense and the color coding

![step1](/assets/images/vsstep2.png)

![step1](/assets/images/vsstep3.png)

Open program.cs which contains the entry point for the application

```csharp
using System;
 
namespace ConsoleApplication
{
    public class Program
    {
        public static void Main(string[] args)
        {
            int a = int.Parse(Console.ReadLine());
            int b = int.Parse(Console.ReadLine());
 
            Console.WriteLine("Sum is {0}", a + b);
        }
    }
}
```

Press F1 and type “Tasks: Run Build Task” or just type Ctrl+Shift+B, it will build the console application.

![step1](/assets/images/vsstep4.png)

SampleCalculator.dll can be found in bin\Debug\netcoreapp1.0\, to execute the samplecalculator use the following command

```bash
dotnet bin\debug\netcoreapp1.0\samplecalculator.dll
```

Happy coding!!!