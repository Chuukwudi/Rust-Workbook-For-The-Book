# Chapter 17: Asynchronous Programming - Workbook

## Chapter Summary

Chapter 17 introduces asynchronous programming in Rust using the `async`/`await` syntax and the `Future` trait. This approach offers an alternative to thread-based concurrency, allowing you to write non-blocking code that can efficiently handle many concurrent operations without the overhead of threads.

Key topics covered include:
1. **Futures**: Values that represent asynchronous operations that will complete at some point in the future
2. **Async/Await Syntax**: Language features that make working with futures more ergonomic
3. **Asynchronous Tasks**: How to create and manage concurrent operations using tasks
4. **Streams**: Working with sequences of asynchronous values over time
5. **Understanding the underlying traits**: How the `Future`, `Pin`, `Unpin`, and `Stream` traits work together

## Key Concepts

### 1. Futures and Async/Await Syntax

A `Future` represents a value that may not be available yet but will be at some point. The `async` and `await` keywords help you work with futures more easily:

```rust
use std::time::Duration;

// An async function returns a Future
async fn delay_and_return(value: String) -> String {
    // The sleep function returns a Future that we can await
    trpl::sleep(Duration::from_secs(1)).await;
    value
}

fn main() {
    // Use a runtime to run the async code
    trpl::run(async {
        let result = delay_and_return("Hello, async world!".to_string()).await;
        println!("{}", result);
    });
}
```

Key points:
- An `async` function returns a `Future` that must be awaited to get its value
- The `await` keyword can only be used within an `async` function or block
- Futures are lazy; they don't do anything until awaited
- An async runtime is needed to execute futures

### 2. Creating and Managing Tasks

Tasks allow you to run multiple asynchronous operations concurrently:

```rust
use std::time::Duration;

fn main() {
    trpl::run(async {
        // Spawn a task that runs concurrently
        let handle = trpl::spawn_task(async {
            trpl::sleep(Duration::from_secs(2)).await;
            println!("Task completed!");
        });
        
        // Main task continues running
        println!("Main task working...");
        trpl::sleep(Duration::from_secs(1)).await;
        println!("Main task still working...");
        
        // Wait for the spawned task to complete
        handle.await.unwrap();
    });
}
```

Key points:
- `spawn_task` creates a new task that runs concurrently with other tasks
- Tasks are lightweight and can be created in large numbers
- You can await a task handle to wait for its completion

### 3. Message Passing with Channels

Channels provide a way to communicate between different tasks:

```rust
fn main() {
    trpl::run(async {
        let (tx, mut rx) = trpl::channel();
        
        // Spawn a task that sends messages
        trpl::spawn_task(async move {
            for i in 1..=5 {
                tx.send(format!("Message {}", i)).unwrap();
                trpl::sleep(Duration::from_millis(100)).await;
            }
        });
        
        // Receive messages as they arrive
        while let Some(message) = rx.recv().await {
            println!("Received: {}", message);
        }
    });
}
```

Key points:
- Channels come in sender (`tx`) and receiver (`rx`) halves
- The sender can be cloned to allow multiple producers
- The receiver's `recv` method returns a future that resolves to `Some(value)` when a message arrives, or `None` when all senders are dropped

### 4. Working with Multiple Futures

You can combine multiple futures to work with them together:

```rust
fn main() {
    trpl::run(async {
        // Create two futures
        let future1 = async {
            trpl::sleep(Duration::from_secs(1)).await;
            "Future 1 completed"
        };
        
        let future2 = async {
            trpl::sleep(Duration::from_secs(2)).await;
            "Future 2 completed"
        };
        
        // Wait for both futures to complete
        let (result1, result2) = trpl::join(future1, future2).await;
        println!("{}, {}", result1, result2);
        
        // Or race futures against each other
        let winner = trpl::race(
            async { 
                trpl::sleep(Duration::from_millis(100)).await;
                "Fast future won"
            },
            async {
                trpl::sleep(Duration::from_millis(200)).await;
                "Slow future won"
            }
        ).await;
        
        println!("Race result: {}", match winner {
            trpl::Either::Left(msg) => msg,
            trpl::Either::Right(msg) => msg,
        });
    });
}
```

Key points:
- `join` awaits multiple futures and returns all their results
- `race` awaits multiple futures and returns the result of whichever completes first
- For a dynamic number of futures, use `join_all`

### 5. Streams

Streams represent a sequence of asynchronous values, similar to how iterators represent a sequence of synchronous values:

```rust
use trpl::StreamExt;

fn main() {
    trpl::run(async {
        // Create a stream of values
        let (tx, rx) = trpl::channel();
        
        // Spawn a task that sends values to the stream
        trpl::spawn_task(async move {
            for i in 1..=10 {
                tx.send(i).unwrap();
                trpl::sleep(Duration::from_millis(100)).await;
            }
        });
        
        // Convert the receiver to a stream
        let mut stream = trpl::ReceiverStream::new(rx);
        
        // Process the stream using next()
        while let Some(value) = stream.next().await {
            println!("Got value: {}", value);
        }
    });
}
```

Key points:
- The `Stream` trait is like an asynchronous version of `Iterator`
- The `StreamExt` trait provides additional methods like `next`, `map`, `filter`, etc.
- You can use `while let Some(value) = stream.next().await` to process stream items as they arrive

### 6. Pin and Unpin Traits

