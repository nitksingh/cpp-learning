# 🎓 RAII & Smart Pointers

---

## 🔄 What is RAII?

**RAII** = **R**esource **A**cquisition **I**s **I**nitialization

The core idea: **Tie resource lifetime to object lifetime.**

```
┌─────────────────────────────────────────────────────────────────┐
│                       RAII PRINCIPLE                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   OBJECT CREATED (Constructor)                                  │
│         ↓                                                       │
│   Resource ACQUIRED (new, open file, lock mutex, etc.)          │
│         ↓                                                       │
│   ... use the resource ...                                      │
│         ↓                                                       │
│   OBJECT DESTROYED (Destructor)                                 │
│         ↓                                                       │
│   Resource RELEASED (delete, close file, unlock mutex)          │
│                                                                 │
│   ✅ No manual cleanup needed!                                  │
│   ✅ Works even if exceptions occur!                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### The Problem RAII Solves

```cpp
// ❌ Without RAII — Easy to forget cleanup!
void process() {
    int* data = new int[1000];
    
    // ... lots of code ...
    
    if (error) {
        return;  // 💥 MEMORY LEAK! Forgot to delete!
    }
    
    // ... more code ...
    
    delete[] data;  // Easy to forget!
}
```

```cpp
// ✅ With RAII — Automatic cleanup!
void process() {
    std::vector<int> data(1000);  // Acquires memory in constructor
    
    // ... lots of code ...
    
    if (error) {
        return;  // ✅ vector's destructor runs automatically!
    }
    
    // ... more code ...
    
}  // ✅ Destructor releases memory automatically!
```

### Constructor/Destructor Symmetry

| Constructor | Destructor |
|-------------|------------|
| `new` | `delete` |
| `open()` | `close()` |
| `lock()` | `unlock()` |
| Acquire | Release |

---

## 📦 Dynamic Memory in Classes: VectorInt Example

Let's build a simplified `std::vector<int>` that uses RAII.

### Class Design

```cpp
class VectorInt {
private:
    int* array_;      // Pointer to dynamic array
    size_t size_;     // Number of elements used
    size_t capacity_; // Total slots available

public:
    // Constructor — ACQUIRE resource
    VectorInt(size_t size, int init_val = 0);
    
    // Destructor — RELEASE resource
    ~VectorInt();
    
    // Accessors
    size_t size() const { return size_; }
    size_t capacity() const { return capacity_; }
    
    // Element access
    int& operator[](size_t index) { return array_[index]; }
    const int& operator[](size_t index) const { return array_[index]; }
    
    // Modifier
    VectorInt& push_back(int value);

private:
    void resize(size_t new_capacity);
    void grow() { resize(capacity_ * 2); }
};
```

### Constructor — Acquire Resource

```cpp
VectorInt::VectorInt(size_t size, int init_val)
    : array_{nullptr},           // Start with null (safe)
      size_{size},
      capacity_{size * 2}        // Extra space for growth
{
    // Acquire the resource (dynamic array)
    try {
        array_ = new int[capacity_];
    } catch (const std::bad_alloc& e) {
        // If allocation fails, nothing to clean up
        throw;  // Re-throw the exception
    }
    
    // Initialize all elements
    for (size_t i = 0; i < size_; ++i) {
        array_[i] = init_val;
    }
}
```

### Destructor — Release Resource

```cpp
VectorInt::~VectorInt() {
    delete[] array_;  // Release the dynamic array
    // That's it! Simple and automatic!
}
```

> 💡 **Key Point:** Users of `VectorInt` never call `new` or `delete`!

### Push Back Implementation

```cpp
VectorInt& VectorInt::push_back(int value) {
    // Check if we need more space
    if (size_ == capacity_) {
        grow();  // Double the capacity
    }
    
    // Add element at the end
    array_[size_] = value;
    ++size_;
    
    return *this;  // Allow chaining: v.push_back(1).push_back(2);
}
```

### Resize Implementation

```cpp
void VectorInt::resize(size_t new_capacity) {
    // 1. Create new array
    int* new_array = new int[new_capacity];
    
    // 2. Determine how many elements to copy
    size_t num_to_copy = (new_capacity > size_) ? size_ : new_capacity;
    
    // 3. Copy old data to new array
    for (size_t i = 0; i < num_to_copy; ++i) {
        new_array[i] = array_[i];
    }
    
    // 4. Delete old array
    delete[] array_;
    
    // 5. Update pointer and capacity
    array_ = new_array;
    capacity_ = new_capacity;
    size_ = num_to_copy;
}
```

### Resize Visualization

```
BEFORE push_back(11) — size == capacity, must grow!

array_ ────► [0][2]     size_ = 2, capacity_ = 2
              ↑  ↑
              0  1

STEP 1: Create new array (capacity × 2)
new_array ──► [ ][ ][ ][ ]

STEP 2-3: Copy elements
new_array ──► [0][2][ ][ ]

STEP 4: Delete old array
array_ ────► (deleted)

