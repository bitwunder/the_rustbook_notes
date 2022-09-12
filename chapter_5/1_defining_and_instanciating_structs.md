[<< Introduction](./introduction.md_) | [Example of program with Structs >>](./example_using_structs.md)


# Defining and Instantiating Structs
* Structs are similar to tuples in that both hold multiple related values
* Like tuples, the pieces of a struct can be different types. 
* **Unlike** with tuples in a struct:
  - you’ll name each piece of data
  - so it’s clear what the values mean
* structs are more flexible than tuples:
  -  you don’t have to rely on the order of the data


#### -------------------------------

* To define a struct, we enter the keyword `struct`
* Then, inside curly brackets,
* define the `names` and `types` of the pieces of data,
* which are called `fields`

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```


```rust
fn main() {
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };
}
```


#### --------------------------------
* To get a specific value from a struct, we use **`dot notation`**
* !!! HERE:
  - If the **instance** is mutable
  - we can change a value
  - by using the dot notation and assigning into a particular field.

> Note that the **entire instance** must be mutable
> 
> Rust **doesn’t allow** us to mark only certain fields as mutable

```rust
fn main() {
    let mut user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };

    user1.email = String::from("anotheremail@example.com");
}
```


#### ---------------------------------
* we can construct a new instance of the struct
* as the last expression in the function body
* It makes sense:
   - to name the function parameters
   - with the same name as the struct fields,

```rust
fn build_user(email: String, username: String) -> User {
    User {
        email: email,
        username: username,
        active: true,
        sign_in_count: 1,
    }
}```

---
## Using the Field Init Shorthand
> Because the parameter names
> 
> and the struct field names are exactly the same
>
> we can use the **`field init shorthand syntax`**

```rust
fn build_user(email: String, username: String) -> User {
    User {
        email, // here
        username, // and here
        active: true,
        sign_in_count: 1,
    }
}
```

---
## Creating Instances From Other Instances With Struct Update Syntax
* It’s often useful
  - to create a new instance of a struct
  - that includes most of the values from another instance
  - but changes some

> You can do this using **`struct update syntax`**.

### WITHOUT update syntax
```rust
fn main() {
    // --snip--

    let user2 = User {
        active: user1.active,
        username: user1.username,
        email: String::from("another@example.com"),
        sign_in_count: user1.sign_in_count,
    };
}
```

### USING update syntax
* The syntax `..`:
  - specifies that the remaining fields
  - that are not explicitly set
  - should have the same value as the fields in the given instance.

```rust
fn main() {
    // --snip--

    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```

* !!! HERE
  - The `..user1` **must** come **last**

* !!! HERE
  - Note that the struct update syntax uses `=` like an assignment
  - this is because **it moves the data**,

> we can no longer use `user1` after creating `user2`
> 
> because the `String` in the `username` field of `user1` was moved into `user2`
> 
> The types of `active` and `sign_in_count` are types that implement the `Copy` trait,


----
## Using Tuple Structs without Named Fields to Create Different Types
* Rust also supports structs that look similar to tuples
  - called **`tuple structs`**. 
* Tuple structs **have the added meaning the struct name provides**
* but don’t have names associated with their fields
* rather, they just have the types of the fields

* Tuple structs are useful when you:
  - want to give the whole tuple a name
  - and make the tuple a different type from other tuples
  - and when naming each field as in a regular struct would be verbose or redundant


```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

> Note that the `black` and `origin` values **are different types**
> 
* For example:
  - a function that takes a parameter of type `Color`
  - cannot take a `Point` as an argument,
  - even though both types are made up **of three i32 values**.


* !!! HERE
  - you can destructure Tuple-Structs into their individual pieces
  - and you can use a `.` followed by the **index** to access an individual value.



---
## Unit-Like Structs Without Any Fields
* You can also define structs **that don’t have any fields**
* These are called **`unit-like`** structs 
  - because they behave similarly to `()` - the unit type  

* Unit-like structs can be useful when:
  - you need to implement a trait on some type 
  - but don’t have any data
  - that you want to store in the type itself.

```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

* Imagine that later we’ll implement behavior for this type such
  - that every instance of `AlwaysEqual`
  - is always equal to every instance of **any** other type


---
## Ownership of Struct Data
* In the User struct definition in Listing 5-1 we:
  - used the owned String type
  - rather than the &str string slice type.
* This is a deliberate choice because we
  - want each instance of this struct
  - to own all of its data
  -  and for that data to be valid
  -  for as long as the entire struct is valid

* It’s also possible for structs:
  - to store references to data **owned by something else**
  - but to do so requires the use of **lifetimes**
* Lifetimes ensure:
  - that the data referenced by a struct
  - is valid for as long as the struct is
