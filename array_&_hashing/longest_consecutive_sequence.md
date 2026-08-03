# Longest Consecutive Sequence

## Problem

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

A consecutive sequence contains numbers that differ by `1`.

Example:

```text
Input:
nums = [100,4,200,1,3,2]

Output:
4

Explanation:
The longest consecutive sequence is

1 → 2 → 3 → 4
```

---

# Initial Thoughts

Whenever we need to find consecutive numbers, one natural idea is to sort the array first.

After sorting, consecutive numbers become adjacent, making them easy to count.

---

# Approach 1: Brute Force

## Thought Process

For every number,

keep checking whether the next consecutive number exists.

For example, if the current number is `5`, search for `6`.

If `6` exists, search for `7`, then `8`, and so on.

Keep track of the longest sequence found.

---

## Algorithm

1. Traverse every number.
2. Start the sequence length as `1`.
3. Repeatedly search for the next number (`current + 1`).
4. If found, increase the sequence length.
5. Update the maximum length.

---

## Pseudo Code

```javascript
maxLength = 0

for each num:

    current = num
    length = 1

    while (array contains current + 1) {
        current++
        length++
    }

    maxLength = max(maxLength, length)
```

---

## Time Complexity

Outer loop

```
O(n)
```

Searching for the next number using `includes()`

```
O(n)
```

Worst case

```
O(n × n)

=

O(n²)
```

---

## Space Complexity

Only a few extra variables are used.

```
O(1)
```

---

# Observation

Searching the entire array repeatedly is expensive.

Can we make consecutive numbers appear together?

**Yes. By sorting the array.**

---

# Approach 2: Sorting + Linear Scan

## Thought Process

Sort the array first.

After sorting,

all consecutive numbers become adjacent.

Example

```text
nums

[2,3,1,20,22,23,21]
```

After sorting

```text
[1,2,3,20,21,22,23]
```

Now simply count consecutive elements.

Ignore duplicates while traversing.

---

## Algorithm

1. Sort the array.
2. Initialize:
   - `currentLength = 1`
   - `maxLength = 1`
3. Traverse the sorted array.
4. If duplicate, ignore it.
5. If current element is previous + 1, increase the current sequence.
6. Otherwise, start a new sequence.
7. Return the maximum sequence length.

---

## Code

```javascript
nums.sort((a, b) => a - b);

let currentLength = 1;
let maxLength = 1;

for (let i = 1; i < nums.length; i++) {

    if (nums[i] === nums[i - 1]) {
        continue;
    }

    if (nums[i] === nums[i - 1] + 1) {
        currentLength++;
    } else {
        maxLength = Math.max(maxLength, currentLength);
        currentLength = 1;
    }
}

return Math.max(maxLength, currentLength);
```

---

## Time Complexity

Sorting

```
O(n log n)
```

Traversal

```
O(n)
```

Total

```
O(n log n)
```

---

## Space Complexity

Ignoring the sorting implementation,

```
O(1)
```

---

# Observation

Sorting works well,

but the problem asks for an **O(n)** solution.

Can we avoid sorting completely?

**Yes. By using a HashSet.**

---

# Approach 3 (Optimal): HashSet

## Thought Process

Store every number in a HashSet.

Instead of starting a sequence from every number,

start only when the current number is the **beginning** of a sequence.

A number is the beginning if

```text
num - 1
```

does **not** exist.

Example

```text
nums

[100,4,200,1,3,2]
```

HashSet

```text
{100,4,200,1,3,2}
```

Only `1` starts a sequence because `0` does not exist.

Now count

```text
1 → 2 → 3 → 4
```

Length

```
4
```

Notice that `2`, `3`, and `4` are **not** used as starting points because they already have predecessors.

This prevents repeated work.

---

## Algorithm

1. Store every number in a HashSet.
2. Traverse every number.
3. If `num - 1` exists, skip it.
4. Otherwise, start counting the sequence.
5. Keep checking `num + 1`.
6. Update the maximum length.

---

## Code

```javascript
const set = new Set(nums);

let longest = 0;

for (const num of set) {

    if (set.has(num - 1)) {
        continue;
    }

    let current = num;
    let length = 1;

    while (set.has(current + 1)) {
        current++;
        length++;
    }

    longest = Math.max(longest, length);
}

return longest;
```

---

## Time Complexity

Creating HashSet

```
O(n)
```

Traversing HashSet

```
O(n)
```

Although there is a `while` loop,

each number is visited only once across all sequences.

Therefore

```
O(n)
```

---

## Space Complexity

HashSet stores all numbers.

```
O(n)
```

---

# Pattern

## Category

**Array & Hashing (NeetCode)**

---

## Pattern

**HashSet + Sequence Detection**

---

## When to Identify This Pattern

Think about this pattern whenever:

- You need fast lookups.
- The problem involves consecutive values.
- Sorting is too slow.
- You need an `O(n)` solution.

---

## Key Idea

Only start counting from the **first element of a sequence**.

A number is the first element if

```text
num - 1
```

does not exist.

This ensures every sequence is counted exactly once.

---

# Related Questions

- Contains Duplicate
- Valid Sudoku
- Happy Number
- Longest Increasing Subsequence *(different pattern)*
- Number of Islands *(different pattern)*

---

# What You Should Learn From This Problem

- Brute force often repeats the same work.
- Sorting can simplify many problems but may not satisfy optimal constraints.
- HashSet enables constant-time lookups.
- Avoid redundant work by identifying the true starting point of a sequence.

---

# Comparison

| Approach | Time | Space |
|-----------|------|--------|
| Brute Force (search next element repeatedly) | O(n²) | O(1) |
| Sorting + Linear Scan | O(n log n) | O(1)\* |
| HashSet + Sequence Detection | O(n) | O(n) |

> **Recommended Interview Solution:** Use **HashSet + Sequence Detection** because it avoids sorting, performs constant-time lookups, and counts each sequence exactly once, achieving **O(n)** time.