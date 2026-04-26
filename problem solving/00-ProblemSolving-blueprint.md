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
  - [Translation Dictionary](#translation-dictionary)
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
- [Section G: How Patterns Connect](#section-g-how-patterns-connect)
- [Section H: Key Insights](#section-h-key-insights)
- [Section I: 38-Day Practice Roadmap](#section-i-38-day-practice-roadmap)
- [Section J: Golden Rules](#section-j-golden-rules)
- [Section K: How to Use This Blueprint](#section-k-how-to-use-this-blueprint)
- [Section L: Personal Notes](#section-l-personal-notes)

---

# Section A: Prerequisites

> **You MUST know these data structures before learning patterns.**
> Patterns are strategies. Data structures are the tools those strategies use.

```
WHICH ARE BUILT INTO JAVASCRIPT:
  ✅ Arrays  → [] — built in
  ✅ Strings → "" — built in
  ✅ Set     → new Set() — built in
  ✅ Map     → new Map() — built in

WHICH YOU MUST BUILD YOURSELF:
  ✗ Stack       → built using Array (push/pop)
  ✗ Queue       → built using Array (push/shift)
  ✗ Linked List → built using class + nodes
  ✗ Tree        → built using class + nodes
  ✗ Graph       → built using Array or Map

WHEN TO READ EACH:
  Arrays + Strings + Set + Map → read BEFORE Day 1
  Stack + Queue                → read on Day 18
  Tree                         → read on Day 20
  Graph                        → read on Day 22
  Linked List                  → read after Day 38
```

---

## 1. Arrays

```
WHAT:    Ordered list of items. Access any item by index.
SYNTAX:  const arr = [10, 20, 30]
ACCESS:  arr[0] → 10, arr[2] → 30
```

```javascript
const fruits = ["apple", "banana", "cherry"];

// Read
fruits[0];          // "apple"
fruits.length;      // 3

// Add
fruits.push("date");       // add to END
fruits.unshift("avocado"); // add to START

// Remove
fruits.pop();       // remove from END
fruits.shift();     // remove from START

// Search
fruits.indexOf("banana");  // 1 — O(n) slow!
fruits.includes("banana"); // true — O(n) slow!

// Loop
for (let i = 0; i < fruits.length; i++) { console.log(fruits[i]); }
for (const fruit of fruits) { console.log(fruit); }

// Other useful methods
fruits.slice(1, 3);           // copy section — does NOT modify original
fruits.splice(1, 1);          // removes 1 item at index 1 — MODIFIES original
[...fruits];                   // copy the array
fruits.sort((a, b) => a - b); // sort ascending — ⚠️ default sort is alphabetical!
```

```
KEY FACTS:
  ✅ Access by index:  O(1) — instant
  ✅ Push / Pop (end):  O(1) — instant
  ⚠️ Shift / Unshift (start): O(n) — slow
  ⚠️ indexOf / includes: O(n) — slow
  ⚠️ sort(): O(n log n)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 2. Strings

```
WHAT:    Sequence of characters. IMMUTABLE in JS.
SYNTAX:  const str = "hello"
ACCESS:  str[0] → "h", str.length → 5
```

```javascript
const str = "hello world";

str[0];             // "h"
str.length;         // 11
str.indexOf("world");  // 6
str.includes("hello"); // true
str.slice(0, 5);    // "hello"
str.split(" ");     // ["hello", "world"]
str.toUpperCase();  // "HELLO WORLD"
str.toLowerCase();  // "hello world"
str.trim();         // removes whitespace from both ends
str.replace("hello", "hi"); // "hi world"

for (const ch of str) { console.log(ch); }

[...str];                        // ["h","e","l","l","o"," ","w","o","r","l","d"]
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
         Instant lookup: O(1)
WHY:     Fast "have I seen this?" checks
```

```javascript
const seen = new Set();
const colors = new Set(["red", "blue", "green"]);

seen.add(5);
seen.add(10);
seen.add(5);    // ignored — 5 already exists
// seen = {5, 10}

seen.has(5);    // true  — O(1) instant!
seen.has(99);   // false — O(1) instant!
seen.delete(5);
seen.size;      // 1

for (const val of seen) { console.log(val); }

const unique = [...new Set([1, 2, 2, 3, 3, 3])]; // [1, 2, 3]
```

```
KEY FACTS:
  ✅ add():    O(1)
  ✅ has():    O(1) — THIS IS WHY SET EXISTS. Array.includes() is O(n).
  ✅ delete(): O(1)
  ⚠️ No duplicates — adding same value twice does nothing
  ⚠️ No index access — can't do set[0]
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 4. Map (Hash Map)

```
WHAT:    Key-value pairs. Instant lookup: O(1)
WHY:     Counting, storing indices, grouping
```

```javascript
const memory = new Map();

memory.set("apple", 3);
memory.set("banana", 5);
memory.set("apple", 7);   // overwrites — apple is now 7

memory.get("apple");   // 7
memory.get("cherry");  // undefined
memory.has("banana");  // true
memory.delete("banana");
memory.size;           // 1

for (const [key, value] of memory) { console.log(key, value); }

// Counting frequency
const freq = new Map();
for (const ch of "hello") {
  freq.set(ch, (freq.get(ch) || 0) + 1);
}
// freq = {"h"→1, "e"→1, "l"→2, "o"→1}
```

```
KEY FACTS:
  ✅ set(): O(1)   ✅ get(): O(1)   ✅ has(): O(1)
  ✅ Keys can be ANY type

SET vs MAP:
  "Have I seen this value?"      → Set
  "How many times did I see it?" → Map (key → count)
  "What index was this value at?"→ Map (key → index)
  "Group items by a category?"   → Map (key → array)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 5. Stack

```
WHAT:    Last-In, First-Out (LIFO). Like a stack of plates.
WHY:     Matching brackets, undo operations, expression evaluation
HOW:     In JS, use array with push() and pop()
READ:    Day 18 — one day before Pattern 9
```

```
VISUAL:
  push(10) → push(20) → push(30)     pop() → pop()

  |  30  | ← top                      |  10  | ← top
  |  20  |                             └──────┘
  |  10  |
  └──────┘
```

```javascript
const stack = [];

stack.push(10);   // [10]
stack.push(20);   // [10, 20]
stack.push(30);   // [10, 20, 30] ← 30 is on top

const top = stack[stack.length - 1];  // peek → 30

stack.pop();      // returns 30, stack is now [10, 20]
stack.pop();      // returns 20, stack is now [10]
stack.pop();      // returns 10, stack is now []
stack.length === 0; // true

// REAL EXAMPLE: Reverse a string
function reverseString(str) {
  const stack = [];
  for (const ch of str) { stack.push(ch); }
  let reversed = "";
  while (stack.length > 0) { reversed += stack.pop(); }
  return reversed;
}
reverseString("hello"); // "olleh"

// REAL EXAMPLE: Check matching brackets
function isBalanced(str) {
  const stack = [];
  const pairs = { ')': '(', ']': '[', '}': '{' };
  for (const ch of str) {
    if (ch === '(' || ch === '[' || ch === '{') {
      stack.push(ch);
    } else if (ch === ')' || ch === ']' || ch === '}') {
      if (stack.pop() !== pairs[ch]) return false;
    }
  }
  return stack.length === 0;
}
isBalanced("([{}])"); // true
isBalanced("([)]");   // false
```

```
KEY FACTS:
  ✅ push(): O(1)   ✅ pop(): O(1)   ✅ peek: O(1)
  Think: "Last In, First Out"

COMMON PROBLEMS:
  - Valid Parentheses (#20)
  - Daily Temperatures (#739)
  - Evaluate RPN (#150)
  - Min Stack (#155)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 6. Queue

```
WHAT:    First-In, First-Out (FIFO). Like a line at a shop.
WHY:     BFS traversal, scheduling, level-order processing
HOW:     In JS, use array with push() and shift()
READ:    Day 22 — one day before Pattern 11
```

```
VISUAL:
  FRONT → | 10 | 20 | 30 | ← BACK
  First in (10) is first out.
```

```javascript
const queue = [];

queue.push(10);   // [10]
queue.push(20);   // [10, 20]
queue.push(30);   // [10, 20, 30]

const front = queue[0]; // 10

queue.shift(); // returns 10, queue is now [20, 30]
queue.shift(); // returns 20, queue is now [30]
queue.shift(); // returns 30, queue is now []
queue.length === 0; // true

// REAL EXAMPLE: Process tasks in order
function processTasks(tasks) {
  const queue = [...tasks];
  const completed = [];
  while (queue.length > 0) {
    const task = queue.shift();
    completed.push(`Done: ${task}`);
  }
  return completed;
}
processTasks(["email", "code", "review"]);
// ["Done: email", "Done: code", "Done: review"]

// REAL EXAMPLE: BFS level by level
function printLevelByLevel(root) {
  if (!root) return;
  const queue = [root];
  while (queue.length > 0) {
    const node = queue.shift();
    console.log(node.val);
    if (node.left) queue.push(node.left);
    if (node.right) queue.push(node.right);
  }
}
```

```
KEY FACTS:
  ✅ push():  O(1) — add to back
  ⚠️ shift(): O(n) — remove from front
  Think: "First In, First Out"

COMMON PROBLEMS:
  - BFS Level Order (#102)
  - Number of Islands (#200)
  - Rotting Oranges (#994)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 7. Linked List

```
WHAT:    Chain of nodes. Each node has value and pointer to next.
WHY:     O(1) insert/delete at head.
READ:    After Day 38 — not in roadmap
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

// CREATE: 1 → 2 → 3
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);

// TRAVERSE
function printList(head) {
  let current = head;
  while (current !== null) {
    console.log(current.val);
    current = current.next;
  }
}

// SEARCH
function search(head, target) {
  let current = head;
  while (current !== null) {
    if (current.val === target) return true;
    current = current.next;
  }
  return false;
}

// INSERT AT HEAD O(1)
function insertAtHead(head, val) {
  const newNode = new ListNode(val);
  newNode.next = head;
  return newNode;
}

// INSERT AT TAIL O(n)
function insertAtTail(head, val) {
  const newNode = new ListNode(val);
  if (!head) return newNode;
  let current = head;
  while (current.next !== null) { current = current.next; }
  current.next = newNode;
  return head;
}

// DELETE BY VALUE
function deleteNode(head, target) {
  if (!head) return null;
  if (head.val === target) return head.next;
  let current = head;
  while (current.next !== null) {
    if (current.next.val === target) {
      current.next = current.next.next;
      return head;
    }
    current = current.next;
  }
  return head;
}

// REVERSE
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

// FIND MIDDLE
function findMiddle(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;
    fast = fast.next.next;
  }
  return slow;
}

// DETECT CYCLE
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
  ⚠️ Insert/delete at tail: O(n)
  ⚠️ Access by position: O(n)
  ⚠️ Search: O(n)

COMMON PROBLEMS:
  - Reverse Linked List (#206)
  - Detect Cycle (#141)
  - Merge Two Sorted Lists (#21)
  - Remove Nth Node From End (#19)
  - Find Middle (#876)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 8. Tree (Binary Tree)

```
WHAT:    Hierarchical structure. Each node has value, left, right.
         BST rule: left < parent < right
WHY:     Searching, sorting, hierarchical data
READ:    Day 20 — one day before Pattern 10
```

```
VISUAL:
         5          ← root
        / \
       3   8
      / \   \
     1   4   9      ← leaves
```

```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

const root = new TreeNode(5);
root.left = new TreeNode(3);
root.right = new TreeNode(8);
root.left.left = new TreeNode(1);
root.left.right = new TreeNode(4);
root.right.right = new TreeNode(9);

// Inorder: Left → Root → Right (sorted order in BST)
function inorder(node) {
  if (!node) return;
  inorder(node.left);
  console.log(node.val);
  inorder(node.right);
}
inorder(root); // 1, 3, 4, 5, 8, 9

// Preorder: Root → Left → Right
function preorder(node) {
  if (!node) return;
  console.log(node.val);
  preorder(node.left);
  preorder(node.right);
}

// Postorder: Left → Right → Root
function postorder(node) {
  if (!node) return;
  postorder(node.left);
  postorder(node.right);
  console.log(node.val);
}

// BFS Level Order
function levelOrder(root) {
  if (!root) return [];
  const queue = [root];
  const result = [];
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
levelOrder(root); // [[5], [3, 8], [1, 4, 9]]

// MAX DEPTH
function maxDepth(node) {
  if (!node) return 0;
  return 1 + Math.max(maxDepth(node.left), maxDepth(node.right));
}

// SEARCH IN BST
function searchBST(node, target) {
  if (!node) return null;
  if (target === node.val) return node;
  if (target < node.val) return searchBST(node.left, target);
  return searchBST(node.right, target);
}

// INSERT INTO BST
function insertBST(node, val) {
  if (!node) return new TreeNode(val);
  if (val < node.val) node.left = insertBST(node.left, val);
  else node.right = insertBST(node.right, val);
  return node;
}

// INVERT TREE
function invertTree(node) {
  if (!node) return null;
  const temp = node.left;
  node.left = invertTree(node.right);
  node.right = invertTree(temp);
  return node;
}

// VALIDATE BST
function isValidBST(node, min = -Infinity, max = Infinity) {
  if (!node) return true;
  if (node.val <= min || node.val >= max) return false;
  return isValidBST(node.left, min, node.val) &&
         isValidBST(node.right, node.val, max);
}
```

```
KEY FACTS:
  ✅ BST search/insert: O(log n) average
  ⚠️ BST worst case: O(n) — unbalanced tree
  ✅ Every tree DFS problem has the same skeleton:
     1. Base case: if null → return something
     2. Recurse left
     3. Recurse right
     4. Combine results

TRAVERSAL ORDERS:
  Inorder   (left, root, right) → sorted order in BST
  Preorder  (root, left, right) → copy tree
  Postorder (left, right, root) → delete tree
  Level order (BFS)             → level by level

COMMON PROBLEMS:
  - Max Depth (#104)
  - Invert Tree (#226)
  - Validate BST (#98)
  - Level Order (#102)
  - Path Sum (#112)
  - Same Tree (#100)
  - Lowest Common Ancestor (#236)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 9. Graph

```
WHAT:    Nodes connected by edges. Directed or undirected.
WHY:     Social networks, maps, dependencies, routing
HOW:     Adjacency list (Map or Array of Arrays)
READ:    Day 22 — one day before Pattern 11
```

```
VISUAL (Undirected):        VISUAL (Directed):
  0 --- 1                     0 → 1
  |     |                     ↓   ↓
  2 --- 3                     2 → 3
```

```javascript
// Method 1: Map
const graph = new Map();
graph.set(0, [1, 2]);
graph.set(1, [0, 3]);
graph.set(2, [0, 3]);
graph.set(3, [1, 2]);

// Method 2: Array of Arrays
const adjList = [
  [1, 2],  // node 0
  [0, 3],  // node 1
  [0, 3],  // node 2
  [1, 2],  // node 3
];

// Method 3: Build from edge list
function buildGraph(n, edges) {
  const graph = Array.from({ length: n }, () => []);
  for (const [from, to] of edges) {
    graph[from].push(to);
    graph[to].push(from); // remove for directed
  }
  return graph;
}

// DFS ON GRAPH
function dfsGraph(graph, start) {
  const visited = new Set();
  const result = [];
  function dfs(node) {
    if (visited.has(node)) return;
    visited.add(node);
    result.push(node);
    for (const neighbor of graph[node]) { dfs(neighbor); }
  }
  dfs(start);
  return result;
}

// BFS ON GRAPH
function bfsGraph(graph, start) {
  const visited = new Set([start]);
  const queue = [start];
  const result = [];
  while (queue.length > 0) {
    const node = queue.shift();
    result.push(node);
    for (const neighbor of graph[node]) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push(neighbor);
      }
    }
  }
  return result;
}

// COUNT CONNECTED COMPONENTS
function countComponents(n, graph) {
  const visited = new Set();
  let count = 0;
  function dfs(node) {
    visited.add(node);
    for (const neighbor of graph[node]) {
      if (!visited.has(neighbor)) dfs(neighbor);
    }
  }
  for (let i = 0; i < n; i++) {
    if (!visited.has(i)) { dfs(i); count++; }
  }
  return count;
}

// SHORTEST PATH (BFS unweighted)
function shortestPath(graph, start, end) {
  const visited = new Set([start]);
  const queue = [[start, 0]];
  while (queue.length > 0) {
    const [node, dist] = queue.shift();
    if (node === end) return dist;
    for (const neighbor of graph[node]) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push([neighbor, dist + 1]);
      }
    }
  }
  return -1;
}

// DETECT CYCLE
function hasCycle(n, graph) {
  const visited = new Set();
  function dfs(node, parent) {
    visited.add(node);
    for (const neighbor of graph[node]) {
      if (!visited.has(neighbor)) {
        if (dfs(neighbor, node)) return true;
      } else if (neighbor !== parent) {
        return true;
      }
    }
    return false;
  }
  for (let i = 0; i < n; i++) {
    if (!visited.has(i)) { if (dfs(i, -1)) return true; }
  }
  return false;
}

// NUMBER OF ISLANDS
function numIslands(grid) {
  let count = 0;
  const rows = grid.length, cols = grid[0].length;
  function sink(r, c) {
    if (r < 0 || r >= rows || c < 0 || c >= cols) return;
    if (grid[r][c] === '0') return;
    grid[r][c] = '0';
    sink(r+1,c); sink(r-1,c); sink(r,c+1); sink(r,c-1);
  }
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      if (grid[r][c] === '1') { count++; sink(r, c); }
    }
  }
  return count;
}
```

```
KEY FACTS:
  ✅ DFS: O(V + E) time, O(V) space
  ✅ BFS: O(V + E) time, O(V) space
  ✅ BFS finds shortest path in UNWEIGHTED graphs
  ⚠️ Always track visited nodes

  DFS → explore all paths, detect cycles, components
  BFS → shortest path, level-by-level, nearest neighbor

COMMON PROBLEMS:
  - Number of Islands (#200)
  - Clone Graph (#133)
  - Course Schedule (#207)
  - Connected Components (#323)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Data Structure Picker

```
"What do I need?"

  Ordered items, access by index       → Array
  Sequence of characters               → String
  Fast "have I seen this?" check       → Set
  Fast key → value lookup              → Map
  Last-in-first-out processing         → Stack (array push/pop)
  First-in-first-out processing        → Queue (array push/shift)
  O(1) insert/delete at head           → Linked List
  Hierarchical / sorted data           → Tree
  Connected relationships              → Graph
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section B: The 6 Phases

> Follow these 6 phases for every problem, every time.

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

POSITIVE FRAMING RULE:
  Write the condition for what you WANT not what you don't want.
  "I keep items that..."  not  "I remove items that..."

  Remove duplicates → think "keep if NOT seen before"
  Move zeros to back → think "keep if NOT zero"
  Validating form → think "what makes it valid"
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

Once you identify the pattern:
  → the pattern gives you a TEMPLATE
  → the algorithm lives INSIDE that template
  → you do not pick algorithm separately

THE FLOW:
  "What do I return?" → Pattern → Template → Algorithm comes free

EXAMPLE:
  Return one value   → 🪣 Bucket → template is a running variable
                                  → algorithm is Linear Scan
  Return true/false  → 🔍 Detective → template is loop + early return
                                     → algorithm is Linear Scan
  Sorted data search → 🔪 Binary Search → template is lo/mid/hi loop
                                         → algorithm is Binary Search

REMEMBER:
  You never ask "which algorithm do I use?"
  You only ask "what do I return?"
  The rest comes automatically.
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

> **This is the most important phase.**

```
Draw a trace table:
  - Columns = your variables
  - Rows = each step
  - Use a small input (3-4 items)
  - If table result = expected result → logic is correct
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
│  DATA STRUCTURES = The TOOLS (pot, pan, oven)               │
│  Array, Set, Map, Stack, Queue, Tree, Graph                 │
│                                                             │
│  ALGORITHMS = The RECIPE (boil 10 min, add salt)           │
│  Binary Search, BFS, DFS, Merge Sort, Kadane's             │
│                                                             │
│  PATTERNS = The COOKING STYLE (grilling, baking)           │
│  Detective, Bucket, Memory, Sliding Window, DP             │
│                                                             │
│  6-PHASE PROCESS = The CHEF'S METHOD                       │
│  Decompose → Identify → Pseudo-code → Dry Run → Code      │
└─────────────────────────────────────────────────────────────┘
```

```
PATTERN TO ALGORITHM MAP:

  Pattern 1  → 🔍 Detective      → Linear Search
  Pattern 2  → 🪣 Bucket         → Linear Search + Kadane's Algorithm
  Pattern 3  → 🏆 Filter         → Linear Search
  Pattern 4  → 🔄 Transformer    → Linear Search
  Pattern 5  → 🔎 Memory         → Hash-Based Search
  Pattern 6  → 📊 Pointer Walker → Two Pointer + Floyd's Cycle Detection
  Pattern 7  → 🪟 Sliding Window → Sliding Window Technique
  Pattern 8  → 🔪 Binary Search  → Binary Search Algorithm
  Pattern 9  → 🏗️ Stack          → Stack-based Matching
  Pattern 10 → 🌳 Tree DFS       → Tree Traversal DFS
  Pattern 11 → 🌊 BFS            → BFS Algorithm + Dijkstra's
  Pattern 12 → 🧩 DP             → Memoization + Tabulation
  Pattern 13 → 🔙 Backtracking   → Recursive Backtracking
  Pattern 14 → 🏃 Greedy         → Greedy Choice
```

---

## Sorting Algorithms

> Sorting is NOT a pattern. It is a TOOL you use before applying a pattern.

### Built-in Sort

```javascript
[10, 2, 30].sort();              // WRONG — alphabetical!
[10, 2, 30].sort((a, b) => a - b); // [2, 10, 30] ascending
[10, 2, 30].sort((a, b) => b - a); // [30, 10, 2] descending
["banana", "apple"].sort();         // ["apple", "banana"]
const people = [{name:"Bob",age:25},{name:"Ana",age:30}];
people.sort((a, b) => a.age - b.age); // sort by age
```

### Bubble Sort

```
WHAT: Swap adjacent elements if wrong order.
TIME: O(n²) — learning only
```

```javascript
function bubbleSort(arr) {
  const n = arr.length;
  for (let i = 0; i < n - 1; i++) {
    let swapped = false;
    for (let j = 0; j < n - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
        swapped = true;
      }
    }
    if (!swapped) break;
  }
  return arr;
}
```

```
Trace: [5, 3, 1]
  Pass 1: 5,3 → swap → [3,5,1] → 5,1 → swap → [3,1,5]
  Pass 2: 3,1 → swap → [1,3,5]
  Done: [1, 3, 5] ✓
```

### Selection Sort

```
WHAT: Find minimum in remaining, put at front.
TIME: O(n²)
```

```javascript
function selectionSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    let minIdx = i;
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[j] < arr[minIdx]) minIdx = j;
    }
    [arr[i], arr[minIdx]] = [arr[minIdx], arr[i]];
  }
  return arr;
}
```

### Insertion Sort

```
WHAT: Insert each element into its correct position.
TIME: O(n²) worst, O(n) best (already sorted)
```

```javascript
function insertionSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    const current = arr[i];
    let j = i - 1;
    while (j >= 0 && arr[j] > current) {
      arr[j + 1] = arr[j];
      j--;
    }
    arr[j + 1] = current;
  }
  return arr;
}
```

### Merge Sort

```
WHAT: Divide in half, sort each half, merge back.
TIME: O(n log n) always
SPACE: O(n)
```

```javascript
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  return merge(left, right);
}

function merge(left, right) {
  const result = [];
  let i = 0, j = 0;
  while (i < left.length && j < right.length) {
    if (left[i] <= right[j]) { result.push(left[i]); i++; }
    else { result.push(right[j]); j++; }
  }
  while (i < left.length) result.push(left[i++]);
  while (j < right.length) result.push(right[j++]);
  return result;
}
```

### Quick Sort

```
WHAT: Pick pivot, put smaller left, bigger right, recurse.
TIME: O(n log n) average, O(n²) worst
SPACE: O(log n)
```

```javascript
function quickSort(arr, lo = 0, hi = arr.length - 1) {
  if (lo >= hi) return arr;
  const pivotIdx = partition(arr, lo, hi);
  quickSort(arr, lo, pivotIdx - 1);
  quickSort(arr, pivotIdx + 1, hi);
  return arr;
}

function partition(arr, lo, hi) {
  const pivot = arr[hi];
  let i = lo;
  for (let j = lo; j < hi; j++) {
    if (arr[j] < pivot) {
      [arr[i], arr[j]] = [arr[j], arr[i]];
      i++;
    }
  }
  [arr[i], arr[hi]] = [arr[hi], arr[i]];
  return i;
}
```

```
SORTING COMPARISON:
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

WHEN TO USE WHAT:
  Interview says "sort this"        → .sort((a, b) => a - b)
  Interview says "implement a sort" → Merge Sort or Quick Sort
  Nearly sorted data                → Insertion Sort is O(n)
  Learning only                     → Bubble Sort
```

---

## Searching Algorithms

### Linear Search

```
WHAT: Check every element one by one.
TIME: O(n)
USED IN: 🔍 Detective, 🪣 Bucket, 🏆 Filter, 🔄 Transformer
```

```javascript
function linearSearch(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) return i;
  }
  return -1;
}
```

### Binary Search

```
WHAT: Cut search space in half. Requires SORTED data.
TIME: O(log n)
USED IN: 🔪 Binary Search pattern
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

### Hash-Based Search

```
WHAT: Store in Set/Map. Lookup is O(1).
TIME: O(1) per lookup, O(n) to build.
USED IN: 🔎 Memory pattern
```

```javascript
const set = new Set(arr);
set.has(target); // O(1) instant!
```

---

## Traversal Algorithms

### Array Traversal

```javascript
for (let i = 0; i < arr.length; i++) { /* forward */ }
for (let i = arr.length - 1; i >= 0; i--) { /* backward */ }
for (const item of arr) { /* no index needed */ }
```

### Tree DFS

```javascript
function inorder(node) {
  if (!node) return;
  inorder(node.left);
  console.log(node.val);
  inorder(node.right);
}

function preorder(node) {
  if (!node) return;
  console.log(node.val);
  preorder(node.left);
  preorder(node.right);
}

function postorder(node) {
  if (!node) return;
  postorder(node.left);
  postorder(node.right);
  console.log(node.val);
}
```

### BFS

```javascript
function bfs(root) {
  if (!root) return;
  const queue = [root];
  while (queue.length > 0) {
    const node = queue.shift();
    console.log(node.val);
    if (node.left) queue.push(node.left);
    if (node.right) queue.push(node.right);
  }
}
```

### Graph DFS and BFS

```javascript
function dfsGraph(graph, start) {
  const visited = new Set();
  function dfs(node) {
    if (visited.has(node)) return;
    visited.add(node);
    for (const neighbor of graph[node]) { dfs(neighbor); }
  }
  dfs(start);
}

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

---

## Classic Named Algorithms

### Kadane's Algorithm (Maximum Subarray Sum)

```
WHAT: Find the contiguous subarray with the largest sum.
LIVES IN: 🪣 Bucket pattern
TIME: O(n)
```

```javascript
function maxSubarraySum(arr) {
  let currentSum = arr[0];
  let bestSum = arr[0];
  for (let i = 1; i < arr.length; i++) {
    currentSum = Math.max(arr[i], currentSum + arr[i]);
    bestSum = Math.max(bestSum, currentSum);
  }
  return bestSum;
}
```

```
Trace: [-2, 1, -3, 4, -1, 2, 1, -5, 4]
  i=1: current=max(1,-1)=1,  best=1
  i=2: current=max(-3,-2)=-2, best=1
  i=3: current=max(4,2)=4,   best=4
  i=4: current=max(-1,3)=3,  best=4
  i=5: current=max(2,5)=5,   best=5
  i=6: current=max(1,6)=6,   best=6
  i=7: current=max(-5,1)=1,  best=6
  i=8: current=max(4,5)=5,   best=6
  Return: 6 ✓ — subarray [4,-1,2,1]
LeetCode: #53
```

### Floyd's Cycle Detection

```
WHAT: Detect cycle using slow + fast pointers.
LIVES IN: 📊 Pointer Walker
TIME: O(n), SPACE: O(1)
```

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
```

```
VISUAL:
  No cycle: 1→2→3→null → fast reaches null → false
  Cycle: 1→2→3→4→(back to 2) → slow and fast meet → true
LeetCode: #141, #142
```

### Dijkstra's Algorithm

```
WHAT: Shortest path from one node to all others in weighted graph.
LIVES IN: 🏃 Greedy pattern
TIME: O((V + E) log V) with heap
```

```javascript
function dijkstra(graph, start) {
  const dist = new Map();
  const visited = new Set();
  for (const node of graph.keys()) { dist.set(node, Infinity); }
  dist.set(start, 0);

  while (visited.size < graph.size) {
    let current = null, minDist = Infinity;
    for (const [node, d] of dist) {
      if (!visited.has(node) && d < minDist) {
        current = node; minDist = d;
      }
    }
    if (current === null) break;
    visited.add(current);

    for (const { neighbor, weight } of graph.get(current)) {
      if (!visited.has(neighbor)) {
        const newDist = dist.get(current) + weight;
        if (newDist < dist.get(neighbor)) {
          dist.set(neighbor, newDist);
        }
      }
    }
  }
  return dist;
}
```

```
VISUAL:
       4
  A ------→ B
  |          |
 1|         1|
  ↓    2     ↓
  C ------→ D
  Shortest: A→C(1)→B(2)=3, then B→D(1)=4
LeetCode: #743, #1631
```

### Topological Sort

```
WHAT: Order tasks so every dependency comes before the task.
LIVES IN: Level 4 specialist
TIME: O(V + E)
```

```javascript
function topologicalSort(numNodes, edges) {
  const graph = Array.from({ length: numNodes }, () => []);
  const inDegree = Array(numNodes).fill(0);

  for (const [from, to] of edges) {
    graph[from].push(to);
    inDegree[to]++;
  }

  const queue = [];
  for (let i = 0; i < numNodes; i++) {
    if (inDegree[i] === 0) queue.push(i);
  }

  const order = [];
  while (queue.length > 0) {
    const node = queue.shift();
    order.push(node);
    for (const neighbor of graph[node]) {
      inDegree[neighbor]--;
      if (inDegree[neighbor] === 0) queue.push(neighbor);
    }
  }
  return order.length === numNodes ? order : [];
}
```

```
LeetCode: #207, #210
```

### Union-Find

```
WHAT: Track which elements belong to the same group.
LIVES IN: Level 4 specialist
TIME: Nearly O(1) per operation
```

```javascript
class UnionFind {
  constructor(n) {
    this.parent = Array.from({ length: n }, (_, i) => i);
    this.rank = Array(n).fill(0);
  }

  find(x) {
    if (this.parent[x] !== x) {
      this.parent[x] = this.find(this.parent[x]);
    }
    return this.parent[x];
  }

  union(x, y) {
    const rootX = this.find(x);
    const rootY = this.find(y);
    if (rootX === rootY) return false;
    if (this.rank[rootX] < this.rank[rootY]) {
      this.parent[rootX] = rootY;
    } else if (this.rank[rootX] > this.rank[rootY]) {
      this.parent[rootY] = rootX;
    } else {
      this.parent[rootY] = rootX;
      this.rank[rootX]++;
    }
    return true;
  }

  connected(x, y) { return this.find(x) === this.find(y); }
}
```

```
LeetCode: #547, #684, #323
```

### Complete Algorithm Map

```
┌──────────────────────┬────────────────────────────┬──────────────┐
│ Algorithm            │ What It Does               │ Lives In     │
├──────────────────────┼────────────────────────────┼──────────────┤
│ Linear Search        │ Check every element         │ 🔍🪣🏆🔄     │
│ Binary Search        │ Halve sorted search space   │ 🔪           │
│ Hash Lookup          │ O(1) lookup via Set/Map     │ 🔎           │
│ Two Pointer          │ Compare from both ends      │ 📊           │
│ Sliding Window       │ Track contiguous window     │ 🪟           │
│ Bubble Sort          │ Swap adjacent (slow)        │ Sorting      │
│ Selection Sort       │ Find min, place it          │ Sorting      │
│ Insertion Sort       │ Insert into sorted part     │ Sorting      │
│ Merge Sort           │ Divide, sort, merge         │ Sorting      │
│ Quick Sort           │ Pivot + partition           │ Sorting      │
│ DFS (tree)           │ Go deep, then backtrack     │ 🌳           │
│ BFS (tree/graph)     │ Go level by level           │ 🌊           │
│ DFS (graph)          │ Explore all paths           │ 🌳           │
│ Kadane's             │ Max subarray sum            │ 🪣           │
│ Floyd's Cycle        │ Detect cycle (fast/slow)    │ 📊           │
│ Dijkstra's           │ Shortest path (weighted)    │ 🏃 + Heap   │
│ Topological Sort     │ Task order with deps        │ 🌊 + Graph  │
│ Union-Find           │ Group connectivity          │ Specialist   │
│ Memoization          │ Cache recursive results     │ 🧩 DP       │
│ Tabulation           │ Bottom-up DP table          │ 🧩 DP       │
│ Backtracking         │ Choose-explore-unchoose     │ 🔙           │
│ Greedy Choice        │ Best local = best global    │ 🏃           │
└──────────────────────┴────────────────────────────┴──────────────┘
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section D: All Patterns

```
Level 1 (6 patterns)  → Solves 80%  → Days 1-14
Level 2 (5 patterns)  → Solves 15%  → Days 15-28
Level 3 (3 patterns)  → Solves 4%   → Days 29-38
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
ALGORITHM: → Section C → Linear Search

NOTE: Detective is the ONE exception to the positive framing rule.
      It deliberately writes the BAD condition.
      Find ONE bad thing → stop immediately. This is intentional.

DON'T USE WHEN:
  ✗ You need to COUNT bad things (use 🪣 Bucket)
  ✗ You need to COLLECT bad items (use 🏆 Filter)
  ✗ The answer isn't true/false
  ✗ Checking if ONE value exists once (use includes())
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
□ Is the array sorted?              □ Are all numbers positive?
□ Does string contain only digits?  □ Is it a valid email?
LeetCode: #1550 Three Consecutive Odds, #896 Monotonic Array
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 2: 🪣 Bucket (Accumulation)

```
USE WHEN:  You need ONE value from a list (sum, max, count, longest)
KEYWORDS:  "Sum of...", "Total...", "Count...", "Maximum...", "Longest..."
LOGIC:     Start with empty bucket. Pour each item in. Return bucket.
ALGORITHM: → Section C → Linear Search + Kadane's Algorithm

  Normal Bucket  → Linear Scan  → touches ALL items
  Kadane's Bucket → Kadane's    → finds BEST subarray

DON'T USE WHEN:
  ✗ You need a list of results (use 🏆 Filter or 🔄 Transformer)
  ✗ You need true/false (use 🔍 Detective)
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
  Sum/Count → 0     Maximum → -Infinity    Minimum → Infinity    Product → 1
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
Trace: [3, 7, 2]
  i=0: 3 > -Inf → biggest=3
  i=1: 7 > 3    → biggest=7
  i=2: 2 > 7? NO
  Return: 7 ✓
```

**Kadane's Example — "Maximum Subarray Sum"**

```javascript
function maxSubarraySum(arr) {
  let currentSum = arr[0];
  let bestSum = arr[0];
  for (let i = 1; i < arr.length; i++) {
    currentSum = Math.max(arr[i], currentSum + arr[i]);
    bestSum = Math.max(bestSum, currentSum);
  }
  return bestSum;
}
// [-2,1,-3,4,-1,2,1,-5,4] → 6 ✓
```

```
Practice:
□ Sum of array    □ Find average    □ Find max/min
□ Count evens     □ Longest string  □ Product of all
LeetCode: #53 Max Subarray, #136 Single Number
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 3: 🏆 Filter (Predicate Selection)

```
USE WHEN:  You need a SHORTER list — only items passing a test
KEYWORDS:  "Get all...", "Find all...", "Remove...", "Keep only..."
LOGIC:     Empty winners list. Test each. Pass → push. Fail → skip.
ALGORITHM: → Section C → Linear Search

DON'T USE WHEN:
  ✗ You need EVERY item changed (use 🔄 Transformer)
  ✗ You need one value (use 🪣 Bucket)
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
```

```
Trace: [1, 4, 3, 8]
  1: odd → skip   winners=[]
  4: even → push  winners=[4]
  3: odd → skip   winners=[4]
  8: even → push  winners=[4,8]
  Return: [4, 8] ✓
```

```
Practice:
□ Get all even numbers              □ Strings longer than 5 chars
□ Remove all negative numbers       □ Remove all falsy values
□ Get words starting with vowel
LeetCode: used in #217, #349
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 4: 🔄 Transformer (Data Projection)

```
USE WHEN:  You need SAME-SIZE list with CHANGED items
KEYWORDS:  "Double each...", "Convert...", "Format...", "Extract..."
LOGIC:     New list. Transform each item. Push transformed version.
ALGORITHM: → Section C → Linear Search

DON'T USE WHEN:
  ✗ Some items should be removed (use 🏆 Filter)
  ✗ You need one value (use 🪣 Bucket)
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
□ Double every number
□ Square every number
□ Celsius → Fahrenheit (C × 9/5 + 32)
□ All strings to uppercase
□ Get length of each string
LeetCode: #977 Squares of Sorted Array
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 5: 🔎 Memory (Lookup / Existence)

```
USE WHEN:
  CASE 1 → "Have I seen this before?"
            Basic    → yes or no      → SET
            Extended → how many times → MAP (store count)

  CASE 2 → "Find a pair/relationship"
            Two values that connect   → MAP (store index)

KEYWORDS:  "Duplicates", "Two Sum", "Frequency", "First unique", "Anagram"
ALGORITHM: → Section C → Hash-Based Search
LOGIC:     Set/Map = instant memory. Check memory before processing.

DON'T USE WHEN:
  ✗ Data is already sorted (use 📊 Pointer Walker)
  ✗ Just filtering by simple condition (use 🏆 Filter)
  ✗ Checking if ONE value exists once (use 🔍 Detective)
```

```
SET → stores VALUE only
      one question → "have I seen it?" → yes or no
      methods      → seen.has(x), seen.add(x)

MAP → stores VALUE + EXTRA INFO
      two questions → "have I seen it?" + "what did I store?"
      methods       → map.has(x), map.get(x), map.set(x, value)

HOW TO IDENTIFY WHICH CASE:
  "Does something exist or repeat?"   → Case 1 Basic    → SET
  "How many times does it appear?"    → Case 1 Extended → MAP count
  "Find two values that relate?"      → Case 2          → MAP index
```

```javascript
// Template (Set) — Case 1 Basic
function memorySet(arr) {
  const seen = new Set();
  for (let i = 0; i < arr.length; i++) {
    if (seen.has(arr[i])) { /* FOUND */ }
    seen.add(arr[i]); // check FIRST → add AFTER
  }
}

// Template (Map — count) — Case 1 Extended
function memoryMapCount(arr) {
  const freq = new Map();
  for (let i = 0; i < arr.length; i++) {
    freq.set(arr[i], (freq.get(arr[i]) || 0) + 1);
  }
  return freq;
}

// Template (Map — index) — Case 2
function memoryMapIndex(arr, target) {
  const memory = new Map();
  for (let i = 0; i < arr.length; i++) {
    const complement = target - arr[i];
    if (memory.has(complement)) return [memory.get(complement), i];
    memory.set(arr[i], i);
  }
}
```

**Example — Remove Duplicates (Case 1 Basic)**

```javascript
function removeDuplicates(arr) {
  const seen = new Set();
  const result = [];
  for (const item of arr) {
    if (!seen.has(item)) { seen.add(item); result.push(item); }
  }
  return result;
}
// [1,3,2,3] → [1,3,2] ✓
```

**Example — Character Frequency (Case 1 Extended)**

```javascript
function charFrequency(str) {
  const freq = new Map();
  for (const ch of str) {
    freq.set(ch, (freq.get(ch) || 0) + 1);
  }
  return freq;
}
// "hello" → {h→1, e→1, l→2, o→1}
```

**Example — Two Sum (Case 2)**

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
// [2,7,11] target=9 → [0,1] ✓
```

```
All Problems:

CASE 1 BASIC → SET
  □ Does array have duplicates?
  □ Are all values unique?
  □ Find first repeated character
  □ Remove duplicates from array
  □ Intersection of two arrays
  □ Union of two arrays
  □ Is this a subset?

CASE 1 EXTENDED → MAP count
  □ Character frequency of a string
  □ Element frequency of an array
  □ Are two strings anagrams?
  □ First unique character in string
  □ Most frequent element
  □ Find elements that appear exactly twice
  □ Group anagrams together
  □ Ransom note

CASE 2 → MAP index
  □ Two Sum
  □ Difference of K
  □ Subarray sum equals K
  □ Longest consecutive sequence
  □ Four Sum

LeetCode: #217, #242, #387, #1
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 6: 📊 Pointer Walker (Two Pointers)

```
USE WHEN:  Compare elements with each other, sorted array problems
KEYWORDS:  "Palindrome", "Sorted array pair", "In-place removal", "Merge"
LOGIC:     Two index variables moving through the data.
ALGORITHM: → Section C → Two Pointer + Floyd's Cycle Detection

DON'T USE WHEN:
  ✗ Data is unsorted and you need pair lookup (use 🔎 Memory)
  ✗ You're not comparing elements with EACH OTHER
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

// Template B — Same Direction (read/write)
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
Trace: "racecar"
  left=0(r) right=6(r) → match
  left=1(a) right=5(a) → match
  left=2(c) right=4(c) → match
  left=3 ≥ right=3 → stop → true ✓
```

```
Practice:
□ Is string a palindrome?           □ Reverse array in-place
□ Two Sum in SORTED array           □ Remove duplicates from sorted
□ Move all zeroes to end            □ Merge two sorted arrays
LeetCode: #125, #26, #167, #283, #88
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

> **Prerequisite:** Level 1 patterns feel easy. Pick them in under 30 seconds.

---

### Pattern 7: 🪟 Sliding Window

```
USE WHEN:  Best/longest/shortest CONTIGUOUS subarray or substring
KEYWORDS:  "Consecutive", "Subarray of size K", "Longest substring"
LOGIC:     Window slides across data. Add new item, drop old item.
ALGORITHM: → Section C → Sliding Window Technique
BUILT ON:  📊 Pointer Walker + 🪣 Bucket

DON'T USE WHEN:
  ✗ Elements don't need to be contiguous
  ✗ You need a pair, not a range
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
Trace: [1,4,2,7,1] k=3
  First window [1,4,2] = 7
  Slide: +7-1 = 13 → best=13
  Slide: +1-4 = 10
  Return: 13 ✓
```

```
Practice:
□ Max sum subarray of size K
□ Longest substring without repeating
□ Minimum size subarray with sum ≥ target
LeetCode: #3, #209, #438, #567
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 8: 🔪 Binary Search

```
USE WHEN:  Find target in SORTED data by halving search space
KEYWORDS:  "Sorted array", "Find position", "First/last occurrence"
LOGIC:     Check middle. Too small → go right. Too big → go left.
ALGORITHM: → Section C → Binary Search Algorithm
BUILT ON:  🔍 Detective (elimination)

DON'T USE WHEN:
  ✗ Data is NOT sorted and can't be sorted
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
  lo=0 hi=4 mid=2 → arr[2]=8 < 12 → lo=3
  lo=3 hi=4 mid=3 → arr[3]=12 → FOUND ✓
```

```
Practice:
LeetCode: #704, #34, #33, #69
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 9: 🏗️ Stack Pattern

```
USE WHEN:  Matching pairs, undo, Last-In-First-Out processing
KEYWORDS:  "Valid parentheses", "Next greater", "Evaluate expression"
LOGIC:     Push items. Pop when condition met. Check what's on top.
ALGORITHM: → Section C → Stack-based Matching

DON'T USE WHEN:
  ✗ Processing order doesn't matter
  ✗ You need FIFO → use Queue / BFS
```

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
  ( → push  stack:[(]
  [ → push  stack:[(,[]
  ] → pop [ → match ✓
  ) → pop ( → match ✓
  Empty → true ✓
```

```
Practice:
LeetCode: #20, #150, #739, #155
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 10: 🌳 Tree DFS

```
USE WHEN:  Explore a tree deeply using recursion
KEYWORDS:  "Max depth", "Path sum", "Invert tree", "Validate BST"
LOGIC:     Base case (null) → Ask left → Ask right → Combine answers
ALGORITHM: → Section C → Tree Traversal DFS

DON'T USE WHEN:
  ✗ You need level-by-level processing (use 🌊 BFS)
  ✗ You need shortest path (use 🌊 BFS)
```

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
        2   3   → maxDepth = 1 + max(1,1) = 2 ✓
```

```
Practice:
LeetCode: #104, #226, #112, #98, #100, #572
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 11: 🌊 BFS

```
USE WHEN:  Level-by-level traversal or shortest path
KEYWORDS:  "Level order", "Shortest path", "Nearest", "Layer by layer"
LOGIC:     Queue. Process all at current level. Add children for next.
ALGORITHM: → Section C → BFS Algorithm + Dijkstra's

DON'T USE WHEN:
  ✗ You need to explore all paths to leaves (use 🌳 DFS)
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
LeetCode: #102, #200, #994, #542
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
ALGORITHM: → Section C → Memoization + Tabulation
BUILT ON:  🪣 Bucket + 🔎 Memory + Recursion

DON'T USE WHEN:
  ✗ Sub-problems don't overlap (use Divide & Conquer)
  ✗ Greedy local choice is provably optimal (use 🏃 Greedy)

3-Step Recipe:
  1. DEFINE: "What does dp[i] mean?"
  2. RELATE: "dp[i] = formula using dp[i-1], dp[i-2]..."
  3. BASE:   "dp[0] = ?, dp[1] = ?"
```

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

```
Practice:
LeetCode: #70, #198, #322, #300, #1143, #416
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 13: 🔙 Backtracking

```
USE WHEN:  Generate ALL combinations, permutations, valid configurations
KEYWORDS:  "All subsets", "All permutations", "N-Queens", "Sudoku"
LOGIC:     CHOOSE → EXPLORE → UN-CHOOSE. Always this order.
ALGORITHM: → Section C → Recursive Backtracking
BUILT ON:  🌳 DFS + trial-and-error
```

```javascript
function backtrack(result, current, choices, start) {
  result.push([...current]);
  for (let i = start; i < choices.length; i++) {
    current.push(choices[i]);                    // CHOOSE
    backtrack(result, current, choices, i + 1);  // EXPLORE
    current.pop();                               // UN-CHOOSE
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

```
Practice:
LeetCode: #78, #46, #39, #79, #51, #17
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 14: 🏃 Greedy

```
USE WHEN:  Best local choice leads to best global result
KEYWORDS:  "Maximum events", "Minimum stops", "Jump game"
LOGIC:     Sort (usually). Pick best option NOW. Never look back.
ALGORITHM: → Section C → Greedy Choice

DON'T USE WHEN:
  ✗ Local best ≠ global best (use 🧩 DP instead)
  Tip: If unsure, default to DP. It is always correct.
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
Trace: [2, 3, 1, 1, 4]
  i=0: maxReach=2
  i=1: maxReach=4
  i=2: maxReach=4
  i=3: maxReach=4
  i=4: reached end → true ✓
```

```
Practice:
LeetCode: #55, #121, #435, #455
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 4 — Specialist (Solves remaining 1%)

> Learn these as you encounter them.

| # | Pattern | Use When | LeetCode |
|---|---------|----------|----------|
| 15 | ⛰️ Heap | "Kth largest", "Top K frequent" | #215, #347, #295 |
| 16 | 📚 Trie | "Autocomplete", "Starts with prefix" | #208, #211, #212 |
| 17 | Prefix Sum | "Sum of range [i..j]" | #303, #560, #238 |
| 18 | Merge Intervals | "Overlapping meetings" | #56, #57, #252 |
| 19 | Union-Find | "Are nodes connected?" | #547, #684, #323 |
| 20 | Topological Sort | "Course prerequisites" | #207, #210, #269 |
| 21 | Bit Manipulation | "Single number (XOR)" | #136, #191, #338 |
| 22 | Monotonic Stack | "Next greater element" | #496, #503, #84 |

[↑ Back to Table of Contents](#table-of-contents)

---

# Section E: Pattern Combos

> Most real problems use 2 patterns together.

### Combo 1: 🔎 Memory + 🏆 Filter → "Remove Duplicates"

```javascript
function removeDuplicates(arr) {
  const seen = new Set();       // 🔎 Memory
  const result = [];            // 🏆 Filter
  for (const item of arr) {
    if (!seen.has(item)) {
      seen.add(item);
      result.push(item);
    }
  }
  return result;
}
// LeetCode: #26, #217
```

---

### Combo 2: 🔎 Memory + 🔍 Detective → "Contains Duplicate"

```javascript
function containsDuplicate(arr) {
  const seen = new Set();       // 🔎 Memory
  for (const item of arr) {
    if (seen.has(item)) return true;  // 🔍 Detective: early exit
    seen.add(item);
  }
  return false;
}
// LeetCode: #217
```

---

### Combo 3: 🪟 Sliding Window + 🔎 Memory → "Longest Substring Without Repeat"

```javascript
function lengthOfLongestSubstring(s) {
  const inWindow = new Set();   // 🔎 Memory
  let left = 0, best = 0;      // 🪟 Window
  for (let right = 0; right < s.length; right++) {
    while (inWindow.has(s[right])) {
      inWindow.delete(s[left]);
      left++;
    }
    inWindow.add(s[right]);
    best = Math.max(best, right - left + 1);
  }
  return best;
}
// LeetCode: #3
```

---

### Combo 4: 🏗️ Stack + 🔍 Detective → "Valid Parentheses"

```javascript
function isValid(s) {
  const stack = [];             // 🏗️ Stack
  const pairs = { ')': '(', ']': '[', '}': '{' };
  for (const ch of s) {
    if ('([{'.includes(ch)) stack.push(ch);
    else if (stack.pop() !== pairs[ch]) return false; // 🔍 Detective
  }
  return stack.length === 0;
}
// LeetCode: #20
```

---

### Combo 5: 🪣 Bucket + 📊 Pointer Walker → "Container With Most Water"

```javascript
function maxArea(height) {
  let left = 0, right = height.length - 1;  // 📊 Pointers
  let best = 0;                              // 🪣 Bucket
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
Remove items based on lookup    → 🔎 + 🏆
Find if something exists fast   → 🔎 + 🔍
Best contiguous + condition     → 🪟 + 🔎
Matching/nesting + validation   → 🏗️ + 🔍
Sorted array optimization       → 📊 + 🪣
Tree path problems              → 🌳 + 🪣
Grid shortest path              → 🌊 + 🔎
Generate + filter valid ones    → 🔙 + 🔍
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section F: Big-O Cheat Sheet

## Common Complexities Ranked

```
FASTEST → SLOWEST

O(1)         Constant      Instant. Hash lookup, array access.
O(log n)     Logarithmic   Binary search. Halves each step.
O(n)         Linear        One loop. Best you can often get.
O(n log n)   Linearithmic  Sorting.
O(n²)        Quadratic     Nested loops. Too slow for n > 10,000.
O(2^n)       Exponential   Backtracking. Only for n < 25.
O(n!)        Factorial     Permutations. Only for n < 12.
```

```
PRACTICAL LIMITS:
  O(n)       → n up to 10,000,000  ✅
  O(n log n) → n up to 1,000,000   ✅
  O(n²)      → n up to 10,000      ⚠️
  O(2^n)     → n up to 25          ⚠️
  O(n!)      → n up to 12          🛑
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
│ 🪟 Sliding Window        │ O(n)       │ O(1)*    │
│ 🔪 Binary Search         │ O(log n)   │ O(1)     │
│ 🏗️ Stack                 │ O(n)       │ O(n)     │
│ 🌳 Tree DFS              │ O(n)       │ O(h)**   │
│ 🌊 BFS                   │ O(n)       │ O(w)***  │
│ 🧩 DP                    │ O(n)–O(n²) │ O(n)     │
│ 🔙 Backtracking          │ O(2^n)     │ O(n)     │
│ 🏃 Greedy                │ O(n log n) │ O(1)     │
└─────────────────────────┴────────────┴──────────┘
*   O(k) if window tracks Set/Map
**  h = height of tree
*** w = max width of tree
```

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
* push end O(1), unshift start O(n)

Array.includes(x) → O(n) = slow
Set.has(x)        → O(1) = instant
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
     Array.includes = O(n). Set.has = O(1). That swap changes everything.
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

# Section I: 38-Day Practice Roadmap

## How to Use the Roadmap

```
EACH DAY HAS THIS STRUCTURE:

  PREPARE A → read this from Section A
  PREPARE C → read this from Section C

  SOLVE     → problem → pattern → LeetCode
  TIME      → time complexity
  SPACE     → space complexity

  Apply Section B — 6 Phases:
    PHASE 1 → DECOMPOSE
    PHASE 2 → IDENTIFY pattern + algorithm
    PHASE 3 → PSEUDO-CODE
    PHASE 4 → TRACE TABLE
    PHASE 5 → CODE
    PHASE 6 → EDGE CASES

TOTAL TIME: 25 minutes per day maximum.

COMPLEX PATTERNS SPLIT ACROSS MULTIPLE DAYS:
  Memory pattern      → 3 days (Days 5-7)
  Sliding Window      → 2 days (Days 15-16)
  Stack               → prep Day 18, solve Days 19-20
  Tree DFS            → prep Day 20, solve Days 21-22
  BFS                 → prep Day 22, solve Days 23-24
  DP                  → 3 days (Days 29-31)
  Backtracking        → 2 days (Days 32-33)
  Greedy              → 2 days (Days 34-35)
```

---

## Week 1-2: Level 1 Patterns

| Day | Prepare A | Prepare C | Solve | Pattern | Time | Space | LeetCode |
|-----|-----------|-----------|-------|---------|------|-------|----------|
| 1 | Arrays | Linear Search | Sum of array | 🪣 Bucket | O(n) | O(1) | — |
| 2 | Arrays | Linear Search | Is array sorted? | 🔍 Detective | O(n) | O(1) | #896 |
| 3 | Arrays | Linear Search | Get all even numbers | 🏆 Filter | O(n) | O(n) | — |
| 4 | Arrays | Linear Search | Double every number | 🔄 Transformer | O(n) | O(n) | — |
| 5 | Set + Map | Hash-Based Search | Has duplicates? | 🔎 Memory Case 1 | O(n) | O(n) | #217 |
| 6 | Map | Hash-Based Search | Character frequency | 🔎 Memory Case 1 Ext | O(n) | O(n) | #387 |
| 7 | Map | Hash-Based Search | Two Sum | 🔎 Memory Case 2 | O(n) | O(n) | #1 |
| 8 | Strings | Two Pointer | Is palindrome? | 📊 Pointer Walker | O(n) | O(1) | #125 |
| 9 | Arrays | Linear Search | Find maximum | 🪣 Bucket | O(n) | O(1) | — |
| 10 | Arrays | Linear Search | All numbers positive? | 🔍 Detective | O(n) | O(1) | — |
| 11 | Arrays | Linear Search | Remove falsy values | 🏆 Filter | O(n) | O(n) | — |
| 12 | Arrays | Linear Search | Celsius → Fahrenheit | 🔄 Transformer | O(n) | O(n) | — |
| 13 | Arrays | Two Pointer | Reverse array in-place | 📊 Pointer Walker | O(n) | O(1) | #344 |
| 14 | Arrays + Set | Hash-Based Search | Remove duplicates | 🏆 + 🔎 Combo | O(n) | O(n) | #26 |

---

## Week 3-4: Level 2 Patterns

| Day | Prepare A | Prepare C | Solve | Pattern | Time | Space | LeetCode |
|-----|-----------|-----------|-------|---------|------|-------|----------|
| 15 | Arrays | Sliding Window | Max sum of K consecutive | 🪟 Sliding Window | O(n) | O(1) | — |
| 16 | Strings + Set | Sliding Window | Longest substring no repeat | 🪟 + 🔎 Combo | O(n) | O(k) | #3 |
| 17 | Arrays | Binary Search | Binary search sorted array | 🔪 Binary Search | O(log n) | O(1) | #704 |
| 18 | **READ Stack + Queue** | Stack-based Matching | — PREP DAY — | — | — | — | — |
| 19 | Stack | Stack-based Matching | Valid parentheses | 🏗️ Stack | O(n) | O(n) | #20 |
| 20 | **READ Tree** | Tree Traversal DFS | Daily temperatures | 🏗️ Stack | O(n) | O(n) | #739 |
| 21 | Tree | Tree Traversal DFS | Max depth binary tree | 🌳 Tree DFS | O(n) | O(h) | #104 |
| 22 | **READ Graph** | BFS Algorithm | Invert binary tree | 🌳 Tree DFS | O(n) | O(h) | #226 |
| 23 | Graph + Queue | BFS Algorithm | Level order traversal | 🌊 BFS | O(n) | O(w) | #102 |
| 24 | Graph | BFS Algorithm | Number of islands | 🌊 BFS | O(m×n) | O(m×n) | #200 |
| 25 | Arrays | Binary Search | First occurrence in sorted | 🔪 Binary Search | O(log n) | O(1) | #34 |
| 26 | Arrays | Sliding Window | Min subarray sum ≥ target | 🪟 Sliding Window | O(n) | O(1) | #209 |
| 27 | Arrays | Binary Search | Rotated sorted search | 🔪 Binary Search | O(log n) | O(1) | #33 |
| 28 | Tree | Tree Traversal DFS | Path sum tree | 🌳 Tree DFS | O(n) | O(h) | #112 |

---

## Week 5: Level 3 Patterns

| Day | Prepare A | Prepare C | Solve | Pattern | Time | Space | LeetCode |
|-----|-----------|-----------|-------|---------|------|-------|----------|
| 29 | Arrays | Memoization + Tabulation | Climbing Stairs | 🧩 DP | O(n) | O(1) | #70 |
| 30 | Arrays | Memoization + Tabulation | House Robber | 🧩 DP | O(n) | O(1) | #198 |
| 31 | Arrays | Memoization + Tabulation | Coin Change | 🧩 DP | O(n×amt) | O(amt) | #322 |
| 32 | Arrays | Recursive Backtracking | All subsets | 🔙 Backtracking | O(2^n) | O(n) | #78 |
| 33 | Arrays | Recursive Backtracking | All permutations | 🔙 Backtracking | O(n!) | O(n) | #46 |
| 34 | Arrays | Greedy Choice | Jump Game | 🏃 Greedy | O(n) | O(1) | #55 |
| 35 | Arrays | Greedy Choice | Buy/Sell Stock | 🏃 Greedy | O(n) | O(1) | #121 |
| 36 | Stack | Stack-based Matching | Evaluate RPN | 🏗️ Stack | O(n) | O(n) | #150 |
| 37 | Arrays | Recursive Backtracking | Combination Sum | 🔙 Backtracking | O(2^n) | O(n) | #39 |
| 38 | Arrays | Greedy Choice | Non-overlapping Intervals | 🏃 Greedy | O(n log n) | O(1) | #435 |

**Day 39+:** Random problems. Identify pattern yourself. Solve under 15 minutes.

---

## Day Structure — Full Example (Day 1)

```
PREPARE A → Section A: Arrays
PREPARE C → Section C: Linear Search

SOLVE  → Sum of array → 🪣 Bucket → no LeetCode
TIME   → O(n)
SPACE  → O(1)

PHASE 1 → DECOMPOSE
  Input     → array of numbers, can be empty
  Output    → one number (the sum)
  Rules     → add all numbers together
  Goal      → "I take an array and return the sum of all numbers"
  Edge cases → [], [5], [-1,2,-3], [0,0,0]

PHASE 2 → IDENTIFY
  Return one value → 🪣 Bucket
  Algorithm        → Linear Scan (Section C → Linear Search)

PHASE 3 → PSEUDO-CODE
  STORE result = 0
  REPEAT for each number
    ADD number to result
  GIVE BACK result

PHASE 4 → TRACE TABLE
  Input: [3, 1, 4]
  | Step  | i | arr[i] | result |
  | Start | — | —      | 0      |
  | i=0   | 0 | 3      | 3      |
  | i=1   | 1 | 1      | 4      |
  | i=2   | 2 | 4      | 8      |
  | End   | — | —      | 8 ✓    |

PHASE 5 → CODE
  function sumArray(arr) {
    if (arr.length === 0) return 0;     // edge case
    let result = 0;                      // STORE result = 0
    for (let i = 0; i < arr.length; i++) { // REPEAT
      result += arr[i];                  // ADD to result
    }
    return result;                       // GIVE BACK
  }

PHASE 6 → EDGE CASES
  sumArray([])          → 0
  sumArray([5])         → 5
  sumArray([-1, 2, -3]) → -2
  sumArray([0, 0, 0])   → 0
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section J: Golden Rules

```
1.  "If you can't explain it in English, you can't code it."
2.  "Brute force FIRST. Optimize SECOND."
3.  "The trace table never lies."
4.  "Every problem is one of 22 patterns wearing a costume."
5.  "60% thinking, 40% typing."
6.  "Edge cases are where bugs hide."
7.  "Name variables for humans, not compilers."
8.  "15 minutes daily beats 5 hours weekly."
9.  "Write the KEEP condition, not the SKIP condition.
     Define what belongs — everything else takes care of itself."
10. "Learn the data structure ONE DAY before you need it.
     Not all upfront. Use it immediately or you forget it."
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section K: How to Use This Blueprint

> Read this before you read anything else.

## The blueprint is a MAP, not a reading assignment.

```
You open it DAILY.
You find today's day number in Section I.
You read ONLY that one day.
You solve ONLY that one problem.
You close it and stop.
```

---

## Think of it like a GPS, not a textbook.

```
Day 1  → PREPARE A + C → SOLVE with 6 phases → DONE
Day 2  → PREPARE A + C → SOLVE with 6 phases → DONE
...
```

---

## The only sections you ever need to open:

```
BEFORE DAY 1   → Section B only (read 6 Phases once)

EVERY DAY      → Section I → find today's day
                 → follow PREPARE A, PREPARE C, SOLVE

WHEN READING A PATTERN:
  → check ALGORITHM line inside the pattern
  → open that exact algorithm in Section C
  → read it BEFORE solving

WHEN STUCK on pattern identification → Section D Pattern Picker
WHEN CHECKING complexity             → Section F
WHEN PROBLEM uses 2 patterns         → Section E
WHEN YOU FEEL LOST                   → Section H Key Insights
WHEN NEED A DATA STRUCTURE REMINDER  → Section A
```

---

## Complex patterns take more than one day

```
This is normal. Not a failure.

Memory pattern  → 3 days   Sliding Window → 2 days
Stack           → 2 days   Tree DFS       → 2 days
BFS             → 2 days   DP             → 3 days
Backtracking    → 2 days   Greedy         → 2 days

One problem per day. Not one pattern per day.
25 minutes. Done. Close it.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section L: Personal Notes — Discoveries While Studying

---

## Note 1: The Positive Framing Rule (Discovered Day 1)

Write the condition for what you WANT, not what you don't want.

```
Remove duplicates
  Wrong: "if it IS a duplicate → skip"
  Right: "if it is NOT seen before → keep"

Move zeros to back
  Wrong: "if it IS zero → don't place"
  Right: "if it is NOT zero → place it"

Applies everywhere:
  Validating a form  → think "what makes it valid"
  Filtering users    → think "who should I keep"
  Tree traversal     → think "when should I go deeper"

EXCEPTION: Detective pattern deliberately uses the BAD condition.
  That is intentional — find ONE bad thing → stop immediately.
```

---

## Note 2: Memory Pattern Takes Multiple Days (Discovered Day 5)

```
Detective   → one template → one idea → one day
Filter      → one template → one idea → one day
Transformer → one template → one idea → one day

Memory      → two tools (Set and Map)
            → two cases
            → three templates
            → three days. That is completely normal.

Do not rush it. Close the blueprint. One problem per day.
```

---

## Note 3: Pattern to Algorithm Connection (Discovered Before Day 1)

```
Every pattern has an algorithm inside it.
You do not pick them separately.

Pattern 2 → 🪣 Bucket → Linear Scan + Kadane's lives inside
Pattern 5 → 🔎 Memory → Hash-Based Search lives inside
Pattern 8 → 🔪 Binary Search → Binary Search Algorithm lives inside

When you identify the pattern:
  → check the ALGORITHM line inside Section D
  → open Section C → read that algorithm
  → then solve the problem
```

---

## Note 4: Data Structures Are Not All Built Into JavaScript

```
JavaScript has BUILT IN:
  Arrays, Strings, Set, Map

You must BUILD yourself:
  Stack → array with push/pop
  Queue → array with push/shift
  Tree  → class with val, left, right
  Graph → array of arrays or Map

Learn each ONE DAY before you need it in the roadmap.
Not all at once before Day 1.
```

---

## One sentence to remember:

> **One day. One pattern. One problem. 25 minutes. Close it.**

[↑ Back to Table of Contents](#table-of-contents)
