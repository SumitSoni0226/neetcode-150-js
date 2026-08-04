# Two Sum II - Input Array Is Sorted

## Problem

Given a **1-indexed** sorted array of integers `numbers` and an integer `target`, return the indices of the two numbers such that they add up to `target`.

- Exactly one solution exists.
- You cannot use the same element twice.
- Return the answer using **1-based indexing**.

### Example

```text
Input:
numbers = [2,7,11,15]
target = 9

Output:
[1,2]
```

---

# Initial Thoughts

The first idea that comes to mind is to try every possible pair.

For every element, iterate over all the elements after it and check whether their sum equals the target.

This approach works, but it repeatedly checks many unnecessary pairs.

Since the array is **already sorted**, we should use this property to find a more efficient solution.

---

# Approach 1: Brute Force

## Thought Process

For every element,

check every element after it.

If their sum equals the target,

return their indices.

---

## Algorithm

1. Traverse the array.
2. For each index `i`, traverse from `i + 1`.
3. If `numbers[i] + numbers[j] == target`, return the indices.
4. Since exactly one solution exists, we don't need any additional handling.

---

## Code

```javascript
for (let i = 0; i < numbers.length; i++) {

    for (let j = i + 1; j < numbers.length; j++) {

        if (numbers[i] + numbers[j] === target) {
            return [i + 1, j + 1];
        }

    }

}
```

---

## Time Complexity

Outer loop

```
O(n)
```

Inner loop

```
O(n)
```

Total

```
O(n²)
```

---

## Space Complexity

Only loop variables are used.

```
O(1)
```

---

# Observation

The array is already sorted.

Can we avoid checking every pair?

Yes.

If the current sum is too small,

we should increase it.

If the current sum is too large,

we should decrease it.

This naturally leads to the **Two Pointers** technique.

---

# Approach 2 (Optimal): Two Pointers

## Thought Process

Maintain two pointers.

- Left pointer starts from the beginning.
- Right pointer starts from the end.

Calculate the current sum.

### If

```text
sum == target
```

Return the answer.

### If

```text
sum < target
```

Move the **left pointer** because we need a larger value.

### If

```text
sum > target
```

Move the **right pointer** because we need a smaller value.

---

## Example

```text
numbers = [2,7,11,15]

target = 9
```

Initial

```text
L               R
2   7   11   15
```

Sum

```text
17
```

Too large

Move

```text
right--
```

Now

```text
L          R
2   7   11
```

Sum

```text
13
```

Still too large

Move

```text
right--
```

Now

```text
L     R
2     7
```

Sum

```text
9
```

Found.

Return

```text
[1,2]
```

---

## Algorithm

1. Initialize
   - `left = 0`
   - `right = numbers.length - 1`
2. While `left < right`
3. Calculate the current sum.
4. If sum equals target, return the indices.
5. If sum is smaller than target, move `left`.
6. Otherwise move `right`.

---

## Code

```javascript
let left = 0;
let right = numbers.length - 1;

while (left < right) {

    const sum = numbers[left] + numbers[right];

    if (sum === target) {
        return [left + 1, right + 1];
    }

    if (sum < target) {
        left++;
    } else {
        right--;
    }
}
```

---

# Why Does This Work?

Since the array is sorted,

### Case 1

```text
sum < target
```

We need a **larger sum**.

Moving the right pointer left would only decrease the sum.

So the only useful move is

```text
left++
```

---

### Case 2

```text
sum > target
```

We need a **smaller sum**.

Moving the left pointer right would only increase the sum.

So the correct move is

```text
right--
```

Because of the sorted property, every pointer movement safely eliminates impossible pairs.

---

# Why Two Pointers Instead of HashMap?

A HashMap solution also solves the problem.

```javascript
const map = new Map();

for (let i = 0; i < numbers.length; i++) {

    const complement = target - numbers[i];

    if (map.has(complement)) {
        return [map.get(complement) + 1, i + 1];
    }

    map.set(numbers[i], i);
}
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(n)
```

---

## Why Isn't HashMap the Preferred Solution?

The array is already **sorted**.

The sorted order gives us extra information that allows us to solve the problem without storing previously seen elements.

Instead of using additional memory, we can place one pointer at each end of the array and eliminate impossible pairs after every comparison.

This gives:

- Same Time Complexity: `O(n)`
- Better Space Complexity: `O(1)`

Therefore, **Two Pointers** is the optimal approach.

---

## Comparison

| Approach | Time | Space |
|----------|------|--------|
| HashMap | O(n) | O(n) |
| Two Pointers | O(n) | O(1) |

---

## Rule of Thumb

### Unsorted Array

Think

```text
HashMap
```

Example

- Two Sum

---

### Sorted Array

Think

```text
Two Pointers
```

Example

- Two Sum II

---

## Interview Explanation

If the interviewer asks why you used Two Pointers instead of HashMap, you can answer:

> "A HashMap also provides an O(n) solution, but it requires O(n) extra space. Since the input array is already sorted, I can leverage that property to eliminate impossible pairs using two pointers. This achieves the same O(n) time complexity while reducing the extra space to O(1), making it the more optimal solution."

---

## Time Complexity

Each pointer moves in only one direction.

- Left pointer moves at most `n` times.
- Right pointer moves at most `n` times.

Total work

```
O(n)
```

---

## Space Complexity

Only two pointers are used.

```
O(1)
```

---

# Pattern

## Category

**Two Pointers (NeetCode)**

---

## Pattern

**Two Pointers (Opposite Ends)**

---

## When to Identify This Pattern

Think about this pattern whenever you see:

- Sorted array
- Pair sum
- Pair difference
- Need an `O(n)` solution
- Need to minimize extra space

---

## Key Idea

Use the sorted property.

- If the sum is too small → Move the left pointer.
- If the sum is too large → Move the right pointer.

Each movement eliminates impossible pairs without missing the correct answer.

---

# Related Questions

### Easy

- Valid Palindrome
- Reverse String
- Merge Sorted Array
- Remove Duplicates from Sorted Array
- Squares of a Sorted Array

### Medium

- 3Sum
- 3Sum Closest
- 4Sum
- Container With Most Water
- Trapping Rain Water
- Boats to Save People

### Frequently Asked in Microsoft

- Two Sum II
- 3Sum
- Container With Most Water
- Trapping Rain Water
- Squares of a Sorted Array
- Merge Sorted Array

---

# What You Should Learn From This Problem

- Always inspect the input constraints before choosing a solution.
- A sorted array often hints at the **Two Pointers** pattern.
- Don't automatically reach for a HashMap—use it only when the array is unsorted or when sorting information isn't available.
- Pointer movement should always be based on whether you need a larger or smaller value.
- Two Pointers can achieve the same time complexity as HashMap while using less memory.

---

# Comparison

| Approach | Time | Space |
|-----------|------|--------|
| Brute Force | O(n²) | O(1) |
| HashMap | O(n) | O(n) |
| Two Pointers | O(n) | O(1) |

> **Recommended Interview Solution:** Use **Two Pointers** because the array is already sorted. It leverages the sorted order to eliminate impossible pairs efficiently, achieving **O(n)** time with **O(1)** extra space.