# Chapter 20: Advanced Features - Workbook

## Chapter Overview

Chapter 20 covers several advanced Rust features that you may not use every day but are important to understand for certain specialized scenarios. These features provide powerful capabilities when you need them, such as working with low-level code, creating custom abstractions, and extending the language itself. This workbook will help you explore and practice these features, focusing on their practical applications.

## Key Concepts

### 1. Unsafe Rust

Unsafe Rust gives you access to five special capabilities that the compiler can't guarantee are memory-safe:

- **Dereferencing raw pointers**
- **Calling unsafe functions or methods**
- **Accessing or modifying mutable static variables**
- **Implementing unsafe traits**
- **Accessing fields of a union**

When to use unsafe:
- Interfacing with C code
- Low-level systems programming
- When creating safe abstractions that the borrow checker doesn't understand
- Performance-critical code where you can verify safety manually

Example of a safe abstraction over unsafe code:
```rust
use std::slice;

// A safe function that uses unsafe internally
fn split_at_mut(values: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    let len = values.len();
    let ptr = values.as_mut_ptr();
    
    assert!(mid <= len);
    
    unsafe {
        (
            slice::from_raw_parts_mut(ptr, mid),
            slice::from_raw_parts_mut(ptr.add(mid), len - mid),
        )
    }
}
```

### 2. Advanced Traits

#### Associated Types
```rust
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}

impl Iterator for Counter {
    type Item = u32;
    
    fn next(&mut self) -> Option<Self::Item> {
        // Implementation here
    }
}
```

#### Default Generic Type Parameters
```rust
// The Add trait with a default type parameter Rhs=Self
trait Add<Rhs=Self> {
    type Output;
    fn add(self, rhs: Rhs) -> Self::Output;
}

// Using the default (adding two Points)
impl Add for Point {
    type Output = Point;
    fn add(self, other: Point) -> Point {
        // Implementation here
    }
}

// Specifying a different type (adding Millimeters and Meters)
impl Add<Meters> for Millimeters {
    type Output = Millimeters;
    fn add(self, other: Meters) -> Millimeters {
        // Implementation here
    }
}
```

#### Fully Qualified Syntax
Used when multiple traits have methods with the same name:

```rust
// Using fully qualified syntax to specify which trait's method to call
Trait::method(&instance);

// For associated functions that don't take self:
<Type as Trait>::function();
```

#### Supertraits
Making a trait require functionality from another trait:

```rust
trait OutlinePrint: fmt::Display {
    fn outline_print(&self) {
        // Implementation uses Display functionality
    }
}
```

#### Newtype Pattern
Using a tuple struct to implement traits on types that you don't own:

```rust
struct Wrapper(Vec<String>);

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}
```

### 3. Advanced Types

#### Type Aliases
Creating a synonym for an existing type:

```rust
type Kilometers = i32;

// Especially useful for complex types
type Thunk = Box<dyn Fn() + Send + 'static>;

// Common in std library for Result types
type Result<T> = std::result::Result<T, std::io::Error>;
```

#### The Never Type (`!`)
Represents a value that never occurs:

```rust
fn bar() -> ! {
    // This function never returns
    panic!();
}

// Used in functions like exit, continue, etc.
```

#### Dynamically Sized Types (DSTs)
Types whose size can only be known at runtime:

```rust
// str is a DST, so we must use &str or Box<str>
let s: &str = "Hello there!";

// dyn Trait is a DST, so we use &dyn Trait or Box<dyn Trait>
let trait_object: Box<dyn Display> = Box::new(String::from("hello"));

// The ?Sized trait bound allows for types that might not have a known size
fn generic<T: ?Sized>(t: &T) {
    // implementation
}
```

### 4. Advanced Functions and Closures

#### Function Pointers
```rust
fn add_one(x: i32) -> i32 {
    x + 1
}

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}

let answer = do_twice(add_one, 5);
```

