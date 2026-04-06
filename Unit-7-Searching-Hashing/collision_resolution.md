# Collision Resolution Techniques (Hashing)

**Questions Asked**: 5 times (Model, 2076, 2079, 2080, 2081)  
**Marks**: 5 marks  
**Difficulty**: ⭐⭐⭐ Medium

---

## Problem Statement

What is a **collision in Hashing**? Explain **Collision Resolution Techniques** (Linear Probing, Chaining, etc.).

---

## Concept Overview

### Hash Function
- Maps key to array index: `h(key) = key % table_size`
- Ideally: one key → one unique index
- In reality: **multiple keys can map to same index** (collision!)

### Collision
```
Hash Table Size: 10
Keys: 23, 13, 43, 45, 35

h(23) = 23 % 10 = 3
h(13) = 13 % 10 = 3  ← COLLISION! Same as 23
h(43) = 43 % 10 = 3  ← COLLISION! Same as 23 & 13
```

### Why Collisions Happen
- Pigeonhole principle: More keys than table size
- Limited hash space vs unlimited keys
- Hash function limitations

---

## Collision Resolution Techniques

### 1. Linear Probing (Open Addressing)

**Idea**: If position occupied, try next position

**Formula**: `h(key, i) = (h(key) + i) % m` where i = 0, 1, 2, ...

**Example**: Insert 23, 13, 43 in table of size 10

```
1. Insert 23: h(23) = 3 → Table[3] = 23
2. Insert 13: h(13) = 3 → occupied!
              h(13,1) = (3+1)%10 = 4 → Table[4] = 13
3. Insert 43: h(43) = 3 → occupied!
              h(43,1) = (3+1)%10 = 4 → occupied!
              h(43,2) = (3+2)%10 = 5 → Table[5] = 43

Table: [_, _, _, 23, 13, 43, _, _, _, _]
Index:  0  1  2  3   4   5   6  7  8  9
```

**Disadvantages**:
- Primary clustering
- Linear search time increases
- Difficult deletion

**Pseudocode**:
```
Insert(key, value):
    i = 0
    while True:
        index = (hash(key) + i) % m
        if table[index] is empty:
            table[index] = value
            return
        i = i + 1
```

---

### 2. Quadratic Probing

**Idea**: Use quadratic offsets instead of linear

**Formula**: `h(key, i) = (h(key) + i²) % m`

**Example**: Same data

```
1. Insert 23: h(23) = 3 → Table[3] = 23
2. Insert 13: h(13) = 3 → occupied!
              h(13,1) = (3+1)%10 = 4 → Table[4] = 13
3. Insert 43: h(43) = 3 → occupied!
              h(43,1) = (3+1)%10 = 4 → occupied!
              h(43,2) = (3+4)%10 = 7 → Table[7] = 43

Table: [_, _, _, 23, 13, _, _, 43, _, _]
```

**Advantages**:
- Reduces primary clustering
- Better distribution

**Disadvantages**:
- Secondary clustering
- May not find empty slot if table > 50% full

---

### 3. Double Hashing

**Idea**: Use second hash function for offset

**Formula**: `h(key, i) = (h₁(key) + i * h₂(key)) % m`

**Example**:
```
h₁(key) = key % 10
h₂(key) = 7 - (key % 7)

Insert 23: h₁(23) = 3 → Table[3] = 23
Insert 13: h₁(13) = 3 → occupied!
           h₂(13) = 7 - (13%7) = 7 - 6 = 1
           (3 + 1*1) % 10 = 4 → Table[4] = 13
Insert 43: h₁(43) = 3 → occupied!
           h₂(43) = 7 - (43%7) = 7 - 1 = 6
           (3 + 1*6) % 10 = 9 → Table[9] = 43
```

**Advantages**:
- Best distribution
- Eliminates clustering
- Good for high load factor

**Disadvantages**:
- Complex implementation
- Slower (two hash computations)

---

### 4. Chaining (Separate Chaining)

**Idea**: Each hash slot contains a linked list

**Structure**:
```
Hash Table:
Index 0: NULL
Index 1: NULL
Index 2: NULL → 12 → NULL
Index 3: NULL → 23 → 13 → 43 → NULL
Index 4: NULL → 34 → NULL
...
Index 9: NULL
```

**Example**: Insert 23, 13, 43, 12, 34

```
1. Insert 23: h(23) = 3 → Table[3] = [23]
2. Insert 13: h(13) = 3 → Table[3] = [23 → 13]
3. Insert 43: h(43) = 3 → Table[3] = [23 → 13 → 43]
4. Insert 12: h(12) = 2 → Table[2] = [12]
5. Insert 34: h(34) = 4 → Table[4] = [34]

Final:
[NULL, NULL, 12↓, 23↓, 34↓, ...]
                  13↓
                  43↓
```

**Advantages**:
- Simple implementation
- No clustering
- Good for high load factor
- Easy deletion

**Disadvantages**:
- Extra memory for pointers
- Cache unfriendly (scattered memory)

---

## Comparison of Techniques

