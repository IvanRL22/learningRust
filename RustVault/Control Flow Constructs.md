Mostly the same as in Java, with a few notable exception and some minor syntax differences.

As a general note, condition expressions are always written without parenthesis, unlike Java.
# if
It can be used to assign the result to a value,
```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };
	// number will be 5
    println!("The value of number is: {number}");
}
```
Note that for this code to be valid both branches of the if statement need to return the same type.
# loop
Executes a block of code over and over forever or until it is explicitly told to stop, using the keyword `break`. Like the if expression, the result can be assigned to a variable.
```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2; // Ends the loop and returns `counter + 2`, which in this case would be 12
        }
    };

    println!("The result is {result}");
}
```
When you have nested loops, `break` and `continue` only apply to the innermost loop at that point. You can label loops to use the same keywords while specifying to which loop they apply.
```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```
#comment This looks wild and a pretty easy way to obfuscate code.

# Looping Through a Collection with for
You can obviously iterate through a collection using good ol' `while`. However, it is much more convenient and les error prone to use `for``:
```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}
```
You can also use `for` in a more general way with a `Range`, like so:
```rust
fn main() {
    for number in (1..4).rev() { // .rev() reverses the Range
        println!("{number}!");
    }
    println!("LIFTOFF!!!");
}
```
# if let
A way to handle values that match one pattern while ignoring the rest.
#tbd This will make more sense after learning pattern matching I guess