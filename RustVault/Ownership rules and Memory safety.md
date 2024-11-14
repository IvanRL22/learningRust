# Memory in Rust
In Rust there are two main memory regions: the stack and the heap
## Stack
Contains local variables
Also contains variables (pointers) that are references to objects in the heap
## Heap
Contains data that can outlive the lifetime of a function, in fact heap data can live indefinitely in the stack
We can use a construct called [Box](https://doc.rust-lang.org/std/boxed/index.html) to directly create data in the heap
Complex types like Vec, String and HashMap implicitly use boxes
# Memory management
Rust does **not** allow manual memory management
Instead, the compiler adds calls to free memory when the owner, and only when the owner of a variable goes out of scope
# Ownership
Ownership is described by three main rules:
1. Each value has a variable that is called its *owner*.
2. There can only be one owner at a time (enter Highlander joke here).
3. When the owner goes out of scope, the value will be dropped.