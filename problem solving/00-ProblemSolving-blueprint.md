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
- [Section C: Algorithms — The Procedures Inside Patterns](#section-c-algorithms)
  - [How Patterns, Algorithms, and Data Structures Relate](#how-they-relate)
  - [Sorting Algorithms](#sorting-algorithms)
  - [Searching Algorithms](#searching-algorithms)
  - [Traversal Algorithms](#traversal-algorithms)
  - [Classic Named Algorithms](#classic-named-algorithms)
- [Section D: All Patterns (4 Levels)](#section-d-all-patterns)
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
- [Section E: Pattern Combos — Real Problems Use 2+ Patterns](#section-e-pattern-combos)
- [Section F: Big-O Cheat Sheet](#section-f-big-o-cheat-sheet)
  - [Common Complexities Ranked](#common-complexities-ranked)
  - [Every Pattern's Complexity](#every-patterns-complexity)
  - [Data Structure Operations](#data-structure-operations)
- [Section G: How Patterns Connect](#section-g-how-patterns-connect)
- [Section H: Key Insights](#section-h-key-insights)
- [Section I: 35-Day Practice Roadmap](#section-i-35-day-practice-roadmap)
  - [Week 1-2: Level 1](#week-1-2-level-1-patterns)
  - [Week 3-4: Level 2](#week-3-4-level-2-patterns)
  - [Week 5: Level 3](#week-5-level-3-patterns)
- [Section J: Golden Rules](#section-j-golden-rules)
- [Section K: How to Use This Blueprint](#section-k-how-to-use-this-blueprint)

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

```
VISUAL:
  push(10) → push(20) → push(30)     pop() → pop()

  |  30  | ← top                      |  10  | ← top
  |  20  |                             └──────┘
  |  10  |
  └──────┘

  Last in (30) is first out.
```

```javascript
const stack = [];

stack.push(10);   // [10]
stack.push(20);   // [10, 20]
stack.push(30);   // [10, 20, 30]

const top = stack[stack.length - 1];  // peek: 30

stack.pop();      // returns 30
stack.pop();      // returns 20
stack.pop();      // returns 10

stack.length === 0;  // true (empty)

// REAL EXAMPLE: Reverse a string
function reverseString(str) {
  const stack = [];
  for (const ch of str) stack.push(ch);
  let reversed = "";
  while (stack.length > 0) reversed += stack.pop();
  return reversed;
}
reverseString("hello");  // "olleh"

// REAL EXAMPLE: Check matching brackets
function isBalanced(str) {
  const stack = [];
  const pairs = { ')': '(', ']': '[', '}': '{' };
  for (const ch of str) {
    if (ch === '(' || ch === '[' || ch === '{') stack.push(ch);
    else if (ch === ')' || ch === ']' || ch === '}') {
      if (stack.pop() !== pairs[ch]) return false;
    }
  }
  return stack.length === 0;
}
isBalanced("([{}])");  // true
isBalanced("([)]");    // false
```

```
KEY FACTS:
  ✅ push():   O(1)
  ✅ pop():    O(1)
  ✅ peek:     O(1)
  Think: "Last In, First Out"

COMMON STACK INTERVIEW PROBLEMS:
  - Valid Parentheses (LeetCode #20)
  - Daily Temperatures (#739)
  - Evaluate Reverse Polish Notation (#150)
  - Min Stack (#155)
  - Next Greater Element (#496)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 6. Queue

```
WHAT:    First-In, First-Out (FIFO). Like a line at a shop.
WHY:     BFS traversal, scheduling, level-order processing
HOW:     In JS, use array with push() and shift()
```

```javascript
const queue = [];

queue.push(10);    // [10]
queue.push(20);    // [10, 20]
queue.push(30);    // [10, 20, 30]

const front = queue[0];  // peek: 10

queue.shift();     // returns 10, queue is now [20, 30]
queue.shift();     // returns 20, queue is now [30]
queue.shift();     // returns 30, queue is now []

queue.length === 0;  // true
```

```
KEY FACTS:
  ✅ push():   O(1) — add to back
  ⚠️ shift():  O(n) — remove from front
  Think: "First In, First Out"

COMMON QUEUE INTERVIEW PROBLEMS:
  - BFS Level Order Traversal (LeetCode #102)
  - Number of Islands (#200)
  - Rotting Oranges (#994)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 7. Linked List

```
WHAT:    Chain of nodes. Each node has a value and a pointer to the next node.
WHY:     O(1) insert/delete at head.
```

```
VISUAL:
  head
   ↓
  [1] → [2] → [3] → null
```

```javascript
class ListNode {
  constructor(val) {
    this.val = val;
    this.next = null;
  }
}

const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);

// Traverse
function printList(head) {
  let current = head;
  while (current !== null) {
    console.log(current.val);
    current = current.next;
  }
}

// Reverse (very common interview question)
function reverseList(head) {
  let prev = null, current = head;
  while (current !== null) {
    const next = current.next;
    current.next = prev;
    prev = current;
    current = next;
  }
  return prev;
}

// Detect Cycle
function hasCycle(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow === fast) return true;
  }
  return false;
}
```

```
KEY FACTS:
  ✅ Insert/delete at head: O(1)
  ⚠️ Access by position: O(n)
  ⚠️ Search: O(n)

COMMON LINKED LIST PROBLEMS:
  - Reverse Linked List (#206)
  - Detect Cycle (#141)
  - Merge Two Sorted Lists (#21)
  - Find Middle (#876)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 8. Tree (Binary Tree)

```
WHAT:    Hierarchical structure. Each node has value, left child, right child.
         BST rule: left < parent < right
```

```
VISUAL:
         5
        / \
       3   8
      / \   \
     1   4   9
```

```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

// Inorder: Left → Root → Right (sorted in BST)
function inorder(node) {
  if (!node) return;
  inorder(node.left);
  console.log(node.val);
  inorder(node.right);
}

// Max Depth
function maxDepth(node) {
  if (!node) return 0;
  return 1 + Math.max(maxDepth(node.left), maxDepth(node.right));
}

// Level Order (BFS)
function levelOrder(root) {
  if (!root) return [];
  const queue = [root], result = [];
  while (queue.length > 0) {
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
KEY FACTS:
  ✅ BST search/insert: O(log n) average
  ✅ Every tree DFS skeleton:
     1. Base case: if null → return
     2. Recurse left
     3. Recurse right
     4. Combine results

COMMON TREE PROBLEMS:
  - Max Depth (#104), Invert Tree (#226)
  - Validate BST (#98), Level Order (#102)
  - Path Sum (#112), Lowest Common Ancestor (#236)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 9. Graph

```
WHAT:    Nodes connected by edges. Directed or undirected.
HOW:     Adjacency list (Map of node → neighbors)
```

```javascript
// Build from edge list
function buildGraph(n, edges) {
  const graph = Array.from({ length: n }, () => []);
  for (const [from, to] of edges) {
    graph[from].push(to);
    graph[to].push(from);
  }
  return graph;
}

// DFS
function dfsGraph(graph, start) {
  const visited = new Set();
  function dfs(node) {
    if (visited.has(node)) return;
    visited.add(node);
    for (const neighbor of graph[node]) dfs(neighbor);
  }
  dfs(start);
}

// BFS (shortest path)
function bfsGraph(graph, start) {
  const visited = new Set([start]);
  const queue = [start];
  while (queue.length > 0) {
    const node = queue.shift();
    for (const neighbor of graph[node]) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push(neighbor);
      }
    }
  }
}
```

```
KEY FACTS:
  ✅ DFS: O(V + E) — explore all paths, detect cycles
  ✅ BFS: O(V + E) — shortest path, level by level
  ⚠️ Always track visited to avoid infinite loops

COMMON GRAPH PROBLEMS:
  - Number of Islands (#200)
  - Course Schedule (#207)
  - Clone Graph (#133)
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
  (see Section D for all patterns)
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

# Section C: Algorithms

> **Patterns tell you WHAT shape the problem is.**
> **Algorithms tell you HOW to execute the solution.**
> **When you pick a pattern, the algorithm comes FREE inside the template.**

---

## How They Relate

```
┌─────────────────────────────────────────────────────────────┐
│  DATA STRUCTURES    = The TOOLS (pot, pan, oven)            │
│  ALGORITHMS         = The RECIPE (boil 10 min, add salt)    │
│  PATTERNS           = The COOKING STYLE (grilling, baking)  │
│  6-PHASE PROCESS    = The CHEF'S METHOD                     │
└─────────────────────────────────────────────────────────────┘

THE FLOW:
  Phase 2 (Identify Pattern) → Pattern gives Template → Template IS the Algorithm
```

```
WHICH ALGORITHM LIVES INSIDE WHICH PATTERN:

  🔍 Detective       → Linear Scan
  🪣 Bucket          → Running Total, Kadane's Algorithm
  🏆 Filter          → Linear Scan + Condition
  🔄 Transformer     → Map Operation
  🔎 Memory          → Hashing
  📊 Pointer Walker  → Two Pointer Technique, Floyd's Cycle Detection
  🪟 Sliding Window  → Sliding Window Technique
  🔪 Binary Search   → Binary Search Algorithm
  🏗️ Stack           → Stack-based Matching, Monotonic Stack
  🌳 Tree DFS        → Inorder / Preorder / Postorder Traversal
  🌊 BFS             → Breadth-First Search Algorithm
  🧩 DP              → Memoization, Tabulation, Kadane's, Knapsack, LCS, LIS
  🔙 Backtracking    → Recursive Backtracking
  🏃 Greedy          → Activity Selection, Greedy Choice
```

---

## Sorting Algorithms

> Sorting is NOT a pattern — it's a TOOL you use before applying a pattern.

```javascript
// Use this 99% of the time
[10, 2, 30].sort((a, b) => a - b); // [2, 10, 30] ascending
[10, 2, 30].sort((a, b) => b - a); // [30, 10, 2] descending
```

```
┌─────────────────┬───────────┬───────────┬──────────┬────────────┐
│ Algorithm       │ Best      │ Average   │ Worst    │ Space      │
├─────────────────┼───────────┼───────────┼──────────┼────────────┤
│ Bubble Sort     │ O(n)      │ O(n²)     │ O(n²)    │ O(1)       │
│ Selection Sort  │ O(n²)     │ O(n²)     │ O(n²)    │ O(1)       │
│ Insertion Sort  │ O(n)      │ O(n²)     │ O(n²)    │ O(1)       │
│ Merge Sort      │ O(n log n)│ O(n log n)│O(n log n)│ O(n)       │
│ Quick Sort      │ O(n log n)│ O(n log n)│ O(n²)    │ O(log n)   │
│ JS .sort()      │ O(n log n)│ O(n log n)│O(n log n)│ O(n)       │
└─────────────────┴───────────┴───────────┴──────────┴────────────┘
```

---

## Searching Algorithms

```
Linear Search  → O(n)     → works on anything
Binary Search  → O(log n) → requires sorted data
Hash Search    → O(1)     → requires Set/Map built first
```

---

## Classic Named Algorithms

### Kadane's Algorithm (Max Subarray Sum)

```javascript
function maxSubarraySum(arr) {
  let currentSum = arr[0], bestSum = arr[0];
  for (let i = 1; i < arr.length; i++) {
    currentSum = Math.max(arr[i], currentSum + arr[i]);
    bestSum = Math.max(bestSum, currentSum);
  }
  return bestSum;
}
// LeetCode: #53
```

### Floyd's Cycle Detection

```javascript
function hasCycle(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow === fast) return true;
  }
  return false;
}
// LeetCode: #141
```

### Dijkstra's Algorithm (Shortest Path — Weighted Graph)

```
WHAT: Shortest path from one node to all others in a weighted graph.
TIME: O((V + E) log V) with heap
LeetCode: #743, #1631
```

### Topological Sort (Task Ordering)

```
WHAT: Order tasks so every dependency comes first.
TIME: O(V + E)
LeetCode: #207, #210
```

### Union-Find (Connected Groups)

```
WHAT: Track which nodes belong to the same group.
TIME: Nearly O(1) per operation
LeetCode: #547, #684
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section D: All Patterns

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
```

```javascript
function detective(arr) {
  for (let i = 0; i < arr.length; i++) {
    if (/* BAD CONDITION */) return false;
  }
  return true;
}
```

**Example — "Is the array sorted?"**

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

```
Practice:
□ Is the array sorted?          □ Are all numbers positive?
□ Does string contain digits?   □ Is it a valid email?
LeetCode: #896 Monotonic Array, #1550 Three Consecutive Odds
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 2: 🪣 Bucket (Accumulation)

```
USE WHEN:  You need ONE value (sum, max, count, longest)
KEYWORDS:  "Sum of...", "Total...", "Count...", "Maximum...", "Longest..."
LOGIC:     Start with empty bucket. Pour each item in. Return bucket.
```

```javascript
function bucket(arr) {
  let result = 0;
  for (let i = 0; i < arr.length; i++) {
    result = /* combine result with arr[i] */;
  }
  return result;
}
```

```
Starting values:
  Sum/Count → 0    Maximum → -Infinity    Minimum → Infinity    Product → 1
```

**Example — "Find the maximum"**

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
Trace: [3, 7, 2] → biggest = 7 ✓
```

```
Practice:
□ Sum of array    □ Find max/min    □ Count evens    □ Longest string
LeetCode: #53 Max Subarray, #136 Single Number
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 3: 🏆 Filter (Predicate Selection)

```
USE WHEN:  You need a SHORTER list — only items passing a test
KEYWORDS:  "Get all...", "Find all...", "Remove...", "Keep only..."
LOGIC:     Empty winners list. Test each. Pass → push. Fail → skip.
```

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

```javascript
function getEvens(arr) {
  const winners = [];
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] % 2 === 0) winners.push(arr[i]);
  }
  return winners;
}
// [1,4,3,8] → [4,8] ✓
```

```
Practice:
□ Get all even numbers    □ Remove negatives    □ Remove falsy values
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 4: 🔄 Transformer (Data Projection)

```
USE WHEN:  You need SAME-SIZE list with CHANGED items
KEYWORDS:  "Double each...", "Convert...", "Format...", "Extract..."
LOGIC:     New list. Transform each item. Push transformed version.
```

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

```javascript
function squareAll(arr) {
  const result = [];
  for (let i = 0; i < arr.length; i++) {
    result.push(arr[i] * arr[i]);
  }
  return result;
}
// [2,3,5] → [4,9,25] ✓
```

```
Practice:
□ Double every number    □ Celsius → Fahrenheit    □ Strings to uppercase
LeetCode: #977 Squares of Sorted Array
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 5: 🔎 Memory (Lookup / Existence)

```
USE WHEN:  "Have I seen this before?" or "Find a pair/relationship"
KEYWORDS:  "Duplicates", "Two Sum", "Frequency", "First unique", "Anagram"
LOGIC:     Set/Map = instant memory. Check memory before processing.
```

```
Set → "have I seen it?"          → seen.has(x), seen.add(x)
Map → "seen it, what was stored?" → map.get(x), map.set(x, value)
```

```javascript
// Template (Set)
function memorySet(arr) {
  const seen = new Set();
  for (let i = 0; i < arr.length; i++) {
    if (seen.has(arr[i])) { /* FOUND */ }
    seen.add(arr[i]);
  }
}

// Template (Map — counting)
function memoryMap(arr) {
  const freq = new Map();
  for (let i = 0; i < arr.length; i++) {
    freq.set(arr[i], (freq.get(arr[i]) || 0) + 1);
  }
  return freq;
}
```

**Example — "Two Sum"**

```javascript
function twoSum(nums, target) {
  const memory = new Map();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (memory.has(complement)) return [memory.get(complement), i];
    memory.set(nums[i], i);
  }
  return [];
}
```

```
Practice:
□ Has duplicates?    □ Two Sum    □ Character frequency    □ Are anagrams?
LeetCode: #1 Two Sum, #217 Contains Duplicate, #242 Valid Anagram
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 6: 📊 Pointer Walker (Two Pointers)

```
USE WHEN:  Compare elements with each other, sorted array problems
KEYWORDS:  "Palindrome", "Sorted array pair", "In-place removal", "Merge"
LOGIC:     Two index variables moving through the data.
```

```javascript
// Template A — Opposite Ends
function oppositeEnds(arr) {
  let left = 0, right = arr.length - 1;
  while (left < right) {
    // compare arr[left] and arr[right]
    // move left++ or right--
  }
}

// Template B — Same Direction
function sameDirection(arr) {
  let write = 0;
  for (let read = 0; read < arr.length; read++) {
    if (/* keep this */) { arr[write] = arr[read]; write++; }
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
    left++; right--;
  }
  return true;
}
// "racecar" → true ✓
```

```
Practice:
□ Is palindrome?    □ Reverse array    □ Two Sum in sorted array
LeetCode: #125 Valid Palindrome, #167 Two Sum II, #283 Move Zeroes
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

---

### Pattern 7: 🪟 Sliding Window

```
USE WHEN:  Best/longest/shortest CONTIGUOUS subarray or substring
KEYWORDS:  "Consecutive", "Subarray of size K", "Longest substring"
LOGIC:     Window slides across data. Add new item, drop old item.
```

```javascript
// Fixed Window
function fixedWindow(arr, k) {
  let windowSum = 0;
  for (let i = 0; i < k; i++) windowSum += arr[i];
  let best = windowSum;
  for (let i = k; i < arr.length; i++) {
    windowSum += arr[i] - arr[i - k];
    best = Math.max(best, windowSum);
  }
  return best;
}

// Variable Window
function varWindow(arr) {
  let left = 0, best = 0;
  for (let right = 0; right < arr.length; right++) {
    // expand: add arr[right]
    while (/* window invalid */) { left++; }
    best = Math.max(best, right - left + 1);
  }
  return best;
}
```

```
Practice:
LeetCode: #3 Longest Substring, #209 Min Size Subarray, #438 Find Anagrams
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 8: 🔪 Binary Search

```
USE WHEN:  Find target in SORTED data by halving search space
KEYWORDS:  "Sorted array", "Find position", "First/last occurrence"
LOGIC:     Check middle. Too small → go right. Too big → go left.
```

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
Trace: [2,5,8,12,16] target=12
  mid=8 < 12 → lo=3 → mid=12 → FOUND ✓
```

```
Practice:
LeetCode: #704 Binary Search, #34 First/Last Position, #33 Rotated Search
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 9: 🏗️ Stack Pattern

```
USE WHEN:  Matching pairs, undo, Last-In-First-Out processing
KEYWORDS:  "Valid parentheses", "Next greater", "Evaluate expression"
LOGIC:     Push items. Pop when condition met.
```

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
Practice:
LeetCode: #20 Valid Parentheses, #739 Daily Temperatures, #150 Evaluate RPN
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 10: 🌳 Tree DFS

```
USE WHEN:  Explore a tree deeply using recursion
KEYWORDS:  "Max depth", "Path sum", "Invert tree", "Validate BST"
LOGIC:     Base case (null) → Ask left → Ask right → Combine answers
```

```javascript
function treeDFS(node) {
  if (!node) return /* base value */;
  const left = treeDFS(node.left);
  const right = treeDFS(node.right);
  return /* combine left and right */;
}

// Example: Max depth
function maxDepth(node) {
  if (!node) return 0;
  return 1 + Math.max(maxDepth(node.left), maxDepth(node.right));
}
```

```
Practice:
LeetCode: #104 Max Depth, #226 Invert Tree, #112 Path Sum, #98 Validate BST
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 11: 🌊 BFS

```
USE WHEN:  Level-by-level traversal or shortest path
KEYWORDS:  "Level order", "Shortest path", "Nearest", "Layer by layer"
LOGIC:     Queue. Process all nodes at current level. Add children for next.
```

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
Practice:
LeetCode: #102 Level Order, #200 Number of Islands, #994 Rotting Oranges
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 3 — Advanced (Solves 4% more)

---

### Pattern 12: 🧩 Dynamic Programming

```
USE WHEN:  Overlapping sub-problems + optimal substructure
KEYWORDS:  "How many ways?", "Minimum cost", "Can I reach?", "Longest..."
LOGIC:     Store answers to small problems. Build up to big answer.

3-Step Recipe:
  1. DEFINE: "What does dp[i] mean?"
  2. RELATE: "dp[i] = formula using dp[i-1], dp[i-2]..."
  3. BASE:   "dp[0] = ?, dp[1] = ?"
```

```javascript
// Example: Climbing Stairs
function climbStairs(n) {
  let prev2 = 1, prev1 = 1;
  for (let i = 2; i <= n; i++) {
    const curr = prev1 + prev2;
    prev2 = prev1;
    prev1 = curr;
  }
  return prev1;
}
// n=5 → 8 ✓
```

```
Practice:
LeetCode: #70 Climbing Stairs, #198 House Robber, #322 Coin Change, #300 LIS
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 13: 🔙 Backtracking

```
USE WHEN:  Generate ALL combinations, permutations, valid configurations
KEYWORDS:  "All subsets", "All permutations", "N-Queens", "Sudoku"
LOGIC:     CHOOSE → EXPLORE → UN-CHOOSE. Always this order.
```

```javascript
function subsets(nums) {
  const result = [];
  function bt(start, current) {
    result.push([...current]);
    for (let i = start; i < nums.length; i++) {
      current.push(nums[i]);   // CHOOSE
      bt(i + 1, current);      // EXPLORE
      current.pop();           // UN-CHOOSE
    }
  }
  bt(0, []);
  return result;
}
// [1,2,3] → [[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]]
```

```
Practice:
LeetCode: #78 Subsets, #46 Permutations, #39 Combination Sum, #79 Word Search
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 14: 🏃 Greedy

```
USE WHEN:  Best local choice leads to best global result
KEYWORDS:  "Maximum events", "Minimum stops", "Jump game"
LOGIC:     Sort (usually). Pick best option NOW. Never look back.
```

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
Practice:
LeetCode: #55 Jump Game, #121 Buy/Sell Stock, #435 Non-overlapping Intervals
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 4 — Specialist (Solves remaining 1%)

> Learn these as you encounter them.

| # | Pattern | Use When | LeetCode |
|---|---------|----------|----------|
| 15 | ⛰️ Heap | "Kth largest", "Top K frequent" | #215, #347 |
| 16 | 📚 Trie | "Autocomplete", "Starts with prefix" | #208, #211 |
| 17 | Prefix Sum | "Sum of range [i..j]" | #303, #560 |
| 18 | Merge Intervals | "Overlapping meetings" | #56, #57 |
| 19 | Union-Find | "Are nodes connected?" | #547, #684 |
| 20 | Topological Sort | "Course prerequisites" | #207, #210 |
| 21 | Bit Manipulation | "Single number (XOR)" | #136, #191 |
| 22 | Monotonic Stack | "Next greater element" | #496, #84 |

[↑ Back to Table of Contents](#table-of-contents)

---

# Section E: Pattern Combos

> Most real problems use 2 patterns together.

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

**Combo examples:**

```javascript
// Memory + Filter → Remove Duplicates
function removeDuplicates(arr) {
  const seen = new Set();
  const result = [];
  for (const item of arr) {
    if (!seen.has(item)) { seen.add(item); result.push(item); }
  }
  return result;
}

// Window + Memory → Longest Substring Without Repeat
function lengthOfLongestSubstring(s) {
  const inWindow = new Set();
  let left = 0, best = 0;
  for (let right = 0; right < s.length; right++) {
    while (inWindow.has(s[right])) { inWindow.delete(s[left]); left++; }
    inWindow.add(s[right]);
    best = Math.max(best, right - left + 1);
  }
  return best;
}
// LeetCode: #3
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section F: Big-O Cheat Sheet

## Common Complexities Ranked

```
FASTEST → SLOWEST

O(1)         Constant      Instant. Hash lookup, array access by index.
O(log n)     Logarithmic   Binary search. Halves each step.
O(n)         Linear        One loop. Best you can often get.
O(n log n)   Linearithmic  Sorting.
O(n²)        Quadratic     Nested loops. Too slow for n > 10,000.
O(2^n)       Exponential   Backtracking. Only for small n (< 25).
O(n!)        Factorial     Permutations. Only for tiny n (< 12).
```

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
│ 🪟 Sliding Window        │ O(n)       │ O(1)     │
│ 🔪 Binary Search         │ O(log n)   │ O(1)     │
│ 🏗️ Stack                 │ O(n)       │ O(n)     │
│ 🌳 Tree DFS              │ O(n)       │ O(h)     │
│ 🌊 BFS                   │ O(n)       │ O(w)     │
│ 🧩 DP                    │ O(n)–O(n²) │ O(n)     │
│ 🔙 Backtracking          │ O(2^n)     │ O(n)     │
│ 🏃 Greedy                │ O(n log n) │ O(1)     │
└─────────────────────────┴────────────┴──────────┘
```

## Data Structure Operations

```
┌───────────────┬──────────┬──────────┬──────────┐
│ Operation     │ Array    │ Set      │ Map      │
├───────────────┼──────────┼──────────┼──────────┤
│ Access [i]    │ O(1)     │ N/A      │ N/A      │
│ Search/has    │ O(n) ❌  │ O(1) ✅  │ O(1) ✅  │
│ Add/push      │ O(1)     │ O(1)     │ O(1)     │
│ Delete        │ O(n)     │ O(1)     │ O(1)     │
└───────────────┴──────────┴──────────┴──────────┘

Array.includes(x) → O(n) = SLOW
Set.has(x)        → O(1) = INSTANT
That one swap turns O(n²) into O(n).
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section G: How Patterns Connect

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

# Section H: Key Insights

```
 #1: If you can't say it in English, you can't code it.
 #2: Brute force FIRST. Optimize SECOND.
 #3: The trace table never lies. Table works → code works.
 #4: Every problem is one of 22 patterns wearing a costume.
 #5: You learn patterns by solving 3-5 problems with each.
 #6: Code is the LAST step. Hard coding = skipped planning.
 #7: Data structure choice is half the solution.
 #8: "Replace linear scan with hash lookup" solves dozens of problems.
 #9: Working brute force > broken optimal.
#10: Patterns COMBINE. Most real problems use 2 patterns together.
#11: The #1 optimization: nested loop → single loop + Set/Map.
#12: "Contiguous subarray" + "max/min" = Sliding Window. Every time.
#13: Binary search works on ANSWERS, not just sorted arrays.
#14: Every tree problem: base case → left → right → combine.
#15: DFS = "explore all paths." BFS = "shortest path / level by level."
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section I: 35-Day Practice Roadmap

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

# Section J: Golden Rules

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

---

# Section K: How to Use This Blueprint

> Read this before you read anything else.

## The blueprint is a MAP, not a reading assignment.

Most people open a document like this and try to read it from top to bottom.

**That is the wrong way.**

Reading everything first is overwhelming. Your brain cannot hold all of it at once. You will read for hours, remember almost nothing, and feel lost before you even write one line of code.

---

## The right way is this:

```
You open it DAILY.
You find today's day number.
You read ONLY that one pattern.
You solve ONLY that one problem.
You close it and stop.
```

That is it. Nothing more.

---

## Think of it like a GPS, not a textbook.

When you drive somewhere new, you do not read every road on the map before you leave. You just follow the next turn. The GPS says "turn left here." You turn left. That is all you need right now.

This blueprint works the same way:

```
Day 1  → "Read Detective pattern. Solve this one problem."
Day 2  → "Read Bucket pattern. Solve this one problem."
Day 3  → "Read Filter pattern. Solve this one problem."
```

You do not need to know what is on Day 20 when you are on Day 1. Just follow the next turn.

---

## Why reading everything first feels overwhelming:

```
❌ Reading all sections at once  = too much, nothing sticks
❌ Trying to learn all 14 patterns before solving = confusion
❌ Reading C, D, E, F, G, H all on Day 1 = brain overload

✅ Read ONE pattern  = clear
✅ Solve ONE problem = practice
✅ Repeat tomorrow   = progress
```

---

## The only sections you ever need to open:

```
TODAY (before Day 1)  → Section B only (6 Phases)

Day 1  → Section D, Pattern 1 only
Day 2  → Section D, Pattern 2 only
Day 3  → Section D, Pattern 3 only
...and so on, following Section I roadmap

When stuck on pattern identification → Section D Pattern Picker
When stuck on an algorithm          → Section C
When checking speed of your code    → Section F
When problem uses 2 patterns        → Section E
```

Every other section exists only for reference. Open it when you need it. Ignore it when you do not.

---

## One sentence to remember:

> **One day. One pattern. One problem. 25 minutes. Close it.**

[↑ Back to Table of Contents](#table-of-contents)
