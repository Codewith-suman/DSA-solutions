# Unit 8 - Trees & Graphs: SOLVED ANSWERS

---

## **Question 1: Write algorithms for INSERT and DELETE in Binary Search Tree [10 Marks]**

**Asked in**: 2074, 2075, 2076, 2078, 2080

### Answer:

#### **BST Properties**

```
Binary Search Tree:

        50
       /  \
      30   70
     / \   / \
    20 40 60 80

Rule: left_child < parent < right_child
```

---

## **INSERT Operation**

### **Algorithm**

```
Algorithm INSERT_BST(root, value)
    
    if root == NULL:
        Create new node with value
        return new node
    
    if value < root.data:
        root.left = INSERT_BST(root.left, value)
    
    else if value > root.data:
        root.right = INSERT_BST(root.right, value)
    
    else:  // value == root.data
        return root  // Duplicate, not inserted
    
    return root
```

### **C Code**

```c
struct Node {
    int data;
    struct Node *left;
    struct Node *right;
};

struct Node *createNode(int value) {
    struct Node *newNode = (struct Node *)malloc(sizeof(struct Node));
    newNode->data = value;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

struct Node *insert(struct Node *root, int value) {
    if (root == NULL) {
        return createNode(value);
    }
    
    if (value < root->data) {
        root->left = insert(root->left, value);
    } 
    else if (value > root->data) {
        root->right = insert(root->right, value);
    }
    // Duplicate values ignored
    
    return root;
}
```

### **Example**

```
Insert: 50, 30, 70, 20, 40, 60, 80

Step 1: Insert 50
  50 (root)

Step 2: Insert 30 (30 < 50, go left)
      50
      /
    30

Step 3: Insert 70 (70 > 50, go right)
      50
     /  \
    30   70

Step 4: Insert 20 (20 < 50, left; 20 < 30, left)
      50
     /  \
    30   70
   /
  20

Step 5: Insert 40 (40 < 50, left; 40 > 30, right)
      50
     /  \
    30   70
   / \
  20 40

And so on... (similar for 60, 80)

Final Tree:
        50
       /  \
      30   70
     / \  / \
    20 40 60 80
```

---

## **DELETE Operation**

**Three cases**:

### **Case 1: Delete Leaf Node (no children)**

```
Delete 20:

Before:        After:
    50             50
   /  \           /  \
  30   70        30   70
 / \            / \
20 40          40
```

**Algorithm**:
```
Simply remove the node
```

### **Case 2: Delete Node with ONE Child**

```
Delete 30 (has only left child):

Before:        After:
    50             50
   /  \           /  \
  30   70  →     20   70
 /
20
```

**Algorithm**:
```
Replace node with its child
```

### **Case 3: Delete Node with TWO Children** (MOST COMPLEX)

```
Delete 50 (has both children):

Before:            After:
    50                 40
   /  \               /  \
  30   70      or    30   70
 / \   /            /
20 40 60           20
```

**Two strategies**:

**Option A: In-order Predecessor**
- Find largest in left subtree (30 → 40)
- Replace node with predecessor
- Delete predecessor from left subtree

**Option B: In-order Successor**
- Find smallest in right subtree (70 → 60)
- Replace node with successor
- Delete successor from right subtree

---

## **Delete Algorithm (Complete)**

```
Algorithm DELETE_BST(root, value)
    
    if root == NULL:
        return NULL
    
    // Case 1 & 2: Go left or right
    if value < root.data:
        root.left = DELETE_BST(root.left, value)
    
    else if value > root.data:
        root.right = DELETE_BST(root.right, value)
    
    // Case 3: Found the node
    else:
        
        // No children (leaf)
        if root.left == NULL AND root.right == NULL:
            free(root)
            return NULL
        
        // One child (right)
        if root.left == NULL:
            struct Node *temp = root.right
            free(root)
            return temp
        
        // One child (left)
        if root.right == NULL:
            struct Node *temp = root.left
            free(root)
            return temp
        
        // Two children: Find in-order predecessor
        struct Node *predecessor = root.left
        while predecessor.right != NULL:
            predecessor = predecessor.right
        
        root.data = predecessor.data  // Copy predecessor's value
        root.left = DELETE_BST(root.left, predecessor.data)  // Delete predecessor
    
    return root
```

---

## **C Code for Delete**

