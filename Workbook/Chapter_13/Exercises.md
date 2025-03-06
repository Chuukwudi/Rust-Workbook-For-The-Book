# Chapter 13: Functional Language Features - Workbook

## Chapter Summary

This chapter explores Rust's functional programming features, specifically closures and iterators:

1. **Closures**: Anonymous functions that can capture their environment
2. **Iterators**: A way to process sequences of elements one at a time
3. **Performance**: Understanding how these high-level abstractions compile down to efficient code

These features allow you to write more expressive, concise code while maintaining Rust's performance characteristics.

## Key Concepts

### 1. Closures

Closures are anonymous functions that can be assigned to variables and passed as arguments. Unlike regular functions, closures can capture values from their environment.

```rust
// Basic closure syntax
let add_one = |x| x + 1;

// Closure with type annotations
let add_one: fn(i32) -> i32 = |x: i32| -> i32 { x + 1 };

// Closure that captures its environment
let x = 4;
let equal_to_x = |z| z == x;
assert!(equal_to_x(4));
```

#### Closure Trait Implementations

Closures automatically implement one or more of these traits based on how they interact with captured values:

1. `FnOnce`: Consumes captured values, can be called only once
2. `FnMut`: Borrows values mutably, can be called multiple times
3. `Fn`: Borrows values immutably, can be called multiple times

```rust
// FnOnce - moves a value out of the environment
let s = String::from("hello");
let take_ownership = move || {
    println!("{}", s);
}; // After calling take_ownership, s is no longer available

// FnMut - mutates captured values
let mut counter = 0;
let mut increment = || {
    counter += 1;
};

// Fn - only reads captured values
let message = String::from("Hello");
let print_message = || {
    println!("{}", message);
};
```

### 2. Iterators

Iterators provide a sequence of values and process them one at a time. They are lazy, meaning they have no effect until consumed.

```rust
// Creating an iterator
let v = vec![1, 2, 3];
let v_iter = v.iter(); // Iterator over immutable references

// Consuming an iterator with a for loop
for val in v_iter {
    println!("{}", val);
}

// Using the next method directly
let mut v_iter = v.iter();
assert_eq!(v_iter.next(), Some(&1));
assert_eq!(v_iter.next(), Some(&2));
assert_eq!(v_iter.next(), Some(&3));
assert_eq!(v_iter.next(), None);
```

#### Iterator Types

- `iter()`: Borrows each element of the collection (returns `&T`)
- `into_iter()`: Takes ownership of the collection (returns `T`)
- `iter_mut()`: Mutably borrows each element (returns `&mut T`)

#### Iterator Adapters

Iterator adapters create new iterators with modified behavior:

```rust
// map - transform each element
let v: Vec<i32> = vec![1, 2, 3];
let v2: Vec<i32> = v.iter().map(|x| x + 1).collect();
assert_eq!(v2, vec![2, 3, 4]);

// filter - select elements based on a condition
let v: Vec<i32> = vec![1, 2, 3, 4, 5];
let evens: Vec<&i32> = v.iter().filter(|&&x| x % 2 == 0).collect();
assert_eq!(evens, vec![&2, &4]);

// chain - combine iterators
let v1 = vec![1, 2];
let v2 = vec![3, 4];
let chained: Vec<&i32> = v1.iter().chain(v2.iter()).collect();
assert_eq!(chained, vec![&1, &2, &3, &4]);
```

### 3. Performance of Closures and Iterators

Rust's compiler optimizes closures and iterators to be as efficient as manually written loops. They are "zero-cost abstractions", meaning they don't add runtime overhead.

## Practice Exercises

### Exercise 1: Basic Closures
Write closures to perform simple operations.

**Tasks:**
1. Create a closure that adds two numbers and call it with different arguments
2. Create a closure that doubles a number
3. Create a closure that computes the factorial of a number
4. Create a closure that checks if a number is prime

### Exercise 2: Closures That Capture Environment
Practice creating closures that capture variables from their environment.

**Tasks:**
1. Create a counter closure that increments an external variable each time it's called
2. Create a configurator closure that uses an external threshold to decide if a number passes a test
3. Create a closure that appends to an external vector
4. Create a closure that checks if a value is in an external set

