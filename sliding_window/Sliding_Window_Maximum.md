# Sliding Window Maximum (LeetCode 239)

> **Pattern:** Sliding Window + Monotonic Deque  
> **Difficulty:** Hard  
> **Time:** O(n)  
> **Space:** O(k)

---

# Problem

Given an integer array `nums` and a window size `k`, return the maximum value in every sliding window.

Example

```text
nums = [1,3,-1,-3,5,3,6,7]
k = 3

Output

[3,3,5,5,6,7]
```

---

# My Initial Thought

My first intuition was:

1. Iterate through every window.
2. Calculate the maximum.
3. Push it into the answer.

```text
Window 1

1 3 -1

Maximum = 3

↓

Window 2

3 -1 -3

Maximum = 3

↓

Window 3

-1 -3 5

Maximum = 5
```

This obviously works.

Time Complexity

```text
O(n * k)
```

because every window scans `k` elements.

---

# Second Thought

I realized we don't always need to recalculate the maximum.

Suppose

```text
Window

5 3 4

Maximum = 5
```

Now slide.

```text
Window

3 4 2
```

The removed element was

```text
5
```

which was also the maximum.

Only now do we need to scan the window again.

Otherwise we can simply compare the new element with the current maximum.

Pseudo code

```text
max = maximum(first window)

for every new window

removed = nums[left]
added = nums[right]

if (added > max)

    max = added

else if (removed == max)

    max = scan current window

answer.push(max)
```

---

# Does this work?

Yes.

Example

```text
Window

3 1 5

Maximum = 5

↓

Window

1 5 2

Removed = 3

Added = 2
```

Removed wasn't the maximum.

Added isn't larger than maximum.

Maximum is still

```text
5
```

No scan needed.

---

Another example

```text
Window

5 3 4

↓

Window

3 4 2
```

Removed

```text
5
```

Since maximum left,

we must scan

```text
3 4 2
```

Maximum becomes

```text
4
```

---

# Problem with this approach

Worst case

```text
9 8 7 6 5 4 3

k = 3
```

Windows

```text
9 8 7

↓

8 7 6

↓

7 6 5

↓

6 5 4
```

Every slide removes the maximum.

Every time we scan the entire window.

Time Complexity

```text
O(n * k)
```

Still not optimal.

---

# Biggest Question

Can we know the next maximum

WITHOUT scanning again?

This is the entire motivation behind the optimal solution.

---

# Key Observation

Suppose our deque currently contains

```text
8 6 5
```

Now a new number arrives

```text
9
```

Question

Can

```text
8
```

ever become the maximum again?

No.

Why?

Because

```text
9 > 8
```

and

```text
9 entered later.
```

As the window slides,

```text
8

↓

will leave

BEFORE

9
```

Since

```text
9

is larger

and

stays longer,
```

8 can never become useful again.

So remove it.

Exactly the same for

```text
6
```

and

```text
5
```

They are permanently useless.

---

# Golden Rule

Whenever a new number arrives

remove every smaller number from the back.

Because

they can never become maximum again.

---

# Why do we store indices instead of values?

Suppose

```text
nums

1 3 -1 -3 5
```

We need to know

```text
Has this element left the window?
```

If we only store values

```text
3 -1
```

we don't know whether

```text
3
```

belongs to the current window.

Indices solve this.

Example

```text
Deque

[1,2]
```

means

```text
nums[1]

nums[2]
```

When

```text
deque.front < left
```

that index has left the window.

Remove it.

---

# Complete Example

## Step 1

Window

```text
[1]
```

Deque

```text
1
```

Nothing to compare.

---

## Step 2

New number

```text
3
```

Deque

```text
1
```

Question

```text
3 > 1 ?
```

Yes.

Will

```text
1
```

ever become maximum?

Never.

Remove it.

Deque

```text
3
```

---

## Step 3

New number

```text
-1
```

Question

```text
-1 > 3 ?
```

No.

Keep both.

Deque

```text
3 -1
```

First window

```text
1 3 -1
```

Maximum

```text
3
```

Answer

```text
[3]
```

---

## Step 4

