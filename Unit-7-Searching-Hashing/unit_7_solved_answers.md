# Unit 7 - Searching & Hashing: SOLVED ANSWERS

---

## **Question 1: Write algorithm for BINARY SEARCH. Explain conditions. [5 Marks]**

**Asked in**: 2074, 2075, 2080, 2081 (Most repeated!)

### Answer:

#### **Concept**

Binary Search finds an element in a **SORTED** array by repeatedly dividing search space in half.

```
Array: [1, 3, 5, 7, 9, 11, 13, 15]  (MUST be sorted!)
Search: 7

Step 1: mid = (0+7)/2 = 3 → arr[3]=7 ✓ Found!
```

---

## **Algorithm**

```
Algorithm BINARY_SEARCH(arr, low, high, target)
    
    while low <= high:
        mid = (low + high) / 2
        
        if arr[mid] == target:
            return mid          // Found!
        
        else if arr[mid] < target:
            low = mid + 1       // Search right half
        
        else:
            high = mid - 1      // Search left half
    
    return -1                   // Not found
```

---

## **C Code - Iterative**

```c
int binarySearch(int arr[], int n, int target) {
    int low = 0, high = n - 1;
    
    while (low <= high) {
        int mid = (low + high) / 2;  // Safe: mid = low + (high-low)/2
        
        if (arr[mid] == target)
            return mid;  // Found at position mid
        
        else if (arr[mid] < target)
            low = mid + 1;  // Target in right half
        
        else
            high = mid - 1;  // Target in left half
    }
    
    return -1;  // Not found
}
```

---

## **C Code - Recursive**

```c
int binarySearchRecursive(int arr[], int low, int high, int target) {
    if (low > high)
        return -1;  // Not found
    
    int mid = (low + high) / 2;
    
    if (arr[mid] == target)
        return mid;
    
    else if (arr[mid] < target)
        return binarySearchRecursive(arr, mid + 1, high, target);
    
    else
        return binarySearchRecursive(arr, low, mid - 1, target);
}
```

---

## **Step-by-Step Example**

```
Array: [1, 3, 5, 7, 9, 11, 13, 15]  (indices 0-7)
Search: 11

Iteration 1:
  low=0, high=7
  mid = (0+7)/2 = 3
  arr[3] = 7
  7 < 11? YES → low = 4

  [1, 3, 5, 7 | 9, 11, 13, 15]
              ↑
           (eliminate left half)

Iteration 2:
  low=4, high=7
  mid = (4+7)/2 = 5
  arr[5] = 11
  11 == 11? YES → return 5 ✓

Found at index 5!
```

---

## **Conditions for Binary Search**

✅ **MUST BE SATISFIED**:
1. **Array MUST be SORTED** (ascending or descending)
2. **Random access** required (array, not linked list)
3. **Comparable elements**

❌ **Does NOT work on**:
- Unsorted arrays
- Linked lists
- Hash tables

---

## **Complexity Analysis**

### **Time Complexity: O(log n)**

```
Array size → Comparisons
1 → 1 (100 = 1)
2 → 2 (21 = 2) 
4 → 3 (22 = 4)
8 → 4 (23 = 8)
16 → 5 (24 = 16)
32 → 6 (25 = 32)
64 → 7 (26 = 64)
1,000,000 → 20 comparisons!

Formula: log₂(n) + 1
```

### **Comparison: Linear vs Binary**

```
Search for element in 1,000,000 items:

Linear Search:     
  Worst case: 1,000,000 comparisons ❌

Binary Search:     
  Worst case: log₂(1,000,000) ≈ 20 comparisons ✓
  
Speedup: 50,000× faster!
```

---

## **Edge Cases**

```c
// Test case: Empty array
result = binarySearch(arr, 0, -1);  // Should return -1

// Test case: Single element
int arr[] = {5};
result = binarySearch(arr, 1, 5);   // Should return 0

// Test case: Element at ends
int arr[] = {1, 2, 3, 4, 5};
result = binarySearch(arr, 5, 1);   // Should return 0
result = binarySearch(arr, 5, 5);   // Should return 4

// Test case: Not in array
result = binarySearch(arr, 5, 6);   // Should return -1
```

---

## **Question 2: Explain Collision Resolution Techniques in Hashing [5 Marks]**

**Asked in**: 2074, 2075, 2076, 2078, 2080

### Answer (MOST REPEATED QUESTION!)

#### **What is Collision?**

When two different keys hash to the same index:

```
Hash Table (size = 5):
Key 12 → h(12) = 12 % 5 = 2
Key 7  → h(7) = 7 % 5 = 2  ← COLLISION!
```

---

## **Collision Resolution Techniques**

### **1. Linear Probing**

**Concept**: If position is occupied, find next empty slot linearly.

```
Hash function: h(key) = key % TABLE_SIZE

Insert 12 (hash = 2):
  [_, _, 12, _, _]

Insert 7 (hash = 2, COLLISION):
  Position 2 occupied → try 3
  [_, _, 12, 7, _]

Insert 17 (hash = 2, COLLISION):
  Position 2 occupied → try 3
  Position 3 occupied → try 4
  [_, _, 12, 7, 17]
```

**Algorithm**:
```
Algorithm INSERT_LINEAR_PROBING(key, value)
    
    index = h(key)  // Hash function
    
    while table[index] is occupied:
        index = (index + 1) % TABLE_SIZE
    
    table[index] = value
```

**Problems**:
```
❌ Clustering: Long sequences of filled slots form
❌ Secondary clustering: Keys with same hash create groups

Example:
[12, 13, 14, 15, 16, _, _]
                    ↑
              Cluster of 5!
              
If new key hashes to this cluster,
must skip whole sequence!
```

