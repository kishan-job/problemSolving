# Master Guide to Computational Thinking
## From "Zero" to "Problem Solver" — The Easy Way

> **The Rule:** Follow these 6 phases for EVERY coding challenge. No skipping. No shortcuts.

---

## PHASE 1: Problem Decomposition [Requirement Analysis]

> **The Goal:** Break the "Big Monster" into tiny, manageable "Bugs."

### Step 1.1 — Analyze the Input

Ask yourself: **"What am I given?"**

```
□ What is the exact data type? (String? Array? Object? Number?)
□ Can it be empty? ([], "", null, undefined?)
□ How big can it be? (5 items? 1 million items?)
□ Are there duplicates? Negatives? Zeroes?
□ Is it sorted or unsorted?
```

**Example — "Remove Duplicates from an Array":**
```
Input: An array of numbers like [1, 3, 2, 3, 4, 1]
  - Type: Array of numbers
  - Can be empty: Yes
  - Can have negatives: Yes
  - Unsorted
```

### Step 1.2 — Define the Output

Ask yourself: **"What must I return?"**

```
□ A true/false?        → Boolean
□ A single number?     → Number (sum, count, max, etc.)
□ A new list?          → Array
□ A changed version?   → Modified Array/String/Object
□ Nothing (just print)? → void / console.log
```

**Example:**
```
Output: A new array with no repeated values → [1, 3, 2, 4]
  - Type: Array
  - Same order as original? Yes
  - New array or change the old one? New array
```

### Step 1.3 — Find the Constraints

Ask yourself: **"What are the rules?"**

```
□ "Don't use built-in methods" → Must code it manually
□ "Must be O(n)"               → Only one loop allowed, no nested loops
□ "Do it in-place"             → Don't create a new array
□ "Preserve order"             → Keep the original sequence
□ No constraints mentioned     → Use whatever works
```

### Step 1.4 — Write the Goal Sentence

Fill in this template:

> **"I am taking [INPUT] and turning it into [OUTPUT] by doing [X]."**

**Example:**
> "I am taking an **array of numbers with duplicates** and turning it into a **new array with only unique numbers** by **checking if I've seen each number before.**"

### Step 1.5 — List Your Edge Cases

These are the "tricky inputs" that break most solutions. **ALWAYS list them.**

```
For almost EVERY array problem, check these:
  1. Empty array        → []
  2. Single element     → [7]
  3. All same values    → [5, 5, 5]
  4. Already "solved"   → [1, 2, 3] (no duplicates)
  5. Negatives & zero   → [-1, 0, -1]
  6. Very large input   → 100,000 items
```

### ✅ Phase 1 Checklist

```
[✓] I know the INPUT type and its quirks
[✓] I know the exact OUTPUT expected
[✓] I know the constraints/rules
[✓] I wrote the Goal Sentence
[✓] I listed edge cases
```

---

## PHASE 2: Pattern Identification [Which "Shape" Is This?]

> **The Goal:** Look at the problem and say "Ah, this is a _____ problem."

There are **6 core patterns**. Every beginner-to-intermediate problem fits one (or a combo).

---

### Pattern 1: 🔍 Boolean Validation [The Detective]

**Use when:** You need a `true` / `false` answer about a whole list.

**The Logic:** Look for the ONE mistake (the Spoiler). The moment you find it, `return false`. If you survive the whole loop without finding a mistake, `return true`.

**Keywords in the problem:** "Is it...?", "Are all...?", "Does every...?", "Is valid?"

```javascript
// Template:
function isValid(arr) {
  for (let i = 0; i < arr.length; i++) {
    if (/* BAD CONDITION */) {
      return false;  // Found the Spoiler → EARLY EXIT
    }
  }
  return true;  // Survived! Everything is valid.
}
```

**Real Example — "Is the array sorted?"**
```javascript
function isSorted(arr) {
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] > arr[i + 1]) {
      return false;  // Found an out-of-order pair → NOT sorted
    }
  }
  return true;  // Every pair was in order → IS sorted
}
```

**More problems that use this:**
- Is this string a palindrome?
- Are all parentheses balanced?
- Does the array contain a duplicate?
- Are all numbers positive?

---

### Pattern 2: 🪣 Accumulation [The Bucket]

**Use when:** You need to turn a whole list into ONE single value.

**The Logic:** Start with an empty "Bucket" (0 for numbers, "" for strings). Walk through the list and pour each item into the bucket.

