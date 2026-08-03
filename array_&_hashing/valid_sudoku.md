# Valid Sudoku

## Problem

Determine if a `9 × 9` Sudoku board is valid.

Only the filled cells need to be validated according to the following rules:

- Each row must contain the digits `1-9` without repetition.
- Each column must contain the digits `1-9` without repetition.
- Each `3 × 3` sub-box must contain the digits `1-9` without repetition.

Empty cells are represented by `'.'`.

---

# Initial Thoughts

Whenever we need to check whether duplicates exist under different constraints, the first idea is to compare every cell with every other relevant cell.

---

# Approach 1: Brute Force

## Thought Process

For every filled cell,

- Check the remaining cells in the same row.
- Check the remaining cells in the same column.
- Check the remaining cells in the same `3 × 3` box.

If any duplicate is found, return `false`.

Otherwise, continue checking.

---

## Algorithm

For every non-empty cell:

1. Traverse its row.
2. Traverse its column.
3. Traverse its `3 × 3` box.
4. If the same number is found again, return `false`.
5. If every cell satisfies the rules, return `true`.

---

## Pseudo Code

```javascript
for every cell:

    if cell is '.'
        continue

    check row

    check column

    check 3×3 box
```

---

## Time Complexity

There are

```
n²
```

cells.

For every cell we check

- Row → `O(n)`
- Column → `O(n)`
- Box → `O(n)`

Therefore

```
O(n² × n)

=

O(n³)
```

For a standard Sudoku

```
n = 9
```

which becomes constant time, but for a generalized Sudoku the complexity is

```
O(n³)
```

---

## Space Complexity

No extra data structures are used.

```
O(1)
```

---

# Observation

We are repeatedly checking the same rows, columns and boxes.

Can we remember what we've already seen?

**Yes.**

---

# Approach 2 (Optimal): HashSet (Constraint Validation)

## Thought Process

Instead of scanning rows, columns and boxes repeatedly,

maintain three HashSets:

- Row Set
- Column Set
- Box Set

As we traverse the board,

- If the current digit already exists in its row, column or box, the board is invalid.
- Otherwise, add it to the corresponding sets.

This avoids repeated scanning.

---

## Key Idea

Each digit should appear only once in

- its row,
- its column,
- and its `3 × 3` box.

HashSet allows duplicate detection in constant time.

---

## Box Identification

A `3 × 3` box is uniquely identified by

```javascript
Math.floor(row / 3) + "-" + Math.floor(col / 3)
```

Example

```text
row = 5
col = 7

↓

Box

1-2
```

---

## Algorithm

1. Create three HashSets:
   - Rows
   - Columns
   - Boxes
2. Traverse every cell.
3. Skip empty cells (`.`).
4. Create unique identifiers:
   - Row key
   - Column key
   - Box key
5. If any key already exists in the HashSet, return `false`.
6. Otherwise insert all three keys.
7. If traversal completes, return `true`.

---

## Code

```javascript
const seen = new Set();

for (let row = 0; row < 9; row++) {
    for (let col = 0; col < 9; col++) {

        const value = board[row][col];

        if (value === ".") continue;

        const rowKey = `row-${row}-${value}`;
        const colKey = `col-${col}-${value}`;
        const boxKey = `box-${Math.floor(row / 3)}-${Math.floor(col / 3)}-${value}`;

        if (
            seen.has(rowKey) ||
            seen.has(colKey) ||
            seen.has(boxKey)
        ) {
            return false;
        }

        seen.add(rowKey);
        seen.add(colKey);
        seen.add(boxKey);
    }
}

return true;
```

---

## Time Complexity

We visit every cell exactly once.

Generalized board

```
n × n
```

Traversal

```
O(n²)
```

Each HashSet operation

```
add()

has()

=

O(1)
```

Therefore

```
O(n²)
```

For LeetCode's fixed `9 × 9` board,

```
81 cells

↓

O(1)
```

because the board size never changes.

---

## Space Complexity

In the worst case,

we store entries for every row, column and box.

Generalized Sudoku

```
O(n²)
```

For LeetCode

```
O(1)
```

because the maximum number of stored entries is bounded by a constant.

---

# Pattern

## Category

**Array & Hashing (NeetCode)**

---

## Pattern

**HashSet (Constraint Validation / Duplicate Detection)**

---

## When to Identify This Pattern

Think about this pattern whenever the problem asks you to:

- Validate constraints.
- Detect duplicates.
- Ensure uniqueness.
- Check whether something already exists.

---

## Key Idea

Instead of repeatedly scanning rows, columns and boxes,

remember previously seen values using a HashSet.

This converts repeated searches into constant-time lookups.

---

# Related Questions

- Contains Duplicate
- Valid Anagram
- Happy Number
- Longest Consecutive Sequence
- Detect Cycle in Linked List (HashSet approach)

---

# What You Should Learn From This Problem

- HashSets are ideal for duplicate detection.
- Constraint validation problems often become simple with hashing.
- Nested loops do **not** always imply `O(n³)`—what matters is how much work is done per iteration.
- Always look for opportunities to replace repeated scanning with constant-time lookups.

---

# Comparison

| Approach | Time | Space |
|-----------|------|--------|
| Brute Force (scan row, column, box for every cell) | O(n³) | O(1) |
| HashSet (Constraint Validation) | O(n²) | O(n²) *(Generalized)* / O(1) *(9×9 Sudoku)* |

> **Recommended Interview Solution:** Use **HashSet** for constraint validation because it avoids repeated scanning and validates the board in a single traversal.