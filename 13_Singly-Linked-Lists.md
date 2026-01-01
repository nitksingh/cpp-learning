# 🎓 Singly Linked Lists

---

## 🔗 What is a Singly Linked List?

A **singly linked list** is a dynamic data structure where:
- Data is stored in **nodes** scattered across the free store
- Each node points to the **next** node in the sequence
- A **head** pointer provides entry to the first node
- A **tail** pointer provides quick access to the last node

```
┌─────────────────────────────────────────────────────────────────┐
│                    SINGLY LINKED LIST                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   head                                              tail        │
│     │                                                 │         │
│     ▼                                                 ▼         │
│   ┌────┬───┐    ┌────┬───┐    ┌────┬───┐    ┌────┬───┐         │
│   │ 7  │ ●─┼───►│ 11 │ ●─┼───►│ 8  │ ●─┼───►│ 3  │ ╱ │         │
│   └────┴───┘    └────┴───┘    └────┴───┘    └────┴───┘         │
│    data next     data next     data next     data next         │
│                                              (nullptr)          │
│                                                                 │
│   ● = pointer to next node                                      │
│   ╱ = nullptr (end of list)                                     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📦 Node Structure

Each **node** contains:
1. **data** — The actual value stored
2. **next** — Pointer to the next node (or `nullptr` if last)

```cpp
struct Node {
    int data;       // The value stored in this node
    Node* next;     // Pointer to the next node
    
    // Constructor: Using Member Initializer List syntax initialize data member to value, and next member to nullptr. Body of constructor function doesn't do anything.
    Node(int value) : data(value), next(nullptr) {}
};
```

### Creating a Node

```cpp
Node* n = new Node(7);  // Create node with value 7

// Result:
// n ──► ┌────┬───────┐
//       │ 7  │nullptr│
//       └────┴───────┘
```

---

## 🏗️ SinglyLinkedList Class

```cpp
class SinglyLinkedList {
public:
    SinglyLinkedList();           // Default constructor
    ~SinglyLinkedList();          // Destructor
    
    void PushBack(int value);     // Add to end
    void PushFront(int value);    // Add to beginning
    void Insert(int value);       // Insert in sorted order
    
private:
    Node* head_;   // Points to first node (entry point)
    Node* tail_;   // Points to last node (quick access)
};
```

### Why Both Head and Tail?

| Pointer | Purpose |
|---------|---------|
| `head_` | Entry point to the list, required for traversal |
| `tail_` | Quick access to last node, makes PushBack O(1) |

---

## 🆕 Default Constructor: Empty List

An **empty list** has both `head_` and `tail_` set to `nullptr`.

```cpp
SinglyLinkedList::SinglyLinkedList() 
    : head_(nullptr), tail_(nullptr) 
{
    // Nothing else to do
}
```

### Visualization

```
SinglyLinkedList sll;  // Create empty list

Stack:
┌─────────────────────┐
│ sll                 │
│ ├── head_ = nullptr │──► (nothing)
│ └── tail_ = nullptr │──► (nothing)
└─────────────────────┘

Empty list: head_ == nullptr
```

---

## ➕ PushBack: Add to End

### Two Cases to Handle

```
Case 1: Empty list (head_ == nullptr)
        → New node becomes BOTH head and tail

Case 2: Non-empty list
        → Attach to current tail, update tail
```

### Implementation

```cpp
void SinglyLinkedList::PushBack(int value) {
    Node* new_node = new Node(value);  // Create new node
    
    if (head_ == nullptr) {
        // Case 1: Empty list
        head_ = new_node;  // New node is head
        tail_ = new_node;  // New node is also tail
    } else {
        // Case 2: Non-empty list
        tail_->next = new_node;  // Current tail points to new node
        tail_ = new_node;        // Update tail to new node
    }
}
```

### Step-by-Step: PushBack into Empty List

```cpp
SinglyLinkedList sll;
sll.PushBack(7);

Step 1: Create new node
new_node ──► [7|nullptr]

Step 2: head_ == nullptr, so empty list
head_ = new_node
tail_ = new_node

Result:
head_ ──► [7|nullptr] ◄── tail_
          (same node)
