# 🎓 Objects, Types, Values & Type Conversions

---

## 🧱 Basic Terminology

### The Core Concepts

```
┌─────────────────────────────────────────────────────────────────────┐
│                     HOW C++ ORGANIZES DATA                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   TYPE ──────► Defines what values & operations are allowed         │
│                                                                     │
│   OBJECT ────► A box in memory that holds a value of some type      │
│                                                                     │
│   VALUE ─────► The actual data stored in the object                 │
│                                                                     │
│   VARIABLE ──► A named object (an object with a name)               │
│                                                                     │
│   DECLARATION► A statement that gives a name to an object           │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Think of It Like Boxes

```cpp
// Each variable is like a labeled box that can hold specific items

int a = 7;       // Box labeled "a" - can hold integers - currently holds 7
int b = 42;      // Box labeled "b" - can hold integers - currently holds 42
char c = 'X';    // Box labeled "c" - can hold characters - currently holds 'X'
double d = 3.14; // Box labeled "d" - can hold decimals - currently holds 3.14
```

```
┌─────────┐   ┌─────────┐   ┌───┐   ┌─────────────┐
│    7    │   │   42    │   │ X │   │    3.14     │
│   int   │   │   int   │   │chr│   │   double    │
│   "a"   │   │   "b"   │   │"c"│   │    "d"      │
└─────────┘   └─────────┘   └───┘   └─────────────┘
     ↑             ↑          ↑            ↑
  Same size     Same size   Smaller     Different size
  (both int)    (both int)  (char)      (double)
```

> 💡 **Key insight:** Different types = different box sizes = different range of values they can store.

---

## 📦 Primitive Built-in Types

These are the basic building blocks of C++. For most programs, you only need these five:

| Type | What It Stores | Example |
|------|----------------|---------|
| `bool` | true or false | `bool is_valid = true;` |
| `char` | Single character | `char grade = 'A';` |
| `int` | Whole numbers | `int age = 25;` |
| `double` | Decimal numbers | `double price = 19.99;` |
| `string` | Text (we'll cover later) | `string name = "Alex";` |

### Type Categories

```
┌─────────────────────────────────────────────────────────┐
│                   ARITHMETIC TYPES                      │
├─────────────────────────┬───────────────────────────────┤
│     INTEGRAL TYPES      │     FLOATING POINT TYPES      │
│  (whole number-ish)     │     (decimal numbers)         │
├─────────────────────────┼───────────────────────────────┤
│  bool   char   int      │     float   double            │
│                         │     (single) (double          │
│                         │              precision)       │
└─────────────────────────┴───────────────────────────────┘
```

---

## ✏️ Literals (How to Write Values)

A **literal** is a fixed value written directly in code.

```cpp
// Boolean literals
bool a = true;     // Only two options: true or false
bool b = false;

// Character literals - use SINGLE quotes
char letter = 'A';      // ✅ Correct
char digit = '9';       // ✅ Correct
char newline = '\n';    // ✅ Special character (line break)
char tab = '\t';        // ✅ Special character (tab)
// char wrong = "A";    // ❌ WRONG - double quotes are for strings!

// Integer literals
int decimal = 42;       // Normal base-10 number
int binary = 0b101010;  // Binary (base-2) - prefix with 0b
int octal = 052;        // Octal (base-8) - prefix with 0
int hex = 0x2A;         // Hexadecimal (base-16) - prefix with 0x
// All four above equal 42!

// Floating point literals
double pi = 3.14159;    // Default: double
float pi_f = 3.14159f;  // Add 'f' suffix for float
```

> 💡 **Remember:** `'A'` (single quotes) = character, `"A"` (double quotes) = string. They are NOT the same!

---

## 🏷️ Variables: More Than Just a Name

A variable has several attributes:

| Attribute | What It Is |
|-----------|------------|
| **Name** | The identifier you use in code |
| **Type** | What kind of values it can hold |
| **Value** | The actual data stored |
| **Address** | Where it lives in memory |
| **Scope** | Where the name is visible |
| **Lifetime** | How long the variable exists |

### Naming Rules

```cpp
// ✅ VALID names
int age;
int player_score;      // Use underscores between words
int playerScore;       // CamelCase (we prefer underscores)
int _internal;         // Valid but AVOID - reserved for system use

// ❌ INVALID names
int 2fast;             // Cannot start with digit
int my-var;            // No hyphens allowed
int my var;            // No spaces allowed
int int;               // Cannot use keywords
```

### Naming Convention (Google Style)

```cpp
// Local variables: lowercase with underscores
int player_health = 100;
double average_score = 85.5;

