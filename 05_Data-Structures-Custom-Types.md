# 🎓 Data Structures & Custom Types

---

## 🤔 What Are Data Structures?

A **data structure** is a way to organize and store data so it can be accessed and modified efficiently.

Think of it like organizing your belongings:
- **Array/Vector** → A row of lockers (numbered slots)
- **Map** → A dictionary (look up by word, get definition)
- **Struct** → A form with different fields (name, age, address)

### Built-in vs Custom Types

```
┌─────────────────────────────────────────────────────────────────┐
│                         C++ TYPES                               │
├─────────────────────────────┬───────────────────────────────────┤
│      PRIMITIVE TYPES        │      COMPOUND/CUSTOM TYPES        │
│      (built-in)             │      (built from primitives)      │
├─────────────────────────────┼───────────────────────────────────┤
│  int, double, char, bool    │  std::vector, std::string         │
│                             │  std::map, arrays, struct         │
│  Store ONE value            │  Store MULTIPLE values or         │
│                             │  group DIFFERENT types            │
└─────────────────────────────┴───────────────────────────────────┘
```

---

## 📦 std::vector — Dynamic Array

A **vector** is a resizable collection of elements of the same type.

```cpp
#include <vector>

// Creating vectors
std::vector<int> empty_vec;                    // Empty vector
std::vector<int> nums{1, 2, 3, 4, 5};          // With initial values
std::vector<double> prices(10);                // 10 elements, all 0.0
std::vector<int> ones(5, 1);                   // 5 elements, all 1
```

### Vector Memory Layout

```
std::vector<int> v{1, 1, 2, 3, 5};

┌──────────────────────────────────────────┐
│  v (knows its size!)                     │
├──────────────────────────────────────────┤
│  size: 5                                 │
│  ┌─────┬─────┬─────┬─────┬─────┐        │
│  │  1  │  1  │  2  │  3  │  5  │        │
│  └─────┴─────┴─────┴─────┴─────┘        │
│  index: 0     1     2     3     4        │
└──────────────────────────────────────────┘
```

### Accessing Elements

```cpp
std::vector<int> v{10, 20, 30, 40, 50};

// Two ways to access elements
v[2];       // Returns 30 — NO bounds checking (dangerous!)
v.at(2);    // Returns 30 — WITH bounds checking (safe!)

// ✅ ALWAYS prefer .at() — catches out-of-bounds errors
v.at(100);  // Throws error: out of range!
v[100];     // ❌ Silent bug — accesses invalid memory!
```

### Growing a Vector

```cpp
std::vector<int> v;  // Empty: size = 0

v.push_back(10);     // v = {10}         size = 1
v.push_back(20);     // v = {10, 20}     size = 2
v.push_back(30);     // v = {10, 20, 30} size = 3

std::cout << v.size();  // 3
```

### Iterating Through a Vector

```cpp
std::vector<int> nums{1, 2, 3, 4, 5};

// Method 1: Index-based loop
for (size_t i = 0; i < nums.size(); ++i) {
    std::cout << nums.at(i) << " ";
}

// Method 2: Range-based for loop (preferred!)
for (int num : nums) {
    std::cout << num << " ";
}
// Output: 1 2 3 4 5
```

### Example: Find Even Numbers

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> nums{1, 2, 3, 4, 5, 6};
    std::vector<int> evens;  // Empty vector
    
    for (int num : nums) {
        if (num % 2 == 0) {
            evens.push_back(num);  // Add to evens
        }
    }
    
    // Print evens: 2 4 6
    for (int e : evens) {
        std::cout << e << " ";
    }
}
```

---

## 📝 std::string — Text Handling

A **string** is a sequence of characters with useful operations.

```cpp
#include <string>

std::string greeting = "Hello";
std::string name = "World";

// Concatenation
std::string message = greeting + ", " + name + "!";
// message = "Hello, World!"

// Size
std::cout << message.size();  // 13

// Access individual characters
char first = message.at(0);   // 'H'
char last = message.at(12);   // '!'
```

### Useful String Operations

```cpp
std::string text = "Hello, World!";

// Substring: substr(start_index, length)
std::string hello = text.substr(0, 5);     // "Hello"
std::string world = text.substr(7, 5);     // "World"

