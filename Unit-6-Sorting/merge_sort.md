# Merge Sort Algorithm

**Questions Asked**: 4 times (2074, 2075, 2078, and variations)  
**Marks**: 10 marks  
**Difficulty**: ⭐⭐⭐ Medium

---

## Problem Statement

Explain the **Merge Sort algorithm** using **Divide and Conquer**. Trace it with an example.

**Example**: Sort `[38, 27, 43, 3, 9, 82, 10]`

---

## Concept Overview

### Divide & Conquer
1. **Divide**: Split array into two halves
2. **Conquer**: Recursively sort both halves
3. **Combine**: Merge two sorted subarrays

### Key Advantages
- **Always O(nlogn)** - guaranteed, unlike QuickSort
- **Stable** - maintains order of equal elements
- **Predictable** - no worst case surprises

### When to Use
- Need guaranteed O(nlogn)
- Stability important (database records)
- Linked lists (better than quick sort)
- External sorting (disk-based)

---

## Algorithm

### Pseudocode

```
Algorithm MERGESORT(array[], left, right)
    
    if left < right:
        // Find middle
        mid = (left + right) / 2
        
        // Sort left half
        MERGESORT(array, left, mid)
        
        // Sort right half
        MERGESORT(array, mid + 1, right)
        
        // Merge sorted halves
        MERGE(array, left, mid, right)

Algorithm MERGE(array[], left, mid, right)
    
    // Create temp arrays
    leftArray = array[left...mid]
    rightArray = array[mid+1...right]
    
    i = 0, j = 0, k = left
    
    // Merge back
    while i < leftArray.length AND j < rightArray.length:
        if leftArray[i] <= rightArray[j]:
            array[k] = leftArray[i]
            i++
        else:
            array[k] = rightArray[j]
            j++
        k++
    
    // Copy remaining
    while i < leftArray.length:
        array[k] = leftArray[i]
        i++, k++
    
    while j < rightArray.length:
        array[k] = rightArray[j]
        j++, k++
```

---

## C Code Implementation

```c
#include <stdio.h>
#include <string.h>

void merge(int arr[], int left, int mid, int right) {
    int leftSize = mid - left + 1;
    int rightSize = right - mid;
    
    // Create temp arrays
    int leftArr[leftSize];
    int rightArr[rightSize];
    
    // Copy data
    for (int i = 0; i < leftSize; i++)
        leftArr[i] = arr[left + i];
    
    for (int j = 0; j < rightSize; j++)
        rightArr[j] = arr[mid + 1 + j];
    
    // Merge
    int i = 0, j = 0, k = left;
    
    printf("   Merging [%d...%d] and [%d...%d]: ", 
           left, mid, mid + 1, right);
    
    while (i < leftSize && j < rightSize) {
        if (leftArr[i] <= rightArr[j]) {
            arr[k] = leftArr[i];
            i++;
        } else {
            arr[k] = rightArr[j];
            j++;
        }
        k++;
    }
    
    // Copy remaining
    while (i < leftSize) {
        arr[k] = leftArr[i];
        i++;
        k++;
    }
    
    while (j < rightSize) {
        arr[k] = rightArr[j];
        j++;
        k++;
    }
    
    // Print result
    for (int x = left; x <= right; x++)
        printf("%d ", arr[x]);
    printf("\n");
}

void mergeSort(int arr[], int left, int right, int depth) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        printf("Depth %d: Dividing [%d...%d]\n", depth, left, right);
        
        // Sort first half
        mergeSort(arr, left, mid, depth + 1);
        
        // Sort second half
        mergeSort(arr, mid + 1, right, depth + 1);
        
        // Merge
        merge(arr, left, mid, right);
    }
}

void printArray(int arr[], int n) {
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");
}

int main() {
    printf("=== MERGE SORT ===\n\n");
    
    int arr[] = {38, 27, 43, 3, 9, 82, 10};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    printf("Original: ");
    printArray(arr, n);
    printf("\n--- Sorting Process ---\n");
    
    mergeSort(arr, 0, n - 1, 0);
    
    printf("\nFinal Sorted: ");
    printArray(arr, n);
    
    return 0;
}
```

---

## Trace Example: `[38, 27, 43, 3]`

### Divide Phase

```
[38, 27, 43, 3]
      |
      v
[38, 27]    [43, 3]
   |          |
   v          v
[38] [27]  [43] [3]
```

### Merge Phase

```
[38] [27]       →  [27, 38]
[43] [3]        →  [3, 43]

[27, 38]  [3, 43]  →  [3, 27, 38, 43]
```

### Complete Trace Table