**Advantages**:
✓ Simple
✓ Cache friendly
✓ No extra space for pointers

---

### **2. Quadratic Probing**

**Concept**: Skip by increasing intervals: 1, 4, 9, 16, ...

```
Algorithm INSERT_QUADRATIC_PROBING(key, value)
    
    index = h(key)
    i = 0
    
    while table[index] is occupied:
        i = i + 1
        index = (h(key) + i²) % TABLE_SIZE
    
    table[index] = value
```

**Example**:
```
Insert 12 (hash = 2):
  [_, _, 12, _, _]

Insert 7 (hash = 2):
  i = 0: index = 2 → occupied
  i = 1: index = (2 + 1) % 5 = 3 → free
  [_, _, 12, 7, _]

Insert 17 (hash = 2):
  i = 0: index = 2 → occupied
  i = 1: index = (2 + 1) % 5 = 3 → occupied
  i = 2: index = (2 + 4) % 5 = 1 → free
  [_, 17, 12, 7, _]

Gaps: 1 → 4 → 9 → 16 ...
      3    4    5
```

**Better spacing** than linear probing.

---

### **3. Double Hashing**

**Concept**: Use two hash functions h₁ and h₂

```
Algorithm INSERT_DOUBLE_HASHING(key, value)
    
    index = h1(key)
    i = 0
    
    while table[index] is occupied:
        i = i + 1
        index = (h1(key) + i * h2(key)) % TABLE_SIZE
    
    table[index] = value
```

**Example**:
```
Hash functions:
h1(key) = key % 5
h2(key) = 1 + (key % 4)

Insert 12:
  h1(12) = 2
  [_, _, 12, _, _]

Insert 7:
  h1(7) = 2 (collision)
  h2(7) = 1 + (7 % 4) = 4
  i = 1: index = (2 + 1*4) % 5 = 1 → free
  [_, 7, 12, _, _]

Insert 17:
  h1(17) = 2 (collision)
  h2(17) = 1 + (17 % 4) = 2
  i = 1: index = (2 + 1*2) % 5 = 4 → free
  [_, 7, 12, _, 17]
```

**Advantages**:
✓ Better distribution than linear/quadratic
✓ Reduced clustering
✓ Good performance

---

### **4. Chaining (Separate Chaining)**

**Concept**: Each table index points to a **linked list** of colliding elements.

```
Array of size 5:

Index 0 → [12] → [22] → NULL
Index 1 → [11] → NULL
Index 2 → [7] → [17] → [27] → NULL
Index 3 → NULL
Index 4 → [14] → NULL

All elements with same hash
share a linked list!
```

**Algorithm**:
```
Algorithm INSERT_CHAINING(key, value)
    
    index = h(key)
    NodePtr = table[index]
    
    // Create new node
    newNode = create node with (key, value)
    
    // Insert at beginning of list
    newNode.next = NodePtr
    table[index] = newNode
```

**C Code**:
```c
struct Node {
    int key;
    int value;
    struct Node *next;
};

void insertChaining(struct Node **table, int tableSize, int key, int value) {
    int index = key % tableSize;
    
    struct Node *newNode = (struct Node *)malloc(sizeof(struct Node));
    newNode->key = key;
    newNode->value = value;
    newNode->next = table[index];
    
    table[index] = newNode;
}

int searchChaining(struct Node **table, int tableSize, int key) {
    int index = key % tableSize;
    
    struct Node *current = table[index];
    while (current != NULL) {
        if (current->key == key)
            return current->value;
        current = current->next;
    }
    
    return -1;  // Not found
}
```

**Advantages**:
✓ Simple
✓ Dynamic sizing
✓ No clustering
✓ Separate space for pointers

**Disadvantages**:
✗ Extra memory for pointers
✗ Linked list traversal (slower)

---

## **Comparison Table**

| Technique | Probe | Clustering | Space | Time |
|-----------|-------|-----------|-------|------|
| Linear | 1, 2, 3, ... | Primary | O(1) | O(n) worst |
| Quadratic | 1, 4, 9, ... | Reduced | O(1) | O(n) worst |
| Double Hash | Variable | Minimal | O(1) | O(n) worst |
| Chaining | — | None | O(n) | O(n) worst |

---

## **Load Factor**

**Load Factor (α)** = Number of elements / Table size

```
Example:
Table size = 10
10 elements inserted
α = 10/10 = 1.0

α ≈ 0.5-0.7: Good performance
α > 1.0: Many collisions, needs rehashing
```

---

## **Hash Function Quality**

Good hash function should:
✅ Distribute uniformly
✅ Fast to compute
✅ Minimize collisions
✅ Use entire table

**Bad hash function**:
```c
// Bad: many collisions
hash(key) = key % small_prime

// Better: multiplication
hash(key) = (key * 2654435761) >> 32

// Best for strings: 
hash(s) = s[0]*31 + s[1]*31² + ... (if available)
```

---

## **When to Use**

| Technique | Best for |
|-----------|----------|
| **Linear** | Small tables, cache friendly |
| **Quadratic** | Better distribution than linear |
| **Double Hash** | Large tables, minimal clustering |
| **Chaining** | Dynamic data, memory available |

---

## **Quick Summary - Unit 7**

| Topic | Key Points |
|-------|-----------|
| **Binary Search** | O(log n), requires sorted array |
| **Linear Probing** | Skip 1, 2, 3... (causes clustering) |
| **Quadratic Probing** | Skip 1², 2², 3² (better) |
| **Double Hashing** | Use two hash functions (best) |
| **Chaining** | Linked list per index (simplest) |
| **Load Factor** | Keep < 0.75 for good performance |

