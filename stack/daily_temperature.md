# Daily Temperatures (LeetCode 739)

> **Pattern:** Monotonic Decreasing Stack  
> **Difficulty:** Medium  
> **Time:** O(n)  
> **Space:** O(n)

---

# Problem Statement

Given an array `temperatures`, return an array `answer` such that

```text
answer[i]
```

is the number of days you have to wait after day `i` to get a warmer temperature.

If no warmer temperature exists,

```text
answer[i] = 0
```

---

## Example

```text
Input

temperatures =

[73,74,75,71,69,72,76,73]

Output

[1,1,4,2,1,1,0,0]
```

Explanation

```text
73 → wait 1 day (74)

74 → wait 1 day (75)

75 → wait 4 days (76)

71 → wait 2 days (72)

69 → wait 1 day (72)

72 → wait 1 day (76)

76 → no warmer day

73 → no warmer day
```

---

# Initial Thought (Brute Force)

For every temperature,

look to the right until we find a warmer temperature.

Count the number of days.

Store the answer.

---

## Brute Force Example

```text
73 74 75 71 69 72 76 73
```

For

```text
73
```

Search

```text
74
```

Found immediately.

Answer

```text
1
```

---

For

```text
75
```

Search

```text
71

69

72

76
```

Found after

```text
4
```

days.

---

## Brute Force Code

```javascript
class Solution {
    dailyTemperatures(temperatures) {

        const answer = new Array(temperatures.length).fill(0);

        for (let i = 0; i < temperatures.length; i++) {

            for (let j = i + 1; j < temperatures.length; j++) {

                if (temperatures[j] > temperatures[i]) {

                    answer[i] = j - i;
                    break;
                }
            }
        }

        return answer;
    }
}
```

---

## Complexity

Time

```text
O(n²)
```

Space

```text
O(1)
```

---

# Why Brute Force Is Slow

Consider

```text
73 74 75 71 69 72 76
```

When solving

```text
71
```

we search

```text
69

72

76
```

Later,

when solving

```text
69
```

we again search

```text
72

76
```

We repeatedly scan the same elements.

There must be a better way.

---

# Key Observation

Instead of asking

> Where is the next warmer day?

Ask

> Which temperatures are still waiting for a warmer day?

Example

```text
73
```

is waiting.

---

Next comes

```text
74
```

74 is warmer than 73.

So

```text
73
```

gets its answer.

---

Next comes

```text
75
```

75 is warmer than 74.

Now

```text
74
```

gets its answer.

---

Next comes

```text
71
```

It is not warmer than 75.

So

```text
71
```

starts waiting.

---

Next comes

```text
69
```

69 also waits.

---

Next comes

```text
72
```

72 is warmer than

```text
69
```

so 69 gets its answer.

72 is also warmer than

```text
71
```

so 71 gets its answer.

72 is NOT warmer than

```text
75
```

so we stop.

---

Notice something.

The most recently waiting temperature gets resolved first.

That is exactly

```text
Last In

↓

First Out

↓

STACK
```

---

# Why Store Indices Instead of Temperatures?

Suppose the stack stores

```text
73

74

75
```

When

```text
76
```

arrives,

we know

```text
76 > 75
```

But what should the answer be?

We need

```text
Current Index - Previous Index
```

If we only store temperatures,

we don't know their positions.

Instead,

store indices.

Example

```text
Stack

0

1

2
```

Corresponding temperatures

```text
73

74

75
```

Now

```text
76
```

arrives at index

```text
6
```

Answer for

```text
75
```

is

```text
6 - 2 = 4
```

Now it's easy.

---

# Optimal Intuition

The stack stores indices of temperatures that are **still waiting** for a warmer day.

Whenever a warmer temperature arrives,

it resolves one or more waiting days.

For each resolved day,

we calculate

```text
Current Index - Waiting Index
```

and store the answer.

Then the current day starts waiting,

so we push its index.

