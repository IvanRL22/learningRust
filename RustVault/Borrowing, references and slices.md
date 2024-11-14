A technique in Rust that allows you to access the data of a particular value while the owner of the value retains control of it. There are two types of borrowing:
- Immutable: permits several read-only borrows, as the value does not change
- Mutable: only allows a single borrower at a time, which can change the value
# References and borrowing
A reference is a kind of pointer, also called a **non-owning pointer**: it does not own the data it references to.
A way to access the value of a referenced pointer is by using the dereference operator `*`:
```Rust
fn main() {
	let mut x: Box<i32> = Box::new(1);
	let a: i32 = *x;         // *x reads the heap value, so a = 1
	*x += 1;                 // *x on the left-side modifies the heap value,
	                         //     so x points to the value 2
	
	let r1: &Box<i32> = &x;  // r1 points to x on the stack
	let b: i32 = **r1;       // two dereferences get us to the heap value
	
	let r2: &i32 = &*x;      // r2 points to the heap value directly
	let c: i32 = *r2;    // so only one dereference is needed to read it
}
```
The dereference operator is not used very much, as the Rust compiler automatically inserts it in many places, such as when accessing methods using the dot (`.`) operator:
```Rust
let x: Box<i32> = Box::new(-1);
let x_abs1 = i32::abs(*x); // explicit dereference
let x_abs2 = x.abs();      // implicit dereference
assert_eq!(x_abs1, x_abs2);

let r: &Box<i32> = &x;
let r_abs1 = i32::abs(**r); // explicit dereference (twice)
let r_abs2 = r.abs();       // implicit dereference (twice)
assert_eq!(r_abs1, r_abs2);

let s = String::from("Hello");
let s_len1 = str::len(&s); // explicit reference
let s_len2 = s.len();      // implicit reference
assert_eq!(s_len1, s_len2);
```
# Avoiding simultaneous aliasing and mutation
Pointers are dangerous because they enable aliasing: accessing the same data through different variables.
This wouldn't be a problem if not for mutation. When we have two variables that point to the same data plenty of things could go wrong, such as:
- One variable deallocates the aliased data, leaving the other variable to point to deallocated memory.
- One variable mutates the aliased data, invalidating runtime properties expected by the other variable.
- One variable _concurrently_ mutates the aliased data, causing a data race with nondeterministic behavior for the other variable.
To avoid this and other issues, Rust uses the...
## Pointer Safety Principle
Data should never be aliased and mutated at the same time.
Data can be aliased. Data can be mutated. But data cannot be _both_ aliased _and_ mutated.
Due to references being non-owning pointers, a different set of rules from boxes are needed.
In order to enforce safety for this references Rust uses the borrow checker.
# Borrow checker
Variables can have three different permissions on their data:
- Read
- Write (only for mutable variables)
- Own
This permissions don't exist at runtime, they are only checked at compile time.
References can **temporarily** remove this permissions
There's a nuanced distinction to make here: there's a difference between a reference's permissions and the referenced data's permissions. This picture from the [Rust Book](https://rust-book.cs.brown.edu/ch04-02-references-and-borrowing.html#references-change-permissions-on-places) explains it well:
![[Pasted image 20241114185535.png]]
Specifically on the second line, the difference between `num` and `\*num` is that, since `num` has been declared constant, it cannot be written. If we were to declare such a reference as mutable, ut it would also have write privileges, like so:
![[Pasted image 20241114185951.png]]
Therefore we can understand that permissions are assigned not only to variables, but to places. A place is anything you may put on the left-hand side of an assignment.

#todo Continue here: https://rust-book.cs.brown.edu/ch04-02-references-and-borrowing.html#references-change-permissions-on-places