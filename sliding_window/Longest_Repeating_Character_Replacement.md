# Longest Repeating Character Replacement (LeetCode 424)

> **Pattern:** Variable Size Sliding Window + Frequency Map

---

# Pattern Recognition

When reading the question, ask yourself:

```text
Longest substring

↓

Substring?

↓

YES

↓

Contiguous?

↓

YES

↓

Can the window size change?

↓

YES

↓

Variable Size Sliding Window
```

This is **NOT** a fixed window because we don't know the answer length beforehand.

---

# Problem Statement

Given a string `s` and an integer `k`.

You can replace **at most k characters**.

Return the length of the **longest substring containing only one repeating character** after replacements.

Example

```
s = "ABAB"
k = 2

Answer = 4

Replace both B's with A.
```

---

# Example

```
s = "AABABBA"

k = 1
```

Possible answer

```
AABA

Replace B → A

AAAA

Length = 4
```

---

# My Initial Thought

My intuition was:

```text
Grow the window.

Whenever a different character appears,

replace it.

Use at most k replacements.

Once replacements finish,

start a new window.
```

---

## Why This Thought Is Wrong

Example

```
AABABBA

k = 1
```

Window

```
AABA
```

uses

```
1 replacement
```

Next

```
AABAB
```

Now

Should we restart?

No.

Maybe removing the first character makes the window valid again.

Restarting throws away useful work.

---

# Second Thought

I realized

Instead of manually replacing characters,

I should count frequencies.

Inside every window,

find

```
Most frequent character.
```

Everything else

can be replaced.

That is the key observation.

---

# Biggest Observation

Suppose the window is

```
AABAB
```

Frequency

```
A → 3

B → 2
```

If I want the entire window to become

```
AAAAA
```

I only need to replace

```
2

characters.
```

Notice

```
Window Size = 5

Highest Frequency = 3

Characters to replace

=

5 - 3

=

2
```

This gives the most important formula.

---

# Core Formula

```text
Replacements Needed

=

Window Size

-

Maximum Frequency
```

If

```
Window Size - MaxFrequency <= k
```

The window is valid.

Otherwise,

it is invalid.

---

# Why This Formula Works

Example

```
ABBBA
```

Frequency

```
A → 2

B → 3
```

Window Size

```
5
```

Highest Frequency

```
3
```

Need

```
5 - 3

=

2

replacements
```

Replace both

```
A

↓

B
```

Result

```
BBBBB
```

Perfect.

---

# Sliding Window Strategy

Grow the window.

Every time

```
Add right character.
```

Update frequency.

Calculate

```
Window Size

-

Max Frequency
```

If

```
<= k
```

Window is valid.

Expand further.

Otherwise

Shrink from left until valid again.

---

# Visual Example

```
A A B A B B A

L
R
```

Window

```
A
```

Valid.

Expand.

---

```
A A

L
  R
```

Valid.

Expand.

---

```
A A B

L
    R
```

Frequency

```
A = 2

B = 1
```

Need

```
3-2=1

replacement
```

Still valid.

Expand.

---

```
A A B A

L
      R
```

Frequency

```
A = 3

B =1
```

Need

```
4-3=1
```

Valid.

Expand.

---

```
A A B A B

L
        R
```

Frequency

```
A=3

B=2
```

Need

```
5-3=2
```

If

```
k=1
```

Invalid.

Shrink.

---

# Brute Force

For every starting index

```
i
```

Try every ending index

```
j
```

Calculate frequency.

Find maximum frequency.

Check

```
WindowSize-MaxFrequency
```

Update answer.

---

## Complexity

```
Time

O(n² × 26)

≈ O(n²)
```

---

# Better Idea

Don't rebuild frequency.

Maintain it while sliding.

Only

```
One character enters.

One character leaves.
```

Frequency updates become

```
O(1)
```

---

# Optimal Algorithm

1. Create frequency map.

2. Expand right.

3. Update frequency.

4. Update max frequency.

5. Calculate

