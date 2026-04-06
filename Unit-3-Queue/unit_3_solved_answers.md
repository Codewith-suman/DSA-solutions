# Unit 3 - Queue: SOLVED ANSWERS

---

## **Question 1: What are the limitations of Linear Queue? How does Circular Queue overcome them? [5 Marks]**

**Asked in**: 2074, 2076, 2079

### Answer:

#### **Linear Queue Issues**

```
Linear Queue (Array-based):

Initial:    [_, _, _, _, _]  (size = 5)

After:      [D, _, _, _, _]  (front=0, rear=0)
Dequeue

After:      [_, D, _, _, _]  (front=1, rear=1)
More ops
```

#### **Problem 1: Wasted Space After Dequeue**

```
State: [_, _, E, F, G]  (front=2, rear=4)

Even though space exists at [0,1]:
Can't add new elements! (rear = 4, at max)
Space is WASTED!

This is called "Queue Overflow" even with free space.
```

#### **Problem 2: Shifting Elements (Costly)**

```
Solution: Shift all elements left

Before:  [_, _, E, F, G]  (1000+ elements?)
After:   [E, F, G, _, _]  (shift = O(n) operation!)

Very expensive for large queues!
```

#### **Linear Queue Disadvantages**

❌ **Wasted space** after dequeues
❌ **Cannot reuse** positions at front
❌ **Expensive shifting** to reclaim space
❌ **Real memory not released**
❌ **Space inefficiency**

---

## **Question 2: Write Algorithms for Enqueue and Dequeue in Circular Queue [5 Marks]**

**Asked in**: Model, 2078, 2080, 2081

### Answer:

#### **Circular Queue Concept**

```
Linear:      [0, 1, 2, 3, 4]
              └─────────────┘

Circular:    [0, 1, 2, 3, 4]
              └──────┬──────┘
              (connects back)

rear = 4 → Next = 0 (wraps around!)
```

#### **Enqueue Operation**

**Pseudocode**:
```
Algorithm ENQUEUE(queue[], element)
    
    if isFull():
        print "Queue is full"
        return
    
    if isEmpty():
        front = 0
    
    rear = (rear + 1) % MAX_SIZE
    queue[rear] = element
    count = count + 1
    
    print "Enqueued " + element
```

**Step-by-Step Example**:

```
Initial:
queue = [_, _, _, _, _]  size = 5
front = -1, rear = -1, count = 0

Enqueue(10):
├─ rear = (−1 + 1) % 5 = 0
├─ queue[0] = 10
├─ count = 1
└─ Result: [10, _, _, _, _]

Enqueue(20):
├─ rear = (0 + 1) % 5 = 1
├─ queue[1] = 20
├─ count = 2
└─ Result: [10, 20, _, _, _]

Enqueue(30):
├─ Result: [10, 20, 30, _, _]

Enqueue(40):
├─ Result: [10, 20, 30, 40, _]

Enqueue(50):
├─ rear = (3 + 1) % 5 = 4
├─ Result: [10, 20, 30, 40, 50]
└─ Queue is FULL!

Next attempt:
Enqueue(60):
├─ isFull() returns true
└─ "Queue is full" error
```

#### **Dequeue Operation**

**Pseudocode**:
```
Algorithm DEQUEUE(queue[])
    
    if isEmpty():
        print "Queue is empty"
        return
    
    element = queue[front]
    queue[front] = NULL
    
    if isEmpty():  // Last element removed
        front = -1, rear = -1
    else:
        front = (front + 1) % MAX_SIZE
    
    count = count - 1
    
    return element
```

**Step-by-Step Example**:

```
From previous state:
queue = [10, 20, 30, 40, 50]
front = 0, rear = 4, count = 5

Dequeue():
├─ element = queue[0] = 10
├─ queue[0] = NULL
├─ front = (0 + 1) % 5 = 1
├─ count = 4
└─ Return: 10
   Result: [_, 20, 30, 40, 50]

Dequeue():
├─ element = 20
├─ front = (1 + 1) % 5 = 2
└─ Result: [_, _, 30, 40, 50]

Dequeue():
├─ Result: [_, _, _, 40, 50]

Dequeue():
├─ Result: [_, _, _, _, 50]

Dequeue():
├─ element = 50
├─ front = (4 + 1) % 5 = 0
├─ rear = -1 (last element)
└─ Result: [_, _, _, _, _]
   Now isEmpty = true

Dequeue():
├─ isEmpty() = true
└─ "Queue is empty"
```

#### **isFull() and isEmpty()**

```
Algorithm isEmpty()
    return front == -1 AND rear == -1

Algorithm isFull()
    return (rear + 1) % MAX_SIZE == front
    
    OR: return (count == MAX_SIZE)
```

---

## **Question 3: What is a Priority Queue? Explain applications. [5 Marks]**

**Asked in**: 2075, 2076, 2080

### Answer:

#### **Definition**

A **Priority Queue** is a data structure where:
- Each element has an associated **priority**
- Higher priority elements dequeued **first**
- Not strictly FIFO like regular queue

```
Regular Queue:  [A, B, C, D]  → Dequeue order: A, B, C, D
Priority Queue: A(3), B(1), C(5), D(2)
                → Dequeue order: C(5), A(3), D(2), B(1)
                  (sorted by priority)
```

#### **Implementation Methods**

| Method | Insert | Delete | Best For |
|--------|--------|--------|----------|
| Array (sorted) | O(n) | O(1) | Small queues |
| Linked List | O(n) | O(1) | Dynamic sizes |
| **Heap** | **O(log n)** | **O(log n)** | **Best! Large data** |
| Binary Tree | O(log n) | O(log n) | Balanced trees |

