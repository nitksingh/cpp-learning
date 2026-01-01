# 🎓 Templates: Function & Class Templates

---

## 🤔 Why Templates?

C++ requires specific types, but often the **logic is identical** for different types.

```cpp
// Same logic, three functions! 😫
int MinimumValue(int a, int b) {
    return (b > a) ? a : b;
}

double MinimumValue(double a, double b) {
    return (b > a) ? a : b;  // Same code!
}

char MinimumValue(char a, char b) {
    return (b > a) ? a : b;  // Same code again!
}
```

**Problem:** Copy-paste for every type — tedious and error-prone!

**Solution:** Create a **template** — a blueprint the compiler uses to generate code for any type.

```
┌─────────────────────────────────────────────────────────────────┐
│                    TEMPLATE CONCEPT                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Template (Blueprint):                                         │
│   ┌─────────────────────────────────────┐                       │
│   │  T MinimumValue(T a, T b) {         │                       │
│   │      return (b > a) ? a : b;        │                       │
│   │  }                                  │                       │
│   └─────────────────────────────────────┘                       │
│                     │                                           │
│         ┌──────────┼──────────┐                                 │
│         ▼          ▼          ▼                                 │
│   ┌──────────┐ ┌──────────┐ ┌──────────┐                        │
│   │ T = int  │ │ T = double│ │ T = char │  Compiler generates   │
│   │ version  │ │ version  │ │ version  │  code for each type   │
│   └──────────┘ └──────────┘ └──────────┘                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📝 Function Template Syntax

### Declaration

```cpp
template <typename T>   // Announce: T is a placeholder for a type
T MinimumValue(T a, T b);
```

### Definition

```cpp
template <typename T>
T MinimumValue(T a, T b) {
    return (b > a) ? a : b;
}
```

### Breaking Down the Syntax

```
template <typename T>    ← Template header: T is a type parameter
    │         │
    │         └── T = placeholder name (convention: T, U, V...)
    └── Keyword announcing a template

T MinimumValue(T a, T b) ← Use T as return type and parameter types
```

> 💡 **Note:** `typename` and `class` are interchangeable here. We prefer `typename`.

---

## 🔧 Using Function Templates

### Explicit Type Specification

```cpp
template <typename T>
T MinimumValue(T a, T b) {
    return (b > a) ? a : b;
}

int main() {
    // Explicitly specify the type in angle brackets
    int result1 = MinimumValue<int>(5, 3);        // T = int
    double result2 = MinimumValue<double>(2.5, 3.7);  // T = double
    char result3 = MinimumValue<char>('a', 'z');  // T = char
}
```

### Template Instantiation

When you use `MinimumValue<int>`, the compiler **generates**:

```cpp
// Compiler generates this code for you!
int MinimumValue(int a, int b) {
    return (b > a) ? a : b;
}
```

---

## 🔍 Template Argument Deduction

The compiler can **infer** T from the arguments — no need to specify explicitly!

```cpp
template <typename T>
T MinimumValue(T a, T b) {
    return (b > a) ? a : b;
}

int main() {
    // Compiler DEDUCES T from arguments
    int x = MinimumValue(5, 3);        // T deduced as int
    double y = MinimumValue(2.5, 3.7); // T deduced as double
    char z = MinimumValue('a', 'z');   // T deduced as char
    
    // Same as:
    // MinimumValue<int>(5, 3);
    // MinimumValue<double>(2.5, 3.7);
    // MinimumValue<char>('a', 'z');
}
```

### ⚠️ Deduction Fails with Mixed Types

```cpp
MinimumValue(2, 2.4);  // ❌ ERROR! T = int or double?

// Error: T cannot be deduced as BOTH int AND double
```

### Solutions for Mixed Types

```cpp
// Solution 1: Cast arguments to same type
MinimumValue(static_cast<double>(2), 2.4);  // T = double

// Solution 2: Explicitly specify T
MinimumValue<double>(2, 2.4);  // T = double

// Solution 3: Use multiple type parameters
template <typename T1, typename T2>
auto MinimumValue(T1 a, T2 b) {
    return (b > a) ? a : b;  // auto deduces return type
}
```

---

## ⚙️ Type Requirements

The type T must support **all operations used** in the template.

```cpp
template <typename T>
T MinimumValue(T a, T b) {
    return (b > a) ? a : b;  // Requires: operator>
}
```

```cpp
struct Point { int x, y; };

