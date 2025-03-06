# Chapter 18: Object-Oriented Programming Features of Rust - Workbook

## Chapter Overview

This chapter explores how Rust's features relate to object-oriented programming (OOP) concepts. While Rust is not strictly an object-oriented language, it provides mechanisms to implement many OOP patterns. The chapter examines how Rust's approach compares to traditional OOP and demonstrates alternative patterns that leverage Rust's unique capabilities.

## Key Concepts

### 1. OOP Characteristics in Rust

#### Objects: Data and Behavior
- In OOP, objects package data and methods that operate on that data
- In Rust, structs and enums store data, while `impl` blocks provide methods
- Example:
```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```

#### Encapsulation
- Rust supports encapsulation through its privacy rules
- By default, everything is private unless marked with `pub`
- Example encapsulated struct:
```rust
pub struct AveragedCollection {
    list: Vec<i32>,         // private field
    average: f64,           // private field
}

impl AveragedCollection {
    pub fn add(&mut self, value: i32) {
        self.list.push(value);
        self.update_average();
    }
    
    pub fn remove(&mut self) -> Option<i32> {
        let result = self.list.pop();
        match result {
            Some(value) => {
                self.update_average();
                Some(value)
            }
            None => None,
        }
    }
    
    pub fn average(&self) -> f64 {
        self.average
    }
    
    // Private helper method
    fn update_average(&mut self) {
        let total: i32 = self.list.iter().sum();
        self.average = total as f64 / self.list.len() as f64;
    }
}
```

#### Inheritance
- Rust does not support inheritance in the traditional OOP sense
- Instead of inheritance for code reuse, Rust uses:
  - Default trait implementations
  - Composition
- Instead of inheritance for polymorphism, Rust uses:
  - Trait objects
  - Generics with trait bounds

### 2. Trait Objects for Polymorphism

- Trait objects enable polymorphism - using values of different types through a common interface
- Syntax: `Box<dyn Trait>` or `&dyn Trait` 
- When to use trait objects:
  - When you need different types in the same collection
  - When you need runtime/dynamic dispatch
- Example:
```rust
pub trait Draw {
    fn draw(&self);
}

pub struct Screen {
    pub components: Vec<Box<dyn Draw>>,
}

impl Screen {
    pub fn run(&self) {
        for component in self.components.iter() {
            component.draw();
        }
    }
}

// Different types implementing the same trait
pub struct Button {
    pub width: u32,
    pub height: u32,
    pub label: String,
}

impl Draw for Button {
    fn draw(&self) {
        // drawing code for button
    }
}

struct SelectBox {
    width: u32,
    height: u32,
    options: Vec<String>,
}

impl Draw for SelectBox {
    fn draw(&self) {
        // drawing code for select box
    }
}
```

#### Static vs. Dynamic Dispatch

- **Static dispatch**: The compiler knows which method to call at compile time
  - Used with generics and trait bounds
  - Zero runtime cost, allows compiler optimizations
  - Creates monomorphized code for each concrete type
  - Works only when all types are known at compile time

- **Dynamic dispatch**: Method resolution happens at runtime
  - Used with trait objects
  - Small runtime cost for the lookup
  - Allows for flexibility when types aren't known at compile time
  - Supports heterogeneous collections

### 3. The State Pattern in Rust

The state pattern is a design pattern where an object's behavior changes based on its internal state. The chapter presents two ways to implement this in Rust:

#### Object-Oriented Approach (Using Trait Objects)

- Uses trait objects to model different states
- State transitions are handled within the objects
- Example blog post workflow:
  - Post starts as Draft
  - Can request review for a Draft
  - Can approve a post under review
  - Only approved posts show content

```rust
pub struct Post {
    state: Option<Box<dyn State>>,
    content: String,
}

trait State {
    fn request_review(self: Box<Self>) -> Box<dyn State>;
    fn approve(self: Box<Self>) -> Box<dyn State>;
    fn content<'a>(&self, post: &'a Post) -> &'a str {
        ""
    }
}

struct Draft {}
struct PendingReview {}
struct Published {}

// Implementation methods for Post and State trait
```

#### Type-Based Approach (Using Rust's Type System)

- Encodes states as different types
- State transitions consume the old state and return a new state
- Makes invalid states unrepresentable at compile time
- Requires reassignment when changing states

```rust
pub struct Post {
    content: String,
}

pub struct DraftPost {
    content: String,
}

pub struct PendingReviewPost {
    content: String,
}

impl Post {
    pub fn new() -> DraftPost {
        DraftPost {
            content: String::new(),
        }
    }
    
    pub fn content(&self) -> &str {
        &self.content
    }
}

impl DraftPost {
    pub fn add_text(&mut self, text: &str) {
        self.content.push_str(text);
    }
    
    pub fn request_review(self) -> PendingReviewPost {
        PendingReviewPost {
            content: self.content,
        }
    }
}

impl PendingReviewPost {
    pub fn approve(self) -> Post {
        Post {
            content: self.content,
        }
    }
}
```

## Practice Exercises

### Exercise 1: Trait Objects with a Drawing Application

Create a simple drawing application that can work with multiple shapes.

**Tasks:**
1. Define a `Draw` trait with methods for drawing and calculating area
2. Create at least three shapes that implement the trait (Circle, Rectangle, Triangle)
3. Create a `Canvas` struct that stores a vector of shapes as trait objects
4. Implement methods to add shapes to the canvas and calculate the total area

