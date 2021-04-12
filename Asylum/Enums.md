# Asylum Reference Manual
Making your programming a little less insane.

## Enums
Enums (Enumerations) are used to represent states or possible outcomes (if person is running, walking, or sprinting, but there are many other examples). Enums in Asylum are similar to Rust, in which they can behave like traditional C enums, or contain data as well. Enums are almost like shortcuts for numbers. The first value starts at 0, with each value increasing in order by default, and each value must be unique: 
```rust
pub enum SomeState {
    Inactive,       // Is 0.
    Disabled = 2,   // Is 2.
    Active          // Is 3.
}

pub fn main() {
    SomeState state = SomeState.Active; // Set our state to active.
    state = (SomeState)3; // This does the same thing as the above statement.
    switch(state) {
        case Inactive:
            println("State is inactive.");
            break;
        case (SomeState)2: // Disabled state.
            println("State is disabled.");
            break;
        case Active:
            println("State is active.");
            break;
    }
}
```
Output:
```
State is active.
```

### Enum Bitwidth
Enum bitwidth specifies how many bits an enum can represent, with the default being 32 bits. This is done with the following syntax:
```rust
pub enum ToggleState : 1 {
    Off,
    On // Any more states after this would fail to compile.
}
```

### Enum Signed Values
All enums in Asylum are unsigned values, but negative numbers are automatically casted to their signed counterparts during compile time.
```rust
pub enum MyState {
    Invalid = -1, // Is 0xFFFFFFFF. 
    Initial, // Is 0.
    Another, // Is 1.
}
```

### Enums With Data
Enum values can even store data as well, as they do in Rust. These structures can either be named or un-named.
```rust
pub enum SomeState {
    Idle = 0,
    Active { u32 uptime, string property, u32 lifetime }, // An enum value that has named data.
    Dead(u32) 
}

pub fn main() {
    SomeState state = SomeState.Active() { uptime: 1000, property: "Purple" }; // Can assign values as appropriate. You can only write data during a construction and retrieve it with a lambda.
    SomeState state2 = SomeState.Dead(2000);
    switch (state) {
        case Idle:
            println("Idle.");
            break;
        case Active(uptime => uptime, property => property): // Use lambda expression to use values as needed in new variables, we don't need to use them all.
            println("Active with uptime %dms and property %s.", uptime, property);
            break;
        case Idle(time): // For tuples we must define variable names for all values.
            println("Has been dead for %dms.", time);
            break;
    }
}
```
Output:
```
Active with uptime 1000ms and property Purple.
```