# Product of Array Except Self

## Problem

Given an integer array `nums`, return an array `answer` such that:

```text
answer[i] = product of all elements of nums except nums[i]
```

**Constraints:**

- Solve in **O(n)** time.
- **Do not use division.**

Example:

```text
Input:
nums = [1,2,3,4]

Output:
[24,12,8,6]
```

---

# Initial Thoughts

Whenever we need the product of all elements except the current one, there are multiple ways to think about the problem.

---

# Approach 1: Brute Force

## Thought Process

For every element, iterate over the entire array again and multiply every element except itself.

Example

```text
nums

[1,2,3,4]

For index 0

2 × 3 × 4 = 24

For index 1

1 × 3 × 4 = 12
```

Repeat this for every index.

---

## Algorithm

1. Traverse the array.
2. For every index, traverse the array again.
3. Skip the current index.
4. Multiply all remaining elements.
5. Store the result.

---

## Code

```javascript
const answer = [];

for (let i = 0; i < nums.length; i++) {
    let product = 1;

    for (let j = 0; j < nums.length; j++) {
        if (i === j) continue;

        product *= nums[j];
    }

    answer.push(product);
}

return answer;
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
O(n × n)

=

O(n²)
```

---

## Space Complexity

Ignoring the output array,

only one extra variable is used.

```
O(1)
```

---

# Approach 2: Total Product + Division

## Thought Process

Instead of calculating the product for every index separately,

calculate the product of the entire array first.

Then,

```text
answer[i]

=

Total Product

/

nums[i]
```

Example

```text
nums

[1,2,3,4]
```

Total Product

```text
24
```

Answer

```text
24/1 = 24

24/2 = 12

24/3 = 8

24/4 = 6
```

---

## Algorithm

1. Compute the product of all elements.
2. Traverse the array again.
3. Divide the total product by the current element.
4. Store the answer.

---

## Code

```javascript
let product = 1;

for (const num of nums) {
    product *= num;
}

const answer = [];

for (const num of nums) {
    answer.push(product / num);
}

return answer;
```

---

## Time Complexity

First traversal

```
O(n)
```

Second traversal

```
O(n)
```

Total

```
O(n)
```

---

## Space Complexity

Ignoring the output array,

```
O(1)
```

---

# Why This Approach Fails

## Case 1: One Zero

```text
nums

[1,2,0,4]
```

Total Product

```text
0
```

Now

```text
0 / 0
```

is undefined.

Expected answer

```text
[0,0,8,0]
```

This approach fails.

---

## Case 2: Multiple Zeros

```text
nums

[1,0,2,0]
```

Every answer should be

```text
[0,0,0,0]
```

Again,

division by zero makes this approach invalid.

---

## Another Problem

The question explicitly states

> **Do not use division.**

Therefore, even if we handled zero cases separately, this solution would still not satisfy the problem constraints.

---

# Approach 3 (Optimal): Prefix Product + Suffix Product

## Thought Process

Instead of finding

```text
Total Product ÷ Current Element
```

Think differently.

For every index,

```text
Product Except Self

=

Left Product

×

Right Product
```

Example

```text
nums

[1,2,3,4]
```

For index 2

```text
Left Product

1 × 2 = 2

Right Product

4

Answer

2 × 4 = 8
```

No division required.

---

## Key Idea

Build the answer in two passes.

### First Pass

Store the product of all elements to the **left**.

### Second Pass

Multiply the product of all elements to the **right**.

---

## Example

```text
nums

[1,2,3,4]
```

### Step 1

Store left products.

Initially

```text
answer

[1,1,1,1]
```

After first traversal

```text
answer

[1,1,2,6]
```

Explanation

```text
answer[0]

=

1

answer[1]

=

1

answer[2]

=

1×2

=

2

answer[3]

=

1×2×3

=

6
```

---

### Step 2

Traverse from right.

Maintain

```text
rightProduct
```

Multiply

```text
answer[3]

6 × 1

=

6

answer[2]

2 × 4

=

8

answer[1]

1 × 12

=

12

answer[0]

1 × 24

=

24
```

Final Answer

```text
[24,12,8,6]
```

---

## Algorithm

1. Create an answer array filled with `1`.
2. Traverse from left to right.
3. Store left product in the answer array.
4. Traverse from right to left.
5. Multiply the answer with the right product.
6. Return the answer.

---

## Code

```javascript
const answer = new Array(nums.length).fill(1);

let leftProduct = 1;

for (let i = 0; i < nums.length; i++) {
    answer[i] = leftProduct;
    leftProduct *= nums[i];
}

let rightProduct = 1;

for (let i = nums.length - 1; i >= 0; i--) {
    answer[i] *= rightProduct;
    rightProduct *= nums[i];
}

return answer;
```

---

## Time Complexity

First traversal

```
O(n)
```

Second traversal

```
O(n)
```

Total

```
O(n)
```

---

## Space Complexity

Extra variables

```text
leftProduct

rightProduct
```

Only constant extra space is used.

Ignoring the output array,

```
O(1)
```

---

# Pattern

## Category

**Array & Hashing (NeetCode)**

---

## Pattern

**Prefix Product + Suffix Product**

(Also known as **Prefix & Suffix Arrays**.)

---

## When to Identify This Pattern

Think about this pattern whenever:

- The answer at every index depends on everything to the **left** and **right**.
- Division is not allowed.
- You need cumulative information from both directions.
- The result can be built using two traversals.

---

## Key Idea

For every index,

```text
Answer

=

Left Product

×

Right Product
```

Instead of recomputing products repeatedly, reuse cumulative products from both directions.

---

# Related Questions

- Trapping Rain Water
- Best Time to Buy and Sell Stock
- Maximum Product Subarray
- Running Sum of 1D Array
- Find Pivot Index

---

# What You Should Learn From This Problem

- A brute-force solution can often be optimized by reusing previous computations.
- Prefix and suffix techniques eliminate repeated work.
- Division-based solutions may fail due to edge cases and problem constraints.
- Prefix and suffix products naturally handle zeros without any special logic.
- The output array itself can be reused to achieve **O(1)** extra space.

---

# Comparison

| Approach | Time | Space | Valid? |
|-----------|------|--------|--------|
| Brute Force | O(n²) | O(1) | ✅ |
| Total Product + Division | O(n) | O(1) | ❌ (division not allowed, fails with zeros) |
| Prefix Product + Suffix Product | O(n) | O(1) | ✅ |

> **Recommended Interview Solution:** Use the **Prefix Product + Suffix Product** approach because it satisfies all constraints, avoids division, handles zeros naturally, and runs in **O(n)** time with **O(1)** extra space.