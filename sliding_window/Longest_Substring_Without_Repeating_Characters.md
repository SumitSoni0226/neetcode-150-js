# Longest Substring Without Repeating Characters (LeetCode 3)

> **Pattern:** Sliding Window (Variable Size)
>
> **Difficulty:** Medium

---

# Pattern Recognition

While reading the question, these words immediately stood out:

- **Substring**
- **Contiguous**
- **Longest**

This immediately suggests **Sliding Window**.

Reason:

A substring is always **continuous**, and Sliding Window is the go-to technique for processing continuous segments efficiently.

---

# My Initial Intuition

My first thought was:

> Since this is a **substring** problem, Sliding Window should work.

I imagined maintaining a window like this:

```
zxyxabc

Window

z
zx
zxy
zxyx
```

Whenever I encounter a duplicate, I'll remove characters from the left.

---

# Brute Force Intuition

Before jumping to Sliding Window, always think about the brute-force solution.

Question:

> How can I generate every possible substring?

Start from every index.

Example:

```
zxyxabc

Start at index 0

z
zx
zxy
zxyx
zxyxa
zxyxab
zxyxabc

Start at index 1

x
xy
xyx
xyxa
xyxab
xyxabc

Start at index 2

y
yx
yxa
yxab
yxabc

...
```

Now check every substring.

If it contains duplicate characters,

ignore it.

Otherwise,

update the maximum length.

---

# Brute Force Algorithm

```
Generate every substring

↓

Check whether substring has duplicate characters

↓

If unique

Update answer
```

---

# Brute Force Code

```javascript
let maxLength = 0;

for (let i = 0; i < s.length; i++) {

    let seen = new Set();

    for (let j = i; j < s.length; j++) {

        if (seen.has(s[j])) {
            break;
        }

        seen.add(s[j]);

        maxLength = Math.max(maxLength, j - i + 1);
    }
}

return maxLength;
```

---

# Complexity

Generating substrings

```
O(n²)
```

Checking duplicates

```
O(1)
```

(using HashSet while expanding)

Overall

```
O(n²)
```

---

# Observation

Notice these substrings

```
abc

↓

abcd
```

We're not creating an entirely new substring.

We're simply adding

```
d
```

Similarly,

```
bcd

↓

bcde
```

Again,

only one new character is added.

Lots of repeated work.

Can we reuse the previous substring instead?

Yes.

That's exactly what Sliding Window does.

---

# My Sliding Window Intuition

I thought:

Maintain one window.

If next character is not present,

expand.

If duplicate appears,

remove one character from the left.

My implementation:

```javascript
let left = 0;
let right = 0;

const slidingWindow = [];

while (right < s.length) {

    if (!slidingWindow.includes(s[right])) {

        slidingWindow.push(s[right]);
        right++;

    } else {

        slidingWindow.shift();
        left++;
    }
}
```

---

# Why My Solution Looks Correct

For

```
abcd
```

Window becomes

```
a

ab

abc

abcd
```

Everything works.

Even for

```
abca
```

```
abc

↓

duplicate a

↓

remove a

↓

bc

↓

add a

↓

bca
```

Still works.

So initially it feels correct.

---

# Flaw #1

Removing **only one character** is not always enough.

Example

```
abbc
```

Window

```
ab
```

Next character

```
b
```

Duplicate found.

My code removes only

```
a
```

Window becomes

```
b
```

The duplicate situation isn't resolved properly by a single removal in general.

Sometimes we must remove

```
1 character

2 characters

3 characters

...
```

until the duplicate disappears.

That's why the optimal solution uses

```javascript
while (...)
```

instead of

```javascript
if (...)
```

---

# Flaw #2

Using Array.includes()

```
slidingWindow.includes(ch)
```

takes

```
O(window size)
```

Worst case

```
O(n)
```

and it runs for every character.

Overall complexity becomes

```
O(n²)
```

---

# Better Data Structure

Instead of storing the window in an array,

store the characters inside a HashSet.

```
Set

↓

O(1)

lookup

insert

delete
```

Perfect for Sliding Window.

---

# Optimal Intuition

Maintain

```
left

right

HashSet
```

HashSet contains every character currently inside the window.

Whenever duplicate appears,

keep removing characters from the left

until the duplicate disappears.

Then continue expanding.

---

# Visualization

```
abcabcbb

left

↓

abc

right
```

Next character

```
a
```

Duplicate.

Remove from left

```
bc
```

Duplicate gone.

Now add

```
a
```

Window becomes

```
bca
```

Continue.

---

# Optimal Code

```javascript
class Solution {

    /**
     * @param {string} s
     * @return {number}
     */

    lengthOfLongestSubstring(s) {

        let left = 0;
        let maxLength = 0;

        const seen = new Set();

        for (let right = 0; right < s.length; right++) {

            while (seen.has(s[right])) {

                seen.delete(s[left]);

                left++;
            }

            seen.add(s[right]);

            maxLength = Math.max(
                maxLength,
                right - left + 1
            );
        }

        return maxLength;
    }
}
```

---

# Why while?

Question:

```
When is the window invalid?
```

Answer

```
Duplicate character exists.
```

Question

```
How do we make it valid?
```

Answer

```
Keep removing characters
from the left
until duplicate disappears.
```

Therefore

```
while

NOT

if
```

---

# Time Complexity

Every character

- enters the window once
- leaves the window once

Total operations

```
2n
```

```
O(n)
```

---

# Space Complexity

HashSet

```
O(min(n, charset))
```

For English letters,

maximum unique characters are limited.

---

# Interview Explanation

Brute Force

```
Generate every substring

↓

Check duplicates

↓

O(n²)
```

Observation

```
Adjacent substrings share most of their characters.

We are repeating work.
```

Optimization

```
Maintain one sliding window

↓

Expand right

↓

Duplicate?

↓

Shrink left

↓

Continue
```

Data Structure

```
HashSet

↓

O(1)

contains

insert

delete
```

Final Complexity

```
Time : O(n)

Space : O(n)
```

---

# What I Learned

✅ "Substring" usually indicates Sliding Window.

✅ Variable Sliding Window becomes invalid when duplicates appear.

✅ Don't remove just one character.

Remove until the window becomes valid.

✅ Array is not the right data structure for membership checks.

HashSet gives O(1) lookup.

✅ Every Sliding Window problem has one important question:

> **When does the current window become invalid?**

Once you answer that,

the algorithm becomes much easier to derive.