# Binary Search Algorithm

**Marks**: 5 marks  
**Difficulty**: ⭐⭐ Easy

---

## Problem Statement

Write an algorithm for **Binary Search**. Compare its efficiency with **Sequential (Linear) Search**.

---

## Concept Overview

### Prerequisites
- Array **must be sorted**
- Works on arrays/lists with random access

### Approach
- Divide and conquer
- Compare with middle element
- Eliminate half of remaining elements each time

### When to Use
- **Large sorted arrays**
- **When speed matters** (O(logn) vs O(n))

---

## Algorithm

### Pseudocode

```
Algorithm BINARY_SEARCH(array[], target, left, right)
    
    while left <= right:
        mid = (left + right) / 2
        
        if array[mid] == target:
            return mid  // Found!
        
        else if array[mid] < target:
            left = mid + 1  // Search right half
        
        else:
            right = mid - 1  // Search left half
    
    return -1  // Not found
```

---

## C Code

```c
#include <stdio.h>

int binarySearch(int arr[], int n, int target) {
    int left = 0, right = n - 1;
    int steps = 0;
    
    while (left <= right) {
        steps++;
        int mid = left + (right - left) / 2;
        
        printf("Step %d: mid=%d, arr[%d]=%d vs target=%d\n",
               steps, mid, mid, arr[mid], target);
        
        if (arr[mid] == target) {
            printf("Found at index %d in %d steps!\n", mid, steps);
            return mid;
        }
        else if (arr[mid] < target) {
            left = mid + 1;
            printf("  → Search right half [%d,%d]\n", left, right);
        }
        else {
            right = mid - 1;
            printf("  → Search left half [%d,%d]\n", left, right);
        }
    }
    
    printf("Not found in %d steps!\n", steps);
    return -1;
}

int main() {
    printf("=== BINARY SEARCH ===\n\n");
    
    int arr[] = {1, 3, 5, 7, 9, 11, 13, 15, 17, 19};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    printf("Array: ");
    for (int i = 0; i < n; i++) printf("%d ", arr[i]);
    printf("\n\nSearching for 13:\n\n");
    
    binarySearch(arr, n, 13);
    
    return 0;
}
```

---

## Trace Example: Search for 13 in `[1,3,5,7,9,11,13,15,17,19]`

| Step | Left | Right | Mid | arr[mid] | Action |
|------|------|-------|-----|----------|--------|
| 1 | 0 | 9 | 4 | 9 | 9 < 13 → left=5 |
| 2 | 5 | 9 | 7 | 15 | 15 > 13 → right=6 |
| 3 | 5 | 6 | 5 | 11 | 11 < 13 → left=6 |
| 4 | 6 | 6 | 6 | 13 | **Found!** ✓ |

---

## Comparison Table

| Aspect | Linear Search | Binary Search |
|--------|---|---|
| Requirement | Unsorted OK | **Must sort** |
| Best Case | O(1) | O(1) |
| Average | O(n/2) | **O(log n)** |
| Worst | O(n) | **O(log n)** |
| Space | O(1) | O(1) or O(log n) |
| Implementation | Simple | More complex |

---

## Growth Comparison

```
n=100:
  Linear: 100 comparisons (avg)
  Binary: 7 comparisons (max)

n=1,000,000:
  Linear: 500,000 comparisons
  Binary: 20 comparisons (!!)
```

---

## Key Points

✅ **Remember**:
1. Array must be **sorted**
2. Calculate mid carefully: `mid = left + (right-left)/2`
3. Adjust left/right based on comparison
4. Stop when found or left > right

❌ **Mistakes**:
- Searching unsorted array
- Wrong mid calculation
- Off-by-one errors

**Time**: O(log n) vs O(n) = **Huge difference!**
