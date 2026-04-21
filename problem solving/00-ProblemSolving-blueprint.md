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
- [Section L: Personal Notes — Discoveries While Studying](#section-l-personal-notes--discoveries-while-studying)
  - [Note 1: The Positive Framing Rule](#note-1-the-positive-framing-rule-discovered-day-1)

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
// === CREATING A STACK ===
const stack = [];

// === PUSH — Add to top ===
stack.push(10);   // [10]
stack.push(20);   // [10, 20]
stack.push(30);   // [10, 20, 30]    ← 30 is on top

// === PEEK — Look at top WITHOUT removing ===
const top = stack[stack.length - 1];  // 30 (still in stack)

// === POP — Remove from top ===
stack.pop();      // returns 30, stack is now [10, 20]
stack.pop();      // returns 20, stack is now [10]
stack.pop();      // returns 10, stack is now []

// === CHECK IF EMPTY ===
stack.length === 0;  // true (empty now)

// === REAL EXAMPLE: Reverse a string using a stack ===
function reverseString(str) {
  const stack = [];
  for (const ch of str) { stack.push(ch); }
  let reversed = "";
  while (stack.length > 0) { reversed += stack.pop(); }
  return reversed;
}
reverseString("hello");  // "olleh"

// === REAL EXAMPLE: Check matching brackets ===
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
isBalanced("([{}])");  // true
isBalanced("([)]");    // false
```

```
KEY FACTS:
  ✅ push():   O(1) — instant
  ✅ pop():    O(1) — instant
  ✅ peek:     O(1) — instant
  Think: "Last In, First Out" — like browser back button, undo, or call stack

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
         The first person in line is the first one served.
WHY:     BFS traversal, scheduling, level-order processing
HOW:     In JS, use array with push() and shift()
```

```
VISUAL:
  enqueue(10) → enqueue(20) → enqueue(30)     dequeue()

  FRONT → | 10 | 20 | 30 | ← BACK            FRONT → | 20 | 30 | ← BACK
                                                (10 removed from front)
  First in (10) is first out.
```

```javascript
const queue = [];

queue.push(10);    // [10]
queue.push(20);    // [10, 20]
queue.push(30);    // [10, 20, 30]

const front = queue[0];  // 10 (still in queue)

queue.shift();     // returns 10, queue is now [20, 30]
queue.shift();     // returns 20, queue is now [30]
queue.shift();     // returns 30, queue is now []

queue.length === 0;  // true

// === REAL EXAMPLE: Process tasks in order ===
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

// === REAL EXAMPLE: BFS on a tree (level by level) ===
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
  ✅ push():   O(1) — add to back
  ⚠️ shift():  O(n) — remove from front (moves all remaining elements)
  Think: "First In, First Out" — like a queue at a ticket counter

  ⚠️ shift() is O(n) because it re-indexes the entire array.
     For most interview problems, this is fine.
     For production code with millions of items, use a linked list queue.

COMMON QUEUE INTERVIEW PROBLEMS:
  - BFS Level Order Traversal (LeetCode #102)
  - Number of Islands (#200)
  - Rotting Oranges (#994)
  - Shortest Path in Grid
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 7. Linked List

```
WHAT:    Chain of nodes. Each node has a value and a pointer to the next node.
         No index access — must walk from the head.
WHY:     O(1) insert/delete at head. Foundation for stacks, queues, and more.
```

```
VISUAL:
  head
   ↓
  [1] → [2] → [3] → null

  Each box (node) has:
    - val:  the data (1, 2, 3)
    - next: pointer to the next node (or null if last)
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

// INSERT AT HEAD (O(1))
function insertAtHead(head, val) {
  const newNode = new ListNode(val);
  newNode.next = head;
  return newNode;
}

// INSERT AT TAIL (O(n))
function insertAtTail(head, val) {
  const newNode = new ListNode(val);
  if (!head) return newNode;
  let current = head;
  while (current.next !== null) { current = current.next; }
  current.next = newNode;
  return head;
}

// DELETE A NODE BY VALUE
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

// REVERSE A LINKED LIST
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
// Before: [1] → [2] → [3] → null
// After:  [3] → [2] → [1] → null

// FIND MIDDLE NODE
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
  ✅ Insert/delete at head: O(1) — instant
  ⚠️ Insert/delete at tail: O(n) — must walk the chain
  ⚠️ Access by position: O(n) — no random access
  ⚠️ Search: O(n) — must walk through

COMMON LINKED LIST INTERVIEW PROBLEMS:
  - Reverse Linked List (LeetCode #206)
  - Detect Cycle (#141)
  - Merge Two Sorted Lists (#21)
  - Remove Nth Node From End (#19)
  - Find Middle (#876)
  - Palindrome Linked List (#234)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 8. Tree (Binary Tree)

```
WHAT:    Hierarchical structure. Each node has a value, left child, right child.
         Binary Search Tree (BST): left < parent < right
WHY:     Searching, sorting, hierarchical data (file systems, DOM, etc.)
```

```
VISUAL:
         5          ← root
        / \
       3   8        ← children of 5
      / \   \
     1   4   9      ← leaves (no children)

  BST rule: everything left of 5 is < 5, everything right is > 5
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

// Inorder: Left → Root → Right (gives SORTED order in BST)
function inorder(node) {
  if (!node) return;
  inorder(node.left);
  console.log(node.val);
  inorder(node.right);
}
inorder(root);  // 1, 3, 4, 5, 8, 9 (sorted!)

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

// BFS — Level Order
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
levelOrder(root);  // [[5], [3, 8], [1, 4, 9]]

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
  ✅ BST search/insert: O(log n) average — halves each step
  ⚠️ BST worst case: O(n) — if tree is a straight line (unbalanced)
  ✅ Traversal orders:
     Inorder (left, root, right)   → sorted order in BST
     Preorder (root, left, right)  → copy tree
     Postorder (left, right, root) → delete tree
     Level order (BFS)             → level by level
  ✅ Every tree DFS problem has the same skeleton:
     1. Base case: if null → return something
     2. Recurse left
     3. Recurse right
     4. Combine results

COMMON TREE INTERVIEW PROBLEMS:
  - Max Depth (LeetCode #104)
  - Invert Tree (#226)
  - Validate BST (#98)
  - Level Order Traversal (#102)
  - Path Sum (#112)
  - Same Tree (#100)
  - Lowest Common Ancestor (#236)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 9. Graph

```
WHAT:    Nodes (vertices) connected by edges. Can be directed or undirected.
WHY:     Social networks, maps, dependencies, routing, shortest paths
HOW:     Usually represented as adjacency list (Map of node → neighbors)
```

```
VISUAL (Undirected):
  0 --- 1
  |     |
  2 --- 3

  Node 0 connects to: [1, 2]
  Node 1 connects to: [0, 3]
  Node 2 connects to: [0, 3]
  Node 3 connects to: [1, 2]

VISUAL (Directed):
  0 → 1
  ↓   ↓
  2 → 3

  Node 0 points to: [1, 2]
  Node 1 points to: [3]
  Node 2 points to: [3]
  Node 3 points to: []
```

```javascript
// === CREATE GRAPH — Adjacency List ===

// Method 1: Using Map
const graph = new Map();
graph.set(0, [1, 2]);
graph.set(1, [0, 3]);
graph.set(2, [0, 3]);
graph.set(3, [1, 2]);

// Method 2: Using Array of Arrays (when nodes are 0..n-1)
const adjList = [
  [1, 2],     // node 0's neighbors
  [0, 3],     // node 1's neighbors
  [0, 3],     // node 2's neighbors
  [1, 2],     // node 3's neighbors
];

// Method 3: Build from edge list
function buildGraph(n, edges) {
  const graph = Array.from({ length: n }, () => []);
  for (const [from, to] of edges) {
    graph[from].push(to);
    graph[to].push(from);  // remove this line for directed graph
  }
  return graph;
}
const g = buildGraph(4, [[0,1], [0,2], [1,3], [2,3]]);
// g = [[1,2], [0,3], [0,3], [1,2]]

// === DFS ON GRAPH (visit all connected nodes) ===
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
dfsGraph(adjList, 0);  // [0, 1, 3, 2]

// === BFS ON GRAPH (level by level, shortest path) ===
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
bfsGraph(adjList, 0);  // [0, 1, 2, 3]

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

// SHORTEST PATH (BFS — unweighted)
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

// DETECT CYCLE IN UNDIRECTED GRAPH
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
    sink(r + 1, c); sink(r - 1, c);
    sink(r, c + 1); sink(r, c - 1);
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
  ⚠️ Always track visited nodes to avoid infinite loops

  DFS vs BFS on graphs:
    DFS → explore all paths, detect cycles, connected components
    BFS → shortest path, level-by-level, nearest neighbor

COMMON GRAPH INTERVIEW PROBLEMS:
  - Number of Islands (LeetCode #200)
  - Clone Graph (#133)
  - Course Schedule (#207)
  - Pacific Atlantic Water Flow (#417)
  - Connected Components (#323)
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
│                                                             │
│  DATA STRUCTURES    = The TOOLS (pot, pan, oven)           │
│  Array, Set, Map, Stack, Queue, Tree, Graph                │
│                                                             │
│  ALGORITHMS         = The RECIPE (boil 10 min, add salt)   │
│  Binary Search, BFS, DFS, Merge Sort, Kadane's            │
│                                                             │
│  PATTERNS           = The COOKING STYLE (grilling, baking) │
│  Detective, Bucket, Memory, Sliding Window, DP             │
│                                                             │
│  6-PHASE PROCESS    = The CHEF'S METHOD                    │
│  Decompose → Identify → Pseudo-code → Dry Run → Code      │
│                                                             │
└─────────────────────────────────────────────────────────────┘

THE FLOW:
  Phase 2 (Identify Pattern) → Pattern gives you Template → Template IS the Algorithm
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
> Example: Sort first → then use 📊 Pointer Walker or 🔪 Binary Search.

### Built-in Sort (Use This 99% of the Time)

```javascript
// ⚠️ DEFAULT .sort() IS ALPHABETICAL — NOT NUMERIC!
[10, 2, 30].sort();              // [10, 2, 30] → WRONG! (lexicographic)
[10, 2, 30].sort((a, b) => a - b); // [2, 10, 30] → CORRECT (numeric ascending)
[10, 2, 30].sort((a, b) => b - a); // [30, 10, 2] → numeric descending

// Sort strings
["banana", "apple", "cherry"].sort(); // ["apple", "banana", "cherry"]

// Sort objects by property
const people = [{name: "Bob", age: 25}, {name: "Ana", age: 30}];
people.sort((a, b) => a.age - b.age);  // sort by age ascending
```

### Bubble Sort (Simple but Slow — Learn for Understanding Only)

```
WHAT: Repeatedly swap adjacent elements if they're in wrong order.
      Like bubbles rising — biggest numbers "bubble" to the end.
TIME: O(n²) — too slow for real use. Only for learning.
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
  Pass 1: compare 5,3 → swap → [3,5,1]
          compare 5,1 → swap → [3,1,5]
  Pass 2: compare 3,1 → swap → [1,3,5]
  Done: [1, 3, 5] ✓
```

### Selection Sort (Simple but Slow)

```
WHAT: Find the minimum in remaining array, put it at the front.
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

```
Trace: [3, 1, 4]
  i=0: find min → 1 at index 1 → swap → [1,3,4]
  Done: [1, 3, 4] ✓
```

### Insertion Sort (Good for Nearly Sorted Data)

```
WHAT: Build sorted array one element at a time. Pick next element,
      insert it into its correct position in the sorted part.
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

```
Trace: [5, 2, 4]
  i=1: current=2 → shift 5 right → [2,5,4]
  i=2: current=4 → shift 5 right → [2,4,5]
  Done: [2, 4, 5] ✓
```

### Merge Sort (Fast, Stable, Predictable)

```
WHAT: Divide array in half, sort each half, merge them back.
      Classic divide-and-conquer algorithm.
TIME: O(n log n) always — guaranteed
SPACE: O(n) — needs extra array for merging
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

```
Trace: [5, 3, 1, 4]
  Split: [5,3] and [1,4]
  Merge: [3,5] and [1,4]
  Merge: [1,3,4,5] ✓
```

### Quick Sort (Fast, In-Place, Most Used)

```
WHAT: Pick a "pivot." Put smaller items left, bigger items right.
      Recursively sort left and right parts.
TIME: O(n log n) average, O(n²) worst (rare with good pivot)
SPACE: O(log n) — in-place, just recursion stack
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
Trace: [5, 3, 1, 4, 2]  pivot = 2
  j=0: 5<2? NO
  j=1: 3<2? NO
  j=2: 1<2? YES → swap arr[0] and arr[2] → [1, 3, 5, 4, 2], i=1
  j=3: 4<2? NO
  Place pivot: swap arr[1] and arr[4] → [1, 2, 5, 4, 3]
  Pivot 2 is at index 1 — everything left is smaller ✓
  Now sort [1] and [5, 4, 3] recursively
```

### Sorting Comparison Table

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

WHEN TO USE WHAT:
  Interview says "sort this"       → use .sort((a, b) => a - b)
  Interview says "implement a sort" → Merge Sort (safe) or Quick Sort (fast)
  Nearly sorted data               → Insertion Sort is O(n)
  Learning only                    → Bubble Sort (simplest)
```

---

## Searching Algorithms

### Linear Search

```
WHAT: Check every element one by one.
TIME: O(n) — slow but works on unsorted data.
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
WHAT: Cut search space in half each step. Requires SORTED data.
TIME: O(log n) — extremely fast.
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
WHAT: Store values in Set/Map. Lookup is O(1).
TIME: O(1) per lookup, O(n) to build.
USED IN: 🔎 Memory pattern
```

```javascript
// Build lookup
const set = new Set(arr);
// Search
set.has(target);  // O(1) instant!
```

```
SEARCHING COMPARISON:
  Linear Search  → O(n)     → works on anything
  Binary Search  → O(log n) → requires sorted data
  Hash Search    → O(1)     → requires building Set/Map first (O(n) space)
```

---

## Traversal Algorithms

### Array Traversal

```javascript
// Forward
for (let i = 0; i < arr.length; i++) { /* arr[i] */ }

// Backward
for (let i = arr.length - 1; i >= 0; i--) { /* arr[i] */ }

// For...of (when you don't need index)
for (const item of arr) { /* item */ }
```

### Tree Traversal — DFS (Depth-First)

```javascript
// Inorder: Left → Root → Right (sorted order in BST)
function inorder(node) {
  if (!node) return;
  inorder(node.left);
  console.log(node.val);  // process
  inorder(node.right);
}

// Preorder: Root → Left → Right (copy tree)
function preorder(node) {
  if (!node) return;
  console.log(node.val);  // process
  preorder(node.left);
  preorder(node.right);
}

// Postorder: Left → Right → Root (delete tree)
function postorder(node) {
  if (!node) return;
  postorder(node.left);
  postorder(node.right);
  console.log(node.val);  // process
}
```

```
Used in: 🌳 Tree DFS pattern
```

### Tree/Graph Traversal — BFS (Breadth-First)

```javascript
function bfs(root) {
  if (!root) return;
  const queue = [root];
  while (queue.length > 0) {
    const node = queue.shift();
    console.log(node.val);  // process
    if (node.left) queue.push(node.left);
    if (node.right) queue.push(node.right);
  }
}
```

```
Used in: 🌊 BFS pattern
DFS vs BFS:
  DFS → goes deep, uses stack/recursion, "explore all paths"
  BFS → goes wide, uses queue, "shortest path / level by level"
```

### Graph Traversal

```javascript
// DFS on Graph
function dfsGraph(graph, start) {
  const visited = new Set();
  function dfs(node) {
    if (visited.has(node)) return;
    visited.add(node);
    console.log(node);  // process
    for (const neighbor of graph[node]) {
      dfs(neighbor);
    }
  }
  dfs(start);
}

// BFS on Graph
function bfsGraph(graph, start) {
  const visited = new Set([start]);
  const queue = [start];
  while (queue.length > 0) {
    const node = queue.shift();
    console.log(node);  // process
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
    // Either extend the current subarray, or start fresh at arr[i]
    currentSum = Math.max(arr[i], currentSum + arr[i]);
    bestSum = Math.max(bestSum, currentSum);
  }
  return bestSum;
}
```

```
Trace: [-2, 1, -3, 4, -1, 2, 1, -5, 4]
  i=1: currentSum = max(1, -2+1) = 1, best=1
  i=2: currentSum = max(-3, 1-3) = -2, best=1
  i=3: currentSum = max(4, -2+4) = 4, best=4
  i=4: currentSum = max(-1, 4-1) = 3, best=4
  i=5: currentSum = max(2, 3+2) = 5, best=5
  i=6: currentSum = max(1, 5+1) = 6, best=6
  i=7: currentSum = max(-5, 6-5) = 1, best=6
  i=8: currentSum = max(4, 1+4) = 5, best=6
  Return: 6 ✓ (subarray [4, -1, 2, 1])

LeetCode: #53 Maximum Subarray
```

### Floyd's Cycle Detection (Tortoise and Hare)

```
WHAT: Detect if a linked list has a cycle using slow + fast pointers.
LIVES IN: 📊 Pointer Walker pattern
TIME: O(n), SPACE: O(1)
```

```javascript
function hasCycle(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;          // moves 1 step
    fast = fast.next.next;     // moves 2 steps
    if (slow === fast) return true;   // they met → cycle!
  }
  return false;   // fast reached end → no cycle
}
```

```
VISUAL:
  No cycle:  1 → 2 → 3 → null    fast reaches null → false
  Cycle:     1 → 2 → 3 → 4
                  ↑         ↓
                  └─────────┘    slow and fast will meet → true

LeetCode: #141 Linked List Cycle, #142 Linked List Cycle II
```

### Dijkstra's Algorithm (Shortest Path in Weighted Graph)

```
WHAT: Find shortest path from one node to all other nodes in a weighted graph.
LIVES IN: 🏃 Greedy pattern (always pick the closest unvisited node)
TIME: O((V + E) log V) with a heap
```

```javascript
function dijkstra(graph, start) {
  // graph[node] = [{neighbor, weight}, ...]
  const dist = new Map();
  const visited = new Set();

  // Initialize all distances as Infinity
  for (const node of graph.keys()) {
    dist.set(node, Infinity);
  }
  dist.set(start, 0);

  // Simple version without heap (O(V²) — heap version is faster)
  while (visited.size < graph.size) {
    // Find unvisited node with smallest distance
    let current = null;
    let minDist = Infinity;
    for (const [node, d] of dist) {
      if (!visited.has(node) && d < minDist) {
        current = node;
        minDist = d;
      }
    }

    if (current === null) break;  // remaining nodes unreachable
    visited.add(current);

    // Update distances to neighbors
    for (const { neighbor, weight } of graph.get(current)) {
      if (!visited.has(neighbor)) {
        const newDist = dist.get(current) + weight;
        if (newDist < dist.get(neighbor)) {
          dist.set(neighbor, newDist);
        }
      }
    }
  }

  return dist;  // Map of node → shortest distance from start
}

// Example usage:
const weightedGraph = new Map();
weightedGraph.set('A', [{neighbor: 'B', weight: 4}, {neighbor: 'C', weight: 1}]);
weightedGraph.set('B', [{neighbor: 'D', weight: 1}]);
weightedGraph.set('C', [{neighbor: 'B', weight: 2}, {neighbor: 'D', weight: 5}]);
weightedGraph.set('D', []);

dijkstra(weightedGraph, 'A');
// A→0, B→3 (A→C→B), C→1 (A→C), D→4 (A→C→B→D)
```

```
VISUAL:
       4
  A ------→ B
  |          |
 1|         1|
  ↓    2     ↓
  C ------→ D
       5
  C also connects to D directly (weight 5)

  Shortest: A→C (1), then C→B (2) = 3, then B→D (1) = 4
  Not A→B (4) directly — going through C is cheaper!

LeetCode: #743 Network Delay Time, #1631 Path with Min Effort
```

### Topological Sort (Task Ordering with Dependencies)

```
WHAT: Order tasks so that every dependency is completed before the task.
      Only works on DAGs (Directed Acyclic Graphs — no cycles).
LIVES IN: Level 4 specialist patterns
TIME: O(V + E)
```

```javascript
function topologicalSort(numNodes, edges) {
  // Build graph and count in-degrees
  const graph = Array.from({ length: numNodes }, () => []);
  const inDegree = Array(numNodes).fill(0);

  for (const [from, to] of edges) {
    graph[from].push(to);
    inDegree[to]++;
  }

  // Start with all nodes that have 0 dependencies
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
      if (inDegree[neighbor] === 0) {
        queue.push(neighbor);  // all dependencies met → ready
      }
    }
  }

  // If order doesn't include all nodes → there was a cycle
  return order.length === numNodes ? order : [];
}

// Example: 4 courses, prerequisites: 1→0, 2→0, 3→1, 3→2
topologicalSort(4, [[1,0], [2,0], [3,1], [3,2]]);
// → [1, 2, 3, 0] or [2, 1, 3, 0] — both valid
// Meaning: do 1 and 2 first (no deps), then 3, then 0
```

```
LeetCode: #207 Course Schedule, #210 Course Schedule II
```

### Union-Find (Disjoint Set)

```
WHAT: Track which elements belong to the same group.
      Two operations: union (merge groups) and find (which group?).
LIVES IN: Level 4 specialist patterns
TIME: Nearly O(1) per operation with path compression
```

```javascript
class UnionFind {
  constructor(n) {
    this.parent = Array.from({ length: n }, (_, i) => i); // each node is its own parent
    this.rank = Array(n).fill(0);
  }

  find(x) {
    if (this.parent[x] !== x) {
      this.parent[x] = this.find(this.parent[x]);  // path compression
    }
    return this.parent[x];
  }

  union(x, y) {
    const rootX = this.find(x);
    const rootY = this.find(y);
    if (rootX === rootY) return false;  // already in same group

    // Union by rank — attach smaller tree under bigger tree
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

  connected(x, y) {
    return this.find(x) === this.find(y);
  }
}

// Example: 5 people, connect friends
const uf = new UnionFind(5);
uf.union(0, 1);   // 0 and 1 are friends
uf.union(2, 3);   // 2 and 3 are friends
uf.connected(0, 1); // true
uf.connected(0, 2); // false
uf.union(1, 2);   // now groups merge
uf.connected(0, 3); // true (0-1-2-3 all connected)
```

```
LeetCode: #547 Number of Provinces, #684 Redundant Connection, #200 Number of Islands
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
│ Selection Sort       │ Find min, place it (slow)   │ Sorting      │
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
Trace: [3, 7, 2] → biggest = 7 ✓
```

```
Practice Problems:
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
Trace: [1, 4, 3, 8]
  1: odd → skip    winners=[]
  4: even → push   winners=[4]
  3: odd → skip    winners=[4]
  8: even → push   winners=[4,8]
  Return: [4, 8] ✓
```

```
Practice Problems:
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
Trace: [2, 3, 5]
  2→4, 3→9, 5→25 → [4, 9, 25] ✓
```

```
Practice Problems:
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
Set → just "have I seen it?"           → seen.has(x), seen.add(x)
Map → "seen it, and WHAT was stored?"  → map.get(x), map.set(x, value)
```

```javascript
// Template (Set)
function memorySet(arr) {
  const seen = new Set();
  for (let i = 0; i < arr.length; i++) {
    if (seen.has(arr[i])) { /* FOUND — handle it */ }
    seen.add(arr[i]);
  }
}

// Template (Map — for counting)
function memoryMap(arr) {
  const freq = new Map();
  for (let i = 0; i < arr.length; i++) {
    freq.set(arr[i], (freq.get(arr[i]) || 0) + 1);
  }
  return freq;
}
```

**Example — "Remove Duplicates"**

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
LeetCode: #1 Two Sum, #217 Contains Duplicate, #242 Valid Anagram, #387 First Unique
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
Practice Problems:
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
  ✗ You need to find a pair, not a range
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
Trace: [1,4,2,7,1] k=3
  First window [1,4,2] = 7
  Slide: +7-1 = 13 → best=13
  Slide: +1-4 = 10
  Return: 13 ✓
```

```
Practice:
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
LeetCode: #704 Binary Search, #34 First/Last Position, #33 Rotated Search, #69 Sqrt
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 9: 🏗️ Stack Pattern

```
USE WHEN:  Matching pairs, undo, Last-In-First-Out processing
KEYWORDS:  "Valid parentheses", "Next greater", "Evaluate expression"
LOGIC:     Push items. Pop when condition met. Check what's on top.

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
  ( → push    stack: [(]
  [ → push    stack: [(, []
  ] → pop [   match ✓
  ) → pop (   match ✓
  Empty → true ✓
```

```
Practice:
LeetCode: #20 Valid Parentheses, #150 Evaluate RPN, #739 Daily Temperatures, #155 Min Stack
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

```
Practice:
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
LeetCode: #102 Level Order, #200 Number of Islands, #994 Rotting Oranges, #542 01 Matrix
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
  ✗ Sub-problems don't overlap (use Divide & Conquer)
  ✗ A greedy/local choice is provably optimal (use 🏃 Greedy)

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
// n=5 → dp: 1,1,2,3,5,8 → return 8 ✓
```

```
Trace: n=5
  dp[0]=1  dp[1]=1  dp[2]=2  dp[3]=3  dp[4]=5  dp[5]=8 ✓
```

```
Practice (in order):
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
LOGIC:     CHOOSE → EXPLORE → UN-CHOOSE. Always this order.
BUILT ON:  🌳 DFS + trial-and-error

DON'T USE WHEN:
  ✗ You need just ONE solution (use 🧩 DP or 🏃 Greedy)
  ✗ You can solve it with a simple loop
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
  Tip: If unsure whether greedy works, default to DP. It's always correct.
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
  i=0: maxReach = max(0, 0+2) = 2
  i=1: maxReach = max(2, 1+3) = 4
  i=2: maxReach = max(4, 2+1) = 4
  i=3: maxReach = max(4, 3+1) = 4
  i=4: reached end → true ✓
```

```
Practice:
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

# Section E: Pattern Combos

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

# Section F: Big-O Cheat Sheet

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

*  O(k) if window tracks a Set/Map
** h = height of tree (log n balanced, n worst case)
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
* push to end is O(1), unshift to start is O(n)

THIS IS WHY PATTERN 5 (🔎 Memory) IS POWERFUL:
  Array.includes(x) → O(n)  = slow
  Set.has(x)        → O(1)  = instant
  Switching from array to set turns O(n²) into O(n).
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

When you drive somewhere new, you do not read every road on the map before you leave.
You just follow the next turn.
The GPS says "turn left here." You turn left. That is all you need right now.

This blueprint works the same way:

```
Day 1  → "Read Detective pattern. Solve this one problem."
Day 2  → "Read Bucket pattern. Solve this one problem."
Day 3  → "Read Filter pattern. Solve this one problem."
```

You do not need to know what is on Day 20 when you are on Day 1.
Just follow the next turn.

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
TODAY (before Day 1)  → Section B only (read the 6 Phases)

Day 1  → Section D, Pattern 1 only
Day 2  → Section D, Pattern 2 only
Day 3  → Section D, Pattern 3 only
...follow Section I roadmap day by day

When stuck on pattern identification → Section D Pattern Picker
When stuck on an algorithm          → Section C
When checking speed of your code    → Section F
When problem uses 2 patterns        → Section E
```

Every other section exists only for reference.
Open it when you need it. Ignore it when you do not.

---


---

# Section L: Personal Notes — Discoveries While Studying

---

## Note 1: The Positive Framing Rule (Discovered Day 1)

Q: In this blueprint, is this thinking missing —

In problem solving we need to think in the right way. For example in remove
the duplicate from the array we need to think there is no duplicate, if found
then only the negative thing executes.

Similarly moving all the zeros to back of the array — here also need to think
in the positive: check if value is not zero, place it at the first index.
If zero then only not.

This thinking in positive applies to every problem, not just arrays.

---

Yes, this is missing from the blueprint.

What you're describing is a real mental rule. It doesn't have a name in the
document, but it should.

The idea is this:

Write the condition for what you WANT, not what you don't want.

Your two examples show it perfectly:

Remove duplicates
  - Wrong framing: "if it IS a duplicate → skip"
  - Right framing:  "if it is NOT seen before → keep"

Move zeros to back
  - Wrong framing: "if it IS zero → don't place"
  - Right framing:  "if it is NOT zero → place it"

The code works either way technically, but the second way forces you to think
clearly about what belongs rather than what doesn't. This makes your logic
cleaner and your bugs fewer.

And you're right that this applies everywhere, not just arrays:

  - Validating a form  → think "what makes it valid"     not "what makes it invalid"
  - Filtering users    → think "who should I keep"        not "who should I remove"
  - Tree traversal     → think "when should I go deeper"  not "when should I stop"

Where it's missing in the blueprint:

It should sit in Section J (Golden Rules) as Rule #9:
"Write the KEEP condition, not the SKIP condition. Define what belongs,
then everything else takes care of itself."

It also belongs as a note inside Phase 1 (Decompose) — when you write your
goal sentence, force yourself to phrase it positively:
"I keep items that..." not "I remove items that..."

Good observation. The blueprint teaches the mechanics but misses this
thinking habit entirely.

## One sentence to remember:

> **One day. One pattern. One problem. 25 minutes. Close it.**

[↑ Back to Table of Contents](#table-of-contents)
