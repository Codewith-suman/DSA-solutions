# COMPLETE SOLUTION INDEX & QUICK REFERENCE

---

## 🎯 **TIER 1: MUST PREPARE (Asked 4-5 Times)**

### **Quick Sort** ⭐⭐⭐⭐
- **File**: [Unit-6-Sorting/quick_sort.md](Unit-6-Sorting/quick_sort.md)
- **Marks**: 10 marks
- **Question**: "Explain Quick Sort using Divide & Conquer. Trace with example"
- **Key Concept**: O(nlogn) average, O(n²) worst case
- **Exam Focus**: Complete trace, explain partitioning, discuss complexity
- **Did You Know**: Quick Sort usually faster than Merge Sort in practice despite worse worst-case

### **Infix to Postfix Conversion** ⭐⭐⭐
- **File**: [Unit-2-Stack/infix_to_postfix_conversion.md](Unit-2-Stack/infix_to_postfix_conversion.md)
- **Marks**: 10 marks
- **Question**: "Explain conversion algorithm and trace with example (e.g., 2+3*4)"
- **Key Concept**: Operator precedence, stack-based, no parentheses in output
- **Exam Focus**: Step-by-step conversion, trace table, show stack changes
- **Common Error**: Forgetting to pop remaining operators at end

### **Collision Resolution (Hashing)** ⭐⭐⭐
- **File**: [Unit-7-Searching-Hashing/collision_resolution.md](Unit-7-Searching-Hashing/collision_resolution.md)
- **Marks**: 5 marks
- **Question**: "What is collision? Explain resolution techniques"
- **Key Techniques**: Linear Probing, Quadratic Probing, Double Hashing, Chaining
- **Exam Focus**: Definition + 2-3 techniques with examples
- **Best Answer**: Mention Chaining advantages for high load factor

### **Tower of Hanoi** ⭐⭐⭐
- **File**: [Unit-4-Recursion/tower_of_hanoi.md](Unit-4-Recursion/tower_of_hanoi.md)
- **Marks**: 5 marks
- **Question**: "Write recursive algorithm for TOH. Trace for 3 disks"
- **Key Formula**: 2^n - 1 moves (for n disks)
- **Exam Focus**: Base case, recursive structure, complete trace (7 moves for 3 disks)
- **Proof**: Each step moves progressively larger disks

### **Kruskal's MST** ⭐⭐⭐
- **File**: [Unit-8-Trees-Graphs/kruskal_algorithm.md](Unit-8-Trees-Graphs/kruskal_algorithm.md)
- **Marks**: 5-10 marks
- **Question**: "Find MST using Kruskal's algorithm"
- **Key Concept**: Sort edges, use Union-Find, avoid cycles
- **Exam Focus**: Sorted edges table, component merging, total weight
- **Edge Case**: Handle equal weight edges

### **Dijkstra's Algorithm** ⭐⭐⭐⭐
- **File**: [Unit-8-Trees-Graphs/dijkstra_algorithm.md](Unit-8-Trees-Graphs/dijkstra_algorithm.md)
- **Marks**: 5-10 marks
- **Question**: "Find shortest path from source using Dijkstra's"
- **Key Concept**: Greedy, non-negative weights only, distance updates
- **Exam Focus**: Distance array updates, visited mechanism, trace table
- **Important**: Why it fails with negative weights

---

## 🎯 **TIER 2: IMPORTANT (Asked 3-4 Times)**

### **Merge Sort** ⭐⭐⭐
- **File**: [Unit-6-Sorting/merge_sort.md](Unit-6-Sorting/merge_sort.md)
- **Marks**: 10 marks
- **Guaranteed**: O(nlogn) - always! (unlike QuickSort)
- **Key Advantage**: Stable sorting
- **Exam Focus**: Divide step, merge algorithm, show recursion tree
- **Note**: Extra O(n) space needed

### **Heap Sort** ⭐⭐⭐⭐
- **File**: [Unit-6-Sorting/heap_sort.md](Unit-6-Sorting/heap_sort.md)
- **Marks**: 10 marks
- **Key Steps**: Build heap, repeatedly extract max
- **Exam Focus**: Max heap structure, array heapification, complete sort trace
- **Parent Formula**: Parent of index i = (i-1)/2

