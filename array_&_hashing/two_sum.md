# Two Sum

## Problem

Given an integer array `nums` and an integer `target`, return the **indices** of the two numbers such that they add up to the target.

You may assume:

- Exactly one valid answer exists.
- You may not use the same element twice.
- Return the indices in any order.

---

# Initial Thoughts

Whenever we need to find a pair of numbers that satisfy a condition, there are multiple ways to think about the problem.

---

# Approach 1: Brute Force (Nested Loops)

## Thought Process

The most straightforward idea is to check every possible pair.

For every element, iterate through all remaining elements and check whether their sum equals the target.

Example:

```text
nums = [2,7,11,15]
target = 9

2 + 7 = 9

Return [0,1]
```

---

## Algorithm

1. Traverse the array using index `i`.
2. For every `i`, traverse the remaining array using index `j`.
3. Check if `nums[i] + nums[j] == target`.
4. If yes, return `[i, j]`.

---

## Code

```javascript
for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
        if (nums[i] + nums[j] === target) {
            return [i, j];
        }
    }
}
```

---

## Time Complexity

### Outer Loop

Runs `n` times.

```
O(n)
```

---

### Inner Loop

Runs at most `n` times for each outer iteration.

```
O(n)
```

---

### Total

```
O(n)
×
O(n)

= O(n²)
```

---

## Space Complexity

No extra data structure is used.

```
O(1)
```

---

# Approach 2: HashMap (Optimal)

## Thought Process

Instead of checking every pair, we can remember the numbers we have already seen.

For every number:

1. Calculate the complement.

```
complement = target - currentNumber
```

2. Check whether the complement already exists in the HashMap.

- If yes, we found our answer.
- Otherwise, store the current number and its index.

Example

```text
nums = [2,7,11,15]
target = 9

i = 0

Current = 2
Complement = 7

HashMap

2 → 0

--------------------

i = 1

Current = 7
Complement = 2

2 already exists

Return [1,0]
```

---

## Algorithm

1. Create an empty HashMap.
2. Traverse the array.
3. Compute the complement.
4. If the complement exists in the HashMap, return both indices.
5. Otherwise, store the current number and its index.
6. Continue until the pair is found.

---

## Code

```javascript
const hashMap = new Map();

for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];

    if (hashMap.has(complement)) {
        return [i, hashMap.get(complement)];
    }

    hashMap.set(nums[i], i);
}
```

---

## Time Complexity

### Traverse Array

We visit every element once.

```
O(n)
```

---

### HashMap Operations

For every element:

- `has()` → O(1)
- `get()` → O(1)
- `set()` → O(1)

Average time for each operation is constant.

---

### Total

```
n × O(1)

= O(n)
```

---

## Space Complexity

In the worst case, no pair is found until the last element.

The HashMap stores every element.

```
O(n)
```

---

# Why Does the HashMap Solution Work?

At every step, the HashMap contains all numbers that appeared **before** the current element.

Instead of asking:

> "Which number should I pair with the current number?"

We ask:

> "Have I already seen the number I need?"

This changes the lookup from **O(n)** to **O(1)**.

---

# Pattern

## Pattern Name

**Hashing (HashMap for Lookup)**

### When to Identify This Pattern

Think about using a HashMap whenever the problem asks:

- Find a pair whose sum equals a target.
- Fast lookup of previously seen elements.
- Store values with their indices.
- Reduce nested loops to a single traversal.
- Check whether an element already exists.

---

## Key Idea

Store useful information while traversing the array so future lookups become **O(1)**.

Trade extra space for faster time.

---

# Related Questions

## Easy

- Two Sum
- Contains Duplicate
- Valid Anagram
- Intersection of Two Arrays
- Happy Number
- Isomorphic Strings

---

## Medium

- Two Sum II (Sorted Array)
- Three Sum
- Four Sum
- Subarray Sum Equals K
- Continuous Subarray Sum
- Top K Frequent Elements
- Group Anagrams

---

# What You Should Learn from This Problem

- Always think whether nested loops can be replaced with a HashMap.
- Store information while traversing instead of searching again.
- HashMaps are extremely useful for lookup-based problems.
- Learn to recognize complement-based problems.

Ask yourself:

> **"If I know the current value, what other value do I need?"**

That required value is usually called the **complement**.

---

# Comparison

| Approach | Time | Space |
|-----------|------|--------|
| Brute Force (Nested Loops) | O(n²) | O(1) |
| HashMap (Complement Lookup) | O(n) | O(n) |

> **Recommended Interview Solution:** Use a **HashMap**. It reduces the time complexity from **O(n²)** to **O(n)** by storing previously seen numbers and performing constant-time lookups.