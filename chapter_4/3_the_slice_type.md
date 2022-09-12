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

let slice = &s[0..2];
let slice = &s[..2];


---
## Words

* brittle - хрупкий, ломкий
