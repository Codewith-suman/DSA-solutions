# Infix to Postfix Conversion (Stack)

**Questions Asked**: 4 times (Model, 2075, 2078, 2080)  
**Marks**: 10 marks  
**Difficulty**: ⭐⭐⭐ Medium-High

---

## Problem Statement

Write an algorithm to convert an **Infix expression** to a **Postfix expression** and trace it with an example.

**Example**: `2 + 3 * 4` → `2 3 4 * +`

---

## Concept Overview

### Infix Notation
- Operator is between operands: `A + B * C`
- Requires parentheses for precedence clarity
- Natural way for humans to read

### Postfix Notation (Reverse Polish Notation)
- Operator comes after operands: `A B C * +`
- No parentheses needed
- Easy for computers to evaluate
- Stack-based evaluation

### Operator Precedence
```
1. Exponentiation (^)    - Highest
2. Multiplication (*), Division (/)
3. Addition (+), Subtraction (-) - Lowest

Associativity: Left to Right (except ^)
```

---

## Algorithm

### Steps:
1. Create an **empty stack** for operators
2. Create an **output string** for postfix expression
3. Scan the infix expression **left to right**

**For each character**:

**If operand** (number/variable):
- Add directly to output

**If '('**: 
- Push to stack

**If ')'**:
- Pop from stack to output until '(' is found
- Don't include '(' in output

**If operator**:
- While stack not empty AND top has **higher/equal precedence**:
  - Pop to output
- Push current operator to stack

**At end**:
- Pop all remaining operators to output

---

## Pseudocode

```
Algorithm INFIX_TO_POSTFIX(infix)
    STACK = empty stack
    POSTFIX = ""
    
    for each character CHAR in infix (left to right):
        
        if CHAR is operand:
            POSTFIX = POSTFIX + CHAR
        
        else if CHAR == '(':
            STACK.push(CHAR)
        
        else if CHAR == ')':
            while STACK is not empty AND top ≠ '(':
                POSTFIX = POSTFIX + STACK.pop()
            STACK.pop()  // Remove '('
        
        else if CHAR is operator:
            while STACK is not empty AND 
                  PRECEDENCE(top) >= PRECEDENCE(CHAR):
                POSTFIX = POSTFIX + STACK.pop()
            STACK.push(CHAR)
    
    // Pop remaining operators
    while STACK is not empty:
        POSTFIX = POSTFIX + STACK.pop()
    
    return POSTFIX
```

---

## C Code Implementation

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MAX 100

// Function to return precedence
int precedence(char c) {
    if (c == '^')
        return 3;
    else if (c == '*' || c == '/')
        return 2;
    else if (c == '+' || c == '-')
        return 1;
    return 0;
}

// Function to check if operator
int isOperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/' || c == '^');
}

// Infix to Postfix Conversion
void infixToPostfix(char *infix) {
    char stack[MAX];
    int top = -1;
    char postfix[MAX];
    int postfixIndex = 0;
    
    printf("Infix Expression: %s\n", infix);
    printf("\n--- Conversion Process ---\n");
    
    for (int i = 0; infix[i] != '\0'; i++) {
        char c = infix[i];
        
        // Skip spaces
        if (c == ' ')
            continue;
        
        // If operand, add to output
        if (isalnum(c)) {
            postfix[postfixIndex++] = c;
            printf("'%c' is operand → Output: %c\n", c, c);
        }
        // If '(', push to stack
        else if (c == '(') {
            stack[++top] = c;
            printf("'(' → Push to stack. Stack: [");
            for(int j=0; j<=top; j++) printf("%c ", stack[j]);
            printf("]\n");
        }
        // If ')', pop until '('
        else if (c == ')') {
            printf("')' → Pop until '(':\n");
            while (top >= 0 && stack[top] != '(') {
                postfix[postfixIndex++] = stack[top--];
                printf("   Pop '%c' to output\n", stack[top+1]);
            }
            top--; // Remove '('
            printf("   Stack: [");
            for(int j=0; j<=top; j++) printf("%c ", stack[j]);
            printf("]\n");
        }
        // If operator
        else if (isOperator(c)) {
            printf("'%c' is operator (prec=%d) → ", c, precedence(c));
            
            // Pop operators with higher/equal precedence
            while (top >= 0 && isOperator(stack[top]) && 
                   precedence(stack[top]) >= precedence(c)) {
                postfix[postfixIndex++] = stack[top];
                printf("Pop '%c' ", stack[top]);
                top--;
            }
            
            stack[++top] = c;
            printf("Push '%c'\n", c);
        }
    }
    
    // Pop all remaining operators
    printf("\nPopping remaining operators:\n");
    while (top >= 0) {
        postfix[postfixIndex++] = stack[top];
        printf("Pop '%c' to output\n", stack[top--]);
    }
    
    postfix[postfixIndex] = '\0';
    
    printf("\n--- Result ---\n");
    printf("Postfix Expression: %s\n", postfix);
}

