# 🎓 Graphs

---

## 📊 What is a Graph?

A **graph** is a data structure consisting of two sets:
- **Vertex Set (V)**: A collection of nodes/vertices
- **Edge Set (E)**: Connections between vertices

```
┌─────────────────────────────────────────────────────────────────┐
│                    GRAPH = VERTICES + EDGES                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│       Vertex Set V = {A, B, C, D}                               │
│                                                                 │
│           (A)────────(B)                                        │
│            │          │                                         │
│            │          │                                         │
│           (C)────────(D)                                        │
│                                                                 │
│       Edge Set E = {{A,B}, {A,C}, {B,D}, {C,D}}                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Trees vs Graphs

```
┌─────────────────────────────────────────────────────────────────┐
│                    TREE vs GRAPH                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   TREE (Hierarchical):          GRAPH (Network):                │
│                                                                 │
│         [A]                        (A)───(B)                    │
│        /   \                        │ \ / │                     │
│      [B]   [C]                      │  X  │                     │
│      / \     \                      │ / \ │                     │
│    [D] [E]   [F]                   (C)───(D)                    │
│                                                                 │
│   - No cycles                    - Can have cycles              │
│   - One path between nodes       - Multiple paths possible      │
│   - Parent-child hierarchy       - Peer relationships           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔄 Types of Graphs

### 1. Undirected vs Directed

```
┌─────────────────────────────────────────────────────────────────┐
│              UNDIRECTED vs DIRECTED GRAPHS                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   UNDIRECTED (Two-way street):    DIRECTED (One-way street):   │
│                                                                 │
│       (A)────(B)                     (A)───→(B)                 │
│        │      │                       ↑      │                  │
│        │      │                       │      ↓                  │
│       (C)────(D)                     (C)←───(D)                 │
│                                                                 │
│   Edge {A,B} = {B,A}              Edge (A,B) ≠ (B,A)            │
│   Can travel both ways           Can only travel one way       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 2. Simple vs Multigraph

```
┌─────────────────────────────────────────────────────────────────┐
│              SIMPLE vs MULTIGRAPH                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   SIMPLE GRAPH:                   MULTIGRAPH:                   │
│                                                                 │
│       (A)────(B)                     (A)═══(B)                  │
│        │      │                       │  ══ │                   │
│        │      │                       │     │                   │
│       (C)────(D)                     (C)────(D)                 │
│                                                                 │
│   - One edge per pair             - Multiple edges allowed      │
│   - No self-loops                 - Self-loops allowed          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 3. Loops and Cycles

```
┌─────────────────────────────────────────────────────────────────┐
│              LOOP vs CYCLE                                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   LOOP (Self-connection):         CYCLE (Circular path):       │
│                                                                 │
│       ┌──┐                           (A)───→(B)                 │
│       │  ↓                            ↑      │                  │
│      (A)──                            │      ↓                  │
│                                      (D)←───(C)                 │
│                                                                 │
│   Edge connects vertex             Path returns to start        │
│   to itself                        (minimum 3 vertices)         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📖 Graph Terminology

### Degree of a Vertex

```
┌─────────────────────────────────────────────────────────────────┐
│              VERTEX DEGREE                                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   UNDIRECTED:                                                   │
│                                                                 │
│           (A)────(B)                                            │
│            │    / │                                             │
│            │   /  │                                             │
│           (C)────(D)                                            │
│                                                                 │
│   Degree of B = 3 (edges to A, C, D)                            │
│   Degree of A = 2 (edges to B, C)                               │
│                                                                 │
│   DIRECTED:                                                     │
│                                                                 │
│           (A)───→(B)                                            │
│            ↑      │                                             │
│            │      ↓                                             │
│           (C)←───(D)                                            │
│                                                                 │
│   In-degree of B = 1 (edges coming IN)                          │
│   Out-degree of B = 1 (edges going OUT)                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Connected vs Not Connected

