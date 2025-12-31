# 🎓 Control Structures & Functions

---

## 🧮 Expressions

An **expression** is anything that evaluates to a value.

```cpp
// Simple expressions
5              // Literal → evaluates to 5
x              // Variable → evaluates to whatever x contains
x + y          // Compound → evaluates to sum of x and y
x = y + 5      // Assignment is also an expression!
```

---

## ⚙️ Operators

### Types by Number of Operands

```cpp
// UNARY: Acts on ONE operand
-7             // Negative sign
!true          // Logical NOT → false

// BINARY: Acts on TWO operands
5 + 3          // Addition
10 - 4         // Subtraction

// TERNARY: Acts on THREE operands (conditional operator)
int x = (a > b) ? a : b;   // If a > b, x = a, else x = b
//       condition  if-true  if-false
```

### Ternary Operator (Shorthand If)

```cpp
// Long way
if (temperature < 32) {
    message = "freezing";
} else {
    message = "not freezing";
}

// Short way (ternary operator)
message = (temperature < 32) ? "freezing" : "not freezing";
//         └─ condition ─┘     └─ true ─┘   └─ false ─┘
```

---

## 📊 Operator Precedence & Associativity

### Precedence: Which operation happens first?

```cpp
int result = 3 + 4 * 5;    // = 23, NOT 35!
//               ↑
//     Multiplication happens first (higher precedence)

int result = (3 + 4) * 5;  // = 35
//           └──┬──┘
//    Parentheses override precedence
```

### Arithmetic Operators (High → Low Precedence)

```
┌────────────────────────────────────────────┐
│  HIGHEST    +x  -x     (unary plus/minus)  │
│      ↓      *  /  %    (multiply, divide)  │
│  LOWEST     +  -       (add, subtract)     │
└────────────────────────────────────────────┘
```

### Associativity: Same precedence → which direction?

```cpp
// LEFT-TO-RIGHT (arithmetic)
int result = 20 - 15 - 3;  // = 2
//           └──┬──┘
//         5 first, then 5 - 3 = 2

// RIGHT-TO-LEFT (assignment)
int a, b;
a = b = 5;      // b gets 5 first, then a gets b's value
//      ↑       // Both a and b are now 5
```

### Relational & Logical Operators

```
┌─────────────────────────────────────────────────┐
│  HIGHEST    !           (logical NOT)           │
│      ↓      <  <=  >  >= (comparisons)          │
│      ↓      ==  !=      (equality)              │
│      ↓      &&          (logical AND)           │
│  LOWEST     ||          (logical OR)            │
└─────────────────────────────────────────────────┘
```

---

## ⚠️ Errors in Expressions

### Integer Overflow

```cpp
#include <limits>

int max_int = std::numeric_limits<int>::max();  // 2147483647
std::cout << max_int << std::endl;              // 2147483647

max_int = max_int + 1;                          // OVERFLOW!
std::cout << max_int << std::endl;              // -2147483648 (wrapped around!)
```

> ⚠️ **Overflow** happens when a calculation produces a result too large for the type. The value "wraps around" unexpectedly!

### Floating-Point Imprecision

```cpp
double result = 0.15 + 0.15;
std::cout << result << std::endl;  // 0.29999999999... NOT 0.3!
```

**Why?** Floating-point numbers are approximations. Some decimals can't be exactly represented in binary.

```cpp
// ❌ NEVER compare floats directly
if (a == b) { ... }  // Dangerous!

// ✅ Compare with tolerance
double tolerance = 0.0001;
if (std::abs(a - b) < tolerance) { ... }  // Safe!
```

---

## 📝 Statements

A **statement** is a complete command that ends with `;`

```cpp
// Simple statements
int x = 5;                           // Declaration
x = x + 1;                           // Assignment
std::cout << x << std::endl;         // Output

// Empty statement (just a semicolon)
;                                    // Does nothing, but valid

// Compound statement (block) - NO semicolon after }
{
    int a = 1;
    int b = 2;
    std::cout << a + b << std::endl;
}
```

