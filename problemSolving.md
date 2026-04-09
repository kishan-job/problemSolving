# The Exhaustive Problem-Solving Guide for Software Engineers
## Using JavaScript as the Primary Language

---

## TABLE OF CONTENTS

1. The Problem-Solving Mindset
2. Phase 1 — Understanding the Problem
3. Phase 2 — Planning & Strategizing
4. Phase 3 — Decomposition
5. Phase 4 — Pattern Recognition & Technique Selection
6. Phase 5 — Implementation (Writing Code)
7. Phase 6 — Testing & Debugging
8. Phase 7 — Optimization & Refactoring
9. Phase 8 — Reflection & Learning
10. Core Problem-Solving Patterns & Techniques (with JS examples)
11. Data Structure Selection Guide
12. Complexity Analysis Cheat Sheet
13. Practice Roadmap

---

## 1. THE PROBLEM-SOLVING MINDSET

Before any technique or pattern, you need the right mental framework.

**Principles:**

- **Embrace discomfort.** If you're not stuck sometimes, you're not growing. Being stuck IS the process.
- **Think before you type.** The biggest mistake engineers make is jumping to code. Spend 40-60% of your time understanding and planning.
- **Be systematic, not random.** Never randomly try things. Have a reason for every line you write.
- **Reduce ego.** Accept that brute force is a valid first step. Elegance comes second.
- **Verbalize your thinking.** Explain the problem out loud (rubber duck debugging). This forces clarity.

---

## 2. PHASE 1 — UNDERSTANDING THE PROBLEM

This is the most underrated and most critical phase. Most failures happen here.

### Step 1.1: Read the Problem Multiple Times

Read it at least 3 times. Each time, focus on a different layer:
- **1st read:** Get the gist. What is being asked?
- **2nd read:** Identify specifics — inputs, outputs, constraints.
- **3rd read:** Find edge cases, hidden assumptions, and tricky wording.

### Step 1.2: Restate the Problem in Your Own Words

Write it down in plain English (or your native language). If you can't explain it simply, you don't understand it.

```
// Example: "Two Sum" problem
// ORIGINAL: Given an array of integers nums and an integer target,
//           return indices of the two numbers that add up to target.

// MY RESTATEMENT: I have a list of numbers. I need to find which
// two numbers, when added, equal the target. I return their positions.
```

### Step 1.3: Identify Inputs and Outputs Explicitly

```
// INPUTS:
//   - nums: an array of integers, e.g., [2, 7, 11, 15]
//   - target: an integer, e.g., 9

// OUTPUTS:
//   - An array of two indices, e.g., [0, 1]
```

### Step 1.4: Work Through Examples by Hand

Manually trace through at least 3 examples:

```
// Example 1: nums = [2, 7, 11, 15], target = 9
//   2 + 7 = 9 ✓ → return [0, 1]

// Example 2: nums = [3, 2, 4], target = 6
//   3 + 2 = 5 ✗
//   3 + 4 = 7 ✗
//   2 + 4 = 6 ✓ → return [1, 2]

// Example 3 (edge): nums = [3, 3], target = 6
//   3 + 3 = 6 ✓ → return [0, 1]
```

### Step 1.5: Identify Constraints and Edge Cases

Ask yourself these questions every single time:

- What is the size range of the input? (1 element? 10^6 elements?)
- Can values be negative? Zero? Duplicates?
- Is the input sorted? Can I sort it?
- What if the input is empty?
- What should I return if there's no valid answer?
- Are there time/space complexity requirements?

```
// CONSTRAINTS for Two Sum:
// - Exactly one solution exists
// - Can't use the same element twice
// - Array length: 2 to 10^4
// - Values: -10^9 to 10^9

// EDGE CASES:
// - Duplicate values: [3, 3], target = 6
// - Negative numbers: [-1, -2, -3, -4, -5], target = -8
// - Minimum array: [1, 2], target = 3
```

### Step 1.6: Clarify Ambiguities

In interviews or real-world scenarios, ASK QUESTIONS:
- "Can the array contain duplicates?"
- "Should I return 0-indexed or 1-indexed?"
- "What if no solution exists?"
- "Is the array sorted?"

---

## 3. PHASE 2 — PLANNING & STRATEGIZING

### Step 2.1: Start with Brute Force