```rust
// Starter code
pub trait Draw {
    fn draw(&self);
    fn area(&self) -> f64;
}

pub struct Canvas {
    pub shapes: Vec<Box<dyn Draw>>,
}

impl Canvas {
    pub fn new() -> Self {
        Canvas { shapes: vec![] }
    }
    
    pub fn add_shape(&mut self, shape: Box<dyn Draw>) {
        self.shapes.push(shape);
    }
    
    pub fn draw_all(&self) {
        // Your implementation here
    }
    
    pub fn total_area(&self) -> f64 {
        // Your implementation here
    }
}

// Implement various shapes
```

### Exercise 2: Implementing Encapsulation

Create a `Counter` type that encapsulates its internal state and provides a controlled interface.

**Tasks:**
1. Create a `Counter` struct with private fields for current count, max value, and min value
2. Implement methods to increment, decrement, and get the current count
3. Ensure the counter never goes below min or above max
4. Add a method to reset the counter to a default value

```rust
// Starter code
pub struct Counter {
    // Your fields here
}

impl Counter {
    pub fn new(min: i32, max: i32) -> Self {
        // Your implementation here
    }
    
    pub fn increment(&mut self) -> i32 {
        // Your implementation here
    }
    
    pub fn decrement(&mut self) -> i32 {
        // Your implementation here
    }
    
    pub fn value(&self) -> i32 {
        // Your implementation here
    }
    
    pub fn reset(&mut self) {
        // Your implementation here
    }
}
```

### Exercise 3: Comparing Approaches to Polymorphism

Implement a simple task management system using both generics with trait bounds and trait objects.

**Tasks:**
1. Define a `Task` trait with methods for description, priority, and completion
2. Create implementations for different task types (Bug, Feature, Documentation)
3. Implement a `TaskManager` using generics and trait bounds
4. Implement a `TaskQueue` using trait objects
5. Compare the trade-offs between the two approaches

```rust
// Starter code
pub trait Task {
    fn description(&self) -> &str;
    fn priority(&self) -> u32;
    fn is_completed(&self) -> bool;
    fn complete(&mut self);
}

// Generic version
pub struct TaskManager<T: Task> {
    tasks: Vec<T>,
}

// Trait object version
pub struct TaskQueue {
    tasks: Vec<Box<dyn Task>>,
}

// Implement different task types and methods for both manager types
```

### Exercise 4: State Pattern - Vending Machine

Implement a vending machine that uses the state pattern.

**Tasks:**
1. Create states for a vending machine: Ready, ItemSelected, Processing, and OutOfStock
2. Implement appropriate transitions between states
3. Allow actions like selecting items, inserting money, and dispensing products
4. Make certain actions only valid in certain states

**Version A:** Implement using trait objects (OOP approach)
**Version B:** Implement using types to represent states (Rust approach)

```rust
// Starter code for trait object approach
pub struct VendingMachine {
    state: Option<Box<dyn State>>,
    items: Vec<Item>,
    selected_item: Option<usize>,
    inserted_money: u32,
}

trait State {
    fn select_item(self: Box<Self>, machine: &mut VendingMachine, item_index: usize) 
        -> Box<dyn State>;
    fn insert_money(self: Box<Self>, machine: &mut VendingMachine, amount: u32) 
        -> Box<dyn State>;
    fn dispense(self: Box<Self>, machine: &mut VendingMachine) 
        -> Box<dyn State>;
}

// Implement states and VendingMachine methods
```

### Exercise 5: GUI Component Library

Design a simple GUI component library that supports composite components.

**Tasks:**
1. Define a `Component` trait with methods like `render`, `handle_click`, and `get_dimensions`
2. Create basic components like Button, TextBox, and Label
3. Create a Container component that can hold other components (composite pattern)
4. Implement layout algorithms for the Container
5. Create a simple application with nested components

```rust
// Starter code
pub trait Component {
    fn render(&self);
    fn handle_click(&mut self, x: u32, y: u32) -> bool;
    fn get_dimensions(&self) -> (u32, u32);
}

pub struct Button {
    x: u32,
    y: u32,
    width: u32,
    height: u32,
    label: String,
    is_clicked: bool,
}

pub struct Container {
    x: u32,
    y: u32,
    width: u32,
    height: u32,
    components: Vec<Box<dyn Component>>,
}

// Implement components and Container methods
```

## Challenge Project: Document Management System

Create a document management system that uses OOP principles and Rust's strengths:

**Requirements:**
1. Support different document types (Text, Spreadsheet, Presentation, PDF)
2. Allow reading, writing, and formatting operations on documents
3. Implement access control with different user roles (Admin, Editor, Viewer)
4. Support document versioning and history
5. Create a system to organize documents in folders and search them

**Core Structures:**

1. Document trait with common operations
2. Specific document type implementations
3. User and Permission system
4. Version control system
5. Document storage and retrieval system

**Extensions:**
- Implement basic content parsing and indexing for search
- Add collaborative editing features
- Implement document conversion between formats

## Additional Resources

- [Rust Design Patterns](https://rust-unofficial.github.io/patterns/) - Community collection of design patterns and idioms in Rust
- [Rust by Example: Traits](https://doc.rust-lang.org/rust-by-example/trait.html) - Official examples of working with traits
- [Design Patterns: Elements of Reusable Object-Oriented Software](https://www.oreilly.com/library/view/design-patterns-elements/0201633612/) - The "Gang of Four" book on OOP design patterns
- [Object-Oriented vs. Functional Programming](https://www.oreilly.com/content/object-oriented-vs-functional-programming/) - Comparison of programming paradigms
- [Trait Objects vs. Generics](https://www.oreilly.com/library/view/programming-rust-2nd/9781492052586/) - Chapter from "Programming Rust" on polymorphism approaches