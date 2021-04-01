# Asylum Reference Manual
Making your programming a little less insane.

## Memory Model
Whenever you declare a variable without the `new` keyword, you are using variables allocated from the stack. This is adequate for most situations, but what happens when you wish to allocate variables on the heap, such that they can be accessed across functions and threads? This is where heap memory comes in, which is utilized whenever you use the `new` keyword.

## Simple Example
```rust
pub struct MyStruct {
    MyStruct* someData1;
    MyStruct* someData2;
}

pub fn addToStruct(MyStruct* data) {
    data.someData1 -> new MyStruct();
    data.someData2 -> MyStruct();
}

pub fn main() {
    MyStruct data;
    addToStruct(&data);
    if (data.someData1 != null) {
        println("Some data 1 is kept!");
        delete data.someData1;
    }
    if (data.someData2 != null) {
        println("Some data 2 is kept!");
        delete data.someData2;
    }
}
```
Output:
```
Some data 1 is kept!
```

## How It Works
Whenever you declare a DP, it actually has another hidden variable called a *scope ID*. 