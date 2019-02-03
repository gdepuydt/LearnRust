# Collections

Stored on the heap, unlike Tuples and Arrays.

## Vec<T>

[API documentation](https://doc.rust-lang.org/std/vec/struct.Vec.html)

```rust
{
    let mut v: Vec<i32> = Vec::new();
    //or
    let mut v = vec![1, 2, 3]; //the type (i32) is inferred by the compliler
    //Updatiing the vector
    v.push(5);

    // Method 1: via a reference
    let third: &i32 = &v[2];
    println!("The third element is {}", third);

    // method 2:
    // get() returns the Option enum 
    match v.get(2) {
        Some(third) => println!("The third element is {}", third),
        None => println!("There is no third element."),
    }

    let mut v = vec![100, 32, 57];
    for i in &mut v {
        *i += 50;
    }

    //Since a vector can store only one T kind, you can use enums as the type to store in the vector, how clever!
    enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
    }

    let row = vec![
        SpreadsheetCell::Int(3),
        SpreadsheetCell::Text(String::from("blue")),
        SpreadsheetCell::Float(10.12),
    ];
} // <- v goes out of scope and is freed here
```


