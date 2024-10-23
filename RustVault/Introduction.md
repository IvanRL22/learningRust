---
tags:
  - introduction
---
# What is Rust?
A compiled programming language known for:
- Safety
- Concurrency
- Performance

Created by Grayden Hoare of the Mozilla Foundation when he wanted to avoid memory bugs in Firefox

It's fast and powerful
	#question What does powerful mean? How powerful exactly? 
Known for being a system language (like C, C++ or Go)
Usually more biased towards systems than for applications, but...
It's very interesting for backend development
Also for Blockchain, but who cares?
It has a powerful compiler, that catches most of the issues

Rust is *NOT* beginner friendly - But I'm not a beginner anyway
## Rust's killer features
### Memory handling
It doesn't have a garbage collector - PANIC! I've never programmed in a language that is not garbage collected
But it does check and remove things from memory when needed
	#question What does this mean exactly?
### Developer experience
Rust compiler is apparently mind-blowing
## Cargo
[Cargo](https://doc.rust-lang.org/cargo/guide/dependencies.html) is Rust's package manager
The central, public package registry is [crates.io](https://crates.io)

To create a new Rust project using cargo:
```sh
cargo new [project_name]
```
Naming convention uses snake_case
This creates a new folder with the name of the project and initializes a local Git repository

To initialize the project in the current folder:
```sh
cargo init
```
To build a cargo project:
```sh
cargo build
```
This builds a dev executable
	#question Is this the only build that can be debugged?
To build and compile at once:
```sh
cargo run
```
To check whether the code compiles:
```sh
cargo check
```
To compile a releasable build:
```sh
cargo build --release
```
