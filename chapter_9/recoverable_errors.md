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


## Matching on Different Errors`
* The type of the value that `File::open` returns inside the `Err` variant is `io::Error`


## Alternatives to Using match with Result\<T, E\>
```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let greeting_file = File::open("hello.txt").unwrap_or_else(|error| {
        if error.kind() == ErrorKind::NotFound {
            File::create("hello.txt").unwrap_or_else(|error| {
                panic!("Problem creating the file: {:?}", error);
            })
        } else {
            panic!("Problem opening the file: {:?}", error);
        }
    });
}
```

## Shortcuts for Panic on Error: unwrap and expect
* Using match works well enough, but it can be a bit verbose 
* The `unwrap` method is a shortcut method implemented just like the `match`

###### --------------------
* If the `Result` value is the `Ok` variant, `unwrap` will return the value inside the `Ok`
* If the `Result` is the `Err` variant, `unwrap` will call the `panic!`

```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt").unwrap();
}
```

###### ---------------------
* Similarly, the `expect` method lets us also choose the `panic!` error **message**
* We use expect in the same way as unwrap: to return the file handle or call the panic! macro. 
* In production-quality code, most Rustaceans choose expect rather 
* and give more context about why the operation is expected to always succeed

```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt")
        .expect("hello.txt should be included in this project");
}
```


## Propagating Errors
* you can return the error to the calling code so that it can decide what to do. 
* This is known as `propagating the error` 
* gives more control to the calling code


### A Shortcut for Propagating Errors: the ? Operator
* The `?` placed after a `Result` value is defined to work as follows: 
  * If the value of the `Result` is an `Ok`, the value inside the Ok will get returned **from this expression**
  * If the value is an Err, the Err will be returned **from the whole function**

#### --------------------
* error values that have the `?` operator called on them go through the `from` function, 
* defined in the `From` trait in the standard library, which is used to convert values from one type into error type defined in the return type of the current function

#### ---------------------
* We could even shorten this code further by chaining method calls immediately after the ?

```rust
use std::fs::File;
use std::io::{self, Read};

fn main() {
  let result = read_username_from_file();
  
  println!("The result is: {res:?}", res=result);
  
  fn read_username_from_file() -> Result<String, io::Error> {
    let mut username = String::new();
    
    File::open("uname.txt")?.read_to_string(&mut username)?;
    Ok(username);
  }
}
```

#####  a way to make this even shorter using fs::read_to_string
##### ---------------------------

```rust
use std::fs;
use std::io;

fn read_username_from_file() -> Result<String, io::Error> {
  fs::read_to_string("hello.txt")
}
```

## Where The ? Operator Can Be Used
* we’re only allowed to use the `?` operator in a function
* that returns `Result`, `Option`, or another type that implements `FromResidual`

###### ---------------
* To fix the error, you have two choices
  * change the return type of your function to be compatible with the value you’re using the ?
  * use a match or one of the Result\<T, E\> methods to handle the Result\<T, E\>


##### ------------------
* example of a function that finds the last character of the first line in 

```rust
fn last_char_of_first_line(text: &str) -> Option<char> {
  text.lines().next()?.chars().last()
}
```
* This function returns `Option\<char\>` because it’s possible that there is a character there
  * `lines()` returns an iterator over the lines in the string
  * Because this function wants to examine the first line, it calls `next` on the iterator
  * The ? extracts the string slice
  * we can call `chars` on that string slice to get an iterator of its characters.
  * we call `last` to return the last item in the iterator.


###### --------------------
* Luckily, main can also return a `Result\<(), E\>`. 

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```
* The Box\<dyn Error\> type is a **trait object**,
* you can read Box<dyn Error> to mean “any kind of error.” 

  * The main function may return any types that implement the `std::process::Termination` trait, 
