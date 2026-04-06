# Tower of Hanoi (Recursion)

**Questions Asked**: 4 times (Model, 2076, 2079, 2081)  
**Marks**: 5 marks  
**Difficulty**: ⭐⭐⭐ Medium

---

## Problem Statement

Write a recursive algorithm to solve the **Tower of Hanoi (TOH) problem**. Trace it for **3 disks**.

**Problem**: Move all disks from **Source** to **Destination** using **Auxiliary** rod, following these rules:
1. Only one disk can be moved at a time
2. Larger disk can never be placed on smaller disk
3. All disks must end on Destination rod

---

## Concept Overview

### The Legend
- A Buddhist monastery had 3 golden rods
- 64 golden disks, largest at bottom
- Priests moved one disk at a time
- Legend says when done, world ends!

### Why It's Recursive
- Problem with n disks depends on problem with (n-1) disks
- Classic example of divide & conquer using recursion

### Key Insight
To move n disks from Source to Destination:
1. **Move (n-1) disks** from Source to Auxiliary (using Destination as temporary)
2. **Move largest disk** from Source to Destination
3. **Move (n-1) disks** from Auxiliary to Destination (using Source as temporary)

---

## Algorithm

### Recursive Pseudocode

```
Algorithm HANOI(n, source, destination, auxiliary)
    
    if n == 1:
        // Base case: move single disk
        Move disk from source to destination
        return
    
    // Step 1: Move (n-1) disks from source to auxiliary
    HANOI(n-1, source, auxiliary, destination)
    
    // Step 2: Move largest disk from source to destination
    Move disk from source to destination
    
    // Step 3: Move (n-1) disks from auxiliary to destination
    HANOI(n-1, auxiliary, destination, source)
```

### How to Call
```
HANOI(3, "A", "C", "B")
  A = Source rod
  C = Destination rod
  B = Auxiliary rod
```

---

## C Code Implementation

```c
#include <stdio.h>

int moveCount = 0;

// Recursive function to solve Tower of Hanoi
void hanoi(int n, char source, char dest, char aux) {
    // Base case: if only one disk
    if (n == 1) {
        moveCount++;
        printf("Move %d: Move disk 1 from %c to %c\n", moveCount, source, dest);
        return;
    }
    
    // Step 1: Move n-1 disks from source to auxiliary using destination
    hanoi(n - 1, source, aux, dest);
    
    // Step 2: Move largest disk from source to destination
    moveCount++;
    printf("Move %d: Move disk %d from %c to %c\n", moveCount, n, source, dest);
    
    // Step 3: Move n-1 disks from auxiliary to destination using source
    hanoi(n - 1, aux, dest, source);
}

int main() {
    printf("=== TOWER OF HANOI ===\n\n");
    
    int n = 3;
    char source = 'A';
    char destination = 'C';
    char auxiliary = 'B';
    
    printf("Solving Tower of Hanoi with %d disks:\n", n);
    printf("Source: %c, Destination: %c, Auxiliary: %c\n\n", 
           source, destination, auxiliary);
    
    hanoi(n, source, destination, auxiliary);
    
    printf("\nTotal moves: %d\n", moveCount);
    printf("Formula: 2^n - 1 = 2^%d - 1 = %d\n", n, (1 << n) - 1);
    
    return 0;
}
```

---

## Trace Example: 3 Disks (A → C using B)

### Visual Start State
```
ROD A      ROD B      ROD C
[3]                   
[2]                   
[1]                   
---        ---        ---
Source   Auxiliary  Destination
```

### Detailed Recursive Trace

```
Hanoi(3, A, C, B)
│
├─ Hanoi(2, A, B, C)          [Move 2 disks from A to B using C]
│  │
│  ├─ Hanoi(1, A, C, B)        [Move 1 disk from A to C]
│  │  └─ MOVE 1: A → C ✓
│  │     State: [3,2] [1] []
│  │
│  ├─ MOVE 2: A → B ✓
│  │  State: [3] [2,1] []
│  │
│  └─ Hanoi(1, C, B, A)        [Move 1 disk from C to B]
│     └─ MOVE 3: C → B ✓
│        State: [3] [2,1] []
│
├─ MOVE 4: A → C ✓             [Move largest disk 3]
│  State: [] [2,1] [3]
│
└─ Hanoi(2, B, C, A)           [Move 2 disks from B to C using A]
   │
   ├─ Hanoi(1, B, A, C)        [Move 1 disk from B to A]
   │  └─ MOVE 5: B → A ✓
   │     State: [1] [2] [3]
   │
   ├─ MOVE 6: B → C ✓
   │  State: [1] [] [3,2]
   │
   └─ Hanoi(1, A, C, B)        [Move 1 disk from A to C]
      └─ MOVE 7: A → C ✓
         State: [] [] [3,2,1]
```

