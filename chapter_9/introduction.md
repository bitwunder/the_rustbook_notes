---
#### [<< Chapter 7 - HashMaps ](../chapter_7/hashmaps.md) | [ Unrecoverable Errors with panic! >>](./panic.md)

---

# Error Handling

* In many cases, Rust requires you to acknowledge the possibility of an error and take some action before your code will compile. 
* Rust groups errors into two major categories:
  * recoverable 
  * unrecoverable errors.
  * > For a recoverable error, such as a file not found error, we most likely just want to report the problem to the user and retry the operation
  * > Unrecoverable errors are always symptoms of bugs, like trying to access a location beyond the end of an array, and so we want to immediately stop the program.

* Rust doesn’t have exceptions. Instead, it has:
* the type `Result\<T, E\>` for **recoverable errors**
* the `panic!` macro that stops execution when the program encounters an **unrecoverable error**.
