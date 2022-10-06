# Generic Types, Traits, and Lifetimes

* Every programming language has tools for effectively handling the duplication of concepts. In Rust, one such tool is **generics**:
* Then you’ll learn how to use traits to define behavior in a generic way.****
* Finally, we’ll discuss lifetimes: a variety of generics that give the compiler information about how references relate to each other.
* Lifetimes allow us to give the compiler enough information about borrowed values so that it can ensure references will be valid i



## Removing Duplication by Extracting a Function
* Generics allow us to replace specific types with a placeholder that represents multiple types to remove code duplication.
*  By looking at how to recognize duplicated code you can extract into a function, you’ll start to recognize duplicated code that can use generics.


```rust
fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let mut largest = &number_list[0];

    for number in &number_list {
        if number > largest {
            largest = number;
        }
    }

    println!("The largest number is {}", largest);
}
```


* We store a list of integers in the variable `number_list` 
* and **place a reference to the first number** in the list in a variable named largest.


###### --------------------
* We've now been tasked with finding the largest number in two different lists of numbers. To do so, we can choose to duplicate the code 
* To eliminate this duplication, we’ll create an abstraction by defining a function
```rust
fn largest(list: &[i32]) -> &i32 {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest(&number_list);
    println!("The largest number is {}", result);

    let number_list = vec![102, 34, 6000, 89, 54, 2, 43, 8];

    let result = largest(&number_list);
    println!("The largest number is {}", result);
}
```



# Phrases
stand-in -  замена,  дублер
