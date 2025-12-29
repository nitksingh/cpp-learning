# 🎓 Introduction to C++ Development & Your First Program

---

## 🔄 The Software Development Process

### Step 1: Define the Problem
Figure out **what** you want to solve. Examples:
- "A program that calculates the average of numbers"
- "A program that generates a 2D maze game"

### Step 2: Plan Your Solution
Determine **how** you'll solve it.

> ⚠️ **Don't skip planning!** Studies show only 10-40% of a programmer's time is spent writing code. The rest (60-90%) goes to debugging and maintenance.

### Step 3: Write the Program
Write your instructions in C++ and save as a `.cpp` file — this is your **source code**.

### Step 4: Build the Program
Transform source code into an executable (detailed below).

### Step 5: Run & Debug
Execute your program and fix any issues.

---

## 📝 Writing Source Code

**Source code** = The C++ instructions you write in a text file.

### File Naming Convention
| Convention | Example |
|------------|---------|
| Primary file | `main.cpp` |
| Other files are named after program | `calculator.cpp` |
| Other valid extensions | `.cc`, `.cxx` |

### Example: Hello World Program

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

Save this as `main.cpp`

---

## 🔨 The Build Process

Building transforms your human-readable code into an executable program. This happens in **two steps**:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          BUILD PROCESS                                  │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│   ┌──────────┐              ┌──────────┐              ┌──────────┐      │
│   │  SOURCE  │   COMPILE    │  OBJECT  │    LINK      │EXECUTABLE│      │
│   │   CODE   │  ─────────►  │   FILE   │  ────────►   │   FILE   │      │
│   │ main.cpp │              │  main.o  │              │  a.out   │      │
│   └──────────┘              └──────────┘              └──────────┘      │
│                                                                         │
│     You write this         Machine code              You can run this   │
│                            (intermediate)                               │
└─────────────────────────────────────────────────────────────────────────┘
```

> 💡 **Why `a.out`?** This is the default executable name on Unix systems — short for "**a**ssembler **out**put" (a historical convention). It's not based on your source file name. Use `-o` flag to choose a custom name: `g++ main.cpp -o myprogram`

### Step 1: Compile (Source Code → Object File)

The **compiler** reads your `.cpp` file and translates it into machine code.

**What happens during compilation:**

1. **Syntax checking** — Did you forget a semicolon? Mistype a keyword? The compiler catches these structural errors.

2. **Translation** — If no errors, your C++ code is translated into **machine language** (binary instructions the CPU understands).

3. **Output** — An **object file** is created (`.o` on Linux/Mac, `.obj` on Windows).

```bash
# Compile ONLY (don't link yet)
g++ -c main.cpp
```
**Result:** Creates `main.o` — the object file.

### Step 2: Link (Object File → Executable)

The **linker** takes your object file(s) and combines them with libraries to create the final executable.

**What happens during linking:**

1. **Combines files** — If you have multiple `.cpp` files, each becomes an object file. The linker merges them all.

2. **Resolves references** — Your code uses `std::cout`, but where does that actually live? The linker connects your code to the C++ standard library.

3. **Output** — The final **executable file** you can run.

```bash
# Link the object file into an executable
g++ main.o -o hello
```
**Result:** Creates `hello` — the executable program.

### See It Step by Step

```bash
# STEP 1: Compile only
g++ -c main.cpp          # Creates: main.o

# STEP 2: Link only  
g++ main.o -o hello      # Creates: hello (executable)

# STEP 3: Run
./hello                  # Output: Hello, World!
```

You can verify the files created:
```bash
ls -la
# main.cpp    (your source code)
# main.o      (object file - machine code)
# hello       (executable - runnable program)
```

---

## 🖥️ Popular C++ Compiler Drivers

Here's the key insight: **`g++` and `clang++` are not just compilers — they are "compiler drivers".**

A compiler driver is a convenience tool that automatically runs:
1. The **compiler** (to create object files)
2. The **linker** (to create the executable)

So when you run `g++ main.cpp`, it does BOTH steps internally!

### Industry Standard Tools

| Tool | What It Is | Used By |
|------|------------|---------|
| **GCC (g++)** | GNU Compiler Collection | Linux kernel, most open-source projects |
| **Clang (clang++)** | LLVM-based compiler | Apple, Google, many tech companies |
| **MSVC (cl.exe)** | Microsoft Visual C++ | Microsoft, Windows development |

> 💡 **Clang** is known for excellent error messages — great for beginners!

---

## ⌨️ Using the Compiler Driver

### The One-Step Way (Most Common)

```bash
# Using g++
g++ main.cpp

# Using clang++
clang++ main.cpp
```

**What happens behind the scenes:**
```
g++ main.cpp
     │
     ├──► [COMPILE] main.cpp → main.o (temporary, deleted after)
     │
     └──► [LINK] main.o → a.out (the executable)
```

The compiler driver runs both steps automatically. The object file is created temporarily and deleted after linking. You only see the final executable: `a.out` (Linux/Mac) or `a.exe` (Windows).

### The Two-Step Way (To See What's Happening)

```bash
# Step 1: Compile only — creates main.o
g++ -c main.cpp

# Step 2: Link — creates executable
g++ main.o -o hello
```

Now you can see both files:
```
main.cpp  →  main.o  →  hello
(source)    (object)   (executable)
```

### Key Flags

| Flag | What it does |
|------|--------------|
| `-c` | Compile only, create object file (`.o`), don't link |
| `-o <name>` | Name the output file (instead of default `a.out`) |

### Specifying Output Name with `-o`

```bash
# Without -o flag
clang++ main.cpp           # Output: a.out (default name)

# With -o flag
clang++ main.cpp -o hello  # Output: hello (custom name)
```

| Flag | Purpose |
|------|---------|
| `-o <name>` | Name your executable instead of default `a.out` |

### Specifying C++ Standard with `-std`

```bash
# Use C++20 standard
clang++ -std=c++20 main.cpp -o hello

# Use C++17 standard
g++ -std=c++17 main.cpp -o hello
```

### Full Example with Clang

```bash
clang++ -std=c++20 ./src/main.cpp -o myprogram
```

| Part | Meaning |
|------|---------|
| `clang++` | The Clang C++ compiler |
| `-std=c++20` | Use C++20 standard features |
| `./src/main.cpp` | Path to source file |
| `-o myprogram` | Name output executable "myprogram" |

---

## ▶️ Running Your Program

```bash
# Linux/macOS
./hello

# Windows
hello.exe
```

**Output:**
```
Hello, World!
```

---

## 🐛 Debugging

If the output isn't what you expected, use a **debugger** to:
- Step through code line by line
- Inspect variable values
- Find where things go wrong

Popular debuggers: **GDB** (for GCC), **LLDB** (for Clang)

---

## 🛠️ Using an IDE (Recommended)

An **IDE (Integrated Development Environment)** like **VS Code** bundles everything in one place:

| Tool | Purpose |
|------|---------|
| Code Editor | Write and edit source code |
| Compiler | Translate code to machine language |
| Linker | Combine files into executable |
| Debugger | Find and fix bugs |

### What is a Project?
A **project** is a container that holds:
- All your source code files (`.cpp`, `.h`)
- Images, data files
- Build settings and configurations