#### Returning Closures
```rust
// Using impl Trait
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}

// Using a trait object when you need multiple distinct closures
fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}
```

### 5. Macros

#### Declarative Macros with `macro_rules!`
```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

#### Procedural Macros
Custom derive macros:
```rust
use proc_macro::TokenStream;
use quote::quote;

#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    // Implementation here
}
```

Attribute-like macros:
```rust
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
    // Implementation here
}
```

Function-like macros:
```rust
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
    // Implementation here
}
```

## Practice Exercises

### Exercise 1: Implementing a Safe Abstraction over Unsafe Code

Create a safe `RingBuffer` implementation using unsafe code for the underlying operations.

```rust
struct RingBuffer<T> {
    buffer: Vec<T>,
    capacity: usize,
    read_index: usize,
    write_index: usize,
    count: usize,
}

impl<T> RingBuffer<T>
where
    T: Default + Clone,
{
    pub fn new(capacity: usize) -> Self {
        // Initialize a vector with default values
        let mut buffer = Vec::with_capacity(capacity);
        for _ in 0..capacity {
            buffer.push(T::default());
        }
        
        RingBuffer {
            buffer,
            capacity,
            read_index: 0,
            write_index: 0,
            count: 0,
        }
    }
    
    pub fn push(&mut self, item: T) -> Option<T> {
        // Use unsafe to handle the buffer as a raw pointer
        // Return the overwritten item if the buffer is full
        // Hint: Use pointer arithmetic to access elements directly
        // YOUR CODE HERE
    }
    
    pub fn pop(&mut self) -> Option<T> {
        // Use unsafe to pop an item from the buffer
        // Hint: Be careful about the count and indices
        // YOUR CODE HERE
    }
    
    pub fn len(&self) -> usize {
        self.count
    }
    
    pub fn is_empty(&self) -> bool {
        self.count == 0
    }
    
    pub fn is_full(&self) -> bool {
        self.count == self.capacity
    }
}

#[cfg(test)]
mod tests {
    use super::*;
    
    #[test]
    fn test_ringbuffer() {
        let mut buffer: RingBuffer<i32> = RingBuffer::new(3);
        
        assert!(buffer.is_empty());
        assert!(!buffer.is_full());
        
        buffer.push(1);
        buffer.push(2);
        buffer.push(3);
        
        assert!(buffer.is_full());
        
        // This should overwrite the first element
        assert_eq!(buffer.push(4), Some(1));
        
        assert_eq!(buffer.pop(), Some(2));
        assert_eq!(buffer.pop(), Some(3));
        assert_eq!(buffer.pop(), Some(4));
        assert_eq!(buffer.pop(), None);
    }
}
```

### Exercise 2: Advanced Traits - Streaming API

Create a streaming API using traits with associated types, similar to the Iterator trait.

```rust
// Define a Stream trait with an associated Item type
trait Stream {
    type Item;
    
    // Returns the next item in the stream, or None if the stream is done
    fn next(&mut self) -> Option<Self::Item>;
    
    // Transforms a stream into another stream using the provided function
    fn map<F, B>(self, f: F) -> Map<Self, F>
    where
        F: FnMut(Self::Item) -> B,
        Self: Sized,
    {
        Map {
            stream: self,
            f,
        }
    }
    
    // Filters items in the stream using the provided predicate
    fn filter<P>(self, predicate: P) -> Filter<Self, P>
    where
        P: FnMut(&Self::Item) -> bool,
        Self: Sized,
    {
        Filter {
            stream: self,
            predicate,
        }
    }
    
    // Collects all items in the stream into a collection
    fn collect<B>(mut self) -> B
    where
        B: FromStream<Self::Item>,
        Self: Sized,
    {
        B::from_stream(&mut self)
    }
}

// Define a trait for converting a stream into a collection
trait FromStream<T> {
    fn from_stream<S: Stream<Item = T>>(stream: &mut S) -> Self;
}

