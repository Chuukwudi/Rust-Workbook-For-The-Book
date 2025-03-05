# Workbook Exercises for Chapter 2: Programming a Guessing Game

These exercises are designed to reinforce the concepts taught in this chapter. You will practice the basics of user input, random number generation, error handling, and using Rust's standard library.

---

## 1. Create a "Hello, Cargo!" Program

- Create a new Rust project named `practice_cargo` using `cargo new`.
- Navigate to the `practice_cargo` directory.
- Modify the `src/main.rs` file to print `"Hello, Cargo!"` instead of `"Hello, world!"`.
- Compile and run the program using `cargo run`.

---

## 2. Simple User Input

Write a program that:
1. Prompts the user to enter their name.
2. Reads the user's input.
3. Prints a message saying, `"Hello, [name]!"`.

*Hint:* Handle user input correctly by creating a `String` variable and using `io::stdin().read_line()`.

---

## 3. Basic Random Number Generation

Write a program that:
1. Uses the `rand` crate to generate a random number between 1 and 10.
2. Prints the generated number to the console.

*Hint:* Add the `rand` crate as a dependency in the `Cargo.toml` file.

---

## 4. Compare Two Numbers

Write a program that:
1. Prompts the user to input a number between 1 and 10.
2. Generates a random number between 1 and 10.
3. Compares the two numbers using `std::cmp::Ordering` and prints:
   - `"Too small!"` if the user's number is smaller.
   - `"Too big!"` if the user's number is larger.
   - `"Just right!"` if the numbers match.

---

## 5. Handling Errors in User Input

Write a program that:
1. Prompts the user to input a number.
2. Attempts to parse the input as a `u32`.
3. If parsing succeeds, prints the number.
4. If parsing fails, prints `"Invalid input. Please try again."` and asks for input again.

*Hint:* Use a loop and `match` to handle the input.

---

## 6. Build a Simple Guessing Game

Implement the following functionality:
1. Generate a random number between 1 and 20.
2. Use a loop to allow multiple guesses.
3. Prompt the user to guess the number.
4. Compare the guess with the secret number and print:
   - `"Too low!"` if the guess is smaller.
   - `"Too high!"` if the guess is larger.
   - `"You win!"` and exit the loop if the guess is correct.
5. Ignore invalid inputs (e.g., non-numbers) and prompt the user to guess again.

---

## 7. Modify the Guessing Game

Using the code from Exercise 6:
1. Add a feature to count how many guesses it took for the user to guess correctly.
2. Print the total number of guesses when the user wins.

---

## 8. Reuse and Expand

Write a program that:
1. Generates a random number between 1 and 50.
2. Prompts the user to guess the number with a hint:
   - If the guess is within 5 numbers of the correct answer, print `"Getting warm!"`.
   - Otherwise, print `"Cold!"`.
3. Continue until the user guesses the number correctly or after a certain number of attempts.

---


## 9. Simple Dice Roller

**Description:** Simulate rolling a dice. The user specifies how many sides the dice should have, and the program generates a random result.

**Features:**
- Allow the user to input the number of sides for the dice (e.g., 6, 12, 20).
- Generate a random number between 1 and the number of sides.
- Print the result of the roll.
- Optionally, allow multiple rolls in one session.

---

## 10. Rock, Paper, Scissors

**Description:** A simple game where the user plays against the computer.

**Features:**
- Accept input from the user (`rock`, `paper`, or `scissors`).
- Generate a random choice for the computer.
- Compare the user’s input with the computer’s choice.
- Determine the winner based on game rules.
- Optionally, allow multiple rounds and keep score.

---

## 11. Simple Quiz Game

**Description:** Create a quiz with multiple-choice questions, and the user must select the correct answer.
**quiz:**:
```
let questions = vec![
        ("How many legs do dogs have?", vec!["1", "2", "4", "6"], 2),
        ("What is the color of the sky during a clear day?", vec!["Red", "Blue", "Green", "Yellow"], 1),
        ("How many continents are there on Earth?", vec!["5", "6", "7", "8"], 2),
        ("What is 2 + 2?", vec!["3", "4", "5", "6"], 1),
        ("Which planet do we live on?", vec!["Mars", "Earth", "Venus", "Jupiter"], 1),
    ];
```
**Features:**
- Present questions with multiple options.
- Accept the user’s input as their answer.
- Compare the input to the correct answer and give feedback (`right` or `wrong`).
- Keep track of the score across multiple questions.