```
┌─────────────────────────────────────────────────────────────────┐
│              CONNECTED vs NOT CONNECTED                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   CONNECTED:                      NOT CONNECTED:                │
│                                                                 │
│       (A)────(B)                     (A)────(B)    (E)────(F)   │
│        │      │                       │      │                  │
│        │      │                       │      │                  │
│       (C)────(D)                     (C)────(D)                 │
│                                                                 │
│   Path exists between              Two separate components      │
│   ANY two vertices                 (no path from A to E)        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Path and Subgraph

```
┌─────────────────────────────────────────────────────────────────┐
│              PATH AND SUBGRAPH                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Original Graph:                 Path A → B → D → C:           │
│                                                                 │
│       (A)────(B)                     (A)───→(B)                 │
│        │ \  / │                              │                  │
│        │  \/  │                              ↓                  │
│        │  /\  │                     (C)←───(D)                  │
│        │ /  \ │                                                 │
│       (C)────(D)                  Path length = 3 edges         │
│                                                                 │
│   A subgraph is a subset of edges and their vertices            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Acyclic Graph

A graph with **no cycles** is called **acyclic**.

> 💡 **A tree is an acyclic connected graph!**

---

## 🌐 Graph Applications

### 1. Social Networks

```
┌─────────────────────────────────────────────────────────────────┐
│              SOCIAL NETWORK GRAPH                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Vertices = People                                             │
│   Edges = Friendships                                           │
│                                                                 │
│       (Alice)────(Bob)                                          │
│          │    \    │                                            │
│          │     \   │                                            │
│       (Carol)───(Dave)                                          │
│                                                                 │
│   Questions we can answer:                                      │
│   - Are Alice and Dave connected?                               │
│   - What's the shortest friend-chain to reach someone?          │
│   - Who has the most friends? (highest degree)                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 2. Transportation Networks

```
┌─────────────────────────────────────────────────────────────────┐
│              TRANSPORTATION GRAPH                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Vertices = Bus stops / Cities / Airports                      │
│   Edges = Routes / Roads / Flights                              │
│                                                                 │
│      (Siebel)───bus───(Transit Plaza)                           │
│          │                    │                                 │
│         walk                 bus                                │
│          │                    │                                 │
│      (Goodwin)────bus────(The ARC)                              │
│                                                                 │
│   Find shortest path from Siebel to ARC!                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📊 Graph Representations

Two main ways to represent a graph in code:

### 1. Adjacency Matrix

A **V × V** 2D array where `matrix[i][j] = 1` if edge exists between vertex i and j.

```
┌─────────────────────────────────────────────────────────────────┐
│              ADJACENCY MATRIX                                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Graph:                    Matrix:                             │
│                                                                 │
│       (0)────(1)              │ 0  1  2  3                      │
│        │      │            ───┼────────────                     │
│        │      │             0 │ 0  1  1  0                      │
│       (2)────(3)             1 │ 1  0  0  1                      │
│                              2 │ 1  0  0  1                      │
│                              3 │ 0  1  1  0                      │
│                                                                 │
│   Edge (0,1) → matrix[0][1] = 1 AND matrix[1][0] = 1            │
│   No edge (0,3) → matrix[0][3] = 0                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### Adjacency Matrix Code

```cpp
#include <iostream>
#include <vector>

class GraphMatrix {
public:
    // Constructor: create V×V matrix filled with 0
    GraphMatrix(int vertices) : num_vertices_(vertices) {
        // Initialize V×V matrix with all zeros
        matrix_.resize(vertices, std::vector<int>(vertices, 0));
    }
    
    // Add undirected edge between v1 and v2
    void AddEdge(int v1, int v2) {
        matrix_[v1][v2] = 1;  // v1 → v2
        matrix_[v2][v1] = 1;  // v2 → v1 (undirected)
    }
    
    // Check if edge exists (constant time O(1)!)
    bool HasEdge(int v1, int v2) {
        return matrix_[v1][v2] == 1;
    }
    
