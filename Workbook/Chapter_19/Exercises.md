# Chapter 19: Patterns and Matching - Workbook

## Chapter Overview

Chapter 19 explores patterns, a powerful feature in Rust for matching against the structure of types. Patterns allow you to destructure complex data types, test for specific conditions, and extract relevant values. This chapter covers where patterns can be used, the difference between refutable and irrefutable patterns, and the various syntax options available for creating patterns.

## Key Concepts

### 1. What are Patterns?

Patterns in Rust are a special syntax for matching against the structure of types. A pattern consists of some combination of:

- Literals (like `5` or `"hello"`)
- Destructured arrays, enums, structs, or tuples
- Variables
- Wildcards (`_`)
- Placeholders

Patterns are used to:
- Check if a value has a particular structure
- Bind parts of a value to variables
- Extract values from complex data structures

```rust
// Simple pattern matching with match
match value {
    1 => println!("One"),
    2 => println!("Two"),
    _ => println!("Other number"),
}
```

### 2. Places Patterns Can Be Used

Rust allows patterns in several contexts:

#### a. `match` Arms
```rust
match some_option {
    None => None,
    Some(i) => Some(i + 1),
}
```

#### b. `if let` Expressions
```rust
if let Some(value) = optional_value {
    println!("The value is: {}", value);
}
```

#### c. `while let` Loops
```rust
while let Some(value) = stack.pop() {
    println!("Popped value: {}", value);
}
```

#### d. `for` Loops
```rust
let pairs = vec![(1, 2), (3, 4), (5, 6)];
for (a, b) in pairs {
    println!("({}, {})", a, b);
}
```

#### e. `let` Statements
```rust
let (x, y, z) = (1, 2, 3);
```

#### f. Function Parameters
```rust
fn print_coordinates(&(x, y): &(i32, i32)) {
    println!("Current location: ({}, {})", x, y);
}
```

### 3. Refutable vs. Irrefutable Patterns

Patterns come in two forms:

- **Irrefutable patterns**: Patterns that will match for any possible value
  - Example: `let x = 5;` (x matches anything)
  - Used in: `let` statements, function parameters, `for` loops

- **Refutable patterns**: Patterns that can fail to match for some possible value
  - Example: `if let Some(x) = a_value` (fails if `a_value` is `None`)
  - Used in: `if let`, `while let`, `match` arms

```rust
// Irrefutable pattern (always matches)
let x = 5;

// Refutable pattern (might not match)
if let Some(value) = optional_value {
    println!("Got a value: {}", value);
}
```

### 4. Pattern Syntax

#### a. Matching Literals
```rust
match x {
    1 => println!("one"),
    2 => println!("two"),
    _ => println!("anything else"),
}
```

#### b. Matching Named Variables
```rust
match x {
    Some(50) => println!("Got 50"),
    Some(y) => println!("Got {}", y),  // y binds to the value in Some
    _ => println!("Got something else"),
}
```

#### c. Multiple Patterns with `|`
```rust
match x {
    1 | 2 => println!("one or two"),
    3 => println!("three"),
    _ => println!("anything else"),
}
```

#### d. Matching Ranges with `..=`
```rust
match x {
    1..=5 => println!("one through five"),
    _ => println!("something else"),
}
```

#### e. Destructuring Structs
```rust
struct Point {
    x: i32,
    y: i32,
}

let p = Point { x: 0, y: 7 };

// Long form
let Point { x: a, y: b } = p;

// Shorthand when variable names match field names
let Point { x, y } = p;

// With literal values
match p {
    Point { x, y: 0 } => println!("On the x axis at {}", x),
    Point { x: 0, y } => println!("On the y axis at {}", y),
    Point { x, y } => println!("On neither axis: ({}, {})", x, y),
}
```

#### f. Destructuring Enums
```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

match msg {
    Message::Quit => println!("Quit"),
    Message::Move { x, y } => println!("Move to ({}, {})", x, y),
    Message::Write(text) => println!("Text: {}", text),
    Message::ChangeColor(r, g, b) => println!("Change color to ({}, {}, {})", r, g, b),
}
```

#### g. Destructuring Nested Types
```rust
enum Color {
    Rgb(i32, i32, i32),
    Hsv(i32, i32, i32),
}

enum Message {
    ChangeColor(Color),
    // ...other variants
}

match msg {
    Message::ChangeColor(Color::Rgb(r, g, b)) => println!("RGB: ({}, {}, {})", r, g, b),
    Message::ChangeColor(Color::Hsv(h, s, v)) => println!("HSV: ({}, {}, {})", h, s, v),
    // ...other arms
}
```

