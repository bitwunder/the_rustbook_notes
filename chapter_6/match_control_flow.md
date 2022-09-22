[<< Defining Enums](./defining_enums.md) | [Concise Control with If Let >>](./if_let_control.md)

# The match Control Flow Construct
* Rust has an extremely powerful control flow construct called `match` that allows you to compare a value against a series of patterns

* The power of match comes from the expressiveness of the patterns and the fact that the **compiler confirms that all possible cases are handled**.

* Think of a match expression as being like a **coin-sorting machine**:
  - each coin falls through the first hole it encounters that it fits into

## Example
```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}


fn main () {
    let c = Coin::Quarter;

    let cents: u32 = value_in_cents(c);

    println!("{cents}"); // 25
}


fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```
#### ----------------------------
#### Expression type
* match is similar to an expression used with if, but with if, the expression needs to return a Boolean value,
  - but match expression can return any type

#### ----------------------------
#### match arms
* An arm has two parts: a pattern and some code
* The code associated with each arm is **an expression**
* the `=>` operator separates **the pattern** and the **code to run**
* We can have as many arms as we need
* If you want to run multiple lines of code in a match arm, you must use curly brackets:
  - and the comma following the arm is then optional

```rust
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        }
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

## Patterns that Bind to Values
> Another useful feature of match arms is that they can bind to the parts of the values that match the pattern. 

> From 1999 through 2008, the United States minted quarters with different designs for each of the 50 states on one side. No other coins got state designs, so only quarters have this extra value. We can add this information to our enum by changing the Quarter variant to include a UsState value stored inside it


#### -------------------
* we add a variable called state to the pattern that matches values of the variant Coin::Quarter
* When a `Coin::Quarter` matches, the **`state` variable** will bind to the value of that quarter’s state

```rust
#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {:?}!", state);
            25
        }
    }
}

fn main() {
    value_in_cents(Coin::Quarter(UsState::Alaska));
}
``` 


## Matching with Option\<T\>
* In the previous section, we wanted to get the inner T value out of the Some case when using Option\<T\>
* Instead of comparing coins, we’ll compare the variants of Option\<T\>

```rust
fn main () {

    fn plus_one(value: Option<i32>) -> Option<i32> {
        match value {
            None => None,
            Some(i) => Some(i+1)
        }
    }

    let increased_or_none = plus_one(Some(16));

    println!("Increased or none value: {:?}", increased_or_none);
}
```
  
## Matches Are Exhaustive
* the `arms`’ patterns **must cover all** possibilities

#### ------------
#### Consider a bug program
* We didn’t handle the None case,

```rust
    fn plus_one(x: Option<i32>) -> Option<i32> {
        match x {
            Some(i) => Some(i + 1),
        }
    }
```

##### !!!!! Matches in Rust are exhaustive
>  Rust prevents us from forgetting to explicitly handle the None case, it protects us from assuming that we have a value when we might have null, thus making the billion-dollar mistake discussed earlier impossible.


## Catch-all Patterns and the _ Placeholder
* Using enums, we can also take special actions for a few particular values, 
* but for all other values take one default action

> Imagine we’re implementing a game where, if you roll a 3 on a dice roll, your player doesn’t move, but instead gets a new fancy hat. If you roll a 7, your player loses a fancy hat. For all other values, your player moves that number of spaces on the game board

```rust
fn main() {
    let dice_roll = 9;
    match dice_roll {
        3 => add_fancy_hat(),
        7 => remove_fancy_hat(),
        other => move_player(other),
    }

    fn add_fancy_hat() {}
    fn remove_fancy_hat() {}
    fn move_player(num_spaces: u8) {}
}
```

* This code compiles, even though we haven’t listed all the possible values a u8 can have, because the last pattern will match all values not specifically listed.
* Note that we have to put the catch-all arm last because the patterns are evaluated in order.

#### -------------------
* Rust also has a pattern we can use when we want a catch-all 
* but don’t want to use the value in the catch-all pattern: `_`

* _ is a special pattern that matches any value and does not bind to that value
*  Rust won’t warn us about an unused variable.

```rust
    let dice_roll = 9;
    match dice_roll {
        3 => add_fancy_hat(),
        7 => remove_fancy_hat(),
        _ => reroll(),
    }

    fn add_fancy_hat() {}
    fn remove_fancy_hat() {}
    fn reroll() {}
```

#### --------------
#### OR to do nothing for all other
```rust
    let dice_roll = 9;
    match dice_roll {
        3 => add_fancy_hat(),
        7 => remove_fancy_hat(),
        _ => (),
    }

    fn add_fancy_hat() {}
    fn remove_fancy_hat() {}
```
