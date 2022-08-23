# Functions

* Rust code uses `snake case` as the conventional style 
* for function and variable names,
* in which `all letters are lowercase` and underscores separate words
* Rust doesn’t care where you define your functions
* only that they’re defined somewhere in a scope that can be seen by the caller.

```rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```


## Parameters
* Technically, the `concrete values` are called `arguments`,
* but in casual conversation, people tend to use the words parameter and argument interchangeably


* In function signatures, you must declare the type of each parameter. 


## Statements and Expressions
* Function bodies are made up of a series of statements optionally ending in an expression
* Rust is an expression-based language
* Expressions do not include ending semicolons
  *  If you add a semicolon to the end of an expression, 
  * you turn it into a statement
  * and it will then `not return` a value
* Calling a function is an expression.
* Calling a macro is an expression

> `Statements` are instructions that perform some action and `do not return` a value
> `Expressions` evaluate to a resulting value

* Function definitions are also statements
* Therefore, you can’t assign a let statement to another variable
* you’ll get an error:
  ```rust
  fn main() {
      let x = (let y = 6);
  }
  ```


* A new scope block created with curly brackets is an expression
```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}");
}
```

* Note that the x + 1 line doesn’t have a semicolon at the end,


## Functions with Return Values
* We don’t name return values,
* but we must declare their type after an arrow (->)

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {x}");
}
```