### **Binary Search Tree** ⭐⭐⭐⭐
- **File**: [Unit-8-Trees-Graphs/binary_search_tree.md](Unit-8-Trees-Graphs/binary_search_tree.md)
- **Marks**: 10 marks (Insert/Delete)
- **Three Cases**: Deletion of leaf, one child, two children
- **Exam Focus**: Clear algorithms, handle all deletion cases
- **Inorder Property**: Gives sorted sequence

### **Prim's Algorithm** ⭐⭐⭐
- **File**: [Unit-8-Trees-Graphs/prim_algorithm.md](Unit-8-Trees-Graphs/prim_algorithm.md)
- **Marks**: 5-10 marks
- **Key Difference from Kruskal**: Vertex-based, start from one vertex
- **Exam Focus**: Key array updates, component growth, comparison with Kruskal
- **When to Use**: Denser graphs

### **BFS/DFS Traversals** ⭐⭐⭐
- **File**: [Unit-8-Trees-Graphs/bfs_dfs.md](Unit-8-Trees-Graphs/bfs_dfs.md)
- **Marks**: 5 marks
- **Key Difference**: BFS = shortest path (unweighted), DFS = deep exploration
- **Exam Focus**: Both algorithms, trace example showing order of visit
- **Data Structures**: BFS=Queue, DFS=Stack

---

## 🎯 **TIER 3: SUPPORTING (Asked Once or Specific Cases)**

### **Postfix Evaluation** ⭐⭐
- **File**: [Unit-2-Stack/postfix_evaluation.md](Unit-2-Stack/postfix_evaluation.md)
- **Marks**: 5 marks
- **Connection**: Follows Infix→Postfix conversion
- **Stack Pattern**: Push operands, pop for operators

### **Binary Search** ⭐⭐
- **File**: [Unit-7-Searching-Hashing/binary_search.md](Unit-7-Searching-Hashing/binary_search.md)
- **Marks**: 5 marks
- **Competition**: O(logn) vs Linear's O(n)
- **Prerequisite**: Array must be sorted
- **For Million Items**: Binary=20 steps vs Linear=500,000 steps

### **Fibonacci (Recursion)** ⭐⭐
- **File**: [Unit-4-Recursion/fibonacci.md](Unit-4-Recursion/fibonacci.md)
- **Marks**: 5 marks
- **Naive Complexity**: O(2^n) - Exponential!
- **Solution**: Use memoization → O(n)
- **Exam Note**: Mention optimization for better marks

### **Factorial (Recursion)** ⭐⭐
- **File**: [Unit-4-Recursion/factorial.md](Unit-4-Recursion/factorial.md)
- **Marks**: 5 marks
- **Time**: O(n) - Linear recursion
- **Space**: O(n) - Call stack depth
- **Overflow**: Use long long for n>13

---

## 📊 **COMPLEXITY CHEAT SHEET**

```
SORTING ALGORITHMS:
┌─────────────────────────────────────────┐
│ Algorithm   │ Best   │ Avg    │ Worst   │
├─────────────────────────────────────────┤
│ QuickSort   │ nlogn  │ nlogn  │ n²  ❌  │
│ MergeSort   │ nlogn  │ nlogn  │ nlogn ✓ │
│ HeapSort    │ nlogn  │ nlogn  │ nlogn ✓ │
│ BubbleSort  │ n²     │ n²     │ n²      │
│ InsertSort  │ n      │ n²     │ n²      │
└─────────────────────────────────────────┘

SEARCHING:
- Binary Search: O(logn) - Must be sorted!
- Linear Search: O(n) - Works on unsorted

GRAPH ALGORITHMS:
- BFS/DFS: O(V+E) - Both same time
- Dijkstra: O((V+E)logV) with heap
- Kruskal: O(E logE) - Sorting dominates
- Prim: O(V²) naive, O(E+VlogV) with heap

RECURSION:
- Fibonacci Naive: O(2^n) ❌ Too slow!
- Fibonacci Memo: O(n) ✓
- Factorial: O(n)
- Tower of Hanoi: O(2^n)
```

---

## 🎓 **EXAM STRATEGY**

### **Time Management (75 marks total)**
```
Part 1: Long Questions (30 marks)
- 3 questions × 10 marks each
- 90 mins total (~30 min per question)
- MUST attempt: Quick Sort, Dijkstra, Kruskal/Prim, BST

Part 2: Short Questions (45 marks)
- 9 questions × 5 marks each  
- 45 mins total (~5 min per question)
- MUST prepare: Infix→Postfix, TOH, Collision, BFS/DFS, Heap, etc.
```

