# 🎓 Memory Management: Stack & Free Store

---

## 🗺️ Program Memory Layout

When your program runs, the operating system gives it a chunk of memory divided into regions:

```
┌─────────────────────────────────────────────────────────────────┐
│                    PROGRAM MEMORY LAYOUT                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   High Address                                                  │
│   ┌─────────────────────────────────────────┐                   │
│   │              STACK                      │ ← Automatic       │
│   │         (grows downward ↓)              │   variables       │
│   ├─────────────────────────────────────────┤                   │
│   │                                         │                   │
│   │           EMPTY SPACE                   │                   │
│   │                                         │                   │
│   ├─────────────────────────────────────────┤                   │
│   │           FREE STORE                    │ ← Dynamic         │
│   │         (grows upward ↑)                │   objects         │
│   ├─────────────────────────────────────────┤                   │
│   │         CODE / STATIC                   │ ← Program code    │
│   │                                         │   Global vars     │
│   └─────────────────────────────────────────┘                   │
│   Low Address                                                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Three Memory Regions

| Region | Contents | Lifetime |
|--------|----------|----------|
| **Code/Static** | Program instructions, global/static variables | Entire program |
| **Stack** | Local variables, function parameters | Until function returns |
| **Free Store** | Dynamically allocated objects (`new`) | Until you `delete` |

> 💡 **Stack Overflow**: When the stack grows too much and collides with the free store!

---

## 📚 The Stack

The **stack** manages function calls automatically.

### Activation Record (Stack Frame)

Each function call gets its own **activation record** containing:

1. **Parameters** — Arguments passed to the function
2. **Local variables** — Variables declared inside the function
3. **Housekeeping** — Return address, etc.

```
┌─────────────────────────────────────────────────────────────────┐
│                    STACK FRAMES                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌─────────────────────┐                                       │
│   │  factorial(1)       │ ← Stack Pointer points here           │
│   │  n = 1              │                                       │
│   ├─────────────────────┤                                       │
│   │  factorial(2)       │                                       │
│   │  n = 2              │                                       │
│   ├─────────────────────┤                                       │
│   │  factorial(3)       │                                       │
│   │  n = 3              │                                       │
│   ├─────────────────────┤                                       │
│   │  main()             │                                       │
│   │  result = ?         │                                       │
│   └─────────────────────┘                                       │
│                                                                 │
│   Each function call PUSHES a new frame                         │
│   Each return POPS the frame off                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Example: Recursive Factorial

```cpp
int factorial(int n) {
    if (n <= 1) return 1;          // Base case
    return n * factorial(n - 1);   // Recursive call
}

int main() {
    int result = factorial(3);     // Call factorial
    return 0;
}
```

### Stack Execution Trace

```
1. main() called         → Push main's frame
2. factorial(3) called   → Push factorial(3)'s frame
3. factorial(2) called   → Push factorial(2)'s frame
4. factorial(1) called   → Push factorial(1)'s frame
5. factorial(1) returns  → Pop factorial(1)'s frame
6. factorial(2) returns  → Pop factorial(2)'s frame
7. factorial(3) returns  → Pop factorial(3)'s frame
8. main() continues      → result = 6
```

---

## 🎯 The Stack Pointer

The **stack pointer** always points to the top of the stack (current activation record).

```cpp
void times_two(int t1) {
    int r1 = t1 * 2;
    return r1;
}

int main() {
    int i = 10;
    int z = times_two(i);
    return 0;
}
```

### Memory During Execution

```
STEP 1: main() starts
┌─────────────────────┐
│  main()             │ ← Stack Pointer (SP)
│  i = 10             │
│  z = ?              │
└─────────────────────┘

STEP 2: times_two(10) called
┌─────────────────────┐
│  times_two()        │ ← Stack Pointer (SP)
│  t1 = 10            │
│  r1 = ?             │
├─────────────────────┤
│  main()             │
│  i = 10             │
│  z = ?              │
└─────────────────────┘

STEP 3: times_two() returns 20
┌─────────────────────┐
│  main()             │ ← Stack Pointer (SP)
│  i = 10             │
│  z = 20             │
└─────────────────────┘

times_two's frame is GONE!
```

