# Workbook Exercises

---

## 1. Variables and Mutability

- Create a variable `x` with an initial value of `5`. Try changing its value to `6`.
  - **Question:** What happens if `mut` is not used?
- Declare a constant named `PI` with the value `3.14159`.
  - **Task:** Try changing its value and observe the error.

---

## 2. Shadowing

- Declare a variable `y` with the value `10`. Then, shadow it by adding `5` to its value. Print the final value of `y`.
- Try changing the type of a variable using shadowing:
  - Example: Shadow a string `"123"` with its parsed integer value.

---

## 3. Data Types

- Create an array of the first 5 prime numbers. Access the third element and print it.
- Define a tuple containing:
  - Your name (string),
  - Your age (integer),
  - Your height (floating-point number).
  - Destructure the tuple to print each value individually.

---

## 4. Functions

- Write a function `add` that takes two integers and returns their sum.
- Create a function `convert_to_celsius` that takes a temperature in Fahrenheit and returns the Celsius equivalent.
  - **Formula:** `(Fahrenheit - 32) * 5 / 9`.

---

## 5. Statements and Expressions

- Assign the result of the expression `3 * (2 + 5)` to a variable `z` and print it.
- Write a function that uses a block expression to calculate the square of a number.

---

## 6. Control Flow

- Write a program that takes a number and prints:
  - `"Positive"` if it’s greater than 0.
  - `"Negative"` if it’s less than 0.
  - `"Zero"` otherwise.
- Modify the program to include an `else if` branch that checks if the number is even or odd.

---

## 7. Loops

- Use a loop to print `"Rust is great!"` 5 times.
- Use a `while` loop to calculate the factorial of a given number.
- Use a `for` loop to iterate over an array of integers and print their squares.

---

## 8. Advanced Looping

- Create a program that counts down from 10 using a `for` loop and prints `"LIFTOFF!"` at the end.
- Use nested loops to print a multiplication table from 1 to 5.
  - Use **labeled loops** to exit both loops when a product exceeds 12.

---

## Bonus Challenges

1. Write a program that converts temperatures between Fahrenheit and Celsius using functions and user input.
2. Create a program to generate the first `n` Fibonacci numbers using a `while` loop.
3. Print the lyrics of *"The Twelve Days of Christmas"* song using a `for` loop and arrays for the verses.

---







# Chapter 3: Common Programming Concepts - Workbook

## Chapter Summary

This chapter covered fundamental programming concepts in Rust:

1. **Variables and Mutability**:
   - By default, variables in Rust are immutable
   - Use `mut` keyword to make variables mutable
   - Constants (`const`) are always immutable and require type annotation
   - Shadowing allows reusing variable names with different values or types

2. **Data Types**:
   - **Scalar Types**:
     - Integers: `i8`, `i16`, `i32`, `i64`, `i128`, `isize` (signed) and `u8`, `u16`, `u32`, `u64`, `u128`, `usize` (unsigned)
     - Floating-point: `f32` and `f64`
     - Boolean: `bool` with values `true` and `false`
     - Character: `char` represents a Unicode Scalar Value
   - **Compound Types**:
     - Tuples: Fixed-length collection of values with possibly different types
     - Arrays: Fixed-length collection of elements with the same type

3. **Functions**:
   - Defined with `fn` keyword followed by name and parameters
   - Parameters must have type annotations
   - Statements perform actions but don't return values
   - Expressions evaluate to a value
   - Return values are specified after `->` and can be implicitly returned as the last expression

4. **Comments**:
   - Line comments begin with `//`
   - Multi-line comments use `//` on each line

5. **Control Flow**:
   - **Conditional Statements**: `if`, `else if`, `else`
   - **Loops**: 
     - `loop`: Infinite loop until explicitly broken
     - `while`: Conditional loop
     - `for`: Iterating through collections
     - Loop labels to disambiguate nested loops

## Exercises

### Exercise 1: Variables and Mutability

**Task 1.1: Experimenting with Mutability**
1. Create a program that tries to modify an immutable variable
2. Observe the compiler error
3. Fix the code by making the variable mutable
4. Run the program to confirm it works

**Task 1.2: Constants vs Variables**
1. Create a program that defines a constant for the speed of light (299,792,458 m/s)
2. Use the constant in a calculation (e.g., how far light travels in one minute)
3. Print the result

