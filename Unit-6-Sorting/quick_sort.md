# Quick Sort Algorithm

**Questions Asked**: 4 times (Model, 2076, 2079, 2081)  
**Marks**: 10 marks  
**Difficulty**: ⭐⭐⭐⭐ High

---

## Problem Statement

Explain the **Quick Sort algorithm** using **Divide and Conquer**. Trace it with given data and discuss efficiency.

**Example**: Sort `[64, 34, 25, 12, 22, 11, 90]`

---

## Concept Overview

### Divide and Conquer Approach
1. **Divide**: Partition array around a pivot element
2. **Conquer**: Recursively sort left and right subarrays
3. **Combine**: Already sorted (in-place sorting)

### How It Works
- Choose a **pivot** element
- Rearrange so that:
  - All elements **< pivot** are on left
  - All elements **> pivot** are on right
- Recursively sort left and right parts

### Pivot Selection Strategies
- **First element** (simple, poor for sorted data)
- **Last element**
- **Middle element**
- **Random element**
- **Median-of-three** (better)

---

## Algorithm

### Pseudocode

```
Algorithm QUICKSORT(array[], left, right)
    if left < right:
        // Partition and get pivot index
        pivotIndex = PARTITION(array, left, right)
        
        // Recursively sort left part
        QUICKSORT(array, left, pivotIndex - 1)
        
        // Recursively sort right part
        QUICKSORT(array, pivotIndex + 1, right)

Algorithm PARTITION(array[], left, right)
    // Choose last element as pivot
    pivot = array[right]
    i = left - 1
    
    for j = left to right - 1:
        if array[j] < pivot:
            i = i + 1
            SWAP(array[i], array[j])
    
    SWAP(array[i+1], array[right])
    return i + 1
```

---

## C Code Implementation

```c
#include <stdio.h>

// Function to swap two elements
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// Partition function
int partition(int arr[], int low, int high) {
    // Choose last element as pivot
    int pivot = arr[high];
    
    // Index of smaller element
    int i = low - 1;
    
    printf("   Partitioning [%d...%d] with pivot = %d\n", low, high, pivot);
    
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(&arr[i], &arr[j]);
            printf("   Swap %d and %d\n", arr[j], arr[i]);
        }
    }
    
    swap(&arr[i + 1], &arr[high]);
    printf("   Pivot %d placed at index %d\n", pivot, i + 1);
    
    return i + 1;
}

// QuickSort function
void quickSort(int arr[], int low, int high, int depth) {
    if (low < high) {
        // Partitioning index
        int pi = partition(arr, low, high);
        
        printf("Depth %d: ", depth);
        for (int i = low; i <= high; i++) {
            printf("%d ", arr[i]);
        }
        printf("\n");
        
        // Separately sort elements before and after partition
        quickSort(arr, low, pi - 1, depth + 1);
        quickSort(arr, pi + 1, high, depth + 1);
    }
}

// Utility function to print array
void printArray(int arr[], int n) {
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");
}

int main() {
    printf("=== QUICK SORT ALGORITHM ===\n\n");
    
    int arr[] = {64, 34, 25, 12, 22, 11, 90};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    printf("Original Array: ");
    printArray(arr, n);
    printf("\n--- Sorting Process ---\n");
    
    quickSort(arr, 0, n - 1, 0);
    
    printf("\nSorted Array: ");
    printArray(arr, n);
    
    return 0;
}
```

---

## Trace Example: `[64, 34, 25, 12, 22, 11, 90]`

### Step 1: Initial Partition
```
Array: [64, 34, 25, 12, 22, 11, 90]
Pivot = 90 (last element)

Comparison:
- 64 < 90 → swap with position 0 (no change)
- 34 < 90 → swap with position 1 (no change)
- 25 < 90 → swap with position 2 (no change)
- 12 < 90 → swap with position 3 (no change)
- 22 < 90 → swap with position 4 (no change)
- 11 < 90 → swap with position 5 (no change)

Result: [64, 34, 25, 12, 22, 11, 90]
Pivot 90 at index 6
```

### Step 2: Sort Left Part [64, 34, 25, 12, 22, 11]
```
Pivot = 11

Comparisons:
- 64 > 11 → no action
- 34 > 11 → no action
- 25 > 11 → no action
- 12 > 11 → no action
- 22 > 11 → no action
- 11 = 11 → swap at correct position

Result: [11, 34, 25, 12, 22, 64, 90]
```