#### h. Ignoring Values

With `_` (ignores a single value)
```rust
fn foo(_: i32, y: i32) {
    // First parameter is ignored
    println!("y = {}", y);
}
```

With `..` (ignores remaining parts)
```rust
struct Point {
    x: i32,
    y: i32,
    z: i32,
}

let origin = Point { x: 0, y: 0, z: 0 };

match origin {
    Point { x, .. } => println!("x is {}", x),
}

// In tuples
let numbers = (2, 4, 8, 16, 32);
match numbers {
    (first, .., last) => println!("First: {}, Last: {}", first, last),
}
```

#### i. Match Guards
```rust
match x {
    Some(n) if n < 5 => println!("less than five: {}", n),
    Some(n) => println!("n: {}", n),
    None => (),
}
```

#### j. `@` Bindings
```rust
match msg {
    Message::Hello { id: id_variable @ 3..=7 } => {
        println!("Found an id in range: {}", id_variable)
    },
    Message::Hello { id: 10..=12 } => {
        println!("Found an id in another range")
    },
    Message::Hello { id } => {
        println!("Found some other id: {}", id)
    },
}
```

## Practice Exercises

### Exercise 1: Weather Forecasting System

Create a weather analysis system that uses pattern matching to classify and respond to different weather conditions.

```rust
enum Temperature {
    Celsius(i32),
    Fahrenheit(i32),
}

enum WindSpeed {
    Calm,
    Breeze(u32), // in km/h
    Gale(u32),   // in km/h
    Storm(u32),  // in km/h
}

enum Precipitation {
    None,
    Rain(f32),   // in mm
    Snow(f32),   // in cm
}

struct WeatherData {
    location: String,
    temp: Temperature,
    wind: WindSpeed,
    precipitation: Precipitation,
}

// 1. Implement this function to classify the severity of the weather
fn weather_severity(data: &WeatherData) -> &str {
    // Use match expressions and patterns to determine severity
    // "Severe" if storm winds OR heavy snow (>10cm) OR freezing temps (<-10C or <14F)
    // "Moderate" if gale winds OR moderate precipitation OR cold temps
    // "Mild" for other conditions
    // Remember to convert between Celsius and Fahrenheit for accurate comparison
    todo!()
}

// 2. Implement this function to provide specific weather warnings
fn weather_warning(data: &WeatherData) -> Option<String> {
    // Return Some with a specific warning message for dangerous conditions
    // Use pattern guards, nested patterns, and @ bindings where appropriate
    // Return None if no warning is needed
    todo!()
}

// 3. Implement this function to suggest activities based on weather
fn suggest_activities(data: &WeatherData) -> Vec<String> {
    // Return different activity suggestions based on the weather conditions
    // Use destructuring and multiple patterns where appropriate
    todo!()
}

fn main() {
    let weather_reports = vec![
        WeatherData {
            location: String::from("Mountains"),
            temp: Temperature::Celsius(-15),
            wind: WindSpeed::Gale(65),
            precipitation: Precipitation::Snow(20.0),
        },
        WeatherData {
            location: String::from("Coast"),
            temp: Temperature::Fahrenheit(72),
            wind: WindSpeed::Breeze(15),
            precipitation: Precipitation::None,
        },
        WeatherData {
            location: String::from("Plains"),
            temp: Temperature::Celsius(22),
            wind: WindSpeed::Calm,
            precipitation: Precipitation::Rain(2.8),
        },
        WeatherData {
            location: String::from("City"),
            temp: Temperature::Fahrenheit(35),
            wind: WindSpeed::Storm(90),
            precipitation: Precipitation::Snow(5.0),
        },
    ];
    
    for report in &weather_reports {
        println!("Weather for {}: {} severity", report.location, weather_severity(report));
        
        match weather_warning(report) {
            Some(warning) => println!("WARNING: {}", warning),
            None => println!("No special warnings"),
        }
        
        let activities = suggest_activities(report);
        println!("Suggested activities:");
        for activity in activities {
            println!("- {}", activity);
        }
        println!();
    }
}
```

### Exercise 2: E-commerce Order Processing

Implement an order processing system that uses patterns to handle different types of orders, payment methods, and shipping options.

