# Heap Sort Algorithm

**Questions Asked**: 3 times (2075, 2080, and variations)  
**Marks**: 10 marks  
**Difficulty**: ⭐⭐⭐⭐ High

---

## Problem Statement

Explain **Heap Sort** algorithm. Build a **max heap** for given data and sort it.

**Example**: Sort `[38, 27, 43, 3, 9, 82, 10]`

---

## Concept Overview

### What's a Heap?
- **Complete binary tree** with special property
- **Max Heap**: Parent ≥ Children
- **Min Heap**: Parent ≤ Children

### Heap Sort Steps
1. Build max heap from array
2. Swap root (max) with last element
3. Reduce heap size and reheapify
4. Repeat until sorted

---

## Heap Array Representation

```
Tree:           Array:
    50         [50, 30, 40, 10, 20, 35, 8]
   /  \        Index: 0  1  2   3   4   5  6
  30  40
 /  \ /
10 20 35
 /
8

For index i:
  Left child: 2i + 1
  Right child: 2i + 2
  Parent: (i-1)/2
```

---

## Algorithm

### Step 1: Build Max Heap

```
For each non-leaf node (bottom to top):
  Heapify down: Move larger child up, parent down
```

### Step 2: Sort

```
For i = n-1 down to 1:
  Swap arr[0] with arr[i]
  Heapify(0, i)  // Reduce heap and reheapify
```

---

## C Code

```c
#include <stdio.h>

void heapify(int arr[], int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    // Find largest among parent, left, right
    if (left < n && arr[left] > arr[largest])
        largest = left;
    
    if (right < n && arr[right] > arr[largest])
        largest = right;
    
    // If largest not parent, swap and heapify
    if (largest != i) {
        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;
        
        heapify(arr, n, largest);
    }
}

void buildHeap(int arr[], int n) {
    printf("Building heap:\n");
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }
    printf("Heap built!\n");
}

void heapSort(int arr[], int n) {
    printf("=== HEAP SORT ===\n\n");
    printf("Original: ");
    for (int i = 0; i < n; i++) printf("%d ", arr[i]);
    printf("\n\n");
    
    // Build max heap
    buildHeap(arr, n);
    
    printf("Heap array: ");
    for (int i = 0; i < n; i++) printf("%d ", arr[i]);
    printf("\n\n");
    
    // Sort
    printf("Sorting:\n");
    for (int i = n - 1; i > 0; i--) {
        // Swap root with last
        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;
        
        printf("Swap %d with %d at heap[1...%d]\n", 
               arr[i], arr[0], i);
        
        // Heapify reduced heap
        heapify(arr, i, 0);
    }
    
    printf("\nSorted: ");
    for (int i = 0; i < n; i++) printf("%d ", arr[i]);
    printf("\n");
}

int main() {
    int arr[] = {38, 27, 43, 3, 9, 82, 10};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    heapSort(arr, n);
    
    return 0;
}
```

---

## Trace Example: `[38, 27, 43, 3]`

### Building Heap

```
Initial: [38, 27, 43, 3]

Tree:       38
           /  \
         27   43
        /
       3

Heapify:
- Index 0 (38): has larger child 43 → swap
- Result: [43, 27, 38, 3]

Final Heap: 43 is root (max)
           43
          /  \
        27   38
       /
      3
```

### Sorting

```
Step 1: Swap 43 (root) with 3 (last)
  [3, 27, 38, 43]
  Heapify [3, 27, 38] with heap size 3
  Result: [38, 27, 3, 43]

Step 2: Swap 38 with 3
  [3, 27, 38, 43]
  Heapify [3, 27] with heap size 2
  Result: [27, 3, 38, 43]

Step 3: Swap 27 with 3
  [3, 27, 38, 43]
  Already sorted sub-array size 1

Final: [3, 27, 38, 43] ✓
```

---

## Complexity Analysis

### Time Complexity
- **Build heap**: O(n)
- **Sorting phase**: O(n log n)
  - n swaps
  - Each heapify: O(log n)
- **Total: O(n log n)** - Always!

### Space Complexity
- **O(1)** - In-place sorting (or O(log n) for recursion)

### Comparison

| Aspect | HeapSort | QuickSort | MergeSort |
|--------|----------|-----------|-----------|
| Best | nlogn | nlogn | nlogn |
| Avg | nlogn | nlogn | nlogn |
| Worst | **nlogn** | n² | nlogn |
| Space | **O(1)** | logn | n |
| Stable | No | No | Yes |
| Cache | Poor | Good | Fair |

---

## Max Heap vs Min Heap

### Max Heap (Used for Sort)
```
       50
      /  \
    30   40
   / \
  10 20
```

### Min Heap
```
        5
      /   \
    10    15
   / \   / \
  20 30 25 35
```

---

## Key Points for Exam

✅ **Must Include**:
1. **Max heap concept**: Parent ≥ children
2. **Array indices**: left=2i+1, right=2i+2
3. **Build phase**: O(n) from bottom-up
4. **Sort phase**: Swap root, heapify, repeat
5. **Time**: O(n log n) guaranteed

❌ **Common Mistakes**:
- Wrong parent-child relationship
- Index calculation errors
- Incomplete heapify logic
- Not building heap first

⚡ **Advantages**:
- **Guaranteed O(n log n)**
- In-place (no extra space)
- Good worst-case

⚡ **Disadvantages**:
- Poor cache performance
- Not stable
- More complex than QuickSort

---

## Heapify Visualization

```
Before Heapify:       After Heapify:
      5                      20
     / \                    /   \
   20  10                  5    10
   /\
  8  7

Process:
1. Compare 5, 20, 10
2. Max is 20, swap with 5
3. Check subtree at 5's new position
4. Compare 5, 8, 7
5. Max is 8, swap with 5
6. Done (leaf node)
```

---

## Priority Queue Connection

Heap Sort is essentially:
1. Insert n elements into heap: O(n)
2. Extract max n times: O(n log n)
= O(n log n) total

```c
// Priority queue using heap
#define MAX_SIZE 100
int heap[MAX_SIZE];
int size = 0;

void insert(int val) { /* Add to heap */ }
int extractMax() { /* Remove root */ }
```

---

## Related Problems

- [QuickSort](./quick_sort.md)
- [MergeSort](./merge_sort.md)
- [Priority Queue](./README.md)
- [Kth Largest Element](./README.md)

---

## Practice Questions

1. Build max heap for [3, 7, 9, 1, 5]
2. Trace heap sort for [10, 20, 15]
3. Why is building O(n) and not O(n log n)?
4. Compare with other sorting algorithms