### Accessing Variables via Stack Pointer

```
Stack Pointer = 1032

┌────────────────────────────┐
│ Address 1032: t1 = 10      │ ← SP + 0 (offset 0)
├────────────────────────────┤
│ Address 1036: r1 = 20      │ ← SP + 4 (offset 4)
└────────────────────────────┘

To access t1: *(SP + 0)
To access r1: *(SP + sizeof(int))
```

---

## ⚠️ Why Size Must Be Known at Compile Time

The compiler needs **fixed offsets** to access variables.

### ✅ Fixed-Size Array (Works)

```cpp
void func() {
    const int size = 3;
    int a[size];      // Compiler knows: 3 × 4 = 12 bytes
    double c = 0.0;   // Compiler knows: c is at offset 12
}
```

```
Stack Frame (always same layout):
┌────────────────┐
│ a[0]           │ offset 0
│ a[1]           │ offset 4
│ a[2]           │ offset 8
│ c              │ offset 12 ← ALWAYS at offset 12!
└────────────────┘
```

### ❌ Variable-Length Array (Doesn't Work in C++)

```cpp
void func(int size) {
    int a[size];      // ❌ NOT ALLOWED IN C++!
    double c = 0.0;   // Where is c? Depends on size!
}
```

```
If size = 3:              If size = 100:
┌────────────────┐        ┌────────────────┐
│ a[0..2]        │        │ a[0..99]       │
│ c at offset 12 │        │ c at offset 400│
└────────────────┘        └────────────────┘

c's offset VARIES — compiler can't handle this!
```

> ⚠️ **Variable-length arrays are NOT supported in C++** (they are in C99).
> Some compilers allow them as an extension, but **don't use them** — it's non-portable!

---

## 🚫 Limitations of the Stack

### Problem 1: Can't Create Variable-Size Collections

```cpp
// We don't know how many items to read!
int read_numbers_from_file(std::string filename) {
    int data[???];  // What size? We don't know yet!
    // ...
}
```

### Problem 2: Data Dies When Function Returns

```cpp
int* problematic() {
    int data[10];        // Array on the stack
    // ... fill data ...
    return data;         // ❌ DISASTER! data is deallocated!
}

int main() {
    int* my_data = problematic();
    my_data[0] = 5;      // 💥 Accessing deallocated memory!
}
```

```
WHAT HAPPENS:

1. problematic() called
   ┌─────────────────────┐
   │ data[0..9]          │ ← On the stack
   └─────────────────────┘

2. problematic() returns address of data[0]
   ┌─────────────────────┐
   │ (deallocated)       │ ← Frame is POPPED!
   └─────────────────────┘
   
   my_data points to garbage! 💥
```

### Solution: The Free Store!

We need memory that:
- Can be sized at **runtime**
- Lives until **we decide** to deallocate it

---

## 🆕 Dynamic Memory: `new` and `delete`

### Allocating with `new`

```cpp
// Single object
int* p = new int;           // Uninitialized
int* q = new int{42};       // Initialized to 42

// Array of objects
int* arr = new int[10];     // Array of 10 ints (uninitialized)
int* arr2 = new int[10]{};  // Array of 10 ints (zero-initialized)
```

### Deallocating with `delete`

```cpp
// Single object
delete p;           // Deallocate single object

// Array
delete[] arr;       // Deallocate array (note the [])
```

> ⚠️ **Critical:** Use `delete[]` for arrays, `delete` for single objects!

### Complete Example

```cpp
int main() {
    // 1. Allocate on free store
    int* ptr = new int{7};
    
    // 2. Use the object
    std::cout << *ptr << std::endl;  // Output: 7
    *ptr = 42;
    
    // 3. Deallocate when done
    delete ptr;
    ptr = nullptr;  // Good practice: null out the pointer
    
    return 0;
}
```

