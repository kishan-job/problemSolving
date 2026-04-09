# The Complete Blueprint — Problem Solving Mastery
## Everything You Need in One Place

---

# Table of Contents

- [Section A: Prerequisites — Data Structures You Must Know First](#section-a-prerequisites)
  - [1. Arrays](#1-arrays)
  - [2. Strings](#2-strings)
  - [3. Set](#3-set)
  - [4. Map (Hash Map)](#4-map-hash-map)
  - [5. Stack](#5-stack)
  - [6. Queue](#6-queue)
  - [7. Linked List](#7-linked-list)
  - [8. Tree (Binary Tree)](#8-tree-binary-tree)
  - [9. Graph](#9-graph)
  - [Data Structure Picker](#data-structure-picker)
- [Section B: The 6 Phases — How to Solve ANY Problem](#section-b-the-6-phases)
  - [Phase 1: Decompose](#phase-1-decompose)
  - [Phase 2: Identify the Pattern](#phase-2-identify-the-pattern)
  - [Phase 3: Pseudo-code](#phase-3-pseudo-code)
  - [Phase 4: Dry Run (Trace Table)](#phase-4-dry-run)
  - [Phase 5: Code](#phase-5-code)
  - [Phase 6: Refine](#phase-6-refine)
  - [Translation Dictionary (English → JavaScript)](#translation-dictionary)
- [Section C: All Patterns (4 Levels)](#section-c-all-patterns)
  - [Level 1 — Foundation (Solves 80%)](#level-1--foundation-solves-80-of-problems)
    - [Pattern 1: 🔍 Detective](#pattern-1--detective-boolean-validation)
    - [Pattern 2: 🪣 Bucket](#pattern-2--bucket-accumulation)
    - [Pattern 3: 🏆 Filter](#pattern-3--filter-predicate-selection)
    - [Pattern 4: 🔄 Transformer](#pattern-4--transformer-data-projection)
    - [Pattern 5: 🔎 Memory](#pattern-5--memory-lookup--existence)
    - [Pattern 6: 📊 Pointer Walker](#pattern-6--pointer-walker-two-pointers)
    - [Level 1 Pattern Picker](#level-1-pattern-picker)
  - [Level 2 — Intermediate (Solves 15% more)](#level-2--intermediate-solves-15-more)
    - [Pattern 7: 🪟 Sliding Window](#pattern-7--sliding-window)
    - [Pattern 8: 🔪 Binary Search](#pattern-8--binary-search)
    - [Pattern 9: 🏗️ Stack Pattern](#pattern-9--stack-pattern)
    - [Pattern 10: 🌳 Tree DFS](#pattern-10--tree-dfs)
    - [Pattern 11: 🌊 BFS](#pattern-11--bfs)
  - [Level 3 — Advanced (Solves 4% more)](#level-3--advanced-solves-4-more)
    - [Pattern 12: 🧩 Dynamic Programming](#pattern-12--dynamic-programming)
    - [Pattern 13: 🔙 Backtracking](#pattern-13--backtracking)
    - [Pattern 14: 🏃 Greedy](#pattern-14--greedy)
  - [Level 4 — Specialist (Solves remaining 1%)](#level-4--specialist-solves-remaining-1)
- [Section D: Pattern Combos — Real Problems Use 2+ Patterns](#section-d-pattern-combos)
- [Section E: Big-O Cheat Sheet](#section-e-big-o-cheat-sheet)
  - [Common Complexities Ranked](#common-complexities-ranked)
  - [Every Pattern's Complexity](#every-patterns-complexity)
  - [Data Structure Operations](#data-structure-operations)
- [Section F: How Patterns Connect](#section-f-how-patterns-connect)
- [Section G: Key Insights](#section-g-key-insights)
- [Section H: 35-Day Practice Roadmap](#section-h-35-day-practice-roadmap)
  - [Week 1-2: Level 1](#week-1-2-level-1-patterns)
  - [Week 3-4: Level 2](#week-3-4-level-2-patterns)
  - [Week 5: Level 3](#week-5-level-3-patterns)
- [Section I: Golden Rules](#section-i-golden-rules)

---

# Section A: Prerequisites

> **You MUST know these data structures before learning patterns.**
> Patterns are strategies. Data structures are the tools those strategies use.
> You can't play chess if you don't know how the pieces move.

---

## 1. Arrays

```
WHAT:    Ordered list of items. Access any item by index.
SYNTAX:  const arr = [10, 20, 30]
ACCESS:  arr[0] → 10, arr[2] → 30
```

```javascript
// Create
const fruits = ["apple", "banana", "cherry"];

// Read
fruits[0];          // "apple"
fruits.length;      // 3

// Add
fruits.push("date");       // add to END   → ["apple","banana","cherry","date"]
fruits.unshift("avocado"); // add to START → ["avocado","apple","banana","cherry","date"]

// Remove
fruits.pop();       // remove from END   → returns "date"
fruits.shift();     // remove from START → returns "avocado"

// Search
fruits.indexOf("banana");  // 1 (position) — O(n) slow!
fruits.includes("banana"); // true          — O(n) slow!

// Loop
for (let i = 0; i < fruits.length; i++) { console.log(fruits[i]); }
for (const fruit of fruits) { console.log(fruit); }

// Other useful methods
fruits.slice(1, 3);         // ["banana","cherry"] (copy a section, does NOT modify original)
fruits.splice(1, 1);        // removes 1 item at index 1 (MODIFIES original)
[...fruits];                 // copy the array
fruits.sort((a, b) => a - b); // sort numbers ascending (⚠️ default sort is alphabetical!)
```

```
KEY FACTS:
  ✅ Access by index:  O(1) — instant
  ✅ Push / Pop (end):  O(1) — instant
  ⚠️ Shift / Unshift (start): O(n) — slow (moves everything)
  ⚠️ indexOf / includes: O(n) — slow (scans whole array)
  ⚠️ sort(): O(n log n)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 2. Strings

```
WHAT:    Sequence of characters. IMMUTABLE in JS (can't change in place).
SYNTAX:  const str = "hello"
ACCESS:  str[0] → "h", str.length → 5
```

```javascript
const str = "hello world";

// Access
str[0];             // "h"
str.length;         // 11

// Search
str.indexOf("world");  // 6
str.includes("hello"); // true

// Extract
str.slice(0, 5);    // "hello"
str.split(" ");     // ["hello", "world"]

// Transform (returns NEW string, original unchanged)
str.toUpperCase();  // "HELLO WORLD"
str.toLowerCase();  // "hello world"
str.trim();         // removes whitespace from both ends
str.replace("hello", "hi"); // "hi world"

// Loop through characters
for (const ch of str) { console.log(ch); }

// Convert to array and back
[...str];                    // ["h","e","l","l","o"," ","w","o","r","l","d"]
[..."hello"].reverse().join(""); // "olleh"
```

```
KEY FACTS:
  ✅ Access by index: O(1)
  ⚠️ Strings are IMMUTABLE — every change creates a new string
  ⚠️ Concatenation in a loop is O(n²) — use array + join instead
  ⚠️ indexOf: O(n)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 3. Set

```
WHAT:    Collection of UNIQUE values. No duplicates allowed.
         Instant lookup: "Is this value in the set?" → O(1)
WHY:     Use when you need fast "have I seen this?" checks
```

```javascript
// Create
const seen = new Set();
const colors = new Set(["red", "blue", "green"]);

// Add
seen.add(5);
seen.add(10);
seen.add(5);    // ignored — 5 already exists
// seen = {5, 10}

// Check
seen.has(5);    // true  — O(1) instant!
seen.has(99);   // false — O(1) instant!

// Remove
seen.delete(5); // removes 5

// Size
seen.size;      // 1

// Loop
for (const val of seen) { console.log(val); }

// Convert array to Set and back (removes duplicates!)
const unique = [...new Set([1, 2, 2, 3, 3, 3])]; // [1, 2, 3]
```

```
KEY FACTS:
  ✅ add():    O(1)
  ✅ has():    O(1) — THIS IS WHY SET EXISTS. Array.includes() is O(n).
  ✅ delete(): O(1)
  ✅ Maintains insertion order
  ⚠️ No duplicates — adding same value twice does nothing
  ⚠️ No index access — can't do set[0]
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 4. Map (Hash Map)

```
WHAT:    Key-value pairs. Like Set but each key stores a value.
         Instant lookup: "What value is stored for this key?" → O(1)
WHY:     Use for counting, storing indices, grouping
```

```javascript
// Create
const memory = new Map();

// Set key-value pairs
memory.set("apple", 3);
memory.set("banana", 5);
memory.set("apple", 7);   // overwrites — apple is now 7

// Get
memory.get("apple");   // 7     — O(1)
memory.get("cherry");  // undefined

// Check
memory.has("banana");  // true  — O(1)

// Delete
memory.delete("banana");

// Size
memory.size;  // 1

// Loop
for (const [key, value] of memory) {
  console.log(key, value);
}

// Common pattern: counting frequency
const freq = new Map();
for (const ch of "hello") {
  freq.set(ch, (freq.get(ch) || 0) + 1);
}
// freq = {"h"→1, "e"→1, "l"→2, "o"→1}
```

```
KEY FACTS:
  ✅ set():    O(1)
  ✅ get():    O(1)
  ✅ has():    O(1)
  ✅ Keys can be ANY type (numbers, strings, objects)
  ✅ Maintains insertion order
  ⚠️ Plain objects {} also work as maps BUT keys are always strings
     → Use Map when keys are numbers or you need .size
```

```
SET vs MAP — When to pick:
  "Have I seen this value?"         → Set
  "How many times did I see it?"    → Map (key → count)
  "What index was this value at?"   → Map (key → index)
  "Group items by a category?"      → Map (key → array)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 5. Stack

```
WHAT:    Last-In, First-Out (LIFO). Like a stack of plates.
         The last plate you put on top is the first one you take off.
WHY:     Matching brackets, undo operations, expression evaluation
HOW:     In JS, just use an array with push() and pop()
```

```javascript
const stack = [];

// Push (add to top)
stack.push(10);   // [10]
stack.push(20);   // [10, 20]
stack.push(30);   // [10, 20, 30]

// Peek (look at top without removing)
stack[stack.length - 1];  // 30

// Pop (remove from top)
stack.pop();      // returns 30, stack = [10, 20]
stack.pop();      // returns 20, stack = [10]

// Check if empty
stack.length === 0;  // false
```

```
KEY FACTS:
  ✅ push():   O(1)
  ✅ pop():    O(1)
  ✅ peek:     O(1)
  Think: "Last In, First Out" — like browser back button
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 6. Queue

```
WHAT:    First-In, First-Out (FIFO). Like a line at a shop.
         The first person in line is the first one served.
WHY:     BFS traversal, scheduling, level-order processing
HOW:     In JS, use array with push() and shift() (simple but shift is O(n))
```

```javascript
const queue = [];

// Enqueue (add to back)
queue.push(10);    // [10]
queue.push(20);    // [10, 20]
queue.push(30);    // [10, 20, 30]

// Dequeue (remove from front)
queue.shift();     // returns 10, queue = [20, 30]
queue.shift();     // returns 20, queue = [30]

// Check front without removing
queue[0];          // 30

// Check if empty
queue.length === 0;  // false
```

```
KEY FACTS:
  ✅ push():   O(1)
  ⚠️ shift():  O(n) — moves all remaining elements
  Think: "First In, First Out" — like a queue at a ticket counter
  For performance-critical code, use a linked list queue (rare in interviews)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 7. Linked List

```
WHAT:    Chain of nodes. Each node has a value and a pointer to the next node.
         No index access — must walk from the head.
WHY:     O(1) insert/delete at head. Used in many interview questions.
```

```javascript
// Node structure
class ListNode {
  constructor(val) {
    this.val = val;
    this.next = null;
  }
}

// Create: 1 → 2 → 3
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);

// Traverse
let current = head;
while (current !== null) {
  console.log(current.val);  // 1, 2, 3
  current = current.next;
}
```

```
KEY FACTS:
  ✅ Insert/delete at head: O(1)
  ⚠️ Access by position: O(n) — must walk the chain
  ⚠️ No random access like arrays
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 8. Tree (Binary Tree)

```
WHAT:    Hierarchical structure. Each node has a value, left child, right child.
         Binary Search Tree (BST): left < parent < right
WHY:     Searching, sorting, hierarchical data (file systems, DOM, etc.)
```

```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

// Create:     5
//            / \
//           3   8
const root = new TreeNode(5);
root.left = new TreeNode(3);
root.right = new TreeNode(8);
```

```
KEY FACTS:
  ✅ BST search:   O(log n) average — halves each step
  ⚠️ BST worst case: O(n) — if tree is a straight line (unbalanced)
  ✅ Traversal orders:
     Inorder (left, root, right)   → gives sorted order in BST
     Preorder (root, left, right)  → used for copying trees
     Postorder (left, right, root) → used for deleting trees
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 9. Graph

```
WHAT:    Nodes (vertices) connected by edges. Can be directed or undirected.
WHY:     Social networks, maps, dependencies, shortest paths
HOW:     Usually represented as adjacency list (Map of node → neighbors)
```

```javascript
// Adjacency list representation
// Graph:  0 -- 1
//         |    |
//         2 -- 3
const graph = new Map();
graph.set(0, [1, 2]);
graph.set(1, [0, 3]);
graph.set(2, [0, 3]);
graph.set(3, [1, 2]);

// Access neighbors of node 0
graph.get(0);  // [1, 2]
```

```
KEY FACTS:
  ✅ Adjacency list space: O(V + E)  (V = vertices, E = edges)
  ✅ Check if edge exists: O(degree of node)
  ✅ Traversed with BFS or DFS
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Data Structure Picker

```
"What do I need?"

  Ordered items, access by index       → Array
  Sequence of characters               → String
  Fast "have I seen this?" check        → Set
  Fast key → value lookup               → Map
  Last-in-first-out processing          → Stack (array with push/pop)
  First-in-first-out processing         → Queue (array with push/shift)
  O(1) insert/delete at head            → Linked List
  Hierarchical / sorted data            → Tree
  Connected relationships               → Graph
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section B: The 6 Phases

> Follow these 6 phases for every problem, every time. This is your process.

```
PHASE 1 → DECOMPOSE:   "What goes in? What comes out?"
PHASE 2 → IDENTIFY:    "Which pattern is this?"
PHASE 3 → PSEUDO-CODE: "Write the solution in English"
PHASE 4 → DRY RUN:     "Trace table — prove it on paper"
PHASE 5 → CODE:        "Translate English → JavaScript"
PHASE 6 → REFINE:      "Test edge cases, clean up"
```

---

## Phase 1: Decompose

```
Ask these 5 questions:
  1. What is the INPUT?  (type, size, can it be empty?)
  2. What is the OUTPUT? (boolean? number? array? string?)
  3. What are the RULES? (constraints, restrictions)
  4. Write the GOAL SENTENCE:
     "I take [INPUT] and return [OUTPUT] by doing [X]"
  5. List EDGE CASES:
     → empty input, single item, all same, negatives, already solved
```

---

## Phase 2: Identify the Pattern

```
Ask: "What do I RETURN?"
  true/false?              → 🔍 Detective
  one value (sum/max)?     → 🪣 Bucket
  shorter list?            → 🏆 Filter
  same-size changed list?  → 🔄 Transformer
  "seen before?" check?    → 🔎 Memory
  compare items?           → 📊 Pointer Walker
  (see Section C for all patterns)
```

---

## Phase 3: Pseudo-code

```
Write 3-5 lines of English using these words:
  STORE    → create a variable
  REPEAT   → loop
  IF/THEN  → condition
  COMPARE  → check two values
  ADD TO   → push to a list
  GIVE BACK → return
```

---

## Phase 4: Dry Run

> **This is the most important phase.** This builds your brain muscle.

```
Draw a trace table:
  - Columns = your variables
  - Rows = each step of your recipe
  - Use a small input (3-4 items)
  - If table result = expected result → your logic is correct
```

---

## Phase 5: Code

```
  1. Paste your English recipe as comments
  2. Write ONE line of code per comment
  3. Use meaningful variable names
  4. Handle edge cases at the top
```

---

## Phase 6: Refine

```
  1. Test with edge cases: [], [single], [all same]
  2. Ask: "Can I make it faster?"
  3. Ask: "Would a teammate understand this in 5 seconds?"
```

---

## Translation Dictionary

```
PLAIN ENGLISH          →  JAVASCRIPT
─────────────────────────────────────────
STORE a value          →  const x = value
STORE empty list       →  const result = []
STORE empty memory     →  const seen = new Set()
REPEAT for each        →  for (let i = 0; i < arr.length; i++)
IF something           →  if (condition) { }
COMPARE two values     →  if (a === b)
ADD TO list            →  result.push(item)
PUT IN memory          →  seen.add(item)
CHECK memory           →  seen.has(item)
GIVE BACK              →  return result
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section C: All Patterns

```
Level 1 (6 patterns)  → Solves 80%  → Weeks 1-2
Level 2 (5 patterns)  → Solves 15%  → Weeks 3-4
Level 3 (3 patterns)  → Solves 4%   → Week 5
Level 4 (8 patterns)  → Solves 1%   → As needed
```

---

## Level 1 — Foundation (Solves 80% of problems)

---

### Pattern 1: 🔍 Detective (Boolean Validation)

```
USE WHEN:  You need a true/false answer
KEYWORDS:  "Is it...?", "Are all...?", "Does every...?", "Is valid?"
LOGIC:     Find ONE bad thing → return false. Survive loop → return true.

DON'T USE WHEN:
  ✗ You need to COUNT how many bad things (use 🪣 Bucket instead)
  ✗ You need to COLLECT the bad items (use 🏆 Filter instead)
  ✗ The answer isn't true/false
```

**Template:**
```javascript
function detective(arr) {
  for (let i = 0; i < arr.length; i++) {
    if (/* BAD CONDITION */) return false;
  }
  return true;
}
```

**Example — "Is the array sorted?"**

```
Goal: "I take an array, return true if ascending, false if not"
English:
  REPEAT for each pair of neighbors
    IF left > right THEN return false
  RETURN true
```

```javascript
function isSorted(arr) {
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] > arr[i + 1]) return false;
  }
  return true;
}
```

```
Trace: [1, 5, 3]
  i=0: 1>5? NO
  i=1: 5>3? YES → false ✓
```

**Practice Problems:**
```
□ Is the array sorted?
□ Are all numbers positive?
□ Does string contain only digits?
□ Are all strings the same length?
□ Is it a valid email? (has @ and .)
LeetCode: #1550 Three Consecutive Odds, #896 Monotonic Array
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 2: 🪣 Bucket (Accumulation)

```
USE WHEN:  You need ONE value from a list (sum, max, count, longest)
KEYWORDS:  "Sum of...", "Total...", "Count...", "Maximum...", "Longest..."
LOGIC:     Start with empty bucket. Pour each item in. Return bucket.

DON'T USE WHEN:
  ✗ You need a list of results (use 🏆 Filter or 🔄 Transformer)
  ✗ You need true/false (use 🔍 Detective)
```

**Template:**
```javascript
function bucket(arr) {
  let result = 0;  // starting value depends on goal
  for (let i = 0; i < arr.length; i++) {
    result = /* combine result with arr[i] */;
  }
  return result;
}
```

```
Starting values:
  Sum/Count → 0     Maximum → -Infinity    Minimum → Infinity
  Product   → 1     Longest → ""
```

**Example — "Find the maximum"**

```
Goal: "I take an array, return the biggest number"
English:
  STORE biggest as -Infinity
  REPEAT for each number
    IF number > biggest THEN update biggest
  RETURN biggest
```

```javascript
function findMax(arr) {
  let biggest = -Infinity;
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] > biggest) biggest = arr[i];
  }
  return biggest;
}
```

```
Trace: [3, 7, 2]
  i=0: 3>-∞ YES → biggest=3
  i=1: 7>3  YES → biggest=7
  i=2: 2>7  NO
  Return: 7 ✓
```

**Practice Problems:**
```
□ Find the sum of an array
□ Find the average
□ Find the maximum / minimum
□ Count how many even numbers
□ Find the longest string
□ Multiply all numbers (product)
LeetCode: #1 (sum variant), #53 Max Subarray (Kadane's), #136 Single Number
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 3: 🏆 Filter (Predicate Selection)

```
USE WHEN:  You need a SHORTER list — only items passing a test
KEYWORDS:  "Get all...", "Find all...", "Remove...", "Keep only..."
LOGIC:     Empty winners list. Test each. Pass → push. Fail → skip.

DON'T USE WHEN:
  ✗ You need EVERY item changed (use 🔄 Transformer — same size list)
  ✗ You need one value like a count (use 🪣 Bucket)
```

**Template:**
```javascript
function filter(arr) {
  const winners = [];
  for (let i = 0; i < arr.length; i++) {
    if (/* PASSES TEST */) winners.push(arr[i]);
  }
  return winners;
}
```

**Example — "Get all even numbers"**

```
Goal: "I take an array, return only even numbers"
English:
  STORE empty winners
  REPEAT for each number
    IF number is even THEN add to winners
  RETURN winners
```

```javascript
function getEvens(arr) {
  const winners = [];
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] % 2 === 0) winners.push(arr[i]);
  }
  return winners;
}
```

```
Trace: [1, 4, 3, 8]
  1: odd → skip    winners=[]
  4: even → push   winners=[4]
  3: odd → skip    winners=[4]
  8: even → push   winners=[4,8]
  Return: [4, 8] ✓
```

**Practice Problems:**
```
□ Get all even numbers
□ Get all strings longer than 5 characters
□ Remove all negative numbers
□ Remove all falsy values
□ Get words starting with a vowel
LeetCode: #1filter (no direct LC, but used in #217, #349)
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 4: 🔄 Transformer (Data Projection)

```
USE WHEN:  You need SAME-SIZE list with CHANGED items
KEYWORDS:  "Double each...", "Convert...", "Format...", "Extract..."
LOGIC:     New list. Transform each item. Push transformed version.

DON'T USE WHEN:
  ✗ Some items should be removed (use 🏆 Filter — shorter list)
  ✗ You need one value, not a list (use 🪣 Bucket)
```

**Template:**
```javascript
function transform(arr) {
  const result = [];
  for (let i = 0; i < arr.length; i++) {
    result.push(/* changed version of arr[i] */);
  }
  return result;
}
```

**Example — "Square every number"**

```
Goal: "I take an array, return each number squared"
English:
  STORE empty result
  REPEAT for each number
    ADD (number × number) to result
  RETURN result
```

```javascript
function squareAll(arr) {
  const result = [];
  for (let i = 0; i < arr.length; i++) {
    result.push(arr[i] * arr[i]);
  }
  return result;
}
```

```
Trace: [2, 3, 5]
  2→4, 3→9, 5→25 → [4, 9, 25] ✓
```

**Practice Problems:**
```
□ Double every number
□ Square every number
□ Convert Celsius to Fahrenheit (C × 9/5 + 32)
□ Convert all strings to uppercase
□ Get length of each string: ["hi","hello"] → [2,5]
LeetCode: #977 Squares of Sorted Array
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 5: 🔎 Memory (Lookup / Existence)

```
USE WHEN:  "Have I seen this before?" or "Find a pair/relationship"
KEYWORDS:  "Duplicates", "Two Sum", "Frequency", "First unique", "Anagram"
LOGIC:     Set/Map = instant memory. Check memory before processing.

DON'T USE WHEN:
  ✗ Data is already sorted (use 📊 Pointer Walker — no extra space)
  ✗ You don't need lookup, just filtering by a simple condition (use 🏆 Filter)
```

```
When to use Set vs Map:
  Set → just "have I seen it?"           → seen.has(x), seen.add(x)
  Map → "seen it, and WHAT was stored?"  → map.get(x), map.set(x, value)
```

**Template (Set):**
```javascript
function memorySet(arr) {
  const seen = new Set();
  for (let i = 0; i < arr.length; i++) {
    if (seen.has(arr[i])) { /* FOUND — handle it */ }
    seen.add(arr[i]);
  }
}
```

**Template (Map — for counting):**
```javascript
function memoryMap(arr) {
  const freq = new Map();
  for (let i = 0; i < arr.length; i++) {
    freq.set(arr[i], (freq.get(arr[i]) || 0) + 1);
  }
  return freq;
}
```

**Example — "Remove Duplicates"**

```
Goal: "I take an array, return unique values keeping order"
Pattern: 🔎 Memory (Set) + 🏆 Filter
English:
  STORE empty memory, empty result
  REPEAT for each item
    IF memory does NOT have item
      ADD to memory AND result
  RETURN result
```

```javascript
function removeDuplicates(arr) {
  const seen = new Set();
  const result = [];
  for (const item of arr) {
    if (!seen.has(item)) {
      seen.add(item);
      result.push(item);
    }
  }
  return result;
}
```

```
Trace: [1, 3, 2, 3]
  1: not seen → add → seen={1}     result=[1]
  3: not seen → add → seen={1,3}   result=[1,3]
  2: not seen → add → seen={1,3,2} result=[1,3,2]
  3: SEEN → skip
  Return: [1, 3, 2] ✓
```

**Example — "Two Sum"**

```javascript
function twoSum(nums, target) {
  const memory = new Map();  // value → index
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (memory.has(complement)) return [memory.get(complement), i];
    memory.set(nums[i], i);
  }
  return [];
}
```

**Practice Problems:**
```
□ Does array have duplicates?
□ Remove duplicates
□ Two Sum
□ Count character frequency
□ First non-repeating character
□ Are two strings anagrams?
LeetCode: #1 Two Sum, #217 Contains Duplicate, #242 Valid Anagram,
          #387 First Unique Character, #349 Intersection of Two Arrays
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 6: 📊 Pointer Walker (Two Pointers)

```
USE WHEN:  Compare elements with each other, sorted array problems
KEYWORDS:  "Palindrome", "Sorted array pair", "In-place removal", "Merge"
LOGIC:     Two index variables moving through the data.

DON'T USE WHEN:
  ✗ Data is unsorted and you need pair lookup (use 🔎 Memory — faster)
  ✗ You're not comparing elements with EACH OTHER
```

**Template A — Opposite Ends:**
```javascript
function oppositeEnds(arr) {
  let left = 0, right = arr.length - 1;
  while (left < right) {
    // compare arr[left] and arr[right]
    // move left++ or right--
  }
}
```

**Template B — Same Direction (read/write):**
```javascript
function sameDirection(arr) {
  let write = 0;
  for (let read = 0; read < arr.length; read++) {
    if (/* keep this */) {
      arr[write] = arr[read];
      write++;
    }
  }
  return write;
}
```

**Example — "Is palindrome?"**

```javascript
function isPalindrome(str) {
  let left = 0, right = str.length - 1;
  while (left < right) {
    if (str[left] !== str[right]) return false;
    left++;
    right--;
  }
  return true;
}
```

```
Trace: "racecar"
  left=0(r) right=6(r) → match
  left=1(a) right=5(a) → match
  left=2(c) right=4(c) → match
  left=3 ≥ right=3 → stop → true ✓
```

**Practice Problems:**
```
□ Is string a palindrome?
□ Reverse array in-place
□ Two Sum in SORTED array
□ Remove duplicates from sorted array
□ Move all zeroes to end
□ Merge two sorted arrays
LeetCode: #125 Valid Palindrome, #26 Remove Duplicates from Sorted,
          #167 Two Sum II, #283 Move Zeroes, #88 Merge Sorted Array
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Level 1 Pattern Picker

```
"What do I RETURN?"

  true / false?             → 🔍 Detective
  one number/string?        → 🪣 Bucket
  shorter list?             → 🏆 Filter
  same-size changed list?   → 🔄 Transformer
  "seen before?" / pairs?   → 🔎 Memory
  compare elements?         → 📊 Pointer Walker
```

---

## Level 2 — Intermediate (Solves 15% more)

> **Prerequisite:** Level 1 patterns feel easy. You pick them in under 30 seconds.

---

### Pattern 7: 🪟 Sliding Window

```
USE WHEN:  Best/longest/shortest CONTIGUOUS subarray or substring
KEYWORDS:  "Consecutive", "Subarray of size K", "Longest substring"
LOGIC:     Window slides across data. Add new item, drop old item.
BUILT ON:  📊 Pointer Walker + 🪣 Bucket

DON'T USE WHEN:
  ✗ Elements don't need to be contiguous/adjacent
  ✗ You need to find a pair, not a range (use 🔎 Memory or 📊 Pointer Walker)
```

**Template — Fixed Window:**
```javascript
function fixedWindow(arr, k) {
  let windowSum = 0;
  for (let i = 0; i < k; i++) windowSum += arr[i];
  let best = windowSum;
  for (let i = k; i < arr.length; i++) {
    windowSum += arr[i] - arr[i - k];  // slide: +new -old
    best = Math.max(best, windowSum);
  }
  return best;
}
```

**Template — Variable Window:**
```javascript
function varWindow(arr) {
  let left = 0, best = 0;
  for (let right = 0; right < arr.length; right++) {
    // expand: add arr[right]
    while (/* window invalid */) {
      // shrink: remove arr[left], left++
    }
    best = Math.max(best, right - left + 1);
  }
  return best;
}
```

**Example — "Max sum of K consecutive numbers"**

```javascript
function maxSumK(arr, k) {
  let windowSum = 0;
  for (let i = 0; i < k; i++) windowSum += arr[i];
  let best = windowSum;
  for (let i = k; i < arr.length; i++) {
    windowSum += arr[i] - arr[i - k];
    best = Math.max(best, windowSum);
  }
  return best;
}
```

```
Trace: [1, 4, 2, 7, 1], k=3
  First window [1,4,2] = 7
  Slide: +7-1 = [4,2,7] = 13 → best=13
  Slide: +1-4 = [2,7,1] = 10
  Return: 13 ✓
```

**Practice:**
```
□ Max sum subarray of size K
□ Longest substring without repeating characters
□ Minimum size subarray with sum ≥ target
□ Find all anagrams in a string
LeetCode: #3 Longest Substring, #209 Min Size Subarray Sum,
          #438 Find All Anagrams, #567 Permutation in String
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 8: 🔪 Binary Search

```
USE WHEN:  Find target in SORTED data by halving search space
KEYWORDS:  "Sorted array", "Find position", "First/last occurrence"
LOGIC:     Check middle. Too small → go right. Too big → go left.
BUILT ON:  🔍 Detective (elimination)

DON'T USE WHEN:
  ✗ Data is NOT sorted and can't be sorted
  ✗ You need to check every element anyway
```

**Template:**
```javascript
function binarySearch(arr, target) {
  let lo = 0, hi = arr.length - 1;
  while (lo <= hi) {
    const mid = Math.floor((lo + hi) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}
```

```
Trace: [2, 5, 8, 12, 16], target=12
  lo=0 hi=4 mid=2 → arr[2]=8 < 12 → lo=3
  lo=3 hi=4 mid=3 → arr[3]=12 = 12 → FOUND ✓
```

**Practice:**
```
□ Find target in sorted array
□ Find first/last occurrence
□ Search in rotated sorted array
□ Find integer square root
LeetCode: #704 Binary Search, #34 Find First and Last Position,
          #33 Search in Rotated, #69 Sqrt(x), #162 Find Peak Element
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 9: 🏗️ Stack Pattern

```
USE WHEN:  Matching pairs, undo, Last-In-First-Out processing
KEYWORDS:  "Valid parentheses", "Next greater", "Evaluate expression"
LOGIC:     Push items. Pop when condition met. Check what's on top.

DON'T USE WHEN:
  ✗ Processing order doesn't matter (LIFO is the key signal)
  ✗ You need FIFO (first-in-first-out) → use Queue / BFS
```

**Template:**
```javascript
function stackPattern(input) {
  const stack = [];
  for (const item of input) {
    if (/* should push */) stack.push(item);
    else {
      const top = stack.pop();
      // compare top with item
    }
  }
  return stack.length === 0;
}
```

**Example — "Valid Parentheses"**

```javascript
function isValid(s) {
  const stack = [];
  const pairs = { ')': '(', ']': '[', '}': '{' };
  for (const ch of s) {
    if ('([{'.includes(ch)) stack.push(ch);
    else if (stack.pop() !== pairs[ch]) return false;
  }
  return stack.length === 0;
}
```

```
Trace: "([])"
  ( → push    stack: [(]
  [ → push    stack: [(, []
  ] → pop [   stack: [(]    match ✓
  ) → pop (   stack: []     match ✓
  Empty → true ✓
```

**Practice:**
```
□ Valid parentheses
□ Evaluate Reverse Polish Notation
□ Daily temperatures
□ Next greater element
LeetCode: #20 Valid Parentheses, #150 Evaluate RPN,
          #739 Daily Temperatures, #496 Next Greater Element I, #155 Min Stack
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 10: 🌳 Tree DFS

```
USE WHEN:  Explore a tree deeply using recursion
KEYWORDS:  "Max depth", "Path sum", "Invert tree", "Validate BST"
LOGIC:     Base case (null) → Ask left → Ask right → Combine answers

DON'T USE WHEN:
  ✗ You need level-by-level processing (use 🌊 BFS)
  ✗ You need shortest path (use 🌊 BFS)
```

**Template:**
```javascript
function treeDFS(node) {
  if (!node) return /* base value */;
  const left = treeDFS(node.left);
  const right = treeDFS(node.right);
  return /* combine left and right */;
}
```

**Example — "Max depth"**

```javascript
function maxDepth(node) {
  if (!node) return 0;
  return 1 + Math.max(maxDepth(node.left), maxDepth(node.right));
}
```

```
Trace:    1
         / \
        2   3    → maxDepth = 1 + max(1, 1) = 2 ✓
```

**Practice:**
```
□ Max depth of binary tree
□ Invert binary tree
□ Path sum (root to leaf)
□ Validate BST
LeetCode: #104 Maximum Depth, #226 Invert Tree, #112 Path Sum,
          #98 Validate BST, #100 Same Tree, #572 Subtree of Another
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 11: 🌊 BFS

```
USE WHEN:  Level-by-level traversal or shortest path
KEYWORDS:  "Level order", "Shortest path", "Nearest", "Layer by layer"
LOGIC:     Queue. Process all nodes at current level. Add children for next.

DON'T USE WHEN:
  ✗ You need to explore all paths to leaves (use 🌳 DFS)
  ✗ You need to search for something that could be anywhere (use 🌳 DFS)
```

**Template:**
```javascript
function bfs(root) {
  if (!root) return [];
  const queue = [root], result = [];
  while (queue.length) {
    const size = queue.length;
    const level = [];
    for (let i = 0; i < size; i++) {
      const node = queue.shift();
      level.push(node.val);
      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    result.push(level);
  }
  return result;
}
```

```
Trace:     1
          / \
         2   3     → [[1], [2, 3]] ✓
```

**Practice:**
```
□ Level order traversal
□ Shortest path in grid
□ Number of islands
□ Rotting oranges
LeetCode: #102 Level Order, #200 Number of Islands,
          #994 Rotting Oranges, #542 01 Matrix, #199 Right Side View
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 3 — Advanced (Solves 4% more)

> **Prerequisite:** Level 2 patterns feel comfortable.

---

### Pattern 12: 🧩 Dynamic Programming

```
USE WHEN:  Overlapping sub-problems + optimal substructure
KEYWORDS:  "How many ways?", "Minimum cost", "Can I reach?", "Longest..."
LOGIC:     Store answers to small problems. Build up to the big answer.
BUILT ON:  🪣 Bucket + 🔎 Memory + Recursion

DON'T USE WHEN:
  ✗ Sub-problems don't overlap (each sub-problem is unique → use Divide & Conquer)
  ✗ A greedy/local choice is provably optimal (use 🏃 Greedy — simpler)

3-Step Recipe:
  1. DEFINE: "What does dp[i] mean?"
  2. RELATE: "dp[i] = some formula using dp[i-1], dp[i-2], etc."
  3. BASE:   "dp[0] = ?, dp[1] = ?"
```

**Template:**
```javascript
function dp(n) {
  let prev2 = /* base 0 */, prev1 = /* base 1 */;
  for (let i = 2; i <= n; i++) {
    const curr = /* combine prev1 and prev2 */;
    prev2 = prev1;
    prev1 = curr;
  }
  return prev1;
}
```

**Example — "Climbing Stairs"**

```
dp[i] = ways to reach step i = dp[i-1] + dp[i-2]
```

```javascript
function climbStairs(n) {
  let prev2 = 1, prev1 = 1;
  for (let i = 2; i <= n; i++) {
    const curr = prev1 + prev2;
    prev2 = prev1;
    prev1 = curr;
  }
  return prev1;
}
```

```
Trace: n=5
  dp[0]=1  dp[1]=1  dp[2]=2  dp[3]=3  dp[4]=5  dp[5]=8 ✓
```

**Practice (in order):**
```
□ Climbing Stairs
□ House Robber
□ Coin Change
□ Longest Increasing Subsequence
LeetCode: #70 Climbing Stairs, #198 House Robber, #322 Coin Change,
          #300 LIS, #1143 Longest Common Subsequence, #416 Partition Equal Subset
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 13: 🔙 Backtracking

```
USE WHEN:  Generate ALL combinations, permutations, valid configurations
KEYWORDS:  "All subsets", "All permutations", "N-Queens", "Sudoku"
LOGIC:     CHOOSE → EXPLORE → UN-CHOOSE (undo). Always this order.
BUILT ON:  🌳 DFS + trial-and-error

DON'T USE WHEN:
  ✗ You need just ONE solution or the optimal one (use 🧩 DP or 🏃 Greedy)
  ✗ You can solve it with a simple loop (backtracking is expensive: O(2^n) or O(n!))
```

**Template:**
```javascript
function backtrack(result, current, choices, start) {
  result.push([...current]);       // save snapshot
  for (let i = start; i < choices.length; i++) {
    current.push(choices[i]);       // CHOOSE
    backtrack(result, current, choices, i + 1);  // EXPLORE
    current.pop();                  // UN-CHOOSE
  }
}
```

**Example — "All subsets of [1,2,3]"**

```javascript
function subsets(nums) {
  const result = [];
  function bt(start, current) {
    result.push([...current]);
    for (let i = start; i < nums.length; i++) {
      current.push(nums[i]);
      bt(i + 1, current);
      current.pop();
    }
  }
  bt(0, []);
  return result;
}
// → [[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]]
```

**Practice:**
```
□ All subsets
□ All permutations
□ Combination Sum
□ Word Search in grid
LeetCode: #78 Subsets, #46 Permutations, #39 Combination Sum,
          #79 Word Search, #51 N-Queens, #17 Letter Combos of Phone Number
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 14: 🏃 Greedy

```
USE WHEN:  Best local choice leads to best global result
KEYWORDS:  "Maximum events", "Minimum stops", "Jump game"
LOGIC:     Sort (usually). Pick best option NOW. Never look back.

DON'T USE WHEN:
  ✗ Local best ≠ global best (use 🧩 DP instead)
  ✗ You can't prove the greedy choice is safe
  Tip: If unsure whether greedy works, default to DP. It's always correct.
```

**Template:**
```javascript
function greedy(items) {
  items.sort(/* criteria */);
  let result = 0, position = 0;
  for (const item of items) {
    if (/* compatible */) {
      result++;
      position = item.end;
    }
  }
  return result;
}
```

**Example — "Jump Game"**

```javascript
function canJump(nums) {
  let maxReach = 0;
  for (let i = 0; i < nums.length; i++) {
    if (i > maxReach) return false;
    maxReach = Math.max(maxReach, i + nums[i]);
  }
  return true;
}
```

```
Trace: [2, 3, 1, 1, 4]
  i=0: maxReach = max(0, 0+2) = 2
  i=1: maxReach = max(2, 1+3) = 4
  i=2: maxReach = max(4, 2+1) = 4
  i=3: maxReach = max(4, 3+1) = 4
  i=4: reached end → true ✓
```

**Practice:**
```
□ Jump Game
□ Best Time to Buy/Sell Stock
□ Max non-overlapping intervals
LeetCode: #55 Jump Game, #121 Best Time to Buy/Sell Stock,
          #435 Non-overlapping Intervals, #455 Assign Cookies
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 4 — Specialist (Solves remaining 1%)

> Learn these as you encounter them.

| # | Pattern | Use When | LeetCode |
|---|---------|----------|----------|
| 15 | ⛰️ Heap | "Kth largest", "Top K frequent", "Running median" | #215, #347, #295 |
| 16 | 📚 Trie | "Autocomplete", "Spell check", "Starts with prefix" | #208, #211, #212 |
| 17 | Prefix Sum | "Sum of range [i..j]", "Subarray sum equals K" | #303, #560, #238 |
| 18 | Merge Intervals | "Overlapping meetings", "Merge ranges" | #56, #57, #252 |
| 19 | Union-Find | "Are nodes connected?", "Number of components" | #547, #684, #323 |
| 20 | Topological Sort | "Course prerequisites", "Build order" | #207, #210, #269 |
| 21 | Bit Manipulation | "Single number (XOR)", "Power of 2" | #136, #191, #338 |
| 22 | Monotonic Stack | "Next greater element", "Largest rectangle" | #496, #503, #84 |

[↑ Back to Table of Contents](#table-of-contents)

---

# Section D: Pattern Combos

> Most real problems use 2 patterns together. Here are the most common combos.

### Combo 1: 🔎 Memory + 🏆 Filter → "Remove Duplicates"

```
Why two patterns?
  🔎 Memory answers: "Have I seen this before?"
  🏆 Filter answers: "Should I keep this item?"
  Together: "Keep only items I haven't seen before"
```

```javascript
function removeDuplicates(arr) {
  const seen = new Set();       // 🔎 Memory
  const result = [];             // 🏆 Filter
  for (const item of arr) {
    if (!seen.has(item)) {       // Memory check
      seen.add(item);
      result.push(item);         // Filter: keep it
    }
  }
  return result;
}
// LeetCode: #26, #217
```

---

### Combo 2: 🔎 Memory + 🔍 Detective → "Contains Duplicate"

```
Why two patterns?
  🔎 Memory: stores what we've seen
  🔍 Detective: the moment we find a duplicate → return true (early exit)
```

```javascript
function containsDuplicate(arr) {
  const seen = new Set();       // 🔎 Memory
  for (const item of arr) {
    if (seen.has(item)) return true;  // 🔍 Detective: found the spoiler
    seen.add(item);
  }
  return false;                 // 🔍 Detective: survived → false
}
// LeetCode: #217
```

---

### Combo 3: 🪟 Sliding Window + 🔎 Memory → "Longest Substring Without Repeat"

```
Why two patterns?
  🪟 Sliding Window: tracks the current window [left...right]
  🔎 Memory (Set): tracks which characters are in the window
```

```javascript
function lengthOfLongestSubstring(s) {
  const inWindow = new Set();    // 🔎 Memory
  let left = 0, best = 0;       // 🪟 Window
  for (let right = 0; right < s.length; right++) {
    while (inWindow.has(s[right])) {
      inWindow.delete(s[left]);
      left++;                    // shrink window
    }
    inWindow.add(s[right]);      // expand window
    best = Math.max(best, right - left + 1);
  }
  return best;
}
// LeetCode: #3
```

---

### Combo 4: 🏗️ Stack + 🔍 Detective → "Valid Parentheses"

```
Why two patterns?
  🏗️ Stack: tracks the opening brackets
  🔍 Detective: any mismatch → return false immediately
```

```javascript
// (already shown in Pattern 9)
// LeetCode: #20
```

---

### Combo 5: 🪣 Bucket + 📊 Pointer Walker → "Container With Most Water"

```
Why two patterns?
  📊 Pointer Walker: left and right move inward
  🪣 Bucket: track the maximum area seen so far
```

```javascript
function maxArea(height) {
  let left = 0, right = height.length - 1;  // 📊 Pointers
  let best = 0;                               // 🪣 Bucket
  while (left < right) {
    const area = Math.min(height[left], height[right]) * (right - left);
    best = Math.max(best, area);
    if (height[left] < height[right]) left++;
    else right--;
  }
  return best;
}
// LeetCode: #11
```

---

### Quick Combo Reference

```
Problem Shape                   → Combo
────────────────────────────────────────────────
Remove items based on lookup    → 🔎 + 🏆 (Memory + Filter)
Find if something exists fast   → 🔎 + 🔍 (Memory + Detective)
Best contiguous + condition     → 🪟 + 🔎 (Window + Memory)
Matching/nesting + validation   → 🏗️ + 🔍 (Stack + Detective)
Sorted array optimization       → 📊 + 🪣 (Pointers + Bucket)
Tree path problems              → 🌳 + 🪣 (DFS + Bucket)
Grid shortest path              → 🌊 + 🔎 (BFS + Memory for visited)
Generate + filter valid ones    → 🔙 + 🔍 (Backtracking + Detective)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section E: Big-O Cheat Sheet

## Common Complexities Ranked

```
FASTEST → SLOWEST

O(1)         Constant      Instant. Hash lookup, array access by index.
O(log n)     Logarithmic   Binary search. Halves each step.
O(n)         Linear        One loop through the data. Best you can often get.
O(n log n)   Linearithmic  Sorting (merge sort, quick sort).
O(n²)        Quadratic     Nested loops. Usually too slow for n > 10,000.
O(2^n)       Exponential   Backtracking, subsets. Only for small n (< 25).
O(n!)        Factorial     Permutations. Only for tiny n (< 12).
```

```
PRACTICAL LIMITS (what n can be for each complexity):
  O(n)       → n up to 10,000,000  ✅
  O(n log n) → n up to 1,000,000   ✅
  O(n²)      → n up to 10,000      ⚠️
  O(2^n)     → n up to 25          ⚠️
  O(n!)      → n up to 12          🛑
```

---

## Every Pattern's Complexity

```
┌─────────────────────────┬────────────┬──────────┐
│ Pattern                 │ Time       │ Space    │
├─────────────────────────┼────────────┼──────────┤
│ 🔍 Detective             │ O(n)       │ O(1)     │
│ 🪣 Bucket                │ O(n)       │ O(1)     │
│ 🏆 Filter                │ O(n)       │ O(n)     │
│ 🔄 Transformer           │ O(n)       │ O(n)     │
│ 🔎 Memory (Set/Map)      │ O(n)       │ O(n)     │
│ 📊 Pointer Walker        │ O(n)       │ O(1)     │
│ 🪟 Sliding Window        │ O(n)       │ O(1)*    │
│ 🔪 Binary Search         │ O(log n)   │ O(1)     │
│ 🏗️ Stack                 │ O(n)       │ O(n)     │
│ 🌳 Tree DFS              │ O(n)       │ O(h)**   │
│ 🌊 BFS                   │ O(n)       │ O(w)***  │
│ 🧩 DP                    │ O(n) - O(n²)│ O(n)   │
│ 🔙 Backtracking          │ O(2^n)     │ O(n)     │
│ 🏃 Greedy                │ O(n log n) │ O(1)     │
└─────────────────────────┴────────────┴──────────┘

*  O(k) if window tracks a Set/Map
** h = height of tree (log n balanced, n worst case)
*** w = max width of tree
```

---

## Data Structure Operations

```
┌───────────────┬──────────┬──────────┬──────────┐
│ Operation     │ Array    │ Set      │ Map      │
├───────────────┼──────────┼──────────┼──────────┤
│ Access [i]    │ O(1)     │ N/A      │ N/A      │
│ Search/has    │ O(n)     │ O(1) ✅  │ O(1) ✅  │
│ Add/push      │ O(1)*    │ O(1)     │ O(1)     │
│ Delete        │ O(n)     │ O(1)     │ O(1)     │
│ Sort          │O(n log n)│ N/A      │ N/A      │
└───────────────┴──────────┴──────────┴──────────┘
* push to end is O(1), unshift to start is O(n)

THIS IS WHY PATTERN 5 (🔎 Memory) IS POWERFUL:
  Array.includes(x) → O(n)  = slow
  Set.has(x)        → O(1)  = instant
  Switching from array to set turns O(n²) into O(n).
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section F: How Patterns Connect

```
Every advanced pattern is built from Level 1:

🪟 Sliding Window    = 📊 Pointer Walker + 🪣 Bucket
🔪 Binary Search     = 🔍 Detective (elimination logic)
🏗️ Stack             = 🔍 Detective + LIFO structure
🌳 Tree DFS          = 🪣 Bucket + Recursion
🌊 BFS               = 🪣 Bucket + Queue
🧩 DP                = 🔎 Memory + 🪣 Bucket + Recursion
🔙 Backtracking      = 🌳 DFS + trial-and-error
🏃 Greedy            = 🪣 Bucket + sorting

→ Weak Level 1 = everything above it breaks.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section G: Key Insights

```
 #1: If you can't say it in English, you can't code it.
 #2: Brute force FIRST. Optimize SECOND.
 #3: The trace table never lies. Table works → code works.
 #4: Every problem is one of 22 patterns wearing a costume.
 #5: You learn patterns by solving 3-5 problems with each.
 #6: Code is the LAST step. Hard coding = skipped planning.
 #7: Data structure choice is half the solution.
     Array.includes = O(n). Set.has = O(1). That one swap changes everything.
 #8: "Replace linear scan with hash lookup" solves dozens of problems.
 #9: Working brute force > broken optimal.
#10: Patterns COMBINE. Most real problems use 2 patterns together.
#11: The #1 optimization move: nested loop → single loop + Set/Map.
#12: "Contiguous subarray" + "max/min" = Sliding Window. Every time.
#13: Binary search works on ANSWERS, not just sorted arrays.
#14: Every tree problem: base case → left → right → combine.
#15: DFS = "explore all paths." BFS = "shortest path / level by level."
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section H: 35-Day Practice Roadmap

## Week 1-2: Level 1 Patterns

| Day | Problem | Pattern | LeetCode |
|-----|---------|---------|----------|
| 1 | Sum of array | 🪣 Bucket | — |
| 2 | Is array sorted? | 🔍 Detective | #896 |
| 3 | Get all even numbers | 🏆 Filter | — |
| 4 | Double every number | 🔄 Transformer | — |
| 5 | Has duplicates? | 🔎 Memory | #217 |
| 6 | Is palindrome? | 📊 Pointer Walker | #125 |
| 7 | Remove duplicates | 🏆 + 🔎 Combo | #26 |
| 8 | Find maximum | 🪣 Bucket | — |
| 9 | All numbers positive? | 🔍 Detective | — |
| 10 | Remove falsy values | 🏆 Filter | — |
| 11 | Celsius → Fahrenheit | 🔄 Transformer | — |
| 12 | Two Sum | 🔎 Memory | #1 |
| 13 | Reverse array in-place | 📊 Pointer Walker | #344 |
| 14 | Character frequency | 🔎 Memory | #387 |

## Week 3-4: Level 2 Patterns

| Day | Problem | Pattern | LeetCode |
|-----|---------|---------|----------|
| 15 | Max sum of K consecutive | 🪟 Sliding Window | — |
| 16 | Longest substring no repeat | 🪟 Sliding Window | #3 |
| 17 | Binary search sorted array | 🔪 Binary Search | #704 |
| 18 | First occurrence in sorted | 🔪 Binary Search | #34 |
| 19 | Valid parentheses | 🏗️ Stack | #20 |
| 20 | Daily temperatures | 🏗️ Stack | #739 |
| 21 | Max depth binary tree | 🌳 Tree DFS | #104 |
| 22 | Invert binary tree | 🌳 Tree DFS | #226 |
| 23 | Level order traversal | 🌊 BFS | #102 |
| 24 | Number of islands | 🌊 BFS | #200 |
| 25 | Min subarray sum ≥ target | 🪟 Sliding Window | #209 |
| 26 | Rotated sorted search | 🔪 Binary Search | #33 |
| 27 | Evaluate RPN | 🏗️ Stack | #150 |
| 28 | Path sum tree | 🌳 Tree DFS | #112 |

## Week 5: Level 3 Patterns

| Day | Problem | Pattern | LeetCode |
|-----|---------|---------|----------|
| 29 | Climbing Stairs | 🧩 DP | #70 |
| 30 | House Robber | 🧩 DP | #198 |
| 31 | Coin Change | 🧩 DP | #322 |
| 32 | All subsets | 🔙 Backtracking | #78 |
| 33 | All permutations | 🔙 Backtracking | #46 |
| 34 | Jump Game | 🏃 Greedy | #55 |
| 35 | Buy/Sell Stock | 🏃 Greedy | #121 |

**Day 36+:** Random problems. Identify pattern yourself. Solve under 15 minutes.

[↑ Back to Table of Contents](#table-of-contents)

---

# Section I: Golden Rules

```
1. "If you can't explain it in English, you can't code it."
2. "Brute force FIRST. Optimize SECOND."
3. "The trace table never lies."
4. "Every problem is one of 22 patterns wearing a costume."
5. "60% thinking, 40% typing."
6. "Edge cases are where bugs hide."
7. "Name variables for humans, not compilers."
8. "15 minutes daily beats 5 hours weekly."
```

[↑ Back to Table of Contents](#table-of-contents)