```
WindowSize-MaxFrequency
```

6. If invalid

Remove left.

7. Update answer.

Repeat.

---

# Important Observation

Many beginners think

When shrinking

```
MaxFrequency

must also decrease.
```

Actually,

we do **NOT** decrease it.

Example

```
AAAAAB
```

Maximum Frequency

```
5
```

After shrinking

actual maximum may become

```
4
```

But we keep

```
5
```

Why?

Because

An overestimated maximum frequency

only makes the window appear **more valid**.

It never causes us to miss the correct answer.

It still guarantees

```
O(n)
```

This is a famous interview optimization.

---

# Optimal Code

```javascript
function characterReplacement(s, k) {

    let left = 0;

    let maxLength = 0;

    let maxFrequency = 0;

    const frequency = new Map();

    for (let right = 0; right < s.length; right++) {

        frequency.set(
            s[right],
            (frequency.get(s[right]) || 0) + 1
        );

        maxFrequency = Math.max(
            maxFrequency,
            frequency.get(s[right])
        );

        while ((right - left + 1) - maxFrequency > k) {

            frequency.set(
                s[left],
                frequency.get(s[left]) - 1
            );

            left++;
        }

        maxLength = Math.max(
            maxLength,
            right - left + 1
        );
    }

    return maxLength;
}
```

---

# Complexity

```
Time

O(n)
```

Every character

enters once

and

leaves once.

---

```
Space

O(26)

≈ O(1)
```

---

# My Mistakes While Solving

## Mistake 1

I tried to actually replace characters.

The problem only asks for

```
length
```

No replacement is required.

---

## Mistake 2

I wanted to restart the window after using all replacements.

Wrong.

Sliding Window teaches us

```
Never restart.

Shrink.
```

---

## Mistake 3

I initially thought

```
Different characters

=

HashMap.size
```

Wrong.

Example

```
AAABB
```

HashMap size

```
2
```

Replacements needed

are not

```
2
```

They are

```
5-3=2
```

The formula depends on

```
Window Size

-

Maximum Frequency
```

not on the number of distinct characters.

---

## Mistake 4

I calculated

```
HashMap.size

instead of

Window Size.
```

Remember

```
HashMap.size

=

Distinct Characters

NOT

Window Size.
```

---

## Mistake 5

I wanted to recalculate

```
Max Frequency
```

every time the window shrank.

That makes the solution slower.

Instead,

keep the historical maximum.

---

# Interview Explanation

If the interviewer asks

> **How did you derive this?**

Explain like this:

> I first noticed the question asks for the longest substring, so I thought of a variable-size sliding window. Instead of actually replacing characters, I realized I only need to know whether the current window can be made uniform using at most `k` replacements. The best target character is always the one with the highest frequency in the window because replacing all other characters minimizes the number of replacements. Therefore, the replacements needed are `windowSize - maxFrequency`. If that value exceeds `k`, I shrink the window from the left; otherwise, I expand it and update the maximum length.

---

# Pattern Learned

This problem teaches one of the most important Sliding Window ideas.

Many problems can be reduced to

```
Window Size

-

Something

<= Limit
```

Examples

```
Longest Repeating Character Replacement

↓

WindowSize-MaxFrequency<=k
```

```
Max Consecutive Ones III

↓

WindowSize-NumberOfOnes<=k
```

```
Maximize Confusion of an Exam

↓

WindowSize-MaxFrequency<=k
```

The pattern is always:

```
Expand

↓

Measure window validity

↓

Shrink if invalid

↓

Record answer
```

---

# Key Takeaways

- ✅ Recognize **substring** → think **Sliding Window**.
- ✅ This is a **Variable Size Sliding Window** problem.
- ✅ The key formula is `windowSize - maxFrequency <= k`.
- ✅ Don't actually replace characters; just **count frequencies**.
- ✅ Never restart the window—**shrink** it when it becomes invalid.
- ✅ `maxFrequency` is intentionally **not decreased** when shrinking, which keeps the solution linear.