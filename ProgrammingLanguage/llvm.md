# How To Make A Programming Language
Ever wanted to create your own programming language?

## Table Of Contents
* [Index](index.md)
* [ANTLR4 Setup](antlrSetup.md)
* [ANTLR4 Grammar](grammar.md)
* [Creating Toylet](creatingToylet.md)
* [Project Setup](projectSetup.md)
* [LLVM Crashcourse](llvm.md) <- You are here.
* [Compiling Toylet](compilingToylet.md)
* [Epilogue](epilogue.md)

## Introduction
Hello, this is Gota7, and today I'm going to teach you how to use the LLVM or *Low Level Virtual Machine* API. When working with the LLVM API, we are using a level language like C, C#, etc. to write a low level language called LLVM IR. If you want to see the kind of madness we will be generating, look [here](https://github.com/Virtual-Machine/ir-examples/tree/master/ll). Believe me, I don't understand the syntax either, no one should be subjected to learn it. The reason why is that this is because it is for *computers* to write, not humans. We instead get to write the code that writes that code.

## What Is LLVM
As discussed in the intro, LLVM stands for *Low Level Virtual Machine*. *Ok, but what does that mean?* It means that when we compile code from our custom language, we will be making it into another language called LLVM IR. But this is very powerful. From this form, it can be compiled to any platform LLVM supports, and LLVM has tons of optimizations it can apply to your code without you even planning for some of them. You get all this, for the price of nothing! It does not even have to compile it to native machine code, it could just run bitcode in the same way C# and Java are ran in a virtual machine.

## Some Rules
I hope you understand assembly, as this makes a lot more sense if you know it. But hopefully this overview makes sense. Note that my example code contains no true LLVM IR or building code for this rules section, but this is to illustrate the concepts in an easy to understand way.

### Types
LLVM is very strong with types, and allows you to use many existing or custom ones.

* It allows you to define structures with whatever other structs and data you wish.
* Pointers can be made, and can point to any data.
* Integers can be of any bitwidth, including being 1 bit!
* There is no such thing as strings. A string is just a pointer to a byte array that is null terminated.
* Structures can not contain functions. Code and data are completely different.

### Functions
Luckily you can define quite a bit about functions. Trust me, it's an amazing thing that functions are also abstracted like types, and you don't have to worry about using the correct registers manually.

* Functions can have any number of arguments, have arguments of any type, and have any return type. Variadic functions are also supported too (have an infinite number of parameters of any type).
* There is no such thing as namespaces, functions inside of structs, etc. You have been lied to. All those functions use a mangled name to represent their name depending on their compiler, and functions inside of a structure that are not static take a pointer to the struct as the first parameter. You can not have any function with a duplicate name, even if the arguments are different. What compilers typically do is mangle the parameters for the function to include in the name.
* You can declare `extern` functions, which promises that a function exists somewhere with the given arguments, as long as the appropriate library is linked.

### Control Flow
When inside a function, you have labels you can define and jump around to. Code is executed from top to bottom, unless you encounter an instruction telling you to go to a label. You would think you have if statements or loops or something? No, those don't exist in assembly. You've been lied to, for loops are fake! Ok, if they *don't* exist, how come I can use them? You can actually turn abstractions into top down code fairly easily.

#### If Statements
```cs
if (myNum == 3) {
    myNum += 5;
} else {
    myNum -= 7;
}
```
Can be turned into this:
```cs
goto notEqualTo3 if myNum != 3;
equals3:
myNum = myNum + 5;
notEqualTo3:
myNum = myNum - 7;
endIf:
```
Hey, you cheated, I see that `if`! But notice that the if makes a single comparison, and then has the program goto another location immediately if the condition is met. In assembly, you need some way to branch to a different place in code if a condition is or isn't met, so we are allowed to this in LLVM IR.

##### Blocks
Notice how we used named labels such as `equals3`? This is what is called a block in LLVM, which basically contains a bunch of code instructions. When you reach the end of a block, you fall into the next block. You can have the program jump to blocks dependent on a condition, or not. When we define code for a function, we will always use what we call the `entry` block at the beginning.

#### Loops
What do you mean loops, don't you mean `for`, `while`, and `loop`? No, I don't. You've been lied to again. They are all the same thing!

##### Generic While True Loop
```cs
while(true) {
    if (condition == true) {
        break;
    }
}
```
Is actually:
```cs
loop:
goto loop if condition != true;
```
Fairly simple, right?

##### While Loops
Same basic idea as a generic loop, just different.
```cs
do {
    number++;
} while (number < 7);
```
```cs
loop:
number = number + 1;
goto loop if number < 7;
```

##### For Loops
They don't do anything special, they are just while loops with more stuff before!
```cs
for (number x = 0; x < 7; x++) {
    // Code.
}
```
Turns into:
```cs
number x = 0;
loop:
x = x + 1;
goto loop if x < 7;
```

### Memory
In LLVM IR, everything is either one of infinitely available virtual registers (think of it as any amount of variables that can store any type of data), or memory allocated from the stack (which you can load and store to). There is no such thing as heap allocation! Those are done by the operating system through library calls like `malloc` and such. So in order to use heap memory, you have to declare `malloc` or the proper function as an `extern` function, and make sure to have the static library linked.

### Variables
Variables in LLVM IR are immutable. Once you declare a variable (all variables use virtual registers) and assign it a value, you can't assign it another value again! But, how can we can do simple code like this:
```cs
number x = 3;
x = 7;
x += 5;
```

#### Global Variables
Global variables have to be initialized with a constant value, but can be changed by loading and storing at the address they are at (like stack variables).

#### Another Register Approach
The first obvious approach is to use separate registers/variables.
```cs
number x = 3;
number x2 = 7;
number x3 = x + 5;
```
This is not only very ugly, but confusing too! We also have to keep track of the latest name!

#### Stack Variable Approach
The best way to solve this problem is to play by the rules, but still kind of cheat. We can allocate stack memory with something called `alloca`. This can allocate a variable from the stack of any type, and returns a pointer to it. I'm going to assume you know how pointers work, which use loading and storing.
```cs
number* x = alloca(number);
*x = 3;
*x = 7;
*x = *x + 5;
```
As you can see, we only used one variable, but yet was able to read and write to it at will!

##### The Catch
What is cool is that LLVM is smart enough to optimize these stack allocations into actual physical registers on your computer when compiling. The bad news is that there is a catch to this of course. Remember the `entry` block at the beginning of every defined function? In order for this optimization to work, we must use `alloca` only within this `entry` block. But then how do we know to allocate memory in the beginning while in something like an if statement? This actually will not be a problem, as we can write to any block at any time when inside the function, and we don't have to give values for the allocated memory, we could just say they should exist.

### What On Earth Are You Talking About?
What, none of this makes sense? It may seem that this has nothing to do with writing code in LLVM, but that couldn't be further from the truth. This section actually taught you how LLVM IR code generation works without showing you a single line of it (or at least the underlining ideas)! Once we get into some actual builder code, you may find it helpful to refer back here for strategies.

## Enough With The Rules, Let's See Some Code!
TODO!!!