// Implement FromStream for Vec
impl<T> FromStream<T> for Vec<T> {
    fn from_stream<S: Stream<Item = T>>(stream: &mut S) -> Self {
        let mut vec = Vec::new();
        while let Some(item) = stream.next() {
            vec.push(item);
        }
        vec
    }
}

// Implement the Map adapter
struct Map<S, F> {
    stream: S,
    f: F,
}

impl<S, F, B> Stream for Map<S, F>
where
    S: Stream,
    F: FnMut(S::Item) -> B,
{
    type Item = B;
    
    fn next(&mut self) -> Option<Self::Item> {
        // Implement next for Map
        // YOUR CODE HERE
    }
}

// Implement the Filter adapter
struct Filter<S, P> {
    stream: S,
    predicate: P,
}

impl<S, P> Stream for Filter<S, P>
where
    S: Stream,
    P: FnMut(&S::Item) -> bool,
{
    type Item = S::Item;
    
    fn next(&mut self) -> Option<Self::Item> {
        // Implement next for Filter
        // YOUR CODE HERE
    }
}

// Implementation of a Range stream
struct RangeStream {
    start: i32,
    end: i32,
}

impl Stream for RangeStream {
    type Item = i32;
    
    fn next(&mut self) -> Option<Self::Item> {
        if self.start < self.end {
            let current = self.start;
            self.start += 1;
            Some(current)
        } else {
            None
        }
    }
}

fn main() {
    // Create a stream from a range and use the Stream trait methods
    let range = RangeStream { start: 1, end: 10 };
    
    // Transform the stream: keep only even numbers and multiply by 2
    let result: Vec<i32> = range
        .filter(|&x| x % 2 == 0)  // Keep only even numbers
        .map(|x| x * 2)           // Multiply each by 2
        .collect();               // Collect into a Vec
    
    println!("Result: {:?}", result);  // Should print [4, 8, 12, 16]
}
```

### Exercise 3: Type-Level State Machine

Implement a type-level state machine for a document workflow using type aliases and the newtype pattern.

```rust
// Define state types
struct Draft;
struct InReview;
struct Published;

// Document type that holds a state
struct Document<State> {
    content: String,
    state: State,
}

// Functions for Draft state
impl Document<Draft> {
    // Create a new document in Draft state
    fn new(content: String) -> Self {
        Document {
            content,
            state: Draft,
        }
    }
    
    // Submit for review, transforming to InReview state
    fn submit_for_review(self) -> Document<InReview> {
        Document {
            content: self.content,
            state: InReview,
        }
    }
    
    // Methods available in Draft state
    fn add_content(&mut self, text: &str) {
        self.content.push_str(text);
    }
}

// Functions for InReview state
impl Document<InReview> {
    // Approve the document, transforming to Published state
    fn approve(self) -> Document<Published> {
        // YOUR CODE HERE
    }
    
    // Reject the document, sending it back to Draft state
    fn reject(self) -> Document<Draft> {
        // YOUR CODE HERE
    }
    
    // Add review comments (no state change)
    fn add_review_comment(&mut self, comment: &str) {
        println!("Review comment added: {}", comment);
    }
}

// Functions for Published state
impl Document<Published> {
    // Methods available in Published state
    fn get_content(&self) -> &str {
        &self.content
    }
    
    // Create a new draft version of this document
    fn create_new_version(&self) -> Document<Draft> {
        // YOUR CODE HERE
    }
}