### Exercise 3: Understanding Closure Traits
Practice with the different closure traits.

**Tasks:**
1. Create an example where only `FnOnce` trait is required
2. Create an example where `FnMut` trait is required
3. Create an example where `Fn` trait is sufficient
4. Create a function that accepts different types of closures using generic parameters

### Exercise 4: Basic Iterators
Practice creating and using iterators.

**Tasks:**
1. Create a vector and iterate over its values using a for loop
2. Use `iter()`, `into_iter()`, and `iter_mut()` and note the differences
3. Create a function that uses `next()` to manually process an iterator
4. Create a function that returns an iterator

### Exercise 5: Iterator Adapters
Practice using iterator adapters to transform data.

**Tasks:**
1. Use `map` to transform a collection of numbers
2. Use `filter` to select elements based on a condition
3. Use `zip` to combine two collections
4. Chain multiple iterator adapters together (`map`, `filter`, and `collect`)

### Exercise 6: Custom Iterator
Create a custom iterator by implementing the `Iterator` trait.

**Tasks:**
1. Create a `Counter` struct that implements `Iterator`
2. Implement a Fibonacci sequence iterator
3. Create an iterator that yields all permutations of a set
4. Create an iterator that yields a "sliding window" over a collection

### Exercise 7: Rewriting Loops as Iterators
Practice converting traditional loops to iterator-based code.

**Tasks:**
1. Rewrite a for loop that sums numbers as an iterator chain
2. Rewrite nested loops using iterators
3. Rewrite a loop that builds a map/dictionary using iterator methods
4. Rewrite a complex data transformation operation with iterators

### Exercise 8: The `minigrep` Refactor
Apply what you've learned to improve the `minigrep` project from Chapter 12.

**Tasks:**
1. Update the `Config::build` function to use an iterator instead of indexing into a slice
2. Refactor the `search` function to use iterator adapters
3. Refactor the `search_case_insensitive` function using iterators
4. Add a new feature to `minigrep` that uses iterators in its implementation

### Exercise 9: Advanced Iterator Techniques
Explore more advanced iterator patterns.

**Tasks:**
1. Implement a function that finds the maximum value in a collection using iterators
2. Create a function that groups elements of a vector by a key function
3. Implement a function that returns the unique elements of a collection
4. Create a function that returns the Cartesian product of two collections

### Exercise 10: Performance Testing
Compare the performance of loops and iterators.

**Tasks:**
1. Write two versions of a function that computes the sum of squares - one using a for loop and one using iterators
2. Write two versions of a function that finds all prime numbers up to n
3. Write two versions of a function that parses and transforms text
4. Use benchmark tests to compare the performance of your implementations

## Challenge Project: Data Pipeline

Create a data processing pipeline that reads data from a CSV file, transforms it, and outputs results. Your pipeline should:

1. Read a CSV file containing numerical data
2. Parse each line into a struct using iterators
3. Apply multiple transformations to the data using closures and iterator adapters
4. Filter the data based on configurable criteria
5. Sort the results using custom comparators implemented as closures
6. Group and aggregate the data
7. Output the results in a formatted manner

Use closures for configuration and transformations, and iterators for all data processing. The solution should be modular, efficient, and demonstrate the power of functional programming features in Rust.

---

## Additional Resources

- [Iterator documentation](https://doc.rust-lang.org/std/iter/trait.Iterator.html) - The official Rust documentation for the Iterator trait
- [Closures examples](https://doc.rust-lang.org/rust-by-example/fn/closures.html) - More examples of closures in Rust by Example
- [Iterators in Rust](https://blog.logrocket.com/rust-iterators-guide/) - An in-depth guide to iterators
- [Zero-cost abstractions in Rust](https://boats.gitlab.io/blog/post/zero-cost-abstractions/) - More about how Rust achieves zero-cost abstractions

Remember that mastering closures and iterators takes practice, but once you're comfortable with them, they'll make your code more concise, expressive, and often more readable. They're a fundamental part of idiomatic Rust!