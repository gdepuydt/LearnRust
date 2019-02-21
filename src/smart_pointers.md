# Smart Pointers

The most common smart pointers in the standard library:

 >  - `Box<T>` for allocating values on the heap
 >  - `Rc<T>`, a reference counting type that enables multiple ownership
 >  - `Ref<T>` and `RefMut<T>`, accessed through `RefCell<T>`, a type that enforces the borrowing rules at runtime instead of compile time


 `Box<T>` is to point to data on the heap.

syntax:
```rust
fn main() {
    let b = Box::new(5);
    println!("b = {}", b);
}
```

## The `deref`trait

Implementing the Deref trait allows you to customize the behavior of the dereference operator, `*`.

*Deref coercion* simplifies dereferencing, at compile time.
**Deref coercion converts a reference to a type that implements Deref into a reference to a type that Deref can convert the original type into.**
Let that sentence sink in :)

Rust does deref coercion when it finds types and trait implementations in three cases:

    From &T to &U when T: Deref<Target=U>
    From &mut T to &mut U when T: DerefMut<Target=U>
    From &mut T to &U when T: Deref<Target=U>

## Running code on cleanup with the `Drop` trait

Customize what happens when a value is about to go out of scope. `Box<T>` customizes Drop to deallocate the space on the heap that the box points to.

Example:
```rust
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}

fn main() {
    let c = CustomSmartPointer { data: String::from("my stuff") };
    let d = CustomSmartPointer { data: String::from("other stuff") };
    println!("CustomSmartPointers created.");
}
```

You can force a value to be cleaned up early, by using the std::mem::drop function (which is in the prelude).

```rust
fn main() {
    let c = CustomSmartPointer { data: String::from("some data") };
    println!("CustomSmartPointer created.");
    drop(c);
    println!("CustomSmartPointer dropped before the end of main.");
}
```
