# Chapter 11: Writing Automated Tests - Workbook

## Chapter Summary

Chapter 11 covered Rust's robust testing framework, which helps ensure code correctness through automated tests. Key concepts include:

1. **Test Functions**:
   - Annotated with `#[test]` attribute
   - Run with `cargo test` command
   - Pass when they execute without panicking, fail when they panic

2. **Test Assertions**:
   - `assert!()` - Verifies a boolean condition is true
   - `assert_eq!()` - Verifies two values are equal
   - `assert_ne!()` - Verifies two values are not equal
   - All assertions can include custom error messages

3. **Test Control**:
   - `#[should_panic]` - Tests that verify code properly panics
   - `#[ignore]` - Marks tests to skip during normal test runs

4. **Test Organization**:
   - Unit tests - Test individual components in isolation (in the same files as the code)
   - Integration tests - Test multiple components working together (in the `tests` directory)
   - Module structure and visibility rules for testing

5. **Running Tests**:
   - Filtering tests by name
   - Running tests in parallel or sequentially
   - Controlling test output
   - Running ignored tests

## Exercises

### Exercise 1: Basic Test Structure

**Task 1.1: Creating Your First Tests**
1. Create a new library crate called `calculator` using `cargo new calculator --lib`
2. Implement basic arithmetic functions (`add`, `subtract`, `multiply`, `divide`)
3. Write unit tests for each function to verify correct behavior
4. Run the tests and verify they pass

**Task 1.2: Handling Edge Cases**
1. Extend your `divide` function to handle division by zero appropriately
2. Write a test that verifies your function behaves correctly when dividing by zero
3. Add edge case tests for other functions (e.g., testing with maximum or minimum integer values)

**Task 1.3: Custom Assert Messages**
1. Modify your tests to include custom error messages in assertions
2. Create a scenario where a test fails and observe the custom message
3. Discuss how custom messages improve test diagnostics

### Exercise 2: Using Different Assertion Types

**Task 2.1: Working with `assert!`**
1. Create a function that checks if a number is prime
2. Write tests using `assert!` to verify the function's behavior for various inputs
3. Include both positive and negative test cases

**Task 2.2: Using `assert_eq!` and `assert_ne!`**
1. Implement a string manipulation function (e.g., reverse, capitalize, or trim)
2. Write tests using `assert_eq!` to verify the function returns expected values
3. Write tests using `assert_ne!` to verify the function properly changes the input

**Task 2.3: Testing Custom Types**
1. Create a custom struct with methods (e.g., a `Point` struct with distance calculation)
2. Add the `PartialEq` and `Debug` traits to your struct to enable equality testing
3. Write tests comparing instances of your custom type

### Exercise 3: Testing for Panics

**Task 3.1: Basic `should_panic` Tests**
1. Create a function that requires its input to meet certain conditions, panicking if they're not met
2. Write a test with `#[should_panic]` to verify the function panics with invalid input
3. Verify the test passes only when the function properly panics

**Task 3.2: Advanced Panic Testing**
1. Modify your function to panic with specific error messages depending on the type of invalid input
2. Update your test to use `#[should_panic(expected = "...")]` to check for specific panic messages
3. Write multiple tests to verify different panic conditions

**Task 3.3: Returning `Result` in Tests**
1. Implement a function that returns a `Result` type instead of panicking
2. Write a test that returns `Result<(), String>` to test this function
3. Use the `?` operator in your test to handle errors
4. Compare this approach with `should_panic` testing

### Exercise 4: Test Organization

**Task 4.1: Unit Test Organization**
1. Create a module with multiple related functions
2. Add a `tests` module within the same file
3. Write unit tests for each function in the module
4. Verify public and private functions can be tested

**Task 4.2: Integration Tests**
1. Create a `tests` directory in your project
2. Write an integration test file that tests your library's public API
3. Run only the integration tests using `cargo test --test`
4. Discuss the differences between unit and integration tests for your codebase

**Task 4.3: Test Helpers and Common Code**
1. Create common setup code needed by multiple test files
2. Place this code in `tests/common/mod.rs`
3. Use this common code from your integration tests
4. Explain why this organization pattern is useful

### Exercise 5: Controlling Test Execution

**Task 5.1: Filtering Tests**
1. Create a test suite with at least 10 test functions with descriptive names
2. Run specific tests by name using the filtering feature
3. Run groups of tests by partial name matching
4. Document the commands you used and their outcomes

**Task 5.2: Ignoring Tests**
1. Mark some tests with `#[ignore]`
2. Run the normal test suite and observe that ignored tests are skipped
3. Run only the ignored tests using `cargo test -- --ignored`
4. Run all tests including ignored ones with `cargo test -- --include-ignored`

**Task 5.3: Controlling Parallelism and Output**
1. Create tests that print output during execution
2. Run tests with `--show-output` to see the printed values
3. Run tests with `--test-threads=1` to execute them sequentially
4. Discuss scenarios where these options might be useful

### Exercise 6: Practical Testing Scenarios

**Task 6.1: Testing a Data Structure**
1. Implement a data structure (e.g., a stack, queue, or binary tree)
2. Write comprehensive tests for all operations on your data structure
3. Include edge cases (empty structure, maximum capacity, etc.)
4. Use both unit tests and integration tests appropriately

**Task 6.2: Test-Driven Development**
1. Choose a simple algorithm to implement (e.g., binary search, sort algorithm)
2. Write tests describing the expected behavior before implementing the algorithm
3. Implement the algorithm to pass the tests
4. Reflect on how writing tests first influenced your implementation

**Task 6.3: Refactoring with Tests**
1. Take an existing function with no tests
2. Write tests that document the current behavior
3. Refactor the function to improve it while keeping the tests passing
4. Discuss how tests provided confidence during refactoring

### Exercise 7: Advanced Testing Techniques

**Task 7.1: Property-Based Testing**
1. Research property-based testing in Rust (e.g., using the `proptest` crate)
2. Implement a simple property test for one of your functions
3. Compare property-based testing with example-based testing

**Task 7.2: Performance Testing**
1. Create a function where performance matters
2. Research how to write benchmarks in Rust
3. Implement a simple benchmark test
4. Optimize your function and observe the benchmark improvements

**Task 7.3: Mock Objects and Test Doubles**
1. Create a module that depends on external resources (e.g., a database or file system)
2. Implement a design that allows for testing with mock objects
3. Write tests using these mock objects
4. Discuss the benefits of this approach for testing

## Bonus Exercise: Creating a Test Suite for a Real Project

Choose an existing small open-source Rust project (or create your own) and perform a thorough test audit:

1. Analyze existing tests and identify gaps in coverage
2. Implement additional unit tests for untested functionality
3. Add integration tests for key scenarios
4. Document your testing strategy and findings
5. Reflect on how the testing improved the project's reliability

This comprehensive exercise will help you apply all the testing concepts from this chapter in a real-world context.