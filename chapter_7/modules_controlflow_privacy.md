[<< Packages and Crates ](./packages_and_crates.md) | [ Paths for Referring to an Item in the Module Tree >>](./paths_in_module_tree.md)
# Defining Modules to Control Scope and Privacy

* In this section, we’ll talk about `modules` and other parts of the module system, 
  * namely `paths` that allow you to name items; 
  * `use` keyword that brings a path into scope;
  * `pub` keyword to make items public;
  * `as` keyword
  * external packages
  * `glob` operator
  
  
## Modules Cheat Sheet

#### ----------------------------
### Start with the root
* When compiling a crate, the compiler first looks in the crate root file
* (usually src/lib.rs for a library crate or src/main.rs for a binary crate)

#### ----------------------------
### Declaring modules
* In the crate root file, you can declare new modules: `mod garden;`
* The compiler will look for the module’s code in these places: 
  * Inline, within curly brackets that replace the semicolon following mod garden
  * In the file src/garden.rs
  * In the file src/garden/mod.rs

#### ----------------------------
### Declaring submodules
* In any file other than the crate root, you can declare submodules.
* For example, you might declare mod vegetables; in src/garden.rs.
* The compiler will look for the submodule’s code within the directory named for the parent module in these places: 
  * Inline, directly following `mod vegetables`, within curly brackets instead of the semicolon
  * In the file src/garden/vegetables.rs
  * In the file src/garden/vegetables/mod.rs
  
  
#### ----------------------------
### Declaring submodules
* Once a module is part of your crate, you can refer to code in that module from anywhere else in that same crate
* For example:
  *  an `Asparagus` type in the garden vegetables module would be found
    * at `crate::garden::vegetables::Asparagus` (or `garden::vegetables::Asparangus` inside of crate)
    
    
#### ----------------------------
### Private vs public:
* Code within a module is private from its parent modules by default
* To make a module public, declare it with `pub mod` instead of `mod`.
* To make items within a public module public as well, use pub before their declarations


    
#### ----------------------------
### The use keyword:
* Within a scope, the use keyword creates shortcuts to items to reduce repetition of long paths.
* In other words: it brings the desired module/path into a current scope
> In any scope that can refer to crate::garden::vegetables::Asparagus, you can create a shortcut with use crate::garden::vegetables::Asparagus; and from then on you only need to write Asparagus to make use of that type in the scope.



## Project architecture example
```plain
backyard
├── Cargo.lock
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs
    ├── garden.rs
    └── main.rs
```

##### Filename: src/main.rs
```rust
use crate::garden::vegetables::Asparagus;

pub mod garden;

fn main() {
    let plant = Asparagus {};
    println!("I'm growing {:?}!", plant);
}
```
* The `pub mod garden;` line tells the compiler to include the code it finds in `src/garden.rs`, which is:


##### Filename: src/garden.rs
```rust
pub mod vegetables;
```
* Here, pub mod vegetables; means the code in src/garden/vegetables.rs is included too. 

##### Filename: src/garden/vegetables.rs
```rust
#[derive(Debug)]
pub struct Asparagus {}
```


## Grouping Related Code in Modules
* Modules let us organize code
* Modules also allow us to control the privacy of items,

### !!!! IMPORTANT 
* because code within a module is private by default


### Example
* In the restaurant industry, some parts of a restaurant are referred to as:
  - `front of house`
  - others as `back of house`


Create a new library named restaurant by running `cargo new restaurant --lib;`

###### Filename: src/lib.rs 
```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}

        fn seat_at_table() {}
    }

    mod serving {
        fn take_order() {}

        fn serve_order() {}

        fn take_payment() {}
    }
}
```

* Earlier, we mentioned that src/main.rs and src/lib.rs are called crate roots. 
* The reason for their name is that:
* the contents of either of these two files form a module named crate at the root of the crate’s module structure,
* known as the `module tree`.

```plain
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```
* Notice that the entire module tree is rooted under the implicit module named crate.

