# Trait Objects

A trait object is a stand-in for any reference to a type that implements the trait.

Syntax examples:

```rust

pub trait Draw {
    fn draw(&self);
}

pub struct Screen {
    // A vector holding Box references to (Draw) trait objects
    pub components: Vec<Box<dyn Draw>>,
}

impl Screen {
    pub fn run(&self) {
        for component in self.components.iter() {
            component.draw();
        }
    }
}

// Using a generic type and trait bounds an implmeentation would look like this:

pub struct Screen<T: Draw> {
    pub components: Vec<T>,
}

impl<T> Screen<T>
    where T: Draw {
    pub fn run(&self) {
        for component in self.components.iter() {
            component.draw();
        }
    }
}

//Notice however that here `Screen` is restricted to only one type `T, whereas using trait objects does not impose this restriction!
```
