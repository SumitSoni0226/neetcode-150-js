# Minimum Window Substring (LeetCode 76)

> **Pattern:** Variable Sliding Window + Frequency Map
>
> **Difficulty:** Hard
>
> **Companies:** Microsoft, Google, Amazon, Meta, Uber

---

# Problem

Given two strings `s` and `t`, return the **minimum window substring** of `s` such that every character in `t` (including duplicates) is present in the window.

If no such substring exists, return `""`.

### Example

```text
s = "ADOBECODEBANC"
t = "ABC"

Output:

"BANC"
```

---

# Pattern Recognition

Whenever you read the question, ask yourself:

### Does it mention Substring?

✅ Yes

A substring is always **contiguous**.

↓

Think Sliding Window.

---

### Fixed or Variable Window?

We don't know the answer length.

Example

```
t = "ABC"
```

Possible answers

```
ABC
```

or

```
ADOBEC
```

or

```
BANC
```

Window size changes.

↓

Variable Sliding Window.

---

# My Initial Thought

My initial thinking was:

1. If `t.length > s.length`, return `""`.
2. Create a window of size `t.length`.
3. Compare frequency of every character.
4. If not valid, expand the window.
5. Once valid, shrink from the left.
6. As soon as removing one character makes the window invalid, stop shrinking.

---

# Is Starting with Window Size = t.length Wrong?

**No.**

This reasoning is logically correct.

Why?

A window smaller than `t.length` can **never** contain all required characters.

Example

```
t = "ABC"
```

These windows can never be valid

```
A
AB
BC
```

because we need at least 3 characters.

So starting with

```javascript
left = 0;
right = t.length - 1;
```

is completely valid.

However, most interview solutions start with an empty window because it creates a cleaner implementation with fewer special cases.

---

# Brute Force

## Intuition

Generate every possible substring.

For every substring,

calculate its frequency.

Compare it with `t`.

Keep the smallest valid substring.

---

### Example

```
s = ADOBEC
```

Check

```
A

AD

ADO

ADOB

ADOBE

ADOBEC

...

DOB

DOBE

DOBEC

...
```

Every substring.

---

## Complexity

Time

```
O(n³)
```

Space

```
O(m)
```

Very slow.

---

# Better Idea (My Approach)

Instead of checking every substring,

maintain a sliding window.

Window grows

↓

Until frequencies match.

Once valid

↓

Shrink from left.

Keep minimum answer.

---

# My First Working Idea

Maintain

```
t Frequency Map

Window Frequency Map
```

Whenever window changes,

compare both maps.

If every required frequency matches,

window is valid.

Otherwise,

keep expanding.

---

## Example

```
s = ADOBECODEBANC

t = ABC
```

Required

```
A -> 1

B -> 1

C -> 1
```

Window

```
ADO
```

Missing

```
B

C
```

Expand.

---

Window

```
ADOBEC
```

Now

```
A ✓

B ✓

C ✓
```

Window becomes valid.

---

Now shrink.

```
ADOBEC
```

↓

```
DOBEC
```

Still valid?

No

because

```
A
```

is removed.

Restore.

Continue expanding.

---

# My Frequency Map Solution

```javascript
class Solution {
    minWindow(s, t) {

        if (s.length < t.length) return "";

        const tFreqMap = new Map();

        for (const ch of t) {
            tFreqMap.set(ch, (tFreqMap.get(ch) || 0) + 1);
        }

        const windowFreqMap = new Map();

        let left = 0;
        let right = 0;

        let start = 0;
        let minLength = Infinity;

        while (right < s.length) {

            windowFreqMap.set(
                s[right],
                (windowFreqMap.get(s[right]) || 0) + 1
            );

            let isValid = true;

            for (const [char, freq] of tFreqMap.entries()) {

                if ((windowFreqMap.get(char) || 0) < freq) {
                    isValid = false;
                    break;
                }

            }

            while (isValid) {

                if (right - left + 1 < minLength) {
                    minLength = right - left + 1;
                    start = left;
                }

                windowFreqMap.set(
                    s[left],
                    windowFreqMap.get(s[left]) - 1
                );

                left++;

                isValid = true;

                for (const [char, freq] of tFreqMap.entries()) {

                    if ((windowFreqMap.get(char) || 0) < freq) {
                        isValid = false;
                        break;
                    }

                }

            }

            right++;
        }

        return minLength === Infinity
            ? ""
            : s.substring(start, start + minLength);
    }
}
```