```c
struct Node *findMin(struct Node *root) {
    while (root->left != NULL)
        root = root->left;
    return root;
}

struct Node *delete(struct Node *root, int value) {
    if (root == NULL)
        return NULL;
    
    if (value < root->data) {
        root->left = delete(root->left, value);
    } 
    else if (value > root->data) {
        root->right = delete(root->right, value);
    } 
    else {
        // Node to delete found
        
        // Case 1: Leaf node
        if (root->left == NULL && root->right == NULL) {
            free(root);
            return NULL;
        }
        
        // Case 2: Only right child
        if (root->left == NULL) {
            struct Node *temp = root->right;
            free(root);
            return temp;
        }
        
        // Case 2: Only left child
        if (root->right == NULL) {
            struct Node *temp = root->left;
            free(root);
            return temp;
        }
        
        // Case 3: Both children - find predecessor
        struct Node *predecessor = findMin(root->right);  // Or findMax(root->left)
        root->data = predecessor->data;
        root->right = delete(root->right, predecessor->data);
    }
    
    return root;
}
```

---

## **Complexity Analysis**

| Operation | Best | Average | Worst |
|-----------|------|---------|-------|
| **Search** | O(log n) | O(log n) | O(n) |
| **Insert** | O(log n) | O(log n) | O(n) |
| **Delete** | O(log n) | O(log n) | O(n) |

```
Best case (balanced):                Worst case (skewed):
      50                             10
     /  \                            /
    30   70        vs              20
   / \   / \                       /
  20 40 60 80                     30
                                  /
                                 40

Height = log n                     Height = n
```

---

## **Question 2: Explain and trace DIJKSTRA'S ALGORITHM [10 Marks]**

**Asked in**: 2076, 2078, 2079, 2081

### Answer:

#### **Concept**

**Dijkstra's Algorithm** finds **shortest path** from source to all vertices in weighted graph.

```
Requirements:
✓ Weighted graph (non-negative edges only!)
✓ Source vertex specified
✓ Finds single-source shortest paths
```

---

## **Algorithm**

```
Algorithm DIJKSTRA(graph, source)
    
    // Initialize
    distance[all] = INFINITY
    distance[source] = 0
    visited[all] = FALSE
    
    for each vertex v:
        v is not visited
    
    while unvisited vertices exist:
        
        // Find unvisited vertex with minimum distance
        currentVertex = vertex with min distance and NOT visited
        visited[currentVertex] = TRUE
        
        // Relax edges
        for each neighbor of currentVertex:
            if distance[currentVertex] + edge_weight < distance[neighbor]:
                distance[neighbor] = distance[currentVertex] + edge_weight
    
    return distance[]
```

---

## **Example: Step-by-Step Trace**

**Graph**:
```
    1
   / \
  1   4
 /     \
0 ---2-- 2
 \     / 
  5   1
   \ /
    3
    
Edges:
0→1: 1
0→2: 2
0→3: 5
1→2: 4
2→3: 1
```

**Trace - Find shortest from 0**:

```
Initial:
distance = [0, ∞, ∞, ∞]
visited = [F, F, F, F]

Step 1: Current = 0 (min distance, not visited)
  visited[0] = T
  
  Relax edges from 0:
  - To 1: 0 + 1 = 1 < ∞ → distance[1] = 1 ✓
  - To 2: 0 + 2 = 2 < ∞ → distance[2] = 2 ✓
  - To 3: 0 + 5 = 5 < ∞ → distance[3] = 5 ✓
  
  distance = [0, 1, 2, 5]
  visited = [T, F, F, F]

Step 2: Current = 1 (min distance=1, not visited)
  visited[1] = T
  
  Relax edges from 1:
  - To 2: 1 + 4 = 5, but 2 < 5 → no update
  
  distance = [0, 1, 2, 5]
  visited = [T, T, F, F]

Step 3: Current = 2 (min distance=2, not visited)
  visited[2] = T
  
  Relax edges from 2:
  - To 3: 2 + 1 = 3 < 5 → distance[3] = 3 ✓
  
  distance = [0, 1, 2, 3]
  visited = [T, T, T, F]

Step 4: Current = 3 (min distance=3, not visited)
  visited[3] = T
  
  No outgoing edges
  
  distance = [0, 1, 2, 3]
  visited = [T, T, T, T]

Final Shortest Distances from 0:
0→0 = 0
0→1 = 1 (via direct edge)
0→2 = 2 (via direct edge)
0→3 = 3 (via 0→2→3)
```

