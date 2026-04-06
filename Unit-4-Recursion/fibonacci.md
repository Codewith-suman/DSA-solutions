# Fibonacci Sequence (Recursion)

**Marks**: 5 marks  
**Difficulty**: ⭐⭐ Easy

---

## Problem Statement

Write a **recursive function** to find the **nth Fibonacci number**.

**Series**: 0, 1, 1, 2, 3, 5, 8, 13, 21, ...

---

## Concept Overview

### Fibonacci Definition
```
F(0) = 0
F(1) = 1
F(n) = F(n-1) + F(n-2)  for n > 1
```

### Recursive Pattern
- **Base cases**: n=0 → 0, n=1 → 1
- **Recursive case**: F(n) = F(n-1) + F(n-2)

### Problem with Naive Recursion
- **Exponential time**: O(2^n)
- **Repeated calculations**
- Only use for small n

---

## Algorithm

### Pseudocode - Naive Recursion

```
Algorithm FIBONACCI_NAIVE(n)
    
    if n == 0:
        return 0
    
    if n == 1:
        return 1
    
    return FIBONACCI_NAIVE(n-1) + FIBONACCI_NAIVE(n-2)
```

### Pseudocode - With Memoization

```
Algorithm FIBONACCI_MEMO(n, memo)
    
    if n in memo:
        return memo[n]
    
    if n == 0:
        return 0
    
    if n == 1:
        return 1
    
    result = FIBONACCI_MEMO(n-1, memo) + FIBONACCI_MEMO(n-2, memo)
    memo[n] = result
    return result
```

---

## C Code - Naive

```c
#include <stdio.h>

int fib_naive(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;
    return fib_naive(n - 1) + fib_naive(n - 2);
}

int main() {
    printf("Fibonacci (Naive Recursion):\n");
    for (int i = 0; i < 10; i++)
        printf("F(%d) = %d\n", i, fib_naive(i));
    return 0;
}
```

---

## C Code - With Memoization

```c
#include <stdio.h>
#include <string.h>

int memo[100];

int fib_memo(int n) {
    if (memo[n] != -1)
        return memo[n];
    
    if (n == 0) return memo[n] = 0;
    if (n == 1) return memo[n] = 1;
    
    memo[n] = fib_memo(n - 1) + fib_memo(n - 2);
    return memo[n];
}

int main() {
    memset(memo, -1, sizeof(memo));
    
    printf("Fibonacci (Memoization):\n");
    for (int i = 0; i < 20; i++)
        printf("F(%d) = %d\n", i, fib_memo(i));
    return 0;
}
```

---

## Trace Example: F(5)

### Recursive Tree

```
                F(5)
               /    \
            F(4)    F(3)
           /   \    /   \
        F(3) F(2) F(2) F(1)
       / \   / \   / \
    F(2) F(1) F(1) F(0) F(1) F(0)
    / \
 F(1) F(0)

Result: F(5) = 5
```

### Call Count

```
n=0: 1 call
n=1: 2 calls
n=2: 3 calls
n=3: 5 calls
n=4: 9 calls
n=5: 15 calls  ← Exponential!
n=10: 177 calls
n(20): 21,891 calls
```

---

## Complexity Comparison

| Approach | Time | Space | Calls for F(20) |
|----------|------|-------|---|
| Naive Recursion | O(2^n) | O(n) | 21,891 ❌ |
| Memoization | O(n) | O(n) | 20 ✓ |
| Iterative DP | O(n) | O(n) | - |
| Space-Optimized | O(n) | O(1) | - |

---

## Iterative Approach (Fastest)

```c
int fib_iterative(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;
    
    int prev = 0, curr = 1;
    for (int i = 2; i <= n; i++) {
        int next = prev + curr;
        prev = curr;
        curr = next;
    }
    return curr;
}
```

**Time**: O(n) | **Space**: O(1) | **Blazing fast!**

---

## Sequence Table

| n | F(n) | n | F(n) |
|---|------|---|------|
| 0 | 0 | 5 | 5 |
| 1 | 1 | 6 | 8 |
| 2 | 1 | 7 | 13 |
| 3 | 2 | 8 | 21 |
| 4 | 3 | 9 | 34 |
| 10 | 55 | 15 | 610 |

---

## Key Points for Exam

✅ **Remember**:
1. **Base cases**: F(0)=0, F(1)=1
2. **Recurrence**: F(n) = F(n-1) + F(n-2)
3. **Time**: Naive is O(2^n) - bad!
4. **Solution**: Use memoization → O(n)

❌ **Common Mistakes**:
- Wrong base cases
- Forgetting it's exponential
- Not mentioning optimization

⚡ **Golden Ratio**:
F(n) ≈ φ^n / √5 where φ = (1+√5)/2 ≈ 1.618

---

## Related Topics

- [Factorial (Recursion)](./factorial.md)
- [Tower of Hanoi](./tower_of_hanoi.md)
- [Dynamic Programming](./README.md)
