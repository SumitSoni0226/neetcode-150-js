# Two Pointers Pattern Summary (NeetCode 150)

## Overview

The **Two Pointers** pattern is used when we need to process **two elements simultaneously** instead of one.

Instead of using nested loops (`O(n²)`), we intelligently move one or both pointers to reduce the search space, often achieving **O(n)** time complexity.

---

# Problems Covered

| Problem | Difficulty | Pattern | Time | Space | Revise |
|----------|------------|---------|------|--------|---------|
| Valid Palindrome | Easy | Opposite-End Two Pointers | O(n) | O(1) | ⭐ |
| Two Sum II | Medium | Opposite-End Two Pointers | O(n) | O(1) | ⭐ |
| 3Sum | Medium | Sorting + Fixed Pointer + Two Pointers | O(n²) | O(1)* | ⭐⭐⭐ |
| Container With Most Water | Medium | Opposite-End Two Pointers (Greedy) | O(n) | O(1) | ⭐⭐ |
| Trapping Rain Water | Hard | Two Pointers + Running Maximums | O(n) | O(1) | ⭐⭐⭐⭐⭐ **REVISE AGAIN** |

---

# 1. Valid Palindrome

### Pattern

Opposite-End Two Pointers

### Key Idea

Start from both ends.

- Ignore non-alphanumeric characters.
- Compare lowercase characters.
- If mismatch → false.
- Else move inward.

### Why Two Pointers?

We only need to compare mirrored characters.

### Complexity

```
Time  : O(n)

Space : O(1)
```

---

# 2. Two Sum II

### Pattern

Opposite-End Two Pointers

### Key Observation

Array is already sorted.

If

```
sum > target
```

Move

```
right--
```

because larger value must decrease.

If

```
sum < target
```

Move

```
left++
```

because smaller value must increase.

### Why Not HashMap?

HashMap works for an unsorted array.

Here sorting is already given.

Two pointers provide

```
O(1)
```

space instead of

```
O(n)
```

### Complexity

```
Time : O(n)

Space : O(1)
```

---

# 3. 3Sum

### Pattern

Sorting + Fixed Pointer + Two Pointers

### Reduction Trick

```
3Sum

↓

Fix one number

↓

Now solve

2Sum
```

This is called **problem reduction**.

### Pointer Movement

```
sum == 0

Store answer

Skip duplicates

left++
right--
```

```
sum > 0

right--
```

```
sum < 0

left++
```

### Biggest Learning

Always skip

- duplicate `i`
- duplicate `left`
- duplicate `right`

Otherwise duplicate triplets will appear.

### Complexity

```
Time : O(n²)

Space : O(1)
```

---

# 4. Container With Most Water

### Pattern

Greedy + Opposite-End Two Pointers

### Formula

```
Area

=

Width

×

Minimum Height
```

### Biggest Observation

The smaller bar limits the water.

Moving the taller bar cannot increase the area.

Therefore

```
Move the shorter bar.
```

### Complexity

```
Time : O(n)

Space : O(1)
```

---

# 5. Trapping Rain Water ⭐⭐⭐⭐⭐

> **⚠️ Marked for Revision**

### Pattern

Two Pointers + Running Maximums

### Formula

Water at every index

```
min(leftMax, rightMax)

-

height[i]
```

### Journey

```
Brute Force

↓

Prefix Max + Suffix Max

↓

Two Pointers
```

### Biggest Observation

Suppose

```
leftMax = 5

rightMax = 10
```

Water level depends on

```
5
```

Even if right becomes

```
100
```

Water level is still

```
5
```

So we can safely process the left pointer.

This is the hardest concept in the entire Two Pointer section.

---

## Why Revise Again?

This problem combines multiple concepts:

- Prefix Maximum
- Suffix Maximum
- Greedy
- Two Pointers
- Proof of correctness

It is one of the most frequently asked SDE-2 interview questions.

> **Revision Status:** 🔴 **Revise at least 2–3 more times.**

### Complexity

```
Time : O(n)

Space : O(1)
```

---

# Common Two Pointer Patterns

## 1. Opposite-End Two Pointers

Start

```
left = 0

right = n-1
```

Examples

- Valid Palindrome
- Two Sum II
- Container With Most Water
- Trapping Rain Water

---

## 2. Fixed Pointer + Two Pointers

Fix one element.

Solve remaining using two pointers.

Examples

- 3Sum
- 4Sum
- 3Sum Closest

---

## 3. Same Direction Two Pointers

Both pointers move left to right.

Examples

- Remove Duplicates from Sorted Array
- Move Zeroes
- Remove Element

(Not covered yet.)

---

# When Should You Think About Two Pointers?

Ask yourself:

### Question 1

Need to choose **2 elements**?

✅ Think Two Pointers.

---

### Question 2

Array already sorted?

✅ Two Pointers is often better than HashMap.

---

### Question 3

Can moving one pointer eliminate many possibilities?

✅ Two Pointers.

---

### Question 4

Need to compare from both ends?

✅ Opposite-End Two Pointers.

---

### Question 5

Can one variable be fixed and the remaining problem become Two Sum?

✅ Fixed Pointer + Two Pointers.

---

# Biggest Learnings from This Section

### ✅ Learning 1

Not every array problem needs a HashMap.

Sometimes sorting + two pointers gives a better solution.

---

### ✅ Learning 2

Whenever a problem asks to choose **two values**, first ask yourself:

> Can I solve this using two pointers instead of nested loops?

---

### ✅ Learning 3

Many higher-level problems reduce to simpler ones.

Example

```
3Sum

↓

Fix one

↓

Two Sum II
```

This **problem reduction** technique appears frequently in SDE-2 interviews.

---

### ✅ Learning 4

Pointer movement is never random.

Each movement has a mathematical proof.

Examples:

- **Two Sum II** → move pointer based on the sum.
- **Container With Most Water** → move the shorter bar.
- **Trapping Rain Water** → process the side with the smaller maximum.

Understanding **why** the pointer moves is more important than memorizing the code.

---

# Microsoft SDE-2 Questions Based on Two Pointers

## Easy

- Valid Palindrome
- Reverse String
- Merge Sorted Array
- Remove Duplicates from Sorted Array

---

## Medium

- Two Sum II
- 3Sum
- 3Sum Closest
- Container With Most Water
- Sort Colors
- Partition Labels
- Boats to Save People

---

## Hard

- Trapping Rain Water ⭐⭐⭐⭐⭐
- First Missing Positive (mixed pattern)
- Minimum Window Substring (Sliding Window variant)

---

# Revision Checklist

- ✅ Valid Palindrome
- ✅ Two Sum II
- ✅ 3Sum
- ✅ Container With Most Water
- 🔴 **Trapping Rain Water (Revise Again)**
  - Understand the `min(leftMax, rightMax)` formula.
  - Be able to derive the Prefix/Suffix solution first.
  - Be able to explain why processing the smaller maximum is always correct.
  - Dry run the two-pointer solution without looking at the code.

---

# Interview Takeaway

The Two Pointer pattern is about **eliminating impossible choices**.

Instead of checking every pair (`O(n²)`), each pointer movement discards a set of candidates that can never lead to a better answer.

This ability to **reduce the search space using problem-specific observations** is what interviewers look for in SDE-2 candidates.