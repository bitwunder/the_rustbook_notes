[<< The Match Control Flow](./match_control_flow.md) | [ Chapter 7 Introduction >>](../chapter_7/introduction.md)

# Concise Control Flow with if let
* The `if let` syntax lets you combine `if` and `let` into a less verbose way to handle values
* **that match one pattern** while ignoring the rest

```rust 
    let config_max = Some(3u8);
    if let Some(max) = config_max {
        println!("The maximum is configured to be {}", max);
    }
```

#### -------------
#### We can include an else with an if let.
* The block of code that goes with the else 
* is the same as the block of code that would go with the _ case in the match

```rust
    let mut count = 0;
    match coin {
        Coin::Quarter(state) => println!("State quarter from {:?}!", state),
        _ => count += 1,
    }
```

* Or we could use an if let and else expression like this:

```rust
    let mut count = 0;
    if let Coin::Quarter(state) = coin {
        println!("State quarter from {:?}!", state);
    } else {
        count += 1;
    }
```
