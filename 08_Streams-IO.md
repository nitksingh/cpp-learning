# 🎓 Streams & Input/Output

---

## 🌊 What is a Stream?

A **stream** is a sequence of characters flowing into or out of your program.

```
┌─────────────────────────────────────────────────────────────────┐
│                     STREAM CONCEPT                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   User types: "Michael Nowak"                                   │
│                                                                 │
│   Stream (std::cin):                                            │
│   ┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐   │
│   │ M │ i │ c │ h │ a │ e │ l │   │ N │ o │ w │ a │ k │\n │   │
│   └───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘   │
│     ↑                                                           │
│     Extraction arm pulls characters out                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Types of Streams

| Stream | Purpose | Header | Example |
|--------|---------|--------|---------|
| `std::cin` | Read from keyboard | `<iostream>` | `std::cin >> name;` |
| `std::cout` | Write to screen | `<iostream>` | `std::cout << name;` |
| `std::cerr` | Write errors to screen | `<iostream>` | `std::cerr << "Error!";` |
| `std::ifstream` | Read from file | `<fstream>` | `ifs >> value;` |
| `std::ofstream` | Write to file | `<fstream>` | `ofs << value;` |

### Stream Operators

| Operator | Name | Direction | Used With |
|----------|------|-----------|-----------|
| `<<` | Insertion | Into stream | `cout`, `ofstream` |
| `>>` | Extraction | Out of stream | `cin`, `ifstream` |

---

## 📤 Writing to Standard Output

Use `std::cout` to display output to the terminal.

```cpp
#include <iostream>

int main() {
    std::string name = "Alice";
    int age = 25;
    
    // Insertion operator << sends data INTO the stream
    std::cout << "Name: " << name << std::endl;
    std::cout << "Age: " << age << std::endl;
    
    return 0;
}
```

### How the Insertion Operator Works

```
std::cout << "Name: " << name << std::endl;

              ↓        ↓       ↓
         "Name: " → stream
                    name → stream  
                           endl → stream → SCREEN

Output: Name: Alice
```

### Formatting Output

```cpp
// Multiple items on one line
std::cout << "Name: " << name << ", Age: " << age << std::endl;

// Escape sequences
std::cout << "Line 1\nLine 2" << std::endl;   // Newline
std::cout << "Col1\tCol2" << std::endl;       // Tab

// No automatic newline
std::cout << "Hello ";
std::cout << "World";  // Output: Hello World (same line)
```

### ⚠️ Can't Directly Output User-Defined Types

```cpp
struct Contact {
    std::string name;
    int age;
};

Contact person{"Alice", 25};

// ❌ This won't work!
std::cout << person;  // ERROR: undefined for this type

// ✅ Must output each field
std::cout << "Name: " << person.name << std::endl;
std::cout << "Age: " << person.age << std::endl;
```

> 💡 Later, you'll learn **operator overloading** to make `std::cout << person` work!

---

## 📁 Writing to Files

Use `std::ofstream` (Output File Stream) to write to files.

```cpp
#include <fstream>  // For file streams

int main() {
    // Create output file stream, bind to file
    std::ofstream ofs{"output.txt"};
    
    // ALWAYS check if file opened!
    if (!ofs.is_open()) {
        std::cerr << "Error: Could not open file!" << std::endl;
        return 1;  // Exit with error
    }
    
    // Write to file (exactly like cout!)
    std::string name = "Alice";
    int number = 42;
    
    ofs << "Name: " << name << std::endl;
    ofs << "Number: " << number << std::endl;
    
    // File automatically closes when ofs goes out of scope
    return 0;
}
```

### File Contents After Running

```
output.txt:
┌─────────────────────┐
│ Name: Alice         │
│ Number: 42          │
└─────────────────────┘
```

### ⚠️ Warning: Overwriting Files

```cpp
// This OVERWRITES existing content!
std::ofstream ofs{"data.txt"};  // Old content is GONE!
```

### Appending to Files

```cpp
// To ADD to end of file (not overwrite):
std::ofstream ofs{"data.txt", std::ios::app};
//                            ↑
//                            Append mode

