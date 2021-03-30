# Home
[Back To Table Of Contents](https://gota7.github.io/GotaGuide/)

# Intro To Assembly
Inside every computer is a processor, or CPU (Central Processor Unit). Some have more than others, some have more cores, threads, are faster/slower, etc. But there is always some way for a computer to execute instructions. These instructions control everything about a computer: they boot it up, allow you to use the mouse, are ran when running an application or game, etc.

## About Code/Programming
I'm sure most of you are familiar with the concept of either code, programming, or scripting. You write a text file (C, C#, Python, SQL, etc. are all text files) and a program/compiler will typically convert that to an EXE, ELF, or whatever kind of executable format your operating system uses. In some cases (such as Python), there is no compiler and the script can just run. But how are each of these different kind of programs ran?

### Compiled Code
The first type of code is compiled. This is when you create a program with C, C++, Rust, and many other languages. What this means is that your code is turned into raw instructions that the CPU can run. This typically is the fastest option, as there is no "conversion" from code to raw instructions.

### Bytecode
Different computers sometimes use different kinds of raw instructions. So instead of compiling code into raw instructions, we compile it into our own custom raw instructions which we have a wrapper program convert into raw instructions at runtime? This ensures that cross-compatibility across systems is independent on the compiler, and only depends on the wrapper that converts this custom "bytecode" we compile to into raw instructions. This is the method used by C#, VB, and Java. The speed can range from slightly slower to compiled code to faster in some cases.

### Interpreted Code
Some code does not need to be compiled and can just be ran. This is the case for Windows batch scripts, Linux bash scripts, and Python scripts. Instead of compiling, a high level interpreter (usually made in another language) runs the code in the script. Since another language is usually being ran, at some point the CPU is running raw instructions.

## The Point
At the end of the day, all code on the computer is boiled down into raw instructions for the processor to run. In the next tutorial, we will explore how simple operations like loading and storing numbers can control almost everything.

# Next
[First Assembly](FirstAssembly.md)