STEP 5: Update pointer, add new element
array_ ────► [0][2][11][ ]   size_ = 3, capacity_ = 4
```

### Using VectorInt

```cpp
int main() {
    VectorInt v(2, 0);   // Constructor: array = [0][0], size=2, cap=4
    
    v.push_back(7);      // array = [0][0][7][ ], size=3
    v.push_back(8);      // array = [0][0][7][8], size=4, cap=4
    v.push_back(9);      // RESIZE! array = [0][0][7][8][9][ ][ ][ ]
    
    for (size_t i = 0; i < v.size(); ++i) {
        std::cout << v[i] << " ";
    }
    
    return 0;
}  // Destructor automatically deletes array! ✅
```

---

## 🧠 Smart Pointers — The Modern Solution

Smart pointers are **RAII wrappers** for raw pointers.

```cpp
#include <memory>  // Required header

// Smart pointer types:
std::unique_ptr<T>  // Single owner (most common)
std::shared_ptr<T>  // Multiple owners (reference counted)
std::weak_ptr<T>    // Non-owning observer
```

---

## 🔒 `std::unique_ptr` — Single Ownership

**One owner.** When owner dies, object dies.

### Basic Usage

```cpp
#include <iostream>
#include <memory>

int main() {
    // Create unique_ptr (preferred way)
    std::unique_ptr<int> p = std::make_unique<int>(42);
    
    std::cout << *p << std::endl;  // 42
    
    // No delete needed! Destructor handles it.
}
```

### How It Works Internally

```cpp
// Simplified concept of unique_ptr
template<typename T>
class UniquePtr {
    T* raw_ptr_;
public:
    UniquePtr(T* p) : raw_ptr_(p) {}
    ~UniquePtr() { delete raw_ptr_; }  // Automatic cleanup!
    
    T& operator*() { return *raw_ptr_; }
    T* get() { return raw_ptr_; }
};
```

### Cannot Copy — Can Only Move

```cpp
std::unique_ptr<int> p1 = std::make_unique<int>(10);

// std::unique_ptr<int> p2 = p1;  // ❌ COMPILE ERROR!
// Copying would mean two owners — not allowed!

std::unique_ptr<int> p2 = std::move(p1);  // ✅ Transfer ownership

// After move:
// p1 is now nullptr
// p2 owns the object
```

```
BEFORE move:
p1 ────────► [10]
p2           (empty)

AFTER move:
p1           (nullptr)
p2 ────────► [10]
```

### Passing unique_ptr to Functions

```cpp
// Transfer ownership INTO function
void take_ownership(std::unique_ptr<int> p) {
    std::cout << *p << std::endl;
}  // p destroyed here, object deleted

// Read-only access (no ownership transfer)
void read_only(const int* p) {
    std::cout << *p << std::endl;
}

int main() {
    auto p = std::make_unique<int>(42);
    
    read_only(p.get());           // Pass raw pointer (no transfer)
    take_ownership(std::move(p)); // Transfer ownership
    
    // p is now nullptr!
}
```

### Returning unique_ptr from Functions

```cpp
std::unique_ptr<int> create_number() {
    return std::make_unique<int>(100);  // Ownership moves to caller
}

int main() {
    auto num = create_number();  // Caller now owns it
    std::cout << *num << std::endl;
}  // Automatically deleted
```

### unique_ptr with Arrays

```cpp
// For arrays, use unique_ptr with []
std::unique_ptr<int[]> arr = std::make_unique<int[]>(10);

arr[0] = 42;
arr[1] = 100;

// Automatically calls delete[] when destroyed
```

---

## 🔗 `std::shared_ptr` — Shared Ownership

**Multiple owners.** Object deleted when **last** owner dies.

### Basic Usage

```cpp
#include <memory>
#include <iostream>

int main() {
    std::shared_ptr<int> p1 = std::make_shared<int>(42);
    
    {
        std::shared_ptr<int> p2 = p1;  // ✅ Copying is allowed!
        std::cout << p1.use_count() << std::endl;  // 2
    }  // p2 destroyed, count = 1
    
    std::cout << p1.use_count() << std::endl;  // 1
    
}  // p1 destroyed, count = 0, object deleted
```

### Reference Counting

```
std::shared_ptr<int> p1 = std::make_shared<int>(42);
                                    ↓
                              ┌─────────┐
                              │   42    │  ref_count = 1
                              └─────────┘
                                    ↑
                                   p1

std::shared_ptr<int> p2 = p1;
                                    ↓
                              ┌─────────┐
                              │   42    │  ref_count = 2
                              └─────────┘
                                 ↑    ↑
                                p1   p2

// When p2 goes out of scope: ref_count = 1
// When p1 goes out of scope: ref_count = 0 → DELETE!
```

### ⚠️ Circular Reference Problem

```cpp
struct Node {
    std::shared_ptr<Node> next;
};

