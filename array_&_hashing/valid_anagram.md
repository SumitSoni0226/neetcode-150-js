# Valid Anagram

## Problem

Given two strings `s` and `t`, return:

- `true` if `t` is an anagram of `s`.
- `false` otherwise.

An **anagram** is a word or phrase formed by rearranging the letters of another word, using all the original letters exactly once.

---

# Initial Thoughts

Whenever we need to compare two strings based on the frequency of their characters, there are multiple ways to think about the problem.

---

# Approach 1: Sort Both Strings

## Thought Process

If two strings are anagrams, then after sorting both strings, they should become identical.

Example:

```text
s = "anagram"
t = "nagaram"

After Sorting

s = aaagmnr
t = aaagmnr
```

Now compare both sorted strings character by character.

If every character matches, the strings are anagrams.

---

## Algorithm

1. If the lengths are different, return `false`.
2. Sort both strings.
3. Traverse both sorted strings.
4. Compare characters at every index.
5. If any character differs, return `false`.
6. Otherwise return `true`.

---

## Code

```javascript
if (s.length !== t.length) return false;

const sortedS = s.split("").sort();
const sortedT = t.split("").sort();

for (let i = 0; i < s.length; i++) {
    if (sortedS[i] !== sortedT[i]) {
        return false;
    }
}

return true;
```

---

## Time Complexity

### Step 1: Sort First String

```
O(n log n)
```

---

### Step 2: Sort Second String

```
O(n log n)
```

---

### Step 3: Compare Both Strings

```
O(n)
```

---

### Total

```
O(n log n)
+
O(n log n)
+
O(n)

= O(n log n)
```

---

## Space Complexity

Sorting creates additional arrays after using `split()`.

```
O(n)
```

---

# Approach 2: Two HashMaps

## Thought Process

Instead of sorting, count the frequency of every character in both strings.

If both strings are anagrams, every character should have the same frequency.

Example

```text
s = "anagram"

a → 3
n → 1
g → 1
r → 1
m → 1


t = "nagaram"

a → 3
n → 1
g → 1
r → 1
m → 1
```

If every frequency matches, return `true`.

---

## Algorithm

1. If lengths differ, return `false`.
2. Create two HashMaps.
3. Count frequencies for both strings.
4. Traverse the first HashMap.
5. Compare frequencies with the second HashMap.
6. If any frequency differs, return `false`.
7. Otherwise return `true`.

---

## Code

```javascript
if (s.length !== t.length) return false;

const hashMapS = new Map();
const hashMapT = new Map();

for (const char of s) {
    hashMapS.set(char, (hashMapS.get(char) || 0) + 1);
}

for (const char of t) {
    hashMapT.set(char, (hashMapT.get(char) || 0) + 1);
}

for (const [char, count] of hashMapS.entries()) {
    if (hashMapT.get(char) !== count) {
        return false;
    }
}

return true;
```

---

## Time Complexity

### Build First HashMap

```
O(n)
```

---

### Build Second HashMap

```
O(n)
```

---

### Compare Frequencies

Suppose there are `k` unique characters.

```
O(k)
```

Worst case

```
k = n
```

So,

```
O(n)
```

---

### Total

```
O(n)
+
O(n)
+
O(n)

= O(n)
```

---

## Space Complexity

Both HashMaps together store all unique characters.

Worst case

```
O(n)
```

---

# Approach 3: Single HashMap (Better)

## Thought Process

Do we really need two HashMaps?

No.

We can:

- Increase the frequency while traversing the first string.
- Decrease the frequency while traversing the second string.

If both strings are anagrams, every frequency should become **0**.

Example

```text
s = "anagram"
t = "nagaram"

After processing s

a → 3
n → 1
g → 1
r → 1
m → 1

After processing t

a → 0
n → 0
g → 0
r → 0
m → 0

All frequencies become 0

Return true
```

---

## Algorithm

1. If lengths differ, return `false`.
2. Create one HashMap.
3. Traverse the first string and increase frequencies.
4. Traverse the second string and decrease frequencies.
5. Traverse the HashMap.
6. If any value is not `0`, return `false`.
7. Otherwise return `true`.

