# Unit 6 - Sorting: SOLVED ANSWERS  

---

## **Question 1: Write algorithm and code for QUICK SORT. Explain partition method. [10 Marks]**

**Asked in**: 2074 (Q), 2075 (Q), 2080 (Q), 2081 (Q)

### Answer:

#### **Basic Concept**

**Quick Sort** is a divide-and-conquer algorithm that:
1. Selects a **pivot** element
2. Partitions array: smaller → pivot ← larger
3. Recursively sorts left and right parts

```
Before: [7, 2, 8, 1, 6, 3, 5, 4]

Pick pivot = 4
Partition: [2, 1, 3] ← 4 → [7, 8, 6, 5]

Recursively sort each side:
[1, 2, 3] ← 4 → [5, 6, 7, 8]

Final: [1, 2, 3, 4, 5, 6, 7, 8] ✓
```

---

## **Partition Method (KEY CONCEPT!)**

#### **How Partitioning Works**

**Goal**: Place pivot in correct position such that all smaller elements are left and larger are right.

**Algorithm**:
```
Algorithm PARTITION(arr, low, high)
    
    pivot = arr[high]  // Last element as pivot
    i = low - 1        // Index of smaller element
    
    for j = low to high - 1:
        if arr[j] < pivot:  // Element smaller than pivot?
            i = i + 1
            SWAP(arr[i], arr[j])
    
    SWAP(arr[i+1], arr[high])  // Place pivot in position
    return i + 1  // Return pivot position
```

#### **Visual Example**

```
Array: [7, 2, 8, 1, 6, 3, 5, 4]
pivot = 4 (last element)

i = -1, j = 0: arr[0]=7
    7 < 4? NO
    
j = 1: arr[1]=2
    2 < 4? YES
    i = 0
    SWAP arr[0]↔arr[1]: [2, 7, 8, 1, 6, 3, 5, 4]

j = 2: arr[2]=8
    8 < 4? NO

j = 3: arr[3]=1
    1 < 4? YES
    i = 1
    SWAP arr[1]↔arr[3]: [2, 1, 8, 7, 6, 3, 5, 4]

j = 4: arr[4]=6
    6 < 4? NO

j = 5: arr[5]=3
    3 < 4? YES
    i = 2
    SWAP arr[2]↔arr[5]: [2, 1, 3, 7, 6, 8, 5, 4]

j = 6: arr[6]=5
    5 < 4? NO

Exit loop, SWAP arr[i+1]↔arr[high]:
    SWAP arr[3]↔arr[7]: [2, 1, 3, 4, 6, 8, 5, 7]
    
Return i+1 = 3

Result: [2, 1, 3] ← 4 → [6, 8, 5, 7]
        (smaller)    (larger)
```

---

## **C Code Implementation**

```c
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int partition(int arr[], int low, int high) {
    int pivot = arr[high];  // Last element as pivot
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(&arr[i], &arr[j]);
        }
    }
    
    swap(&arr[i + 1], &arr[high]);  // Place pivot
    return i + 1;  // Pivot position
}

void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);  // Partition and get pivot index
        
        quickSort(arr, low, pi - 1);      // Sort left side
        quickSort(arr, pi + 1, high);     // Sort right side
    }
}

void printArray(int arr[], int n) {
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");
}

int main() {
    int arr[] = {64, 34, 25, 12, 22, 11, 90};
    int n = 7;
    
    printf("Original: ");
    printArray(arr, n);
    
    quickSort(arr, 0, n - 1);
    
    printf("Sorted:   ");
    printArray(arr, n);
    
    return 0;
}

// Output:
// Original: 64 34 25 12 22 11 90 
// Sorted:   11 12 22 25 34 64 90
```

---

## **Complexity Analysis**

### **Best Case: O(n log n)**
```
Every partition divides array in half

       [8 elements]
         /        \
        /          \
    [4]            [4]
   /   \          /   \
 [2]   [2]      [2]   [2]
 / \   / \      / \   / \
[1][1][1][1]  [1][1][1][1]

Levels: log n = 3 (for 8 elements)
Work per level: n
Total: n log n = 8×3 = 24 operations
```

### **Average Case: O(n log n)**
```
Reasonably balanced partitions
Still roughly log n levels
n log n operations
```

### **Worst Case: O(n²)**
```
When pivot is always smallest/largest!

Array: [1, 2, 3, 4, 5, 6, 7, 8]  (already sorted)

Pick pivot = 8 (largest)
Partition: [1,2,3,4,5,6,7] ← 8
           (7 elements on left!)

Next iteration: Pick 7
Partition: [1,2,3,4,5,6] ← 7 ← 8
           (6 elements on left!)

This becomes like selection sort!
n + (n-1) + (n-2) + ... + 1 = n(n+1)/2 = O(n²)
```