```

### Step-by-Step: PushBack into Non-Empty List

```cpp
sll.PushBack(11);  // List already has [7]

Step 1: Create new node
new_node ──► [11|nullptr]

Step 2: Attach to current tail
tail_->next = new_node

head_ ──► [7|●]───► [11|nullptr]
              ↑           ↑
            tail_     new_node

Step 3: Update tail
tail_ = new_node

head_ ──► [7|●]───► [11|nullptr] ◄── tail_
```

---

## ⬅️ PushFront: Add to Beginning

### Implementation

```cpp
void SinglyLinkedList::PushFront(int value) {
    Node* new_node = new Node(value);  // Create new node
    
    if (head_ == nullptr) {
        // Empty list: new node is both head and tail
        head_ = new_node;
        tail_ = new_node;
    } else {
        // Non-empty: new node points to old head
        new_node->next = head_;  // Link new to old head
        head_ = new_node;        // Update head to new node
    }
}
```

### Visualization

```cpp
// Before: head_ ──► [7|●]───► [11|nullptr] ◄── tail_

sll.PushFront(5);

// Step 1: Create node, link to old head
new_node ──► [5|●]───► [7|●]───► [11|nullptr]
                         ↑
                       head_

// Step 2: Update head
head_ ──► [5|●]───► [7|●]───► [11|nullptr] ◄── tail_
```

---

## 🚶 Traversing a Linked List

To visit each node, start at `head_` and follow `next` pointers.

### ⚠️ Common Mistake: DON'T Use Head!

```cpp
// ❌ WRONG: Moving head loses the list!
while (head_ != nullptr) {
    // process head_
    head_ = head_->next;  // 💥 Lost previous nodes!
}
// head_ is now nullptr — entire list is LEAKED!
```

### ✅ Correct: Use a Temporary Pointer

```cpp
// ✅ CORRECT: Use a separate pointer
Node* current = head_;  // Start at head
while (current != nullptr) {
    std::cout << current->data << " ";  // Process node
    current = current->next;             // Move to next
}
// head_ is unchanged, list is preserved!
```

### Traversal Visualization

```
head_ ──► [7|●]───► [11|●]───► [8|nullptr] ◄── tail_

Step 1: current = head_
        current
           ↓
        [7|●]───► [11|●]───► [8|nullptr]
        Print: 7

Step 2: current = current->next
                  current
                     ↓
        [7|●]───► [11|●]───► [8|nullptr]
        Print: 11

Step 3: current = current->next
                            current
                               ↓
        [7|●]───► [11|●]───► [8|nullptr]
        Print: 8

Step 4: current = current->next
        current = nullptr → STOP
```

---

## 📍 Insert in Sorted Order

Insert a node so the list stays sorted (ascending order).

### Key Insight: Stop One Node Before!

To insert between nodes, you need access to the node **before** the insertion point.

```
Insert 10 into: [7] → [11] → [15]

Need to stop at [7] because:
- [7]->next must point to new [10]
- [10]->next must point to [11]

Result: [7] → [10] → [11] → [15]
```

### Implementation

```cpp
void SinglyLinkedList::Insert(int value) {
    Node* new_node = new Node(value);
    
    // Case 1: Empty list or insert at beginning
    if (head_ == nullptr || value <= head_->data) {
        new_node->next = head_;
        head_ = new_node;
        if (tail_ == nullptr) {
            tail_ = new_node;  // Was empty, so tail = head
        }
        return;
    }
    
    // Case 2: Find insertion point (stop ONE BEFORE)
    Node* current = head_;
    while (current->next != nullptr && current->next->data < value) {
        current = current->next;
    }
    
    // Insert after current
    new_node->next = current->next;
    current->next = new_node;
    
    // Case 3: Inserted at end, update tail
    if (new_node->next == nullptr) {
        tail_ = new_node;
    }
}
```

### Insert Visualization

```cpp
// List: [7] → [15] → nullptr
// Insert value 10

Step 1: Find position (stop when current->next->data >= value)
        current
           ↓
        [7|●]───► [15|nullptr]
        current->next->data (15) >= 10, STOP!

Step 2: Create new node, link to current->next
        new_node ──► [10|●]───► [15|nullptr]
                            ↑
                     current->next

