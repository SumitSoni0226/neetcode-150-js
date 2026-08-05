# Best Time to Buy and Sell Stock

## Problem

You are given an array `prices` where:

- `prices[i]` is the price of a stock on day `i`.
- You may buy **one** stock and sell **one** stock.
- You **must buy before you sell**.

Return the maximum profit you can achieve.

If no profit is possible, return `0`.

---

# Pattern

- **Primary Pattern:** Sliding Window (Forward Moving Window) / Two Pointers
- **Core Idea:** Keep track of the **lowest buying price** seen so far while treating every future day as a possible selling day.

> **Important:** This problem is often categorized under Sliding Window because we maintain two forward-moving pointers (`left` = buy day, `right` = sell day). However, unlike classic sliding window problems, we are **not maintaining a valid subarray**. Instead, we're maintaining the **best buying day** seen so far.

---

# Initial Thought

The first intuition that came to my mind was:

- `left` should point to the **lowest buying price**.
- `right` should point to the **highest selling price**.
- Profit = `sell - buy`.

I initially tried to move both pointers depending on profit.

The difficulty was:

- When should I move the buy pointer?
- When should I move the sell pointer?
- What should be the loop condition?

---

# Brute Force Approach

## Thought Process

For every buying day, try selling on every future day and calculate the profit.

Example:

```text
prices = [7,1,5,3,6,4]

Buy at 7
    Sell at 1
    Sell at 5
    Sell at 3
    Sell at 6
    Sell at 4

Buy at 1
    Sell at 5
    Sell at 3
    Sell at 6
    Sell at 4

...
```

Calculate every possible profit and keep the maximum.

---

## Algorithm

1. Pick every day as the buying day.
2. For each buying day, check every later day as the selling day.
3. Calculate the profit.
4. Update the maximum profit.

---

## Code

```javascript
let maxProfit = 0;

for (let i = 0; i < prices.length; i++) {
    for (let j = i + 1; j < prices.length; j++) {
        maxProfit = Math.max(maxProfit, prices[j] - prices[i]);
    }
}

return maxProfit;
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

# Better Observation

Notice something.

Suppose today is the selling day.

To maximize profit, do we need to know **all previous prices**?

No.

We only need the **lowest buying price** before today.

Example

```text
Prices

7 3 5 4 1 8 9
```

When we are selling at `8`, we don't care about every previous price.

We only care about the minimum among

```text
7 3 5 4 1
```

which is

```text
1
```

So instead of checking every previous price repeatedly, we simply remember the **lowest price seen so far**.

---

# Why is this considered Sliding Window?

Unlike classic sliding window problems where we maintain a valid subarray,

here the pointers represent:

```text
left  = buying day
right = selling day
```

Both pointers move only forward.

Window:

```text
[left................right]
```

The window simply represents:

> "Buy on the left day and sell on the right day."

---

## Pointer Movement

Initially

```text
L R

7 1 5 3 6 4
```

If

```text
profit < 0
```

buying at `L` is useless because we've found a cheaper buying day.

So

```text
left = right
```

Otherwise

```text
right++
```

Keep checking future selling days.

---

# Why does only the Right Pointer Expand?

This was my biggest confusion.

Why don't we keep moving both pointers?

The answer comes from the problem statement.

Every day should be considered once as a possible **selling day**.

So

```text
right
```

must visit every day.

The left pointer only changes when we discover a better buying opportunity.

---

# Important Edge Case (The Biggest Doubt)

Consider

```text
7 3 5 100 1 8 9
```

Initially

```text
Buy = 3
Sell = 100

Profit = 97
```

Now we encounter

```text
1
```

Question:

If we move the buy pointer to `1`, won't we lose the huge profit of `97`?

No.

Because we have **already calculated** that profit.

It has already been stored in

```text
maxProfit
```

After passing day `100`, we can never sell there again.

So for **future selling days** (`8`, `9`...), buying at `1` is always better than buying at `3`.

This is why we safely update

```text
left = right
```

---

# Key Invariant

At every iteration:

```text
left always points to the minimum price
seen from day 0 to right.
```

Example

| Right Position | Prices Seen | Left Points To |
|---------------|------------|----------------|
| 3 | 7,3 | 3 |
| 5 | 7,3,5 | 3 |
| 4 | 7,3,5,4 | 3 |
| 1 | 7,3,5,4,1 | 1 |
| 8 | 7,3,5,4,1,8 | 1 |
| 9 | 7,3,5,4,1,8,9 | 1 |

---

# Optimal Solution

## Thought Process

Instead of checking every previous buying day,

we maintain:

- `lowestBuy`
- `highestSell`

If profit becomes negative,

it means we have found a cheaper buying day.

Move the buy pointer.

Otherwise,

keep moving the selling pointer.

---

## Algorithm

1. Initialize buy pointer at day `0`.
2. Initialize sell pointer at day `1`.
3. Calculate current profit.
4. Update maximum profit.
5. If current profit is negative:
   - Move buy pointer to current sell day.
6. Move sell pointer forward.
7. Continue until all selling days are processed.

---

## Code

```javascript
maxProfit(prices) {

    let profit = 0;
    let lowestBuy = 0;
    let highestSell = 1;

    while (highestSell < prices.length) {

        let currentProfit = prices[highestSell] - prices[lowestBuy];

        profit = Math.max(currentProfit, profit);

        if (currentProfit < 0) {
            lowestBuy = highestSell;
        }

        highestSell++;
    }

    return profit;
}
```

---

# Time Complexity

Each pointer only moves forward.

The sell pointer visits every element exactly once.

The buy pointer only moves forward when a lower price is found.

Total

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
| Brute Force | O(n²) | O(1) |
| Sliding Window / Running Minimum | O(n) | O(1) |

---

# What I Learned

- Not every Sliding Window problem maintains a valid subarray.
- Always derive the optimal solution from the brute force solution.
- If every future answer only depends on one value from the past, try maintaining that value instead of recomputing it.
- Here, that value is the **minimum price seen so far**.
- `right` always moves because every day should be considered as a selling day.
- `left` only moves when we discover a cheaper buying day.
- `maxProfit` stores the best answer found so far, so changing `left` later never loses previously calculated profits.

---

# Similar Microsoft SDE-2 Questions

This problem introduces the idea of maintaining information while traversing the array.

Related interview questions:

- Best Time to Buy and Sell Stock II
- Best Time to Buy and Sell Stock III
- Best Time to Buy and Sell Stock with Cooldown
- Maximum Subarray (Kadane's Algorithm)
- Maximum Product Subarray
- Longest Substring Without Repeating Characters
- Minimum Window Substring
- Sliding Window Maximum

These problems build intuition for maintaining state while scanning the array.