ofs << "New line added!" << std::endl;
```

---

## 📥 Reading from Standard Input

Use `std::cin` to read input from the user.

```cpp
#include <iostream>
#include <string>

int main() {
    std::string first_name;
    std::string last_name;
    
    std::cout << "Enter your name: ";
    std::cin >> first_name >> last_name;
    //       ↑
    //       Extraction operator: pulls data OUT of stream
    
    std::cout << "Hello, " << first_name << " " << last_name << "!" << std::endl;
    
    return 0;
}
```

### How Extraction Works (Whitespace-Delimited)

```
User types: "Michael Nowak"

Stream: [M][i][c][h][a][e][l][ ][N][o][w][a][k][\n]
                             ↑
                         Whitespace stops reading

first_name gets: "Michael"
last_name gets:  "Nowak"
```

### Reading a Whole Line

Use `std::getline()` to read an entire line (including spaces):

```cpp
std::string full_name;

std::cout << "Enter your full name: ";
std::getline(std::cin, full_name);
//           ↑          ↑
//           stream     destination

// User types: "Michael Nowak"
// full_name gets: "Michael Nowak" (with the space!)
```

---

## 📂 Reading from Files

Use `std::ifstream` (Input File Stream) to read from files.

```cpp
#include <fstream>
#include <string>

int main() {
    // Bind input file stream to file
    std::ifstream ifs{"data.txt"};
    
    // ALWAYS check if file opened!
    if (!ifs.is_open()) {
        std::cerr << "Error: Could not open file!" << std::endl;
        return 1;
    }
    
    // Read from file (exactly like cin!)
    int number;
    double value;
    std::string text;
    
    ifs >> number >> value >> text;
    
    std::cout << "Read: " << number << ", " << value << ", " << text << std::endl;
    
    return 0;
}
```

### Reading All Lines from a File

```cpp
std::ifstream ifs{"data.txt"};

if (!ifs.is_open()) {
    return 1;
}

std::string line;
while (std::getline(ifs, line)) {
    std::cout << line << std::endl;
}
```

### Reading All Values of a Type

```cpp
std::ifstream ifs{"numbers.txt"};
// File contains: "1 2 3 4 5"

int value;
while (ifs >> value) {
    std::cout << value << " ";
}
// Output: 1 2 3 4 5
```

---

## 🔢 Formatted Reads

The extraction operator (`>>`) interprets input based on the **target type**.

### Key Rules

1. **Whitespace-delimited** — stops at spaces, tabs, newlines
2. **Type-based** — interprets characters according to target type
3. **Format must match** — or the read fails!

### Reading Different Types

```cpp
int i;
double d;
std::string s;

std::cout << "Enter int, double, string: ";
std::cin >> i >> d >> s;

// User types: "10 3.14 text"
// i = 10
// d = 3.14
// s = "text"
```

### How It Works Step-by-Step

```
Input: "10 3.14 text"

┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ 1 │ 0 │   │ 3 │ . │ 1 │ 4 │   │ t │ e │ x │ t │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
  └─┬─┘   └────┬────┘   └────┬────┘
    ↓          ↓              ↓
   i=10      d=3.14        s="text"
   (int)     (double)      (string)
```

### Type Determines What Gets Read

```cpp
int i;
std::cin >> i;

// User types: "3.14"
// What happens?
```

```
Input: "3.14"

┌───┬───┬───┬───┐
│ 3 │ . │ 1 │ 4 │
└───┴───┴───┴───┘
  ↓   ↑
  │   '.' cannot be part of int!
  │
  i = 3

Remaining in stream: ".14"
```

### Parsing Structured Data

```cpp
// Reading "3.14" as int, char, int
int whole;
char dot;
int fraction;

std::cin >> whole >> dot >> fraction;

// whole = 3
// dot = '.'
// fraction = 14
```

```
Input: "3.14"

┌───┬───┬───┬───┐
│ 3 │ . │ 1 │ 4 │
└───┴───┴───┴───┘
  ↓   ↓   └─┬─┘
  │   │     └── fraction = 14 (int)
  │   └── dot = '.' (char)
  └── whole = 3 (int)
