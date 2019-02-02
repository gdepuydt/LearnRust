# Using structs

Struct definition:

```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}
```

Create an instance of a struct:

```rust
let mut user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};

//change value of a struct instance
user1.email = String::from("anotheremail@example.com");

//Use a function to instanctiate structs
fn build_user(email: String, username: String) -> User {
    //This is an expression (instance value is returned)
    User {
        email,
        username,
        active: true,
        sign_in_count: 1,
    }
}
```
Struct update syntax:

```rust
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    //re-use the values of a previous struct instance
    ..user1
};
```

Tuple structs:
Basically tuples with a name.

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

Ownership of struct data:

```rust
struct User {
    username: &str,
    email: &str,
    sign_in_count: u64,
    active: bool,
}

fn main() {
    let user1 = User {
        email: "someone@example.com",
        username: "someusername123",
        active: true,
        sign_in_count: 1,
    };
}
```

The compiler will complain that it needs lifetime specifiers:

```
error[E0106]: missing lifetime specifier
 -->
  |
2 |     username: &str,
  |               ^ expected lifetime parameter

error[E0106]: missing lifetime specifier
 -->
  |
3 |     email: &str,
  |            ^ expected lifetime parameter
```

The &str isn't *owned* by the struct and this will result in a compiler error.

It’s possible for structs to store references to data owned by something else, but to do so requires the use of lifetimes, a Rust feature that we’ll discuss *later*.

## Derived traits for structs

```rust
#[derive(Debug)] // annotation to enable deriving debug information from the Rectange struct
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };

    println!("rect1 is {:?}", rect1); // :? = specifier for printing debug format string
    //using :#? will print prettier debug string formatting
}
```

## Method syntax

Methods are called within the context of a struct, enum or trait object. The first parameter is always `self` (representing the instance of the struct).

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

//start an implementation block
impl Rectangle {
    fn area(&self) -> u32 { //&mut self is struct instance must be mutable
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
``` 

Associated functions (static functions in other languages):

don't take `self` as first argument.

```rust
impl Rectangle {
    fn square(size: u32) -> Rectangle {
        Rectangle { width: size, height: size }
    }
}

let sq = Rectangle::square(3); //call static method
```