```rust
enum PaymentMethod {
    CreditCard { number: String, expiry: String, cvv: u32 },
    DebitCard { number: String, expiry: String, pin: u32 },
    PayPal(String), // email
    ApplePay,
    CryptoCurrency { currency: String, address: String },
}

enum ShippingMethod {
    Standard,
    Express,
    NextDay,
    Pickup { store_id: String },
    Digital,
}

enum ProductType {
    Physical { weight: f32, dimensions: (f32, f32, f32) },
    Digital { size_mb: u32, format: String },
    Subscription { duration_months: u32 },
}

struct Product {
    id: String,
    name: String,
    price: f32,
    product_type: ProductType,
}

struct Order {
    order_id: String,
    customer_name: String,
    items: Vec<Product>,
    payment: PaymentMethod,
    shipping: ShippingMethod,
    total: f32,
}

// 1. Implement a function to validate payment information
fn validate_payment(payment: &PaymentMethod) -> Result<(), String> {
    // Use patterns to validate different payment methods
    // Return Ok(()) for valid payments or Err with a message for invalid ones
    // Credit/Debit cards must have 16-digit numbers and valid expiry
    // PayPal must have @ in the email
    // CryptoCurrency address must be at least 20 chars
    todo!()
}

// 2. Calculate shipping costs based on order details
fn calculate_shipping_cost(order: &Order) -> f32 {
    // Use patterns to calculate different shipping costs based on:
    // - Shipping method
    // - Product types and weights
    // - Order total
    // Digital products and Pickup have no shipping cost
    todo!()
}

// 3. Process the order and return a status message
fn process_order(order: &Order) -> String {
    // Use patterns to generate different processing instructions
    // Consider special cases like:
    // - All digital products can be fulfilled immediately
    // - NextDay shipping needs priority processing
    // - Orders with certain payment methods need additional verification
    todo!()
}

fn main() {
    let orders = vec![
        Order {
            order_id: String::from("ORD-1234"),
            customer_name: String::from("Alice Johnson"),
            items: vec![
                Product {
                    id: String::from("PROD-001"),
                    name: String::from("Laptop"),
                    price: 1299.99,
                    product_type: ProductType::Physical {
                        weight: 2.5,
                        dimensions: (35.0, 25.0, 2.5),
                    },
                },
            ],
            payment: PaymentMethod::CreditCard {
                number: String::from("4111111111111111"),
                expiry: String::from("12/24"),
                cvv: 123,
            },
            shipping: ShippingMethod::Express,
            total: 1299.99,
        },
        Order {
            order_id: String::from("ORD-5678"),
            customer_name: String::from("Bob Smith"),
            items: vec![
                Product {
                    id: String::from("PROD-002"),
                    name: String::from("E-book"),
                    price: 14.99,
                    product_type: ProductType::Digital {
                        size_mb: 5,
                        format: String::from("PDF"),
                    },
                },
                Product {
                    id: String::from("PROD-003"),
                    name: String::from("Online Course"),
                    price: 49.99,
                    product_type: ProductType::Subscription {
                        duration_months: 6,
                    },
                },
            ],
            payment: PaymentMethod::PayPal(String::from("bob.smith@example.com")),
            shipping: ShippingMethod::Digital,
            total: 64.98,
        },
        // Add more test orders with different combinations
    ];
    
    for order in &orders {
        println!("Processing order: {}", order.order_id);
        
        match validate_payment(&order.payment) {
            Ok(()) => println!("Payment validated successfully"),
            Err(msg) => println!("Payment error: {}", msg),
        }
        
        let shipping_cost = calculate_shipping_cost(order);
        println!("Shipping cost: ${:.2}", shipping_cost);
        
        let status = process_order(order);
        println!("Order status: {}", status);
        println!();
    }
}
```

### Exercise 3: Task Scheduler

Create a task scheduling system that uses pattern matching to handle different types of tasks and their states.