```

---

## 🚦 Stream States

Every stream has a **state** that tells you if operations succeeded.

```cpp
std::ifstream ifs("data.txt");

ifs.good();  // True if everything is OK
ifs.fail();  // True if recoverable error OR bad
ifs.bad();   // True if unrecoverable error (corrupted)
ifs.eof();   // True if end of file reached
```

### State Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                     STREAM STATES                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌──────────┐                                                  │
│   │   GOOD   │  ← Normal state, reads succeed                   │
│   └────┬─────┘                                                  │
│        │                                                        │
│        │ Format error or EOF                                    │
│        ▼                                                        │
│   ┌──────────┐                                                  │
│   │   FAIL   │  ← Recoverable error (format mismatch)           │
│   └────┬─────┘    Can recover with clear() + ignore()           │
│        │                                                        │
│        │ Corruption                                             │
│        ▼                                                        │
│   ┌──────────┐                                                  │
│   │   BAD    │  ← Unrecoverable error (corrupted stream)        │
│   └──────────┘    Usually must terminate program                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Checking States — Order Matters!

```cpp
// Check bad FIRST, then fail
if (ifs.bad()) {
    // Unrecoverable error — stream is corrupted
    std::cerr << "Fatal error!" << std::endl;
    return 1;
}

if (ifs.fail()) {
    // Recoverable error — format mismatch
    // (We know it's not bad because we checked bad first)
}
```

> 💡 **Important:** `fail()` returns `true` for BOTH bad AND recoverable errors. Check `bad()` first!

---

## ❌ Format Read Errors

A **format error** occurs when the input doesn't match the expected type.

### Example: Expecting int, Getting char

```cpp
int any_number = 0;
std::cout << "Enter a number: ";
std::cin >> any_number;  // User types "AB"

// What happens?
```

```
Input: "AB"

┌───┬───┐
│ A │ B │
└───┴───┘
  ↑
  Extraction arm latches on

Expected: integer
Got: character 'A'

❌ FORMAT MISMATCH! Read FAILS!

- Stream state changes to FAIL
- any_number may get 0 (zero value of type)
- 'A' is still in the stream (not removed!)
```

### Example: Reading from File

```cpp
std::ifstream ifs("data.txt");
// File contains: "1 2 a 3"

int value;
while (ifs.good()) {
    ifs >> value;
    std::cout << value << " ";
}
```

```
File: "1 2 a 3"

Read 1: ✅ Success, output "1"
Read 2: ✅ Success, output "2"
Read a: ❌ FAIL! 'a' is not an integer
        - Stream state → FAIL
        - value gets 0
        - 'a' still in stream
        - Loop exits (good() returns false)

Output: 1 2 0
(Never reads 3!)
```

---

## 🔧 Recovery: The Wrong Way

### ❌ Common Mistake: Only Using `clear()`

```cpp
while (ifs.good()) {
    ifs >> value;
    
    if (ifs.fail()) {
        ifs.clear();  // Reset state to good
        // ⚠️ BUT 'a' is still in the stream!
    }
    
    std::cout << value << " ";
}
```

```
What happens:

1. Read 1 → success
2. Read 2 → success
3. Read 'a' → FAIL, clear() resets state
4. But 'a' is still there! → Read 'a' → FAIL, clear()
5. 'a' is still there! → Read 'a' → FAIL, clear()
... INFINITE LOOP! ...

Output: 1 2 0 0 0 0 0 0 0 0 ... (forever)
```

### Why This Happens

```
After clear():
┌───┬───┬───┐
│ a │   │ 3 │     Stream state: GOOD ✓
└───┴───┴───┘
  ↑
  Still latched on 'a'!

clear() only resets FLAGS — it does NOT remove characters!
```

---

## ✅ Recovery: The Right Way

You need **BOTH** `clear()` AND `ignore()`:

1. **`clear()`** — Reset stream state flags to GOOD
2. **`ignore()`** — Remove problematic characters from stream

```cpp
while (ifs.good()) {
    ifs >> value;
    
    if (ifs.fail()) {
        ifs.clear();     // Step 1: Reset flags
        ifs.ignore(1);   // Step 2: Skip 1 character
    } else {
        std::cout << value << " ";  // Only use valid data!
    }
}
```

```
Step-by-step:

