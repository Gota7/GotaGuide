# Asylum Reference Manual
Making your programming a little less insane.

## Memory Model
Memory management in Asylum follows a few rules:
* All memory is either allocated on the stack, on the heap (heap uses a system library call), or static (only one instance of it).
* Stack memory dies when it is out of scope. All DPs (Diet Pointers) to it are nulled. Stack memory consists of all things constructed without the `new` keyword.
* Heap memory dies when the owner of the data is out of scope. All DPs (Diet Pointers) to it are nulled. Heap memory consists of all things constructed with the `new` keyword.
* DPs owning heap memory are allowed to transfer ownership to another DP, allowing data to live longer than it normally would. Only one DP is allowed to be considered the owner.
* A special case is that any return value can not be considered going out of scope.
* Unsafe pointers do no memory abstraction. Any heap memory allocated to them must be manually freed with the `delete` keyword. Unsafe pointers do not automatically null either.
* It is possible for an unsafe pointer to point to the same data a normal DP does, but an unsafe pointer can not take ownership away from a DP. A DP can point to the same data an unsafe pointer does, but can not take ownership.

### Heap Allocation VS Stack Allocation
Whenever you declare a variable without the `new` keyword, you are using variables allocated from the stack. This is adequate for most situations, but what happens when you wish to allocate variables on the heap, such that they can be accessed across functions and threads? This is where heap memory comes in, which is utilized whenever you use the `new` keyword.

#### Simple Example
```rust
pub struct MyStruct {
    pub MyStruct* someData1;
    pub MyStruct* someData2;
}

pub fn addToStruct(MyStruct* data) {
    data.someData1 -> new MyStruct(); // Data is declared on the heap, data.someData1 is given ownership.
    data.someData2 -> MyStruct(); // Data is declared on the stack. Since neither MyStruct() or the data containing it are returned, it immediately dies.
}

pub fn main() {
    MyStruct data;
    addToStruct(&data);
    if (data.someData1 != null) {
        println("Some data 1 is kept!");
    }
    if (data.someData2 != null) {
        println("Some data 2 is kept!");
    }
    // data.someData1 goes out of scope here. Since it owns the MyStruct() instance from earlier, it gets deleted.
}
```
Output:
```
Some data 1 is kept!
```

### Return Values
As stated, return values are a special case:
```rust
pub struct MyStruct {
    pub MyStruct* someData;
}

pub fn returnStack() -> MyStruct* {
    MyStruct* dp -> MyStruct();
    dp // Even though the DP itself is not out of scope, the data it's pointing to is!
}

pub fn returnStack2() -> MyStruct {
    MyStruct() // The stack memory itself is being returned, so it can not be going out of scope.
}

pub fn returnHeap() -> MyStruct* {
    MyStruct* dp -> new MyStruct();
    dp // Owner to data is being returned, so it can not be considered going out of scope.
}

pub fn modifyStackVar(MyStruct* data) -> MyStruct* {
    data.someData -> MyStruct();
    data // Despite data being returned, the data it points to is going out of scope.
}

pub fn modifyHeapVar(MyStruct* data) -> MyStruct* {
    data.someData -> new MyStruct();
    data // The owner to the data does not die here, regardless of the struct being returned or not since it is a DP parameter. This return value is not the same as the parameter, and is not the owner.
}

pub fn main() {
    MyStruct* s1 = returnStack();
    MyStruct* s2 -> returnHeap();
    if (s1 != null) {
        println("Struct 1 exists.");
    }
    if (s2 != null) {
        println("Struct 2 exists.");
    }
    s1 -> returnStack2();
    if (s1 != null) {
        println("Struct 1 exists this time.");
        modifyStackVar(@s1); // Even though a new pointer is returned here an immediately deleted, since it is not the owner (s1 is), the data is not removed.
        if (s1.someData != null) {
            println("Stack allocated property exists.");
        }
        modifyHeapVar(@s1); // Same story with the call above.
        if (s1.someData != null) {
            println("Heap allocated property exists.");
        }
        s1 -> modifyHeapVar(@s1); // The return value does not have ownership, yet s1 must get a new value. This means the data is deleted.
        if (s1 != null) {
            println("Struct 1 still exists.");
        }
    }
}
```
Output:
```
Struct 2 exists.
Struct 1 exists this time.
Heap allocated property exists.
```

### Transferring Ownership
Whenever you use the address assign operator `->`, you are transferring ownership if the value on the right has ownership to begin with.
```rust
pub struct MyStruct {
    pub MyStruct* someData;
}

pub fn transferOwnership(MyStruct** toReceive) { // We must pass a DP to a DP, otherwised the value would be passed by value.
    MyStruct* dp -> new MyStruct();
    *toReceive -> dp; // Ownership is transferred.
}

pub fn dontTransferOwnsership(MyStruct** toReceive) {
    MyStruct* dp -> new MyStruct();
    toReceive = @dp;
}

pub fn main() {
    MyStruct* s1 -> new MyStruct();
    s1 -> s1; // The value on the left is giving up ownership in order to receive data, so data is deleted. This means the value it gets assigned to is null.
    if (s != null) {
        println("Struct 1 is alive.");
    }
    s1 -> new MyStruct();
    @s1 = @s1; // The value on the left owns data, so it must give it up in order to receive a new value, meaning the data was deleted.
    if (s != null) {
        println("Struct 1 is still alive.");
    }
    MyStruct* s2;
    transferOwnership(&@s2);
    if (s2 != null) {
        println("Struct 2 is the owner.");
    }
    dontTransferOwnership(&@s2); // In this function call, the data assigned earlier is deleted.
    if (s2 != null) {
        println("Struct 2 is the owner again.");
    }
}
```
Output:
```
Struct 2 is the owner.
```

### Static Variables
Static variables only exist as one instance across classes or functions.
```rust
pub struct MyStruct {
    pub static u32 counter = 0;
}

pub fn count() {
    static u32 count = 5;
    println("Count is %d.", count++);
}

pub fn main() {
    count();
    count();
    count();
    MyStruct s1;
    MyStruct s2;
    s1.counter++;
    s2.counter += 2;
    println("Counter is %d.", s1.counter);
}
```
Output:
```
Count is 5.
Count is 6.
Count is 7.
Counter is 3.
```

### Unsafe Pointers
Unsafe pointers do no custom memory management. You have to clean up memory you allocate to them when you are done. Unsafe pointers can point to the same data regular DPs do, but can not own data. Similarly, DPs can point to the same data unsafe pointers do, but can not take ownership. Ownership can not be transferred with DPs and unsafe pointers.
```rust
pub struct MyStruct {
    pub MyStruct* someData;
}

pub unsafe fn main() {
    unsafe MyStruct* s -> new MyStruct();
    MyStruct* dp -> s; // This will try and take ownership of s, but will not since s is unsafe.
    delete s; // Memory must be automatically deleted when handling unsafe pointers.
}