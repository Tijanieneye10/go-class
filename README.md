# The Complete Go (Golang) Masterclass — From Zero to Production

> **A comprehensive, hands-on guide covering everything from Go fundamentals to building production-grade REST APIs with Chi.**
> Arranged in progressive learning order. Every concept builds on the previous one.

---

## Table of Contents

1. [Getting Started — Installation & Setup](#1-getting-started)
2. [Go Fundamentals](#2-go-fundamentals)
3. [Control Flow & Functions](#3-control-flow--functions)
4. [Data Structures — Arrays, Slices, Maps](#4-data-structures)
5. [Structs, Methods & Interfaces](#5-structs-methods--interfaces)
6. [Pointers & Memory](#6-pointers--memory)
7. [Error Handling](#7-error-handling)
8. [Packages & Modules](#8-packages--modules)
9. [Concurrency — Goroutines & Channels](#9-concurrency--goroutines--channels)
10. [Context Package](#10-context-package)
11. [Working with JSON](#11-working-with-json)
12. [HTTP Fundamentals — net/http](#12-http-fundamentals--nethttp)
13. [Introduction to Chi Router](#13-introduction-to-chi-router)
14. [Building a REST API with Chi](#14-building-a-rest-api-with-chi)
15. [Middleware — Concepts & Implementation](#15-middleware)
16. [Request Validation](#16-request-validation)
17. [Database — SQL with pgx & sqlx](#17-database--sql-with-pgx)
18. [Database Migrations with Goose](#18-database-migrations-with-goose)
19. [Database Transactions](#19-database-transactions)
20. [Dependency Injection](#20-dependency-injection)
21. [Caching with Redis](#21-caching-with-redis)
22. [Authentication & Authorization](#22-authentication--authorization)
23. [Testing](#23-testing)
24. [Configuration Management](#24-configuration-management)
25. [Logging & Observability](#25-logging-with-slog)
26. [Graceful Shutdown & Production Patterns](#26-graceful-shutdown)
27. [Project Structure — Putting It All Together](#27-project-structure)

**Appendices**

- [Appendix A: Generics (Go 1.18+)](#appendix-a-generics-go-118)
- [Appendix B: Chi Middleware Reference](#appendix-b-chi-middleware-reference)
- [Appendix C: Go vs Laravel Quick Reference](#appendix-c-go-vs-laravel-quick-reference)
- [Appendix D: File I/O](#appendix-d-file-io)
- [Appendix E: Regular Expressions](#appendix-e-regular-expressions)
- [Appendix F: Embedded Files — go:embed](#appendix-f-embedded-files--goembed)
- [Appendix G: CLI with the flag Package](#appendix-g-cli-with-the-flag-package)
- [Appendix H: Functional Options Pattern](#appendix-h-functional-options-pattern)
- [Appendix I: Sending Email](#appendix-i-sending-email)

---

## 1. Getting Started

### Installing Go

```bash
# macOS
brew install go

# Ubuntu/Debian
sudo apt update && sudo apt install golang-go

# Or download from https://go.dev/dl/
wget https://go.dev/dl/go1.23.0.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.23.0.linux-amd64.tar.gz
```

Add to your shell profile (`~/.bashrc`, `~/.zshrc`):

```bash
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

Verify:

```bash
go version
# go version go1.23.0 linux/amd64
```

### Your First Go Program

```bash
mkdir hello && cd hello
go mod init github.com/yourusername/hello
```

Create `main.go`:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

Run it:

```bash
go run main.go       # Compile and run
go build -o hello .  # Compile to binary
./hello
```

### Understanding go mod

Every Go project starts with `go mod init`. This creates a `go.mod` file that tracks your module name and dependencies — similar to `composer.json` in Laravel.

```bash
go mod init github.com/brainy/myproject
go mod tidy     # Download and clean up dependencies
go mod vendor   # Vendor dependencies (like composer install)
```

### Essential Go Commands

```bash
go run main.go      # Compile and run
go build .          # Compile to binary
go test ./...       # Run all tests
go fmt ./...        # Format all code (Go enforces a standard style)
go vet ./...        # Static analysis for bugs
go mod tidy         # Clean up dependencies
go get pkg@version  # Add a dependency
```

---

## 2. Go Fundamentals

### Variables & Types

```go
package main

import "fmt"

func main() {
    // Explicit type declaration
    var name string = "Brainy"
    var age int = 25
    var balance float64 = 1500.50
    var isActive bool = true

    // Type inference (shorthand — most common)
    city := "Lagos"
    count := 42
    rate := 3.14
    done := false

    // Multiple declarations
    var (
        firstName string = "John"
        lastName  string = "Doe"
        score     int    = 100
    )

    // Constants
    const Pi = 3.14159
    const AppName = "FraudGuard"

    // Zero values — Go initializes to zero value
    var s string   // "" (empty string)
    var i int      // 0
    var f float64  // 0.0
    var b bool     // false

    fmt.Println(name, age, balance, isActive)
    fmt.Println(city, count, rate, done)
    fmt.Println(firstName, lastName, score)
    fmt.Println(Pi, AppName)
    fmt.Println(s, i, f, b)
}
```

### Core Types

```go
// Integers
var i8  int8    // -128 to 127
var i16 int16   // -32768 to 32767
var i32 int32   // -2 billion to 2 billion
var i64 int64   // huge range
var i   int     // platform-dependent (32 or 64 bit)

// Unsigned integers
var u8  uint8   // 0 to 255 (also called byte)
var u16 uint16
var u32 uint32
var u64 uint64

// Floating point
var f32 float32
var f64 float64 // Default for decimals

// String — UTF-8 encoded, immutable
var s string

// Boolean
var b bool

// Byte and Rune
var by byte     // alias for uint8
var r  rune     // alias for int32, represents a Unicode code point
```

### Type Conversion

Go has **no implicit type conversion**. You must convert explicitly:

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

// String conversions
import "strconv"

s := strconv.Itoa(42)                       // int → string: "42"
i, err := strconv.Atoi("42")                // string → int: 42
f, err := strconv.ParseFloat("3.14", 64)    // string → float64
s := fmt.Sprintf("%d", 42)                  // int → string: "42"
```

### String Operations

```go
import (
    "fmt"
    "strings"
)

func main() {
    s := "Hello, World!"

    fmt.Println(len(s))                        // 13 (byte length)
    fmt.Println(strings.ToUpper(s))            // HELLO, WORLD!
    fmt.Println(strings.ToLower(s))            // hello, world!
    fmt.Println(strings.Contains(s, "World"))  // true
    fmt.Println(strings.HasPrefix(s, "Hello")) // true
    fmt.Println(strings.HasSuffix(s, "!"))     // true
    fmt.Println(strings.Replace(s, "World", "Go", 1))
    fmt.Println(strings.Split("a,b,c", ","))   // [a b c]
    fmt.Println(strings.TrimSpace("  hi  "))   // "hi"
    fmt.Println(strings.Index(s, "World"))      // 7

    // String formatting (like sprintf in PHP)
    name := "Brainy"
    age := 25
    msg := fmt.Sprintf("Name: %s, Age: %d", name, age)

    // Format verbs:
    // %s  — string       %d  — integer
    // %f  — float        %.2f — 2 decimal places
    // %v  — any value    %+v — struct with field names
    // %T  — type         %t  — boolean
    // %p  — pointer      %q  — quoted string
}
```

### String Builder (efficient concatenation)

```go
// BAD — creates many allocations
result := ""
for i := 0; i < 1000; i++ {
    result += "hello "
}

// GOOD — efficient
var sb strings.Builder
for i := 0; i < 1000; i++ {
    sb.WriteString("hello ")
}
result := sb.String()
```

---

## 3. Control Flow & Functions

### If/Else

```go
age := 20

if age >= 18 {
    fmt.Println("Adult")
} else if age >= 13 {
    fmt.Println("Teenager")
} else {
    fmt.Println("Child")
}

// If with initialization statement (very common in Go)
if err := doSomething(); err != nil {
    fmt.Println("Error:", err)
}
// err is only available inside this if block
```

### Switch

```go
// Basic switch (no break needed — Go doesn't fall through by default)
day := "Monday"
switch day {
case "Monday":
    fmt.Println("Start of work week")
case "Friday":
    fmt.Println("TGIF!")
case "Saturday", "Sunday":
    fmt.Println("Weekend!")
default:
    fmt.Println("Midweek")
}

// Expressionless switch (like if-else chain)
score := 85
switch {
case score >= 90:
    fmt.Println("A")
case score >= 80:
    fmt.Println("B")
case score >= 70:
    fmt.Println("C")
default:
    fmt.Println("F")
}

// Type switch (very useful with interfaces)
func checkType(i interface{}) {
    switch v := i.(type) {
    case int:
        fmt.Printf("Integer: %d\n", v)
    case string:
        fmt.Printf("String: %s\n", v)
    case bool:
        fmt.Printf("Boolean: %t\n", v)
    default:
        fmt.Printf("Unknown type: %T\n", v)
    }
}
```

### For Loops (Go's only loop construct)

```go
// Classic for loop
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// While-style loop
count := 0
for count < 10 {
    count++
}

// Infinite loop
for {
    fmt.Println("Running forever...")
    break
}

// Range over slice
fruits := []string{"apple", "banana", "cherry"}
for index, value := range fruits {
    fmt.Printf("%d: %s\n", index, value)
}

// Ignore index with _
for _, fruit := range fruits {
    fmt.Println(fruit)
}

// Range over map
ages := map[string]int{"Alice": 30, "Bob": 25}
for key, value := range ages {
    fmt.Printf("%s is %d\n", key, value)
}

// Range over string (iterates runes, not bytes)
for i, ch := range "Hello" {
    fmt.Printf("%d: %c\n", i, ch)
}
```

### Functions

```go
// Basic function
func greet(name string) string {
    return "Hello, " + name
}

// Multiple parameters of same type
func add(a, b int) int {
    return a + b
}

// Multiple return values (very common in Go)
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("cannot divide by zero")
    }
    return a / b, nil
}

result, err := divide(10, 3)
if err != nil {
    log.Fatal(err)
}

// Named return values
func calculateArea(width, height float64) (area float64, perimeter float64) {
    area = width * height
    perimeter = 2 * (width + height)
    return // naked return
}

// Variadic functions (like ...$args in PHP)
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}
sum(1, 2, 3, 4, 5) // 15

// Functions as values (first-class functions)
multiply := func(a, b int) int {
    return a * b
}
fmt.Println(multiply(3, 4)) // 12

// Closures
func counter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}
c := counter()
fmt.Println(c()) // 1
fmt.Println(c()) // 2
fmt.Println(c()) // 3
```

### Defer, Panic, and Recover

```go
// defer — executes when the surrounding function returns
func readFile(path string) {
    f, err := os.Open(path)
    if err != nil {
        log.Fatal(err)
    }
    defer f.Close() // Guaranteed to run when function returns
    // Read file...
}

// Multiple defers execute in LIFO order
func example() {
    defer fmt.Println("1")
    defer fmt.Println("2")
    defer fmt.Println("3")
    // Output: 3, 2, 1
}

// panic — like throwing an unrecoverable exception
func mustParse(s string) int {
    i, err := strconv.Atoi(s)
    if err != nil {
        panic(fmt.Sprintf("failed to parse %q: %v", s, err))
    }
    return i
}

// recover — catch a panic
func safeExecute() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered from panic:", r)
        }
    }()
    panic("something went wrong!")
}
```

---

## 4. Data Structures

### Arrays (fixed size — rarely used directly)

```go
var nums [5]int
nums[0] = 10

names := [3]string{"Alice", "Bob", "Charlie"}
auto := [...]int{1, 2, 3, 4} // Compiler counts: [4]int
```

### Slices (dynamic — what you use 99% of the time)

```go
// Create a slice
fruits := []string{"apple", "banana", "cherry"}
nums := make([]int, 5)       // [0, 0, 0, 0, 0] — length 5
nums2 := make([]int, 0, 10)  // length 0, capacity 10

// Append
fruits = append(fruits, "date")
fruits = append(fruits, "elderberry", "fig")

// Slicing
sub := fruits[1:3]   // ["banana", "cherry"]
first := fruits[:2]  // ["apple", "banana"]
last := fruits[2:]   // ["cherry", ...]

// Length and Capacity
fmt.Println(len(fruits))
fmt.Println(cap(fruits))

// Copy (slices share underlying memory!)
src := []int{1, 2, 3}
dst := make([]int, len(src))
copy(dst, src)

// Remove element at index i
i := 1
fruits = append(fruits[:i], fruits[i+1:]...)

// Contains (Go 1.21+)
import "slices"
found := slices.Contains(fruits, "apple")

// Sort
import "sort"
nums := []int{3, 1, 4, 1, 5}
sort.Ints(nums)

sort.Slice(fruits, func(i, j int) bool {
    return fruits[i] < fruits[j]
})
```

### Maps (like PHP associative arrays)

```go
// Create
ages := map[string]int{
    "Alice": 30,
    "Bob":   25,
}

scores := make(map[string]int)

// Set / Get
scores["math"] = 95
age := ages["Alice"]          // 30
missing := ages["Charlie"]    // 0 (zero value)

// Check if key exists ("comma ok" idiom)
age, ok := ages["Charlie"]
if !ok {
    fmt.Println("Charlie not found")
}

// Delete
delete(ages, "Bob")

// Iterate
for key, value := range ages {
    fmt.Printf("%s: %d\n", key, value)
}

// Length
fmt.Println(len(ages))

// NOTE: Maps are NOT safe for concurrent use
// Use sync.Map or a mutex for concurrent access
```

---

## 5. Structs, Methods & Interfaces

### Structs

```go
type User struct {
    ID        int
    FirstName string
    LastName  string
    Email     string
    Age       int
    IsActive  bool
    CreatedAt time.Time
}

// Create instances
user1 := User{
    ID:        1,
    FirstName: "Brainy",
    LastName:  "Dev",
    Email:     "brainy@example.com",
    Age:       25,
    IsActive:  true,
    CreatedAt: time.Now(),
}

// Partial initialization (others get zero values)
user2 := User{
    FirstName: "Alice",
    Email:     "alice@example.com",
}

// Access fields
fmt.Println(user1.FirstName)
user1.Age = 26

// Anonymous struct (one-off use)
config := struct {
    Host string
    Port int
}{
    Host: "localhost",
    Port: 8080,
}
```

### Struct Embedding (composition, not inheritance)

```go
type Address struct {
    Street string
    City   string
    State  string
}

type Employee struct {
    User              // Embedded
    Address           // Embedded
    Department string
    Salary     float64
}

emp := Employee{
    User:       User{FirstName: "Brainy", Email: "brainy@co.com"},
    Address:    Address{City: "Lagos", State: "Lagos"},
    Department: "Engineering",
    Salary:     150000,
}

// Access embedded fields directly (promoted)
fmt.Println(emp.FirstName)  // From User
fmt.Println(emp.City)       // From Address
```

### Methods

```go
// Value receiver — works on a COPY
func (u User) FullName() string {
    return u.FirstName + " " + u.LastName
}

// Pointer receiver — works on the ORIGINAL (can modify)
func (u *User) Deactivate() {
    u.IsActive = false
}

// When to use pointer receivers:
// 1. Method modifies the receiver
// 2. Struct is large (avoids copying)
// 3. Consistency — if one needs pointer, use pointer for all

user := User{FirstName: "Brainy", LastName: "Dev", IsActive: true}
fmt.Println(user.FullName()) // "Brainy Dev"
user.Deactivate()
fmt.Println(user.IsActive)   // false
```

### Interfaces (Go's most powerful feature)

```go
// Interfaces define BEHAVIOR, not structure
// Any type that implements all methods satisfies the interface
// No "implements" keyword needed

type PaymentProcessor interface {
    Charge(amount float64) error
    Refund(transactionID string) error
}

type StripeProcessor struct {
    APIKey string
}

func (s *StripeProcessor) Charge(amount float64) error {
    fmt.Printf("Charging $%.2f via Stripe\n", amount)
    return nil
}

func (s *StripeProcessor) Refund(transactionID string) error {
    fmt.Printf("Refunding %s via Stripe\n", transactionID)
    return nil
}

type PayPalProcessor struct {
    ClientID string
}

func (p *PayPalProcessor) Charge(amount float64) error {
    fmt.Printf("Charging $%.2f via PayPal\n", amount)
    return nil
}

func (p *PayPalProcessor) Refund(transactionID string) error {
    fmt.Printf("Refunding %s via PayPal\n", transactionID)
    return nil
}

// Function accepts any PaymentProcessor — polymorphism
func processPayment(pp PaymentProcessor, amount float64) error {
    return pp.Charge(amount)
}

stripe := &StripeProcessor{APIKey: "sk_test_123"}
paypal := &PayPalProcessor{ClientID: "client_456"}
processPayment(stripe, 99.99)
processPayment(paypal, 49.99)
```

### Common Interface Patterns

```go
// Empty interface — accepts any type (like mixed in PHP 8)
func printAnything(v any) { // any = interface{}
    fmt.Println(v)
}

// Stringer interface (like __toString in PHP)
func (t Transaction) String() string {
    return fmt.Sprintf("TX[%s]: $%.2f (%s)", t.ID, t.Amount, t.Status)
}

// error interface (Go's built-in)
type error interface {
    Error() string
}

// io.Reader and io.Writer — the most important interfaces in Go
type Reader interface {
    Read(p []byte) (n int, err error)
}
type Writer interface {
    Write(p []byte) (n int, err error)
}

// Interface composition
type ReadWriter interface {
    Reader
    Writer
}
```

---

## 6. Pointers & Memory

```go
func main() {
    x := 42
    p := &x    // p is a pointer to x

    fmt.Println(x)  // 42
    fmt.Println(p)  // 0xc000012080 (memory address)
    fmt.Println(*p) // 42 (dereference)

    *p = 100
    fmt.Println(x)  // 100 — x was changed through the pointer
}

// Go is pass-by-value: functions get COPIES
func doubleValue(n int) {
    n = n * 2 // Modifies the copy only
}

func doublePointer(n *int) {
    *n = *n * 2 // Modifies the original
}

num := 10
doubleValue(num)
fmt.Println(num) // 10 — unchanged

doublePointer(&num)
fmt.Println(num) // 20 — changed

// new() vs make()
p := new(int)             // Returns *int, value is 0
s := make([]int, 5)       // Creates initialized slice
m := make(map[string]int) // Creates initialized map

// Nil pointers
var p *int // nil
if p != nil {
    fmt.Println(*p) // Only dereference non-nil pointers
}
```

---

## 7. Error Handling

Go doesn't have try/catch. Errors are VALUES.

```go
import (
    "errors"
    "fmt"
)

// Basic error handling
func findUser(id int) (*User, error) {
    if id <= 0 {
        return nil, fmt.Errorf("invalid user ID: %d", id)
    }
    return &User{ID: id, FirstName: "Brainy"}, nil
}

user, err := findUser(0)
if err != nil {
    log.Fatal(err)
}

// Custom error types
type NotFoundError struct {
    Resource string
    ID       int
}

func (e *NotFoundError) Error() string {
    return fmt.Sprintf("%s with ID %d not found", e.Resource, e.ID)
}

// Error wrapping (Go 1.13+)
func getAmount(id int) (float64, error) {
    tx, err := findTransaction(id)
    if err != nil {
        return 0, fmt.Errorf("getAmount: %w", err) // %w wraps the error
    }
    return tx.Amount, nil
}

// Unwrapping and checking error types
var notFoundErr *NotFoundError
if errors.As(err, &notFoundErr) {
    fmt.Printf("Resource: %s, ID: %d\n", notFoundErr.Resource, notFoundErr.ID)
}

if errors.Is(err, ErrNotFound) {
    fmt.Println("Not found!")
}

// Sentinel errors — predefined error values
var (
    ErrNotFound     = errors.New("not found")
    ErrUnauthorized = errors.New("unauthorized")
    ErrForbidden    = errors.New("forbidden")
    ErrValidation   = errors.New("validation error")
)
```

---

## 8. Packages & Modules

### Package Structure

```
myproject/
├── go.mod
├── go.sum
├── main.go               // package main
├── internal/              // Private to this module
│   ├── handler/
│   │   └── user.go        // package handler
│   ├── service/
│   │   └── user.go        // package service
│   └── repository/
│       └── user.go        // package repository
├── pkg/                   // Public, importable by others
│   └── validator/
│       └── validator.go
└── cmd/                   // Entry points
    └── server/
        └── main.go
```

### Visibility Rules

```go
// internal/service/user.go
package service

// Exported (public) — starts with UPPERCASE
type UserService struct{}
func NewUserService() *UserService { return &UserService{} }
func (s *UserService) GetUser(id int) string { return "user" }

// unexported (private) — starts with lowercase
func (s *UserService) validateID(id int) bool { return id > 0 }
```

### init() Functions

```go
package config

var DatabaseURL string

// init() runs automatically when package is imported — before main()
func init() {
    DatabaseURL = os.Getenv("DATABASE_URL")
    if DatabaseURL == "" {
        DatabaseURL = "postgres://localhost:5432/mydb"
    }
}
```

---

## 9. Concurrency — Goroutines & Channels

This is Go's superpower.

### Goroutines (lightweight threads)

```go
// A goroutine costs ~2KB of memory (vs ~1MB for OS threads)

func fetchData(source string) {
    time.Sleep(2 * time.Second)
    fmt.Printf("Fetched from %s\n", source)
}

func main() {
    // Sequential — 6 seconds
    fetchData("API 1")
    fetchData("API 2")
    fetchData("API 3")

    // Concurrent — ~2 seconds
    go fetchData("API 1")
    go fetchData("API 2")
    go fetchData("API 3")

    // Problem: main() exits before goroutines finish!
    time.Sleep(3 * time.Second) // Bad solution
}
```

### WaitGroup (proper way to wait)

```go
func main() {
    var wg sync.WaitGroup

    sources := []string{"API 1", "API 2", "API 3"}

    for _, src := range sources {
        wg.Add(1)
        go func(source string) {
            defer wg.Done()
            fetchData(source)
        }(src) // Pass src as argument to avoid closure issues
    }

    wg.Wait()
    fmt.Println("All fetches complete!")
}
```

### Channels (communication between goroutines)

```go
// Channels are typed, thread-safe queues
ch := make(chan string)        // Unbuffered
bch := make(chan int, 10)      // Buffered (capacity 10)

// Send and receive
go func() {
    ch <- "hello" // Send
}()
msg := <-ch       // Receive
fmt.Println(msg)  // "hello"

// Practical example: fetch prices concurrently
func fetchPrice(exchange string, ch chan<- float64) {
    // chan<- means send-only
    prices := map[string]float64{
        "binance": 45000.00, "coinbase": 45100.00, "kraken": 44900.00,
    }
    time.Sleep(time.Duration(rand.Intn(3)) * time.Second)
    ch <- prices[exchange]
}

func main() {
    ch := make(chan float64, 3)
    exchanges := []string{"binance", "coinbase", "kraken"}

    for _, ex := range exchanges {
        go fetchPrice(ex, ch)
    }

    var total float64
    for i := 0; i < len(exchanges); i++ {
        total += <-ch
    }
    fmt.Printf("Average: $%.2f\n", total/float64(len(exchanges)))
}
```

### Select (multiplexing channels)

```go
func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go func() {
        time.Sleep(1 * time.Second)
        ch1 <- "result from service 1"
    }()
    go func() {
        time.Sleep(2 * time.Second)
        ch2 <- "result from service 2"
    }()

    select {
    case msg := <-ch1:
        fmt.Println(msg)
    case msg := <-ch2:
        fmt.Println(msg)
    case <-time.After(3 * time.Second):
        fmt.Println("Timeout!")
    }
}
```

### Mutex (protecting shared data)

```go
type SafeCounter struct {
    mu    sync.RWMutex
    count map[string]int
}

func NewSafeCounter() *SafeCounter {
    return &SafeCounter{count: make(map[string]int)}
}

func (c *SafeCounter) Increment(key string) {
    c.mu.Lock()
    defer c.mu.Unlock()
    c.count[key]++
}

func (c *SafeCounter) Get(key string) int {
    c.mu.RLock()         // Multiple readers allowed
    defer c.mu.RUnlock()
    return c.count[key]
}
```

### Worker Pool Pattern

```go
func workerPool() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)

    // Start 5 workers
    for w := 0; w < 5; w++ {
        go func(id int) {
            for job := range jobs {
                fmt.Printf("Worker %d processing job %d\n", id, job)
                time.Sleep(time.Second)
                results <- job * 2
            }
        }(w)
    }

    // Send 20 jobs
    for j := 0; j < 20; j++ {
        jobs <- j
    }
    close(jobs)

    // Collect results
    for r := 0; r < 20; r++ {
        fmt.Println(<-results)
    }
}
```

### errgroup (goroutines with error handling)

```go
import "golang.org/x/sync/errgroup"

func fetchAllData(ctx context.Context) error {
    g, ctx := errgroup.WithContext(ctx)

    g.Go(func() error { return fetchUsers(ctx) })
    g.Go(func() error { return fetchOrders(ctx) })
    g.Go(func() error { return fetchProducts(ctx) })

    // If one fails, ctx is cancelled — signaling others to stop
    return g.Wait()
}
```

### sync.Once (run exactly once — singleton initialization)

```go
import (
    "context"
    "fmt"
    "sync"

    "github.com/jackc/pgx/v5/pgxpool" // go get github.com/jackc/pgx/v5
)

// sync.Once guarantees a function runs exactly once, even across goroutines.
// Perfect for lazy initialization of shared resources.

type Database struct {
    pool *pgxpool.Pool
}

var (
    dbInstance *Database
    dbOnce     sync.Once
)

func GetDB(connStr string) *Database {
    dbOnce.Do(func() {
        pool, err := pgxpool.New(context.Background(), connStr)
        if err != nil {
            panic(fmt.Sprintf("failed to connect to db: %v", err))
        }
        dbInstance = &Database{pool: pool}
    })
    return dbInstance
}

// sync.Map — concurrent-safe map (use when reads far outnumber writes)
var cache sync.Map

func setCache(key string, value interface{}) {
    cache.Store(key, value)
}

func getCache(key string) (interface{}, bool) {
    return cache.Load(key)
}

func deleteCache(key string) {
    cache.Delete(key)
}

// Iterate
cache.Range(func(key, value interface{}) bool {
    fmt.Printf("%v: %v\n", key, value)
    return true // return false to stop iteration
})
```

---

## 10. Context Package

Context carries deadlines, cancellation signals, and request-scoped values. **Every handler and DB call should accept a context.**

```go
import "context"

// Creating contexts
ctx := context.Background()    // Root context
ctx := context.TODO()          // Placeholder

// With timeout
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel() // ALWAYS defer cancel

// With cancellation
ctx, cancel := context.WithCancel(context.Background())

// With values (use sparingly)
type contextKey string
const userIDKey contextKey = "userID"
ctx = context.WithValue(ctx, userIDKey, 42)
userID := ctx.Value(userIDKey).(int)

// Using in functions
func fetchUser(ctx context.Context, id int) (*User, error) {
    select {
    case <-ctx.Done():
        return nil, ctx.Err() // context.DeadlineExceeded or context.Canceled
    default:
    }

    row := db.QueryRowContext(ctx, "SELECT * FROM users WHERE id = $1", id)
    // ...
}

// In HTTP handlers
func handleRequest(w http.ResponseWriter, r *http.Request) {
    ctx, cancel := context.WithTimeout(r.Context(), 3*time.Second)
    defer cancel()

    result, err := slowQuery(ctx)
    if errors.Is(err, context.DeadlineExceeded) {
        http.Error(w, "Request timed out", http.StatusGatewayTimeout)
        return
    }
    json.NewEncoder(w).Encode(result)
}
```

---

## 11. Working with JSON

```go
import "encoding/json"

// Struct tags control JSON marshaling
type Transaction struct {
    ID        string    `json:"id"`
    Amount    float64   `json:"amount"`
    Currency  string    `json:"currency"`
    Status    string    `json:"status"`
    CreatedAt time.Time `json:"created_at"`
    DeletedAt time.Time `json:"deleted_at,omitempty"` // Omit if zero
    Internal  string    `json:"-"`                     // Never in JSON
}

// Struct → JSON (Marshal)
tx := Transaction{ID: "tx_001", Amount: 150.00, Currency: "USD"}
jsonBytes, err := json.Marshal(tx)
jsonBytes, _ = json.MarshalIndent(tx, "", "  ") // Pretty

// JSON → Struct (Unmarshal)
jsonStr := `{"id":"tx_002","amount":200,"currency":"EUR"}`
var tx2 Transaction
err = json.Unmarshal([]byte(jsonStr), &tx2)

// Decode from io.Reader (HTTP body — more efficient)
func handleCreate(w http.ResponseWriter, r *http.Request) {
    var tx Transaction
    decoder := json.NewDecoder(r.Body)
    decoder.DisallowUnknownFields()
    if err := decoder.Decode(&tx); err != nil {
        http.Error(w, "Invalid JSON", http.StatusBadRequest)
        return
    }
}

// Encode to io.Writer (HTTP response)
func respondJSON(w http.ResponseWriter, status int, data interface{}) {
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(status)
    json.NewEncoder(w).Encode(data)
}

// Dynamic JSON
var result map[string]interface{}
json.Unmarshal([]byte(jsonStr), &result)

// Raw JSON (delay parsing)
type Event struct {
    Type    string          `json:"type"`
    Payload json.RawMessage `json:"payload"`
}
```

---

## 12. HTTP Fundamentals — net/http

Before using Chi, understand Go's built-in HTTP capabilities.

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "time"
    "io"
    "bytes"
)

// Basic HTTP server
func main() {
    http.HandleFunc("/", homeHandler)
    http.HandleFunc("/health", healthHandler)
    http.HandleFunc("/api/users", usersHandler)

    fmt.Println("Server starting on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}

func homeHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Welcome to the API")
}

func healthHandler(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    json.NewEncoder(w).Encode(map[string]string{"status": "ok"})
}

func usersHandler(w http.ResponseWriter, r *http.Request) {
    switch r.Method {
    case http.MethodGet:
        json.NewEncoder(w).Encode([]string{"Alice", "Bob"})
    case http.MethodPost:
        w.WriteHeader(http.StatusCreated)
        fmt.Fprintln(w, "User created")
    default:
        http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
    }
}

// HTTP Client (making requests)
func fetchExternalAPI() {
    client := &http.Client{Timeout: 10 * time.Second}

    resp, err := client.Get("https://api.example.com/data")
    if err != nil {
        log.Fatal(err)
    }
    defer resp.Body.Close()

    body, _ := io.ReadAll(resp.Body)
    fmt.Println(string(body))

    // POST request
    payload := map[string]string{"name": "Brainy"}
    jsonData, _ := json.Marshal(payload)

    resp, err = client.Post(
        "https://api.example.com/users",
        "application/json",
        bytes.NewBuffer(jsonData),
    )
    if err != nil {
        log.Fatal(err)
    }
    defer resp.Body.Close()
}
```

---

## 13. Introduction to Chi Router

Chi is a lightweight, idiomatic HTTP router for Go. Think of it as Go's equivalent to Laravel's routing — but much simpler.

### Why Chi?

- 100% compatible with `net/http` (uses standard `http.Handler`)
- Middleware support (like Laravel middleware)
- URL parameters, grouping, sub-routers
- No external dependencies
- Fast and battle-tested in production

### Installation

```bash
go get github.com/go-chi/chi/v5
go get github.com/go-chi/chi/v5/middleware
```

### Basic Chi Setup

```go
package main

import (
    "encoding/json"
    "log"
    "net/http"

    "github.com/go-chi/chi/v5"
    "github.com/go-chi/chi/v5/middleware"
)

func main() {
    r := chi.NewRouter()

    // Built-in middleware
    r.Use(middleware.Logger)
    r.Use(middleware.Recoverer)
    r.Use(middleware.RequestID)
    r.Use(middleware.RealIP)

    r.Get("/", func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("Hello, Chi!"))
    })

    r.Get("/health", func(w http.ResponseWriter, r *http.Request) {
        json.NewEncoder(w).Encode(map[string]string{"status": "healthy"})
    })

    log.Println("Server starting on :3000")
    http.ListenAndServe(":3000", r)
}
```

### URL Parameters

```go
r.Get("/users/{id}", func(w http.ResponseWriter, r *http.Request) {
    userID := chi.URLParam(r, "id")
    w.Write([]byte("User ID: " + userID))
})

r.Get("/users/{userID}/posts/{postID}", func(w http.ResponseWriter, r *http.Request) {
    userID := chi.URLParam(r, "userID")
    postID := chi.URLParam(r, "postID")
    fmt.Fprintf(w, "User %s, Post %s", userID, postID)
})

// Query parameters
r.Get("/search", func(w http.ResponseWriter, r *http.Request) {
    query := r.URL.Query().Get("q")
    page := r.URL.Query().Get("page")
    fmt.Fprintf(w, "Search: %s, Page: %s", query, page)
})
```

### Route Grouping (like Laravel Route::group)

```go
r := chi.NewRouter()

// Public routes
r.Group(func(r chi.Router) {
    r.Get("/", homeHandler)
    r.Post("/login", loginHandler)
    r.Post("/register", registerHandler)
})

// Protected routes
r.Group(func(r chi.Router) {
    r.Use(AuthMiddleware)
    r.Get("/dashboard", dashboardHandler)
    r.Get("/profile", profileHandler)
})

// Resource-style routes with Route()
r.Route("/api/v1", func(r chi.Router) {
    r.Route("/users", func(r chi.Router) {
        r.Get("/", listUsers)
        r.Post("/", createUser)

        r.Route("/{userID}", func(r chi.Router) {
            r.Get("/", getUser)
            r.Put("/", updateUser)
            r.Delete("/", deleteUser)

            r.Route("/transactions", func(r chi.Router) {
                r.Get("/", listUserTransactions)
                r.Post("/", createTransaction)
            })
        })
    })
})
```

### Mount Sub-Routers

```go
func adminRouter() chi.Router {
    r := chi.NewRouter()
    r.Use(AdminOnly)
    r.Get("/", adminDashboard)
    r.Get("/users", adminListUsers)
    r.Delete("/users/{id}", adminDeleteUser)
    return r
}

r := chi.NewRouter()
r.Mount("/admin", adminRouter())
```

---

## 14. Building a REST API with Chi

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "strconv"
    "sync"
    "time"

    "github.com/go-chi/chi/v5"
    "github.com/go-chi/chi/v5/middleware"
)

// --- Models ---

type Transaction struct {
    ID        int       `json:"id"`
    UserID    int       `json:"user_id"`
    Amount    float64   `json:"amount"`
    Currency  string    `json:"currency"`
    Type      string    `json:"type"`
    Status    string    `json:"status"`
    Reference string    `json:"reference"`
    CreatedAt time.Time `json:"created_at"`
}

type CreateTransactionRequest struct {
    UserID   int     `json:"user_id"`
    Amount   float64 `json:"amount"`
    Currency string  `json:"currency"`
    Type     string  `json:"type"`
}

type APIResponse struct {
    Success bool        `json:"success"`
    Message string      `json:"message,omitempty"`
    Data    interface{} `json:"data,omitempty"`
    Error   string      `json:"error,omitempty"`
}

// --- In-Memory Store ---

type TransactionStore struct {
    mu           sync.RWMutex
    transactions map[int]*Transaction
    nextID       int
}

func NewTransactionStore() *TransactionStore {
    return &TransactionStore{
        transactions: make(map[int]*Transaction),
        nextID:       1,
    }
}

func (s *TransactionStore) Create(tx *Transaction) *Transaction {
    s.mu.Lock()
    defer s.mu.Unlock()
    tx.ID = s.nextID
    tx.CreatedAt = time.Now()
    tx.Status = "pending"
    tx.Reference = fmt.Sprintf("TXN-%d-%d", tx.UserID, s.nextID)
    s.transactions[tx.ID] = tx
    s.nextID++
    return tx
}

func (s *TransactionStore) GetByID(id int) (*Transaction, bool) {
    s.mu.RLock()
    defer s.mu.RUnlock()
    tx, ok := s.transactions[id]
    return tx, ok
}

func (s *TransactionStore) List() []*Transaction {
    s.mu.RLock()
    defer s.mu.RUnlock()
    txs := make([]*Transaction, 0, len(s.transactions))
    for _, tx := range s.transactions {
        txs = append(txs, tx)
    }
    return txs
}

// --- Handlers ---

type TransactionHandler struct {
    store *TransactionStore
}

func NewTransactionHandler(store *TransactionStore) *TransactionHandler {
    return &TransactionHandler{store: store}
}

func (h *TransactionHandler) List(w http.ResponseWriter, r *http.Request) {
    respondJSON(w, http.StatusOK, APIResponse{Success: true, Data: h.store.List()})
}

func (h *TransactionHandler) GetByID(w http.ResponseWriter, r *http.Request) {
    id, err := strconv.Atoi(chi.URLParam(r, "id"))
    if err != nil {
        respondJSON(w, http.StatusBadRequest, APIResponse{Success: false, Error: "Invalid ID"})
        return
    }
    tx, ok := h.store.GetByID(id)
    if !ok {
        respondJSON(w, http.StatusNotFound, APIResponse{Success: false, Error: "Not found"})
        return
    }
    respondJSON(w, http.StatusOK, APIResponse{Success: true, Data: tx})
}

func (h *TransactionHandler) Create(w http.ResponseWriter, r *http.Request) {
    var req CreateTransactionRequest
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        respondJSON(w, http.StatusBadRequest, APIResponse{Success: false, Error: "Invalid JSON"})
        return
    }
    tx := h.store.Create(&Transaction{
        UserID: req.UserID, Amount: req.Amount, Currency: req.Currency, Type: req.Type,
    })
    respondJSON(w, http.StatusCreated, APIResponse{Success: true, Data: tx})
}

func respondJSON(w http.ResponseWriter, status int, data interface{}) {
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(status)
    json.NewEncoder(w).Encode(data)
}

func main() {
    store := NewTransactionStore()
    txHandler := NewTransactionHandler(store)

    r := chi.NewRouter()
    r.Use(middleware.RequestID)
    r.Use(middleware.RealIP)
    r.Use(middleware.Logger)
    r.Use(middleware.Recoverer)
    r.Use(middleware.Timeout(30 * time.Second))

    r.Route("/api/v1/transactions", func(r chi.Router) {
        r.Get("/", txHandler.List)
        r.Post("/", txHandler.Create)
        r.Get("/{id}", txHandler.GetByID)
    })

    log.Println("Server starting on :3000")
    http.ListenAndServe(":3000", r)
}
```

---

## 15. Middleware

### Understanding the Pattern

```go
// A middleware takes a handler and returns a handler
func MyMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        // BEFORE the handler
        fmt.Println("Before")
        next.ServeHTTP(w, r) // Call next handler
        // AFTER the handler
        fmt.Println("After")
    })
}
```

### CORS Middleware

```go
func CORSMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("Access-Control-Allow-Origin", "*")
        w.Header().Set("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS")
        w.Header().Set("Access-Control-Allow-Headers", "Content-Type, Authorization")
        w.Header().Set("Access-Control-Max-Age", "86400")

        if r.Method == http.MethodOptions {
            w.WriteHeader(http.StatusNoContent)
            return
        }
        next.ServeHTTP(w, r)
    })
}
```

### Logging Middleware with Response Capture

```go
func LoggingMiddleware(logger *slog.Logger) func(http.Handler) http.Handler {
    return func(next http.Handler) http.Handler {
        return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
            start := time.Now()
            ww := &responseWriter{ResponseWriter: w, statusCode: http.StatusOK}

            next.ServeHTTP(ww, r)

            logger.Info("request completed",
                "method", r.Method,
                "path", r.URL.Path,
                "status", ww.statusCode,
                "duration", time.Since(start),
                "ip", r.RemoteAddr,
            )
        })
    }
}

type responseWriter struct {
    http.ResponseWriter
    statusCode int
}

func (rw *responseWriter) WriteHeader(code int) {
    rw.statusCode = code
    rw.ResponseWriter.WriteHeader(code)
}
```

### Rate Limiting Middleware

```go
import "golang.org/x/time/rate"

func RateLimitMiddleware(rps int) func(http.Handler) http.Handler {
    limiter := rate.NewLimiter(rate.Limit(rps), rps*2)
    return func(next http.Handler) http.Handler {
        return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
            if !limiter.Allow() {
                respondJSON(w, http.StatusTooManyRequests, APIResponse{
                    Success: false, Error: "Rate limit exceeded",
                })
                return
            }
            next.ServeHTTP(w, r)
        })
    }
}

// Per-IP rate limiter
type IPRateLimiter struct {
    mu       sync.RWMutex
    limiters map[string]*rate.Limiter
    rate     rate.Limit
    burst    int
}

func NewIPRateLimiter(rps, burst int) *IPRateLimiter {
    return &IPRateLimiter{
        limiters: make(map[string]*rate.Limiter),
        rate:     rate.Limit(rps),
        burst:    burst,
    }
}

func (l *IPRateLimiter) GetLimiter(ip string) *rate.Limiter {
    l.mu.Lock()
    defer l.mu.Unlock()
    limiter, exists := l.limiters[ip]
    if !exists {
        limiter = rate.NewLimiter(l.rate, l.burst)
        l.limiters[ip] = limiter
    }
    return limiter
}

func (l *IPRateLimiter) Middleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        if !l.GetLimiter(r.RemoteAddr).Allow() {
            respondJSON(w, http.StatusTooManyRequests, APIResponse{
                Success: false, Error: "Too many requests",
            })
            return
        }
        next.ServeHTTP(w, r)
    })
}
```

### Using Middleware in Chi

```go
r := chi.NewRouter()

// Global middleware
r.Use(middleware.Logger)
r.Use(CORSMiddleware)

// Group-level
r.Route("/api", func(r chi.Router) {
    r.Use(RateLimitMiddleware(100))

    r.Post("/login", loginHandler)

    r.Group(func(r chi.Router) {
        r.Use(AuthMiddleware)
        r.Get("/profile", profileHandler)

        r.Group(func(r chi.Router) {
            r.Use(AdminMiddleware) // Stacked: Auth + Admin
            r.Get("/admin/users", adminUsersHandler)
        })
    })
})

// Inline middleware on specific routes
r.With(RequireJSON).Post("/api/data", dataHandler)
```

---

## 16. Request Validation

### Using go-playground/validator

```bash
go get github.com/go-playground/validator/v10
```

```go
import "github.com/go-playground/validator/v10"

var validate = validator.New()

type CreateUserRequest struct {
    FirstName string `json:"first_name" validate:"required,min=2,max=100"`
    LastName  string `json:"last_name" validate:"required,min=2,max=100"`
    Email     string `json:"email" validate:"required,email"`
    Password  string `json:"password" validate:"required,min=8,max=72"`
    Age       int    `json:"age" validate:"required,gte=18,lte=120"`
    Phone     string `json:"phone" validate:"omitempty,e164"`
    Country   string `json:"country" validate:"required,iso3166_1_alpha2"`
}

type CreateTransactionRequest struct {
    UserID   int     `json:"user_id" validate:"required,gt=0"`
    Amount   float64 `json:"amount" validate:"required,gt=0"`
    Currency string  `json:"currency" validate:"required,oneof=USD EUR NGN GBP"`
    Type     string  `json:"type" validate:"required,oneof=credit debit"`
}

type ValidationError struct {
    Field   string `json:"field"`
    Message string `json:"message"`
}

func validateRequest(req interface{}) []ValidationError {
    err := validate.Struct(req)
    if err == nil {
        return nil
    }
    var errs []ValidationError
    for _, e := range err.(validator.ValidationErrors) {
        errs = append(errs, ValidationError{
            Field:   e.Field(),
            Message: formatValidationMessage(e),
        })
    }
    return errs
}

func formatValidationMessage(e validator.FieldError) string {
    switch e.Tag() {
    case "required":
        return fmt.Sprintf("%s is required", e.Field())
    case "email":
        return "Must be a valid email address"
    case "min":
        return fmt.Sprintf("Must be at least %s characters", e.Param())
    case "max":
        return fmt.Sprintf("Must be at most %s characters", e.Param())
    case "gte":
        return fmt.Sprintf("Must be at least %s", e.Param())
    case "lte":
        return fmt.Sprintf("Must be at most %s", e.Param())
    case "oneof":
        return fmt.Sprintf("Must be one of: %s", e.Param())
    case "gt":
        return fmt.Sprintf("Must be greater than %s", e.Param())
    default:
        return fmt.Sprintf("Failed validation: %s", e.Tag())
    }
}

// Generic decode + validate helper (Go 1.18+ generics)
func DecodeAndValidate[T any](r *http.Request) (*T, []ValidationError, error) {
    var req T
    decoder := json.NewDecoder(r.Body)
    decoder.DisallowUnknownFields()
    if err := decoder.Decode(&req); err != nil {
        return nil, nil, fmt.Errorf("invalid JSON: %w", err)
    }
    if errs := validateRequest(&req); len(errs) > 0 {
        return nil, errs, nil
    }
    return &req, nil, nil
}

// Usage:
func createUser(w http.ResponseWriter, r *http.Request) {
    req, validationErrs, err := DecodeAndValidate[CreateUserRequest](r)
    if err != nil {
        respondJSON(w, http.StatusBadRequest, APIResponse{Success: false, Error: err.Error()})
        return
    }
    if validationErrs != nil {
        respondJSON(w, http.StatusUnprocessableEntity, APIResponse{
            Success: false, Error: "Validation failed", Data: validationErrs,
        })
        return
    }
    // req is validated and ready
}
```

### Common Validation Tags

```
required       — Must be present and non-zero
omitempty      — Skip if zero value
email          — Valid email
url            — Valid URL
min=N / max=N  — Length or value bounds
gt=N / gte=N   — Greater than / greater or equal
lt=N / lte=N   — Less than / less or equal
oneof=A B C    — Must be one of listed values
alpha          — Letters only
alphanum       — Letters and numbers only
uuid           — Valid UUID
e164           — E.164 phone number
ip             — Valid IP address
```

---

## 17. Database — SQL with pgx

```bash
go get github.com/jackc/pgx/v5
go get github.com/jackc/pgx/v5/pgxpool
```

### Connection Pool

```go
package database

import (
    "context"
    "fmt"
    "time"
    "github.com/jackc/pgx/v5/pgxpool"
)

func NewPostgresPool(ctx context.Context, databaseURL string) (*pgxpool.Pool, error) {
    config, err := pgxpool.ParseConfig(databaseURL)
    if err != nil {
        return nil, fmt.Errorf("parse config: %w", err)
    }

    config.MaxConns = 25
    config.MinConns = 5
    config.MaxConnLifetime = time.Hour
    config.MaxConnIdleTime = 30 * time.Minute

    pool, err := pgxpool.NewWithConfig(ctx, config)
    if err != nil {
        return nil, fmt.Errorf("create pool: %w", err)
    }

    if err := pool.Ping(ctx); err != nil {
        return nil, fmt.Errorf("ping: %w", err)
    }
    return pool, nil
}
```

### Repository Pattern

```go
package repository

import (
    "context"
    "fmt"
    "time"
    "github.com/jackc/pgx/v5"
    "github.com/jackc/pgx/v5/pgxpool"
)

type User struct {
    ID        int       `json:"id"`
    FirstName string    `json:"first_name"`
    LastName  string    `json:"last_name"`
    Email     string    `json:"email"`
    Password  string    `json:"-"`
    IsActive  bool      `json:"is_active"`
    CreatedAt time.Time `json:"created_at"`
    UpdatedAt time.Time `json:"updated_at"`
}

type UserRepository struct {
    pool *pgxpool.Pool
}

func NewUserRepository(pool *pgxpool.Pool) *UserRepository {
    return &UserRepository{pool: pool}
}

func (r *UserRepository) Create(ctx context.Context, user *User) (*User, error) {
    query := `
        INSERT INTO users (first_name, last_name, email, password_hash, is_active)
        VALUES ($1, $2, $3, $4, true)
        RETURNING id, created_at, updated_at`

    err := r.pool.QueryRow(ctx, query,
        user.FirstName, user.LastName, user.Email, user.Password,
    ).Scan(&user.ID, &user.CreatedAt, &user.UpdatedAt)

    if err != nil {
        return nil, fmt.Errorf("create user: %w", err)
    }
    return user, nil
}

func (r *UserRepository) FindByID(ctx context.Context, id int) (*User, error) {
    query := `SELECT id, first_name, last_name, email, password_hash, is_active, created_at, updated_at
              FROM users WHERE id = $1`
    var user User
    err := r.pool.QueryRow(ctx, query, id).Scan(
        &user.ID, &user.FirstName, &user.LastName, &user.Email,
        &user.Password, &user.IsActive, &user.CreatedAt, &user.UpdatedAt,
    )
    if err != nil {
        if err == pgx.ErrNoRows {
            return nil, nil
        }
        return nil, fmt.Errorf("find user: %w", err)
    }
    return &user, nil
}

func (r *UserRepository) FindByEmail(ctx context.Context, email string) (*User, error) {
    query := `SELECT id, first_name, last_name, email, password_hash, is_active, created_at, updated_at
              FROM users WHERE email = $1`
    var user User
    err := r.pool.QueryRow(ctx, query, email).Scan(
        &user.ID, &user.FirstName, &user.LastName, &user.Email,
        &user.Password, &user.IsActive, &user.CreatedAt, &user.UpdatedAt,
    )
    if err != nil {
        if err == pgx.ErrNoRows {
            return nil, nil
        }
        return nil, fmt.Errorf("find user by email: %w", err)
    }
    return &user, nil
}

func (r *UserRepository) List(ctx context.Context, limit, offset int) ([]*User, int, error) {
    var total int
    r.pool.QueryRow(ctx, "SELECT COUNT(*) FROM users WHERE is_active = true").Scan(&total)

    query := `SELECT id, first_name, last_name, email, password_hash, is_active, created_at, updated_at
              FROM users WHERE is_active = true ORDER BY created_at DESC LIMIT $1 OFFSET $2`

    rows, err := r.pool.Query(ctx, query, limit, offset)
    if err != nil {
        return nil, 0, fmt.Errorf("list users: %w", err)
    }
    defer rows.Close()

    var users []*User
    for rows.Next() {
        var u User
        if err := rows.Scan(&u.ID, &u.FirstName, &u.LastName, &u.Email,
            &u.Password, &u.IsActive, &u.CreatedAt, &u.UpdatedAt); err != nil {
            return nil, 0, err
        }
        users = append(users, &u)
    }
    return users, total, nil
}

func (r *UserRepository) Update(ctx context.Context, user *User) error {
    query := `UPDATE users SET first_name=$1, last_name=$2, email=$3, updated_at=NOW() WHERE id=$4`
    result, err := r.pool.Exec(ctx, query, user.FirstName, user.LastName, user.Email, user.ID)
    if err != nil {
        return fmt.Errorf("update user: %w", err)
    }
    if result.RowsAffected() == 0 {
        return fmt.Errorf("user not found")
    }
    return nil
}

func (r *UserRepository) Delete(ctx context.Context, id int) error {
    _, err := r.pool.Exec(ctx, "UPDATE users SET is_active=false, updated_at=NOW() WHERE id=$1", id)
    return err
}
```

---

## 18. Database Migrations with Goose

```bash
go install github.com/pressly/goose/v3/cmd/goose@latest
```

### Creating Migrations

```bash
goose -dir migrations create create_users_table sql
goose -dir migrations create create_transactions_table sql
```

```sql
-- migrations/00001_create_users_table.sql

-- +goose Up
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
CREATE INDEX idx_users_email ON users(email);

-- +goose Down
DROP TABLE IF EXISTS users;
```

```sql
-- migrations/00002_create_transactions_table.sql

-- +goose Up
CREATE TABLE transactions (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id),
    amount DECIMAL(20, 8) NOT NULL,
    currency VARCHAR(10) NOT NULL DEFAULT 'USD',
    type VARCHAR(20) NOT NULL CHECK (type IN ('credit', 'debit')),
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    reference VARCHAR(100) UNIQUE,
    risk_score DECIMAL(5, 2) DEFAULT 0.00,
    metadata JSONB DEFAULT '{}',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
CREATE INDEX idx_transactions_user_id ON transactions(user_id);
CREATE INDEX idx_transactions_status ON transactions(status);

-- +goose Down
DROP TABLE IF EXISTS transactions;
```

### Running Migrations

```bash
export GOOSE_DBSTRING="postgres://user:pass@localhost:5432/mydb?sslmode=disable"
export GOOSE_DRIVER="postgres"

goose -dir migrations up          # Apply all pending
goose -dir migrations down        # Rollback last one
goose -dir migrations status      # Show status
```

### Programmatic Migrations

```go
import (
    "database/sql"
    "github.com/pressly/goose/v3"
    _ "github.com/jackc/pgx/v5/stdlib"
)

func RunMigrations(databaseURL string) error {
    db, err := sql.Open("pgx", databaseURL)
    if err != nil {
        return err
    }
    defer db.Close()
    goose.SetDialect("postgres")
    return goose.Up(db, "migrations")
}
```

---

## 19. Database Transactions

```go
func (r *TransactionRepository) Transfer(ctx context.Context, fromID, toID int, amount float64) error {
    tx, err := r.pool.Begin(ctx)
    if err != nil {
        return fmt.Errorf("begin: %w", err)
    }
    defer tx.Rollback(ctx) // No-op if committed

    // Lock sender's row
    var balance float64
    err = tx.QueryRow(ctx,
        "SELECT balance FROM accounts WHERE user_id = $1 FOR UPDATE", fromID,
    ).Scan(&balance)
    if err != nil {
        return fmt.Errorf("get balance: %w", err)
    }
    if balance < amount {
        return fmt.Errorf("insufficient balance: have %.2f, need %.2f", balance, amount)
    }

    // Debit sender
    _, err = tx.Exec(ctx,
        "UPDATE accounts SET balance = balance - $1, updated_at = NOW() WHERE user_id = $2",
        amount, fromID)
    if err != nil {
        return fmt.Errorf("debit: %w", err)
    }

    // Credit receiver
    _, err = tx.Exec(ctx,
        "UPDATE accounts SET balance = balance + $1, updated_at = NOW() WHERE user_id = $2",
        amount, toID)
    if err != nil {
        return fmt.Errorf("credit: %w", err)
    }

    // Record transactions
    _, err = tx.Exec(ctx, `INSERT INTO transactions (user_id, amount, type, status, reference)
        VALUES ($1, $2, 'debit', 'completed', $3)`,
        fromID, amount, fmt.Sprintf("TRF-%d-%d", fromID, toID))
    if err != nil {
        return err
    }

    _, err = tx.Exec(ctx, `INSERT INTO transactions (user_id, amount, type, status, reference)
        VALUES ($1, $2, 'credit', 'completed', $3)`,
        toID, amount, fmt.Sprintf("TRF-%d-%d", toID, fromID))
    if err != nil {
        return err
    }

    return tx.Commit(ctx)
}

// Reusable transaction wrapper
func WithTransaction(ctx context.Context, pool *pgxpool.Pool, fn func(pgx.Tx) error) error {
    tx, err := pool.Begin(ctx)
    if err != nil {
        return err
    }
    defer tx.Rollback(ctx)
    if err := fn(tx); err != nil {
        return err
    }
    return tx.Commit(ctx)
}
```

---

## 20. Dependency Injection

Go uses **constructor injection** — no container needed.

```go
// --- Define interfaces (contracts) ---
type UserRepository interface {
    Create(ctx context.Context, user *User) (*User, error)
    FindByID(ctx context.Context, id int) (*User, error)
    FindByEmail(ctx context.Context, email string) (*User, error)
}

type CacheService interface {
    Get(ctx context.Context, key string) (string, error)
    Set(ctx context.Context, key string, value string, ttl time.Duration) error
    Delete(ctx context.Context, key string) error
}

// --- Service depends on interfaces ---
type UserService struct {
    users  UserRepository
    cache  CacheService
    logger *slog.Logger
}

func NewUserService(users UserRepository, cache CacheService, logger *slog.Logger) *UserService {
    return &UserService{users: users, cache: cache, logger: logger}
}

// --- Handler depends on service ---
type UserHandler struct {
    service *UserService
}

func NewUserHandler(service *UserService) *UserHandler {
    return &UserHandler{service: service}
}

// --- Wire everything in main.go ---
func main() {
    ctx := context.Background()
    pool, _ := database.NewPostgresPool(ctx, os.Getenv("DATABASE_URL"))
    defer pool.Close()

    logger := slog.New(slog.NewJSONHandler(os.Stdout, nil))
    redisCache := cache.NewRedisCache("localhost:6379", "", 0)

    // Build dependency graph (bottom-up)
    userRepo := repository.NewUserRepository(pool)
    userService := service.NewUserService(userRepo, redisCache, logger)
    userHandler := handler.NewUserHandler(userService)

    r := chi.NewRouter()
    r.Post("/register", userHandler.Register)
    http.ListenAndServe(":3000", r)
}
```

---

## 21. Caching with Redis

```bash
go get github.com/redis/go-redis/v9
```

```go
package cache

import (
    "context"
    "encoding/json"
    "errors"
    "time"
    "github.com/redis/go-redis/v9"
)

var ErrCacheMiss = errors.New("cache miss")

type RedisCache struct {
    client *redis.Client
}

func NewRedisCache(addr, password string, db int) *RedisCache {
    return &RedisCache{
        client: redis.NewClient(&redis.Options{
            Addr: addr, Password: password, DB: db,
            DialTimeout: 5 * time.Second, PoolSize: 20,
        }),
    }
}

func (c *RedisCache) Get(ctx context.Context, key string) (string, error) {
    val, err := c.client.Get(ctx, key).Result()
    if err == redis.Nil {
        return "", ErrCacheMiss
    }
    return val, err
}

func (c *RedisCache) Set(ctx context.Context, key, value string, ttl time.Duration) error {
    return c.client.Set(ctx, key, value, ttl).Err()
}

func (c *RedisCache) Delete(ctx context.Context, key string) error {
    return c.client.Del(ctx, key).Err()
}

func (c *RedisCache) GetJSON(ctx context.Context, key string, dest interface{}) error {
    val, err := c.Get(ctx, key)
    if err != nil {
        return err
    }
    return json.Unmarshal([]byte(val), dest)
}

func (c *RedisCache) SetJSON(ctx context.Context, key string, value interface{}, ttl time.Duration) error {
    data, err := json.Marshal(value)
    if err != nil {
        return err
    }
    return c.Set(ctx, key, string(data), ttl)
}

// Cache-aside pattern with generics
func GetOrSet[T any](ctx context.Context, cache *RedisCache, key string, ttl time.Duration, fn func() (T, error)) (T, error) {
    var result T
    if err := cache.GetJSON(ctx, key, &result); err == nil {
        return result, nil // Cache hit
    }
    result, err := fn() // Cache miss — fetch from source
    if err != nil {
        return result, err
    }
    go cache.SetJSON(context.Background(), key, result, ttl) // Store async
    return result, nil
}

// Usage in service:
func (s *UserService) GetByID(ctx context.Context, id int) (*User, error) {
    return GetOrSet(ctx, s.cache, fmt.Sprintf("user:%d", id), 15*time.Minute, func() (*User, error) {
        return s.users.FindByID(ctx, id)
    })
}
```

---

## 22. Authentication & Authorization

```bash
go get github.com/golang-jwt/jwt/v5
go get golang.org/x/crypto/bcrypt
```

```go
package auth

import (
    "context"
    "net/http"
    "strings"
    "time"
    "github.com/golang-jwt/jwt/v5"
)

type Claims struct {
    UserID int    `json:"user_id"`
    Email  string `json:"email"`
    Role   string `json:"role"`
    jwt.RegisteredClaims
}

type JWTService struct {
    secretKey []byte
    duration  time.Duration
}

func NewJWTService(secret string, duration time.Duration) *JWTService {
    return &JWTService{secretKey: []byte(secret), duration: duration}
}

func (s *JWTService) GenerateToken(userID int, email, role string) (string, error) {
    claims := &Claims{
        UserID: userID, Email: email, Role: role,
        RegisteredClaims: jwt.RegisteredClaims{
            ExpiresAt: jwt.NewNumericDate(time.Now().Add(s.duration)),
            IssuedAt:  jwt.NewNumericDate(time.Now()),
            Issuer:    "myapp",
        },
    }
    return jwt.NewWithClaims(jwt.SigningMethodHS256, claims).SignedString(s.secretKey)
}

func (s *JWTService) ValidateToken(tokenString string) (*Claims, error) {
    token, err := jwt.ParseWithClaims(tokenString, &Claims{}, func(token *jwt.Token) (interface{}, error) {
        return s.secretKey, nil
    })
    if err != nil {
        return nil, err
    }
    claims, ok := token.Claims.(*Claims)
    if !ok || !token.Valid {
        return nil, fmt.Errorf("invalid token")
    }
    return claims, nil
}

// Auth middleware
type contextKey string
const UserClaimsKey contextKey = "userClaims"

func AuthMiddleware(jwtSvc *JWTService) func(http.Handler) http.Handler {
    return func(next http.Handler) http.Handler {
        return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
            header := r.Header.Get("Authorization")
            if header == "" {
                respondJSON(w, 401, APIResponse{Success: false, Error: "Missing auth header"})
                return
            }
            parts := strings.SplitN(header, " ", 2)
            if len(parts) != 2 || parts[0] != "Bearer" {
                respondJSON(w, 401, APIResponse{Success: false, Error: "Invalid auth format"})
                return
            }
            claims, err := jwtSvc.ValidateToken(parts[1])
            if err != nil {
                respondJSON(w, 401, APIResponse{Success: false, Error: "Invalid token"})
                return
            }
            ctx := context.WithValue(r.Context(), UserClaimsKey, claims)
            next.ServeHTTP(w, r.WithContext(ctx))
        })
    }
}

func GetUserClaims(ctx context.Context) *Claims {
    claims, _ := ctx.Value(UserClaimsKey).(*Claims)
    return claims
}

func RequireRole(roles ...string) func(http.Handler) http.Handler {
    return func(next http.Handler) http.Handler {
        return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
            claims := GetUserClaims(r.Context())
            if claims == nil {
                respondJSON(w, 401, APIResponse{Success: false, Error: "Unauthorized"})
                return
            }
            for _, role := range roles {
                if claims.Role == role {
                    next.ServeHTTP(w, r)
                    return
                }
            }
            respondJSON(w, 403, APIResponse{Success: false, Error: "Forbidden"})
        })
    }
}
```

---

## 23. Testing

```go
// Table-driven tests
func TestValidateEmail(t *testing.T) {
    tests := []struct {
        name  string
        email string
        want  bool
    }{
        {"valid", "user@example.com", true},
        {"missing @", "userexample.com", false},
        {"empty", "", false},
    }
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            if got := isValidEmail(tt.email); got != tt.want {
                t.Errorf("isValidEmail(%q) = %v, want %v", tt.email, got, tt.want)
            }
        })
    }
}

// HTTP handler tests
func TestTransactionHandler_Create(t *testing.T) {
    store := NewTransactionStore()
    handler := NewTransactionHandler(store)
    r := chi.NewRouter()
    r.Post("/transactions", handler.Create)

    body := `{"user_id":1,"amount":100.50,"currency":"USD","type":"credit"}`
    req := httptest.NewRequest(http.MethodPost, "/transactions", strings.NewReader(body))
    req.Header.Set("Content-Type", "application/json")
    rr := httptest.NewRecorder()
    r.ServeHTTP(rr, req)

    if rr.Code != http.StatusCreated {
        t.Errorf("expected 201, got %d", rr.Code)
    }
}
```

```bash
go test ./...                 # All tests
go test -v -race -cover ./... # Verbose + race detection + coverage
go test -run TestName ./...   # Specific test
```

### Benchmark Tests

```go
// Benchmarks measure performance. Run with: go test -bench=. -benchmem ./...

func BenchmarkSum(b *testing.B) {
    nums := make([]int, 1000)
    for i := range nums {
        nums[i] = i
    }
    b.ResetTimer() // Don't count setup time
    for i := 0; i < b.N; i++ {
        sum(nums)
    }
}

// Compare implementations
func BenchmarkStringConcat(b *testing.B) {
    for i := 0; i < b.N; i++ {
        result := ""
        for j := 0; j < 100; j++ {
            result += "x"
        }
        _ = result
    }
}

func BenchmarkStringBuilder(b *testing.B) {
    for i := 0; i < b.N; i++ {
        var sb strings.Builder
        for j := 0; j < 100; j++ {
            sb.WriteString("x")
        }
        _ = sb.String()
    }
}
```

```bash
go test -bench=. -benchmem ./...
# BenchmarkStringConcat-8     100000   14523 ns/op   5336 B/op   99 allocs/op
# BenchmarkStringBuilder-8   1000000    1021 ns/op    568 B/op    8 allocs/op
```

### Mock Testing with testify

```bash
go get github.com/stretchr/testify
```

```go
// Define an interface for your dependency
type UserRepository interface {
    GetByID(ctx context.Context, id int) (*User, error)
    Create(ctx context.Context, u *User) error
}

// UserService depends on the interface, not the concrete type
type UserService struct {
    repo UserRepository
}

func (s *UserService) GetUser(ctx context.Context, id int) (*User, error) {
    if id <= 0 {
        return nil, errors.New("invalid id")
    }
    return s.repo.GetByID(ctx, id)
}

// --- In your test file ---
import (
    "testing"
    "github.com/stretchr/testify/assert"
    "github.com/stretchr/testify/mock"
)

// Mock implements UserRepository
type MockUserRepo struct {
    mock.Mock
}

func (m *MockUserRepo) GetByID(ctx context.Context, id int) (*User, error) {
    args := m.Called(ctx, id)
    if args.Get(0) == nil {
        return nil, args.Error(1)
    }
    return args.Get(0).(*User), args.Error(1)
}

func (m *MockUserRepo) Create(ctx context.Context, u *User) error {
    return m.Called(ctx, u).Error(0)
}

func TestGetUser_Found(t *testing.T) {
    repo := new(MockUserRepo)
    svc := &UserService{repo: repo}
    ctx := context.Background()

    expected := &User{ID: 1, FirstName: "Brainy"}
    repo.On("GetByID", ctx, 1).Return(expected, nil)

    user, err := svc.GetUser(ctx, 1)

    assert.NoError(t, err)
    assert.Equal(t, expected, user)
    repo.AssertExpectations(t)
}

func TestGetUser_InvalidID(t *testing.T) {
    repo := new(MockUserRepo)
    svc := &UserService{repo: repo}

    _, err := svc.GetUser(context.Background(), 0)

    assert.EqualError(t, err, "invalid id")
    repo.AssertNotCalled(t, "GetByID")
}

func TestGetUser_NotFound(t *testing.T) {
    repo := new(MockUserRepo)
    svc := &UserService{repo: repo}
    ctx := context.Background()

    repo.On("GetByID", ctx, 99).Return(nil, errors.New("not found"))

    _, err := svc.GetUser(ctx, 99)

    assert.EqualError(t, err, "not found")
    repo.AssertExpectations(t)
}
```

---

## 24. Configuration Management

```bash
go get github.com/kelseyhightower/envconfig
```

```go
package config

import (
    "fmt"
    "time"
    "github.com/kelseyhightower/envconfig"
)

type Config struct {
    ServerPort  int           `envconfig:"SERVER_PORT" default:"3000"`
    Env         string        `envconfig:"APP_ENV" default:"development"`
    DatabaseURL string        `envconfig:"DATABASE_URL" required:"true"`
    RedisAddr   string        `envconfig:"REDIS_ADDR" default:"localhost:6379"`
    JWTSecret   string        `envconfig:"JWT_SECRET" required:"true"`
    JWTDuration time.Duration `envconfig:"JWT_DURATION" default:"24h"`
    LogLevel    string        `envconfig:"LOG_LEVEL" default:"info"`
}

func Load() (*Config, error) {
    var cfg Config
    if err := envconfig.Process("", &cfg); err != nil {
        return nil, fmt.Errorf("load config: %w", err)
    }
    return &cfg, nil
}

func (c *Config) IsProduction() bool { return c.Env == "production" }
```

---

## 25. Logging with slog

```go
import "log/slog"

// JSON for production, text for dev
func NewLogger(level, env string) *slog.Logger {
    opts := &slog.HandlerOptions{Level: parseLevel(level)}
    if env == "production" {
        return slog.New(slog.NewJSONHandler(os.Stdout, opts))
    }
    return slog.New(slog.NewTextHandler(os.Stdout, opts))
}

// Usage:
logger.Info("server starting", "port", 3000)
logger.Error("payment failed", "error", err, "user_id", 42)

// With persistent fields
txLogger := logger.With("tx_id", "tx_001", "user_id", 42)
txLogger.Info("transaction started")
txLogger.Info("transaction completed")
```

---

## 26. Graceful Shutdown

```go
func main() {
    // ... setup code ...

    server := &http.Server{
        Addr:         ":3000",
        Handler:      r,
        ReadTimeout:  15 * time.Second,
        WriteTimeout: 15 * time.Second,
        IdleTimeout:  60 * time.Second,
    }

    go func() {
        logger.Info("server starting", "addr", ":3000")
        if err := server.ListenAndServe(); err != http.ErrServerClosed {
            logger.Error("server error", "error", err)
            os.Exit(1)
        }
    }()

    // Wait for interrupt signal
    quit := make(chan os.Signal, 1)
    signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
    <-quit

    logger.Info("shutting down...")
    ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
    defer cancel()

    if err := server.Shutdown(ctx); err != nil {
        logger.Error("forced shutdown", "error", err)
    }
    logger.Info("server stopped")
}
```

---

## 27. Project Structure

```
myapp/
├── cmd/server/main.go         # Entry point — wires everything
├── internal/
│   ├── config/config.go       # Configuration
│   ├── database/postgres.go   # DB connection
│   ├── handler/               # HTTP handlers (controllers)
│   ├── middleware/             # Auth, CORS, logging, rate limit
│   ├── model/                 # Domain models
│   ├── repository/            # Data access (interfaces + implementations)
│   ├── service/               # Business logic
│   ├── auth/jwt.go            # JWT service
│   ├── cache/redis.go         # Cache layer
│   └── logging/logger.go      # Logger setup
├── migrations/                # Goose SQL files
├── go.mod
├── go.sum
├── Makefile
├── Dockerfile
└── .env.example
```

---

## Appendix A: Generics (Go 1.18+)

```go
// Generic functions
func Map[T any, R any](items []T, fn func(T) R) []R {
    result := make([]R, len(items))
    for i, v := range items { result[i] = fn(v) }
    return result
}

func Filter[T any](items []T, fn func(T) bool) []T {
    var result []T
    for _, v := range items {
        if fn(v) { result = append(result, v) }
    }
    return result
}

func Find[T any](items []T, fn func(T) bool) (T, bool) {
    for _, v := range items {
        if fn(v) { return v, true }
    }
    var zero T
    return zero, false
}

// Usage:
doubled := Map([]int{1,2,3}, func(n int) int { return n * 2 })
active := Filter(users, func(u User) bool { return u.IsActive })
```

---

## Appendix B: Chi Middleware Reference

```go
r.Use(middleware.RequestID)     // Unique X-Request-Id header
r.Use(middleware.RealIP)        // Extract real IP from proxy headers
r.Use(middleware.Logger)        // Log requests
r.Use(middleware.Recoverer)     // Recover from panics
r.Use(middleware.Timeout(30*time.Second)) // Request timeout
r.Use(middleware.Throttle(100)) // Max concurrent requests
r.Use(middleware.Compress(5))   // Gzip compression
r.Use(middleware.Heartbeat("/ping")) // Health check endpoint
r.Use(middleware.StripSlashes)  // Remove trailing slashes
```

---

## Appendix C: Go vs Laravel Quick Reference

| Laravel | Go Equivalent |
|---------|---------------|
| `Route::get()` | `r.Get()` (Chi) |
| `Route::group()` | `r.Group()` / `r.Route()` |
| `Route::middleware()` | `r.Use()` / `r.With()` |
| Controller | Handler struct |
| Eloquent | Struct + Repository |
| Service Provider | Constructor injection in `main()` |
| Service Container | Explicit wiring (no container) |
| FormRequest | `validator/v10` struct tags |
| `DB::transaction()` | `pool.Begin()` / `tx.Commit()` |
| Migration | Goose SQL files |
| `php artisan` | `Makefile` commands |
| Queue jobs | Goroutines + channels |
| Cache facade | Redis wrapper |
| `try/catch` | `if err != nil` |
| Trait | Struct embedding |
| Interface `implements` | Implicit (just implement methods) |
| `composer.json` | `go.mod` |

---

## Appendix D: File I/O

```go
import (
    "bufio"
    "fmt"
    "log"
    "os"
    "path/filepath"
    "strings"
)

// Read entire file
data, err := os.ReadFile("config.json")
if err != nil {
    log.Fatal(err)
}
fmt.Println(string(data))

// Write entire file (creates or truncates)
err = os.WriteFile("output.txt", []byte("hello\n"), 0644)

// Append to file
f, err := os.OpenFile("log.txt", os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0644)
if err != nil {
    log.Fatal(err)
}
defer f.Close()
fmt.Fprintln(f, "new log line")

// Read line-by-line (efficient for large files)
func readLines(path string) ([]string, error) {
    f, err := os.Open(path)
    if err != nil {
        return nil, err
    }
    defer f.Close()

    var lines []string
    scanner := bufio.NewScanner(f)
    for scanner.Scan() {
        lines = append(lines, scanner.Text())
    }
    return lines, scanner.Err()
}

// Write with bufio (batches writes — faster for many small writes)
func writeLines(path string, lines []string) error {
    f, err := os.Create(path)
    if err != nil {
        return err
    }
    defer f.Close()

    w := bufio.NewWriter(f)
    for _, line := range lines {
        fmt.Fprintln(w, line)
    }
    return w.Flush() // IMPORTANT: flush buffered data
}

// Check if file / directory exists
func exists(path string) bool {
    _, err := os.Stat(path)
    return !os.IsNotExist(err)
}

// Walk a directory tree
err = filepath.WalkDir("./uploads", func(path string, d os.DirEntry, err error) error {
    if err != nil {
        return err
    }
    if !d.IsDir() && strings.HasSuffix(path, ".json") {
        fmt.Println("Found:", path)
    }
    return nil
})

// Create temp file (useful in tests)
tmp, err := os.CreateTemp("", "myapp-*.json")
defer os.Remove(tmp.Name())
```

---

## Appendix E: Regular Expressions

```go
import "regexp"

// Compile once, reuse (panic on invalid pattern — use at package level)
var emailRe = regexp.MustCompile(`^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$`)
var phoneRe = regexp.MustCompile(`^\+?[1-9]\d{7,14}$`)
var uuidRe  = regexp.MustCompile(`^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$`)

// Match
fmt.Println(emailRe.MatchString("user@example.com"))  // true
fmt.Println(emailRe.MatchString("not-an-email"))       // false

// Find first match
re := regexp.MustCompile(`\d+`)
fmt.Println(re.FindString("order-12345-abc"))          // "12345"

// Find all matches
nums := re.FindAllString("a1 b22 c333", -1)            // ["1", "22", "333"]

// Named capture groups
dateRe := regexp.MustCompile(`(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})`)
match := dateRe.FindStringSubmatch("2024-01-15")
names := dateRe.SubexpNames()
result := map[string]string{}
for i, name := range names {
    if i != 0 && name != "" && i < len(match) {
        result[name] = match[i]
    }
}
fmt.Println(result) // map[day:15 month:01 year:2024]

// Replace
cleaned := regexp.MustCompile(`\s+`).ReplaceAllString("hello   world", " ")
fmt.Println(cleaned) // "hello world"

// Split
parts := regexp.MustCompile(`[,;\s]+`).Split("a, b; c d", -1)
fmt.Println(parts) // [a b c d]

// In a validator (compile at package level for performance)
var (
    reEmail = regexp.MustCompile(`^[^\s@]+@[^\s@]+\.[^\s@]+$`)
    reSlug  = regexp.MustCompile(`^[a-z0-9]+(?:-[a-z0-9]+)*$`)
)

func isValidEmail(s string) bool { return reEmail.MatchString(s) }
func isValidSlug(s string) bool  { return reSlug.MatchString(s) }
```

---

## Appendix F: Embedded Files — go:embed

Embed static files directly into your binary at compile time. No need to ship separate asset directories.

```go
import _ "embed"

// Embed a single file as a string
//go:embed templates/welcome.html
var welcomeHTML string

// Embed a single file as bytes
//go:embed configs/default.json
var defaultConfig []byte

// Embed an entire directory (use io/fs to read)
import "embed"

//go:embed static
var staticFiles embed.FS

//go:embed migrations
var migrationsFS embed.FS
```

```go
// Serve embedded static files over HTTP
import (
    "embed"
    "io/fs"
    "net/http"
)

//go:embed static
var staticFS embed.FS

func main() {
    // Serve files under static/ at /static/
    sub, _ := fs.Sub(staticFS, "static")
    http.Handle("/static/", http.StripPrefix("/static/", http.FileServer(http.FS(sub))))
}

// Read embedded migration files with goose
import "github.com/pressly/goose/v3"

//go:embed migrations/*.sql
var migrationsFS embed.FS

func runMigrations(db *sql.DB) error {
    goose.SetBaseFS(migrationsFS)
    return goose.Up(db, "migrations")
}

// Read an embedded config file
//go:embed configs/default.json
var defaultConfigBytes []byte

func loadDefaultConfig() (*Config, error) {
    var cfg Config
    return &cfg, json.Unmarshal(defaultConfigBytes, &cfg)
}

// Note: json.Unmarshal requires: import "encoding/json"
```

---

## Appendix G: CLI with the flag Package

```go
package main

import (
    "flag"
    "fmt"
    "os"
    "time"
)

func main() {
    // Define flags
    port    := flag.Int("port", 3000, "HTTP server port")
    host    := flag.String("host", "localhost", "Host to bind to")
    verbose := flag.Bool("verbose", false, "Enable verbose logging")
    timeout := flag.Duration("timeout", 30*time.Second, "Request timeout")

    // Parse flags (reads os.Args)
    flag.Parse()

    // Remaining positional arguments after flags
    args := flag.Args()

    if *verbose {
        fmt.Printf("Starting on %s:%d (timeout: %s)\n", *host, *port, *timeout)
        fmt.Printf("Extra args: %v\n", args)
    }

    // Usage: ./myapp -port=8080 -host=0.0.0.0 -verbose
    // Or:    ./myapp --port 8080 --verbose
}

// Sub-commands pattern (like git commit, git push)
func main() {
    if len(os.Args) < 2 {
        fmt.Println("Usage: myapp <command> [options]")
        fmt.Println("Commands: serve, migrate, seed")
        os.Exit(1)
    }

    switch os.Args[1] {
    case "serve":
        serveCmd := flag.NewFlagSet("serve", flag.ExitOnError)
        port := serveCmd.Int("port", 3000, "Port to listen on")
        serveCmd.Parse(os.Args[2:])
        runServer(*port)

    case "migrate":
        migrateCmd := flag.NewFlagSet("migrate", flag.ExitOnError)
        dir := migrateCmd.String("dir", "migrations", "Migration directory")
        migrateCmd.Parse(os.Args[2:])
        runMigrations(*dir)

    default:
        fmt.Fprintf(os.Stderr, "Unknown command: %s\n", os.Args[1])
        os.Exit(1)
    }
}
```

### Interactive CLI — Reading Instructions from the User

The `flag` package handles arguments passed at startup. For a program that **prompts the user and reacts to what they type**, use `bufio.Scanner` on `os.Stdin`.

#### Minimal example — read one command at a time

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

func main() {
	scanner := bufio.NewScanner(os.Stdin)

	fmt.Println("Simple CLI. Type a command (hello / greet <name> / quit):")

	for {
		fmt.Print("> ")

		if !scanner.Scan() { // EOF or error
			break
		}

		line := strings.TrimSpace(scanner.Text())
		if line == "" {
			continue
		}

		parts := strings.Fields(line) // split on whitespace
		cmd := parts[0]

		switch cmd {
		case "hello":
			fmt.Println("Hello, World!")

		case "greet":
			if len(parts) < 2 {
				fmt.Println("Usage: greet <name>")
			} else {
				fmt.Printf("Hello, %s!\n", parts[1])
			}

		case "quit", "exit":
			fmt.Println("Bye!")
			return

		default:
			fmt.Fprintf(os.Stderr, "Unknown command: %q. Type 'quit' to exit.\n", cmd)
		}
	}

	if err := scanner.Err(); err != nil {
		fmt.Fprintln(os.Stderr, "read error:", err)
		os.Exit(1)
	}
}
```

Run it:

```
$ go run main.go
Simple CLI. Type a command (hello / greet <name> / quit):
> hello
Hello, World!
> greet Alice
Hello, Alice!
> unknown
Unknown command: "unknown". Type 'quit' to exit.
> quit
Bye!
```

#### Full interactive CLI with a help menu

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

// Action registry — add new commands here without changing the main loop.
type CommandFunc func(args []string)

var commands map[string]CommandFunc

func init() {
	commands = map[string]CommandFunc{
		"add":    cmdAdd,
		"upper":  cmdUpper,
		"repeat": cmdRepeat,
		"help":   cmdHelp,
		"quit":   nil, // handled separately
	}
}

func main() {
	fmt.Println("Interactive CLI — type 'help' for available commands.")
	scanner := bufio.NewScanner(os.Stdin)

	for {
		fmt.Print("cli> ")
		if !scanner.Scan() {
			break
		}

		input := strings.TrimSpace(scanner.Text())
		if input == "" {
			continue
		}

		parts := strings.Fields(input)
		name, args := parts[0], parts[1:]

		if name == "quit" || name == "exit" {
			fmt.Println("Goodbye!")
			return
		}

		fn, ok := commands[name]
		if !ok {
			fmt.Fprintf(os.Stderr, "unknown command %q — type 'help'\n", name)
			continue
		}

		fn(args)
	}

	if err := scanner.Err(); err != nil {
		fmt.Fprintln(os.Stderr, "error:", err)
		os.Exit(1)
	}
}

// cmdAdd sums all integer arguments.
func cmdAdd(args []string) {
	if len(args) == 0 {
		fmt.Println("Usage: add <num1> <num2> ...")
		return
	}
	total := 0
	for _, a := range args {
		n, err := strconv.Atoi(a)
		if err != nil {
			fmt.Fprintf(os.Stderr, "not a number: %q\n", a)
			return
		}
		total += n
	}
	fmt.Println(total)
}

// cmdUpper converts text to upper-case.
func cmdUpper(args []string) {
	if len(args) == 0 {
		fmt.Println("Usage: upper <text...>")
		return
	}
	fmt.Println(strings.ToUpper(strings.Join(args, " ")))
}

// cmdRepeat prints text N times.
func cmdRepeat(args []string) {
	if len(args) < 2 {
		fmt.Println("Usage: repeat <n> <text...>")
		return
	}
	n, err := strconv.Atoi(args[0])
	if err != nil || n < 1 {
		fmt.Fprintln(os.Stderr, "first argument must be a positive integer")
		return
	}
	text := strings.Join(args[1:], " ")
	for i := 0; i < n; i++ {
		fmt.Println(text)
	}
}

// cmdHelp lists available commands.
func cmdHelp(_ []string) {
	fmt.Println("Available commands:")
	fmt.Println("  add <num...>       — sum numbers")
	fmt.Println("  upper <text...>    — convert to upper-case")
	fmt.Println("  repeat <n> <text>  — print text n times")
	fmt.Println("  help               — show this message")
	fmt.Println("  quit / exit        — exit the program")
}
```

Sample session:

```
$ go run main.go
Interactive CLI — type 'help' for available commands.
cli> help
Available commands:
  add <num...>       — sum numbers
  upper <text...>    — convert to upper-case
  repeat <n> <text>  — print text n times
  help               — show this message
  quit / exit        — exit the program
cli> add 3 4 5
12
cli> upper hello world
HELLO WORLD
cli> repeat 3 Go is great
Go is great
Go is great
Go is great
cli> quit
Goodbye!
```

#### Key points

| Topic | Detail |
|---|---|
| Reading input | `bufio.NewScanner(os.Stdin)` — handles lines of any length |
| Parsing | `strings.Fields(line)` splits on any whitespace |
| Extensibility | A `map[string]CommandFunc` registry lets you add commands without changing the loop |
| EOF / Ctrl-D | `scanner.Scan()` returns `false`; call `scanner.Err()` to distinguish EOF from error |
| Ctrl-C | The OS sends SIGINT; the program exits immediately — no extra handling needed for basic tools |

---

## Appendix H: Functional Options Pattern

A common Go pattern for configuring structs with optional parameters — avoids long constructor argument lists and keeps the API clean.

```go
import "time"

// The type being configured
type Server struct {
    host         string
    port         int
    timeout      time.Duration
    maxConns     int
    tlsEnabled   bool
}

// Option is a function that modifies a Server
type Option func(*Server)

// Option constructors
func WithHost(host string) Option {
    return func(s *Server) { s.host = host }
}

func WithPort(port int) Option {
    return func(s *Server) { s.port = port }
}

func WithTimeout(d time.Duration) Option {
    return func(s *Server) { s.timeout = d }
}

func WithMaxConns(n int) Option {
    return func(s *Server) { s.maxConns = n }
}

func WithTLS() Option {
    return func(s *Server) { s.tlsEnabled = true }
}

// Constructor applies defaults, then overrides with options
func NewServer(opts ...Option) *Server {
    s := &Server{
        host:     "localhost",
        port:     3000,
        timeout:  30 * time.Second,
        maxConns: 100,
    }
    for _, opt := range opts {
        opt(s)
    }
    return s
}

// Usage — only specify what you need to change
s1 := NewServer()                                     // all defaults
s2 := NewServer(WithPort(8080), WithTLS())            // override port + TLS
s3 := NewServer(
    WithHost("0.0.0.0"),
    WithPort(443),
    WithTimeout(60*time.Second),
    WithMaxConns(500),
    WithTLS(),
)

// Real-world example: Redis client options
// Requires: go get github.com/redis/go-redis/v9
import "github.com/redis/go-redis/v9"

type RedisOptions struct {
    Addr         string
    Password     string
    DB           int
    PoolSize     int
    ReadTimeout  time.Duration
    WriteTimeout time.Duration
}

type RedisOption func(*RedisOptions)

func WithRedisAddr(addr string) RedisOption {
    return func(o *RedisOptions) { o.Addr = addr }
}

func WithRedisPassword(pw string) RedisOption {
    return func(o *RedisOptions) { o.Password = pw }
}

func WithRedisPoolSize(n int) RedisOption {
    return func(o *RedisOptions) { o.PoolSize = n }
}

func NewRedisClient(opts ...RedisOption) *redis.Client {
    o := &RedisOptions{
        Addr:         "localhost:6379",
        DB:           0,
        PoolSize:     10,
        ReadTimeout:  3 * time.Second,
        WriteTimeout: 3 * time.Second,
    }
    for _, opt := range opts {
        opt(o)
    }
    return redis.NewClient(&redis.Options{
        Addr:         o.Addr,
        Password:     o.Password,
        DB:           o.DB,
        PoolSize:     o.PoolSize,
        ReadTimeout:  o.ReadTimeout,
        WriteTimeout: o.WriteTimeout,
    })
}
```

---

---

## Appendix I: Sending Email

Go can send email via the standard `net/smtp` package or the more ergonomic `gomail` library.

### 1. Plain-text email with net/smtp (stdlib)

No external dependency — good for simple use-cases or when you control the SMTP server.

```go
import (
    "fmt"
    "net/smtp"
)

func sendPlainEmail(
    smtpHost, smtpPort string,
    from, password string,
    to []string,
    subject, body string,
) error {
    auth := smtp.PlainAuth("", from, password, smtpHost)

    // RFC 822 message format
    msg := fmt.Sprintf(
        "From: %s\r\nTo: %s\r\nSubject: %s\r\n\r\n%s",
        from, to[0], subject, body,
    )

    addr := smtpHost + ":" + smtpPort
    return smtp.SendMail(addr, auth, from, to, []byte(msg))
}

// Usage
err := sendPlainEmail(
    "smtp.gmail.com", "587",
    "sender@gmail.com", "app-password",
    []string{"recipient@example.com"},
    "Hello from Go",
    "This is a plain text email sent from Go.",
)
if err != nil {
    log.Fatal(err)
}
```

### 2. HTML email with attachments using gomail

`gomail` is the most widely used Go email library — supports HTML, multiple recipients, CC/BCC, and file attachments.

```bash
go get gopkg.in/gomail.v2
```

```go
import (
    "strings"

    "gopkg.in/gomail.v2"
)

func sendHTMLEmail() error {
    m := gomail.NewMessage()

    m.SetHeader("From", "sender@example.com")
    m.SetHeader("To", "alice@example.com", "bob@example.com")
    m.SetAddressHeader("Cc", "charlie@example.com", "Charlie")
    m.SetHeader("Subject", "Your invoice is ready")

    // Plain-text fallback + HTML body
    m.SetBody("text/plain", "Your invoice is attached.")
    m.AddAlternative("text/html", `<h1>Invoice</h1><p>Please find your invoice <b>attached</b>.</p>`)

    // Attach a file from disk
    m.Attach("/tmp/invoice.pdf")

    // Attach an in-memory file
    m.AttachReader("report.csv", strings.NewReader("date,amount\n2024-01-01,100"))

    // Send via Gmail SMTP (port 587 = STARTTLS)
    d := gomail.NewDialer("smtp.gmail.com", 587, "sender@example.com", "app-password")
    return d.DialAndSend(m)
}
```

### 3. Reusable Mailer service

Wrap the mailer in a struct so it can be injected into handlers and mocked in tests.

```go
package mailer

import (
    "bytes"
    "fmt"
    "html/template"

    "gopkg.in/gomail.v2"
)

// Sender is the interface your handlers depend on — easy to mock in tests.
type Sender interface {
    Send(to, subject, htmlBody string) error
    SendTemplate(to, subject, tmplName string, data any) error
}

type Config struct {
    Host     string
    Port     int
    Username string
    Password string
    From     string
}

type Mailer struct {
    cfg       Config
    dialer    *gomail.Dialer
    templates *template.Template
}

func New(cfg Config, templates *template.Template) *Mailer {
    d := gomail.NewDialer(cfg.Host, cfg.Port, cfg.Username, cfg.Password)
    return &Mailer{cfg: cfg, dialer: d, templates: templates}
}

func (m *Mailer) Send(to, subject, htmlBody string) error {
    msg := gomail.NewMessage()
    msg.SetHeader("From", m.cfg.From)
    msg.SetHeader("To", to)
    msg.SetHeader("Subject", subject)
    msg.SetBody("text/html", htmlBody)
    return m.dialer.DialAndSend(msg)
}

func (m *Mailer) SendTemplate(to, subject, tmplName string, data any) error {
    var buf bytes.Buffer
    if err := m.templates.ExecuteTemplate(&buf, tmplName, data); err != nil {
        return fmt.Errorf("mailer: execute template %q: %w", tmplName, err)
    }
    return m.Send(to, subject, buf.String())
}
```

Load templates at startup:

```go
// main.go
import "html/template"

tmpl := template.Must(template.ParseGlob("templates/email/*.html"))

mailerCfg := mailer.Config{
    Host:     cfg.SMTPHost,
    Port:     cfg.SMTPPort,
    Username: cfg.SMTPUser,
    Password: cfg.SMTPPassword,
    From:     cfg.SMTPFrom,
}
emailer := mailer.New(mailerCfg, tmpl)
```

Example email template (`templates/email/welcome.html`):

```html
<!DOCTYPE html>
<html>
<body>
  <h1>Welcome, {{.Name}}!</h1>
  <p>Click <a href="{{.VerifyURL}}">here</a> to verify your email.</p>
</body>
</html>
```

### 4. Sending email from a Chi handler

```go
type UserHandler struct {
    repo   UserRepository
    mailer mailer.Sender
}

func (h *UserHandler) Register(w http.ResponseWriter, r *http.Request) {
    var req RegisterRequest
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        respondJSON(w, http.StatusBadRequest, APIResponse{Error: "invalid JSON"})
        return
    }

    user, err := h.repo.Create(r.Context(), req)
    if err != nil {
        respondJSON(w, http.StatusInternalServerError, APIResponse{Error: "could not create user"})
        return
    }

    // Send welcome email asynchronously — don't block the HTTP response
    go func() {
        data := map[string]string{
            "Name":      user.FirstName,
            "VerifyURL": "https://myapp.com/verify?token=" + user.VerifyToken,
        }
        if err := h.mailer.SendTemplate(user.Email, "Welcome to MyApp", "welcome.html", data); err != nil {
            slog.Error("failed to send welcome email", "error", err, "user_id", user.ID)
        }
    }()

    respondJSON(w, http.StatusCreated, APIResponse{Success: true, Data: user})
}
```

### 5. Testing the mailer (mock)

Because handlers depend on the `Sender` interface, you can swap in a test double — no real SMTP required.

```go
import (
    "fmt"
    "net/http"
    "net/http/httptest"
    "strings"
    "sync"
    "testing"
    "time"

    "github.com/stretchr/testify/assert"
)

// mock_mailer.go (in your test helpers or testdata package)
type MockMailer struct {
    mu   sync.Mutex
    Sent []SentEmail
}

type SentEmail struct {
    To      string
    Subject string
    Body    string
}

func (m *MockMailer) Send(to, subject, body string) error {
    m.mu.Lock()
    defer m.mu.Unlock()
    m.Sent = append(m.Sent, SentEmail{to, subject, body})
    return nil
}

func (m *MockMailer) SendTemplate(to, subject, tmplName string, data any) error {
    return m.Send(to, subject, fmt.Sprintf("template:%s", tmplName))
}

// For testing the goroutine in Register, wrap the handler so we can signal when
// the email goroutine is done.  In production, use the plain UserHandler from
// section 4 without the emailDone field.
type testableUserHandler struct {
    UserHandler
    emailDone chan struct{}
}

func (h *testableUserHandler) Register(w http.ResponseWriter, r *http.Request) {
    var req RegisterRequest
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        respondJSON(w, http.StatusBadRequest, APIResponse{Error: "invalid JSON"})
        return
    }
    user, err := h.repo.Create(r.Context(), req)
    if err != nil {
        respondJSON(w, http.StatusInternalServerError, APIResponse{Error: "could not create user"})
        return
    }

    go func() {
        data := map[string]string{"Name": user.FirstName, "VerifyURL": "https://myapp.com/verify?token=" + user.VerifyToken}
        if err := h.mailer.SendTemplate(user.Email, "Welcome to MyApp", "welcome.html", data); err != nil {
            slog.Error("failed to send welcome email", "error", err, "user_id", user.ID)
        }
        h.emailDone <- struct{}{}
    }()

    respondJSON(w, http.StatusCreated, APIResponse{Success: true, Data: user})
}

// In your test
func TestRegister_SendsWelcomeEmail(t *testing.T) {
    mockMail := &MockMailer{}
    done := make(chan struct{}, 1)
    handler := &testableUserHandler{
        UserHandler: UserHandler{repo: &MockUserRepo{}, mailer: mockMail},
        emailDone:   done,
    }

    body := `{"email":"alice@example.com","first_name":"Alice","password":"secret"}`
    req := httptest.NewRequest(http.MethodPost, "/register", strings.NewReader(body))
    req.Header.Set("Content-Type", "application/json")
    rr := httptest.NewRecorder()

    handler.Register(rr, req)

    // Wait for the email goroutine to finish (with a timeout for safety)
    select {
    case <-done:
    case <-time.After(2 * time.Second):
        t.Fatal("timed out waiting for email goroutine")
    }

    assert.Equal(t, http.StatusCreated, rr.Code)
    assert.Len(t, mockMail.Sent, 1)
    assert.Equal(t, "alice@example.com", mockMail.Sent[0].To)
    assert.Contains(t, mockMail.Sent[0].Subject, "Welcome")
}
```

### 6. Configuration (environment variables)

```go
type Config struct {
    // ... existing fields ...
    SMTPHost     string `envconfig:"SMTP_HOST"     default:"smtp.gmail.com"`
    SMTPPort     int    `envconfig:"SMTP_PORT"     default:"587"`
    SMTPUser     string `envconfig:"SMTP_USER"     required:"true"`
    SMTPPassword string `envconfig:"SMTP_PASSWORD" required:"true"`
    SMTPFrom     string `envconfig:"SMTP_FROM"     required:"true"`
}
```

`.env.example`:

```env
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=sender@gmail.com
SMTP_PASSWORD=your-app-password   # Generate at myaccount.google.com/apppasswords
SMTP_FROM="MyApp <noreply@myapp.com>"
```

> **Popular SMTP providers:** Gmail (port 587), SendGrid (`smtp.sendgrid.net:587`), Mailgun (`smtp.mailgun.org:587`), AWS SES (`email-smtp.<region>.amazonaws.com:587`).

---

## Next Steps

1. **Practice basics** (Ch 1–8) — write small CLI programs
2. **Build a CRUD API** (Ch 12–14) — in-memory store first
3. **Add a real database** (Ch 17–19) — PostgreSQL + migrations
4. **Layer in middleware & auth** (Ch 15, 16, 22)
5. **Add caching & concurrency** (Ch 9, 21)
6. **Structure for production** (Ch 20, 24–27)
7. **Write tests throughout** (Ch 23)

> **Key mindset shift from Laravel:** Go doesn't hide complexity behind magic. No auto-loading, no service container, no facades. You wire everything explicitly, handle every error, and write more code — but you get clarity, performance, and confidence that every piece works exactly as you wrote it.