    // Print the matrix
    void Print() {
        std::cout << "  ";
        for (int i = 0; i < num_vertices_; i++) {
            std::cout << i << " ";
        }
        std::cout << "\n";
        
        for (int i = 0; i < num_vertices_; i++) {
            std::cout << i << " ";
            for (int j = 0; j < num_vertices_; j++) {
                std::cout << matrix_[i][j] << " ";
            }
            std::cout << "\n";
        }
    }
    
private:
    int num_vertices_;
    std::vector<std::vector<int>> matrix_;
};

int main() {
    GraphMatrix g(4);  // 4 vertices: 0, 1, 2, 3
    
    g.AddEdge(0, 1);
    g.AddEdge(0, 2);
    g.AddEdge(1, 3);
    g.AddEdge(2, 3);
    
    g.Print();
    
    std::cout << "\nHas edge (0,1)? " << (g.HasEdge(0, 1) ? "Yes" : "No") << "\n";
    std::cout << "Has edge (0,3)? " << (g.HasEdge(0, 3) ? "Yes" : "No") << "\n";
    
    return 0;
}
```

**Output:**
```
  0 1 2 3 
0 0 1 1 0 
1 1 0 0 1 
2 1 0 0 1 
3 0 1 1 0 

Has edge (0,1)? Yes
Has edge (0,3)? No
```

### 2. Adjacency List

An array of lists where `adj[i]` contains all vertices connected to vertex i.

```
┌─────────────────────────────────────────────────────────────────┐
│              ADJACENCY LIST                                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Graph:                    Adjacency List:                     │
│                                                                 │
│       (0)────(1)             0 → [1, 2]                         │
│        │      │              1 → [0, 3]                         │
│        │      │              2 → [0, 3]                         │
│       (2)────(3)             3 → [1, 2]                         │
│                                                                 │
│   Only stores EXISTING edges (no zeros!)                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### Adjacency List Code

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <algorithm>

class GraphList {
public:
    // Constructor: create array of V empty lists
    GraphList(int vertices) : num_vertices_(vertices) {
        adj_list_.resize(vertices);
    }
    
    // Add undirected edge
    void AddEdge(int v1, int v2) {
        adj_list_[v1].push_back(v2);  // Add v2 to v1's list
        adj_list_[v2].push_back(v1);  // Add v1 to v2's list (undirected)
    }
    
    // Check if edge exists (O(degree) - must search list)
    bool HasEdge(int v1, int v2) {
        auto& neighbors = adj_list_[v1];
        return std::find(neighbors.begin(), neighbors.end(), v2) != neighbors.end();
    }
    
    // Get all neighbors of a vertex
    const std::list<int>& GetNeighbors(int v) {
        return adj_list_[v];
    }
    
    // Print the adjacency list
    void Print() {
        for (int i = 0; i < num_vertices_; i++) {
            std::cout << i << " → [";
            bool first = true;
            for (int neighbor : adj_list_[i]) {
                if (!first) std::cout << ", ";
                std::cout << neighbor;
                first = false;
            }
            std::cout << "]\n";
        }
    }
    
private:
    int num_vertices_;
    std::vector<std::list<int>> adj_list_;
};

int main() {
    GraphList g(4);  // 4 vertices: 0, 1, 2, 3
    
    g.AddEdge(0, 1);
    g.AddEdge(0, 2);
    g.AddEdge(1, 3);
    g.AddEdge(2, 3);
    
    g.Print();
    
    std::cout << "\nNeighbors of 0: ";
    for (int n : g.GetNeighbors(0)) {
        std::cout << n << " ";
    }
    std::cout << "\n";
    
    return 0;
}
```

**Output:**
```
0 → [1, 2]
1 → [0, 3]
2 → [0, 3]
3 → [1, 2]

