# Chapter 5: Using Structs to Structure Related Data - Workbook

## Chapter Summary

This chapter introduced structs, a fundamental data type in Rust that allows you to create custom types that group related values together. Key concepts covered:

1. **Struct Definition and Instantiation**:
   - Creating custom data types with named fields
   - Instantiating structs with specific values
   - Using the dot notation to access fields
   - Making instances mutable to change field values

2. **Struct Features**:
   - Field init shorthand syntax for cleaner code
   - Struct update syntax to create instances from other instances
   - Tuple structs for creating named tuples with their own types
   - Unit-like structs for implementing traits without data

3. **Ownership with Structs**:
   - Using owned types like `String` rather than references in struct fields
   - Understanding lifetime requirements for references in structs

4. **Methods and Associated Functions**:
   - Implementing methods on structs using `impl` blocks
   - Using `self` to access the instance's data
   - Creating associated functions for constructors and utility functions
   - Automatic referencing and dereferencing when calling methods

5. **Debugging with Structs**:
   - Using `#[derive(Debug)]` to enable printing struct instances
   - Using `:?` and `:#?` formatting specifiers
   - Using the `dbg!` macro for debugging

## Exercises

### Exercise 1: Basic Struct Definition and Usage

**Task 1.1: Person Struct**
1. Define a struct named `Person` with the following fields:
   - `name` of type `String`
   - `age` of type `u32`
   - `email` of type `String`
2. Create an instance of `Person` with your information
3. Print the name and age of the person using dot notation

**Task 1.2: Mutable Struct Instance**
1. Create a mutable instance of the `Person` struct
2. Change the email field after creation
3. Print the updated email

**Task 1.3: Create a Function that Returns a Struct**
1. Write a function named `create_person` that takes name, age, and email as parameters
2. Use the field init shorthand syntax where appropriate
3. Return a new `Person` instance
4. Test your function by creating a person and printing their details

### Exercise 2: More Struct Types

**Task 2.1: Tuple Structs**
1. Define a tuple struct named `Point` that contains two `f64` values for x and y coordinates
2. Define a tuple struct named `Color` that contains three `u8` values for RGB
3. Create instances of both and demonstrate how to access their fields

**Task 2.2: Unit-Like Structs**
1. Define a unit-like struct named `AlwaysEqual`
2. Create an instance of it and print a message demonstrating its existence

**Task 2.3: Struct Update Syntax**
1. Create a `Person` instance named `person1`
2. Use the struct update syntax to create `person2` with the same name but different age and email
3. Verify that you can't use `person1` after this operation if its `name` field was moved

### Exercise 3: Implementing Methods

**Task 3.1: Basic Methods**
1. Define a `Rectangle` struct with `width` and `height` fields of type `u32`
2. Implement an `area` method that returns the rectangle's area
3. Implement a `perimeter` method that returns the rectangle's perimeter
4. Create a rectangle instance and call both methods

**Task 3.2: Methods with Parameters**
1. Add a `scale` method to `Rectangle` that takes a factor of type `f64` and returns a new scaled `Rectangle`
2. Demonstrate the method by creating a rectangle and scaling it by different factors

**Task 3.3: Self Ownership in Methods**
1. Add a method to `Rectangle` that takes ownership of `self` (not a reference)
2. Make this method transform the rectangle into a string description and return it
3. Demonstrate what happens when you try to use the rectangle after calling this method

### Exercise 4: Associated Functions

**Task 4.1: Constructors**
1. Add an associated function called `new` to `Rectangle` that creates a rectangle from width and height parameters
2. Add an associated function called `square` that creates a square with the given side length
3. Create instances using both of these functions

**Task 4.2: Utility Functions**
1. Add an associated function to `Rectangle` that returns the minimum dimensions needed to have a given area
2. Implement this function to return a square (equal width and height)
3. Test it with different area values

### Exercise 5: Working with Debug

**Task 5.1: Debug Trait**
1. Add the `Debug` trait to your `Rectangle` struct using the derive attribute
2. Print your rectangle using both the `:?` and `:#?` format specifiers
3. Observe the difference in output

**Task 5.2: Using `dbg!` Macro**
1. Create a new rectangle with some calculation for the width
2. Use the `dbg!` macro to debug the calculation
3. Also use `dbg!` to inspect the entire rectangle
4. Compare this with using `println!` with debug formatting

### Exercise 6: Complex Struct Relationships

**Task 6.1: Struct Composition**
1. Define a `Address` struct with fields for street, city, state, and zip code
2. Update your `Person` struct to include an `address` field of type `Address`
3. Create a person with an address and access nested fields

**Task 6.2: Methods with Other Structs**
1. Add a `can_fit_inside` method to `Rectangle` that takes another `Rectangle` as a parameter
2. The method should return true if `self` can fit inside the other rectangle
3. Test this with various rectangle combinations

**Task 6.3: Multiple impl Blocks**
1. Split your `Rectangle` implementation into multiple `impl` blocks
2. Group related methods together (e.g., one block for construction, one for calculations)
3. Verify everything still works correctly

### Exercise 7: Practical Application

**Task 7.1: Building a Simple Inventory System**
1. Define a `Product` struct with fields for name, price, and quantity
2. Implement methods for:
   - calculating the total value of the product (price × quantity)
   - updating the quantity
   - applying a discount to the price
3. Create a vector of products representing an inventory
4. Calculate the total value of the inventory using your methods

**Task 7.2: Enhanced Rectangle Application**
1. Create a program that:
   - Defines a `Rectangle` struct with width, height, and an optional color field
   - Implements various methods for calculating properties (area, diagonal length, etc.)
   - Allows creating rectangles in different ways (from dimensions, from area, etc.)
   - Includes a method to check if a point (x,y) is inside the rectangle
2. Create multiple rectangles and demonstrate all functionality

### Exercise 8: Challenge Problem

**Task 8.1: Shape Library**
1. Design a system of structs for different shapes:
   - `Circle` with radius
   - `Rectangle` with width and height
   - `Triangle` with base and height
2. Implement methods for each shape to calculate:
   - Area
   - Perimeter/circumference
3. Create a function that takes any shape and returns its area
4. Create an array of different shapes and calculate the total area

**Task 8.2: Bank Account System**
1. Design a `BankAccount` struct with:
   - Account number
   - Owner name
   - Balance
   - Account type (use an enum)
2. Implement methods for:
   - Depositing money
   - Withdrawing money (with validation)
   - Transferring money to another account
   - Calculating interest based on account type
3. Create a program that demonstrates all these operations
4. Include error handling for invalid operations (e.g., withdrawing more than the balance)

## Bonus Exercise: Putting It All Together

Create a simple task management system with the following features:
1. A `Task` struct with fields for description, due date, priority, and completion status
2. A `TaskList` struct that contains a collection of tasks
3. Methods for:
   - Adding tasks to the list
   - Marking tasks as complete
   - Filtering tasks by completion status or priority
   - Sorting tasks by due date or priority
4. A function to display tasks in a formatted way
5. A demonstration program that creates a task list, adds several tasks, and shows various operations

This project will give you practice combining structs, methods, associated functions, and collection types into a cohesive system.