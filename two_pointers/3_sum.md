# 3Sum

## Problem

Given an integer array `nums`, return all the **unique triplets** `[nums[i], nums[j], nums[k]]` such that:

- `i != j`
- `i != k`
- `j != k`
- `nums[i] + nums[j] + nums[k] == 0`

The solution set **must not contain duplicate triplets**.

---

# Pattern

- **Primary Pattern:** Two Pointers
- **Prerequisite:** Sorting
- **Idea:** Fix one element and reduce the problem to **Two Sum**

---

# Initial Thought 1: Brute Force

The first idea that came to mind was:

- Pick every possible combination of three numbers.
- Check if their sum is `0`.
- If yes, add them to the result.

## Code

```javascript
const result = [];

for (let i = 0; i < nums.length; i++) {

    for (let j = 0; j < nums.length; j++) {

        for (let k = 0; k < nums.length; k++) {

            if (
                i !== j &&
                j !== k &&
                i !== k &&
                nums[i] + nums[j] + nums[k] === 0
            ) {
                result.push([nums[i], nums[j], nums[k]]);
            }

        }

    }

}

return result;
```

---

## Problem

This generates the same triplet multiple times.

Example

```text
[-1,0,1]

Generated

[-1,0,1]
[-1,1,0]
[0,-1,1]
[0,1,-1]
[1,-1,0]
[1,0,-1]
```

All of these represent the same triplet.

---

## Better Brute Force

Instead of checking every permutation,

generate only combinations.

```javascript
for (let i = 0; i < n; i++) {

    for (let j = i + 1; j < n; j++) {

        for (let k = j + 1; k < n; k++) {

        }

    }

}
```

This removes permutation duplicates.

Still,

duplicate values like

```text
[-1,-1,2]
```

may appear multiple times because they come from different indices.

To solve that,

- Sort each triplet.
- Store it inside a `Set`.

---

## Time Complexity

```
O(n³)
```

---

## Space Complexity

```
O(n)
```

(for storing unique triplets)

---

# Initial Thought 2: Sort + Two Pointers

After solving Two Sum II,

the next thought was:

> "Since the array is sorted, let's use two pointers."

The first attempt looked like this:

```javascript
let left = 0;
let right = nums.length - 1;

while (left < right) {

    for (let k = left + 1; k < right; k++) {

        if (nums[left] + nums[k] + nums[right] === 0) {

            result.push(...);

        }

    }

}
```

---

## Why This Doesn't Work

At first glance it feels like a Two Pointer solution,

but there is a problem.

There are **three changing variables**.

```text
left

middle

right
```

Suppose

```text
sum > 0
```

Which pointer should move?

```text
left ?

middle ?

right ?
```

There is no clear answer.

The algorithm has no deterministic way to eliminate impossible combinations.

---

# The Important Realization

Two Pointers only works when there are **exactly two unknowns**.

Current equation

```text
a + b + c = 0
```

Three unknowns.

So first,

fix one number.

Suppose

```text
-4  -1  -1  0  1  2

 ^
 i
```

Now

```text
-4 + x + y = 0

↓

x + y = 4
```

Suddenly,

this becomes the **Two Sum** problem.

Now only

```text
left

right
```

are changing.

Now Two Pointers works perfectly.

---

# Correct Thinking

```text
Sort

↓

Outer loop fixes one element

↓

Remaining array becomes Two Sum

↓

Solve using Two Pointers
```

---

# First Implementation

```javascript
const sorted = nums.sort((a, b) => a - b);

const result = [];

for (let i = 0; i < sorted.length; i++) {

    let left = i + 1;
    let right = sorted.length - 1;

    while (left < right) {

        const sum =
            sorted[i] +
            sorted[left] +
            sorted[right];

        if (sum === 0) {

            result.push([
                sorted[i],
                sorted[left],
                sorted[right]
            ]);

        } else if (sum > 0) {

            right--;

        } else {

            left++;

        }

    }

}

return result;
```

---

# Mistake 1

After finding a triplet,

I forgot to move the pointers.

```javascript
if (sum === 0) {

    result.push(...);

}
```

Example

