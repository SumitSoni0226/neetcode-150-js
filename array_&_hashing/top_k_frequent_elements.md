# Top K Frequent Elements

## Problem

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

You may return the answer in any order.

Example:

```text
Input:
nums = [1,1,1,2,2,3]
k = 2

Output:
[1,2]
```

---

# Initial Thoughts

Whenever we need to return the elements with the highest frequency, there are multiple ways to think about the problem.

---

# Approach 1: HashMap + Sorting

## Thought Process

The first idea is:

1. Count the frequency of every unique element.
2. Sort the elements based on their frequency in descending order.
3. Return the first `k` elements.

Example

```text
nums =

[1,1,1,2,2,3]
```

Frequency Map

```text
1 → 3
2 → 2
3 → 1
```

Sort by frequency

```text
[
    [1,3],
    [2,2],
    [3,1]
]
```

Take first `k`

```text
[1,2]
```

---

## Algorithm

1. Create a HashMap.
2. Count the frequency of every number.
3. Convert the HashMap into an array.
4. Sort the array based on frequency (descending).
5. Extract only the keys.
6. Return the first `k` elements.

---

## Code

```javascript
const frequencyMap = new Map();

for (const num of nums) {
    frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
}

return Array.from(frequencyMap.entries())
    .sort((a, b) => b[1] - a[1])
    .slice(0, k)
    .map(([num]) => num);
```

---

## Time Complexity

### Step 1

Build frequency map

```
O(n)
```

---

### Step 2

Convert HashMap into array

```
O(m)
```

where

```
m = number of unique elements
```

---

### Step 3

Sort

```
O(m log m)
```

Worst case

```
m = n
```

So

```
O(n log n)
```

---

### Step 4

Take first `k` elements

```
O(k)
```

---

### Total

```
O(n + m log m + k)
```

Worst case

```
O(n log n)
```

---

## Space Complexity

HashMap

```
O(m)
```

Sorted array

```
O(m)
```

Overall

```
O(m)
```

Worst case

```
O(n)
```

---

# Problem With This Approach

Sorting is expensive.

We don't actually need all elements sorted.

We only need the elements having the highest frequencies.

Can we avoid sorting completely?

**Yes.**

---

# Approach 2: Bucket Sort (Optimal)

## Key Observation

The maximum frequency of any element can never exceed the array length.

Example

```text
nums =

[1,1,1,2,2,3]
```

Maximum frequency

```text
3
```

Array length

```text
6
```

In general

```
Maximum Frequency ≤ nums.length
```

Therefore,

instead of sorting,

create buckets where

```
bucket[i]
```

stores all numbers having frequency `i`.

---

## Example

Frequency Map

```text
1 → 3

2 → 2

3 → 1
```

Buckets

```text
Index

0 → []

1 → [3]

2 → [2]

3 → [1]

4 → []

5 → []

6 → []
```

Now traverse buckets from right to left.

```text
6

5

4

3 → [1]

Take 1

2 → [2]

Take 2

Done
```

Answer

```text
[1,2]
```

No sorting required.

---

## Algorithm

1. Count frequencies using a HashMap.
2. Create `nums.length + 1` buckets.
3. Put every number into its corresponding frequency bucket.
4. Traverse buckets from highest frequency to lowest.
5. Collect elements until `k` elements are found.

---

## Code

```javascript
const frequencyMap = new Map();

for (const num of nums) {
    frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
}

const buckets = Array(nums.length + 1)
    .fill(null)
    .map(() => []);

for (const [num, frequency] of frequencyMap.entries()) {
    buckets[frequency].push(num);
}

const result = [];

for (let i = buckets.length - 1; i >= 0; i--) {
    for (const num of buckets[i]) {
        result.push(num);

        if (result.length === k) {
            return result;
        }
    }
}
```

---

## Time Complexity

### Step 1

Build frequency map

```
O(n)
```

---

### Step 2

Create buckets

```
O(n)
```

---

### Step 3

Place every unique element into a bucket

```
O(m)
```

where

```
m = number of unique elements
```

---

### Step 4

Traverse buckets

Outer loop

```
O(n)
```

Inner loops combined

```
O(m)
```

Since

```
m ≤ n
```

```
O(n + m)

=

O(n)
```

---

### Total

```
O(n)
+
O(n)
+
O(m)
+
O(n)

=

O(n)
```

---

## Space Complexity

Frequency Map

```
O(m)
```

Buckets

```
O(n)
```

Result

```
O(k)
```

Overall

```
O(n)
```

---

# Why Isn't Bucket Sort O(n²)?

Although there are nested loops,

```javascript
for (...) {
    for (...) {
    }
}
```

the inner loop **does not run completely for every outer iteration**.

Every unique element is stored in **exactly one bucket**.

Example

```text
Bucket 1

3
4
5

Bucket 2

1
2
```

Total work

```
3
+
2

=

5
```

Not

```
7 buckets

×

5 elements
```

General proof

Suppose

```
bucket1 has a elements

bucket2 has b elements

bucket3 has c elements
```

Total work

```
a + b + c + ...

= m
```

Therefore

```
Outer Loop

O(n)

+

All Inner Loops Combined

O(m)
```

Since

```
m ≤ n
```

Final complexity

```
O(n + m)

=

O(n)
```

---

# Pattern

## Pattern Name

**Hashing + Bucket Sort**

---

## When to Identify This Pattern

Think about Bucket Sort whenever:

- You need Top K Frequent Elements.
- Frequencies are bounded.
- Maximum frequency cannot exceed `n`.
- Sorting can be avoided by grouping elements into buckets.

---

## Key Idea

Instead of sorting by frequency,

place elements into buckets where the bucket index represents the frequency.

Then traverse buckets from highest frequency to lowest.

---

# Related Questions

## Easy

- Contains Duplicate
- Valid Anagram
- Two Sum

---

## Medium

- Top K Frequent Elements
- Top K Frequent Words
- Group Anagrams
- Sort Characters By Frequency

---

# What You Should Learn From This Problem

- HashMaps are useful for frequency counting.
- Sorting is not always necessary.
- Bucket Sort converts an `O(n log n)` solution into `O(n)` when the frequency range is bounded.
- **Nested loops do not always imply `O(n²)`**.
- Always ask yourself:

> **Can I group elements by their frequency instead of sorting them?**

---

# Comparison

| Approach | Time | Space |
|-----------|------|--------|
| HashMap + Sorting | O(n log n) | O(n) |
| HashMap + Bucket Sort | O(n) | O(n) |

> **Recommended Interview Solution:** Use **Bucket Sort** because it avoids sorting and achieves **O(n)** time complexity.