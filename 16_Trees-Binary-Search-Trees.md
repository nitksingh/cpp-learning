# 🎓 Trees & Binary Search Trees

---

## 🌳 What is a Tree?

A **tree** is a **non-linear** data structure that creates a **hierarchical** ordering of data.

```
┌─────────────────────────────────────────────────────────────────┐
│         LINKED LIST vs TREE                                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Linked List (Linear):                                         │
│   [A] → [B] → [C] → [D] → nullptr                               │
│                                                                 │
│   Tree (Non-Linear, Hierarchical):                              │
│                                                                 │
│                    [A]  ← Root (top of tree)                    │
│                   /   \                                         │
│                 [B]   [C]  ← Children of A                      │
│                /  \     \                                       │
│              [D]  [E]   [F]  ← Leaves (no children)             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Tree Terminology

```
                    [20]  ← ROOT: Top node, no parent
                   /    \
    PARENT →    [8]     [22]  ← LEAF: No children
               /   \
   CHILD →  [4]   [12]  ← INTERNAL NODE: Has children
                  /  \
               [10] [14]  ← LEAVES
```

| Term | Definition |
|------|------------|
| **Root** | Top node, has no parent |
| **Parent** | Node with children below it |
| **Child** | Node directly below a parent |
| **Leaf** | Node with no children |
| **Internal Node** | Node with at least one child |
| **Subtree** | A node and all its descendants |
| **Depth** | Distance from root (root = 0) |
| **Height** | Longest path to a leaf (leaf = 0) |

---

## 🌲 Binary Trees

A **binary tree** is a tree where each node has **at most 2 children** (left and right).

### Node Structure

```cpp
template <typename T>
struct BinaryTreeNode {
    T data;                    // Data stored in node
    BinaryTreeNode* parent;    // Pointer to parent node
    BinaryTreeNode* left;      // Pointer to left child
    BinaryTreeNode* right;     // Pointer to right child
    
    BinaryTreeNode(T val) 
        : data(val), parent(nullptr), left(nullptr), right(nullptr) {}
};
```

### Visual Representation

```
┌─────────────────────────────────────────────────────────────────┐
│             BINARY TREE NODE                                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                      ┌───────────┐                              │
│         parent ──────│  PARENT   │                              │
│                      └─────┬─────┘                              │
│                            │                                    │
│                            ▼                                    │
│                    ┌───────────────┐                            │
│                    │     NODE      │                            │
│                    │  ┌─────────┐  │                            │
│                    │  │  data   │  │                            │
│                    │  └─────────┘  │                            │
│                    │  ┌───┬───┐   │                            │
│                    │  │ L │ R │   │                            │
│                    │  └─┬─┴─┬─┘   │                            │
│                    └────┼───┼────┘                              │
│                         │   │                                   │
│                    ┌────┘   └────┐                              │
│                    ▼             ▼                              │
│              ┌──────────┐  ┌──────────┐                         │
│              │  LEFT    │  │  RIGHT   │                         │
│              │  CHILD   │  │  CHILD   │                         │
│              └──────────┘  └──────────┘                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔍 Binary Search Trees (BST)

A **Binary Search Tree** is a binary tree that maintains a special **ordering property**.

### The BST Property

> For any node **X**:
> - All nodes in **left subtree** have keys **< X's key**
> - All nodes in **right subtree** have keys **≥ X's key**

```
┌─────────────────────────────────────────────────────────────────┐
│              BST PROPERTY                                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                        [20]                                     │
│                       /    \                                    │
│        LEFT SUBTREE  /      \  RIGHT SUBTREE                    │
│        (all < 20)   /        \  (all >= 20)                     │
│                   [8]        [22]                               │
│                  /   \                                          │
│                [4]   [12]                                       │
│                      /  \                                       │
│                   [10] [14]                                     │
│                                                                 │
│   Inorder traversal gives SORTED order:                         │
│   4, 8, 10, 12, 14, 20, 22                                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Why BST?

With a **balanced** BST, operations like search, insert, delete take **O(log n)** time!

```
┌─────────────────────────────────────────────────────────────────┐
│         BALANCED vs UNBALANCED BST                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   BALANCED (Good):          UNBALANCED (Bad - like a list):     │
│                                                                 │
│         [8]                        [2]                          │
│        /   \                         \                          │
│      [4]   [12]                      [4]                        │
│     /  \   /  \                        \                        │
│   [2] [6][10][14]                      [6]                      │
│                                          \                      │
│   Height: 2                              [8]                    │
│   Search: O(log n)                         \                    │
│                                            [10]                 │
│                                                                 │
│                              Height: 4                          │
│                              Search: O(n) 😢                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📝 BST Node Implementation

