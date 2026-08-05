# Permutation in String (LeetCode 567)

> **Pattern:** Fixed Size Sliding Window + Frequency Map

---

# Pattern Recognition

When I read the question, I should notice:

```text
Check whether

any permutation of s1

exists as a substring of s2.
```

Ask myself:

```text
Substring?

↓

YES

↓

Contiguous?

↓

YES

↓

Length fixed?

↓

YES (always s1.length)

↓

Fixed Size Sliding Window
```

So before thinking about code, I should already recognize:

> **This is a Fixed Sliding Window problem.**

---

# My Initial Thought (Wrong)

I first thought:

```js
// Sort both strings
// Check if sorted s1 exists inside sorted s2
```

```js
return s2.split("").sort().join("")
    .includes(s1.split("").sort().join(""));
```

## Why It Fails

Example

```text
s1 = "ab"

s2 = "eidbaooo"
```

Sorted

```text
s1

↓

ab

s2

↓

abdeiooo
```

Now

```text
ab
```

appears inside

```text
abdeiooo
```

But sorting destroyed the original positions.

The problem asks

```text
Permutation should appear

as one contiguous substring.
```

Sorting the entire string loses that information.

---

# Second Thought (Better)

I noticed

```text
Permutation

↓

Same characters

↓

Same length
```

Therefore,

every candidate substring must have length

```text
s1.length
```

Example

```text
s1 = "ab"

Length = 2
```

Possible windows

```text
ei

id

db

ba

ao

oo

oo
```

Now compare each window.

---

# My Brute Force Solution

Idea

```text
Take every window

↓

Sort the window

↓

Sort s1

↓

Compare
```

Example

Window

```text
ba
```

Sort

```text
ab
```

Sorted s1

```text
ab
```

Equal

↓

Return true.

---

## Complexity

Suppose

```text
n = s2.length

m = s1.length
```

For every window

```text
Sort

↓

Compare
```

Sorting costs

```text
O(m log m)
```

Total windows

```text
O(n)
```

Overall

```text
O(n × m log m)
```

Correct

but not optimal.

---

# Biggest Observation

Ask myself

> **Why am I sorting?**

Example

```text
ab

↓

ab

ba

↓

ab
```

Sorting only helps because

```text
Both strings have

same character frequencies.
```

Sorting is not the real goal.

The real goal is

```text
Compare frequencies.
```

---

# Third Thought

Instead of sorting

Let's count characters.

Example

```text
ab
```

Frequency

```text
a -> 1

b -> 1
```

Window

```text
ba
```

Frequency

```text
a -> 1

b -> 1
```

Equal.

Permutation found.

No sorting needed.

---

# Intermediate Solution

Build

```text
Frequency Map of s1
```

Then for every window

```text
Build Frequency Map

↓

Compare Maps
```

Example

Window

```text
db
```

Frequency

```text
d -> 1

b -> 1
```

Not equal.

Next window

```text
ba
```

Frequency

```text
b -> 1

a -> 1
```

Equal.

Return true.

---

## Complexity

For every window

```text
Build Frequency Map

↓

O(m)
```

Total windows

```text
O(n)
```

Overall

```text
O(n × m)
```

Better than sorting.

Still not optimal.

---

# Final Observation

Now ask

> **Am I rebuilding something repeatedly?**

Yes.

Example

Current window

```text
ei
```

Frequency

```text
e -> 1

i -> 1
```

Next window

```text
id
```

Do I really need to count again?

No.

Only one character changed.

```text
Removed

e

Added

d
```

Everything else stayed the same.

---

# This Is The Sliding Window Optimization

Instead of rebuilding

```text
Current Window

ei

↓

Count Again
```

Update

```text
Remove

e

↓

Add

d
```

Done.

Only two updates.

---

Example

Current

```text
ei
```

Frequency

```text
e -> 1

i -> 1
```

Slide

```text
id
```

Update

```text
e--

↓

d++
```

New Frequency

```text
i -> 1

d -> 1
```

No rebuilding.

---

# Visual Sliding Window

```text
s2 = e i d b a o o o
      L R

Window = "ei"
```

Slide

```text
e i d b a o o o
  L   R

Window = "id"
```

Slide

```text
e i d b a o o o
    L   R

Window = "db"
```

Slide

```text
e i d b a o o o
      L   R

Window = "ba"
```

Found.

---

# Optimal Algorithm

Step 1

Create frequency map of

```text
s1
```

Step 2

Create frequency map of

```text
first window
```

Step 3

Compare maps.

If equal

Return true.

Step 4

Slide window.

```text
Remove left character

↓

Add right character

↓

Compare again
```

Repeat.

---

# Complexity

Building maps

```text
O(m)
```

Sliding

```text
O(n)
```

Each slide

```text
One decrement

+

One increment
```

Overall

```text
O(n)
```

Space

```text
O(1)
```

(26 lowercase letters)

---

# My Mistakes While Solving

## Mistake 1

Sorted the entire string.

Lost the contiguous property.

---

## Mistake 2

Realized window size should be fixed.

Good improvement.

---

## Mistake 3

Sorted every window.

Correct

