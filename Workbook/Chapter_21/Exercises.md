# Chapter 21: Building a Multithreaded Web Server - Workbook

## Chapter Overview

Chapter 21 brings together many of the concepts you've learned throughout the book by building a multithreaded web server. This project demonstrates how to apply Rust's features to a real-world scenario, focusing on concurrent programming using threads. You'll build a server that handles HTTP requests, processes them in parallel, and returns appropriate responses.

The chapter walks through:
1. Building a single-threaded server
2. Upgrading it to a multithreaded server using a thread pool
3. Implementing proper shutdown and cleanup

This workbook will help you practice and extend these concepts through hands-on exercises.

## Key Concepts

### 1. HTTP and TCP Basics

- **TCP (Transmission Control Protocol)**: A lower-level protocol that handles the connection between clients and servers
- **HTTP (Hypertext Transfer Protocol)**: A higher-level protocol that defines the format of requests and responses

HTTP requests have the following format:
```
Method Request-URI HTTP-Version CRLF
headers CRLF
message-body
```

HTTP responses have this format:
```
HTTP-Version Status-Code Reason-Phrase CRLF
headers CRLF
message-body
```

### 2. Single-Threaded Server Implementation

Key components:
- Using `TcpListener` to listen for incoming connections
- Using `TcpStream` to read from and write to the connected client
- Parsing HTTP requests
- Generating HTTP responses
- Handling different request paths

```rust
use std::{
    fs,
    io::{prelude::*, BufReader},
    net::{TcpListener, TcpStream},
};

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

    for stream in listener.incoming() {
        let stream = stream.unwrap();
        handle_connection(stream);
    }
}

fn handle_connection(mut stream: TcpStream) {
    let buf_reader = BufReader::new(&stream);
    let request_line = buf_reader.lines().next().unwrap().unwrap();

    let (status_line, filename) = match &request_line[..] {
        "GET / HTTP/1.1" => ("HTTP/1.1 200 OK", "hello.html"),
        _ => ("HTTP/1.1 404 NOT FOUND", "404.html"),
    };

    let contents = fs::read_to_string(filename).unwrap();
    let length = contents.len();
    let response = format!(
        "{status_line}\r\nContent-Length: {length}\r\n\r\n{contents}"
    );

    stream.write_all(response.as_bytes()).unwrap();
}
```

### 3. Multithreaded Server with a Thread Pool

Limitations of a single-threaded server:
- Can only process one request at a time
- Slow requests block all other requests

Thread pool implementation:
- Create a fixed number of threads that wait for work
- Distribute incoming connections among these threads
- Use channels to communicate between the main thread and worker threads
- Use thread synchronization primitives (`Arc` and `Mutex`) to share data safely

```rust
// ThreadPool public API
pub struct ThreadPool {
    workers: Vec<Worker>,
    sender: Option<mpsc::Sender<Job>>,
}

impl ThreadPool {
    pub fn new(size: usize) -> ThreadPool {
        // Implementation details
    }

    pub fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
        // Implementation details
    }
}
```

### 4. Graceful Shutdown and Cleanup

- Implementing the `Drop` trait to clean up resources
- Signaling worker threads to finish their work
- Joining threads to wait for them to complete
- Ensuring proper cleanup even when errors occur

## Practice Exercises

### Exercise 1: Building a Basic HTTP Server

Create a simple HTTP server that responds to requests with different content based on the URL path.

```rust
use std::{
    fs,
    io::{prelude::*, BufReader},
    net::{TcpListener, TcpStream},
};

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

    for stream in listener.incoming() {
        let stream = stream.unwrap();
        handle_connection(stream);
    }
}

fn handle_connection(mut stream: TcpStream) {
    // IMPLEMENT THIS FUNCTION:
    // 1. Read and parse the HTTP request
    // 2. Extract the request path
    // 3. Create appropriate responses for:
    //    - "/" -> Return a greeting HTML page
    //    - "/about" -> Return an about HTML page
    //    - Any other path -> Return a 404 page
}
```

