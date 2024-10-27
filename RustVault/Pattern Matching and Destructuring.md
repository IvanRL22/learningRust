A tool that allows destructuring data types.
The main structures for this are `match`and `if let`.
# match
Similar to a switch, in that it allows the comparison of a value against a series of patterns, then execute code depending on which pattern matches.
The compiler checks that all possible cases are handled.
```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```
Curly brackets can be used if a branch has more complex code:
```rust
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        }
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```
## Matching with Option\<T>
We can also use `match` with `Option` enumerations. #todo link Option note
```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
	match x {
		None => None,
		Some(i) => Some(i + 1),
	}
}

let five = Some(5);
let six = plus_one(five);
let none = plus_one(None);
```
## Matches are exhaustive
The arm's patterns must cover all possible values. Something like this won't compile:
```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
        match x {
            Some(i) => Some(i + 1),
        }
    }
```
We're missing the `None` case.
## Catch-all Patterns and the `_` Placeholder
You can use the keyword `other` to catch all remaining cases in an arm. This arm must always go last, otherwise any remaining arms would never run (the compiler will warn you about it).
For example, this is a game where rolling a 3 or a 7 has some special action, otherwise you move whatever you rolled:
```rust
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	other => move_player(other),
}

fn add_fancy_hat() {}
fn remove_fancy_hat() {}
fn move_player(num_spaces: u8) {}
```
Let's supposes we change the rules, and now you need to roll until you roll a 3 or a 7. Now you don't need to store the value you rolled when it's not a 3 or a 7, so you can use `_` to "skip" it:
```rust
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	_ => reroll(),
}

fn add_fancy_hat() {}
fn remove_fancy_hat() {}
fn reroll() {}
```
If we make a last change, so that nothing happens when you roll anything other than a 3 or a 7, we can use the unit value as the expression of that arm:
```rust
let dice_roll = 9;
match dice_roll {
	3 => add_fancy_hat(),
	7 => remove_fancy_hat(),
	_ => (),
}

fn add_fancy_hat() {}
fn remove_fancy_hat() {}
```
#todo go back [here](https://rust-book.cs.brown.edu/ch06-02-match.html) after learning about borrowing and check the last part
#todo go back [here](https://rust-book.cs.brown.edu/ch06-02-match.html) after learning about traits and complete the quiz
# if let
A way to handle values that match one pattern while ignoring the rest. It is useful when we want to do something only in once case. For instance, instead of this:
```rust
let config_max = Some(3u8);
match config_max {
	Some(max) => println!("The maximum is configured to be {max}"),
	_ => (),
}
```
We could write this:
```rust
let config_max = Some(3u8);
if let Some(max) = config_max {
	println!("The maximum is configured to be {max}");
}
```
Which is a bit more concise and readable.