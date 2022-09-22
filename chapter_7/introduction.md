[<< Chapter 6: Concise Control Flow with if let](../chapter_6/if_let_control.md) | [ Packages and Crates >>](./packages_and_crates.md)

# Managing Growing Projects with Packages, Crates, and Modules
> As you write large programs, organizing your code will become increasingly important

#### --------------------

> We’ll also discuss encapsulating implementation details, which lets you reuse code at a higher level:
> 
>   once you’ve implemented an operation, 
>   
>   other code can call your code via its public interface 
>   
>   without having to know how the implementation works.

#### -----------------

> Rust has a number of features that allow you to manage your code’s organization
> 
> including which details are exposed, 
> 
> which details are private, 
> 
> and what names are in each scope in your programs. 
> 


* These features, sometimes collectively referred to as the module system, include:
    - Packages: A Cargo feature that lets you build, test, and share crates
    - Crates: A tree of modules that produces a library or executable
    - Modules and use: Let you control the organization, scope, and privacy of paths
    - Paths: A way of naming an item, such as a struct, function, or module
