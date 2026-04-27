# The Complete Blueprint — Problem Solving Mastery
## Everything You Need in One Place — Version 5

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
  - [10. Heap (Priority Queue)](#10-heap-priority-queue)
  - [11. Deque (Double-Ended Queue)](#11-deque-double-ended-queue)
  - [Data Structure Picker](#data-structure-picker)
- [Section B: The 6 Phases](#section-b-the-6-phases)
- [Section C: Algorithms](#section-c-algorithms)
- [Section D: All Patterns (4 Levels)](#section-d-all-patterns)
- [Section E: Pattern Combos](#section-e-pattern-combos)
- [Section F: Big-O Cheat Sheet](#section-f-big-o-cheat-sheet)
- [Section G: How Patterns Connect](#section-g-how-patterns-connect)
- [Section H: Key Insights](#section-h-key-insights)
- [Section I: 53-Day Practice Roadmap](#section-i-53-day-practice-roadmap)
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
  ✗ Heap        → built using Array + heapify logic
  ✗ Deque       → built using Array (both ends)

WHEN TO READ EACH:
  Arrays + Strings + Set + Map → read BEFORE Day 1
  Stack + Queue                → read on Day 19
  Tree                         → read on Day 21
  Graph                        → read on Day 23
  Linked List                  → read on Day 42
  Heap + Deque                 → read on Day 46
```

---

## 1. Arrays

```
WHAT:    Ordered list of items. Access any item instantly by index.
SYNTAX:  const arr = [10, 20, 30]
ACCESS:  arr[0] → 10, arr[2] → 30

WHEN TO USE:
  ✅ You need to access items by position (index)
  ✅ You need to loop through items in order
  ✅ You need to store a sequence of things
  ✅ Most problems — arrays are the default choice

WHEN NOT TO USE:
  ✗ You need fast "have I seen this?" checks → use Set
  ✗ You need fast key → value lookup → use Map
  ✗ You need last-in-first-out → use Stack
  ✗ You need first-in-first-out → use Queue
```

```javascript
const fruits = ["apple", "banana", "cherry"];

// Read
fruits[0];          // "apple" — O(1) instant
fruits.length;      // 3

// Add
fruits.push("date");       // add to END   — O(1)
fruits.unshift("avocado"); // add to START — O(n) slow!

// Remove
fruits.pop();       // remove from END   — O(1)
fruits.shift();     // remove from START — O(n) slow!

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
  ✅ Access by index:        O(1) — instant
  ✅ Push / Pop (end):       O(1) — instant
  ⚠️ Shift / Unshift (start): O(n) — slow, shifts everything
  ⚠️ indexOf / includes:     O(n) — checks one by one
  ⚠️ sort():                 O(n log n)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 2. Strings

```
WHAT:    Sequence of characters. IMMUTABLE in JavaScript.
         Every time you change a string, a new one is created.
SYNTAX:  const str = "hello"
ACCESS:  str[0] → "h", str.length → 5

WHEN TO USE:
  ✅ Working with text, characters, words
  ✅ Checking patterns in text (palindrome, anagram)
```

```javascript
const str = "hello world";

str[0];                          // "h"
str.length;                      // 11
str.indexOf("world");            // 6
str.includes("hello");           // true
str.slice(0, 5);                 // "hello" — does NOT modify
str.split(" ");                  // ["hello", "world"]
str.toUpperCase();               // "HELLO WORLD"
str.toLowerCase();               // "hello world"
str.trim();                      // removes whitespace from both ends
str.replace("hello", "hi");      // "hi world"

for (const ch of str) { console.log(ch); }

[...str];                        // ["h","e","l","l","o"," ","w","o","r","l","d"]
[..."hello"].reverse().join(""); // "olleh" — reverse a string
```

```
KEY FACTS:
  ✅ Access by index: O(1)
  ⚠️ IMMUTABLE — str[0] = "x" does NOT work
     Every change creates a NEW string
  ⚠️ Concatenation in a loop: O(n²) — very slow
     Use array.push() then .join("") instead
  ⚠️ indexOf: O(n)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 3. Set

```
WHAT:    Collection of UNIQUE values. No duplicates allowed.
         Lookup is O(1) — instant no matter how big.
WHY:     Array.includes() is O(n) — slow.
         Set.has() is O(1) — instant.
         That one swap turns O(n²) solutions into O(n).

WHEN TO USE:
  ✅ "Have I seen this before?" — fast existence check
  ✅ Remove duplicates from array
  ✅ Check if two arrays share values
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

// Remove duplicates from array
const unique = [...new Set([1, 2, 2, 3, 3, 3])]; // [1, 2, 3]
```

```
KEY FACTS:
  ✅ add():    O(1)
  ✅ has():    O(1) — THIS IS WHY SET EXISTS
  ✅ delete(): O(1)
  ⚠️ No duplicates — adding same value twice does nothing
  ⚠️ No index access — cannot do set[0]

WHY SET EXISTS:
  Array.includes(x) → checks every item one by one → O(n) SLOW
  Set.has(x)        → jumps directly to answer     → O(1) FAST
  If you check membership in a loop → ALWAYS use Set
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 4. Map (Hash Map)

```
WHAT:    Key-value pairs. Instant lookup O(1).
WHY:     Store extra information about each item.
         "I have seen this value — and here is what I remember about it."

WHEN TO USE:
  ✅ Count how many times each item appears
  ✅ Remember the index where you saw something
  ✅ Group items by a category
  ✅ Any time you need value → extra info
```

```javascript
const memory = new Map();

memory.set("apple", 3);
memory.set("banana", 5);
memory.set("apple", 7);   // overwrites — apple is now 7

memory.get("apple");   // 7   — O(1)
memory.get("cherry");  // undefined
memory.has("banana");  // true — O(1)
memory.delete("banana");
memory.size;           // 1

for (const [key, value] of memory) { console.log(key, value); }

// Counting frequency — most common Map use case
const freq = new Map();
for (const ch of "hello") {
  freq.set(ch, (freq.get(ch) || 0) + 1);
}
// freq = {"h"→1, "e"→1, "l"→2, "o"→1}
```

```
KEY FACTS:
  ✅ set(): O(1)   ✅ get(): O(1)   ✅ has(): O(1)
  ✅ Keys can be ANY type — numbers, strings, objects

SET vs MAP — how to decide:
  "Have I seen this value?"       → Set  (just yes/no)
  "How many times did I see it?"  → Map  (key → count)
  "What index was this at?"       → Map  (key → index)
  "Group items by a category?"    → Map  (key → array)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 5. Stack

```
WHAT:    Last-In, First-Out (LIFO).
         Like a stack of plates — you add and remove from the TOP only.
WHY:     Matching brackets, undo operations, expression evaluation,
         any problem where you need to remember what came JUST BEFORE.
HOW:     In JS, use array with push() and pop()
READ:    Day 19 — one day before Pattern 9
```

```
VISUAL:
  push(10) → push(20) → push(30)

  Before:          After push(30):      After pop():
  empty            |  30  | ← top       |  20  | ← top
                   |  20  |             |  10  |
                   |  10  |             └──────┘
                   └──────┘

  You can ONLY see and remove the TOP item.
  The bottom items are not accessible.
```

```javascript
const stack = [];

stack.push(10);   // [10]
stack.push(20);   // [10, 20]
stack.push(30);   // [10, 20, 30] ← 30 is on top

const top = stack[stack.length - 1];  // peek at top → 30 (don't remove)

stack.pop();      // returns 30, stack is now [10, 20]
stack.pop();      // returns 20, stack is now [10]
stack.pop();      // returns 10, stack is now []
stack.length === 0; // true — stack is empty

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
    if ('([{'.includes(ch)) stack.push(ch);
    else if (stack.pop() !== pairs[ch]) return false;
  }
  return stack.length === 0;
}
isBalanced("([{}])"); // true
isBalanced("([)]");   // false
```

```
KEY FACTS:
  ✅ push(): O(1)   ✅ pop(): O(1)   ✅ peek: O(1)
  Think: "Last In, First Out" — like undoing your last action

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
WHAT:    First-In, First-Out (FIFO).
         Like a line at a shop — first person in is first served.
WHY:     BFS traversal, scheduling, level-order processing.
         Any problem where order of arrival matters.
HOW:     In JS, use array with push() and shift()
READ:    Day 23 — one day before Pattern 11
```

```
VISUAL:
  Items enter from BACK, leave from FRONT.

  push(10) → push(20) → push(30):
  FRONT → | 10 | 20 | 30 | ← BACK

  shift() → removes 10 from FRONT:
  FRONT → | 20 | 30 | ← BACK

  First in (10) is first out. Order is preserved.
```

```javascript
const queue = [];

queue.push(10);   // [10]
queue.push(20);   // [10, 20]
queue.push(30);   // [10, 20, 30]

const front = queue[0]; // peek at front → 10

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
  ⚠️ shift(): O(n) — remove from front (acceptable for most problems)
  Think: "First In, First Out" — like a fair waiting line

COMMON PROBLEMS:
  - BFS Level Order (#102)
  - Number of Islands (#200)
  - Rotting Oranges (#994)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 7. Linked List

```
WHAT:    Chain of nodes. Each node holds a value and a pointer to the next node.
WHY:     O(1) insert and delete at head — no shifting needed.
HOW:     Build using a class with val and next properties.
READ:    Day 42 — before Fast Slow Pointer days
```

```
VISUAL:
  head
   ↓
  [1] → [2] → [3] → null

  Each box is a NODE.
  Arrow is the POINTER (next).
  Last node points to null — end of list.

  To get to node 3, you MUST walk through 1 and 2 first.
  No jumping by index like arrays.
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

// FIND MIDDLE (Fast Slow Pointer)
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
  ✅ Insert/delete at head: O(1) — just change pointer
  ⚠️ Insert/delete at tail: O(n) — must walk to end
  ⚠️ Access by position:    O(n) — must walk from head
  ⚠️ Search:                O(n) — must walk from head

COMMON PROBLEMS:
  #206 Reverse    #141 Detect Cycle   #21 Merge Two Sorted
  #19 Remove Nth  #876 Find Middle    #234 Palindrome
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 8. Tree (Binary Tree)

```
WHAT:    Hierarchical structure.
         Each node has a value, a left child, and a right child.
         BST rule: left child < parent < right child
WHY:     Searching sorted data, representing hierarchical relationships.
READ:    Day 21 — one day before Pattern 10
```

```
VISUAL:
         5          ← root (top of tree)
        / \
       3   8        ← internal nodes
      / \   \
     1   4   9      ← leaves (no children)

  Depth: root is level 0, children are level 1, etc.
  Height: longest path from root to leaf = 2 here

BST PROPERTY:
  Left subtree values < parent value
  Right subtree values > parent value
  → allows binary search: O(log n) average
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
inorder(root); // 1, 3, 4, 5, 8, 9 ← sorted!

// Preorder: Root → Left → Right (used to copy tree)
function preorder(node) {
  if (!node) return;
  console.log(node.val);
  preorder(node.left);
  preorder(node.right);
}

// Postorder: Left → Right → Root (used to delete tree)
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
  ⚠️ BST worst case:    O(n) — unbalanced (like a linked list)
  ✅ Every tree DFS problem has the same skeleton:
     1. Base case: if null → return something
     2. Recurse left
     3. Recurse right
     4. Combine results

TRAVERSAL ORDERS:
  Inorder   (L→Root→R) → sorted order in BST
  Preorder  (Root→L→R) → copy tree
  Postorder (L→R→Root) → delete tree
  Level order (BFS)    → level by level

COMMON PROBLEMS:
  #104 Max Depth    #226 Invert    #98 Validate BST
  #102 Level Order  #112 Path Sum  #236 LCA  #543 Diameter
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 9. Graph

```
WHAT:    Nodes (vertices) connected by edges.
         Can be directed (one-way) or undirected (two-way).
WHY:     Social networks, maps, dependencies, routing.
HOW:     Adjacency list — array of arrays or Map.
READ:    Day 23 — one day before Pattern 11
```

```
VISUAL:

  Undirected (two-way):    Directed (one-way):
    0 --- 1                  0 → 1
    |     |                  ↓   ↓
    2 --- 3                  2 → 3

  Adjacency list for undirected:
    0: [1, 2]
    1: [0, 3]
    2: [0, 3]
    3: [1, 2]

  Each node lists its NEIGHBORS.
```

```javascript
// Build from edge list
function buildGraph(n, edges) {
  const graph = Array.from({ length: n }, () => []);
  for (const [from, to] of edges) {
    graph[from].push(to);
    graph[to].push(from); // remove for directed graph
  }
  return graph;
}

// DFS — explore all paths deeply
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

// BFS — explore level by level, finds shortest path
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
  ✅ DFS: O(V+E) time, O(V) space
  ✅ BFS: O(V+E) time, O(V) space
  ✅ BFS finds shortest path in UNWEIGHTED graphs
  ⚠️ Always track visited nodes — or you loop forever

  DFS → explore all paths, detect cycles, find components
  BFS → shortest path, level-by-level, nearest neighbor

COMMON PROBLEMS:
  #200 Islands  #133 Clone  #207 Course Schedule  #323 Components
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 10. Heap (Priority Queue)

```
WHAT:    A special tree where the MINIMUM (or MAXIMUM) element
         is always at the top and accessible in O(1).
         Inserting and removing takes O(log n).
WHY:     When you repeatedly need the smallest or largest item.
         A sorted array gives O(1) peek but O(n) insert.
         A Heap gives O(1) peek AND O(log n) insert — much better.
HOW:     Built using Array with heapify logic.
READ:    Day 46 — before Heap days 47-48
```

```
VISUAL (MinHeap):
         1          ← MINIMUM is always at top
        / \
       3   2
      / \
     7   5

  Parent is ALWAYS SMALLER than its children.
  This is the "heap property."

  Array representation: [1, 3, 2, 7, 5]
    Index 0 = root = minimum
    Parent of index i    → Math.floor((i-1)/2)
    Left child of i      → 2*i + 1
    Right child of i     → 2*i + 2

WHEN TO USE:
  ✅ "Find Kth largest/smallest"
  ✅ "Top K frequent elements"
  ✅ "Running median"
  ✅ "Merge K sorted lists"
  ✅ "Shortest path (Dijkstra)"
```

```javascript
class MinHeap {
  constructor() { this.heap = []; }

  parent(i) { return Math.floor((i - 1) / 2); }
  left(i)   { return 2 * i + 1; }
  right(i)  { return 2 * i + 2; }

  peek()  { return this.heap[0]; }   // O(1) — get minimum
  size()  { return this.heap.length; }

  // insert — O(log n)
  insert(val) {
    this.heap.push(val);
    let i = this.heap.length - 1;
    while (i > 0 && this.heap[this.parent(i)] > this.heap[i]) {
      [this.heap[this.parent(i)], this.heap[i]] =
      [this.heap[i], this.heap[this.parent(i)]];
      i = this.parent(i);
    }
  }

  // extract minimum — O(log n)
  extractMin() {
    if (!this.heap.length) return null;
    const min = this.heap[0];
    const last = this.heap.pop();
    if (this.heap.length > 0) {
      this.heap[0] = last;
      let i = 0;
      while (true) {
        let smallest = i;
        const l = this.left(i), r = this.right(i);
        if (l < this.heap.length && this.heap[l] < this.heap[smallest]) smallest = l;
        if (r < this.heap.length && this.heap[r] < this.heap[smallest]) smallest = r;
        if (smallest === i) break;
        [this.heap[smallest], this.heap[i]] = [this.heap[i], this.heap[smallest]];
        i = smallest;
      }
    }
    return min;
  }
}

// REAL EXAMPLE: Kth Largest using MinHeap of size k
function findKthLargest(nums, k) {
  const heap = new MinHeap();
  for (const num of nums) {
    heap.insert(num);
    if (heap.size() > k) heap.extractMin(); // keep only k largest
  }
  return heap.peek(); // minimum of top k = kth largest
}
findKthLargest([3,2,1,5,6,4], 2); // 5 ✓
```

```
KEY FACTS:
  ✅ peek():       O(1)      — get min instantly
  ✅ insert():     O(log n)  — add and fix heap
  ✅ extractMin(): O(log n)  — remove min and fix heap

  MinHeap → smallest always at top
  MaxHeap → largest always at top (flip all comparisons)

COMMON PROBLEMS:
  #215 Kth Largest    #347 Top K Frequent
  #295 Find Median    #373 K Pairs
```

[↑ Back to Table of Contents](#table-of-contents)

---

## 11. Deque (Double-Ended Queue)

```
WHAT:    Queue that allows adding AND removing from BOTH ends.
         Front ← add/remove    Back → add/remove
WHY:     Used in Sliding Window Maximum to maintain a decreasing
         window of candidates efficiently.
HOW:     Simulate using Array in JavaScript.
READ:    Day 46 — same day as Heap
```

```
VISUAL:
  Normal Queue:  can only remove from FRONT
  FRONT → | 10 | 20 | 30 | ← BACK

  Deque: can add/remove from BOTH ends
  ← | 10 | 20 | 30 | →
    remove        remove
    or add        or add
```

```javascript
const deque = [];

deque.push(5);     // add to back  → [5]
deque.push(3);     // add to back  → [5, 3]
deque.unshift(7);  // add to front → [7, 5, 3]
deque.pop();       // remove from back  → returns 3, deque=[7,5]
deque.shift();     // remove from front → returns 7, deque=[5]

const front = deque[0];
const back  = deque[deque.length - 1];

// REAL EXAMPLE: Sliding Window Maximum
function maxSlidingWindow(nums, k) {
  const result = [];
  const deque = []; // stores INDICES, values stay decreasing

  for (let i = 0; i < nums.length; i++) {

    // remove indices that are outside the current window
    while (deque.length > 0 && deque[0] < i - k + 1) {
      deque.shift();
    }

    // remove indices whose VALUES are smaller than current
    // (they can never be the window maximum)
    while (deque.length > 0 && nums[deque[deque.length-1]] < nums[i]) {
      deque.pop();
    }

    deque.push(i);

    // window is full — front of deque = index of maximum
    if (i >= k - 1) {
      result.push(nums[deque[0]]);
    }
  }
  return result;
}

maxSlidingWindow([1,3,-1,-3,5,3,6,7], 3);
// [3,3,5,5,6,7] ✓
```

```
KEY FACTS:
  ✅ push/pop (back):       O(1)
  ⚠️ shift/unshift (front): O(n) — acceptable for most problems

WHEN TO USE:
  Sliding Window Maximum → Deque maintains decreasing candidates
  Shortest Subarray with Sum ≥ K

COMMON PROBLEMS:
  #239 Sliding Window Maximum
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
  Last-in-first-out (LIFO)             → Stack  (array push/pop)
  First-in-first-out (FIFO)            → Queue  (array push/shift)
  Add/remove from BOTH ends            → Deque  (array both ends)
  O(1) insert/delete at head           → Linked List
  Hierarchical / sorted data           → Tree
  Connected relationships              → Graph
  Always need min or max quickly       → Heap
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section B: The 6 Phases

> Follow these 6 phases for every problem, every time.
> Do not skip phases. The phases exist because skipping them causes bugs.

```
PHASE 1 → DECOMPOSE:   "What goes in? What comes out?"
PHASE 2 → IDENTIFY:    "Which pattern is this?"
PHASE 3 → PSEUDO-CODE: "Write the solution in English"
PHASE 4 → DRY RUN:     "Trace table — prove it on paper"
PHASE 5 → CODE:        "Translate English → JavaScript"
PHASE 6 → REFINE:      "Test edge cases, check complexity"
```

---

## Phase 1: Decompose

```
Ask these 5 questions BEFORE writing any code:

  1. What is the INPUT?
     → What type? (array, string, number, tree)
     → What size? (small, large, can it be empty?)
     → Any constraints? (sorted? all positive? unique?)

  2. What is the OUTPUT?
     → Boolean? Number? Array? String? Nothing?
     → This tells you which pattern to use.

  3. What are the RULES?
     → What must always be true?
     → What are the restrictions?

  4. Write the GOAL SENTENCE:
     "I take [INPUT] and return [OUTPUT] by doing [X]"
     If you cannot write this sentence, you do not understand the problem yet.

  5. List EDGE CASES:
     → Empty input: [] or ""
     → Single item: [5]
     → All same: [3,3,3]
     → Negative numbers: [-1,2,-3]
     → Already solved: sorted array for "sort this"

POSITIVE FRAMING RULE:
  Write the condition for what you WANT, not what you don't want.
  "I keep items that..."  not  "I remove items that..."

  WRONG: "Remove duplicates" → think "skip if already seen"
  RIGHT: "Remove duplicates" → think "keep if NOT seen before"

  WRONG: "Move zeros to end" → think "skip if zero"
  RIGHT: "Move zeros to end" → think "keep if NOT zero, place it"

  This makes your code cleaner and easier to read.
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
  (see Section D for all 14 patterns)

Once you identify the pattern:
  → the pattern gives you a TEMPLATE
  → the algorithm lives INSIDE that template
  → you do not pick the algorithm separately

THE FLOW:
  "What do I return?" → Pattern → Template → Algorithm comes free

EXAMPLES:
  Return one value   → 🪣 Bucket → template: running variable
                                  → algorithm: Linear Scan
  Return true/false  → 🔍 Detective → template: loop + early return
                                     → algorithm: Linear Scan
  Sorted data search → 🔪 Binary Search → template: lo/mid/hi loop
                                         → algorithm: Binary Search

REMEMBER:
  You NEVER ask "which algorithm do I use?"
  You ONLY ask "what do I return?"
  The rest comes automatically from the pattern.
```

---

## Phase 3: Pseudo-code

```
Write 3-5 lines of English BEFORE writing JavaScript.
Use these words:

  STORE    → create a variable or data structure
  REPEAT   → loop through items
  IF/THEN  → condition check
  COMPARE  → check two values against each other
  ADD TO   → push item to a list
  GIVE BACK → return the result

EXAMPLE for "sum of array":
  STORE result = 0
  REPEAT for each number in array
    ADD number to result
  GIVE BACK result

If you cannot write it in English, you cannot code it.
The English IS the solution. Code is just translation.
```

---

## Phase 4: Dry Run (Trace Table)

> **This is the most important phase.**
> Most bugs are caught here, not during coding.
> A trace table that gives the right answer = correct logic.
> Wrong trace table = catch the bug BEFORE writing code.

```
How to draw a trace table:
  1. Pick a SMALL input (3-4 items)
  2. List your variables as COLUMNS
  3. Each ROW = one step of the loop
  4. Fill in what each variable holds at each step
  5. Check if final row = expected answer

EXAMPLE — Sum of [3, 1, 4]:
  | Step  | i | arr[i] | result |
  | Start | — | —      | 0      |
  | i=0   | 0 | 3      | 3      |
  | i=1   | 1 | 1      | 4      |
  | i=2   | 2 | 4      | 8      |
  | End   | — | —      | 8 ✓    |

RULES:
  ✅ If trace gives expected output → logic is correct → code it
  ✗  If trace gives wrong output  → fix logic first → never code wrong logic
  ✅ Use at least 2 different inputs to verify
```

---

## Phase 5: Code

```
Translate your English pseudo-code into JavaScript.
Four rules:

  1. Paste your English recipe as COMMENTS first
     // STORE result = 0
     // REPEAT for each number
     // ADD number to result
     // GIVE BACK result

  2. Write ONE line of JavaScript per comment
     let result = 0;
     for (let i = 0; i < arr.length; i++) {
       result += arr[i];
     }
     return result;

  3. Use MEANINGFUL variable names
     ✅ result, seen, current, left, right
     ✗  x, temp, a, b, foo

  4. Handle EDGE CASES at the very top
     if (arr.length === 0) return 0;
```

---

## Phase 6: Refine

```
Three checks after coding:

  1. Test EDGE CASES:
     → Empty:      []  or  ""
     → Single:     [5]
     → All same:   [3,3,3]
     → Negatives:  [-1, 2, -3]

  2. Ask TIME complexity:
     → One loop → O(n)
     → Loop inside loop → O(n²)
     → Halving each step → O(log n)
     → No loop → O(1)

  3. Ask SPACE complexity:
     → Created new array/Set/Map growing with input → O(n)
     → Only a few variables → O(1)

  Then ask: "Can I make it faster?"
  And ask:  "Would a teammate understand this in 5 seconds?"
```

---

## Translation Dictionary

```
PLAIN ENGLISH          →  JAVASCRIPT
─────────────────────────────────────────────────────────
STORE a value          →  const x = value
STORE empty list       →  const result = []
STORE empty memory     →  const seen = new Set()
STORE frequency map    →  const freq = new Map()
REPEAT for each        →  for (let i = 0; i < arr.length; i++)
REPEAT while condition →  while (condition) { }
IF something           →  if (condition) { }
COMPARE two values     →  if (a === b)
ADD TO list            →  result.push(item)
PUT IN memory (Set)    →  seen.add(item)
CHECK memory (Set)     →  seen.has(item)
PUT IN memory (Map)    →  freq.set(key, value)
GET FROM memory (Map)  →  freq.get(key)
GIVE BACK              →  return result
STOP early             →  return false (inside loop)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section C: Algorithms

> **Patterns tell you WHAT shape the problem is.**
> **Algorithms tell you HOW to execute the solution.**
> **When you pick a pattern, the algorithm comes FREE inside the template.**

---

## Section C — Quick Navigation

| What I Need | Go To |
|---|---|
| Which algorithm does my pattern use? | [Pattern to Algorithm Map](#pattern-to-algorithm-map) |
| All two pointer approaches | [Two Pointer Approaches](#two-pointer-approaches) |
| Sort an array | [Sorting Algorithms](#sorting-algorithms) |
| Search — linear, binary, hash | [Searching Algorithms](#searching-algorithms) |
| Walk through array/tree/graph | [Traversal Algorithms](#traversal-algorithms) |
| Max subarray sum | [Kadane's Algorithm](#kadanes-algorithm) |
| Detect cycle / find middle | [Floyd's and Fast Slow Pointer](#floyds-cycle-detection) |
| Shortest path weighted graph | [Dijkstra's Algorithm](#dijkstras-algorithm) |
| Order tasks with dependencies | [Topological Sort](#topological-sort) |
| Track connected groups | [Union-Find](#union-find) |
| Range sum queries O(1) | [Prefix Sum](#prefix-sum) |
| Next greater element | [Monotonic Stack](#monotonic-stack) |
| Three-way partition | [Dutch National Flag](#dutch-national-flag) |
| Sort small range fast | [Counting Sort](#counting-sort) |
| Heap insert/extract | [Heap Operations](#heap-operations) |
| Find pattern in string O(n) | [KMP String Search](#kmp-string-search) |
| All algorithms in one place | [Complete Algorithm Map](#complete-algorithm-map) |

---

## How They Relate

```
┌─────────────────────────────────────────────────────────────┐
│  DATA STRUCTURES = The TOOLS (pot, pan, oven)               │
│  Array, Set, Map, Stack, Queue, Tree, Graph, Heap           │
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

---

## Pattern to Algorithm Map

```
Pattern 1  → 🔍 Detective      → Linear Search
Pattern 2  → 🪣 Bucket         → Linear Search + Kadane's
Pattern 3  → 🏆 Filter         → Linear Search
Pattern 4  → 🔄 Transformer    → Linear Search
Pattern 5  → 🔎 Memory         → Hash-Based Search
Pattern 6  → 📊 Pointer Walker → Two Pointer (all 4 approaches)
                                + Floyd's Cycle Detection
                                + Fast Slow Pointer
                                + Dutch National Flag
Pattern 7  → 🪟 Sliding Window → Sliding Window Technique
Pattern 8  → 🔪 Binary Search  → Binary Search Algorithm
Pattern 9  → 🏗️ Stack          → Stack-based Matching
                                + Monotonic Stack
Pattern 10 → 🌳 Tree DFS       → Tree Traversal DFS
Pattern 11 → 🌊 BFS            → BFS Algorithm + Dijkstra's
                                + Topological Sort
Pattern 12 → 🧩 DP             → Memoization + Tabulation
Pattern 13 → 🔙 Backtracking   → Recursive Backtracking
Pattern 14 → 🏃 Greedy         → Greedy Choice

TOOLS — used BEFORE patterns, not inside them:
  Sorting Algorithms → used before Pattern 6, 8, 14
    JS Built-in Sort  ← use this 80% of the time
    Merge Sort        ← implement when interviewer asks
    Quick Sort        ← implement when interviewer asks
    Insertion Sort    ← use for nearly sorted data
    Bubble Sort       ← learning only
    Selection Sort    ← learning only

  Prefix Sum    → used before Level 4 Pattern 17
  Counting Sort → used in specific problems
  Heap          → used in Level 4 Pattern 15
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

## Two Pointer Approaches

> All 4 approaches live inside Pattern 6 📊 Pointer Walker.
> When you see a two pointer problem, ask: "Which approach fits?"

---

### Approach 1 — Opposite Ends

```
WHEN TO USE:
  Compare or find pairs by looking at both ends simultaneously.
  Works best on sorted arrays or palindrome checks.

MOVE: left starts at 0, right starts at end.
      They move TOWARD each other until they meet.

USED FOR: palindrome, two sum sorted, container water, 3sum
```

```javascript
function oppositeEnds(arr) {
  let left = 0, right = arr.length - 1;
  while (left < right) {
    // compare arr[left] and arr[right]
    if (/* condition met */) left++;
    else right--;
  }
}

// Example: Two Sum in sorted array
function twoSumSorted(arr, target) {
  let left = 0, right = arr.length - 1;
  while (left < right) {
    const sum = arr[left] + arr[right];
    if (sum === target) return [left, right];
    else if (sum < target) left++;  // need bigger sum → move left right
    else right--;                   // need smaller sum → move right left
  }
  return [];
}
// [2,7,11,15] target=9 → [0,1] ✓
```

---

### Approach 2 — Same Direction Read/Write

```
WHEN TO USE:
  Modify array IN-PLACE. Keep certain elements, remove others.
  The write pointer is always behind or equal to read pointer.

MOVE: write stays behind, read scans ahead.

USED FOR: remove duplicates, move zeroes, remove element
```

```javascript
function sameDirection(arr) {
  let write = 0;
  for (let read = 0; read < arr.length; read++) {
    if (/* keep this element */) {
      arr[write] = arr[read];
      write++;
    }
  }
  return write; // new length
}

// Example: Move Zeroes to end
function moveZeroes(nums) {
  let write = 0;
  for (let read = 0; read < nums.length; read++) {
    if (nums[read] !== 0) {    // KEEP if NOT zero
      nums[write] = nums[read];
      write++;
    }
  }
  // fill remaining with zeros
  while (write < nums.length) {
    nums[write] = 0;
    write++;
  }
  return nums;
}
// [0,1,0,3,12] → [1,3,12,0,0] ✓
```

---

### Approach 3 — Fast and Slow Pointer

```
WHEN TO USE:
  Problems on LINKED LISTS where you need to find the middle
  or detect a cycle without extra memory.

MOVE: slow moves 1 step, fast moves 2 steps every iteration.
      When fast reaches the end, slow is at the middle.

USED FOR: find middle, detect cycle, palindrome linked list
```

```javascript
function fastSlow(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;       // 1 step
    fast = fast.next.next;  // 2 steps
  }
  return slow; // slow is at middle when fast reaches end
}

// Example: Find middle of linked list
function findMiddle(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;
    fast = fast.next.next;
  }
  return slow;
}
// 1→2→3→4→5 → returns node with value 3 ✓
// 1→2→3→4   → returns node with value 2 ✓
```

---

### Approach 4 — Two Pointers on Sorted Array

```
WHEN TO USE:
  Multi-number sum problems (3Sum, 4Sum).
  Sort the array first, then use opposite ends for the inner loop.

MOVE: outer loop fixes one element, inner uses opposite ends.

USED FOR: 3Sum, 4Sum, triplets with condition
```

```javascript
// Example: 3Sum — all triplets that sum to 0
function threeSum(nums) {
  nums.sort((a, b) => a - b); // sort FIRST
  const result = [];

  for (let i = 0; i < nums.length - 2; i++) {
    if (i > 0 && nums[i] === nums[i-1]) continue; // skip duplicates

    let left = i + 1, right = nums.length - 1;
    while (left < right) {
      const sum = nums[i] + nums[left] + nums[right];
      if (sum === 0) {
        result.push([nums[i], nums[left], nums[right]]);
        while (left < right && nums[left] === nums[left+1]) left++;
        while (left < right && nums[right] === nums[right-1]) right--;
        left++; right--;
      } else if (sum < 0) left++;
      else right--;
    }
  }
  return result;
}
// [-1,0,1,2,-1,-4] → [[-1,-1,2],[-1,0,1]] ✓
```

---

```
TWO POINTER SUMMARY:
┌──────────────────────┬────────────────────────────┬──────────────────────┐
│ Approach             │ When to Use                │ Examples             │
├──────────────────────┼────────────────────────────┼──────────────────────┤
│ Opposite Ends        │ Pair from both ends        │ palindrome, 2sum     │
│ Same Direction R/W   │ Modify array in-place      │ move zeroes, remove  │
│ Fast and Slow        │ Middle or cycle (LL)       │ find middle, cycle   │
│ Sorted Array         │ Multi-sum problems         │ 3Sum, 4Sum           │
└──────────────────────┴────────────────────────────┴──────────────────────┘
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

## Sorting Algorithms

> Sorting is NOT a pattern. It is a TOOL used BEFORE applying a pattern.

```
SORTING QUICK REFERENCE:
  JS Built-in Sort  ← use this 80% of the time
  Bubble Sort       ← learning only (do not use in real problems)
  Selection Sort    ← learning only (do not use in real problems)
  Insertion Sort    ← use for nearly sorted data (best case O(n))
  Merge Sort        ← implement when interviewer asks
  Quick Sort        ← implement when interviewer asks

WHEN TO USE WHAT:
  Interview says "sort this array"          → .sort((a, b) => a - b)
  Interview says "implement a sort"         → Merge Sort or Quick Sort
  Data is nearly sorted already             → Insertion Sort (O(n) best)
  Learning how sorting works internally     → Bubble Sort
  Guaranteed O(n log n) with stable order   → Merge Sort
  Fastest in practice on average            → Quick Sort
```

### Built-in Sort

```javascript
[10, 2, 30].sort();                   // WRONG — sorts alphabetically!
[10, 2, 30].sort((a, b) => a - b);   // [2, 10, 30] — ascending ✓
[10, 2, 30].sort((a, b) => b - a);   // [30, 10, 2] — descending ✓
["banana", "apple"].sort();           // ["apple", "banana"] — strings OK
const people = [{name:"Bob",age:25},{name:"Ana",age:30}];
people.sort((a, b) => a.age - b.age); // sort objects by property
```

### Bubble Sort

```
WHAT: Compare adjacent elements. Swap if wrong order. Repeat.
      Each pass "bubbles" the largest unsorted element to its place.
TIME: O(n²) — learning only, too slow for real use
```

```javascript
function bubbleSort(arr) {
  const n = arr.length;
  for (let i = 0; i < n - 1; i++) {
    let swapped = false;
    for (let j = 0; j < n - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j+1]] = [arr[j+1], arr[j]];
        swapped = true;
      }
    }
    if (!swapped) break; // already sorted
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

### Selection Sort

```
WHAT: Find the minimum in the unsorted part. Put it at the front.
      Repeat for the remaining unsorted part.
TIME: O(n²) — learning only
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
WHAT: Take each element and insert it into its correct position
      in the already-sorted left portion.
      Like sorting cards in your hand one at a time.
TIME: O(n²) worst case, O(n) best case (already sorted)
USE:  Best choice when data is nearly sorted
```

```javascript
function insertionSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    const current = arr[i];
    let j = i - 1;
    while (j >= 0 && arr[j] > current) {
      arr[j + 1] = arr[j]; // shift right
      j--;
    }
    arr[j + 1] = current; // insert in correct spot
  }
  return arr;
}
```

### Merge Sort

```
WHAT: Divide array in half. Sort each half. Merge them back together.
      Divide until single elements, then merge in sorted order.
TIME: O(n log n) always — guaranteed, never worse
SPACE: O(n) — needs extra space for merging
USE:  When you need guaranteed performance + stable sort
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
WHAT: Pick a pivot. Put smaller elements left, larger right.
      Recursively sort left and right sides.
TIME: O(n log n) average, O(n²) worst (bad pivot choice)
SPACE: O(log n)
USE:  Fastest in practice on average data
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
  const pivot = arr[hi]; // choose last element as pivot
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
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

## Searching Algorithms

### Linear Search

```
WHAT: Check every element one by one until found.
TIME: O(n) — checks up to n elements
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
WHAT: Cut the search space in HALF each step.
      Check the middle. Too small → go right half.
      Too big → go left half. Repeat.
TIME: O(log n) — eliminates half the options each step
REQUIREMENT: Data MUST be sorted first
USED IN: 🔪 Binary Search pattern
```

```javascript
function binarySearch(arr, target) {
  let lo = 0, hi = arr.length - 1;
  while (lo <= hi) {
    const mid = Math.floor((lo + hi) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) lo = mid + 1; // go right
    else hi = mid - 1;                   // go left
  }
  return -1;
}
```

### Hash-Based Search

```
WHAT: Build a Set or Map first, then look up in O(1).
      The lookup is instant no matter the size.
TIME: O(n) to build, O(1) per lookup
USED IN: 🔎 Memory pattern
```

```javascript
const set = new Set(arr);
set.has(target); // O(1) — instant!
```

```
SEARCHING COMPARISON:
┌─────────────────┬───────────┬──────────────────────────────────┐
│ Algorithm       │ Time      │ Requirement                      │
├─────────────────┼───────────┼──────────────────────────────────┤
│ Linear Search   │ O(n)      │ works on anything, no prep needed│
│ Binary Search   │ O(log n)  │ data MUST be sorted first        │
│ Hash Search     │ O(1)      │ must build Set/Map first O(n)    │
└─────────────────┴───────────┴──────────────────────────────────┘

Rule: If you check membership more than once → build Set/Map first.
      One-time check → just use includes().
      Many checks → Set.has() pays for itself immediately.
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

## Traversal Algorithms

### Array Traversal

```javascript
for (let i = 0; i < arr.length; i++) { /* forward — has index */ }
for (let i = arr.length - 1; i >= 0; i--) { /* backward */ }
for (const item of arr) { /* no index needed */ }
```

### Tree DFS

```javascript
// Inorder: Left → Root → Right — gives SORTED order in BST
function inorder(node) {
  if (!node) return;
  inorder(node.left);
  console.log(node.val);
  inorder(node.right);
}

// Preorder: Root → Left → Right — used to copy tree
function preorder(node) {
  if (!node) return;
  console.log(node.val);
  preorder(node.left);
  preorder(node.right);
}

// Postorder: Left → Right → Root — used to delete tree
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
// DFS — explore all paths, goes deep before backtracking
function dfsGraph(graph, start) {
  const visited = new Set();
  function dfs(node) {
    if (visited.has(node)) return;
    visited.add(node);
    for (const neighbor of graph[node]) { dfs(neighbor); }
  }
  dfs(start);
}

// BFS — explore level by level, finds shortest path
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
DFS vs BFS — when to use which:
  DFS → "explore all paths" → components, cycle detection, path exists
  BFS → "shortest path" or "level by level" → nearest node, min steps
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

## Classic Named Algorithms

### Kadane's Algorithm

```
WHAT: Find the contiguous subarray with the LARGEST sum.
      In plain English: at each position, decide whether to
      START a new subarray here or EXTEND the existing one.
      If extending makes it smaller than starting fresh → start fresh.
LIVES IN: 🪣 Bucket pattern
TIME: O(n)
```

```javascript
function maxSubarraySum(arr) {
  let currentSum = arr[0]; // best sum ending at current position
  let bestSum = arr[0];    // best sum seen so far

  for (let i = 1; i < arr.length; i++) {
    // should I extend current subarray or start new one here?
    currentSum = Math.max(arr[i], currentSum + arr[i]);
    bestSum = Math.max(bestSum, currentSum);
  }
  return bestSum;
}
```

```
Trace: [-2, 1, -3, 4, -1, 2, 1, -5, 4]
  i=1: current=max(1, -2+1)=max(1,-1)=1,  best=1
  i=2: current=max(-3,1-3)=max(-3,-2)=-2, best=1
  i=3: current=max(4,-2+4)=max(4,2)=4,    best=4
  i=4: current=max(-1,4-1)=max(-1,3)=3,   best=4
  i=5: current=max(2,3+2)=max(2,5)=5,     best=5
  i=6: current=max(1,5+1)=max(1,6)=6,     best=6
  i=7: current=max(-5,6-5)=max(-5,1)=1,   best=6
  i=8: current=max(4,1+4)=max(4,5)=5,     best=6
  Return: 6 ✓ — subarray is [4,-1,2,1]
LeetCode: #53
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

### Floyd's Cycle Detection

```
WHAT: Detect if a linked list has a cycle.
      Uses slow + fast pointers. If cycle exists, they MUST meet.
      If no cycle, fast reaches null first.
LIVES IN: 📊 Pointer Walker
TIME: O(n), SPACE: O(1)
```

```javascript
function hasCycle(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;       // moves 1 step
    fast = fast.next.next;  // moves 2 steps
    if (slow === fast) return true; // they met — cycle exists
  }
  return false; // fast reached null — no cycle
}
```

```
VISUAL:
  No cycle:  1→2→3→null
    fast reaches null → return false ✓

  Cycle: 1→2→3→4→(back to 2)
    slow: 1,2,3,4,2,3,4...
    fast: 1,3,2,4,3,2,4...
    they meet eventually → return true ✓

LeetCode: #141, #142
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

### Fast Slow Pointer

```
WHAT: Uses two pointers at different speeds to find the
      middle of a linked list or the entry of a cycle.
LIVES IN: 📊 Pointer Walker
TIME: O(n), SPACE: O(1)
```

```javascript
// Find middle of linked list
function findMiddle(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;       // 1 step
    fast = fast.next.next;  // 2 steps
  }
  // when fast reaches end, slow is at middle
  return slow;
}

// Find cycle entry point
function detectCycleEntry(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow === fast) {
      slow = head; // reset slow to head
      while (slow !== fast) {
        slow = slow.next; // both move 1 step now
        fast = fast.next;
      }
      return slow; // entry point of cycle
    }
  }
  return null;
}
```

```
LeetCode: #876 Find Middle, #142 Cycle Entry, #234 Palindrome List
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

### Dijkstra's Algorithm

```
WHAT: Find shortest path from one node to all others in a WEIGHTED graph.
      Greedy approach — always process the unvisited node with smallest distance.
LIVES IN: 🏃 Greedy pattern
TIME: O((V+E) log V) with heap
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
      if (!visited.has(node) && d < minDist) { current = node; minDist = d; }
    }
    if (current === null) break;
    visited.add(current);

    for (const { neighbor, weight } of graph.get(current)) {
      if (!visited.has(neighbor)) {
        const newDist = dist.get(current) + weight;
        if (newDist < dist.get(neighbor)) dist.set(neighbor, newDist);
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

  Start at A:
  A→C = 1   (direct)
  A→C→D = 3 (1+2)
  A→B = 4   (direct) but A→C→D→? skip
  A→C→B = 3 (1+2) — shorter than direct 4!

LeetCode: #743, #1631
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

### Topological Sort

```
WHAT: Order tasks so every dependency comes BEFORE the task that needs it.
      "Before you can take course B, you must take course A."
LIVES IN: Level 4 + 🌊 BFS
TIME: O(V+E)
```

```javascript
function topologicalSort(numNodes, edges) {
  const graph = Array.from({ length: numNodes }, () => []);
  const inDegree = Array(numNodes).fill(0);

  for (const [from, to] of edges) {
    graph[from].push(to);
    inDegree[to]++; // to depends on from
  }

  // start with nodes that have no dependencies
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
      if (inDegree[neighbor] === 0) queue.push(neighbor); // dependency met
    }
  }

  // if order has all nodes, no cycle exists
  return order.length === numNodes ? order : [];
}
```

```
LeetCode: #207 Course Schedule, #210 Course Schedule II
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

### Union-Find

```
WHAT: Track which elements belong to the same connected group.
      Two operations: union (merge groups), find (which group?)
LIVES IN: Level 4
TIME: Nearly O(1) per operation (amortized)
```

```javascript
class UnionFind {
  constructor(n) {
    this.parent = Array.from({ length: n }, (_, i) => i);
    this.rank = Array(n).fill(0);
  }

  find(x) {
    if (this.parent[x] !== x) {
      this.parent[x] = this.find(this.parent[x]); // path compression
    }
    return this.parent[x];
  }

  union(x, y) {
    const rootX = this.find(x), rootY = this.find(y);
    if (rootX === rootY) return false; // already connected
    if (this.rank[rootX] < this.rank[rootY]) this.parent[rootX] = rootY;
    else if (this.rank[rootX] > this.rank[rootY]) this.parent[rootY] = rootX;
    else { this.parent[rootY] = rootX; this.rank[rootX]++; }
    return true;
  }

  connected(x, y) { return this.find(x) === this.find(y); }
}
```

```
LeetCode: #547, #684, #323
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

### Prefix Sum

```
WHAT: Precompute running totals so range sum queries become O(1).
      Build once in O(n). Then answer ANY range sum in O(1).
LIVES IN: Level 4 Pattern 17
TIME: O(n) build, O(1) query
SPACE: O(n)
```

```javascript
function buildPrefix(arr) {
  const prefix = [0]; // prefix[0] = 0 (empty prefix)
  for (const n of arr) {
    prefix.push(prefix[prefix.length - 1] + n);
  }
  return prefix;
}

function rangeSum(prefix, i, j) {
  return prefix[j + 1] - prefix[i];
}

// EXAMPLE
const arr    = [1,  2,  3,  4,  5];
const prefix = [0,  1,  3,  6, 10, 15];
//              ↑   ↑   ↑   ↑   ↑   ↑
//             empty  1  1+2 ...  all

rangeSum(prefix, 1, 3); // sum of arr[1..3] = 2+3+4 = 9
// prefix[4] - prefix[1] = 10 - 1 = 9 ✓

rangeSum(prefix, 0, 4); // sum of all = 15
// prefix[5] - prefix[0] = 15 - 0 = 15 ✓
```

```
LeetCode: #303 Range Sum Query, #560 Subarray Sum K, #238 Product Except Self
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

### Monotonic Stack

```
WHAT: Stack that stays ALWAYS increasing or ALWAYS decreasing.
      When a new element breaks the order, pop until order restored.
      Used for "next greater element" type problems.
LIVES IN: 🏗️ Stack → Level 4 Pattern 22
TIME: O(n), SPACE: O(n)
```

```javascript
// Next Greater Element for each position
function nextGreaterElement(arr) {
  const result = new Array(arr.length).fill(-1);
  const stack = []; // stores INDICES, values stay in decreasing order

  for (let i = 0; i < arr.length; i++) {
    // while current element is GREATER than what is on top of stack
    while (stack.length > 0 && arr[i] > arr[stack[stack.length - 1]]) {
      const idx = stack.pop();
      result[idx] = arr[i]; // current element is the next greater for idx
    }
    stack.push(i);
  }
  return result;
}
```

```
Trace: [2, 1, 5, 3, 6]
  i=0: stack empty → push 0       stack=[0]       values=[2]
  i=1: arr[1]=1 < arr[0]=2 → push 1  stack=[0,1]  values=[2,1]
  i=2: arr[2]=5 > arr[1]=1 → pop 1 → result[1]=5
       arr[2]=5 > arr[0]=2 → pop 0 → result[0]=5
       push 2                      stack=[2]       values=[5]
  i=3: arr[3]=3 < arr[2]=5 → push 3  stack=[2,3]  values=[5,3]
  i=4: arr[4]=6 > arr[3]=3 → pop 3 → result[3]=6
       arr[4]=6 > arr[2]=5 → pop 2 → result[2]=6
       push 4                      stack=[4]       values=[6]
  Remaining in stack have no next greater → result stays -1

  Return: [5, 5, 6, 6, -1] ✓

LeetCode: #496 Next Greater I, #739 Daily Temperatures, #84 Largest Rectangle, #42 Trapping Rain Water
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

### Dutch National Flag

```
WHAT: Sort an array with exactly THREE distinct values in ONE pass.
      Uses three pointers: low, mid, high.
LIVES IN: 📊 Pointer Walker
TIME: O(n), SPACE: O(1)
```

```javascript
function sortColors(arr) {
  let low = 0;               // everything before low is 0
  let mid = 0;               // current element being examined
  let high = arr.length - 1; // everything after high is 2

  while (mid <= high) {
    if (arr[mid] === 0) {
      [arr[low], arr[mid]] = [arr[mid], arr[low]];
      low++; mid++;
    } else if (arr[mid] === 1) {
      mid++; // already in correct zone
    } else { // arr[mid] === 2
      [arr[mid], arr[high]] = [arr[high], arr[mid]];
      high--;
      // do NOT increment mid — need to re-examine the swapped element
    }
  }
  return arr;
}
// [2,0,2,1,1,0] → [0,0,1,1,2,2] ✓
// LeetCode: #75 Sort Colors
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

### Counting Sort

```
WHAT: Sort by counting how many times each value appears.
      Rebuild array from counts. No comparisons needed.
      Faster than O(n log n) when value range is small and known.
TIME: O(n+k) where k = range of values
SPACE: O(k)
```

```javascript
function countingSort(arr, maxVal) {
  const count = new Array(maxVal + 1).fill(0);

  for (const n of arr) count[n]++; // count each value

  const result = [];
  for (let i = 0; i <= maxVal; i++) {
    while (count[i]-- > 0) result.push(i); // output in order
  }
  return result;
}

// EXAMPLE
countingSort([4, 2, 2, 8, 3, 3, 1], 8);
// [1, 2, 2, 3, 3, 4, 8] ✓

// USE WHEN:
// Values are integers in a known small range
// n is large but range k is small
// Example: sort ages (0-150), sort grades (0-100)
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

### Heap Operations

```
WHAT: Insert and extract min/max from heap efficiently.
LIVES IN: Level 4 Pattern 15
TIME: O(log n) insert/extract, O(1) peek
```

```javascript
class MinHeap {
  constructor() { this.heap = []; }
  parent(i) { return Math.floor((i - 1) / 2); }
  left(i)   { return 2 * i + 1; }
  right(i)  { return 2 * i + 2; }
  peek()    { return this.heap[0]; }
  size()    { return this.heap.length; }

  insert(val) {
    this.heap.push(val);
    let i = this.heap.length - 1;
    // bubble up until parent is smaller
    while (i > 0 && this.heap[this.parent(i)] > this.heap[i]) {
      [this.heap[this.parent(i)], this.heap[i]] =
      [this.heap[i], this.heap[this.parent(i)]];
      i = this.parent(i);
    }
  }

  extractMin() {
    if (!this.heap.length) return null;
    const min = this.heap[0];
    const last = this.heap.pop();
    if (this.heap.length > 0) {
      this.heap[0] = last;
      let i = 0;
      // bubble down until children are larger
      while (true) {
        let smallest = i;
        const l = this.left(i), r = this.right(i);
        if (l < this.heap.length && this.heap[l] < this.heap[smallest]) smallest = l;
        if (r < this.heap.length && this.heap[r] < this.heap[smallest]) smallest = r;
        if (smallest === i) break;
        [this.heap[smallest], this.heap[i]] = [this.heap[i], this.heap[smallest]];
        i = smallest;
      }
    }
    return min;
  }
}
// LeetCode: #215, #347, #295
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

### KMP String Search

```
WHAT: Find a pattern inside a text string in O(n) time.
      Brute force would be O(n×m). KMP avoids re-checking characters.
      Build a "failure function" that tells you how far to skip back.
TIME: O(n+m) where n = text length, m = pattern length
SPACE: O(m)
```

```javascript
function kmpSearch(text, pattern) {
  if (pattern.length === 0) return 0;

  // Step 1: Build failure function (partial match table)
  const fail = new Array(pattern.length).fill(0);
  let j = 0;
  for (let i = 1; i < pattern.length; i++) {
    while (j > 0 && pattern[i] !== pattern[j]) j = fail[j - 1];
    if (pattern[i] === pattern[j]) j++;
    fail[i] = j;
  }

  // Step 2: Search using failure function
  j = 0;
  for (let i = 0; i < text.length; i++) {
    while (j > 0 && text[i] !== pattern[j]) j = fail[j - 1];
    if (text[i] === pattern[j]) j++;
    if (j === pattern.length) {
      return i - pattern.length + 1; // found at this index
    }
  }
  return -1; // not found
}

kmpSearch("hello world", "world"); // 6 ✓
kmpSearch("aabaabaab", "aab");     // 0 ✓
// LeetCode: #28
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

---

## Complete Algorithm Map

```
┌──────────────────────────┬──────────────────────────────┬──────────────┐
│ Algorithm                │ What It Does                 │ Lives In     │
├──────────────────────────┼──────────────────────────────┼──────────────┤
│ Linear Search            │ Check every element           │ 🔍🪣🏆🔄     │
│ Binary Search            │ Halve sorted search space     │ 🔪           │
│ Hash Lookup              │ O(1) lookup via Set/Map       │ 🔎           │
│ Two Pointer Opp Ends     │ Compare from both ends        │ 📊           │
│ Two Pointer Same Dir R/W │ Modify array in-place         │ 📊           │
│ Fast Slow Pointer        │ Find middle, cycle entry      │ 📊           │
│ Dutch National Flag      │ Three-way partition           │ 📊           │
│ Sliding Window           │ Track contiguous window       │ 🪟           │
│ Bubble Sort              │ Swap adjacent — learning      │ Sorting      │
│ Selection Sort           │ Find min, place it — learning │ Sorting      │
│ Insertion Sort           │ Insert into sorted part       │ Sorting      │
│ Merge Sort               │ Divide, sort, merge           │ Sorting      │
│ Quick Sort               │ Pivot + partition             │ Sorting      │
│ Counting Sort            │ Count occurrences (fast)      │ Sorting      │
│ DFS (tree)               │ Go deep, then backtrack       │ 🌳           │
│ BFS (tree/graph)         │ Go level by level             │ 🌊           │
│ DFS (graph)              │ Explore all paths             │ 🌳           │
│ Kadane's                 │ Max subarray sum              │ 🪣           │
│ Floyd's Cycle Detection  │ Detect cycle — fast/slow      │ 📊           │
│ Dijkstra's               │ Shortest path (weighted)      │ 🏃 + Heap   │
│ Topological Sort         │ Task order with dependencies  │ 🌊 + Graph  │
│ Union-Find               │ Group connectivity            │ Specialist   │
│ Prefix Sum               │ Range sum in O(1)             │ Specialist   │
│ Monotonic Stack          │ Next greater/smaller element  │ 🏗️ Stack    │
│ Heap Operations          │ Min/max in O(log n)           │ ⛰️ Heap     │
│ KMP String Search        │ Pattern in string O(n+m)      │ String       │
│ Memoization              │ Cache recursive results       │ 🧩 DP       │
│ Tabulation               │ Bottom-up DP table            │ 🧩 DP       │
│ Backtracking             │ Choose-explore-unchoose       │ 🔙           │
│ Greedy Choice            │ Best local = best global      │ 🏃           │
└──────────────────────────┴──────────────────────────────┴──────────────┘
```

[↑ Back to Section C Navigation](#section-c--quick-navigation)

[↑ Back to Table of Contents](#table-of-contents)

---

# Section D: All Patterns

```
Level 1 (6 patterns)  → Solves 80%  → Days 1-14
Level 2 (5 patterns)  → Solves 15%  → Days 15-28
Level 3 (3 patterns)  → Solves 4%   → Days 29-38
Level 4 (8 patterns)  → Solves 1%   → Days 39-48
```

---

## Level 1 — Foundation (Solves 80% of problems)

---

### Pattern 1: 🔍 Detective (Boolean Validation)

```
USE WHEN:  You need to return true or false about a collection.
KEYWORDS:  "Is it...?", "Are all...?", "Does every...?", "Is valid?"
LOGIC:     Find ONE bad thing → return false immediately.
           Survive the entire loop → return true.
ALGORITHM: → Section C → Linear Search
TOOLS:     → Section A: Arrays or Strings

NOTE: Detective is the ONE exception to the positive framing rule.
      It deliberately looks for the BAD condition.
      The moment you find ONE bad thing → stop. Return false.
      This is called "early exit" and it is intentional.

DON'T USE WHEN:
  ✗ Need to COUNT bad things (use 🪣 Bucket)
  ✗ Need to COLLECT bad items (use 🏆 Filter)
  ✗ Answer is not true/false
  ✗ Checking if ONE value exists once → just use includes()
```

```javascript
function detective(arr) {
  // loop from 0 to arr.length - 1 (not arr.length)
  // because we compare arr[i] with arr[i+1]
  for (let i = 0; i < arr.length - 1; i++) {
    if (/* BAD CONDITION */) return false; // stop immediately
  }
  return true; // survived = true
}
```

**Example — "Is the array sorted?"**

```javascript
function isSorted(arr) {
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] > arr[i + 1]) return false; // found bad pair → false
  }
  return true; // no bad pairs found → true
}
```

```
Trace: [1, 5, 3]
  i=0: arr[0]=1, arr[1]=5 → 1 > 5? NO → continue
  i=1: arr[1]=5, arr[2]=3 → 5 > 3? YES → return false ✓

Trace: [1, 3, 5]
  i=0: 1 > 3? NO
  i=1: 3 > 5? NO
  Loop ends → return true ✓
```

```
Practice:
□ Is the array sorted?              □ Are all numbers positive?
□ Does string contain only digits?  □ Is it a valid email?
□ Is palindrome? (simple version)
LeetCode: #1550 Three Consecutive Odds, #896 Monotonic Array
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 2: 🪣 Bucket (Accumulation)

```
USE WHEN:  You need ONE value calculated from the entire list.
           (sum, max, min, count, product, longest, shortest)
KEYWORDS:  "Sum of...", "Total...", "Count...", "Maximum...",
           "Minimum...", "Longest...", "Product of..."
LOGIC:     Start with empty bucket.
           Pour each item into the bucket using some operation.
           Return the bucket.
ALGORITHM: → Section C → Linear Search + Kadane's Algorithm
TOOLS:     → Section A: Arrays or Strings

  Normal Bucket  → Linear Scan → touches ALL items equally
  Kadane's Bucket → Kadane's   → finds BEST contiguous subarray

DON'T USE WHEN:
  ✗ Need a list of results (use 🏆 Filter or 🔄 Transformer)
  ✗ Need true/false (use 🔍 Detective)
```

```javascript
function bucket(arr) {
  let result = /* starting value */;
  for (let i = 0; i < arr.length; i++) {
    result = /* combine result with arr[i] */;
  }
  return result;
}
```

```
STARTING VALUES — pick the right one:
  Sum         → result = 0          (adding to 0 gives first item)
  Count       → result = 0
  Maximum     → result = -Infinity  (any real number will be bigger)
  Minimum     → result = Infinity   (any real number will be smaller)
  Product     → result = 1          (multiplying by 1 gives first item)
  Longest     → result = 0

WHY -Infinity for max?
  If you start with 0 and all numbers are negative,
  you would incorrectly return 0.
  -Infinity ensures the first real number always replaces it.
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
  Start: biggest = -Infinity
  i=0: 3 > -Infinity → biggest = 3
  i=1: 7 > 3         → biggest = 7
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
// [-2,1,-3,4,-1,2,1,-5,4] → 6 ✓ (subarray [4,-1,2,1])
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
USE WHEN:  You need a SHORTER list — only the items that pass a test.
KEYWORDS:  "Get all...", "Find all...", "Remove...", "Keep only..."
LOGIC:     Start with empty winners list.
           Test each item. Pass the test → push to winners. Fail → skip.
ALGORITHM: → Section C → Linear Search
TOOLS:     → Section A: Arrays or Strings

DON'T USE WHEN:
  ✗ Need EVERY item changed (use 🔄 Transformer)
  ✗ Need one value (use 🪣 Bucket)
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
    if (arr[i] % 2 === 0) winners.push(arr[i]); // keep if even
  }
  return winners;
}
```

```
Trace: [1, 4, 3, 8]
  i=0: 1 % 2 = 1 → NOT even → skip   winners=[]
  i=1: 4 % 2 = 0 → even    → push    winners=[4]
  i=2: 3 % 2 = 1 → NOT even → skip   winners=[4]
  i=3: 8 % 2 = 0 → even    → push    winners=[4,8]
  Return: [4, 8] ✓
```

```
Practice:
□ Get all even numbers        □ Strings longer than 5 chars
□ Remove all negatives        □ Remove all falsy values
□ Get words starting with vowel
LeetCode: used in #217, #349
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 4: 🔄 Transformer (Data Projection)

```
USE WHEN:  You need a SAME-SIZE list where every item is changed.
KEYWORDS:  "Double each...", "Convert...", "Format...", "Extract...",
           "Calculate for each..."
LOGIC:     Start with empty result list.
           Transform each item.
           Push the transformed version into result.
ALGORITHM: → Section C → Linear Search
TOOLS:     → Section A: Arrays or Strings

DON'T USE WHEN:
  ✗ Some items should be removed (use 🏆 Filter)
  ✗ Need one value (use 🪣 Bucket)
```

```javascript
function transform(arr) {
  const result = [];
  for (let i = 0; i < arr.length; i++) {
    result.push(/* changed version of arr[i] */);
  }
  return result;
}
// Result is always SAME LENGTH as input
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
□ Double every number           □ Square every number
□ Celsius → Fahrenheit (×9/5+32) □ All strings to uppercase
□ Get length of each string     □ Extract first letter of each word
LeetCode: #977 Squares of Sorted Array
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 5: 🔎 Memory (Lookup / Existence)

```
USE WHEN:
  CASE 1 → "Have I seen this before?"
            Basic    → yes or no only            → SET
            Extended → how many times / details  → MAP (store count)

  CASE 2 → "Find a pair/relationship"
            Two different values that connect     → MAP (store index)

KEYWORDS:  "Duplicates", "Two Sum", "Frequency", "First unique",
           "Anagram", "Seen before"
ALGORITHM: → Section C → Hash-Based Search
TOOLS:     → Section A: Set
           → Section A: Map
LOGIC:     Set/Map = instant memory.
           Check the memory BEFORE processing each item.

HOW TO IDENTIFY WHICH CASE:
  "Does something exist or repeat?"    → Case 1 Basic    → SET
  "How many times does it appear?"     → Case 1 Extended → MAP count
  "Find two values that connect?"      → Case 2          → MAP index

DON'T USE WHEN:
  ✗ Data is already sorted (use 📊 Pointer Walker — more efficient)
  ✗ Just filtering by simple math (use 🏆 Filter)
  ✗ Checking if ONE value exists once (just use includes())
```

```javascript
// Template (Set) — Case 1 Basic: yes or no
function memorySet(arr) {
  const seen = new Set();
  for (let i = 0; i < arr.length; i++) {
    if (seen.has(arr[i])) { /* already seen this */ }
    seen.add(arr[i]); // always check FIRST, then add AFTER
  }
}

// Template (Map — count) — Case 1 Extended: how many times
function memoryMapCount(arr) {
  const freq = new Map();
  for (let i = 0; i < arr.length; i++) {
    freq.set(arr[i], (freq.get(arr[i]) || 0) + 1);
  }
  return freq;
}

// Template (Map — index) — Case 2: find pair that connects
function memoryMapIndex(arr, target) {
  const memory = new Map();
  for (let i = 0; i < arr.length; i++) {
    const complement = target - arr[i];
    if (memory.has(complement)) return [memory.get(complement), i];
    memory.set(arr[i], i); // store index for later lookup
  }
}
```

**Example — Remove Duplicates (Case 1 Basic → SET)**

```javascript
function removeDuplicates(arr) {
  const seen = new Set();
  const result = [];
  for (const item of arr) {
    if (!seen.has(item)) {  // keep if NOT seen before
      seen.add(item);
      result.push(item);
    }
  }
  return result;
}
// [1,3,2,3] → [1,3,2] ✓
```

**Example — Character Frequency (Case 1 Extended → MAP count)**

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

**Example — Are Two Strings Anagrams? (Case 1 Extended → MAP count)**

```javascript
function isAnagram(str1, str2) {
  if (str1.length !== str2.length) return false;

  const freq = new Map();

  // Build counts UP for str1
  for (const ch of str1) {
    freq.set(ch, (freq.get(ch) || 0) + 1);
  }

  // Check counts DOWN for str2
  for (const ch of str2) {
    if (!freq.get(ch)) return false; // char not seen or count = 0
    freq.set(ch, freq.get(ch) - 1); // subtract 1
  }

  return true;
}
// isAnagram("listen","silent") → true  ✓
// isAnagram("hello","world")   → false ✓
```

```
WHY SUBTRACT in anagram:
  str1 builds counts UP   → freq.set(ch, count + 1)
  str2 checks counts DOWN → freq.set(ch, count - 1)

  If we did NOT subtract:
    str1 = "aa", str2 = "bb"
    str1 builds: {a→2}
    str2 checks: is 'b' in freq? NO → return false ✓ correct here

  But without subtract, this would pass:
    str1 = "ab", str2 = "aa"
    str1 builds: {a→1, b→1}
    str2 checks: is 'a' in freq? YES → passes
                 is 'a' in freq? YES → passes again
    Returns true! WRONG.

  WITH subtract:
    str1 = "ab", str2 = "aa"
    str1 builds: {a→1, b→1}
    str2 checks: is 'a' > 0? YES → subtract → a→0
                 is 'a' > 0? NO  → return false ✓ CORRECT
```

**Example — Two Sum (Case 2 → MAP index)**

```javascript
function twoSum(nums, target) {
  const memory = new Map(); // value → index
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (memory.has(complement)) return [memory.get(complement), i];
    memory.set(nums[i], i);
  }
  return [];
}
// [2,7,11] target=9 → complement of 2 is 7 → [0,1] ✓
```

```
All Problems:

CASE 1 BASIC → SET (just yes/no)
  □ Does array have duplicates?       □ Are all values unique?
  □ Find first repeated character     □ Remove duplicates
  □ Intersection of two arrays        □ Union of two arrays
  □ Is this a subset?

CASE 1 EXTENDED → MAP count (how many times)
  □ Character frequency of string     □ Element frequency of array
  □ Are two strings anagrams? (#242)  □ First unique character (#387)
  □ Most frequent element             □ Elements appearing exactly twice
  □ Group anagrams together (#49)     □ Ransom note
  □ Top K Frequent Elements (#347)

CASE 2 → MAP index (find pair)
  □ Two Sum (#1)                      □ Difference of K
  □ Subarray sum equals K             □ Four Sum
  □ Longest Consecutive Sequence (#128)

LeetCode: #217, #242, #387, #1, #49, #128, #347
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 6: 📊 Pointer Walker (Two Pointers)

```
USE WHEN:  Compare elements with each other, sorted array problems,
           in-place modifications, linked list problems.
KEYWORDS:  "Palindrome", "Sorted array pair", "In-place removal",
           "Merge sorted", "Move elements", "Find middle"
LOGIC:     Two index variables (or node pointers) moving through data.
ALGORITHM: → Section C → Two Pointer (all 4 approaches)
                        → Floyd's Cycle Detection
                        → Fast Slow Pointer
                        → Dutch National Flag
TOOLS:     → Section A: Arrays or Strings
           → Section C: Sorting Algorithms
               JS Built-in Sort  ← use this 80% of the time
               Merge Sort        ← implement when interviewer asks
               Quick Sort        ← implement when interviewer asks
               Insertion Sort    ← use for nearly sorted data
               Bubble Sort       ← learning only
               Selection Sort    ← learning only

WHICH APPROACH TO USE:
  Palindrome, two sum sorted, container water → Approach 1: Opposite Ends
  Move zeroes, remove duplicates in-place     → Approach 2: Same Direction R/W
  Find middle, detect cycle (Linked List)     → Approach 3: Fast and Slow
  3Sum, 4Sum, multi-number sum                → Approach 4: Sorted Array

DON'T USE WHEN:
  ✗ Data unsorted and need pair lookup (use 🔎 Memory — O(n) vs O(n log n))
  ✗ Not comparing elements with EACH OTHER
```

```javascript
// Template A — Opposite Ends (palindrome, two sum sorted)
function oppositeEnds(arr) {
  let left = 0, right = arr.length - 1;
  while (left < right) {
    // compare arr[left] and arr[right]
    // move left++ or right-- based on result
  }
}

// Template B — Same Direction Read/Write (remove duplicates, move zeroes)
function sameDirection(arr) {
  let write = 0;
  for (let read = 0; read < arr.length; read++) {
    if (/* keep this element */) {
      arr[write] = arr[read];
      write++;
    }
  }
  return write; // new valid length
}

// Template C — Fast and Slow (find middle, detect cycle in linked list)
function fastSlow(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;       // 1 step
    fast = fast.next.next;  // 2 steps
  }
  return slow; // at middle when fast reaches end
}

// Template D — Two Pointers on Sorted Array (3Sum, 4Sum)
function sortedTwoPointer(arr, target) {
  arr.sort((a, b) => a - b); // sort FIRST
  let left = 0, right = arr.length - 1;
  while (left < right) {
    const sum = arr[left] + arr[right];
    if (sum === target) { /* found */ left++; right--; }
    else if (sum < target) left++;  // need bigger
    else right--;                   // need smaller
  }
}
```

**Example — "Is palindrome?" (Approach A — Opposite Ends)**

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
  left=0(r), right=6(r) → r === r → match → left=1, right=5
  left=1(a), right=5(a) → a === a → match → left=2, right=4
  left=2(c), right=4(c) → c === c → match → left=3, right=3
  left=3 is NOT < right=3 → loop ends → return true ✓
```

**Example — "Move Zeroes" (Approach B — Same Direction)**

```javascript
function moveZeroes(nums) {
  let write = 0;
  for (let read = 0; read < nums.length; read++) {
    if (nums[read] !== 0) {    // KEEP if NOT zero
      nums[write] = nums[read];
      write++;
    }
  }
  while (write < nums.length) { nums[write] = 0; write++; }
  return nums;
}
// [0,1,0,3,12] → [1,3,12,0,0] ✓
```

```
Practice:
□ Is palindrome (#125)             □ Reverse array in-place (#344)
□ Two Sum sorted (#167)            □ Remove duplicates sorted (#26)
□ Move zeroes (#283)               □ Merge sorted arrays (#88)
□ 3Sum (#15)                       □ Container With Most Water (#11)
□ Trapping Rain Water (#42)        □ Sort Colors (#75)
LeetCode: #125, #26, #167, #283, #88, #15, #11, #42, #75
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
USE WHEN:  Find the best/longest/shortest CONTIGUOUS subarray or substring.
KEYWORDS:  "Consecutive", "Subarray of size K", "Longest substring",
           "Shortest subarray with condition"
LOGIC:     Maintain a window of elements.
           Slide it: add new element at right, drop old element at left.
           Track the best window seen.
ALGORITHM: → Section C → Sliding Window Technique
TOOLS:     → Section A: Arrays or Strings
           → Section A: Set (for variable window — track unique chars)
           → Section A: Map (for variable window — track char counts)
BUILT ON:  📊 Pointer Walker + 🪣 Bucket

DON'T USE WHEN:
  ✗ Elements do not need to be contiguous
  ✗ You need a pair not a range
```

```javascript
// Fixed Window — window size k never changes
function fixedWindow(arr, k) {
  let windowSum = 0;
  for (let i = 0; i < k; i++) windowSum += arr[i]; // first window
  let best = windowSum;
  for (let i = k; i < arr.length; i++) {
    windowSum += arr[i] - arr[i - k]; // add new, remove old
    best = Math.max(best, windowSum);
  }
  return best;
}

// Variable Window — window grows/shrinks based on condition
function varWindow(s) {
  let left = 0, best = 0;
  const inWindow = new Set();
  for (let right = 0; right < s.length; right++) {
    while (inWindow.has(s[right])) { // shrink until valid
      inWindow.delete(s[left]);
      left++;
    }
    inWindow.add(s[right]);
    best = Math.max(best, right - left + 1);
  }
  return best;
}
```

```
Trace: [1,4,2,7,1] k=3 (fixed window, find max sum)
  First window [1,4,2] → sum = 7 → best = 7
  Slide: +arr[3]-arr[0] → +7-1 = 13 → best = 13
  Slide: +arr[4]-arr[1] → +1-4 = 10 → best = 13
  Return: 13 ✓
```

```
Practice:
□ Max sum subarray size K
□ Longest substring without repeating (#3)
□ Min subarray sum ≥ target (#209)
□ Min Window Substring (#76)
□ Sliding Window Maximum (#239)
LeetCode: #3, #209, #438, #567, #76, #239
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 8: 🔪 Binary Search

```
USE WHEN:  Find a target in SORTED data by eliminating half each step.
KEYWORDS:  "Sorted array", "Find position", "First/last occurrence",
           "Search in rotated"
LOGIC:     Check middle. Target smaller → search left half.
           Target bigger → search right half. Repeat.
ALGORITHM: → Section C → Binary Search Algorithm
TOOLS:     → Section A: Arrays
           → Section C: Sorting Algorithms
               JS Built-in Sort ← data must be sorted first
BUILT ON:  🔍 Detective (elimination logic)

DON'T USE WHEN:
  ✗ Data is NOT sorted and cannot be sorted
```

```javascript
function binarySearch(arr, target) {
  let lo = 0, hi = arr.length - 1;
  while (lo <= hi) {
    const mid = Math.floor((lo + hi) / 2);
    if (arr[mid] === target) return mid;       // found
    if (arr[mid] < target) lo = mid + 1;       // go right
    else hi = mid - 1;                         // go left
  }
  return -1; // not found
}
```

```
Trace: [2,5,8,12,16] target=12
  lo=0 hi=4 → mid=2 → arr[2]=8 → 8 < 12 → lo=3
  lo=3 hi=4 → mid=3 → arr[3]=12 → FOUND at index 3 ✓
```

```
Practice:
LeetCode: #704, #34, #33, #69, #162, #74
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 9: 🏗️ Stack Pattern

```
USE WHEN:  Matching pairs, undo history, Last-In-First-Out processing,
           "next greater element" type problems.
KEYWORDS:  "Valid parentheses", "Next greater", "Evaluate expression",
           "Largest rectangle"
LOGIC:     Push items onto stack. Pop when condition is met.
           What is currently on top determines next action.
ALGORITHM: → Section C → Stack-based Matching + Monotonic Stack
TOOLS:     → Section A: Stack

DON'T USE WHEN:
  ✗ Processing order does not matter
  ✗ You need FIFO → use Queue / BFS
```

```javascript
// Template A — Stack-based Matching (parentheses, pairs)
function stackMatching(input) {
  const stack = [];
  for (const item of input) {
    if (/* should push */) {
      stack.push(item);
    } else {
      const top = stack.pop();
      if (/* top does not match item */) return false;
    }
  }
  return stack.length === 0; // true only if all matched
}

// Template B — Monotonic Stack (next greater/smaller element)
function monotonicStack(arr) {
  const result = new Array(arr.length).fill(-1);
  const stack = []; // stores INDICES
  for (let i = 0; i < arr.length; i++) {
    while (stack.length > 0 && arr[i] > arr[stack[stack.length-1]]) {
      const idx = stack.pop();
      result[idx] = arr[i]; // arr[i] is the next greater for idx
    }
    stack.push(i);
  }
  return result;
}
```

**Example — "Valid Parentheses" (Template A)**

```javascript
function isValid(s) {
  const stack = [];
  const pairs = { ')': '(', ']': '[', '}': '{' };
  for (const ch of s) {
    if ('([{'.includes(ch)) stack.push(ch);     // push open bracket
    else if (stack.pop() !== pairs[ch]) return false; // check match
  }
  return stack.length === 0; // all brackets matched
}
```

```
Trace: "([])"
  ch='(' → push → stack=['(']
  ch='[' → push → stack=['(','[']
  ch=']' → pop '[' → pairs[']']='[' → '[' === '[' → match ✓
  ch=')' → pop '(' → pairs[')']='(' → '(' === '(' → match ✓
  stack is empty → return true ✓

Trace: "([)]"
  ch='(' → push → stack=['(']
  ch='[' → push → stack=['(','[']
  ch=')' → pop '[' → pairs[')']='(' → '[' !== '(' → return false ✓
```

```
Practice:
LeetCode: #20, #150, #739, #155, #84, #496
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 10: 🌳 Tree DFS

```
USE WHEN:  Explore every node in a tree by going deep first.
KEYWORDS:  "Max depth", "Path sum", "Invert tree", "Validate BST",
           "Diameter", "Lowest common ancestor"
LOGIC:     Base case (null) → Ask left subtree → Ask right subtree
           → Combine their answers.
ALGORITHM: → Section C → Tree Traversal DFS
TOOLS:     → Section A: Tree

DON'T USE WHEN:
  ✗ Need level-by-level processing (use 🌊 BFS)
  ✗ Need shortest path (use 🌊 BFS)
```

```javascript
// Universal DFS template — works for almost every tree problem
function treeDFS(node) {
  if (!node) return /* base value */; // base case — what to return for null

  const left = treeDFS(node.left);   // get answer from left subtree
  const right = treeDFS(node.right); // get answer from right subtree

  return /* combine left + right + node.val into one answer */;
}
```

**Example — "Max depth"**

```javascript
function maxDepth(node) {
  if (!node) return 0;  // null node contributes 0 depth
  const left = maxDepth(node.left);
  const right = maxDepth(node.right);
  return 1 + Math.max(left, right); // 1 for this node + deeper subtree
}
```

```
Trace:
       1
      / \
     2   3
    maxDepth(2) = 1 + max(0,0) = 1
    maxDepth(3) = 1 + max(0,0) = 1
    maxDepth(1) = 1 + max(1,1) = 2 ✓
```

```
Practice:
□ Max depth (#104)        □ Invert tree (#226)
□ Path sum (#112)         □ Validate BST (#98)
□ Same tree (#100)        □ Diameter (#543)
□ LCA (#236)              □ Max Path Sum (#124)
LeetCode: #104, #226, #112, #98, #100, #543, #236, #124
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 11: 🌊 BFS

```
USE WHEN:  Process a tree or graph level by level,
           or find the SHORTEST path.
KEYWORDS:  "Level order", "Shortest path", "Nearest neighbor",
           "Minimum steps", "Layer by layer"
LOGIC:     Use a queue. Add root/start.
           Process all nodes at current level, add their children.
           Repeat for next level.
ALGORITHM: → Section C → BFS Algorithm + Dijkstra's + Topological Sort
TOOLS:     → Section A: Queue
           → Section A: Graph

DON'T USE WHEN:
  ✗ Need to explore all paths to leaves (use 🌳 DFS)
  ✗ Tree height problems (use 🌳 DFS)
```

```javascript
// Template A — Level Order BFS (trees, level-by-level)
function levelOrderBFS(root) {
  if (!root) return [];
  const queue = [root];
  const result = [];
  while (queue.length > 0) {
    const size = queue.length; // number of nodes at this level
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

// Template B — Graph BFS with visited (shortest path, components)
function graphBFS(graph, start) {
  const visited = new Set([start]);
  const queue = [start];
  while (queue.length > 0) {
    const node = queue.shift();
    // process node here
    for (const neighbor of graph[node]) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push(neighbor);
      }
    }
  }
}

// Template C — Topological Sort (course prerequisites, build order)
function topoSort(numNodes, edges) {
  const graph = Array.from({ length: numNodes }, () => []);
  const inDegree = Array(numNodes).fill(0);
  for (const [from, to] of edges) { graph[from].push(to); inDegree[to]++; }
  const queue = [];
  for (let i = 0; i < numNodes; i++) if (inDegree[i] === 0) queue.push(i);
  const order = [];
  while (queue.length > 0) {
    const node = queue.shift();
    order.push(node);
    for (const nb of graph[node]) { if (--inDegree[nb] === 0) queue.push(nb); }
  }
  return order.length === numNodes ? order : []; // empty = cycle detected
}
```

```
Practice:
□ Level Order (#102)     □ Number of Islands (#200)
□ Rotting Oranges (#994) □ Word Ladder (#127)
□ Course Schedule (#207) □ Clone Graph (#133)
LeetCode: #102, #200, #994, #542, #127, #207, #133
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 3 — Advanced (Solves 4% more)

> **Prerequisite:** Level 2 patterns feel comfortable.

---

### Pattern 12: 🧩 Dynamic Programming

```
USE WHEN:  Problem has overlapping sub-problems AND optimal substructure.
           Overlapping = same smaller problem solved multiple times.
           Optimal = best overall answer built from best sub-answers.
KEYWORDS:  "How many ways?", "Minimum cost to reach", "Can I reach?",
           "Longest increasing", "Maximum profit"
LOGIC:     Store answers to smaller problems.
           Use stored answers to build up to the big answer.
           Never solve the same sub-problem twice.
ALGORITHM: → Section C → Memoization + Tabulation
TOOLS:     → Section A: Arrays
BUILT ON:  🪣 Bucket + 🔎 Memory + Recursion

3-Step Recipe — always follow this order:
  1. DEFINE:  "What does dp[i] mean in plain English?"
              Example: "dp[i] = number of ways to climb i stairs"
  2. RELATE:  "How does dp[i] connect to smaller values?"
              Example: "dp[i] = dp[i-1] + dp[i-2]"
              (1 step from i-1, or 2 steps from i-2)
  3. BASE:    "What are the smallest known values?"
              Example: "dp[0] = 1, dp[1] = 1"

DON'T USE WHEN:
  ✗ Sub-problems don't overlap (use Divide & Conquer)
  ✗ Greedy local choice is provably always optimal (use 🏃 Greedy)
  Tip: If unsure between DP and Greedy → default to DP. Always correct.
```

```javascript
// Space-optimized DP template (when only need last 2 values)
function dp(n) {
  let prev2 = /* dp[0] base case */;
  let prev1 = /* dp[1] base case */;
  for (let i = 2; i <= n; i++) {
    const curr = /* formula using prev1 and prev2 */;
    prev2 = prev1;
    prev1 = curr;
  }
  return prev1;
}
```

**Example — "Climbing Stairs" (#70)**

```javascript
// DEFINE:  dp[i] = number of ways to reach step i
// RELATE:  dp[i] = dp[i-1] + dp[i-2]
//          (came from step i-1 taking 1 step,
//           OR came from step i-2 taking 2 steps)
// BASE:    dp[0]=1 (one way to be at start), dp[1]=1 (one way)

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
  i=2: curr=1+1=2,  prev2=1, prev1=2
  i=3: curr=2+1=3,  prev2=2, prev1=3
  i=4: curr=3+2=5,  prev2=3, prev1=5
  i=5: curr=5+3=8,  prev2=5, prev1=8
  Return: 8 ✓ (ways: 1+1+1+1+1, 2+1+1+1, 1+2+1+1, 1+1+2+1, 1+1+1+2, 2+2+1, 2+1+2, 1+2+2)
```

```
Practice:
□ Climbing Stairs (#70)   □ House Robber (#198)
□ Coin Change (#322)      □ LIS (#300)
□ Word Break (#139)       □ Max Product Subarray (#152)
□ LCS (#1143)
LeetCode: #70, #198, #322, #300, #139, #152, #1143, #416
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 13: 🔙 Backtracking

```
USE WHEN:  Generate ALL valid combinations, permutations, or configurations.
KEYWORDS:  "All subsets", "All permutations", "N-Queens", "Sudoku",
           "Word search", "Generate all..."
LOGIC:     CHOOSE an option → EXPLORE that path fully →
           UN-CHOOSE (undo) → try next option.
           Always in this exact order.
ALGORITHM: → Section C → Recursive Backtracking
TOOLS:     → Section A: Arrays or Strings
BUILT ON:  🌳 DFS + trial-and-error

WHY UN-CHOOSE MATTERS:
  If you don't undo your choice, the next path starts
  with leftover state from the previous path.
  This corrupts all subsequent results.
  UN-CHOOSE restores the state before trying next option.
```

```javascript
function backtrack(result, current, choices, start) {
  result.push([...current]); // save a COPY of current state

  for (let i = start; i < choices.length; i++) {
    current.push(choices[i]);                    // CHOOSE
    backtrack(result, current, choices, i + 1);  // EXPLORE
    current.pop();                               // UN-CHOOSE (undo)
  }
}

// Call it:
const result = [];
backtrack(result, [], choices, 0);
```

**Example — "All subsets of [1,2,3]"**

```javascript
function subsets(nums) {
  const result = [];
  function bt(start, current) {
    result.push([...current]); // [] is also a valid subset
    for (let i = start; i < nums.length; i++) {
      current.push(nums[i]); // CHOOSE
      bt(i + 1, current);    // EXPLORE
      current.pop();         // UN-CHOOSE
    }
  }
  bt(0, []);
  return result;
}
// [1,2,3] → [[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]]
```

```
Practice:
□ All subsets (#78)      □ All permutations (#46)
□ Combination Sum (#39)  □ Word Search (#79)
□ N-Queens (#51)         □ Palindrome Partitioning (#131)
LeetCode: #78, #46, #39, #79, #51, #131, #17
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 14: 🏃 Greedy

```
USE WHEN:  The best LOCAL choice at each step leads to the best GLOBAL result.
KEYWORDS:  "Maximum events attended", "Minimum stops", "Jump game",
           "Schedule tasks", "Minimum intervals"
LOGIC:     Sort the data (almost always).
           Pick the best available option right now.
           Never go back. Never reconsider.
ALGORITHM: → Section C → Greedy Choice
TOOLS:     → Section A: Arrays
           → Section C: Sorting Algorithms
               JS Built-in Sort  ← use this 80% of the time
               Merge Sort        ← implement when interviewer asks
               Quick Sort        ← implement when interviewer asks
               Insertion Sort    ← use for nearly sorted data
               Bubble Sort       ← learning only
               Selection Sort    ← learning only

WHY GREEDY WORKS (when it works):
  Some problems have a special property:
  The locally optimal choice at every step
  happens to also be globally optimal.
  Example: always attending the earliest-ending meeting
  leaves the most time for future meetings.

WHY GREEDY FAILS (when it fails):
  When local best ≠ global best.
  Example: coin change with unusual denominations.
  If coins = [1, 3, 4], target = 6:
  Greedy picks 4 first → then 1+1 → 3 coins.
  But optimal is 3+3 → 2 coins.
  → For coin change: use DP, not Greedy.

RULE: If unsure → use DP. DP is always correct.
      Only use Greedy when you can PROVE local = global.

DON'T USE WHEN:
  ✗ Local best ≠ global best (use 🧩 DP)
```

```javascript
// Example: Jump Game — can you reach the last index?
function canJump(nums) {
  let maxReach = 0;
  for (let i = 0; i < nums.length; i++) {
    if (i > maxReach) return false;            // can't reach here → stuck
    maxReach = Math.max(maxReach, i + nums[i]); // update furthest reachable
  }
  return true;
}
```

```
Trace: [2, 3, 1, 1, 4]
  i=0: 0 ≤ maxReach=0 → ok → maxReach=max(0,0+2)=2
  i=1: 1 ≤ maxReach=2 → ok → maxReach=max(2,1+3)=4
  i=2: 2 ≤ maxReach=4 → ok → maxReach=max(4,2+1)=4
  i=3: 3 ≤ maxReach=4 → ok → maxReach=max(4,3+1)=4
  i=4: 4 ≤ maxReach=4 → ok → reached end → return true ✓

Trace: [3, 2, 1, 0, 4]
  i=0: maxReach=3
  i=1: maxReach=3
  i=2: maxReach=3
  i=3: maxReach=3
  i=4: 4 > maxReach=3 → return false ✓
```

```
Practice:
□ Jump Game (#55)             □ Buy/Sell Stock (#121)
□ Non-overlapping Int (#435)  □ Jump Game II (#45)
□ Gas Station (#134)          □ Task Scheduler (#621)
LeetCode: #55, #121, #435, #455, #45, #134, #621
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 4 — Specialist (Solves remaining 1%)

> Learn these in Days 39-48. Each has its own algorithm in Section C.

### ⛰️ Heap / Priority Queue — Pattern 15

```
USE WHEN:  "Kth largest", "Top K frequent", "Running median"
           Any time you need repeated min/max access efficiently.
ALGORITHM: → Section C → Heap Operations
           → Section A: Heap (data structure)
TIME:      O(log n) insert/extract, O(1) peek
LeetCode:  #215, #347, #295, #373
```

### 📚 Trie (Prefix Tree) — Pattern 16

```
USE WHEN:  "Autocomplete", "Spell check", "Starts with prefix",
           "Word search in dictionary"
HOW IT WORKS:
  A tree where each NODE is a CHARACTER.
  Root → first letters. Each path = a word.
  "cat" and "car" share the path c→a, then split.
TIME:  O(m) per operation where m = word length
```

```javascript
class TrieNode {
  constructor() {
    this.children = {};
    this.isEnd = false;
  }
}

class Trie {
  constructor() { this.root = new TrieNode(); }

  insert(word) {
    let node = this.root;
    for (const ch of word) {
      if (!node.children[ch]) node.children[ch] = new TrieNode();
      node = node.children[ch];
    }
    node.isEnd = true;
  }

  search(word) {
    let node = this.root;
    for (const ch of word) {
      if (!node.children[ch]) return false;
      node = node.children[ch];
    }
    return node.isEnd;
  }

  startsWith(prefix) {
    let node = this.root;
    for (const ch of prefix) {
      if (!node.children[ch]) return false;
      node = node.children[ch];
    }
    return true;
  }
}
// LeetCode: #208, #211, #212
```

### Prefix Sum — Pattern 17

```
USE WHEN:  "Sum of range [i..j]", "Subarray sum equals K"
ALGORITHM: → Section C → Prefix Sum
TIME:      O(n) build, O(1) query
LeetCode:  #303, #560, #238
```

### Merge Intervals — Pattern 18

```
USE WHEN:  "Overlapping meetings", "Merge time ranges"
APPROACH:  Sort by start time. Merge if current start ≤ prev end.
TIME:      O(n log n)
LeetCode:  #56, #57, #252
```

### Union-Find — Pattern 19

```
USE WHEN:  "Are nodes connected?", "Number of connected components"
ALGORITHM: → Section C → Union-Find
TIME:      Nearly O(1) per operation
LeetCode:  #547, #684, #323
```

### Topological Sort — Pattern 20

```
USE WHEN:  "Course prerequisites", "Build order", "Task scheduling"
ALGORITHM: → Section C → Topological Sort
TIME:      O(V+E)
LeetCode:  #207, #210, #269
```

### Bit Manipulation — Pattern 21

```
USE WHEN:  "Single number (XOR)", "Power of 2", "Count set bits"
KEY TRICKS:
  a XOR a = 0           (number XOR itself = 0)
  a XOR 0 = a           (number XOR 0 = number)
  n & (n-1) clears LSB  (check if power of 2: n & (n-1) === 0)
TIME:      O(1) or O(log n)
LeetCode:  #136, #191, #338, #268
```

### Monotonic Stack — Pattern 22

```
USE WHEN:  "Next greater element", "Next smaller element",
           "Largest rectangle in histogram", "Trapping rain water"
ALGORITHM: → Section C → Monotonic Stack
TIME:      O(n)
LeetCode:  #496, #503, #84, #739, #42
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section E: Pattern Combos

> Most real interview problems use 2 patterns together.
> Recognizing the combo is a skill you build over time.

---

### Combo 1: 🔎 Memory + 🏆 Filter → "Remove Duplicates While Keeping Order"

```
WHY TWO PATTERNS?
  🔎 Memory answers: "Have I seen this item before?"
  🏆 Filter answers: "Should I keep this item?"
  Together: "Keep only items I haven't seen before."
  Memory alone doesn't produce a list.
  Filter alone doesn't know what was seen.
  Together they solve it perfectly.
```

```javascript
function removeDuplicates(arr) {
  const seen = new Set();       // 🔎 Memory — tracks what was seen
  const result = [];            // 🏆 Filter — collects keepers
  for (const item of arr) {
    if (!seen.has(item)) {      // not seen before → keep
      seen.add(item);
      result.push(item);
    }
  }
  return result;
}
// [1,3,2,3,1] → [1,3,2] ✓
// LeetCode: #26, #217
```

---

### Combo 2: 🔎 Memory + 🔍 Detective → "Contains Duplicate Check"

```
WHY TWO PATTERNS?
  🔎 Memory answers: "Have I seen this before?"
  🔍 Detective answers: "Is this true or false?"
  Together: "Return true the moment I see something twice."
  Detective alone has no fast lookup.
  Memory alone doesn't produce true/false.
  Together they give early exit + fast lookup.
```

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

### Combo 3: 🪟 Sliding Window + 🔎 Memory → "Longest Substring Without Repeating"

```
WHY TWO PATTERNS?
  🪟 Sliding Window tracks the current window boundaries.
  🔎 Memory tracks which characters are currently IN the window.
  Together: "Slide the window. Shrink it when a repeat enters."
  Window alone cannot check if a character is already inside.
  Memory alone cannot track a contiguous range.
```

```javascript
function lengthOfLongestSubstring(s) {
  const inWindow = new Set();   // 🔎 Memory — what's in current window
  let left = 0, best = 0;      // 🪟 Window boundaries

  for (let right = 0; right < s.length; right++) {
    while (inWindow.has(s[right])) { // duplicate found — shrink window
      inWindow.delete(s[left]);
      left++;
    }
    inWindow.add(s[right]);
    best = Math.max(best, right - left + 1);
  }
  return best;
}
// "abcabcbb" → 3 ("abc") ✓
// LeetCode: #3
```

---

### Combo 4: 🏗️ Stack + 🔍 Detective → "Valid Parentheses"

```
WHY TWO PATTERNS?
  🏗️ Stack remembers previously seen opening brackets (LIFO order).
  🔍 Detective checks: does this closing bracket match what's on top?
  Together: "Track open brackets. Return false the moment a mismatch found."
  Stack alone doesn't produce true/false.
  Detective alone has no memory of what opened.
```

```javascript
function isValid(s) {
  const stack = [];             // 🏗️ Stack — remembers open brackets
  const pairs = { ')': '(', ']': '[', '}': '{' };
  for (const ch of s) {
    if ('([{'.includes(ch)) stack.push(ch);
    else if (stack.pop() !== pairs[ch]) return false; // 🔍 Detective
  }
  return stack.length === 0;
}
// "([])" → true  ✓
// "([)]" → false ✓
// LeetCode: #20
```

---

### Combo 5: 🪣 Bucket + 📊 Pointer Walker → "Container With Most Water"

```
WHY TWO PATTERNS?
  📊 Pointer Walker moves two pointers from both ends.
  🪣 Bucket tracks the best (maximum) area seen so far.
  Together: "Compare all container candidates. Keep the best."
  Pointer Walker alone doesn't track max.
  Bucket alone doesn't know which pairs to compare.
```

```javascript
function maxArea(height) {
  let left = 0, right = height.length - 1;  // 📊 Pointers from both ends
  let best = 0;                              // 🪣 Bucket for max area

  while (left < right) {
    const area = Math.min(height[left], height[right]) * (right - left);
    best = Math.max(best, area);
    if (height[left] < height[right]) left++;  // move shorter wall
    else right--;
  }
  return best;
}
// LeetCode: #11
```

---

```
Quick Combo Reference:
  Remove items based on lookup     → 🔎 Memory + 🏆 Filter
  Fast existence check             → 🔎 Memory + 🔍 Detective
  Best contiguous + condition      → 🪟 Window + 🔎 Memory
  Matching/nesting + validation    → 🏗️ Stack + 🔍 Detective
  Sorted array optimization        → 📊 Pointer Walker + 🪣 Bucket
  Tree path problems               → 🌳 DFS + 🪣 Bucket
  Grid shortest path               → 🌊 BFS + 🔎 Memory
  Generate + filter valid configs  → 🔙 Backtracking + 🔍 Detective
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section F: Big-O Cheat Sheet

## What Is Big-O? (Plain English)

```
Big-O tells you ONE thing:

  "How does my code SLOW DOWN as input gets BIGGER?"

It is a measurement of speed.
Not the actual time in seconds.
How time GROWS as n grows.

EXAMPLE:
  n = 100 items → your code takes 0.001 seconds
  n = 1,000 items → how long now?

  O(n)  → 10× more items = 10× more time    = 0.01 seconds
  O(n²) → 10× more items = 100× more time   = 0.1 seconds
  O(1)  → 10× more items = SAME time        = 0.001 seconds

WHY EVERY SOFTWARE ENGINEER NEEDS THIS:
  Two solutions both work correctly for 100 items.
  In production your app gets 1,000,000 items.
  One solution takes 1 second.
  The other takes 11 days.
  Big-O tells you BEFORE you build which one will survive.
```

---

## Common Complexities Ranked

```
FASTEST → SLOWEST

O(1)         Constant      Instant. Same speed always.
             Example: arr[0], set.has(x), map.get(x)

O(log n)     Logarithmic   Very fast. Halves work each step.
             Example: binary search
             1,000,000 items → only ~20 steps

O(n)         Linear        One loop through all items.
             Example: find max, sum array
             Best you can often get for array problems.

O(n log n)   Linearithmic  Slightly worse than linear. Sorting.
             Example: merge sort, quick sort

O(n²)        Quadratic     Loop inside a loop.
             Example: bubble sort, brute force pair check
             10,000 items → 100,000,000 operations. Slow.

O(2^n)       Exponential   Doubles with each new item.
             Example: generating all subsets
             30 items → over 1 billion operations.

O(n!)        Factorial     All permutations.
             Example: travelling salesman brute force
             12 items → 479 million operations. Maximum practical.
```

---

## Best Case vs Worst Case

```
BEST CASE:   The lucky scenario. Input is ideal.
WORST CASE:  The unlucky scenario. Input is the hardest possible.
AVERAGE CASE: What happens with typical random input.

EXAMPLE: Is array sorted? (Detective pattern)
  Input: [5, 1, 2, 3, 4] — bad pair at position 0
  BEST CASE:  Check arr[0] > arr[1] → YES → return false immediately
              Only 1 comparison. O(1).

  Input: [1, 2, 3, 4, 5] — perfectly sorted
  WORST CASE: Check every pair, find no bad pair, return true
              n-1 comparisons. O(n).

WHY WORST CASE MATTERS MORE:
  You build software for REAL users.
  Real users give you ALL kinds of input.
  You cannot guarantee best case.
  You must plan for worst case.

  If your solution is O(1) best and O(n²) worst:
  You quote O(n²) — because that's what can actually happen.

  We say: "This algorithm is O(n)" meaning worst case is O(n).
```

---

## Practical Limits

```
O(n)       → n up to 10,000,000  ✅ fast
O(n log n) → n up to 1,000,000   ✅ fast
O(n²)      → n up to 10,000      ⚠️ starts getting slow
O(2^n)     → n up to 25          ⚠️ small inputs only
O(n!)      → n up to 12          🛑 very small inputs only
```

---

## Every Pattern's Complexity

```
┌──────────────────────────────┬────────────────┬──────────────┐
│ Pattern / Algorithm          │ Time           │ Space        │
├──────────────────────────────┼────────────────┼──────────────┤
│ 🔍 Detective                  │ O(n)           │ O(1)         │
│ 🪣 Bucket                     │ O(n)           │ O(1)         │
│ 🏆 Filter                     │ O(n)           │ O(n)         │
│ 🔄 Transformer                │ O(n)           │ O(n)         │
│ 🔎 Memory (Set/Map)           │ O(n)           │ O(n)         │
│ 📊 Pointer Walker             │ O(n)           │ O(1)         │
│ 🪟 Sliding Window             │ O(n)           │ O(1)*        │
│ 🔪 Binary Search              │ O(log n)       │ O(1)         │
│ 🏗️ Stack                      │ O(n)           │ O(n)         │
│ 🌳 Tree DFS                   │ O(n)           │ O(h)**       │
│ 🌊 BFS                        │ O(n)           │ O(w)***      │
│ 🧩 DP                         │ O(n)–O(n²)     │ O(n)         │
│ 🔙 Backtracking               │ O(2^n)         │ O(n)         │
│ 🏃 Greedy                     │ O(n log n)     │ O(1)         │
│ ─────────────────────────────│────────────────│──────────────│
│ Prefix Sum                   │ O(n)+O(1)query │ O(n)         │
│ Monotonic Stack               │ O(n)           │ O(n)         │
│ Fast Slow Pointer             │ O(n)           │ O(1)         │
│ Dutch National Flag           │ O(n)           │ O(1)         │
│ Counting Sort                 │ O(n+k)         │ O(k)         │
│ Heap insert/extract           │ O(log n)       │ O(n)         │
│ Heap peek                     │ O(1)           │ O(n)         │
│ KMP String Search             │ O(n+m)         │ O(m)         │
└──────────────────────────────┴────────────────┴──────────────┘

Notes:
  *   Sliding Window: O(k) space if window tracks Set/Map with k items
  **  Tree DFS: O(h) where h = height. Balanced → O(log n). Worst → O(n)
  *** BFS: O(w) where w = max width of tree
      k in Counting Sort = range of values
      m in KMP = pattern length
```

---

## Data Structure Operations

```
┌───────────────┬──────────┬──────────┬──────────┬──────────────┐
│ Operation     │ Array    │ Set      │ Map      │ Heap         │
├───────────────┼──────────┼──────────┼──────────┼──────────────┤
│ Access [i]    │ O(1) ✅  │ N/A      │ N/A      │ O(1) peek   │
│ Search/has    │ O(n) ⚠️  │ O(1) ✅  │ O(1) ✅  │ O(n)         │
│ Add/insert    │ O(1)*    │ O(1)     │ O(1)     │ O(log n)     │
│ Delete        │ O(n)     │ O(1)     │ O(1)     │ O(log n)     │
│ Sort          │O(n log n)│ N/A      │ N/A      │ N/A          │
└───────────────┴──────────┴──────────┴──────────┴──────────────┘
* Array push to end O(1), unshift to front O(n)

THE KEY INSIGHT:
  Array.includes(x) → loops through every item → O(n) SLOW
  Set.has(x)        → jumps straight to answer → O(1) FAST

  If you check membership in a loop:
    BAD:  for each item → arr.includes(item) → O(n²) total
    GOOD: build Set first → for each item → set.has(item) → O(n) total
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section G: How Patterns Connect

```
Every advanced pattern is BUILT FROM Level 1 patterns.
They are not separate skills. They are extensions.

🪟 Sliding Window    = 📊 Pointer Walker (two indices)
                     + 🪣 Bucket (track window total)

🔪 Binary Search     = 🔍 Detective (eliminate half each step)

🏗️ Stack Pattern     = 🔍 Detective (find mismatch → return false)
                     + LIFO data structure

🌳 Tree DFS          = 🪣 Bucket (accumulate answer while recursing)
                     + Recursion

🌊 BFS               = 🪣 Bucket (accumulate level results)
                     + Queue (process in order)

🧩 DP                = 🔎 Memory (cache sub-problem answers)
                     + 🪣 Bucket (build up to final answer)
                     + Recursion (break into sub-problems)

🔙 Backtracking      = 🌳 DFS (go deep, then backtrack)
                     + Trial-and-error (choose, explore, undo)

🏃 Greedy            = 🪣 Bucket (accumulate best choices)
                     + Sorting (to know what "best" is)

WHAT THIS MEANS PRACTICALLY:
  If Level 1 is weak → everything above breaks.
  You cannot do Sliding Window if you don't understand Pointer Walker.
  You cannot do DP if you don't understand Memory (Set/Map).
  You cannot do BFS if you don't understand Bucket.

  Before moving to Level 2:
  Pick any Level 1 pattern in under 30 seconds.
  Solve a Level 1 problem in under 10 minutes.
  Then and only then move to Level 2.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section H: Key Insights

```
 #1: If you can't say it in English, you can't code it.
     The English IS the solution. Code is just translation.

 #2: Brute force FIRST. Optimize SECOND.
     A working O(n²) solution is better than a broken O(n) attempt.

 #3: The trace table never lies.
     If the table gives the right answer, the logic is correct.
     Code a correct logic — not a "feels right" logic.

 #4: Every problem is one of 22 patterns wearing a costume.
     The costume is the story. The pattern is the structure.
     Strip the costume → reveal the pattern.

 #5: You learn patterns by solving 3-5 problems with each one.
     Reading is not learning. Solving is learning.

 #6: Code is the LAST step.
     Hard coding = skipping phases 1-4 = writing untested logic.

 #7: Data structure choice is half the solution.
     Array.includes() = O(n). Set.has() = O(1).
     That one swap changes O(n²) into O(n).

 #8: "Replace linear scan with hash lookup" solves dozens of problems.
     Whenever you see: "for each item, check if it exists in list"
     Replace the list with a Set. Complexity drops immediately.

 #9: Working brute force > broken optimal.
     Interviewers prefer a correct O(n²) over an incorrect O(n).

#10: Patterns COMBINE. Most real problems use 2 patterns together.
     Section E shows you the 5 most common combos.

#11: The #1 optimization: nested loop → single loop + Set/Map.
     Any time you see two loops, ask: "can the inner loop become a Set lookup?"

#12: "Contiguous subarray" + "max/min" = Sliding Window. Every time.

#13: Binary search works on ANSWERS, not just sorted arrays.
     "Find minimum valid X" → binary search on the answer range.

#14: Every tree problem: base case → left → right → combine.
     The skeleton never changes. Only the combination changes.

#15: DFS = "explore all paths." BFS = "shortest path / level by level."
     Wrong choice here wastes significant time.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section I: 53-Day Practice Roadmap

## How to Use the Roadmap

```
EACH DAY HAS THIS STRUCTURE:

  PREPARE A → open Section A → read that data structure
  PREPARE C → open Section C → read that algorithm

  SOLVE     → one problem → one pattern → LeetCode number
  TIME      → time complexity
  SPACE     → space complexity

  Apply all 6 phases from Section B:
    PHASE 1 → DECOMPOSE   (5 questions)
    PHASE 2 → IDENTIFY    (pattern + algorithm)
    PHASE 3 → PSEUDO-CODE (English first)
    PHASE 4 → TRACE TABLE (prove on paper)
    PHASE 5 → CODE        (translate English)
    PHASE 6 → EDGE CASES + BIG-O

TOTAL TIME: 25 minutes per day maximum.
One problem. Done. Close it.

PREP DAYS:
  Some days are reading-only. No problem solving.
  READ the data structure or algorithm.
  Understand the code. Trace through examples.
  Close blueprint. Prep day done. 15 minutes maximum.
  Tomorrow you will USE what you read today.
```

---

## Prep Day Structure

```
WHAT TO DO ON A PREP DAY:

  STEP 1 → Open Section A: [today's data structure]
           Read the WHAT and WHY.
           Read the code. Run examples in your head.

  STEP 2 → Open Section C: [today's algorithm]
           Read the algorithm.
           Trace through the example step by step.

  STEP 3 → Close the blueprint.
           15 minutes maximum. Done.

  ✅ You do NOT solve a problem on a prep day.
  ✅ Tomorrow's problem will use what you read today.
```

---

## Complex Patterns and Algorithms Take Multiple Days

```
This is normal. Not a failure.

Memory pattern      → 3 days   Sliding Window  → 2 days
Stack               → 2 days   Tree DFS        → 2 days
BFS                 → 2 days   DP              → 3 days
Backtracking        → 2 days   Greedy          → 2 days
Prefix Sum          → 2 days   Monotonic Stack → 2 days
Fast Slow Pointer   → 2 days   Heap            → 2 days

One problem per day. Not one pattern per day.
25 minutes. Done. Close it.
```

---

## Days 1-14: Level 1 Patterns

| Day | Prepare A | Prepare C | Solve | Pattern | Time | Space | LeetCode |
|-----|-----------|-----------|-------|---------|------|-------|----------|
| 1 | Arrays | Linear Search | Sum of array | 🪣 Bucket | O(n) | O(1) | — |
| 2 | Arrays | Linear Search | Is array sorted? | 🔍 Detective | O(n) | O(1) | #896 |
| 3 | Arrays | Linear Search | Get all even numbers | 🏆 Filter | O(n) | O(n) | — |
| 4 | Arrays | Linear Search | Double every number | 🔄 Transformer | O(n) | O(n) | — |
| 5 | Set + Map | Hash-Based Search | Has duplicates? | 🔎 Memory Case 1 Basic | O(n) | O(n) | #217 |
| 6 | Map | Hash-Based Search | Character frequency | 🔎 Memory Case 1 Ext | O(n) | O(n) | #387 |
| 7 | Map | Hash-Based Search | Two Sum | 🔎 Memory Case 2 | O(n) | O(n) | #1 |
| 8 | Arrays | JS Built-in Sort | Sort array of numbers | Sorting Tool | O(n log n) | O(n) | — |
| 9 | Strings | Two Pointer Approaches | Is palindrome? | 📊 Pointer Walker | O(n) | O(1) | #125 |
| 10 | Arrays | Linear Search | Find maximum | 🪣 Bucket | O(n) | O(1) | — |
| 11 | Arrays | Linear Search | All numbers positive? | 🔍 Detective | O(n) | O(1) | — |
| 12 | Arrays | Linear Search | Remove falsy values | 🏆 Filter | O(n) | O(n) | — |
| 13 | Arrays | Linear Search | Celsius → Fahrenheit | 🔄 Transformer | O(n) | O(n) | — |
| 14 | Arrays | Two Pointer Approaches | Reverse array in-place | 📊 Pointer Walker | O(n) | O(1) | #344 |

---

## Days 15-28: Level 2 Patterns

| Day | Prepare A | Prepare C | Solve | Pattern | Time | Space | LeetCode |
|-----|-----------|-----------|-------|---------|------|-------|----------|
| 15 | Arrays + Set | Hash-Based Search | Remove duplicates (combo) | 🏆 + 🔎 | O(n) | O(n) | #26 |
| 16 | Arrays | Sliding Window | Max sum K consecutive | 🪟 Sliding Window | O(n) | O(1) | — |
| 17 | Strings + Set | Sliding Window | Longest substr no repeat | 🪟 + 🔎 | O(n) | O(k) | #3 |
| 18 | Arrays | Binary Search | Binary search sorted | 🔪 Binary Search | O(log n) | O(1) | #704 |
| 19 | **READ Stack+Queue** | Stack-based Matching | — PREP DAY — | — | — | — | — |
| 20 | Stack | Stack-based Matching | Valid parentheses | 🏗️ Stack | O(n) | O(n) | #20 |
| 21 | **READ Tree** | Tree Traversal DFS | Daily temperatures | 🏗️ Stack | O(n) | O(n) | #739 |
| 22 | Tree | Tree Traversal DFS | Max depth binary tree | 🌳 Tree DFS | O(n) | O(h) | #104 |
| 23 | **READ Graph** | BFS Algorithm | Invert binary tree | 🌳 Tree DFS | O(n) | O(h) | #226 |
| 24 | Graph + Queue | BFS Algorithm | Level order traversal | 🌊 BFS | O(n) | O(w) | #102 |
| 25 | Graph | BFS Algorithm | Number of islands | 🌊 BFS | O(m×n) | O(m×n) | #200 |
| 26 | Arrays | Binary Search | First occurrence in sorted | 🔪 Binary Search | O(log n) | O(1) | #34 |
| 27 | Arrays | Sliding Window | Min subarray sum ≥ target | 🪟 Sliding Window | O(n) | O(1) | #209 |
| 28 | Tree | Tree Traversal DFS | Path sum | 🌳 Tree DFS | O(n) | O(h) | #112 |

---

## Days 29-38: Level 3 Patterns

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

---

## Days 39-48: New Algorithms (Level 4)

| Day | Prepare A | Prepare C | Solve | Algorithm | Time | Space | LeetCode |
|-----|-----------|-----------|-------|-----------|------|-------|----------|
| 39 | Arrays | Prefix Sum | Range sum query | Prefix Sum Day 1 | O(n)+O(1) | O(n) | #303 |
| 40 | Arrays | Prefix Sum | Subarray sum equals K | Prefix Sum Day 2 | O(n) | O(n) | #560 |
| 41 | Arrays | Monotonic Stack | Next greater element | Monotonic Stack Day 1 | O(n) | O(n) | #496 |
| 42 | **READ Linked List** | Monotonic Stack | Daily temperatures | Monotonic Stack Day 2 | O(n) | O(n) | #739 |
| 43 | Linked List | Fast Slow Pointer | Find middle of list | Fast Slow Pointer Day 1 | O(n) | O(1) | #876 |
| 44 | Linked List | Fast Slow Pointer | Linked list cycle II | Fast Slow Pointer Day 2 | O(n) | O(1) | #142 |
| 45 | Arrays | Dutch National Flag | Sort colors | Dutch National Flag | O(n) | O(1) | #75 |
| 46 | **READ Heap + Deque** | Heap Operations | — PREP DAY — | — | — | — | — |
| 47 | Heap | Heap Operations | Kth largest element | ⛰️ Heap Day 1 | O(n log k) | O(k) | #215 |
| 48 | Heap | Heap Operations | Top K frequent elements | ⛰️ Heap Day 2 | O(n log k) | O(n) | #347 |

---

## Days 49-53: Interview Prep — Sorting Algorithms

```
These days are NOT for daily problem solving.
They are for when an interviewer says:
  "Can you implement a sorting algorithm from scratch?"

Learn once. Know them. Answer confidently.
Do NOT try to learn these before Day 49.
They are not needed for solving daily problems.
JS Built-in Sort handles everything before Day 49.
```

| Day | Prepare C | Solve | Algorithm | Time | Space |
|-----|-----------|-------|-----------|------|-------|
| 49 | Bubble Sort | Implement from scratch | Bubble Sort | O(n²) | O(1) |
| 50 | Selection Sort | Implement from scratch | Selection Sort | O(n²) | O(1) |
| 51 | Insertion Sort | Implement from scratch | Insertion Sort | O(n²) worst O(n) best | O(1) |
| 52 | Merge Sort | Implement from scratch | Merge Sort | O(n log n) | O(n) |
| 53 | Quick Sort | Implement from scratch | Quick Sort | O(n log n) avg | O(log n) |

---

## Day 54+: Open Practice

```
HOW TO PICK PROBLEMS:
  Go to LeetCode.
  Filter by: Easy first, then Medium.
  Pick any problem tagged with a pattern you have learned.

HOW TO SOLVE:
  Do NOT look at hints.
  Apply all 6 phases. Phase 4 trace table is mandatory.
  Give yourself 15 minutes maximum.
  If stuck after 15 minutes → look at the pattern hint only.
  If still stuck → read solution, understand it, solve it yourself.

WHEN YOU FEEL STUCK:
  Open Section D → find the pattern → re-read the template.
  Open Section C → re-read the algorithm.
  Do NOT skip to the solution immediately.

GOAL: Identify the pattern in under 2 minutes.
      Code the solution in under 15 minutes.
```

---

## Day Structure — Full Example (Day 1)

```
PREPARE A → Section A: Arrays
PREPARE C → Section C: Linear Search

SOLVE  → Sum of array → 🪣 Bucket → no LeetCode number
TIME   → O(n)
SPACE  → O(1)

PHASE 1 → DECOMPOSE
  Input     → array of numbers, can be empty
  Output    → one number (the total sum)
  Rules     → add all numbers together
  Goal      → "I take an array and return the sum of all its numbers"
  Edge cases → [], [5], [-1,2,-3], [0,0,0]

PHASE 2 → IDENTIFY
  Return one value → 🪣 Bucket
  Algorithm        → Linear Scan (Section C → Linear Search)
  Tools            → Section A: Arrays

PHASE 3 → PSEUDO-CODE
  STORE result = 0
  REPEAT for each number in the array
    ADD that number to result
  GIVE BACK result

PHASE 4 → TRACE TABLE
  Input: [3, 1, 4]
  | Step  | i | arr[i] | result |
  | Start | — | —      | 0      |
  | i=0   | 0 | 3      | 3      |
  | i=1   | 1 | 1      | 4      |
  | i=2   | 2 | 4      | 8      |
  | End   | — | —      | 8 ✓    |
  Expected: 8. Table gives: 8. Logic is correct.

PHASE 5 → CODE
  function sumArray(arr) {
    if (arr.length === 0) return 0;  // edge case
    // STORE result = 0
    let result = 0;
    // REPEAT for each number
    for (let i = 0; i < arr.length; i++) {
      // ADD to result
      result += arr[i];
    }
    // GIVE BACK result
    return result;
  }

PHASE 6 → EDGE CASES + BIG-O
  sumArray([])          → 0 ✓
  sumArray([5])         → 5 ✓
  sumArray([-1, 2, -3]) → -2 ✓
  sumArray([0, 0, 0])   → 0 ✓

  TIME  → O(n) — one loop through n items
  SPACE → O(1) — only one variable (result), no new arrays
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
    result, seen, current, left, right — not x, temp, a, b

8.  "15 minutes daily beats 5 hours weekly."

9.  "Write the KEEP condition, not the SKIP condition.
     Define what belongs — everything else takes care of itself."

10. "Learn the data structure ONE DAY before you need it.
     Not all upfront. Use it immediately or you forget it."
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section K: How to Use This Blueprint

## The blueprint is a MAP, not a reading assignment.

```
Think of it like GPS navigation.

When you drive somewhere new, you do NOT:
  ✗ Read every road on the map before you leave
  ✗ Memorise every turn for every possible route
  ✗ Study the whole map for 3 hours before starting

You DO:
  ✅ Open the GPS when you need it
  ✅ Follow one instruction at a time
  ✅ Close it when you arrive

This blueprint works the same way:
  ✅ Open it when you need it
  ✅ Follow today's day only
  ✅ Close it when the 25 minutes are done

Reading ALL sections at once = wrong.
  Too much information. Brain overload. Nothing sticks.
  Reading Pattern 12 on Day 1 teaches you nothing.
  You have no foundation to connect it to yet.

Reading ONE pattern = right.
  One concept. One problem. One trace table. One solution.
  That sticks. That builds skill.
```

---

## The only sections you ever need to open:

```
BEFORE DAY 1   → Section B only
                 Read the 6 Phases once. Understand the process.
                 Then close it.

EVERY DAY      → Section I → find today's day number
                 Follow PREPARE A, PREPARE C, then SOLVE

WHEN READING A PATTERN (in Section D):
  → Check ALGORITHM line → open that in Section C → read it
  → Check TOOLS line → open Section A → review that structure
  → THEN start solving

WHEN STUCK identifying pattern → Section D Pattern Picker
WHEN CHECKING complexity       → Section F
WHEN PROBLEM uses 2 patterns   → Section E Combos
WHEN YOU FEEL LOST             → Section H Key Insights
WHEN NEED DATA STRUCTURE HELP  → Section A
WHEN NEED ALGORITHM QUICKLY    → Section C Quick Navigation table
```

---

## Complex patterns and algorithms take more than one day

```
This is normal. Not a failure.

Memory pattern      → 3 days   Sliding Window  → 2 days
Stack               → 2 days   Tree DFS        → 2 days
BFS                 → 2 days   DP              → 3 days
Backtracking        → 2 days   Greedy          → 2 days
Prefix Sum          → 2 days   Monotonic Stack → 2 days
Fast Slow Pointer   → 2 days   Heap            → 2 days

One problem per day. Not one pattern per day.
25 minutes. Done. Close it.
```

---

## Sorting algorithms are interview prep, not daily tools

```
JS Built-in Sort → Day 8 → use this for ALL daily problems
  .sort((a, b) => a - b)  ← ascending numbers
  .sort((a, b) => b - a)  ← descending numbers
  .sort()                 ← strings alphabetically

Bubble / Selection / Insertion / Merge / Quick Sort
  → Days 49-53 → ONLY for when interviewer asks "implement a sort"
  → Do NOT try to learn these before Day 49
  → They appear in Section C only as reference material
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Section L: Personal Notes — Discoveries While Studying

---

## Note 1: The Positive Framing Rule (Discovered Day 1)

```
Write the condition for what you WANT, not what you don't want.

Remove duplicates:
  Wrong framing: "if it IS a duplicate → skip"
  Right framing: "if it is NOT seen before → keep"

Move zeros to back:
  Wrong framing: "if it IS zero → don't place"
  Right framing: "if it is NOT zero → place it"

Applies everywhere:
  Validating a form  → "what makes it VALID?"
  Filtering users    → "who should I KEEP?"
  Tree traversal     → "when should I go DEEPER?"

EXCEPTION: Detective pattern deliberately uses the BAD condition.
  It looks for ONE bad thing → stops immediately.
  This is intentional and is the ONLY exception.
```

---

## Note 2: Memory Pattern Takes Multiple Days (Discovered Day 5)

```
Detective   → one template → one idea → one day ✓
Filter      → one template → one idea → one day ✓
Transformer → one template → one idea → one day ✓

Memory      → two tools (Set and Map)
            → two cases (Case 1 Basic, Case 1 Extended, Case 2)
            → three templates
            → three days

This is completely normal.
Do not rush it. Close the blueprint. One problem per day.
```

---

## Note 3: Pattern to Algorithm Connection (Before Day 1)

```
Every pattern has an algorithm inside it.
You never pick them separately.

Pattern 2 → 🪣 Bucket → Linear Scan + Kadane's lives inside
Pattern 5 → 🔎 Memory → Hash-Based Search lives inside
Pattern 8 → 🔪 Binary Search → Binary Search Algorithm lives inside

When you identify the pattern today:
  → check ALGORITHM line in Section D
  → open Section C → read that algorithm
  → check TOOLS line in Section D
  → open Section A → review that data structure
  → THEN solve the problem
```

---

## Note 4: Data Structures Not All Built Into JavaScript (Before Day 1)

```
JavaScript has BUILT IN:
  Arrays, Strings, Set, Map — use these from Day 1

You must BUILD yourself from Array or class:
  Stack      → learn Day 19 → use Day 20
  Queue      → learn Day 19 → use Day 23
  Tree       → learn Day 21 → use Day 22
  Graph      → learn Day 23 → use Day 24
  Linked List → learn Day 42 → use Day 43
  Heap       → learn Day 46 → use Day 47
  Deque      → learn Day 46 → use for advanced problems

Rule: Learn ONE DAY before you need it.
      Never learn a data structure you don't use that same week.
```

---

## Note 5: Sorting Algorithms Are Interview Prep Not Daily Tools (Discovered Day 8)

```
For daily problem solving:
  JS Built-in Sort → Day 8 → always use this
  .sort((a, b) => a - b) → ascending
  Never need to implement from scratch for actual problems

For interview preparation → Days 49-53:
  Bubble Sort    → Day 49 → learning only, never use in real code
  Selection Sort → Day 50 → learning only
  Insertion Sort → Day 51 → use for nearly sorted data O(n) best case
  Merge Sort     → Day 52 → implement when interviewer asks, O(n log n) stable
  Quick Sort     → Day 53 → implement when interviewer asks, O(n log n) average

The sorting algorithms in Section C exist as reference material.
You read them on Days 49-53. Not before.
```

---

## Note 6: Two Pointer Has 4 Approaches (Discovered Day 9)

```
Pattern 6 is NOT just one template.
It has 4 distinct approaches. Each solves different problems.

  Approach 1 → Opposite Ends
    left=0, right=end, move TOWARD each other
    USE FOR: palindrome, two sum sorted, 3sum, container water

  Approach 2 → Same Direction Read/Write
    write pointer stays behind, read scans ahead
    USE FOR: move zeroes, remove duplicates in-place, remove element

  Approach 3 → Fast and Slow Pointer
    slow=1 step, fast=2 steps simultaneously
    USE FOR: find middle of linked list, detect cycle, palindrome LL

  Approach 4 → Two Pointers on Sorted Array
    sort first, then outer loop + inner opposite ends
    USE FOR: 3Sum, 4Sum, triplets with condition

When you see a pointer walker problem:
  Step 1: Ask "which approach fits?"
  Step 2: Pick the right template.
  Step 3: Solve.
```

---

## One sentence to remember:

> **One day. One pattern. One problem. 25 minutes. Close it.**

[↑ Back to Table of Contents](#table-of-contents)