**Keywords:** "Sum of...", "Total...", "Count how many...", "Longest...", "Smallest..."

```javascript
// Template:
function accumulate(arr) {
  let bucket = 0;  // or "" or -Infinity or [] depending on goal
  for (let i = 0; i < arr.length; i++) {
    bucket = bucket + arr[i];  // or max, min, concat, etc.
  }
  return bucket;
}
```

**Real Example — "Find the sum of all numbers"**
```javascript
function sum(arr) {
  let bucket = 0;
  for (let i = 0; i < arr.length; i++) {
    bucket = bucket + arr[i];
  }
  return bucket;
}
// [1, 2, 3] → bucket: 0 → 1 → 3 → 6 → return 6
```

**Real Example — "Find the longest string"**
```javascript
function longestWord(words) {
  let longest = "";
  for (let i = 0; i < words.length; i++) {
    if (words[i].length > longest.length) {
      longest = words[i];
    }
  }
  return longest;
}
```

**More problems that use this:**
- Find the maximum / minimum value
- Count how many even numbers
- Find the average
- Concatenate all strings

---

### Pattern 3: 🏆 Predicate Selection [The Filter]

**Use when:** You want a SHORTER list — only the items that pass a test.

**The Logic:** Start with an empty Winners list. Test each item. If it passes, `push` it in.

**Keywords:** "Find all...", "Filter out...", "Keep only...", "Remove the..."

```javascript
// Template:
function filterItems(arr) {
  const winners = [];
  for (let i = 0; i < arr.length; i++) {
    if (/* PASSES THE TEST */) {
      winners.push(arr[i]);
    }
  }
  return winners;
}
```

**Real Example — "Get all even numbers"**
```javascript
function getEvens(arr) {
  const winners = [];
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] % 2 === 0) {
      winners.push(arr[i]);
    }
  }
  return winners;
}
// [1, 2, 3, 4, 5] → winners: [] → [2] → [2, 4] → return [2, 4]
```

**Real Example — "Remove Duplicates" (our problem!)**
```javascript
function removeDuplicates(arr) {
  const winners = [];
  const seen = new Set();
  for (let i = 0; i < arr.length; i++) {
    if (!seen.has(arr[i])) {     // Test: "Is this new?"
      seen.add(arr[i]);
      winners.push(arr[i]);      // Winner! First time seeing it.
    }
  }
  return winners;
}
```

**More problems that use this:**
- Get all strings longer than 5 characters
- Find all numbers greater than 10
- Remove all falsy values
- Get all unique elements ← OUR PROBLEM

---

### Pattern 4: 🔄 Data Projection [The Transformer]

**Use when:** You want a list of the **SAME SIZE** but with **changed** data.

**The Logic:** Walk through the list. Change every item. Put the changed version in a new list.

**Keywords:** "Convert each...", "Transform...", "Double every...", "Format all..."

```javascript
// Template:
function transformItems(arr) {
  const transformed = [];
  for (let i = 0; i < arr.length; i++) {
    transformed.push(/* CHANGED VERSION of arr[i] */);
  }
  return transformed;
}
```

**Real Example — "Double every number"**
```javascript
function doubleAll(arr) {
  const transformed = [];
  for (let i = 0; i < arr.length; i++) {
    transformed.push(arr[i] * 2);
  }
  return transformed;
}
// [1, 2, 3] → [2, 4, 6]  (same size, changed values)
```

**Real Example — "Get the first letter of each word"**
```javascript
function initials(words) {
  const transformed = [];
  for (let i = 0; i < words.length; i++) {
    transformed.push(words[i][0]);
  }
  return transformed;
}
// ["Hello", "World"] → ["H", "W"]
```

**More problems that use this:**
- Convert temperatures from C to F
- Capitalize every string
- Square every number
- Extract one property from each object

---

### Pattern 5: 🔎 Lookup / Existence [The Memory]

**Use when:** You need to answer "Have I seen this before?" or "Does this exist?"

**The Logic:** Use a **Set** or **Map** as "Memory." Store things you've seen. Check the memory before acting.

**Keywords:** "Find duplicates", "Two numbers that add to...", "First unique...", "Count frequency..."

```javascript
// Template:
function lookupPattern(arr) {
  const memory = new Set();  // or new Map() for key-value pairs
  for (let i = 0; i < arr.length; i++) {
    if (memory.has(/* what I'm looking for */)) {
      // FOUND IT → do something
    }
    memory.add(arr[i]);  // Remember this for later
  }
}
```

