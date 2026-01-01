# 🎓 Classes, Function & Operator Overloading

---

## 🔄 Function Overloading

**Function overloading** allows multiple functions with the **same name** but **different parameter lists**.

```cpp
// Same name "AreEqual", different parameter types
bool AreEqual(int a, int b);                    // For integers
bool AreEqual(double a, double b);              // For doubles
bool AreEqual(std::string a, std::string b);    // For strings

// The compiler picks the right one based on arguments!
AreEqual(5, 10);           // Calls int version
AreEqual(3.14, 2.71);      // Calls double version
AreEqual("hi", "bye");     // Calls string version
```

### Rules for Overloading

```cpp
// ✅ Can differ by: number of parameters
void Print(int x);
void Print(int x, int y);

// ✅ Can differ by: types of parameters
void Print(int x);
void Print(double x);

// ✅ Can differ by: order of types
void Print(int x, double y);
void Print(double x, int y);

// ❌ CANNOT differ ONLY by return type!
int Calculate(int x);      // ❌ ERROR if both exist
double Calculate(int x);   // Cannot overload by return type alone
```

### How the Compiler Resolves Overloads

```
Call: AreEqual(1, 2)

Step 1: Look for EXACT match      → AreEqual(int, int) ✓ Found!

If no exact match:
Step 2: Try PROMOTION             → char → int, float → double
Step 3: Try STANDARD CONVERSION   → double → int (narrowing)

If multiple matches with same "rank":
→ ERROR: Ambiguous call!

If no match possible:
→ ERROR: No matching function!
```

### When to Use Function Overloading

```cpp
// ✅ Good: Same semantic meaning, different types
bool AreEqual(int a, int b);
bool AreEqual(std::string a, std::string b);

// ❌ Bad: Different meanings — use different names!
void Process(int data);      // Processes data
void Process(std::string s); // Parses a string — different purpose!
```

> 💡 **Use overloading when the name is semantically significant** across different types.

---

## 🏗️ User-Defined Types — The Big Picture

A **user-defined type** represents a concept in your program.

```
┌─────────────────────────────────────────────────────────────────┐
│                     USER-DEFINED TYPE                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   REPRESENTATION          │        OPERATIONS                  │
│   (Data Members)          │        (Member Functions)          │
│                           │                                    │
│   What data does it       │        What can it DO?             │
│   store?                  │                                    │
│                           │                                    │
│   Examples:               │        Examples:                   │
│   - r, g, b values        │        - GetRed(), SetRed()        │
│   - size, elements        │        - push_back(), at()         │
│                           │                                    │
└─────────────────────────────────────────────────────────────────┘
```

### Key Concepts

| Term | Meaning |
|------|---------|
| **Abstraction** | Focus on essential details, hide complexity |
| **Encapsulation** | Bundle data + operations together |
| **Information Hiding** | Restrict access to internal details |
| **Interface** | Public operations users can perform |

### Why User-Defined Types?

```cpp
// ❌ Without user-defined types: parallel vectors, messy!
std::vector<int> r_values, g_values, b_values;

// ✅ With user-defined types: clean and organized!
std::vector<Color> colors;  // Each Color knows its r, g, b
```

---

## 📦 Classes vs Structs

Both `struct` and `class` create user-defined types. The **only difference** is default visibility:

```cpp
struct Point {
    int x;  // PUBLIC by default
    int y;  // PUBLIC by default
};

class Point {
    int x;  // PRIVATE by default
    int y;  // PRIVATE by default
};
```

### The Problem with Public Data

```cpp
struct Color {
    int r, g, b;  // Public: anyone can access!
};

Color c;
c.r = 999;  // ❌ Invalid! Valid range is 0-255
            // But no error — struct can't protect itself!
```

### Classes Protect Data

```cpp
class Color {
 public:
    void SetR(int value);  // Controlled access
    int GetR() const;
    
 private:
    int r_;  // Hidden from outside
    int g_;
    int b_;
};

// Now we can validate!
void Color::SetR(int value) {
    if (value < 0 || value > 255) {
        throw std::runtime_error("Invalid color value!");
    }
    r_ = value;
}
```