---

## **C Code**

```c
#include <stdio.h>
#include <limits.h>

#define V 4  // Number of vertices

int minDistance(int dist[], int visited[]) {
    int min = INT_MAX, minIdx = -1;
    
    for (int i = 0; i < V; i++) {
        if (!visited[i] && dist[i] < min) {
            min = dist[i];
            minIdx = i;
        }
    }
    
    return minIdx;
}

void dijkstra(int graph[V][V], int src) {
    int dist[V];
    int visited[V];
    
    // Initialize
    for (int i = 0; i < V; i++) {
        dist[i] = INT_MAX;
        visited[i] = 0;
    }
    dist[src] = 0;
    
    // Main loop
    for (int i = 0; i < V; i++) {
        int u = minDistance(dist, visited);
        
        if (u == -1) break;
        
        visited[u] = 1;
        
        // Relax edges
        for (int v = 0; v < V; v++) {
            if (graph[u][v] && !visited[v] && 
                dist[u] + graph[u][v] < dist[v]) {
                dist[v] = dist[u] + graph[u][v];
            }
        }
    }
    
    // Print results
    printf("Vertex  Distance\n");
    for (int i = 0; i < V; i++)
        printf("%d       %d\n", i, dist[i]);
}
```

**Complexity**: O(V²) or O((V+E)log V) with priority queue

---

## **Question 3: Explain KRUSKAL'S and PRIM'S Algorithms [10 Marks]**

**Asked in**: 2074, 2075, 2076, 2078, 2080

### Answer:

#### **Minimum Spanning Tree (MST)**

An **MST** connects all vertices with minimum total edge weight (no cycles).

```
Graph:              MST (one possible):
    1
   / \              1
  1   4            / \
 /     \           1   2
0 ---2-- 2    →   0---2--2
 \     / 
  5   1
   \ /
    3

Total weight: 1 + 4 + 2 + 1 = 8 (for MST)
```

---

## **KRUSKAL'S ALGORITHM**

**Greedy approach**: Sort edges, add smallest edge if no cycle formed.

### **Algorithm**

```
Algorithm KRUSKAL(graph)
    
    Sort all edges by weight (ascending)
    Initialize: each vertex is own component
    
    result = empty list
    
    for each edge (u, v) in sorted order:
        if u and v are in different components:
            Add edge to result (MST)
            Merge components of u and v
    
    return result
```

### **Example**

```
Graph edges (sorted):
0-3: 1
2-3: 1
0-1: 1
1-2: 4
0-2: 2
1-3: 5

Kruskal's Process:

Edge 0-3(1): Different components → Add ✓
Edge 2-3(1): Different components → Add ✓
Edge 0-1(1): Different components → Add ✓
Edge 1-2(4): Create cycle (all connected) → Skip ✗
Edge 0-2(2): Create cycle → Skip ✗
Edge 1-3(5): Create cycle → Skip ✗

MST edges: 0-3, 2-3, 0-1
Total: 1 + 1 + 1 = 3
```

### **Union-Find (Disjoint Set)**

```c
int parent[V];
int rank[V];

void makeSet() {
    for (int i = 0; i < V; i++) {
        parent[i] = i;
        rank[i] = 0;
    }
}

int find(int x) {
    if (parent[x] != x)
        parent[x] = find(parent[x]);  // Path compression
    return parent[x];
}

void unite(int x, int y) {
    int px = find(x);
    int py = find(y);
    
    if (px == py) return;  // Already connected
    
    // Union by rank
    if (rank[px] < rank[py])
        parent[px] = py;
    else if (rank[px] > rank[py])
        parent[py] = px;
    else {
        parent[py] = px;
        rank[px]++;
    }
}
```

---

## **PRIM'S ALGORITHM**

**Greedy approach**: Start from vertex, add minimum edge connecting tree to new vertex.

### **Algorithm**

```
Algorithm PRIM(graph, startVertex)
    
    key[all] = INFINITY
    key[start] = 0
    inMST[all] = FALSE
    
    for i = 0 to V-1:
        // Find vertex with min key not in MST
        u = vertex with minimum key and NOT inMST
        inMST[u] = TRUE
        
        // Update keys of neighbors
        for each neighbor v of u:
            if v not in MST AND weight(u,v) < key[v]:
                key[v] = weight(u,v)
                parent[v] = u
    
    return parent[]  // MST edges
```