Slide

Window

```text
3 -1 -3
```

Insert

```text
-3
```

Question

```text
-3 > -1 ?
```

No.

Deque

```text
3 -1 -3
```

Maximum

```text
3
```

Answer

```text
3 3
```

---

## Step 5

Window

```text
-1 -3 5
```

Now

```text
3
```

leaves the window.

Remove it from the front.

Deque

```text
-1 -3
```

Insert

```text
5
```

Compare with back

```text
5 > -3
```

Remove

```text
-3
```

Compare again

```text
5 > -1
```

Remove

```text
-1
```

Deque

```text
5
```

Maximum

```text
5
```

Answer

```text
3 3 5
```

Notice

We never scanned the window.

---

## Step 6

Window

```text
-3 5 3
```

Insert

```text
3
```

Question

```text
3 > 5 ?
```

No.

Deque

```text
5 3
```

Maximum

```text
5
```

Answer

```text
3 3 5 5
```

---

## Step 7

Window

```text
5 3 6
```

Insert

```text
6
```

Compare

```text
6 > 3
```

Remove

```text
3
```

Compare

```text
6 > 5
```

Remove

```text
5
```

Deque

```text
6
```

Maximum

```text
6
```

Answer

```text
3 3 5 5 6
```

---

## Step 8

Window

```text
3 6 7
```

Insert

```text
7
```

Question

```text
7 > 6 ?
```

Yes.

Remove

```text
6
```

Deque

```text
7
```

Maximum

```text
7
```

Final Answer

```text
3 3 5 5 6 7
```

---

# Complete Algorithm

For every element

### Step 1

Remove expired indices.

```text
while deque.front < left

remove front
```

---

### Step 2

Remove every smaller element from the back.

```text
while nums[last] < nums[right]

pop back
```

---

### Step 3

Insert current index.

```text
deque.push(right)
```

---

### Step 4

If first window formed

```text
right >= k-1
```

Front of deque

is always the answer.

```text
nums[deque.front]
```

---

# Optimal Code (JavaScript)

```javascript
maxSlidingWindow(nums, k) {
    const deque = []; // stores indices
    const result = [];

    let left = 0;

    for (let right = 0; right < nums.length; right++) {

        // Remove indices outside the window
        while (deque.length && deque[0] < left) {
            deque.shift();
        }

        // Remove all smaller elements
        while (
            deque.length &&
            nums[deque[deque.length - 1]] < nums[right]
        ) {
            deque.pop();
        }

        // Add current index
        deque.push(right);

        // First window completed
        if (right >= k - 1) {
            result.push(nums[deque[0]]);
            left++;
        }
    }

    return result;
}
```

---

# Why is it O(n)?

Many beginners think

```text
There is a while loop inside a for loop.

Therefore O(n²).
```

This is wrong.

Each index

```text
Gets inserted once.

↓

Gets removed from back at most once.

OR

Gets removed from front once.
```

No index ever comes back again.

Therefore

Total pushes

```text
n
```

Total pops

```text
≤ n
```

Overall operations

```text
O(n)
```

---

# Mental Model

Imagine you're hiring employees.

A stronger employee joins the company.

Every weaker employee behind him is fired immediately.

Why?

Because they'll never become the strongest while the stronger employee is still there.

The deque always keeps only the employees who still have a chance to become the strongest.

---

# Interview Thought Process

> Brute Force

Calculate maximum of every window.

```text
O(n*k)
```

↓

Notice repeated work.

↓

Keep track of current maximum.

↓

Only rescan if maximum leaves.

↓

Still

```text
O(n*k)
```

↓

Ask

> Can I avoid rescanning?

↓

Yes.

Keep only candidates for maximum.

↓

Maintain them in decreasing order.

↓

Use a Monotonic Deque.

↓

Optimal

```text
Time  : O(n)

Space : O(k)
```

---

# Key Takeaways

- Store **indices**, not values.
- Front of deque is always the current maximum.
- Remove expired indices from the front.
- Remove all smaller elements from the back.
- Every element enters and leaves the deque at most once.
- This is why the algorithm runs in **O(n)**.