---

## Code

```javascript
if (s.length !== t.length) return false;

const frequencyMap = new Map();

for (const char of s) {
    frequencyMap.set(char, (frequencyMap.get(char) || 0) + 1);
}

for (const char of t) {
    frequencyMap.set(char, (frequencyMap.get(char) || 0) - 1);
}

for (const count of frequencyMap.values()) {
    if (count !== 0) {
        return false;
    }
}

return true;
```

---

## Time Complexity

### Traverse First String

```
O(n)
```

---

### Traverse Second String

```
O(n)
```

---

### Traverse HashMap

Suppose there are `k` unique characters.

Worst case

```
k = n
```

```
O(n)
```

---

### Total

```
O(n)
+
O(n)
+
O(n)

= O(n)
```

---

## Space Complexity

HashMap stores unique characters.

Worst case

```
O(n)
```

---

# Approach 4 (Most Optimal): Frequency Array

## Thought Process

The problem states that strings contain only lowercase English letters (`a-z`).

Instead of a HashMap, use an array of size **26**.

Each index represents one character.

```
index 0 -> a
index 1 -> b
...
index 25 -> z
```

Increase frequency while traversing `s`.

Decrease frequency while traversing `t`.

Finally, every value should become `0`.

---

## Algorithm

1. If lengths differ, return `false`.
2. Create an array of size 26 initialized with `0`.
3. Traverse both strings together.
4. Increment frequency for `s`.
5. Decrement frequency for `t`.
6. Traverse the frequency array.
7. If any value is not `0`, return `false`.
8. Otherwise return `true`.

---

## Code

```javascript
if (s.length !== t.length) return false;

const frequency = new Array(26).fill(0);

for (let i = 0; i < s.length; i++) {
    frequency[s.charCodeAt(i) - 97]++;
    frequency[t.charCodeAt(i) - 97]--;
}

for (const count of frequency) {
    if (count !== 0) {
        return false;
    }
}

return true;
```

---

## Time Complexity

Traverse both strings once.

```
O(n)
```

Traverse frequency array of size 26.

```
O(26)
```

Since 26 is a constant,

```
O(1)
```

---

### Total

```
O(n)
+
O(1)

= O(n)
```

---

## Space Complexity

The frequency array always contains **26 elements**, regardless of input size.

```
O(26)

= O(1)
```

---

# Pattern

## Pattern Name

**Hashing (HashMap / Frequency Counting)**

### When to Identify This Pattern

Think about using a HashMap or frequency array whenever the problem asks:

- Check if two strings are anagrams.
- Count character frequencies.
- Compare frequencies of elements.
- Find duplicates.
- Find unique elements.
- Perform fast lookups.
- Store previously seen values.

---

## Key Idea

Instead of repeatedly searching for characters, store their frequencies.

This reduces repeated work and helps achieve **O(n)** time complexity.

---

# Related Questions

## Easy

- Contains Duplicate
- Valid Anagram
- Ransom Note
- Find the Difference
- Isomorphic Strings
- Majority Element
- Intersection of Two Arrays

---

## Medium

- Group Anagrams
- Top K Frequent Elements
- Find All Anagrams in a String
- Longest Substring Without Repeating Characters
- Minimum Window Substring
- Subarray Sum Equals K

---

# What You Should Learn from This Problem

- Sorting is simple but increases time complexity to **O(n log n)**.
- HashMaps are useful when counting frequencies.
- One HashMap is better than maintaining two separate HashMaps.
- If the character set is fixed (like lowercase English letters), a frequency array is the most optimal solution.
- Always ask yourself:
  > **Can I solve this by counting frequencies instead of sorting?**

---

# Comparison

| Approach | Time | Space |
|-----------|------|--------|
| Sort + Compare | O(n log n) | O(n) |
| Two HashMaps | O(n) | O(n) |
| One HashMap | O(n) | O(n) |
| Frequency Array (26) | O(n) | O(1) |

> **Recommended Interview Solution:** Use a **Frequency Array** when the character set is fixed (e.g., lowercase English letters). Otherwise, use a **single HashMap**.