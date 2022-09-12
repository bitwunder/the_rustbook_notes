# The Slice Type
> Slices let you reference a contiguous sequence of elements in a collection
> 
> rather than the whole collection


----
## Here’s a small programming problem:
> write a function,
> 
> that takes a string of words separated by spaces
> 
> and returns the first word it finds in that string.
> 
> If the function doesn’t find a space in the string,
> 
> the whole string must be one word,
> 
> so the entire string should be returned.


----
```rust
fn first_word(s: &String) -> usize {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return i;
        }
    }

    s.len()
}
```

1) convert our String to an **array of bytes** using the `as_bytes` method
2) we **create an iterator** over the array of bytes using the `iter` method:
3) `enumerate` wraps the result of iter and returns each element as part of a **tuple instead**.
  - The first element of the tuple is the index
  - and the second element is a reference to the element.
4) Inside the for loop, we search for the byte that represents the space
  -  by using the `**byte literal**` syntax


#### -----------------------

 * We’re returning a `usize` on its own,
 * but it’s only a meaningful number in the context of the `&String`.
 * In other words, because it’s a separate value from the String,
 * there’s no guarantee that it will still be valid in the future:

```rust
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s); // word will get the value 5

    s.clear(); // this empties the String, making it equal to ""

    // word still has the value 5 here, but there's no more string that
    // we could meaningfully use the value 5 with. word is now totally invalid!
}
```

#### -----------------------


---
## String Slices
> A **string slice** is a reference to part of a String, and it looks like this:

```rust
    let s = String::from("hello world");

    let hello = &s[0..5];
    let world = &s[6..11];
```

* Rather than a reference to the entire String
* `hello` is a reference to a **portion** of the String

* !!! HERE:
  - `starting_index` is the first position in the slice 
  - `ending_index` is **one more than** the last position in the slice

* !!! HERE
>  **Internally**, the slice data structure
>  
>  stores the **starting position** and the **length of the slice**


#### -------------------------------

* This syntax is also allowed:

```rust

let s = String::from("hello");
let len = s.len();

let slice = &s[0..2];
let slice = &s[..2];
let slice = &s[..]; // Whole string
let slice = &s[..len];
```

#### --------------------------------

* !!! HERE
> Note: String slice range indices must occur **at valid UTF-8 character boundaries****
> 
> If you attempt to create a string slice
> 
> in the middle of a multibyte character,
> 
> your program will exit with an error.


#### --------------------------------

```rust
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s);

    s.clear(); // error!

    println!("the first word is: {}", word);
}
```

* Here’s the compiler error:
```text
$ cargo run
   Compiling ownership v0.1.0 (file:///projects/ownership)
error[E0502]: cannot borrow `s` as mutable because it is also borrowed as immutable
  --> src/main.rs:18:5
   |
16 |     let word = first_word(&s);
   |                           -- immutable borrow occurs here
17 | 
18 |     s.clear(); // error!
   |     ^^^^^^^^^ mutable borrow occurs here
19 | 
20 |     println!("the first word is: {}", word);
   |                                       ---- immutable borrow later used here

For more information about this error, try `rustc --explain E0502`.
error: could not compile `ownership` due to previous error
```

* Recall from the borrowing rules that:
    - **if we have an `immutable` reference** to something
    - we cannot also take a **mutable** reference


----
## String Literals Are Slices
```rust
let s = "Hello, world!";
```

* The type of `s` here is `&str`

* !!!HERE:
> This is also why **string literals are immutable**;
>
>  `&str` is an **immutable** reference


----
## String Slices as Parameters

```rust
fn first_word(s: &str) -> &str {
```
* it allows us to use the same function on both:
    - `&String` values
    - and `&str` values
* If we have a **string slice**, we can pass that directly.
* If we have a `String`, we can pass a **slice of the String** OR a **reference to the String**.


```rust
fn main() {
    let my_string = String::from("hello world");

    // `first_word` works on slices of `String`s, whether partial or whole
    let word = first_word(&my_string[0..6]);
    let word = first_word(&my_string[..]);
    // `first_word` also works on references to `String`s, which are equivalent
    // to whole slices of `String`s
    let word = first_word(&my_string);

    let my_string_literal = "hello world";

    // `first_word` works on slices of string literals, whether partial or whole
    let word = first_word(&my_string_literal[0..6]);
    let word = first_word(&my_string_literal[..]);

    // Because string literals *are* string slices already,
    // this works too, without the slice syntax!
    let word = first_word(my_string_literal);
}
```


----
## Other Slices

* This slice has the type `&[i32]`
* It works the same way as string slices do:
    - by storing a reference to the first element and a length
* You’ll use this kind of slice for all sorts of other collections. 


```rust

let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];

assert_eq!(slice, &[2, 3]);
```


----
## References
* `Deref coercions` -> https://doc.rust-lang.org/book/ch15-02-deref.html#implicit-deref-coercions-with-functions-and-methods


---
## Words

* brittle - хрупкий, ломкий
