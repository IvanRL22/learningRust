Different elements used in Rust to build the structure of the program.
# Enums
Also called enumerations. They are a custom data type that's defined by enumerating (roll credits!) all of its possible variants. A good, simple example could be an enum for the rocks paper scissors game: 
```Rust
num RpsChoice { Rock, Paper, Scissors }
```
## Enums characteristics
- An instance of an enum can one and only one of the enum's variants.
- In Rust, variants are not restricted to be the same data type. #lovethis
	- Also, each variant may hold additional embedded data. This data can also be of different types for each variant.
- We can define methods on enums.
## Option
A special kind of enum used to handle cases where a value could be something or nothing, similarly to Java's Optional.
#todo add reference to Option when we get there in more detail
# Structs
Goold ol' structs. They are a data type used to group related values into one entity. Kinda like and object, but not quite the same.
```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```
This defines the structure of a struct (heh). To use it properly, we need to create an instance of it- To access it's fields we can use do notation (again, this is all like an object in Java):
```Rust
fn main() {
    let mut user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };
    
	user1.email = String::from("anotheremail@example.com");
}
```
Note that if the struct is mutable, all of its fields will be mutable as well.
## Field Init Shortand
To avoid too much verbosity when we have "simple" fields, we can use the *field init shorthand* syntax to create an instance without having to write the field names a gazillion times: #lovethis
```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```
## Struct Update Syntax
An easy way to "clone" structs while changing some of the data. This would be the usual, manual way to create a new struct from an existing one:
```Rust
fn main() {
	let user1 = User {
	  email: String::from("someone@example.com"),
	  username: String::from("someusername123"),
	  active: true,
	  sign_in_count: 1,
	};

	let user2 = User {
		active: user1.active,
		username: user1.username,
		email: String::from("another@example.com"),
		sign_in_count: user1.sign_in_count,
	};
}
```
An this is the shorthand way of doing this copy:
```rust
fn main() {
    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```
Using the syntax `..`specifies that the remaining fields not explicitly set should have the same value as the fields in the instance `user1`. This syntax must be the last one when assigning values to a new struct. Note that due to ownership rules, we cannot use `user1` after creating `user2` due to the String `username`.
#todo Come back here after learning about ownership. I don't quite understand this.
# Tuple structs
Structs that do not define names for their fields, just the types. They are useful when:
- You want to give a name to a tuple
- Naming every field of a struct would be too verbose or redundant
```Rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```
Here black and origin are different types, due to being instances of different tuple structs.
# Unit-Like structs
Structs with no fields. They are called *unit-like* because they are similar to to the unit type. They are useful for implementing a trait on some type but you don't need them to store any data in the type itself. 
```Rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```
#todo Come back here after learning about traits.
## Borrowing fields of a Struct
#todo Come back after learning about borrowing
# Traits
Define behaviors that are shared among different data types (like interfaces?). Traits are abstract and thus, impossible to instantiate. However we can define pointers of trait types that can hold any data type that implements the trait.
## Defining a trait
The following example declares a trait `Summary`with the following characteristics:
- It uses `pub`to make it accessible to crates that depend on this crate.
- It declares a method signature for `summarize` that has a self reference and returns a String.
```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```
## Implementing a Trait
Here is an example of two different structs implementing the previously declared trait:
```rust
pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```
Traits can only be implemented if the trait, type or both are local to our crate.
## Default implementations
You can define the implementation of a trait's method in the trait declaration. In this way, implementing types are not required to implement that method and can defer to this default implementation:
```rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}
```
## Traits as parameters
Much like in Java, traits can be used as parameter types:
```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```
## Trait bounds
Much like in Java, traits can be used to bound generics:
```rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```
You can also specify multiple bounds:
```rust
pub fn notify(item: &(impl Summary + Display)) {
```
Notably, when bounding generics with a lot of traits, there is a way to avoid making the signature hard to read using the `where` clause:
```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{
```
## Returning types that implement traits
Again, much like in Java, you can specify a trait as a return type, and return a struct or enum that implements that type:
```rust
fn returns_summarizable() -> impl Summary {
    Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    }
}
```
There is a particular restriction to this: the function can only return one type. Even if you want to return different types that implement the correct trait (let's say, depending on a condition you return either type), the compiler won't let you do this.
## Conditionally implement methods according to trait bounds
We can implement different methods on the same generic if we specify different bounds for it. In this example, we implement `new` for any `Pair\<T>`, but we also implement `cmp_display` for when `T` also implements `Display`and `PartialOrd`:
```rust
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```
## Blanket implementations
We can implement a trait for any type that implements another trait. For instance, the standard library implements `ToString`on any type that also implements `Display`:
```rust
impl<T: Display> ToString for T {
    // --snip--
}
```
#todo go back to this and reread to make sure I understood it.
# Impl blocks
We've already seen the use of `impl` to implement traits on structs or enums. We've also seen it used to bound generics. But `impl` can also be used to implement behavior in the form of methods. Remember that methods are functions associated to a struct, enum or, as we've already seen, a trait. For instance, here we define a struct and implement a method area:
```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```
Everything inside the curly braces after we declare the impl block will be associated with the `Rectangle` type.
`&self` is be a reference to the instance of the struct of type `Rectangle` that calls the method. This is short for `self: &Self`. Within an `impl`block, `Self` is an alias for the type of the impl block.
These methods behave like the methods in a Java object. In rust, these are also called *associated functions*.
There are also associated functions that are not methods: they do not have a `Self`reference as their first parameter, bevause they don't need an instance of the type to work with (like static methods in Java).
## Methods and Ownership
#todo Go back [here](https://rust-book.cs.brown.edu/ch05-03-method-syntax.html#methods-and-ownership) and read again after learning about ownership and borrowing