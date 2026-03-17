<!-- markdown-toc start -->

# Table Of Contents

- [Rust](#rust)
- [The Rust Ecosystem](#the-rust-ecosystem)
- [Rust Information](#rust-information)
    - [An example of print formatting](#an-example-of-print-formatting)
        - [Format specifiers in prinln! macro](#format-specifiers-in-prinln-macro)
- [Datatypes](#datatypes)
    - [An example](#an-example)
        - [`Arrays` have to follow the below rules:](#arrays-have-to-follow-the-below-rules)
    - [Copy types](#copy-types)
    - [Structs](#structs)
        - [Is struct copy or move?](#is-struct-copy-or-move)
    - [enums](#enums)
- [Modules](#modules)
- [Memory Management](#memory-management)
- [Associated functions and Methods](#associated-functions-and-methods)
- [Ownership, Borrowing & Lifetimes](#ownership-borrowing--lifetimes)
    - [Ownership](#ownership)
        - [Ownership rules](#ownership-rules)
        - [Variables and Data Interacting with Move](#variables-and-data-interacting-with-move)
        - [Variables and Data Interacting with Clone](#variables-and-data-interacting-with-clone)
        - [Ownership and Functions](#ownership-and-functions)
        - [Return Values and Scope](#return-values-and-scope)
        - [Note](#note)
    - [Borrowing](#borrowing)
        - [Borrowing rules](#borrowing-rules)
            - [Violations](#violations)
        - [Some pointers about Move](#some-pointers-about-move)
        - [Some pointers about Copy](#some-pointers-about-copy)
    - [Lifetimes](#lifetimes)
        - [Another example for lifetimes](#another-example-for-lifetimes)
        - [Lifetime elision](#lifetime-elision)
    - [Note on using Function arguments](#note-on-using-function-arguments)
    - [Compound Datatypes](#compound-datatypes)
        - [Option enum](#option-enum)
        - [Result for Error Handling](#result-for-error-handling)
            - [The try-operator `?`](#the-try-operator-)
- [Strings in Rust](#strings-in-rust)
    - [String vs &str in Rust functions](#string-vs-str-in-rust-functions)
        - [Types of &str](#types-of-str)
    - [Shallow Copy & Deep Copy](#shallow-copy--deep-copy)
- [Conditional statements](#conditional-statements)
    - [Iterator functions](#iterator-functions)
- [Some examples](#some-examples)
    - [ref keyword](#ref-keyword)
- [Some Examples](#some-examples)
    - [Pattern matching for tuple](#pattern-matching-for-tuple)
    - [An error scenario](#an-error-scenario)
- [Inferior Mutability](#inferior-mutability)
- [Cow](#cow)
    - [Usage of Cow](#usage-of-cow)
- [Anonymous functions or Closures in Rust](#anonymous-functions-or-closures-in-rust)
    - [Capturing variables in closures](#capturing-variables-in-closures)
    - [Understanding the closure traits](#understanding-the-closure-traits)
        - [Case: Trait `FnOnce` - Moves Captured Variables](#case-trait-fnonce---moves-captured-variables)
        - [Case: Trait `FnMut` - Mutates Captured Variables](#case-trait-fnmut---mutates-captured-variables)
        - [Case: Trait `Fn` - Only Reads Captured Variables](#case-trait-fn---only-reads-captured-variables)
    - [Trait hierarchy](#trait-hierarchy)
    - [Closures as Arguments](#closures-as-arguments)
    - [Passing closures to structs](#passing-closures-to-structs)
    - [Additional pointers on closures](#additional-pointers-on-closures)
- [Smart Pointers](#smart-pointers)
- [Testing](#testing)
- [MACROS](#macros)
    - [Declarative Macros with macro_rules!](#declarative-macros-with-macro_rules)
    - [Repetitions](#repetitions)
    - [Procedural Macros](#procedural-macros)
- [Appendix](#appendix)
    - [Version checks](#version-checks)
    - [View the installed and active toolchains](#view-the-installed-and-active-toolchains)
    - [To switch between the toolchains](#to-switch-between-the-toolchains)
    - [Checking and Updating the toolchain](#checking-and-updating-the-toolchain)
    - [Accessing help and documentation](#accessing-help-and-documentation)
- [Resources](#resources)
- [References](#references)

<!-- markdown-toc end -->
# Rust
`Rust` is a general purpose programming language with emphasis on reliability, performance, type safety, concurrency and memory safety. Few of the features which makes it stand along with languages like `C` and `C++` are:
+ Memory Safety
+ Type Safety
+ Modern Tooling
+ Fast Performance
+ Extreme Concurrency

A few domains where `rust` can excel are:
+ Systems Programming
+ Embedded Systems
+ Cubersecurity
+ Finance & High Frequency Trading
+ Mission Critical Systems

The core motto of `rust` is that, it is impossible to write memory unsafe code. It does not have the concept of `null pointer` and uses an intelligent borrow and scope model for dynamic deallocation of unused objects and memory automatically without any intervention from the programmer.

# The Rust Ecosystem
The Rust ecosystem consists of many tools, of which the main ones are:

- `rustc`: the Rust compiler which turns `.rs` files into `binaries` and other intermediate formats.
- `cargo`: the Rust dependency manager and build tool.
- `rustup`: the Rust toolchain installer and updater. It is used to install and update `rustc` and `cargo` when new versions of Rust are released.

Here are the current versions:
```bash
λ rustc --version
rustc 1.92.0 (ded5c06cf 2025-12-08)

λ cargo --version
cargo 1.92.0 (344c4567c 2025-10-21)
```

# Rust Information

In `Rust` variables are also called as `bindings`.
`Rust` provides type safety via static typing and the variable bindings are declared using the `let` keyword as shown here:

```rust
fn main() {
    let q: i32 = 10;
    println!("q: {q}");
}
```

The `!` at the end of the `println` indicates that it is a *macro* and not a *function*. `Macros` are a part of the Rust standard library.

In `Rust` `\u` is used to specify a unicode code point. The number within the braces `{221E}` is the hexadecimal representation of the unicode code point like the one here `∞`
```rust
/// Unicode character handling
let c = '©';
println!("{:X}", c as i32);
// This prints => A9

let cp = '\u{00A9}';
println!("{}", cp);
// This prints => ©
```

Handling `unicode` characters:
```rust
// playing with unicode
let ka_in_telugu = '\u{C15}';
let ka_in_kannada = '\u{C95}';
let ka_in_hindi = '\u{915}';
let ka_in_chinese = '\u{5f00}';

println!("{}\n{}\n{}\n{}", ka_in_telugu, ka_in_kannada, ka_in_hindi, ka_in_chinese);
```

This will produce below output
```bash
 క
 ಕ
 क
 开
```

Handling `characters`:

```rust
let z = (65..91).collect::<Vec<u8>>().iter().map(|x| *x as char).collect::<Vec<char>>();
println!("{:#?}", z);
```

This way of providing the type in `collect()` using `::<>` explicitly is called `TurboFish` syntax where type is specified directly with the `collect()` method.
The above will print the below character vector:

```bash
[
     'A',
     'B',
     'C',
     'D',
     'E',
     'F',
     'G',
     'H',
     'I',
     'J',
     'K',
     'L',
     'M',
     'N',
     'O',
     'P',
     'Q',
     'R',
     'S',
     'T',
     'U',
     'V',
     'W',
     'X',
     'Y',
     'Z',
]
```

Another example for `TurboFish` syntax using a lazy collection is:
```rust
(1..).take(10).into_iter().map(|x| x.to_string()).collect::<Vec<String>>()

// ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10"]
```

For printing multiple times:
```rust
println!("{:+^11}", "\u{C15}");

// This will print
//+++++క+++++
```
## An example of print formatting
Here is an example for formatting the output during printing:
```rust
fn main() {
    let title = "LORD SRIRAMA";
    println!("{:-^30}", title);
    let bar = "|";
    println!("{: <15}{: >15}", bar, bar);
    let a = "ANJANEYA";
    let b = "HANUMAN";
    println!("{name1:-<15}{name2:->15}", name1 = a, name2 = b);
}
```

The above will print:
```sh
---------LORD SRIRAMA---------
|                            |
ANJANEYA---------------HANUMAN
```

### Format specifiers in prinln! macro
In the previous example, we used `format` specification within the `printlon!` macro and they are defined as below:

```rust
// Default alignment: Left Aligns the argument within the specified width
println!("{}")

// Right aligns the argument within the specified width
println!("{:>width}")

// Left aligns the argument within the specified width
println!("{:width}")

// Center aligns the argument within the specified width
println!("{:^width}")

// Center aligns the argument within the specified width with "="
// as the filling character
println!("{:=^width}")
```

+ Examples here:
```bash
>> println!("{}", "sampath".to_string());
sampath
>> println!("{:>15}", "sampath".to_string());
        sampath
>> println!("{:15}", "sampath".to_string());
sampath
>> println!("{:^15}", "sampath".to_string());
    sampath
>> println!("{:=^15}", "sampath".to_string());
====sampath====
```

# Datatypes
Data types are a fundamental programming concept that determines the kind of data that can be stored and manipulated within a program. `Rust` is a statically typed language and hence, the compiler must know what are the types of each variable during compile time. This feature enables the `Rust` compiler to check and enforce type safety, reducing many kinds of errors and ensuring that operations on variables are valid as per their types.

`Rust` has two main categories of types:

`Scalar` types and `Compound` or `Aggregate` types. `Scalar` types represent a single value, and `Compound` types  group multiple values into one type representing a collection of values.

The following are the primitive data types (built-in data types) available in `Rust`.

1. `Booleans`: `bool`, which can have the values **true** or **false**. It is `8` bits wide.
2. `Integers`: `i8, i16, i32, i64, i128, u8, u16, u32, u64, u128` which are _signed_ and _unsigned_ integers of different sizes along with `isize` and `usize`. The `isize` and `usize` are the width of a pointer.
3. `Floating-point` numbers: `f32` and `f64`, which are _single-precision_ and _double-precision_ floating-point numbers, respectively.
4. `Characters`: `char`, which represents a single Unicode character. It is `32` bits wide.
5. `Strings`: `&str` and `String`, which represent a sequence of Unicode characters.
6. `Arrays`: `[T; N]`, which represent a fixed-size array of `N` elements each of type `T`.
7. `Slices`: `[T] and &[T]`, which represent a variable-size sequence of elements of type `T`.
8. `Tuples`: `(T1, T2, ..., Tn)`, which represent a fixed-size sequence of elements of different types.
9. `Unit type`: `()`, which represents an empty tuple and is used when no value is needed.

> [!ATTENTION]
> *`isize` and `usize` types*:
> `isize` and `usize` are platform dependent with their size being `4` bytes on `32-bit`
> platforms and `8` bytes on `64-bit` platforms.

In the above data types, items `1` through `5` represent `Scalar` data types, while
`6` through `8` represent `Compound` or `Aggregate` data types.

The `Unit Type ()` type has exactly one value `()`, and is used when there is no other meaningful value that could be returned. `()` is most commonly seen implicitly:
functions without `a -> ...` implicitly have return type as `()`, these are equivalent:

```rust
fn long() -> () {}

// above and below are same

fn short() {}
```

If a semicolon `;` is used at the end, it discard(s) the result of the expression at the end of a block, making the expression (and thus the block) evaluate to `()`. For example,

```rust
fn returns_i64() -> i64 {
    1i64
}

/// this returns a Unit type as semicolon is present
fn returns_unit() {
    1i64;
}

let is_i64 = {
    returns_i64()
};

/// the value has a Unit type
let is_unit = {
    returns_i64();
};
```

## An example
Write a Rust program which takes the time of the day in terms of seconds
as integers and converts that into `24-hour` time format `(HH:MM:SS)`.

```rust
// Expected Output:
//
// Enter the time of the day in seconds(0 to 86,399):
//
// 81000
//
// Time in 24hr format is: 22:30:00
use std::io;

fn main() {
    let mut input = String::new();
    println!("Enter the time of the day in seconds(0 to 86,399):");
    io::stdin()
        .read_line(&mut input)
        .expect("Failed to read line");
    let time: u32 = input.trim().parse().expect("Numeric value only");
    input.clear();

    if time <= 86900 {
        let h = time / 3600;
        let ms = time % 3600;
        let (m, s) = div_rem(ms, 60);
        println!("Time in 24hr format is: {:0>2}:{:0>2}:{:0>2}", h, m, s);
    } else {
        println!("Input error");
    }
}

fn div_rem(a: u32, b: u32) -> (u32, u32) {
    (a / b, a % b)
}
```

### `Arrays` have to follow the below rules:

> [!TLDR]
> **Array rules:**
> Array holds fixed number of elements and all the elements should be of same data type.
> Once declared, Arrays cannot change their size and size should be known at compile time.


## Copy types
One of the simplest types in `Rust` are the `Copy` types. They live on the `stack` and compiler is aware of the size. These types are easy to copy, so the compiler always copies their data when we send such types to any function. Also `Copy` types are very small and can be easy to get copied.

`Ownership` does not affect such types. Types which are not of `Copy` type are always **moved** like the `String`, `Vec` and other `Complex` data types. These complex data types allocate memory on the Heap.

> `Copy` types include `integers`, `floats`, `booleans`, `char` and others.
> `Move` types include complex data types like `String` and 'Vec'.

By checking the documentation for the specific type we can find whether the type _implements_ `Copy Trait` or not.

For example checking the documentation for [bool][1] the left side shows various `Trait Implementations` from which we can check that `Copy` is implemented indicating that it can be copied.


## Structs
`Structs` are user defined `Aggregate` data types that allow us to organise and encapsulate related pieces of data consisting of heterogeneous data types into a single unit. A `struct` is a type that groups data together. `Structs` contain member fields — variables of (almost) any type. Grouping data together makes it easier to access complicated information once you’re looking in the right `struct`, individual pieces of data are available by name.


There are three main types of `structs` available in `Rust`:
1. Tuple struct
2. Named struct
3. Unit struct or Zero sized struct

Here is how we can create each type of the struct:
```rust
// 1. A Tuple struct
// Instantiate a tuple struct
struct Point(i32, i32);

fn main() {
    let p = Point(17, 23);
    // Access the fields of a tuple struct
    println!("({}, {})", p.0, p.1);
}

// 2. A Named struct with two fields
struct Point {
    x: f32,
    y: f32,
}

fn main() {
    // Instantiate a `Point`
    let point: Point = Point { x: 5.2, y: 0.4 };
    // Access the fields of the point
    println!("point coordinates: ({}, {})", point.x, point.y);
}

// 3. A Unit type structure
struct Unit;

// Instantiate a unit struct
let _unit = Unit;
```

### Is struct copy or move?
- Whether a `struct` is *Copy* or *Move* depends on the types of its member fields
- When we assign a `struct` variable to another, it is a `move` by default because `structs` do not implement the `Copy` trait.

```rust
#[derive(Debug)]
struct Point {
    x: i32,
    y: i32
}

fn main() {
    let p1 = Point {x: 1, y: 2};
    let p2 = p1; // Here, p1 us moved to p2
    println!("p1: {:#?}, p2: {:#?}", p1, p2);
}
```
The code throws below error:
```bash
8  |     let p1 = Point {x: 1, y: 2};
   |         -- move occurs because `p1` has type `Point`, which does not implement the `Copy` trait
9  |     let p2 = p1; // Here, p1 us moved to p2
   |              -- value moved here
10 |     println!("p1: {:#?}, p2: {:#?}", p1, p2);
   |                                      ^^ value borrowed here after move
```

However, changing the code as below will work:
```rust
// Clone is a supertrait of Copy, so everything which is
// Copy must also implement Clone
#[derive(Debug,Copy,Clone)]
struct Point {
    x: i32,
    y: i32
}

fn main() {
    let p1 = Point {x: 1, y: 2};
    let p2 = p1;            // Here, p1 is copied to p2
    let p3 = p1.clone();    // Here, p1 is cloned to p3
    println!("p1: {:?}, p2: {:?}, p3: {:?}", p1, p2, p3); // Works OK now
}
```

The `rust` documentation at the link [Copy Trait](https://doc.rust-lang.org/std/marker/trait.Copy.html "Copy Trait") has versatile information describing about `Copy`.

The `Default` trait helps to initialize the values of `Struct` with it's default values as shown below:
```rust
#![allow(dead_code)]
#![allow(unused_variables)]

// To get default values of a `struct` we can use the `Default` impl as below:
#[derive(Debug, Default)]
struct Process {
    name: String,
    pid: u32,
    is_system_process: bool,
    lifetime: f32,
    prefix: char,
}

fn main() {
    let p1 = Process::default();
    let p2 = Process {
        name: String::from("Ping"),
        pid: 0x1234,
        ..Default::default()
    };
    let p3 = Process {
        name: String::from("Monitor"),
        pid: 0x4567,
        is_system_process: true,
        lifetime: 121.23,
        prefix: 's',
    };
    let p4 = Process {
        name: String::from("Route"),
        ..p3
    };
    println!("{:#X?}", p1);
    println!("{:#X?}", p2);
    println!("{:#X?}", p3);
    println!("{:#X?}", p4);
}
```

Output:
```bash
Process {
    name: "",
    pid: 0x0,
    is_system_process: false,
    lifetime: 0.0,
    prefix: '\0',
}
Process {
    name: "Ping",
    pid: 0x1234,
    is_system_process: false,
    lifetime: 0.0,
    prefix: '\0',
}
Process {
    name: "Monitor",
    pid: 0x4567,
    is_system_process: true,
    lifetime: 121.23,
    prefix: 's',
}
Process {
    name: "Route",
    pid: 0x4567,
    is_system_process: true,
    lifetime: 121.23,
    prefix: 's',
}
```

## enums
`enum` can be considered as a jewel in the crown in `Rust's` type system. The `enum` allows you to specify a set of mutually exclusive values, possibly with a numeric value attached.

```rust
enum HttpResultCode {
    Ok = 200,
    NotFound = 404,
    Teapot = 418,
}

let code = HttpResultCode::NotFound;
assert_eq!(code as i32, 404);
```

`Structs` are useful when we want _many things_ together, while `Enums` are useful when we want to have _many possible choices_.


# Modules
`Functions`, `Structs`, `Enums` etc., must have the `pub` keyword as a prefix to qualify them as visible from outside the modules.

Format of a project with individual module files:

```bash
# individual modules
content/
    media.rs
    catalog.rs

# module definitions
content/
    mod.rs

# main module
main.rs
```

```rust
// media module
// content/media.rs
pub media { /* fields */ }
/* Media impl */

// catalog module
// content/catalog.rs
pub catalog { /* fields */}
/* Catalog impl */

// content/mod.rs
pub mod media;
pub mod catalog;

// root module
// main.rs
mod content;

fn main() {
    let catalog = content::catalog::Catalog::new();
}
```

Here, every separate file and folder makes it's own module and the format shown is appropriate for large projects, but has some confusing moving parts.

# Memory Management
Memory safety is the property of a program where memory pointers used always point to valid memory, i.e. allocated and of the correct type/size. Memory safety is a correctness issue; a memory unsafe program may crash or produce nondeterministic output depending on the bug.

In practice, this means that there are languages that allow us to write “memory unsafe” code, in the sense that it’s fairly easy to introduce bugs. Some of those bugs are:

+ Dangling pointers: Pointers that point to invalid data (this will make more sense once we look at how data is stored in memory).
+ Double frees: Trying to free the same memory location twice, which can lead to “undefined behaviour”.

To illustrate the concept of a dangling pointer, let’s take a look at the following `C++` code and how it is represented in memory:

```cpp
std::string s = "Have a nice day";
```

The initialized string is usually represented in memory using the `stack` and `heap` as below:

<details>
<summary>String representation in Stack & Heap</summary>

```text
             pointer
               |  length
               |   |   capacity
               |   |    |
               |   |    |
            +–––+––––+––––+
stack frame │ • │ 15 │ 16 │ <– s
            +–│–+––––+––––+
              │
            [–│––––––––––––––––––––––––– capacity ––––––––––––––––––––––––––]
              │
            +–V–+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+
       heap │ H │ a │ v │ e │   │ a │   │ n │ i │ c │ e │   │ d │ a │ y │   │
            +–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+

            [––––––––––––––––––––––––– length ––––––––––––––––––––––––––]
```
</details>

What gets stored on the stack is the `std::string` object itself which is of a three words long, fixed size.
The fields are:

- A `pointer` to the heap-allocated buffer which holds the actual data,
- The buffers `capacity` and
- The `length` of the text.

In other words, the `std::string` owns its buffer. When the program destroys this string, it’ll free the corresponding buffer as well through the string’s destructor.

However, it’s totally possible to create other pointer objects to a character living inside that same buffer which won’t get destroyed as automatically, leaving them invalid after the string has been destroyed, and there we have it - a `dangling pointer!`. Such issues will not occur in `Garbage Collected` languages like `go, python, javascript` etc.

>`Rust` does not come with *garbage collection*, instead, it solves the issue of guaranteeing memory safety using `ownership` and `borrowing`. When we say that `Rust` comes with memory safety, we refer to the fact that, by default, `Rust’s` compiler doesn’t even allow us to write code that is not memory safe.


The three different parts of the computer memory are:

> [!NOTE]
> **Computer Memory**
> `Stack`:
>       Fast, but limited in size (2 to 8MB)
>       In `Rust`, any data of fixed size (or known size at compile time), such
>       as machine integers, floating-point numeric types, pointer types and a
>       few others, are stored on the `stack`.
> `Heap`:
>       Slow, but can grow to accommodate more data. It stores the actual data.
>       Dynamic and "unsized" data is stored on the `heap`.
> `Data Segment` or `ROData` or `Static Segment`:
>       Stores literal values, which we write in our code. This avoids running
>       out of memory in the stack if the data structure grows to hold lot of data.
>       ex:
> ```rust
> let val = 51;
> let item = "sword";
> let nums = vec![1, 2, 3, 4, 5];
> ```

A `Stack` usually stores metadata about a data structure. If we consider the `nums` vector declared in the example above, the way a `Stack` stores the same will be as below:

*Vec Struct* metadata
| Item              | Details                       |
|-------------------|-------------------------------|
| Pointer to Values | Actual values are on Heap     |
| Length            | length or number of elements  |
| Capacity          | How long the Vector can grow  |

If the data structure owns another data structure, then the child's metadata will be placed in the `Heap`.
Ex:

```rust
let outer_vector = vec![
    vec![1, 2, 3, 4, 5];
];
```

In this case, metadata of `outer_vector` will be on `Stack` while that of the inner vector will be on `Heap`.

# Associated functions and Methods

All functions defined within an `impl` block are called `associated functions` because they’re associated with the type named after the `impl`. The `associated functions` are `static` methods associated with that `module`, `struct` or an `enum`. We can define `associated functions` that don’t have `self` as their first parameter (and thus are not methods) because they don’t need an instance of the type to work with.

Since `associated function` is a `static method` we do not have to create an instance first and can be directly called on the `type` itself.

Hence, the below part for creating a new `String` is valid:
```rust
let my_string = String::new();
```

In the code below, `new` is an `Associated` function for `struct`.

```rust
// Point struct
struct Point {
    x: f64,
    y: f64,
}

// Implementation block, all `Point` associated functions & methods defined here.
impl Point {
    // This is an "associated function" because this function is associated with
    // a particular type, that is, Point.
    //
    // Associated functions don't need to be called with an instance.
    // These functions are generally used like constructors.
    fn origin() -> Point {
        Point { x: 0.0, y: 0.0 }
    }

    // Another associated function, that takes two arguments:
    fn new(x: f64, y: f64) -> Point {
        Point { x: x, y: y }
    }
}

// Rectangle struct
struct Rectangle {
    p1: Point,
    p2: Point,
}

// Implementation block, all `Rectangle` associated functions & methods defined here.
impl Rectangle {
    // This is an Instance Method invoked with a dot operator
    // `&self` is sugar for `self: &Self`, where `Self` is the type of the
    // caller object. In this case `Self` = `Rectangle`
    fn area(&self) -> f64 {
        // `self` gives access to the struct fields via dot operator.
        let Point { x: x1, y: y1 } = self.p1;
        let Point { x: x2, y: y2 } = self.p2;

        // `abs` is an `f64` method that returns absolute value of the caller
        ((x1 - x2) * (y1 - y2)).abs()
    }

    fn perimeter(&self) -> f64 {
        let Point { x: x1, y: y1 } = self.p1;
        let Point { x: x2, y: y2 } = self.p2;

        2.0 * ((x1 - x2).abs() + (y1 - y2).abs())
    }

    // This method requires the caller object to be mutable
    // `&mut self` desugars to `self: &mut Self`
    fn translate(&mut self, x: f64, y: f64) {
        self.p1.x += x;
        self.p2.x += x;

        self.p1.y += y;
        self.p2.y += y;
    }
}

// `Pair` owns resources: two heap allocated integers.
struct Pair(Box<i32>, Box<i32>);

impl Pair {
    // It takes ownership of the struct
    // This method "consumes" the resources of the caller object
    // `self` desugars to `self: Self`
    fn destroy(self) {
        // Destructure `self`
        let Pair(first, second) = self;

        println!("Destroying Pair({}, {})", first, second);

        // `first` and `second` go out of scope and get freed.
    }
}

fn main() {
    let rectangle = Rectangle {
        // Associated functions are called using double colons
        p1: Point::origin(),
        p2: Point::new(3.0, 4.0),
    };

    // Methods are called using the dot operator.
    // Note that the first argument `&self` is implicitly passed, i.e.
    // `rectangle.perimeter()` === `Rectangle::perimeter(&rectangle)`
    println!("Rectangle perimeter = {}", rectangle.perimeter());
    println!("Rectangle area = {}", rectangle.area());

    let mut square = Rectangle {
		// Again, associated functions are called using double colons
        p1: Point::origin(),
        p2: Point::new(1.0, 1.0),
    };

    // Error! `rectangle` is immutable, but this method needs a mutable object.
    // rectangle.translate(1.0, 0.0);
    // TODO ^ Try uncommenting this line

    // Okay! Mutable objects can call mutable methods
    square.translate(1.0, 1.0);

    let pair = Pair(Box::new(1), Box::new(2));

    pair.destroy();

    // Error! Previous `destroy` call "consumed" `pair`
    // pair.destroy();
    // TODO ^ Try uncommenting this line
}
```

Output from the above code is:
```bash
Rectangle perimeter = 14
Rectangle area = 12
Destroying Pair(1, 2)
```

`Methods` are similar to functions, but they have parameters and a return value. Unlike `associated functions`, `methods` are defined within the context of a `struct` (or an `enum` or a `trait` object), and their _first_ parameter is always `self`, which represents the instance of the struct the method is being called on.
Hence, they can only work on an instance of a `struct`.

> [!IMPORTANT]
> `self` and `Self` are 2 important items to be distinguished:
> `self`: Represents the instance of the `struct` on which the method is being called.
> `Self`: Represents the `type` of the `struct` for which the methods or associated functions have been implemented.


Here are the main points which distinguish between *Methods* and *Associated functions*:


| Methods                                   | Associated functions                     |
|:------------------------------------------|:-----------------------------------------|
| Methods are  associated with  a specific  | `Associated` functions  are not  tied to |
| instance of the `struct` or `enum`        | any particular  instance and  are called |
|                                           | on the `type` itself                     |
| Methods  are defined  within the  `impl`  | `Associated`   functions   are   defined |
| block  for the  `struct`  or `enum`  and  | within  tje `impl`  block  as well,  but |
| their  first   parameter  is   always  a  | their first parameter is not a reference |
| reference to the instance of `struct` or  | to the `type`                            |
| `enum`                                    |                                          |
| `struct` methods  are accessed  using an  | `Associated`   functions  are   accessed |
| instance  of  `struct`   followed  by  a  | using  the `struct`  name folled  by the |
| dot(.) operator.                          | *double colon* (`::`) operator           |
| For  instance,  if  we have  a  `struct`  | For    instance,    if   we    have    a |
| *TrafficLight*  `struct`  named  `light`  | `struct`  named  `TrafficLight` with  an |
| with  a method  named `get_state()`,  we  | `Associated`   function  `new`   we  can |
| can access it using `light.get_state()`   | access it using `light::new()`           |


# Ownership, Borrowing & Lifetimes
These are the *3* closely connected concepts in `Rust` and almost **90%** of difficulty associated with`Rust` language is attributed to these. `Rust` wants to minimise unexpected updates to the data which will eliminate majority of bugs.

The `Rust` compiler enforces these concepts and corresponding rules, ensuring that the code is safe from __data race__ conditions and other memory related bugs. When these rules are followed, we will be able to write safer and more efficient code that takes advantage of `Rust's` ownership system.

> [!NOTE] All of Rust's primitive/scalar types are listed below
> `int`,
> `bool`,
> `float`,
> `char`,
> `string literal` etc., are all stored in the `Stack memory`.

> [!NOTE] All of `Rust's` complex types:
> `String`,
> `Box` etc., are all stored into `Heap memory`.


## Ownership
Understanding `Onwership` gives a solid foundation for understanding the features that make `Rust` unique. `Ownership` is a set of rules that govern how a `Rust` program manages memory. Every language manages the computer's memory in a different way. Some use garbage collection which looks for unused memory regularly, while in some others, the programmer should take responsibility of explicitly allocating/deallocating the memory. `Rust` uses a third approach, where memory is managed through a system of `ownership` with a set of rules that the compiler checks.

Keeping track of what parts of code are using what data on the `heap`, minimizing the amount of duplicate data on the `heap`, and cleaning up unused data on the `heap` so you don't run out of space are all problems that `ownership` addresses. Once these are understand, we don't have to think about `stack` and `heap` very often, but knowing that the main purpose of `ownership` is to manage `heap` data can help explain why it works the way it does.

The goal of `Ownership` is to limit the ways you can reference and update the data. This limitation will reduce the number of bugs and will also make code more understandable.

### Ownership rules
A few rules governing the `ownership` system are:

> 1. Every value is *owned* by a single variable, struct, vector, etc at a time.
> 2. There can be only one owner at a time
> 3. Once the owner goes out of scope, the value will be dropped. Which means that
> reassigning the value to another variable,  passing it to a function, putting it
> into a vector etc., moves the value. The old variable cannot be used anymore.

In `Rust` the memory is automatically returned once the variable that owns it goes out of scope. When a variable goes out of scope `Rust` calls a special method `drop()`, that is implemented on the specific type. This function is responsible for cleaning up memory for that type.

An example:

```rust
fn greet() {
  let s = "Hello!".to_string();
  println!("{}", s);    // `s` is dropped here
}
```

A longer example showing the stringent rules enforced by `Rust` for controlling
the `ownership` and `move`:
```rust
#[derive(Debug)]
struct CubeSat {
    id: u64,
}

#[derive(Debug)]
enum StatusMessage {
    Ok,
}

fn check_status(sat_id: CubeSat) -> StatusMessage {
    StatusMessage::Ok
}

fn main() {
    // Ownership starts here at the creation of CubeSat object
    let sat_a = CubeSat { id: 0 };
    let sat_b = CubeSat { id: 1 };
    let sat_c = CubeSat { id: 2 };

    // Here, Ownership of the object moves to check_status()
    // but it is not returned to main()
    let a_status = check_status(sat_a); // sat_a is dropped here
    let b_status = check_status(sat_b); // sat_b is dropped here
    let c_status = check_status(sat_c); // sat_c is dropped here
    println!("a: {:?}, b: {:?}, c: {:?}", a_status, b_status, c_status);

    // Now, sat_a is no longer the owner of the object,
    // making access invalid...
    let a_status = check_status(sat_a);
    let b_status = check_status(sat_b);
    let c_status = check_status(sat_c);
    println!("a: {:?}, b: {:?}, c: {:?}", a_status, b_status, c_status);
}
```

The example above will fail with errors:

<details>
    <summary>Error stack due to Ownership & Borrowing</summary>

```shell
error[E0382]: use of moved value: `sat_a`
  --> src/main.rs:30:33
   |
17 |     let sat_a = CubeSat { id: 0 };
   |         ----- move occurs because `sat_a` has type `CubeSat`, which does not implement the `Copy` trait
...
23 |     let a_status = check_status(sat_a);
   |                                 ----- value moved here
...
30 |     let a_status = check_status(sat_a);
   |                                 ^^^^^ value used here after move
   |
note: consider changing this parameter type in function `check_status` to borrow instead if owning the value isn't necessary
  --> src/main.rs:11:25
   |
11 | fn check_status(sat_id: CubeSat) -> StatusMessage {
   |    ------------         ^^^^^^^ this parameter takes ownership of the value
   |    |
   |    in this function
note: if `CubeSat` implemented `Clone`, you could clone the value
  --> src/main.rs:2:1
   |
2  | struct CubeSat {
   | ^^^^^^^^^^^^^^ consider implementing `Clone` for this type
...
23 |     let a_status = check_status(sat_a);
   |                                 ----- you could clone this value
```
</details>


`Rust` will drop the value of `s` when it reaches the end of the function block. The same applies when we deal with more complex data structures. Consider the following code:

```rust
let vehicles = vec!["Cycle".to_string(), "Car".to_string()];
```

This creates a `vector` of `vehicles`. A `vector` in `Rust` is like an `array`, or `list`, but it’s dynamic in size. We can `push()` values into it in run-time. Memory will look like below:

```text
           [-– vehicles –-]
            +–––+–––+–––+
stack frame │ • │ 3 │ 2 │
            +–│–+–––+–––+
              │
            [–│–– 0 –––] [–––– 1 ––––]
            +–V–+–––+–––+–––+–––+–––+–––+–––+
       heap │ • │ 8 │ 5 │ • │ 4 │ 3 │       │
            +–│–+–––+–––+–│–+–––+–––+–––+–––+
              │\   \   \  │
              │ \   \    capacity
              │  \    length
              │  pointer  │
              │           │
            +–V–+–––+–––+–––+–––+–––+–––+–––+
            │ C │ y │ c │ l │ e │   │   │   │
            +–––+–––+–––+–––+–––+–––+–––+–––+
                          │
                          │
                        +–V–+–––+–––+–––+
                        │ C │ a │ r │   │
                        +–––+–––+–––+–––+
```

Notice how the `vector` object itself, similar to the `string` object earlier, is stored on the `stack` with its `capacity`, and `length`. It also comes with a `pointer`, pointing at the location in the `heap` where the `vector` data is located. The `string` objects of the `vector` are then stored on the `heap`, which in turn own their dedicated buffer.

This creates a tree structure of data where every value is owned by a single variable. When the variable `vehicles` goes out of scope, its values will be dropped which eventually cause the string buffers to be dropped as well.


### Variables and Data Interacting with Move
`Rust` always does a shallow copy.

```rust
	let s1 = String::from("hello");
	// s1 and s2 are pointing to the same data
	// as data isn't copied.
	//
	// s1's ownership is moved to s2. s1 no longer valid
	let s2 = s1;

	println!("{}, world!", s1); // Won't compile s1 not valid
```

`Rust` moves values to their new owner when doing things like value assignment or passing values to functions. This is a very important concept as it affects how we write code in `Rust`.

### Variables and Data Interacting with Clone
It is possible to do deep copy with the `clone()` method.

```rust
	let s1 = String::from("hello");
	let s2 = s1.clone();

	println!("s1 = {}, s2 = {}", s1, s2);
```


### Ownership and Functions
`Ownership` is also move in function parameters:

```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it's okay to still
                                    // use x afterward

	// This won't compile as the ownership is moved
	println!("String {s}");

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

### Return Values and Scope
Return values can also transfer `Ownership`

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 was moved, so nothing
  // happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it

    let some_string = String::from("yours"); // some_string comes into scope

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function
}

// This function takes a String and returns one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into scope
    a_string  // a_string is returned and moves out to the calling function
}
```

### Note
It should be noted that _`primitive` types in `Rust` have special behavior. These implement the `Copy` trait by default_. Types implementing `Copy` trait are duplicated at times that would otherwise be illegal. Formally, `primitive` types are said to possess `copy` semantics, whereas all other types have `move` semantics.
Hence, the below code will work

```rust
#![allow(unused_variables)]

#[derive(Debug)]
enum StatusMessage {
  Ok,
}

fn check_status(sat_id: u64) -> StatusMessage {
  StatusMessage::Ok
}

fn main () {
  let sat_a = 0;
  let sat_b = 1;
  let sat_c = 2;

  let a_status = check_status(sat_a);
  let b_status = check_status(sat_b);
  let c_status = check_status(sat_c);
  println!("a: {:?}, b: {:?}, c: {:?}", a_status, b_status, c_status);

  // "waiting" ...
  let a_status = check_status(sat_a);
  let b_status = check_status(sat_b);
  let c_status = check_status(sat_c);
  println!("a: {:?}, b: {:?}, c: {:?}", a_status, b_status, c_status);
}
```
Types implementing `Copy` are duplicated at times that would otherwise be illegal. Formally, `Primitive types` are said to possess `copy` semantics, whereas all other types have `move` semantics.


## Borrowing
After `Ownership`, the next important concept to understamd is `Borrowing`.
Consider the below code snippet:

```rust
let x = vec![1, 2, 3, 4, 5];
let y = x;                  // here ownership of x is moved to y
```

In this example, the `Ownership` of the vector `x` is moved to `y`, and `x` can no longer be used. However, what if we want to use `x` after we've assigned it to `y`? This is where the concept of `Borrowing` comes in. `Borrowing` is a way of giving temporary access to a value without transferring the `Ownership`.

`Borrowing` in `Rust` is achieved through _references_, which are _pointers_ to values that are _borrowed_. A _reference_ is denoted by the symbol `&`, followed by the type of the value being referenced. For example:

```rust
let x = vec![1, 2, 3, 4, 5];
let y = &x;                 // here y is a reference to x
```

In the example, `y` is a reference to the `x` and the reference `y` can be used to access the vector's elements without transferring `ownership`. However, `y` can't modify the vector because it's an *immutable reference*. To create a *mutable reference*, `&mut` syntax has to be used as below:

```rust
let x = vec![1, 2, 3, 4, 5];
let y = &mut x;             // here y is a mutable reference to x
```

Now, reference `y` can be used to modify the vector's elements without transferring `ownership` as `y` is a mutable reference to `x`.


### Borrowing rules
While `Borrowing` is a powerful feature in `Rust`, it comes with a set of rules which must be followed to ensure memory safety and avoid data races. These rules include the following:

> [!NOTE] Borrowing Rules
> 1. Each resource can only have one mutable reference or any number of immutable
>    (read only) references at a time.
> 2. You cannot move or modify a value while a reference to the value exists.
> 3. References  must always  be  valid,  which  means  that the  resource  being
>    referenced must remain in scope for the entire lifetime of the reference.
> 4. A  mutable reference cannot  exist at the same  time as any  other reference,
>    whether mutable or immutable. Only one mutable reference can exist at a time.
> 5. Certain types of values are copied by default instead of moved, like:
>    `numbers`, `bools`, `chars`, `arrays/tuples` with *Copyable* elements.

+ `Shared references` are `Copy`
+ `Mutable references` are `not Copy`

The distinction between `shared` and `mutable` references can be understood as enforcing *multiple readers* or *single writer* during compile time. This rule in fact doesn't apply only to references, but it also covers the borrowed references as well. As long as there are shared references to a value, not even its owner can modify it; the value is just locked down.

> Keeping sharing and mutation fully separate turns out to be essential for memory safety,

#### Violations
Below is a violation to *rule no. 4*:

> [!WARNING] Violation of rule no 4
> ```rust
> fn main() {
>     let mut x = vec![1, 2, 3, 4, 5];
>     let y = &x;         // immutable reference to x
>     let z = &x;         // another immutable reference to x
>     let w = &mut x;     // error: can't have a mutable reference and
>                         // immutable references at the same time
>                         // violation of rule no. 4
>
>     println!("x: {:?}, y: {:?}, z: {:?}, w: {:?} ", x ,y, z, w);
> }
> ```

Below is a violation to *rule no.2*:

> [!WARNING] Violation of rule no. 2
> ```rust
> fn main() {
>     let mut x = 5;
>     let y = &x;
>
>     x += 1;             // error: can't modify x while it's borrowed
>     println!("{}", y);
> }
> ```

The `Rust` compiler enforces these rules during compile time, ensuring that the code is safe from data races and other memory related bugs.

### Some pointers about Move
Below are some of the points to keep in mind while understanding `move` in `Rust`.

> [!IMPORTANT]: Move semantics
> 1. Ownership of a value can be transferred between variables or functions
>    using the move  semantics. When move happens, the  previous owner will
>    no longer  be able to access  the value. This prevents  issues such as
>    use-after-free bugs that can occur in other programming languages.
>
> 2. Move  happens with  Heap-allocated data  types like  String, Vec,  and
>    other complex types that allocate memory on the heap.
>
> 3. Move  doesn’t apply  to  primitive data  types  like integer,  char,
>    float,  bool and  other  simple Stack-based  data  types because  they
>    implement the Copy trait.
>
> 4. Types that are composed only of  other types that implement Copy trait
>    are also automatically considered Copy.  For example, i32 and bool are
>    both Copy, so a struct containing  only i32 and bool fields would also
>    be Copy.

### Some pointers about Copy
The below pointers will help understand `Copy` better.

> [!IMPORTANT]: Copy semantics
> 1. When ownership of a variable is copied, a new variable is created that
>    is a complete copy of the original variable, including its data.
>
> 2. Types that implement Copy trait are automatically copied when they are
>    assigned to a new variable, passed as a function argument, or returned
>    from a function.

## Lifetimes
`Lifetimes` refers to how long an owner or reference exists before `Rust` decides to reclaim that memory it was using and they serve as a way of tracking the `scope` of a reference to an object in memory. Every value in `Rust` has one owner, and when that owner goes out of `scope`, the value is dropped, and its memory is freed. `Lifetimes` allow `Rust` to ensure that a reference to an object remains valid for as long as it’s needed.

There is another concept called `Generic Lifetimes` which refer to the extra syntax added to clarify relationship between different lifetimes. It is also sometimes referred as `Lifetime Annotations`.

Essentially, the `lifetime` system tracks the lifespan of every value, ensuring that references do not outlive their intended lifetime and preventing issues, such as dangling pointers/references and memory leaks.

>[!IMPORTANT]
> The following are the rules which govern `Lifetime` system:
> 1) When an owner  goes out of scope, the value owned by  it is dropped to help cleanup and reclaim the memory.
> 2) There can't be references to a value when its owner goes out of scope.
> 3) References to a value can't outlive the value they refer to.

Two types of `lifetimes` exist:
+ Input Lifetime: This is the `lifetime` of a reference passed as an input argument to a function.
+ Output Lifetime: This is the `lifetime` of a reference returned by a function.

In `Rust`, `lifetimes` are denoted using a special syntax `'a`, where the `'a`  is a placeholder for the actual `lifetime`. The `lifetime` can be defined as a generic parameter in a `function`, `struct` or `trait` using *angle* brackets. An example is shown here:

```rust
struct Path<'a> {
    point_x: &'a i32,
    point_y: &'a i32,
}

fn main() {
    let p_x = 3200;
    let p_y = (p_x / 2) as i32;
    let maze = Path { point_x: &p_x, point_y: &p_y };
    println!("x = {}, y = {}", maze.point_x, maze.point_y);
}
```

The output of above would be:

```bash
x = 3200, y = 1600
```

Here, `struct Path` is defined with two fields:

`point_x` and `point_y`,

which references an `i32` type.

The `i32` value (or type) represents a signed integer with value range of `-2147483648` to `2147483647`. The lifetime `'a` specifies that the reference must live at least as long as the instance of the `struct`.

The below example which is similar to earlier will fail:

> [!FAILURE] This code will fail
> ```rust
>     …
> fn main() {
>     let p_x = 3200;
>     let p_y = {
>         let temp = 42;
>         &temp
>     };
>     let maze = Path { point_x: &p_x, point_y: p_y };
>     println!("x = {}, y = {}", maze.point_x, maze.point_y);
> }
> ```

> [!ERROR] Error message from the above
> ```bash
> |
> |     let p_y = {
> |         --- borrow later stored here
> |         let temp = 42;
> |         &temp
> |         ^^^^^ borrowed value does not live long enough
> |     };
> |     - `temp` dropped here while still borrowed
> ```

In this example, the compiler detects an error when the lifetime reference of `temp` goes out of scope. This error prevents the further use of `p_y` in the program because the value of temp has already been dropped. The issue arises because `p_y` is assigned a borrowed reference, `&temp`, which cannot exist outside the scope of `p_y`. This error occurs due to the mismatched lifetimes.

And here is the modified version of the code that works:

```rust
    …

fn main() {
    let p_x = 3200;
    let temp;
    let p_y = {
        temp = 42;
        &temp
    };
    let maze = Path { point_x: &p_x, point_y: p_y };
    println!("x = {}, y = {}", maze.point_x, maze.point_y);
}
```

This approach works because `temp` and `p_y` have the same lifetime, allowing the `temp` variable to exist for the duration of the program. This means that the reference to `temp`, assigned to `p_y`, remains valid and can be used throughout the program.

We have to think about annotations, anytime when our function receives a `ref` and returns a `ref`.

An example for these cases follow:
In the example for function `next_language` a lifetime annotation was required.

```rust
// Here, the function takes 2 `refs` and returns a `ref`
fn next_language<'a>(languages: &'a [String], current: &str) -> &'a str {
    let mut found = false;

    for lang in languages {
        if found {
            return lang;
        }

        if lang == current {
            found = true;
        }
    }

    languages.last().unwrap()
}

// this is a corner for lifetime annotations where, the function takes
// a `ref` + any number of args (or or more than 1) and returns a `ref`
fn last_language(languages: &[String]) -> &str {
    languages.last().unwrap()
}

// Here the result should know whether the value is from reference of first
// argument or reference of second argument and hence the annotations
fn longest_language<'a>(lang_a: &'a str, lang_b: &'a str) -> &'a str {
    if lang_a.len() >= lang_b.len() {
        lang_a
    } else {
        lang_b
    }
}

fn main() {
    let languages = vec![
        String::from("rust"),
        String::from("go"),
        String::from("typescript"),
    ];

    let result = next_language(&languages, "go");

    println!("{:#?}", result);
}
```

### Another example for lifetimes

Here is another example with explicit lifetime annotations
```rust
fn add_with_lifetimes<a', b'>(x: &'a i32, y: &'b i32) -> i32 {
    *x + *y
}

fn main() {
    let a = 10;
    let b = 20;

    // &10 and &20 mean reference 10 and 20 respectively. No
    // lifetime notation is required while calling the function
    let res = add_with_lifetimes(&a, &b);

    println!("{}", res);
}
```

> 1. The values `<'a, 'b>` declare two lifetime variables, `'a` and `'b`
>    within  the scope  of the  function `add_with_lifetimes()`.  These are
>    normally spoken as *lifetime of x* and *lifetime of y*.
> 2. `x: &'a  i32` binds lifetime variable `'a` to  the lifetime of `x`.
>    The syntax  reads as "parameter  `x` is a  reference to an  `i32` with
>    lifetime `'a`".
> 3. `y: &'b  i32` binds lifetime variable `'b` to  the lifetime of `y`.
>    The syntax  reads as "parameter  `y` is a  reference to an  `i32` with
>    lifetime `'b`".


### Lifetime elision

Omitting lifetime annotations is referred to as `elision`.

While reading documentation about *lifetime annotations*, we might see the below as a portion of speech which needs to be understood.

| Statement                             | Inference                             |
|---------------------------------------|---------------------------------------|
| I `removed` the lifetime annotations  | I `elided` the lifetime `annotations` |
| We can remove the `annotations`       | We can `elide` the `annotations`      |
| Think about removal of `annotations`  | Think about `elision` of `annotations`|


>[!NOTE]
> Rules of lifetime elision:
>+ The first  rule of lifetime  elision is  specifically for input  lifetimes. Each
>  parameter that is a reference gets its own lifetime parameter.
>+ If there is  exactly one input lifetime parameter, that  lifetime is assigned to
>  all the output lifetime parameters.
>+ If there are multiple  input lifetime parameters, but one of  them is `&self` or
>  `&mut self` as a part of a method, the lifetime of `self` is assigned to all the
>  output lifetime parameters.


## Note on using Function arguments
The following pointers serve as rough guidelines to help decide on the type of function arguments:

>1. Need to store the argument somewhere?
>   Favour taking `Ownership` (receive a value)
>2. Need to do a calculation with the value?
>   Favor receiving a `Read Only` reference
>3. Need to change the value in some way?
>   Favor receiving a `Mutable` reference.

## Compound Datatypes
When we are modelling several different items which are all of the same kind, then we have two options to follow:

+ `Structs`: If each item have some same but some different methods
+ `Enum`: If each item have same methods

An example for using `Enums` is demonstrated here:
where, *Book*, *Movie* & *AudioBook* all have one common method called `description()`

```rust
enum Media {
    Book { title: String, author: String },
    Movie { title: String, director: String },
    AudioBook { title: String },
}

impl Media {
    fn description(&self) -> String {
        match self {
            Media::Book { title, author } => {
                format!("Book: {} {}", title, author)
            }
            Media::Movie { title, director } => {
                format!("Movie: {} {}", title, director)
            }
            Media::AudioBook { title } => {
                format!("AudioBook: {}", title)
            }
            _ => String::from("Unknown Media"),
        }
    }
}

fn main() {
    let audiobook = Media::AudioBook {
        title: String::from("A Million Thoughts!"),
    };
    let movie = Media::Movie {
        title: String::from("From Russion With Love"),
        director: String::from("Terence Young"),
    };
    let book = Media::Book {
        title: String::from("Ancient Science of Mantras"),
        author: String::from("Om Swamy"),
    };

    println!("{}", audiobook.description());
    println!("{}", movie.description());
    println!("{}", book.description());
}
```

Had each of the *3* `structs` contained different methods, then we could have preferred a `Struct` instead of `Enum`.

### Option enum
`Rust` does not have the concept of `Null`, `Nil` or `undefined`. Instead, it returns a built-in `enum` called `Option` whose definition is as shown below:

```rust
enum Option {
    Some(value),
    None
}
```

It has two variants `Some` and `None` and it has to be pattern matched in order to extract the value.

### Result for Error Handling
In the `std::io` module, we can find the `Result` type definition as below:

```rust
pub type ResultT> = Result<T, Error>
```

+ The `std::io::Result` is a type alias for the standard `Result` type in `rust`,
  with the error type fixed to `std::io::Error`. The purpose of this type alias
  is to make it easier to work with `Result` values which may return `I/O` errors.
  
+ If we are returning an error of type `std::io::Error` we can either use the
  standard `Result<T, E>` type or the `std::io::Result<T>` type. Both types are
  essentially the same with the only difference being the _error_ type.
  The standard `Result<T, E>` can hold any type `E` as the error, whereas the
  `std::io::Result<T>` type is a type alias for `Result<T, std::io::Error>,
  indicating that it can only hold the `std::io::Error` objects as the error.

> [!NOTE]
> `std::io::Error` is an error type provided by the `Rust` standard library `io`
> module. `std::io::Error` type is used to represent errors that occur during `I/O`
> operations, like reading or writing to a file or network socket.

An example for demonstration:
```rust
use std::fs;
use std::io;

fn rename_file_v1(from: &str, to: &str) -> Result<(), io::Error> {
    match fs::rename(from, to) {
        Ok(_) => Ok(()),
        Err(e) => Err(e),
    }
}

fn rename_file_v2(from: &str, to: &str) -> io::Result<()> {
    // match fs::rename(from, to) {
    //     Ok(_) => Ok(()),
    //     Err(e) => Err(e),
    // }
    // using error propagation operator ? this time
    fs::rename(from, to)?;
    Ok(())
}

fn main() {
    let res1 = rename_file_v1("inlog1.txt", "outlog1.txt");
    println!("Using Variant 1 to rename from inlog1.txt to outlog1.txt");
    if res1.is_err() {
        let error = res1.unwrap_err();
        eprintln!("Rename failed");
        eprintln!("Error kind: {:?}", error.kind());
        eprintln!("Error details: {:?}", error);
    } else {
        println!("Succesfully renamed");
    }

    let res2 = rename_file_v2("inlog2.txt", "outlog2.txt");
    println!("Using Variant 2 to rename from inlog2.txt to outlog2.txt");
    if res2.is_err() {
        let error = res2.unwrap_err();
        eprintln!("Rename failed");
        eprintln!("Error kind: {:?}", error.kind());
        eprintln!("Error details: {:?}", error);
    } else {
        println!("Succesfully renamed");
    }
}
```

Assuming there are no files to rename, so that error conditions are triggered, here is the output:
```bash
λ cargo run -q
Using Variant 1 to rename from inlog1.txt to outlog1.txt
Rename failed
Error kind: NotFound
Error details: Os { code: 2, kind: NotFound, message: "No such file or directory" }
Using Variant 2 to rename from inlog2.txt to outlog2.txt
Rename failed
Error kind: NotFound
Error details: Os { code: 2, kind: NotFound, message: "No such file or directory" }
```


Usually pattern matching is used on the results to return a valid value for such types, but a shorthand operator `?` can be used instead to avoid boiler plate code. It is also called as **try-operator**.

#### The try-operator `?`

It is used for propagating errors. Sometimes we write code that might fail but we do not want to catch and handle error immediately. Your code will not be readable if you have too much code to handle the error in every place. Instead, if an error occurs, we might want to let our caller deal with it. We want errors to propagate up the call stack.

```rust
// file type is Result if "?" is not used
// file:Result<File,Error>
let mut file = File::create("foo.txt");

 // file type is File if "?" is used
 // file:File
 let mut file = File::create("foo.txt")?;
 // if an error occurs, code after this line will not be executed
 // function will return the error
 // if we do not return here, the program will continue execution
 // even though an error occurred. This could lead to unexpected
 // behavior or incorrect results.
```


The behavior of `?` here depends on whether this function returns a successful result or an error result:

- If it is a `success`, it unwraps the `Result` to get the `success` value inside. The value is assigned to the variable `file`.
- If the result is an `error`, the `error` is NOT assigned to the variable `file`. `Error` is returned by the function to the caller. If you are inside `main()` and `?` returns `error`, this `error` message will be printed on the terminal

Using `?` is sugar coating to the below code.

```rust
let mut file = match File::create("foo.txt") {
        Err(why) => panic!("couldn't create {}: {}", display, why),
        Ok(file) => file,
    };
```

Another small code snippet to demo the use of `From::from()` before this question mark:

```rust
fn _parse(str: &str) -> Result<i32, &str> {
    if let Ok(num) = str.parse::<i32>() {
        Ok(num)
    } else {
        Err(str)
    }
}

fn parse(str: &str) -> Result<()> {
    let num = _parse(str)?;
    println!("{}", num);
    Ok(())
}
```

The use of `?` in function `parse()` can be manually rewritten as:

```rust
fn parse(str: &str) -> Result<(), String> {
    match _parse(str) {
        Ok(n) => {
            println!("{}", n);
            Ok(())
        }
        Err(str) => Err(<String as From<&str>>::from(str)),
    }
}
```

Another example of handling errors using the `?` operator:

Here the `?` operator for handling the `Result` type will:
> [!HINT] The ? operator
> + Give what is present inside the `Result` if it is `Ok`
> + Pass the error back if it is `Err` (which is called early return)

```rust
use std::num::ParseIntError;

fn parse_and_log_str(input: &str) -> Result<i32, ParseIntError> {
    // Using ? operator to handle Result
    let parsed_number = input.parse::<i32>()?;
    println!("Number paesed successfully into {parsed_number:#?}");
    Ok(parsed_number)
}

fn main() {
    let str_vec = vec!["Six", "7", "8", "9.0", "nice", "9999"];
    for i in str_vec {
        let p = parse_and_log_str(i);
        println!("Result: {p:?}");
    }
}
```


When run, it returns below:
```bash
Result: Err(ParseIntError { kind: InvalidDigit })
Number paesed successfully into 7
Result: Ok(7)
Number paesed successfully into 8
Result: Ok(8)
Result: Err(ParseIntError { kind: InvalidDigit })
Result: Err(ParseIntError { kind: InvalidDigit })
Number paesed successfully into 9999
Result: Ok(9999)
```

> [!ATTENTION] Other usage of `?`
> The `?` operator works with `Option` type as well, although the
> majority of the time we see it used to handle a `Result` type.


# Strings in Rust
`Rust’s` string handling mechanism is a bit different from other languages, due to its focus on systems level programming. Strings are re-sizeable data structures and any time we have a data structure of variable size, things can get tricky. That said, `Rust’s` strings also work differently than in some other systems languages like `C`.

A `‘string’` is a sequence of Unicode scalar values encoded as a stream of `UTF-8` bytes and all `strings` are guaranteed to be a valid encoding of `UTF-8` sequences. Also, unlike some systems languages, `strings` in `Rust` are not _NULL_ terminated and can contain _NULL_ bytes.

`Rust` has two main types of strings: `&str` and `String`.

- `&str`: These are called **‘string slices’**. They have a fixed size and
  cannot be mutated. It is a reference to a sequence of `UTF-8` bytes.


```rust
let greeting = "Hi Hello!";  // greeting: &'static str
```

"Hi Hello!" is a `string literal` and  its type is `&'static str`. A `string literal`
is a  `string slice` that is  statically allocated, meaning that  it’s saved inside
our compiled  program, and  exists for  the entire duration  it runs.  The `greeting`
binding is a reference to this statically allocated string. _Any function expecting a
string slice will also accept a string literal._

> [!NOTE] Note about str
> Note that you normally cannot access an `str` directly, but only via `&str` reference.
> This is because `str` is an `unsized` type requiring additional runtime information to be usable.


- `String`: These are called **String literals** and they can span multiple
  lines. This type is growable, and is also guaranteed to be `UTF-8`. `Strings`
  are commonly created by converting from a `string slice` using the `to_string`
  method.

The `String` type is used when the length  of the string is not known at compile
time  or when  it  needs to  be  modified  during runtime,  such  as taking  and
processing user inputs.

If we want to create a mutable string  that needs to be modified during run time
of the program, use the `“String”` type instead of a `string literal`.

There are multiple ways of creating a string literal as under:
```rust
// Creating an empty string
let mut s = String::new();

// Converting as string literal to a String
let s = "initial contents".to_string();

// Same as above, but different syntax
let s = String::from("initial contents");
```

```rust
fn main() {
    let mut s = "Hello".to_string(); // mut s: String
    println!("{}", s);

    s.push_str(", world.");
    println!("{}", s);
}
```

Let's check the types of values for each for below program:
```rust
fn main() {
    let s1 = String::from("abcdefghijk");   // utf-8 encoded string
    let s2 = s1.clone();                    // utf-8 encoded string
    let s3 = &s1[1..=3];                    // string slice
    let s4: String = "abcdef".to_string();  // utf-8 encoded string
    let s5 = "abcdef";                      // string slice

    println!("type of s1 {}", type_of(&s1));
    println!("type of s2 {}", type_of(&s2));
    println!("type of s3 {}", type_of(&s3));
    println!("type of s4 {}", type_of(&s4));
    println!("type of s5 {}", type_of(&s5));
}

#[warn(dead_code)]
fn type_of<T>(_: &T) -> String {
    format!("{}", std::any::type_name::<T>())
}
```

Output from the above is:
```sh
type of s1 alloc::string::String
type of s2 alloc::string::String
type of s3 &str
type of s4 alloc::string::String
type of s5 &str
```


## String vs &str in Rust functions
Both `String` and `&str` provide a read-only reference to the text data. The `&str` type which is called as *String slice* allows us to refer to the text data in `Data Segment` without `Heap` memory allocation, and hence are slightly more performant than the `String literals` denoted by `String`.

> `&str` is a *Stack* allocated UTF-8 string, which can be borrowed but cannot be moved or mutated.
> `String` is a *Heap* allocated UTF-8 string which can be borrowed or mutated.

So, essentially below snippet can be replaced

```rust
let color = String::from("red");
let color_ref = &color;
```

with

```rust
let color = "red"; // This is an `&str` or a string literal but not a `String`
```

A string literal is created by surrounding text with double quotes as shown earlier.

> [!NOTE]
> `&str` also let's us take a slice (portion of the text) which is already on `Heap`.
> `&String::from("text")` will be automatically converted by `Rust` into an `&str`
> string slice and hence `&String` is very rarely used.
> Viewing a `String` as an `&str` is cheap, but converting the `&str` to a `String`
> involves allocating memory.

| Name    | When to use                                     | Uses memory In | Notes                             |
|---------|-------------------------------------------------|----------------|-----------------------------------|
| String  | When we want to take ownership of text data     | Stack and Heap |                                   |
|         | When we need a string that might grow or shrink |                |                                   |
| &String | Very rare and not used often                    | Stack          | Rust automatically turns `&String`|
|         |                                                 |                | into `&str` for you               |
| &str    | When we want to read all or a portion of some   |                | Refers directly to Heap allocated |
|         | text owned by someone/something else            | Stack          | or data allocated text            |


### Types of &str
There is an interesting fact about `&str` as there is more than one type of it.
The two ways we can see `&str` are:

>[!IMPORTANT]
> &str types
>1) `String Literals`: If we write something like `let mystr = "A sample &str";`, it
>   will last for the  whole life of the program as it is  written directly into the
>   **binary**. It  will have  the type  `&'static string`  with the  string literals
>   having a lifetime called `static`.
>   
>2) `Borrowed  str`: This  one  is the  regular `&str`  form  without the  `'static`
>   lifetime  specifier. If  we have  a `String`  and pass  a reference  to it  (_an
>   `&String`_), `rust` will  convert it to an `&str` when  needed. This is possible
>   due to the `trait` called `Deref`.

## Shallow Copy & Deep Copy

When a `String` value is assigned  to another variable, the variable is assigned
a  copy of  the `pointer`,  `length`, and  `capacity` of  the original  `String`
value,  but the  underlying `Heap  memory` is  not copied.  This is  known as  a
`Shallow Copy` or a `Copy By Reference`.

```rust
fn main() {
    let s1 = String::from("Hello");

    // Now s1 is out of scope and is not more valid after here
    let s2 = s1;

    // s2 essentially points to the same memory as s1
    println!("{}, World!", s2);

    // this will not work as ownership already moved to s2
    // println!("{}", s1);
}
```

<details>
    <summary>Details of Ownership of s1 being transferred to s2</summary>

```text
  // Ownership of s1 is transferred to s2
  // s1 on Stack

   Capacity    Length   Reference
  +----------+--------+----------+
  |    5     |    5   |   0x216  |
  +----------+--------+----------+

  // Representation On Heap for Data allocation

  // Originally s1 points to this ==>
                                  0x216
                                  +---+---+---+---+---+
                                  | h | e | l | l | o |
                                  +---+---+---+---+---+

  // After let s2 = s1;
  // s2 now points to the above Heap ==>
  // Heap will be untouched, but the reference to Heap will move
  // to s2 and s1 becomes invalid

   Capacity    Length   Reference
  +----------+--------+----------+
  |    5     |    5   |   0x216  |
  +----------+--------+----------+
```
</details>

When a `String` value  is `cloned`, a new `Heap` allocation  is created with the
same contents  as the original  `String`. This  is known as  a `Deep Copy`  or a
`Copy By Value`.

```rust
let s1 = String::from("Hello");
let s2 = s1.clone();

println!("{}, World!", s2);

// no problem with s1 as well
println!("{}, World!", s1);
```

`Cloning`  is a  more  expensive  operation than  a  `Shallow  Copy` because  it
requires allocating new memory and copying the original string's data to the new
location.

<details>
    <summary>Deep Copy in Action</summary>

```text
// After Deep copy, both references point to same Heap
// s1

     Capacity    Length   Reference
    +----------+--------+----------+
    |    5     |    5   |   0x216  |
    +----------+--------+----------+


                                  0x216
   // s1 ==>                      +---+---+---+---+---+
                                  | h | e | l | l | o |
                                  +---+---+---+---+---+

   // After Deep copy, the details would be

                                  0x810
                                  +---+---+---+---+---+
                                  | h | e | l | l | o |
   // s2 ==>                      +---+---+---+---+---+

// s2
     Capacity    Length   Reference
    +----------+--------+----------+
    |    5     |    5   |   0x810  |
    +----------+--------+----------+
```
</details>

# Conditional statements
The section provides pointers on when to use which conditional statement:

| Conditional statement                             | Details                                                   |
|---------------------------------------------------|-----------------------------------------------------------|
| `match` of `if let` statement                     | When we are ready to meaningfully deal with an error      |
| `unwrap()` or `expect()` on `Result`              | Quick debugging or if we want tp crash on an `Err()`      |
| *try* operator `?` to unwrap or propagate result  | When there is no way to handle error in current function  |

`Match` statements are good for dealing with errors.

```rust
fn read_config_file() -> Result<String, Error> {
    fn::read_to_string("config.json")
}

fn get_config() -> String {
    let default_config = String::from(
        "{ enable_debug: true }"
    );

    match read_config_file() {
        Ok(config) => config, // file read successfully
        Err(_err) => {
            // here, in case of error, we are doing something beyond logging
            println!("Config read err, using default");
            default_config
        }
    }
}

fn main() {
    let config = get_config();

    priintln!("Got a config: {}", config);
}
```

## Iterator functions

| Function      | Description                                                                           |
|---------------|---------------------------------------------------------------------------------------|
| `iter()`      | The iterator will give a *read-only* reference to each element                        |
| `iter_mut()`  | The iterator will give a *mutable* reference to each element                          |
| `into_iter()` | The iterator will give *ownership* of each element uless called in `ref` to a `vector`|


# Some examples

```rust
// iter()
let v1 = vec![1, 2, 3];
let mut v2 = vec![4, 5, 6];

/// Here, after using iter() v1 is not destroyed and still available
for i in v1.iter() {
    println!("&i32: {i}");
}

/// v1 is still available here after using iter() earlier
for i in v1 {
    println!("i32: {i}");
}

/// Here, using iter_mut() we are looping over mutable references
/// so v2 still persists after it is done
for i in v2.iter_mut() {
    *i *= 2;
    println!("i is now: {i}");
}

/// still able to access v2 here, but v1 is not available
println!("{v2:?}");
```

An example using `if-else-if` for pattern matching:

```rust
fn main() {
    let point = (5, 8);

    if let (0, 0) = point {
        println!("Point is at the origin");
    } else if let (_, y @ 1..=8) = point {
        println!("{} is within the range 1..8", y);
    } else {
        println!("somewhere else");
    }
}
```

Example for `iter` and `unwrap` together:
```rust
let data = (0..).take(11).collect::<Vec<i16>>().iter().skip(4).take(4).map(|i| i*2).collect::<Vec<i16>>();

// data will have [8, 10, 12, 14]
// if we want to extract element outside the data
let v = data.get(4).unwrap_or_else(|| &99);
// v will now have 99
```

example using `closure`
```rust
"abcdef".chars().into_iter().enumerate().for_each(|(i, c)| println!("{i}: {c}"));

// Will print
// 0: a
// 1: b
// 2: c
// 3: d
// 4: e
// 5: f
```

example using `zip`:
```rust
use std::collections::HashMap;

fn main() {
    let keys = (0..).take(6).collect::<Vec<u8>>();
    let values = vec!["zero", "one", "two", "three", "four", "five", "six"];

    let num_word_hashmap = keys
        .into_iter()
        .zip(values.into_iter())
        .collect::<HashMap<_, _>>();
    // println!("{:#?}", num_word_hashmap);
    // this will return
    // {5: "five", 4: "four", 1: "one", 2: "two", 3: "three", 0: "zero"}
    println!("Value at the key 3: {}", num_word_hashmap.get(&3).unwrap());
    // The above will return:
    // Value at the key 3: three
}
```

An example for `HashMap`:
```rust
use std::collections::HashMap;

fn main() {
    let text = "this is a sample text that contains some sample details of a text";
    let count = count_words(text);

    for (w, c) in &count {
        println!("{}: {}", w, c);
    }
}

fn count_words(text: &str) -> HashMap<String, u32> {
    let mut count = HashMap::new();
    let processed_text: String = text.chars().filter(|c| c.is_alphanumeric() || c.is_whitespace()).collect();

    for word in processed_text.split_whitespace() {
        count.entry(word.to_string()).and_modify(|c| *c += 1).or_insert(1);
    }

    count
}
```

Output:
```bash
this: 1
that: 1
a: 2
contains: 1
is: 1
details: 1
text: 2
some: 1
of: 1
sample: 2
```

Another example with `filter`, `iter` and `into_iter`:
```rust
#[derive(Debug)]
struct Point {
    x: i32,
    y: i32,
}

// using into_iter() will consume the input
// using iter() will allow for reuse
fn main() {
    let numbers1 = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    let numbers2 = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

    let evens_1 = numbers1.iter().filter(|x| (*x % 2) == 0);

    let evens_2: Vec<_> = numbers2.into_iter().filter(|&x| (x % 2) == 0).collect();

    for x in evens_1 {
        println!("{:?}", x);
    }

    println!("{:?}", evens_2);

    let evens_3 = (0..)
        .take(10)
        .collect::<Vec<i32>>()
        .into_iter()
        .filter(|&x| (x % 2) == 0)
        .collect::<Vec<i32>>();
    println!("{:?}", evens_3);

    let pts = vec![Point { x: 4, y: 6 }, Point { x: 5, y: 12 }, Point { x: 7, y: 13 }];
    let less_than_four = pts.iter().filter(|p| p.x <= 5);
    for n in less_than_four {
        println!("{:?}", n);
    }
    
    // using iter()
    let strs1 = vec!["eagles".to_string(), "tomatoes".to_string(), "rust".to_string(), "ant".to_string()];
    let fstr1 = strs1.iter().filter(|s| s.len() <= 4);
    
    for s in fstr1 {
        println!("{:?}", s);
    }
    
    // using into_iter() to consume original
    // strs2 will not be valid after this...
    let strs2 = vec!["eagles".to_string(), "tomatoes".to_string(), "rust".to_string(), "ant".to_string()];
    let fstr2 = strs2.into_iter().filter(|s| s.len() > 4);
    
    for s in fstr2 {
        println!("{:?}", s);
    }
}
```
Output:
```sh
    2
    4
    6
    8
    10
    [2, 4, 6, 8, 10]
    [0, 2, 4, 6, 8]
    Point { x: 4, y: 6 }
    Point { x: 5, y: 12 }
    "rust"
    "ant"
    "eagles"
    "tomatoes"
```

## ref keyword
When you use `ref` keyword with a variable, it means that the variable is a reference to the value being matched, rather than taking ownership of the value. This allows you to work with a borrowed reference to the value and avoids possible move.

```rust
fn main() {
    let data = ("something".to_string(), "another".to_string());

    // important here
    let (ref x, ref y) = data;

    println!("{:?}", data);
}
```

If we replace the line `let (ref x, ref y)` with `let (x, y)`, then the code will fail with partial borrow error as `data` has been borrowed.

# Some Examples

```rust
(1..=10).step_by(2).collect::<Vec<i32>>()
/// returns [1, 3, 5, 7, 9]

(0..=10).step_by(2).collect::<Vec<i32>>()
/// returns [0, 2, 4, 6, 8, 10]

(1..=10).filter(|x| x % 2 == 0).collect::<Vec<i32>>()
/// returns [2, 4, 6, 8, 10]
```

## Pattern matching for tuple
Here is a pattern matching example for `tuple` using a binding operator `@`:
```rust
fn main {
    let number = (10, 20, 30);
    match number｛
        (_, _, x @ 10..=40) => println! ("last element is : {}",x),
        _ => (),
    ｝
｝

// Below is for an exact match
fn main {
    let number = (10, 20, 30);
    match number｛
        (_, _, x @ 30) => println! ("last element is : {}",x),
        _ => (),
    ｝
｝
```

Here is a pattern matching example for `tuple` using a rest operator `..`:
```rust
fn main {
    let tuple = (1, "hello", 9.9, true, 'z');
    match tuple｛
        (_, _, c, ..) if c > 9.0 => println! ("Third element is greater than 9.0"),
        _ => (),
    ｝
｝
```
The above method is useful while ignoring some elements as shown.

Here is a pattern matching example for `tuple` using variable binding and rest operator:
```rust
fn main() {
    let tup = (2, "greeting", 8.7, true, 's');
    match tup {
        (a @ 2, b @ "greeting", ..) => println!("First and Second elements: {} & {}", a, b),
        _ => (),
    }
}
```

## An error scenario
Consider the below sample.

```rust
fn main() {
    let my_date = ("Monday".to_string(), "25".to_string(), "July".to_string(), "2024".to_string());

    match my_date {
        // replacing the line using below will make it work
        // (ref day, ...) if day == "Sunday" => {
        (day, ..) if day == "Sunday" => {
            println!("It is Sunday");
        }
        _ => println!("Something else...")
    }

    println!("{:?}", my_date);
}
```

The above will throw an error:
> [!ERROR]
> This gives  error because  the my_date  tuple  was partially
> moved  within   the  match  statement.  The   first  element
> was  moved  to  ‘day’   and  consumed,  making  the_date
> inaccessible for further use outside of the match statement.

In such cases, we can use the `ref` keyword as mentioned in earlier section.


# Inferior Mutability
In `Rust` there is a way to safely change values inside a `struct` without qualifying the `struct` itself as `mut`. It has four ways to allow some safe mutability inside of something which is _immutable_.

- `Cell`
- `RefCell`
- `Mutex`
- `RwLock`

# Cow
The abbreviation `Cow` stands for "clone-to-write" and is an `enum` of convenience. It is a smart pointer and is a convenient wrapper that allows us to express the idea of optional ownership in `Rust`. One of the striking features of `Cow` is that it only __clones__ the data when __mutation__ is necessary, making it particularly useful in optimizing the performance by avoiding unecessary allocations.

Since it is an `enum`, it has few values associated with it; which are the two states of the `enum`:

> [!NOTE] 2 states of `Cow`
> + `Borrowed`
> + `Owned`

It is inside the `std::borrow` module providing a clever way of handling both __borrowed__ and __owned__ data with the same *interface*.

Here is how it is defined:
```rust
pub enum Cow<'a, B>
where
    B: 'a + ToOwned + ?Sized,
{
    Borrowed(&'a B),
    Owned(<B as ToOwned>::Owned),
}
```

> [!IMPORTANT] Breaking down the above definition of `Cow`:
> `Cow` is an `enum` consisting of two parameters
> - A lifetime parameter `'a` and
> - A type parameter `B`

`B` can represent any type that implements the `ToOwned` trait, but may or may not be sized as indicated by `?Sized` trait. Furthermore, if `B` contains any references, they must live for at least as long as the `'a` lifetime. The `ToOwned` trait allows a type to create an owned version of itself from a borrowed one. For example, an `&str` can be converted to a `String` using `ToOwned`.

## Usage of Cow
`Cow` will be useful when we want to avoid unnecessary _cloning_ or _allocation_ of data which may not need to be mutated. In other words, it is mainly used to reduce memory allocation and copying; it can be used to copy memory only when needed, thereby reducing the number of memory copies greatly.

For instance, let us say we have a function which takes a slice of integers and returns a new slice with all negative values replaced with corresponding absolute values. If the input slice has no negative values, then a new slice is not required to be created at all and the input slice can be returned as is in such case. But if the input slice does contain negative values, then a new slice will be created and the positive values as well as the absolute values will be copied into it. The input slice can be wrapped with `Cow` and either return a `borrowed` or an `owned` slice depending on the case.

```rust
use std::borrow::Cow;

const CENSORED_LETTER_SET: &str = "aeiouAEIOU";

// Here, we are avoiding unnecessary allocation  of new string unless required. The
// returned `Cow` can be used as a normal string though.
fn mask_letters<'a>(input: &'a str, mask: &'a str) -> Cow<'a, str> {
    // check if the input string contains anything from the censored
    // letter set or word
    if input.contains(|c| CENSORED_LETTER_SET.contains(c)) {
        // if contains a letter from censored list then create a
        // new string with the letters replaced by mask
        let output = input.replace(|c| CENSORED_LETTER_SET.contains(c), mask);
        // now return the string as an owned string
        Cow::Owned(output)
    } else {
        // return the input string as a borrowed string here
        Cow::Borrowed(input)
    }
}

fn abs_all(input: &[i32]) -> Cow<[i32]> {
    // check if the input slice contains any negative values
    if input.iter().any(|&x| x < 0) {
        // if value is negative, create a new slice with absolute values
        let mut output = vec![];
        for &x in input {

        }
    }
    for i in 0..input.len() {
        let v = input[i];
        if v < 0 {
            // Clones into a vector if not already owned.
            input.to_mut()[i] = -v;
        }
    }
}

fn main() {
    // No clone occurs because `slice1` doesn't need to be mutated.
    let slice1 = [0, 1, 2];
    println!("s1: {:#?}", abs_all(&slice1));

    // Clone occurs because `slice2` needs to be mutated.
    let slice2 = [-1, 0, 1];
    println!("s1: {:#?}", abs_all(&slice2));

    // We can mutate the returned Cow by getting a mutable reference of
    // Cow using the to_mut method for cloning the data if borrowed
    let mut masked = mask_letters("Hello world", "*");
    masked.to_mut().push('!');
    println!("{}", masked);

    // Here, we are using the into_owned method to get an owned value
    // We may also clone the data if it was borrowed
    let omasked = mask_letters("Hello world", "*");
    let owned = omasked.into_owned();
    println!("{}", owned); // Prints "H*ll* w*rld"
}
```

Another advantage of `Cow` is that it allows us to mutate the data lazily, only when required. For instance if we have a function which takes a `string` and returns a new `string` with if it contains characters from a sensitive string by replacing the characters with a masked value. If the input `string` does not contain anything from the sensitive string, it is returned as is. But, if the input string does contain anything from the sensitive string a new string is created and the non-matching chcracters are copied along with the masked values. We can use `Cow` to wrap the input string and return either a _borrowed_ or an _owned_ string depending on the situation.

Another example:
```rust
use std::borrow::Cow;

const SENSITIVE_WORD: &str = "evil";

fn remove_sensitive_word<'a>(words: &'a str) -> Cow<'a, str> {
    if words.contains(SENSITIVE_WORD) {
        Cow::Owned(words.replace(SENSITIVE_WORD, ""))
    } else {
        Cow::Borrowed(words)
    }
}

fn remove_sensitive_word_old(words: &str) -> String {
    if words.contains(SENSITIVE_WORD) {
        words.replace(SENSITIVE_WORD, "")
    } else {
        words.to_owned()
    }
}

fn main() {
    let words = "He is an evil genius!";
    let new_words = remove_sensitive_word(words);
    println!("{}", new_words);

    let new_words = remove_sensitive_word_old(words);
    println!("{}", new_words);
}
```
The example shows `remove_sensitive_word` and `remove_sensitive_word_old` implementations, in which the former uses `Cow` as it's return value, and the latter uses `String` as it's return value. After careful analysis, it is obvious that the former is more efficient. This is because if there are no sensitive words in the input string, the former `Cow::Borrowed(words)` will not allocate or copy the heap memory, while the latter utlizing `words.to_owned()` will allocate and copy the heap memory once.

While designing and writing `Rust` code, using `Cow` can achieve memory allocation and copying only when needed. In scenarios where there are more reads than writes, the number of heap memory allocations can be reduced, greatly improving system efficiency.

# Anonymous functions or Closures in Rust
`Closures` in `Rust` are `Anonymous functions` which have the flexibility to be saved to a variable or pass in as arguments to other functions. They are similar to functions, but with added capability to capture variables from their surrounding environment and use them. They are very flexible and can be used in place where regular function pointers are used. Because of the flexibility to use variables from it's surroundings, `Closures` can reference and use variables defined outside pf *scope*.

## Capturing variables in closures
`Closures` do not capture all variables from their surrounding environment in the same pattern. The capture behaviour is defined by particular trait` that `closure` implements and there are *3* such `traits` as below with their hierarchy from top to bottom during compile time.


| Trait    | Functionality                 |
|----------|-------------------------------|
| `Fn`     | Only Reads Captured Variables |
| `FnMut`  | Mutates Captured Variables    |
| `FnOnce` | Moves Captured Variables      |


>[!NOTE]
> It has  to be noted  that `closures` do  not have a  specific type and  they are
> often discussed in terms of the above `traits` they implement.

## Understanding the closure traits
Based on how the `closure` uses captured variables, the `Rust` compiler automatically assigns one of the three `traits` Depending on how a `closure` captures variables, it may be called more than once or only once whether mutably or immutably.

>[!NOTE]
> All `closures` implement the `FnOnce` trait, meanning they can all be called at least once.

### Case: Trait `FnOnce` - Moves Captured Variables
The example demonstrates that the closure captures a variable by value, consuming it.
The closure can only be called once.

>[!IMPORTANT]
`FnOnce` trait  allows a closure to  consume the variables it  captures from its
surrounding environment. After being called  once, an `FnOnce` closure cannot be
called again as the environment has been consumed.


```rust
fn main() {
    let msg = "FnOnce()".to_string();
    
    let closure_fn_once = || {
        let _ = format!("Dropped {} here", msg);
        msg
    };
    
    println!("First Call {}", closure_fn_once());       // line:9  Works!
    println!("Subsequent Call {}", closure_fn_once());  // line:10 Fails!
}
```

Running the above program will throw below error:

```bash
error[E0382]: use of moved value: `closure_fn_once`
  --> src/main.rs:10:36
   |
 9 |     println!("First Call {}", closure_fn_once());       // line:9  Works!
   |                               ----------------- `closure_fn_once` moved due to this call
10 |     println!("Subsequent Call {}", closure_fn_once());  // line:10 Fails!
   |                                    ^^^^^^^^^^^^^^^ value used here after move
   |
note: closure cannot be invoked more than once because it moves the variable `msg` out of its environment
  --> src/main.rs:6:9
   |
 6 |         msg
   |         ^^^
note: this value implements `FnOnce`, which causes it to be moved when called
  --> src/main.rs:9:31
   |
 9 |     println!("First Call {}", closure_fn_once());       // line:9  Works!
   |                               ^^^^^^^^^^^^^^^
```

After the variable `msg` is dropped, calling the `closure` again will try to **drop** the same memory twice. This leads to an undefined behaviour resulting in failure. `Rust` compiler prevents dropping the memory more than once by making `closures` to be called only once.

If *line 10* is commented out, the code will work producing below output:

```sh
First Call FnOnce()
```


### Case: Trait `FnMut` - Mutates Captured Variables
The example demonstrates that a closure captures a variable by mutable reference.
It has to be noted that, here the closure can modify the variable.

>[!IMPORTANT]
> If a  closure does  not consume  its surrounding environment,  then it  can also
> implement the `FnMut`  trait indicating that the closure can  be called multiple
> times and can mutate the environment.

```rust
fn main() {
    let mut msg = "FnMut()".to_string();
    
    let mut closure_fn_mut = || {
        msg += " updated";
        format!("{}", msg)
    };
    
    println!("{} once", closure_fn_mut());              // called here
    println!("{} again", closure_fn_mut());             // can be called again
    println!("{} multiple times", closure_fn_mut());    // can be called multiple times...
    println!("Original value: {}", msg);                // final mutated value
}
```

Output for the anove code will be:

```bash
FnMut() - updated once
FnMut() - updated - updated again
FnMut() - updated - updated - updated multiple times
Original value: FnMut() - updated - updated - updated
```

### Case: Trait `Fn` - Only Reads Captured Variables
The example demonstrates that the `closure` captures a variable by `reference`, there by allowing it to be called multiple times in the read-only mode.

>[!IMPORTANT]
> If a  closure does not  mutate its environment, it  can also implement  the `Fn`
> trait, which  means that it  can be called  multiple times without  mutating its
> surrounding environment.

```rust
fn main() {
    let msg = String::from("Fn()");
    
    let closure_fn = || {
        format!("{} called", msg)
    };
    
    println!("{} once", closure_fn());              // called here
    println!("{} again", closure_fn());             // can be called again
    println!("{} multiple times", closure_fn());    // can be called multiple times...
    println!("Original value: {}", msg);
}
```

Output from the above code will be:

```bash
Fn() called once
Fn() called again
Fn() called multiple times
Original value: Fn()
```

## Trait hierarchy
As shown in the [traits table](#capturing-variables-in-closures) they follow a particular hierarchy and the pattern is evident from the examples covered earlier. `Rust` compiler determines the appropriate trait for a closure based on how it interacts with its surrounding values. This is explained with examples in the previous sections.

Here is the hierarchy of traits again:

> `Fn` -> `FnMut` -> `FnOnce`

From the hierarchy above, we can have the following patterns:

1. If a closure implements `Fn` it also implements `FnMut` and `FnOnce`.
2. If a closure implements `FnMut` it also implements `FnOnce`
3. A closure implemeting `FnOnce` may not necessarily implemeht `FnMut` or `Fn`.

Example for Case (1):
```rust
// This function expecte a closure that implements Fn trait
fn call_fn<F: Fn() -> i32>(f: F) {
    println!("Fn called: {}", f());
}

// This function expecte a closure that implements FnMut trait
fn call_fn_mut<F: FnMut() -> i32>(mut f: F) {
    println!("FnMut called: {}", f());
}

// If the closure implements Fn it also implements FnMut
fn main() {
    let x = 3;
    let closure = || x * 2;     // Closure Implements Fn by default
    
    println!("{}", closure());  // Can be called any number of times
    println!("{}", closure());
    
    call_fn(&closure);          // passing Fn - No Problem
    call_fn_mut(&closure);      // passing Fn for FnMut - satisfies (1)
}
```

Output:
```sh
6
6
Fn called: 6
FnMut called: 6
```

The vice-versa of the above code will fail and does not comply with (1):
```rust
// Failing Code
fn call_fn<F: Fn() -> i32>(f: F) {
    println!("Fn called: {}", f());
}

fn call_fn_mut<F: FnMut() -> i32>(mut f: F) {
    println!("FnMut called: {}", f());
}

fn main() {
    let mut x = 3;
    // this closure now implements FnMut trait
    let mut closure = || {
        x *= 2;     // value mutated here
        x
    };
    
    println!("{}", closure());
    println!("{}", closure());
    
    // This call now fails because FnMut cannot be passed for Fn
    // call_fn(&mut closure);   // This call now fails because FnMut cannot be passed for Fn
    call_fn_mut(&mut closure);  // This works as closure is an FnMut
}
```

## Closures as Arguments
While passing closures to functions, we have the below general options:

1. Using function pointers
`closures` which do  not capture variables from its  surrounding environment can
be coerced to `fn` function pointer types. This indicates the `closure` does not
hold any state and can be represented as a simple function pointer.

An example:
```rust
// apply takes a closure as one of its argument
fn apply(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg)
}

// main runs without any problem with output = 25
fn main() {
    // This closure is not consuming any surrounding environment
    // variables type of this closure is fn(i32) -> i32
    // We are opting for function pointer type fn here
    let multiply = |x| x * x;
    
    let result = apply(multiply, 5);
    
    println!("{}", result);
}
```

If we modify the program to make closure consume environment, it fails with an error:
```rust
// apply takes a closure as one of its argument
fn apply(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg)
}

fn main() {
    let y = 6;
    // This closure is now immutably using the environment
    let multiply = |x| x * y;
    
    let result = apply(multiply, 5);
    
    println!("{}", result);
}
```

Error information:

```sh
error[E0308]: mismatched types
  --> src/main.rs:11:24
   |
 9 |     let multiply = |x| x * y;
   |                    --- the found closure
10 |     
11 |     let result = apply(multiply, 5);
   |                  ----- ^^^^^^^^ expected fn pointer, found closure
   |                  |
   |                  arguments to this function are incorrect
   |
   = note: expected fn pointer `fn(i32) -> i32`
                 found closure `{closure@src/main.rs:9:20: 9:23}`
note: closures can only be coerced to `fn` types if they do not capture any variables
  --> src/main.rs:9:28
   |
 9 |     let multiply = |x| x * y;
   |                            ^ `y` captured here
note: function defined here
  --> src/main.rs:2:4
   |
 2 | fn apply(f: fn(i32) -> i32, arg: i32) -> i32 {
   |    ^^^^^ -----------------
```

2. Using generic functions with trait bounds
This method is used when the closure does capture variables from its scope. Such
closures implement one of the `Fn`, `FnMut` or `FnOnce` traits, depending on how
they use the values they capture.

Example with closure implementing `Fn` and consuming variables:
```rust
// apply takes a closure as one of its argument
fn apply<F>(f: F, arg: i32) -> i32
where
    F: Fn(i32) -> i32,
{
    f(arg)
}

// It returns 30
fn main() {
    let y = 6;
    // This closure is now immutably using the environment
    let multiply = |x| x * y;   // implement the fn trait

    let result = apply(multiply, 5);

    println!("{}", result);
}
```

Example with closure implementing `FnMul` and consuming variables:
```rust
// Trait hierarchy
// Fn -> FnMut -> FnOnce
// apply takes a closure as one of its argument
// If f is of type FnMut then F can have both FnMut as well as FnOnce
//
// So both these are valid with caveats
// F: FnMut(i32) -> i32     - can be called multiple times
// fn apply<F>(mut f: F, arg: i32) -> i32
// where
//     F: FnMut(i32) -> i32,
//
// F: FnOnce(i32) -> i32    - can only be called once and f cannot be mut
// fn apply<F>(f: F, arg: i32) -> i32
// where
//     F: FnMut(i32) -> i32,
fn apply<F>(mut f: F, arg: i32) -> i32
where
    F: FnMut(i32) -> i32,
{
    // since the closure is actually modifies environment, f should be mut
    f(arg); // If FnMut can be called multiple times
    f(arg)  // If FnOnce only called once
}

fn main() {
    let mut y = 6;
    // This closure is now mutably borrows y from the environment
    // and modifies it. So it implements the FnMut
    let multiply = |x| {
        println!("invoking multiply");
        y += x;
        y
    };

    let result = apply(multiply, 5);

    println!("{}", result);
}
```

output:
```bash
invoking multiply
invoking multiply
16
```


Modifying the above example with closure implementing `FnOnce` and consuming variables:
```rust
// Trait hierarchy
// Fn -> FnMut -> FnOnce
// apply takes a closure as one of its argument
// If f is of type FnMut then F can have both FnMut as well as FnOnce
//
// So both these are valid with caveats
// F: FnMut(i32) -> i32     - can be called multiple times
// fn apply<F>(mut f: F, arg: i32) -> i32
// where
//     F: FnMut(i32) -> i32,
//
// F: FnOnce(i32) -> i32    - can only be called once and f cannot be mut
// fn apply<F>(f: F, arg: i32) -> i32
// where
//     F: FnMut(i32) -> i32,
fn apply<F>(f: F, arg: i32) -> i32
where
    F: FnOnce(i32) -> i32,
{
    // f cannot be mut now as it is immutably consuming environment
    f(arg)  // If FnOnce only called once
}

fn main() {
    let mut y = 6;
    // This closure is now mutably borrows y from the environment
    // and modifies it. So it implements the FnMut
    let multiply = |x| {
        println!("invoking multiply");
        y += x;
        y
    };

    let result = apply(multiply, 5);

    println!("{}", result);
}
```

Output:
```bash
invoking multiply
11
```

## Passing closures to structs
Passing closures as struct fields have the below advantages:

+ Event handling
+ Customizable behaviour
+ Iterator adaptor
+ Deferred execution

The ways to store a closure into a struct field are:
1. As Trait Object
```rust
// Use smart pointers like Box with dyn
Box<dyn Fn()>
Box<dyn FnMut()>
Box<dyn FnOnce()>
```

The `dyn` before the trait means that we want to use it as a trait object allowing dynamic dispatch for any type that implements the trait. The `dyn` trait is `DST` (*Dynamically Sized Type*) in `Rust`. Hence its size is not known during compilation time.

An example for this would be:
```rust
struct MyStruct {
    val: i32,
    action: Box<dyn Fn(i32) -> i32>, // this is a trait object
    // action: Box<dyn FnOnce(i32) -> i32>, // this is also correct due to hierarchy
}

// output would be 88
fn main() {
    let y = 8;  // this will be on stack
    let closure = Box::new(move |x| x * y); // consuming the environment immutably
    
    let ms = MyStruct {
        val: 89,
        action: closure,
    };
    println!("{:?}", (ms.action)(11));
}
```

2. As Generic struct
Using generics with trait bounds is another way of storing closures in struct fields. Unlike previous method where, the `Box<dyn Trait>` was used, this method uses `dynamic dispatch` which might be faster but makes the struct itself generic.

## Additional pointers on closures
One important point to remember about closures is that "*No two closures, even if identical, have the same type*".
The below code demonstrates exactly that:

```rust
fn main() {
    let y = 9;
    
    // below closures are capturing environment
    let c1 = |x| x + y;     // identical closure
    let c2 = |x| x + y;     // identical closure
    
    let vec_of_closures = vec![c1, c2];
    println!("{}", vec_of_closures[0](5));
}
```
The aboce code willr eturn below error:
```sh
error[E0308]: mismatched types
  --> src/main.rs:10:36
   |
 4 |     let c1 = |x| x + y;
   |              --- the expected closure
 5 |     let c2 = |x| x + y;
   |              --- the found closure
...
10 |     let vec_of_closures = vec![c1, c2];
   |                                    ^^ expected closure, found a different closure
   |
   = note: expected closure `{closure@src/main.rs:4:14: 4:17}`
              found closure `{closure@src/main.rs:5:14: 5:17}`
   = note: no two closures, even if identical, have the same type
   = help: consider boxing your closure and/or using it as a trait object
```

To make it work, we have to provide `Box` type for the closures as below:
```rust
// This code runs with output = 20

fn main() {
    let y = 9;
    
    // below closures are capturing environment
    let c1: Box<dyn Fn(i32) -> i32> = Box::new(|x| x + y);
    let c2: Box<dyn Fn(i32) -> i32> = Box::new(|x| x + y);
    
    let vec_of_closures = vec![c1, c2];
    println!("{}", vec_of_closures[0](11));
}
```

# Smart Pointers


# Testing
Below are the two general testing processes followed as a part of the software development practices:

1. Test Driven Development (`TDD`)
2. Test Last Development (`TLD`)

`TDD` follows the below process:

A. Write a test case that fails
B. Make the code work (Write just enough code to make it pass the test)
C. Refactor to Eliminate redundancy

The steps from `A` through `C` will repeat.

`TLD` follows the process in which tests are written fter the code is developed for a feature.


# MACROS
`Macros` are one of the most powerful features of `Rust` which enable metaprogramming (code that writes code). Unlike languages like `C` and `C++`, where `macros` are simple text substitution mechanisms, `Rust’s` macros are hygienic and operate on the **Abstract Syntax Tree** (`AST`), making them both powerful and safe. They allow for extending the language, reducing boilerplate, creating domain-specific languages, and implement compile-time code generation without sacrificing `Rust’s` safety guarantees.

When it is mentioned that `Rust` macro's are hygenic, it means that the variables inside the `macro` do not accidentally clash with variables outside. Each expansion gets its own scope for locally defined names.

`macros` are a way to write code that writes other code and `Rust` has two main types of macros:

>1. Declarative macros (also called `macro_rules!`)
>2. Procedural macros, which has the following three forms:
>   a. Function-Like macros
>   b. Derive macros
>   c. Attribute macros

## Declarative Macros with macro_rules!
Declarative macros use pattern matching to transform code and they are defined using the `macro_rules!` syntax as below:
```rust
// syntax to declare macros in rust
macro_rules! a_macro {
	( ) => { };
//	^^^    ^^^
//	 |      └── body of the rule (transcriber)
//	 └── the parameters of the rule (matcher)
}


macro_rules! $name {
    $rule0 ;
    $rule1 ;
    // …
    $ruleN ;
}
```
There must be at least one rule, the semicolon after the last rule may be omitted. We can use brackets(`[]`), parentheses(`()`) or braces(`{}`).

Each rule will have the following form:
```rust
    ($matcher) => {$expansion}
```
The `expansion` part is also called `transcriber`. When  `macro_rules!` macro is invoked, the interpreter checks the rules one by one, in declaration order and for each rule, it tries to match the contents of the input token tree against that rule’s _matcher_. A matcher must match the entirety of the input to be considered a _match_.

If the input matches the _matcher_, the invocation is replaced by the expansion; otherwise, the next rule is tried. If all rules fail to match, the expansion fails with an `error`.

`Matchers` can also contain `captures`, which allow input to be matched based on some general grammar rules, with the result captured to a metavariable which can then be substituted into the output. `Captures` are written as a dollar (`$`) followed by an identifier, a colon (`:`), and finally the kind of capture which is also called the `fragment-specifier`, which must be one of the following:

> [!NOTE]
> `expr`: An expression (e.g., `1 + 2`, `foo()`, `bar.baz`, `vec![1,2,3]`)
> `ident`: An identifier (e.g., `variable names`, `function names`)
> `literal`: A literal value (e.g., `42`, `"hello"`, `true`)

> `ty`: A type (e.g., `i32`, `String`, `Vec<T>`)
> `pat`: A pattern (e.g., `Some(x)`, `_`, `1..=9`)
> `path`: A path (e.g., `std::collections::HashMap`)
> `stmt`: A statement (e.g., `let x = 1;`)
> `block`: A block of code (e.g., ``{ println!("hello"); }``)
> `item`: An item (e.g., `functions`, `structs`, `modules`)
> `meta`: Meta information (e.g., `attributes`)
> `tt`: A single token tree. (e.g., Any token or `(...)`, `[...]`, `{...}`)

## Repetitions
`Matchers` can contain `repetitions`, which allow a sequence of tokens to be matched. These have the general form
+ `$`
+ `( ... )`
+ `sep`
+ `rep` - this is the required repeat operator which can have below:
    - `?`: indicating at most one repetition
    - `*`: indicating zero or more repetitions
    - `+`: indicating one or more repetitions

>[!IMPORTANT]
> `?` represents at most one occurrence, it cannot be used with a separator.
> `$(...)*` repetition syntax handles variable numbers of arguments
> `$(,)?`   handles trailing commas

An example:
```rust
macro_rules! my_vec {
    (
        // start a repetition:
        $(
            // each repetition must contain an expression...
            $element:expr
        )
        // ...separated by commas... or semicolons
        ,
        // ...zero or more times `? + *`
        *
        $(,)?
    ) => {
        // enclose the expansion in a block so that we can use
        // multiple statements.
        {
            let mut tempv = Vec::new();
            // start a repetition:
            $(
                // sach repeat will contain the following statement, with
                // $element replaced with the corresponding expression.
                temp.push($element);
            )*

            tempv
        }
    };
}

fn main() {
    let numbers = my_vec![1, 2, 3, 4, 5];
    let words = my_vec!["quick", "brown", "fox", "jumps"];
    let with_trailing = my_vec![100, 99, 98,]; // trailing comma is fine

    println!("Numbers: {:?}", numbers);
    println!("Words: {:?}", words);
    println!("With trailing comma: {:?}", with_trailing);
}
```

## Procedural Macros
While the `macro_rules!` cover most of the use cases, for more complex and advanced use cases there are _procedural macros_ available. These are separate crates that receive `Rust` code as input and produce new code as output. They power some of the popular libraries like `Serde` and `Tokio`.

```text
       
       Declarative Macros          Procedural Macros
       (macro_rules!)              (proc-macro crates)
            |                            |
            |                            |
       Pattern matching           Parse full AST
       Limited flexibility        Full Rust code
       Simpler to write          More powerful
            |                            |
            └──────── Use Cases ─────────┘
                          |
                  ┌───────┴────────┐
                  |                |
              Simple patterns   Derive macros
              Code generation   Attributes
              DSLs              Complex transforms
       
```

For debugging the `macros` we can use certain `cargo` commands as below:

```bash
cargo install cargo-expand
cargo expand
cargo expand --bin binary_name main

cargo check
```


# Appendix
Here, some of the `Rust` ecosystems commands and their were listed:

## Version checks
To verify that the Rust compiler (`rustc`) and Rust package manager (`cargo`) are installed:
```bash
λ rustc --version
rustc 1.83.0 (90b35a623 2024-11-26)

λ cargo --version
cargo 1.83.0 (5ffbef321 2024-10-29)
```

## View the installed and active toolchains
We can view the different toolchains that were installed and which one is the active one using the following command:
```bash
λ rustup show
Default host: aarch64-apple-darwin
rustup home:  /Users/sampathsingamsetty/.rustup

stable-aarch64-apple-darwin (default)
rustc 1.83.0 (90b35a623 2024-11-26)
```

## To switch between the toolchains
We can view the default toolchain and change it as needed. In case if we’re currently on a stable toolchain and wish to try out a newly introduced feature that is available in the nightly version we may easily switch to the nightly toolchain using the below commands:
```shell
λ rustup default
λ rustup default nightly
```

To see the exact path of the compiler and package manager of Rust:
```shell
λ rustup which rustc
/Users/sampathsingamsetty/.rustup/toolchains/stable-aarch64-apple-darwin/bin/rustc

λ rustup which cargo
/Users/sampathsingamsetty/.rustup/toolchains/stable-aarch64-apple-darwin/bin/cargo
```

## Checking and Updating the toolchain
To check whether a new Rust toolchain is available:
```sh
λ rustup check
stable-aarch64-apple-darwin - Up to date : 1.83.0 (90b35a623 2024-11-26)
rustup - Up to date : 1.27.1
```

If a new version of Rust is released with some interesting features, and we would like to get the latest version of Rust, then we can do so with the update subcommand:
```sh
λ rustup update
```

## Accessing help and documentation
`rustup` tool has a variety of commands which can be used to refer to the help section for additional details:
```bash
λ rustup --help
rustup 1.27.1 (54dd3d00f 2024-04-24)

The Rust toolchain installer

Usage: rustup [OPTIONS] [+toolchain] [COMMAND]

Commands:
  show         Show the active and installed toolchains or profiles
  update       Update Rust toolchains and rustup
  check        Check for updates to Rust toolchains and rustup
  default      Set the default toolchain
  toolchain    Modify or query the installed toolchains
  target       Modify a toolchain's supported targets
  component    Modify a toolchain's installed components
  override     Modify toolchain overrides for directories
  run          Run a command with an environment configured for a given toolchain
  which        Display which binary will be run for a given command
  doc          Open the documentation for the current toolchain
  man          View the man page for a given command
  self         Modify the rustup installation
  set          Alter rustup settings
  completions  Generate tab-completion scripts for your shell
  help         Print this message or the help of the given subcommand(s)

Arguments:
  [+toolchain]  release channel (e.g. +stable) or custom toolchain to set override

Options:
  -v, --verbose  Enable verbose output
  -q, --quiet    Disable progress output
  -h, --help     Print help
  -V, --version  Print version

Discussion:
    Rustup installs The Rust Programming Language from the official
    release channels, enabling you to easily switch between stable,
    beta, and nightly compilers and keep them updated. It makes
    cross-compiling simpler with binary builds of the standard library
    for common platforms.

    If you are new to Rust consider running `rustup doc --book` to
    learn Rust.
```

Local documentation of any standard library or feature may be readily accessible using below:
```bash
λ rustup doc
λ rustup doc --book
λ rustup doc --std
λ rustup doc --cargo
```

Additional details are available at the online link [The rustup book][The rustup book]

The `cargo` command line utility has few useful options to introspect the code:

```bash
cargo check

cargo expand
```

# Resources
Some interesting links for `Rust`

[15 Rust Projects]: https://codecrafters.io/blog/rust-projects
[Rust Cheat Sheet]: https://cheats.rs
[RustLabs Quickstart]: https://rustlabs.github.io/docs/rust101
[Let's Get Rust]: https://learn.letsgetrusty.com
[src letsgetrusty]: https://github.com/letsgetrusty/bootcamp
[RustJobs]: https://rustjobs.dev/blog/
[Roadmap]: https://roadmap.sh/
[ActixWeb sample]: https://codevoweb.com/build-a-simple-api-with-rust-and-actix-web/
[Rust for safety critical systems]: https://arxiv.org/html/2405.18135v1
[The rustup book]: https://rust-lang.github.io/rustup/
[Interview Questions]: https://macronepal.com/category/rust/


# References
[1]: https://doc.rust-lang.org/std/primitive.bool.html
[2]: https://rustwiki.org/en/