**Real Example — "Does the array contain duplicates?"**
```javascript
function hasDuplicates(arr) {
  const memory = new Set();
  for (let i = 0; i < arr.length; i++) {
    if (memory.has(arr[i])) {
      return true;   // Seen before → duplicate exists!
    }
    memory.add(arr[i]);
  }
  return false;  // Never saw a repeat
}
```

**Real Example — "Two Sum" (find two numbers that add to target)**
```javascript
function twoSum(nums, target) {
  const memory = new Map();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (memory.has(complement)) {
      return [memory.get(complement), i];  // Found the pair!
    }
    memory.set(nums[i], i);
  }
  return [];
}
```

**More problems that use this:**
- Remove duplicates ← OUR PROBLEM
- Find the first non-repeating character
- Check if two strings are anagrams
- Group anagrams together

---

### Pattern 6: 📊 Compare Neighbors [The Pointer Walker]

**Use when:** You need to compare elements WITH EACH OTHER (next, previous, or both ends).

**The Logic:** Use **two pointers** (two index variables). Move them based on comparisons.

**Keywords:** "Sorted array", "Pair that...", "Palindrome", "Merge two...", "Remove in-place"

```javascript
// Template A: Opposite Ends (start from both sides)
function twoPointers(arr) {
  let left = 0;
  let right = arr.length - 1;
  while (left < right) {
    // Compare arr[left] and arr[right]
    // Move left++ or right-- based on result
  }
}

// Template B: Same Direction (fast and slow)
function walkForward(arr) {
  let write = 0;
  for (let read = 0; read < arr.length; read++) {
    if (/* condition */) {
      arr[write] = arr[read];
      write++;
    }
  }
}
```

**Real Example — "Is this string a palindrome?"**
```javascript
function isPalindrome(str) {
  let left = 0;
  let right = str.length - 1;
  while (left < right) {
    if (str[left] !== str[right]) return false;
    left++;
    right--;
  }
  return true;
}
```

**Real Example — "Remove duplicates from SORTED array in-place"**
```javascript
function removeDupsSorted(nums) {
  if (nums.length === 0) return 0;
  let write = 0;
  for (let read = 1; read < nums.length; read++) {
    if (nums[read] !== nums[write]) {
      write++;
      nums[write] = nums[read];
    }
  }
  return write + 1;
}
```

---

### 🧭 The Pattern Picker — Quick Decision Guide

Ask these questions in order:

```
Q1: "Do I need a YES/NO answer?"
     → YES: Pattern 1 — Boolean Validation (Detective)

Q2: "Do I need ONE value from a list?" (sum, max, count, longest)
     → YES: Pattern 2 — Accumulation (Bucket)

Q3: "Do I need a SHORTER list?" (filter, remove, keep only)
     → YES: Pattern 3 — Predicate Selection (Filter)

Q4: "Do I need a SAME-SIZE list with CHANGED items?"
     → YES: Pattern 4 — Data Projection (Transformer)

Q5: "Do I need to check EXISTENCE or find RELATIONSHIPS?"
     → YES: Pattern 5 — Lookup (Memory with Set/Map)

Q6: "Do I need to COMPARE elements with each other?"
     → YES: Pattern 6 — Compare Neighbors (Pointer Walker)
```

### ✅ Phase 2 Checklist

```
[✓] I asked the 6 questions above
[✓] I identified the pattern (or combo of patterns)
[✓] I know which template to start with
```

---

## PHASE 3: The Human Algorithm [Pseudo-coding]

> **The Goal:** Solve it in plain English BEFORE touching the keyboard.
> **The Rule:** If you can't explain it with a pen and paper, you can't code it.

### 3.1 — Write the "Recipe" for a Human

Pretend you're explaining to a friend who knows ZERO about coding. Use these keywords:

```
REPEAT     → loop
IF / THEN  → condition
STORE      → create a variable
COMPARE    → check two values
ADD TO     → push to a list
GIVE BACK  → return
```

### 3.2 — Example: Remove Duplicates

**Plain English Recipe:**

```
1. STORE an empty "seen" box (to remember what I've encountered)
2. STORE an empty "result" bag (to collect unique items)
3. REPEAT for each item in the list:
   a. COMPARE: Is this item already in my "seen" box?
   b. IF yes → SKIP it (it's a duplicate)
   c. IF no  → PUT it in the "seen" box AND ADD it to the "result" bag
4. GIVE BACK the "result" bag
```

### 3.3 — Example: Find the Average

