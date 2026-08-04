# Trapping Rain Water

## Problem

You are given an array `height` where each element represents the height of a building.

After raining, water gets trapped between the buildings.

Return the total amount of trapped water.

Example

```text
height = [0,1,0,2,1,0,1,3,2,1,2,1]

Answer = 6
```

---

# Pattern

- **Primary Pattern:** Two Pointers
- **Other Patterns:** Prefix Maximum + Suffix Maximum
- **Key Observation:** Water at an index depends on the tallest bar on both sides.

---

# Formula

Water trapped at index `i`

```text
water[i] =
min(maxLeft, maxRight)
-
height[i]
```

Example

```text
      4
      |
4     |     5
|     |     |
| 2   | 1   |
------------
```

Current height

```
2
```

Maximum on left

```
4
```

Maximum on right

```
5
```

Water

```
min(4,5)-2

=2
```

---

# Initial Thought 1 : Brute Force

For every building,

find

- tallest building on the left
- tallest building on the right

Then calculate trapped water.

---

## Algorithm

For every index

1. Traverse left.
2. Find tallest wall.
3. Traverse right.
4. Find tallest wall.
5. Calculate trapped water.

---

## Code

```javascript
let water = 0;

for (let i = 0; i < height.length; i++) {

    let leftMax = 0;
    let rightMax = 0;

    for (let l = i; l >= 0; l--) {
        leftMax = Math.max(leftMax, height[l]);
    }

    for (let r = i; r < height.length; r++) {
        rightMax = Math.max(rightMax, height[r]);
    }

    water += Math.min(leftMax, rightMax) - height[i];
}

return water;
```

---

## Time Complexity

For every index

```
Find left max

O(n)
```

Find right max

```
O(n)
```

Total

```
O(n²)
```

---

## Space Complexity

```
O(1)
```

---

# Approach 2 : Prefix & Suffix Maximum Arrays

Instead of repeatedly finding left and right maximums,

store them once.

---

## Idea

Create

```text
leftMax[]

rightMax[]
```

Example

```text
height

0 1 0 2 1 0 1 3
```

leftMax

```text
0 1 1 2 2 2 2 3
```

rightMax

```text
3 3 3 3 3 3 3 3
```

Now every answer becomes

```text
min(leftMax[i], rightMax[i])

-

height[i]
```

---

## Algorithm

1. Build leftMax array.
2. Build rightMax array.
3. Traverse once.
4. Calculate trapped water.

---

## Code

```javascript
const n = height.length;

const leftMax = new Array(n);
const rightMax = new Array(n);

leftMax[0] = height[0];

for (let i = 1; i < n; i++) {
    leftMax[i] = Math.max(leftMax[i - 1], height[i]);
}

rightMax[n - 1] = height[n - 1];

for (let i = n - 2; i >= 0; i--) {
    rightMax[i] = Math.max(rightMax[i + 1], height[i]);
}

let water = 0;

for (let i = 0; i < n; i++) {

    water += Math.min(
        leftMax[i],
        rightMax[i]
    ) - height[i];

}

return water;
```

---

## Time Complexity

Build leftMax

```
O(n)
```

Build rightMax

```
O(n)
```

Calculate answer

```
O(n)
```

Total

```
O(n)
```

---

## Space Complexity

Two arrays

```
leftMax

rightMax
```

Total

```
O(n)
```

---

# Optimal Approach : Two Pointers

## Observation

Water at every index depends on

```text
Minimum of

left maximum

and

right maximum
```

Suppose

```text
leftMax = 3

rightMax = 10
```

Current water

```text
min(3,10)

=

3
```

Notice

Right side is already taller.

No matter how much taller it becomes,

minimum will still be

```text
3
```

Therefore,

we already know the answer for the left side.

So process the left pointer.

Similarly,

if

```text
rightMax < leftMax
```

process the right pointer.

This removes the need for extra arrays.

---

# Algorithm

1. Place two pointers.
2. Maintain leftMax.
3. Maintain rightMax.
4. If leftMax < rightMax
   - process left
   - move left
5. Else
   - process right
   - move right

---

## Code

```javascript
let left = 0;
let right = height.length - 1;

let leftMax = 0;
let rightMax = 0;

let water = 0;

while (left < right) {

    if (height[left] < height[right]) {

        if (height[left] >= leftMax) {
            leftMax = height[left];
        } else {
            water += leftMax - height[left];
        }

        left++;

    } else {

        if (height[right] >= rightMax) {
            rightMax = height[right];
        } else {
            water += rightMax - height[right];
        }

        right--;
    }
}

return water;
```

---

# Dry Run

```text
height

0 1 0 2 1 0 1 3
```

Initially

```text
left = 0

right = 7

leftMax = 0

rightMax = 3
```

Move left

```
0 water
```

Next

```
leftMax = 1
```

Next

```
height = 0

water = 1
```

Continue

Final answer

```
6
```

---

# Why Does Two Pointer Work?

Suppose

```text
leftMax = 4

rightMax = 8
```

Water level

```text
min(4,8)

=

4
```

Even if right side becomes

```text
20
```

Water level remains

```text
4
```

So left answer is already fixed.

We don't need to know the exact future right maximum.

This is the key insight.

---

# Time Complexity

Each pointer moves only once.

```
Left

O(n)
```

Right

```
O(n)
```

Together

```
O(n)
```

---

# Space Complexity

Only

```text
left

right

leftMax

rightMax
```

```
O(1)
```

---

# Comparison

| Approach | Time | Space |
|----------|------|--------|
| Brute Force | O(n²) | O(1) |
| Prefix & Suffix Arrays | O(n) | O(n) |
| Two Pointers | O(n) | O(1) |

---

# Pattern Recognition

Whenever you see

- nearest/tallest on both sides
- left influence
- right influence
- answer depends on left and right simultaneously

Think

```text
Prefix Maximum

Suffix Maximum
```

Then ask

```text
Can I remove the arrays?

↓

Two Pointers
```

---

# Similar Interview Questions

## Prefix / Suffix

- Product of Array Except Self
- Maximum Difference
- Buildings With an Ocean View

---

## Two Pointer

- Container With Most Water
- 3Sum
- Two Sum II
- Valid Palindrome

---

# What We Learned

### Learning 1

If every element needs information from both left and right, first think about **Prefix and Suffix arrays**.

---

### Learning 2

If prefix and suffix arrays can be replaced by maintaining running information from both ends, think about **Two Pointers**.

---

### Learning 3

The biggest interview insight is realizing that once one side's maximum is smaller than the other, the smaller side completely determines the water level for that pointer. This allows us to process one side confidently without knowing the exact future values on the opposite side.

---

# Interview Takeaway

Most candidates stop at the Prefix & Suffix solution.

Strong candidates recognize that storing both arrays is unnecessary.

By maintaining only

```text
leftMax

rightMax
```

and always processing the side with the smaller maximum, the space complexity improves from

```text
O(n)

↓

O(1)
```

This optimization is the expected solution in Microsoft, Google, Amazon, Meta, and other SDE-2 interviews.