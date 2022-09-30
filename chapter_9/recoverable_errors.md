---
#### [<< Panic ](./panic.md) | [ To `panic!` or not to `panic!` >>](./panic_or_panic.md)

---


# Recoverable Errors with Result

* Most errors aren’t serious enough to require the program to stop entirely.
* Recall the Result enum is defined as having two variants, `Ok` and `Err`:
  ```rust
  enum Result\<T, E\> {
      Ok(T),
      Err(E),
  }
  ```
  * `T` represents the type of the value that will be returned in a **success** case within the `Ok` variant
  * `E` represents the type of the error that will be returned in a **failure** case within the `Err` variant

* Let's try to open a file
  ```rust
    use std::fs::File;

  fn main() {
      let greeting_file_result = File::open("hello.txt");
  }
  ```
  * The return type of `File::open` is a `Result\<T, E\>`
  * if success: `Result::Ok(std::fs::File)`
  * if failure: `Result::Err(std::io::Error)`


### Handling with `match` expression
```rust
use std::fs::File;

fn main() {
  let greeting_file_result = File::open("hello.txt");
  let greeting_file = match greeting_file_result {
    Ok(file) => file,
    Err(error) => panic!("Problem opening the file: {:?}", error),
  };
}
```

* Note that, like the Option enum, the Result enum and its variants have been brought into scope by the prelude:
* so we don’t need to specify `Result::` before the `Ok` and `Err`