Create two HTML files for this exercise:
1. `hello.html` - A simple greeting page
2. `about.html` - A page with information about yourself or a topic of interest
3. `404.html` - A page indicating the requested resource wasn't found

Test your server with a web browser, visiting:
- http://127.0.0.1:7878/
- http://127.0.0.1:7878/about
- http://127.0.0.1:7878/nonexistent

### Exercise 2: Adding Request Logging

Extend the server to log incoming requests to a file.

```rust
use std::{
    fs::{self, File, OpenOptions},
    io::{prelude::*, BufReader},
    net::{TcpListener, TcpStream},
    time::SystemTime,
};

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
    
    // IMPLEMENT:
    // 1. Open or create a log file named "server_log.txt"
    // 2. Log each request to the file with timestamp, path, and response code
    
    for stream in listener.incoming() {
        let stream = stream.unwrap();
        // MODIFY THIS TO PASS THE LOG FILE TO handle_connection
        handle_connection(stream);
    }
}

fn handle_connection(mut stream: TcpStream /* Add parameter for log file */) {
    // IMPLEMENT:
    // 1. Parse the HTTP request and extract path and method
    // 2. Log request information to file
    // 3. Return appropriate response
}

fn log_request(/* Add parameters for log file, request info */) {
    // IMPLEMENT:
    // Format and write request information to the log file
}
```

The log format should look something like:
```
[2023-04-12 15:34:23] GET / 200 OK
[2023-04-12 15:35:01] GET /about 200 OK
[2023-04-12 15:35:45] GET /nonexistent 404 NOT FOUND
```

### Exercise 3: Implementing a Thread Pool

Implement your own thread pool from scratch. This is similar to the implementation in the chapter but try to do it without referring to the solution.

```rust
use std::{
    sync::{mpsc, Arc, Mutex},
    thread,
};

type Job = Box<dyn FnOnce() + Send + 'static>;

pub struct ThreadPool {
    // IMPLEMENT:
    // Store worker threads and a channel sender
}

struct Worker {
    // IMPLEMENT:
    // Store worker ID and thread handle
}

impl ThreadPool {
    /// Create a new ThreadPool.
    ///
    /// The size is the number of threads in the pool.
    ///
    /// # Panics
    ///
    /// The `new` function will panic if the size is zero.
    pub fn new(size: usize) -> ThreadPool {
        // IMPLEMENT:
        // 1. Create a channel for communication
        // 2. Create workers and store them
        // 3. Return the ThreadPool
    }

    pub fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
        // IMPLEMENT:
        // Send the job to a worker thread
    }
}

impl Drop for ThreadPool {
    fn drop(&mut self) {
        // IMPLEMENT:
        // 1. Signal workers to shut down
        // 2. Join each worker thread
    }
}

impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<mpsc::Receiver<Job>>>) -> Worker {
        // IMPLEMENT:
        // 1. Create a thread that loops waiting for jobs
        // 2. Return a Worker with the ID and thread handle
    }
}
```

### Exercise 4: HTTP Load Tester

Create a simple load tester that can send multiple concurrent requests to your server and measure response times.

```rust
use std::{
    io::{Read, Write},
    net::TcpStream,
    sync::{Arc, Mutex},
    thread,
    time::{Duration, Instant},
};

struct LoadTestResults {
    total_requests: usize,
    successful_requests: usize,
    failed_requests: usize,
    min_response_time: Duration,
    max_response_time: Duration,
    avg_response_time: Duration,
}

fn main() {
    // Parameters
    let url = "127.0.0.1:7878";
    let path = "/";
    let num_requests = 100;
    let concurrency = 10;
    
    println!("Starting load test with {} concurrent users, {} total requests", 
             concurrency, num_requests);
    
    // IMPLEMENT:
    // 1. Create threads to send requests (concurrency parameter determines how many)
    // 2. Each thread should send its portion of requests
    // 3. Collect timing information
    // 4. Calculate and display statistics
}

fn send_request(url: &str, path: &str) -> Result<Duration, std::io::Error> {
    // IMPLEMENT:
    // 1. Measure time to connect, send request, and receive response
    // 2. Return the time taken or error
}

fn print_results(results: &LoadTestResults) {
    // IMPLEMENT:
    // Format and print test results
}
```

