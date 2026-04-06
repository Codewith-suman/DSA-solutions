# Prim's Algorithm - Minimum Spanning Tree

**Questions Asked**: 4 times (2075, 2078, 2081, and variations)  
**Marks**: 5-10 marks  
**Difficulty**: ⭐⭐⭐ Medium

---

## Problem Statement

Find the **Minimum Spanning Tree** using **Prim's Algorithm**.

---

## Concept Overview

### Prim's Approach (Vertex-Based)
1. Start from any vertex
2. Key insight: Always add the **minimum weight edge** that connects tree to outside
3. Keep expanding tree until all vertices included

### Comparison with Kruskal
- **Kruskal**: Edge-based (sort edges)
- **Prim**: Vertex-based (grow from vertex)

---

## Algorithm

```
Algorithm PRIM(Graph G, source)
    
    // Initialize
    for each vertex v:
        key[v] = INFINITY
        inMST[v] = false
    
    key[source] = 0
    
    // Build MST with V vertices
    for i = 1 to V:
        
        // Find minimum key vertex not in MST
        u = vetex with min key[u] where inMST[u] = false
        inMST[u] = true
        
        // Update key values of adjacent vertices
        for each vertex v adjacent to u:
            if not inMST[v] and weight(u,v) < key[v]:
                key[v] = weight(u,v)
                parent[v] = u
    
    return MST from parent array
```

---

## Trace Example

### Graph Setup
```
Graph with edges:
A-B:1  A-D:8  B-C:2  B-E:1  C-E:1  D-E:5

Start: A
```

### Step 1: Initialize at A
```
key:     [0, ∞, ∞, ∞, ∞]
inMST:   [T, F, F, F, F]
        A  B  C  D  E
parent:  [-1, A, -, -, -]
```

### Step 2: Add neighbors of A
```
Select A (min key = 0)
Update:
  B: key=min(∞, 1) = 1, parent=A
  D: key=min(∞, 8) = 8, parent=A

State:
key:     [0, 1, ∞, 8, ∞]
inMST:   [T, F, F, F, F]
parent:  [-1, A, -, A, -]
```

### Step 3: Select B (key=1)
```
Select B (min unvisited key = 1)
Update neighbors:
  C: key=min(∞, 2) = 2, parent=B
  E: key=min(∞, 1) = 1, parent=B

State:
key:     [0, 1, 2, 8, 1]
inMST:   [T, T, F, F, F]
parent:  [-1, A, B, A, B]
```

### Step 4: Select E (key=1)
```
Select E (min unvisited key = 1)
Update neighbors:
  C: key=min(2, 1) = 1, parent=E
  D: key=min(8, 5) = 5, parent=E

State:
key:     [0, 1, 1, 5, 1]
inMST:   [T, T, F, F, T]
parent:  [-1, A, E, E, B]
```

### Step 5: Select C (key=1)
```
Select C (min unvisited key = 1)
No improvements

State:
key:     [0, 1, 1, 5, 1]
inMST:   [T, T, T, F, T]
parent:  [-1, A, E, E, B]
```

### Step 6: Select D (key=5)
```
Select D (key=5)
All vertices visited

MST Complete!
```

---

## Step Table

| Step | Selected | Key | inMST | Parent | Total Weight |
|------|----------|-----|-------|--------|--------------|
| 1 | A | 0 | A | - | 0 |
| 2 | B | 1 | A,B | A,A | 1 |
| 3 | E | 1 | A,B,E | A,B,B | 2 |
| 4 | C | 1 | A,B,E,C | A,B,E,E | 3 |
| 5 | D | 5 | A,B,E,C,D | A,B,B,E,E | **8** ✓ |

---

## C Code Implementation

```c
#include <stdio.h>
#include <limits.h>

#define V 5
#define INF INT_MAX

int minKey(int key[], int inMST[]) {
    int min = INF;
    int min_idx = -1;
    
    for (int v = 0; v < V; v++) {
        if (!inMST[v] && key[v] < min) {
            min = key[v];
            min_idx = v;
        }
    }
    
    return min_idx;
}

void prim(int graph[V][V]) {
    printf("=== PRIM'S ALGORITHM ===\n\n");
    
    int key[V];
    int inMST[V] = {0};
    int parent[V];
    
    // Initialize
    for (int i = 0; i < V; i++) {
        key[i] = INF;
        parent[i] = -1;
    }
    
    key[0] = 0;  // Start from vertex 0 (A)
    
    for (int count = 0; count < V; count++) {
        int u = minKey(key, inMST);
        
        if (u == -1) break;
        
        inMST[u] = 1;
        
        printf("Step %d: Select %c (key=%d)\n", count + 1, 'A' + u, key[u]);
        
        // Update adjacent vertices
        for (int v = 0; v < V; v++) {
            if (graph[u][v] && !inMST[v] && graph[u][v] < key[v]) {
                parent[v] = u;
                key[v] = graph[u][v];
                printf("  Update %c: key=%d, parent=%c\n",
                       'A' + v, key[v], 'A' + u);
            }
        }
        printf("\n");
    }
    
    // Print MST
    printf("=== MST EDGES ===\n");
    int totalWeight = 0;
    for (int i = 1; i < V; i++) {
        if (parent[i] != -1) {
            printf("%c-%c: %d\n", 'A' + parent[i], 'A' + i, key[i]);
            totalWeight += key[i];
        }
    }
    
    printf("\nTotal Weight: %d\n", totalWeight);
}

int main() {
    int graph[V][V] = {
        {0, 1, 0, 8, 0},  // A
        {1, 0, 2, 0, 1},  // B
        {0, 2, 0, 0, 1},  // C
        {8, 0, 0, 0, 5},  // D
        {0, 1, 1, 5, 0}   // E
    };
    
    prim(graph);
    
    return 0;
}
```

---

## Complexity Analysis

### Time Complexity
- **Naive**: O(V²) - Linear search for minimum
- **With Min-Heap**: O(E log V)
- **Best**: O(E + V log V)

### Space Complexity
- **O(V)** for key, inMST, parent arrays

---

## Prim vs Kruskal

| Factor | Prim | Kruskal |
|--------|------|---------|
| Start | Any vertex | All edges |
| Tree Growth | Vertex-by-vertex | Edge-by-edge |
| Sorting | No | Yes (O(E logE)) |
| Dense Graph | Faster | Slower |
| Sparse Graph | Slower | Faster |
| Implementation | Simple | Union-Find needed |

---

## Key Points

✅ **Remember**:
1. Start from any vertex
2. Always pick minimum edge to unvisited vertex
3. Continue until all vertices in MST
4. Total edges = V-1

❌ **Mistakes**:
- Choosing arbitrary edges
- Not tracking visited vertices
- Wrong parent updates

---

## Related Problems

- [Kruskal's Algorithm](./kruskal_algorithm.md)
- [Dijkstra's Algorithm](./dijkstra_algorithm.md)
