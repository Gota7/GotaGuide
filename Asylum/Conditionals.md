# Asylum Reference Manual
Making your programming a little less insane.

## Conditionals
There comes a point in every program where a condition must be checked. If it is rainy you get an umbrella, if it is cold you wear a jacket, else you may wear something warmer. The same such logic applies to programs with what is called an `if` statement.

### The Comparison Operators
Let us define a few basic comparison operators, which will return `true` if true, or `false` if false. Let us define truth tables with `0` representing `false`, and `1` representing `true`. `X` marks a value in which we do not care about. The table is read from top to bottom. Note that only `0`s and `1`s are used for simplicity.

#### Number / Decimals
Any number of any kind is `true` as long as it is not equal to `0`.

| Value | Result |
|-------|--------|
| 0 | 0 |
| X | 1 |

#### Strings
A string is regarded as `true`, as long as it is not empty `""`.

| Value | Result |
|-------|--------|
| "" | 0 |
| X | 1 |

#### Not (!)
If `a` is true, then `!a` produces false. If `a` is false, `!a` produces true.

| A | Result |
|-------|--------|
| 0 | 1 |
| 1 | 0 |

#### And (&&)
In `a && b`, both values must be true in order for this to be true.

| A | B | Result |
|---|---|--------|
| 0 | X | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

#### Or (||)
In `a || b`, either value can be true in order for this to be true.

| A | B | Result |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | X | 1 |

#### Equals (==)
In `a == b`, if `a` and `b` share the same value, this expression is true. Unlike some programming languages, this applies to strings as well.

| A | B | Result |
|---|---|--------|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

#### Not Equals (!=)
In `a != b`, this expression is true only if `a` and `b` don't share the same value.

| A | B | Result |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

#### Less Than (<)
In `a < b`, this expression is true only if `a` has a smaller value than `b`, or is first in alphabetical order for strings.

| A | B | Result |
|---|---|--------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | X | 0 |

#### Greater Than (>)
In `a > b`, this expression is true only if `a` has a greater value than `b`, or is later in alphabetical order for strings.

| A | B | Result |
|---|---|--------|
| 0 | X | 0 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

#### Less Than Or Equal To (<=)
In `a <= b`, this expression is true only if `a` has a smaller value or equal value compared to `b`, or is first in alphabetical order for strings.

| A | B | Result |
|---|---|--------|
| 0 | X | 1 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

#### Greater Than Or Equal To (>=)
In `a >= b`, this expression is true only if `a` has a greater value or equal value compared to `b`, or is later in alphabetical order for strings.

| A | B | Result |
|---|---|--------|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | X | 1 |

### If Statements
If statements allow us to apply these conditional operators in our code. Concept is fairly straightforward: if the condition is met, execute the code inside the block.
```rust
pub fn main() {
    u32 num = 3;
    if (num <= 7) {
        println("Number is less than or equal to 7!");
    }
}
```
Output:
```
Number is less than or equal to 7!
```

#### No Bodies
It is also possible for if statements to not have a body. In this case, the statement following directly after is executed if the statement is true. This rule applies for any statement that does not use a body.

```rust
pub fn main() {
    u32 num = 3;
    if (num > 7)
        println("Number is bigger than 7!");
    println("Number exists!");
}
```
Output:
```
Number exists!
```

### Else If Statements
Sometimes if the first condition does not execute correctly, you would like to test another condition only if the first one failed. This can be done with `else if` statements.

```rust
pub fn main() {
    u32 num = 3;
    if (num > 7)
        println("Number is bigger than 7!");
    else if (num < 5) {
        println("Number is less than 5!");
    }
}
```
Output:
```
Number is less than 5!
```

### Else Statements
But what if we want code to execute if none of the conditions are true? This is done with `else`.
```rust
pub fn main() {
    u32 num = 7;
    if (num > 7)
        println("Number is bigger than 7!");
    else if (num < 5) {
        println("Number is less than 5!");
    } else {
        println("Number is less than or equal to 7, or is greater than or equal to 5!");
    }
}
```
Output:
```
Number is less than or equal to 7, or is greater than or equal to 5!
```

### Constant Expression Ifs
In most cases, general `if` statements are fine. But there may be situations in which you want code to be compiled differently depending on certain factors (machine architecture, operating system, etc). This is managed with constant expression `constexpr` if statements.
```rust
pub fn main() {
    constexpr if (X86) {
        println("This system is 32-bit!");
    } else if (X64) {
        println("This system is 64-bit!");
    } else {
        println("System architecture is not x86_64!");
    }
}
```
Output depends on what target the system is being compiled for. Notice that the same thing can be accomplished with standard `if` statements, but this is typically preferred for target specific stuff, as it ensures no functions used that only exist for a certain target are trying to be referenced from another (will cause compile issues otherwise).

### Switch Case
A switch case statement is used to choose one of many possible outcomes. What separates an Asylum switch case statement from traditional ones is that you are allowed to give ranges. This applies to strings as well. A default case at the bottom is the catch all, if none of the above cases succeed.
```rust
pub fn main() {
    u32 num = 7;
    switch (num) {
        case 0:
        case 1: // Cases 0 or 1.
            println("I'm 0 or 1!");
            break;
        case 2..4: // Cases 2-4 with 4 not being inclusive.
            println("I'm 2 or 3!");
            break;
        case 5..=7: // Cases 5-7 inclusive.
            println("I'm 5, 6, or 7!");
            break;
        default:
            println("Unknown!");
            break;
    }
}
```
Output:
```
I'm 5, 6, or 7!
```