### Exercise 5: Extended Features for the Server

Implement several additional features for your web server:

1. **Request Parameters**: Parse query parameters from URLs (e.g., `/?name=value`)
2. **Dynamic Content**: Generate dynamic responses based on query parameters
3. **Request Headers**: Parse and use HTTP headers from requests
4. **Response Headers**: Add custom headers to responses
5. **Content Types**: Support different content types (HTML, plain text, JSON)

```rust
use std::{
    collections::HashMap,
    fs,
    io::{prelude::*, BufReader},
    net::{TcpListener, TcpStream},
};

fn main() {
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();
    let pool = ThreadPool::new(4);

    for stream in listener.incoming() {
        let stream = stream.unwrap();
        pool.execute(|| {
            handle_connection(stream);
        });
    }
}

struct Request {
    method: String,
    path: String,
    query_params: HashMap<String, String>,
    headers: HashMap<String, String>,
}

struct Response {
    status_line: String,
    headers: HashMap<String, String>,
    body: String,
}

fn handle_connection(mut stream: TcpStream) {
    // IMPLEMENT:
    // 1. Parse the request into the Request struct
    // 2. Handle different paths with appropriate responses
    // 3. Create a Response and send it
}

fn parse_request(buf_reader: BufReader<&TcpStream>) -> Request {
    // IMPLEMENT:
    // Parse a raw HTTP request into a Request struct
}

fn parse_query_string(query: &str) -> HashMap<String, String> {
    // IMPLEMENT:
    // Parse query parameters from a URL
}

fn send_response(stream: &mut TcpStream, response: Response) {
    // IMPLEMENT:
    // Format and send the HTTP response
}
```

## Challenge Project: REST API Server

Create a more feature-complete HTTP server that implements a simple REST API for a task management system. The API should support the following operations:

- `GET /tasks` - List all tasks
- `GET /tasks/{id}` - Get a specific task
- `POST /tasks` - Create a new task
- `PUT /tasks/{id}` - Update a task
- `DELETE /tasks/{id}` - Delete a task

Tasks should be stored in memory (you can use a thread-safe data structure like `Arc<Mutex<HashMap<...>>>` to store them).

Each task should have:
- An ID (automatically assigned)
- A title
- A description
- A status (e.g., "pending", "in progress", "completed")

The server should:
1. Parse JSON requests
2. Return JSON responses
3. Use appropriate HTTP status codes
4. Implement proper error handling
5. Use a thread pool for handling requests
6. Implement graceful shutdown

You'll need to decide on the specific data structures and implement the HTTP parsing and response generation.

This challenge integrates many aspects of what you've learned about Rust, including:
- Threads and synchronization
- Error handling
- Parsing and generating structured data
- Network programming
- Proper resource management

## Additional Resources

- [Actix Web](https://actix.rs/) - A powerful, pragmatic, and extremely fast web framework for Rust
- [Rocket](https://rocket.rs/) - A web framework for Rust that makes it simple to write fast, secure web applications
- [Tokio](https://tokio.rs/) - An asynchronous runtime for Rust, perfect for building networking applications
- [Hyper](https://hyper.rs/) - A fast and correct HTTP implementation for Rust
- [The Rust Cookbook: Network Programming](https://rust-lang-nursery.github.io/rust-cookbook/net.html) - Recipes for common networking tasks in Rust
- [Mozilla's MDN Web Docs: HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) - Comprehensive documentation on HTTP