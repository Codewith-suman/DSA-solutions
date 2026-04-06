# Unit 4 - Recursion: ADDITIONAL SOLVED ANSWERS

---

## **Question 1: Differentiate between Recursion and Iteration [5 Marks]**

**Asked in**: 2074

### Answer:

#### **Definition Recap**

**Recursion**: Function calls itself with modified parameters until base case
**Iteration**: Loop (for/while) that repeats until condition



 false

#### **Detailed Comparison Table**

| Aspect | Recursion | Iteration |
|--------|-----------|-----------|
| **Definition** | Function calling itself | Loop repeating code block |
| **Termination** | Base case | Loop condition becomes false |
| **Memory** | Call stack (O(n)) | No extra stack (O(1)) |
| **Speed** | Slower (function calls) | Faster (direct loop) |
| **Code Length** | Short, elegant | More verbose |
| **Readability** | More elegant | More straightforward |
| **Easy to Understand** | Harder | Easier |
| **Stack Overflow Risk** | Yes (deep recursion) | No |
| **When to Use** | Tree/Graph traversal | Repetitive calculations |

---

## **Example 1: Factorial**

### Recursive Approach

```c
int factorial_recursive(int n) {
    // Base case
    if (n <= 1)
        return 1;
    
    // Recursive case
    return n * factorial_recursive(n - 1);
}
```

**Call stack for factorial(5)**:
```
factorial(5)
  → 5 * factorial(4)
    → 4 * factorial(3)
      → 3 * factorial(2)
        → 2 * factorial(1)
          → return 1 (base case)
        → return 2 * 1 = 2
      → return 3 * 2 = 6
    → return 4 * 6 = 24
  → return 5 * 24 = 120
```

**Stack depth**: 5 levels deep
**Memory**: O(n) for call stack

### Iterative Approach

```c
int factorial_iterative(int n) {
    int result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;  // Simply multiply
    }
    return result;
}
```

**Execution for factorial(5)**:
```
i=2: result = 1 * 2 = 2
i=3: result = 2 * 3 = 6
i=4: result = 6 * 4 = 24
i=5: result = 24 * 5 = 120
return 120
```

**Memory**: O(1) - constant space
**Speed**: ~5-10x faster than recursive!

---

## **Example 2: Sum of Array**

### Recursive

```c
int sum_recursive(int arr[], int n) {
    // Base case
    if (n == 0)
        return 0;
    
    // Recursive case
    return arr[n - 1] + sum_recursive(arr, n - 1);
}

// Call: sum_recursive(arr, 5)
```

**Call stack**:
```
sum([1,2,3,4,5], 5)
  → 5 + sum([1,2,3,4,5], 4)
    → 4 + sum([1,2,3,4,5], 3)
      → 3 + sum([1,2,3,4,5], 2)
        → 2 + sum([1,2,3,4,5], 1)
          → 1 + sum([1,2,3,4,5], 0)
            → 0 (base case)
          → 1 + 0 = 1
        → 2 + 1 = 3
      → 3 + 3 = 6
    → 4 + 6 = 10
  → 5 + 10 = 15
```

**Depth**: 6 levels

### Iterative

```c
int sum_iterative(int arr[], int n) {
    int sum = 0;
    for (int i = 0; i < n; i++) {
        sum += arr[i];
    }
    return sum;
}
```

**Execution**:
```
i=0: sum = 0 + 1 = 1
i=1: sum = 1 + 2 = 3
i=2: sum = 3 + 3 = 6
i=3: sum = 6 + 4 = 10
i=4: sum = 10 + 5 = 15
return 15
```

---

## **Side-by-Side Comparison Chart**

```
Fibonacci(5):

RECURSIVE:              ITERATIVE:
f(5)                    a=0, b=1
├─f(4)                  i=1: result=1
│ ├─f(3)                i=2: result=1
│ │ ├─f(2)              i=3: result=2
│ │ │ ├─f(1)→1          i=4: result=3
│ │ │ └─f(0)→0          i=5: result=5
│ │ └─f(2)              return 5
│ └─f(3)
└─f(4)

Calls: 15                Loops: 5
Stack: O(n)              Stack: O(1)
Time: O(2^n)             Time: O(n)
```

---

## **When to Use Which?**

### Use **Recursion** When:
✅ Natural recursive structure (trees, graphs)
✅ Divide and conquer algorithms
✅ Code clarity more important than speed
✅ Problem has optimal substructure
✅ Shallow recursion depth likely

**Examples**: Tree traversal, Merge sort, Binary search

### Use **Iteration** When:
✅ Need maximum performance
✅ Memory is limited
✅ Deep loops possible (avoid stack overflow)
✅ Simple repetition needed
✅ Must minimize overhead

**Examples**: Array summation, Linear search, Simple loops

---

## **Performance Comparison**

```c
// TESTING: Find sum of 1 to 1000000

Recursive: STACK OVERFLOW! (call stack too deep)
Iterative: 1 millisecond ✓

Recursive: Find sum of 1 to 1000
           ~0.1 seconds (slow)
Iterative: ~0.0001 seconds (FAST!)
           
Speedup: 1000x faster!
```

---

## **Question 2: What is Tail Recursion? Explain with example [5 Marks]**

