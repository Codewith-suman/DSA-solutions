# Factorial (Recursion)

**Marks**: 5 marks  
**Difficulty**: ⭐⭐ Easy

---

## Problem Statement

Write a **recursive function** to find the **factorial** of a number.

**Definition**: n! = n × (n-1) × ... × 2 × 1

**Examples**:
- 5! = 5 × 4 × 3 × 2 × 1 = 120
- 0! = 1 (by definition)

---

## Concept Overview

### Recursive Definition
```
Base case:     0! = 1
Recursive:     n! = n × (n-1)!
```

### Why It's Easy
- Simple base case
- Clear recursive relationship
- Linear time: O(n)

---

## Algorithm

### Pseudocode

```
Algorithm FACTORIAL(n)
    
    if n == 0 or n == 1:
        return 1
    
    return n × FACTORIAL(n-1)
```

---

## C Code

```c
#include <stdio.h>

int factorial(int n) {
    printf("Calling factorial(%d)\n", n);
    
    // Base case
    if (n == 0 || n == 1) {
        printf("Base case reached: %d! = 1\n", n);
        return 1;
    }
    
    // Recursive case
    int result = n * factorial(n - 1);
    printf("factorial(%d) = %d\n", n, result);
    return result;
}

int main() {
    printf("=== FACTORIAL (RECURSION) ===\n\n");
    
    int n = 5;
    printf("Finding %d! using recursion:\n\n", n);
    
    int result = factorial(n);
    
    printf("\nResult: %d! = %d\n", n, result);
    
    return 0;
}
```

---

## Trace Example: 5!

```
factorial(5)
  → 5 × factorial(4)
    → 4 × factorial(3)
      → 3 × factorial(2)
        → 2 × factorial(1)
          → Base case: return 1
        → 2 × 1 = 2
      → 3 × 2 = 6
    → 4 × 6 = 24
  → 5 × 24 = 120

Result: 120
```

---

## Call Stack Visualization

```
Level 1: factorial(5) waits for factorial(4)
         |
Level 2: factorial(4) waits for factorial(3)
         |
Level 3: factorial(3) waits for factorial(2)
         |
Level 4: factorial(2) waits for factorial(1)
         |
Level 5: factorial(1) → returns 1
         |
Level 4: 2 × 1 = 2 → returns 2
         |
Level 3: 3 × 2 = 6 → returns 6
         |
Level 2: 4 × 6 = 24 → returns 24
         |
Level 1: 5 × 24 = 120 → returns 120
```

---

## Factorial Values

| n | n! | n | n! |
|---|-----|---|-----|
| 0 | 1 | 5 | 120 |
| 1 | 1 | 6 | 720 |
| 2 | 2 | 7 | 5,040 |
| 3 | 6 | 8 | 40,320 |
| 4 | 24 | 10 | 3,628,800 |
| 12 | 479,001,600 | 20 | 2.4 × 10^18 |

---

## Complexity Analysis

### Time Complexity
- **O(n)** - Make n recursive calls
- Each call: O(1) work

### Space Complexity
- **O(n)** - Recursion stack depth is n
- Each call stored on call stack

---

## Iterative Version (Better)

```c
int factorial_iterative(int n) {
    int result = 1;
    for (int i = 2; i <= n; i++)
        result *= i;
    return result;
}
```

**Advantages**:
- Same time O(n)
- Better space O(1)
- No stack overflow risk

---

## Key Points

✅ **Remember**:
1. **Base case**: 0! = 1 or 1! = 1
2. **Recursive**: n! = n × (n-1)!
3. **Time**: O(n)
4. **Space**: O(n) due to recursion stack

❌ **Mistakes**:
- Wrong base case
- Not handling n=0
- Integer overflow for large n

⚡ **Pro Tip**: Use iterative for production code!

---

## Integer Overflow Alert

```
Beyond n=20, factorial overflows int
Use:
- unsigned int for n ≤ 13
- long long for n ≤ 20
- Big integers for larger n
```

---

## Related Topics

- [Fibonacci (Recursion)](./fibonacci.md)
- [Tower of Hanoi](./tower_of_hanoi.md)
- [Recursion Basics](./README.md)
