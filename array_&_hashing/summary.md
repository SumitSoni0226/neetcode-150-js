# NeetCode 150 - Array & Hashing
# Section Summary

---

# What Did We Learn?

The **Array & Hashing** section is not about learning only HashMaps.

It teaches **how to recognize different patterns that can be solved efficiently using arrays, strings, hashing, and simple preprocessing techniques.**

By the end of this section, you should be able to answer questions like:

- Should I use a HashSet or HashMap?
- Do I need frequency counting?
- Should I sort first?
- Can I preprocess prefix/suffix information?
- Can I avoid repeated work?
- How do I design a custom encoding?
- How do I validate constraints efficiently?

---

# Pattern 1: Duplicate Detection

## Idea

Whenever the question asks

- Is there any duplicate?
- Have we already seen this element?

Think

```text
HashSet
```

---

## Key Insight

Instead of searching the entire array repeatedly,

store previously seen elements.

Searching

```text
O(n)
```

becomes

```text
O(1)
```

using a HashSet.

---

## Problems Practiced

- ✅ Contains Duplicate

---

## Similar Microsoft Questions

- Contains Duplicate
- Happy Number
- Detect Cycle in Linked List (HashSet solution)
- Intersection of Two Arrays
- Remove Duplicates from Sorted Array
- Find the Duplicate Number *(different optimal solution exists)*

---

# Pattern 2: Frequency Counting

## Idea

Whenever the problem asks

- Count occurrences
- Compare frequencies
- Check character counts

Think

```text
HashMap
```

---

## Key Insight

Store

```text
Element → Frequency
```

instead of repeatedly counting.

---

## Problems Practiced

- ✅ Valid Anagram

---

## Similar Microsoft Questions

- Ransom Note
- Find All Anagrams in a String
- Majority Element
- First Unique Character in a String
- Word Pattern
- Isomorphic Strings

---

# Pattern 3: Complement Lookup

## Idea

Whenever you need

```text
Target

=

Current + Something
```

Think

```text
HashMap
```

---

## Key Insight

Instead of searching for the second number,

store what you've already seen.

---

## Problems Practiced

- ✅ Two Sum

---

## Similar Microsoft Questions

- Two Sum II
- 4Sum
- 3Sum
- Two Sum BST
- Two Sum Less Than K

---

# Pattern 4: Canonical Representation

## Idea

Different inputs can represent the same thing.

Convert them into one common representation.

Example

```text
eat

tea

ate
```

↓

```text
aet
```

---

## Key Insight

Generate a unique key.

Store

```text
Key

↓

Group
```

---

## Problems Practiced

- ✅ Group Anagrams

---

## Similar Microsoft Questions

- Group Shifted Strings
- Find Duplicate Files in System
- Accounts Merge
- Group Strings

---

# Pattern 5: Bucket Sort

## Idea

When frequencies are bounded,

sorting is unnecessary.

Use buckets.

---

## Key Insight

Frequency

```text
1

↓

Bucket[1]
```

Frequency

```text
4

↓

Bucket[4]
```

Instead of sorting

```text
O(n log n)
```

Bucket Sort gives

```text
O(n)
```

---

## Problems Practiced

- ✅ Top K Frequent Elements

---

## Similar Microsoft Questions

- Top K Frequent Words
- Sort Characters By Frequency
- K Closest Points to Origin *(Heap is more common)*
- Find K Frequent Numbers

---

# Pattern 6: String Serialization

## Idea

Convert structured data into one string.

Later recover it exactly.

---

## Key Insight

Never depend only on separators.

Instead,

store metadata.

```text
length#string
```

---

## Problems Practiced

- ✅ Encode and Decode Strings

---

## Similar Microsoft Questions

- Serialize and Deserialize Binary Tree
- Serialize and Deserialize BST
- Design TinyURL
- Decode String

---

# Pattern 7: Prefix & Suffix Computation

## Idea

Answer at index

```text
i
```

depends on

everything

to the left

and

to the right.

---

## Key Insight

Precompute information.

Reuse it.

Avoid recalculating.

---

## Problems Practiced

- ✅ Product of Array Except Self

---

## Similar Microsoft Questions

- Trapping Rain Water
- Find Pivot Index
- Running Sum of 1D Array
- Maximum Product Subarray
- Best Time to Buy and Sell Stock *(different implementation)*

---

# Pattern 8: Constraint Validation

## Idea

Validate

- uniqueness
- rules
- constraints

---

## Key Insight

Maintain HashSets while traversing.

Instead of checking repeatedly,

remember what you've already seen.

---

## Problems Practiced