### Step 3: Continue Recursively
```
Left part: [34, 25, 12, 22, 64]
Pivot = 64

Result: [34, 25, 12, 22, 64, 90]

Continue until all sorted...

Final: [11, 12, 22, 25, 34, 64, 90]
```

### Visual Recursion Tree

```
         [64, 34, 25, 12, 22, 11, 90]
                    (pivot=90)
                    /            \
        [11, 34, 25, 12, 22]    [90]
              (pivot=11)
              /           \
          [11]      [34, 25, 12, 22]
                          (pivot=22)
                          /         \
                   [12, 22]      [34, 25]
                  (pivot=22)     (pivot=25)
                   /    \         /    \
                [12]  [22]    [25]  [34]
                
Final: [11, 12, 22, 25, 34, 64, 90]
```

---

## Animated Trace Table

| Pass | Array | Pivot | Action |
|------|-------|-------|--------|
| 1 | [64,34,25,12,22,11,90] | 90 | Partition around 90 |
| 2 | [11,34,25,12,22,64,90] | 11 | Left sub-array |
| 3 | [11,34,25,12,22,64,90] | 22 | Right sub-array |
| 4 | [11,12,22,25,34,64,90] | 25 | Recursive splits |
| Final | **[11,12,22,25,34,64,90]** | - | Sorted ✓ |

---

## Complexity Analysis

### Time Complexity

**Best Case**: O(n log n)
- When pivot is median
- Array divided into equal halves

**Average Case**: O(n log n)
- Random pivot selection
- Expected behavior

**Worst Case**: O(n²)
- Pivot always smallest or largest
- When array is already sorted (with first/last pivot)
- Array never properly divided

### Space Complexity
- **O(log n)** - Recursion stack space
- **O(1)** - No extra space for sorting (in-place)

### Comparison

| Algorithm | Best | Avg | Worst | Space | Stable |
|-----------|------|-----|-------|-------|--------|
| QuickSort | nlogn | nlogn | n² | logn | No |
| MergeSort | nlogn | nlogn | nlogn | n | Yes |
| HeapSort | nlogn | nlogn | nlogn | 1 | No |
| BubbleSort | n² | n² | n² | 1 | Yes |

---

## Key Points for Exam

✅ **Must Answer**:
1. **Divide & Conquer**: Explain both concepts clearly
2. **Partitioning**: How pivot divides array
3. **Why O(n²) worst case**: When pivot is always extreme
4. **Pivot selection**: Why it matters for performance
5. **In-place**: No extra space needed (unlike Merge Sort)

❌ **Common Mistakes**:
- Confusing with Merge Sort
- Not explaining recursive calls
- Wrong worst-case complexity
- Forgetting partition returns pivot index

⚡ **Advantages**:
- Fastest average case
- In-place sorting
- Cache-friendly

⚡ **Disadvantages**:
- Worst case is O(n²)
- Not stable
- Poor for nearly sorted data

---

## When pivot = 90, 64, 25 (Different Pivots)

### With Median-of-Three Pivot Strategy
```
Elements: 64, 34, 25
Median = 34
Result: Better partitioning!
```

---

## Optimizations

### 1. Random Pivot
```c
int randomPivot = arr[low + rand() % (high - low + 1)];
```

### 2. Median-of-Three
```c
int getPivot(int arr[], int low, int high) {
    int mid = low + (high - low) / 2;
    // Return median of arr[low], arr[mid], arr[high]
}
```

### 3. 3-Way Partitioning (Duplicate handling)
```c
// For arrays with many duplicates
// Partitions into: <pivot, =pivot, >pivot
```

---

## Related Problems

- [Merge Sort](./merge_sort.md)
- [Heap Sort](./heap_sort.md)
- [Selection Algorithm](./README.md)
- [Kth Smallest Element](./README.md)

---

## Practice Questions

1. What happens when pivot is always the median?
2. Why is Merge Sort O(nlogn) guaranteed?
3. Trace Quick Sort on already sorted array `[1,2,3,4,5]`
4. Compare Quick Sort vs Heap Sort