The `Pin` type prevents a value from being moved in memory, which is important for certain async operations:

```rust
use std::pin::Pin;
use std::future::Future;

fn main() {
    trpl::run(async {
        // Create futures
        let fut1 = async { "future 1" };
        let fut2 = async { "future 2" };
        let fut3 = async { "future 3" };
        
        // Create a collection of pinned futures
        let futures: Vec<Pin<Box<dyn Future<Output = &str>>>> = vec![
            Box::pin(fut1),
            Box::pin(fut2),
            Box::pin(fut3),
        ];
        
        // Wait for all futures to complete
        let results = trpl::join_all(futures).await;
        println!("Results: {:?}", results);
    });
}
```

Key points:
- `Pin` ensures that a value won't be moved in memory
- This is necessary for self-referential structures generated by async blocks
- `Unpin` is a marker trait indicating a type can be safely moved even when pinned
- Most Rust types implement `Unpin` automatically

## Practice Exercises

### Exercise 1: Basic Async/Await
Create a simple async function and await it.

**Tasks:**
1. Create an async function `greet` that takes a name parameter, waits for 1 second, and returns a greeting
2. Call the function with different names and await the results
3. Experiment with removing the `await` and observe the compiler errors

### Exercise 2: Concurrent Tasks
Work with multiple async tasks running concurrently.

**Tasks:**
1. Create a function that spawns 5 tasks, each printing a message after a random delay
2. Use task handles and `join_all` to wait for all tasks to complete
3. Measure the total time it takes to run all tasks concurrently vs. sequentially

### Exercise 3: Async Message Passing
Implement a producer-consumer pattern using async channels.

**Tasks:**
1. Create a producer task that generates numbers and sends them through a channel
2. Create multiple consumer tasks that receive numbers and process them
3. Implement a graceful shutdown mechanism

### Exercise 4: Building an Async Timer
Create a reusable timer utility.

**Tasks:**
1. Write an async function `repeat_timer` that repeatedly calls a callback function at a specified interval
2. Add a parameter to limit the number of times the timer fires
3. Add a way to cancel the timer before it completes all iterations

### Exercise 5: Working with Timeouts
Implement a timeout mechanism for async operations.

**Tasks:**
1. Create a function that attempts to do some work with a timeout
2. Return a `Result` that indicates success or a timeout error
3. Test with operations that complete both within and outside the timeout

### Exercise 6: Error Handling in Async Code
Practice proper error handling in async functions.

**Tasks:**
1. Create an async function that can fail in multiple ways
2. Implement proper error handling using `Result` and the `?` operator
3. Create a retry mechanism that attempts the operation multiple times before giving up

### Exercise 7: Racing Futures
Implement a utility that races multiple async operations.

**Tasks:**
1. Create a function that takes multiple URLs and returns the content of whichever loads first
2. Add a timeout to prevent waiting too long
3. Handle errors gracefully, continuing with other URLs if one fails

### Exercise 8: Custom Stream Implementation
Create your own implementation of a stream.

**Tasks:**
1. Design a type that implements the `Stream` trait
2. Your stream should produce a sequence of values asynchronously
3. Add methods to control the stream's behavior (pause, resume, etc.)

### Exercise 9: Stream Processing Pipeline
Build a pipeline to transform stream data.

**Tasks:**
1. Create a stream of data (e.g., numbers, strings, or simulated measurements)
2. Apply multiple transformations to the stream (filter, map, etc.)
3. Implement a rate limiting mechanism to control how quickly data flows through the pipeline

### Exercise 10: Async Web Scraper
Create an async web scraper that fetches and processes multiple pages concurrently.

**Tasks:**
1. Write a function to fetch a web page and extract links
2. Create a concurrent crawler that follows links up to a specified depth
3. Implement rate limiting to avoid overwhelming target servers
4. Extract and save relevant information from the pages

## Challenge Project: Async Task Scheduler

Create an async task scheduler with the following features:

1. A way to register tasks with priorities
2. The ability to schedule tasks to run at specific times or with specific intervals
3. A limit on how many tasks can run concurrently
4. Graceful handling of task failures
5. The ability to cancel or reschedule tasks
6. A progress tracking mechanism

Use the concepts learned in this chapter:
- Futures for representing pending tasks
- Streams for tracking the flow of tasks through the system
- Channels for communication between components
- Timeouts for preventing tasks from running too long
- Proper error handling throughout

Extension: Add a web API to interact with your scheduler, allowing users to add tasks, monitor progress, and see results through an HTTP interface.

---

## Additional Resources

- [Asynchronous Programming in Rust](https://rust-lang.github.io/async-book/) - The Async Book
- [Tokio Documentation](https://tokio.rs/tokio/tutorial) - Tutorial for the most popular async runtime
- [futures crate Documentation](https://docs.rs/futures/latest/futures/) - Documentation for the futures crate
- [async-std Documentation](https://docs.rs/async-std/latest/async_std/) - Another popular async runtime
- [Pin and Unpin in Rust](https://doc.rust-lang.org/std/pin/index.html) - Detailed documentation on Pin and Unpin
- [Streams in Rust](https://docs.rs/futures/latest/futures/stream/trait.Stream.html) - Documentation for the Stream trait

Remember that async programming takes practice. Start with small examples and gradually build up to more complex scenarios as you become comfortable with the concepts.