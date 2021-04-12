# Asylum Reference Manual
Making your programming a little less insane.

## Table Of Contents
* [Introduction](Introduction.md)
* [Print Functions](PrintFunctions.md) <- You are here.

## Print Functions
So far, we have went over our basic Hello World program, but now we will be exploring how to get different things to print on the console. To review, here is our program:
```rust
pub fn main() {
    println("Hello World!");
}
```
Output:
```
Hello World!
```

### Print Vs Println
The `print` function will print without adding a newline, while `println` will print with a newline after.
```rust
pub fn main() {
    print("Hello World!");
    print("Hello World Again!");
    println();
    println("Hello World!");
    println("Hello World Again!");
}
```
Output:
```
Hello World!Hello World Again!
Hello World!
Hello World Again!
```

### Strings Are Special
In Asylum, you can add strings with numbers, decimals, other strings, or any data type really.
```rust
pub fn main() {
    println("Hello #" + 7 + ", how may #" + 3.3 + " help you today?");
}
```
Output:
```
Hello #7, how may #3.3 help you today?
```

### Format Specifiers
TODO!!!