Neighbors of 0: 1 2 
```

---

## ⚖️ Matrix vs List Comparison

```
┌─────────────────────────────────────────────────────────────────┐
│              ADJACENCY MATRIX vs LIST                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Operation          │ Matrix      │ List                       │
│   ───────────────────┼─────────────┼───────────────────────     │
│   Space              │ O(V²)       │ O(V + E)                   │
│   Add Edge           │ O(1)        │ O(1)                       │
│   Check Edge         │ O(1) ✓      │ O(degree)                  │
│   Get Neighbors      │ O(V)        │ O(degree) ✓                │
│   ───────────────────┼─────────────┼───────────────────────     │
│   Best for           │ Dense graph │ Sparse graph               │
│                      │ (many edges)│ (few edges)                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### When to Use Which?

| Use **Adjacency Matrix** when: | Use **Adjacency List** when: |
|-------------------------------|------------------------------|
| Graph is dense (E ≈ V²) | Graph is sparse (E << V²) |
| Need fast edge lookup | Need to iterate neighbors often |
| Small number of vertices | Large number of vertices |

---

## 🗺️ Undirected Graph with Strings

A more flexible implementation using `std::map` and `std::list`:

```cpp
#include <iostream>
#include <map>
#include <list>
#include <string>
#include <stdexcept>
#include <algorithm>

class UndirectedGraph {
public:
    // Add a new vertex to the graph
    void AddVertex(const std::string& vertex) {
        // Check if vertex already exists
        if (graph_.contains(vertex)) {
            throw std::runtime_error(vertex + " already in graph");
        }
        // Add vertex with empty adjacency list
        graph_[vertex] = std::list<std::string>();
        num_vertices_++;
    }
    
    // Add an edge between two vertices
    void AddEdge(const std::string& v1, const std::string& v2) {
        // Verify both vertices exist
        if (!graph_.contains(v1) || !graph_.contains(v2)) {
            throw std::runtime_error("Vertex not found");
        }
        
        // Check if edge already exists
        if (AreNeighbors(v1, v2)) {
            throw std::runtime_error("Edge already exists");
        }
        
        // Add to both adjacency lists (undirected)
        graph_[v1].push_back(v2);
        graph_[v2].push_back(v1);
        num_edges_++;
    }
    
    // Check if two vertices are neighbors
    bool AreNeighbors(const std::string& v1, const std::string& v2) {
        if (!graph_.contains(v1)) return false;
        
        auto& neighbors = graph_[v1];
        return std::find(neighbors.begin(), neighbors.end(), v2) 
               != neighbors.end();
    }
    
    // Get all neighbors of a vertex
    const std::list<std::string>& GetNeighbors(const std::string& v) {
        if (!graph_.contains(v)) {
            throw std::runtime_error("Vertex not found");
        }
        return graph_[v];
    }
    
    // Print the graph
    void Print() {
        std::cout << "Graph (" << num_vertices_ << " vertices, " 
                  << num_edges_ << " edges):\n";
        for (auto& [vertex, neighbors] : graph_) {
            std::cout << "  " << vertex << " → [";
            bool first = true;
            for (auto& n : neighbors) {
                if (!first) std::cout << ", ";
                std::cout << n;
                first = false;
            }
            std::cout << "]\n";
        }
    }
    
private:
    std::map<std::string, std::list<std::string>> graph_;
    int num_vertices_ = 0;
    int num_edges_ = 0;
};

int main() {
    UndirectedGraph g;
    
    // Add vertices (colors)
    g.AddVertex("Red");
    g.AddVertex("Blue");
    g.AddVertex("Green");
    g.AddVertex("Yellow");
    
    // Add edges
    g.AddEdge("Red", "Blue");
    g.AddEdge("Red", "Green");
    g.AddEdge("Blue", "Yellow");
    g.AddEdge("Green", "Yellow");
    
    g.Print();
    
    std::cout << "\nAre Red and Blue neighbors? " 
              << (g.AreNeighbors("Red", "Blue") ? "Yes" : "No") << "\n";
    std::cout << "Are Red and Yellow neighbors? " 
              << (g.AreNeighbors("Red", "Yellow") ? "Yes" : "No") << "\n";
    
    std::cout << "\nNeighbors of Red: ";
    for (auto& n : g.GetNeighbors("Red")) {
        std::cout << n << " ";
    }
    std::cout << "\n";
    
    return 0;
}
```

