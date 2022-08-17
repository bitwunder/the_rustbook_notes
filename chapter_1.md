In Rust, **packages** of code are referred to as **_crates_**. 

This file is in the TOML (Tom’s Obvious, Minimal Language) format, which is Cargo’s configuration format.]

```
cargo new hello_cargo
cd hello_cargo
cargo build
cargo run
```
This command creates an executable file in target/debug/hello_cargo


> Running cargo build for the first time also causes Cargo to create a new file at the top level: **`Cargo.lock`**. 
> This file keeps track of the exact versions of dependencies in your project.


> Cargo also provides a command called **`cargo check`**
> This command quickly checks your code to make sure it compiles but doesn’t produce an executable
> Often, cargo check is much faster than cargo build

> When your project is finally ready for release, you can use cargo build --release to compile it with optimizations. 
> This command will create an executable in target/release instead of target/debug


### Cargo as Convention
With simple projects, Cargo doesn’t provide a lot of value over just using rustc,
