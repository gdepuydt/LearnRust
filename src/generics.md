# Generic Types, Traits, and Lifetimes

Let the code speak for itself:

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

impl<T, U> Point<T, U> {
    fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 5, y: 10.4 };
    let p2 = Point { x: "Hello", y: 'c'};

    let p3 = p1.mixup(p2);

    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
}
```

## Traits: Defining Shared Behavior

```rust
// Trait definition
pub trait Summary {
    fn summarize(&self) -> String;
    // or use a default implementation
    //    fn summarize(&self) -> String {
    //      String::from("(Read more...)")
    //     }

}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

// Trait implementation for NewsArticle
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

## Traits as arguments

```rust
pub fn notify(item: impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

which is syntax sugar for:

```rust
pub fn notify<T: Summary>(item: T) {
    println!("Breaking news! {}", item.summarize());
}
```

`Summary` is a trait bound for the generic type, ie. the type of the argument must implment a Summary trait or the code will fail to compile.

Specify multiple traits with `+`

```rust
pub fn notify(item: impl Summary + Display) {
```

or

```rust
pub fn notify<T: Summary + Display>(item: T) {
```

Rust has alternate syntax for specifying trait bounds inside a `where` clause after the function signature.
Useful when many trait bounds are used.

```rust
fn some_function<T: Display + Clone, U: Clone + Debug>(t: T, u: U) -> i32 { // ..
}

//using the where clause
fn some_function<T, U>(t: T, u: U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{
    // --code--
```

We can use the impl Trait syntax in return position as well, to return something that implements a trait:

```rust
fn returns_summarizable() -> impl Summary {
    //return a Tweet object, which indeed has the Summary trait 
    Tweet {
        username: String::from("horse_ebooks"),
        content: String::from("of course, as you probably already know, people"),
        reply: false,
        retweet: false,
    }
}
```

Trait bounds can also be used to conditionally implement methods:

```rust
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self {
            x,
            y,
        }
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

Conditionally implement traits for any type that implements another trait:

```rust
impl<T: Display> ToString for T {
    // --snip--
}
```