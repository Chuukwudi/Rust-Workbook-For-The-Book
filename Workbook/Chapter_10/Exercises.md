# Chapter 10: Generic Types, Traits, and Lifetimes - Workbook

## Chapter Summary

This chapter covered three fundamental Rust features that allow for powerful abstractions without runtime cost:

1. **Generic Types**:
   - Abstract placeholders for concrete types
   - Enable code reuse across different data types
   - Used in functions, structs, enums, and methods
   - Monomorphized at compile time for zero-cost abstractions

2. **Traits**:
   - Define shared behavior across types
   - Similar to interfaces in other languages but with unique capabilities
   - Allow for default implementations
   - Enable trait bounds to constrain generic types
   - Support conditional implementations

3. **Lifetimes**:
   - Ensure references remain valid
   - Prevent dangling references
   - Used in function signatures, structs, and impl blocks
   - Follow elision rules that reduce explicit annotation needs

These concepts work together to create flexible and safe abstractions while maintaining Rust's memory safety guarantees without garbage collection.

## Exercises

### Exercise 1: Generic Functions and Types

**Task 1.1: Generic Function**
1. Implement the `largest` function from the chapter that works for any type that implements `PartialOrd` and `Copy`
2. Test it with vectors of integers and characters
3. Explain why the trait bounds are necessary

**Task 1.2: Generic Point Struct**
1. Create a `Point<T>` struct that can hold x and y coordinates of any type
2. Implement methods for your `Point<T>` struct:
   - A constructor `new` that takes x and y values
   - A method `distance_from_origin` for `Point<f32>` that calculates the distance from (0.0, 0.0)
3. Create points with different numeric types and test your methods

**Task 1.3: Multiple Generic Parameters**
1. Modify your `Point` struct to accept two generic types `Point<T, U>` where x is of type T and y is of type U
2. Implement a `mixup` method that takes another Point and returns a new Point with x from self and y from the other point
3. Demonstrate using points with different types (e.g., combining a point with integer coordinates and a point with floating-point coordinates)

### Exercise 2: Traits 

**Task 2.1: Basic Trait Definition**
1. Define a `Summarizable` trait with a method `summarize` that returns a String
2. Create at least two structs that implement this trait:
   - `Article` with title, author, and content fields
   - `Tweet` with username and content fields
3. Have each implementation provide a custom summary

**Task 2.2: Default Implementations**
1. Add a default implementation to your `Summarizable` trait
2. Create a new type that relies on the default implementation
3. Override the default implementation for one of your existing types

**Task 2.3: Traits with Dependencies**
1. Modify your `Summarizable` trait to have a required method `content` and a default `summarize` method that uses `content`
2. Update your implementations to accommodate this change
3. Discuss how this design allows for code reuse while still requiring specific behavior from implementors

**Task 2.4: Trait Bounds**
1. Write a function `notify` that takes any type that implements `Summarizable` and prints its summary
2. Create another function that takes two parameters, both of which must implement `Summarizable` and `Display`
3. Demonstrate how to use trait bounds with `where` clauses for more complex constraints

### Exercise 3: Trait Objects and Dynamic Dispatch

**Task 3.1: Vector of Trait Objects**
1. Create a vector that can hold different types that implement the `Summarizable` trait
2. Add different instances to the vector (e.g., Articles and Tweets)
3. Iterate through the vector and call `summarize` on each item
4. Discuss the difference between this approach and using generics with trait bounds

**Task 3.2: Returning Trait Objects**
1. Create a function that returns a trait object implementing `Summarizable`
2. Make the function return different implementations based on some condition
3. Test your function with different inputs

### Exercise 4: Basic Lifetimes

**Task 4.1: Understanding the Borrow Checker**
1. Write a function that attempts to return a reference to a value created inside the function
2. Observe and explain the compilation error
3. Fix the function to return an owned value instead of a reference

**Task 4.2: Implementing the `longest` Function**
1. Implement the `longest` function that returns the longer of two string slices
2. Add the correct lifetime annotations
3. Test your function with string slices of different lifetimes
4. Create a scenario where the borrow checker prevents invalid usage

**Task 4.3: Lifetime in Structs**
1. Create a struct `Excerpt` that holds a string slice
2. Add the necessary lifetime annotations
3. Create instances of this struct and demonstrate valid and invalid usage patterns
4. Implement methods on this struct with appropriate lifetime annotations

### Exercise 5: Advanced Lifetimes

**Task 5.1: Multiple Lifetime Parameters**
1. Create a function that takes string slices with different lifetime parameters
2. Return a reference with an appropriate lifetime
3. Demonstrate when you would need distinct lifetime parameters rather than using the same one

**Task 5.2: Lifetime Elision**
1. Identify functions where lifetime annotations can be elided
2. Explain which elision rules apply in each case
3. Write the same functions with explicit lifetime annotations

**Task 5.3: Static Lifetime**
1. Create examples using the `'static` lifetime
2. Discuss appropriate and inappropriate uses of `'static`
3. Convert a function to use `'static` references and explain the implications

### Exercise 6: Bringing It All Together

**Task 6.1: Generic Data Structure with Lifetimes**
1. Implement a generic `Cache<T>` struct that stores a reference to a key and a value of type T
2. Add appropriate lifetime annotations
3. Implement methods for inserting and retrieving values
4. Demonstrate the usage of your cache with different types

**Task 6.2: Iterator Trait Implementation**
1. Create a struct that implements the `Iterator` trait
2. Use generics to make your iterator work with different types
3. Test your iterator with various types

**Task 6.3: Complete Library Crate**
1. Create a small library crate that demonstrates generics, traits, and lifetimes working together
2. Include at least:
   - A generic data structure
   - A trait with default methods
   - Functions that use trait bounds
   - Appropriate lifetime annotations
3. Write documentation explaining your design decisions
4. Add tests that verify the correctness of your code

### Exercise 7: Challenge Problems

**Task 7.1: Type-Safe Builder Pattern**
1. Implement a builder pattern for a complex struct using generics and traits
2. Use traits to enforce that required fields are set before building
3. Test your implementation with different configurations

**Task 7.2: Custom `Result<T, E>` Type**
1. Create your own version of the `Result<T, E>` enum
2. Implement methods like `map`, `and_then`, and `unwrap_or`
3. Demonstrate how your implementation can be used for error handling

**Task 7.3: Generic Tree Structure**
1. Implement a generic tree data structure
2. Add methods for inserting, traversing, and searching the tree
3. Use traits to allow comparing tree nodes
4. Ensure proper lifetime annotations for references within the tree

## Bonus Exercise: Performance Analysis

**Task B.1: Monomorphization Analysis**
1. Write a simple function using generics
2. Use `cargo expand` or another tool to examine the generated monomorphized code
3. Compare the performance of generic code vs. non-generic code with benchmarks
4. Discuss the tradeoffs between generics and dynamic dispatch with trait objects

**Task B.2: Optimization Techniques**
1. Implement two versions of the same algorithm:
   - One using generics with trait bounds
   - One using trait objects with dynamic dispatch
2. Benchmark both approaches with different types and data sizes
3. Analyze when each approach performs better and why