```
1. STORE a bucket with value 0 (for the running total)
2. REPEAT for each number in the list:
   a. ADD the number to the bucket
3. DIVIDE the bucket by how many numbers there are
4. GIVE BACK the answer
```

### 3.4 — Example: Is the Array Sorted?

```
1. REPEAT for each pair of neighbors in the list:
   a. COMPARE the current item with the next item
   b. IF current > next → GIVE BACK "false" (it's not sorted)
2. If I survived the whole loop → GIVE BACK "true"
```

### ✅ Phase 3 Checklist

```
[✓] I wrote the recipe in plain English
[✓] It uses REPEAT, IF/THEN, STORE, COMPARE, ADD TO, GIVE BACK
[✓] A non-coder could follow my recipe
[✓] I haven't written any code yet
```

---

## PHASE 4: Mental Simulation [The Trace Table]

> **The Goal:** Act like the computer. Prove your recipe works BEFORE coding.
> **This is the MOST IMPORTANT phase.** This builds your "brain muscle."

### 4.1 — Draw the Table

Create columns for every variable in your recipe.

### 4.2 — Walk Through a Tiny Input

Use a SMALL example (3-5 items). Update every cell as you follow your recipe.

### Example: Remove Duplicates with `[1, 3, 2, 3]`

```
Recipe Reminder:
  - "seen" box starts empty
  - "result" bag starts empty
  - For each item: check seen → if new, add to both

┌──────┬──────┬────────────────┬──────────────┬────────────────────┐
│ Step │ Item │ In "seen" box? │ seen (after) │ result (after)     │
├──────┼──────┼────────────────┼──────────────┼────────────────────┤
│  1   │  1   │ NO → it's new  │ {1}          │ [1]                │
│  2   │  3   │ NO → it's new  │ {1, 3}       │ [1, 3]             │
│  3   │  2   │ NO → it's new  │ {1, 3, 2}    │ [1, 3, 2]          │
│  4   │  3   │ YES → skip     │ {1, 3, 2}    │ [1, 3, 2]          │
└──────┴──────┴────────────────┴──────────────┴────────────────────┘

Final result: [1, 3, 2]
Expected:     [1, 3, 2]
Match? ✅ YES — My recipe is correct!
```

### Example: Find Average with `[10, 20, 30]`

```
Recipe: bucket starts at 0, add each number, divide by count

┌──────┬────────┬────────────────┐
│ Step │ Number │ Bucket (after) │
├──────┼────────┼────────────────┤
│  1   │  10    │ 0 + 10 = 10   │
│  2   │  20    │ 10 + 20 = 30  │
│  3   │  30    │ 30 + 30 = 60  │
└──────┴────────┴────────────────┘

Divide: 60 / 3 = 20
Expected: 20
Match? ✅ YES
```

### Example: Is Sorted? with `[1, 5, 3]`

```
Recipe: compare each pair, if current > next → false

┌──────┬─────────┬──────────┬──────────────────┐
│ Step │ Current │ Next     │ current > next?   │
├──────┼─────────┼──────────┼──────────────────┤
│  1   │    1    │   5      │ 1 > 5? NO → next │
│  2   │    5    │   3      │ 5 > 3? YES → ❌   │
└──────┴─────────┴──────────┴──────────────────┘

Return: false
Expected: false (it's NOT sorted)
Match? ✅ YES
```

### 4.3 — Also Test ONE Edge Case

Always trace through at least one tricky input:

```
Remove Duplicates with [] (empty array):
  - "seen" box starts empty
  - "result" bag starts empty
  - REPEAT: nothing to repeat (0 items)
  - GIVE BACK: []

Expected: []
Match? ✅ YES
```

### ✅ Phase 4 Checklist

```
[✓] I drew a trace table with columns for each variable
[✓] I walked through a normal input (3-5 items)
[✓] My table result matches the expected output
[✓] I traced at least one edge case
[✓] I have NOT opened my code editor yet
```

---

## PHASE 5: Implementation [Translation to Code]

> **The Goal:** Convert your VERIFIED English recipe into JavaScript.
> **The Rule:** You're just TRANSLATING now. The hard thinking is already done.

### 5.1 — Comment-First Method

Paste your recipe as comments. Then write ONE line of code for each comment.

