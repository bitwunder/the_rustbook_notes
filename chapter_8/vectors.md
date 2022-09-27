---
#### [<< Introduction ](./introduction.md) | [ String collection >>](./string_collection.md)

---

# Storing Lists of Values with Vectors
* Vectors can **only** store values of the same type
* all the values next to each other in memory
* Vectors are implemented using generics (Vec\<T\> can hold any type)

## Creating a New Vector
```rust 
    let v: Vec<i32> = Vec::new(); // type annotation is needed
```

#### ---------------------
* Rust conveniently provides the `vec!` macro*

```
    let v = vec![1, 2, 3]; // Vec<i32> inferred
```


## Updating a Vector
* we can use the `push` method

```rust
    let mut v = Vec::new();

    v.push(5);
    v.push(6);
    v.push(7);
    v.push(8);
```


## Reading Elements of Vectors
* via **indexing** or using the **`get`** method

```rust
let v = vec![1,2,3,4,5];

let third: &i32 = &v[2];

println!("The third element is {}", third);

let third: Option<&i32> = v.get(2);

match third {
  Some(third_el) => println!("The third element is {}", third_el),
  None => println!("There is no third element!"),
}
```

#### ----------------------
#### This WON'T compile
* because vectors put the values next to each other in memory
* adding a new element onto the end of the vector might:
  * require allocating new memory
  * and copying the old elements to the new space


```rust
    let mut v = vec![1, 2, 3, 4, 5];

    let first = &v[0];

    v.push(6);

    println!("The first element is: {}", first);
```

## Iterating over the Values in a Vector
* use a for loop to get immutable references to each element in a vector 

```rust
let v = vec![100, 32, 57];

for i in &v {
  println!("{}", i);
}
```

* We can also iterate over mutable references:
* we have to use the `*` dereference operator to get to the value in i
```rust
let mut v = vec![100, 32, 57];

for i in &mut v {
  *i += 50;
}
```

## Using an Enum to Store Multiple Types
```rust
    enum SpreadsheetCell {
        Int(i32),
        Float(f64),
        Text(String),
    }

    let row = vec![
        SpreadsheetCell::Int(3),
        SpreadsheetCell::Text(String::from("blue")),
        SpreadsheetCell::Float(10.12),
    ];
```

OR 

```rust
#[derive(Debug)]
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}


fn main() {
    let mut somevector:Vec<SpreadsheetCell> = Vec::new();

    somevector.push(SpreadsheetCell::Int(32));
    println!("THE VALUE is : {:?}", somevector[0]);
}
```


## Dropping a Vector Drops Its Elements
* Like any other struct, a vector is freed when it goes out of scope,

```rust
{
  let v = vec![1,2,3,4,5];
} // v is out of scope
```