Point p1{1, 2}, p2{3, 4};
MinimumValue(p1, p2);  // ❌ ERROR! Point has no operator>
```

**Fix:** Define the required operator for your type:

```cpp
struct Point { 
    int x, y;
    bool operator>(const Point& other) const {
        return (x + y) > (other.x + other.y);
    }
};

MinimumValue(p1, p2);  // ✅ Now works!
```

---

## 📁 Templates and Header Files

### ⚠️ Key Rule: Both Declaration AND Definition in Header!

Unlike regular functions, templates must be **fully defined** where they're used.

```cpp
// ❌ WRONG: Definition in .cpp file
// math.hpp
template <typename T>
T MinimumValue(T a, T b);  // Declaration only

// math.cpp
template <typename T>
T MinimumValue(T a, T b) { return (b > a) ? a : b; }  // ❌ Won't work!

// main.cpp
#include "math.hpp"
MinimumValue(5, 3);  // ❌ Linker error! Definition not visible
```

```cpp
// ✅ CORRECT: Everything in header file
// math.hpp
template <typename T>
T MinimumValue(T a, T b) {  // Full definition in header
    return (b > a) ? a : b;
}

// main.cpp
#include "math.hpp"
MinimumValue(5, 3);  // ✅ Works! Compiler can generate code
```

### Why?

The compiler needs to see the **complete template** to generate code for each type. It can't wait until link time.

---

## 📦 Example: Overloading `<<` for Vector

Add elements to a vector using `<<` operator:

```cpp
#include <iostream>
#include <vector>

// Function template: works for any vector<T>
template <typename T>
std::vector<T>& operator<<(std::vector<T>& vec, const T& value) {
    vec.push_back(value);
    return vec;  // Return reference for chaining
}

int main() {
    std::vector<int> nums;
    
    nums << 2;           // Add 2
    nums << 7 << 11;     // Chain: add 7, then 11
    
    // nums is now: [2, 7, 11]
    
    for (int n : nums) {
        std::cout << n << " ";
    }
    // Output: 2 7 11
    
    return 0;
}
```

---

## 🏗️ Class Templates

Classes can also be parameterized by types!

### Motivation: Vector for Different Types

```cpp
// Without templates: separate class for each type 😫
class VectorDouble {
    double* elem;
    // ...
};

class VectorInt {
    int* elem;
    // ...
};

class VectorChar {
    char* elem;
    // ...
};
```

### With Class Template

```cpp
template <typename T>
class Vector {
    T* elem;        // T can be any type!
    size_t size;
    // ...
};

// Now create vectors of any type:
Vector<int> vi;
Vector<double> vd;
Vector<std::string> vs;
```

---

## 📝 Class Template Syntax

### Declaration and Definition

```cpp
template <typename T>  // Template header
class Stack {
public:
    Stack();
    void Push(const T& element);  // Use T in member functions
    T Pop();
    bool IsEmpty() const;
    
private:
    T elements_[100];  // Use T for data members
    int top_ = -1;
};
```

### Member Function Definitions (Outside Class)

```cpp
// Each member function is also a template!
template <typename T>
void Stack<T>::Push(const T& element) {
    elements_[++top_] = element;
}

template <typename T>
T Stack<T>::Pop() {
    return elements_[top_--];
}

template <typename T>
bool Stack<T>::IsEmpty() const {
    return top_ < 0;
}
```

### Key Points

```
template <typename T>     ← Required for EACH member function
void Stack<T>::Push(...)  ← Use Stack<T> as the class name
           ↑
    Type is Stack<T>, not just Stack
```

---

## 🔧 Using Class Templates

### No Argument Deduction for Classes!

Unlike function templates, you **must** specify the type explicitly.

```cpp
Stack<int> intStack;        // Stack of integers
Stack<double> doubleStack;  // Stack of doubles
Stack<std::string> strStack; // Stack of strings

intStack.Push(42);
doubleStack.Push(3.14);
strStack.Push("hello");
```

### Each Instantiation is a Distinct Type

```cpp
Stack<int> s1;
Stack<double> s2;

s1 = s2;  // ❌ ERROR! Different types!
```

---

## 📁 Class Templates and Header Files

**Same rule:** Everything goes in the header file!

```cpp
// stack.hpp — ALL of this in the header file

