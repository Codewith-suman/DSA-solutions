# Unit 1 - Introduction to DSA: SOLVED ANSWERS

---

## **Question 1: What is an Abstract Data Type (ADT)? Explain with examples. [5 Marks]**

**Asked in**: Model, 2075

### Answer:

#### **Definition**
An **Abstract Data Type (ADT)** is a mathematical model of a data structure that specifies:
- **What data it holds** (data structure)
- **What operations are allowed** (functions/procedures)
- **HOW operations work** (semantics)
- **NOT how it's implemented** (hidden details)

#### **Key Characteristics**
1. **Abstraction**: Implementation details are hidden from user
2. **Encapsulation**: Data and operations bundled together
3. **Interface**: Only public operations visible
4. **Specification**: Clear definition of behavior

#### **Examples with Details**

##### **Example 1: Stack ADT**

```
Data: Collection of elements in LIFO order

Operations:
  push(element) → Add element on top
  pop() → Remove and return top element
  peek() → View top without removing
  isEmpty() → Check if empty
  isFull() → Check if full

Real Implementation: Array or Linked List
(User doesn't care how it's stored)
```

##### **Example 2: Queue ADT**

```
Data: Collection of elements in FIFO order

Operations:
  enqueue(element) → Add at rear
  dequeue() → Remove from front
  peek() → View front element
  isEmpty() → Check if empty

Implementation: Array or Linked List
```

##### **Example 3: List ADT**

```
Data: Ordered collection of elements

Operations:
  insert(position, element)
  delete(position)
  get(position)
  size()
  search(element)

Can be: Array List or Linked List
```

#### **Real-World Analogy**

Think of a **Library System**:
- **ADT**: Library interface (borrow book, return book, search catalog)
- **User**: Doesn't know how library stores books
- **Implementation**: Shelves, databases, organization system (hidden)
- **Interface**: Simple: "Borrow book with ID"

#### **Benefits**
✅ Code reusability
✅ Easy to understand
✅ Can change implementation without affecting user
✅ Better testing and maintenance

---

## **Question 2: Explain Dynamic Memory Allocation in C. Differentiate between malloc() and calloc(). [5 Marks]**

**Asked in**: 2074, 2078, 2081

### Answer:

#### **Dynamic Memory Allocation - Concept**

Allocating memory **at runtime** (not compile time) from the **heap**.

```
Static (Compile-time):     int arr[10];       // Size fixed
Dynamic (Runtime):         int *arr = malloc(10*sizeof(int));
```

#### **malloc() vs calloc()**

| Feature | malloc() | calloc() |
|---------|----------|----------|
| **Syntax** | `malloc(size)` | `calloc(n, size)` |
| **Parameters** | Total bytes | # of blocks, size per block |
| **Initialization** | Garbage values | All zeros ✓ |
| **Speed** | Faster | Slower (initializes) |
| **Memory clear** | No | Yes |
| **Example** | `malloc(40)` | `calloc(10, 4)` |

#### **Detailed Example**

```c
// Allocate memory for 10 integers

// Using malloc()
int *arr1 = (int *)malloc(10 * sizeof(int));
// Values: [garbage, garbage, garbage, ...]

// Using calloc()
int *arr2 = (int *)calloc(10, sizeof(int));
// Values: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

#### **When malloc() fails**
```c
int *ptr = malloc(100);

if (ptr == NULL) {  // Memory allocation failed!
    printf("Out of memory\n");
    return;
}
```

#### **Memory deallocation (Both)**
```c
free(ptr);  // Same for both malloc() and calloc()
ptr = NULL;  // Good practice to avoid dangling pointer
```

#### **Code Example**

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("=== malloc() vs calloc() ===\n\n");
    
    // malloc() - no initialization
    int *arr1 = (int *)malloc(5 * sizeof(int));
    printf("malloc() values: ");
    for (int i = 0; i < 5; i++)
        printf("%d ", arr1[i]);  // Random values
    printf("\n");
    
    // calloc() - initialized to 0
    int *arr2 = (int *)calloc(5, sizeof(int));
    printf("calloc() values: ");
    for (int i = 0; i < 5; i++)
        printf("%d ", arr2[i]);  // All zeros
    printf("\n");
    
    // Free memory
    free(arr1);
    free(arr2);
    
    return 0;
}
```

#### **Key Differences Summary**
```
malloc(40):              calloc(10, 4):
├─ Allocates 40 bytes   ├─ Allocates 10 × 4 = 40 bytes
├─ Faster               ├─ Slower (clears memory)
├─ Not initialized      └─ Initialized to zero

Both return NULL if failed!
Both freed with free()
```

