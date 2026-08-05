# Sliding Window Pattern - Complete Beginner Guide

> **Goal:** Learn **when** to think about Sliding Window, **why** it works, and **how** to derive it naturally before solving problems.

---

# What is Sliding Window?

Sliding Window is an optimization technique used when we process a **continuous (contiguous)** subarray or substring.

Instead of recomputing every range from scratch, we reuse previous work by expanding and/or shrinking a window.

---

# Important Note

Not every problem in the "Sliding Window" section uses a **classic sliding window**.

There are three closely related techniques:

1. Fixed Sliding Window
2. Variable Sliding Window
3. Forward Two Pointers (Running Minimum / Running Maximum)

Example:

- Best Time to Buy and Sell Stock belongs to many Sliding Window roadmaps, but it is actually a **Forward Two Pointer** problem.

---

# How to Recognize Sliding Window

Ask these questions:

- Does the problem mention **subarray** or **substring**?
- Does it mention **contiguous** or **continuous**?
- Are we looking for longest / shortest / maximum / minimum continuous segment?
- Can I reuse work by removing one element and adding another?
- Does the window become valid/invalid?

If yes, think Sliding Window.

---

# Sliding Window vs Two Pointers

## Sliding Window

The entire range [left...right] matters.

Example:

abcde

[left........right]

---

## Two Pointers

Pointers move independently.

Examples:

- Two Sum II
- 3Sum
- Container With Most Water
- Trapping Rain Water
- Valid Palindrome

---

# Type 1 - Fixed Sliding Window

Window size never changes.

Template:

```js
let left = 0;

for (let right = 0; right < nums.length; right++) {

    // include right

    if (right - left + 1 > k) {

        // remove left
        left++;
    }

    if (right - left + 1 === k) {

        // process answer

    }
}
```

---

# Type 2 - Variable Sliding Window

Window grows.

If condition becomes invalid, shrink from left.

Template:

```js
let left = 0;

for (let right = 0; right < n; right++) {

    // include right

    while (window is invalid) {

        // remove left
        left++;

    }

    // update answer
}
```

---

# Type 3 - Forward Two Pointers

One pointer stores the **best candidate seen so far**.

The other pointer scans every remaining element.

Example:

Best Time to Buy and Sell Stock

left = cheapest buying day

right = current selling day

Every day must be checked as a selling day.

Therefore **right always moves**.

Whenever we discover a cheaper buying price,

move left to that index.

Notice:

This is NOT a classic window.

---

# Biggest Insight

Whenever brute force repeatedly searches previous elements, ask:

"Can I maintain this information while traversing?"

Examples:

- minimum seen so far
- maximum seen so far
- frequency
- running sum

Maintaining state usually converts O(n²) into O(n).

---

# Which Pointer Should Move?

Never memorize pointer movement.

Define what every pointer represents.

Example:

Best Time to Buy and Sell Stock

left = best buying day

right = current selling day

Question:

Can I skip checking today's selling price?

No.

Therefore right must move every iteration.

Left only changes when a better buying price is found.

---

# Mental Checklist

1. Subarray / Substring?
2. Continuous segment?
3. Can previous computation be reused?
4. Fixed size?
5. Variable size?
6. Best value seen so far while scanning?

These questions usually reveal the correct pattern.

---

# Problems

## Forward Two Pointers

- Best Time to Buy and Sell Stock

## Fixed Sliding Window

- Maximum Sum Subarray of Size K
- Maximum Average Subarray I

## Variable Sliding Window

- Longest Substring Without Repeating Characters
- Longest Repeating Character Replacement
- Minimum Window Substring
- Permutation in String
- Find All Anagrams in a String
- Minimum Size Subarray Sum
- Fruit Into Baskets
- Max Consecutive Ones III

---

# Not Sliding Window

- Two Sum II
- 3Sum
- Container With Most Water
- Trapping Rain Water
- Valid Palindrome

---

# Common Beginner Mistakes

- Thinking every two-pointer problem is Sliding Window.
- Forgetting to shrink the window.
- Updating the answer before making the window valid.
- Using Sliding Window on non-contiguous problems.
- Memorizing pointer movement instead of understanding pointer roles.

---

# Cheat Sheet

```
Subarray / Substring?

↓

YES

↓

Continuous?

↓

YES

↓

Reuse previous computation?

↓

YES

↓

Sliding Window


--------------------------

Need to scan every element once?

↓

Maintain best value so far?

↓

YES

↓

Forward Two Pointers
```

---

# Learning Goal

By the end of this section you should be able to:

- Recognize Sliding Window problems immediately.
- Distinguish Fixed vs Variable windows.
- Distinguish Sliding Window from general Two Pointers.
- Recognize Forward Two Pointer problems.
- Derive optimal solutions from brute force.