fn main() {
    // Create a document in Draft state
    let mut doc = Document::new(String::from("Initial draft content"));
    
    // Modify the draft
    doc.add_content("\nMore content for the draft.");
    
    // Submit for review
    let mut doc = doc.submit_for_review();
    
    // Add a review comment
    doc.add_review_comment("Looks good, but please fix the typo.");
    
    // We can't add content in the InReview state
    // Uncommenting the following line would cause a compilation error
    // doc.add_content("This won't work");
    
    // Approve the document
    let published_doc = doc.approve();
    
    // Access the content of the published document
    println!("Published content: {}", published_doc.get_content());
    
    // Create a new version
    let mut new_draft = published_doc.create_new_version();
    new_draft.add_content("\nUpdates for the new version.");
    
    println!("New draft content: {}", new_draft.content);
}
```

### Exercise 4: Function Pointers and Higher-Order Functions

Implement a simple command pattern using function pointers.

```rust
// Define a type for operations that can be executed
type Operation = fn(i32, i32) -> i32;

// Command structure that holds an operation and operands
struct Command {
    operation: Operation,
    x: i32,
    y: i32,
}

impl Command {
    fn new(operation: Operation, x: i32, y: i32) -> Self {
        Command { operation, x, y }
    }
    
    fn execute(&self) -> i32 {
        (self.operation)(self.x, self.y)
    }
}

// A history of commands that can be executed
struct CommandHistory {
    commands: Vec<Command>,
    results: Vec<i32>,
}

impl CommandHistory {
    fn new() -> Self {
        CommandHistory {
            commands: Vec::new(),
            results: Vec::new(),
        }
    }
    
    fn add_command(&mut self, command: Command) {
        let result = command.execute();
        self.commands.push(command);
        self.results.push(result);
    }
    
    // Higher-order function that applies an operation to all results
    fn process_results<F>(&self, f: F) -> Vec<i32>
    where
        F: Fn(i32) -> i32,
    {
        // YOUR CODE HERE
    }
    
    // Return a function that computes the sum of all results
    fn result_summer(&self) -> impl Fn() -> i32 + '_ {
        // YOUR CODE HERE
    }
}

// Operations
fn add(x: i32, y: i32) -> i32 {
    x + y
}

fn subtract(x: i32, y: i32) -> i32 {
    x - y
}

fn multiply(x: i32, y: i32) -> i32 {
    x * y
}

fn divide(x: i32, y: i32) -> i32 {
    if y == 0 {
        panic!("Division by zero");
    }
    x / y
}

fn main() {
    let mut history = CommandHistory::new();
    
    // Add some commands
    history.add_command(Command::new(add, 5, 3));        // 8
    history.add_command(Command::new(multiply, 2, 4));   // 8
    history.add_command(Command::new(subtract, 10, 5));  // 5
    history.add_command(Command::new(divide, 20, 4));    // 5
    
    // Process the results - double each one
    let doubled = history.process_results(|x| x * 2);
    println!("Doubled results: {:?}", doubled);  // [16, 16, 10, 10]
    
    // Get a function that sums all results
    let summer = history.result_summer();
    println!("Sum of all results: {}", summer());  // 26
    
    // Add another command and check the sum again
    history.add_command(Command::new(add, 7, 7));  // 14
    println!("New sum: {}", summer());  // 40
}
```

### Exercise 5: Create a Simple Declarative Macro

Implement a `hashmap!` macro for creating HashMaps with a convenient syntax.

```rust
use std::collections::HashMap;

// Create a macro that allows creating a HashMap with a concise syntax
// The macro should support multiple key-value pairs
// Example usage: hashmap!{1 => "one", 2 => "two", 3 => "three"}

// YOUR CODE HERE - Implement the hashmap! macro

fn main() {
    // Create a HashMap with string keys and integer values
    let map1 = hashmap!{
        "one" => 1,
        "two" => 2,
        "three" => 3
    };
    
    println!("{:?}", map1);
    
    // Create a HashMap with integer keys and string values
    let map2 = hashmap!{
        1 => "one",
        2 => "two",
        3 => "three"
    };
    
    println!("{:?}", map2);
    
    // Create an empty HashMap
    let map3: HashMap<u32, String> = hashmap!{};
    println!("{:?}", map3);
    
    // Test with a single item
    let map4 = hashmap!{"single" => "item"};
    println!("{:?}", map4);
}
```

## Challenge Project: Memory-Safe FFI Wrapper

Create a memory-safe Rust wrapper for a C library's functionality. In this exercise, we'll simulate this by writing our own "unsafe" code and then wrapping it with a safe API.

```rust
// Imaginary "C library" functions (we'll implement them in Rust with unsafe)
mod unsafe_lib {
    // Represents a handle to a resource in the C library
    pub type ResourceHandle = *mut libc::c_void;
    
