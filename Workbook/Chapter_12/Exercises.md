# Chapter 12: An I/O Project - Workbook

## Chapter Summary

This chapter walks through creating a command-line application called `minigrep`, which is a simplified version of the classic `grep` tool. By building this project, we learn how to:

1. **Structure a Larger Rust Program**: Separating code into `main.rs` and `lib.rs` for better testing and organization
2. **Process Command-Line Arguments**: Using `std::env::args()` to accept user input
3. **Read Files**: Using `std::fs` to read file contents
4. **Error Handling**: Implementing robust error handling with `Result` and error propagation
5. **Test-Driven Development**: Writing tests first to guide functionality implementation
6. **Environment Variables**: Using environment variables for configuration
7. **Output Control**: Directing output to standard output vs standard error

The `minigrep` program demonstrates how Rust's features combine to create robust, maintainable command-line tools.

## Key Concepts

### 1. Accepting Command-Line Arguments

```rust
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();
    
    // Command line args include the program name at index 0
    // So user arguments start at index 1
    let query = &args[1];
    let file_path = &args[2];
    
    println!("Searching for {}", query);
    println!("In file {}", file_path);
}
```

### 2. Reading Files

```rust
use std::fs;

fn main() {
    let file_path = "some_file.txt";
    let contents = fs::read_to_string(file_path)
        .expect("Should have been able to read the file");
    
    println!("File contents:\n{contents}");
}
```

### 3. Separating Configuration

```rust
struct Config {
    query: String,
    file_path: String,
}

impl Config {
    fn build(args: &[String]) -> Result<Config, &'static str> {
        if args.len() < 3 {
            return Err("not enough arguments");
        }
        
        let query = args[1].clone();
        let file_path = args[2].clone();
        
        Ok(Config { query, file_path })
    }
}
```

### 4. Separating Logic into a Library

```rust
// In lib.rs
pub fn run(config: Config) -> Result<(), Box<dyn Error>> {
    let contents = fs::read_to_string(config.file_path)?;
    
    for line in search(&config.query, &contents) {
        println!("{line}");
    }
    
    Ok(())
}

// In main.rs
use std::process;
use minigrep::Config;

fn main() {
    let args: Vec<String> = env::args().collect();
    
    let config = Config::build(&args).unwrap_or_else(|err| {
        eprintln!("Problem parsing arguments: {err}");
        process::exit(1);
    });
    
    if let Err(e) = minigrep::run(config) {
        eprintln!("Application error: {e}");
        process::exit(1);
    }
}
```

### 5. Test-Driven Development

```rust
#[cfg(test)]
mod tests {
    use super::*;
    
    #[test]
    fn one_result() {
        let query = "duct";
        let contents = "\
Rust:
safe, fast, productive.
Pick three.";
        
        assert_eq!(vec!["safe, fast, productive."], search(query, contents));
    }
}
```

### 6. Working with Environment Variables

```rust
use std::env;

// Check if environment variable is set
let case_sensitive = env::var("CASE_SENSITIVE").is_ok();
```

### 7. Using Standard Error for Errors

```rust
// Print to stdout (for normal output)
println!("Normal output");

// Print to stderr (for error messages)
eprintln!("Error message");
```

## Practice Exercises

### Exercise 1: Command-Line Arguments Parser
Create a program that accepts and displays command-line arguments.

**Tasks:**
1. Create a new project using `cargo new args_parser`
2. Write code that collects command-line arguments
3. Print each argument with its position (index)
4. Handle the case where no arguments are provided with a helpful message

### Exercise 2: File Reader
Create a program that reads and displays the contents of a file.

**Tasks:**
1. Create a new project using `cargo new file_reader`
2. Accept a file path as a command-line argument
3. Read the file's contents
4. Display the file contents along with:
   - The number of lines
   - The number of words
   - The number of characters
5. Implement proper error handling if the file doesn't exist or can't be read

### Exercise 3: Config Builder
Implement a program with a configuration struct.

**Tasks:**
1. Create a new project using `cargo new config_app`
2. Define a `Config` struct with at least three fields
3. Implement a `build` method for the `Config` struct that validates arguments
4. Use the `Result` type to handle invalid configurations
5. In `main.rs`, handle the `Result` returned by the `build` method

### Exercise 4: Mini Grepper
Implement a simplified version of the `minigrep` program.

**Tasks:**
1. Create a program that accepts a search string and file path
2. Implement the `search` function that returns lines containing the search string
3. Add a case-insensitive search option that can be enabled with an environment variable
4. Use proper error handling throughout
5. Separate the code into a library and a binary crate

### Exercise 5: Test-Driven Word Counter
Use TDD to create a word counting utility.

**Tasks:**
1. Write a test for a `count_words` function before implementing it
2. Implement the `count_words` function to make the test pass
3. Write a test for a `count_lines` function before implementing it
4. Implement the `count_lines` function to make the test pass
5. Write a test for a `count_chars` function before implementing it
6. Implement the `count_chars` function to make the test pass

### Exercise 6: Configuration from Multiple Sources
Create a program that accepts configuration from both command-line arguments and environment variables.

**Tasks:**
1. Define a configuration that includes:
   - Verbosity level (can be set via `--verbose` or `VERBOSE` environment variable)
   - Output file (can be set via `--output` or `OUTPUT_FILE` environment variable)
2. Implement logic to prioritize command-line arguments over environment variables
3. Print the final configuration that will be used

### Exercise 7: Directory Lister
Create a utility that lists files in a directory.

**Tasks:**
1. Accept a directory path as a command-line argument
2. List all files in the directory
3. Add a `--recursive` flag to list files in subdirectories
4. Add a `--filter` option to only show files matching a pattern
5. Send error messages to stderr and regular output to stdout

### Exercise 8: Log Parser
Create a log file parser.

**Tasks:**
1. Accept a log file path as a command-line argument
2. Parse the log file looking for lines containing "ERROR:"
3. Count and display the number of error lines
4. Add an option to display the error lines
5. Add an option to search for specific error types

### Exercise 9: Simple Todo App
Create a command-line TODO application.

**Tasks:**
1. Create a program that maintains a list of todo items in a file
2. Implement commands for:
   - Adding a new todo item
   - Listing all todo items
   - Marking an item as complete
   - Removing an item
3. Use proper error handling and file I/O
4. Write tests for the core functionality

### Exercise 10: Enhanced minigrep
Extend the minigrep program with additional features.

**Tasks:**
1. Add line numbers to the output
2. Add a `--count` flag that only shows the number of matching lines
3. Add a `--before-context` and `--after-context` option that shows lines before and after matches
4. Add a `--recursive` flag to search in subdirectories
5. Add colorized output for matched text

## Challenge Project: Multi-File Search Tool

Create a more advanced command-line search tool with the following features:

1. Search for a pattern across multiple files
2. Accept file glob patterns (like `*.rs` or `src/*.txt`)
3. Support for regular expressions
4. Display results with file names, line numbers, and context
5. Option to replace matched text in files
6. Colorized output
7. Support for ignoring certain directories (like `.git`)
8. Configuration via both command-line arguments and a config file

Document your code well, implement robust error handling, and write tests for the core functionality.

---

Remember the key lessons from this chapter:
- Separate concerns in your code for better testing and maintainability
- Use Rust's type system to handle configuration and errors
- Follow the test-driven development process for new features
- Use library crates for code that you want to test and binary crates for application entry points
- Consider user experience when designing command-line interfaces