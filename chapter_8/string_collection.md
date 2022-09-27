---
#### [<< Vectors ](./vectors.md) | [  >>](./.md)

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







---
# Words
* propensity - схильність

