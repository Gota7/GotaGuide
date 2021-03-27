# How To Make A Programming Language
Ever wanted to create your own programming language?

## Table Of Contents
* [Index](index.md)
* [ANTLR4 Setup](antlrSetup.md)
* [Creating The Grammar](grammar.md)

## Introduction
This is hard. Of course, it depends on how complex you want your language to be, but in general, this will not be a simple task and it may be confusing at some points. Hopefully with this guide, you'll have a better idea on how to implement your language. In this tutorial, I will be utilizing modern technologies such as ANTLR4 and LLVM at the time of writing, which luckily do a lot of the grunt work for us. So with that said, let's get started!

## Our Language
The first step of making a programming language is to first write some sample code to explore the syntax. In this tutorial, we will be implemented a toy language with no practical purpose called Toylet.

```
func doThing(number num, string str, decimal dec) {
    number a = num + 5 * 3 - 1;
    a += (number)dec;
    println(str + a);
}

doThing(3, "Result: ", 5.7);
```

As you can see, this contains a few concepts you would use in a language, such as data types, functions, operations, string concatting, and casting.

## Next
Next thing to do is to set up ANTLR!
[ANTLR4 Setup](antlrSetup.md)