---

## 📁 Class Structure

A class is typically split into two files:

```
color.h          ← Declaration (what it looks like)
color.cc         ← Definition (how it works)
```

### Header File (`color.h`)

```cpp
#ifndef COLOR_H_
#define COLOR_H_

class Color {
 public:
    // === INTERFACE (what users can do) ===
    
    // Constructors
    Color();                        // Default constructor
    Color(int r, int g, int b);     // Parameterized constructor
    
    // Accessors (getters) — marked const!
    int R() const;
    int G() const;
    int B() const;
    
    // Mutators (setters)
    void R(int r);
    void G(int g);
    void B(int b);
    
 private:
    // === IMPLEMENTATION DETAILS (hidden) ===
    
    // Data members (prefixed with underscore)
    int r_;
    int g_;
    int b_;
    
    // Constants shared by all Color objects
    static constexpr int kMaxColorValue = 255;
    static constexpr int kMinColorValue = 0;
    
    // Private helper function
    bool IsValidValue(int value) const;
};

#endif
```

### Source File (`color.cc`)

```cpp
#include "color.h"
#include <stdexcept>

// Default constructor — initializer list!
Color::Color() : r_(0), g_(0), b_(0) {
    // Body can be empty if initializer list does the work
}

// Parameterized constructor
Color::Color(int r, int g, int b) : r_(r), g_(g), b_(b) {
    // Validate after initialization
    if (!IsValidValue(r) || !IsValidValue(g) || !IsValidValue(b)) {
        throw std::runtime_error("Invalid color value!");
    }
}

// Accessor (getter) — const because it doesn't modify state
int Color::R() const {
    return r_;
}

// Mutator (setter) — validates before assigning
void Color::R(int r) {
    if (!IsValidValue(r)) {
        throw std::runtime_error("Invalid color value!");
    }
    r_ = r;
}

// Private helper function
bool Color::IsValidValue(int value) const {
    return value >= kMinColorValue && value <= kMaxColorValue;
}
```

### Scope Resolution Operator (`::`)

```cpp
// This defines Color's R function (not some random R function)
int Color::R() const {
//   ^^^^^^
//   "This R belongs to Color class"
    return r_;
}
```

---

## 🔨 Constructors

Constructors are **special member functions** that initialize objects.

### The Problem: Uninitialized Data

```cpp
Color c;  // Without constructor: r_, g_, b_ are GARBAGE!
std::cout << c.R();  // Could print anything: -4358923, 99999...
```

### Default Constructor

```cpp
// Declaration (in .h)
Color();

// Definition (in .cc) — uses INITIALIZER LIST
Color::Color() : r_(0), g_(0), b_(0) {
    // r_, g_, b_ start with value 0
}

// Usage
Color black;  // Calls default constructor → r=0, g=0, b=0
```

### Parameterized Constructor

```cpp
// Declaration
Color(int r, int g, int b);

// Definition
Color::Color(int r, int g, int b) : r_(r), g_(g), b_(b) {
    if (!IsValidValue(r) || !IsValidValue(g) || !IsValidValue(b)) {
        throw std::runtime_error("Invalid color!");
    }
}

// Usage
Color red(255, 0, 0);      // r=255, g=0, b=0
Color maroon(80, 0, 0);    // r=80, g=0, b=0
```

### ⚠️ Initializer List vs Assignment

```cpp
// ✅ CORRECT: Initializer list (true initialization)
Color::Color() : r_(0), g_(0), b_(0) { }
//             ↑
//             Colon starts the initializer list

// ❌ WRONG: Assignment in body (two-step: undefined → assign)
Color::Color() {
    r_ = 0;  // r_ was garbage, now assigned 0
    g_ = 0;
    b_ = 0;
}
```

### In-Class Member Initializers (C++11)

