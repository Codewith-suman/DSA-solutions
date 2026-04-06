# 📋 QUICK REFERENCE CHEAT SHEET

---

## SORTING AT A GLANCE

### Quick Sort
```
Partition around pivot → Recursively sort left & right
Best: O(n log n) | Avg: O(n log n) | Worst: O(n²)
In-place, Unstable, Fast in practice
```

### Merge Sort
```
Divide in half → Recursively sort → Merge
Best: O(n log n) | Avg: O(n log n) | Worst: O(n log n) ✓ ALWAYS
Extra O(n) space, Stable, Guaranteed
```

### Heap Sort
```
Build max heap → Extract max repeatedly
Best: O(n log n) | Avg: O(n log n) | Worst: O(n log n)
O(1) space, Unstable, Less efficient than Quick
```

---

## SEARCHING AT A GLANCE

### Binary Search
```
Sorted array required
Compare with mid: if target < arr[mid] go left, else go right
O(log n) time | O(1) space
1 million items → ~20 steps (vs 500k for linear)
```

### Linear Search
```
Works on unsorted array
O(n) time | O(1) space
Simple but slow for large data
```

---

## STACK OPERATIONS

### Infix to Postfix
```
1. Operands → directly to output
2. '(' → push to stack
3. ')' → pop until '('
4. Operator → pop higher/equal precedence first
5. End → pop all remaining
```

### Postfix Evaluation
```
1. Operands → PUSH
2. Operators → POP 2, compute, PUSH result
3. Final answer at top of stack
Order matters: if pop a then b, compute as a OP b
```

---

## RECURSION PATTERNS

### Tower of Hanoi
```
TOH(n, source, dest, aux):
  TOH(n-1, source, aux, dest)   // Move n-1 to aux
  Move(source, dest)             // Move largest
  TOH(n-1, aux, dest, source)   // Move n-1 to dest

Total moves = 2^n - 1
Space = O(n) call stack
```

### Fibonacci
```
F(n) = F(n-1) + F(n-2)
Base: F(0)=0, F(1)=1

Naive: O(2^n) ❌
With memo: O(n) ✓
Iterative: Most efficient
```

### Factorial
```
n! = n × (n-1)!
Base: 0!=1 or 1!=1

Recursive O(n) | Iterative O(n) better
Watch for overflow at n>20
```

---

## COLLISION RESOLUTION

| Method | Formula | Pros | Cons |
|--------|---------|------|------|
| Linear | (h+i)%m | Simple | Clustering |
| Quad | (h+i²)%m | Less clustering | Holes |
| Double | (h+i×h₂)%m | Best distribution | Complex |
| Chaining | Linked list | Simple, good for high load | Extra memory |

---

## GRAPH ALGORITHMS

### Dijkstra (Shortest Path)
```
1. Initialize: distance[source]=0, others=∞
2. While unvisited vertices:
   - Pick min distance unvisited vertex
   - Mark visited
   - Update neighbor distances
3. Works only with non-negative weights
Time: O(V²) naive, O((V+E)log V) with heap
```

### Kruskal (MST)
```
1. Sort edges by weight
2. Use Union-Find for components
3. For each edge:
   - If endpoints in different components, add to MST
   - Merge components
4. Stop at V-1 edges
Time: O(E log E)
```

### Prim (MST)
```
1. Start from any vertex
2. Key array tracks min edge to tree
3. While unvisited vertices:
   - Pick min key unvisited vertex
   - Add to tree
   - Update keys of neighbors
4. Time: O(V²) naive, O(E log V) with heap
```

### BFS (Level Order)
```
Queue-based, finds shortest path (unweighted)
for each neighbor of current: enqueue if unvisited
Time: O(V+E)
```

### DFS (Deep Search)
```
Stack/Recursion-based, deep exploration
Good for cycles, topological sort
for each neighbor of current: recurse if unvisited
Time: O(V+E)
```

---

## TREES

### Binary Search Tree (BST)
```
Properties: Left < Node < Right
           Inorder = Sorted sequence

Insert: Recursive placement (left if <, right if >)

Delete: 
  Leaf → remove
  One child → replace with child
  Two children → replace with inorder successor
  
Time: O(log n) if balanced, O(n) if skewed
```

---

## COMPLEXITY AT A GLANCE