template <typename T>
class Stack {
public:
    Stack() : top_(-1) {}
    
    void Push(const T& element) {
        elements_[++top_] = element;
    }
    
    T Pop() {
        return elements_[top_--];
    }
    
    bool IsEmpty() const {
        return top_ < 0;
    }
    
private:
    T elements_[100];
    int top_;
};

// No .cpp file needed for templates!
```

---

## 💻 Complete Example: Stack Class Template

```cpp
#include <iostream>
#include <stdexcept>

// ============================================
// Stack Class Template
// ============================================
template <typename T>
class Stack {
public:
    // Constructor: empty stack
    Stack() : top_(-1) {}
    
    // Push element onto stack
    void Push(const T& element) {
        if (top_ >= 99) {
            throw std::overflow_error("Stack overflow");
        }
        elements_[++top_] = element;
    }
    
    // Pop element from stack
    T Pop() {
        if (IsEmpty()) {
            throw std::underflow_error("Stack underflow");
        }
        return elements_[top_--];
    }
    
    // Peek at top element
    const T& Top() const {
        if (IsEmpty()) {
            throw std::underflow_error("Stack is empty");
        }
        return elements_[top_];
    }
    
    // Check if empty
    bool IsEmpty() const {
        return top_ < 0;
    }
    
    // Get size
    int Size() const {
        return top_ + 1;
    }
    
private:
    T elements_[100];  // Fixed-size array (for simplicity)
    int top_;          // Index of top element (-1 if empty)
};

// ============================================
// Main: Test the Stack
// ============================================
int main() {
    // Stack of integers
    Stack<int> intStack;
    intStack.Push(10);
    intStack.Push(20);
    intStack.Push(30);
    
    std::cout << "Int stack: ";
    while (!intStack.IsEmpty()) {
        std::cout << intStack.Pop() << " ";
    }
    std::cout << std::endl;
    // Output: 30 20 10 (LIFO order)
    
    // Stack of strings
    Stack<std::string> strStack;
    strStack.Push("Hello");
    strStack.Push("World");
    
    std::cout << "String stack top: " << strStack.Top() << std::endl;
    // Output: World
    
    return 0;
}
```

---

## 💻 Complete Example: PriorityQueue Class Template

A priority queue that maintains elements in **sorted order** using a linked list.

```cpp
#include <iostream>
#include <stdexcept>

// ============================================
// Node: Template struct for linked list
// ============================================
template <typename T>
struct Node {
    T data;
    Node<T>* next;
    
    // Constructor with data only
    Node(T data) : data(data), next(nullptr) {}
    
    // Constructor with data and next pointer
    Node(T data, Node<T>* next) : data(data), next(next) {}
};

// ============================================
// PriorityQueue: Maintains sorted linked list
// ============================================
template <typename T>
class PriorityQueue {
public:
    // Default constructor: empty queue
    PriorityQueue() : head_(nullptr) {}
    
    // Rule of Three: Delete copy operations (we have destructor)
    PriorityQueue(const PriorityQueue<T>& rhs) = delete;
    PriorityQueue<T>& operator=(const PriorityQueue<T>& rhs) = delete;
    
    // Destructor: free all nodes
    ~PriorityQueue() {
        while (head_ != nullptr) {
            Node<T>* next = head_->next;
            delete head_;
            head_ = next;
        }
    }
    
    // Enqueue: Insert in ascending sorted order
    void Enqueue(const T& data) {
        Node<T>* new_node = new Node<T>(data);
        
        // Case 1: Empty list or insert at beginning
        if (head_ == nullptr || data <= head_->data) {
            new_node->next = head_;
            head_ = new_node;
            return;
        }
        
        // Case 2: Find insertion point (keep sorted)
        Node<T>* current = head_;
        while (current->next != nullptr && current->next->data < data) {
            current = current->next;
        }
        
        // Insert after current
        new_node->next = current->next;
        current->next = new_node;
    }
    
    // Dequeue: Remove and return smallest element (head)
    T Dequeue() {
        if (head_ == nullptr) {
            throw std::runtime_error("Queue is empty");
        }
        
        T data = head_->data;      // Copy data
        Node<T>* old_head = head_; // Save pointer
        head_ = head_->next;       // Move head forward
        delete old_head;           // Free old node
        
        return data;
    }
    
