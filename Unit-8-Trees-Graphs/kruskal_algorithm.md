# Kruskal's Algorithm - Minimum Spanning Tree

**Questions Asked**: 5 times (Model, 2075, 2078, 2079, 2081)  
**Marks**: 5-10 marks  
**Difficulty**: ⭐⭐⭐ Medium

---

## Problem Statement

Find the **Minimum Spanning Tree (MST)** using **Kruskal's Algorithm**.

**Example Graph**:
```
    1        2
A --- B --- C
|          /
|8        1
|        /
D ------ E
   5
```

Find MST with minimum total edge weight.

---

## Concept Overview

### Spanning Tree
- Subgraph containing all vertices
- Exactly (V-1) edges
- No cycles
- Connected

### Minimum Spanning Tree
- Spanning tree with **minimum total edge weight**
- Multiple MSTs possible if equal weights
- Cost = sum of all edge weights

### Kruskal's Approach (Greedy)
- Sort edges by weight
- Add edges one by one
- Skip if creates cycle
- Stop when (V-1) edges added

---

## Algorithm

### Key Idea
```
1. Sort all edges by weight (ascending)
2. Initialize: each vertex is separate component
3. For each edge (u,v) in sorted order:
   - If u and v in different components:
     - Add edge to MST
     - Merge components
4. Continue until (V-1) edges selected
```

### Pseudocode

```
Algorithm KRUSKAL(Graph G)
    
    // Sort edges by weight
    edges = all edges in G
    SORT(edges) by weight ascending
    
    // Initialize disjoint sets
    for each vertex v in G:
        MakeSet(v)
    
    // Build MST
    MST = {}  // Empty set
    
    for each edge (u,v) with weight w in edges:
        if FindSet(u) ≠ FindSet(v):
            // Different components - safe to add
            ADD (u,v,w) to MST
            Union(FindSet(u), FindSet(v))
    
    return MST
```

### Union-Find (Disjoint Set)

```
MakeSet(x):      Create component with just x
FindSet(x):      Return representative of component
Union(x, y):     Merge components of x and y
```

---

## Trace Example

### Graph & Edges
```
Vertices: A, B, C, D, E
Edges:
  A-B: 1   A-D: 8   B-C: 2
  B-E: 1   C-E: 1   D-E: 5

Sorted by weight:
1. A-B: 1
2. B-E: 1  
3. C-E: 1  ← Actually (we have choices)
4. B-C: 2
5. D-E: 5
6. A-D: 8
```

### Step 1: Initialize
```
Components: {A}, {B}, {C}, {D}, {E}
MST weight: 0
Edges selected: 0
```

### Step 2: Add A-B (weight=1)
```
Check: A and B in different components? YES
Action: Add A-B to MST
Union: {A,B}, {C}, {D}, {E}
MST weight: 1
```

### Step 3: Add B-E (weight=1)
```
Check: B and E in different components? YES
Action: Add B-E to MST
Union: {A,B,E}, {C}, {D}
MST weight: 2
```

### Step 4: Try C-E (weight=1)
```
Check: C and E in different components? YES
Action: Add C-E to MST
Union: {A,B,C,E}, {D}
MST weight: 3
Edges selected: 3
```

### Step 5: Try B-C (weight=2)
```
Check: B and C in different components? NO
Action: SKIP (would create cycle)
MST weight: still 3
```

### Step 6: Add D-E (weight=5)
```
Check: D and E in different components? YES
Action: Add D-E to MST
Union: {A,B,C,D,E}
MST weight: 8
Edges selected: 4 = V-1 ✓
STOP
```

### Final MST
```
Edges: A-B(1), B-E(1), C-E(1), D-E(5)
Total weight: 8
```

---

## Step-by-Step Table

| Step | Edge | Weight | Components | Action | Total | Edges |
|------|------|--------|-----------|--------|-------|-------|
| 1 | A-B | 1 | {A,B},{C},{D},{E} | Add | 1 | 1 |
| 2 | B-E | 1 | {A,B,E},{C},{D} | Add | 2 | 2 |
| 3 | C-E | 1 | {A,B,C,E},{D} | Add | 3 | 3 |
| 4 | B-C | 2 | Same | Skip | 3 | 3 |
| 5 | D-E | 5 | {A,B,C,D,E} | Add | **8** | **4** ✓ |

---

## C Code Implementation

