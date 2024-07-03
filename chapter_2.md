#### 1. Guess code 1
```rust
use std::io;


fn main()
{
    println!("Guess the number!");
    println!("Please input your guess:");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```


> By default, Rust has a set of items defined in the standard library 
> that it brings into the scope of every program.
> **This set** is called the **`prelude`**, and you can see everything in it in the standard library documentation.

- |


`rust
let mut guess = String::new();
`
We use the let statement to create the variable.

> In Rust, variables are **`immutable by default`**
> To make a variable **mutable**, we add **`mut`** before the variable name:

```rust
let apples = 5; // immutable
let mut bananas = 5; // mutable
```

- |

`String::new`, a function that returns a new instance of a **`String`**
> **`String`** is a string type provided by the standard library that **is a growable**, **UTF-8 encoded bit of text**

- |

> The `::` syntax in the ```::new``` indicates that **new** 
> is an **associated function** of the String type
> an **`associated function`** is a function that’s implemented **on a type**


-|
```rust

```
>  The full job of `read_line` is to take whatever the user types into standard input 
>  and append that into a string (**without overwriting its contents**)
>  The string argument needs to be **mutable** so the method can change the string’s content.

-|

> The `&` indicates that this argument **is a reference**
> which gives you a way to let multiple parts of your code access one piece of data 
> without needing to copy that data into memory multiple times

> **references are immutable** by default.
> Hence, you need to write `&mut guess` rather than `&guess` to make it mutable


-|

```rust
.expect("Failed to read line");
```
> As mentioned earlier, `read_line` puts whatever the user enters into the string we pass to it, 
> but it also returns a **`Result`** value
> Result **is an `enumeration`** often called an `enum`
> which is a type that can **be in one of** **multiple possible states**
> We call each possible state a **`variant`**.
> 
> The purpose of these `Result` types is to encode **error-handling** information.

> Result's variants are `Ok` and `Err`
> Values of the Result type, like values of any type, **have methods defined on them**.
> Result has an `expect` **method**
> 
> If this instance of Result is an Err value, 
> expect will cause the program to crash
> and display the message that you passed as an argument to expect
> If this instance of Result is an Ok value, 
> expect will take the return value that Ok is holding and
>  return **just that value** to you so you can use it.
> In this case, that value is the number of bytes in the user’s input.

> If you don’t call expect, the program will compile, but you’ll get a warning:

-|

```rust
println!("You guessed: {guess}");
```

This line prints the string that now contains the user’s input. The {} set of curly brackets is a placeholder

```rust
let x = 5;
let y = 10;

println!("x = {} and y = {}", x, y);
```

---

### Generating a Secret Number
> Rust **doesn’t** yet include random number functionality in its standard library.
> However, the Rust team does provide a `rand` crate with said functionality

> Remember that a **`crate` is a collection of Rust source code files**. 

> The project we’ve been building `is a binary crate`, which is an executable.
> 
> The rand crate `is a library crate`,
>  which contains code intended to be used in other programs

-|

> Before we can write code that uses rand, we need to modify the `Cargo.toml `
```toml
[dependencies]
rand = "0.8.3"
```

> In the Cargo.toml file, 
> everything that follows a header is part of that section 
> that continues until another section starts

> Cargo understands [**`Semantic Versioning`**](https://semver.org/) (sometimes called SemVer)

> The number `0.8.3` is actually **shorthand** for `^0.8.3`, 
> which means** any version** that is **at least** 0.8.3 **but below 0.9.0**.

-|

> Cargo fetches the latest versions of everything that dependency needs from the `registry`, 
> which is a copy of data from `Crates.io`. 
> Crates.io is where people in the Rust ecosystem
>  post their open source Rust projects for others to use.


### Updating a Crate to Get a New Version

> When you do want to update a crate, Cargo provides the command `update`
>


### Generating a Random Number

```rust
use std::io;
use rand::Rng;



fn main()
{
    println!("Guess the number!");
    println!("Please input your guess:");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```
> The `Rng trait` defines methods that random number generators implement

-|
> the `rand::thread_rng` function that gives us the particular random number generator 
> one that **is local to the current thread** of execution and seeded by the operating system


-|
>  The kind of **range expression** we’re using here 
>  takes the form **`start..=end`** 
>  and **is inclusive** on the lower and upper bounds, 
>  so we need to specify `1..=100` to request a number between 1 and 100.



### Comparing the Guess to the Secret Number
>  Note that this code won’t compile quite yet, as we will explain
>  
```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    // --snip--

    println!("You guessed: {guess}");

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```



---

#### Cargo fetures
> running the `cargo doc --open` command 
> will build documentation 
> provided **by all of your dependencies locally** and open it in your browser. 

-|
> bringing a type called `std::cmp::Ordering` into scope from the standard library.
> The `Ordering type` is another `enum` and has the variants `Less`, `Greater`, and `Equal`.

> The `cmp method` compares two values 
> and `can be called` on anything that can be compared

> Then it returns a variant of the `Ordering enum` we brought into scope

-|
> We use a `match` expression to decide what to do next
> based on which variant of `Ordering` was **returned** from the call to `cmp`
> with the values in `guess` and `secret_number`.

-|
> A `match expression` is made up of **`arm`**


-|
> Rust **has a strong**, **static**** type system**. 
> However, it also has **type inference**

> When we wrote `let mut guess = String::new()`,
> Rust was able **to infer** that guess should be a `String`

-|

> `i32`, a 32-bit number; 
> `u32`, an unsigned 32-bit number; 
> `i64`, a 64-bit number; 
> Rust defaults to an `i32`

-|

```rust
 // --snip--

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    let guess: u32 = guess.trim().parse().expect("Please type a number!");

    println!("You guessed: {guess}");

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
  ```

> Rust allows us to **`shadow`** the previous value of guess with a new one. 

> The `parse method` on `strings` converts a string to **another type**
> The colon (`:`) after guess tells Rust we’ll annotate the **variable’s type**
> 

-|

> Because it might fail, the `parse method` **returns a `Result` type**, 
> as much as the read_line method does

> If parse returns an `Err Result` variant because it couldn’t create a number from the string,
> the `expect` call will crash the game and print the message we give it. 

> If parse can **successfully** convert the string to a number, 
> it will return the `Ok variant` of `Result`, 
> and `expect` will return the number that we want from the Ok value.

### Allowing Multiple Guesses with Looping
> The **`loop`** keyword creates an infinite loop.

```rust
    // --snip--

    println!("The secret number is: {secret_number}");

    loop {
        println!("Please input your guess.");

        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => println!("You win!"),
        }
    }
}
```


### Quitting After a Correct Guess

> Let’s program the game to quit when the user wins by adding a` break statement`:

```rust
        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}```


### Handling Invalid Input

```rust
        // --snip--

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

        // --snip--
        ```

> We **switch** from an `expect call` to a `match expression`
> to move from crashing on an error to handling the error.

> Remember that `parse` **returns a `Result`** type 
> and `Result` **is `an enum`** that has the variants `Ok` and `Err`.


# Words
- insatiable