```javascript
function removeDuplicates(arr) {
  // Edge case: if empty or single item, return as-is
  if (arr.length <= 1) return [...arr];

  // STORE an empty "seen" box
  const seen = new Set();

  // STORE an empty "result" bag
  const result = [];

  // REPEAT for each item in the list
  for (let i = 0; i < arr.length; i++) {

    // COMPARE: Is this item already in "seen"?
    if (!seen.has(arr[i])) {

      // IF no → PUT in seen AND ADD to result
      seen.add(arr[i]);
      result.push(arr[i]);
    }
    // IF yes → do nothing (skip)
  }

  // GIVE BACK the result bag
  return result;
}
```

### 5.2 — The Translation Dictionary

```
PLAIN ENGLISH          →  JAVASCRIPT
─────────────────────────────────────────────────
STORE a value          →  let x = ... / const x = ...
STORE an empty box     →  const seen = new Set()
STORE an empty bag     →  const result = []
REPEAT for each item   →  for (let i = 0; i < arr.length; i++)
   (or)                →  for (const item of arr)
IF something           →  if (condition) { ... }
IF NOT                 →  if (!condition) { ... }
COMPARE two values     →  if (a === b) / if (a > b)
ADD TO the bag         →  result.push(item)
PUT IN the box         →  seen.add(item)
IS IN the box?         →  seen.has(item)
GIVE BACK              →  return result
SKIP / DO NOTHING      →  continue (or just don't write anything)
```

### 5.3 — Naming Rules

```
GOOD names (explain what they ARE):           BAD names:
─────────────────────────────────────────────────────────
const seen = new Set()                        const s = new Set()
const uniqueItems = []                        const a = []
let totalPrice = 0                            let t = 0
let isValid = true                            let f = true
function removeDuplicates(arr)                function fn(a)
```

### ✅ Phase 5 Checklist

```
[✓] Pasted my recipe as comments first
[✓] Wrote one line of code per comment
[✓] Used meaningful variable names
[✓] Used === (not ==)
[✓] Handled edge cases at the top
[✓] Code reads like my English recipe
```

---

## PHASE 6: Refinement [Refactoring & Edge Cases]

> **The Goal:** Make your working code clean, fast, and "senior-level."

### 6.1 — Run Your Edge Cases

```javascript
console.log(removeDuplicates([]));                 // []
console.log(removeDuplicates([7]));                // [7]
console.log(removeDuplicates([5, 5, 5]));          // [5]
console.log(removeDuplicates([1, 2, 3]));          // [1, 2, 3]
console.log(removeDuplicates([-1, 0, -1]));        // [-1, 0]
console.log(removeDuplicates([1, 3, 2, 3, 4, 1]));// [1, 3, 2, 4]
```

### 6.2 — The Optimization Questions

```
Q: "Can I make this faster?"
   My solution is O(n). That's already optimal. ✓

Q: "Can I use less memory?"
   Only if the array is sorted (then use two pointers for O(1) space).
   For unsorted, O(n) space is necessary. ✓

Q: "Can I remove any unnecessary variables?"
   The Set and result are both needed. ✓

Q: "Is there a simpler way to write this?"
   Yes — the one-liner: [...new Set(arr)]
   But the manual version shows understanding. Both are valid.
```

### 6.3 — The Clean-Up Checklist

```
[✓] Every variable has a clear name
[✓] No "magic numbers" (unexplained raw values)
[✓] Edge cases handled at the top
[✓] No console.logs left in the code
[✓] Code is indented consistently
[✓] A teammate could understand this in 5 seconds
```

### 6.4 — Before vs After

**Before (messy but works):**
```javascript
function f(a) {
  var s = new Set();
  var r = [];
  for (var i = 0; i < a.length; i++) {
    if (!s.has(a[i])) { s.add(a[i]); r.push(a[i]); }
  }
  return r;
}
```

**After (clean and professional):**
```javascript
function removeDuplicates(arr) {
  if (!arr || arr.length <= 1) return arr ? [...arr] : [];

  const seen = new Set();
  const uniqueItems = [];

  for (const item of arr) {
    if (!seen.has(item)) {
      seen.add(item);
      uniqueItems.push(item);
    }
  }

  return uniqueItems;
}
```

### ✅ Phase 6 Checklist

```
[✓] All edge cases pass
[✓] I know my time and space complexity
[✓] Code is clean and readable
[✓] I considered if a simpler version exists
[✓] A teammate could read this in 5 seconds
```

---

## THE MASTER SUMMARY

### All 6 Phases at a Glance