**Output:**
```
Graph (4 vertices, 4 edges):
  Blue → [Red, Yellow]
  Green → [Red, Yellow]
  Red → [Blue, Green]
  Yellow → [Blue, Green]

Are Red and Blue neighbors? Yes
Are Red and Yellow neighbors? No

Neighbors of Red: Blue Green 
```

---

## 🔍 Breadth-First Search (BFS)

BFS explores a graph **level by level**, visiting all neighbors before moving deeper.

### What BFS Can Answer

1. **Is there a path** from A to B?
2. **What is the shortest path** from A to B? (assuming equal edge weights)

### BFS Algorithm

```
┌─────────────────────────────────────────────────────────────────┐
│              BFS: LEVEL-BY-LEVEL EXPLORATION                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Start at A, find path to F:                                   │
│                                                                 │
│       (A)────(B)────(E)                                         │
│        │      │                                                 │
│        │      │                                                 │
│       (C)────(D)────(F)                                         │
│                                                                 │
│   Level 0: [A]           ← Start                                │
│   Level 1: [B, C]        ← A's neighbors                        │
│   Level 2: [E, D]        ← B and C's neighbors                  │
│   Level 3: [F]           ← D's neighbor → FOUND!                │
│                                                                 │
│   Shortest path: A → C → D → F (3 edges)                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### BFS Uses a Queue

```
┌─────────────────────────────────────────────────────────────────┐
│              BFS QUEUE OPERATIONS                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. Enqueue start vertex                                       │
│   2. While queue not empty:                                     │
│      a. Dequeue front vertex                                    │
│      b. If it's the goal → DONE!                                │
│      c. For each unvisited neighbor:                            │
│         - Mark as visited                                       │
│         - Enqueue it                                            │
│                                                                 │
│   Queue ensures FIFO: First neighbors are explored first        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### BFS Implementation

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <queue>
#include <unordered_map>
#include <unordered_set>
#include <algorithm>

class Graph {
public:
    Graph(int vertices) : num_vertices_(vertices) {
        adj_list_.resize(vertices);
    }
    
    void AddEdge(int v1, int v2) {
        adj_list_[v1].push_back(v2);
        adj_list_[v2].push_back(v1);
    }
    
    const std::list<int>& GetNeighbors(int v) {
        return adj_list_[v];
    }
    
    // BFS: Find shortest path from start to goal
    // Returns the path as a vector, or empty if no path exists
    std::vector<int> BFS(int start, int goal) {
        // Queue for BFS
        std::queue<int> to_visit;
        
        // Track visited vertices
        std::unordered_set<int> visited;
        
        // Track parent of each vertex (for path reconstruction)
        std::unordered_map<int, int> parent;
        
        // Start BFS
        to_visit.push(start);
        visited.insert(start);
        parent[start] = -1;  // Start has no parent
        
        while (!to_visit.empty()) {
            // Dequeue front vertex
            int current = to_visit.front();
            to_visit.pop();
            
            // Check if we found the goal
            if (current == goal) {
                // Reconstruct path from goal to start
                std::vector<int> path;
                int node = goal;
                while (node != -1) {
                    path.push_back(node);
                    node = parent[node];
                }
                // Reverse to get start → goal order
                std::reverse(path.begin(), path.end());
                return path;
            }
            
            // Explore all neighbors
            for (int neighbor : adj_list_[current]) {
                // Only visit unvisited neighbors
                if (visited.find(neighbor) == visited.end()) {
                    visited.insert(neighbor);
                    parent[neighbor] = current;
                    to_visit.push(neighbor);
                }
            }
        }
        
        // No path found
        return {};
    }
    
    // BFS: Check if path exists
    bool HasPath(int start, int goal) {
        return !BFS(start, goal).empty();
    }
    
private:
    int num_vertices_;
    std::vector<std::list<int>> adj_list_;
};

