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

## Strings

```rust

// CreatingStrings with initial content
let data = "initial contents";

let s = data.to_string();

// the method also works on a literal directly:
let s = "initial contents".to_string();

//Alternatively
let s = String::from("initial contents");

// Concatenation using the `+` operator
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // note s1 has been moved here and can no longer be used

let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{}-{}-{}", s1, s2, s3);

// slicing *bytes* 
let s = &hello[0..4];

// looping over all chars in the String (.bytes() for loopingover bytes)
for c in "नमस्ते".chars() {
    println!("{}", c);
}
```

## Hash maps

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

//only insert when key is not present
scores.entry(String::from("Blue")).or_insert(50);



let text = "hello world wonderful world";

//if key is not in the map, insert the parameter value under that (new) key. Genius!
//If the key is there return a *reference* to the value associated withtha key.
//Here we dereference that calue reference and add 1. Since this is a reference we are actually updating the value in the HashMap. 
for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}

let score = scores.get(&team_name); //returns an Option<&V>

// Loop over the HashMap
for (key, value) in &scores {
    println!("{}: {}", key, value);
}

// zip vecs into hashmap
let teams  = vec![String::from("Blue"), String::from("Yellow")];
let initial_scores = vec![10, 50];

// Look at that syntax
let scores: HashMap<_, _> = teams.iter().zip(initial_scores.iter()).collect();

let field_name = String::from("Favorite color");
let field_value = String::from("Blue");


let mut map = HashMap::new();
map.insert(field_name, field_value);
// field_name and field_value are invalid at this point, try using them and
// see what compiler error you get! (they were moved into the HashMap who is the owner now)
```







