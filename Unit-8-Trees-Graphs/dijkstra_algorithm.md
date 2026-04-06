# Dijkstra's Algorithm - Shortest Path

**Questions Asked**: 4 times (2074, 2076, 2080, and variations)  
**Marks**: 5-10 marks  
**Difficulty**: ⭐⭐⭐⭐ High

---

## Problem Statement

Find the **shortest path from a source vertex** to all other vertices using **Dijkstra's Algorithm**.

**Example Graph**:
```
    (4)          (2)
A ------- B ------- C
|                   |
|(7)            (1)|
|                   |
D ---------- E -----+
    (5)      (3)
```

Find shortest path from A to all vertices.

---

## Concept Overview

### Greedy Algorithm
- Makes locally optimal choices
- Works for non-negative edge weights
- Always picks nearest unvisited vertex
- Guarantees globally optimal path

### Why It Works
- Once shortest path to vertex found, it's final
- No negative edge can shorten it
- Can't improve by visiting longer paths first

### Key Components
1. **Distance array**: Current shortest distance from source
2. **Visited set**: Vertices with confirmed shortest path
3. **Previous array**: Track path for reconstruction

---

## Algorithm

### Pseudocode

```
Algorithm DIJKSTRA(Graph, source)
    
    // Initialize
    for each vertex v in Graph:
        distance[v] = INFINITY
        visited[v] = false
    
    distance[source] = 0
    
    // Find shortest path
    repeat for all vertices:
        
        // Find unvisited vertex with minimum distance
        u = vertex with minimum distance[u] where visited[u] = false
        
        if distance[u] = INFINITY:
            break  // Disconnected vertex
        
        visited[u] = true
        
        // Update distances to neighbors
        for each neighbor v of u:
            if not visited[v]:
                newDistance = distance[u] + weight(u,v)
                
                if newDistance < distance[v]:
                    distance[v] = newDistance
                    previous[v] = u
    
    return distance[], previous[]
```

---

## Trace Example

### Initial Setup
```
Graph:
    1        2
A --- B --- C
|          /
|8        1
|        /
D ------ E
   5

From: A
Distance: [0, ∞, ∞, ∞, ∞]
           A  B  C  D  E
```

### Step 1: Start at A
```
Select A (distance=0, unvisited)
Mark A as visited
Update neighbors:
  B: 0 + 1 = 1 (better than ∞)
  D: 0 + 8 = 8 (better than ∞)

Distance: [0, 1, ∞, 8, ∞]
Visited:  [✓, , , , ]
```

### Step 2: Go to B (min distance=1)
```
Select B (distance=1)
Mark B as visited
Update neighbors:
  C: 1 + 2 = 3 (better than ∞)
  E: 1 + 1 = 2 (better than ∞)

Distance: [0, 1, 3, 8, 2]
Visited:  [✓, ✓, , , ]
```

### Step 3: Go to E (min distance=2)
```
Select E (distance=2)
Mark E as visited
Update neighbors:
  C: 2 + 1 = 3 (not better)
  D: 2 + 5 = 7 (better than 8!)

Distance: [0, 1, 3, 7, 2]
Visited:  [✓, ✓, , , ✓]
```

### Step 4: Go to C (min distance=3)
```
Select C (distance=3)
Mark C as visited
No unvisited neighbors

Distance: [0, 1, 3, 7, 2]
Visited:  [✓, ✓, ✓, , ✓]
```

### Step 5: Go to D (min distance=7)
```
Select D (distance=7)
Mark D as visited
All neighbors visited

Distance: [0, 1, 3, 7, 2]
Visited:  [✓, ✓, ✓, ✓, ✓]
```

### Final Result
```
Shortest Distances from A:
A → A: 0
A → B: 1  (path: A-B)
A → C: 3  (path: A-B-C)
A → D: 7  (path: A-E-D)
A → E: 2  (path: A-B-E)
```

---

## Table Format

| Step | Current | Distance | B | C | D | E | Visited |
|------|---------|----------|---|---|---|---|---------|
| 0 | A | 0 | 1 | ∞ | 8 | ∞ | A |
| 1 | B | 1 | - | 3 | 8 | 2 | A,B |
| 2 | E | 2 | - | 3 | 7 | - | A,B,E |
| 3 | C | 3 | - | - | 7 | - | A,B,E,C |
| 4 | D | 7 | - | - | - | - | A,B,E,C,D |

