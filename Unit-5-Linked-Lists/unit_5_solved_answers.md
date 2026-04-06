# Unit 5 - Linked Lists: SOLVED ANSWERS

---

## **Question 1: Write algorithms to INSERT and DELETE in Singly Linked List [10 Marks]**

**Asked in**: 2074, 2079

### Answer:

#### **Node Structure**

```c
struct Node {
    int data;
    struct Node *next;
};
```

---

## **INSERTION Algorithms**

### **1. Insert at Beginning**

**Pseudocode**:
```
Algorithm INSERT_AT_BEGINNING(head, value)
    
    Create new_node with data = value
    new_node.next = head
    head = new_node
    return head
```

**C Code**:
```c
struct Node *insertAtBeginning(struct Node *head, int value) {
    struct Node *new_node = (struct Node *)malloc(sizeof(struct Node));
    new_node->data = value;
    new_node->next = head;
    return new_node;  // New head
}
```

**Visual Example**:
```
Before: 20 → 30 → 40 → NULL
        ↑
       head

Insert 10 at beginning:

Step 1: Create node: [10 | ?]
Step 2: Point to head: [10 | @20]
Step 3: Make new head: 10 → 20 → 30 → 40 → NULL

After: 10 → 20 → 30 → 40 → NULL
       ↑
      head (new)
```

**Time**: O(1)
**Space**: O(1)

---

### **2. Insert at End**

**Pseudocode**:
```
Algorithm INSERT_AT_END(head, value)
    
    Create new_node with data = value
    new_node.next = NULL
    
    if head == NULL:
        return new_node
    
    current = head
    while current.next != NULL:
        current = current.next
    
    current.next = new_node
    return head
```

**C Code**:
```c
struct Node *insertAtEnd(struct Node *head, int value) {
    struct Node *new_node = (struct Node *)malloc(sizeof(struct Node));
    new_node->data = value;
    new_node->next = NULL;
    
    if (head == NULL)
        return new_node;
    
    struct Node *current = head;
    while (current->next != NULL) {
        current = current->next;
    }
    
    current->next = new_node;
    return head;
}
```

**Visual Example**:
```
Before: 10 → 20 → 30 → NULL
                   ↑
                current

Insert 40 at end:

Step 1: Create node: [40 | NULL]
Step 2: Find last node (30)
Step 3: Point 30→next to 40: 30 → 40

After: 10 → 20 → 30 → 40 → NULL
```

**Time**: O(n) - must traverse to end
**Space**: O(1)

---

### **3. Insert at Specified Position**

**Pseudocode**:
```
Algorithm INSERT_AT_POSITION(head, value, position)
    
    if position == 1:
        return INSERT_AT_BEGINNING(head, value)
    
    current = head
    count = 1
    
    while count < position - 1 AND current != NULL:
        current = current.next
        count = count + 1
    
    if current == NULL:
        print "Position out of range"
        return head
    
    new_node = create new node with value
    new_node.next = current.next
    current.next = new_node
    return head
```

**C Code**:
```c
struct Node *insertAtPosition(struct Node *head, int value, int position) {
    if (position == 1)
        return insertAtBeginning(head, value);
    
    struct Node *current = head;
    int count = 1;
    
    while (count < position - 1 && current != NULL) {
        current = current.next;
        count++;
    }
    
    if (current == NULL) {
        printf("Position out of range\n");
        return head;
    }
    
    struct Node *new_node = (struct Node *)malloc(sizeof(struct Node));
    new_node->data = value;
    new_node->next = current->next;
    current->next = new_node;
    return head;
}
```

**Visual Example**:
```
Before: 10 → 20 → 40 → NULL

Insert 30 at position 3:

Step 1: Find position 2 node (value=20)
Step 2: Create new node [30]
Step 3: Insert: 20 → 30 → 40

After: 10 → 20 → 30 → 40 → NULL
            ↑pos 2 ↑new
```

**Time**: O(n) - traverse to position
**Space**: O(1)

---

## **DELETION Algorithms**

### **1. Delete from Beginning**

**Pseudocode**:
```
Algorithm DELETE_AT_BEGINNING(head)
    
    if head == NULL:
        print "List is empty"
        return NULL
    
    temp = head
    head = head.next
    free(temp)
    return head
```

**C Code**:
```c
struct Node *deleteFromBeginning(struct Node *head) {
    if (head == NULL) {
        printf("List is empty\n");
        return NULL;
    }
    
    struct Node *temp = head;
    head = head->next;
    free(temp);
    return head;
}
```

**Visual Example**:
```
Before: 10 → 20 → 30 → NULL
        ↑
       head (delete)

Step 1: Save head: temp = 10
Step 2: Move head forward: head = 20
Step 3: Free temp: delete 10

After: 20 → 30 → NULL
       ↑
      head
```

**Time**: O(1)
**Space**: O(1)

---

### **2. Delete from End**