```cpp
#include <iostream>

// Node structure for BST
template <typename T>
struct Node {
    T key;           // Key for ordering
    Node* parent;    // Pointer to parent
    Node* left;      // Pointer to left child
    Node* right;     // Pointer to right child
    
    // Constructor: initialize key, all pointers to nullptr
    Node(T k) : key(k), parent(nullptr), left(nullptr), right(nullptr) {}
};
```

---

## 🏗️ BST Class Structure

```cpp
template <typename T>
class BST {
public:
    // Constructor: empty tree
    BST() : root_(nullptr) {}
    
    // Parameterized: tree with one node
    BST(T key) : root_(nullptr) {
        root_ = new Node<T>(key);
    }
    
    // Rule of Three
    BST(const BST& other) = delete;  // Copy constructor (implement if needed)
    BST& operator=(const BST& other) = delete;  // Copy assignment
    
    // Destructor: free all nodes
    ~BST() { Clear(root_); root_ = nullptr; }
    
    // Public interface
    void Insert(T key);
    Node<T>* Search(T key);
    Node<T>* Minimum();
    Node<T>* Maximum();
    
private:
    Node<T>* root_;
    
    // Helper for destruction
    void Clear(Node<T>* node);
};
```

---

## ➕ BST Insertion

Insert a new node while **maintaining the BST property**.

### Algorithm

1. Start at root
2. Compare key with current node
3. If key < current: go left
4. If key >= current: go right
5. Repeat until finding empty spot
6. Insert new node, update parent pointer

### Visual Example

```
Insert 5 into this BST:

        [10]                    [10]
       /    \                  /    \
     [6]    [15]    →       [6]    [15]
    /                       /  \
  [3]                     [3]  [5] ← New node!

Step 1: 5 < 10, go left
Step 2: 5 < 6, go left  
Step 3: 5 > 3, go right → empty! Insert here
```

### Implementation

```cpp
template <typename T>
void BST<T>::Insert(T key) {
    // Create new node
    Node<T>* new_node = new Node<T>(key);
    
    // Case 1: Empty tree
    if (root_ == nullptr) {
        root_ = new_node;
        return;
    }
    
    // Case 2: Find correct position
    Node<T>* current = root_;
    Node<T>* parent = root_;
    
    // Walk down tree to find insertion point
    while (current != nullptr) {
        parent = current;  // Keep track of parent
        
        if (key < current->key) {
            current = current->left;   // Go left
        } else {
            current = current->right;  // Go right
        }
    }
    
    // Insert: attach new_node to parent
    if (key < parent->key) {
        parent->left = new_node;   // Attach as left child
    } else {
        parent->right = new_node;  // Attach as right child
    }
    
    // Don't forget to set parent pointer!
    new_node->parent = parent;
}
```

---

## 🚶 BST Traversals

Three ways to visit all nodes in a BST:

### 1. Inorder Traversal (Left → Node → Right)

**Visits nodes in SORTED order!**

```cpp
// Inorder: Left, Process, Right
template <typename T>
void InorderWalk(Node<T>* node) {
    if (node == nullptr) return;
    
    InorderWalk(node->left);         // 1. Visit left subtree
    std::cout << node->key << " ";   // 2. Process current node
    InorderWalk(node->right);        // 3. Visit right subtree
}
```

```
        [20]
       /    \
     [8]    [22]
    /   \
  [4]   [12]
        /  \
      [10] [14]

Inorder: 4, 8, 10, 12, 14, 20, 22  ← SORTED!
```

### 2. Preorder Traversal (Node → Left → Right)

**Process root BEFORE subtrees.**

```cpp
// Preorder: Process, Left, Right
template <typename T>
void PreorderWalk(Node<T>* node) {
    if (node == nullptr) return;
    
    std::cout << node->key << " ";   // 1. Process current node
    PreorderWalk(node->left);        // 2. Visit left subtree
    PreorderWalk(node->right);       // 3. Visit right subtree
}
```

```
Preorder: 20, 8, 4, 12, 10, 14, 22
```

### 3. Postorder Traversal (Left → Right → Node)

**Process root AFTER subtrees. Used for deletion!**

```cpp
// Postorder: Left, Right, Process
template <typename T>
void PostorderWalk(Node<T>* node) {
    if (node == nullptr) return;
    
    PostorderWalk(node->left);       // 1. Visit left subtree
    PostorderWalk(node->right);      // 2. Visit right subtree
    std::cout << node->key << " ";   // 3. Process current node
}
```