---

## C Code Implementation

```c
#include <stdio.h>
#include <limits.h>

#define V 5  // Number of vertices
#define INF INT_MAX

int minDistance(int distance[], int visited[]) {
    int min = INF;
    int min_index = -1;
    
    for (int i = 0; i < V; i++) {
        if (!visited[i] && distance[i] < min) {
            min = distance[i];
            min_index = i;
        }
    }
    
    return min_index;
}

void dijkstra(int graph[V][V], int source) {
    int distance[V];
    int visited[V] = {0};
    
    // Initialize distances
    for (int i = 0; i < V; i++)
        distance[i] = INF;
    
    distance[source] = 0;
    
    printf("=== DIJKSTRA'S ALGORITHM ===\n");
    printf("Source: %c (index %d)\n\n", 'A' + source, source);
    
    // Main loop
    for (int count = 0; count < V; count++) {
        int u = minDistance(distance, visited);
        
        if (u == -1)
            break;  // Disconnected
        
        visited[u] = 1;
        
        printf("Step %d: Visit %c (distance=%d)\n", count + 1, 'A' + u, distance[u]);
        printf("  Updates:\n");
        
        // Update distances
        for (int v = 0; v < V; v++) {
            if (!visited[v] && graph[u][v] && 
                distance[u] != INF &&
                distance[u] + graph[u][v] < distance[v]) {
                
                distance[v] = distance[u] + graph[u][v];
                printf("    %c: %d\n", 'A' + v, distance[v]);
            }
        }
        printf("\n");
    }
    
    // Print result
    printf("=== SHORTEST DISTANCES ===\n");
    for (int i = 0; i < V; i++) {
        printf("%c: %d\n", 'A' + i, 
               distance[i] == INF ? -1 : distance[i]);
    }
}

int main() {
    // Graph representation (adjacency matrix)
    // 0-A, 1-B, 2-C, 3-D, 4-E
    int graph[V][V] = {
        {0, 1, 0, 8, 0},  // A
        {1, 0, 2, 0, 1},  // B
        {0, 2, 0, 0, 1},  // C
        {8, 0, 0, 0, 5},  // D
        {0, 1, 1, 5, 0}   // E
    };
    
    dijkstra(graph, 0);  // Source = A
    
    return 0;
}
```

---

## Complexity Analysis

### Time Complexity
- **Unoptimized**: O(V²) with adjacency matrix
- **With MinHeap**: O((V+E)logV)
- **With Fibonacci Heap**: O(E + VlogV)

**Where**:
- V = number of vertices
- E = number of edges

### Space Complexity
- **O(V)** for distance and visited arrays

### Trade-offs
- Matrix: Simple, fast for dense graphs
- Heap: Better for sparse graphs
- Fibonacci: Theoretical best, complex

---

## Key Points for Exam

✅ **Must Include**:
1. Initialize distances (0 for source, ∞ others)
2. Repeatedly select min distance vertex
3. Update neighbor distances
4. Show step-by-step updates
5. Final shortest distances

❌ **Common Mistakes**:
- Not initializing distances properly
- Forgetting to mark as visited
- Wrong update formula
- Incomplete trace table

⚡ **Important Facts**:
- Only works with **non-negative weights**
- Greedy choice is always optimal here
- Proved by proof of correctness theorem

---

## Why Not Negative Weights?

**Can fail with negative edges**:
```
A --1--> B --(-5)--> C

Dijkstra:
  A-B: 1, then A-C: ∞
  Correct: A-B-C = -4

Bellman-Ford handles negatives!
```

---

## Shortest Path Reconstruction

```c
void printPath(int previous[], int source, int dest) {
    if (previous[dest] == -1) {
        printf("%c ", 'A' + dest);
        return;
    }
    
    printPath(previous, source, previous[dest]);
    printf("%c ", 'A' + dest);
}
```

---

## Variations

### All Pairs Shortest Path
- **Floyd-Warshall**: O(V³)
- Run Dijkstra V times (one for each source)

### Bellman-Ford Algorithm
- Handles negative weights
- O(VE) time
- Slower but more general

---

## Related Problems

- [Kruskal's Algorithm (MST)](./kruskal_algorithm.md)
- [Prim's Algorithm (MST)](./prim_algorithm.md)
- [BFS for Shortest Path (unweighted)](./bfs_dfs.md)
