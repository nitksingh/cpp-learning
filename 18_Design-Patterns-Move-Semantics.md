# 🎓 Design Patterns, Iterators & Move Semantics

---

## 🎨 What is a Design Pattern?

A **design pattern** is a reusable solution to a commonly occurring problem in software design.

> "Each pattern describes a problem which occurs over and over again, and then describes the core of the solution to that problem in such a way that you can use this solution a million times over without ever doing it the same way twice."
> — Christopher Alexander

### Four Essential Elements

```
┌─────────────────────────────────────────────────────────────────┐
│              DESIGN PATTERN ELEMENTS                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. NAME        → A handle to describe the pattern             │
│                    (e.g., "Strategy", "Observer")               │
│                                                                 │
│   2. PROBLEM     → When to apply it                             │
│                    (What problem does it solve?)                │
│                                                                 │
│   3. SOLUTION    → Abstract description of how to solve it      │
│                    (Classes, relationships, responsibilities)  │
│                                                                 │
│   4. CONSEQUENCES→ Trade-offs and results of using it           │
│                    (Space/time, flexibility, complexity)        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Why Use Patterns?

- **Vocabulary**: Communicate complex ideas with a single name
- **Reuse**: Apply proven solutions instead of reinventing
- **Best practices**: Leverage community knowledge

---

## 🔬 Micro-Patterns

Simple, common patterns you've probably used without naming them.

### 1. Most Wanted Holder

**Problem**: Find the "best" element in a collection (max, min, etc.)

**Solution**: Initialize with first element, compare against rest.

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> scores = {85, 92, 78, 96, 88};
    
    // ═══════════════════════════════════════════════════════
    // MOST WANTED HOLDER PATTERN
    // Find the maximum score
    // ═══════════════════════════════════════════════════════
    
    int most_wanted = scores[0];  // Step 1: Initialize with first element
    
    for (size_t i = 1; i < scores.size(); i++) {  // Step 2: Compare with rest
        if (scores[i] > most_wanted) {  // Step 3: Update if better
            most_wanted = scores[i];
        }
    }
    
    std::cout << "Highest score: " << most_wanted << "\n";  // Output: 96
    
    return 0;
}
```

### 2. One Way Flag

**Problem**: Check if ALL elements satisfy a property (or if ANY violates it).

**Solution**: Start with assumption (true/false), flip if violated.

```cpp
#include <iostream>
#include <vector>

bool IsPositive(int n) { return n > 0; }

int main() {
    std::vector<int> numbers = {5, 3, 8, 2, 7};
    
    // ═══════════════════════════════════════════════════════
    // ONE WAY FLAG PATTERN
    // Check if ALL numbers are positive
    // ═══════════════════════════════════════════════════════
    
    bool all_positive = true;  // Step 1: Assume true
    
    for (int num : numbers) {
        if (!IsPositive(num)) {  // Step 2: Check each element
            all_positive = false;  // Step 3: Flip flag if violated
            break;  // No need to check further
        }
    }
    
    std::cout << "All positive? " << (all_positive ? "Yes" : "No") << "\n";
    
    return 0;
}
```

### 3. Follower

**Problem**: Compare adjacent elements or need to remember the previous element.

**Solution**: Keep a "follower" variable that trails behind current.

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 3, 5, 7, 9};
    
    // ═══════════════════════════════════════════════════════
    // FOLLOWER PATTERN
    // Check if array is in ascending order
    // ═══════════════════════════════════════════════════════
    
    bool is_sorted = true;
    int follower = numbers[0];  // Step 1: Initialize follower
    
    for (size_t i = 1; i < numbers.size(); i++) {
        if (follower > numbers[i]) {  // Step 2: Compare with current
            is_sorted = false;
            break;
        }
        follower = numbers[i];  // Step 3: Move follower forward
    }
    
    std::cout << "Is sorted? " << (is_sorted ? "Yes" : "No") << "\n";
    
    return 0;
}
```

**Linked List Application:**
```cpp
// Follower pattern for linked list insertion
Node* prev = nullptr;  // Follower
Node* curr = head_;    // Current