**Task 1.3: Shadowing**
1. Write a program that:
   - Creates a variable called `value` with a string value
   - Shadows it with an integer
   - Shadows it again with a floating-point number
   - Prints each value after shadowing

### Exercise 2: Data Types

**Task 2.1: Working with Integer Types**
1. Create variables of different integer types
2. Perform operations with them (addition, subtraction, multiplication, division, remainder)
3. Observe what happens with operations between different integer types

**Task 2.2: Floating-Point Operations**
1. Create variables with `f32` and `f64` types
2. Perform the same operation with both types
3. Compare the results and precision

**Task 2.3: Character Display**
1. Create a program that defines several characters including:
   - ASCII characters
   - Unicode characters
   - Emoji
2. Print them to see how they display

**Task 2.4: Tuples and Arrays**
1. Create a tuple with at least 3 different types
2. Extract values using both destructuring and dot notation
3. Create an array of integers with a fixed length
4. Access elements by index
5. Try to access an element beyond the array bounds and observe the error

### Exercise 3: Functions

**Task 3.1: Function Parameters**
1. Write a function that takes two parameters of different types
2. Use the parameters in calculations or operations
3. Call the function with appropriate arguments

**Task 3.2: Return Values**
1. Write a function that returns the maximum of three integers
2. Use both explicit `return` and implicit return syntax
3. Call the function and use its result

**Task 3.3: Statements vs Expressions**
1. Create a function that uses a block expression to calculate a value
2. Show the difference between a statement with a semicolon and an expression without one
3. Use the block expression result in another calculation

### Exercise 4: Control Flow

**Task 4.1: If Expressions**
1. Write a program that uses an `if` expression to determine if a number is positive, negative, or zero
2. Use an `if` expression within a `let` statement to assign a value

**Task 4.2: Multiple Conditions with `else if`**
1. Write a program that:
   - Takes a student's score as input
   - Uses `else if` to determine the grade (A, B, C, D, F)
   - Prints the grade

**Task 4.3: Loop Practice**
1. Write a program using the `loop` keyword that:
   - Counts up from 1
   - Exits the loop when it reaches 10
   - Returns the final count from the loop
   - Prints the result

**Task 4.4: Nested Loops with Labels**
1. Create a program with nested loops that:
   - Uses loop labels
   - Uses `break` with a label to exit the outer loop
   - Uses `continue` to skip iterations

**Task 4.5: While Loops**
1. Write a program that uses a `while` loop to:
   - Generate a sequence (like a countdown)
   - Print each value in the sequence

**Task 4.6: For Loops**
1. Create a program that uses a `for` loop to:
   - Iterate through an array
   - Iterate through a range
   - Iterate through a range in reverse using `.rev()`

### Exercise 5: Practical Applications

**Task 5.1: Temperature Converter**
1. Create a program that converts temperatures between Fahrenheit and Celsius
2. Implement functions for each conversion direction
3. Use control flow to determine which conversion to perform
4. Return and display the result

**Task 5.2: Fibonacci Generator**
1. Write a program that generates the nth Fibonacci number
2. Implement it using a loop
3. Allow the user to specify n
4. Print the result

**Task 5.3: Simple Pattern Printer**
1. Create a program that prints patterns using nested loops
2. Examples of patterns could be:
   - A triangle of asterisks
   - A square of numbers
   - A checkerboard pattern

**Task 5.4: Simple Calculator**
1. Create a program that:
   - Takes two numbers and an operation as input
   - Uses control flow to determine which operation to perform
   - Returns and displays the result
   - Handles potential errors (like division by zero)

### Exercise 6: Challenge Problems

**Task 6.1: Prime Number Checker**
1. Write a function that determines if a number is prime
2. Use loops and control flow to check divisibility
3. Return a boolean result

**Task 6.2: Factorial Calculator**
1. Implement a function to calculate the factorial of a number
2. Use a loop or recursion
3. Handle potential overflow for large inputs

**Task 6.3: Simple Word Counter**
1. Define a string with multiple words
2. Write a function that counts the number of words
3. Use control flow to handle special cases (e.g., multiple spaces)

## Bonus Exercise: Putting It All Together

Create a small program that combines multiple concepts from this chapter:
1. Define variables and constants
2. Use different data types including compound types
3. Define and call functions with parameters and return values
4. Use control flow (conditionals and loops)
5. Add appropriate comments

For example, you could create a simple quiz game, a number guessing game with hints, or a basic text-based menu system.