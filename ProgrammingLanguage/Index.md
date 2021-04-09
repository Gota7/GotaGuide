# How To Make A Programming Language
Ever wanted to create your own programming language?

## Table Of Contents
* [Index](Index.md) <- You are here.
* [ANTLR4 Setup](AntlrSetup.md)
* [ANTLR4 Grammar](Grammar.md)
* [Creating Toylet](CreatingToylet.md)
* [Project Setup](ProjectSetup.md)
* [LLVM Crashcourse](Llvm.md)
* [Compiling Toylet](CompilingToylet.md)
* [Epilogue](Epilogue.md)

## Introduction
This is hard. Of course, it depends on how complex you want your language to be, but in general, this will not be a simple task and it may be confusing at some points. Hopefully with this guide, you'll have a better idea on how to implement your language. In this tutorial, I will be utilizing modern technologies such as ANTLR4 and LLVM at the time of writing, which luckily do a lot of the grunt work for us. So with that said, let's get started!

## Our Language
The first step of making a programming language is to first write some sample code to explore the syntax. In this tutorial, we will be implemented a toy language with no practical purpose called Toylet.

```cs
func doThing(number num, string str, decimal dec) {
    number a = num + 5 * 3 - 1;
    a += (number)dec;
    println(str + a);
}

doThing(3, "Result: ", 5.7);
```

As you can see, this contains a few concepts you would use in a language, such as data types, functions, operations, string concatting, and casting.

## Roadmap
As you probably guessed, making a language has a lot of different parts. Let's go over the main points of interest:
* Defining the syntax of our language. I'm telling you how Toylet works, so this step is done.
* Converting input into an Abstract Syntax Tree (AST). In order to *do* something with any program, we have to convert it into a form the compiler can work with, We call this the AST. We will be using ANTLR4 to handle this. We first must learn ANTLR4, then write a grammar for our language.
* Compile AST into code. Using the AST, we will make sure the code is valid while compiling. When we compile, we will convert the AST into LLVM IR, which is a very low-level, assembly like language. While you *could* learn LLVM IR syntax, all we need to know is how to work with it and emit it. Once in this form, LLVM can optimize our code, run it in a virtual machine using a bytecode, or compile it as a native binary for any platforms supported by LLVM!

## Next
Next thing to do is to set up ANTLR!
[ANTLR4 Setup](AntlrSetup.md)