| Step | Operation | Array |
|------|-----------|-------|
| 1 | Divide [38,27,43,3] | Split |
| 2 | Divide [38,27] | Split |
| 3 | Divide [38] | Base case |
| 4 | Divide [27] | Base case |
| 5 | Merge [38],[27] | [27,38] |
| 6 | Divide [43,3] | Split |
| 7 | Base cases [43],[3] | Individual |
| 8 | Merge [43],[3] | [3,43] |
| 9 | Merge [27,38],[3,43] | **[3,27,38,43]** ✓ |

### Visual Tree

```
                    [38,27,43,3]
                    /          \
              [38,27]          [43,3]
              /     \          /    \
           [38]    [27]    [43]    [3]
              \     /          \    /
               [27,38]         [3,43]
                \              /
                [3,27,38,43]
```

---

## Full 7-Element Example

```
[38, 27, 43, 3, 9, 82, 10]

Divide:
[38, 27, 43, 3] [9, 82, 10]
[38, 27] [43, 3] [9, 82] [10]
[38] [27] [43] [3] [9] [82] [10]

Merge (Bottom-up):
[27, 38] [3, 43] [9, 82] [10]
[3, 27, 38, 43] [9, 10, 82]
[3, 9, 10, 27, 38, 43, 82]
```

---

## Complexity Analysis

### Time Complexity

**Recurrence**: T(n) = 2T(n/2) + n

**Solving**:
- Divide: O(1)
- Merge: O(n)
- Each level: O(n)
- Number of levels: log(n)
- **Total: O(n log n)** - Always!

**Examples**:
- n=1: 0 comparisons (base)
- n=2: 1 merge
- n=4: 4+2=6 compares
- n=8: 8+4+2=14 compares
- n=1,000,000: ~20,000,000 ops

### Space Complexity
- **O(n)** - Temporary arrays in merge
- **O(log n)** - Recursion stack
- **Total: O(n)**

### Comparison with QuickSort

| Aspect | QuickSort | MergeSort |
|--------|-----------|-----------|
| Best | O(nlogn) | O(nlogn) |
| Average | O(nlogn) | O(nlogn) |
| **Worst** | **O(n²)** | **O(nlogn)** |
| Space | O(logn) | O(n) |
| Stable | No | **Yes** |
| Cache | Good | Poor |

---

## Merge Step Detailed

```
Left:  [27, 38, 3, 43]
Right exists? Compare:

Compare 27 vs 3:
  3 < 27 → Take 3, advance right pointer
  Result: [3]

Compare 27 vs 43:
  27 < 43 → Take 27, advance left pointer
  Result: [3, 27]

Compare 38 vs 43:
  38 < 43 → Take 38, advance left pointer
  Result: [3, 27, 38]

Left empty, copy remaining:
  Result: [3, 27, 38, 43]
```

---

## Key Points for Exam

✅ **Must Include**:
1. Divide step (recursively find middle)
2. Conquer (recursive calls)
3. Merge logic (compare and combine)
4. Trace with complete steps
5. **O(nlogn) ALWAYS** - key difference

❌ **Common Mistakes**:
- Forgetting merge is crucial
- Saying "O(n²) worst case" (Wrong!)
- Incomplete trace
- Wrong merge algorithm

⚡ **Key Facts**:
- **Guaranteed O(nlogn)** - exam favorite
- **Stable sort** - equal elements maintain order
- **Best for linked lists** - no random access needed

---

## When Merge Matters in Exam

```
Question: "What's advantage over QuickSort?"
Answer: "Guaranteed O(nlogn), QuickSort can be O(n²)"

Question: "Trace with example"
Answer: Show ALL division and merge steps

Question: "Why stable?"
Answer: "Use <= in merge, preserves order of equals"
```

---

## Sorting Stability Example

```
Input: [(3,a), (1,b), (3,c), (2,d)]

MergeSort (Stable): [(1,b), (2,d), (3,a), (3,c)] ✓
QuickSort (Unstable): [(1,b), (2,d), (3,c), (3,a)] ✗
```

---

## Optimizations

### 1. Insertion Sort for Small Arrays
```c
// Use insertion sort for n < 10
if (right - left < 10)
    insertionSort(arr, left, right);
```

### 2. Check Already Sorted
```c
// Skip merge if already sorted
if (arr[mid] <= arr[mid+1])
    return;
```

### 3. Iterative Merge Sort
```
// Bottom-up: No recursion overhead
// Better cache performance
```

---

## Related Problems

- [QuickSort](./quick_sort.md)
- [HeapSort](./heap_sort.md)
- [Inversion Count Problem](./README.md)