while (curr != nullptr && curr->data < value) {
    prev = curr;       // Follower catches up
    curr = curr->next; // Current moves ahead
}
// Now prev points to where we should insert!
```

---

## 🎯 Strategy Pattern

**Problem**: You need different algorithms/behaviors that can be swapped at runtime.

**Solution**: Define a family of algorithms, encapsulate each one, make them interchangeable.

```
┌─────────────────────────────────────────────────────────────────┐
│              STRATEGY PATTERN STRUCTURE                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌─────────────┐         ┌──────────────────────┐             │
│   │   Context   │────────▶│  Strategy (abstract) │             │
│   │  (Duck)     │         │  + DoAlgorithm()     │             │
│   └─────────────┘         └──────────┬───────────┘             │
│         │                            │                          │
│         │ has-a              ┌───────┴───────┐                 │
│         │                    │               │                  │
│         │            ┌───────┴──────┐ ┌──────┴───────┐         │
│         └───────────▶│ ConcreteA    │ │ ConcreteB    │         │
│                      │ (LoudQuack)  │ │ (QuietQuack) │         │
│                      └──────────────┘ └──────────────┘         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Example: Duck Quacking Behavior

```cpp
#include <iostream>
#include <string>
#include <memory>

// ═══════════════════════════════════════════════════════════════
// STRATEGY: Abstract base class defining the interface
// ═══════════════════════════════════════════════════════════════
class QuackBehavior {
public:
    virtual ~QuackBehavior() = default;
    virtual std::string Quack() const = 0;  // Pure virtual - must implement
};

// ═══════════════════════════════════════════════════════════════
// CONCRETE STRATEGIES: Different implementations
// ═══════════════════════════════════════════════════════════════
class LoudQuack : public QuackBehavior {
public:
    std::string Quack() const override {
        return "QUACK!!!";  // Loud quacking
    }
};

class QuietQuack : public QuackBehavior {
public:
    std::string Quack() const override {
        return "quack...";  // Quiet quacking
    }
};

class NoQuack : public QuackBehavior {
public:
    std::string Quack() const override {
        return "(silence)";  // Rubber ducks don't quack
    }
};

// ═══════════════════════════════════════════════════════════════
// CONTEXT: The class that uses a strategy
// ═══════════════════════════════════════════════════════════════
class Duck {
public:
    // Default: no quack behavior
    Duck() : quack_behavior_(new NoQuack()) {}
    
    // Parameterized: set initial behavior
    Duck(QuackBehavior* behavior) : quack_behavior_(behavior) {}
    
    virtual ~Duck() {
        delete quack_behavior_;
    }
    
    // Can change behavior at runtime!
    void SetQuackBehavior(QuackBehavior* behavior) {
        delete quack_behavior_;
        quack_behavior_ = behavior;
    }
    
    // Delegate quacking to the strategy
    std::string PerformQuack() const {
        return quack_behavior_->Quack();
    }
    
protected:
    QuackBehavior* quack_behavior_;
};

// ═══════════════════════════════════════════════════════════════
// CONCRETE DUCKS: Different types with default behaviors
// ═══════════════════════════════════════════════════════════════
class MallardDuck : public Duck {
public:
    MallardDuck() : Duck(new QuietQuack()) {}  // Mallards quack quietly
};

class RubberDuck : public Duck {
public:
    RubberDuck() : Duck(new NoQuack()) {}  // Rubber ducks are silent
};

int main() {
    MallardDuck mallard;
    RubberDuck rubber;
    
    std::cout << "Mallard: " << mallard.PerformQuack() << "\n";
    std::cout << "Rubber: " << rubber.PerformQuack() << "\n";
    
    // Change behavior at runtime!
    mallard.SetQuackBehavior(new LoudQuack());
    std::cout << "Mallard (now loud): " << mallard.PerformQuack() << "\n";
    
    return 0;
}
```

**Output:**
```
Mallard: quack...
Rubber: (silence)
Mallard (now loud): QUACK!!!
```

### When to Use Strategy Pattern

- Many related classes differ only in behavior
- You need different variants of an algorithm
- A class has multiple conditional behaviors

---

## 🔄 Iterators

### The Problem: Generic Algorithms

We want to write **one** `find` function that works with vectors, lists, trees, etc.