int main() {
    /*
     * Graph structure:
     *     0 ─── 1 ─── 4
     *     │     │
     *     │     │
     *     2 ─── 3 ─── 5
     */
    Graph g(6);
    g.AddEdge(0, 1);
    g.AddEdge(0, 2);
    g.AddEdge(1, 3);
    g.AddEdge(1, 4);
    g.AddEdge(2, 3);
    g.AddEdge(3, 5);
    
    // Find shortest path from 0 to 5
    std::cout << "Finding path from 0 to 5:\n";
    std::vector<int> path = g.BFS(0, 5);
    
    if (!path.empty()) {
        std::cout << "Path found: ";
        for (size_t i = 0; i < path.size(); i++) {
            std::cout << path[i];
            if (i < path.size() - 1) std::cout << " → ";
        }
        std::cout << "\nPath length: " << path.size() - 1 << " edges\n";
    } else {
        std::cout << "No path found!\n";
    }
    
    // Check connectivity
    std::cout << "\nHas path 0 → 4? " << (g.HasPath(0, 4) ? "Yes" : "No") << "\n";
    std::cout << "Has path 2 → 5? " << (g.HasPath(2, 5) ? "Yes" : "No") << "\n";
    
    return 0;
}
```

**Output:**
```
Finding path from 0 to 5:
Path found: 0 → 2 → 3 → 5
Path length: 3 edges

Has path 0 → 4? Yes
Has path 2 → 5? Yes
```

### BFS Step-by-Step Visualization

```
Finding path from 0 to 5:

Step 1: Queue = [0], Visited = {0}
        Dequeue 0, check neighbors [1, 2]
        
Step 2: Queue = [1, 2], Visited = {0, 1, 2}
        Dequeue 1, check neighbors [0, 3, 4]
        (0 already visited, add 3 and 4)
        
Step 3: Queue = [2, 3, 4], Visited = {0, 1, 2, 3, 4}
        Dequeue 2, check neighbors [0, 3]
        (0 and 3 already visited)
        
Step 4: Queue = [3, 4], Visited = {0, 1, 2, 3, 4}
        Dequeue 3, check neighbors [1, 2, 5]
        (1 and 2 visited, add 5)
        
Step 5: Queue = [4, 5], Visited = {0, 1, 2, 3, 4, 5}
        Dequeue 4... but let's check 5 first conceptually
        5 is our GOAL! → FOUND!
        
Path reconstruction: 5 ← 3 ← 2 ← 0
Reversed: 0 → 2 → 3 → 5
```

---

## 🔑 Key Takeaways

### Graph Basics

| Concept | Definition |
|---------|------------|
| **Graph** | Vertices + Edges |
| **Undirected** | Edges go both ways |
| **Directed** | Edges have direction |
| **Degree** | Number of edges at a vertex |
| **Path** | Sequence of vertices connected by edges |
| **Cycle** | Path that returns to starting vertex |
| **Connected** | Path exists between all vertex pairs |

### Representations

| Representation | Space | Edge Check | Best For |
|---------------|-------|------------|----------|
| **Adjacency Matrix** | O(V²) | O(1) | Dense graphs |
| **Adjacency List** | O(V+E) | O(degree) | Sparse graphs |

### BFS Algorithm

```cpp
// BFS Pseudocode
queue.push(start);
visited.insert(start);

while (!queue.empty()) {
    current = queue.front();
    queue.pop();
    
    if (current == goal) return SUCCESS;
    
    for (neighbor : current.neighbors) {
        if (!visited.contains(neighbor)) {
            visited.insert(neighbor);
            queue.push(neighbor);
        }
    }
}
return NOT_FOUND;
```

### BFS Properties

- Uses a **queue** (FIFO)
- Explores **level by level**
- Finds **shortest path** (unweighted edges)
- Time complexity: **O(V + E)**
- Space complexity: **O(V)**

