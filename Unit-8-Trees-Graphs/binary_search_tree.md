# Binary Search Tree (BST) Basics

**Marks**: 10 marks (Insert/Delete)  
**Difficulty**: ⭐⭐⭐⭐ High

---

## Problem Statement

What is a **Binary Search Tree (BST)**? Write algorithms for **Insertion** and **Deletion**.

---

## Concept Overview

### BST Properties
1. **Left subtree** < Node
2. **Right subtree** > Node
3. **No duplicates** (or handle separately)
4. **In-order traversal** gives sorted sequence

### Example
```
        50
       /  \
      30   70
     / \   / \
    20 40 60 80
```

---

## Node Structure

```c
struct Node {
    int data;
    struct Node *left;
    struct Node *right;
};
```

---

## Insertion Algorithm

### Pseudocode

```
Algorithm INSERT(root, key)
    
    if root == NULL:
        Create new node
        return new node
    
    if key < root.data:
        root.left = INSERT(root.left, key)
    
    else if key > root.data:
        root.right = INSERT(root.right, key)
    
    else:  // key == root.data
        return root  // Duplicate handling
    
    return root
```

### Example: Insert 40, 20, 50, 10, 30

```
Insert 40:        40
               /    \
              NULL  NULL

Insert 20:        40
               /    \
              20    NULL

Insert 50:        40
               /    \
              20    50

Insert 10:        40
               /    \
              20    50
             /
            10

Insert 30:        40
               /    \
              20    50
             / \
            10 30
```

---

## Deletion Algorithm

### Cases

**Case 1: Node is Leaf**
- Simply remove

**Case 2: Node has One Child**
- Replace with child

**Case 3: Node has Two Children**
- **Option A**: Replace with inorder successor (smallest in right subtree)
- **Option B**: Replace with inorder predecessor (largest in left subtree)

### Pseudocode

```
Algorithm DELETE(root, key)
    
    if root == NULL:
        return NULL
    
    if key < root.data:
        root.left = DELETE(root.left, key)
    
    else if key > root.data:
        root.right = DELETE(root.right, key)
    
    else:  // Found node to delete
        
        // Case 1: No children (leaf)
        if root.left == NULL and root.right == NULL:
            return NULL
        
        // Case 2: One child
        if root.left == NULL:
            return root.right
        if root.right == NULL:
            return root.left
        
        // Case 3: Two children
        // Find inorder successor
        successor = MIN(root.right)
        root.data = successor.data
        root.right = DELETE(root.right, successor.data)
    
    return root
```

---

## C Code

```c
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *left;
    struct Node *right;
};

struct Node *newNode(int data) {
    struct Node *node = (struct Node *)malloc(sizeof(struct Node));
    node->data = data;
    node->left = NULL;
    node->right = NULL;
    return node;
}

struct Node *insert(struct Node *root, int key) {
    if (root == NULL) {
        printf("Inserted %d\n", key);
        return newNode(key);
    }
    
    if (key < root->data) {
        root->left = insert(root->left, key);
    } else if (key > root->data) {
        root->right = insert(root->right, key);
    } else {
        printf("Duplicate %d ignored\n", key);
    }
    
    return root;
}

struct Node *findMin(struct Node *root) {
    while (root->left != NULL)
        root = root->left;
    return root;
}

struct Node *delete(struct Node *root, int key) {
    if (root == NULL)
        return NULL;
    
    if (key < root->data) {
        root->left = delete(root->left, key);
    } else if (key > root->data) {
        root->right = delete(root->right, key);
    } else {
        // Case 1: Leaf
        if (root->left == NULL && root->right == NULL) {
            free(root);
            printf("Deleted leaf %d\n", key);
            return NULL;
        }
        
        // Case 2: One child
        if (root->left == NULL) {
            struct Node *temp = root->right;
            free(root);
            printf("Deleted node %d with right child\n", key);
            return temp;
        }
        if (root->right == NULL) {
            struct Node *temp = root->left;
            free(root);
            printf("Deleted node %d with left child\n", key);
            return temp;
        }
        
        // Case 3: Two children
        struct Node *successor = findMin(root->right);
        root->data = successor->data;
        root->right = delete(root->right, successor->data);
        printf("Deleted node %d, replaced with successor\n", key);
    }
    
    return root;
}

void inorder(struct Node *root) {
    if (root == NULL) return;
    
    inorder(root->left);
    printf("%d ", root->data);
    inorder(root->right);
}

int main() {
    printf("=== BINARY SEARCH TREE ===\n\n");
    
    struct Node *root = NULL;
    
    printf("Inserting: 50, 30, 70, 20, 40, 60, 80\n");
    root = insert(root, 50);
    root = insert(root, 30);
    root = insert(root, 70);
    root = insert(root, 20);
    root = insert(root, 40);
    root = insert(root, 60);
    root = insert(root, 80);
    
    printf("\nInorder traversal: ");
    inorder(root);
    printf("\n");
    
    printf("\nDeleting 20 (leaf)...\n");
    root = delete(root, 20);
    
    printf("Inorder: ");
    inorder(root);
    printf("\n");
    
    printf("\nDeleting 30 (one child)...\n");
    root = delete(root, 30);
    
    printf("Inorder: ");
    inorder(root);
    printf("\n");
    
    printf("\nDeleting 50 (two children)...\n");
    root = delete(root, 50);
    
    printf("Inorder: ");
    inorder(root);
    printf("\n");
    
    return 0;
}
```

---

## Complexity Analysis

### Search
- **Balanced**: O(log n)
- **Skewed**: O(n)
- **Average**: O(log n)

### Insert
- Same as search + O(1) for insertion

### Delete
- Same as search + manipulation

---

## BST Traversals

### Inorder: Left → Root → Right
```
50
30, 20, 40, 50, 70, 60, 80  (sorted!)
```

### Preorder: Root → Left → Right
```
50, 30, 20, 40, 70, 60, 80
```

### Postorder: Left → Right → Root
```
20, 40, 30, 60, 80, 70, 50
```

---

## When Balanced

```
Balanced:           Skewed (like linked list):
      50                    10
     /  \                     \
    30  70                     20
   / \                           \
  20 40                           30
                                    \
  O(log n)                         O(n)
```

---

## Key Points

✅ **Remember**:
1. **Left < Node < Right**
2. **Inorder traversal** = sorted sequence
3. **Insertion**: Recursive placement
4. **Deletion**: Handle 3 cases
5. **Complexity**: O(log n) if balanced

❌ **Mistakes**:
- Not checking all conditions
- Wrong deletion case handling
- Not finding successor correctly

⚡ **Balanced BST Variants**:
- **AVL Tree**: Balance factor ≠ 2
- **Red-Black Tree**: Color constraints
- **B-Tree**: Multi-way tree

---

## Related Problems

- [AVL Trees](./avl_tree.md)
- [Tree Traversals](./tree_traversals.md)