```c
#include <stdio.h>
#include <stdlib.h>

#define V 5
#define E 6

struct Edge {
    int u, v, weight;
};

struct Subset {
    int parent;
    int rank;
};

// Find with path compression
int find(struct Subset subsets[], int i) {
    if (subsets[i].parent != i)
        subsets[i].parent = find(subsets, subsets[i].parent);
    return subsets[i].parent;
}

// Union by rank
void unionSets(struct Subset subsets[], int x, int y) {
    int xroot = find(subsets, x);
    int yroot = find(subsets, y);
    
    if (subsets[xroot].rank < subsets[yroot].rank)
        subsets[xroot].parent = yroot;
    else if (subsets[xroot].rank > subsets[yroot].rank)
        subsets[yroot].parent = xroot;
    else {
        subsets[yroot].parent = xroot;
        subsets[xroot].rank++;
    }
}

int compare(const void *a, const void *b) {
    return ((struct Edge *)a)->weight - ((struct Edge *)b)->weight;
}

void kruskal(struct Edge edges[E]) {
    printf("=== KRUSKAL'S ALGORITHM ===\n\n");
    
    // Sort edges
    qsort(edges, E, sizeof(struct Edge), compare);
    
    printf("Sorted Edges:\n");
    for (int i = 0; i < E; i++)
        printf("%c-%c: %d\n", 'A' + edges[i].u, 'A' + edges[i].v, edges[i].weight);
    printf("\n");
    
    // Initialize subsets
    struct Subset subsets[V];
    for (int i = 0; i < V; i++) {
        subsets[i].parent = i;
        subsets[i].rank = 0;
    }
    
    // Build MST
    struct Edge result[V - 1];
    int resultIndex = 0;
    int totalWeight = 0;
    
    printf("Building MST:\n");
    for (int i = 0; i < E && resultIndex < V - 1; i++) {
        int x = find(subsets, edges[i].u);
        int y = find(subsets, edges[i].v);
        
        if (x != y) {
            result[resultIndex++] = edges[i];
            totalWeight += edges[i].weight;
            
            printf("✓ Add %c-%c (%d): Components merge\n",
                   'A' + edges[i].u, 'A' + edges[i].v, edges[i].weight);
            
            unionSets(subsets, x, y);
        } else {
            printf("✗ Skip %c-%c (%d): Creates cycle\n",
                   'A' + edges[i].u, 'A' + edges[i].v, edges[i].weight);
        }
    }
    
    // Print result
    printf("\n=== MINIMUM SPANNING TREE ===\n");
    printf("Edges:\n");
    for (int i = 0; i < resultIndex; i++)
        printf("  %c-%c: %d\n", 
               'A' + result[i].u, 'A' + result[i].v, result[i].weight);
    
    printf("Total Weight: %d\n", totalWeight);
}

int main() {
    struct Edge edges[E] = {
        {0, 1, 1},  // A-B
        {0, 3, 8},  // A-D
        {1, 2, 2},  // B-C
        {1, 4, 1},  // B-E
        {2, 4, 1},  // C-E
        {3, 4, 5}   // D-E
    };
    
    kruskal(edges);
    
    return 0;
}
```

---

## Comparison: Kruskal vs Prim

| Aspect | Kruskal | Prim |
|--------|---------|------|
| Type | Edge-based | Vertex-based |
| Greedy | Sort edges | Grow tree |
| Implementation | Union-Find | Min-Heap |
| Best for | Sparse graphs | Dense graphs |
| Complexity | O(E log E) | O(E + V log V) |

---

## Union-Find Optimization

### Path Compression (Find)
```c
int find(int x) {
    if (parent[x] != x)
        parent[x] = find(parent[x]);  // Compress path
    return parent[x];
}
```

### Union by Rank
```c
void union(int x, int y) {
    x = find(x);
    y = find(y);
    if (rank[x] < rank[y])
        parent[x] = y;
    else
        parent[y] = x;
}
```

**Result**: Nearly O(1) per operation!

---

## Complexity Analysis

### Time Complexity

**Without optimization**:
- Sort edges: O(E log E)
- Union-Find: O(E·α(V)) ≈ O(E)
- **Total: O(E log E)**

**With Union-Find optimization**:
- α(V) → Ackermann inverse → practically constant
- Same: **O(E log E)** due to sorting

### Space Complexity
- **O(V + E)** for storing edges and sets

---

## Key Points for Exam

✅ **Must Answer**:
1. **Sort edges** by weight first
2. **Union-Find** for cycle detection
3. **Add edge if** different components
4. **Stop when** V-1 edges added
5. Show complete trace table

❌ **Common Mistakes**:
- Forgetting to sort edges
- Wrong order of processing
- Not checking for cycles
- Incomplete trace

⚡ **Important**:
- **Always produces MST**
- **O(E log E)** due to sorting
- Does not require starting vertex
- Good for sparse graphs

---

## Exam Answer Template

**Question**: Find MST using Kruskal's algorithm.

**Answer**:
1. "First, sort edges by weight"
2. "Use Union-Find to track components"
3. "Add edges only if they don't create cycle"
4. "Continue until (V-1) edges are added"
5. [Show trace table]
6. "Final MST weight: X"

---

## Related Problems

- [Prim's Algorithm](./prim_algorithm.md)
- [Dijkstra's Algorithm](./dijkstra_algorithm.md)
- [BFS/DFS](./bfs_dfs.md)