---

# Is This Optimal?

❌ No.

Why?

Every time

```
Right expands
```

we compare the **entire frequency map**.

Again,

every time

```
Left shrinks
```

we compare the **entire frequency map**.

If

```
s length = n

t unique chars = m
```

Time

```
O(n × m)
```

---

# How Can We Improve?

Instead of comparing the whole map every time,

track only

```
How many required characters are already satisfied.
```

This introduces two variables.

```
need

have
```

---

# Need

Number of unique characters required.

Example

```
t = AABC
```

Frequency

```
A -> 2

B -> 1

C -> 1
```

Need

```
3
```

because

```
A

B

C
```

are the only unique characters.

---

# Have

Whenever one character reaches its required frequency,

increase

```
have
```

Example

Window

```
AA
```

Now

```
A
```

is satisfied.

```
have = 1
```

Window

```
AAB
```

Now

```
B
```

is satisfied.

```
have = 2
```

Window

```
AABC
```

Now

```
C
```

is satisfied.

```
have = 3
```

Since

```
have == need
```

Window becomes valid.

Notice

We never compare the whole frequency map.

---

# Optimal Algorithm

1. Expand the window.
2. Update current character frequency.
3. If one required character becomes satisfied,

```
have++
```

4. When

```
have == need
```

window becomes valid.

5. Record answer.

6. Shrink from left.

7. If shrinking breaks a required frequency,

```
have--
```

8. Expand again.

---

# Optimal Code

```javascript
class Solution {
    minWindow(s, t) {

        if (t.length > s.length) return "";

        const countT = new Map();
        const window = new Map();

        for (const c of t) {
            countT.set(c, (countT.get(c) || 0) + 1);
        }

        let have = 0;
        const need = countT.size;

        let left = 0;
        let res = [-1, -1];
        let resLen = Infinity;

        for (let right = 0; right < s.length; right++) {

            const c = s[right];

            window.set(c, (window.get(c) || 0) + 1);

            if (
                countT.has(c) &&
                window.get(c) === countT.get(c)
            ) {
                have++;
            }

            while (have === need) {

                if (right - left + 1 < resLen) {
                    res = [left, right];
                    resLen = right - left + 1;
                }

                const leftChar = s[left];

                window.set(leftChar, window.get(leftChar) - 1);

                if (
                    countT.has(leftChar) &&
                    window.get(leftChar) < countT.get(leftChar)
                ) {
                    have--;
                }

                left++;
            }
        }

        if (resLen === Infinity) return "";

        return s.substring(res[0], res[1] + 1);
    }
}
```

---

# Complexity Comparison

### Brute Force

Time

```
O(n³)
```

Space

```
O(m)
```

---

### Frequency Map Comparison

Time

```
O(n × m)
```

Space

```
O(m)
```

---

### Optimal

Time

```
O(n)
```

Space

```
O(m)
```

---

# Interview Learning

For this problem I learned:

- Minimum Window Substring is a **Variable Sliding Window** problem.
- The window becomes **valid** only when all required frequencies are satisfied.
- Once valid, greedily shrink to obtain the minimum answer.
- Starting with a window of size `t.length` is logically correct, but starting with an empty window leads to a cleaner implementation.
- Comparing the entire frequency map after every move is unnecessary.
- The `have` / `need` technique eliminates repeated comparisons and reduces the solution to **O(n)**.

---

# Mental Model

```
Expand Window

↓

Valid?

↓

No

↓

Expand

↓

Valid

↓

Record Answer

↓

Shrink

↓

Still Valid?

↓

Shrink Again

↓

Invalid?

↓

Expand Again
```

---

# Microsoft Interview Takeaway

An interviewer expects you to recognize:

- ✅ Substring → Sliding Window
- ✅ Unknown window size → Variable Sliding Window
- ✅ Frequency Map
- ✅ Expand until valid
- ✅ Shrink while valid
- ✅ `have / need` optimization for `O(n)`

This is one of the most important Sliding Window problems and teaches the core technique used in many hard interview questions.