---

## **When to Use Quick Sort**

✅ **Use when**:
- Average case O(n log n) expected
- In-place sorting needed (O(log n) space)
- Good cache locality (faster in practice than merge)

❌ **Avoid when**:
- Guaranteed O(n log n) needed (use merge sort)
- Data nearly sorted (worst case likely)
- Stability required (use merge sort)

---

## **Question 2: Write algorithm for MERGE SORT [10 Marks]**

**Asked in**: 2075 (Q), 2076 (Q), 2081 (Q)

### Answer:

#### **Concept**

**Merge Sort** divides array into halves, sorts each, then **merges** them.

```
Workflow:
1. Divide: [7,2,8,1,6,3,5,4]
           /              \
        [7,2,8,1]     [6,3,5,4]

2. Conquer (recursively sort each half):
        [1,2,7,8]     [3,4,5,6]

3. Merge: Combine while maintaining order
        [1,2,3,4,5,6,7,8] ✓
```

---

## **Algorithm**

```
Algorithm MERGE_SORT(arr, left, right)
    
    if left < right:
        mid = (left + right) / 2
        MERGE_SORT(arr, left, mid)      // Sort left half
        MERGE_SORT(arr, mid+1, right)   // Sort right half
        MERGE(arr, left, mid, right)    // Merge both halves
```

---

## **Merge Operation (KEY PART)**

```
Algorithm MERGE(arr, left, mid, right)
    
    Create temporary arrays:
    leftArr = arr[left...mid]
    rightArr = arr[mid+1...right]
    
    i = 0, j = 0, k = left
    
    // Compare elements from both arrays
    while i < len(leftArr) AND j < len(rightArr):
        if leftArr[i] <= rightArr[j]:
            arr[k] = leftArr[i]
            i = i + 1
        else:
            arr[k] = rightArr[j]
            j = j + 1
        k = k + 1
    
    // Copy remaining elements
    while i < len(leftArr):
        arr[k] = leftArr[i]
        i = i + 1
        k = k + 1
    
    while j < len(rightArr):
        arr[k] = rightArr[j]
        j = j + 1
        k = k + 1
```

---

## **C Code**

```c
void merge(int arr[], int left, int mid, int right) {
    int leftSize = mid - left + 1;
    int rightSize = right - mid;
    
    // Create temporary arrays
    int *leftArr = (int *)malloc(leftSize * sizeof(int));
    int *rightArr = (int *)malloc(rightSize * sizeof(int));
    
    // Copy data to temporary arrays
    for (int i = 0; i < leftSize; i++)
        leftArr[i] = arr[left + i];
    
    for (int i = 0; i < rightSize; i++)
        rightArr[i] = arr[mid + 1 + i];
    
    // Merge the two arrays
    int i = 0, j = 0, k = left;
    
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
    
    // Copy remaining elements from leftArr
    while (i < leftSize) {
        arr[k] = leftArr[i];
        i++;
        k++;
    }
    
    // Copy remaining elements from rightArr
    while (j < rightSize) {
        arr[k] = rightArr[j];
        j++;
        k++;
    }
    
    // Free temporary arrays
    free(leftArr);
    free(rightArr);
}

void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

int main() {
    int arr[] = {64, 34, 25, 12, 22, 11, 90};
    int n = 7;
    
    printf("Original: ");
    for(int i = 0; i < n; i++) printf("%d ", arr[i]);
    
    mergeSort(arr, 0, n - 1);
    
    printf("\nSorted:   ");
    for(int i = 0; i < n; i++) printf("%d ", arr[i]);
    
    return 0;
}

// Output:
// Original: 64 34 25 12 22 11 90 
// Sorted:   11 12 22 25 34 64 90
```

---

## **Merge Example Step-by-Step**

```
Array: [40, 10, 3, 2]

Divide:
    [40, 10]        [3, 2]
    /     \         /    \
  [40]   [10]     [3]    [2]

Sort individual elements (already "sorted"):
  [40]   [10]     [3]    [2]

Merge pairs:
  Merge [40] and [10]:  [10, 40]
  Merge [3] and [2]:    [2, 3]

Merge halves:
  Merge [10, 40] and [2, 3]:
  
  Compare 10 vs 2 → 2 < 10, add 2: [2]
  Compare 10 vs 3 → 3 < 10, add 3: [2, 3]
  Add remaining 10, 40: [2, 3, 10, 40] ✓
```

---

## **Complexity Analysis**

### **Time Complexity: Always O(n log n)**

