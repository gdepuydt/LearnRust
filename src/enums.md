# enums

The Rust enums remind me of C unions. They probably are not the same, but the use case feels very similar to me.

See this [link](http://patshaughnessy.net/2018/3/15/how-rust-implements-tagged-unions) to read all about this.

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);

let loopback = IpAddr::V6(String::from("::1"));
```

or

```rust
struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}
```

Other example illustrating difference getween structs and enums:

enums:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

versus structs:

```rust

struct QuitMessage; // unit struct
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // tuple struct
struct ChangeColorMessage(i32, i32, i32); // tuple struct
```

Enums have abig advantage here, because we now can have a function that takes the Message enum as parameter.

```rust
fn getMessage(message: Message) {
    // --snip-- -> here you'd select based on the enum type and act accordingly
}
```

## The `match`Control Flow Operator

`match` provides `switch ... case` functionality 

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u32 {
    match coin {
        //match arms: <pattern> => <code>
        Coin::Penny => {
            println!("Lucky penny!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

Patterns can also bind values that can be used in the code of that arm.
Here this is illutrated with the [Option](https://doc.rust-lang.org/std/option/enum.Option.html) enum, part of the standard library.

```rust
pub enum Option<T> {
    None,
    Some(T),
}
```

`Option` had `None` and `Some` members, the `Some` arm can bind a value. This is the standard way of dealing with potential `null` values:


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

Note that matches in Rust are exhaustive: you **MUST** handle all possible cases.

## The `_` Placeholder

`_` match any value (the `else` clause basically)

```rust
let some_u8_value = 0u8;
match some_u8_value {
    1 => println!("one"),
    3 => println!("three"),
    5 => println!("five"),
    7 => println!("seven"),
    _ => (),
}
```

Prevents ous from having to branch on 250+ arms (0u8 type)!

`if let` when there's only one arm to deal with:

This is too verbose:

```rust
let some_u8_value = Some(8);
match some_u8_value {
    Some(3) => println!("three"),
    _ => (),
}
``` 

Instead you can write:

```rust
let some_u8_value = Some(8);
if let Some(3) = some_u8_value {
    println!("three!");
} else {
    println("nah!");
}
``` 

That's much better! Note that an `else` clause replaces the `_` syntax. 