**Pseudocode**:
```
Algorithm DELETE_AT_END(head)
    
    if head == NULL:
        return NULL
    
    if head.next == NULL:
        free(head)
        return NULL
    
    current = head
    while current.next.next != NULL:
        current = current.next
    
    free(current.next)
    current.next = NULL
    return head
```

**C Code**:
```c
struct Node *deleteFromEnd(struct Node *head) {
    if (head == NULL)
        return NULL;
    
    if (head->next == NULL) {
        free(head);
        return NULL;
    }
    
    struct Node *current = head;
    while (current->next->next != NULL) {
        current = current->next;
    }
    
    free(current->next);
    current->next = NULL;
    return head;
}
```

**Visual Example**:
```
Before: 10 → 20 → 30 → NULL
                   ↑
              (delete)

Step 1: Find second-to-last (20)
Step 2: Point it to NULL: 20 → NULL
Step 3: Free 30

After: 10 → 20 → NULL
```

**Time**: O(n) - must traverse to end
**Space**: O(1)

---

### **3. Delete from Specified Position**

**Pseudocode**:
```
Algorithm DELETE_AT_POSITION(head, position)
    
    if position == 1:
        return DELETE_AT_BEGINNING(head)
    
    current = head
    count = 1
    
    while count < position - 1 AND current != NULL:
        current = current.next
        count = count + 1
    
    if current == NULL OR current.next == NULL:
        print "Position out of range"
        return head
    
    temp = current.next
    current.next = temp.next
    free(temp)
    return head
```

**C Code**:
```c
struct Node *deleteAtPosition(struct Node *head, int position) {
    if (position == 1)
        return deleteFromBeginning(head);
    
    struct Node *current = head;
    int count = 1;
    
    while (count < position - 1 && current != NULL) {
        current = current->next;
        count++;
    }
    
    if (current == NULL || current->next == NULL) {
        printf("Position out of range\n");
        return head;
    }
    
    struct Node *temp = current->next;
    current->next = temp->next;
    free(temp);
    return head;
}
```

**Visual Example**:
```
Before: 10 → 20 → 30 → 40 → NULL

Delete at position 3 (value 30):

Step 1: Find position 2 (20)
Step 2: Point 20→next to 40: 20 → 40
Step 3: Free 30

After: 10 → 20 → 40 → NULL
```

**Time**: O(n)
**Space**: O(1)

---

## **Question 2: Write algorithm to INSERT and DELETE in Doubly Linked List [10 Marks]**

**Asked in**: 2076, 2081

### Answer:

#### **Node Structure**

```c
struct Node {
    int data;
    struct Node *next;
    struct Node *prev;  // Previous pointer!
};
```

---

## **INSERTION in Doubly Linked List**

### **Insert at Beginning**

**Pseudocode**:
```
Algorithm DLL_INSERT_AT_BEGINNING(head, value)
    
    new_node = create node with value
    new_node.next = head
    new_node.prev = NULL
    
    if head != NULL:
        head.prev = new_node
    
    head = new_node
    return head
```

**C Code**:
```c
struct Node *dllInsertBeginning(struct Node *head, int value) {
    struct Node *new_node = (struct Node *)malloc(sizeof(struct Node));
    new_node->data = value;
    new_node->next = head;
    new_node->prev = NULL;
    
    if (head != NULL)
        head->prev = new_node;
    
    return new_node;
}
```

**Visual Example**:
```
Before: NULL ← 20 ↔ 30 ↔ NULL

Insert 10 at beginning:

Step 1: Create: [10 | ?, NULL]
Step 2: Link: 10 ↔ 20
Step 3: Set prev: [10 | @20, NULL]
                  [20 | ?, @10]

After: NULL ← 10 ↔ 20 ↔ 30 ← NULL
```

**Time**: O(1)

---

### **Insert at End**

**Pseudocode**:
```
Algorithm DLL_INSERT_AT_END(head, value)
    
    new_node = create node with value
    new_node.next = NULL
    
    if head == NULL:
        new_node.prev = NULL
        return new_node
    
    current = head
    while current.next != NULL:
        current = current.next
    
    new_node.prev = current
    current.next = new_node
    return head
```

**Time**: O(n)

---

### **Insert at Position**

```c
struct Node *dllInsertAtPosition(struct Node *head, int value, int position) {
    if (position == 1)
        return dllInsertBeginning(head, value);
    
    struct Node *current = head;
    int count = 1;
    
    while (count < position - 1 && current != NULL) {
        current = current->next;
        count++;
    }
    
    if (current == NULL) return head;
    
    struct Node *new_node = (struct Node *)malloc(sizeof(struct Node));
    new_node->data = value;
    new_node->next = current->next;
    new_node->prev = current;
    
    if (current->next != NULL)
        current->next->prev = new_node;
    
    current->next = new_node;
    return head;
}
```

**Visual**:
```
Before: NULL ← 10 ↔ 20 ↔ 30 ← NULL

Insert 25 at position 3:

NULL ← 10 ↔ 20 ↔ 25 ↔ 30 ← NULL
            ↑    prev  next   ↑
            └────new───────┘
```

---

## **DELETION in Doubly Linked List**

