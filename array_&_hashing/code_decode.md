# Encode and Decode Strings

## Problem

Design an algorithm to encode a list of strings into a single string and decode it back to the original list.

The encoded string should allow us to recover the original strings exactly, even if they contain special characters.

Example:

```text
Input:
["we","say",":","yes","!@#$%^&*()"]

Output:
Encoded String:
2#we3#say1#:3#yes10#!@#$%^&*()

Decoded Array:
["we","say",":","yes","!@#$%^&*()"]
```

---

# Initial Thoughts

Whenever we need to combine multiple strings into one, the first idea is usually to use a separator.

For example:

```javascript
return strs.join("#");
```

---

# Approach 1: Join Using a Separator (Incorrect)

## Thought Process

Join all strings using a special character like `#`.

Example

```text
["hello", "world"]

↓

hello#world
```

Later, split using `#`.

---

## Problem

What if the original string already contains `#`?

Example

```text
["hello#", "world"]
```

Encoded

```text
hello##world
```

After decoding

```text
["hello", "", "world"]
```

The original data is lost.

Therefore, using only a separator is **not reliable**.

---

# Approach 2: Store Length Before Every String (Optimal)

## Thought Process

Instead of relying on a separator between strings, store the length of each string before the string itself.

Format

```text
length#string
```

Examples

```text
we

↓

2#we
```

```text
say

↓

3#say
```

```text
!@#$%^&*()

↓

10#!@#$%^&*()
```

Combine everything

```text
2#we3#say1#:3#yes10#!@#$%^&*()
```

Now decoding becomes straightforward.

---

# Why is `#` Safe Here?

A common question is:

> **What if the original string also contains `#`?**

Example

```text
["ab#cd"]
```

Encoded

```text
5#ab#cd
```

During decoding:

- Read digits until the first `#`
- Length = `5`
- Read exactly **5 characters**

So the second `#` becomes part of the original string.

Therefore,

`#` is **not acting as a separator between strings**.

It only separates

```
length

and

string
```

---

# Encoding Algorithm

1. Create an empty string.
2. Traverse every string.
3. Append

```
length#string
```

4. Return the final encoded string.

---

## Encoding Code

```javascript
encode(strs) {
    let encoded = "";

    for (const str of strs) {
        encoded += str.length + "#" + str;
    }

    return encoded;
}
```

---

# Decoding Algorithm

1. Start from index `0`.
2. Read digits until `#`.
3. Convert those digits into the string length.
4. Read exactly `length` characters.
5. Add that substring to the answer.
6. Move the pointer after the extracted string.
7. Repeat until the encoded string ends.

---

## Decoding Code

```javascript
decode(str) {
    let pointer = 0;
    const result = [];

    while (pointer < str.length) {

        let j = pointer;

        while (str[j] !== "#") {
            j++;
        }

        const length = Number(str.slice(pointer, j));

        const word = str.slice(j + 1, j + 1 + length);

        result.push(word);

        pointer = j + 1 + length;
    }

    return result;
}
```

---

# Example Walkthrough

Encoded String

```text
2#we3#say1#:3#yes10#!@#$%^&*()
```

---

### Iteration 1

```text
2#we
```

Length

```text
2
```

Extract

```text
we
```

Move pointer

---

### Iteration 2

```text
3#say
```

Length

```text
3
```

Extract

```text
say
```

Move pointer

---

### Iteration 3

```text
1#:
```

Length

```text
1
```

Extract

```text
:
```

Move pointer

---

### Iteration 4

```text
3#yes
```

Extract

```text
yes
```

---

### Iteration 5

```text
10#!@#$%^&*()
```

Length

```text
10
```

Extract

```text
!@#$%^&*()
```

Final Result

```text
["we","say",":","yes","!@#$%^&*()"]
```

---

# Time Complexity

Let

- `n` = number of strings
- `m` = total number of characters across all strings

## Encoding

Every character is copied exactly once.

```
O(m)
```

---

## Decoding

Every character is read exactly once.

```
O(m)
```

---

# Space Complexity

The encoded string stores all characters.

The decoded array stores all original strings.

Overall

```
O(m)
```

---

# Common Mistakes

## 1. Using Only a Separator

```javascript
strs.join("#")
```

Fails when the original strings contain `#`.

---

## 2. Not Supporting Multi-digit Lengths

Example

```text
10#!@#$%^&*()
```

If you read only one digit (`1`) as the length, decoding fails.

Always read digits until `#`.

---

## 3. Incorrect Pointer Update

Incorrect

```javascript
pointer = length + 2;
```

Correct

```javascript
pointer = j + 1 + length;
```

Explanation:

- `j` → index of `#`
- `j + 1` → first character of the string
- `length` → number of characters to read

---

## 4. Off-by-One Error While Reading Characters

Incorrect

```javascript
for (let i = j + 1; i <= j + length + 1; i++)
```

This reads **one extra character**.

Correct

```javascript
for (let i = j + 1; i < j + 1 + length; i++)
```

or simply

```javascript
const word = str.slice(j + 1, j + 1 + length);
```

---

# Pattern

## Pattern Name

**String Encoding / Parsing**

---

## When to Identify This Pattern

Think about this pattern whenever the problem asks you to:

- Serialize and deserialize data.
- Encode multiple values into a single string.
- Recover original data exactly.
- Handle strings containing arbitrary characters.

---

## Key Idea

Instead of relying on separators that may appear in the data,

store **metadata (the length)** before the actual content.

This makes decoding deterministic.

---

# Related Questions

- Encode and Decode Strings
- Serialize and Deserialize Binary Tree
- Serialize and Deserialize BST
- Design TinyURL
- Decode String

---

# What You Should Learn From This Problem

- Separators alone are often unreliable.
- Prefixing data with its length is a common serialization technique.
- Always think about edge cases such as:
  - Empty strings
  - Multi-digit lengths
  - Special characters
  - Delimiter characters appearing in the original data

---

# Comparison

| Approach | Time | Space | Works for Any Characters? |
|-----------|------|--------|---------------------------|
| Join using `#` | O(m) | O(m) | ❌ No |
| `length#string` Encoding | O(m) | O(m) | ✅ Yes |

> **Recommended Interview Solution:** Use the **`length#string` encoding** approach because it correctly handles strings of any length and any character set while maintaining linear time complexity.