### **Example**

```
Start from vertex 0:

Initial:
key = [0, ∞, ∞, ∞]
inMST = [F, F, F, F]

Step 1: Add 0 to MST
  inMST[0] = T
  
  Update neighbors:
  key[1] = min(∞, 1) = 1
  key[2] = min(∞, 2) = 2
  key[3] = min(∞, 5) = 5
  
  key = [0, 1, 2, 5]

Step 2: Add 1 (min key=1)
  inMST[1] = T
  
  Update:
  key[2] = min(2, 4) = 2  (no change)
  key[3] = min(5, ∞) = 5  (no edge)
  
  key = [0, 1, 2, 5]

Step 3: Add 2 (min key=2)
  inMST[2] = T
  
  Update:
  key[3] = min(5, 1) = 1  ✓
  
  key = [0, 1, 2, 1]

Step 4: Add 3 (min key=1)
  inMST[3] = T
  
  All vertices in MST!

MST: 0-1(1), 0-2(2), 2-3(1)
Total: 4
```

---

## **Comparison: Kruskal vs Prim**

| Aspect | Kruskal | Prim |
|--------|---------|------|
| **Approach** | Edge-based | Vertex-based |
| **Greedy** | Sort edges | Expand tree |
| **Structure** | Forest → Tree | Tree grows |
| **Best** | Sparse graphs | Dense graphs |
| **Data structure** | Union-Find | Priority queue |
| **Time** | O(E log E) | O(E log V) |

---

## **Question 4: Explain BFS and DFS [5 Marks]**

**Asked in**: 2074, 2076

### Answer:

#### **Graph Traversal**

**BFS (Breadth-First Search)**: Level by level (queue)
**DFS (Depth-First Search)**: Deep exploration (stack)

```
Graph:      0
           /|\
          1 2 3
           \|
            4

BFS:  0 → 1,2,3 → 4     (levels: {0}, {1,2,3}, {4})
DFS:  0 → 1 → 4 → back → 2 → back → 3
```

---

## **BFS Algorithm**

```
Algorithm BFS(graph, start)
    
    queue = empty
    visited[all] = FALSE
    
    queue.enqueue(start)
    visited[start] = TRUE
    
    while queue is not empty:
        vertex = queue.dequeue()
        print vertex
        
        for each neighbor of vertex:
            if not visited[neighbor]:
                queue.enqueue(neighbor)
                visited[neighbor] = TRUE
```

**C Code**:
```c
void bfs(int adj[V][V], int start) {
    int visited[V] = {0};
    int queue[V], front = 0, rear = -1;
    
    visited[start] = 1;
    queue[++rear] = start;
    
    while (front <= rear) {
        int v = queue[front++];
        printf("%d ", v);
        
        for (int i = 0; i < V; i++) {
            if (adj[v][i] && !visited[i]) {
                visited[i] = 1;
                queue[++rear] = i;
            }
        }
    }
}
```

---

## **DFS Algorithm**

```
Algorithm DFS(graph, vertex)
    
    visited[vertex] = TRUE
    print vertex
    
    for each neighbor of vertex:
        if not visited[neighbor]:
            DFS(graph, neighbor)
```

**C Code**:
```c
void dfs(int adj[V][V], int v, int visited[]) {
    visited[v] = 1;
    printf("%d ", v);
    
    for (int i = 0; i < V; i++) {
        if (adj[v][i] && !visited[i]) {
            dfs(adj, i, visited);
        }
    }
}
```

---

## **BFS vs DFS**

| Feature | BFS | DFS |
|---------|-----|-----|
| **Data Structure** | Queue | Stack/Recursion |
| **Order** | Level by level | Depth first |
| **Space** | O(V) queue | O(V) recursion |
| **Time** | O(V+E) | O(V+E) |
| **Use** | Shortest path | Topological sort |
| **Memory** | More (breadth) | Less (depth) |

---

## **Quick Summary - Unit 8**

| Topic | Key Formula/Points |
|-------|-------------------|
| **BST Insert** | Compare, go left/right, insert |
| **BST Delete** | 3 cases: leaf, one child, two children |
| **Dijkstra** | O(V²), non-negative weights required |
| **Kruskal** | Sort edges, use Union-Find for cycles |
| **Prim** | Add minimum edge to expanding tree |
| **BFS** | Level by level, use queue |
| **DFS** | Deep exploration, use recursion/stack |