---

# Algorithm

Traverse the array from left to right.

For every temperature

### While

Current temperature is warmer than the temperature at the top index of the stack

```text
↓

Pop that index

↓

Calculate waiting days

↓

Store answer
```

Finally,

push the current index into the stack.

---

# Dry Run

```text
temperatures =

[73,74,75,71,69,72,76,73]
```

Initially

```text
Stack

[]

Answer

[0,0,0,0,0,0,0,0]
```

---

## Day 0

Temperature

```text
73
```

Stack empty

Push

```text
0
```

Stack

```text
[0]
```

---

## Day 1

Temperature

```text
74
```

74 > 73

Pop

```text
0
```

Answer

```text
1 - 0 = 1
```

Push

```text
1
```

Stack

```text
[1]
```

Answer

```text
[1,0,0,0,0,0,0,0]
```

---

## Day 2

Temperature

```text
75
```

75 > 74

Pop

```text
1
```

Answer

```text
2 - 1 = 1
```

Push

```text
2
```

Stack

```text
[2]
```

Answer

```text
[1,1,0,0,0,0,0,0]
```

---

## Day 3

Temperature

```text
71
```

71 is NOT warmer than 75

Push

```text
3
```

Stack

```text
[2,3]
```

---

## Day 4

Temperature

```text
69
```

69 is NOT warmer than 71

Push

```text
4
```

Stack

```text
[2,3,4]
```

---

## Day 5

Temperature

```text
72
```

72 > 69

Pop

```text
4
```

Answer

```text
5 - 4 = 1
```

---

Still

72 > 71

Pop

```text
3
```

Answer

```text
5 - 3 = 2
```

---

72 > 75?

No.

Push

```text
5
```

Stack

```text
[2,5]
```

Continue similarly.

Final answer

```text
[1,1,4,2,1,1,0,0]
```

---

# Optimal Code

```javascript
class Solution {
    /**
     * @param {number[]} temperatures
     * @return {number[]}
     */
    dailyTemperatures(temperatures) {

        const stack = []; // stores indices
        const answer = new Array(temperatures.length).fill(0);

        for (let i = 0; i < temperatures.length; i++) {

            while (
                stack.length &&
                temperatures[i] > temperatures[stack[stack.length - 1]]
            ) {

                const previousIndex = stack.pop();

                answer[previousIndex] = i - previousIndex;
            }

            stack.push(i);
        }

        return answer;
    }
}
```

---

# Complexity

### Time

Each index is

- pushed once
- popped once

Total

```text
O(n)
```

---

### Space

The stack may contain every index.

```text
O(n)
```

---

# Pattern Recognition

Whenever you read questions like

- Next Greater Element
- Next Smaller Element
- Previous Greater Element
- Previous Smaller Element
- Daily Temperatures
- Stock Span

Think

```text
Monotonic Stack
```

---

# Interview Explanation

> Instead of searching to the right for every temperature, I maintain a **monotonic decreasing stack** containing indices of temperatures whose warmer day hasn't been found yet. As I iterate through the array, whenever the current temperature is warmer than the temperature at the top index of the stack, I pop that index and calculate the number of waiting days as `currentIndex - poppedIndex`. Then I push the current index onto the stack. Since each index is pushed and popped at most once, the overall complexity is **O(n)**.

---

# Key Learnings

- Store **indices**, not temperatures.
- The stack contains **days still waiting** for a warmer temperature.
- A warmer temperature can resolve multiple waiting days.
- Each index is pushed and popped only once.
- This is a classic **Monotonic Decreasing Stack** problem.

---

# Cheat Sheet

```text
For every temperature

↓

Current temperature >

Top of stack temperature?

↓

YES

↓

Pop index

↓

answer[index] = currentIndex - index

↓

Repeat

----------------------------

Push current index

----------------------------

End

↓

Remaining indices

↓

No warmer day

↓

Answer remains 0
```