ALWAYS start here. It proves you understand the problem and gives you a baseline.

```javascript
// Two Sum — Brute Force
// Strategy: Check every pair of numbers
function twoSumBrute(nums, target) {
  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[i] + nums[j] === target) {
        return [i, j];
      }
    }
  }
  return [];
}
// Time: O(n²) | Space: O(1)
```

### Step 2.2: Analyze the Brute Force

- What is the time complexity? O(n²)
- What is the space complexity? O(1)
- WHERE is the bottleneck? The nested loop — we're checking every pair.
- WHAT is being repeated? For each number, we're scanning the entire array to find its complement.

### Step 2.3: Ask the Key Optimization Questions

These questions unlock better solutions:

1. **"Can I trade space for time?"** → Use a hash map, cache, or auxiliary data structure.
2. **"Can I sort the input first?"** → Enables binary search or two pointers.
3. **"Can I use a different traversal order?"** → Right-to-left, level-order, etc.
4. **"Am I doing redundant work?"** → Can I store intermediate results (memoization)?
5. **"Can I reduce the problem size?"** → Divide and conquer.
6. **"Is there a mathematical property I can exploit?"** → Modular arithmetic, XOR, etc.
7. **"What if I processed the data in a stream?"** → Sliding window, running totals.

### Step 2.4: Map Problem to Known Patterns

(See Section 10 for the full pattern catalog.)

```
// Two Sum optimization thought process:
// "For each number x, I need to find if (target - x) exists."
// "If I could look up (target - x) instantly..." → Hash Map!
// A hash map gives O(1) lookup.
```

### Step 2.5: Write Pseudocode Before Real Code

```
// PSEUDOCODE:
// 1. Create an empty hash map
// 2. For each number in the array (with its index):
//    a. Calculate complement = target - current number
//    b. If complement exists in the hash map:
//       → Return [hash map's index for complement, current index]
//    c. Otherwise, store current number and its index in the hash map
// 3. Return empty (no solution found)
```

---

## 4. PHASE 3 — DECOMPOSITION

Breaking complex problems into smaller, manageable sub-problems.

### Technique 3.1: Top-Down Decomposition

Start with the big picture and break it into layers.

```javascript
// Problem: "Build a function that validates and processes a user order"

// Level 1 (Main function):
function processOrder(order) {
  const validationResult = validateOrder(order);
  if (!validationResult.isValid) return { error: validationResult.errors };

  const pricedOrder = calculatePricing(order);
  const confirmedOrder = confirmOrder(pricedOrder);
  return confirmedOrder;
}

// Level 2 (Sub-functions):
function validateOrder(order) { /* ... */ }
function calculatePricing(order) { /* ... */ }
function confirmOrder(order) { /* ... */ }

// Level 3 (Deeper sub-functions):
function validateItems(items) { /* ... */ }
function validatePayment(payment) { /* ... */ }
function applyDiscounts(order) { /* ... */ }
function calculateTax(order) { /* ... */ }
```

### Technique 3.2: Input/Output Chaining

Define what each sub-function takes in and returns.

```javascript
// validateOrder: Order → { isValid: boolean, errors: string[] }
// calculatePricing: Order → PricedOrder
// applyDiscounts: PricedOrder → PricedOrder
// calculateTax: PricedOrder → PricedOrder
// confirmOrder: PricedOrder → ConfirmedOrder
```

### Technique 3.3: Identify the "Hard Part"

Every problem has one core difficulty. Isolate it.

```
// "Build a calendar booking system"
// Easy parts: UI rendering, storing events, basic CRUD
// HARD PART: Detecting and handling overlapping time ranges
// → Focus your energy HERE first
```

---

## 5. PHASE 4 — PATTERN RECOGNITION & TECHNIQUE SELECTION

### The Decision Framework

Ask these questions in order to select the right pattern:

```
Q1: Is this about searching or sorting?
  → Binary Search, Merge Sort, Quick Sort

Q2: Does it involve a sequence/array with a contiguous sub-section?
  → Sliding Window

Q3: Does it involve a sorted array or finding pairs?
  → Two Pointers

Q4: Does it involve looking up complements or counting frequencies?
  → Hash Map / Hash Set

Q5: Does it involve a tree or graph traversal?
  → BFS (level-order/shortest path) or DFS (exhaustive search)

Q6: Does it involve making choices to reach an optimal solution?
  → Dynamic Programming (if overlapping sub-problems)
  → Greedy (if local optimal = global optimal)

Q7: Does it involve generating all combinations/permutations?
  → Backtracking

Q8: Does it involve intervals, ranges, or schedules?
  → Interval techniques (sort + merge/compare)

Q9: Does it involve a stream of data with ordering?
  → Heap / Priority Queue

Q10: Does it involve prefix sums, range queries?
  → Prefix Sum Array
```