Step 3: Update current->next to point to new_node
        [7|●]───► [10|●]───► [15|nullptr]
              └─────────┘

Result:
head_ ──► [7|●]───► [10|●]───► [15|nullptr] ◄── tail_
```

---

## 🗑️ Destructor: Clean Up All Nodes

Must delete **every** node to avoid memory leaks.

### ⚠️ Cannot Just `delete head_`!

```cpp
// ❌ WRONG: Only deletes first node!
delete head_;  // 💥 All other nodes LEAKED!
```

### ✅ Correct: Walk and Delete Each Node

```cpp
SinglyLinkedList::~SinglyLinkedList() {
    while (head_ != nullptr) {
        Node* next = head_->next;  // Save next BEFORE deleting
        delete head_;              // Delete current node
        head_ = next;              // Move to saved next
    }
    // tail_ automatically becomes invalid (object is being destroyed)
}
```

### Destruction Visualization

```
Initial: head_ ──► [7|●]───► [11|●]───► [8|nullptr]

Iteration 1:
  next = head_->next (points to [11])
  delete head_ (deletes [7])
  head_ = next
  
  head_ ──► [11|●]───► [8|nullptr]

Iteration 2:
  next = head_->next (points to [8])
  delete head_ (deletes [11])
  head_ = next
  
  head_ ──► [8|nullptr]

Iteration 3:
  next = head_->next (nullptr)
  delete head_ (deletes [8])
  head_ = next (nullptr)
  
  head_ = nullptr → DONE!
```

---

## 🖨️ Printing the List (Overload `<<`)

```cpp
std::ostream& operator<<(std::ostream& os, const SinglyLinkedList& sll) {
    os << "[ ";
    Node* current = sll.head_;
    while (current != nullptr) {
        os << current->data;
        if (current->next != nullptr) {
            os << " -> ";
        }
        current = current->next;
    }
    os << " ]";
    return os;
}
```

---

## 💻 Complete Implementation

```cpp
#include <iostream>

// ============================================
// Node: Building block of the linked list
// ============================================
struct Node {
    int data;       // Value stored in node
    Node* next;     // Pointer to next node
    
    Node(int value) : data(value), next(nullptr) {}
};

// ============================================
// SinglyLinkedList: Container managing nodes
// ============================================
class SinglyLinkedList {
public:
    // Default constructor: creates empty list
    SinglyLinkedList() : head_(nullptr), tail_(nullptr) {}
    
    // Destructor: frees all nodes
    ~SinglyLinkedList() {
        while (head_ != nullptr) {
            Node* next = head_->next;  // Save next
            delete head_;              // Delete current
            head_ = next;              // Move forward
        }
    }
    
    // Add node to END of list
    void PushBack(int value) {
        Node* new_node = new Node(value);
        
        if (head_ == nullptr) {
            // Empty list: new node is both head and tail
            head_ = tail_ = new_node;
        } else {
            // Attach to current tail, update tail
            tail_->next = new_node;
            tail_ = new_node;
        }
    }
    
    // Add node to BEGINNING of list
    void PushFront(int value) {
        Node* new_node = new Node(value);
        
        if (head_ == nullptr) {
            // Empty list
            head_ = tail_ = new_node;
        } else {
            // New node points to old head, becomes new head
            new_node->next = head_;
            head_ = new_node;
        }
    }
    
    // Insert node in SORTED (ascending) order
    void Insert(int value) {
        Node* new_node = new Node(value);
        
        // Case 1: Empty list or insert at beginning
        if (head_ == nullptr || value <= head_->data) {
            new_node->next = head_;
            head_ = new_node;
            if (tail_ == nullptr) {
                tail_ = new_node;
            }
            return;
        }
        
        // Case 2: Find position (stop ONE BEFORE insertion point)
        Node* current = head_;
        while (current->next != nullptr && current->next->data < value) {
            current = current->next;
        }
        
        // Insert: new_node links to rest, current links to new_node
        new_node->next = current->next;
        current->next = new_node;
        
        // Case 3: If inserted at end, update tail
        if (new_node->next == nullptr) {
            tail_ = new_node;
        }
    }
    