int main() {
    auto a = std::make_shared<Node>();
    auto b = std::make_shared<Node>();
    
    a->next = b;  // a owns b
    b->next = a;  // b owns a ← CIRCULAR!
    
}  // MEMORY LEAK! Neither can reach count 0!
```

```
     a ──────────► Node A
                    │
                    │ next
                    ▼
     b ──────────► Node B
                    │
                    │ next
                    └──────► back to Node A!

ref_count of A = 2 (a + B's next)
ref_count of B = 2 (b + A's next)

When a,b die: counts become 1, never 0!
```

---

## 👀 `std::weak_ptr` — Non-Owning Observer

**Does NOT increase reference count.** Breaks circular references.

### Breaking the Cycle

```cpp
struct Node {
    std::shared_ptr<Node> next;
    std::weak_ptr<Node> prev;    // ← weak_ptr breaks the cycle!
};
```

### Using weak_ptr

```cpp
std::shared_ptr<int> sp = std::make_shared<int>(42);
std::weak_ptr<int> wp = sp;  // Does NOT increase count

// To use weak_ptr, must convert to shared_ptr
if (auto locked = wp.lock()) {  // Returns shared_ptr or nullptr
    std::cout << *locked << std::endl;
} else {
    std::cout << "Object no longer exists!" << std::endl;
}
```

### Checking if Object Still Exists

```cpp
std::weak_ptr<int> wp;

{
    auto sp = std::make_shared<int>(100);
    wp = sp;
    std::cout << wp.expired() << std::endl;  // false
}  // sp destroyed

std::cout << wp.expired() << std::endl;  // true — object is gone!
```

---

## 📊 Smart Pointer Comparison

| Feature | `unique_ptr` | `shared_ptr` | `weak_ptr` |
|---------|--------------|--------------|------------|
| Owners | 1 | Many | 0 (observer) |
| Can copy? | ❌ No | ✅ Yes | ✅ Yes |
| Can move? | ✅ Yes | ✅ Yes | ✅ Yes |
| Overhead | Minimal | Reference counting | Minimal |
| Use when | Default choice | Shared ownership needed | Break cycles |

### Decision Flowchart

```
Do you need a pointer?
        │
        ▼
   Does it own the object?
        │
   ┌────┴────┐
  YES       NO
   │         │
   ▼         ▼
Single or  Use raw pointer
shared?    (non-owning)
   │
┌──┴──┐
│     │
▼     ▼
ONE  MANY
│     │
▼     ▼
unique_ptr  shared_ptr
```

---

## 🎯 When to Use What

### Use `unique_ptr` When:
- **Default choice** for heap allocation
- Single owner is clear
- Ownership may transfer but not share

```cpp
class FileHandler {
    std::unique_ptr<std::fstream> file_;
public:
    FileHandler(const std::string& path) 
        : file_(std::make_unique<std::fstream>(path)) {}
    // Destructor automatically closes file!
};
```

### Use `shared_ptr` When:
- Multiple parts of code need to own the object
- Object lifetime is unclear
- Building complex data structures (graphs, caches)

```cpp
class Cache {
    std::map<std::string, std::shared_ptr<Data>> cache_;
public:
    std::shared_ptr<Data> get(const std::string& key) {
        return cache_[key];  // Caller shares ownership
    }
};
```

### Use Raw Pointer When:
- **Not owning** the memory
- Short-lived access
- Interacting with C libraries

```cpp
void process(const Widget* w) {  // Not owning!
    if (w) {
        w->doSomething();
    }
}
```

---

## 🔑 Key Takeaways

### RAII Principle

```
Constructor = Acquire resources
Destructor = Release resources
```

### Smart Pointer Rules

| Type | Copy | Move | Ownership |
|------|------|------|-----------|
| `unique_ptr` | ❌ | ✅ | Single |
| `shared_ptr` | ✅ | ✅ | Shared |
| `weak_ptr` | ✅ | ✅ | None |

### Golden Rules

1. **Prefer `unique_ptr`** — upgrade to `shared_ptr` only if needed
2. **Use `make_unique` and `make_shared`** — safer than raw `new`
3. **Never use `new`/`delete` directly** in modern C++ (usually)
4. **Pass raw pointer or reference** for non-owning access
5. **Use `weak_ptr`** to break circular references

### Modern C++ Memory Safety

```cpp
// ❌ Old style (dangerous)
int* p = new int(42);
// ... forget to delete? LEAK!
// ... exception thrown? LEAK!
delete p;

// ✅ Modern style (safe)
auto p = std::make_unique<int>(42);
// Automatically cleaned up, always.
```

### Quick Reference

```cpp
// Create
auto up = std::make_unique<Type>(args);
auto sp = std::make_shared<Type>(args);

// Access
*up         // Dereference
up->member  // Member access
up.get()    // Raw pointer (non-owning)

// Transfer ownership
auto up2 = std::move(up);  // up becomes nullptr

// Check
if (up) { ... }  // Check if not null
sp.use_count()   // Reference count (shared_ptr)
wp.expired()     // Check if shared object still exists
wp.lock()        // Get shared_ptr from weak_ptr
```