---

## 6. PHASE 5 — IMPLEMENTATION (Writing Code)

### Step 5.1: Set Up the Skeleton

```javascript
function solve(input) {
  // 1. Edge case handling

  // 2. Initialize data structures

  // 3. Core logic

  // 4. Return result
}
```

### Step 5.2: Code Incrementally

Write small pieces, test each piece mentally before moving on.

### Step 5.3: Use Meaningful Variable Names

```javascript
// BAD
function fn(a, t) {
  const m = {};
  for (let i = 0; i < a.length; i++) {
    const c = t - a[i];
    if (m[c] !== undefined) return [m[c], i];
    m[a[i]] = i;
  }
}

// GOOD
function twoSum(nums, target) {
  const seen = new Map();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (seen.has(complement)) {
      return [seen.get(complement), i];
    }
    seen.set(nums[i], i);
  }
  return [];
}
```

### Step 5.4: Handle Edge Cases First

```javascript
function twoSum(nums, target) {
  // Edge cases
  if (!nums || nums.length < 2) return [];

  // Core logic
  const seen = new Map();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (seen.has(complement)) {
      return [seen.get(complement), i];
    }
    seen.set(nums[i], i);
  }
  return [];
}
```

### Step 5.5: JavaScript-Specific Best Practices

```javascript
// Use Map instead of plain objects for hash maps (handles all key types)
const map = new Map();   // ✓
const obj = {};          // ✗ (keys are always strings)

// Use Set for unique value lookups
const set = new Set([1, 2, 3]);
set.has(2); // O(1) lookup

// Array destructuring for swaps
[arr[i], arr[j]] = [arr[j], arr[i]];

// Use Math.max/Math.min with spread cautiously (stack overflow on large arrays)
// For large arrays, use a loop instead
let max = -Infinity;
for (const num of arr) max = Math.max(max, num);

// Use ?? (nullish coalescing) and ?. (optional chaining)
const val = map.get(key) ?? defaultValue;

// Typed comparisons
if (a === b) // ✓ strict equality
if (a == b)  // ✗ loose equality (avoid)
```

---

## 7. PHASE 6 — TESTING & DEBUGGING

### Step 6.1: Trace Through Your Code by Hand (Dry Run)

Use a table to track variable states:

```javascript
// twoSum([2, 7, 11, 15], 9)
// ┌───────┬──────┬────────────┬──────────────────┬────────┐
// │ i     │ nums │ complement │ seen             │ result │
// ├───────┼──────┼────────────┼──────────────────┼────────┤
// │ 0     │ 2    │ 7          │ {2→0}            │ -      │
// │ 1     │ 7    │ 2          │ seen has 2!      │ [0,1]  │
// └───────┴──────┴────────────┴──────────────────┴────────┘
```

### Step 6.2: Test with Your Identified Edge Cases

```javascript
console.log(twoSum([2, 7, 11, 15], 9));  // [0, 1] — normal
console.log(twoSum([3, 3], 6));           // [0, 1] — duplicates
console.log(twoSum([1, 2], 3));           // [0, 1] — minimum input
console.log(twoSum([-1, -2, -3], -5));    // [1, 2] — negatives
console.log(twoSum([], 5));               // []     — empty
console.log(twoSum([1], 1));              // []     — single element
```

### Step 6.3: Common Bug Checklist (JavaScript)

```
□ Off-by-one errors (i < n vs i <= n)
□ Forgetting to return a value
□ Mutating input arrays unintentionally (use [...arr] or arr.slice())
□ Using == instead of ===
□ Integer overflow (JS uses 64-bit floats; safe up to 2^53 - 1)
□ Forgetting that Array.sort() sorts lexicographically by default
    → arr.sort((a, b) => a - b) for numeric sort
□ Not handling undefined/null
□ Infinite loops (while loops without proper termination)
□ Using object keys (auto-stringified) when you need numeric/object keys (use Map)
□ Modifying array while iterating over it
```

