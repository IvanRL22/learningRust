# Variables
By default, variables in Rust are immutable.
We can opt out of this by using the keyword ```mut```
```Rust
fn main() {
	let x = 5;      // x is immutable
	x = 6;          // This would cause a compiler error
    let mut y = 5;  // y is mutable
    y = 6;          // This is fine
}
```
# Constants
You might think a constant is just an immutable variable, but that's not quite right. There are some key differences:
- Variables are mutable by default but they can be made mutable, while constants cannot ever be made mutable.
- Constants can be declared in any scope, including the global scope.
- They are valid, within their scope, for the entire time the program runs.
- Constants must be assigned an expressions which can be calculated at compile time.
```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```
Naming convention, like in java, is all uppercase with underscores between words.
# Shadowing
Using the same variable name in a new declaration.
```rust
fn main() {
	// Create x with value 5
    let x = 5;
	// Redeclare x with value 6.
    let x = x + 1;

    {
	    // Inside this scope, now x is redeclared with a valur  of 12
	    // When this scope ends, x will revert to its outer value: 6
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}
```
When redeclaring a variable, we can actually change its type. This is the difference between shadowing and just using ```let mut```.
# Data types
Most data types in Rust are similar to Java and other languages. Here are some differences and relevant (for me) facts about Rust's data types.
## Overflows
Rust includes checks for integer overflows when compiling in debug mode. If it they fail, the program will panic at runtime.
However, it won't add this checks when compiling with the ```--release``` flag.
### Tuples
A way of grouping together several values with different types into one compound type. They have a fixed length: once declared, they cannot grow or shrink.
Tuples are created and accessed in the following manner:
```rust
fn main() {
	// Create a tuple with 3 values of different numeric types
    let t: (i32, f64, u8) = (500, 6.4, 1);

	// First value
	let five_hundred = x.0;
	// Second value
    let six_point_four = x.1;
	// Third value
    let one = x.2;
}
```
We can use pattern matching to destructure a tuple:
```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = t;

    println!("The value of y is: {y}");
}
```
## Arrays
Mostly the same as in Java. There are some interesting ways to declare them though:
```rust
let a = [3; 5];
```
