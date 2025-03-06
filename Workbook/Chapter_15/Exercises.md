# Chapter 15: Smart Pointers - Workbook

## Chapter Summary

This chapter explores smart pointers, which are data structures that act like pointers but with additional metadata and capabilities. The main smart pointers covered are:

1. **Box\<T\>**: Allows storing data on the heap rather than the stack
2. **Rc\<T\>**: Enables multiple ownership through reference counting
3. **RefCell\<T\>**: Provides interior mutability, allowing mutation of data even through immutable references
4. **Weak\<T\>**: Prevents reference cycles when using Rc\<T\>

These smart pointers implement important traits like `Deref` (for dereferencing operations) and `Drop` (for cleanup when going out of scope). Understanding these concepts is crucial for memory management and creating complex data structures in Rust.

## Key Concepts

### 1. Box\<T\> for Heap Allocation

`Box<T>` is the simplest smart pointer, allowing you to store data on the heap rather than the stack:

```rust
fn main() {
    // Allocate an integer on the heap
    let b = Box::new(5);
    println!("b = {}", b);
}
```

Key use cases for `Box<T>`:
- When you have a type whose size can't be known at compile time
- When you have a large amount of data and want to transfer ownership without copying
- When you want to own a value that implements a particular trait

Example of using `Box<T>` for recursive types:

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}

fn main() {
    let list = List::Cons(1, Box::new(List::Cons(2, Box::new(List::Nil))));
}
```

### 2. Deref Trait

The `Deref` trait allows you to customize the behavior of the dereference operator (`*`):

```rust
use std::ops::Deref;

struct MyBox<T>(T);

impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

fn main() {
    let x = 5;
    let y = MyBox::new(x);

    assert_eq!(5, x);
    assert_eq!(5, *y); // This works because of Deref
}
```

Deref coercion is a convenience feature that allows Rust to automatically convert:
- `&T` to `&U` when `T: Deref<Target=U>`
- `&mut T` to `&mut U` when `T: DerefMut<Target=U>`
- `&mut T` to `&U` when `T: Deref<Target=U>`

### 3. Drop Trait

The `Drop` trait lets you customize what happens when a value goes out of scope:

```rust
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}

fn main() {
    let c = CustomSmartPointer { data: String::from("my stuff") };
    let d = CustomSmartPointer { data: String::from("other stuff") };
    println!("CustomSmartPointers created.");
    // c and d go out of scope here, and drop is called
}
```

To drop a value early, use `std::mem::drop`:

```rust
fn main() {
    let c = CustomSmartPointer { data: String::from("some data") };
    println!("CustomSmartPointer created.");
    drop(c); // Explicitly drop c
    println!("CustomSmartPointer dropped before the end of main.");
}
```

### 4. Rc\<T\> for Multiple Ownership

`Rc<T>` (Reference Counting) enables multiple ownership of data:

```rust
use std::rc::Rc;

enum List {
    Cons(i32, Rc<List>),
    Nil,
}

use crate::List::{Cons, Nil};

fn main() {
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    let b = Cons(3, Rc::clone(&a));
    let c = Cons(4, Rc::clone(&a));
}
```

You can track the number of references with `Rc::strong_count`:

```rust
fn main() {
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    println!("count after creating a = {}", Rc::strong_count(&a));
    
    let b = Cons(3, Rc::clone(&a));
    println!("count after creating b = {}", Rc::strong_count(&a));
    
    {
        let c = Cons(4, Rc::clone(&a));
        println!("count after creating c = {}", Rc::strong_count(&a));
    }
    
    println!("count after c goes out of scope = {}", Rc::strong_count(&a));
}
```

### 5. RefCell\<T\> and Interior Mutability

`RefCell<T>` enforces borrowing rules at runtime instead of compile time:

```rust
use std::cell::RefCell;

fn main() {
    let data = RefCell::new(5);
    
    let borrowing = data.borrow();
    println!("Borrowed value: {}", borrowing);
    
    // This would panic at runtime (borrowing rules violation)
    // let mut borrow_mut1 = data.borrow_mut();
    // let mut borrow_mut2 = data.borrow_mut();
    
    // This works fine
    drop(borrowing);
    let mut borrow_mut = data.borrow_mut();
    *borrow_mut += 10;
    println!("Modified value: {}", borrow_mut);
}
```

The interior mutability pattern is useful for mock objects in testing:

```rust
pub trait Messenger {
    fn send(&self, msg: &str);
}

