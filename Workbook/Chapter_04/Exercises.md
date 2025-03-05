# Chapter 4: Understanding Ownership - Workbook

## Chapter Summary

Ownership is Rust's most distinctive feature and enables memory safety without a garbage collector. This chapter covered these key concepts:

1. **Ownership Rules**:
   - Each value in Rust has a single owner
   - When the owner goes out of scope, the value is dropped
   - There can only be one owner of a value at a time

2. **Memory Management**:
   - The stack vs. the heap: how Rust manages different types of memory
   - Variables with known, fixed size are stored on the stack
   - Data with dynamic size is stored on the heap

3. **Variable Interactions**:
   - Move semantics: when a value is assigned to another variable, ownership transfers
   - The original variable becomes invalid after a move
   - The `Copy` trait enables stack-only values to be duplicated instead of moved
   - The `clone()` method performs deep copies for heap data

4. **References and Borrowing**:
   - References allow you to refer to a value without taking ownership
   - Borrowing lets you use data without transferring ownership
   - References are immutable by default
   - You can have either one mutable reference OR any number of immutable references
   - References must always be valid (no dangling references)

5. **Slices**:
   - Slices reference a contiguous sequence of elements in a collection
   - String slices (`&str`) reference part of a string
   - Slices store a pointer to the first element and a length

## Exercises

### Exercise 1: Ownership Basics

**Task 1.1: Understanding Scope and Drop**
1. Create a function that:
   - Creates a `String` inside a scope 
   - Prints a message when the scope ends
   - Demonstrates how values are dropped when they go out of scope
2. Call this function and observe the behavior

**Task 1.2: Moves and Ownership Transfer**
1. Write a program that:
   - Creates a `String` variable
   - Moves ownership to another variable
   - Attempts to use the original variable (which will cause a compiler error)
   - Comment out the error-causing line and fix the program

**Task 1.3: The Copy Trait**
1. Create a program that:
   - Demonstrates which types implement the `Copy` trait (integers, booleans, etc.)
   - Demonstrates which types don't implement `Copy` (String, etc.)
   - Explains why the behavior is different in each case

### Exercise 2: Working with References

**Task 2.1: Immutable References**
1. Write a function that:
   - Takes an immutable reference to a `String`
   - Calculates and returns the length of the string
   - Does not take ownership of the string
2. Call this function and demonstrate that the original string is still usable after the function call

**Task 2.2: Mutable References**
1. Create a function that:
   - Takes a mutable reference to a `String`
   - Appends " - Modified" to the string
   - Does not take ownership
2. Call this function and show that the original string has been modified

**Task 2.3: Multiple References**
1. Write a program that demonstrates:
   - Creating multiple immutable references (which is allowed)
   - The rule that you can't have a mutable reference while having immutable references
   - Using scopes to allow multiple mutable references (not simultaneously)

**Task 2.4: Reference Validation**
1. Write a function that attempts to return a dangling reference
2. Observe the compiler error
3. Fix the function to properly return an owned value instead

### Exercise 3: Slices

**Task 3.1: String Slices**
1. Implement a function called `first_word` that:
   - Takes a string slice (`&str`) as a parameter
   - Returns the first word in the string as a slice
2. Test the function with both `String` and string literals

**Task 3.2: Improved First Word**
1. Extend your `first_word` function to handle:
   - Strings with no spaces (return the whole string)
   - Empty strings (return an empty slice)
   - Strings with multiple spaces

**Task 3.3: Last Word**
1. Implement a function called `last_word` that:
   - Takes a string slice as a parameter
   - Returns the last word in the string as a slice
2. Test your function with various inputs

**Task 3.4: Middle Word**
1. Create a function called `nth_word` that:
   - Takes a string slice and an index `n`
   - Returns the nth word in the string (0-indexed)
   - Returns None (using an Option type) if there aren't enough words

**Task 3.5: Array Slices**
1. Write a function that:
   - Takes a slice of integers
   - Returns the sum of the elements
2. Test the function with different array slices

### Exercise 4: Functions and Ownership

**Task 4.1: Ownership Transfer**
1. Create functions that demonstrate:
   - Taking ownership of a parameter
   - Returning ownership
   - Taking and returning ownership in the same function

**Task 4.2: References in Functions**
1. Rewrite a function that previously took ownership to:
   - Instead take a reference
   - Accomplish the same task without taking ownership

**Task 4.3: Mutable References in Functions**
1. Create a function that:
   - Takes a mutable reference to an array or vector
   - Modifies its contents (e.g., sorts the elements)
2. Show that the original array is modified after the function call

### Exercise 5: Practical Applications

**Task 5.1: Word Counter**
1. Create a function that:
   - Takes a string slice
   - Returns a count of words in the string
   - Uses string slices appropriately

**Task 5.2: String Trimmer**
1. Implement a function that:
   - Takes a string slice
   - Returns a new string slice with leading and trailing whitespace removed
   - Handles empty strings correctly

**Task 5.3: Text Analyzer**
1. Create a program that:
   - Takes a paragraph of text
   - Analyzes and reports:
     - Total number of words
     - Longest word
     - Average word length
   - Uses references and slices appropriately to avoid unnecessary copying

**Task 5.4: Sentence Splitter**
1. Implement a function that:
   - Takes a string slice containing multiple sentences
   - Returns a vector of string slices, each containing one sentence
   - Handles various sentence-ending punctuation (.!?)

### Exercise 6: Challenge Problems

**Task 6.1: String Builder**
1. Create a struct called `StringBuilder` that:
   - Allows appending strings
   - Efficiently builds a final string
   - Properly implements ownership rules
   - Has methods like `append`, `clear`, and `build`

**Task 6.2: Reference Counter**
1. Create a simple reference counting system:
   - A struct that holds data and a count of references
   - Functions to create, clone, and drop references
   - Logic to free the data when the reference count reaches zero

**Task 6.3: Safe Buffer**
1. Implement a buffer struct that:
   - Holds a fixed-size array of data
   - Provides safe access using slices
   - Prevents out-of-bounds access
   - Manages ownership correctly

## Bonus Exercise: Memory Management Library

Create a small library that demonstrates Rust's ownership model:
1. Implement various data structures that efficiently use Rust's ownership system
2. Create functions that demonstrate moving, borrowing, and slicing
3. Add a comprehensive test suite that verifies correctness
4. Include documentation explaining the ownership concepts demonstrated by each feature
5. Reflect on how Rust's ownership model differs from garbage collection or manual memory management

This exercise will test your understanding of Rust's ownership model and help you build intuition for how to use it effectively in real-world applications.