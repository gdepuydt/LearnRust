# Packages and Crates


    - A crate is a binary or library.
    - The crate root is a source file that is used to know how to build a crate.
    - A package has a Cargo.toml that describes how to build one or more crates. At most one crate in a package can be a library.

## The module system

Modules let us organize code into groups.

```rust
mod sound {
    pub mod instrument {
        pub fn clarinet() {
            // Function body code goes here
        }
    }
}

fn main() {
    // Absolute path
    crate::sound::instrument::clarinet();

    // Relative path
    sound::instrument::clarinet();
}
```

You can start relative paths with the `super` keyword, this is the `../` in a filesystem.

```rust
mod sound {
    mod instrument {
        fn clarinet() {
            super::breathe_in();
        }
    }

    fn breathe_in() {
        // Function body code goes here
    }
}
```

note: pub structs in a module still has private fields by default, you need to pub all of them individually.
When you pub an enum, however, all its fields are also pub by default.

The ùse`keyword brings Paths into scope:

```rust
mod sound {
    pub mod instrument {
        pub fn clarinet() {
            // Function body code goes here
        }
    }
}

use crate::sound::instrument;
// use self::sound::instrument; with relative path you have to start it with `self`

fn main() {
    instrument::clarinet();
    instrument::clarinet();
    instrument::clarinet();
}
```

Rename types brought into scope:
`use std::io::Result as IoResult;`

Some scope tips&tricks:

```rust
use std::cmp::Ordering;
use std::io;
// ---snip---

//or:
use std::{cmp::Ordering, io};

////////////

use std::io;
use std::io::Write;

//or:

use std::io::{self, Write};

//Bring all public definitions into scope:

use std::collections::*;

```

## separating modules into different files

```rust
mod sound; // src/sound.rs

fn main() {
    // Absolute path
    crate::sound::instrument::clarinet();

    // Relative path
    sound::instrument::clarinet();
}
```

file src/sound/instrument.rs

The filetree must follows the logical module hierarchy.