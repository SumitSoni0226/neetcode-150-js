# Evolution of Solutions

---

# Solution 1 - Brute Force (Repeated String Replacement)

## Intuition

Keep removing valid pairs

```text
()

[]

{}
```

until nothing changes.

If the string becomes empty,

it was valid.

Otherwise,

it was invalid.

---

Example

```text
{[()]}
```

Iteration 1

```text
{[]}
```

Iteration 2

```text
{}
```

Iteration 3

```text
""
```

Return

```text
true
```

---

Example

```text
([)]
```

There is no

```text
()

[]

{}
```

adjacent pair.

Nothing gets removed.

Return

```text
false
```

---

## Code

```javascript
function isValid(s) {

    let previous = "";

    while (previous !== s) {

        previous = s;

        s = s
            .replace("()", "")
            .replace("[]", "")
            .replace("{}", "");
    }

    return s.length === 0;
}
```

---

## Complexity

Each replace scans the entire string.

Worst case

```text
((((((((()))))))))
```

Many scans.

Time

```text
O(n²)
```

Space

```text
O(n)
```

---

# Why can we do better?

Notice something.

Every time we see

```text
(
```

we only care about

its most recent unmatched opening bracket.

We never need to search the whole string.

This immediately suggests

```text
Last Opened

↓

First Closed

↓

LIFO

↓

STACK
```

---

# Solution 2 - Optimal (Stack)

## Intuition

Push every opening bracket.

Whenever a closing bracket appears,

pop the latest opening bracket and verify that they match.

If they don't,

return false.

---

## Example

Input

```text
{[]}
```

Read

```text
{
```

Stack

```text
{
```

---

Read

```text
[
```

Stack

```text
{
[
```

---

Read

```text
]
```

Pop

```text
[
```

Matches.

Stack

```text
{
```

---

Read

```text
}
```

Pop

```text
{
```

Matches.

Stack becomes empty.

Answer

```text
true
```

---

## Code

```javascript
class Solution {

    isValid(s) {

        const stack = [];

        const pairs = new Map([
            ["(", ")"],
            ["[", "]"],
            ["{", "}"]
        ]);

        for (const ch of s) {

            // Opening bracket
            if (pairs.has(ch)) {
                stack.push(ch);
            }

            // Closing bracket
            else {

                if (stack.length === 0)
                    return false;

                const last = stack.pop();

                if (pairs.get(last) !== ch)
                    return false;
            }
        }

        return stack.length === 0;
    }
}
```

---

## Complexity

Time

```text
O(n)
```

Every character is processed exactly once.

Space

```text
O(n)
```

Worst case

```text
((((((((
```

All opening brackets are stored in the stack.

---

# Why Stack is Better

Brute Force

```text
Repeatedly scan

↓

Remove matching pairs

↓

Scan again

↓

O(n²)
```

---

Stack

```text
Read character once

↓

Push opening bracket

↓

Pop on closing bracket

↓

O(n)
```

---

# Interview Thought Process

Question asks

```text
Validate brackets
```

↓

Need to preserve order?

↓

Yes

↓

Last opened should close first?

↓

Yes

↓

Think

```text
STACK
```

---

# Pattern Recognition

Whenever you see

- Nested structures
- Matching pairs
- Undo operations
- Expression evaluation
- Last In First Out (LIFO)

Immediately think

```text
STACK
```