---

## 📊 Stack vs Free Store

```cpp
int main() {
    int capacity = 4;                          // Stack
    int* arr = new int[capacity]{10,20,30,40}; // Pointer on stack,
                                               // Array on free store
    // ...
    delete[] arr;
    arr = nullptr;
    return 0;
}
```

### Memory Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                    STACK vs FREE STORE                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   STACK                           FREE STORE                    │
│   ┌────────────────────┐          ┌────────────────────┐        │
│   │ capacity = 4       │          │ [10][20][30][40]   │        │
│   ├────────────────────┤          └────────────────────┘        │
│   │ arr = 0x1000  ─────┼──────────────────►                     │
│   └────────────────────┘                                        │
│                                                                 │
│   Named objects (variables)       Unnamed objects               │
│   on the STACK                    on the FREE STORE             │
│   Automatic lifetime              Manual lifetime               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Key Differences

| Aspect | Stack | Free Store |
|--------|-------|------------|
| **Allocation** | Automatic (declaration) | Manual (`new`) |
| **Deallocation** | Automatic (function returns) | Manual (`delete`) |
| **Size** | Must know at compile time | Can determine at runtime |
| **Lifetime** | Tied to function scope | You control it |
| **Access** | Direct (by name) | Indirect (through pointer) |

---

## 🔄 Resizing a Dynamically Allocated Array

Arrays can't be "stretched" — you must:
1. Create a new, larger array
2. Copy the old data
3. Delete the old array

```cpp
struct MyArray {
    int* data;
    size_t size;      // Number of elements used
    size_t capacity;  // Total slots available
};

void resize(MyArray& arr) {
    // 1. Calculate new capacity (e.g., double it)
    size_t new_capacity = arr.capacity * 2;
    
    // 2. Allocate new array
    int* new_data = new int[new_capacity];
    
    // 3. Copy old data to new array
    for (size_t i = 0; i < arr.size; ++i) {
        new_data[i] = arr.data[i];
    }
    
    // 4. Delete old array
    delete[] arr.data;
    
    // 5. Update pointer and capacity
    arr.data = new_data;
    arr.capacity = new_capacity;
}
```

### Resize Visualization

```
BEFORE resize():
┌───────────────────────────────────────┐
│  arr.data ──────► [10][20]            │  capacity = 2, size = 2
└───────────────────────────────────────┘

STEP 1-2: Create new array (capacity × 2)
┌───────────────────────────────────────┐
│  arr.data ──────► [10][20]            │
│  new_data ──────► [ ][ ][ ][ ]        │
└───────────────────────────────────────┘

STEP 3: Copy data
┌───────────────────────────────────────┐
│  arr.data ──────► [10][20]            │
│  new_data ──────► [10][20][ ][ ]      │
└───────────────────────────────────────┘

STEP 4-5: Delete old, update pointer
┌───────────────────────────────────────┐
│  arr.data ──────► [10][20][ ][ ]      │  capacity = 4, size = 2
└───────────────────────────────────────┘
```

---

## 📜 The Memory Contract

When you use `new`, you enter an **implicit contract** with the free store manager:

### Clause 1: Return What You Borrow

**Violation:** Memory Leak

```cpp
void memory_leak() {
    int* p = new int{7};
    p = new int{11};      // ❌ Lost pointer to 7!
    // Can never delete the first object!
}
```