but repeated work.

---

## Mistake 4

Built frequency map for every window.

Better

but still repeated work.

---

## Mistake 5

Didn't notice

```text
Only one character leaves

Only one character enters
```

This is exactly the Sliding Window optimization.

---

# How I Should Think Next Time

Question

↓

Contains

```text
Substring
```

↓

Length fixed?

↓

YES

↓

Fixed Sliding Window

↓

Need permutation?

↓

Same frequencies

↓

Need Frequency Map

↓

Window moves?

↓

Only one character removed

Only one character added

↓

Update Frequency Map

↓

Don't rebuild it.

---

# Learning

The biggest lesson from this problem is:

> **Sliding Window is not just about moving pointers.**

The real optimization comes from asking:

```text
What changed

after moving the window by one position?
```

If only one element leaves

and

one element enters,

then I should update my previous computation

instead of rebuilding everything.

That single observation transforms

```text
O(n × m)

↓

O(n)
```

and is the essence of the Sliding Window pattern.


# Code Progression

---

# Approach 1 - Sort Every Window (Brute Force)

### Intuition

Every permutation has the same sorted order.

Example

```
"ab"

↓

sort

↓

"ab"

"ba"

↓

sort

↓

"ab"
```

So if the sorted window equals sorted `s1`, we've found a permutation.

```javascript
function checkInclusion(s1, s2) {
    const windowSize = s1.length;

    if (windowSize > s2.length) return false;

    const sortedS1 = s1.split("").sort().join("");

    for (let i = 0; i <= s2.length - windowSize; i++) {
        const window = s2.slice(i, i + windowSize);

        if (window.split("").sort().join("") === sortedS1) {
            return true;
        }
    }

    return false;
}
```

### Complexity

```
Time : O(n × m log m)

Space : O(m)
```

where

```
n = s2.length

m = s1.length
```

---

# Approach 2 - Frequency Map For Every Window

### Observation

Why are we sorting?

Only to verify

```
Character frequencies match.
```

Instead of sorting, compare frequency maps.

```javascript
function checkInclusion(s1, s2) {
    const windowSize = s1.length;

    if (windowSize > s2.length) return false;

    const s1Map = new Map();

    for (const ch of s1) {
        s1Map.set(ch, (s1Map.get(ch) || 0) + 1);
    }

    for (let i = 0; i <= s2.length - windowSize; i++) {

        const windowMap = new Map();

        for (let j = i; j < i + windowSize; j++) {
            windowMap.set(s2[j], (windowMap.get(s2[j]) || 0) + 1);
        }

        if (mapsEqual(s1Map, windowMap)) {
            return true;
        }
    }

    return false;
}

function mapsEqual(map1, map2) {

    if (map1.size !== map2.size) return false;

    for (const [key, value] of map1) {
        if (map2.get(key) !== value) {
            return false;
        }
    }

    return true;
}
```

### Complexity

```
Time : O(n × m)

Space : O(26)
```

Still rebuilding the map every window.

---

# Approach 3 - Optimal Sliding Window

### Observation

When the window moves

```
abc

↓

bcd
```

Only two characters change.

```
Remove

a

Add

d
```

Everything else remains the same.

So instead of rebuilding the frequency map,

update it.

---

## Code

```javascript
function checkInclusion(s1, s2) {

    if (s1.length > s2.length) return false;

    const need = new Map();
    const window = new Map();

    // Frequency of s1
    for (const ch of s1) {
        need.set(ch, (need.get(ch) || 0) + 1);
    }

    // Build first window
    for (let i = 0; i < s1.length; i++) {
        window.set(s2[i], (window.get(s2[i]) || 0) + 1);
    }

    if (mapsEqual(need, window)) return true;

    let left = 0;

    // Slide window
    for (let right = s1.length; right < s2.length; right++) {

        // Add new character
        window.set(s2[right], (window.get(s2[right]) || 0) + 1);

        // Remove old character
        window.set(s2[left], window.get(s2[left]) - 1);

        if (window.get(s2[left]) === 0) {
            window.delete(s2[left]);
        }

        left++;

        if (mapsEqual(need, window)) {
            return true;
        }
    }

    return false;
}

function mapsEqual(map1, map2) {

    if (map1.size !== map2.size) return false;

    for (const [key, value] of map1) {
        if (map2.get(key) !== value) {
            return false;
        }
    }

    return true;
}
```

### Complexity

```
Time : O(n)

Space : O(26)
```

---

# Microsoft Interview Follow-up

The above solution is accepted and is **O(n)** because the alphabet size is fixed (26 lowercase letters). Comparing two maps is effectively constant time.

However, Microsoft interviewers often ask:

> **"Can you avoid comparing the entire map on every slide?"**

The optimized approach maintains a **`matches`** counter that tracks how many character frequencies currently match between `s1` and the window.

Instead of comparing all 26 counts every time:

- When one character enters the window, update its count and adjust `matches`.
- When one character leaves, update its count and adjust `matches`.
- If `matches === 26`, all frequencies match, so return `true`.

This also runs in **O(n)** but with a smaller constant factor and demonstrates a deeper understanding of the sliding window technique.