pub struct LimitTracker<'a, T: Messenger> {
    messenger: &'a T,
    value: usize,
    max: usize,
}

impl<'a, T> LimitTracker<'a, T>
where
    T: Messenger,
{
    pub fn new(messenger: &'a T, max: usize) -> LimitTracker<'a, T> {
        LimitTracker {
            messenger,
            value: 0,
            max,
        }
    }

    pub fn set_value(&mut self, value: usize) {
        self.value = value;
        
        let percentage_of_max = self.value as f64 / self.max as f64;
        
        if percentage_of_max >= 1.0 {
            self.messenger.send("Error: You are over your quota!");
        } else if percentage_of_max >= 0.9 {
            self.messenger.send("Urgent warning: You've used up over 90% of your quota!");
        } else if percentage_of_max >= 0.75 {
            self.messenger.send("Warning: You've used up over 75% of your quota!");
        }
    }
}

// In tests
#[cfg(test)]
mod tests {
    use super::*;
    use std::cell::RefCell;

    struct MockMessenger {
        sent_messages: RefCell<Vec<String>>,
    }

    impl MockMessenger {
        fn new() -> MockMessenger {
            MockMessenger {
                sent_messages: RefCell::new(vec![]),
            }
        }
    }

    impl Messenger for MockMessenger {
        fn send(&self, message: &str) {
            self.sent_messages.borrow_mut().push(String::from(message));
        }
    }

    #[test]
    fn it_sends_an_over_75_percent_warning_message() {
        let mock_messenger = MockMessenger::new();
        let mut limit_tracker = LimitTracker::new(&mock_messenger, 100);
        limit_tracker.set_value(80);
        assert_eq!(mock_messenger.sent_messages.borrow().len(), 1);
    }
}
```

### 6. Combining Rc\<T\> and RefCell\<T\>

You can combine `Rc<T>` and `RefCell<T>` to have multiple owners of mutable data:

```rust
use std::cell::RefCell;
use std::rc::Rc;

fn main() {
    let value = Rc::new(RefCell::new(5));
    
    let a = Rc::new(List::Cons(Rc::clone(&value), Rc::new(List::Nil)));
    let b = List::Cons(Rc::new(RefCell::new(3)), Rc::clone(&a));
    let c = List::Cons(Rc::new(RefCell::new(4)), Rc::clone(&a));
    
    *value.borrow_mut() += 10;
    
    println!("a after = {:?}", a);
    println!("b after = {:?}", b);
    println!("c after = {:?}", c);
}
```

### 7. Preventing Reference Cycles with Weak\<T\>

`Weak<T>` creates a non-owning reference to avoid reference cycles:

```rust
use std::cell::RefCell;
use std::rc::{Rc, Weak};

#[derive(Debug)]
struct Node {
    value: i32,
    parent: RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}

fn main() {
    let leaf = Rc::new(Node {
        value: 3,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![]),
    });
    
    println!("leaf parent = {:?}", leaf.parent.borrow().upgrade());
    
    let branch = Rc::new(Node {
        value: 5,
        parent: RefCell::new(Weak::new()),
        children: RefCell::new(vec![Rc::clone(&leaf)]),
    });
    
    *leaf.parent.borrow_mut() = Rc::downgrade(&branch);
    
    println!("leaf parent = {:?}", leaf.parent.borrow().upgrade());
}
```

## Practice Exercises

### Exercise 1: Basic Box Usage
Create a program to practice basic Box usage.

**Tasks:**
1. Create a Box holding an integer
2. Create a Box holding a string
3. Create a function that takes a Box as an argument and returns a Box
4. Create a nested structure of Boxes (Box within a Box)

### Exercise 2: Creating a Recursive Type with Box
Implement a binary tree structure using Box for recursive references.

**Tasks:**
1. Define a `BinaryTree` enum with `Node` and `Empty` variants
2. Implement methods to insert values into the tree
3. Implement a method to check if a value exists in the tree
4. Create a function to print the tree in-order

### Exercise 3: Implementing Deref
Create your own smart pointer type and implement the Deref trait.

**Tasks:**
1. Create a struct `SmartString` that wraps a String
2. Implement the Deref trait for SmartString
3. Demonstrate that you can use string methods directly on your SmartString
4. Create a function that takes a `&str` parameter and show that it works with SmartString through deref coercion

### Exercise 4: Implementing Drop
Create types that implement the Drop trait for resource cleanup.

**Tasks:**
1. Create a `FileResource` struct that simulates opening a file
2. Implement Drop to ensure the "file" is closed when the resource goes out of scope
3. Create nested structures where one resource depends on another
4. Demonstrate explicitly dropping a resource early with `std::mem::drop`

### Exercise 5: Using Rc for Shared Ownership
Practice using Rc for shared data.

**Tasks:**
1. Create a shared configuration object with Rc
2. Create multiple struct instances that hold references to the shared configuration
3. Track and print the reference count as references are created and dropped
4. Create a function that takes ownership of an Rc and returns a new Rc pointing to the same data

### Exercise 6: Interior Mutability with RefCell
Experiment with RefCell for interior mutability.

**Tasks:**
1. Create a cache that contains a RefCell for storing computed values
2. Create a function that checks the cache before computing a value
3. Create a test that uses RefCell to track method calls on a mock object
4. Deliberately create a runtime borrow error to see how RefCell panics

### Exercise 7: Combining Rc and RefCell
Create data structures that use both Rc and RefCell.

**Tasks:**
1. Create a shared, mutable document structure
2. Implement functionality for multiple views to update the document
3. Track changes to the document and notify all views
4. Ensure proper cleanup when views are dropped

### Exercise 8: Creating a Graph Structure
Implement a directed graph structure.

**Tasks:**
1. Create a Node struct that can have multiple outgoing edges
2. Use Rc to allow nodes to be shared
3. Add methods to add and remove edges
4. Implement a depth-first traversal algorithm

### Exercise 9: Managing Reference Cycles
Practice identifying and preventing reference cycles.

**Tasks:**
1. Create a data structure that results in a reference cycle
2. Detect the reference cycle through reference counting
3. Modify the structure to use Weak references to break the cycle
4. Write a test that proves the memory is properly cleaned up

### Exercise 10: Creating a Custom Smart Pointer
Design and implement your own smart pointer type.

**Tasks:**
1. Create a smart pointer that logs when values are accessed
2. Implement appropriate traits (Deref, Drop, etc.)
3. Add functionality to track how many times a value has been accessed
4. Create a function that works with both your smart pointer and regular references

## Challenge Project: Document Object Model (DOM)

Create a simplified Document Object Model (DOM) structure similar to HTML:

1. Create Element and Text node types
2. Allow elements to have parent-child relationships
3. Enable multiple views into the same document structure
4. Implement methods to:
   - Add and remove child nodes
   - Query and traverse the document structure
   - Modify attributes and properties of elements
   - Clone subtrees
5. Ensure no memory leaks or reference cycles
6. Write tests to verify the functionality

This project will exercise your understanding of:
- Smart pointers for memory management
- Reference counting for shared ownership
- Interior mutability for document modifications
- Weak references to prevent cycles
- Drop trait for cleanup

## Additional Resources

- [The Rust Programming Language - Smart Pointers Chapter](https://doc.rust-lang.org/book/ch15-00-smart-pointers.html)
- [The Rustonomicon - Smart Pointers](https://doc.rust-lang.org/nomicon/smart-ptrs.html)
- [API documentation for Box](https://doc.rust-lang.org/std/boxed/struct.Box.html)
- [API documentation for Rc](https://doc.rust-lang.org/std/rc/struct.Rc.html)
- [API documentation for RefCell](https://doc.rust-lang.org/std/cell/struct.RefCell.html)
- [API documentation for Weak](https://doc.rust-lang.org/std/rc/struct.Weak.html)

The concepts learned in this chapter will be essential for creating complex data structures and safe memory management in Rust.