```
┌─────────────────────────────────────────────────────────────────┐
│                      MEMORY LEAK                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   STEP 1: p points to 7                                         │
│   p ──────► [7]                                                 │
│                                                                 │
│   STEP 2: p reassigned to 11                                    │
│   p ──────► [11]                                                │
│             [7]  ← LEAKED! No pointer to it!                    │
│                                                                 │
│   We can NEVER call delete on [7] — it's lost forever!          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Clause 2: Stop Using Returned Memory

**Violation:** Dangling Pointer

```cpp
void dangling_pointer() {
    int* p = new int{7};
    delete p;             // Memory returned
    
    std::cout << *p;      // ❌ Dangling pointer!
    // p still holds the address, but memory is deallocated
}
```

```
┌─────────────────────────────────────────────────────────────────┐
│                    DANGLING POINTER                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   AFTER delete:                                                 │
│   p ──────► [???]   Memory deallocated, but p still points!    │
│                                                                 │
│   The value might still be there temporarily,                   │
│   but it can be overwritten at ANY time!                        │
│                                                                 │
│   FIX: Set p = nullptr after delete                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Clause 3: Don't Return Twice / Don't Return What You Didn't Borrow

**Violation:** Runtime Error (Crash!)

```cpp
void double_delete() {
    int* p = new int{7};
    delete p;
    delete p;             // ❌ CRASH! Double delete!
}
```

```
Error: pointer being freed was not allocated
```

### Multiple Pointers to Same Object

```cpp
int* p = new int{7};
int* q = p;               // Both point to same object

delete p;
p = nullptr;

// q is now a DANGLING POINTER!
std::cout << *q;          // ❌ Undefined behavior!
```

---

## ✅ Best Practices

### Always Pair `new` with `delete`

```cpp
int* p = new int{42};
// ... use p ...
delete p;
p = nullptr;  // Prevent accidental reuse
```

### Use `delete[]` for Arrays

```cpp
int* arr = new int[10];
// ... use arr ...
delete[] arr;     // ✅ Correct
// delete arr;    // ❌ Wrong! Undefined behavior!
arr = nullptr;
```

### Null Out Pointers After Delete

```cpp
delete p;
p = nullptr;    // Now dereferencing p will crash immediately
                // (easier to debug than random garbage)
```

---

## 🔧 Memory Debugging Tools

### AddressSanitizer (ASan)

Detects memory access errors at runtime.

```bash
# Compile with ASan
clang++ -fsanitize=address -g main.cpp -o main

# Run
./main
```

**Detects:**
- Out-of-bounds access
- Use-after-free
- Double-free

### UndefinedBehaviorSanitizer (UBSan)

Detects undefined behavior.

```bash
# Compile with UBSan
clang++ -fsanitize=undefined -g main.cpp -o main
```

**Detects:**
- Null pointer dereference
- Integer overflow
- Array out-of-bounds (for stack arrays)

### Valgrind (Memory Leak Detection)

```bash
# Compile with debug symbols
g++ -g main.cpp -o main

# Run with Valgrind
valgrind --leak-check=full ./main
```

**Output Example:**
```
HEAP SUMMARY:
    in use at exit: 40 bytes in 1 blocks
    total heap usage: 2 allocs, 1 frees

LEAK SUMMARY:
    definitely lost: 40 bytes in 1 blocks
```

---

## 🔑 Key Takeaways

### Memory Regions

| Region | What's Stored | Lifetime |
|--------|---------------|----------|
| Stack | Local variables, parameters | Automatic (function scope) |
| Free Store | `new` objects | Manual (`delete`) |
| Static | Global/static variables | Entire program |

### Stack Rules

1. Size must be known at **compile time**
2. Memory is deallocated when function **returns**
3. **Never return pointers** to stack variables!

### Free Store Rules

1. Every `new` needs a matching `delete`
2. Use `delete[]` for arrays
3. Set pointers to `nullptr` after delete
4. Don't use memory after deleting it

### Memory Bugs

| Bug | Cause | Result |
|-----|-------|--------|
| Memory Leak | Forgot to `delete` | Memory consumption grows |
| Dangling Pointer | Using after `delete` | Random crashes/data |
| Double Delete | Calling `delete` twice | Crash |

### Golden Rule

```cpp
// For every new, there must be exactly one delete
int* p = new int{42};    // ← Allocation
// ...
delete p;                 // ← Deallocation (1:1 match)
p = nullptr;              // ← Good practice
```

