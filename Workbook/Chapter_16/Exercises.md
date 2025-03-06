# Chapter 16: Fearless Concurrency - Workbook

## Chapter Summary

This chapter explores Rust's approach to concurrent programming, which leverages the ownership and type systems to prevent common concurrency bugs at compile time. The key topics covered are:

1. **Creating Threads**: Using Rust's standard library to spawn and manage threads
2. **Message Passing**: Transferring data between threads using channels
3. **Shared State**: Safely sharing data between threads using mutexes
4. **Thread Safety Traits**: Understanding the `Send` and `Sync` traits that enable safe concurrency

Rust's "fearless concurrency" approach allows you to write concurrent code with confidence, as many potential errors are caught during compilation rather than at runtime.

## Key Concepts

### 1. Creating Threads with std::thread

Rust uses a 1:1 threading model, where each Rust thread corresponds to one operating system thread:

```rust
use std::thread;
use std::time::Duration;

fn main() {
    // Create a new thread
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("hi number {} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(1));
        }
    });

    // Code in the main thread runs concurrently
    for i in 1..5 {
        println!("hi number {} from the main thread!", i);
        thread::sleep(Duration::from_millis(1));
    }

    // Wait for the spawned thread to finish
    handle.join().unwrap();
}
```

Key points:
- The `thread::spawn` function takes a closure containing the code to run in the new thread
- It returns a `JoinHandle` that can be used to wait for the thread to finish
- When the main thread ends, all spawned threads are shut down

### 2. Using move Closures with Threads

To use data from the main thread in a spawned thread, we need to move ownership:

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];
    
    let handle = thread::spawn(move || {
        println!("Here's a vector: {:?}", v);
    });
    
    handle.join().unwrap();
    // v is no longer accessible here because ownership was moved to the thread
}
```

The `move` keyword forces the closure to take ownership of the values it uses, ensuring they're valid for the thread's entire lifetime.

### 3. Message Passing with Channels

Channels provide a way to send data between threads:

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    // Create a channel
    let (tx, rx) = mpsc::channel();
    
    // Spawn a thread that sends a message
    thread::spawn(move || {
        let message = String::from("hello");
        tx.send(message).unwrap();
        // message is moved and can no longer be used here
    });
    
    // Receive the message in the main thread
    let received = rx.recv().unwrap();
    println!("Got: {}", received);
}
```

Key points:
- `mpsc` stands for "multiple producer, single consumer"
- `send` transfers ownership of the data to the receiver
- `recv` blocks until a message is received
- `try_recv` doesn't block, returning an error if no message is available

For multiple messages:

```rust
// Sender
for val in vec!["hi", "from", "the", "thread"] {
    tx.send(val.to_string()).unwrap();
    thread::sleep(Duration::from_secs(1));
}

// Receiver
for received in rx {
    println!("Got: {}", received);
}
```

Multiple senders can be created by cloning the transmitter:

```rust
let tx1 = tx.clone();
```

### 4. Shared State with Mutex

