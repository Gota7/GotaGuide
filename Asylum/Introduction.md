# Asylum Reference Manual
Making your programming a little less insane.

## Table Of Contents
* [Introduction](Introduction.md) <- You are here.
* [Print Functions](PrintFunctions.md)

## Introduction
Hello, this is Gota7 from the Asylum Language Foundation, and today I will be showing you how to program in Asylum. A while back, I was thinking about programming languages such as C, C#, and Rust and thought that well they all have their pros and cons, there was not one that satisfied the low and high level demands I desire. I then thought to make my own programming language to combine them, then I laughed about how insane that idea was, as I believed I couldn't make a language. I then dubbed the theoretical language Asylum because of it.

### Hello World
The entry point of the function is called then `main` function. It is allowed to have parameters in the form `string[] args`; however, this is optional. There are two functions you can try calling: `print` and `println`. Both of them will output text onto the console. `print` prints without adding a new line, while `println` adds a newline after it prints what you tell it to.
```rust
pub fn main(string[] args) {
    println("Hello World!");
}
```
Can be shortened to this as command-line arguments are not used:
```rust
pub fn main() {
    println("Hello World!");
}
```
Output:
```
Hello World!
```

### Next
Do not worry too much about the `pub fn main() {}` thing at the moment, as that will be covered later. Just know for now that any code inside `pub fn main() {}` will be executed when your program runs. In the next section, we will be covering [Print Functions](PrintFunctions.md).