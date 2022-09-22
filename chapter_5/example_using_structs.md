[<< Defining and Instantiating Structs](./1_defining_and_instanciating_structs.md_) | [The Match Control Flow >>](./match_control_flow.md)

# An Example Program Using Structs
> To understand when we might want to use structs,
> 
>   let’s write a program that calculates the area of a rectangle.

#### ----------------------

```rust
fn main() {
    let width1 = 30;
    let height1 = 50;

    println!(
        "The area of the rectangle is {} square pixels.",
        area(width1, height1)
    );
}

fn area(width: u32, height: u32) -> u32 {
    width * height
}
```

* the function we wrote has two parameters
  - and **it’s not clear** anywhere in our program
  - that the parameters are related


---
## Refactoring with Tuples
* In one way, this program is better.
  - Tuples let us add a bit of structure
* But in another way, this version is less clear:
  - tuples don’t name their elements

```rust
fn main() {
    let rect1 = (30, 50);

    println!(
        "The area of the rectangle is {} square pixels.",
        area(rect1)
    );
}

fn area(dimensions: (u32, u32)) -> u32 {
    dimensions.0 * dimensions.1
}
```

----
## Refactoring with Structs: Adding More Meaning
* Our `area` function is now defined with **one parameter**
  - which we’ve named `rectangle`
  - whose type is an **immutable borrow** of a struct `Rectangle` instance.
  - we want to **borrow** the struct rather than take **ownership** of it

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        area(&rect1)
    );
}

fn area(rectangle: &Rectangle) -> u32 {
    rectangle.width * rectangle.height
}
```


---
## Adding Useful Functionality with Derived Traits
* It’d be useful:
  - to be able to print an instance of Rectangle
  - while we’re debugging our program 
  - and see the values for all its fields

#### THIS WON"T WORK
```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("rect1 is {}", rect1);
}
```

* !!! HERE
  ```text
  error[E0277]: `Rectangle` doesn't implement `std::fmt::Display`
    = help: the trait `std::fmt::Display` is not implemented for `Rectangle`
    = note: in format strings you may be able to use `{:?}` (or {:#?} for pretty-print) instead

  ```

#### ------------------------
* by default, the **curly bracket**s
  - tell `println!` to use formatting known as `Display`
  - output intended for direct end user consumption
  - The primitive types we’ve seen so far implement Display

#### ------------------------
. Putting the specifier `:?` inside the **curly brackets**:
  - tells `println!` we want to use an output format called `Debug`

```rust
println!("rect1 is {:?}", rect1);
```

### **!!!** HERE
  - Rust does include functionality
  - to print out debugging information,
  - but we have to explicitly opt
  ```text
  error[E0277]: `Rectangle` doesn't implement `Debug`
  ```
  
 * Like this
 ```rust
  #[derive(Debug)]
  struct Rectangle {
      width: u32,
      height: u32,
  }

  fn main() {
      let rect1 = Rectangle {
          width: 30,
          height: 50,
      };

      println!("rect1 is {:?}", rect1);
  }

```


#### -------------------------
* When we have larger structs:
  - it’s useful to have output
  - that’s a bit easier to read;
  - in those cases, we can use **`{:#?}`**

Result:
```text
$ cargo run
   Compiling rectangles v0.1.0 (file:///projects/rectangles)
    Finished dev [unoptimized + debuginfo] target(s) in 0.48s
     Running `target/debug/rectangles`
rect1 is Rectangle {
    width: 30,
    height: 50,
}
```

* Another way to print out a value using the Debug format:
  - is to use the `dbg!` macro
  - which **takes ownershi**p of an expression
  - and returns ownership of the value
  - Calling the `dbg!` macro prints to the standard error console stream
  - (as opposed to println! that takes a reference)

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let scale = 2;
    let rect1 = Rectangle {
        width: dbg!(30 * scale),
        height: 50,
    };

    dbg!(&rect1);
}
```
