# Valid Palindrome

## Problem

Given a string `s`, return `true` if it is a palindrome, otherwise return `false`.

A palindrome reads the same forward and backward.

**Constraints:**

- Ignore non-alphanumeric characters.
- Ignore uppercase and lowercase differences.

Example:

```text
Input:
"A man, a plan, a canal: Panama"

Output:
true
```

---

# Initial Thoughts

The first idea is to reverse the string and compare it with the original.

However, before comparing, we must:

- Remove all non-alphanumeric characters.
- Convert everything to lowercase.

---

# Approach 1: Clean + Reverse + Compare

## Thought Process

1. Remove all non-alphanumeric characters.
2. Convert the string to lowercase.
3. Reverse the cleaned string.
4. Compare the original cleaned string with the reversed string.

If both are equal, the string is a palindrome.

---

## Algorithm

1. Convert the string to lowercase.
2. Remove all non-alphanumeric characters.
3. Reverse the cleaned string.
4. Compare both strings.

---

## Code

```javascript
const cleaned = s
    .toLowerCase()
    .replace(/[^a-z0-9]/g, "");

const reversed = cleaned
    .split("")
    .reverse()
    .join("");

return cleaned === reversed;
```

---

## Example

```text
Input

"A man, a plan, a canal: Panama"
```

Cleaned

```text
amanaplanacanalpanama
```

Reversed

```text
amanaplanacanalpanama
```

Result

```text
true
```

---

## Time Complexity

Cleaning

```
O(n)
```

Reverse

```
O(n)
```

Comparison

```
O(n)
```

Total

```
O(n)
```

---

## Space Complexity

We create

- Cleaned string
- Reversed string

Therefore

```
O(n)
```

---

# Observation

Do we really need another string?

No.

We only need to compare characters from both ends.

---

# Approach 2: Two Pointers (Optimal)

## Thought Process

Maintain two pointers.

- Left pointer starts from the beginning.
- Right pointer starts from the end.

Ignore all non-alphanumeric characters.

Compare only valid characters.

If they differ, return `false`.

Otherwise continue moving inward.

---

## Example

```text
"A man, a plan, a canal: Panama"
```

Pointers

```text
l                             r
A man, a plan, a canal: Panama
```

Compare

```text
A == a
```

Move inward.

Skip

```text
spaces

,

:
```

Continue until pointers cross.

---

## Algorithm

1. Initialize two pointers.
2. Skip non-alphanumeric characters from both sides.
3. Compare lowercase characters.
4. If different, return `false`.
5. Move both pointers.
6. Return `true`.

---

## Code

```javascript
let left = 0;
let right = s.length - 1;

while (left < right) {

    while (
        left < right &&
        !/[a-zA-Z0-9]/.test(s[left])
    ) {
        left++;
    }

    while (
        left < right &&
        !/[a-zA-Z0-9]/.test(s[right])
    ) {
        right--;
    }

    if (
        s[left].toLowerCase() !==
        s[right].toLowerCase()
    ) {
        return false;
    }

    left++;
    right--;
}

return true;
```

---

## Time Complexity

Each character is visited at most once.

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

Think about Two Pointers whenever:

- You need to compare both ends of an array or string.
- The problem involves palindromes.
- You need to avoid creating extra arrays or strings.
- You are traversing from both directions simultaneously.

---

## Key Idea

Instead of reversing the string,

compare characters directly from both ends.

Skip invalid characters and keep moving inward.

---

# Related Questions

- Valid Palindrome II
- Reverse String
- Reverse Vowels of a String
- Merge Sorted Array
- Squares of a Sorted Array
- Container With Most Water
- 3Sum
- Two Sum II – Input Array Is Sorted

---

# What You Should Learn From This Problem

- Two Pointers often eliminate the need for extra memory.
- Don't blindly compare characters—read the constraints carefully.
- Preprocessing (cleaning the string) is simple but uses extra space.
- Two Pointers provide the optimal solution with constant extra space.
- Always consider edge cases:
  - Empty string
  - String with only special characters
  - Mixed uppercase/lowercase letters
  - Numbers in the string

---

# Comparison

| Approach | Time | Space |
|-----------|------|--------|
| Clean + Reverse + Compare | O(n) | O(n) |
| Two Pointers | O(n) | O(1) |

> **Recommended Interview Solution:** Use the **Two Pointers** approach because it compares characters in-place, avoids creating additional strings, and achieves **O(1)** extra space.