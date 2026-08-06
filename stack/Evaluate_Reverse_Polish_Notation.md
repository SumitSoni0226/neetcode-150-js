# Evaluate Reverse Polish Notation (LeetCode 150)

> **Pattern:** Stack  
> **Difficulty:** Medium  
> **Time:** O(n)  
> **Space:** O(n)

---

# Problem Statement

You are given an array of strings representing an expression in **Reverse Polish Notation (Postfix Expression)**.

Evaluate the expression and return the result.

Rules:

- `+` → Addition
- `-` → Subtraction
- `*` → Multiplication
- `/` → Division (truncate toward zero)

---

## Example 1

```text
Input

["2","1","+","3","*"]

Expression

(2 + 1) * 3

Output

9
```

---

## Example 2

```text
Input

["4","13","5","/","+"]

Expression

4 + (13 / 5)

Output

6
```

---

## Example 3

```text
Input

["10","6","9","3","+","-11","*","/","*","17","+","5","+"]

Output

22
```

---

# Understanding Reverse Polish Notation

Unlike normal expressions,

```text
2 + 3
```

the operator comes **after** the operands.

Example

```text
2 3 +
```

means

```text
2 + 3
```

Another example

```text
2 1 + 3 *
```

means

```text
(2 + 1) * 3
```

---

# Initial Intuition

When we encounter a number,

we don't know what operation will be performed on it yet.

So we save it.

When an operator appears,

it always uses the **latest two numbers**.

Example

```text
2 1 +
```

The operator `+` uses

```text
2

1
```

After computing

```text
3
```

that result may be used later.

So we save it again.

This is exactly

```text
Last In

↓

First Out

↓

STACK
```

---

# First Thought (Using eval)

My first thought was:

- Push every number into the stack.
- Whenever an operator appears:
  - Pop two numbers.
  - Create an expression as a string.
  - Use JavaScript's `eval()`.

Example

```javascript
const exp = `${a}+${b}`;

stack.push(eval(exp));
```

---

## Why This Is Not Recommended

Although it works for simple cases,

there are several problems.

### Problem 1

`eval()` is slow.

---

### Problem 2

Interviewers generally don't allow using `eval()` because the goal is to implement the operations yourself.

---

### Problem 3

Division becomes incorrect.

Example

```text
13 / 5
```

JavaScript

```text
2.6
```

Problem expects

```text
2
```

Similarly

```text
-13 / 5
```

Expected

```text
-2
```

Need

```javascript
Math.trunc(a / b)
```

instead.

---

# Key Observation

Whenever we see an operator,

it always works on the **latest two numbers**.

Example

```text
2 1 +
```

Stack

```text
2
1
```

Operator

```text
+
```

Pop

```text
1

2
```

Compute

```text
2 + 1
```

Push

```text
3
```

Now stack becomes

```text
3
```

---

Another example

```text
4 13 5 / +
```

Process

```text
4

13

5
```

Encounter

```text
/
```

Pop

```text
5

13
```

Important

The first pop is the **second operand**.

```text
b = 5

a = 13
```

Compute

```text
13 / 5
```

Push

```text
2
```

Stack becomes

```text
4

2
```

Encounter

```text
+
```

Compute

```text
4 + 2
```

Answer

```text
6
```

---

# Why Operand Order Matters

Many beginners write

```javascript
const a = stack.pop();
const b = stack.pop();

a - b
```

This is wrong.

Example

```text
9 4 -
```

Means

```text
9 - 4
```

Stack

```text
9

4
```

First pop

```text
4
```

Second pop

```text
9
```

Correct code

```javascript
const b = stack.pop();
const a = stack.pop();

a - b
```

Otherwise

```text
4 - 9
```

which is incorrect.

---

# Algorithm

Traverse every token.

---

### If token is a number

Push into stack.

---

### If token is an operator

Pop

```text
b
```

Pop

```text
a
```

Apply

```text
a operator b
```

Push the result.

---

### At the end

The stack contains exactly one value.

Return it.

---

# Dry Run

Input

```text
["2","1","+","3","*"]
```

Initially

```text
Stack

[]
```

---

Read

```text
2
```

Stack

```text
[2]
```

---

Read

```text
1
```

Stack

```text
[2,1]
```

---

Read

```text
+
```

Pop

```text
b = 1

a = 2
```

Compute

```text
2 + 1 = 3
```

Stack

```text
[3]
```

---

Read

```text
3
```

Stack

```text
[3,3]
```

---

Read

```text
*
```

Pop

```text
b = 3

a = 3
```

Compute

```text
3 * 3 = 9
```

Stack

```text
[9]
```

Answer

```text
9
```

---

# Optimal Code

```javascript
class Solution {
    /**
     * @param {string[]} tokens
     * @return {number}
     */
    evalRPN(tokens) {

        const stack = [];

        const operators = new Set([
            "+",
            "-",
            "*",
            "/"
        ]);

        for (let token of tokens) {

            if (!operators.has(token)) {

                stack.push(Number(token));
            }

            else {

                const b = stack.pop();
                const a = stack.pop();

                if (token === "+") {

                    stack.push(a + b);

                } else if (token === "-") {

                    stack.push(a - b);

                } else if (token === "*") {

                    stack.push(a * b);

                } else {

                    stack.push(Math.trunc(a / b));
                }
            }
        }

        return stack.pop();
    }
}
```

---

# Complexity

## Time

Every token is processed once.

```text
O(n)
```

---

## Space

Worst case

```text
["1","2","3","4","5"]
```

All numbers remain in the stack.

```text
O(n)
```

---

# Pattern Recognition

Whenever you see

- Postfix Expression
- Reverse Polish Notation
- Expression Evaluation
- Latest operands are used first

Think

```text
STACK
```

---

# Interview Explanation

> Reverse Polish Notation places operators after their operands. I use a stack to store numbers. Whenever I encounter a number, I push it onto the stack. Whenever I encounter an operator, I pop the latest two operands, apply the operation in the correct order (`a op b`), and push the result back onto the stack. At the end, the stack contains exactly one value, which is the final answer.

---

# Key Learnings

- Reverse Polish Notation is naturally solved using a **Stack**.
- The **first popped value is the second operand**.
- Never use `eval()` in interviews.
- Use `Math.trunc()` for division to match the problem's requirement.
- Push intermediate results back into the stack because they may be used in later operations.

---

# Cheat Sheet

```text
Read token

↓

Number?

↓

Push into stack

-------------------------

Operator?

↓

Pop b

↓

Pop a

↓

Compute

a op b

↓

Push result

-------------------------

End

↓

Return stack.pop()
```