Mutexes allow safe access to shared data:

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    // Create a mutex containing data
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];
    
    for _ in 0..10 {
        // Clone the Arc, not the Mutex
        let counter = Arc::clone(&counter);
        
        let handle = thread::spawn(move || {
            // Lock the mutex to get exclusive access
            let mut num = counter.lock().unwrap();
            *num += 1;
            // Lock is automatically released when num goes out of scope
        });
        
        handles.push(handle);
    }
    
    // Wait for all threads to finish
    for handle in handles {
        handle.join().unwrap();
    }
    
    println!("Result: {}", *counter.lock().unwrap());
}
```

Key points:
- `Mutex<T>` provides interior mutability, allowing mutation through an immutable reference
- `lock()` blocks until the lock is acquired and returns a `MutexGuard`
- `MutexGuard` implements `Deref` and `Drop` to access the data and release the lock
- `Arc<T>` (Atomic Reference Counting) is used for shared ownership between threads

### 5. Send and Sync Traits

These marker traits define the thread-safety properties of types:

- `Send`: Types that can be transferred between threads
- `Sync`: Types that can be shared between threads (via references)

Most Rust types are `Send` and `Sync`, but there are exceptions:
- `Rc<T>` is neither `Send` nor `Sync` (use `Arc<T>` instead)
- `RefCell<T>` is not `Sync` (use `Mutex<T>` instead)
- Raw pointers are neither `Send` nor `Sync`

Types composed entirely of `Send` types are automatically `Send`. Similarly, types composed entirely of `Sync` types are automatically `Sync`.

## Practice Exercises

### Exercise 1: Basic Thread Creation
Practice creating and managing threads.

**Tasks:**
1. Create a program that spawns 5 threads
2. Each thread should print its ID (using `thread::current().id()`) and a unique message
3. Ensure all threads complete before the program exits
4. Experiment with the behavior when you do not join the threads

### Exercise 2: Thread Performance
Compare sequential vs. parallel execution to understand performance characteristics.

**Tasks:**
1. Create a CPU-intensive function (e.g., calculating the sum of 1 to n)
2. Run the function sequentially 10 times and measure the total time
3. Run the function in 10 parallel threads and measure the total time
4. Compare the results and explain the differences

### Exercise 3: Data Sharing between Threads
Practice using the `move` keyword and transferring ownership.

**Tasks:**
1. Create a program with a vector of numbers
2. Spawn a thread that calculates the sum of the numbers
3. Spawn another thread that calculates the product of the numbers
4. Print both results from the main thread
5. Ensure the code is memory safe by using `move` closures

### Exercise 4: Simple Message Passing
Create a program using channels for communication.

**Tasks:**
1. Create a "worker" thread that waits for messages
2. Send the worker 10 different tasks to process (e.g., "calculate factorial of 5")
3. Have the worker send back results for each task
4. Print all the results in the main thread

### Exercise 5: Multiple Producers
Explore multiple sender channels.

**Tasks:**
1. Create a "collector" thread that receives and prints messages
2. Spawn 5 "producer" threads, each with its own sender
3. Have each producer send 3 unique messages to the collector
4. Verify that all 15 messages are received by the collector

### Exercise 6: Pipeline Processing
Create a multi-stage processing pipeline using channels.

**Tasks:**
1. Create a producer thread that generates numbers from 1 to 20
2. Create a filter thread that receives numbers and forwards only even numbers
3. Create a consumer thread that receives the filtered numbers and computes their squares
4. Print the final results in the main thread

### Exercise 7: Thread-Safe Counter
Implement a counter that can be incremented from multiple threads.

**Tasks:**
1. Create a shared counter protected by a mutex
2. Spawn 100 threads that each increment the counter 100 times
3. Verify that the final count is 10,000
4. Implement a version using atomic types instead of a mutex and compare

### Exercise 8: Concurrent Map
Implement a thread-safe map that can be used concurrently.

**Tasks:**
1. Create a structure that wraps a `HashMap` in a `Mutex`
2. Implement methods for inserting, retrieving, and removing elements
3. Test the map by having multiple threads modify it simultaneously
4. Ensure the final state is consistent

### Exercise 9: Reader-Writer Problem
Implement a solution to the classic reader-writer problem.

**Tasks:**
1. Create a shared resource that can be read by multiple threads but written by only one
2. Spawn several reader threads and several writer threads
3. Readers should be able to access the resource simultaneously
4. Writers should have exclusive access
5. Verify that the system doesn't deadlock

### Exercise 10: Custom Thread Pool
Implement a thread pool for task execution.

**Tasks:**
1. Create a thread pool with a fixed number of worker threads
2. Implement a method to execute tasks (closures) on the pool
3. Ensure tasks are distributed among the threads
4. Include a method to gracefully shut down the pool
5. Test the pool with various tasks and verify they all complete

## Challenge Project: Concurrent Web Crawler

Create a concurrent web crawler that fetches and processes web pages:

1. The crawler should accept a starting URL and a maximum depth
2. It should follow links up to the maximum depth
3. Use a thread pool to fetch pages concurrently
4. Use channels to communicate between:
   - A coordinator thread that manages the crawl
   - Worker threads that fetch and parse pages
   - A result collector that stores/processes the results
5. Implement proper error handling for failed requests
6. Include mechanisms to avoid duplicate work (don't fetch the same URL twice)
7. Provide status updates as the crawl progresses
8. Allow graceful shutdown through a signal (e.g., Ctrl+C)

This project will exercise your understanding of threads, channels, mutexes, and thread safety concepts.

---

## Additional Resources

- [Rust By Example: Concurrency](https://doc.rust-lang.org/rust-by-example/std_misc/threads.html)
- [The Rustonomicon: Concurrency](https://doc.rust-lang.org/nomicon/concurrency.html)
- [Thread documentation](https://doc.rust-lang.org/std/thread/index.html)
- [Mutex documentation](https://doc.rust-lang.org/std/sync/struct.Mutex.html)
- [mpsc channel documentation](https://doc.rust-lang.org/std/sync/mpsc/index.html)
- [Crossbeam crate](https://crates.io/crates/crossbeam) - Advanced concurrency tools
- [Rayon crate](https://crates.io/crates/rayon) - Data parallelism library

Remember that Rust's approach to concurrency helps catch many issues at compile time, but it's still important to design your concurrent programs carefully to avoid logical issues like deadlocks and livelocks.