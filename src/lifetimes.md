# Lifetimes

Every reference in Rust has a lifetime, which is the scope for which that reference is valid. We must annotate lifetimes when the lifetimes of references could be related in a few different ways. Lifetime annotations describe the relationships of the lifetimes of multiple references to each other without affecting the lifetimes.

## Lifetime annotation syntax

```rust
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

As with generic type parameters, we need to declare generic lifetime parameters inside angle brackets between the function name and the parameter list. The constraint we want to express in this signature is that all the references in the parameters and the return value must have the same lifetime. We’re not changing the lifetimes of any values passed in or returned, instead we’re specifying that the borrow checker should reject any values that don’t adhere to these constraints.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```


Similar for structs thatcontain references to other types:

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.')
        .next()
        .expect("Could not find a '.'");
    let i = ImportantExcerpt { part: first_sentence };
}
```
bb
## Lifetime elision rules

After writing a lot of Rust code, the Rust team found that Rust programmers were entering the same lifetime annotations over and over in particular situations. These situations were predictable and followed a few deterministic patterns. The developers programmed these patterns into the compiler’s code so the borrow checker could infer the lifetimes in these situations and wouldn’t need explicit annotations. The patterns programmed into Rust’s analysis of references are called the lifetime elision rules. These aren’t rules for programmers to follow; they’re a set of particular cases that the compiler will consider, and if your code fits these cases, you don’t need to write the lifetimes explicitly.

Rule 1: The first rule is that each parameter that is a reference gets its own lifetime parameter.

Rule 2: If there is exactly one input lifetime parameter, that lifetime is assigned to all output lifetime parameters.

Rule 3: If there are multiple input lifetime parameters, but one of them is &self or &mut self because this is a method, the lifetime of self is assigned to all output lifetime parameters.