    // Peek at front element
    const T& Front() const {
        if (head_ == nullptr) {
            throw std::runtime_error("Queue is empty");
        }
        return head_->data;
    }
    
    // Check if empty
    bool IsEmpty() const {
        return head_ == nullptr;
    }
    
    // Print queue (for debugging)
    void Print() const {
        std::cout << "[ ";
        Node<T>* current = head_;
        while (current != nullptr) {
            std::cout << current->data;
            if (current->next != nullptr) {
                std::cout << " <- ";
            }
            current = current->next;
        }
        std::cout << " ]" << std::endl;
    }
    
private:
    Node<T>* head_;  // Pointer to first (smallest) element
};

// ============================================
// Main: Test the PriorityQueue
// ============================================
int main() {
    std::cout << "=== PriorityQueue<int> ===" << std::endl;
    
    PriorityQueue<int> pq;
    
    // Enqueue in random order
    pq.Enqueue(30);
    pq.Print();  // [ 30 ]
    
    pq.Enqueue(10);
    pq.Print();  // [ 10 <- 30 ]
    
    pq.Enqueue(20);
    pq.Print();  // [ 10 <- 20 <- 30 ]
    
    pq.Enqueue(5);
    pq.Print();  // [ 5 <- 10 <- 20 <- 30 ]
    
    pq.Enqueue(25);
    pq.Print();  // [ 5 <- 10 <- 20 <- 25 <- 30 ]
    
    // Dequeue (always returns smallest)
    std::cout << "\nDequeue order: ";
    while (!pq.IsEmpty()) {
        std::cout << pq.Dequeue() << " ";
    }
    std::cout << std::endl;
    // Output: 5 10 20 25 30 (sorted!)
    
    std::cout << "\n=== PriorityQueue<std::string> ===" << std::endl;
    
    PriorityQueue<std::string> spq;
    spq.Enqueue("banana");
    spq.Enqueue("apple");
    spq.Enqueue("cherry");
    spq.Print();  // [ apple <- banana <- cherry ]
    
    std::cout << "Front: " << spq.Front() << std::endl;  // apple
    
    return 0;
}  // Destructors clean up automatically!
```

### Expected Output

```
=== PriorityQueue<int> ===
[ 30 ]
[ 10 <- 30 ]
[ 10 <- 20 <- 30 ]
[ 5 <- 10 <- 20 <- 30 ]
[ 5 <- 10 <- 20 <- 25 <- 30 ]

Dequeue order: 5 10 20 25 30 

=== PriorityQueue<std::string> ===
[ apple <- banana <- cherry ]
Front: apple
```

---

## 🔑 Key Takeaways

### Function Templates

```cpp
template <typename T>
T MinValue(T a, T b) {
    return (a < b) ? a : b;
}

// Usage: compiler deduces T
MinValue(5, 3);      // T = int
MinValue(2.5, 3.7);  // T = double
```

### Class Templates

```cpp
template <typename T>
class Container {
    T data_;
public:
    void Set(const T& val) { data_ = val; }
    T Get() const { return data_; }
};

// Usage: must specify T explicitly
Container<int> c1;
Container<std::string> c2;
```

### Template Rules Summary

| Aspect | Function Template | Class Template |
|--------|------------------|----------------|
| Type deduction | ✅ Yes (from args) | ❌ No (must specify) |
| Definition location | Header file | Header file |
| Member functions | Each is a template | Each is a template |
| Instantiation | When called | When declared |

### Checklist for Templates

1. ✅ Use `template <typename T>` header
2. ✅ Put **everything** in header file (.hpp)
3. ✅ Type T must support all operations used
4. ✅ For class templates, use `ClassName<T>::` in member definitions
5. ✅ Each instantiation creates a **new type**

### Common Mistakes

```cpp
// ❌ Wrong: Definition in .cpp file
template <typename T>
T Max(T a, T b);  // Declaration in .hpp
// Definition in .cpp won't work!

// ❌ Wrong: Missing template header for member function
void Stack<T>::Push(T val) { }  // Missing: template <typename T>

// ❌ Wrong: No type specified for class template
Stack s;  // Must be: Stack<int> s;

// ❌ Wrong: Mixed types without explicit specification
MinValue(5, 3.14);  // T = int or double?
```