    // Friend function to print the list
    friend std::ostream& operator<<(std::ostream& os, 
                                     const SinglyLinkedList& sll) {
        os << "[ ";
        Node* current = sll.head_;
        while (current != nullptr) {
            os << current->data;
            if (current->next != nullptr) {
                os << " -> ";
            }
            current = current->next;
        }
        os << " ]";
        return os;
    }

private:
    Node* head_;  // First node (entry point)
    Node* tail_;  // Last node (quick access for PushBack)
};

// ============================================
// Main: Test the SinglyLinkedList
// ============================================
int main() {
    std::cout << "=== Testing SinglyLinkedList ===" << std::endl;
    
    // Create empty list
    SinglyLinkedList sll;
    std::cout << "Empty list: " << sll << std::endl;
    
    // Test PushBack
    sll.PushBack(11);
    std::cout << "After PushBack(11): " << sll << std::endl;
    
    sll.PushBack(7);
    std::cout << "After PushBack(7): " << sll << std::endl;
    
    sll.PushBack(8);
    std::cout << "After PushBack(8): " << sll << std::endl;
    
    // Test PushFront
    sll.PushFront(5);
    std::cout << "After PushFront(5): " << sll << std::endl;
    
    std::cout << "\n=== Testing Insert (sorted) ===" << std::endl;
    
    // Create new list for sorted insert
    SinglyLinkedList sorted;
    sorted.Insert(10);
    std::cout << "Insert(10): " << sorted << std::endl;
    
    sorted.Insert(5);
    std::cout << "Insert(5): " << sorted << std::endl;
    
    sorted.Insert(15);
    std::cout << "Insert(15): " << sorted << std::endl;
    
    sorted.Insert(7);
    std::cout << "Insert(7): " << sorted << std::endl;
    
    sorted.Insert(12);
    std::cout << "Insert(12): " << sorted << std::endl;
    
    sorted.Insert(1);
    std::cout << "Insert(1): " << sorted << std::endl;
    
    return 0;
}  // Destructors automatically clean up all nodes!
```

### Expected Output

```
=== Testing SinglyLinkedList ===
Empty list: [ ]
After PushBack(11): [ 11 ]
After PushBack(7): [ 11 -> 7 ]
After PushBack(8): [ 11 -> 7 -> 8 ]
After PushFront(5): [ 5 -> 11 -> 7 -> 8 ]

=== Testing Insert (sorted) ===
Insert(10): [ 10 ]
Insert(5): [ 5 -> 10 ]
Insert(15): [ 5 -> 10 -> 15 ]
Insert(7): [ 5 -> 7 -> 10 -> 15 ]
Insert(12): [ 5 -> 7 -> 10 -> 12 -> 15 ]
Insert(1): [ 1 -> 5 -> 7 -> 10 -> 12 -> 15 ]
```

---

## 🔑 Key Takeaways

### Singly Linked List Structure

```
head_ ──► [data|next] ──► [data|next] ──► [data|nullptr] ◄── tail_
```

### Operations Summary

| Operation | Time | Key Steps |
|-----------|------|-----------|
| PushBack | O(1) | `tail_->next = new`, `tail_ = new` |
| PushFront | O(1) | `new->next = head_`, `head_ = new` |
| Insert (sorted) | O(n) | Traverse to position, link in |
| Traverse | O(n) | `current = head_; while(current)` |
| Destructor | O(n) | Delete each node, head to end |

### Golden Rules

1. **Never move `head_`** during traversal — use a temporary pointer
2. **Save `next` before `delete`** — or you lose the rest of the list
3. **Check for empty list** — `head_ == nullptr`
4. **Update `tail_`** when modifying the end of the list
5. **Stop one before** insertion point when inserting in middle

### Common Patterns

```cpp
// Traversal pattern
Node* current = head_;
while (current != nullptr) {
    // Process current->data
    current = current->next;
}

// Destruction pattern
while (head_ != nullptr) {
    Node* next = head_->next;
    delete head_;
    head_ = next;
}

// Insert pattern (after finding position)
new_node->next = current->next;  // 1. Link new to rest
current->next = new_node;        // 2. Link prev to new
```