#### **Applications of Priority Queue**

**1. CPU Scheduling**
```
Processes with different priorities:
- System process (high priority)
- User application (medium)
- Background task (low)

Priority Queue ensures:
✓ Important processes run first
✓ Fair CPU distribution
```

**2. Hospital Emergency Room**
```
Patients queue:
- Critical condition: Priority 1 (highest)
- Urgent: Priority 2
- Non-urgent: Priority 3 (lowest)

Treated in order of priority, not arrival
```

**3. Network Routers**
```
Packets with priorities:
- Video streaming: High priority
- Email: Medium priority
- File download: Low priority

High-priority packets sent first
```

**4. Dijkstra's Algorithm**
```
Finds shortest path:
- Uses priority queue to select nearest unvisited vertex
- O((V+E)log V) complexity thanks to heap

Without priority queue: O(V²)
```

**5. Huffman Coding (Compression)**
```
Build optimal binary tree for data compression:
- Repeatedly combine lowest frequency nodes
- Priority queue maintains sorted order

Used in MP3, JPEG compression!
```

**6. Print Queue Management**
```
Print jobs:
- Urgent document: Priority 10
- Regular: Priority 5
- Background: Priority 1

Urgent documents printed first
```

**7. Bandwidth Allocation**
```
Network traffic prioritization:
- Video call: High (must be smooth)
- Web browsing: Medium
- Torrents: Low

Bandwidth allocated by priority
```

#### **Max Priority vs Min Priority**

```
Max Priority Queue:
- Largest priority dequeued first
- Used for: Scheduling urgent tasks
- Example: {A:10, B:5, C:8}
  Dequeue: A(10), C(8), B(5)

Min Priority Queue:
- Smallest priority dequeued first
- Used for: Dijkstra, minimum cost
- Example: {A:10, B:5, C:8}
  Dequeue: B(5), C(8), A(10)
```

#### **C Pseudocode**

```c
struct PriorityQueueElement {
    int data;
    int priority;  // Higher number = higher priority
};

Insert(queue, element):
    Find correct position (sorted by priority)
    Insert element
    
Delete(queue):
    Remove element with highest priority
    Return it
    
Peek(queue):
    Return highest priority element (don't remove)
```

#### **Heap Implementation** (Most Efficient)

```c
// Using max heap
PriorityQueue queue;

Enqueue(element, priority):
    Insert into max heap with (priority, element)
    // Automatically maintains heap property
    // O(log n)

Dequeue():
    Remove root (highest priority)
    Reheapify
    // O(log n)
```

#### **Real Performance Impact**

```
Task: Process 1 million jobs with priorities

Array-based:     1,000,000 × O(n) = 10^12 operations! ❌
Heap-based:      1,000,000 × O(log n) ≈ 20 million ops ✓
                 Speedup: 50,000× faster!
```

---

## **Question 4: Write short notes on Dequeue [2.5 Marks]**

**Asked in**: 2081

### Answer:

#### **What is Dequeue?**

**Dequeue** (Double-Ended Queue) is a queue where:
- Elements can be added/removed from **BOTH ends**
- Combines properties of Stack **AND** Queue
- Also called **Double Stack Queue**

```
Dequeue:
  Left end:    [front]
  Right end:   [   rear]

     ←Add/Remove→ [A, B, C] ←Add/Remove→

Both ends can add AND remove!
```

#### **Operations**

```
addFront(X)     - Add at front
addRear(X)      - Add at rear
removeFront()   - Remove from front
removeRear()    - Remove from rear
peekFront()     - View front
peekRear()      - View rear
```

#### **Real Example**

```
Initial: [empty]

addRear(10):     [10]
addRear(20):     [10, 20]
addFront(5):     [5, 10, 20]
addRear(30):     [5, 10, 20, 30]

removeFront():   [10, 20, 30]    (removed 5)
removeRear():    [10, 20]        (removed 30)
```

#### **Applications**

- **Undo/Redo** in text editors
- **Browser history** (back/forward)
- **Palindrome checking** (compare both ends)
- **Multiprocessor scheduling**
- **Stealing work** in parallel systems

#### **Palindrome Check Using Dequeue**

```
Check if "RACECAR" is palindrome:

Add all to dequeue: [R, A, C, E, C, A, R]

Compare removals:
removeFront() → R
removeRear()  → R   ✓ Match!

removeFront() → A
removeRear()  → A   ✓ Match!

removeFront() → C
removeRear()  → C   ✓ Match!

removeFront() → E
(last element)      ✓ Palindrome!
```

#### **Dequeue Variations**

1. **Input-restricted dequeue**: Can add both ends, remove one end only
2. **Output-restricted dequeue**: Can remove both ends, add one end only
3. **Full dequeue**: Can add/remove both ends (most general)

---

## **Quick Summary - Unit 3**

| Topic | Key Points |
|-------|-----------|
| **Linear Queue Issues** | Wasted space after dequeues, can't reuse positions |
| **Circular Queue Fix** | Use modulo (%) for wraparound, reuses space |
| **Circular Enqueue** | `rear = (rear + 1) % MAX` |
| **Circular Dequeue** | `front = (front + 1) % MAX`|
| **isFull()** | `(rear + 1) % MAX == front` |
| **Priority Queue** | Elements have priority, dequeue by priority not FIFO |
| **Priority Apps** | CPU scheduling, hospital ER, Dijkstra, compression |
| **Best Implementation** | Heap - O(log n) for insert/delete |
| **Dequeue** | Double-ended queue, add/remove from both ends |
| **Dequeue Apps** | Undo/Redo, palindrome check |

