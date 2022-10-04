---
#### [<< Recoverable Errors ](./recoverable_errors.md) | [ Chapter 10 - iNTRODUCTION >>](../chapter_10/introduction.md)

---

# To panic! or Not to panic!
* In situations such as examples, prototype code, and tests, it’s more appropriate to write code that panics instead of returning a Result. 


## Examples, Prototype Code, and Tests
If a method call fails in a test, you’d want the whole test to fail, even if that method isn’t the functionality under test. Because panic! is how a test is marked as a failure, calling unwrap or expect is exactly what should happen.

## Cases in Which You Have More Information Than the Compiler
* having a hardcoded, valid string doesn’t change the return type of the parse method: we still get a Result value
* the compiler isn’t smart enough to see that this string is always a valid IP address
```rust
    use std::net::IpAddr;

    let home: IpAddr = "127.0.0.1"
        .parse()
        .expect("Hardcoded IP address should be valid");
```


## Guidelines for Error Handling
* a bad state is when some assumption, guarantee, contract, or invariant has been broken
* such as when invalid values, contradictory values, or missing values are passed to your code
* plus one or more of the following:
  * something that is unexpected, as opposed to something that will likely happen occasionally, like a user entering data in the wrong format.
  * Your code after this point needs to rely on not being in this bad state, rather than checking for the problem at every step.
  * There’s not a good way to encode this information in the types you use.
  
##### -------------------
* If someone calls your code and passes in values that don’t make sense, it’s best to return an error if you can so the user of the library can decide what they want to do in that case.
* However, in cases where continuing could be insecure or harmful, the best choice might be to call panic!
* Similarly, panic! is often appropriate if you’re calling external code that is out of your control and it returns an invalid state that you have no way of fixing.
* However, when failure is expected, it’s more appropriate to return a Result
  * Examples include a parser being given malformed data or an HTTP request returning a status that indicates you have hit a rate limit
  
##### -----------------
* Functions often have contracts: their behavior is only guaranteed if the inputs meet particular requirements.
* Panicking when the contract is violated makes sense because a contract violation always indicates a caller-side bug 
  * the calling programmers need to fix the code. 
  *  Contracts for a function, especially when a violation will cause a panic, should be explained in the API documentation for the function.
  
  
  
## Creating Custom Types for Validation
```rust
    loop {
        // --snip--

        let guess: i32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        if guess < 1 || guess > 100 {
            println!("The secret number will be between 1 and 100.");
            continue;
        }

        match guess.cmp(&secret_number) {
            // --snip--
    }
```

* Instead, we can make a **new type** and put the validations in a function
* to create an instance of the type rather than repeating the validations everywhere

```rust
pub struct Guess {
    value: i32,
}

impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {}.", value);
        }

        Guess { value }
    }

    pub fn value(&self) -> i32 {
        self.value
    }
}
```

### -------------------
Next, we implement a method named value that borrows self, doesn’t have any other parameters, and returns an i32. This kind of method is sometimes called a getter, because its purpose is to get some data from its fields and return it. This public method is necessary because the value field of the Guess struct is private. It’s important that the value field be private so code using the Guess struct is not allowed to set value directly: code outside the module must use the Guess::new function to create an instance of Guess, thereby ensuring there’s no way for a Guess to have a value that hasn’t been checked by the conditions in the Guess::new function.