```
┌─────────────────────────────────────────────────────────────────┐
│              THE ITERATOR SOLUTION                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌────────────┐                      ┌────────────┐           │
│   │ Algorithms │◀────ITERATORS────────│ Containers │           │
│   │ (find,     │                      │ (vector,   │           │
│   │  sort...)  │                      │  list...)  │           │
│   └────────────┘                      └────────────┘           │
│                                                                 │
│   Algorithms don't know about containers                        │
│   Containers don't know about algorithms                        │
│   Iterators bridge the gap!                                     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### What is an Iterator?

An **iterator** is an abstraction of a pointer that:
- Points to an element in a container
- Can move to the next element
- Can access the current element's value

```
┌─────────────────────────────────────────────────────────────────┐
│              ITERATOR OPERATIONS                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   *iter        → Access current element (dereference)           │
│   ++iter       → Move to next element (prefix increment)        │
│   iter++       → Move to next, return old (postfix increment)   │
│   iter1 == iter2 → Check if pointing to same element           │
│   iter1 != iter2 → Check if pointing to different elements     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Begin and End

Every STL container provides:
- `begin()` → Iterator to **first** element
- `end()` → Iterator to **one past last** element

```
┌─────────────────────────────────────────────────────────────────┐
│              BEGIN AND END                                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   vector: [10] [20] [30] [40]  ???                              │
│            ↑                    ↑                               │
│          begin()              end()                             │
│                                                                 │
│   list:  [A] → [B] → [C] → nullptr                              │
│           ↑                    ↑                                │
│         begin()              end()                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Using STL Iterators

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> nums = {10, 20, 30, 40};
    
    // ═══════════════════════════════════════════════════════
    // Manual iteration with iterators
    // ═══════════════════════════════════════════════════════
    std::cout << "Manual: ";
    for (auto it = nums.begin(); it != nums.end(); ++it) {
        std::cout << *it << " ";  // Dereference to get value
    }
    std::cout << "\n";
    
    // ═══════════════════════════════════════════════════════
    // Range-based for loop (uses iterators internally!)
    // ═══════════════════════════════════════════════════════
    std::cout << "Range-based: ";
    for (int n : nums) {
        std::cout << n << " ";
    }
    std::cout << "\n";
    
    // ═══════════════════════════════════════════════════════
    // Using STL algorithm with iterators
    // ═══════════════════════════════════════════════════════
    auto result = std::find(nums.begin(), nums.end(), 30);
    
    if (result != nums.end()) {
        std::cout << "Found: " << *result << "\n";
    } else {
        std::cout << "Not found\n";
    }
    
    return 0;
}
```

**Output:**
```
Manual: 10 20 30 40 
Range-based: 10 20 30 40 
Found: 30
```

### Implementing a Custom Iterator

For a doubly linked list:

```cpp
#include <iostream>
#include <iterator>

// Forward declaration
template <typename T>
struct Node {
    T data;
    Node<T>* next;
    Node<T>* prev;
    
    Node(T val) : data(val), next(nullptr), prev(nullptr) {}
};

// ═══════════════════════════════════════════════════════════════
// CUSTOM ITERATOR for DoublyLinkedList
// ═══════════════════════════════════════════════════════════════
template <typename T>
class DLLIterator : public std::iterator<std::forward_iterator_tag, T> {
public:
    // Default constructor: points to nothing
    DLLIterator() : current_(nullptr) {}
    
    // Parameterized: points to a specific node
    DLLIterator(Node<T>* ptr) : current_(ptr) {}
    
    // DEREFERENCE: Return the DATA, not the node!
    T& operator*() {
        return current_->data;
    }
    
    // PREFIX INCREMENT: Move to next, return new position
    DLLIterator& operator++() {
        if (current_ != nullptr) {
            current_ = current_->next;
        }
        return *this;
    }
    
    // POSTFIX INCREMENT: Move to next, return OLD position
    DLLIterator operator++(int) {
        DLLIterator clone(*this);  // Save current state
        if (current_ != nullptr) {
            current_ = current_->next;
        }
        return clone;  // Return old state
    }
    
    // EQUALITY: Same node?
    bool operator==(const DLLIterator& other) const {
        return current_ == other.current_;
    }
    
    // INEQUALITY
    bool operator!=(const DLLIterator& other) const {
        return !(*this == other);
    }
    
private:
    Node<T>* current_;
};

// ═══════════════════════════════════════════════════════════════
// DOUBLY LINKED LIST with iterator support
// ═══════════════════════════════════════════════════════════════
template <typename T>
class DoublyLinkedList {
public:
    using Iterator = DLLIterator<T>;  // Type alias
    
    DoublyLinkedList() : head_(nullptr), tail_(nullptr) {}
    
    ~DoublyLinkedList() {
        while (head_ != nullptr) {
            Node<T>* temp = head_;
            head_ = head_->next;
            delete temp;
        }
    }
    
    void PushBack(T value) {
        Node<T>* new_node = new Node<T>(value);
        if (head_ == nullptr) {
            head_ = tail_ = new_node;
        } else {
            tail_->next = new_node;
            new_node->prev = tail_;
            tail_ = new_node;
        }
    }
    
    // Return iterator to first element
    Iterator begin() { return Iterator(head_); }
    
    // Return iterator to one past last (nullptr)
    Iterator end() { return Iterator(nullptr); }
    
private:
    Node<T>* head_;
    Node<T>* tail_;
};

int main() {
    DoublyLinkedList<int> list;
    list.PushBack(10);
    list.PushBack(20);
    list.PushBack(30);
    
    // Now we can use range-based for loop!
    std::cout << "List: ";
    for (int val : list) {
        std::cout << val << " ";
    }
    std::cout << "\n";
    
    // Or explicit iterator
    std::cout << "With iterator: ";
    for (auto it = list.begin(); it != list.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << "\n";
    
    return 0;
}
```