    // Creates a new resource
    pub unsafe fn create_resource() -> ResourceHandle {
        let layout = std::alloc::Layout::new::<u64>();
        let ptr = std::alloc::alloc(layout) as ResourceHandle;
        if ptr.is_null() {
            panic!("Failed to allocate memory");
        }
        
        // Simulate initializing the resource
        let data_ptr = ptr as *mut u64;
        *data_ptr = 0;
        
        ptr
    }
    
    // Destroys a resource
    pub unsafe fn destroy_resource(handle: ResourceHandle) {
        if !handle.is_null() {
            let layout = std::alloc::Layout::new::<u64>();
            std::alloc::dealloc(handle as *mut u8, layout);
        }
    }
    
    // Sets a value in the resource
    pub unsafe fn set_value(handle: ResourceHandle, value: u64) -> bool {
        if handle.is_null() {
            return false;
        }
        
        let data_ptr = handle as *mut u64;
        *data_ptr = value;
        true
    }
    
    // Gets a value from the resource
    pub unsafe fn get_value(handle: ResourceHandle) -> Option<u64> {
        if handle.is_null() {
            return None;
        }
        
        let data_ptr = handle as *mut u64;
        Some(*data_ptr)
    }
}

// Now implement a safe Rust wrapper for this "C library"
struct Resource {
    // YOUR CODE HERE: Store the handle safely
}

impl Resource {
    // Create a new resource safely
    fn new() -> Self {
        // YOUR CODE HERE
    }
    
    // Set a value safely
    fn set_value(&mut self, value: u64) -> bool {
        // YOUR CODE HERE
    }
    
    // Get a value safely
    fn get_value(&self) -> Option<u64> {
        // YOUR CODE HERE
    }
}

// Implement Drop to ensure resources are properly cleaned up
impl Drop for Resource {
    fn drop(&mut self) {
        // YOUR CODE HERE
    }
}

fn main() {
    // Use the safe wrapper
    let mut resource = Resource::new();
    
    // Set some values
    resource.set_value(42);
    println!("Value: {:?}", resource.get_value());  // Should print 42
    
    resource.set_value(100);
    println!("Value: {:?}", resource.get_value());  // Should print 100
    
    // Resource will be automatically cleaned up when it goes out of scope
    
    // Test that our wrapper prevents memory leaks and use-after-free
    {
        let res1 = Resource::new();
        let res2 = Resource::new();
        // res1 and res2 will be dropped automatically
    }
    
    println!("No segfaults or memory leaks!");
}
```

## Additional Resources

- [The Rustonomicon](https://doc.rust-lang.org/nomicon/) - Advanced and unsafe Rust programming
- [Rust By Example: Unsafe Operations](https://doc.rust-lang.org/rust-by-example/unsafe.html)
- [The Little Book of Rust Macros](https://veykril.github.io/tlborm/) - In-depth guide to Rust macros
- [Rust Design Patterns](https://rust-unofficial.github.io/patterns/) - Common patterns including newtype and type state
- [Jon Gjengset's "Crust of Rust" videos](https://www.youtube.com/playlist?list=PLqbS7AVVErFiWDOAVrPt7aYmnuuOLYvOa) - Deep dives into Rust's advanced features
- [proc-macro Workshop](https://github.com/dtolnay/proc-macro-workshop) - Hands-on exercises for procedural macros