int main() {
    char infix[MAX];
    
    printf("=== INFIX TO POSTFIX CONVERSION ===\n\n");
    
    // Test Case 1
    strcpy(infix, "2+3*4");
    infixToPostfix(infix);
    
    printf("\n\n");
    
    // Test Case 2
    strcpy(infix, "(2+3)*4");
    infixToPostfix(infix);
    
    printf("\n\n");
    
    // Test Case 3
    strcpy(infix, "A+B*C-D/E");
    infixToPostfix(infix);
    
    return 0;
}
```

---

## Trace Example 1: `2 + 3 * 4`

| Step | Input | Output | Stack | Action |
|------|-------|--------|-------|--------|
| 1 | 2 | 2 | | Operand → Output |
| 2 | + | 2 | + | Operator → Push |
| 3 | 3 | 23 | + | Operand → Output |
| 4 | * | 23 | +* | * has higher prec → Push |
| 5 | 4 | 234 | +* | Operand → Output |
| End | - | 234*+ | | Pop all from stack |

**Result**: `2 3 4 * +`

---

## Trace Example 2: `(2 + 3) * 4`

| Step | Input | Output | Stack | Action |
|------|-------|--------|-------|--------|
| 1 | ( | | ( | Open paren → Push |
| 2 | 2 | 2 | ( | Operand → Output |
| 3 | + | 2 | (+ | Operator → Push |
| 4 | 3 | 23 | (+ | Operand → Output |
| 5 | ) | 23+ | | Pop until ( |
| 6 | * | 23+ | * | Operator → Push |
| 7 | 4 | 23+4 | * | Operand → Output |
| End | - | 23+4* | | Pop all |

**Result**: `2 3 + 4 *`

---

## Trace Example 3: `A + B * C - D / E`

| Step | Input | Output | Stack | Action |
|------|-------|--------|-------|--------|
| 1 | A | A | | Operand |
| 2 | + | A | + | Operator |
| 3 | B | AB | + | Operand |
| 4 | * | AB | +* | * > + → Push |
| 5 | C | ABC | +* | Operand |
| 6 | - | ABC* | +- | * ≥ - → Pop *, - ≥ - → Push - |
| 7 | D | ABC*D | +- | Operand |
| 8 | / | ABC*D | +-/ | / > - → Push |
| 9 | E | ABC*DE | +-/ | Operand |
| End | - | ABC*DE/- | | Pop all: /, - |

**Result**: `A B C * + D E / -`

---

## Key Points for Exam

✅ **Must Remember**:
1. **Operands** go directly to output in same order
2. **Precedence matters**: `*,/` > `+,-`
3. **Parentheses** control precedence, not in output
4. **When operator comes**: pop higher/equal precedence first
5. **At end**: pop everything remaining

❌ **Common Mistakes**:
- Forgetting to pop operators at the end
- Popping lower precedence operators
- Including parentheses in output
- Wrong precedence order

⚡ **Time Complexity**: O(n) - Single pass
⚡ **Space Complexity**: O(n) - Stack and output string

---

## Follow-up Questions

1. **Postfix Evaluation**: How to evaluate the postfix expression?
2. **Infix to Prefix**: Reverse scan + modified rules
3. **Error Handling**: Invalid expressions with unmatched parentheses

---

## Related Problems

- [Postfix Evaluation](./postfix_evaluation.md)
- [Infix to Prefix Conversion](../Unit-2-Stack/README.md)
- [Expression Parsing](../Unit-2-Stack/README.md)

