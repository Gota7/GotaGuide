# Asylum Reference Manual
Making your programming a little less insane.

## Target Specific Code
Sometimes you have code that will only run on a certain target, be it due to different libraries, architecture specific code being called, etc. Luckily, Asylum gives you two main ways of defining platform specific code. This is done at compile time, and only the code for the target is included in the executable.

### Attributes
In Asylum, you can use attributes to declare functions, structs, and even namespaces as working only for a specific platform. This is the recommended way. Also notice that only one possible definition at most can exist for a given target. If a function is declared to have a target for windows, and another one of the same name and parameters has a target of x86_64, this leads to a multiple definition as it is possible for a system to be both windows and x86_64 at the same time.
```rust
[Target("windows")]
pub fn myFunction() {
    println("Hello Windows!");
}

[Target("armhf-linux")]
pub fn myFunction() {
    println("Hello ARM Linux!");
}

pub fn main() {
    myFunction(); // Automatically calls the one for the target, as it is determined at compile time. Notice that if we are on x86_64 linux, this would fail to compile as no function exists for that target.
}
```

### Constant Expressions
Another way to declare target specific stuff is within a function itself with constant expressions. This is useful if there are extra steps needed for only certain platforms or have certain platforms share code, but in most cases it is recommended to have different functions for different targets altogether using attributes as shown above. This code achieves the same thing:
```rust
pub fn main() {
    constexpr if (WINDOWS) {
        println("Hello Windows!");
    } else if (ARMHF && LINUX) {
        println("Hello ARM Linux!");
    }
}
```