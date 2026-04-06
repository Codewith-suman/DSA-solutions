# Postfix Expression Evaluation (Stack)

**Marks**: 5 marks  
**Difficulty**: ⭐⭐ Easy-Medium

---

## Problem Statement

Evaluate a **Postfix expression** using a **Stack**.

**Example**: `2 3 4 * +` = 2 + (3 * 4) = 14

---

## Concept Overview

### Why Postfix?
- No parentheses needed
- Unambiguous evaluation
- Perfect for stack-based processing
- Used in calculators and compilers

### Stack-Based Evaluation
- Operands → PUSH to stack
- Operators → POP two operands, compute, PUSH result
- Final result = top of stack

---

## Algorithm

```
Algorithm EVALUATE_POSTFIX(postfix_expr)
    
    STACK = empty stack
    
    for each element in postfix_expr:
        
        if element is operand:
            STACK.push(element)
        
        else:  // element is operator
            b = STACK.pop()
            a = STACK.pop()
            result = a operator b
            STACK.push(result)
    
    return STACK.pop()  // Final result
```

---

## Trace Example: `2 3 4 * +`

| Step | Element | Stack | Action |
|------|---------|-------|--------|
| 1 | 2 | [2] | Push 2 |
| 2 | 3 | [2,3] | Push 3 |
| 3 | 4 | [2,3,4] | Push 4 |
| 4 | * | [2,12] | Pop 4,3 → 3*4=12 → Push 12 |
| 5 | + | [14] | Pop 12,2 → 2+12=14 → Push 14 |
| **Final** | | **[14]** | **Result = 14** ✓ |

---

## C Code

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>

#define MAX 100

int stack[MAX];
int top = -1;

void push(int val) {
    stack[++top] = val;
}

int pop() {
    return stack[top--];
}

int evaluate(char *postfix) {
    printf("Evaluating: %s\n", postfix);
    printf("\nSteps:\n");
    
    for (int i = 0; postfix[i] != '\0'; i++) {
        char c = postfix[i];
        
        if (c == ' ') continue;
        
        if (isdigit(c)) {
            push(c - '0');
            printf("Push %d → Stack: [", c - '0');
            for (int j = 0; j <= top; j++)
                printf("%d ", stack[j]);
            printf("]\n");
        }
        else {
            int b = pop();
            int a = pop();
            int result;
            
            switch (c) {
                case '+': result = a + b; break;
                case '-': result = a - b; break;
                case '*': result = a * b; break;
                case '/': result = a / b; break;
                case '^': result = 1;
                    for (int k = 0; k < b; k++) result *= a;
                    break;
            }
            
            printf("Pop %d,%d → %d %c %d = %d → Push\n",
                   a, b, a, c, b, result);
            
            push(result);
            printf("Stack: [");
            for (int j = 0; j <= top; j++)
                printf("%d ", stack[j]);
            printf("]\n");
        }
    }
    
    return stack[top];
}

int main() {
    printf("=== POSTFIX EVALUATION ===\n\n");
    
    // Test cases
    printf("Test 1:\n");
    int result = evaluate("2 3 4 * +");
    printf("\nResult: %d\n\n", result);
    
    printf("Test 2:\n");
    top = -1;
    result = evaluate("5 3 2 * -");
    printf("\nResult: %d\n\n", result);
    
    printf("Test 3:\n");
    top = -1;
    result = evaluate("10 2 / 3 +");
    printf("\nResult: %d\n", result);
    
    return 0;
}
```

---

## More Examples

```
"6 2 /" → [6, 2] → Pop 6,2 → 6/2=3 → Result: 3

"3 4 + 2 *" → [3,4] → + → [7] → [7,2] → * → Result: 14

"5 3 2 * -" → [5] → [5,3] → [5,3,2] → * → [5,6] → - → Result: -1
```

---

## Key Points

✅ **Remember**:
1. **Operands** → push
2. **Operators** → pop 2, compute, push result
3. **Order matters**: a-b when pop a then b
4. **Final answer** at stack top

---

## Related Topics

- [Infix to Postfix](./infix_to_postfix_conversion.md)
- [Stack ADT](./README.md)