- ✅ Valid Sudoku

---

## Similar Microsoft Questions

- Valid Sudoku
- N-Queens (constraint tracking)
- Word Search
- Happy Number
- Verify Preorder Serialization of Binary Tree

---

# Pattern 9: Sequence Detection

## Idea

Need to find

longest

continuous

consecutive

sequence.

---

## Key Insight

Don't start counting from every element.

Start only from the beginning of a sequence.

```text
num-1

doesn't exist
```

---

## Problems Practiced

- ✅ Longest Consecutive Sequence

---

## Similar Microsoft Questions

- Longest Consecutive Sequence
- Longest Increasing Subsequence *(different DP pattern)*
- Number of Longest Increasing Subsequences
- Consecutive Characters
- Longest Arithmetic Subsequence *(different DP pattern)*

---

# Complexity Lessons

One of the biggest learnings from this section is understanding **why a solution is fast**, not just memorizing the final complexity.

---

## Nested Loops ≠ Always O(n²)

Example

```javascript
for (...) {
    while (...) {
    }
}
```

can still be

```text
O(n)
```

if every element is processed only once overall.

Example

- Longest Consecutive Sequence

---

## Sorting Isn't Always the Best Choice

Sorting often simplifies the problem.

But

```text
Sorting

↓

O(n log n)
```

Sometimes HashMaps can solve the same problem in

```text
O(n)
```

Examples

- Contains Duplicate
- Group Anagrams
- Top K Frequent Elements

---

## Avoid Repeated Work

Many brute-force solutions repeatedly compute the same thing.

Instead,

reuse previous computations.

Examples

- Product Except Self
- Longest Consecutive Sequence

---

## Think About Constraints

Whenever the question says

```text
Without Division

Without Extra Sorting

O(n)

In-place
```

the constraint is usually hinting toward the intended pattern.

---

# Interview Mindset

Instead of immediately coding,

ask yourself

### 1. Do I need fast lookup?

↓

HashSet / HashMap

---

### 2. Am I counting frequencies?

↓

HashMap

---

### 3. Am I looking for a complement?

↓

HashMap

---

### 4. Can sorting simplify this?

↓

Sorting

---

### 5. Can I preprocess left/right information?

↓

Prefix & Suffix

---

### 6. Am I repeatedly doing the same work?

↓

Reuse previous computations.

---

### 7. Can I uniquely represent an object?

↓

Canonical Key

---

### 8. Am I validating constraints?

↓

HashSet

---

### 9. Can I process every element only once?

↓

Look for amortized O(n).

---

# Microsoft SDE-2 Focus

Microsoft interviews frequently test **fundamental patterns** rather than obscure algorithms. From this section, the highest-value patterns are:

| Priority | Pattern | Common Microsoft Questions |
|----------|----------|----------------------------|
| ⭐⭐⭐⭐⭐ | HashMap / HashSet | Two Sum, Valid Anagram, Contains Duplicate, Happy Number |
| ⭐⭐⭐⭐⭐ | Frequency Counting | Top K Frequent, Majority Element, Find All Anagrams |
| ⭐⭐⭐⭐☆ | Prefix & Suffix | Product Except Self, Trapping Rain Water |
| ⭐⭐⭐⭐☆ | Constraint Validation | Valid Sudoku, N-Queens |
| ⭐⭐⭐⭐☆ | Canonical Representation | Group Anagrams, Accounts Merge |
| ⭐⭐⭐⭐☆ | Sequence Detection | Longest Consecutive Sequence |
| ⭐⭐⭐☆☆ | Bucket Sort | Top K Frequent Elements |
| ⭐⭐⭐☆☆ | Serialization | Serialize/Deserialize Tree, Design TinyURL |

---

# Final Takeaways

By completing the **Array & Hashing** section, you have learned to:

- Choose between **HashSet** and **HashMap** based on the problem.
- Use **frequency counting** instead of repeated searches.
- Apply **complement lookup** to reduce nested loops.
- Group similar data using a **canonical representation**.
- Replace sorting with **Bucket Sort** when frequencies are bounded.
- Design robust **serialization** formats for strings and data structures.
- Use **prefix and suffix preprocessing** to avoid redundant calculations.
- Validate complex constraints efficiently using **HashSets**.
- Detect sequences in **O(n)** by avoiding repeated work.
- Analyze time complexity carefully, understanding that nested loops do **not** always imply `O(n²)`.

---

# What's Next?

The next major NeetCode section is **Two Pointers**, where you'll learn how to optimize problems involving sorted arrays, strings, and linked lists by moving pointers intelligently instead of using extra space.