// Constants: start with 'k', then CamelCase
const int kMaxPlayers = 4;
const double kPi = 3.14159;
```

---

## 👁️ Scope: Where Names Are Visible

A **scope** is a region of code (usually inside `{ }`). Names are only visible from their declaration to the end of their scope.

```cpp
#include <iostream>

int global_var = 100;  // Global scope - visible everywhere

int main() {
    int a = 1;         // Visible from here to end of main
    
    {                  // Inner scope begins
        int b = 2;     // Only visible inside this block
        int a = 99;    // This 'a' HIDES the outer 'a'
        
        std::cout << a << std::endl;  // Prints: 99 (inner a)
        std::cout << b << std::endl;  // Prints: 2
    }                  // Inner scope ends - 'b' and inner 'a' gone
    
    std::cout << a << std::endl;      // Prints: 1 (outer a is back)
    // std::cout << b << std::endl;   // ❌ ERROR: b doesn't exist here
    
    return 0;
}
```

```
┌─────────────────────────────────────────────────────┐
│ main() scope                                        │
│   int a = 1;  ←── visible throughout main           │
│   ┌─────────────────────────────────────────────┐   │
│   │ inner scope                                 │   │
│   │   int b = 2;  ←── only visible here         │   │
│   │   int a = 99; ←── hides outer 'a'           │   │
│   └─────────────────────────────────────────────┘   │
│   // 'b' is gone, outer 'a' is visible again        │
└─────────────────────────────────────────────────────┘
```

### Best Practice: Scope Variables Tightly

```cpp
// ❌ BAD: Variables declared far from use
int main() {
    int a, b, c;           // What are these for?
    
    // ... 50 lines later ...
    
    a = 5;
    b = 10;
    
    // ... 20 lines later ...
    
    c = a + b;
}

// ✅ GOOD: Declare close to first use, in tightest scope
int main() {
    int a = 5;             // Declared and initialized where needed
    
    {
        int b = 10;        // Only used in this block
        // use b here...
    }
    
    int c = a + 10;        // Declared right when needed
}
```

---

## 📜 Declarations

A **declaration** introduces a name and specifies its type.

### Declaration Structure

```
┌──────────────┬───────────┬────────────┬─────────────────┐
│  [specifier] │ base_type │ declarator │ [= initializer] │
│  (optional)  │ (required)│ (name)     │   (optional*)   │
└──────────────┴───────────┴────────────┴─────────────────┘

*But ALWAYS initialize primitive types!
```

```cpp
// Examples broken down:
int x = 5;
//  │   │   └─ initializer (= 5)
//  │   └───── declarator (x)
//  └───────── base type (int)

const double kPi = 3.14159;
//│     │      │      └─ initializer
//│     │      └──────── declarator
//│     └─────────────── base type
//└───────────────────── specifier (const)
```

### Constants: `const` and `constexpr`

```cpp
// const - value cannot change after initialization
const int kMaxScore = 100;
// kMaxScore = 200;          // ❌ ERROR: cannot modify const

// constexpr - value must be known at compile time
constexpr double kPi = 3.14159;      // ✅ Value known at compile time
constexpr int kSquare = 5 * 5;       // ✅ Computed at compile time

// When to use which?
// - Use constexpr when value is truly constant (like pi, max size)
// - Use const when value is set once but determined at runtime
```

### Avoid Magic Numbers

```cpp
// ❌ BAD: What is 7? Why 7?
int result = score * 7;

// ✅ GOOD: Clear meaning
constexpr int kBonusMultiplier = 7;  // Explained with a name
int result = score * kBonusMultiplier;
```

---

## 🎬 Initialization vs Assignment

These are **different operations** — understanding this is crucial!

### Initialization = Setting the First Value

```cpp
int a = 5;    // Initialization: 'a' starts life with value 5
//      └──── happens DURING declaration
```

### Assignment = Changing to a New Value

```cpp
int a = 5;    // Initialization
a = 10;       // Assignment: 'a' now holds 10
//└────────── happens AFTER declaration (no type specified)
```

### The Box Analogy

```
INITIALIZATION:                    ASSIGNMENT:
┌─────────┐                       ┌─────────┐     ┌─────────┐
│  empty  │  ──put 5 in──►        │    5    │ ──► │   10    │
│   box   │                       │ (take   │     │ (put    │
└─────────┘                       │  out 5) │     │  in 10) │
                                  └─────────┘     └─────────┘
  "Give box its                    "Replace old value
   first coin"                      with new coin"
```

### ⚠️ DANGER: Uninitialized Variables

```cpp
// ❌ VERY BAD: Uninitialized variable
int a;              // What's in 'a'? GARBAGE! Unknown value!
std::cout << a;     // Could print anything: 0, -234234, 999...