// Find
size_t pos = text.find("World");           // pos = 7
size_t not_found = text.find("xyz");       // not_found = std::string::npos

// Iterate through characters
for (char c : text) {
    std::cout << c << " ";
}
// H e l l o ,   W o r l d !
```

### Example: Remove Punctuation

```cpp
std::string input = "Hello, world!";
std::string clean;

for (char c : input) {
    if (c != '.' && c != ',' && c != '!') {
        clean += c;  // Append character
    }
}

std::cout << clean;  // "Hello world"
```

---

## 📊 2D Vectors — Grids & Matrices

A **2D vector** is a vector of vectors — like a spreadsheet or game board.

```cpp
#include <vector>

// Vector of vectors = 2D structure
std::vector<std::vector<int>> grid;
```

### Creating a 2D Vector

```cpp
// Method 1: Push back rows one by one
std::vector<std::vector<int>> grid;
grid.push_back({1, 2, 3});    // Row 0
grid.push_back({4, 5, 6});    // Row 1
grid.push_back({7, 8, 9});    // Row 2

// Method 2: Pre-allocate (better for known sizes!)
unsigned int height = 3;
unsigned int width = 4;
std::vector<std::vector<int>> matrix(height, std::vector<int>(width, 0));
//                                   └─ 3 rows ─┘  └─ each row: 4 zeros ─┘
```

### 2D Vector Layout

```
std::vector<std::vector<int>> grid = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

        col 0   col 1   col 2
       ┌───────┬───────┬───────┐
row 0  │   1   │   2   │   3   │  ← grid.at(0)
       ├───────┼───────┼───────┤
row 1  │   4   │   5   │   6   │  ← grid.at(1)
       ├───────┼───────┼───────┤
row 2  │   7   │   8   │   9   │  ← grid.at(2)
       └───────┴───────┴───────┘

grid.at(1).at(2) = 6   // Row 1, Column 2
```

### Accessing 2D Elements

```cpp
// Get element at row 1, column 2
int value = grid.at(1).at(2);  // = 6

// Modify element
grid.at(0).at(0) = 100;  // Top-left is now 100

// Get entire row
std::vector<int> row = grid.at(1);  // {4, 5, 6}

// Get row size (number of columns)
size_t num_cols = grid.at(0).size();  // 3

// Ask a row for its size
size_t row_size = grid.at(1).size();  // 3
```

### Iterating Through 2D Vector

```cpp
std::vector<std::vector<int>> grid = {
    {1, 2, 3},
    {4, 5, 6}
};

// Nested loops
for (size_t row = 0; row < grid.size(); ++row) {
    for (size_t col = 0; col < grid.at(row).size(); ++col) {
        std::cout << grid.at(row).at(col) << " ";
    }
    std::cout << std::endl;  // New line after each row
}

// Output:
// 1 2 3
// 4 5 6
```

### Pre-allocation Example

```cpp
// Known dimensions: 4 rows, 2 columns, all zeros
unsigned int height = 4;
unsigned int width = 2;

std::vector<std::vector<int>> vect(height, std::vector<int>(width, 0));

// Result:
// 0 0
// 0 0
// 0 0
// 0 0

// Access and modify
vect.at(2).at(1) = 99;  // Row 2, Col 1 is now 99
```

> 💡 **Tip:** Pre-allocate when you know the size — it's faster than repeated push_back!

---

## 🗺️ std::map — Key-Value Pairs

A **map** associates keys with values — like a dictionary or phone book.

```cpp
#include <map>

// Map: string → long (name → phone number)
std::map<std::string, long> phone_book;
```

### Adding and Accessing Entries

```cpp
std::map<std::string, long> phone_book;

// Add entries using subscript
phone_book["Alice"] = 5551234567;
phone_book["Bob"] = 5559876543;

// Access value by key
long alice_num = phone_book["Alice"];  // 5551234567

// ⚠️ WARNING: Subscript creates entry if key doesn't exist!
long unknown = phone_book["Charlie"];  // Creates entry with value 0!
```

### Safe Access with .at()

```cpp
std::map<std::string, int> scores;
scores["Alice"] = 95;

// ✅ Safe: throws error if key not found
int alice = scores.at("Alice");  // 95
int bob = scores.at("Bob");      // ERROR: out of range!

