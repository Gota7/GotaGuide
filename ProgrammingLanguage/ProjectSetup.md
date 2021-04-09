# How To Make A Programming Language
Ever wanted to create your own programming language?

## Table Of Contents
* [Index](Index.md)
* [ANTLR4 Setup](AntlrSetup.md)
* [ANTLR4 Grammar](Grammar.md)
* [Creating Toylet](CreatingToylet.md)
* [Project Setup](ProjectSetup.md) <- You are here.
* [LLVM Crashcourse](Llvm.md)
* [Compiling Toylet](CompilingToylet.md)
* [Epilogue](Epilogue.md)

## Required Tools
As you probably guessed, we are going to need a lot more than ANTLR4 to make a programming language. Now I will list out everything you need.

### Visual Studio Code
I'm sure most of you in the modern day have either heard of it, or are using it. If not, then you should probably upgrade from using dial up internet. Anyways, you can download it [here](https://code.visualstudio.com/Download).

### .NET (C#)
No, don't use .NET Framework if you have it (It didn't seem to work out for me with LLVM). I'm talking about the opensource .NET platform that is multi-platform, and I recommend you use it. You can get it [here](https://dotnet.microsoft.com/download). The reason why I am using C# for this project is that it is high-level (can abuse features C# offers), easier to understand for people of different programming backgrounds, and is a lot easier to setup than a good amount of other languages.

### LLVM
Now this is the most important part. This is what we will be using to actually *make* the language compile into machine code. LLVM is its whole thing and the next section will teach you all about it, but for now we just need to install it and make it work. For Linux, you can usually just install the LLVM package with `sudo apt-get install llvm` or something like that. Also make sure to install clang with `sudo apt-get install clang`. For Windows or anything else you just click on the link to the releases page in [here](https://releases.llvm.org/download.html), which leads to a Github page. From there, install Clang+LLVM, whatever you want and has LLVM. Just make sure you check the box to add LLVM to PATH if applicable. I'm using LLVM 11.0.1 for this tutorial.

### Reboot
If you are on Windows or maybe even MacOS, you should reboot your PC. Wouldn't hurt to do it with Linux either, just have to ensure those PATH variables are all correct.

## Making The C# Project
Make a new folder somewhere with the name you want your compiler to be. I'm just going to call my compiler Plunger for the sake of humor. Once you open a terminal inside of that folder, you want to run `dotnet new console`. Hopefully this should work, if not I'd be very concerned about how .NET was installed. Once you do that, open the folder using Visual Studio Code.

### Visual Studio Code Extensions
Visual Studio Code probably wants you to install some extensions at this point. You should install the C# extension, and also go ahead and install the NuGet Package Manager extension by jmrog.

### Sanity Test
Open `Program.cs` and under the `Run` tab, hit `Run Without Debugging`. You should be greeted with the classic hello world.

### Install NuGet Packages
Open the `Command Palette`, enter for it to add a NuGet package, then search for `LLVMSharp`. DO NOT USE A BETA VERSION. Use the latest stable, which for me is 5.0.0. You also want to do the same for `Antlr4.Runtime.Standard` to match your ANTLR version, which for me is `4.9.2`. If you did not get a prompt to restore packages, run `dotnet restore` in the terminal to restore these packages.

### Another Sanity Test
At the top of your `Program.cs` file, add the lines `using LLVMSharp;` and `using Antlr4.Runtime;`.
Under the `Run` tab, hit `Run Without Debugging`. You should be greeted with the classic hello world. This should hopefully mean that the packages were installed correctly for your project.

## Exporting Toylet Grammar To C#
So you know that grammar file we made for Toylet? Obviously it's not just going to sit around and do nothing, we need to use it! In the same folder of our `Toylet.g4` file, run the command `antlr4 -o bin -Dlanguage=CSharp -visitor Toylet.g4`. This should create a `bin` folder with a bunch of files in it. For your C# project, make a new folder and call it something like `Toylet`, and just copy everything from that `bin` folder into your new folder in the C# project. The new folder is to keep things organized, in the case that we need to make a change to our grammar. Now you are probably wondering what the `-visitor` flag does. Basically, this allows us to make use of a visitor interface that can visit each sub-section of the AST. You'll see what I mean very soon. You should also try running your project again to make sure it still works.

### Visitor Interface
Now what we will do is create a new file in the root of our project, and name it `Vistor.cs`. This file is very important. You may also want to copy the following code:
```cs
using Antlr4.Runtime;
using LLVMSharp;

/// <summary>
/// For storing information about visiting a node.
/// </summary>
public class VisitResult {
    // TODO: STUFF!!!
}

/// <summary>
/// Will visit segments of our code.
/// </summary>
public class Visitor : IToyletVisitor<VisitResult> {
}
```
You should notice that it is complaining about how the Visitor class does not implement a bunch of functions. Hit `Ctrl+.` on the red squiggles, hit `Implement Interface`, and it'll add a bunch of functions! Try running your project again to verify that everything is ok.

### What Just Happened
So far I have just been telling you to do things without really explaining, but it's hard to until this point. If you look at the names of the functions, a lot of them should look familiar. This is because these are all the Rules you defined in your grammar file, with a few new functions added. When we will tell our compiler to compile a file, it will first break it into an Abstract Syntax Tree using those C# files ANTLR generated for us. Why this is important, is because it will do things in order. Say we have a line of code such as `number one = 3 - 2 * 1;` We will first visit the variable declaration Rule, then visit the number expression Rule recursively. Eventually, we will get to the operation `2 * 1`. When visiting this node, our goal is to return the result of the operation (the VisitResult class defined above will store any data we need). Then we can return something for `3 - 2 * 1` since the value of `2 * 1` is returned, you get the point. Since when generating assembly-like code with LLVM, we need to make code for what needs to be done first first, this means we will be generating code in the order we should be!

## Next
Before we make our final compiler, we first have to learn LLVM, what it does, and how it works. So far we have learned how to read, but now it is time for us to learn how to write: [LLVM Crashcourse](Llvm.md)