```
Postorder: 4, 10, 14, 12, 8, 22, 20
```

### Traversal Summary

```
┌─────────────────────────────────────────────────────────────────┐
│              TRAVERSAL COMPARISON                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Tree:     [20]                                                │
│            /    \                                               │
│          [8]    [22]                                            │
│         /   \                                                   │
│       [4]   [12]                                                │
│                                                                 │
│   Inorder:   4, 8, 12, 20, 22    (SORTED - Left, Node, Right)   │
│   Preorder:  20, 8, 4, 12, 22    (Node first - Node, Left, Right)│
│   Postorder: 4, 12, 8, 22, 20    (Node last - Left, Right, Node) │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔎 BST Minimum & Maximum

By BST property:
- **Minimum**: Walk LEFT as far as possible
- **Maximum**: Walk RIGHT as far as possible

```cpp
// Find node with MINIMUM key (leftmost node)
template <typename T>
Node<T>* BST<T>::Minimum() {
    if (root_ == nullptr) return nullptr;
    
    Node<T>* current = root_;
    
    // Keep going left until we can't
    while (current->left != nullptr) {
        current = current->left;
    }
    
    return current;  // Leftmost node = minimum
}

// Find node with MAXIMUM key (rightmost node)
template <typename T>
Node<T>* BST<T>::Maximum() {
    if (root_ == nullptr) return nullptr;
    
    Node<T>* current = root_;
    
    // Keep going right until we can't
    while (current->right != nullptr) {
        current = current->right;
    }
    
    return current;  // Rightmost node = maximum
}
```

```
        [20]
       /    \
     [8]    [22] ← Maximum (rightmost)
    /   \
  [4]   [12]    
   ↑
Minimum (leftmost)
```

---

## 🔍 BST Search

Find a node with a specific key.

### Recursive Search

```cpp
template <typename T>
Node<T>* TreeSearch(Node<T>* node, T key) {
    // Base case: not found or found
    if (node == nullptr || node->key == key) {
        return node;
    }
    
    // Recursive case: go left or right
    if (key < node->key) {
        return TreeSearch(node->left, key);   // Search left
    } else {
        return TreeSearch(node->right, key);  // Search right
    }
}
```

### Iterative Search (More Efficient)

```cpp
template <typename T>
Node<T>* BST<T>::Search(T key) {
    Node<T>* current = root_;
    
    // Walk down tree
    while (current != nullptr && current->key != key) {
        if (key < current->key) {
            current = current->left;   // Go left
        } else {
            current = current->right;  // Go right
        }
    }
    
    return current;  // nullptr if not found
}
```

### Search Example

```
Search for 12:

        [20]  ← Start: 12 < 20, go LEFT
       /    \
     [8]      [22]  ← 12 > 8, go RIGHT
    /   \
  [4]   [12] ← FOUND!
```

---

## ⏭️ BST Successor

The **successor** of a node is the node with the **smallest key greater than it** (next in inorder traversal).

### Three Cases

```
┌─────────────────────────────────────────────────────────────────┐
│              SUCCESSOR CASES                                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ Case 1: Node has RIGHT subtree                                  │
│         → Successor = MINIMUM of right subtree                  │
│                                                                 │
│         [8]                                                     │
│        /   \                                                    │
│      [4]   [12]  ← Successor of 8                               │
│            /  \     is MIN of right subtree                     │
│         [10] [14]   = 10                                        │
│                                                                 │
│ Case 2: Node is LEFT child (no right subtree)                   │
│         → Successor = PARENT                                    │
│                                                                 │
│         [12]  ← Successor of 10                                 │
│         /  \                                                    │
│      [10] [14]                                                  │
│                                                                 │
│ Case 3: Node is RIGHT child (no right subtree)                  │
│         → Walk up until we find ancestor that has               │
│           current node in its LEFT subtree                      │
│                                                                 │
│         [20]  ← Successor of 14                                 │
│        /    \    (walk up: 14→12→8→20)                          │
│      [8]    [22]                                                │
│     /   \                                                       │
│   [4]   [12]                                                    │
│         /  \                                                    │
│      [10] [14]                                                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Implementation