1. Read 1 → success → output "1"
2. Read 2 → success → output "2"
3. Read 'a' → FAIL
   - clear() → state is GOOD
   - ignore(1) → skip 'a'
   ┌───┬───┐
   │   │ 3 │     'a' removed!
   └───┴───┘
       ↑
       Now at whitespace
       
4. Read 3 → success → output "3"

Output: 1 2 3 ✓
```

---

## 📚 The `ignore()` Function

```cpp
stream.ignore(count, delimiter);
```

| Parameter | Meaning |
|-----------|---------|
| `count` | Maximum number of characters to skip |
| `delimiter` | Stop when this character is found |

Ignores characters until **whichever comes first**: count reached OR delimiter found.

### Common Patterns

```cpp
// Skip exactly 1 character
ifs.ignore(1);

// Skip until newline (clear rest of line)
ifs.ignore(100, '\n');

// Skip until whitespace
ifs.ignore(100, ' ');

// Skip ALL remaining characters until delimiter
#include <limits>
ifs.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
//         ↑
//         Maximum possible characters (skip everything)
```

---

## 🔄 Complete Input Validation Workflow

```cpp
#include <iostream>
#include <fstream>
#include <limits>

int main() {
    std::ifstream ifs("data.txt");
    int value;
    
    while (ifs.good()) {
        ifs >> value;
        
        // Step 1: Check for unrecoverable error
        if (ifs.bad()) {
            std::cerr << "Fatal error: stream corrupted!" << std::endl;
            return 1;  // Exit program
        }
        
        // Step 2: Check for recoverable error (format mismatch)
        if (ifs.fail()) {
            ifs.clear();     // Reset state flags
            ifs.ignore(1);   // Skip problematic character
            continue;        // Try reading again
        }
        
        // Step 3: Only work with data if read succeeded!
        std::cout << value << " ";
    }
    
    return 0;
}
```

### Workflow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                  INPUT VALIDATION WORKFLOW                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌──────────────────┐                                          │
│   │  Attempt Read    │                                          │
│   │  stream >> value │                                          │
│   └────────┬─────────┘                                          │
│            │                                                    │
│            ▼                                                    │
│   ┌──────────────────┐     YES    ┌─────────────────────┐       │
│   │  stream.bad()?   │ ─────────► │ Unrecoverable!      │       │
│   └────────┬─────────┘            │ Exit with error     │       │
│            │ NO                   └─────────────────────┘       │
│            ▼                                                    │
│   ┌──────────────────┐     YES    ┌─────────────────────┐       │
│   │  stream.fail()?  │ ─────────► │ clear() + ignore()  │       │
│   └────────┬─────────┘            │ Try again           │──┐    │
│            │ NO                   └─────────────────────┘  │    │
│            ▼                                               │    │
│   ┌──────────────────┐                                     │    │
│   │ Read succeeded!  │                                     │    │
│   │ Use the data     │◄────────────────────────────────────┘    │
│   └──────────────────┘                                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🎯 Command Line Arguments

Programs can receive arguments when launched from the terminal.

```bash
$ ./my_program arg1 arg2 arg3
```

### argc and argv

```cpp
int main(int argc, char* argv[]) {
//        ↑            ↑
//        │            └── Array of C-strings (the arguments)
//        └── Argument Count (how many)
    
    return 0;
}
```

### What's in argv?

```bash
$ ./greet Michael Nowak
```

```
argc = 3

argv[0] = "./greet"      ← Program name (always first!)
argv[1] = "Michael"
argv[2] = "Nowak"
```

```
┌──────────────────────────────────────────────────────┐
│                     argv                             │
├──────────────────────────────────────────────────────┤
│  [0]        [1]          [2]                         │
│    ↓          ↓            ↓                         │
│ "./greet" "Michael"    "Nowak"                       │
│                                                      │
│  argc = 3 (program name counts!)                     │
└──────────────────────────────────────────────────────┘
```

### Converting to std::string

```cpp
#include <iostream>
#include <string>