```
┌───────┬──────────────────────┬───────────────────┬──────────────────────┐
│ Phase │ Formal Name          │ Tool              │ Time Spent           │
├───────┼──────────────────────┼───────────────────┼──────────────────────┤
│   1   │ Decomposition        │ Thinking          │ 5-10 min             │
│   2   │ Pattern Matching     │ Mental Library    │ 2-5 min              │
│   3   │ Pseudo-code          │ Plain English     │ 5 min                │
│   4   │ Dry Run              │ Pen & Paper       │ 5-10 min ★ CRITICAL  │
│   5   │ Implementation       │ VS Code           │ 5-10 min             │
│   6   │ Refinement           │ Code Review       │ 5 min                │
└───────┴──────────────────────┴───────────────────┴──────────────────────┘
```

### The 6 Patterns at a Glance

```
┌─────┬───────────────────┬────────────────────────────┬───────────────────┐
│  #  │ Pattern Name      │ Use When                   │ Template Start    │
├─────┼───────────────────┼────────────────────────────┼───────────────────┤
│  1  │ 🔍 Detective       │ Need true/false answer     │ Look for Spoiler  │
│  2  │ 🪣 Bucket          │ Need ONE value from list   │ Start with 0 / "" │
│  3  │ 🏆 Filter          │ Need a SHORTER list        │ Empty winners []  │
│  4  │ 🔄 Transformer     │ Need SAME SIZE, new values │ Change each item  │
│  5  │ 🔎 Memory          │ Need to check "seen this?" │ Use a Set / Map   │
│  6  │ 📊 Pointer Walker  │ Need to compare elements   │ Two index vars    │
│     │                   │ with each other            │                   │
└─────┴───────────────────┴────────────────────────────┴───────────────────┘
```

### The Pattern Picker Flowchart

```
START: "What does the problem ask me to return?"
  │
  ├─→ true / false?          → Pattern 1: 🔍 Detective
  │
  ├─→ One number / string?   → Pattern 2: 🪣 Bucket
  │
  ├─→ A shorter list?        → Pattern 3: 🏆 Filter
  │
  ├─→ A same-size list       → Pattern 4: 🔄 Transformer
  │   with changed items?
  │
  ├─→ Need "seen before?"    → Pattern 5: 🔎 Memory (Set/Map)
  │   or find relationships?
  │
  └─→ Need to compare items  → Pattern 6: 📊 Pointer Walker
      with their neighbors?
```

---

## YOUR DAILY PRACTICE PLAN

### Week 1 — Do ONLY Phases 1-4 on Paper (No Code!)

```
Day 1: "Find the sum of an array"                  → Pattern 2 (Bucket)
Day 2: "Is the array sorted?"                      → Pattern 1 (Detective)
Day 3: "Get all even numbers"                      → Pattern 3 (Filter)
Day 4: "Double every number"                       → Pattern 4 (Transformer)
Day 5: "Does the array have duplicates?"            → Pattern 5 (Memory)
Day 6: "Is this string a palindrome?"               → Pattern 6 (Pointer Walker)
Day 7: "Remove duplicates from array"               → Pattern 3 + 5 (Filter + Memory)
```

### Week 2 — Add Phases 5-6 (Now Code It)

Redo the same 7 problems but now translate your paper recipe into JavaScript.

### Week 3 — New Problems, All 6 Phases

```
Day 1: "Find the maximum value"                    → Pattern 2
Day 2: "Are all strings the same length?"           → Pattern 1
Day 3: "Get words longer than 5 characters"         → Pattern 3
Day 4: "Convert Celsius array to Fahrenheit"        → Pattern 4
Day 5: "Two Sum"                                    → Pattern 5
Day 6: "Merge two sorted arrays"                    → Pattern 6
Day 7: "Count frequency of each character"          → Pattern 5
```

### The 15-Minute Rule

> Pick ONE problem. Set a 15-minute timer. Do ONLY Phases 1-4 on paper.
> No code editor. No Google. Just your brain and a pen.
> This single habit will transform your problem-solving ability faster than anything else.

---

## GOLDEN RULES TO REMEMBER

```
Rule 1: "If you can't explain it in English, you can't code it."
Rule 2: "Brute force FIRST. Optimize SECOND."
Rule 3: "The trace table never lies. Trust the table."
Rule 4: "Every problem is just one of the 6 patterns wearing a costume."
Rule 5: "Spend 60% of your time THINKING, 40% TYPING."
Rule 6: "Edge cases are where bugs hide. Always check empty and single."
Rule 7: "Name variables like you're writing for a teammate, not a compiler."
```
