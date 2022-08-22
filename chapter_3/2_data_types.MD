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

| ----------------


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


