# Group Anagrams

## Problem

Given an array of strings `strs`, group all the anagrams together.

You can return the answer in any order.

Example:

```text
Input:
["eat","tea","tan","ate","nat","bat"]

Output:
[
  ["eat","tea","ate"],
  ["tan","nat"],
  ["bat"]
]
```

---

# Initial Thoughts

Whenever we need to group strings that contain the same characters, we need a way to identify whether two strings are anagrams.

There are multiple ways to create a unique identifier (key) for every group of anagrams.

---

# Approach 1: Sort Each String (Most Intuitive)

## Thought Process

If two strings are anagrams, then after sorting their characters, both strings become identical.

Example

```text
eat

↓

aet


tea

↓

aet


ate

↓

aet
```

Since all three sorted strings are identical, they belong to the same group.

So,

- Key = Sorted String
- Value = Array of original strings

Example HashMap

```text
aet → ["eat","tea","ate"]

ant → ["tan","nat"]

abt → ["bat"]
```

Finally, return all the values of the HashMap.

---

## Algorithm

1. Create an empty HashMap.
2. Traverse every string.
3. Sort the current string.
4. Use the sorted string as the key.
5. If the key doesn't exist, create a new array.
6. Otherwise, append the current string to the existing array.
7. Return all the HashMap values.

---

## Code

```javascript
const hashMap = new Map();

for (const currentString of strs) {
    const sortedString = currentString
        .split("")
        .sort()
        .join("");

    if (!hashMap.has(sortedString)) {
        hashMap.set(sortedString, [currentString]);
    } else {
        hashMap.get(sortedString).push(currentString);
    }
}

return Array.from(hashMap.values());
```

> **Note:** Instead of creating a new array using the spread operator (`[...anagramArray, currentString]`), using `.push()` is more efficient because it modifies the existing array in place.

---

## Time Complexity

Suppose

- `n` = number of strings
- `k` = average length of each string

### Traverse all strings

```
O(n)
```

---

### Sort each string

Sorting one string of length `k`

```
O(k log k)
```

Since sorting is done for every string,

```
O(n × k log k)
```

---

### Total

```
O(n × k log k)
```

---

## Space Complexity

HashMap stores every string.

```
O(n × k)
```

The sorted strings also require extra space while creating the keys.

Overall,

```
O(n × k)
```

---

# Approach 2: Character Frequency Key (Most Optimal)

## Thought Process

Sorting every string is expensive.

Instead, count the frequency of each character.

Since the strings contain only lowercase English letters, we can create an array of size **26**.

Example

```text
eat

a : 1
e : 1
t : 1

Frequency

[1,0,0,0,1,0,0,...,1,...]
```

Now convert this frequency array into a string.

That frequency string becomes the HashMap key.

All anagrams will generate the same frequency array.

Example

```text
eat

1#0#0#0#1#...#1

tea

1#0#0#0#1#...#1

ate

1#0#0#0#1#...#1
```

All three map to the same key.

---

## Algorithm

1. Create an empty HashMap.
2. Traverse every string.
3. Create a frequency array of size 26.
4. Count every character.
5. Convert the frequency array into a unique string.
6. Use this string as the HashMap key.
7. Store the original string in the corresponding group.
8. Return all grouped values.

---

## Code

```javascript
const hashMap = new Map();

for (const word of strs) {
    const frequency = new Array(26).fill(0);

    for (const char of word) {
        frequency[char.charCodeAt(0) - 97]++;
    }

    const key = frequency.join("#");

    if (!hashMap.has(key)) {
        hashMap.set(key, []);
    }

    hashMap.get(key).push(word);
}

return Array.from(hashMap.values());
```

---

## Time Complexity

Let

- `n` = number of strings
- `k` = average length of each string

### Traverse all strings

```
O(n)
```

---

### Count characters

For every string

```
O(k)
```

Done for all strings

```
O(n × k)
```

---

### Convert frequency array to string

Frequency array size is always **26**.

```
O(26)

= O(1)
```

---

### Total

```
O(n × k)
```

---

## Space Complexity

HashMap stores every string.

Frequency array always contains only 26 elements.

Overall,

```
O(n × k)
```

---

# Why is Approach 2 Better?

Sorting takes

```
O(k log k)
```

Counting frequencies takes

```
O(k)
```

Since

```
k < k log k
```

The frequency-array solution is faster for long strings.

---

# Pattern

## Pattern Name

**Hashing + Frequency Counting**

### When to Identify This Pattern

Think about this pattern whenever the problem asks:

- Group anagrams
- Compare strings based on character frequencies
- Count characters
- Use a canonical representation as a HashMap key
- Categorize strings by their contents instead of their order

---

## Key Idea

Instead of comparing every string with every other string,

create a **unique key** that represents every anagram group.

Strings with the same key belong to the same group.

---

# Related Questions

## Easy

- Valid Anagram
- Find the Difference
- Ransom Note
- Isomorphic Strings
- Contains Duplicate

---

## Medium

- Group Anagrams
- Find All Anagrams in a String
- Top K Frequent Words
- Top K Frequent Elements
- Longest Substring Without Repeating Characters
- Minimum Window Substring

---

# What You Should Learn from This Problem

- HashMaps can be used for grouping, not just lookups.
- The hardest part is choosing the correct **key**.
- A sorted string is an easy canonical key.
- A frequency array is a more optimal canonical key when the character set is fixed.
- Always ask yourself:

> **"Can I convert similar objects into the same unique key?"**

---

# Comparison

| Approach | Time | Space |
|-----------|------|--------|
| Sort Each String | O(n × k log k) | O(n × k) |
| Frequency Array Key | O(n × k) | O(n × k) |

> **Recommended Interview Solution:** Use the **Frequency Array** approach when the input contains only lowercase English letters. Otherwise, use the **Sorted String** approach because it works for any character set.