```cpp
class Color {
 private:
    int r_ = 0;   // Default value if not in initializer list
    int g_ = 0;
    int b_ = 0;
};

// Now constructors can skip members that use defaults
Color::Color(int r) : r_(r) {
    // g_ and b_ automatically get 0 from in-class initializers
}
```

### Constructor Overloading

```cpp
class Color {
 public:
    Color();                        // Default: black
    Color(int r, int g, int b);     // Full RGB
    Color(int gray);                // Grayscale: r=g=b=gray
};

// Usage
Color c1;              // Black (0, 0, 0)
Color c2(255, 0, 0);   // Red
Color c3(128);         // Gray (128, 128, 128)
```

### ⚠️ No Default Constructor?

```cpp
class Color {
 public:
    Color(int r, int g, int b);  // Only parameterized
    // No default constructor!
};

Color c;  // ❌ ERROR! No default constructor

std::vector<Color> colors(100);  // ❌ ERROR! Can't create 100 defaults
```

> 💡 If you define ANY constructor, the compiler stops generating the default one.

---

## 🔐 Accessors & Mutators

### Accessors (Getters) — Read Data

```cpp
// Marked const — promises not to modify the object
int Color::R() const {
    return r_;
}

// Usage
Color c(255, 0, 0);
int red = c.R();  // Gets value: 255
```

### Mutators (Setters) — Write Data

```cpp
// NOT const — it modifies the object
void Color::R(int r) {
    if (!IsValidValue(r)) {
        throw std::runtime_error("Invalid!");
    }
    r_ = r;  // Assigns new value
}

// Usage
Color c;
c.R(255);  // Sets r_ to 255
```

### Why `const` Matters

```cpp
void PrintColor(const Color& c) {
    std::cout << c.R();  // ✅ OK: R() is const
    c.R(100);            // ❌ ERROR: Cannot call non-const on const ref
}
```

---

## 🎭 Operator Overloading

Define what operators mean for your class!

```cpp
Color c1(100, 50, 25);
Color c2(50, 50, 50);

// Without overloading:
c1 == c2;  // ❌ ERROR! Compiler doesn't know how to compare

// With overloading:
c1 == c2;  // ✅ Returns true/false based on YOUR definition
```

### Operator Function Syntax

```cpp
// Operator is a function with special name: operator<symbol>
bool operator==(const Color& lhs, const Color& rhs);
//   ↑         ↑
//   return    name: "operator" + symbol
```

### Member vs Non-Member

| Operator Type | Implement As | Example |
|---------------|--------------|---------|
| Assignment (`=`) | **Member** (required) | `c1 = c2` |
| Compound (`+=`, `-=`) | **Member** (recommended) | `c1 += c2` |
| Symmetric (`==`, `+`, `-`) | **Non-member** | `c1 == c2` |
| Stream (`<<`, `>>`) | **Non-member** (required) | `cout << c1` |

### Why the Difference?

```cpp
// Member operator: left operand is "this" object
class Color {
    Color& operator+=(const Color& rhs);  // c1 += c2
    //                                    // c1 is "this"
};

// Non-member operator: both operands are explicit
bool operator==(const Color& lhs, const Color& rhs);
//              ↑                 ↑
//              explicit          explicit
```

---

## ⚖️ Equality Operator (`==`)

Defined as **non-member** (symmetric operator).

```cpp
// Declaration (outside class, often in header after class)
bool operator==(const Color& lhs, const Color& rhs);

// Definition
bool operator==(const Color& lhs, const Color& rhs) {
    return lhs.R() == rhs.R() &&
           lhs.G() == rhs.G() &&
           lhs.B() == rhs.B();
}

// Usage
Color c1(255, 0, 0);
Color c2(255, 0, 0);
if (c1 == c2) {  // Calls operator==(c1, c2)
    std::cout << "Same color!";
}
```

---

## ➕ Compound Assignment (`+=`)

Defined as **member** (modifies left operand).

