----
#### [<< `use` keyword ](./use_keyword.md) | [ Chapter 8 - Introduction >>](../chapter_8/introduction.md)

---

# Separating Modules into Different Files
* When modules get large, you might want to move their definitions to a separate file
* Note that you only need to load a file using a mod declaration once in your module tree
  * Once the compiler knows the file is part of the project, you can reffer it by path
  * In other words, mod is not an “include” operation that you may have seen in other programming languages.

###### Filename: src/lib.rs
```rust
mod front_of_house;

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```


###### Filename: src/front_of_house.rs
```rust
pub mod hosting {
    pub fn add_to_waitlist() {}
}
```


### -----------------

##### Filename: src/front_of_house.rs
```rust
pub mod hosting;
```

### -----------------
```rust
Filename: src/front_of_house/hosting.rs

pub fn add_to_waitlist() {}
```

> If we instead put hosting.rs in the src directory, the compiler would expect the hosting.rs code to be in a hosting module declared in the crate root, and not declared as a child of the front_of_house module.

## Alternate File Paths
* For a module named front_of_house declared in the crate root, the compiler will look for the module’s code in:

    - src/front_of_house.rs (what we covered)
    - src/front_of_house/mod.rs (older style, still supported path)

* For a module named hosting that is a submodule of front_of_house, the compiler will look for the module’s code in:

    - src/front_of_house/hosting.rs (what we covered)
    - src/front_of_house/hosting/mod.rs (older style, still supported path)

> The main downside to the style that uses files named mod.rs is that your project can end up with many files named mod.rs
> 
> which can get confusing when you have them open in your editor at the same time.
