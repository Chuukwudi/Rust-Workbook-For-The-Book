# Chapter 7: Managing Growing Projects with Packages, Crates, and Modules - Workbook

## Chapter Summary

This chapter covered Rust's module system for organizing code in large projects:

1. **Packages and Crates**:
   - A **package** is a bundle of one or more crates with a Cargo.toml file
   - A **crate** is a tree of modules that produces a library or executable
   - A package can contain at most one library crate and any number of binary crates
   - Conventions for crate locations: src/main.rs (binary) and src/lib.rs (library)

2. **Modules**:
   - **Modules** allow organizing code into groups with controlled visibility
   - The module tree starts from the crate root (src/main.rs or src/lib.rs)
   - Child modules can be defined inline or in separate files
   - Code is private by default and can be made public with `pub`

3. **Paths**:
   - **Absolute paths** start from the crate root with `crate::`
   - **Relative paths** start from the current module
   - The `super::` keyword refers to the parent module

4. **Visibility**:
   - Items are private by default
   - Use `pub` to make modules, functions, structs, and enums public
   - For structs, individual fields must be marked `pub` 
   - For enums, making the enum `pub` makes all variants public

5. **Bringing Paths into Scope**:
   - The `use` keyword creates shortcuts to items
   - The `as` keyword renames imports
   - `pub use` re-exports items, making them part of your public API
   - Nested paths can be used to organize imports
   - The glob operator (`*`) brings all items from a path into scope

6. **Separating Code into Files**:
   - Modules can be moved to separate files as they grow
   - File structure generally follows the module tree structure

## Exercises

### Exercise 1: Package and Crate Basics

**Task 1.1: Create a New Package**
1. Create a new package called `calculator` using `cargo new calculator`
2. Verify the package structure includes src/main.rs and Cargo.toml
3. Run the default "Hello, world!" program

**Task 1.2: Add a Library Crate**
1. Create a src/lib.rs file in your calculator package
2. Add a simple function `add(a: i32, b: i32) -> i32` that returns the sum of two integers
3. Make the function public with the `pub` keyword

**Task 1.3: Connect the Binary and Library Crates**
1. In src/main.rs, use the `add` function from your library crate
2. Update main to take two command-line arguments, parse them as numbers, and display their sum
3. Test your program with various inputs

### Exercise 2: Creating a Module Structure

**Task 2.1: Basic Module Structure**
1. In src/lib.rs, create a module structure for a calculator with these modules:
   - `arithmetic` (addition, subtraction, multiplication, division)
   - `scientific` (power, square root, logarithm)
   - `conversion` (temperature, length, weight)
2. Define simple functions in each module
3. Ensure all modules and functions are properly exported

**Task 2.2: Nested Modules**
1. Add submodules to your `conversion` module:
   - `temperature` (celsius_to_fahrenheit, fahrenheit_to_celsius)
   - `length` (meters_to_feet, feet_to_meters)
   - `weight` (kg_to_pounds, pounds_to_kg)
2. Implement the functions with appropriate signatures and return types
3. Make sure the nested modules are properly exported

**Task 2.3: Documentation**
1. Add documentation comments to your modules and functions
2. Generate and view the documentation using `cargo doc --open`
3. Make sure all public items are properly documented

### Exercise 3: Paths and Visibility

**Task 3.1: Using Different Path Types**
1. In src/main.rs, import and use functions from your library using:
   - Absolute paths starting with `crate::`
   - Relative paths
   - Paths with `super::`
2. Create examples that demonstrate each type of path

**Task 3.2: Working with Privacy**
1. Add private helper functions to your modules
2. Test that these functions can be used within their modules
3. Verify that they cannot be accessed from outside their modules
4. Create tests that ensure public functions work as expected

**Task 3.3: Struct and Enum Privacy**
1. Add a public struct `Calculator` with private and public fields
2. Add a public enum `Operation` with variants for arithmetic operations
3. Implement methods for the struct that use the enum
4. Test the privacy rules for struct fields and enum variants

### Exercise 4: Using the `use` Keyword

**Task 4.1: Basic Imports**
1. Use the `use` keyword to bring functions from your modules into scope
2. Follow Rust's idiomatic practices for importing functions vs. structs/enums
3. Compare code with and without `use` to see the difference in readability

**Task 4.2: Renaming and Re-exporting**
1. Use the `as` keyword to rename imports that might conflict
2. Use `pub use` to re-export items from internal modules
3. Create a clean facade API that hides the internal module structure

**Task 4.3: Organizing Imports**
1. Use nested paths to organize your imports
2. Try the glob operator to import everything from a module
3. Implement best practices for organizing imports in your code

### Exercise 5: Separating Code into Files

**Task 5.1: Moving Modules to Files**
1. Move each top-level module to its own file
2. Update your code to use the new file structure
3. Ensure all tests still pass after the refactoring

**Task 5.2: Moving Nested Modules**
1. Move the nested modules to appropriate files and directories
2. Use both styles for file paths (mod.rs and non-mod.rs)
3. Compare the two styles and note their differences

**Task 5.3: Complete Project Organization**
1. Organize your entire calculator library using proper file structure
2. Create a clear and intuitive public API
3. Add more functionality while maintaining the organized structure

### Exercise 6: Practical Applications

**Task 6.1: Calculator CLI**
1. Enhance your binary crate to provide a command-line interface for the calculator
2. Support all operations from your library
3. Add proper error handling and user feedback

**Task 6.2: External Dependencies**
1. Add the `rand` crate as a dependency
2. Create a `random` module with functions that use randomness
3. Properly import and use functionality from the external crate

**Task 6.3: Building a Library for Others**
1. Design a clean and intuitive public API for your calculator library
2. Use re-exports to create a simpler interface
3. Write comprehensive documentation for users of your library
4. Create examples showing how to use your library

### Exercise 7: Challenge Project - Building a Data Processing Library

Create a data processing library with the following features:

1. **Module Structure**:
   - `input`: Reading data from different sources (files, strings, etc.)
   - `parse`: Parsing data in different formats (CSV, JSON, etc.)
   - `transform`: Functions for transforming and cleaning data
   - `analysis`: Basic statistical analysis functions
   - `output`: Functions for outputting processed data

2. **Implementation Requirements**:
   - Proper privacy controls
   - Clear module organization
   - Well-designed public API
   - Comprehensive documentation
   - Tests for all public functionality

3. **Binary Application**:
   - Create a command-line tool that uses your library
   - Allow users to process data files using your library's features
   - Implement proper error handling and user feedback

This challenge project will give you practice with all the concepts covered in this chapter in a real-world context. Remember to focus on designing a clean and intuitive API while maintaining proper organization internally.

## Bonus Exercise: Exploring Standard Library Organization

Explore how the Rust standard library is organized:

1. Look at the documentation for key modules like `std::collections`, `std::io`, etc.
2. Observe how functionality is grouped into modules
3. Note how re-exports are used to create a clean public API
4. Write a short report on the organization patterns you observe and what you can learn from them for your own projects