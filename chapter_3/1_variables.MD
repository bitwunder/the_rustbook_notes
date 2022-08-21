## Variables and Mutability
*  by default variables are **immutable**
*  When a variable is immutable,
    *  once a value is bound to a name, you can’t change that value
      ```rust
      fn main() {
          let x = 5;
          println!("The value of x is: {x}");

          // immutability error
          x = 6;
          println!("The value of x is: {x}");
      }
      ```
      
      
| ---

* If **one part (1)** of our code operates on the assumption
* that a value will _never change_
* and **another part (2)** of our code changes that value,
* it’s possible that the **first part (1)** of the code won’t do what it was designed to do.

* The cause of this kind of bug can be difficult to track down after the fact,
* especially when the second piece of code changes the value only sometimes. 


| ---

* Although variables are immutable by default,
* you **can make them `mutable`** by adding `mut` in front of the _variable name _
  ```rust
    fn main() {
      let mut x = 5;
      println!("The value of x is: {x}");
      x = 6;
      println!("The value of x is: {x}");
  }```
  
  
 ---
 ## Constants
 -   constants are values that **are bound to a name** and are **not allowed to change**

1) Yu are **not allowed** to use `mut` with constants
    * Constants aren’t just immutable by default—they’re always immutable
2) You declare constants using the `const` keyword instead of the `let` keyword
3) The type of the value **must be annotated**
4) Constants can be declared in any scope
5) Constants may be set only **to a constant expression**
    * not the result of a value that could only be computed at runtime.
6) Constants are valid for the entire time a program runs, within the scope they were declared in

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

> Rust’s **naming convention** for constants
> 
> is to use **all uppercase** **with underscores** between words


---
## Shadowing
* Shadowing is different from marking a variable as `mut`
  * because we’ll get a compile-time error if we accidentally try to reassign
* Shadowing thus spares us from having to come up with different names
  * instead, we can reuse the name
* We can shadow a variable
  
  * by using the **same variable’s name**
  * and **repeating the use of the `let`** keyword
  ```rust
    fn main() {
      let x = 5; // x = 5

      let x = x + 1; // x =6

      {
          let x = x * 2; // x = 12
          println!("The value of x in the inner scope is: {x}");
      }

      println!("The value of x is: {x}"); // x = 6
  }
  ```
  
* if we try to use mut for this, as shown here, we’ll get a compile-time error:
```rust
    let mut spaces = "   ";
    spaces = spaces.len(); // different type
```
  
  
