[<< An Example Program Using Structs](./example_using_structs.md_) | [Method Syntax >>](../chapter_6/introduction.md)
# Method Syntax
> Methods are similar to functions: we declare them with the `fn` keyword

* Unlike functions methods are defined:
  - within the context of a struct (or an enum or a trait object)
  - and their first parameter is always `self`


# Defining Methods
* we start an `impl` (**implementation**) block for `Rectangle`
* The `&self` is actually short for `self: &Self`
* Within an `impl` block
  - the type `Self` is an alias
  - for the type that the `impl` block is for.
* Rust lets you abbreviate this:
  - with **only** the name **`self`** in the first parameter spot

* Methods can:
  - take **ownership** of self
  - borrow self **immutably** as we’ve done here
  - borrow self **mutably**, just as they can any other parameter
  - .

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```

#### -------------------------
* !!! Note that we can choose:
  - to give a method the **same name** 
  - as one of the struct’s fields.
  - For example: we can define a method on `Rectangle` also named `width`:

```rust
impl Rectangle {
    fn width(&self) -> bool {
        self.width > 0
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    if rect1.width() {
        println!("The rectangle has a nonzero width; it is {}", rect1.width);
    }
}
```

* when we follow `rect1.width` with parentheses
  - Rust knows we mean the **method `width`**
* When we don’t use parentheses
  - Rust knows we mean the **field `width`**


#### -------------------------
> Often, but not always, when we give methods with the same name as a field
> 
> we want it to only return the value in the field and do nothing else.

> Methods like this are called `getters`
> 
> and Rust does not implement them automatically for struct fields as some other languages do.

> Getters are useful because you can make the field private
> 
> but the method public
> 
> and thus enable **read-only access** to that field as part of the type’s public API.


---
## Where’s the -> Operator?
* Rust doesn’t have an equivalent to the `->` operator;
* instead, Rust has a feature called:
  **automatic referencing and dereferencing**.
  
* Here’s how it works:
  > when you call a method with object.something(),
  > d
  > ust automatically adds in `&`, `&mut`, or `*`
  > f
  > so object matches the signature of the method

```rust

p1.distance(&p2);
(&p1).distance(&p2);
```

>  The fact that Rust makes borrowing implicit for method receivers 
>  
>  is a big part of making ownership ergonomic in practice.


---
## Methods with More Parameters

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }

    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```
 
---
## Associated Functions

* All functions defined within an impl block are called **associated functions**
* We can define associated functions that:
  - don’t have `self` as their first parameter (and thus are not methods)
  - because they don’t need an instance of the type to work with
* We’ve already used one function like this: the `String::from` function
* Associated functions that aren’t methods are often used for constructors
* These are often called new, but new isn’t a special 

* The `Self` keywords in the return type and in the body of the function:
  - are aliases for the type
  - that appears after the impl keyword
  - which in this case is `Rectangle`.
* To call this associated function, we use the `::` syntax
* This function is namespaced by the struct: the :: syntax:
  - is used for both associated functions
  - and namespaces created by modules.


```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}
```


---
## Multiple impl Blocks
* Each struct is allowed to have multiple impl blocks.

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

impl Rectangle {
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```
