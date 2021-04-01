# Asylum Reference Manual
Making your programming a little less insane.

## Diet Pointers
What if you were to combine pointers in languages such as C and C++ with references found in languages such as C# and Java? The result is Diet Pointers, which act like references but can be used as pointers. Diet Pointers (DPs) allow you to refer to data without having to copy it over.

### Simple Use Case
Let's look at a simple example utilizing DPs:
```rust
u32 value = 3;
u32* dpToValue -> value; // This basically means that dpToValue points to value.
dpToValue += 4; // Same as dpToValue = dpToValue + 4.
println(value);
println(dpToValue);
```
Output:
```
7
7
```
When we declare the DP `dpToValue`, we are saying that it *points* to our existing value. So whenever we reference `dpToValue`, it gets treated as if we are working with `value`. This means that trying to get or assign the value of `dpToValue`, we instead are modifying `value`!

### Address Assign Operator (->)
You probably noticed that a DP is declared by using `->` instead of `=`. But why is that the case? Well, remember that when you use the name of a DP, it implies that you are setting the value the DP points to. So that means `u32* dp = value;` would not set the `dp` to point to the value, but would instead try and set what `dp` points to (which doesn't exist) to be `value`, which makes no sense! The address assign operator `->` instead implies that you are setting the address a DP points to is where the value on the right lies in memory.
```rust
u32 value1 = 3;
u32 value2 = 7;
u32* dp -> value1;
println(dp);
dp -> value2;
println(dp);
```
Output:
```
3
7
```
As you can see, we can set our DP to point to a new value with the address assign `->` operator.

### Address Of Operator (&)
What if you wanted to get the address of an item in memory? This can be done with the address of operator `&`. This is useful for declaring a DP without having to make a separate variable for it. Consider the following example:
```rust
pub fn printData(u32* valDP) {
    println(valDP);
}

pub fn main() {
    u32 val = 7;
    printData(&val);
}
```
Output:
```
7
```
Of course we could declare a new `u32*` DP variable to store a pointer to `val`, but having a shorthand to reference the value like this is a lot faster.

### Force Address Operator (@)
What if you wanted to treat a DP like a pointer though, and get or set it's address? The force address `@` operator allows you to do this:
```rust
u32 value1 = 3;
u32 value2 = 7;
u32* @dp = &value1; // Same thing as u32* dp -> value1;
println(dp);
println(@dp);
dp = 5;
println(dp);
println(@dp);
@dp = &value2; // Same thing as dp -> value2;
println(dp);
println(@dp);
u32* @otherDP = @dp; // Same thing as u32* otherDP -> dp.
println(@otherDP);
```
Output:
```
3
4050183500
5
4050183500
7
4050186048
4050186048
```
Notice how you are also allowed to assign what a DP points to using `@` as well. Also take into account that printing the address a DP refers to prints a location in memory rather than a value.

### The Dereference Operator (*)
There are rare cases where you may want to access a value, but it is behind multiple DPs. The dereference operator `*` allows you to get a value from this. Take into account the following example:
```rust
u32 val = 7;
u32* dp -> val;
u32** dp2 -> @dp; // This is how you make a DP point to a DP!
println(@dp2);
println(dp2);
println(*dp2);
*dp2 = 3;
println(dp);
```
Output:
```
4050186048
4050183500
7
3
```
Notice how `dp2` points to another DP, so therefore outputting its forced address or its value results in a memory address being printed. In order to access its value, you have to dereference it.

### Accessing Elements
Accessing elements or functions in a value pointed to by a DP is nothing special.
```rust
pub struct MyStruct {
    u32 data;
}

impl MyStruct {
    pub fn doThing() {}
}

pub fn main() {
    MyStruct dat;
    MyStruct* dp -> dat;
    MyStruct** dp2 -> @dp;
    dp.data = 3;
    dp.doThing();
    (*dp2).data = 7;
    (*dp2).doThing();
}
```
Notice that since `dp2` is a double DP, the dereference operator `*` has to be used.

### Null Data
When data is no longer in scope, the DP to the data will be set to `null`. Trying to access or set data to a null value will cause an exception. You are also allowed to set a DP to null.
```rust
u32* dat -> null;
u32* @dat2 = null;
if (dat == dat2) {
    println("Both are null!");
}
```
Output:
```
Both are null!
```

### Skipping The Middle Man
It's also possible to skip the middle man, and just have a DP refer to a newly created struct.
```rust
MyStruct* dat -> new MyStruct();
```
This is explained more when talking about how memory works in Asylum, but any item created this way is heap allocated and dies when the user frees it. Although you can allocate data on the stack like such:
```rust
MyStruct* data -> MyStruct();
```
This is the equivalent of making an instance of a struct, then a DP directly too it afterwards.

## Unsafe Diet Pointers
By default, Asylum does not let you do any of the traditional pointer math that C and C++ allow. However, you can make DPs act like fully fledged pointers in an unsafe context:
```rust
pub unsafe fn main() {
    u32 val = 7;
    u32* @end = &val + 1; // Pointer to a u32 after val.
    end = 3; // Sets data right after where val is in memory.
    println(@end); // Print data right after value.
    println(*(@end - 1)); // Print the data before where end is, which is val.
}
```
Output:
```
3
7
```
Of course this is only allowed in an unsafe context, and Asylum will not allow you to do pointer math otherwise.