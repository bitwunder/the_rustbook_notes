[<< Introduction ](./introduction.md) | [  >>](./modules_controlflow_privacy.md)
# Packages and Crates

* A **`crate`** is the smallest amount of code that the Rust compiler considers at a time.
* Even if you run rustc rather than cargo and pass a single source code file, the compiler considers that file to be a crate
* Crates can contain **`modules`**
* **`modules`** may be defined in other files that get compiled with the crate


##### -------------
##### Crates
* Crate can come in one of two forms:
  - a **binary crate **
    - are programs you can compile to an executable that you can run
    -  such as a command-line program or a server. 
    -  Each must have a function called main
  - a **library crate.**
    - don’t have a main function
    - and they don’t compile to an executable. 
    -  Most of the time when Rustaceans say “crate”, they mean library crate
* The **crate root** is a source file that the Rust compiler starts from and makes up the root module of your crate


##### --------------
##### Packages
* A **package** is a bundle of one or more **crates** that provides a set of functionality
* A package contains a **`Cargo.toml`**: 
  - that describes how to build those crates
* A package can contain as many binary **crates** as you like, but at **most only one library crate**


##### --------------
##### Creating package
* Let’s walk through what happens when we create a package. 
* First, we enter the command `cargo new`:
  - In the project directory, there’s a Cargo.toml file
  - There’s also a src directory that contains main.rs
  - note there’s **no mention** of src/main.rs in your Cargo.toml .
  
> Cargo follows a convention that `src/main.rs` **is the crate root** of a binary crate 
> 
> **with the same name as the package**

> Likewise, Cargo knows that if the package directory contains `src/lib.rs`, 
> 
> the package contains a library crate **with the same name as the package**, and `src/lib.rs` **is its crate root**


* A package can have multiple binary crates by placing files in the src/bin directory:
  - each file will be a separate binary crate.
