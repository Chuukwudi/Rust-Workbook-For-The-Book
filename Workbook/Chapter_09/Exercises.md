# Chapter 9: Error Handling - Workbook

## Chapter Summary

This chapter explored Rust's robust error handling mechanisms, which are categorized into two main types:

1. **Unrecoverable Errors with `panic!`**:
   - Used for situations where code has reached an invalid state that can't be handled
   - Terminates the program immediately (or unwinds the stack in the default configuration)
   - Can be triggered explicitly via `panic!` or implicitly (e.g., array access out of bounds)
   - Useful for prototype code, examples, tests, and truly unrecoverable situations

2. **Recoverable Errors with `Result<T, E>`**:
   - Represented as an enum with two variants: `Ok(T)` for success and `Err(E)` for failure
   - Forces explicit handling of error cases via pattern matching
   - Provides methods like `unwrap`, `expect`, and `?` for different error handling styles
   - Enables error propagation throughout the call stack
   - Appropriate for most error cases where the calling code might want to handle the error

3. **Error Handling Guidelines**:
   - When to panic vs. when to return `Result`
   - How to utilize the type system for validation
   - Creating custom types for validation

## Exercises

### Exercise 1: Working with `panic!`

**Task 1.1: Inducing a Panic**
1. Write a program that deliberately triggers a panic in at least three different ways:
   - By calling the `panic!` macro explicitly
   - By accessing an array element beyond its bounds
   - By attempting to convert a string to a number when it contains non-numeric characters
2. Run the program with the `RUST_BACKTRACE=1` environment variable and examine the output
3. Note the differences in the error messages and backtraces for each case

**Task 1.2: Configuring Panic Behavior**
1. Create a simple program that panics
2. In the `Cargo.toml` file, add the `[profile.release]` section with the line `panic = 'abort'`
3. Build the program in release mode and observe how the panic behavior changes
4. Write a brief note about the differences between unwinding and aborting

**Task 1.3: Understanding Backtraces**
1. Create a program with several nested function calls that eventually causes a panic
2. Run the program with `RUST_BACKTRACE=1`
3. Analyze the backtrace to understand the execution path leading to the panic
4. Identify which part of your code triggered the panic and explain how you determined this

### Exercise 2: Error Handling with `Result`

**Task 2.1: Basic `Result` Handling**
1. Write a function that opens a file and returns a `Result<String, std::io::Error>` containing either the file's contents or an error
2. Use pattern matching on the `Result` to handle both success and failure cases
3. Demonstrate appropriate error messages for various failure cases (file not found, permission denied, etc.)

**Task 2.2: Using `unwrap` and `expect`**
1. Modify your file-opening function to use `unwrap` in one case
2. Create another version that uses `expect` with a meaningful error message
3. Create test scenarios where these calls succeed and fail
4. Comment on the differences in behavior and usage cases for each approach

**Task 2.3: Error Propagation**
1. Create a function that reads a configuration file, parses it, and returns a configuration object
2. Implement error propagation using pattern matching and early returns
3. Rewrite the same function using the `?` operator
4. Compare the two implementations and discuss the readability benefits of the `?` operator

**Task 2.4: Chaining `Result` Operations**
1. Write a function that must perform several operations that can fail (e.g., read a file, parse it, validate it)
2. Implement this using chained operations with the `?` operator
3. Add appropriate error handling at each step
4. Ensure the function returns a meaningful error if any step fails

### Exercise 3: Working with `Option` and `?`

**Task 3.1: Implementing a Function with `Option`**
1. Create a function that finds the last character of the first line of a string, returning `Option<char>`
2. Use the `?` operator to handle the `Option` values in the implementation
3. Test your function with inputs that should return `Some(char)` and `None`
4. Explain how the `?` operator works with `Option` compared to `Result`

**Task 3.2: Converting Between `Option` and `Result`**
1. Write a function that takes an `Option<T>` and converts it to a `Result<T, E>` with a custom error
2. Write another function that takes a `Result<T, E>` and converts it to an `Option<T>`
3. Demonstrate practical use cases for each conversion
4. Explain when and why you might need to convert between these types

### Exercise 4: Custom Error Types

