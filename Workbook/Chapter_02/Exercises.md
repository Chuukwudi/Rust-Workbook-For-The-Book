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
3. Continue until the user guesses the number correctly.

---

## 9. Guess the Number Game

**Description:** The program generates a random number, and the user must guess it. The program provides feedback ("Too high" or "Too low") after each guess.

**Features:**
- Generate a random number between 1 and 100.
- Allow the user to input guesses.
- Compare the user’s guess to the random number.
- End the game when the user guesses correctly or after a certain number of attempts.

---

## 10. Simple Dice Roller

**Description:** Simulate rolling a dice. The user specifies how many sides the dice should have, and the program generates a random result.

**Features:**
- Allow the user to input the number of sides for the dice (e.g., 6, 12, 20).
- Generate a random number between 1 and the number of sides.
- Print the result of the roll.
- Optionally, allow multiple rolls in one session.

---

## 11. Rock, Paper, Scissors

**Description:** A simple game where the user plays against the computer.

**Features:**
- Accept input from the user (`rock`, `paper`, or `scissors`).
- Generate a random choice for the computer.
- Compare the user’s input with the computer’s choice.
- Determine the winner based on game rules.
- Optionally, allow multiple rounds and keep score.

---

## 12. Simple Quiz Game

**Description:** Create a quiz with multiple-choice questions, and the user must select the correct answer.

**Features:**
- Present questions with multiple options.
- Accept the user’s input as their answer.
- Compare the input to the correct answer and give feedback (`right` or `wrong`).
- Keep track of the score across multiple questions.

---

## 13. Basic Password Strength Checker

**Description:** Check the strength of a password entered by the user based on simple rules.

**Features:**
- Accept a password as input.
- Check if the password is at least 8 characters long.
- Check if it contains both letters and numbers.
- Provide feedback such as `"Weak"`, `"Moderate"`, or `"Strong"` based on the criteria.