### Moves Table

| Move | Disk | From | To | State After |
|------|------|------|----|----|
| 1 | 1 | A | C | A:[3,2] B:[] C:[1] |
| 2 | 2 | A | B | A:[3] B:[2,1] C:[] |
| 3 | 1 | C | B | A:[3] B:[2,1] C:[] |
| 4 | 3 | A | C | A:[] B:[2,1] C:[3] |
| 5 | 1 | B | A | A:[1] B:[2] C:[3] |
| 6 | 2 | B | C | A:[1] B:[] C:[3,2] |
| 7 | 1 | A | C | A:[] B:[] C:[3,2,1] ✓ |

### Visual Progress

```
Start:        Move 1:       Move 2:       Move 3:       Move 4:
A[3,2,1]     A[3,2]        A[3]          A[3]          A[]
B[]          B[]           B[2,1]        B[2,1]        B[2,1]
C[]          C[1]          C[]           C[1]          C[3]

Move 5:       Move 6:       Move 7:       Final:
A[1]          A[1]          A[]           A[]
B[2]          B[]           B[]           B[]
C[3]          C[3,2]        C[3,2,1]      C[3,2,1]✓
```

---

## For n=4 Disks

```
Hanoi(4, A, C, B)
Steps:
1. Hanoi(3, A, B, C) - 7 moves
2. Move disk 4 from A to C - 1 move
3. Hanoi(3, B, C, A) - 7 moves

Total = 7 + 1 + 7 = 15 moves
Formula: 2^4 - 1 = 15 ✓
```

---

## Complexity Analysis

### Time Complexity

**Recurrence Relation**:
```
T(n) = 2·T(n-1) + 1
T(1) = 1

Solving:
T(n) = 2^n - 1
```

**Examples**:
- n=1: 1 move (2¹-1=1)
- n=2: 3 moves (2²-1=3)
- n=3: 7 moves (2³-1=7)
- n=4: 15 moves (2⁴-1=15)
- n=64: 18,446,744,073,709,551,615 moves! 💀

### Space Complexity
- **O(n)** - Recursion stack depth
- Call stack max depth = n

### Why Exponential?
- Each problem of size n creates **2 recursive calls** for size (n-1)
- Unbalanced tree of recursive calls
- Classical exponential growth

---

## Key Points for Exam

✅ **Must Answer**:
1. **Base case**: n=1, move directly
2. **Recursive case**: 3 steps clearly
3. **Why it works**: Constraints prevent invalid moves
4. **Complexity**: 2^n - 1 moves exactly
5. **Trace example**: Show all 7 moves for n=3

❌ **Common Mistakes**:
- Wrong order of recursive calls
- Missing base case
- Writing `2^n` instead of `2^n - 1`
- Tracing incorrectly

⚡ **Important Facts**:
- Optimal solution (proven)
- Can't do it in fewer moves
- At temple: 64 disks needs 18 quintillion years!

---

## Pattern Recognition

```
n    Moves   Pattern
1    1       2^1 - 1
2    3       2^2 - 1
3    7       2^3 - 1
4    15      2^4 - 1
5    31      2^5 - 1
64   2^64-1  The legend!

Each disk must be moved:
- 2^(n-1) times exactly
- Largest disk: once (in middle)
- Disk 1: 2^(n-1) times
```

---

## Thinking Process for Exam

**Question**: How would you solve TOH for 3 disks?

**Answer Structure**:
1. "To move 3 disks from A to C using B:"
2. "Step 1: Move 2 disks from A to B (using C)"
3. "Step 2: Move disk 3 from A to C"
4. "Step 3: Move 2 disks from B to C (using A)"
5. "For 2 disks, apply same logic recursively"
6. "Base case: 1 disk moves directly"

---

## Variations

### Tower of Hanoi with 4 Rods (Frame-Stewart Algorithm)
- Can be solved in O(n^(log 3/log 2)) ≈ O(n^1.585) moves
- More efficient than 3-rod version

### Tower of Hanoi Puzzle (no recursion needed)
- Find minimum moves to transfer k disks (without full transfer)

---

## Related Problems

- [Factorial Recursion](./factorial.md)
- [Fibonacci Recursion](./fibonacci.md)
- [Tree Traversals](../Unit-8-Trees-Graphs/tree_traversals.md)

---

## Practice Questions

1. How many moves for n=5 disks?
2. Trace Hanoi(2, A, B, C) step by step
3. Why must it take exactly 2^n - 1 moves?
4. Prove base case and induction