**Output:**
```
List: 10 20 30 
With iterator: 10 20 30 
```

---

## 🚀 Move Semantics

### L-values vs R-values

```
┌─────────────────────────────────────────────────────────────────┐
│              L-VALUES vs R-VALUES                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   L-VALUE:                        R-VALUE:                      │
│   - Has identifiable address      - Temporary, no address       │
│   - Lives beyond expression       - Dies at end of expression   │
│   - Can appear on LEFT of =       - Can only appear on RIGHT    │
│                                                                 │
│   int x = 10;     ← x is L-value, 10 is R-value                 │
│   x = x + 5;      ← x is L-value, (x + 5) is R-value            │
│                                                                 │
│   ERROR: (x + 5) = 20;  ← Can't assign to R-value!              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

```cpp
int x = 10;      // x is L-value (has address, persists)
int y = x + 5;   // (x + 5) is R-value (temporary result)

// x + 5 = 20;   // ERROR! Can't assign to R-value
```

### R-value References (&&)

C++11 introduced **R-value references** to bind to temporary objects:

```cpp
int x = 10;

int& lref = x;       // L-value reference: binds to L-value ✓
// int& lref2 = 10;  // ERROR: can't bind L-value ref to R-value

int&& rref = 20;     // R-value reference: binds to R-value ✓
// int&& rref2 = x;  // ERROR: can't bind R-value ref to L-value
```

### Why Move Semantics?

**Problem**: Copying large objects is expensive.

```
┌─────────────────────────────────────────────────────────────────┐
│              COPY vs MOVE                                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   COPY (expensive):                                             │
│                                                                 │
│   v1: [ptr]──▶[10][20][30]     v2: [ptr]──▶[10][20][30]         │
│                                     (allocate + copy all!)      │
│                                                                 │
│   MOVE (cheap):                                                 │
│                                                                 │
│   v1: [nullptr]                v2: [ptr]──▶[10][20][30]         │
│       (just steal the pointer!)                                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Rule of Five

If you define **any** of these, define **all five**:

```
┌─────────────────────────────────────────────────────────────────┐
│              RULE OF FIVE                                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. Destructor                  ~ClassName()                   │
│   2. Copy Constructor            ClassName(const ClassName&)    │
│   3. Copy Assignment Operator    operator=(const ClassName&)    │
│   4. Move Constructor            ClassName(ClassName&&)     NEW │
│   5. Move Assignment Operator    operator=(ClassName&&)     NEW │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### std::move()

`std::move()` **casts** an L-value to an R-value (doesn't actually move anything!):

```cpp
std::vector<int> v1 = {1, 2, 3};
std::vector<int> v2 = std::move(v1);  // v1 is now "moved-from"