```cpp
// Declaration (inside class, under public)
Color& operator+=(const Color& rhs);

// Definition — blends two colors
Color& Color::operator+=(const Color& rhs) {
    r_ = (r_ + rhs.R()) / 2;  // Average
    g_ = (g_ + rhs.G()) / 2;
    b_ = (b_ + rhs.B()) / 2;
    return *this;  // Return self-reference for chaining
}

// Usage
Color c1(100, 100, 100);
Color c2(200, 200, 200);
c1 += c2;  // c1 is now (150, 150, 150)
```

### What is `*this`?

```cpp
// "this" is a pointer to the object that called the function
Color& Color::operator+=(const Color& rhs) {
    // ...
    return *this;  // Dereference to get the object itself
}

// Enables chaining:
c1 += c2 += c3;  // Works because += returns reference
```

---

## ➕ Addition Operator (`+`)

Defined as **non-member** (returns NEW object).

```cpp
// Declaration (outside class)
Color operator+(const Color& lhs, const Color& rhs);

// Definition
Color operator+(const Color& lhs, const Color& rhs) {
    // Create NEW color (don't modify originals)
    Color result;
    result.R((lhs.R() + rhs.R()) / 2);
    result.G((lhs.G() + rhs.G()) / 2);
    result.B((lhs.B() + rhs.B()) / 2);
    return result;
}

// Usage
Color c1(100, 0, 0);
Color c2(0, 100, 0);
Color c3 = c1 + c2;  // c3 is (50, 50, 0), c1 and c2 unchanged
```

---

## 📤 Stream Insertion (`<<`)

Defined as **non-member** (can't modify `std::ostream`).

```cpp
// Declaration
std::ostream& operator<<(std::ostream& os, const Color& c);

// Definition
std::ostream& operator<<(std::ostream& os, const Color& c) {
    os << "RGB(" << c.R() << ", " << c.G() << ", " << c.B() << ")";
    return os;  // ⚠️ MUST return os for chaining!
}

// Usage
Color c(255, 128, 0);
std::cout << c << std::endl;  // Prints: RGB(255, 128, 0)
```

### Why Non-Member?

```cpp
// We CAN'T add members to std::ostream!
// So we must define << as non-member.

std::cout << c;
// ↑         ↑
// ostream   Color
// (not ours) (ours)
```

---

## 📝 Copy Assignment (`=`)

Defined as **member** (required).

```cpp
// Declaration (inside class)
Color& operator=(const Color& rhs);

// Definition
Color& Color::operator=(const Color& rhs) {
    r_ = rhs.r_;  // Member function: can access private!
    g_ = rhs.g_;
    b_ = rhs.b_;
    return *this;
}

// Or use default (memberwise copy)
Color& operator=(const Color& rhs) = default;
```

---

## ➕➖ Prefix vs Postfix Increment/Decrement

```cpp
// PREFIX: ++c (returns modified value)
Color& Color::operator++() {
    r_ = std::min(r_ + 1, 255);
    g_ = std::min(g_ + 1, 255);
    b_ = std::min(b_ + 1, 255);
    return *this;
}

// POSTFIX: c++ (returns OLD value)
Color Color::operator++(int) {  // int is dummy parameter!
    Color old = *this;  // Save old state
    ++(*this);          // Use prefix
    return old;         // Return old state
}
```

---

## 🔑 Key Takeaways

### Function Overloading
- Same name, different parameter lists
- Compiler picks best match
- Cannot differ only by return type

### Classes
- Bundle data + operations
- `class` = private by default, `struct` = public by default
- Use initializer lists in constructors

### Access Control
| Specifier | Meaning |
|-----------|---------|
| `public` | Anyone can access |
| `private` | Only class members can access |

### Operator Overloading Guidelines

| Operator | Implement As | Returns |
|----------|--------------|---------|
| `=` | Member | `T&` (self-reference) |
| `+=`, `-=` | Member | `T&` (self-reference) |
| `++`, `--` (prefix) | Member | `T&` (self-reference) |
| `++`, `--` (postfix) | Member | `T` (old copy) |
| `==`, `!=`, `<` | Non-member | `bool` |
| `+`, `-`, `*` | Non-member | `T` (new object) |
| `<<`, `>>` | Non-member | `stream&` |