```rust
use std::time::{Duration, Instant};

enum Priority {
    Low, 
    Medium, 
    High, 
    Critical
}

enum Frequency {
    Once,
    Daily,
    Weekly(u8), // Day of week (0 = Sunday)
    Monthly(u8), // Day of month
    Custom(Duration),
}

enum Status {
    Pending,
    InProgress { start_time: Instant },
    Completed { start_time: Instant, end_time: Instant },
    Failed { error: String },
    Cancelled,
}

struct Task {
    id: u32,
    name: String,
    description: Option<String>,
    priority: Priority,
    status: Status,
    frequency: Frequency,
    tags: Vec<String>,
}

// 1. Implement a function to get the next run time for a task
fn get_next_run_time(task: &Task) -> Option<String> {
    // Use pattern matching to determine when the task should run next
    // Return None if the task won't run again (completed one-time tasks)
    // For this exercise, return a string description of when it will run next
    // Use nested patterns and pattern guards where appropriate
    todo!()
}

// 2. Implement a task filter function
fn filter_tasks<'a>(tasks: &'a [Task], filter: &str) -> Vec<&'a Task> {
    // Use pattern matching to filter tasks based on the filter string
    // Filter formats:
    //   "priority:high" - match tasks with high priority
    //   "status:pending" - match tasks with pending status
    //   "tag:urgent" - match tasks with the "urgent" tag
    //   "frequency:daily" - match tasks with daily frequency
    //   "id:42" - match task with id 42
    // Return all tasks if filter doesn't match any known format
    todo!()
}

// 3. Implement a function to get a task summary
fn task_summary(task: &Task) -> String {
    // Use pattern matching to generate a summary string for the task
    // Include different details based on status, priority, etc.
    // Summarize duration for completed tasks
    // Include error message for failed tasks
    // etc.
    todo!()
}

fn main() {
    let now = Instant::now();
    let one_hour_ago = now - Duration::from_secs(3600);
    
    let tasks = vec![
        Task {
            id: 1,
            name: String::from("Complete project proposal"),
            description: Some(String::from("Finish the Q4 project proposal for client")),
            priority: Priority::High,
            status: Status::Completed {
                start_time: one_hour_ago,
                end_time: now,
            },
            frequency: Frequency::Once,
            tags: vec![String::from("client"), String::from("proposal")],
        },
        Task {
            id: 2,
            name: String::from("Team meeting"),
            description: None,
            priority: Priority::Medium,
            status: Status::Pending,
            frequency: Frequency::Weekly(1), // Monday
            tags: vec![String::from("meeting"), String::from("team")],
        },
        Task {
            id: 3,
            name: String::from("Server backup"),
            description: Some(String::from("Run full server backup")),
            priority: Priority::Critical,
            status: Status::Failed {
                error: String::from("Insufficient disk space"),
            },
            frequency: Frequency::Daily,
            tags: vec![String::from("maintenance"), String::from("server")],
        },
        // Add more test tasks...
    ];
    
    println!("Task Summaries:");
    for task in &tasks {
        println!("{}", task_summary(task));
        
        if let Some(next_run) = get_next_run_time(task) {
            println!("Next run: {}", next_run);
        } else {
            println!("No future runs scheduled");
        }
        println!();
    }
    
    let filters = vec!["priority:high", "status:pending", "tag:server", "frequency:daily"];
    for filter in filters {
        let filtered_tasks = filter_tasks(&tasks, filter);
        println!("Filter '{}' matched {} tasks:", filter, filtered_tasks.len());
        for task in filtered_tasks {
            println!("- {} (ID: {})", task.name, task.id);
        }
        println!();
    }
}
```

### Exercise 4: Data Analysis Pipeline

Create a data processing system that uses pattern matching to transform and analyze different types of data.