// v1 is valid but in unspecified state (probably empty)
// v2 owns the data that v1 used to own
```

### Implementing Move Semantics

```cpp
#include <iostream>
#include <utility>  // For std::move

template <typename T>
struct Node {
    T data;
    Node<T>* next;
    Node<T>* prev;
    Node(T val) : data(val), next(nullptr), prev(nullptr) {}
};

template <typename T>
class DoublyLinkedList {
public:
    // ═══════════════════════════════════════════════════════
    // CONSTRUCTORS
    // ═══════════════════════════════════════════════════════
    
    // Default constructor
    DoublyLinkedList() : head_(nullptr), tail_(nullptr), size_(0) {}
    
    // ═══════════════════════════════════════════════════════
    // DESTRUCTOR (1 of 5)
    // ═══════════════════════════════════════════════════════
    ~DoublyLinkedList() {
        Clear();
    }
    
    // ═══════════════════════════════════════════════════════
    // COPY CONSTRUCTOR (2 of 5)
    // Deep copy: allocate new nodes, copy data
    // ═══════════════════════════════════════════════════════
    DoublyLinkedList(const DoublyLinkedList& other) 
        : head_(nullptr), tail_(nullptr), size_(0) {
        Node<T>* current = other.head_;
        while (current != nullptr) {
            PushBack(current->data);  // Deep copy each element
            current = current->next;
        }
    }
    
    // ═══════════════════════════════════════════════════════
    // COPY ASSIGNMENT OPERATOR (3 of 5)
    // ═══════════════════════════════════════════════════════
    DoublyLinkedList& operator=(const DoublyLinkedList& other) {
        if (this != &other) {  // Self-assignment check
            Clear();  // Free old data
            Node<T>* current = other.head_;
            while (current != nullptr) {
                PushBack(current->data);
                current = current->next;
            }
        }
        return *this;
    }
    
    // ═══════════════════════════════════════════════════════
    // MOVE CONSTRUCTOR (4 of 5)
    // Steal resources from source, leave source empty
    // ═══════════════════════════════════════════════════════
    DoublyLinkedList(DoublyLinkedList&& other) noexcept
        : head_(other.head_),       // Step 1: Shallow copy (steal pointers)
          tail_(other.tail_),
          size_(other.size_) {
        // Step 2: Nullify source (break the connection)
        other.head_ = nullptr;
        other.tail_ = nullptr;
        other.size_ = 0;
    }
    
    // ═══════════════════════════════════════════════════════
    // MOVE ASSIGNMENT OPERATOR (5 of 5)
    // ═══════════════════════════════════════════════════════
    DoublyLinkedList& operator=(DoublyLinkedList&& other) noexcept {
        if (this != &other) {       // Step 1: Self-assignment check
            Clear();                 // Step 2: Free old resources
            
            // Step 3: Shallow copy (steal pointers)
            head_ = other.head_;
            tail_ = other.tail_;
            size_ = other.size_;
            
            // Step 4: Nullify source
            other.head_ = nullptr;
            other.tail_ = nullptr;
            other.size_ = 0;
        }
        return *this;
    }
    
    // ═══════════════════════════════════════════════════════
    // HELPER FUNCTIONS
    // ═══════════════════════════════════════════════════════
    void PushBack(T value) {
        Node<T>* new_node = new Node<T>(value);
        if (head_ == nullptr) {
            head_ = tail_ = new_node;
        } else {
            tail_->next = new_node;
            new_node->prev = tail_;
            tail_ = new_node;
        }
        size_++;
    }
    
    void Clear() {
        while (head_ != nullptr) {
            Node<T>* temp = head_;
            head_ = head_->next;
            delete temp;
        }
        tail_ = nullptr;
        size_ = 0;
    }
    
    size_t Size() const { return size_; }
    bool Empty() const { return size_ == 0; }
    
    void Print() const {
        std::cout << "[";
        Node<T>* current = head_;
        while (current != nullptr) {
            std::cout << current->data;
            if (current->next) std::cout << ", ";
            current = current->next;
        }
        std::cout << "]\n";
    }
    
private:
    Node<T>* head_;
    Node<T>* tail_;
    size_t size_;
};