// This is like grabbing a random box off a shelf
// Someone else might have left something in it!

// ✅ ALWAYS initialize
int a = 0;          // Now we KNOW 'a' is 0
```

> ⚠️ **Critical Rule:** ALWAYS initialize variables of primitive types. Uninitialized variables cause bugs that are extremely hard to find!

---

## 🔄 Type Conversions

Sometimes we need to convert values from one type to another.

### Two Directions

```
┌─────────────────────────────────────────────────────────────┐
│                    TYPE CONVERSIONS                         │
├─────────────────────────────┬───────────────────────────────┤
│     WIDENING (Safe)         │     NARROWING (Risky)         │
│     Small → Big             │     Big → Small               │
├─────────────────────────────┼───────────────────────────────┤
│  int → double               │  double → int                 │
│  char → int                 │  int → char                   │
│  float → double             │  double → float               │
│                             │                               │
│  ✅ Value preserved         │  ⚠️ Value may be lost!        │
└─────────────────────────────┴───────────────────────────────┘
```

### The Box Analogy

```
WIDENING: Small box → Big box (always fits!)
┌───┐          ┌─────────┐
│ 5 │  ─────►  │    5    │     ✅ No problem
└───┘          └─────────┘
 int            double

NARROWING: Big box → Small box (might not fit!)
┌─────────┐          ┌───┐
│  3.99   │  ─────►  │ 3 │     ⚠️ Lost the .99!
└─────────┘          └───┘
  double              int
```

---

## 🔮 Implicit Type Conversion (Coercion)

The compiler automatically converts types when needed.

### In Mixed-Mode Expressions

```cpp
// Narrower type is promoted to wider type
double result = 3.14 + 8;
//                     └── int 8 is converted to double 8.0
//              └──────── 3.14 + 8.0 = 11.14 (double)
```

### In Assignment/Initialization

```cpp
// Value is converted to match the left side
int i = 9.0 / 5.0;     // 1.8 → truncated to 1 (not rounded!)
//          └──────────── double result = 1.8
//      └──────────────── converted to int = 1

double d = 9 / 5;      // Integer division first!
//         └───────────── int / int = 1 (not 1.8!)
//     └───────────────── then 1 → 1.0
```

> ⚠️ **Watch out:** `9 / 5` = 1 (integer division), NOT 1.8!

---

## 🎯 Explicit Type Conversion (Casting)

When YOU want to control the conversion.

### Syntax: `static_cast<type>(value)`

```cpp
int a = 9;
int b = 5;

// ❌ Problem: integer division
double wrong = a / b;           // = 1.0 (9/5 = 1, then → 1.0)

// ✅ Solution: cast one operand to double
double correct = static_cast<double>(a) / b;  // = 1.8
//               └────────────────────────────── 9.0 / 5
//               mixed mode: 5 promoted to 5.0 → 9.0/5.0 = 1.8
```

### More Examples

```cpp
// Convert char to its ASCII value
char letter = 'A';
int ascii = static_cast<int>(letter);    // ascii = 65

// Convert double to int (truncates!)
double price = 19.99;
int dollars = static_cast<int>(price);   // dollars = 19 (not 20!)

// Make integer division explicit
int x = 7, y = 2;
int quotient = x / y;                    // = 3 (implicit integer division)
double precise = static_cast<double>(x) / y;  // = 3.5
```

---

## ✅ Safe vs ⚠️ Unsafe Conversions

### Safe Conversions (Widening)

| From | To | Why Safe |
|------|----|----|
| `bool` → `int` | true=1, false=0 | Value preserved |
| `char` → `int` | 'A'=65 | Value preserved |
| `int` → `double` | 42 → 42.0 | Value preserved |
| `float` → `double` | More precision | Value preserved |

### Unsafe Conversions (Narrowing)

| From | To | What Happens |
|------|----|----|
| `double` → `int` | 3.99 → 3 | Decimal truncated! |
| `int` → `char` | 1000 → ? | Overflow! |
| `double` → `float` | Loses precision | Rounding errors |

```cpp
// Examples of unsafe conversions
double big = 1.5e25;          // A very large number
int trouble = big;            // ❌ Result is garbage!

double pi = 3.14159265359;
float less_precise = pi;      // Some precision lost

int large = 1000;
char overflow = large;        // ❌ char can only hold up to 127!
```

---

## 🔑 Key Takeaways

1. **Type determines everything** — what values fit, what operations work

2. **Always initialize** primitive type variables — uninitialized = undefined bugs

3. **Scope tightly** — declare variables close to first use

4. **Watch integer division** — `9/5 = 1`, not `1.8`

5. **Use `static_cast`** to make conversions explicit and clear

6. **Narrowing is risky** — big → small can lose data