```rust
use std::collections::HashMap;

enum DataValue {
    Integer(i64),
    Float(f64),
    Text(String),
    Boolean(bool),
    Array(Vec<DataValue>),
    Map(HashMap<String, DataValue>),
    Null,
}

enum Transformation {
    ExtractField(String),
    ConvertToInteger,
    ConvertToFloat,
    ConvertToBoolean,
    ArrayLength,
    Filter { field: String, pattern: FilterPattern },
    SortBy(String),
    GroupBy(String),
}

enum FilterPattern {
    Equals(DataValue),
    GreaterThan(DataValue),
    LessThan(DataValue),
    Contains(String),
    StartsWith(String),
    EndsWith(String),
}

struct DataPipeline {
    transformations: Vec<Transformation>,
}

// 1. Implement a function to apply a single transformation to a data value
fn apply_transformation(data: DataValue, transformation: &Transformation) -> Result<DataValue, String> {
    // Use pattern matching to handle different transformation types
    // and different input data types
    // Return Err with a descriptive message if the transformation cannot be applied
    todo!()
}

// 2. Implement a function to execute the entire pipeline
fn execute_pipeline(pipeline: &DataPipeline, input: DataValue) -> Result<DataValue, String> {
    // Apply each transformation in sequence
    // Use pattern matching to handle errors and intermediate results
    todo!()
}

// 3. Implement a function to pretty-print the results
fn format_data_value(data: &DataValue, indent: usize) -> String {
    // Use pattern matching to format different data types
    // Recursive function to handle nested structures
    // Return nicely formatted string representation
    todo!()
}

fn main() {
    // Create test data
    let mut user_map = HashMap::new();
    user_map.insert(String::from("name"), DataValue::Text(String::from("Alice Johnson")));
    user_map.insert(String::from("age"), DataValue::Integer(32));
    user_map.insert(String::from("is_active"), DataValue::Boolean(true));
    
    let mut address_map = HashMap::new();
    address_map.insert(String::from("street"), DataValue::Text(String::from("123 Main St")));
    address_map.insert(String::from("city"), DataValue::Text(String::from("Springfield")));
    address_map.insert(String::from("zip"), DataValue::Text(String::from("12345")));
    
    user_map.insert(String::from("address"), DataValue::Map(address_map));
    
    let mut skills = Vec::new();
    skills.push(DataValue::Text(String::from("Programming")));
    skills.push(DataValue::Text(String::from("Design")));
    skills.push(DataValue::Text(String::from("Communication")));
    
    user_map.insert(String::from("skills"), DataValue::Array(skills));
    
    let user_data = DataValue::Map(user_map);
    
    // Create pipelines to test
    let pipelines = vec![
        DataPipeline {
            transformations: vec![
                Transformation::ExtractField(String::from("skills")),
                Transformation::ArrayLength,
            ],
        },
        DataPipeline {
            transformations: vec![
                Transformation::ExtractField(String::from("address")),
                Transformation::ExtractField(String::from("city")),
            ],
        },
        DataPipeline {
            transformations: vec![
                Transformation::ExtractField(String::from("age")),
                Transformation::ConvertToFloat,
            ],
        },
    ];
    
    // Execute and display results
    for (i, pipeline) in pipelines.iter().enumerate() {
        println!("Pipeline #{}:", i + 1);
        
        match execute_pipeline(pipeline, user_data.clone()) {
            Ok(result) => {
                println!("Result: {}", format_data_value(&result, 2));
            },
            Err(e) => {
                println!("Error: {}", e);
            },
        }
        println!();
    }
}
```

### Exercise 5: Network Protocol Parser

Build a protocol parser that uses pattern matching to decode and process network packets.