| Technique | Primary Clustering | Memory Overhead | Search | Insertion | Deletion |
|-----------|------------------|-----------------|--------|-----------|----------|
| Linear Probing | Yes | None | Slow | Slow | Hard |
| Quadratic Probing | Moderate | None | Moderate | Moderate | Hard |
| Double Hashing | No | None | Fast | Fast | Hard |
| **Chaining** | **No** | **Yes** | **Fast** | **Fast** | **Easy** |

---

## C Code: Linear Probing

```c
#include <stdio.h>
#define TABLE_SIZE 10
#define EMPTY -1
#define DELETED -2

int table[TABLE_SIZE];

// Initialize table
void initTable() {
    for (int i = 0; i < TABLE_SIZE; i++)
        table[i] = EMPTY;
}

// Hash function
int hash(int key) {
    return key % TABLE_SIZE;
}

// Insert using linear probing
int insert(int key) {
    int i = 0;
    while (i < TABLE_SIZE) {
        int index = (hash(key) + i) % TABLE_SIZE;
        
        if (table[index] == EMPTY || table[index] == DELETED) {
            table[index] = key;
            printf("Inserted %d at index %d\n", key, index);
            return index;
        }
        i++;
    }
    printf("Hash table is full!\n");
    return -1;
}

// Search using linear probing
int search(int key) {
    int i = 0;
    while (i < TABLE_SIZE) {
        int index = (hash(key) + i) % TABLE_SIZE;
        
        if (table[index] == key)
            return index;
        
        if (table[index] == EMPTY)
            return -1;
        
        i++;
    }
    return -1;
}

// Display table
void display() {
    printf("\nHash Table:\n");
    for (int i = 0; i < TABLE_SIZE; i++) {
        printf("Index %d: ", i);
        if (table[i] == EMPTY)
            printf("EMPTY\n");
        else if (table[i] == DELETED)
            printf("DELETED\n");
        else
            printf("%d\n", table[i]);
    }
}

int main() {
    printf("=== LINEAR PROBING ===\n\n");
    initTable();
    
    printf("--- Insertion ---\n");
    insert(23);
    insert(13);
    insert(43);
    insert(45);
    insert(35);
    
    display();
    
    printf("\n--- Search ---\n");
    int pos = search(13);
    if (pos != -1)
        printf("Found 13 at index %d\n", pos);
    
    pos = search(50);
    if (pos == -1)
        printf("50 not found\n");
    
    return 0;
}
```

---

## C Code: Chaining

```c
#include <stdio.h>
#include <stdlib.h>

#define TABLE_SIZE 5

struct Node {
    int data;
    struct Node *next;
};

struct Node *table[TABLE_SIZE];

void initTable() {
    for (int i = 0; i < TABLE_SIZE; i++)
        table[i] = NULL;
}

int hash(int key) {
    return key % TABLE_SIZE;
}

void insert(int key) {
    int index = hash(key);
    struct Node *new_node = (struct Node *)malloc(sizeof(struct Node));
    new_node->data = key;
    new_node->next = table[index];
    table[index] = new_node;
    printf("Inserted %d at index %d\n", key, index);
}

int search(int key) {
    int index = hash(key);
    struct Node *temp = table[index];
    
    while (temp != NULL) {
        if (temp->data == key)
            return index;
        temp = temp->next;
    }
    return -1;
}

void display() {
    printf("\nHash Table (Chaining):\n");
    for (int i = 0; i < TABLE_SIZE; i++) {
        printf("Index %d: ", i);
        struct Node *temp = table[i];
        while (temp != NULL) {
            printf("%d → ", temp->data);
            temp = temp->next;
        }
        printf("NULL\n");
    }
}

int main() {
    printf("=== CHAINING ===\n\n");
    initTable();
    
    insert(12);
    insert(22);
    insert(23);
    insert(13);
    
    display();
    
    return 0;
}
```

---

## Exam Answer Template

**Question**: What is collision? Explain resolution techniques.

**Answer**:

**Collision Definition**:
- Two or more keys hash to same index
- `h(k₁) = h(k₂)` but k₁ ≠ k₂

**Techniques** [Choose 2-3 for 5 marks]:

1. **Linear Probing**: Try next position, then next, etc.
   - Formula: `(h+i) % m`
   - Advantage: Simple
   - Disadvantage: Clustering

2. **Chaining**: Link list at each index
   - Advantage: Simple, great for high load
   - Disadvantage: Extra memory

3. **Double Hashing**: Use second hash for offset
   - Formula: `(h₁ + i*h₂) % m`
   - Advantage: Best distribution

---

## Key Points

✅ **Must Know**:
1. Definition of collision
2. At least 2 techniques with examples
3. Advantages/disadvantages
4. Load factor concept

❌ **Common Mistakes**:
- Confusing clustering concepts
- Wrong formulas
- Not mentioning load factor

⚡ **Load Factor = n/m** (keys/table size)
- Affects collision probability
- 0.75 load factor considered heavy

---

## Related Concepts

- Hash function design
- Load factor
- Open addressing vs separate chaining
- Universal hashing