```cpp
template <typename T>
Node<T>* Successor(Node<T>* node) {
    if (node == nullptr) return nullptr;
    
    // Case 1: Has right subtree → find minimum in right subtree
    if (node->right != nullptr) {
        Node<T>* current = node->right;
        while (current->left != nullptr) {
            current = current->left;
        }
        return current;
    }
    
    // Case 2 & 3: Walk up to find ancestor
    Node<T>* successor = node->parent;
    
    // Keep going up while node is in RIGHT subtree of successor
    while (successor != nullptr && node == successor->right) {
        node = successor;
        successor = successor->parent;
    }
    
    return successor;  // nullptr if no successor (node was max)
}
```

---

## 🗑️ BST Destruction

Use **postorder traversal** to delete nodes safely!

### Why Postorder?

```
┌─────────────────────────────────────────────────────────────────┐
│              WHY POSTORDER FOR DELETION?                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ❌ Preorder (Node first):                                     │
│      Delete 20 first → LOSE access to children!                 │
│                                                                 │
│   ❌ Inorder (Node in middle):                                  │
│      Delete 8 after left subtree → LOSE right subtree!          │
│                                                                 │
│   ✅ Postorder (Node last):                                     │
│      Delete children FIRST, then parent                         │
│      → Safe! Never lose access to nodes                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Implementation

```cpp
template <typename T>
void BST<T>::Clear(Node<T>* node) {
    if (node == nullptr) return;
    
    // Postorder: visit children BEFORE deleting current
    Clear(node->left);   // 1. Delete left subtree
    Clear(node->right);  // 2. Delete right subtree
    delete node;         // 3. Delete current node (safe now!)
}
```

---

## 💻 Complete BST Implementation

```cpp
#include <iostream>
#include <stdexcept>

// ============================================
// Node Structure
// ============================================
template <typename T>
struct Node {
    T key;
    Node* parent;
    Node* left;
    Node* right;
    
    Node(T k) : key(k), parent(nullptr), left(nullptr), right(nullptr) {}
};

// ============================================
// Binary Search Tree Class
// ============================================
template <typename T>
class BST {
public:
    // Default constructor: empty tree
    BST() : root_(nullptr) {}
    
    // Parameterized constructor: tree with root
    BST(T key) : root_(nullptr) {
        root_ = new Node<T>(key);
    }
    
    // Destructor
    ~BST() {
        Clear(root_);
        root_ = nullptr;
    }
    
    // Delete copy operations (Rule of Three)
    BST(const BST&) = delete;
    BST& operator=(const BST&) = delete;
    
    // ========== INSERT ==========
    void Insert(T key) {
        Node<T>* new_node = new Node<T>(key);
        
        // Empty tree case
        if (root_ == nullptr) {
            root_ = new_node;
            return;
        }
        
        // Find insertion point
        Node<T>* current = root_;
        Node<T>* parent = nullptr;
        
        while (current != nullptr) {
            parent = current;
            if (key < current->key) {
                current = current->left;
            } else {
                current = current->right;
            }
        }
        
        // Attach new node
        new_node->parent = parent;
        if (key < parent->key) {
            parent->left = new_node;
        } else {
            parent->right = new_node;
        }
    }
    
    // ========== SEARCH ==========
    Node<T>* Search(T key) {
        Node<T>* current = root_;
        
        while (current != nullptr && current->key != key) {
            if (key < current->key) {
                current = current->left;
            } else {
                current = current->right;
            }
        }
        
        return current;  // nullptr if not found
    }
    
    // ========== MINIMUM ==========
    Node<T>* Minimum() {
        if (root_ == nullptr) return nullptr;
        
        Node<T>* current = root_;
        while (current->left != nullptr) {
            current = current->left;
        }
        return current;
    }
    
    // ========== MAXIMUM ==========
    Node<T>* Maximum() {
        if (root_ == nullptr) return nullptr;
        
        Node<T>* current = root_;
        while (current->right != nullptr) {
            current = current->right;
        }
        return current;
    }
    
    // ========== SUCCESSOR ==========
    Node<T>* Successor(Node<T>* node) {
        if (node == nullptr) return nullptr;
        
        // Case 1: Has right subtree
        if (node->right != nullptr) {
            Node<T>* current = node->right;
            while (current->left != nullptr) {
                current = current->left;
            }
            return current;
        }
        
        // Case 2 & 3: Walk up
        Node<T>* successor = node->parent;
        while (successor != nullptr && node == successor->right) {
            node = successor;
            successor = successor->parent;
        }
        return successor;
    }
    
    // ========== TRAVERSALS ==========
    void InorderPrint() {
        InorderWalk(root_);
        std::cout << std::endl;
    }
    