int main() {
    DoublyLinkedList<int> list1;
    list1.PushBack(10);
    list1.PushBack(20);
    list1.PushBack(30);
    
    std::cout << "list1: "; list1.Print();
    std::cout << "list1 size: " << list1.Size() << "\n\n";
    
    // ═══════════════════════════════════════════════════════
    // COPY CONSTRUCTOR
    // ═══════════════════════════════════════════════════════
    DoublyLinkedList<int> list2 = list1;  // Deep copy
    std::cout << "After copy:\n";
    std::cout << "list1: "; list1.Print();
    std::cout << "list2: "; list2.Print();
    std::cout << "\n";
    
    // ═══════════════════════════════════════════════════════
    // MOVE CONSTRUCTOR
    // ═══════════════════════════════════════════════════════
    DoublyLinkedList<int> list3 = std::move(list1);  // Steal resources
    std::cout << "After move:\n";
    std::cout << "list1 (moved-from): "; list1.Print();
    std::cout << "list1 size: " << list1.Size() << "\n";
    std::cout << "list3 (moved-to): "; list3.Print();
    std::cout << "list3 size: " << list3.Size() << "\n\n";
    
    // ═══════════════════════════════════════════════════════
    // MOVE ASSIGNMENT
    // ═══════════════════════════════════════════════════════
    DoublyLinkedList<int> list4;
    list4.PushBack(100);
    list4.PushBack(200);
    std::cout << "list4 before: "; list4.Print();
    
    list4 = std::move(list2);  // Steal from list2
    std::cout << "After move assignment:\n";
    std::cout << "list2 (moved-from): "; list2.Print();
    std::cout << "list4 (moved-to): "; list4.Print();
    
    return 0;
}
```

**Output:**
```
list1: [10, 20, 30]
list1 size: 3

After copy:
list1: [10, 20, 30]
list2: [10, 20, 30]

After move:
list1 (moved-from): []
list1 size: 0
list3 (moved-to): [10, 20, 30]
list3 size: 3

list4 before: [100, 200]
After move assignment:
list2 (moved-from): []
list4 (moved-to): [10, 20, 30]
```

### Move Semantics Summary

```
┌─────────────────────────────────────────────────────────────────┐
│              MOVE SEMANTICS SUMMARY                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   MOVE CONSTRUCTOR:                                             │
│   ClassName(ClassName&& other) noexcept {                       │
│       // 1. Shallow copy (steal pointers)                       │
│       data_ = other.data_;                                      │
│       // 2. Nullify source                                      │
│       other.data_ = nullptr;                                    │
│   }                                                             │
│                                                                 │
│   MOVE ASSIGNMENT:                                              │
│   ClassName& operator=(ClassName&& other) noexcept {            │
│       if (this != &other) {           // 1. Self-check          │
│           delete data_;                // 2. Free old           │
│           data_ = other.data_;         // 3. Steal              │
│           other.data_ = nullptr;       // 4. Nullify source     │
│       }                                                         │
│       return *this;                                             │
│   }                                                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔑 Key Takeaways

### Design Patterns

| Pattern | Problem | Solution |
|---------|---------|----------|
| **Most Wanted Holder** | Find best element | Init with first, compare rest |
| **One Way Flag** | Check all satisfy property | Assume true, flip if violated |
| **Follower** | Compare adjacent elements | Keep previous element reference |
| **Strategy** | Interchangeable behaviors | Family of encapsulated algorithms |

### Iterators

| Operation | Meaning |
|-----------|---------|
| `*iter` | Access current element |
| `++iter` | Move to next |
| `iter == other` | Same position? |
| `begin()` | Iterator to first |
| `end()` | Iterator past last |

### Move Semantics

| Concept | Key Point |
|---------|-----------|
| **L-value** | Has address, persists |
| **R-value** | Temporary, dies at end of line |
| **&&** | R-value reference |
| **std::move()** | Casts to R-value |
| **Move constructor** | Steals resources |
| **Move assignment** | Free old + steal |
| **Rule of Five** | Define all 5 special functions |

### When Move is Called

```cpp
T a;
T b = a;           // Copy constructor (a is L-value)
T c = std::move(a); // Move constructor (std::move makes R-value)
T d = GetT();      // Move constructor (return value is R-value)

b = a;             // Copy assignment
b = std::move(a);  // Move assignment
b = GetT();        // Move assignment
```