---

## 🔀 Branching: if Statement

### Basic Structure

```cpp
if (condition) {
    // Executes if condition is TRUE
} else {
    // Executes if condition is FALSE
}
```

### Example

```cpp
int temperature = 28;

if (temperature <= 32) {
    std::cout << "Freezing warning should be issued." << std::endl;
} else {
    std::cout << "Freezing warning should not be issued." << std::endl;
}
```

### else-if Chain

```cpp
int score = 85;

if (score >= 90) {
    std::cout << "Grade: A" << std::endl;
} else if (score >= 80) {
    std::cout << "Grade: B" << std::endl;
} else if (score >= 70) {
    std::cout << "Grade: C" << std::endl;
} else {
    std::cout << "Grade: F" << std::endl;
}
```

### ⚠️ Scope Inside Branches

```cpp
double x = -5.5;

// ❌ WRONG: absx only exists inside the if/else blocks
if (x < 0) {
    double absx = -x;
} else {
    double absx = x;
}
// std::cout << absx;  // ERROR: absx doesn't exist here!

// ✅ CORRECT: Declare outside, modify inside
double absx = x;
if (x < 0) {
    absx = -x;
}
std::cout << absx << std::endl;  // Works!
```

### Style: Always Use Braces

```cpp
// ✅ GOOD: Always use curly braces
if (condition) {
    DoSomething();
} else {
    DoSomethingElse();
}

// ❌ AVOID: No braces (error-prone)
if (condition)
    DoSomething();
```

---

## 🔀 Branching: switch Statement

Use `switch` when comparing ONE value against MULTIPLE constants.

```cpp
int day = 3;

switch (day) {
    case 1: {
        std::cout << "Monday" << std::endl;
        break;  // EXIT the switch
    }
    case 2: {
        std::cout << "Tuesday" << std::endl;
        break;
    }
    case 3: {
        std::cout << "Wednesday" << std::endl;
        break;
    }
    default: {
        std::cout << "Unknown day" << std::endl;
        break;
    }
}
```

### ⚠️ Don't Forget `break`!

```cpp
int op_code = 1;

switch (op_code) {
    case 0: {
        std::cout << "Zero" << std::endl;
        break;
    }
    case 1: {
        std::cout << "One" << std::endl;
        // NO BREAK! Falls through to case 2
    }
    case 2: {
        std::cout << "Two" << std::endl;
        break;
    }
}
// Output: "One" AND "Two" (fall-through!)
```

### Switch Rules

| Rule | Explanation |
|------|-------------|
| Switch on `int`, `char`, or `enum` only | Can't switch on `double` or `string` |
| Case labels must be constants | `case x:` is INVALID, use `case 5:` |
| Case labels must be unique | Can't have two `case 1:` |
| Always end cases with `break` | Unless fall-through is intentional |
| Always include `default` | Handle unexpected values |

---

## 🔁 Iteration: while Loop

Repeat **while** a condition is true. Use when you **don't know** how many times to repeat.

```cpp
int count = 1;

while (count <= 5) {
    std::cout << count << std::endl;
    count++;  // MUST move toward termination!
}
// Output: 1 2 3 4 5
```

### Example: Collatz Sequence

```cpp
int x = 7;

while (x != 1) {
    std::cout << x << " ";
    if (x % 2 == 0) {
        x = x / 2;       // If even, divide by 2
    } else {
        x = 3 * x + 1;   // If odd, multiply by 3 and add 1
    }
}
std::cout << x << std::endl;  // Always ends at 1
// Output: 7 22 11 34 17 52 26 13 40 20 10 5 16 8 4 2 1
```

---

## 🔁 Iteration: do-while Loop

Same as `while`, but **guaranteed to execute at least once**.

```cpp
int x = -1;

do {
    std::cout << "Enter a positive number: ";
    std::cin >> x;
} while (x <= 0);  // Keep asking until positive
//       ↑
//  Condition checked AFTER first execution

std::cout << "You entered: " << x << std::endl;
```

