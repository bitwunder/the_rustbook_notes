[<< Introduction](./introduction.md_) | [The Match Control Flow >>](./match_control_flow.md)
# Defining an Enum

* enums give you a way of saying a value is one of a possible set of values
* we may want to say that Rectangle is one of a set of possible shapes that also includes Circle and Triangle. 

## Situation we might want an enum 
Say we need to work with IP addresses. 
Currently, two major standards are used for IP addresses: version four and version six.
We can enumerate all possible variants
* Any IP address can be either a version four or a version six address, but not both at the same time


```rust
enum IpAddrKind {
    V4,
    V6,
}
```


## Enum Values
```rust
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;
```

* Note that the variants of the enum are namespaced under its identifier
* now both values IpAddrKind::V4 and IpAddrKind::V6 are of the same type: IpAddrKind
* We can then, for instance, define a function that takes any IpAddrKind:

```rust
fn route(ip_kind: IpAddrKind) {}

route(IpAddrKind::V4);
route(IpAddrKind::V6);
```


## Using enums has even more advantages.
* Thinking more about our IP address type:
  - at the moment we don’t have a way to store the actual IP address data;
  - we only know what kind it is
  
  
  #### --------------
  * You might be tempted to tackle this problem with structs as shown in Listing 
  ```rust
      enum IpAddrKind {
        V4,
        V6,
    }

    struct IpAddr {
        kind: IpAddrKind,
        address: String,
    }

    let home = IpAddr {
        kind: IpAddrKind::V4,
        address: String::from("127.0.0.1"),
    };

    let loopback = IpAddr {
        kind: IpAddrKind::V6,
        address: String::from("::1"),
    };
    ```
    
    
    ### -----------------
    * However, representing the same concept using just an enum is more concise:
      - rather than an enum inside a struct,
      
    * This new definition of the IpAddr enum says that both V4 and V6 variants will have associated String values
    ```rust
    

    enum IpAddr {
        V4(String),
        V6(String),
    }

    let home = IpAddr::V4(String::from("127.0.0.1"));

    let loopback = IpAddr::V6(String::from("::1"));
    ```
    
    * the name of each enum variant that we define
    * also becomes a function that constructs an instance of the enum.
    * That is, IpAddr::V4() is a function call that takes a String argument and returns an instance of the IpAddr type
    
    
    #### There’s another advantage to using an enum rather than a struct:
    * each variant can have different types and amounts of associated data. 
    ```rust
        enum IpAddr {
        V4(u8, u8, u8, u8),
        V6(String),
    }

    let home = IpAddr::V4(127, 0, 0, 1);

    let loopback = IpAddr::V6(String::from("::1"));
```

#### Let’s look at how the standard library defines IpAddr
```rust

struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}
```

* This code illustrates that you can put any kind of data inside an enum variant

#### ------------
#### Let’s look at another example of an enum in Listing 
```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```


    - Quit has no data associated with it at all.
    - Move has named fields like a struct does.
    - Write includes a single String.
    - ChangeColor includes three i32 values.'
    
### --------------
### we’re also able to define methods on enums.
```rust
   impl Message {
        fn call(&self) {
            // method body would be defined here
        }
    }

    let m = Message::Write(String::from("hello"));
    m.call();
```

* The body of the method
* would use self to get the value (enum)
* that we called the method on
+++
In this example, we’ve created a variable m that has the value Message::Write(String::from("hello"))\n
and that is what self will be in the body of the call method when m.call() runs



## The Option Enum and Its Advantages Over Null Values
* The Option type encodes the very common scenario in which:
  - a value could be something
  - or it could be nothing.
