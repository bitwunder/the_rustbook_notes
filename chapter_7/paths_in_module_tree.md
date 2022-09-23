----
#### [<< Modules ](./modules_controlflow_privacy.md) | [ `Use` keyword >>](./use_keyword.md)

---

# Paths for Referring to an Item in the Module Tree
* A path can take two forms:
  * An absolute path:
    * is the full path starting from a crate root
    * for code from an external crate, the absolute path begins with the crate name
    * #### !!!! and for code from the current crate, it starts with the literal crate.
  * A relative path:
    * starts from the current module and uses self, super, or an identifier in the current module.

* Both absolute and relative paths are followed by one or more identifiers separated by double colons **(::).**


##### -------------------
* compilation will fail cause module members are private by default for parent
```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    // Absolute path
    crate::front_of_house::hosting::add_to_waitlist();

    // Relative path
    front_of_house::hosting::add_to_waitlist();
}
```
* The first time we call the add_to_waitlist function in eat_at_restaurant, we use an absolute path.


## Exposing Paths with the pub Keyword
* The `pub` keyword on a module only lets code in its ancestor modules refer to it, not access its inner code.
* we need to go further and choose to make one or more of the items within the module public as well.

## Starting Relative Paths with super
* We can construct relative paths that begin in the parent module, rather than the current module or the crate root
  * by using `super` at the start of the path.
  * This is like starting a filesystem path with the `..` syntax.
  * Using super allows us to reference an item that we know is in the parent module

```rust
fn deliver_order() {}

mod back_of_house {
    fn fix_incorrect_order() {
        cook_order();
        super::deliver_order();
    }

    fn cook_order() {}
}
```
> We think the `back_of_house` module and the `deliver_order` function are likely to stay in the same relationship to each other
> 
> and get moved together should we decide to reorganize the crate’s module tree.


## Making Structs and Enums Public
* We can also use pub to designate structs and enums as public, but there are a few details extra to the usage of pub with structs and enums

>  If we use pub before a struct definition, we make the struct public, but the struct’s fields will still be private.

```rust


mod back_of_house {
    pub struct Breakfast {
        pub toast: String,
        seasonal_fruit: String,
    }

    impl Breakfast {
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches"),
            }
        }
    }
}

pub fn eat_at_restaurant() {
    // Order a breakfast in the summer with Rye toast
    let mut meal = back_of_house::Breakfast::summer("Rye");
    // Change our mind about what bread we'd like
    meal.toast = String::from("Wheat");
    println!("I'd like {} toast please", meal.toast);

    // The next line won't compile if we uncomment it; we're not allowed
    // to see or modify the seasonal fruit that comes with the meal
    // meal.seasonal_fruit = String::from("blueberries");
}
```

* Also, note that because `back_of_house::Breakfast` has a private field,
* the struct needs to provide a public associated function that constructs an instance of `Breakfast`

> If Breakfast didn’t have such a function, we couldn’t create an instance of `Breakfast` in `eat_at_restaurant` 
> 
> because we couldn’t **set the value** of the **private `seasonal_fruit` field** in `eat_at_restaurant`.

#### -------------------------

* In contrast, if we make an **`enum`** public, all of its variants are then public. We only need the pub before the enum keyword

```rust
mod back_of_house {
    pub enum Appetizer {
        Soup,
        Salad,
    }
}

pub fn eat_at_restaurant() {
    let order1 = back_of_house::Appetizer::Soup;
    let order2 = back_of_house::Appetizer::Salad;
}
```