### **High-Impact Topics (Most Likely to Appear)**

```
Top 3 Questions (80% confident):
1. Quick Sort - Always appears
2. Infix to Postfix - Always appears
3. MST (Kruskal/Prim) - Always appears

Probable (70%):
4. Dijkstra's Algorithm
5. Binary Search Tree Insert/Delete
6. Collision Resolution

Possible (50%):
7. Merge Sort or Heap Sort
8. Tower of Hanoi
9. BFS/DFS Traversals
10. Binary Search
```

---

## ✏️ **EXAM ANSWER TEMPLATE**

### For Algorithm Questions:
```
1. Clear definition (what/why)
2. Algorithm in pseudocode
3. Main steps explanation
4. Trace with given example
5. Complexity analysis (time & space)
6. Advantages/disadvantages (if 10 marks)
```

### For Trace Questions:
```
1. Write step-by-step
2. Show intermediate states
3. Use table format if possible
4. Show final result clearly
5. Verify with expected output
```

---

## 🔥 **FINAL CHECKLIST (Before Exam)**

- [ ] Quick Sort - Algorithm, trace, Partition() function
- [ ] Infix→Postfix - Precedence rules, trace, stack operations
- [ ] Collision Resolution - 3 techniques with examples
- [ ] Kruskal's MST - Sort, Union-Find, cycle detection
- [ ] Dijkstra - Distance array, visited set, relaxation
- [ ] Merge Sort - Merge algorithm, guaranteed O(nlogn)
- [ ] Heap Sort - Max heap, heapify, in-place sorting
- [ ] BST - All 3 deletion cases, inorder traversal
- [ ] Tower of Hanoi - Formula 2^n-1, recursive structure
- [ ] Postfix Evaluation - Stack-based, operator precedence
- [ ] Binary Search - Mid calculation, Why O(logn)
- [ ] BFS/DFS - Queue vs Stack, when to use each
- [ ] Prim vs Kruskal - Key differences
- [ ] Fibonacci - Naive O(2^n) vs Memo O(n)

---

## 📚 **QUICK LINKS TO ALL FILES**

### Unit 2 - Stack
- [Infix to Postfix](Unit-2-Stack/infix_to_postfix_conversion.md)
- [Postfix Evaluation](Unit-2-Stack/postfix_evaluation.md)

### Unit 4 - Recursion
- [Tower of Hanoi](Unit-4-Recursion/tower_of_hanoi.md)
- [Fibonacci](Unit-4-Recursion/fibonacci.md)
- [Factorial](Unit-4-Recursion/factorial.md)

### Unit 6 - Sorting
- [Quick Sort](Unit-6-Sorting/quick_sort.md)
- [Merge Sort](Unit-6-Sorting/merge_sort.md)
- [Heap Sort](Unit-6-Sorting/heap_sort.md)

### Unit 7 - Searching & Hashing
- [Binary Search](Unit-7-Searching-Hashing/binary_search.md)
- [Collision Resolution](Unit-7-Searching-Hashing/collision_resolution.md)

### Unit 8 - Trees & Graphs
- [Binary Search Tree](Unit-8-Trees-Graphs/binary_search_tree.md)
- [Dijkstra's Algorithm](Unit-8-Trees-Graphs/dijkstra_algorithm.md)
- [Kruskal's Algorithm](Unit-8-Trees-Graphs/kruskal_algorithm.md)
- [Prim's Algorithm](Unit-8-Trees-Graphs/prim_algorithm.md)
- [BFS/DFS](Unit-8-Trees-Graphs/bfs_dfs.md)

---

## 🏆 **Success Tips**

1. **Practice Tracing**: Most important for algorithms
2. **Know Complexities**: Can get easy marks explaining why
3. **C Code**: Having code ready boosts confidence
4. **Practice Questions**: Do old exams 2074-2081
5. **Time Management**: 10-mark questions need full traces
6. **Show Intermediate Steps**: Partial marks for correct approach

---

**Last Updated**: 2026  
**Total Solutions**: 15 comprehensive topics  
**Estimated Study Time**: 20-30 hours for mastery