### Step 6.4: Debugging Strategies

```javascript
// 1. Strategic console.log (not random ones)
function binarySearch(arr, target) {
  let lo = 0, hi = arr.length - 1;
  while (lo <= hi) {
    const mid = Math.floor((lo + hi) / 2);
    console.log(`lo=${lo}, hi=${hi}, mid=${mid}, arr[mid]=${arr[mid]}`);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}

// 2. Reduce input size — if it fails on 1000 elements, try 5
// 3. Compare brute force output with optimized output
// 4. Check boundary conditions — first element, last element, empty input
```

---

## 8. PHASE 7 — OPTIMIZATION & REFACTORING

### Step 7.1: Analyze Current Complexity

```javascript
// Ask: "What is my current time and space complexity?"
// Two Sum with Hash Map:
// Time:  O(n) — single pass through array
// Space: O(n) — hash map stores up to n elements
```

### Step 7.2: Can You Do Better?

Use the optimization checklist:

```
□ Can I reduce the number of passes?
□ Can I eliminate a data structure?
□ Can I use bit manipulation?
□ Can I process data in-place?
□ Can I use a more efficient data structure?
□ Am I doing unnecessary copies? (strings, arrays)
```

### Step 7.3: Refactor for Readability

```javascript
// BEFORE — works but messy
function solve(s) {
  let m = 0;
  for (let i = 0; i < s.length; i++) {
    let st = new Set();
    for (let j = i; j < s.length; j++) {
      if (st.has(s[j])) break;
      st.add(s[j]);
      m = Math.max(m, j - i + 1);
    }
  }
  return m;
}

// AFTER — clean, named, documented
function longestUniqueSubstring(str) {
  let maxLength = 0;

  for (let start = 0; start < str.length; start++) {
    const seen = new Set();

    for (let end = start; end < str.length; end++) {
      if (seen.has(str[end])) break;
      seen.add(str[end]);
      maxLength = Math.max(maxLength, end - start + 1);
    }
  }

  return maxLength;
}
```

---

## 9. PHASE 8 — REFLECTION & LEARNING

After solving any problem, spend 5-10 minutes on these:

```
□ What pattern did this problem use?
□ What was the "aha" moment / key insight?
□ Where did I get stuck and why?
□ What similar problems does this remind me of?
□ Can I solve it a different way?
□ Write a 1-sentence summary of the technique for future reference.
```

```javascript
// REFLECTION LOG EXAMPLE:
// Problem: Two Sum
// Pattern: Hash Map (complement lookup)
// Key Insight: Instead of searching for a pair, store what you've seen
//              and check if the current element's complement was seen before.
// Stuck On: Initially forgot to handle duplicate values.
// Similar To: Three Sum, Four Sum, Pair with given difference.
// Sentence: "When looking for pairs that satisfy a condition,
//            use a hash map to store seen elements for O(1) lookup."
```

---

## 10. CORE PROBLEM-SOLVING PATTERNS & TECHNIQUES

### PATTERN 1: HASH MAP / HASH SET

**When to use:** Frequency counting, finding duplicates, complement lookup, grouping.

```javascript
// Technique 1A: Frequency Counter
function hasDuplicates(arr) {
  const freq = new Map();
  for (const num of arr) {
    if (freq.has(num)) return true;
    freq.set(num, true);
  }
  return false;
}

// Technique 1B: Anagram Check (compare two frequency maps)
function isAnagram(s, t) {
  if (s.length !== t.length) return false;
  const count = new Map();
  for (const ch of s) count.set(ch, (count.get(ch) || 0) + 1);
  for (const ch of t) {
    if (!count.has(ch) || count.get(ch) === 0) return false;
    count.set(ch, count.get(ch) - 1);
  }
  return true;
}

// Technique 1C: Group By Key
function groupAnagrams(strs) {
  const map = new Map();
  for (const s of strs) {
    const key = [...s].sort().join('');
    if (!map.has(key)) map.set(key, []);
    map.get(key).push(s);
  }
  return [...map.values()];
}
```

---

### PATTERN 2: TWO POINTERS

**When to use:** Sorted arrays, finding pairs, palindromes, partitioning, merging.

