---
#### [<< Introduction ](./introduction.md) | [ Recoverable Errrors with Result >>](./recoverable_errors.md)

---

# Unrecoverable Errors with panic!
* By default, these panics will print a failure message, unwind, clean up the stack, and quit.
* Via an environment variable, you can also have Rust display the call stack when a panic occurs to make it easier to track down


## Unwinding the Stack or Aborting in Response to a Panic
* By default, when a panic occurs, the program starts **unwinding**, which means:
  *  Rust walks back up the stack and cleans up the data from each function it encounters
* However, this walking back and cleanup is a lot of work
*  Rust, therefore, allows you to choose the alternative of:
  * **immediately aborting**, which ends the program **without cleaning up**.
* you can switch from **unwinding** to **aborting** upon a panic by adding:
  * `panic = 'abort'` to the appropriate `[profile]` sections in your `Cargo.toml` file
  * 
  ```toml
  [profile.release]
  panic = 'abort'
  ```
  
  
# Simple panic!
```rust
fn main() {
    panic!("crash and burn");
}
```

## Using a panic! Backtrace
* Let’s look at another example to see what it’s like when a panic! call comes from a library because of a bug

```rust
fn main() {
    let v = vec![1, 2, 3];

    v[99];
}
```

* **In C**, attempting to read beyond the end of a data structure is undefined behavior.
* You might get whatever is at the location in memory 
* This is called a `buffer overread` and can lead to security vulnerabilities:
  * if an attacker is able to manipulate the index in such a way
  * as to read data they shouldn’t be allowed  to that is stored after the data structure
* When it comes to Rust it will stop execution and refuse to continue


## Backtrace
* Backtraces in Rust work as they do in other languages:
  * the key to reading the backtrace is to start from the top
  * and read until you see files you wrote
* The **lines above** that spot are code that **your code has called**;
* the **lines below** are code that **called your code**.

##### ------------------

* Debug symbols are enabled by default when using cargo build or cargo run without the --release flag, as we have here.




# Words
unwind