```
O(1) - Hash lookup, array access
O(log n) - Binary search, balanced tree operations
O(n) - Linear search, simple loop
O(n log n) - Merge sort, heap sort, Dijkstra with heap
O(n²) - Quick sort worst, bubble sort, insertion sort
O(2^n) - Fibonacci naive, subsets, permutations
O(n!) - All permutations, traveling salesman brute force
```

---

## EXAM FORMULAS TO REMEMBER

```
Tower of Hanoi moves = 2^n - 1

BST height (balanced) = O(log n)
Binary Search height = O(log n)

Fibonacci sequence = 0,1,1,2,3,5,8,13,21,34...

Load Factor = elements / table size
For hash table = n/m (should stay <0.75)

Array to Heap transformation:
  Parent of i = (i-1)/2
  Left child of i = 2i+1
  Right child of i = 2i+2

Merge time = T(n) = 2T(n/2) + n = O(n log n)
Quick time = T(n) = T(k) + T(n-k-1) + n
  Best: k=n/2 → O(n log n)
  Worst: k=0 or k=n-1 → O(n²)
```

---

## PSEUDO CODE TEMPLATES

### Binary Search Pattern
```
left=0, right=n-1
while left <= right:
  mid = left + (right-left)/2
  if arr[mid] == target: return mid
  if arr[mid] < target: left = mid+1
  else: right = mid-1
return -1
```

### Recursive Pattern
```
function(n):
  if n == base_case:
    return base_value
  return n op function(n-1)
```

### Complete example:
```
factorial(n):
  if n <= 1: return 1
  return n * factorial(n-1)
```

### Merge Pattern
```
while left_pointer < left_size AND right_pointer < right_size:
  if left[lp] <= right[rp]:
    output[op] = left[lp++]
  else:
    output[op] = right[rp++]
  op++
// Copy remaining
while lp < left_size:
  output[op++] = left[lp++]
while rp < right_size:
  output[op++] = right[rp++]
```

---

## WHEN TO USE WHAT

| Problem | Best Algorithm | Why |
|---------|---|---|
| Fastest general sort | QuickSort | O(n log n) avg, cache friendly |
| Need guaranteed speed | MergeSort | O(n log n) worst case |
| Large dataset, stable needed | MergeSort | Stability guaranteed |
| In-place, no extra space | HeapSort | O(1) extra space |
| Shortest path, unweighted | BFS | O(V+E) |
| Shortest path, weighted | Dijkstra | O((V+E)log V) |
| Minimum spanning tree | Kruskal/Prim | Both O(E log E) |
| Find duplicate fast | Hashing | O(1) expected |
| Maintain order, insert anywhere | Linked List | O(1) insert |
| Quick search in sorted | Binary Search | O(log n) |
| Explore all paths | DFS | Depth-first traversal |
| Explore all routes | BFS | Level-by-level |

---

## QUICK PASS STRATEGY

### Easy 30 marks (Short Questions)
- Tower of Hanoi ✓
- Postfix Evaluation ✓
- Fibonacci/Factorial ✓
- BFS/DFS basic example ✓
- Binary Search ✓
- Collision Resolution techniques ✓

### Medium 25 marks (10-mark concepts)
- Infix to Postfix (with trace) ✓
- BST Insert/Delete (one or two cases) ✓
- One MST (Kruskal or Prim) ✓
- Dijkstra (partial trace) ✓

### Hard 20 marks (Full 10-mark trace)
- Quick Sort (complete trace) ✓
- Merge Sort (complete divide-merge) ✓
- Heap Sort (build + extract) ✓
- BST Delete (complex case) ✓
- Dijkstra (full table) ✓

---

## LAST-MINUTE FOCUS (2 hours before exam)

### Ultra Important (MUST know)
- [ ] Quick Sort algorithm
- [ ] Infix to Postfix conversion
- [ ] Tower of Hanoi formula
- [ ] Collision Resolution
- [ ] BST Insert
- [ ] Dijkstra/Kruskal algorithm

### Second Priority (Should know)
- [ ] Merge Sort
- [ ] Binary Search
- [ ] Postfix Evaluation
- [ ] BFS/DFS

### Nice to Have (If time)
- [ ] Heap Sort
- [ ] Fibonacci optimization
- [ ] Prim's Algorithm

---

**Remember**: Partial marks for correct approach even if trace is incomplete!