**Asked in**: 2075, 2080; Notes in 2081

### Answer:

#### **Definition**

**Tail Recursion**: Recursive call is the **LAST operation** in the function.

```
Regular Recursion:
f(n) = n + f(n-1)  ← After recursion, must add n
       (more work)

Tail Recursion:
f(n, acc) = if n==0 → acc
            else → f(n-1, acc+n)  ← Recursion is LAST action
                                     (no more work after)
```

#### **Example 1: Factorial**

### Non-Tail Recursive

```c
int factorial(int n) {
    if (n <= 1)
        return 1;
    
    return n * factorial(n - 1);  ← Multiply AFTER recursion!
}
```

**Call stack**:
```
factorial(5)
└─ 5 * factorial(4)
   └─ 4 * factorial(3)
      └─ 3 * factorial(2)
         └─ 2 * factorial(1)
            └─ return 1
         ← return 2*1=2
      ← return 3*2=6
   ← return 4*6=24
← return 5*24=120

Stack depth: 5 (all kept in memory!)
```

### Tail Recursive Version

```c
int factorial_tail(int n, int acc) {
    // acc = accumulator (carries result)
    
    if (n <= 1)
        return acc;  ← Base case returns result directly
    
    return factorial_tail(n - 1, n * acc);  ← Recursive call with updated accumulator
}

// Call: factorial_tail(5, 1)
```

**Call stack**:
```
factorial_tail(5, 1)
└─ factorial_tail(4, 5)
   └─ factorial_tail(3, 20)
      └─ factorial_tail(2, 60)
         └─ factorial_tail(1, 120)
            └─ return 120

Code optimization (compiler):
Each call can REUSE the same stack frame!
Stack depth: 1 (optimized!)
```

---

## **Example 2: Sum of Array**

### Non-Tail Recursive

```c
int sum_regular(int arr[], int n) {
    if (n == 0)
        return 0;
    
    return arr[n - 1] + sum_regular(arr, n - 1);
    // ↑ Addition AFTER recursion (not tail)
}
```

**Stack remains active waiting for additions**.

### Tail Recursive

```c
int sum_tail(int arr[], int n, int acc) {
    if (n == 0)
        return acc;      ← Return directly
    
    return sum_tail(arr, n - 1, acc + arr[n - 1]);
    // ↑ Nothing after the recursive call!
}

// Call: sum_tail(arr, 5, 0)
```

**Compiler can optimize to iteration automatically!**

---

## **Example 3: Fibonacci (Most Clear)**

### Non-Tail

```c
int fib_regular(int n) {
    if (n <= 1)
        return n;
    
    return fib_regular(n-1) + fib_regular(n-2);
    // ↑ Addition AFTER - not tail recursive
    // Also: exponential time O(2^n)!
}
```

### Tail Recursive

```c
int fib_tail(int n, int a, int b) {
    // a, b maintain previous two values
    if (n == 0)
        return a;
    if (n == 1)
        return b;
    
    return fib_tail(n - 1, b, a + b);
    // ↑ Only recursion at end, returns directly
}

// Call: fib_tail(5, 0, 1)
// Returns: 5
```

**Much faster! O(n) instead of O(2^n)**

---

##**Tail Call Optimization**

**Smart compilers** detect tail recursion and optimize:

```
Before Optimization:
factorial_tail(5, 1) → stack frame 1
  └─ factorial_tail(4, 5) → stack frame 2
     └─ factorial_tail(3, 20) → stack frame 3
        └─ ... 5 frames total

After Optimization (by compiler):
factorial_tail(5, 1)
└─ Update parameters, jump back
   (REUSES same stack frame!)
   
Result: O(1) space instead of O(n)!
```

**Languages with TCO (Tail Call Optimization)**:
- Scheme ✓ (guaranteed)
- Functional languages (Haskell, Lisp) ✓
- C compilers (gcc -O2) ✓ (optional)
- Java ✗ (no TCO)
- Python ✗ (by design)

---

## **Key Differences Summary**

| Item | Regular Recursion | Tail Recursion |
|------|-------------------|-----------------|
| **Last operation** | More work after call | Recursion is last |
| **Stack depth** | O(n) | O(1) with optimization |
| **Time** | Slower | Can be optimized to loop speed |
| **Space** | O(n) memory | O(1) if optimized |
| **Compiler magic** | No | Converts to iteration! |
| **When to use** | Clear logic | Performance critical |
| **Example** | Merge sort | Fibonacci, factorial |

---

## **How to Convert to Tail Recursion**

**Step 1**: Identify operations after recursive call
**Step 2**: Move them to helper function parameters
**Step 3**: Use accumulator pattern

**Example**:

```
BEFORE:
sum(n) = n + sum(n-1)

AFTER:
sum(n) = helper(n, 0)
helper(n, acc) = if n==0 → acc
                 else → helper(n-1, acc+n)
```

---

## **Quick Tips**

✅ **Tail recursion** = last operation is recursive call
✅ **Compiler optimization** converts (somecompilers) to loop
✅ **Accumulator pattern** = helper parameter carrying result
✅ **Performance gain** = O(1) space vs O(n)
✅ **Not always better** = may sacrifice readability

**EXAM TIP**: Explain both regular and how to convert to tail!