// Use .at() for lookups, subscript for insertion
```

### Inserting with .insert()

```cpp
#include <map>
#include <utility>  // for std::pair

std::map<std::string, int> ages;

// Insert using pair
ages.insert(std::pair<std::string, int>("Alice", 25));
ages.insert({"Bob", 30});  // Shorthand

// Check if insert succeeded (keys must be unique!)
auto result = ages.insert({"Alice", 99});  // Alice already exists!
if (result.second == false) {
    std::cout << "Key already exists!" << std::endl;
}
```

### Iterating Through a Map

```cpp
std::map<std::string, int> scores = {
    {"Alice", 95},
    {"Bob", 87},
    {"Charlie", 92}
};

// Range-based for loop
for (auto& entry : scores) {
    std::cout << entry.first << ": " << entry.second << std::endl;
}

// Output (sorted by key automatically!):
// Alice: 95
// Bob: 87
// Charlie: 92
```

> 💡 **Note:** Maps keep entries sorted by key automatically!

---

## 🔧 struct — Custom Types

A **struct** groups different types of data together under one name.

### Defining a Struct

```cpp
// Define a new type called "Contact"
struct Contact {
    std::string first_name;   // Data member
    std::string last_name;    // Data member
    long phone_number;        // Data member
    std::string email;        // Data member
};  // ← Don't forget the semicolon!
```

### Creating and Using Struct Objects

```cpp
// Create a Contact object
Contact person;

// Access members with dot notation
person.first_name = "Alice";
person.last_name = "Smith";
person.phone_number = 5551234567;
person.email = "alice@email.com";

// Print member values
std::cout << person.first_name << " " << person.last_name << std::endl;
// Output: Alice Smith
```

### Initializing Structs

```cpp
// Uniform initialization (preferred!)
// Values must match order of data members
Contact alice{"Alice", "Smith", 5551234567, "alice@email.com"};

// Access after initialization
std::cout << alice.first_name;  // "Alice"
std::cout << alice.phone_number;  // 5551234567
```

### Structs with Vectors

```cpp
struct Student {
    std::string name;
    int age;
    double gpa;
};

// Vector of structs
std::vector<Student> class_roster;

class_roster.push_back({"Alice", 20, 3.8});
class_roster.push_back({"Bob", 21, 3.5});

// Access
std::cout << class_roster.at(0).name;  // "Alice"
std::cout << class_roster.at(1).gpa;   // 3.5
```

### Structs Can Be Passed and Returned

```cpp
// Structs can be function parameters
void PrintContact(Contact c) {
    std::cout << c.first_name << " " << c.last_name << std::endl;
}

// Structs can be returned from functions
Contact CreateContact(std::string first, std::string last) {
    Contact c;
    c.first_name = first;
    c.last_name = last;
    return c;
}
```

### ⚠️ Struct Limitations

```cpp
Contact a{"Alice", "Smith", 123, "a@b.com"};
Contact b{"Alice", "Smith", 123, "a@b.com"};

// ❌ Cannot compare directly (not defined!)
if (a == b) { }  // ERROR!

// ❌ Cannot output directly (not defined!)
std::cout << a;  // ERROR!

// ✅ Must compare/output members individually
if (a.first_name == b.first_name && a.last_name == b.last_name) {
    std::cout << "Same name!" << std::endl;
}
```

### File Organization for Structs

```cpp
// contact.h (header file)
#ifndef CONTACT_H_
#define CONTACT_H_

#include <string>

struct Contact {
    std::string first_name;
    std::string last_name;
    long phone_number;
};

#endif

// main.cc
#include "contact.h"

int main() {
    Contact c{"Alice", "Smith", 5551234567};
    return 0;
}
```

### Struct vs std::pair

```cpp
// ❌ Using pair — unclear what first/second mean
std::pair<std::string, int> p{"Alice", 25};
p.first;   // What is this? Name? ID?
p.second;  // What is this? Age? Score?

// ✅ Using struct — self-documenting
struct Person {
    std::string name;
    int age;
};
Person person{"Alice", 25};
person.name;  // Clear!
person.age;   // Clear!
```

> 💡 **Rule:** Prefer `struct` over `std::pair` for clarity.

---

## 📚 C-Style Arrays (Under the Hood)

Arrays are **fixed-size** containers. Understanding them helps you understand vectors.

### Declaring Arrays

```cpp
constexpr int SIZE = 5;  // Size MUST be known at compile time!
int arr[SIZE];           // Array of 5 integers (uninitialized!)

