# Control Flow

# IF expressions
* the condition in this code must be a bool
* If the condition isn’t a bool, we’ll get an error

```rust
fn main() {
    let number = 3;

    if number {
        println!("number was three");
    }
}
```

## Using if in a let Statement
* Because if is an expression,
* Remember that blocks of code evaluate to the last expression in them,
```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```



* the values that have the potential to be results
* from each arm of the if must be the same type
```rust
fn main() {
    let condition = true;

    let number = if condition { 5 } else { "six" };

    println!("The value of number is: {number}");
}
```

> The expression in the if block evaluates to an integer,
> and the expression in the else block evaluates to a string. 
> This won’t work because variables must have a single type, 
> and Rust needs to know at compile time what type the number variable is, definitively. 
> Knowing the type of number lets the compiler verify the type is valid everywhere we use number.
> Rust wouldn’t be able to do that if the type of number was only determined at runtime;
> the compiler would be more complex and would make fewer guarantees about the code 
> if it had to keep track of multiple hypothetical types for any variable.


## Repetition with Loops
* Rust has three kinds of loops: `loop`, `while`, and `for`.

### Repeating Code with loop
* The loop keyword tells Rust to execute a block of code forever
* or until you explicitly tell it to stop.

```rust
fn main() {
    loop {
        println!("again!");
    }
}
```

### Returning Values from Loops
* One of the uses of a loop is to retry an operation you know might fail,
* such as checking whether a thread has completed its job
* To do this, you can add the value you want returned after the`break` expression 

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}");
}
```

### Loop Labels to Disambiguate Between Multiple Loops
* Loop labels must `begin with a single quote`. 

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```


### Conditional Loops with while

```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```

### Looping Through a Collection with for

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}
```

```rust
fn main() {
    for number in (1..4).rev() {
        println!("{number}!");
    }
    println!("LIFTOFF!!!");
}
```