#### **Best Practices**
```c
// ✓ Good
int *ptr = (int *)malloc(n * sizeof(int));
if (ptr == NULL) { handle error; }

// ✓ Better (calloc for initialization)
int *arr = (int *)calloc(n, sizeof(int));

// ✓ Always check
if (!ptr) return;

// ✓ Always free
free(ptr);
ptr = NULL;
```

---

## **Question 3: Define Asymptotic Notations. Explain Big-Oh, Omega, Theta with examples. [5 Marks]**

**Asked in**: 2076, 2079, 2080, 2081

### Answer:

#### **What is Asymptotic Notation?**

Mathematical notation to describe **algorithm efficiency** as input size (n) grows large.

Focus: **Trend, not exact operations**

#### **Big-Oh (O) - Upper Bound**

**Definition**: Algorithm takes AT MOST this many operations.

```
f(n) = O(g(n)) if:
  f(n) ≤ c·g(n) for some constant c and large n

Meaning: Algorithm won't be slower than this
```

**Examples**:
```
Linear Search: O(n)
  - Worst case: check all n elements

Merge Sort: O(n log n)
  - Always takes n log n operations

Bubble Sort: O(n²)
  - Worst case: n² comparisons
```

#### **Big-Omega (Ω) - Lower Bound**

**Definition**: Algorithm takes AT LEAST this many operations.

```
f(n) = Ω(g(n)) if:
  f(n) ≥ c·g(n) for some constant c and large n

Meaning: Algorithm won't be faster than this
```

**Examples**:
```
Search in array: Ω(1)
  - Best case: found at first position

Merge Sort: Ω(n log n)
  - Even in best case: n log n operations

Bubble Sort: Ω(n)
  - Best case: already sorted (one pass)
```

#### **Big-Theta (Θ) - Tight Bound**

**Definition**: Algorithm takes EXACTLY this many operations (within constants).

```
f(n) = Θ(g(n)) if:
  c₁·g(n) ≤ f(n) ≤ c₂·g(n)
  for constants c₁, c₂ and large n

Meaning: Both upper and lower bounds match!
```

**Examples**:
```
Merge Sort: Θ(n log n)
  - Best: n log n, Worst: n log n → Same!

Binary Search: Θ(log n)
  - Always log n in middle

Selection Sort: Θ(n²)
  - Always n² comparisons
```

#### **Visual Comparison**

```
       f(n)
        ↑
    Θ(n²)----|  ↗╱  ← Tight bound
         |  ╱╱    
    O(n²)--|╱─────── ← Upper bound
         |╱ / 
         |/╱
    Ω(n)-|/──────── ← Lower bound
         +────→ n
         
Θ is best when O and Ω match!
```

#### **Real Algorithm Example: Linear Search**

```
Algorithm: Search for element in array

Best Case:    Element at position 1
              Operations: 1
              Ω(1) - Lower bound

Worst Case:   Element at end (or not found)
              Operations: n
              O(n) - Upper bound

Average Case: O(n) and Ω(1)
              No Θ! (bounds don't match)
```

#### **Sorting Algorithm Comparison**

| Algorithm | Best | Average | Worst | Θ (if exists) |
|-----------|------|---------|-------|---------------|
| Bubble | Ω(n²) | O(n²) | O(n²) | Θ(n²) |
| Quick | Ω(n log n) | O(n log n) | O(n²) | None |
| Merge | Ω(n log n) | O(n log n) | O(n log n) | **Θ(n log n)** |
| Heap | Ω(n log n) | O(n log n) | O(n log n) | **Θ(n log n)** |

#### **Key Points to Remember**

✅ **O (Big-Oh)**: Worst-case, upper bound
✅ **Ω (Big-Omega)**: Best-case, lower bound  
✅ **Θ (Big-Theta)**: Exact, when O = Ω
✅ **Use O notation most commonly**
✅ **Θ is stronger (more precise)**
✅ **Ignore constants**: O(2n) = O(n)
✅ **Ignore lower order**: O(n²+n) = O(n²)

#### **Common Complexity Ranking**

```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2^n) < O(n!)

Fast                                              Slow
```

---

## **Quick Summary Table - Unit 1**

| Topic | Key Answer |
|-------|-----------|
| **ADT** | Specification without implementation details. Example: Stack = push, pop, peek |
| **malloc()** | Allocates memory, not initialized, fast |
| **calloc()** | Allocates and initializes to zero, slower |
| **malloc() vs calloc()** | Same size memory, calloc initializes, calloc slower |
| **Big-Oh (O)** | Upper bound, worst case |
| **Big-Omega (Ω)** | Lower bound, best case |
| **Big-Theta (Θ)** | Tight bound, both match |

