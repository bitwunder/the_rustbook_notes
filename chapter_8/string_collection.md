---
#### [<< Vectors ](./vectors.md) | [ Hash Maps >>](./hashmaps.md)

---


# Storing UTF-8 Encoded Text with Strings
* New Rustaceans commonly get stuck on strings for a combination of three reasons:
  * Rust’s propensity for exposing possible errors
  * strings being a more complicated data structure than many programmers give them credit for
  * UTF-8
* We discuss strings in the context of collections because strings are implemented as a collection of bytes



# What Is a String?
* Rust has only one string type in the core language, which is the string slice str
* String literals, for example, are stored in the program’s binary and are therefore string slices.
* both String and string slices are UTF-8 encoded


# Creating a New String
* String is actually implemented as a wrapper around a vector of bytes with some extra guarantees, restrictions, and capabilities.
* `to_string` method, which is available on any type that implements the `Display` trait,
* We can also use the function `String::from` to create a `String` from a string literal. 

```rust
    let mut s = String::new();
    
    let data = "initial contents";

    let s = data.to_string();

    // the method also works on a literal directly:
    let s = "initial contents".to_string();
```


## Updating a String
* A String can grow in size and its contents can change
* you can conveniently use the `+` operator or the `format!` macro to concatenate `String` values.

### Appending to a String with push_str and push

* The push method takes a single character

```rust
    let mut s = String::from("lo");
    s.push('l');
```
```rust
    let mut s = String::from("foo");
    s.push_str("bar");
    s ; //foobar
```

##### ----------
* * The `push_str` method takes a string slice because we don’t necessarily want to take ownership of the parameter
```rust
    let mut s1 = String::from("foo");
    let s2 = "bar";
    s1.push_str(s2);
    println!("s2 is {}", s2);
```


### Concatenation with the + Operator or the format! Macro

```rust
fn main() {
    let s1 = String::from("Hello, ");
    let s2 = String::from("world!");
    let s3 = s1 + &s2; // note s1 has been moved here and can no longer be used
}
```
* The `+` operator uses the add method, whose signature looks something like this:
```rust
fn add(self, s: &str) -> String {
```

| -------------

1) First, `s2` has an `&`, meaning that we’re adding a reference of the second string to the first string
2) we can only add a `&str` to a `String`
3) we can’t add two `String` values


| -------------

4) The reason we’re able to use `&s2` in the call to add is that the compiler can coerce the `&String` argument into a `&str`
5) When we call the add method, Rust uses a `deref coercion`,
6) which here turns `&s2` into `&s2[..]`

| -------------

7) Second, we can see in the signature that `add` takes **ownership** of `self`</p>
 * `let s3 = s1 + &s2;`:
   *  actually takes ownership of `s1`
   *  appends a copy of the contents of `s2`
   *  and then **returns ownership** of the result

| -------------
* If we need to concatenate multiple strings, the behavior of the `+ operator` gets **unwieldy**:
```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = s1 + "-" + &s2 + "-" + &s3;
```
* For more complicated string combining, we can instead use the `format!`:
```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{}-{}-{}", s1, s2, s3);
```
* The `format!` macro works like `println!`
* but instead of printing the output to the screen, it returns a String


## Indexing into Strings
* Rust strings **DON’T** support indexing.
* ERROR:
```rust
    let s1 = String::from("hello");
    let h = s1[0];
```

## Internal Representation
* A `String` is a wrapper over a `Vec<u8>`
* UTF-8 takes `2 bytes` in memory
* So no letter can be returned as rust have to return byte, but it will be the first byte of an UTF8 str
* So such code won't compile at all



## Bytes and Scalar Values and Grapheme Clusters! Oh My!
* In Rust there are three relevant ways to look at strings:
 * as bytes
 * scalar values
 * graphene clustrers (what we would call letter, consisting from unicode code points)
 
 |-----
 * If we look at the Hindi word “नमस्ते”
  * it is stored as a vector of `u8` values:
  * That’s 18 bytes and is how computers ultimately store this data.
   ```text
   [224, 164, 168, 224, 164, 174, 224, 164, 184, 224, 165, 141, 224, 164, 164,
224, 165, 135]
   ```

* If we look at them as Unicode **scalar** values,
* (which are what Rust’s `char` type is)
* those bytes look like this:
* 
* There are six char values here
* but the fourth and sixth are diacritics

```text
['न', 'म', 'स', '्', 'त', 'े']
```


* Finally, if we look at them as **grapheme clusters
* we’d get what a person would call the four letters
* that make up the Hindi word
```
["न", "म", "स्", "ते"]
```

##### | -----------------
##### | Why there is no String `indexing` in Rust
* An index into the string’s bytes will not always correlate to a valid Unicode scalar value. 
 * Consider this:
 ```
 let hello = "Здравствуйте";
 let answer = &hello[0];
 ```
 * encoded in UTF-8, the **first byte** of `З` is `208` and the **second** is `151`
 * seem that answer should in fact be 208, but 208 is not a valid character on its own.
 * however, that’s the only data that Rust has at byte index 0
* A final reason Rust doesn’t allow us to index into a String to get a character is that indexing operations are expected to always take constant time (O(1)). Rust would have to walk through the contents from the beginning to the index to determine how many valid characters there were




# Slicing Strings
* Indexing into a string is often a bad idea because:
 * it’s not clear what the return type of the string-indexing operation should be:
  * a byte value
  * a character
  * a grapheme cluster
  * or a string slice
* Rather than indexing using [] with a single number, you can use [] with a range


```rust
let hello = "Здравствуйте";

let s = &hello[0..4];
```

* Here, s will be a `&str` that contains the **first `4 bytes`** of the string.
* each of these characters was 2 bytes, which means `s` will be `Зд`

##### !!!! 
* If we were to try to slice only part of a character’s bytes with something like `&hello[0..1]`, Rust would panic at runtime
* You should use ranges to create string slices with caution, because doing so can crash your program.


## Methods for Iterating Over Strings
* The best way to operate on pieces of strings is to be explicit about whether you want characters or bytes.
* For individual Unicode scalar values, use the **`chars` method**

```rust
for c in "Зд".chars() {
    println!("{}", c);
}
```

```rust
fn main() {
    let st: &str = "Здрфвствуйте";

    for c in st.chars() {
        println!("{}", c);
    }
}
```

* You can also iterate over bytes:
* But be sure to remember that valid Unicode scalar values may be made up of more than 1 byte.
```rust
for b in "Зд".bytes() {
    println!("{}", b);
}
```


| --------------------------
* Getting grapheme clusters from strings as with the Devanagari script is complex, so this functionality is not provided by the standard librar


## Strings Are Not So Simple
* Different programming languages make different choices about how to present this complexity to the programmer.
* programmers have to put more thought into handling UTF-8 data upfront
* This trade-off exposes more of the complexity of strings than is apparent in other programming languages, but it prevents you from having to handle errors involving non-ASCII characters later in your development life cycle.
* Be sure to check out the documentation for useful methods like
 * `contains`
 * `replace`


---
# Words
* propensity - схильність

