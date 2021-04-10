# Asylum Reference Manual
Making your programming a little less insane.

## Loops
Loops are a great tool in any programming language, so of course Asylum has options in store for you to use. The 3 loops available are `loop`, `while`, and `for`. A segment can be broken out of using `break`, or `break C` where C is a constant to break out of multiple loops at once.

### Loop
A generic `loop` is the most basic kind of loop. It is the equivalent of your `while (true)` loop in C.
```rust
pub fn main() {
    u32 i = 0;
    loop {
        println("i is %d.", i);
        if (i == 3) {
            break; // Exit the loop if i is 3.
        }
        i++;
    }
    println("i is finished at %d.", i);
}
```
Output:
```
i is 0.
i is 1.
i is 2.
i is 3.
i is finished at 3.
```

#### Break Statements
There may be certain cases in which you wish to break out of multiple loops instantly. Below is an example of such, when you wish to have a case to exit the loop entirely when `i` and `j` are both 3.
```rust
pub fn main() {
    u32 i = 0;
    loop {
        u32 j = 0;
        println("i is %d.", i);
        loop {
            println("j is %d.", j);
            if (j == 3) {
                if (i == 3) {
                    break 2; // Exit both loops if i is also 3.
                } else {
                    break; // Exit only the current one.
                }
            }
            j++;
        }
        i++;
    }
}
```
Output:
```
i is 0.
j is 0.
j is 1.
j is 2.
j is 3.
i is 1.
j is 0.
j is 1.
j is 2.
j is 3.
i is 2.
j is 0.
j is 1.
j is 2.
j is 3.
i is 3.
j is 0.
j is 1.
j is 2.
j is 3.
```

### While
As you have probably noticed, it is annoying to have to use an if statement and a break to manually exit a loop. Luckily, `while` abstracts `loop` further by running only when a given condition is true. The given program will run until `i` is no longer greater than 3. Notice how we still have to increment `i` manually.
```rust
pub fn main() {
    u32 i = 0;
    while (i <= 3) {
        println("i is %d.", i);
        i++;
    }
    println("i is finally %d.", i);
}
```
Output:
```
i is 0.
i is 1.
i is 2.
i is 3.
i is finally 3.
```

### For
A `for` loop is the big guns when it comes to looping. It allows you to simplify the looping procedure greatly with some syntactic sugar, and there are multiple ways to utilize them in Asylum.

#### C Style
The most commonly known version of the `for` loop is the one in C, in which a variable declaration or assignment followed by a condition, followed by another variable assignment expression. All of these inputs are also optional. Here is a basic example that accomplishes the same thing our `while` loop does:
```rust
pub fn main() {
    for (u32 i = 0; i <= 3; i++) { // Variable declaration/assignment ; condition ; variable assignment makes the inside of a for loop, but any part is optional. The assignment and condition don't even have to be related to i, but usually are.
        println("i is %d.", i);
    }
    println("i is finally 3."); // We can not use the value of i anymore, as it only exists within the scope of the for loop.
}
```
Output:
```
i is 0.
i is 1.
i is 2.
i is 3.
i is finally 3.
```

##### Partial For Loop
We can use a `for` loop like a `while` loop in C style `for` loops as well, but it looks messy:
```rust
pub fn main() {
    u32 i;
    for (i = 0; i <= 3;) { // Notice how we don't have to include i++ in the for loop, and it is actually optional.
        println("i is %d.", i);
        i++; // But we do need to increment i in order to eventually escape the loop.
    }
    println("i is finally %d.", i); // We can do this again since i is outside the scope of the for loop.
}
```
Output:
```
i is 0.
i is 1.
i is 2.
i is 3.
i is finally 3.
```

#### Asylum/Rust Style
Asylum copies the Rust syntax in which a `for` loop can be a variable declared for each iteration in range of another value. Let us look at the following example:
```rust
pub fn main() {
    for (u32 i in 0..3) { // i is iterated through the range 0-3. Keep in mind the first value is inclusive, while the last one is exclusive.
        println("i is %d.", i);
    }
}
```
Output:
```
i is 0.
i is 1.
i is 2.
```

##### Inclusive Range
But like earlier, we wish `i` to iterate from `0` to `3` inclusively. While we could do this by going to `4` instead, there is special syntax to make the range inclusive.
```rust
pub fn main() {
    for (u32 i in 0..=3) {
        println("i is %d.", i);
    }
}
```
Output:
```
i is 0.
i is 1.
i is 2.
i is 3.
```

##### Iterators
Similar to C#'s `foreach` loops, `for` loops in Asylum can iterate over any variable of a type that implements ASL's (Asylum Standard Library) `IIterable<T>` interface. One such type available is `List<T>` within the `ASL.Collections` namespace.
```rust
using ASL.Collections;

pub fn main() {
    List<u32> vals = { 0, 3, 5, 7 };
    for (u32 x in vals) {
        println("x is %d.", x);
    }
}
```
Output:
```
x is 0.
x is 3.
x is 5.
x is 7.
```

###### DP Iterators
Sometimes you want to modify the value within the collection itself without copying it. This can be done using DP Iterators, which simply use a DP instead of the given type.
```rust
pub fn main() {
    List<u32> vals1 = { 0, 3, 5, 7 };
    List<u32> vals2 = { 0, 3, 5, 7 };
    for (u32 x in vals1) {
        x = 0;
    }
    for (u32* x in vals2) {
        x = 0;
    }
    for (u32 x in vals1) {
        println("x in vals1 is %d.", x);
    }
    for (u32 x in vals2) {
        println("x in vals2 is %d.", x);
    }
}
```
Output:
```
x in vals1 is 0.
x in vals1 is 3.
x in vals1 is 5.
x in vals1 is 7.
x in vals2 is 0.
x in vals2 is 0.
x in vals2 is 0.
x in vals2 is 0.
```