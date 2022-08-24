# References and Borrowing

* A reference is like a pointer in that
* it’s an address we can follow to access the data stored at that address;
* that data is owned by some other variable
* We call the **action of creating a reference** ---> `borrowing`.
| --------

* Unlike a pointer, a reference **is guaranteed** to point to a valid value
* of a particular type for the life of that reference.


> Note: The **opposite of referencing** by using `&`
> is **dereferencing**,
> which is accomplished with the dereference operator, `*`.

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}


|---------------
* **WON'T COMPILE**

```rust
fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world"); // TRYING TO MODIFY
}
```


## Mutable References
* if you have a mutable reference to a value,
* you can have no other references to that value

```rust
fn main() {
    let mut s = String::from("hello"); // here - mut

    change(&mut s); // here
}

fn change(some_string: &mut String) { // Change reference to be MUTABLE
```

| ----------------------

* WON'T COMPILE
```rust
    let mut s = String::from("hello");

    let r1 = &mut s;
    let r2 = &mut s;

    println!("{}, {}", r1, r2);

    some_string.push_str(", world");
}
```

* because we cannot borrow s as mutable more than once
* The benefit of having this restriction
* is that Rust can prevent data races at compile time.


#### A data race
* is similar to a race condition and happens when these three behaviors occur:
  * Two or more pointers access the same data at the same time.
  * At least one of the pointers is being used to write to the data.
  * There’s no mechanism being used to synchronize access to the data.

#### An alternative
* However, `multiple immutable` references are **`allowed`**

* As always, we can use curly brackets to create a new scope,
* allowing for multiple mutable references, just not simultaneous ones:

```rust
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 goes out of scope here, so we can make a new reference with no problems.

    let r2 = &mut s;
 ```
 
 
 |----------------------
 
 ```rust
     let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    let r3 = &mut s; // BIG PROBLEM

    println!("{}, {}, and {}", r1, r2, r3);
    
```


> Note that a reference’s scope starts from where it is introduced
> and continues through the last time that reference is used.

*  The ability of the compiler
*  to tell that a reference is no longer being used at a point before the end of the scope
*  is called `Non-Lexical Lifetimes` (NLL for short), 
*  and you can read more about it in [The Edition Guide](https://doc.rust-lang.org/edition-guide/rust-2018/ownership-and-lifetimes/non-lexical-lifetimes.html).

```rust
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    println!("{} and {}", r1, r2);
    // variables r1 and r2 will not be used after this point

    let r3 = &mut s; // no problem
    println!("{}", r3);
```


## Dangling References
* `dangling pointer`--a pointer that references a location in memory
*  that may have been given to someone
*  by freeing some memory while preserving a pointer to that memory.

|--------

* In Rust, by contrast, the compiler **guarantees**
* that references** will never be dangling** references:
* Because s is created inside dangle,
* when the code of dangle is finished, s will be deallocated. 
* But we tried to return a reference to it.
* That means this reference would be pointing to an invalid String.
```rust
fn main() {
    let reference_to_nothing = dangle();
    // ERROR &s cannot be returned
    // as its scope is ended within <<dangle>> function
}

fn dangle() -> &String {
    let s = String::from("hello");

    &s // scope ends here so cannot return dangling pointer
}
```

* The solution here is to return the String directly:

```rust
fn no_dangle() -> String {
    let s = String::from("hello");

    s
}
```


## The Rules of References
* Let’s recap what we’ve discussed about references:

    * At any given time, you can have either one mutable reference or any number of immutable references.
    * References must always be valid.

