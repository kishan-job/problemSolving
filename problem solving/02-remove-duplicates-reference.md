# Remove Duplicates from an Array — Complete 8-Phase Reference

> **How a human should think through this problem step-by-step, from scratch to mastery.**

---

## PHASE 1 — UNDERSTANDING THE PROBLEM

### 1.1 — First Read (Get the Gist)

I have an array. It might contain repeated values. I need to remove the repeats.

### 1.2 — Second Read (Get Specific)

I stop and ask myself clarifying questions:

- Is the array **sorted** or **unsorted**?
- Should I return a **new array** or **modify in-place**?
- Should I **preserve the original order**?
- What **type** of elements? Numbers? Strings? Mixed?
- What should I return? The new array? The count of unique elements?

**My Answers (for this walkthrough):**
- Unsorted array of numbers
- Return a new array
- Preserve original order
- Numbers only (but I'll note how to handle mixed types)

### 1.3 — Third Read (Find Tricky Parts)

- What if the array is empty?
- What if there's only one element?
- What about negative numbers or zeroes?
- What if ALL elements are duplicates?
- What if NO elements are duplicates?

### 1.4 — Restate in My Own Words

> "I'm given a list of numbers. Some numbers appear more than once. I need to produce a new list where every number appears exactly once, in the same order as its first appearance."

### 1.5 — Identify Inputs and Outputs

```
INPUT:  [1, 3, 2, 3, 4, 1, 2]
OUTPUT: [1, 3, 2, 4]

INPUT TYPE:  Array of numbers (integers)
OUTPUT TYPE: Array of numbers (no repeats, original order preserved)
```

### 1.6 — Work Through Examples by Hand

I grab a pen and paper (or mental notepad) and trace through:

**Example 1 — Normal case:**
```
[1, 3, 2, 3, 4, 1]

I walk through one by one:
  1 → Have I seen 1 before? NO  → Keep it.  Seen: {1}
  3 → Have I seen 3 before? NO  → Keep it.  Seen: {1, 3}
  2 → Have I seen 2 before? NO  → Keep it.  Seen: {1, 3, 2}
  3 → Have I seen 3 before? YES → Skip it.
  4 → Have I seen 4 before? NO  → Keep it.  Seen: {1, 3, 2, 4}
  1 → Have I seen 1 before? YES → Skip it.

Result: [1, 3, 2, 4] ✓
```

**Example 2 — All duplicates:**
```
[5, 5, 5, 5]

  5 → NO  → Keep. Seen: {5}
  5 → YES → Skip.
  5 → YES → Skip.
  5 → YES → Skip.

Result: [5] ✓
```

**Example 3 — No duplicates:**
```
[1, 2, 3]

  1 → NO → Keep.
  2 → NO → Keep.
  3 → NO → Keep.

Result: [1, 2, 3] (unchanged) ✓
```

**Example 4 — Empty:**
```
[]
Result: [] ✓
```

**Example 5 — Single element:**
```
[7]
Result: [7] ✓
```

**Example 6 — Negatives and zero:**
```
[-1, 0, -1, 0, 2]

  -1 → NO  → Keep.
   0 → NO  → Keep.
  -1 → YES → Skip.
   0 → YES → Skip.
   2 → NO  → Keep.

Result: [-1, 0, 2] ✓
```

### 1.7 — My Complete Edge Case List

```
1. Empty array             []                  → []
2. Single element          [7]                 → [7]
3. All same values         [5, 5, 5, 5]       → [5]
4. No duplicates           [1, 2, 3]           → [1, 2, 3]
5. Negatives and zero      [-1, 0, -1, 0]      → [-1, 0]
6. Large array             100,000+ elements    → must be efficient
7. Duplicates at the end   [1, 2, 3, 1]        → [1, 2, 3]
8. Duplicates at the start [1, 1, 2, 3]        → [1, 2, 3]
9. Two elements, same      [4, 4]              → [4]
10. Two elements, different [3, 7]             → [3, 7]
```

### ✅ Phase 1 Complete Checklist

```
[✓] Read problem 3 times
[✓] Restated in my own words
[✓] Identified inputs and outputs with types
[✓] Traced through 6 examples by hand
[✓] Listed 10 edge cases
[✓] Clarified all ambiguities
```

---

## PHASE 2 — PLANNING & STRATEGIZING

### 2.1 — Start with Brute Force

I always start with the simplest possible approach, even if it's slow.

**My Brute Force Idea:**
> "For each element, scan everything before it. If I've seen it before, skip it. Otherwise, keep it."

```
PSEUDOCODE:
  create empty result array
  for each element in the input:
      scan the result array from start to end
      if element is NOT found in result:
          add element to result
  return result
```

```javascript
function removeDupsBrute(arr) {
  const result = [];
  for (let i = 0; i < arr.length; i++) {
    let found = false;
    for (let j = 0; j < result.length; j++) {
      if (arr[i] === result[j]) {
        found = true;
        break;
      }
    }
    if (!found) result.push(arr[i]);
  }
  return result;
}
```

### 2.2 — Analyze the Brute Force

```
Time Complexity:  O(n²)
  Why? For EACH of n elements, I scan up to n elements in result → n × n

Space Complexity: O(n)
  Why? The result array can hold up to n elements (if all unique)

WHERE is the bottleneck?
  → The INNER LOOP. For every element, I do a linear scan to answer
    one simple question: "Have I seen this before?"

WHAT exactly is slow?
  → The "Have I seen this before?" check is O(n) each time.
  → I'm asking this question n times → O(n) × n = O(n²)
```

### 2.3 — Ask the Key Optimization Questions

I go through each question one by one:

**"Can I trade space for time?"**
> YES! If I use a Set (hash-based), "Have I seen this?" becomes O(1) instead of O(n).
> Cost: O(n) extra space for the Set.
> Benefit: Total time drops from O(n²) to O(n).
> **This is the key unlock.**

**"Can I sort the input first?"**
> YES! Sorting makes duplicates adjacent. Then I only compare each element with its neighbor.
> Cost: O(n log n) for sorting, and I lose the original order.
> Benefit: Simple comparison logic, no extra data structure needed.

**"Am I doing redundant work?"**
> YES! The linear scan is redundant. A Set eliminates it entirely.

**"Is there a mathematical property I can exploit?"**
> Not really. This is a data structure problem, not a math problem.

### 2.4 — List All Possible Approaches

```
APPROACH A: Brute Force (nested loops)     → O(n²) time, O(n) space
APPROACH B: Hash Set (manual)              → O(n) time,  O(n) space  ★ BEST
APPROACH C: Sort + scan neighbors          → O(n log n) time, O(n) space
APPROACH D: Sorted array + two pointers    → O(n) time,  O(1) space  (if pre-sorted)
APPROACH E: Built-in Set (JS one-liner)    → O(n) time,  O(n) space  ★ MOST CONCISE
APPROACH F: filter + indexOf               → O(n²) time, O(n) space
APPROACH G: reduce + includes              → O(n²) time, O(n) space
APPROACH H: Object keys (type-aware)       → O(n) time,  O(n) space
```

### 2.5 — Choose My Approach

```
For interviews:  APPROACH B (Hash Set manual) — shows understanding
For production:  APPROACH E (Built-in Set)    — clean, idiomatic
For sorted input: APPROACH D (Two Pointers)   — O(1) space
```

### ✅ Phase 2 Complete Checklist

```
[✓] Wrote brute force solution
[✓] Analyzed its time and space complexity
[✓] Identified the bottleneck (inner linear scan)
[✓] Asked all optimization questions
[✓] Found the key insight (Set gives O(1) lookup)
[✓] Listed all possible approaches with complexities
[✓] Chose the best approach for my context
```

---

## PHASE 3 — DECOMPOSITION

### 3.1 — Break into Sub-Tasks

```
The problem has 4 sub-tasks:

  Sub-task 1: Handle edge cases (empty array, single element)
  Sub-task 2: Iterate through the array element by element
  Sub-task 3: For each element, CHECK if it's a duplicate
  Sub-task 4: If NOT a duplicate, ADD it to the output
```

### 3.2 — Identify the "Hard Part"

```
Sub-task 3 is the ONLY hard part.

Everything else (looping, pushing, returning) is trivial.
The entire problem's efficiency depends on ONE question:

    ┌──────────────────────────────────────┐
    │  "Have I seen this value before?"    │
    │                                      │
    │  If this check is O(n) → total O(n²) │
    │  If this check is O(1) → total O(n)  │
    └──────────────────────────────────────┘
```

### 3.3 — Data Flow

```
Input Array
    ↓
[For each element]
    ↓
[Check: seen before?] ← THIS IS THE CORE
    ↓              ↓
   YES            NO
    ↓              ↓
  Skip it    Add to result + mark as seen
    ↓              ↓
    └──────────────┘
          ↓
    Return result
```

### ✅ Phase 3 Complete Checklist

```
[✓] Broke problem into 4 sub-tasks
[✓] Identified Sub-task 3 as the hard part
[✓] Drew the data flow
[✓] Understood that the data structure choice for "seen" determines everything
```

---

## PHASE 4 — PATTERN RECOGNITION & TECHNIQUE SELECTION

### 4.1 — Walk Through the Decision Framework

```
Q1: Is this about searching or sorting?
  → Partially. I'm "searching" for whether I've seen a value.

Q2: Does it involve a contiguous sub-section of an array?
  → No. I need ALL unique elements, not a subarray.

Q3: Does it involve a sorted array or finding pairs?
  → Not necessarily sorted. Not finding pairs.

Q4: Does it involve looking up values or counting frequencies?
  → YES! I need to look up "have I seen this value?" → HASH SET

MATCH FOUND: Pattern = HASH SET
```

### 4.2 — Why Hash Set?

```
A Set is a collection where:
  - Every element is unique (no duplicates by definition)
  - .has(value) is O(1) — constant time lookup
  - .add(value) is O(1) — constant time insertion

This is EXACTLY what I need:
  - Check if seen: set.has(element)   → O(1)
  - Mark as seen:  set.add(element)   → O(1)
  - Do this n times                   → O(n) total
```

### 4.3 — Alternative Pattern: Sort + Two Pointers

```
If the array is already sorted (or I'm allowed to sort it):
  - Sort the array: duplicates become ADJACENT
  - Use two pointers: compare each element with the previous one
  - If different → keep it. If same → skip it.

When to prefer this:
  - When you need O(1) extra space (no Set)
  - When the array is already sorted
  - When preserving order doesn't matter
```

### ✅ Phase 4 Complete Checklist

```
[✓] Walked through decision framework question by question
[✓] Matched to Hash Set pattern
[✓] Understood WHY it's the right pattern (O(1) lookup)
[✓] Identified alternative pattern (Sort + Two Pointers)
[✓] Know when to use each
```

---

## PHASE 5 — IMPLEMENTATION

### 5.1 — Write the Skeleton First

```javascript
function removeDuplicates(arr) {
  // Step 1: Edge cases

  // Step 2: Initialize data structures

  // Step 3: Core logic — iterate and check

  // Step 4: Return result
}
```

### 5.2 — Fill in Edge Cases

```javascript
function removeDuplicates(arr) {
  // Step 1: Edge cases
  if (!arr || arr.length === 0) return [];
  if (arr.length === 1) return [...arr];

  // Step 2: Initialize data structures

  // Step 3: Core logic

  // Step 4: Return result
}
```

### 5.3 — Fill in Data Structures

```javascript
function removeDuplicates(arr) {
  if (!arr || arr.length === 0) return [];
  if (arr.length === 1) return [...arr];

  // Step 2: Initialize
  const seen = new Set();    // Tracks what we've encountered
  const result = [];          // Stores unique elements

  // Step 3: Core logic

  // Step 4: Return result
}
```

### 5.4 — Fill in Core Logic

```javascript
function removeDuplicates(arr) {
  if (!arr || arr.length === 0) return [];
  if (arr.length === 1) return [...arr];

  const seen = new Set();
  const result = [];

  // Step 3: Core logic
  for (let i = 0; i < arr.length; i++) {
    if (!seen.has(arr[i])) {      // O(1) — "Have I seen this?"
      seen.add(arr[i]);            // O(1) — "Now I've seen it"
      result.push(arr[i]);         // O(1) — "Keep it"
    }
    // If seen.has(arr[i]) → it's a duplicate → do nothing
  }

  // Step 4: Return
  return result;
}
```

### 5.5 — The Complete Solution (Approach B — Hash Set)

```javascript
function removeDuplicates(arr) {
  if (!arr || arr.length === 0) return [];
  if (arr.length === 1) return [...arr];

  const seen = new Set();
  const result = [];

  for (let i = 0; i < arr.length; i++) {
    if (!seen.has(arr[i])) {
      seen.add(arr[i]);
      result.push(arr[i]);
    }
  }

  return result;
}
```

### 5.6 — Alternative: Built-in One-Liner (Approach E)

```javascript
const removeDuplicates = (arr) => [...new Set(arr)];
```

How this works internally:
- `new Set(arr)` → iterates through arr, adds each element, auto-ignores duplicates
- `[...set]` → spreads the Set back into an Array
- Order IS preserved (JS Sets maintain insertion order)

### 5.7 — Alternative: Sort + Two Pointers (Approach D — for sorted arrays)

```javascript
function removeDupsSorted(nums) {
  if (!nums || nums.length === 0) return 0;
  if (nums.length === 1) return 1;

  let write = 1;  // Where the next unique element goes

  for (let read = 1; read < nums.length; read++) {
    if (nums[read] !== nums[write - 1]) {
      nums[write] = nums[read];
      write++;
    }
  }

  return write;  // Count of unique elements
  // Array is modified in-place: first `write` elements are unique
}
```

### 5.8 — Alternative: filter + indexOf (Approach F)

```javascript
function removeDuplicates(arr) {
  return arr.filter((item, index) => arr.indexOf(item) === index);
}
```

How this works:
- `indexOf(item)` returns the FIRST position of that item
- If the first position equals the current index → it's the first occurrence → keep it
- If the first position is earlier → it's a duplicate → filter it out

### 5.9 — Alternative: reduce + includes (Approach G)

```javascript
function removeDuplicates(arr) {
  return arr.reduce((unique, item) => {
    if (!unique.includes(item)) unique.push(item);
    return unique;
  }, []);
}
```

### 5.10 — Alternative: Object Keys with Type Safety (Approach H)

```javascript
function removeDuplicates(arr) {
  const seen = {};
  const result = [];

  for (const item of arr) {
    const key = typeof item + ':' + item;  // "number:1" vs "string:1"
    if (!seen[key]) {
      seen[key] = true;
      result.push(item);
    }
  }

  return result;
}
```

### 5.11 — JavaScript Gotchas I Watched For

```
⚠️ GOTCHA 1: Array.sort() is lexicographic by default
   [10, 2, 1].sort()       → [1, 10, 2]  WRONG!
   [10, 2, 1].sort((a,b) => a - b) → [1, 2, 10]  CORRECT!

⚠️ GOTCHA 2: Object keys are always strings
   obj[1] and obj["1"] are THE SAME KEY
   Use Map or Set instead for type-safe keys

⚠️ GOTCHA 3: === vs ==
   Always use === (strict equality)
   1 == "1"  → true   (DANGEROUS)
   1 === "1" → false   (SAFE)

⚠️ GOTCHA 4: Don't mutate the input unless asked
   Use [...arr] or arr.slice() to create a copy first

⚠️ GOTCHA 5: NaN === NaN is false in JS
   Set handles NaN correctly (treats two NaNs as duplicates)
   But manual === comparison won't catch NaN duplicates
```

### ✅ Phase 5 Complete Checklist

```
[✓] Wrote skeleton first (4-step structure)
[✓] Filled in edge cases BEFORE core logic
[✓] Implemented primary approach (Hash Set)
[✓] Implemented alternative approaches (5 more)
[✓] Used meaningful variable names (seen, result, write, read)
[✓] Noted JavaScript-specific gotchas
```

---

## PHASE 6 — TESTING & DEBUGGING

### 6.1 — Dry Run the Primary Solution (Hash Set)

I trace through my code with a real input, tracking every variable:

**Input: `[1, 3, 2, 3, 4, 1]`**

```
┌──────┬────────┬───────────────┬────────────────┬──────────────────────┐
│ Step │ arr[i] │ seen.has()?   │ seen (after)   │ result (after)       │
├──────┼────────┼───────────────┼────────────────┼──────────────────────┤
│ i=0  │   1    │ has(1)? NO    │ {1}            │ [1]                  │
│ i=1  │   3    │ has(3)? NO    │ {1, 3}         │ [1, 3]              │
│ i=2  │   2    │ has(2)? NO    │ {1, 3, 2}      │ [1, 3, 2]          │
│ i=3  │   3    │ has(3)? YES   │ {1, 3, 2}      │ [1, 3, 2] (skip)   │
│ i=4  │   4    │ has(4)? NO    │ {1, 3, 2, 4}   │ [1, 3, 2, 4]       │
│ i=5  │   1    │ has(1)? YES   │ {1, 3, 2, 4}   │ [1, 3, 2, 4] (skip)│
└──────┴────────┴───────────────┴────────────────┴──────────────────────┘

Return: [1, 3, 2, 4] ✓
```

### 6.2 — Dry Run the Sorted In-Place Solution (Two Pointers)

**Input: `[1, 1, 2, 2, 3, 4, 4]` (pre-sorted)**

```
┌──────┬───────┬─────────────┬──────────────────┬───────────────────────────┐
│ read │ write │ nums[read]  │ nums[write - 1]  │ Action & Array State      │
├──────┼───────┼─────────────┼──────────────────┼───────────────────────────┤
│  1   │  1    │     1       │ 1                │ SAME → skip               │
│  2   │  1    │     2       │ 1                │ DIFF → nums[1]=2, write=2 │
│      │       │             │                  │ [1, 2, 2, 2, 3, 4, 4]    │
│  3   │  2    │     2       │ 2                │ SAME → skip               │
│  4   │  2    │     3       │ 2                │ DIFF → nums[2]=3, write=3 │
│      │       │             │                  │ [1, 2, 3, 2, 3, 4, 4]    │
│  5   │  3    │     4       │ 3                │ DIFF → nums[3]=4, write=4 │
│      │       │             │                  │ [1, 2, 3, 4, 3, 4, 4]    │
│  6   │  4    │     4       │ 4                │ SAME → skip               │
└──────┴───────┴─────────────┴──────────────────┴───────────────────────────┘

Return: write = 4 → first 4 elements = [1, 2, 3, 4] ✓
```

### 6.3 — Test All Edge Cases

```javascript
// I run each edge case through my solution:

removeDuplicates([])                    // → []         ✓ Empty
removeDuplicates([7])                   // → [7]        ✓ Single
removeDuplicates([5, 5, 5, 5])         // → [5]        ✓ All same
removeDuplicates([1, 2, 3])            // → [1, 2, 3]  ✓ No dups
removeDuplicates([-1, 0, -1, 0])       // → [-1, 0]    ✓ Negatives
removeDuplicates([1, 2, 3, 1])         // → [1, 2, 3]  ✓ Dup at end
removeDuplicates([1, 1, 2, 3])         // → [1, 2, 3]  ✓ Dup at start
removeDuplicates([4, 4])               // → [4]        ✓ Two same
removeDuplicates([3, 7])               // → [3, 7]     ✓ Two different
```

### 6.4 — Bug Checklist (Things I Verify)

```
[✓] Does it handle empty arrays without crashing?
[✓] Does it handle single-element arrays?
[✓] Does it preserve the original order?
[✓] Does it use === (strict equality) not == ?
[✓] Does it return a NEW array (not mutate the original)?
[✓] Does it handle negative numbers and zero?
[✓] Am I not accidentally skipping the first or last element?
[✓] Is my loop bound correct? (i < arr.length, NOT i <= arr.length)
[✓] Does Set.has() handle NaN correctly? (Yes, it does)
[✓] Am I using Map/Set and not plain Objects for type safety?
```

### 6.5 — Debugging Strategy (If Something Goes Wrong)

```
Step 1: Reduce the input to the SMALLEST failing case
        e.g., if [1,3,2,3,4,1] fails, try [3,3] first

Step 2: Add strategic console.logs inside the loop:
        console.log(`i=${i}, arr[i]=${arr[i]}, seen.has=${seen.has(arr[i])}`);

Step 3: Compare my dry-run table with actual console output
        The mismatch reveals the bug

Step 4: Check the common JS gotchas list (Section 5.11)
```

### ✅ Phase 6 Complete Checklist

```
[✓] Dry-ran primary solution with variable-tracking table
[✓] Dry-ran alternative solution (Two Pointers)
[✓] Tested all 9 edge cases
[✓] Went through the bug checklist
[✓] Have a debugging strategy ready if needed
```

---

## PHASE 7 — OPTIMIZATION & REFACTORING

### 7.1 — Analyze Current Complexity

```
My Hash Set solution:
  Time:  O(n) — one pass through the array, each Set operation is O(1)
  Space: O(n) — the Set and result array each store up to n elements
```

### 7.2 — Can I Do Better?

```
Time: O(n) is OPTIMAL.
  Why? I must look at every element at least once → Ω(n) lower bound.
  I'm already at O(n). Cannot improve.

Space: Can I reduce from O(n)?
  → If the array is SORTED: YES! Use Two Pointers for O(1) space.
  → If UNSORTED and I must preserve order: NO. I need the Set.
  → If I'm allowed to modify the input: I can sort first, then two-pointer.
    But sorting costs O(n log n) time, so there's a tradeoff.
```

### 7.3 — Full Complexity Comparison

```
┌─────────────────────────────┬────────────┬──────────┬────────────────────────┐
│ Approach                    │ Time       │ Space    │ Best For               │
├─────────────────────────────┼────────────┼──────────┼────────────────────────┤
│ A. Brute Force (nested)     │ O(n²)      │ O(n)     │ Understanding only     │
│ B. Hash Set (manual)        │ O(n)       │ O(n)     │ ★ Interviews           │
│ C. Sort + scan              │ O(n log n) │ O(n)     │ When order doesn't     │
│                             │            │          │   matter               │
│ D. Sorted + Two Pointers    │ O(n)       │ O(1)     │ ★ Pre-sorted input     │
│ E. Built-in Set one-liner   │ O(n)       │ O(n)     │ ★ Production code      │
│ F. filter + indexOf         │ O(n²)      │ O(n)     │ Quick and readable     │
│ G. reduce + includes        │ O(n²)      │ O(n)     │ Functional style       │
│ H. Object keys              │ O(n)       │ O(n)     │ Mixed types (careful)  │
└─────────────────────────────┴────────────┴──────────┴────────────────────────┘
```

### 7.4 — Refactoring for Readability

**Before (works but unclear):**
```javascript
function f(a) {
  const s = new Set();
  const r = [];
  for (let i = 0; i < a.length; i++) {
    if (!s.has(a[i])) { s.add(a[i]); r.push(a[i]); }
  }
  return r;
}
```

**After (clean, readable, production-ready):**
```javascript
function removeDuplicates(arr) {
  if (!arr || arr.length === 0) return [];

  const seen = new Set();
  const uniqueElements = [];

  for (const element of arr) {
    if (!seen.has(element)) {
      seen.add(element);
      uniqueElements.push(element);
    }
  }

  return uniqueElements;
}
```

**Changes I made:**
- Meaningful function name: `removeDuplicates` instead of `f`
- Meaningful variable names: `seen`, `uniqueElements`, `element`
- Edge case guard at the top
- `for...of` loop instead of index-based (cleaner when index isn't needed)
- Each line does one clear thing

### ✅ Phase 7 Complete Checklist

```
[✓] Analyzed time and space complexity
[✓] Confirmed O(n) is optimal for this problem
[✓] Compared all approaches side by side
[✓] Refactored for readability
[✓] Identified when to use each approach
```

---

## PHASE 8 — REFLECTION & LEARNING

### 8.1 — What pattern did this problem use?

**Primary:** Hash Set (for O(1) "have I seen this?" checks)
**Alternative:** Two Pointers (for sorted arrays, O(1) space)

### 8.2 — What was the "aha" moment?

> The entire problem boils down to ONE question: "Have I seen this before?"
> The ONLY thing that changes between O(n²) and O(n) is HOW FAST I answer that question.
>
>    Array scan  → O(n) per check → O(n²) total
>    Set lookup  → O(1) per check → O(n) total

### 8.3 — Where did I get stuck (or where would beginners get stuck)?

```
Stuck Point 1: Jumping straight to code without understanding edge cases
  Fix: Force yourself to trace 3+ examples by hand first

Stuck Point 2: Not recognizing that the inner loop is the bottleneck
  Fix: Always ask "WHERE is the repeated work?"

Stuck Point 3: JS-specific — using {} instead of Set (keys become strings)
  Fix: Always use Map/Set for type-safe lookups

Stuck Point 4: Forgetting that Array.sort() is lexicographic
  Fix: Always provide a comparison function: .sort((a, b) => a - b)
```

### 8.4 — What similar problems does this connect to?

```
DIRECTLY SIMILAR:
  - Contains Duplicate (LeetCode #217) — just return true/false
  - Remove Duplicates from Sorted Array (#26) — in-place, two pointers
  - Remove Duplicates from Sorted Array II (#80) — allow max 2 copies
  - Find All Duplicates in an Array (#442)

SAME PATTERN (Hash Set for lookup):
  - Two Sum (#1) — "Have I seen the complement?"
  - Intersection of Two Arrays (#349)
  - Longest Substring Without Repeating Characters (#3)
  - Valid Sudoku (#36) — "Have I seen this digit in this row/col/box?"
  - First Unique Character in a String (#387)
```

### 8.5 — One-Sentence Summary

> **"To remove duplicates, use a Set for O(1) 'seen before?' checks, reducing O(n²) to O(n). For sorted arrays, use two pointers for O(1) space."**

### 8.6 — Template I Can Reuse

```javascript
// REUSABLE TEMPLATE: "Track what I've seen"
//
// Anytime a problem asks:
//   "Have I seen this before?"
//   "Is this a duplicate?"
//   "Does the complement exist?"
//   "How many times has X appeared?"
//
// Use this skeleton:

function solveWithSet(arr) {
  const seen = new Set();   // or new Map() if I need counts
  const result = [];

  for (const item of arr) {
    const lookupValue = item;   // or (target - item) for complement problems

    if (seen.has(lookupValue)) {
      // It's a duplicate / complement found / already processed
      // → handle accordingly
    } else {
      // First time seeing this value
      seen.add(item);
      result.push(item);
    }
  }

  return result;
}
```

### ✅ Phase 8 Complete Checklist

```
[✓] Identified the pattern (Hash Set)
[✓] Articulated the key insight ("how fast can I check 'seen before?'")
[✓] Noted where I'd get stuck
[✓] Listed 10 similar problems
[✓] Wrote a one-sentence summary
[✓] Created a reusable template for future problems
```

---

## MASTER CHECKLIST — ALL 8 PHASES

```
PHASE 1 — UNDERSTAND
  [✓] Read 3 times (gist → specifics → tricky parts)
  [✓] Restated in my own words
  [✓] Identified inputs/outputs with types
  [✓] Traced 6 examples by hand
  [✓] Listed 10 edge cases
  [✓] Clarified all ambiguities

PHASE 2 — PLAN
  [✓] Wrote brute force
  [✓] Analyzed its complexity and found bottleneck
  [✓] Asked optimization questions (space-time trade?)
  [✓] Found the key insight (Set = O(1) lookup)
  [✓] Listed all 8 approaches with complexities

PHASE 3 — DECOMPOSE
  [✓] Broke into 4 sub-tasks
  [✓] Identified the hard part (duplicate detection)
  [✓] Drew the data flow

PHASE 4 — PATTERN MATCH
  [✓] Walked through decision framework
  [✓] Selected Hash Set pattern
  [✓] Identified alternative (Two Pointers)

PHASE 5 — IMPLEMENT
  [✓] Wrote skeleton first
  [✓] Edge cases before core logic
  [✓] Implemented 6 approaches
  [✓] Noted JS gotchas

PHASE 6 — TEST
  [✓] Dry-ran with variable tracking table
  [✓] Tested all edge cases
  [✓] Went through bug checklist

PHASE 7 — OPTIMIZE
  [✓] Confirmed O(n) is optimal
  [✓] Compared all approaches
  [✓] Refactored for readability

PHASE 8 — REFLECT
  [✓] Identified pattern and insight
  [✓] Listed similar problems
  [✓] Created reusable template
  [✓] Wrote one-sentence summary
```
