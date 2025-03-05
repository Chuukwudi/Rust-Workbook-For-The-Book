# Chapter 8: Common Collections - Workbook

## Chapter Summary

This chapter covered three of Rust's most important collection types, which are used to store multiple values:

1. **Vectors (`Vec<T>`)**:
   - Store multiple values of the same type in a contiguous memory allocation
   - Can grow or shrink in size at runtime
   - Access elements by index or iterate through them
   - Provide safe access with bounds checking

2. **Strings (`String`)**:
   - Store UTF-8 encoded text
   - Complex due to Unicode handling
   - Cannot be indexed by single values due to variable-width encoding
   - Provide methods for manipulating text safely

3. **Hash Maps (`HashMap<K, V>`)**:
   - Store key-value pairs for efficient lookups
   - Keys must be of the same type, and values must be of the same type
   - Useful for counting, caching, and associating data
   - Provide methods for insertion, updating, and retrieval

All these collections are stored on the heap, which means they can grow or shrink as the program runs, unlike arrays or tuples which have fixed sizes known at compile time.

## Exercises

### Exercise 1: Vectors

**Task 1.1: Creating and Updating Vectors**
1. Create an empty vector of integers using `Vec::new()`
2. Add five elements to the vector using `push()`
3. Create another vector with initial values using the `vec!` macro
4. Combine both vectors using `extend()`
5. Print the final vector's contents

**Task 1.2: Accessing Vector Elements**
1. Create a vector with elements 1 through 5
2. Access the third element using indexing syntax (`&v[2]`)
3. Access the third element using the `get` method (`v.get(2)`)
4. Try to access an element that's out of bounds using both methods
5. Handle the `Option<&T>` returned by `get` appropriately

**Task 1.3: Iterating Over Vectors**
1. Create a vector with some numerical values
2. Use a `for` loop to calculate the sum of all elements
3. Use a `for` loop with mutable references to double each value in the vector
4. Use an iterator with `map` and `collect` to create a new vector with each value tripled
5. Print both the original and the new vector

**Task 1.4: Storing Different Types in a Vector**
1. Define an enum `Shape` with variants `Circle(f64)`, `Rectangle(f64, f64)`, and `Triangle(f64, f64, f64)`
2. Create a vector that holds different shapes
3. Write a function that calculates the area of each shape
4. Use the function to compute and print the area of each shape in the vector

### Exercise 2: Strings

**Task 2.1: Creating and Manipulating Strings**
1. Create a new empty `String` using `String::new()`
2. Create a `String` from a string literal using `String::from()`
3. Create a `String` from a string literal using the `to_string()` method
4. Append text to the string using `push_str()` and `push()`
5. Concatenate two strings using the `+` operator
6. Use the `format!` macro to combine multiple strings
7. Print each result to observe the differences

**Task 2.2: Working with UTF-8**
1. Create strings containing characters from different languages (e.g., English, Spanish, Russian, Japanese)
2. Print the length of each string in bytes using `.len()`
3. Iterate through each string using `.chars()` and count the number of characters
4. Compare the byte length with the character count for each string
5. Explain why they might differ

**Task 2.3: String Slicing**
1. Create a string with both ASCII and non-ASCII characters
2. Create a slice of the first few characters using range syntax (`&s[0..4]`)
3. Try creating invalid slices that split UTF-8 characters and handle the potential panic
4. Use `.chars()` to safely extract substrings
5. Compare the different approaches

**Task 2.4: String Operations**
1. Write a function that reverses a string by characters (not bytes)
2. Write a function that counts the number of words in a string
3. Write a function that capitalizes the first letter of each word
4. Test these functions with strings containing characters from different languages

### Exercise 3: Hash Maps

**Task 3.1: Creating and Populating Hash Maps**
1. Create an empty hash map using `HashMap::new()`
2. Insert several key-value pairs (e.g., team names and scores)
3. Create a hash map from parallel vectors using `zip` and `collect`
4. Print the contents of each hash map
5. Check if a specific key exists in the hash map

**Task 3.2: Accessing and Updating Values**
1. Create a hash map of names and ages
2. Retrieve the age for a specific name using the `get` method
3. Update the age for an existing name
4. Use the `entry` API to insert a value only if the key doesn't exist
5. Use the `entry` API to update a value based on its old value

**Task 3.3: Ownership with Hash Maps**
1. Create a hash map with `String` keys and values
2. Demonstrate that ownership of the keys and values is moved to the hash map
3. Use references as values in the hash map
4. Explain the ownership implications of each approach

**Task 3.4: Word Counter**
1. Write a program that counts the frequency of each word in a text
2. Sort the words by frequency
3. Print the top 5 most common words and their counts
4. Handle case sensitivity appropriately

### Exercise 4: Practical Applications

**Task 4.1: Vector-Based Stack**
1. Implement a stack data structure using a vector
2. Include methods for `push`, `pop`, `peek`, and `is_empty`
3. Write a function that uses your stack to reverse a vector
4. Write a function that uses your stack to check if parentheses in a string are balanced

**Task 4.2: Contact Book with Hash Maps**
1. Create a contact book application using a hash map
2. Support adding, updating, and deleting contacts
3. Support looking up contacts by name
4. Add functionality to save and load the contact book from a file

**Task 4.3: Text Analysis**
1. Write a program that reads text and provides statistics:
   - Word count
   - Character count
   - Average word length
   - Most common words
   - Distribution of word lengths
2. Use appropriate collection types for each part of the analysis

### Exercise 5: Challenge Problems

**Task 5.1: Implementing a Median and Mode Calculator**
1. Write a function that takes a vector of integers and returns the median value
2. Write a function that takes a vector of integers and returns the mode (most frequent value)
3. Test these functions with various inputs including empty vectors, vectors with one element, and vectors with duplicate elements

**Task 5.2: Pig Latin Converter**
1. Write a function that converts a string to pig latin according to these rules:
   - For words beginning with a consonant, move the first consonant to the end and add "ay"
   - For words beginning with a vowel, add "hay" to the end
2. Handle both uppercase and lowercase letters appropriately
3. Preserve non-alphabetic characters
4. Test with various inputs including empty strings, single words, and sentences

**Task 5.3: Employee Management System**
1. Create a system that maintains a database of employees by department
2. Support adding employees to departments
3. Support listing all employees in a department
4. Support listing all employees in the company by department
5. Ensure the names are sorted alphabetically in the output

## Bonus Exercise: Implementing a Cache

Implement a simple LRU (Least Recently Used) cache using Rust's collections:

1. Create a struct called `LRUCache` with a specified capacity
2. Implement methods to add key-value pairs and retrieve values by key
3. When the cache reaches capacity, remove the least recently used item
4. When an item is accessed, update it to be the most recently used
5. Use a combination of a hash map for O(1) lookups and a vector to track usage order

This exercise will give you practice combining multiple collection types to build a more complex data structure with efficient operations.