```text
[-1,0,1]
```

```
left = 1

right = 2

sum = 0

↓

Triplet found

↓

left and right never change

↓

Infinite loop
```

### Fix

```javascript
left++;

right--;
```

---

# Mistake 2

Duplicate First Element

Example

```text
[-4,-1,-1,0,1,2]
```

When

```text
i = 1

↓

-1
```

Triplets are generated.

Again

```text
i = 2

↓

-1
```

The same triplets are generated again.

### Fix

```javascript
if (i > 0 && nums[i] === nums[i - 1]) {

    continue;

}
```

---

# Mistake 3

Duplicate Left Pointer

Example

```text
[-2,0,0,2,2]
```

After finding

```text
[-2,0,2]
```

Moving only

```javascript
left++;
```

still points to another `0`.

The same answer is generated again.

### Fix

```javascript
while (
    left < right &&
    nums[left] === nums[left - 1]
) {

    left++;

}
```

---

# Mistake 4

Duplicate Right Pointer

Exactly the same issue.

Example

```text
[-2,0,2,2]
```

After moving

```javascript
right--;
```

another `2` is found.

Same triplet.

### Fix

```javascript
while (
    left < right &&
    nums[right] === nums[right + 1]
) {

    right--;

}
```

---

# Final Algorithm

1. Sort the array.
2. Traverse the array using `i`.
3. Skip duplicate values of `i`.
4. Set
   - `left = i + 1`
   - `right = n - 1`
5. While `left < right`
6. Calculate the sum.
7. If sum is `0`
   - Store triplet.
   - Move both pointers.
   - Skip duplicate left values.
   - Skip duplicate right values.
8. If sum < 0
   - Move left.
9. Else
   - Move right.

---

# Final Code

```javascript
nums.sort((a, b) => a - b);

const result = [];

for (let i = 0; i < nums.length - 2; i++) {

    if (i > 0 && nums[i] === nums[i - 1]) {
        continue;
    }

    let left = i + 1;
    let right = nums.length - 1;

    while (left < right) {

        const sum =
            nums[i] +
            nums[left] +
            nums[right];

        if (sum === 0) {

            result.push([
                nums[i],
                nums[left],
                nums[right]
            ]);

            left++;
            right--;

            while (
                left < right &&
                nums[left] === nums[left - 1]
            ) {
                left++;
            }

            while (
                left < right &&
                nums[right] === nums[right + 1]
            ) {
                right--;
            }

        } else if (sum < 0) {

            left++;

        } else {

            right--;

        }

    }

}

return result;
```

---

# Time Complexity

Sorting

```
O(n log n)
```

Outer loop

```
O(n)
```

Inner Two Pointer

```
O(n)
```

Total

```
O(n²)
```

---

# Space Complexity

Ignoring the output array,

```
O(1)
```

---

# Biggest Learnings

## Learning 1

Do **not** think of 3Sum as a Three Pointer problem.

Think

```text
Fix one

↓

Two Sum
```

---

## Learning 2

Whenever an array is sorted,

ask yourself

```text
Can Two Pointers solve it?
```

---

## Learning 3

Whenever the question asks for

```text
Unique pairs

Unique triplets

Unique combinations
```

always ask:

- Can duplicate fixed elements generate the same answer?
- Can duplicate left values generate the same answer?
- Can duplicate right values generate the same answer?

If yes,

skip duplicates.

---

# Pattern Summary

```text
2 Sum

↓

Two Pointers
```

```text
3 Sum

↓

Fix one

↓

Two Pointers
```

```text
4 Sum

↓

Fix one

↓

3 Sum

↓

Fix one

↓

Two Pointers
```

---

# Related Questions

- Two Sum II
- 3Sum Closest
- 4Sum
- Container With Most Water
- Trapping Rain Water
- Boats to Save People

---

# Interview Takeaway

The biggest mistake was trying to apply Two Pointers directly to three changing variables.

The correct way is to reduce the problem dimension:

```text
3Sum

↓

Fix one element

↓

2Sum

↓

Two Pointers
```

This "reduce the problem to a smaller known problem" is one of the most powerful problem-solving techniques in coding interviews.