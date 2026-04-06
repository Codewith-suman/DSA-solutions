# BFS and DFS Graph Traversals

**Marks**: 5 marks  
**Difficulty**: ⭐⭐⭐ Medium

---

## Problem Statement

Explain **Graph Traversals**: **BFS (Breadth-First Search)** and **DFS (Depth-First Search)**.

---

## Concept Overview

### BFS (Breadth-First Search)
- Explore level by level
- Uses **Queue** (FIFO)
- Finds shortest path in unweighted graph

### DFS (Depth-First Search)
- Go as deep as possible
- Uses **Stack** (LIFO) or recursion
- Good for topological sort, cycle detection

---

## BFS Algorithm

```
Algorithm BFS(graph, start)
    
    QUEUE = empty queue
    visited = empty set
    
    QUEUE.enqueue(start)
    visited.add(start)
    
    while QUEUE not empty:
        v = QUEUE.dequeue()
        print v
        
        for each neighbor u of v:
            if u not in visited:
                visited.add(u)
                QUEUE.enqueue(u)
```

---

## DFS Algorithm

### Recursive

```
Algorithm DFS_RECURSIVE(v, visited)
    
    visited.add(v)
    print v
    
    for each neighbor u of v:
        if u not in visited:
            DFS_RECURSIVE(u, visited)
```

### Iterative (Using Stack)

```
Algorithm DFS_ITERATIVE(graph, start)
    
    STACK = empty stack
    visited = empty set
    
    STACK.push(start)
    
    while STACK not empty:
        v = STACK.pop()
        
        if v not in visited:
            visited.add(v)
            print v
            
            for each neighbor u of v:
                if u not in visited:
                    STACK.push(u)
```

---

## C Code: BFS

```c
#include <stdio.h>
#include <stdlib.h>

#define V 6

void bfs(int graph[V][V], int start) {
    int queue[V];
    int visited[V] = {0};
    int front = 0, rear = 0;
    
    printf("=== BFS (Breadth-First Search) ===\n");
    printf("Start: %c\n\n", 'A' + start);
    
    queue[rear++] = start;
    visited[start] = 1;
    
    printf("Order: ");
    
    while (front < rear) {
        int v = queue[front++];
        printf("%c ", 'A' + v);
        
        for (int u = 0; u < V; u++) {
            if (graph[v][u] && !visited[u]) {
                visited[u] = 1;
                queue[rear++] = u;
            }
        }
    }
    printf("\n");
}

int main() {
    int graph[V][V] = {
        {0, 1, 1, 0, 0, 0},  // A
        {1, 0, 0, 1, 1, 0},  // B
        {1, 0, 0, 1, 0, 1},  // C
        {0, 1, 1, 0, 1, 0},  // D
        {0, 1, 0, 1, 0, 1},  // E
        {0, 0, 1, 0, 1, 0}   // F
    };
    
    bfs(graph, 0);  // Start from A
    
    return 0;
}
```

---

## C Code: DFS (Recursive)

```c
#include <stdio.h>

#define V 6

void dfs(int graph[V][V], int v, int visited[V]) {
    visited[v] = 1;
    printf("%c ", 'A' + v);
    
    for (int u = 0; u < V; u++) {
        if (graph[v][u] && !visited[u]) {
            dfs(graph, u, visited);
        }
    }
}

int main() {
    printf("=== DFS (Depth-First Search) ===\n");
    printf("Start: A\n\n");
    printf("Order: ");
    
    int graph[V][V] = {
        {0, 1, 1, 0, 0, 0},  // A
        {1, 0, 0, 1, 1, 0},  // B
        {1, 0, 0, 1, 0, 1},  // C
        {0, 1, 1, 0, 1, 0},  // D
        {0, 1, 0, 1, 0, 1},  // E
        {0, 0, 1, 0, 1, 0}   // F
    };
    
    int visited[V] = {0};
    dfs(graph, 0, visited);
    printf("\n");
    
    return 0;
}
```

---

## Trace Example

### Graph
```
A --- B --- D --- E
|           |     |
C ----------+-----F
```

### Adjacency Matrix
```
  A B C D E F
A[0 1 1 0 0 0]
B[1 0 0 1 1 0]
C[1 0 0 1 0 1]
D[0 1 1 0 1 0]
E[0 1 0 1 0 1]
F[0 0 1 0 1 0]
```

### BFS from A

```
Queue:          Visited:        Print:
[A]             {A}
[B, C]          {A, B, C}       A
[C, D, E]       {A, B, C, D, E} B
[D, E, F]       {A, B, C, D, E} C
[E, F]                          D
[F]                             E
[]                              F

Result: A B C D E F
```

### DFS from A

```
Stack:          Path:
[A]             A
[B, C]          A → B
[C, D, E]       A → B → D
[C, E, F]       A → B → D → C
[C, E, F]       A → B → D → C (no new)
[E, F]          A → B → D → E
[F]             A → B → D → E → F
[]              

Result: A B D C E F
```

---

## Comparison Table

| Aspect | BFS | DFS |
|--------|-----|-----|
| Data Structure | Queue | Stack |
| Pattern | Level-by-level | Go deep |
| Shortest Path | Yes (unweighted) | No |
| Time | O(V+E) | O(V+E) |
| Space | O(V) | O(V) |
| Implementation | Loop | Recursion/Loop |
| Cycle Detection | Yes | **Yes** |
| Topological Sort | No | **Yes** |

---

## Applications

### BFS
- Shortest path (unweighted)
- Level order traversal
- Connected components
- Bipartite checking

### DFS
- Topological sort
- Cycle detection
- Path finding
- Strongly connected components
- Spanning tree

---

## Key Points

✅ **Remember**:
1. **BFS**: Queue, level-by-level
2. **DFS**: Stack, go deep
3. **Both**: O(V+E) time, O(V) space
4. **BFS**: Shortest unweighted path
5. **DFS**: Good for directed graphs

❌ **Mistakes**:
- Forgetting to mark visited
- Confusing queue/stack usage
- Incomplete traversal logic

⚡ **Expert Tips**:
- BFS: Use for shortest path
- DFS: Use for connectivity/cycles
- Both find all reachable nodes

---

## Related Problems

- [Dijkstra's Algorithm](./dijkstra_algorithm.md)
- [Cycle Detection](./README.md)
- [Topological Sort](./README.md)