```
Divide:      log n levels (5 for 32 elements)
             Each element goes through log n divisions
             
Merge:       Total work = n per level
             
Total:       n × log n = O(n log n)
                          ✓ Even in worst case!
```

### **Space Complexity: O(n)**
```
Temporary arrays needed during merge
Extra space = n
```

---

## **Comparison: Quick Sort vs Merge Sort**

| Feature | Quick Sort | Merge Sort |
|---------|-----------|-----------|
| **Best** | O(n log n) | O(n log n) |
| **Average** | O(n log n) | O(n log n) |
| **Worst** | O(n²) | O(n log n) ← Better! |
| **Space** | O(log n) | O(n) |
| **Stable** | No | Yes ✓ |
| **In-place** | Yes | No |
| **Cache friendly** | Yes ✓ | No |
| **Best for** | General | Guarantees needed |

---

## **Question 3: Compare Bubble Sort, Insertion Sort, Selection Sort [5 Marks]**

**Asked in**: 2074, 2076

### Answer:

#### **Table Comparison**

| Aspect | Bubble Sort | Insertion | Selection |
|--------|------------|-----------|-----------|
| **Algorithm** | Adjacent swap | Shift and insert | Min swap |
| **Best** | O(n) | O(n) | O(n²) |
| **Average** | O(n²) | O(n²) | O(n²) |
| **Worst** | O(n²) | O(n²) | O(n²) |
| **Space** | O(1) | O(1) | O(1) |
| **Stable** | Yes | Yes | No |
| **Use** | Educational | Small/nearly sorted | Educational |

---

### **Bubble Sort**

```
Algorithm BUBBLE_SORT(arr, n)
    
    for i = 0 to n-1:
        for j = 0 to n-i-2:
            if arr[j] > arr[j+1]:
                SWAP(arr[j], arr[j+1])
```

**Example**: [5, 2, 8, 1]
```
Pass 1: [5,2] → [2,5], [5,8] → [5,8], [8,1] → [1,8]
        Result: [2, 5, 1, 8]
                
Pass 2: [2,5] → [2,5], [5,1] → [1,5]
        Result: [2, 1, 5, 8]
        
Pass 3: [2,1] → [1,2]
        Result: [1, 2, 5, 8] ✓
```

✓ **Advantage**: Can detect if already sorted (O(n) if sorted)
✗ **Disadvantage**: Slow, many unnecessary swaps

---

### **Insertion Sort**

```
Algorithm INSERTION_SORT(arr, n)
    
    for i = 1 to n-1:
        key = arr[i]
        j = i - 1
        
        while j >= 0 AND arr[j] > key:
            arr[j+1] = arr[j]
            j = j - 1
        
        arr[j+1] = key
```

**Example**: [5, 2, 8, 1]
```
Pass 1: key=2, Insert in [5] → [2, 5, 8, 1]
Pass 2: key=8, Already in place → [2, 5, 8, 1]
Pass 3: key=1, Insert in [2,5,8] → [1, 2, 5, 8] ✓
```

✓ **Advantage**: Good for nearly sorted, O(n) best
✓ **Advantage**: Stable sorting
✗ **Disadvantage**: Still O(n²) average

---

### **Selection Sort**

```
Algorithm SELECTION_SORT(arr, n)
    
    for i = 0 to n-2:
        minIdx = i
        
        for j = i+1 to n-1:
            if arr[j] < arr[minIdx]:
                minIdx = j
        
        SWAP(arr[i], arr[minIdx])
```

**Example**: [5, 2, 8, 1]
```
Pass 1: Find min (1) in [5,2,8,1], swap: [1, 2, 8, 5]
Pass 2: Find min (2) in [2,8,5], already placed: [1, 2, 8, 5]
Pass 3: Find min (5) in [8,5], swap: [1, 2, 5, 8] ✓
```

✓ **Advantage**: Minimum swaps (n-1 swaps max)
✗ **Disadvantage**: Always O(n²), not stable, slow

---

## **When to Use**

- **Bubble Sort**: Only for educational purposes! (or very small arrays)
- **Insertion Sort**: When array nearly sorted or small size
- **Selection Sort**: When swaps are expensive (write-restricted memory)
- **Merge Sort**: When stability required
- **Quick Sort**: General purpose, best average performance
- **Heap Sort**: Guaranteed O(n log n) without extra memory

---

## **Quick Summary - Unit 6**

| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | No |
| Quick | O(nlogn) | O(nlogn) | O(n²) | O(logn) | No |
| Merge | O(nlogn) | O(nlogn) | O(nlogn) | O(n) | Yes |
| Heap | O(nlogn) | O(nlogn) | O(nlogn) | O(1) | No |