### while vs do-while

```
WHILE:                          DO-WHILE:
┌─────────────┐                 ┌─────────────┐
│ Check first │                 │ Execute     │
└──────┬──────┘                 └──────┬──────┘
       ↓                               ↓
┌─────────────┐                 ┌─────────────┐
│ Execute     │                 │ Check after │
└─────────────┘                 └─────────────┘

May run 0 times                 Runs at least 1 time
```

---

## 🔁 Iteration: for Loop

Use when you **know** how many times to repeat.

### Structure

```cpp
for (initialization; condition; step) {
    // Loop body
}
```

### Example

```cpp
for (int i = 1; i <= 5; i++) {
    std::cout << i << " squared = " << i * i << std::endl;
}
```

### How It Works

```
┌─────────────────────────────────────────────────────────────┐
│  for (int i = 1;  i <= 5;  i++)                             │
│       └───┬───┘   └──┬──┘  └─┬─┘                            │
│           │          │       │                               │
│      1. INIT     2. CHECK  4. STEP                          │
│      (once)      (each)    (after body)                     │
│                      │                                       │
│                      ↓                                       │
│               3. EXECUTE BODY                                │
└─────────────────────────────────────────────────────────────┘

Execution order: INIT → CHECK → BODY → STEP → CHECK → BODY → STEP → ...
```

### Loop Variable Scope

```cpp
for (int i = 0; i < 10; i++) {
    std::cout << i << std::endl;  // i is visible here
}
// std::cout << i;  // ❌ ERROR: i doesn't exist here!
```

---

## 🎮 Loop Control: break & continue

### `break` — Exit the loop entirely

```cpp
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        break;  // Exit loop when i reaches 5
    }
    std::cout << i << " ";
}
// Output: 1 2 3 4
```

### `continue` — Skip to next iteration

```cpp
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        continue;  // Skip printing 3
    }
    std::cout << i << " ";
}
// Output: 1 2 4 5
```

```
BREAK:                          CONTINUE:
┌─────────────┐                 ┌─────────────┐
│ Exit loop   │                 │ Skip rest   │
│ completely  │                 │ of this     │
│             │                 │ iteration   │
└──────┬──────┘                 └──────┬──────┘
       ↓                               ↓
   After loop                    Next iteration
```

---

## 📦 Functions: Why?

Functions let you:
- **Organize** code into logical pieces
- **Reuse** code without copying
- **Name** a block of code descriptively
- **Test** pieces independently

---

## ✍️ Writing Functions Well

### 1. Keep Functions Small

```cpp
// ❌ BAD: 100+ lines doing multiple things
void DoEverything() {
    // ... endless code ...
}

// ✅ GOOD: Small, focused functions
void ReadInput() { ... }
void ProcessData() { ... }
void DisplayResults() { ... }
```

> 💡 **Rule:** If a function exceeds ~40 lines, it's probably too long.

### 2. Do ONE Thing

```cpp
// ❌ BAD: Does multiple things
void ProcessAndPrint(int x) {
    int result = x * x;
    std::cout << result << std::endl;
    SaveToFile(result);
}

// ✅ GOOD: Each function does one thing
int Square(int x) {
    return x * x;
}

void Print(int value) {
    std::cout << value << std::endl;
}
```

### 3. Use Descriptive Names

```cpp
// ❌ BAD: What does this do?
int f(int a, int b) { ... }

// ✅ GOOD: Clear purpose
int CalculateRectangleArea(int width, int height) { ... }
```

### 4. Minimize Arguments

```cpp
// Best: 0 arguments (if possible)
int GetCurrentTime();

// Good: 1 argument
int Square(int x);

// Okay: 2 arguments
int Add(int a, int b);

// Avoid: 3+ arguments
// If you need many arguments, consider grouping them into a struct
```

### 5. Don't Repeat Yourself (DRY)

