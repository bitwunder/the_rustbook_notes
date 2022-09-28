---
#### [<< String Collections ](./string_collection.md) | [ Chapter 9 - Error Handling >>](../chapter_9/introduction.md)

---

# Storing Keys with Associated Values in Hash Maps
* The type `HashMap<K, V>` stores a mapping of **keys** of type `K` to **values** of type `V` using a hashing function

## Creating a New Hash Map
* One way
  * empty hash map is using `new` static method
  * adding elements with `insert` method
* This collection (HashMap) is the least often used, so it’s not included in the prelude
* Just like vectors, hash maps store their data on the heap
* Like vectors, hash maps are homogeneous: 
  * all of the keys must have the same type as each other
  * and all of the values must have the same type.

```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);
```

## Accessing Values in a Hash Map
* The `get` method returns an `Option<&V>`
* This code will print each pair in an** arbitrary order**
* If there’s no value for that key in the hash map, get will return `None`
* This program handles the `Option` by calling `copied` to get an `Option<i32>` rather than an `Option<&i32>`
```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);

    let team_name = String::from("Blue");
    let score = scores.get(&team_name).copied().unwrap_or(0);
```

* We can iterate over each key/value pair in a hash map in a similar manner as we do with vectors
```
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);

    for (key, value) in &scores {
        println!("{}: {}", key, value);
    }
```


## Hash Maps and Ownership
* For types that implement the `Copy` trait, like `i32`, the values are copied into the hash map
* For owned values like `String`, the values will be moved and HashMap will be the owner of that value

```rust
    use std::collections::HashMap;

    let field_name = String::from("Favorite color");
    let field_value = String::from("Blue");

    let mut map = HashMap::new();
    map.insert(field_name, field_value);
    // field_name and field_value are invalid at this point, try using them and
    // see what compiler error you get!
```


## Updating a Hash Map

### Overwriting a Value
* If we insert a key and a value into a hash map
  * and then insert that same key with a different value,
  * the value associated with that key will be replaced.
* This code will print {"Blue": 25}

```rust
    use std::collections::HashMap;


    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Blue"), 25);

    println!("{:?}", scores);
```

### Adding a Key and Value Only If a Key Isn’t Present
* Hash maps have a special API for this called `entry`
* The return value of the `entry` method is an enum called `Entry`

```rust
use std::collections::HashMap;


fn main() {
    let mut hashmap: HashMap<&str, &str> = HashMap::new();

    // hashmap.insert("key", "key");
    hashmap.entry("key").or_insert("kaka");
    println!("{:?}", hashmap);
}
```

### Updating a Value Based on the Old Value
* The `split_whitespace` method returns an **iterator** over **sub-slices**, separated by whitespace
* The `or_insert` method returns a mutable reference (`&mut V`) 
```rust
use std::collections::HashMap;


fn main() {
    let word_list = "word some another fuck lucky bucky stop word common simon another";
    let mut hm = HashMap::new();

    for word in word_list.split_whitespace() {
        let count = hm.entry(word).or_insert(0);
        *count += 1;
    }
    println!("Counted words in hashmap {:?}", hm);
    // Counted words in hashmap {"stop": 1, "another": 2, "fuck": 1, "bucky": 1, "simon": 1, "word": 2, "common": 1, "lucky": 1, "some": 1}
}
```


## Hashing Functions
* By default, HashMap uses a hashing function called `SipHash`
* that can provide resistance to Denial of Service (DoS) attacks involving hash tables [1](https://en.wikipedia.org/wiki/SipHash)

| ----------------
* if you find that the default hash function is too slow for your purposes,
  * you can switch to another function by specifying a different `hasher`.
* A `hasher` is a type that implements the `BuildHasher` trait

