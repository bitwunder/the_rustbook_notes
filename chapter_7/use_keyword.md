---
#### [<< Module Paths ](./paths_in_module_tree.md) | [ Separating Modules into Different Files >>](./separating_modules.md)

---

# Bringing Paths into Scope with the use Keyword
* Adding use and a path in a scope is similar to creating a symbolic link in the filesystem
* Note that use only creates the shortcut for the particular scope in which the use occurs

>  function body won’t compile:
```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

mod customer {
    pub fn eat_at_restaurant() {
        hosting::add_to_waitlist();
    }
}
```

* To fix this problem:
  * move the use within the customer module too
  * or reference the shortcut in the parent module with `super::hosting` within the child customer module.


## Creating Idiomatic use Paths
> you might have wondered why we specified use crate::front_of_house::hosting and then called hosting::add_to_waitlist in eat_at_restaurant rather than specifying the use path all the way out to the add_to_waitlist function to achieve the same result,
```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting::add_to_waitlist;

pub fn eat_at_restaurant() {
    add_to_waitlist();
}
```
* Bringing the function’s **parent module** into scope with use means we have to specify the parent module when calling the function. 
* Specifying the parent module when calling the function makes it clear that the function isn’t locally defined while still minimizing repetition of the full path.

#### -----------
* On the other hand, when bringing in `structs, enums, and other items with use`, it’s idiomatic to specify the full path. 
* There’s no strong reason behind this idiom: it’s just the convention that has emerged

```rust
use std::collections::HashMap;

fn main() {
    let mut map = HashMap::new();
    map.insert(1, 2);
}
```
#### -----------------------------
* The exception to this idiom is if we’re bringing **two items** with the same name into scope with use 

```rust
use std::fmt;
use std::io;

fn function1() -> fmt::Result {
    // --snip--
}

fn function2() -> io::Result<()> {
    // --snip--
}
```


## Providing New Names with the as Keyword
* after the path, we can specify as and a new local name, or alias, for the type. 
```rust
use std::fmt::Result;
use std::io::Result as IoResult;

fn function1() -> Result {
    // --snip--
}

fn function2() -> IoResult<()> {
    // --snip--
}
```

## Re-exporting Names with pub use
* When we bring a name into scope with the **`use`** keyword, the **name** available in the new scope **is private**.
* we can combine `pub` and `use`
*  This technique is called `re-exporting` 

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

## Using External Packages
* In Chapter 2, we programmed a guessing game project that used an external package called rand \
* To use rand in our project, we added this line to Cargo.toml:

```toml
rand = "0.8.3"
```

```rust
use rand::Rng;

fn main() {
    let secret_number = rand::thread_rng().gen_range(1..=100);
}

```

## Using Nested Paths to Clean Up Large use Lists
> If we’re using multiple items defined in the same crate or same module, listing each item on its own line can take up a lot of vertical space in our files.

```rust
// --snip--
use std::cmp::Ordering;
use std::io;
// --snip--
```

* Instead, we can use nested paths to bring the same items into scope in one line.
```rust


// --snip--
use std::{cmp::Ordering, io};
// --snip--
```

#### -----------
```rust
use std::io;
use std::io::Write;
```

to --->

```rust
use std::io::{self, Write};
```

## The Glob Operator
* If we want to bring all public items defined in a path into scope, we can specify that path followed by the `*` **glob** operator:

```rust
use std::collections::*;
```
