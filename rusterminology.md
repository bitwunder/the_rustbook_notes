# ERRORS

* There is a `Result` enum:
```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

* Returns `Result<std::fs::File, std::io::Error>`
```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");
}
```