**Task 4.1: Creating a Custom Error Type**
1. Define a custom error type for a specific domain (e.g., configuration errors, network errors)
2. Implement the `std::fmt::Display` and `std::fmt::Debug` traits for your error type
3. Implement the `std::error::Error` trait for your error type
4. Create functions that return your custom error type and demonstrate their usage

**Task 4.2: Error Type Conversion**
1. Extend your custom error type to include the `From` trait implementation for converting from standard library errors
2. Demonstrate how this enables using the `?` operator with different error types
3. Show an example where various error types are converted to your custom error type
4. Discuss the benefits of this approach for library code

**Task 4.3: Error Context**
1. Design an approach that allows adding context to errors as they propagate up the call stack
2. Implement a function that adds context information to errors it encounters
3. Demonstrate how this improves error reporting for the user
4. Compare this approach with simply returning the original errors

### Exercise 5: Practical Error Handling

**Task 5.1: Building a Robust File Processing Tool**
1. Create a program that processes a CSV file and performs some analysis on it
2. Implement comprehensive error handling for:
   - File opening and reading
   - CSV parsing
   - Data validation
   - Analysis calculations
3. Ensure the program reports useful error messages to the user
4. Implement appropriate error recovery strategies where possible

**Task 5.2: Command-Line Tool with Good Error Messages**
1. Build a simple command-line tool that takes arguments and performs an operation
2. Implement thorough error handling with custom error types
3. Make sure the error messages are user-friendly and include relevant context
4. Handle both user errors (e.g., invalid input) and system errors (e.g., file operations)

**Task 5.3: Main Function with `Result` Return Type**
1. Create a program whose `main` function returns `Result<(), Box<dyn Error>>`
2. Use the `?` operator throughout the main function for error propagation
3. Ensure all errors are properly propagated and reported
4. Discuss the advantages of this approach compared to handling errors directly in `main`

### Exercise 6: Validation with Types

**Task 6.1: Creating a Validation Type**
1. Create a custom type that guarantees its value is within a specific range (similar to the `Guess` type in the chapter)
2. Implement appropriate methods for creating and accessing instances of this type
3. Write a function that accepts only this validated type
4. Demonstrate how this approach prevents invalid values from being used

**Task 6.2: Multiple Validation Types**
1. Extend the previous exercise by creating several validation types for different constraints
2. Build a complex data structure that uses these validation types to ensure data integrity
3. Implement functions that operate on this data structure
4. Show how the type system helps prevent invalid states in your program

**Task 6.3: Generic Validation Type**
1. Design a generic validation type that can be parameterized with different validation rules
2. Implement this type and show how it can be used for various validation scenarios
3. Compare this approach with creating specific validation types for each case
4. Discuss the trade-offs between flexibility and compile-time safety

### Exercise 7: Challenge Problems

**Task 7.1: Hierarchical Error Handling**
1. Design an error handling system for a complex application with multiple modules
2. Create custom error types for each module that can be composed into higher-level error types
3. Implement context propagation through the error hierarchy
4. Demonstrate how this approach allows for both detailed error reporting and high-level error handling

**Task 7.2: Error Handling in an Async Context**
1. Research and discuss the challenges of error handling in asynchronous Rust code
2. Implement a simple async function that demonstrates proper error handling
3. Compare error handling approaches in synchronous and asynchronous code
4. Recommend best practices for error handling in async Rust

**Task 7.3: Library Design with Error Handling**
1. Design a small library with a public API
2. Implement appropriate error handling strategies for the library
3. Document the error handling behavior in the API documentation
4. Create examples showing how users of the library should handle errors

## Bonus Exercise: To `panic!` or Not to `panic!`

For each of the following scenarios, decide whether you would use `panic!` or `Result`, and explain your reasoning:

1. A function that divides two numbers
2. A function that validates a username during account creation
3. A function that accesses the 5th element of a vector
4. A library function for parsing a custom file format
5. A test helper function that creates temporary data
6. A function that performs cryptographic operations
7. A constructor for a data structure that requires certain invariants to hold
8. A function that performs network requests
9. A function that converts between different units of measurement
10. A function that allocates memory for a critical operation

This exercise will help you develop intuition for appropriate error handling strategies in different scenarios.