    void PreorderPrint() {
        PreorderWalk(root_);
        std::cout << std::endl;
    }
    
    void PostorderPrint() {
        PostorderWalk(root_);
        std::cout << std::endl;
    }
    
    // ========== UTILITY ==========
    bool IsEmpty() { return root_ == nullptr; }

private:
    Node<T>* root_;
    
    // Postorder deletion
    void Clear(Node<T>* node) {
        if (node == nullptr) return;
        Clear(node->left);
        Clear(node->right);
        delete node;
    }
    
    // Traversal helpers
    void InorderWalk(Node<T>* node) {
        if (node == nullptr) return;
        InorderWalk(node->left);
        std::cout << node->key << " ";
        InorderWalk(node->right);
    }
    
    void PreorderWalk(Node<T>* node) {
        if (node == nullptr) return;
        std::cout << node->key << " ";
        PreorderWalk(node->left);
        PreorderWalk(node->right);
    }
    
    void PostorderWalk(Node<T>* node) {
        if (node == nullptr) return;
        PostorderWalk(node->left);
        PostorderWalk(node->right);
        std::cout << node->key << " ";
    }
};

// ============================================
// Main: Test the BST
// ============================================
int main() {
    BST<int> tree;
    
    // Insert nodes
    tree.Insert(20);
    tree.Insert(8);
    tree.Insert(22);
    tree.Insert(4);
    tree.Insert(12);
    tree.Insert(10);
    tree.Insert(14);
    
    /*
     * Tree structure:
     *         20
     *        /  \
     *       8    22
     *      / \
     *     4   12
     *        /  \
     *       10   14
     */
    
    // Traversals
    std::cout << "Inorder:   ";
    tree.InorderPrint();   // 4 8 10 12 14 20 22 (SORTED!)
    
    std::cout << "Preorder:  ";
    tree.PreorderPrint();  // 20 8 4 12 10 14 22
    
    std::cout << "Postorder: ";
    tree.PostorderPrint(); // 4 10 14 12 8 22 20
    
    // Min/Max
    std::cout << "\nMinimum: " << tree.Minimum()->key << std::endl;  // 4
    std::cout << "Maximum: " << tree.Maximum()->key << std::endl;    // 22
    
    // Search
    Node<int>* found = tree.Search(12);
    if (found) {
        std::cout << "\nFound: " << found->key << std::endl;  // 12
    }
    
    Node<int>* not_found = tree.Search(100);
    if (!not_found) {
        std::cout << "100 not found in tree" << std::endl;
    }
    
    // Successor
    Node<int>* node8 = tree.Search(8);
    Node<int>* succ = tree.Successor(node8);
    if (succ) {
        std::cout << "\nSuccessor of 8: " << succ->key << std::endl;  // 10
    }
    
    Node<int>* node14 = tree.Search(14);
    succ = tree.Successor(node14);
    if (succ) {
        std::cout << "Successor of 14: " << succ->key << std::endl;  // 20
    }
    
    return 0;
}  // Destructor cleans up all nodes
```

### Expected Output

```
Inorder:   4 8 10 12 14 20 22 
Preorder:  20 8 4 12 10 14 22 
Postorder: 4 10 14 12 8 22 20 

Minimum: 4
Maximum: 22

Found: 12
100 not found in tree

Successor of 8: 10
Successor of 14: 20
```

---

## 🔑 Key Takeaways

### BST Property

```
For any node X:
- Left subtree: all keys < X.key
- Right subtree: all keys >= X.key
```

### Operations Complexity (Balanced BST)

| Operation | Average | Worst (Unbalanced) |
|-----------|---------|-------------------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Minimum | O(log n) | O(n) |
| Maximum | O(log n) | O(n) |
| Successor | O(log n) | O(n) |

### Traversal Uses

| Traversal | Order | Use Case |
|-----------|-------|----------|
| **Inorder** | Left, Node, Right | Print sorted |
| **Preorder** | Node, Left, Right | Copy tree |
| **Postorder** | Left, Right, Node | Delete tree |

### Common Mistakes

```cpp
// ❌ Forgot to update parent pointer on insert
parent->left = new_node;
// Missing: new_node->parent = parent;

// ❌ Using preorder for deletion (memory leak!)
delete node;           // Lose children!
Clear(node->left);     // Never executes

// ❌ Not handling empty tree
Node<T>* max = root_;
while (max->right)...  // Crash if root_ is nullptr!

// ✅ Always check for nullptr first
if (root_ == nullptr) return nullptr;
```