```javascript
// Technique 2A: Opposite Ends (sorted array pair sum)
function twoSumSorted(nums, target) {
  let left = 0, right = nums.length - 1;
  while (left < right) {
    const sum = nums[left] + nums[right];
    if (sum === target) return [left, right];
    if (sum < target) left++;
    else right--;
  }
  return [];
}

// Technique 2B: Fast & Slow (linked list cycle detection)
function hasCycle(head) {
  let slow = head, fast = head;
  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow === fast) return true;
  }
  return false;
}

// Technique 2C: Same Direction (remove duplicates in-place)
function removeDuplicates(nums) {
  if (nums.length === 0) return 0;
  let write = 1;
  for (let read = 1; read < nums.length; read++) {
    if (nums[read] !== nums[read - 1]) {
      nums[write] = nums[read];
      write++;
    }
  }
  return write;
}

// Technique 2D: Palindrome Check
function isPalindrome(s) {
  let left = 0, right = s.length - 1;
  while (left < right) {
    if (s[left] !== s[right]) return false;
    left++;
    right--;
  }
  return true;
}
```

---

### PATTERN 3: SLIDING WINDOW

**When to use:** Finding contiguous subarrays/substrings that satisfy a condition.

```javascript
// Technique 3A: Fixed-Size Window
function maxSumSubarray(arr, k) {
  let windowSum = 0;
  for (let i = 0; i < k; i++) windowSum += arr[i];

  let maxSum = windowSum;
  for (let i = k; i < arr.length; i++) {
    windowSum += arr[i] - arr[i - k]; // slide: add new, remove old
    maxSum = Math.max(maxSum, windowSum);
  }
  return maxSum;
}

// Technique 3B: Variable-Size Window (expand/contract)
function longestSubstringWithoutRepeat(s) {
  const charIndex = new Map();
  let maxLen = 0, start = 0;

  for (let end = 0; end < s.length; end++) {
    if (charIndex.has(s[end]) && charIndex.get(s[end]) >= start) {
      start = charIndex.get(s[end]) + 1;
    }
    charIndex.set(s[end], end);
    maxLen = Math.max(maxLen, end - start + 1);
  }
  return maxLen;
}

// Technique 3C: Window with Frequency Map (minimum window substring)
function minWindow(s, t) {
  const need = new Map();
  for (const ch of t) need.set(ch, (need.get(ch) || 0) + 1);

  let have = 0, required = need.size;
  let left = 0, minLen = Infinity, result = '';

  const window = new Map();
  for (let right = 0; right < s.length; right++) {
    const ch = s[right];
    window.set(ch, (window.get(ch) || 0) + 1);

    if (need.has(ch) && window.get(ch) === need.get(ch)) have++;

    while (have === required) {
      if (right - left + 1 < minLen) {
        minLen = right - left + 1;
        result = s.slice(left, right + 1);
      }
      const leftCh = s[left];
      window.set(leftCh, window.get(leftCh) - 1);
      if (need.has(leftCh) && window.get(leftCh) < need.get(leftCh)) have--;
      left++;
    }
  }
  return result;
}
```

---

### PATTERN 4: BINARY SEARCH

**When to use:** Sorted data, search space reduction, finding boundaries, optimization problems (min/max that satisfies a condition).

```javascript
// Technique 4A: Standard Binary Search
function binarySearch(arr, target) {
  let lo = 0, hi = arr.length - 1;
  while (lo <= hi) {
    const mid = lo + Math.floor((hi - lo) / 2); // avoids overflow
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}

// Technique 4B: Left Boundary (first occurrence)
function leftBound(arr, target) {
  let lo = 0, hi = arr.length - 1, result = -1;
  while (lo <= hi) {
    const mid = lo + Math.floor((hi - lo) / 2);
    if (arr[mid] === target) { result = mid; hi = mid - 1; }
    else if (arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return result;
}

// Technique 4C: Binary Search on Answer (min capacity to ship packages in D days)
function shipWithinDays(weights, days) {
  let lo = Math.max(...weights);
  let hi = weights.reduce((a, b) => a + b, 0);

  while (lo < hi) {
    const mid = lo + Math.floor((hi - lo) / 2);
    if (canShip(weights, days, mid)) hi = mid;
    else lo = mid + 1;
  }
  return lo;
}

function canShip(weights, days