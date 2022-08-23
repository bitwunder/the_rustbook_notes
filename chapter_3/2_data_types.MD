* Rust is a statically typed language
  * which means that it must know the types of all variables at compile time.


scalar and compound





|-----------------


```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

* If we don’t add the `: u32`
* Rust will display the error
* which means the compiler needs more information from us
* to know which type we want to use



## Scalar Types
* A scalar type represents a `single value`

##### Rust has four primary scalar types:
1) integers
2) floating-point 
3) Booleans
4) characters

### 1. Integers
* signed integer types start with `i`, instead of `u`
* Signed numbers are stored using two’s [complement representation](https://en.wikipedia.org/wiki/Two%27s_complement)
* integer types default to i32

| Length | Signed | Unsigned |
|-|-|-|
| 8-bit   | i8   | u8   |
| 16-bit  | i16  | u16  |
| 32-bit  | i32  | u32  |
| 64-bit  | i64  | u64  |
| 128-bit | i128 | u128 |
| arch    | isize| usize|

* Each signed variant can store numbers from -(2<sup>n - 1</sup>) to 2<sup>n -1</sup> - 1 inclusive,
* where <em>n</em> is the number of bits that variant uses.

| ---------------

* So an <code class="hljs">i8</code> can store numbers from -(2<sup>7</sup>) to 2<sup>7</sup> - 1,
* which equals -128 to 127. Unsigned variants can store numbers from 0 to 2<sup>n</sup> - 1,

| ---------------

* so a <code class="hljs">u8</code> can store numbers from 0 to 2<sup>8</sup> - 1, which equals 0 to 255.

| ---------------

* Each `signed` variant can store numbers from -(2<sup>n - 1</sup>) to 2<sup>n -1</sup> - 1 inclusive

| ---------------

* Additionally, the `isize` and `usize` types depend on the architecture of the computer your program is running on,
* which is denoted in the table as “arch”: 
  - 64 bits if you’re on a 64-bit architecture
  - 32 bits if you’re on a 32-bit architecture.

```rust
fn main ()
{
  let constant: isize = 356;
  
  println!("{constant}");
}
```



* Number literals can also use `_` as a visual separator 
* to make the number easier to read, such as `1_000`


| -------------------
|Number         | literals	Example
|-|-
|Decimal        | 98_222
|Hex            | 0xff
|Octal          | 0o77
|Binary         | 0b1111_0000
|Byte (u8 only) | b'A'





#### Integer Overflow
If you try to change the variable to a value outside of that range, such as 256, integer overflow will occur,


* When you’re compiling in debug mode, Rust includes checks for integer overflow
* to panic at runtime
* if overflow occurs, Rust performs **two’s complement wrapping**.


| ---------------

To explicitly handle the possibility of overflow,
you can use these families of methods
provided by the standard library for primitive numeric types:

|----------
* When you’re compiling in release mode with the --release flag, Rust does not include checks for integer overflow


    * Wrap in all modes with the wrapping_* methods, such as wrapping_add
    * Return the None value if there is overflow with the checked_* methods
    * Return the value and a boolean indicating whether there was overflow with the overflowing_* methods
    * Saturate at the value’s minimum or maximum values with saturating_* methods


| ----------------


### 2. Floating point
* All floating-point types are signed.
* The f32 type is a single-precision float, and f64 has double precision.

```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```


### 3. Operations
```rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let floored = 2 / 3; // Results in 0

    // remainder
    let remainder = 43 % 5;
}
```


### 4. Boolean
```rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}
```

### 5. Character
* Rust’s char type is `four bytes` in size and represents a `Unicode` Scalar Value
* can represent a lot more than just ASCII.
  * Accented letters;
  * Chinese, Japanese, and Korean characters;
  * emoji;
  * zero-width spaces
 
```rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit type annotation
    let heart_eyed_cat = '😻';
}
```


---
## Compound Types
* Compound types can group `multiple` values `into one` type.
* Rust has two **primitive** compound types: `tuples` and `arrays`.


### The Tuple
* Tuples have a fixed length: once declared,
* they **cannot grow or shrink** in size.


> `Destructuring` - turn it into multiple separate variables
> ( break the single tuple into three parts )


> The tuple `without any values` has a special name, `unit`
> This value and its corresponding type are both written `()`
> and represent an empty value or an empty return type.
>
> Expressions implicitly return the unit value if they don’t return any other value.


```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

| -----------------

```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {y}");
}
```

```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

### The Array
*  every element of an array must have the same type. 
*  arrays in Rust have a `fixed length`.
* Arrays are useful when you want your data allocated on the `stack` rather than the `heap`

| -

* Invalid Array Element Access results
* in a **runtime error**
* at the point of using an invalid value in the indexing operation.


```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
}
```

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

* You can also initialize an array
* to contain the **same value for each element** by specifying the initial
* value, followed by a semicolon and then the length

```rust
let a = [3; 5];
```