---

## 12. Basic Password Strength Checker

**Description:** Check the strength of a password entered by the user based on simple rules.

**Features:**
- Accept a password as input.
- Check if the password is at least 8 characters long.
- Check if it contains both letters and numbers.
- Provide feedback such as `"Weak"`, `"Moderate"`, or `"Strong"` based on the criteria.





# Chapter 2: Programming a Guessing Game - Workbook

## Chapter Summary
This chapter introduced several fundamental Rust concepts through building a guessing game:

1. **User Input**: Using `std::io` to read user input from the terminal
2. **Error Handling**: Using `.expect()` for simple error handling and `match` with `Result` for more robust error handling
3. **Comparison**: Using `std::cmp::Ordering` to compare values
4. **Random Number Generation**: Using the `rand` crate to generate random numbers
5. **External Dependencies**: Adding and using external crates through Cargo
6. **Variable Shadowing**: Reusing variable names to convert between types
7. **Match Expressions**: Using pattern matching for control flow
8. **Loops**: Creating repeating program behavior with the `loop` keyword

## Exercises

### Exercise 1: Hello, Cargo!
Create a new Rust project and modify it to print a custom message.

**Tasks:**
1. Create a new project called `hello_cargo` using `cargo new hello_cargo`
2. Navigate to the project directory
3. Modify the `main.rs` file to print "Welcome to Rust Programming!"
4. Compile and run your program using `cargo run`

### Exercise 2: Name Greeter
Write a program that asks for and processes user input.

**Tasks:**
1. Create a program that prompts the user to enter their name
2. Read the input using `std::io`
3. Trim whitespace from the input
4. Print a greeting using the entered name

### Exercise 3: Random Number Generator
Practice using the `rand` crate.

**Tasks:**
1. Add the `rand` crate as a dependency in your `Cargo.toml`
2. Write a program that generates a random number between 1 and 20
3. Print the generated number
4. Run the program multiple times to verify it generates different numbers

### Exercise 4: Number Comparison
Practice using the `std::cmp::Ordering` enum.

**Tasks:**
1. Ask the user to enter two numbers
2. Compare the numbers using `cmp`
3. Print "First number is larger" or "Second number is larger" or "Numbers are equal" based on the comparison

### Exercise 5: Error Handling with Match
Practice robust error handling.

**Tasks:**
1. Write a program that asks the user to enter a number
2. Use a `match` expression with `parse()` to handle both successful number parsing and errors
3. If parsing succeeds, print the number doubled
4. If parsing fails, print an error message and ask for input again

### Exercise 6: Mini Guessing Game
Create a simplified version of the guessing game.

**Tasks:**
1. Generate a random number between 1 and 10
2. Give the user 3 attempts to guess the number
3. After each guess, tell them if their guess was too high, too low, or correct
4. If they guess correctly, congratulate them and end the game
5. If they use all 3 attempts without guessing correctly, reveal the answer

### Exercise 7: Temperature Converter
Create a practical tool using concepts from this chapter.

**Tasks:**
1. Ask the user if they want to convert from Celsius to Fahrenheit (C->F) or from Fahrenheit to Celsius (F->C)
2. Ask for the temperature value
3. Convert the temperature using the appropriate formula:
   - C to F: (C × 9/5) + 32
   - F to C: (F - 32) × 5/9
4. Handle invalid inputs gracefully with appropriate error messages

### Exercise 8: Dice Roller
Create a program that simulates rolling dice.

**Tasks:**
1. Ask the user how many sides the dice should have
2. Ask how many dice they want to roll
3. Generate and display random numbers for each die
4. Calculate and display the total sum
5. Ask if they want to roll again

### Exercise 9: Number Guessing Game with Hints
Enhance the guessing game with additional features.

**Tasks:**
1. Generate a random number between 1 and 100
2. Track the number of guesses the user makes
3. Give "warmer" or "colder" hints based on how close the new guess is to the correct number compared to the previous guess
4. When the user guesses correctly, tell them how many attempts it took
5. Handle non-numeric inputs properly

### Exercise 10: Password Validator
Use string handling and conditional logic to validate a password.

**Tasks:**
1. Ask the user to create a password
2. Check if the password meets these requirements:
   - At least 8 characters long
   - Contains at least one uppercase letter
   - Contains at least one number
3. For each failed requirement, print a specific message
4. If all requirements are met, tell the user their password is accepted