int main(int argc, char* argv[]) {
    // Convert C-string to std::string
    std::string program_name = argv[0];
    
    std::cout << "Program: " << program_name << std::endl;
    
    // Print all arguments
    for (int i = 0; i < argc; ++i) {
        std::string arg = argv[i];  // Convert each one
        std::cout << "Arg " << i << ": " << arg << std::endl;
    }
    
    return 0;
}
```

### Validating Arguments

```cpp
int main(int argc, char* argv[]) {
    // Expect exactly 2 arguments: program name + user name
    if (argc != 2) {
        std::cerr << "Usage: " << argv[0] << " <username>" << std::endl;
        return 1;  // Exit with error
    }
    
    std::string username = argv[1];
    std::cout << "Hello, " << username << "!" << std::endl;
    
    return 0;
}
```

```bash
$ ./greet
Usage: ./greet <username>

$ ./greet Alice
Hello, Alice!

$ ./greet Alice Bob
Usage: ./greet <username>
```

### Passing Strings with Spaces

```bash
# Whitespace-delimited by default
$ ./program one two three
# argv[1]="one", argv[2]="two", argv[3]="three"

# Use quotes for strings with spaces
$ ./program "Hello World" test
# argv[1]="Hello World", argv[2]="test"
```

---

## 💡 Practical Examples

### Example 1: Validate User Input

```cpp
int GetValidInteger() {
    int value;
    
    while (true) {
        std::cout << "Enter a number: ";
        std::cin >> value;
        
        if (std::cin.fail()) {
            std::cin.clear();
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
            std::cout << "Invalid input! Try again." << std::endl;
        } else {
            return value;  // Valid input!
        }
    }
}
```

### Example 2: Read Integers, Skip Non-Integers

```cpp
// File: "1 2 abc 3 xyz 4 5"
// Goal: Read all integers, skip garbage

std::ifstream ifs("data.txt");
int value;
std::vector<int> numbers;

while (ifs >> value || !ifs.eof()) {
    if (ifs.fail()) {
        ifs.clear();
        ifs.ignore(1);  // Skip one character
    } else {
        numbers.push_back(value);
    }
}

// numbers = {1, 2, 3, 4, 5}
```

### Example 3: Command Line File Processor

```cpp
int main(int argc, char* argv[]) {
    if (argc != 2) {
        std::cerr << "Usage: " << argv[0] << " <filename>" << std::endl;
        return 1;
    }
    
    std::string filename = argv[1];
    std::ifstream ifs{filename};
    
    if (!ifs.is_open()) {
        std::cerr << "Error: Cannot open " << filename << std::endl;
        return 1;
    }
    
    std::string line;
    while (std::getline(ifs, line)) {
        std::cout << line << std::endl;
    }
    
    return 0;
}
```

---

## 🔑 Key Takeaways

### Stream States

| Function | Returns `true` when... |
|----------|------------------------|
| `good()` | No errors, OK to read |
| `fail()` | Recoverable OR unrecoverable error |
| `bad()` | Unrecoverable error only |
| `eof()` | End of file reached |

### Recovery Functions

| Function | What it does |
|----------|--------------|
| `clear()` | Resets state flags to GOOD |
| `ignore(n, delim)` | Removes up to n chars or until delim |

### Common Patterns

```cpp
// Writing
std::cout << "Message" << std::endl;
ofs << data << std::endl;

// Reading (whitespace-delimited)
std::cin >> variable;
ifs >> variable;

// Reading whole line
std::getline(std::cin, line);
std::getline(ifs, line);

// File safety check
if (!file_stream.is_open()) {
    return 1;
}

// Recovery
stream.clear();
stream.ignore(1);
```

### Command Line Template

```cpp
int main(int argc, char* argv[]) {
    // 1. Validate argument count
    if (argc != expected_count) {
        std::cerr << "Usage: " << argv[0] << " <args>" << std::endl;
        return 1;
    }
    
    // 2. Convert to std::string
    std::string arg1 = argv[1];
    
    // 3. Use the arguments
    // ...
    
    return 0;
}
```

