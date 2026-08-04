# Container With Most Water

## Problem

You are given an integer array `heights` where each element represents the height of a vertical line.

Choose any two lines such that together with the x-axis they form a container that holds the maximum amount of water.

Return the maximum amount of water.

---

# Pattern

- **Primary Pattern:** Two Pointers
- **Technique:** Opposite-End Two Pointers
- **Key Observation:** Area depends on both width and the shorter height.

---

# Formula

The amount of water stored between two bars is

```text
Area = Width × Minimum Height

Area = (right - left) × min(height[left], height[right])
```

---

# Initial Thought 1: Brute Force

The first idea is to try every possible pair of bars.

For every pair:

- Calculate width.
- Calculate minimum height.
- Calculate area.
- Update the maximum answer.

---

## Algorithm

1. Pick first bar.
2. Pick every remaining bar.
3. Compute area.
4. Update maximum.

---

## Code

```javascript
let maxArea = 0;

for (let i = 0; i < heights.length; i++) {

    for (let j = i + 1; j < heights.length; j++) {

        const width = j - i;
        const height = Math.min(heights[i], heights[j]);

        maxArea = Math.max(maxArea, width * height);

    }

}

return maxArea;
```

---

## Time Complexity

Outer loop

```
O(n)
```

Inner loop

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

# Initial Thought 2

Instead of checking every pair,

notice the formula.

```text
Area = width × minimum height
```

The maximum possible width is obtained by choosing

```text
First bar

and

Last bar
```

So instead of trying every pair,

start with the widest possible container.

Then gradually reduce the width while trying to increase the limiting (smaller) height.

This naturally suggests using **Two Pointers**.

---

# Important Observation

The water level is always limited by the **shorter bar**.

Example

```text
Height = 2           Height = 10

Water can only rise up to 2.
```

The taller bar does not help once one side is shorter.

---

# Why Move the Smaller Pointer?

Suppose

```text
left = 2

right = 10
```

Current area

```text
2 × width
```

Now imagine moving the taller bar.

Width becomes smaller.

The limiting height is still

```text
2
```

So

```text
Area cannot increase.
```

The only hope of getting a larger area is to replace the shorter bar with a taller one.

Therefore,

Move the pointer pointing to the **smaller height**.

---

# Proof

Suppose

```text
height[left] = 4

height[right] = 10
```

Current area

```text
4 × width
```

### Case 1

Move right

```text
width ↓

minimum height still ≤ 4
```

Area cannot increase.

---

### Case 2

Move left

Maybe

```text
4

↓

8
```

Now

```text
minimum height

4

↓

8
```

Although width decreased,

the minimum height increased.

Area **may** increase.

Hence we always move the smaller pointer.

---

# Optimal Algorithm

1. Place one pointer at the beginning.
2. Place another pointer at the end.
3. Calculate current area.
4. Update maximum area.
5. Move the pointer having smaller height.
6. Repeat until pointers meet.

---

# Code

```javascript
let left = 0;
let right = heights.length - 1;

let maxWaterRetention = 0;

while (left < right) {

    const height = Math.min(
        heights[left],
        heights[right]
    );

    const width = right - left;

    const area = height * width;

    maxWaterRetention = Math.max(
        maxWaterRetention,
        area
    );

    if (heights[left] < heights[right]) {
        left++;
    } else {
        right--;
    }

}

return maxWaterRetention;
```

---

# Dry Run

Input

```text
[1,8,6,2,5,4,8,3,7]
```

Initially

```text
left = 1

right = 7

width = 8

Area = 8
```

Move left because

```text
1 < 7
```

Now

```text
left = 8

right = 7

width = 7

Area = 49
```

Current answer

```text
49
```

Continue until pointers meet.

Final answer

```text
49
```

---

# Why Doesn't Greedy by Height Work?

Suppose we simply choose the two tallest bars.

Example

```text
[100,1,1,100]
```

Works.

But consider

```text
[1,100,2,100]
```

The tallest bars are adjacent.

Width is very small.

A slightly shorter bar farther away may produce a larger area.

So choosing the tallest bars alone is not sufficient.

---

# Why Doesn't Greedy by Width Work?

Suppose we always choose the farthest bars.

Example

```text
[1,100,100]
```

Maximum width

```
2
```

Area

```
2 × 1 = 2
```

But

```text
100 and 100
```

give

```
1 × 100 = 100
```

Much larger.

Hence width alone is also insufficient.

---

# Time Complexity

Each iteration moves exactly one pointer.

Left pointer moves at most

```
n
```

times.

Right pointer also moves at most

```
n
```

times.

Together,

they perform at most

```
n - 1
```

moves.

Therefore

```
O(n)
```

---

# Space Complexity

Only a few variables are used.

```
O(1)
```

---

# Comparison

| Approach | Time | Space |
|----------|------|--------|
| Brute Force (Every Pair) | O(n²) | O(1) |
| Two Pointers | O(n) | O(1) |

---

# Pattern Recognition

Whenever you see:

- Choose **two elements**
- Array is **not required to be sorted**
- One pointer can start from each end
- Objective depends on both ends simultaneously
- One side can be safely eliminated after each comparison

Think:

```text
Opposite-End Two Pointers
```

---

# Similar Interview Questions

## Easy

- Valid Palindrome
- Two Sum II

---

## Medium

- 3Sum
- Boats to Save People
- Sort Colors
- Remove Duplicates from Sorted Array II

---

## Hard

- Trapping Rain Water

---

# What We Learned

### Learning 1

Not every problem should start with a HashMap.

Sometimes the structure of the formula itself suggests a better approach.

---

### Learning 2

Whenever the answer depends on **two ends of an array**, think about Two Pointers before considering nested loops.

---

### Learning 3

The most important insight is not the code—it is proving **why moving the shorter bar is always safe**. This proof is what interviewers often ask after you write the optimal solution.

---

# Interview Takeaway

The biggest breakthrough is recognizing that:

```text
Area = Width × Minimum Height
```

Width always decreases as pointers move.

Therefore, the only way to potentially increase the area is to increase the **minimum height**, which is possible only by moving the pointer pointing to the shorter bar.

This observation reduces the solution from **O(n²)** to **O(n)** and is the key insight expected in interviews.