# Chapter 6: Enums and Pattern Matching - Workbook

## Chapter Summary

This chapter introduced enums (enumerations) and pattern matching, which are powerful features in Rust for expressing different types of data and handling different cases:

1. **Enums**:
   - Custom types that can be one of several variants
   - Can hold different types and amounts of data in each variant
   - Provide type safety by representing a value that can be only one of a set of defined possibilities

2. **The Option Enum**:
   - Represents a value that can be either something (`Some`) or nothing (`None`)
   - Safer alternative to null values, making the possibility of absence explicit
   - Requires explicit handling before using the contained value

3. **Pattern Matching with match**:
   - Powerful control flow construct that compares a value against patterns
   - Allows extracting data from enum variants
   - Exhaustive matching ensures all possible cases are handled
   - Uses catch-all patterns with variables or the `_` placeholder

4. **Concise Control Flow**:
   - `if let` syntax for handling a single pattern while ignoring others
   - `let else` syntax for staying on the "happy path" in functions

## Exercises

### Exercise 1: Basic Enum Definition and Usage

**Task 1.1: Define a Simple Enum**
1. Define an enum `Season` with variants for the four seasons
2. Create variables for each season
3. Write a function that takes a `Season` and returns a string describing typical weather for that season

**Task 1.2: Enums with Associated Data**
1. Define an enum `Temperature` with variants:
   - `Celsius(f32)`
   - `Fahrenheit(f32)`
2. Create instances of both variants
3. Write a function that converts any `Temperature` to Celsius

**Task 1.3: Complex Enum**
1. Define an enum `Shape` with variants:
   - `Circle` with a radius field
   - `Rectangle` with width and height fields
   - `Triangle` with base and height fields
2. Create instances of each shape
3. Write a function that calculates the area of any shape

### Exercise 2: Working with the Option Enum

**Task 2.1: Using Option to Represent Optional Values**
1. Write a function `find_divisible` that takes two parameters: a vector of integers and a divisor
2. Have the function return `Option<i32>` representing the first number in the vector divisible by the divisor
3. If no such number exists, return `None`
4. Test your function with various inputs

**Task 2.2: Handling Option Values**
1. Write a function `double_optional` that takes an `Option<i32>` and returns:
   - `Some` with the value doubled if the input is `Some`
   - `None` if the input is `None`
2. Test your function with both `Some` and `None` inputs

**Task 2.3: Chaining Option Operations**
1. Write a function that:
   - Takes a string representing a number
   - Attempts to parse it to an integer
   - If successful, adds 10 to the number
   - Returns an `Option<i32>` with the result
2. Use the `.map()` method on `Option` to simplify your code

### Exercise 3: Pattern Matching with match

**Task 3.1: Basic Pattern Matching**
1. Define an enum `Coin` with variants for different coins (penny, nickel, dime, quarter)
2. Write a function `value_in_cents` that returns the value in cents for each coin
3. Use a `match` expression to handle each variant

**Task 3.2: Pattern Matching with Bound Values**
1. Modify your `Coin` enum to have the `Quarter` variant include a `UsState` value
2. Update your `value_in_cents` function to handle quarters with states
3. Print a special message when a quarter from a specific state is encountered

**Task 3.3: Exhaustive Matching**
1. Write a function that takes an `Option<i32>` and:
   - Increments the value by 1 if it's `Some`
   - Returns `None` if it's `None`
2. Try to handle only the `Some` case and observe the compiler error
3. Fix your function to handle all cases

**Task 3.4: Match with Guards**
1. Write a function that takes an integer and returns a string:
   - "Small negative" if the number is negative and less than -10
   - "Small positive" if the number is positive and less than 10
   - "Big negative" if the number is negative and at least -10
   - "Big positive" if the number is positive and at least 10
   - "Zero" if the number is 0
2. Use match expressions with guards to implement this

### Exercise 4: Concise Control Flow

**Task 4.1: Using if let**
1. Write a function that takes an `Option<String>` and prints:
   - The string in uppercase if it's `Some`
   - Nothing if it's `None`
