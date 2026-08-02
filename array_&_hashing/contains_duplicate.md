# Contains Duplicate

## Problem

Given an integer array `nums`, return:

- `true` if any value appears more than once.
- `false` if all values are unique.

---

# Initial Thoughts

Whenever we need to find duplicates, there are multiple ways to think about the problem.

### Approach 1: Sort the Array

### Thought Process

If the array is sorted, all duplicate elements will come next to each other.

Example:

```text
Before Sorting
[4, 2, 1, 3, 2]

After Sorting
[1, 2, 2, 3, 4]
```

Now we only need to compare every element with the previous one.

If two adjacent elements are equal, we found a duplicate.

---

### Algorithm

1. Sort the array.
2. Store the first element.
3. Traverse the remaining elements.
4. Compare current element with previous.
5. If they are equal → return `true`.
6. Otherwise update previous element.
7. If traversal finishes → return `false`.

---

### Code

```javascript
const sortedNums = nums.sort((a, b) => a - b);

let prevNum = sortedNums[0];

for (let i = 1; i < sortedNums.length; i++) {
    if (prevNum === sortedNums[i]) {
        return true;
    }

    prevNum = sortedNums[i];
}

return false;
```

---

## Time Complexity

### Step 1: Sorting

Sorting `n` elements takes

```
O(n log n)
```

---

### Step 2: Traverse once

We visit every element once.

```
O(n)
```

---

### Total

```
O(n log n) + O(n)

= O(n log n)
```

Because **n log n** is larger than **n**.

---

## Space Complexity

We only use one extra variable.

```
prevNum
```

So,

```
O(1)
```

> Note:
> JavaScript's internal sorting algorithm may use extra memory depending on the engine, but in interviews we usually mention **O(1)** extra space (ignoring the sorting implementation).

---

# Approach 2: Count Frequency Using HashMap

## Thought Process

Instead of sorting, we can count how many times each number appears.

Example

```text
nums = [1,2,2,3]

Map

1 → 1
2 → 2
3 → 1
```

Now if any frequency is greater than 1, a duplicate exists.

---

## Algorithm

1. Create a HashMap.
2. Traverse the array.
3. Increase the frequency of each number.
4. Store all frequencies into an array.
5. Traverse the frequency array.
6. If any frequency is greater than 1 → return `true`.
7. Otherwise return `false`.

---

## Code

```javascript
const numFrequency = new Map();

for (let i = 0; i < nums.length; i++) {
    const num = nums[i];

    if (!numFrequency.has(num)) {
        numFrequency.set(num, 1);
    } else {
        numFrequency.set(num, numFrequency.get(num) + 1);
    }
}

const frequencyArray = Array.from(numFrequency.values());

for (let i = 0; i < frequencyArray.length; i++) {
    if (frequencyArray[i] > 1) {
        return true;
    }
}

return false;
```

---

## Time Complexity

### First Loop

We visit every element once.

```
O(n)
```

---

### Convert Map values into Array

Suppose there are `k` unique numbers.

```
O(k)
```

Worst case:

```
k = n
```

So,

```
O(n)
```

---

### Second Loop

Traverse the frequency array.

```
O(k)
```

Worst case:

```
O(n)
```

---

### Total

```
O(n)
+
O(n)
+
O(n)

= O(n)
```

We ignore constant factors.

---

## Space Complexity

The HashMap stores every unique number.

Worst case:

```
All numbers are unique.
```

```
Map size = n
```

Also,

```
frequencyArray size = n
```

Therefore,

```
O(n) + O(n)

= O(n)
```

---

# Approach 3 (Better): HashSet

## Thought Process

Do we really need to count frequencies?

No.

The moment we see the same number again, we already know the answer is `true`.

So instead of storing frequencies, we only store the numbers we have already seen.

Example

```text
nums = [3,1,4,2,1]

Seen Set

3
1
4
2

Next number = 1

Already exists

Return true
```

---

## Algorithm

1. Create an empty Set.
2. Traverse the array.
3. If the current number already exists in the Set, return `true`.
4. Otherwise add it to the Set.
5. If traversal finishes, return `false`.

---

## Code

```javascript
const seen = new Set();

for (const num of nums) {
    if (seen.has(num)) {
        return true;
    }

    seen.add(num);
}

return false;
```

---

## Time Complexity

We traverse the array only once.

For every element,

- `has()` → O(1)
- `add()` → O(1)

So,

```
n × O(1)

= O(n)
```

---

## Space Complexity

Worst case:

All numbers are unique.

The Set stores every element.

```
O(n)
```

---

# Comparison

| Approach | Time | Space |
|-----------|------|--------|
| Sort + Compare | O(n log n) | O(1)* |
| HashMap (Frequency) | O(n) | O(n) |
| HashSet (Seen Numbers) | O(n) | O(n) |

> **Recommended Interview Solution:** **HashSet**, because it is the simplest, fastest, and stops immediately when a duplicate is found.