```rust
enum IpAddress {
    V4([u8; 4]),
    V6([u16; 8]),
}

enum Protocol {
    TCP,
    UDP,
    ICMP,
    HTTP,
    HTTPS,
    Unknown(u8),
}

enum HttpMethod {
    GET,
    POST,
    PUT,
    DELETE,
    OPTIONS,
    HEAD,
    PATCH,
    Unknown(String),
}

struct MacAddress([u8; 6]);

struct EthernetFrame {
    destination: MacAddress,
    source: MacAddress,
    protocol: u16, // 0x0800 for IPv4, 0x86DD for IPv6
    payload: Vec<u8>,
}

struct IpPacket {
    version: u8,     // 4 for IPv4, 6 for IPv6
    source: IpAddress,
    destination: IpAddress,
    protocol: u8,    // 6 for TCP, 17 for UDP, etc.
    ttl: u8,
    payload: Vec<u8>,
}

struct TcpSegment {
    source_port: u16,
    destination_port: u16,
    sequence_number: u32,
    ack_number: u32,
    flags: u8,  // SYN, ACK, FIN, etc.
    payload: Vec<u8>,
}

struct HttpRequest {
    method: HttpMethod,
    uri: String,
    version: String,
    headers: Vec<(String, String)>,
    body: Option<Vec<u8>>,
}

enum NetworkPacket {
    Ethernet(EthernetFrame),
    IP(IpPacket),
    TCP(TcpSegment),
    HTTP(HttpRequest),
    Unparsed(Vec<u8>),
}

// 1. Implement a function to parse raw bytes into a NetworkPacket
fn parse_packet(data: &[u8]) -> NetworkPacket {
    // Use pattern matching to identify and parse different packet types
    // Check for header signatures and protocol identifiers
    // For this exercise, use simplified parsing (we're focusing on pattern matching)
    todo!()
}

// 2. Implement a function to extract protocol information
fn extract_protocol(packet: &NetworkPacket) -> Protocol {
    // Use pattern matching to determine the protocol
    // Handle nested protocols (e.g., HTTP inside TCP inside IP)
    todo!()
}

// 3. Implement a function to build a human-readable packet summary
fn packet_summary(packet: &NetworkPacket) -> String {
    // Use pattern matching to format the packet details
    // Provide different levels of detail based on packet type
    todo!()
}

fn main() {
    // Simplified packet samples (in real life these would be raw bytes)
    let sample_packets = vec![
        // Simplified Ethernet frame with IPv4 protocol
        NetworkPacket::Ethernet(EthernetFrame {
            source: MacAddress([0x00, 0x1A, 0x3F, 0x98, 0x76, 0x54]),
            destination: MacAddress([0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF]),
            protocol: 0x0800, // IPv4
            payload: vec![0, 1, 2, 3, 4], // Simplified payload
        }),
        
        // IPv4 packet with TCP protocol
        NetworkPacket::IP(IpPacket {
            version: 4,
            source: IpAddress::V4([192, 168, 1, 100]),
            destination: IpAddress::V4([93, 184, 216, 34]),
            protocol: 6, // TCP
            ttl: 64,
            payload: vec![0, 1, 2, 3, 4], // Simplified payload
        }),
        
        // TCP segment with HTTP payload
        NetworkPacket::TCP(TcpSegment {
            source_port: 52123,
            destination_port: 80,
            sequence_number: 123456789,
            ack_number: 987654321,
            flags: 0x18, // PSH, ACK
            payload: b"GET / HTTP/1.1\r\nHost: example.com\r\n\r\n".to_vec(),
        }),
        
        // HTTP request
        NetworkPacket::HTTP(HttpRequest {
            method: HttpMethod::GET,
            uri: String::from("/index.html"),
            version: String::from("HTTP/1.1"),
            headers: vec![
                (String::from("Host"), String::from("www.example.com")),
                (String::from("User-Agent"), String::from("Rust Pattern Matcher")),
            ],
            body: None,
        }),
    ];
    
    for packet in &sample_packets {
        println!("Packet Protocol: {:?}", extract_protocol(packet));
        println!("Summary: {}", packet_summary(packet));
        println!();
    }
    
    // Let's imagine we have some raw bytes to parse
    let raw_data = vec![
        // This would be raw network packet data in a real scenario
        0x45, 0x00, 0x00, 0x40, 0xad, 0x30, 0x40, 0x00,
        0x40, 0x06, 0x96, 0x15, 0xc0, 0xa8, 0x01, 0x64,
        // ... more bytes ...
    ];
    

## Challenge Project: Command Parser

Create a command parser for a text-based game that uses pattern matching to interpret user commands.

### Requirements:
1. Parse commands like "go north", "take sword", "drop book", "talk to wizard"
2. Handle complex commands with optional modifiers like "attack dragon with sword"
3. Support abbreviations and alternative phrasings
4. Provide helpful error messages for invalid commands

```rust
enum Direction {
    North, South, East, West, Up, Down
}

enum Item {
    Weapon(String),
    Armor(String),
    Potion(String),
    Key(String),
    Treasure(String),
    Misc(String),
}

enum Character {
    Friend(String),
    Enemy(String),
    Neutral(String),
}

enum Command {
    Go(Direction),
    Take(Item),
    Drop(Item),
    Use(Item),
    Attack(Character, Option<Item>),
    Talk(Character),
    Look(Option<Direction>),
    Inventory,
    Help,
    Quit,
    Unknown(String),
}

fn parse_command(input: &str) -> Command {
    // Implement the command parser using pattern matching
    // Extract command components with string methods and match them
    // Return the appropriate Command enum variant
}

fn execute_command(command: Command) {
    // Execute the command and display appropriate messages
    match command {
        // Handle each command type
    }
}

fn main() {
    println!("Welcome to the Pattern Matching Adventure Game!");
    println!("Type 'help' for a list of commands, or 'quit' to exit.");
    
    let mut input = String::new();
    
    loop {
        input.clear();
        print!("> ");
        std::io::stdout().flush().unwrap();
        std::io::stdin().read_line(&mut input).unwrap();
        
        let command = parse_command(input.trim());
        execute_command(command);
    }
}
```

## Additional Resources

- [Rust By Example: Patterns](https://doc.rust-lang.org/rust-by-example/flow_control/match.html) - Examples of pattern matching in various contexts
- [Rust Documentation on Patterns](https://doc.rust-lang.org/book/ch18-00-patterns.html) - More in-depth exploration of patterns
- [Rust Reference: Patterns](https://doc.rust-lang.org/reference/patterns.html) - Technical details about pattern syntax
- [Matching in Rust Playground](https://play.rust-lang.org/) - Experiment with patterns interactively