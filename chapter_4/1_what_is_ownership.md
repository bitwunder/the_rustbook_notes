# What Is Ownership?

## What Is Ownership?
* `Ownership` is a set of rules that governs how a Rust program manages memory.
* memory is managed through a system of ownership with a set of rules that the compiler checks.
* None of the features of ownership will slow down your program while it’s running.


## Ownership Rules
#### 1. Each value in Rust has an owner.
#### 2. There can only be one owner at a time.
#### 3. When the owner goes out of scope, the value will be dropped.


## Variable Scope
* A scope is the range within a program for which an item is valid
```rust
{                      // s is not valid here, it’s not yet declared
    let s = "hello";   // s is valid from this point forward

    // do stuff with s
}                      // this scope is now over, and s is no longer valid
```
* In other words, there are two important points in time here:
  - When s comes into scope, it is valid.
  - It remains valid until it goes out of scope.


## The String Type
* we want to look at data that is stored on the heap
* and explore how Rust knows when to clean up that data
* and the String type is a great example.

|-

* Rust has a second string type, `String`.
* This type manages data allocated **on the heap** 
* and as such is able to store an amount of text **that is unknown to us at compile time**
* This kind of string can be mutated:
```rust
let mut s = String::from("hello");

s.push_str(", world!"); // push_str() appends a literal to a String

println!("{}", s); // This will print `hello, world!`
```


## Memory and Allocation
* With the String type, in order to support a mutable, growable piece of text
* we need to allocate an amount of memory on the heap,
* unknown at compile time, to hold the contents. This means:
  * The memory must be requested from the memory allocator at runtime.
  * We need a way of returning this memory to the allocator when we’re done with our String.

| ---

* Rust takes a different path:
  * the memory is automatically returned
  * once the variable that owns it **goes out of scope**

```rust
{
    let s = String::from("hello"); // s is valid from this point forward

    // do stuff with s
}  // this scope is now over, and s is no longer valid
```

| ---------

* When a variable goes out of scope
* Rust calls a **special function** for us.
* This function is called [`drop`](https://doc.rust-lang.org/stable/std/ops/trait.Drop.html#tymethod.drop),
* and it’s where the author of String
* can put the code to return the memory

> Note: In C++, this pattern of deallocating resources at the end of an item’s lifetime
> is sometimes called `Resource Acquisition Is Initialization (RAII)`



## Ways Variables and Data Interact: Move
```rust
let s1 = String::from("hello");
let s2 = s1;
```

* A String is made up of three parts:
* This group of data is stored on the stack.
* On the right is the memory on the heap that holds the contents.
![This is an image](./img/chap4/string_memory.png)

| -------------

* When we assign s1 to s2,
* the String data is copied,
* meaning we copy the `pointer, the length, and the capacity`
* that are on the stack.

![This is an image](./img/chap4/string_memory_copying.png)


| --------------------------

* Earlier, we said that when a variable goes out of scope,
* Rust automatically calls the drop function and cleans up the heap memory 
* But Figure 4-2 shows both data pointers pointing to the same location.

| -

* This is a problem: when s2 and s1 go out of scope,
* they will both try to free the same memory
* This is known as a `double free error`

| -

* To ensure memory safety,
* after the line `let s2 = s1`,
* Rust considers **s1 as no longer valid.** 

```rust
// COMPILE TIME ERROR

let s1 = String::from("hello");
let s2 = s1;

println!("{}, world!", s1);
```

| -------------------------
* terms **`shallow copy`** and **`deep copy`**
* But because Rust also **invalidates** the first variable,
* instead of calling it a `shallow copy`, it’s known as a **`move`**.

| ---------------------------
> Rust will never automatically create “deep” copies of your data.
> Therefore, any automatic copying can be assumed
> to be inexpensive in terms of runtime performance.


## Ways Variables and Data Interact: Clone
* If we do want to deeply copy the heap data of the String
* not just the stack data,
* we can use a common method called `clone`

```rust
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
```

> When you see a call to clone,
> you know that some arbitrary code is being executed
> and that code may be expensive.
> It’s a visual indicator that something different is going on.



## Stack-Only Data: Copy

* But this code seems to contradict what we just learned:
  *  we don’t have a call to clone, but `x` is still valid and wasn’t moved into `y`.
```rust
    let x = 5;
    let y = x;

    println!("x = {}, y = {}", x, y);
```

> The reason is that types such as integers that have a known size at compile time
> are stored entirely on the stack,
> so copies of the actual values are quick to make.
> That means there’s no reason we would want
> to prevent x from being valid after we create the variable y


| ------------------------------

* Rust won’t let us annotate a type with `Copy`
* if the type, or any of its parts, has implemented the Drop trait

+
* nothing that requires allocation or is some form of resource can implement Copy






## Ownership and Functions
* The mechanics of passing a value to a function
* are similar to those when assigning a value to a variable.
* Passing a variable to a function **will move or copy**,

```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it's okay to still
                                    // use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

* If we tried to use s after the call to takes_ownership, Rust would throw a compile-time error. 


## Return Values and Scope
* Returning values can also **transfer ownership**

```rust
Filename: src/main.rs

fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 was moved, so nothing
  // happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it

    let some_string = String::from("yours"); // some_string comes into scope

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function
}

// This function takes a String and returns one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope

    a_string  // a_string is returned and moves out to the calling function
}
```

* While this works,
* taking ownership 
* and then returning ownership with every function is a bit tedious. 

```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() returns the length of a String

    (s, length)
}
```

> Luckily for us,
> Rust has a feature for using a value without transferring ownership,
> called `references`.




----
## The Stack and the Heap
* Both the `stack` and the `heap` **are** **parts of memory** available to your code to use at runtime


| ------- STACK -------------
> The `stack` stores values **in the order** it gets them and removes the values in the opposite order
> This is referred to as `last in, first out`.
>
> Adding data is called `pushing onto the stack`
> removing data is called `popping off the stack`

* All data stored on the stack must have a known, fixed size

| ------- HEAP ----------------

* The heap is less organized
  * you request a certain amount of space
  * The memory allocator finds an empty spot in the heap that is big enough
  * marks it as being in use
  * returns a pointer, which is the address of that location

* This process is called `allocating on the heap`
* and is sometimes abbreviated as just `allocating`

> Because the pointer to the heap is a known, fixed size
> you can store the pointer on the stack,
> but when you want the actual data, you must follow the pointer.


> When your code `calls a function`,
> the values passed into the function (including, potentially, pointers to data on the heap)
> and the function’s local variables `get pushed onto the stack`.
> When the function is over, those values get popped off the stack.