// Initialize with values
int nums[5] = {1, 2, 3, 4, 5};

// Let compiler infer size
int values[] = {10, 20, 30};  // Size = 3
```

### Array vs Vector

```
┌─────────────────────────┬──────────────────────────────┐
│        ARRAY            │          VECTOR              │
├─────────────────────────┼──────────────────────────────┤
│ Fixed size at compile   │ Can grow dynamically         │
│ Doesn't know its size   │ Knows its size (.size())     │
│ No bounds checking      │ .at() checks bounds          │
│ Cannot be returned      │ Can be returned from func    │
│ Decays to pointer       │ Copies when passed           │
└─────────────────────────┴──────────────────────────────┘
```

### ⚠️ Arrays Don't Know Their Size

```cpp
constexpr int SIZE = 5;
int arr[SIZE] = {1, 2, 3, 4, 5};

// Must track size separately!
for (int i = 0; i < SIZE; ++i) {
    std::cout << arr[i] << " ";
}

// Pass size along with array to functions
void PrintArray(int arr[], int size) {
    for (int i = 0; i < size; ++i) {
        std::cout << arr[i] << " ";
    }
}
```

### ⚠️ No Bounds Checking

```cpp
int arr[5] = {1, 2, 3, 4, 5};

arr[10] = 999;  // ❌ No error! Writes to invalid memory!
                // This can crash your program or cause bugs

// Valid indices: 0, 1, 2, 3, 4
// arr[5] is ALREADY out of bounds!
```

### Array Decay to Pointer

```cpp
void ModifyArray(int arr[]) {
    arr[0] = 999;  // Modifies ORIGINAL array!
}

int main() {
    int nums[3] = {1, 2, 3};
    ModifyArray(nums);
    std::cout << nums[0];  // 999 (changed!)
}
```

> ⚠️ Arrays passed to functions become pointers — changes affect the original!

> 💡 **Use `std::vector` instead of arrays** unless you have a specific reason not to.

---

## 📜 Character Sequences (C-Strings)

Old-style strings are null-terminated character arrays.

```cpp
// C-string: array of chars ending with '\0'
char greeting[6] = {'H', 'e', 'l', 'l', 'o', '\0'};

// Shorthand (compiler adds '\0' automatically)
char greeting[] = "Hello";  // Size = 6 (5 chars + '\0')
```

```
char name[] = "Hello";

┌─────┬─────┬─────┬─────┬─────┬─────┐
│ 'H' │ 'e' │ 'l' │ 'l' │ 'o' │ '\0'│
└─────┴─────┴─────┴─────┴─────┴─────┘
  [0]   [1]   [2]   [3]   [4]   [5]
                                 ↑
                          Null terminator
                          (marks end of string)
```

### Using C-Strings with I/O

```cpp
char name[80];
std::cout << "Enter name: ";
std::cin >> name;  // Works with char arrays
std::cout << "Hello, " << name << "!" << std::endl;
```

### Converting C-String to std::string

```cpp
char cstr[] = "Hello";
std::string str = cstr;  // Automatic conversion
```

### C-String Limitations

```cpp
char name[20];

// ❌ Cannot assign after declaration
name = "Hello";  // ERROR!

// ✅ Just use std::string instead!
std::string name = "Hello";
name = "World";  // Works perfectly
```

> 💡 **Always prefer `std::string`** over C-strings. They're safer and easier to use.

---

## 🔑 Key Takeaways

| Container | Use When |
|-----------|----------|
| `std::vector` | Need a resizable list of same-type elements |
| `std::string` | Working with text |
| `std::map` | Need to look up values by key |
| `struct` | Grouping related data of different types |
| `2D vector` | Grid/matrix data (rows and columns) |
| `array` | Fixed size known at compile time (prefer vector) |

### Best Practices

1. **Use `.at()` instead of `[]`** for bounds checking
2. **Pre-allocate vectors** if you know the size
3. **Use `struct` instead of `std::pair`** for clarity
4. **Prefer `std::string` over C-strings**
5. **Prefer `std::vector` over C-style arrays**
6. **Maps keep keys sorted** — remember this for iteration order