2. Implement this using:
   - First, a `match` expression
   - Then, an `if let` expression
3. Compare the two implementations

**Task 4.2: Combining if let with else**
1. Write a function that counts non-quarter coins in a collection:
   - Takes a vector of `Coin` enum values
   - Counts all non-quarter coins
   - Prints state information for quarters
2. Implement this using both:
   - A `match` expression
   - An `if let` with `else`

**Task 4.3: Using let else**
1. Write a function `extract_positive` that:
   - Takes an `Option<i32>`
   - Returns the value if it's positive
   - Returns a default value (e.g., 0) if the option is `None` or contains a non-positive number
2. Implement this using:
   - First, an `if let` approach
   - Then, a `let else` approach

### Exercise 5: Practical Applications

**Task 5.1: Message Processor**
1. Define an enum `Message` with these variants:
   - `Quit` - no data
   - `Move { x: i32, y: i32 }` - named fields like a struct
   - `Write(String)` - includes a String
   - `ChangeColor(i32, i32, i32)` - includes three i32 values
2. Implement methods on `Message` to:
   - Process each type of message appropriately
   - Return a description of the action taken

**Task 5.2: Result Type**
1. Learn about the `Result<T, E>` enum in the standard library
2. Write a function `divide` that takes two integers and:
   - Returns `Ok(result)` if division is possible
   - Returns `Err("division by zero")` if the divisor is zero
3. Use pattern matching to handle the result appropriately

**Task 5.3: Advanced Pattern Matching**
1. Create an enum `JSON` to represent simple JSON data:
   - `Null`
   - `Boolean(bool)`
   - `Number(f64)`
   - `String(String)`
   - `Array(Vec<JSON>)`
   - `Object(HashMap<String, JSON>)`
2. Write a function that takes a `JSON` value and returns a string representation
3. Use nested pattern matching to handle the different cases

### Exercise 6: Challenge Problems

**Task 6.1: Custom Calculator**
1. Define an enum `Operation` with variants:
   - `Add(f64, f64)`
   - `Subtract(f64, f64)`
   - `Multiply(f64, f64)`
   - `Divide(f64, f64)`
   - `Power(f64, f64)`
2. Implement a function `calculate` that:
   - Takes an `Operation`
   - Returns a `Result<f64, String>` with the result or an error message
   - Handles division by zero and other potential errors

**Task 6.2: State Machine**
1. Design a vending machine as a state machine using enums:
   - Define an enum `VendingMachineState` with variants for different states
   - Define an enum `VendingMachineInput` for different inputs (coin insertion, selection, etc.)
   - Write a function that takes the current state and an input and returns the new state
   - Use pattern matching to handle state transitions

**Task 6.3: Expression Evaluator**
1. Create an enum `Expr` to represent simple mathematical expressions:
   - `Number(f64)`
   - `Add(Box<Expr>, Box<Expr>)`
   - `Subtract(Box<Expr>, Box<Expr>)`
   - `Multiply(Box<Expr>, Box<Expr>)`
   - `Divide(Box<Expr>, Box<Expr>)`
2. Write a function `evaluate` that takes an `Expr` and returns a `Result<f64, String>`
3. Use recursive pattern matching to evaluate complex expressions

## Bonus Exercise: Mini Library Management System

Create a small library management system:

1. Define enums for:
   - `BookStatus` (Available, Borrowed, Under Repair, Lost)
   - `MediaType` (Book, Magazine, DVD, AudioBook)
   - `SearchResult` (Found with item details, NotFound with reason)

2. Define structs for:
   - `LibraryItem` with fields for ID, title, media type, and status
   - `Member` with fields for ID, name, and borrowed items

3. Implement functions for:
   - Adding new items to the library
   - Borrowing and returning items
   - Searching for items by title or ID
   - Checking a member's borrowed items

4. Use pattern matching throughout to handle different cases

This exercise will give you practice combining enums with structs and using pattern matching in a realistic scenario.