```cpp
// ❌ BAD: Same code in multiple places
int main() {
    // Calculate area here...
    int area1 = width1 * height1;
    
    // Same calculation elsewhere...
    int area2 = width2 * height2;
}

// ✅ GOOD: Reusable function
int CalculateArea(int width, int height) {
    return width * height;
}

int main() {
    int area1 = CalculateArea(width1, height1);
    int area2 = CalculateArea(width2, height2);
}
```

---

## 🔧 Function Syntax

### Declaration vs Definition

```cpp
// DECLARATION: Tells compiler "this function exists"
// Goes in HEADER FILE (.h)
int Square(int x);  // Note the semicolon!

// DEFINITION: The actual implementation
// Goes in SOURCE FILE (.cc or .cpp)
int Square(int x) {
    return x * x;
}
```

### Anatomy of a Function

```cpp
int Square(int x) {
    return x * x;
}
│     │      │       │
│     │      │       └── Body: statements to execute
│     │      └────────── Parameter: input to the function
│     └───────────────── Name: how to call it
└─────────────────────── Return type: what it gives back
```

### void Functions (No Return Value)

```cpp
void PrintGreeting(std::string name) {
    std::cout << "Hello, " << name << "!" << std::endl;
    // No return statement needed
}
```

---

## 📁 File Organization

### Header File (.h) — Declarations

```cpp
// my_math.h
#ifndef MY_MATH_H_    // Header guard (prevents double-include)
#define MY_MATH_H_

// Declarations only (no definitions!)
int Square(int x);
int Add(int a, int b);
double Average(double a, double b);

#endif  // MY_MATH_H_
```

### Source File (.cc) — Definitions

```cpp
// my_math.cc
#include "my_math.h"  // Include the matching header

// Definitions (implementation)
int Square(int x) {
    return x * x;
}

int Add(int a, int b) {
    return a + b;
}

double Average(double a, double b) {
    return (a + b) / 2.0;
}
```

### Using the Functions

```cpp
// main.cc
#include <iostream>
#include "my_math.h"  // Now we can use Square, Add, Average

int main() {
    int result = Square(5);
    std::cout << "5 squared = " << result << std::endl;
    return 0;
}
```

### File Structure

```
┌─────────────────────────────────────────────────────────────┐
│                     PROJECT STRUCTURE                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  my_math.h ──────────► Declarations (how to use)             │
│       ↓                                                      │
│  my_math.cc ─────────► Definitions (implementation)          │
│       ↓                                                      │
│  main.cc ────────────► Uses the functions                    │
│       │                                                      │
│       └──► #include "my_math.h"                              │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔄 How Function Calls Work

```cpp
int Square(int x) {
    return x * x;
}

int main() {
    int result = Square(11);
    //              │
    //    1. Argument 11 initializes parameter x
    //    2. Control transfers to Square
    //    3. Square executes: 11 * 11 = 121
    //    4. Return value 121 goes back to main
    //    5. result is initialized with 121
}
```

```
main()                          Square(int x)
┌──────────────┐               ┌──────────────┐
│ Square(11)   │──── 11 ──────►│ x = 11       │
│              │               │ return x * x │
│ result = 121 │◄─── 121 ─────│ (121)        │
└──────────────┘               └──────────────┘
```

---

## 📋 Style Guide Summary

| Rule | Example |
|------|---------|
| Function names: CamelCase | `CalculateArea()` |
| Max ~40 lines per function | Break up long functions |
| Use braces with if/else/loops | `if (x) { ... }` |
| Comment non-trivial functions | In header file |
| One declaration per line | `int x;` not `int x, y;` |

---

## 🔑 Key Takeaways

1. **Expressions** evaluate to values; **statements** are commands

2. **Precedence** determines which operations happen first

3. **Beware** of integer overflow and floating-point imprecision

4. **if/else** for two-way decisions, **switch** for multiple constants

5. **while** when iterations unknown, **for** when iterations known

6. **break** exits loops, **continue** skips to next iteration

7. Functions should be **small**, do **one thing**, have **descriptive names**

8. **Header files** (.h) = declarations, **Source files** (.cc) = definitions