### **Delete from Beginning**

```c
struct Node *dllDeleteBeginning(struct Node *head) {
    if (head == NULL)
        return NULL;
    
    struct Node *temp = head;
    
    if (head->next != NULL)
        head->next->prev = NULL;
    
    head = head->next;
    free(temp);
    return head;
}
```

**Visual**:
```
Before: NULL ← 10 ↔ 20 ↔ 30 ← NULL
             ↑delete

Step: Remove prev link from 20, move head

After: NULL ← 20 ↔ 30 ← NULL
```

---

### **Delete from End**

```c
struct Node *dllDeleteEnd(struct Node *head) {
    if (head == NULL)
        return NULL;
    
    if (head->next == NULL) {
        free(head);
        return NULL;
    }
    
    struct Node *current = head;
    while (current->next != NULL) {
        current = current->next;
    }
    
    current->prev->next = NULL;
    free(current);
    return head;
}
```

---

### **Delete at Position**

```c
struct Node *dllDeleteAtPosition(struct Node *head, int position) {
    if (position == 1)
        return dllDeleteBeginning(head);
    
    struct Node *current = head;
    int count = 1;
    
    while (count < position && current != NULL) {
        current = current->next;
        count++;
    }
    
    if (current == NULL) return head;
    
    if (current->prev != NULL)
        current->prev->next = current->next;
    
    if (current->next != NULL)
        current->next->prev = current->prev;
    
    free(current);
    return head;
}
```

---

## **Question 3: Explain Circular Linked List [5 Marks]**

**Asked in**: 2076

### Answer:

#### **What is Circular Linked List?**

A linked list where the **last node points back to the first node** instead of NULL.

```
Singly:     10 → 20 → 30 → NULL

Circular:   10 → 20 → 30
             ↑         │
             └─────────┘
          (last points to first!)
```

#### **Node Structure**
```c
struct Node {
    int data;
    struct Node *next;  // Points to next, or back to head if last
};
```

#### **Characteristics**

✅ No NULL pointer (always has next)
✅ Head and tail indistinguishable
✅ Can traverse infinitely
✅ Must have condition to stop

#### **Traversal**

```c
void displayCircular(struct Node *head) {
    if (head == NULL) return;
    
    struct Node *current = head;
    
    do {
        printf("%d → ", current->data);
        current = current->next;
    } while (current != head);  // Stop when back at head!
    
    printf("(back to head)\n");
}

// Example:
// Display: 10 → 20 → 30 → (back to head)
```

#### **Operations**

**Insert at Beginning**:
```c
struct Node *insertCircularBeginning(struct Node *head, int value) {
    struct Node *new_node = (struct Node *)malloc(sizeof(struct Node));
    new_node->data = value;
    
    if (head == NULL) {
        new_node->next = new_node;  // Points to itself!
        return new_node;
    }
    
    // Find last node
    struct Node *current = head;
    while (current->next != head)  // Stop at last node
        current = current->next;
    
    new_node->next = head;
    current->next = new_node;
    return new_node;
}
```

#### **Real Applications**

1. **Round-Robin Scheduling**
```
CPU scheduling: Processes in circular queue
Process 1 → Process 2 → Process 3
  ↑                        │
  └────────────────────────┘

Take from head, do work, put back
(circular ensures fairness)
```

2. **Music Playlists**
```
Song 1 → Song 2 → Song 3 → Song 4
  ↑                           │
  └───────────────────────────┘

After last song, repeat!
```

3. **Carousel/Rotations**
```
Display items in rotation
Always have next item available
```

4. **Game Problems**
```
Josephus problem: people in circle
```

#### **Advantages vs Disadvantages**

| Advantage | Disadvantage |
|-----------|---|
| No NULL checking needed | More complex deletion |
| Suitable for cyclical data | Can cause infinite loops |
| CPU scheduling friendly | Extra pointer maintenance |
| No sentinel needed | Harder to traverse once |

---

## **Comparison Table: Singly vs Doubly vs Circular**

| Feature | Singly | Doubly | Circular |
|---------|--------|--------|----------|
| **Storage** | data, next | data, next, prev | data, next |
| **Traverse** | Forward only | Both ways | Circular |
| **Insert** | O(n) | O(n) | O(n) |
| **Delete** | O(n) | O(n) | O(n) |
| **Space** | Less | More (prev) | Less |
| **Access previous** | No | Yes | Must loop |
| **Last node** | NULL | Points back | Points to head |
| **Use case** | General | Reversible, undo | Round-robin |

---

## **Quick Summary** - Unit 5

| Operation | Time | Notes |
|-----------|------|-------|
| **Insert at beginning** | O(1) | Fastest |
| **Insert at end** | O(n) | Must find end |
| **Insert at position** | O(n) | Search + insert |
| **Delete at beginning** | O(1) | Fastest |
| **Delete at end** | O(n) | Single linked, O(1) in double |
| **Delete at position** | O(n) | Search + delete |
| **Circular traversal** | Check != head | Avoid infinite loop! |

