# The Complete Blueprint — Problem Solving Mastery
## Everything You Need in One Place — Version 8

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
  - [Pattern to Algorithm Map](#pattern-to-algorithm-map)
  - [Two Pointer Approaches](#two-pointer-approaches)
  - [Sorting Algorithms](#sorting-algorithms)
  - [Searching Algorithms — Linear, Binary, Hash](#searching-algorithms)
  - [Traversal Algorithms](#traversal-algorithms)
  - [Kadane's Algorithm](#kadanes-algorithm)
  - [Floyd's Cycle Detection](#floyds-cycle-detection)
  - [Fast Slow Pointer](#fast-slow-pointer)
  - [Dijkstra's Algorithm](#dijkstras-algorithm)
  - [Topological Sort](#topological-sort)
  - [Union-Find](#union-find)
  - [Prefix Sum](#prefix-sum)
  - [Monotonic Stack](#monotonic-stack)
  - [Dutch National Flag](#dutch-national-flag)
  - [Counting Sort](#counting-sort)
  - [Heap Operations](#heap-operations)
  - [KMP String Search](#kmp-string-search)
  - [Complete Algorithm Map](#complete-algorithm-map)
- [Section D: All Patterns (4 Levels)](#section-d-all-patterns)
  - [Section D — Quick Navigation (all 14 patterns)](#section-d--quick-navigation)
  - [Level 1 — Foundation](#level-1--foundation-solves-80-of-problems)
    - [Pattern 1: 🔍 Detective](#pattern-1--detective-boolean-validation)
    - [Pattern 2: 🪣 Bucket](#pattern-2--bucket-accumulation)
    - [Pattern 3: 🏆 Filter](#pattern-3--filter-predicate-selection)
    - [Pattern 4: 🔄 Transformer](#pattern-4--transformer-data-projection)
    - [Pattern 5: 🔎 Memory](#pattern-5--memory-lookup--existence)
      - [Case 1 Basic — SET](#case-1-basic--set-yes-or-no)
      - [Case 1 Extended — MAP count](#case-1-extended--map-count-how-many-times)
      - [Case 2 — MAP index](#case-2--map-index-find-a-pair)
    - [Pattern 6: 📊 Pointer Walker](#pattern-6--pointer-walker-two-pointers)
      - [Approach 1 — Opposite Ends](#approach-1--opposite-ends)
      - [Approach 2 — Same Direction R/W](#approach-2--same-direction-readwrite)
      - [Approach 3 — Fast and Slow](#approach-3--fast-and-slow-pointer)
      - [Approach 4 — Sorted Array](#approach-4--two-pointers-on-sorted-array)
  - [Level 2 — Intermediate](#level-2--intermediate-solves-15-more)
    - [Pattern 7: 🪟 Sliding Window](#pattern-7--sliding-window)
      - [Variation 1 — Fixed Window](#variation-1--fixed-window)
      - [Variation 2 — Variable Window](#variation-2--variable-window)
    - [Pattern 8: 🔪 Binary Search](#pattern-8--binary-search)
    - [Pattern 9: 🏗️ Stack Pattern](#pattern-9--stack-pattern)
      - [Template A — Stack Matching](#template-a--stack-based-matching)
      - [Template B — Monotonic Stack](#template-b--monotonic-stack)
    - [Pattern 10: 🌳 Tree DFS](#pattern-10--tree-dfs)
    - [Pattern 11: 🌊 BFS](#pattern-11--bfs)
      - [Template A — Level Order BFS](#template-a--level-order-bfs-trees)
      - [Template B — Graph BFS](#template-b--graph-bfs-shortest-path)
      - [Template C — Topological Sort](#template-c--topological-sort-dependencies)
  - [Level 3 — Advanced](#level-3--advanced-solves-4-more)
    - [Pattern 12: 🧩 Dynamic Programming](#pattern-12--dynamic-programming)
      - [Approach 1 — Memoization](#approach-1--memoization-top-down)
      - [Approach 2 — Tabulation](#approach-2--tabulation-bottom-up)
    - [Pattern 13: 🔙 Backtracking](#pattern-13--backtracking)
    - [Pattern 14: 🏃 Greedy](#pattern-14--greedy)
  - [Level 4 — Specialist](#level-4--specialist-solves-remaining-1)
- [Section E: Pattern Combos](#section-e-pattern-combos)
- [Section F: Big-O Cheat Sheet](#section-f-big-o-cheat-sheet)
- [Section G: How Patterns Connect](#section-g-how-patterns-connect)
- [Section H: Key Insights](#section-h-key-insights)
- [Section I: 53-Day Practice Roadmap](#section-i-53-day-practice-roadmap)
- [Section J: Golden Rules](#section-j-golden-rules)
- [Section K: How to Use This Blueprint](#section-k-how-to-use-this-blueprint)
- [Section L: Personal Notes](#section-l-personal-notes)
- [Section M: Practice Questions — All Patterns](#section-m-practice-questions--all-patterns)

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
     → What must always be true about the output?
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
Then use the picker below to find the pattern.
```

**LEVEL 1 — Foundation (use these first)**

```
  true / false?                → 🔍 Pattern 1: Detective
  one value (sum/max/count)?   → 🪣 Pattern 2: Bucket
  shorter list?                → 🏆 Pattern 3: Filter
  same-size changed list?      → 🔄 Pattern 4: Transformer
  "seen before?" / find pair?  → 🔎 Pattern 5: Memory
  compare elements / in-place? → 📊 Pattern 6: Pointer Walker
```

**LEVEL 2 — Intermediate**

```
  best contiguous range?       → 🪟 Pattern 7: Sliding Window
  find in sorted data?         → 🔪 Pattern 8: Binary Search
  matching pairs / LIFO?       → 🏗️ Pattern 9: Stack
  explore tree deeply?         → 🌳 Pattern 10: Tree DFS
  level by level / shortest?   → 🌊 Pattern 11: BFS
```

**LEVEL 3 — Advanced**

```
  overlapping sub-problems?    → 🧩 Pattern 12: DP
  generate ALL combinations?   → 🔙 Pattern 13: Backtracking
  local best = global best?    → 🏃 Pattern 14: Greedy
```

> For the full pattern descriptions, examples, and templates
> → open [Section D: All Patterns](#section-d-all-patterns)

```
Once you identify the pattern:
  → the pattern gives you a TEMPLATE
  → the algorithm lives INSIDE that template
  → you do not pick the algorithm separately

THE FLOW:
  "What do I return?" → Pattern → Template → Algorithm comes free

EXAMPLES:
  Return one value   → 🪣 Bucket  → template: running variable
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
  ✗  If trace gives wrong output   → fix logic first → never code wrong logic
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
PLAIN ENGLISH           →  JAVASCRIPT
──────────────────────────────────────────────────────────
STORE a value           →  const x = value
STORE empty list        →  const result = []
STORE empty memory      →  const seen = new Set()
STORE frequency map     →  const freq = new Map()
REPEAT for each         →  for (let i = 0; i < arr.length; i++)
REPEAT while condition  →  while (condition) { }
IF something            →  if (condition) { }
COMPARE two values      →  if (a === b)
ADD TO list             →  result.push(item)
PUT IN memory (Set)     →  seen.add(item)
CHECK memory (Set)      →  seen.has(item)
PUT IN memory (Map)     →  freq.set(key, value)
GET FROM memory (Map)   →  freq.get(key)
GIVE BACK               →  return result
STOP early              →  return false (inside loop)
```

[↑ Back to Table of Contents](#table-of-contents)
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
| Search — linear scan O(n) | [Linear Search](#linear-search) |
| Search — sorted array O(log n) | [Binary Search](#binary-search) |
| Search — fast lookup O(1) via Set/Map | [Hash-Based Search](#hash-based-search) |
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

> When you identify a pattern, the algorithm comes FREE inside the template.
> Use this map to know which algorithm to read in Section C.

---

**LEVEL 1 — Foundation**

| Pattern | Algorithm Inside | Go To |
|---|---|---|
| 🔍 Pattern 1: Detective | Linear Search | [Linear Search](#linear-search) |
| 🪣 Pattern 2: Bucket | Linear Search + Kadane's | [Linear Search](#linear-search) · [Kadane's](#kadanes-algorithm) |
| 🏆 Pattern 3: Filter | Linear Search | [Linear Search](#linear-search) |
| 🔄 Pattern 4: Transformer | Linear Search | [Linear Search](#linear-search) |
| 🔎 Pattern 5: Memory | Hash-Based Search | [Hash-Based Search](#hash-based-search) |
| 📊 Pattern 6: Pointer Walker | Two Pointer (4 approaches) + Floyd's + Fast Slow + Dutch Flag | [Two Pointer](#two-pointer-approaches) · [Floyd's](#floyds-cycle-detection) · [Fast Slow](#fast-slow-pointer) · [Dutch Flag](#dutch-national-flag) |

**LEVEL 2 — Intermediate**

| Pattern | Algorithm Inside | Go To |
|---|---|---|
| 🪟 Pattern 7: Sliding Window | Sliding Window Technique | [Two Pointer Approaches](#two-pointer-approaches) |
| 🔪 Pattern 8: Binary Search | Binary Search Algorithm | [Binary Search](#binary-search) |
| 🏗️ Pattern 9: Stack | Stack-based Matching + Monotonic Stack | [Monotonic Stack](#monotonic-stack) |
| 🌳 Pattern 10: Tree DFS | Tree Traversal DFS | [Traversal Algorithms](#traversal-algorithms) |
| 🌊 Pattern 11: BFS | BFS Algorithm + Dijkstra's + Topological Sort | [Traversal Algorithms](#traversal-algorithms) · [Dijkstra's](#dijkstras-algorithm) · [Topological Sort](#topological-sort) |

**LEVEL 3 — Advanced**

| Pattern | Algorithm Inside | Go To |
|---|---|---|
| 🧩 Pattern 12: DP | Memoization + Tabulation | [Complete Algorithm Map](#complete-algorithm-map) |
| 🔙 Pattern 13: Backtracking | Recursive Backtracking | [Complete Algorithm Map](#complete-algorithm-map) |
| 🏃 Pattern 14: Greedy | Greedy Choice | [Complete Algorithm Map](#complete-algorithm-map) |

---

**TOOLS — used BEFORE patterns, not inside them**

| Tool | Used Before | Go To |
|---|---|---|
| JS Built-in Sort | Pattern 6, 8, 14 | [Sorting Algorithms](#sorting-algorithms) |
| Merge Sort | Pattern 6, 8, 14 (interview) | [Sorting Algorithms](#sorting-algorithms) |
| Quick Sort | Pattern 6, 8, 14 (interview) | [Sorting Algorithms](#sorting-algorithms) |
| Insertion Sort | Pattern 6 (nearly sorted data) | [Sorting Algorithms](#sorting-algorithms) |
| Bubble Sort | Learning only | [Sorting Algorithms](#sorting-algorithms) |
| Selection Sort | Learning only | [Sorting Algorithms](#sorting-algorithms) |
| Prefix Sum | Level 4 Pattern 17 | [Prefix Sum](#prefix-sum) |
| Counting Sort | Specific problems | [Counting Sort](#counting-sort) |
| Heap | Level 4 Pattern 15 | [Heap Operations](#heap-operations) |

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

## Section D — Quick Navigation

| Pattern | Name | Use When | Level |
|---|---|---|---|
| Pattern 1 | [🔍 Detective](#pattern-1--detective-boolean-validation) | Return true/false | 1 |
| Pattern 2 | [🪣 Bucket](#pattern-2--bucket-accumulation) | Return one value (sum, max) | 1 |
| Pattern 3 | [🏆 Filter](#pattern-3--filter-predicate-selection) | Return shorter list | 1 |
| Pattern 4 | [🔄 Transformer](#pattern-4--transformer-data-projection) | Return same-size changed list | 1 |
| Pattern 5 | [🔎 Memory](#pattern-5--memory-lookup--existence) | "Have I seen this?" / pairs | 1 |
| Pattern 6 | [📊 Pointer Walker](#pattern-6--pointer-walker-two-pointers) | Compare elements / in-place | 1 |
| Pattern 7 | [🪟 Sliding Window](#pattern-7--sliding-window) | Best/longest contiguous range | 2 |
| Pattern 8 | [🔪 Binary Search](#pattern-8--binary-search) | Find in sorted data | 2 |
| Pattern 9 | [🏗️ Stack Pattern](#pattern-9--stack-pattern) | Matching pairs / LIFO | 2 |
| Pattern 10 | [🌳 Tree DFS](#pattern-10--tree-dfs) | Explore tree deeply | 2 |
| Pattern 11 | [🌊 BFS](#pattern-11--bfs) | Level by level / shortest path | 2 |
| Pattern 12 | [🧩 Dynamic Programming](#pattern-12--dynamic-programming) | Overlapping sub-problems | 3 |
| Pattern 13 | [🔙 Backtracking](#pattern-13--backtracking) | Generate all combinations | 3 |
| Pattern 14 | [🏃 Greedy](#pattern-14--greedy) | Local best = global best | 3 |
| Level 4 | [⛰️ Heap, Trie, Union-Find...](#level-4--specialist-solves-remaining-1) | Specialist problems | 4 |

---

## Pattern Picker — Quick Version

```
"What do I RETURN?"

  true / false?              → 🔍 Pattern 1: Detective
  one number / string?       → 🪣 Pattern 2: Bucket
  shorter list?              → 🏆 Pattern 3: Filter
  same-size changed list?    → 🔄 Pattern 4: Transformer
  "seen before?" / pairs?    → 🔎 Pattern 5: Memory
  compare elements?          → 📊 Pattern 6: Pointer Walker
  best contiguous range?     → 🪟 Pattern 7: Sliding Window
  find in sorted data?       → 🔪 Pattern 8: Binary Search
  matching pairs / LIFO?     → 🏗️ Pattern 9: Stack
  explore tree deeply?       → 🌳 Pattern 10: Tree DFS
  level by level / shortest? → 🌊 Pattern 11: BFS
  overlapping sub-problems?  → 🧩 Pattern 12: DP
  generate all combos?       → 🔙 Pattern 13: Backtracking
  local best = global best?  → 🏃 Pattern 14: Greedy
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 1 — Foundation (Solves 80% of problems)

---

### Pattern 1: 🔍 Detective (Boolean Validation)

```
USE WHEN:
  You need to return true or false about a collection.
```

```
KEYWORDS:
  "Is it...?", "Are all...?", "Does every...?", "Is valid?"
```

**ALGORITHM:** → [Section C → Linear Search](#linear-search)

**TOOLS:**
→ [Section A: Arrays](#1-arrays) or [Section A: Strings](#2-strings)

```
LOGIC:
  Find ONE bad thing → return false immediately.
  Survive the entire loop → return true.

NOTE:
  Detective is the ONE exception to the positive framing rule.
  It deliberately looks for the BAD condition.
  The moment you find ONE bad thing → stop. Return false.
  This is called "early exit" and it is intentional.
```

```
DON'T USE WHEN:
  ✗ Need to COUNT bad things (use 🪣 Bucket)
  ✗ Need to COLLECT bad items (use 🏆 Filter)
  ✗ Answer is not true/false
  ✗ Checking if ONE value exists once → just use includes()
```

```javascript
function detective(arr) {
  for (let i = 0; i < arr.length - 1; i++) {
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
  i=0: 1 > 5? NO → continue
  i=1: 5 > 3? YES → return false ✓

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
USE WHEN:
  You need ONE value calculated from the entire list.
  (sum, max, min, count, product, longest, shortest)
```

```
KEYWORDS:
  "Sum of...", "Total...", "Count...", "Maximum...",
  "Minimum...", "Longest...", "Product of..."
```

**ALGORITHM:** → [Section C → Linear Search](#linear-search) + [Kadane's Algorithm](#kadanes-algorithm)

**TOOLS:**
→ [Section A: Arrays](#1-arrays) or [Section A: Strings](#2-strings)

```
LOGIC:
  Start with empty bucket.
  Pour each item into the bucket using some operation.
  Return the bucket.

  Normal Bucket   → Linear Scan → touches ALL items equally
  Kadane's Bucket → Kadane's    → finds BEST contiguous subarray
```

```
STARTING VALUES — pick the right one:
  Sum     → result = 0         (adding to 0 gives first item)
  Count   → result = 0
  Maximum → result = -Infinity (any real number will be bigger)
  Minimum → result = Infinity  (any real number will be smaller)
  Product → result = 1         (multiplying by 1 gives first item)
  Longest → result = 0

WHY -Infinity for max?
  If you start with 0 and all numbers are negative,
  you would incorrectly return 0.
  -Infinity ensures the first real number always replaces it.
```

```
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
USE WHEN:
  You need a SHORTER list — only the items that pass a test.
```

```
KEYWORDS:
  "Get all...", "Find all...", "Remove...", "Keep only..."
```

**ALGORITHM:** → [Section C → Linear Search](#linear-search)

**TOOLS:**
→ [Section A: Arrays](#1-arrays) or [Section A: Strings](#2-strings)

```
LOGIC:
  Start with empty winners list.
  Test each item. Pass the test → push to winners. Fail → skip.
```

```
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
    if (arr[i] % 2 === 0) winners.push(arr[i]);
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
USE WHEN:
  You need a SAME-SIZE list where every item is changed.
```

```
KEYWORDS:
  "Double each...", "Convert...", "Format...", "Extract...",
  "Calculate for each..."
```

**ALGORITHM:** → [Section C → Linear Search](#linear-search)

**TOOLS:**
→ [Section A: Arrays](#1-arrays) or [Section A: Strings](#2-strings)

```
LOGIC:
  Start with empty result list.
  Transform each item.
  Push the transformed version into result.
  Result is always SAME LENGTH as input.
```

```
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
  i=0: 2*2 = 4  → result=[4]
  i=1: 3*3 = 9  → result=[4,9]
  i=2: 5*5 = 25 → result=[4,9,25]
  Return: [4, 9, 25] ✓
```

```
Practice:
□ Double every number            □ Square every number
□ Celsius → Fahrenheit (×9/5+32) □ All strings to uppercase
□ Get length of each string      □ Extract first letter of each word
LeetCode: #977 Squares of Sorted Array
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 5: 🔎 Memory (Lookup / Existence)

```
KEYWORDS:
  "Duplicates", "Two Sum", "Frequency", "First unique",
  "Anagram", "Seen before", "Pair that adds to target"
```

```
HOW TO IDENTIFY WHICH CASE:
  "Does something exist or repeat?"     → Case 1 Basic    → SET
  "How many times does it appear?"      → Case 1 Extended → MAP (count)
  "Find TWO values that connect?"       → Case 2          → MAP (index)

THE SIGNAL FOR CASE 2:
  You see a TARGET or CONDITION
  that requires TWO values together.
  Formula connects them: a + b = target
```

```
DON'T USE WHEN:
  ✗ Data is already sorted (use 📊 Pointer Walker — more efficient)
  ✗ Just filtering by simple math (use 🏆 Filter)
  ✗ Checking if ONE value exists once (just use includes())
```

---

#### Case 1 Basic — SET (yes or no)

```
USE WHEN:
  "Have I seen this value before?"
  You only need YES or NO.
  You do NOT need to count or remember position.
```

```
KEYWORDS:
  "Does it have duplicates?", "Are all unique?",
  "Find first repeated", "Remove duplicates"
```

**ALGORITHM:** → [Section C → Hash-Based Search](#hash-based-search)

**TOOLS:**
→ [Section A: Set](#3-set)

```
LOGIC:
  Create empty SET.
  For each item — check SET first.
  Already in SET → you have seen it before.
  Not in SET → add it and continue.
  CHECK first. ADD after.
```

```javascript
function memorySetBasic(arr) {
  const seen = new Set();
  for (let i = 0; i < arr.length; i++) {
    if (seen.has(arr[i])) {
      // already seen this value
    }
    seen.add(arr[i]); // CHECK first, ADD after
  }
}
```

**Example — "Does array have duplicates?"**

```javascript
function hasDuplicates(arr) {
  const seen = new Set();
  for (const item of arr) {
    if (seen.has(item)) return true; // found duplicate
    seen.add(item);
  }
  return false;
}
```

```
Trace: [1, 3, 2, 3]
  item=1: seen={} → not in set → add → seen={1}
  item=3: seen={1} → not in set → add → seen={1,3}
  item=2: seen={1,3} → not in set → add → seen={1,2,3}
  item=3: seen={1,2,3} → IN SET → return true ✓

Trace: [1, 2, 3]
  item=1: add → seen={1}
  item=2: add → seen={1,2}
  item=3: add → seen={1,2,3}
  Loop ends → return false ✓
```

```
Practice:
□ Does array have duplicates? (#217)  □ Are all values unique?
□ Find first repeated character       □ Remove duplicates from array
□ Intersection of two arrays (#349)   □ Is this a subset?
LeetCode: #217, #349
```

[↑ Back to Table of Contents](#table-of-contents)

---

#### Case 1 Extended — MAP count (how many times)

```
USE WHEN:
  "How many times does each value appear?"
  You need to COUNT occurrences.
  Yes/no is not enough — you need the actual number.
```

```
KEYWORDS:
  "Frequency of...", "How many times...", "Most frequent",
  "Anagram", "First unique character"
```

**ALGORITHM:** → [Section C → Hash-Based Search](#hash-based-search)

**TOOLS:**
→ [Section A: Map](#4-map-hash-map)

```
LOGIC:
  Create empty MAP.
  For each item — get current count from MAP (or 0 if not seen).
  Add 1 and store back.
  MAP stores: value → count
```

```javascript
function memoryMapCount(arr) {
  const freq = new Map();
  for (let i = 0; i < arr.length; i++) {
    freq.set(arr[i], (freq.get(arr[i]) || 0) + 1);
  }
  return freq;
}
```

**Example — "Character Frequency"**

```javascript
function charFrequency(str) {
  const freq = new Map();
  for (const ch of str) {
    freq.set(ch, (freq.get(ch) || 0) + 1);
  }
  return freq;
}
// "hello" → { h→1, e→1, l→2, o→1 }
```

**Example — "Are Two Strings Anagrams?"**

```javascript
function isAnagram(str1, str2) {
  if (str1.length !== str2.length) return false;
  const freq = new Map();
  for (const ch of str1) {
    freq.set(ch, (freq.get(ch) || 0) + 1); // count UP
  }
  for (const ch of str2) {
    if (!freq.get(ch)) return false;        // not seen or count = 0
    freq.set(ch, freq.get(ch) - 1);         // count DOWN
  }
  return true;
}
```

```
WHY COUNT DOWN in anagram:
  str1 builds a BUDGET per character.
  str2 SPENDS that budget one at a time.
  If budget hits 0 before str2 finishes → not an anagram.

  str1="ab" str2="aa":
    Build: {a→1, b→1}
    Check 'a': freq=1 → ok → subtract → {a→0, b→1}
    Check 'a': freq=0 → FAIL → return false ✓
```

```
Trace: "cat" vs "tac"
  Build from "cat": { c→1, a→1, t→1 }
  Check "tac":
    t: freq=1 → ok → subtract → { c→1, a→1, t→0 }
    a: freq=1 → ok → subtract → { c→1, a→0, t→0 }
    c: freq=1 → ok → subtract → { c→0, a→0, t→0 }
  Return true ✓
```

```
Practice:
□ Character frequency of string       □ Element frequency of array
□ Are two strings anagrams? (#242)    □ First unique character (#387)
□ Most frequent element               □ Group anagrams together (#49)
□ Top K Frequent Elements (#347)      □ Ransom note
LeetCode: #242, #387, #49, #347
```

[↑ Back to Table of Contents](#table-of-contents)

---

#### Case 2 — MAP index (find a pair)

```
USE WHEN:
  "Find TWO values that together satisfy a condition."
  A FORMULA connects the two values.
  You need to return WHERE both values were found.
```

```
KEYWORDS:
  "Two Sum", "Pair that adds to target",
  "Find two numbers whose difference is K",
  "Two numbers whose product equals target"
```

**ALGORITHM:** → [Section C → Hash-Based Search](#hash-based-search)

**TOOLS:**
→ [Section A: Map](#4-map-hash-map)

```
LOGIC:
  Create empty MAP.
  For each value — calculate its COMPLEMENT using the formula.
  Check if complement is already in MAP.
  YES → found the pair → return both indices.
  NO  → store current value → index in MAP.
  CHECK complement first. STORE current value after.

WHY MAP not SET:
  SET can answer "have I seen 7?" → yes or no.
  MAP can answer "have I seen 7?" AND "where was 7?" → index.
  You need both to return the correct answer.

THE COMPLEMENT TRICK:
  Every Case 2 problem has a formula.
  Two Sum:      a + b = target → complement = target - a
  Difference K: a - b = k     → complement = a - k
  You calculate the complement and check if it exists.
  Never search for it — just check the MAP instantly.

WHY CHECK BEFORE STORE:
  Input [3, 3] target=6
  If you STORE first then CHECK:
    i=0: store 3→0, check complement=3 → found at 0 → return [0,0] WRONG
  If you CHECK first then STORE:
    i=0: check complement=3 → not found → store 3→0
    i=1: check complement=3 → found at 0 → return [0,1] CORRECT
```

```javascript
function memoryMapIndex(arr, target) {
  const memory = new Map(); // stores value → index
  for (let i = 0; i < arr.length; i++) {
    const complement = target - arr[i]; // calculate what I NEED
    if (memory.has(complement)) {
      return [memory.get(complement), i]; // found the pair
    }
    memory.set(arr[i], i); // CHECK first, STORE after
  }
  return []; // no pair found
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
// [2,7,11,15] target=9 → [0,1] ✓
```

```
Trace: [2, 7, 11, 15]  target=9

  i=0: complement=9-2=7  → MAP has 7? NO  → store 2→0  MAP={2→0}
  i=1: complement=9-7=2  → MAP has 2? YES at 0 → return [0,1] ✓

Trace: [3, 3]  target=6

  i=0: complement=6-3=3  → MAP has 3? NO  → store 3→0  MAP={3→0}
  i=1: complement=6-3=3  → MAP has 3? YES at 0 → return [0,1] ✓
```

```
Practice:
□ Two Sum (#1)                         □ Two Sum — return true/false
□ Difference of K                      □ Subarray Sum Equals K (#560)
□ Longest Consecutive Sequence (#128)  □ 4Sum II (#454)
LeetCode: #1, #560, #128, #454
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

### Pattern 6: 📊 Pointer Walker (Two Pointers)

```
KEYWORDS:
  "Palindrome", "Sorted array pair", "In-place removal",
  "Merge sorted", "Move elements", "Find middle",
  "Cycle detection", "Sort three values"
```

```
HOW TO IDENTIFY WHICH APPROACH:
  Compare from both ends / palindrome / pair in sorted → Approach 1: Opposite Ends
  Modify array in-place / move elements / remove       → Approach 2: Same Direction R/W
  Find middle / detect cycle (Linked List)             → Approach 3: Fast and Slow
  3Sum / 4Sum / multi-number sum                       → Approach 4: Sorted Array
```

```
SORTING TOOL — needed before some Pointer Walker problems:
  JS Built-in Sort  ← use this 80% of the time
  Merge Sort        ← implement when interviewer asks
  Quick Sort        ← implement when interviewer asks
```

**SORTING TOOL:** → [Section C → Sorting Algorithms](#sorting-algorithms)

```
DON'T USE WHEN:
  ✗ Data unsorted and need pair lookup (use 🔎 Memory — faster setup)
  ✗ Not comparing elements with EACH OTHER
```

[↑ Back to Table of Contents](#table-of-contents)

---

#### Approach 1 — Opposite Ends

```
USE WHEN:
  Compare items from BOTH ends simultaneously.
  Move toward the middle.
  Problems: palindrome, pair in sorted array,
  container water, trapping rain water.
```

```
KEYWORDS:
  "Palindrome", "Two Sum sorted", "Container water",
  "Reverse", "Symmetric"
```

**ALGORITHM:** → [Section C → Two Pointer Approaches](#two-pointer-approaches)

**TOOLS:**
→ [Section A: Arrays](#1-arrays) or [Section A: Strings](#2-strings)

```
LOGIC:
  left starts at index 0.
  right starts at last index.
  Compare arr[left] and arr[right].
  Move left++ or right-- based on result.
  Loop ends when left meets right.
```

```javascript
function oppositeEnds(arr) {
  let left = 0, right = arr.length - 1;
  while (left < right) {
    // compare arr[left] and arr[right]
    // move left++ or right-- based on result
    left++;
    right--;
  }
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
  left=0(r) right=6(r) → match → left=1 right=5
  left=1(a) right=5(a) → match → left=2 right=4
  left=2(c) right=4(c) → match → left=3 right=3
  left not < right → stop → return true ✓

Trace: "hello"
  left=0(h) right=4(o) → h !== o → return false ✓
```

**Example — "Two Sum in sorted array"**

```javascript
function twoSumSorted(arr, target) {
  let left = 0, right = arr.length - 1;
  while (left < right) {
    const sum = arr[left] + arr[right];
    if (sum === target) return [left, right];
    else if (sum < target) left++;  // need bigger → move left right
    else right--;                   // need smaller → move right left
  }
  return [];
}
// [2,7,11,15] target=9 → [0,1] ✓
```

```
Practice:
□ Is palindrome (#125)              □ Reverse string (#344)
□ Two Sum sorted (#167)             □ Container Most Water (#11)
□ Valid Palindrome II (#680)        □ Trapping Rain Water (#42)
□ Squares of Sorted Array (#977)
LeetCode: #125, #167, #344, #11, #42, #680, #977
```

[↑ Back to Table of Contents](#table-of-contents)

---

#### Approach 2 — Same Direction Read/Write

```
USE WHEN:
  Modify array IN-PLACE.
  Keep certain elements, remove or skip others.
  Write pointer builds the result.
  Read pointer scans ahead.
```

```
KEYWORDS:
  "Move zeroes", "Remove duplicates in-place",
  "Remove element", "Compact array"
```

**ALGORITHM:** → [Section C → Two Pointer Approaches](#two-pointer-approaches)

**TOOLS:**
→ [Section A: Arrays](#1-arrays)

```
LOGIC:
  write pointer starts at 0.
  read pointer scans from 0 to end.
  If current element should be KEPT → copy to write position → write++.
  If current element should be SKIPPED → read moves on, write stays.
  At end write = new valid length.
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
  return write; // new valid length
}
```

**Example — "Move Zeroes to end"**

```javascript
function moveZeroes(nums) {
  let write = 0;
  for (let read = 0; read < nums.length; read++) {
    if (nums[read] !== 0) {       // KEEP if NOT zero
      nums[write] = nums[read];
      write++;
    }
  }
  while (write < nums.length) {  // fill rest with zeros
    nums[write] = 0;
    write++;
  }
  return nums;
}
// [0,1,0,3,12] → [1,3,12,0,0] ✓
```

```
Trace: [0, 1, 0, 3]
  read=0: nums[0]=0 → skip  → write=0
  read=1: nums[1]=1 → keep  → nums[0]=1 → write=1
  read=2: nums[2]=0 → skip  → write=1
  read=3: nums[3]=3 → keep  → nums[1]=3 → write=2
  Fill zeros: nums[2]=0, nums[3]=0
  Return: [1,3,0,0] ✓
```

```
Practice:
□ Move zeroes (#283)                 □ Remove element (#27)
□ Remove duplicates sorted (#26)     □ Remove duplicates II (#80)
□ Merge sorted arrays (#88)
LeetCode: #283, #27, #26, #80, #88
```

[↑ Back to Table of Contents](#table-of-contents)

---

#### Approach 3 — Fast and Slow Pointer

```
USE WHEN:
  Problems on LINKED LISTS.
  Find the middle without counting length first.
  Detect if a cycle exists.
  No extra memory allowed.
```

```
KEYWORDS:
  "Find middle of list", "Detect cycle",
  "Cycle entry point", "Palindrome linked list"
```

**ALGORITHM:** → [Section C → Fast Slow Pointer](#fast-slow-pointer) + [Floyd's Cycle Detection](#floyds-cycle-detection)

**TOOLS:**
→ [Section A: Linked List](#7-linked-list)

```
LOGIC:
  slow pointer moves 1 step per iteration.
  fast pointer moves 2 steps per iteration.
  When fast reaches end → slow is at middle.
  If cycle exists → fast and slow MUST meet.
  If no cycle → fast reaches null.
```

```javascript
function fastSlow(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;       // 1 step
    fast = fast.next.next;  // 2 steps
  }
  return slow; // at middle when fast reaches end
}
```

**Example — "Find middle of linked list"**

```javascript
function findMiddle(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;
    fast = fast.next.next;
  }
  return slow;
}
// 1→2→3→4→5 → returns node 3 ✓
// 1→2→3→4   → returns node 2 ✓
```

**Example — "Detect cycle"**

```javascript
function hasCycle(head) {
  let slow = head, fast = head;
  while (fast !== null && fast.next !== null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow === fast) return true; // they met → cycle exists
  }
  return false; // fast reached null → no cycle
}
```

```
Trace: 1→2→3→4→5 (no cycle)
  slow=1 fast=1
  slow=2 fast=3
  slow=3 fast=5
  fast.next=null → stop → return false ✓

Trace: 1→2→3→4→(back to 2) (cycle)
  slow and fast will eventually meet → return true ✓
```

```
Practice:
□ Find middle (#876)                 □ Linked list cycle (#141)
□ Linked list cycle II (#142)        □ Palindrome linked list (#234)
□ Reorder list (#143)
LeetCode: #876, #141, #142, #234, #143
```

[↑ Back to Table of Contents](#table-of-contents)

---

#### Approach 4 — Two Pointers on Sorted Array

```
USE WHEN:
  Find combinations of multiple numbers that sum to target.
  Sort the array first.
  Outer loop fixes one element.
  Inner Opposite Ends finds the pair.
```

```
KEYWORDS:
  "3Sum", "4Sum", "All triplets that sum to target",
  "Closest sum to target"
```

**ALGORITHM:** → [Section C → Two Pointer Approaches](#two-pointer-approaches)

**TOOLS:**
→ [Section A: Arrays](#1-arrays)
→ [Section C: Sorting Algorithms](#sorting-algorithms) ← sort FIRST

```
LOGIC:
  Sort the array.
  Outer loop fixes one number at index i.
  Inner loop uses Opposite Ends from i+1 to end.
  Adjust left and right based on sum vs target.
  Skip duplicates to avoid repeated results.
```

```javascript
function sortedMultiSum(arr, target) {
  arr.sort((a, b) => a - b); // sort FIRST
  const result = [];
  for (let i = 0; i < arr.length - 2; i++) {
    if (i > 0 && arr[i] === arr[i-1]) continue; // skip duplicates
    let left = i + 1, right = arr.length - 1;
    while (left < right) {
      const sum = arr[i] + arr[left] + arr[right];
      if (sum === target) {
        result.push([arr[i], arr[left], arr[right]]);
        while (left < right && arr[left] === arr[left+1]) left++;
        while (left < right && arr[right] === arr[right-1]) right--;
        left++; right--;
      } else if (sum < target) left++;
      else right--;
    }
  }
  return result;
}
```

**Example — "3Sum"**

```javascript
function threeSum(nums) {
  nums.sort((a, b) => a - b);
  const result = [];
  for (let i = 0; i < nums.length - 2; i++) {
    if (i > 0 && nums[i] === nums[i-1]) continue;
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

```
Practice:
□ 3Sum (#15)                         □ 4Sum (#18)
□ 3Sum Closest (#16)                 □ Triplets with smaller sum
LeetCode: #15, #18, #16
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 2 — Intermediate (Solves 15% more)

> **Prerequisite:** Level 1 patterns feel easy. Pick them in under 30 seconds.

---

### Pattern 7: 🪟 Sliding Window

```
KEYWORDS:
  "Consecutive", "Subarray of size K", "Longest substring",
  "Shortest subarray with condition", "Contiguous"
```

```
HOW TO IDENTIFY WHICH VARIATION:
  Window size never changes         → Fixed Window
  Window grows and shrinks          → Variable Window
```

```
DON'T USE WHEN:
  ✗ Elements do not need to be contiguous
  ✗ You need a pair not a range (use 🔎 Memory Case 2)
```

**BUILT ON:** 📊 Pointer Walker + 🪣 Bucket

[↑ Back to Table of Contents](#table-of-contents)

---

#### Variation 1 — Fixed Window

```
USE WHEN:
  Window size K is given and never changes.
  Slide across the array: add right, remove left.
  Find best window (max sum, min sum, etc).
```

**ALGORITHM:** → [Section C → Two Pointer Approaches](#two-pointer-approaches)

**TOOLS:**
→ [Section A: Arrays](#1-arrays) or [Section A: Strings](#2-strings)

```
LOGIC:
  Build first window of size K.
  Slide: add arr[i] on right, subtract arr[i-k] on left.
  Track best result seen.
```

```javascript
function fixedWindow(arr, k) {
  let windowSum = 0;
  for (let i = 0; i < k; i++) windowSum += arr[i]; // first window
  let best = windowSum;
  for (let i = k; i < arr.length; i++) {
    windowSum += arr[i] - arr[i - k]; // add new right, remove old left
    best = Math.max(best, windowSum);
  }
  return best;
}
```

**Example — "Max sum subarray of size K"**

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
Trace: [1,4,2,7,1]  k=3
  First window [1,4,2] → sum=7 → best=7
  Slide: +7-1=13 → best=13
  Slide: +1-4=10 → best=13
  Return: 13 ✓
```

```
Practice:
□ Max sum subarray size K           □ Average of all K-length subarrays
□ First negative in every window    □ Max of all K-length subarrays
```

[↑ Back to Table of Contents](#table-of-contents)

---

#### Variation 2 — Variable Window

```
USE WHEN:
  Window size changes based on a condition.
  Expand right to grow window.
  Shrink from left when condition is violated.
  Find longest or shortest window satisfying condition.
```

**ALGORITHM:** → [Section C → Two Pointer Approaches](#two-pointer-approaches)

**TOOLS:**
→ [Section A: Arrays](#1-arrays) or [Section A: Strings](#2-strings)
→ [Section A: Set](#3-set) (track unique chars in window)
→ [Section A: Map](#4-map-hash-map) (track char counts in window)

```
LOGIC:
  left and right both start at 0.
  right expands window — add element.
  When condition violated → shrink from left.
  Track best window size seen.
```

```javascript
function variableWindow(s) {
  let left = 0, best = 0;
  const inWindow = new Set();
  for (let right = 0; right < s.length; right++) {
    while (inWindow.has(s[right])) { // condition violated — shrink
      inWindow.delete(s[left]);
      left++;
    }
    inWindow.add(s[right]);
    best = Math.max(best, right - left + 1);
  }
  return best;
}
```

**Example — "Longest substring without repeating characters"**

```javascript
function lengthOfLongestSubstring(s) {
  const inWindow = new Set();
  let left = 0, best = 0;
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
// "abcabcbb" → 3 ("abc") ✓
```

```
Practice:
□ Longest substr no repeat (#3)       □ Min subarray sum ≥ target (#209)
□ Min Window Substring (#76)          □ Sliding Window Maximum (#239)
□ Longest substr K distinct chars
LeetCode: #3, #209, #76, #239, #438, #567
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 8: 🔪 Binary Search

```
USE WHEN:
  Find a target in SORTED data by eliminating half each step.
```

```
KEYWORDS:
  "Sorted array", "Find position", "First/last occurrence",
  "Search in rotated", "Minimum in rotated"
```

**ALGORITHM:** → [Section C → Binary Search](#binary-search)

**TOOLS:**
→ [Section A: Arrays](#1-arrays)
→ [Section C: Sorting Algorithms](#sorting-algorithms) ← data must be sorted first

```
LOGIC:
  lo = 0, hi = last index.
  mid = floor((lo + hi) / 2).
  arr[mid] === target → found.
  arr[mid] < target  → go right (lo = mid + 1).
  arr[mid] > target  → go left  (hi = mid - 1).
  Loop ends when lo > hi → not found.

BUILT ON: 🔍 Detective (elimination logic)
```

```
DON'T USE WHEN:
  ✗ Data is NOT sorted and cannot be sorted
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

**Example — "Find target in sorted array"**

```javascript
function search(arr, target) {
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
Trace: [2,5,8,12,16]  target=12
  lo=0 hi=4 → mid=2 → arr[2]=8 → 8<12 → lo=3
  lo=3 hi=4 → mid=3 → arr[3]=12 → FOUND at 3 ✓

Trace: [2,5,8,12,16]  target=6
  lo=0 hi=4 → mid=2 → arr[2]=8 → 8>6 → hi=1
  lo=0 hi=1 → mid=0 → arr[0]=2 → 2<6 → lo=1
  lo=1 hi=1 → mid=1 → arr[1]=5 → 5<6 → lo=2
  lo=2 > hi=1 → stop → return -1 ✓
```

```
Practice:
□ Binary Search (#704)               □ First/Last position (#34)
□ Search Rotated (#33)               □ Sqrt(x) (#69)
□ Find Peak Element (#162)           □ Search 2D Matrix (#74)
LeetCode: #704, #34, #33, #69, #162, #74
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 9: 🏗️ Stack Pattern

```
KEYWORDS:
  "Valid parentheses", "Next greater", "Next smaller",
  "Evaluate expression", "Largest rectangle", "Undo"
```

```
HOW TO IDENTIFY WHICH TEMPLATE:
  Matching pairs / brackets / undo history → Template A: Stack Matching
  Next greater / smaller element            → Template B: Monotonic Stack
```

```
DON'T USE WHEN:
  ✗ Processing order does not matter
  ✗ You need FIFO order → use Queue / BFS
```

**TOOLS:**
→ [Section A: Stack](#5-stack)

[↑ Back to Table of Contents](#table-of-contents)

---

#### Template A — Stack-based Matching

```
USE WHEN:
  Matching PAIRS of opening and closing items.
  What came JUST BEFORE matters.
  Need to check if things opened and closed correctly.
  Undo / redo type problems.
```

```
KEYWORDS:
  "Valid parentheses", "Matching brackets",
  "Evaluate expression", "Min stack"
```

**ALGORITHM:** → [Section C → Stack-based Matching](#pattern-9--stack-pattern)

```
LOGIC:
  Push items onto stack when they OPEN.
  When you see a CLOSING item — pop from stack.
  Check if popped item matches current closing item.
  Mismatch → return false immediately.
  End: stack must be empty (all matched).
```

```javascript
function stackMatching(input) {
  const stack = [];
  for (const item of input) {
    if (/* should push — opening item */) {
      stack.push(item);
    } else {
      const top = stack.pop();
      if (/* top does not match item */) return false;
    }
  }
  return stack.length === 0; // all matched
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
  ch='(' → push → stack=['(']
  ch='[' → push → stack=['(','[']
  ch=']' → pop '[' → pairs[']']='[' → match ✓
  ch=')' → pop '(' → pairs[')']='(' → match ✓
  stack empty → return true ✓

Trace: "([)]"
  ch='(' → push → stack=['(']
  ch='[' → push → stack=['(','[']
  ch=')' → pop '[' → pairs[')']='(' → '[' !== '(' → return false ✓
```

```
Practice:
□ Valid Parentheses (#20)             □ Min Stack (#155)
□ Evaluate RPN (#150)                 □ Basic Calculator (#224)
LeetCode: #20, #155, #150, #224
```

[↑ Back to Table of Contents](#table-of-contents)

---

#### Template B — Monotonic Stack

```
USE WHEN:
  Find the NEXT GREATER or NEXT SMALLER element for each position.
  Stack stays ALWAYS increasing or ALWAYS decreasing.
  When new element breaks the order — pop and record answer.
```

```
KEYWORDS:
  "Next greater element", "Next smaller element",
  "Daily temperatures", "Largest rectangle",
  "Trapping rain water"
```

**ALGORITHM:** → [Section C → Monotonic Stack](#monotonic-stack)

```
LOGIC:
  Stack stores INDICES (not values).
  Values in stack stay in decreasing order (for next greater).
  For each new element:
    While stack not empty AND current > stack top value:
      Pop → current element IS the next greater for popped index.
    Push current index.
  Elements remaining in stack → no next greater exists → -1.
```

```javascript
function monotonicStack(arr) {
  const result = new Array(arr.length).fill(-1);
  const stack = []; // stores INDICES
  for (let i = 0; i < arr.length; i++) {
    while (stack.length > 0 && arr[i] > arr[stack[stack.length - 1]]) {
      const idx = stack.pop();
      result[idx] = arr[i]; // arr[i] is next greater for idx
    }
    stack.push(i);
  }
  return result;
}
```

**Example — "Next Greater Element"**

```javascript
function nextGreaterElement(arr) {
  const result = new Array(arr.length).fill(-1);
  const stack = [];
  for (let i = 0; i < arr.length; i++) {
    while (stack.length > 0 && arr[i] > arr[stack[stack.length - 1]]) {
      const idx = stack.pop();
      result[idx] = arr[i];
    }
    stack.push(i);
  }
  return result;
}
```

```
Trace: [2, 1, 5, 3, 6]
  i=0: stack empty → push 0    stack=[0]   values=[2]
  i=1: 1 < 2 → push 1          stack=[0,1] values=[2,1]
  i=2: 5 > 1 → pop 1→result[1]=5
       5 > 2 → pop 0→result[0]=5
       push 2                   stack=[2]   values=[5]
  i=3: 3 < 5 → push 3          stack=[2,3] values=[5,3]
  i=4: 6 > 3 → pop 3→result[3]=6
       6 > 5 → pop 2→result[2]=6
       push 4                   stack=[4]   values=[6]
  Return: [5,5,6,6,-1] ✓
```

```
Practice:
□ Next Greater Element I (#496)       □ Daily Temperatures (#739)
□ Largest Rectangle (#84)             □ Trapping Rain Water (#42)
□ Next Greater Element II (#503)
LeetCode: #496, #739, #84, #42, #503
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 10: 🌳 Tree DFS

```
USE WHEN:
  Explore every node in a tree by going DEEP first.
  Recursively ask left subtree, then right subtree.
  Combine answers bottom-up.
```

```
KEYWORDS:
  "Max depth", "Path sum", "Invert tree", "Validate BST",
  "Diameter", "Lowest common ancestor", "Same tree"
```

**ALGORITHM:** → [Section C → Tree Traversal DFS](#traversal-algorithms)

**TOOLS:**
→ [Section A: Tree](#8-tree-binary-tree)

```
LOGIC:
  Every tree DFS problem follows the SAME skeleton:
  1. Base case: node is null → return base value
  2. Recurse left  → get answer from left subtree
  3. Recurse right → get answer from right subtree
  4. Combine left + right + current node → return answer

  Only the BASE VALUE and COMBINE step change per problem.
  The skeleton never changes.
```

```
DON'T USE WHEN:
  ✗ Need level-by-level processing (use 🌊 BFS)
  ✗ Need shortest path (use 🌊 BFS)
```

```javascript
function treeDFS(node) {
  if (!node) return /* base value */; // 1. base case

  const left  = treeDFS(node.left);  // 2. recurse left
  const right = treeDFS(node.right); // 3. recurse right

  return /* combine left + right + node.val */; // 4. combine
}
```

**Example — "Max depth"**

```javascript
function maxDepth(node) {
  if (!node) return 0;                              // null = depth 0
  const left  = maxDepth(node.left);
  const right = maxDepth(node.right);
  return 1 + Math.max(left, right);                 // 1 + deeper side
}
```

**Example — "Path sum exists"**

```javascript
function hasPathSum(node, target) {
  if (!node) return false;
  if (!node.left && !node.right) return node.val === target; // leaf
  return hasPathSum(node.left,  target - node.val) ||
         hasPathSum(node.right, target - node.val);
}
```

```
Trace maxDepth:
       1
      / \
     2   3
  maxDepth(2) = 1 + max(0,0) = 1
  maxDepth(3) = 1 + max(0,0) = 1
  maxDepth(1) = 1 + max(1,1) = 2 ✓
```

```
Practice:
□ Max depth (#104)         □ Invert tree (#226)
□ Path sum (#112)          □ Validate BST (#98)
□ Same tree (#100)         □ Diameter (#543)
□ LCA (#236)               □ Max Path Sum (#124)
LeetCode: #104, #226, #112, #98, #100, #543, #236, #124
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 11: 🌊 BFS

```
KEYWORDS:
  "Level order", "Shortest path", "Nearest neighbor",
  "Minimum steps", "Layer by layer", "Course prerequisites"
```

```
HOW TO IDENTIFY WHICH TEMPLATE:
  Tree — level by level results     → Template A: Level Order BFS
  Graph — shortest path             → Template B: Graph BFS
  Dependencies — task ordering      → Template C: Topological Sort
```

```
DON'T USE WHEN:
  ✗ Need to explore all paths to leaves (use 🌳 DFS)
  ✗ Tree height / depth problems (use 🌳 DFS)
```

**TOOLS:**
→ [Section A: Queue](#6-queue)

[↑ Back to Table of Contents](#table-of-contents)

---

#### Template A — Level Order BFS (trees)

```
USE WHEN:
  Process a TREE level by level.
  Collect results per level.
  Find nodes at a specific depth.
```

```
KEYWORDS:
  "Level order", "Level by level", "Right side view",
  "Zigzag traversal", "Average of each level"
```

**ALGORITHM:** → [Section C → BFS Algorithm](#traversal-algorithms)

**TOOLS:**
→ [Section A: Queue](#6-queue)
→ [Section A: Tree](#8-tree-binary-tree)

```
LOGIC:
  Add root to queue.
  While queue not empty:
    Record size of current level.
    Process exactly size nodes — add their children.
    After processing all size nodes → one full level done.
```

```javascript
function levelOrderBFS(root) {
  if (!root) return [];
  const queue = [root];
  const result = [];
  while (queue.length > 0) {
    const size = queue.length; // nodes at THIS level
    const level = [];
    for (let i = 0; i < size; i++) {
      const node = queue.shift();
      level.push(node.val);
      if (node.left)  queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    result.push(level);
  }
  return result;
}
```

**Example — "Level order traversal"**

```javascript
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
      if (node.left)  queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    result.push(level);
  }
  return result;
}
// Tree: 3→[9,20]→[15,7] → [[3],[9,20],[15,7]] ✓
```

```
Practice:
□ Level Order (#102)                  □ Right Side View (#199)
□ Zigzag Level Order (#103)           □ Average of Levels (#637)
□ Max Width of Binary Tree (#662)
LeetCode: #102, #199, #103, #637, #662
```

[↑ Back to Table of Contents](#table-of-contents)

---

#### Template B — Graph BFS (shortest path)

```
USE WHEN:
  Find SHORTEST PATH in an unweighted graph.
  Find nearest neighbor.
  Explore all nodes reachable from a start node.
  Count connected components.
```

```
KEYWORDS:
  "Shortest path", "Minimum steps", "Nearest",
  "Number of islands", "Rotting oranges"
```

**ALGORITHM:** → [Section C → BFS Algorithm](#traversal-algorithms)

**TOOLS:**
→ [Section A: Queue](#6-queue)
→ [Section A: Graph](#9-graph)
→ [Section A: Set](#3-set) (track visited nodes)

```
LOGIC:
  Add start node to queue AND visited set.
  While queue not empty:
    Dequeue node.
    For each neighbor not yet visited:
      Mark visited. Add to queue.
  Visited set prevents infinite loops in cycles.
```

```javascript
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
```

**Example — "Number of Islands"**

```javascript
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
Practice:
□ Number of Islands (#200)            □ Rotting Oranges (#994)
□ Word Ladder (#127)                  □ Clone Graph (#133)
□ Shortest Path in Binary Matrix (#1091)
LeetCode: #200, #994, #127, #133, #542, #1091
```

[↑ Back to Table of Contents](#table-of-contents)

---

#### Template C — Topological Sort (dependencies)

```
USE WHEN:
  Tasks have DEPENDENCIES — some must happen before others.
  "Before course B you must take course A."
  Detect if a cycle exists in a directed graph.
  Find valid ordering of tasks.
```

```
KEYWORDS:
  "Course prerequisites", "Build order",
  "Task scheduling", "Dependency order"
```

**ALGORITHM:** → [Section C → Topological Sort](#topological-sort)

**TOOLS:**
→ [Section A: Queue](#6-queue)
→ [Section A: Graph](#9-graph)

```
LOGIC:
  Build adjacency list and inDegree count per node.
  Start: add all nodes with inDegree=0 to queue.
  Process: dequeue node → add to order.
    For each neighbor → reduce inDegree by 1.
    If neighbor inDegree hits 0 → add to queue.
  End: if order has all nodes → valid order exists.
       If not → cycle detected → no valid order.
```

```javascript
function topoSort(numNodes, edges) {
  const graph = Array.from({ length: numNodes }, () => []);
  const inDegree = Array(numNodes).fill(0);
  for (const [from, to] of edges) {
    graph[from].push(to);
    inDegree[to]++;
  }
  const queue = [];
  for (let i = 0; i < numNodes; i++) {
    if (inDegree[i] === 0) queue.push(i); // no dependencies
  }
  const order = [];
  while (queue.length > 0) {
    const node = queue.shift();
    order.push(node);
    for (const nb of graph[node]) {
      if (--inDegree[nb] === 0) queue.push(nb);
    }
  }
  return order.length === numNodes ? order : []; // [] = cycle
}
```

```
Practice:
□ Course Schedule (#207)              □ Course Schedule II (#210)
□ Alien Dictionary (#269)
LeetCode: #207, #210, #269
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 3 — Advanced (Solves 4% more)

> **Prerequisite:** Level 2 patterns feel comfortable.

---

### Pattern 12: 🧩 Dynamic Programming

```
KEYWORDS:
  "How many ways?", "Minimum cost to reach", "Can I reach?",
  "Longest increasing", "Maximum profit", "Minimum coins"
```

```
USE WHEN:
  Problem has BOTH of these:
  1. Overlapping sub-problems — same smaller problem solved multiple times
  2. Optimal substructure — best overall answer built from best sub-answers
```

```
HOW TO IDENTIFY WHICH APPROACH:
  Start from top (n) and cache as you go    → Approach 1: Memoization
  Start from bottom (base) and build up     → Approach 2: Tabulation
  Both give same answer. Tabulation is usually cleaner to code.
```

```
3-STEP RECIPE — always follow this order:
  1. DEFINE:  "What does dp[i] mean in plain English?"
  2. RELATE:  "How does dp[i] connect to smaller values?"
  3. BASE:    "What are the smallest known values?"
```

**BUILT ON:** 🔎 Memory + 🪣 Bucket + Recursion

```
DON'T USE WHEN:
  ✗ Sub-problems don't overlap (use Divide and Conquer)
  ✗ Greedy local choice provably always optimal (use 🏃 Greedy)
  TIP: If unsure between DP and Greedy → use DP. Always correct.
```

**TOOLS:**
→ [Section A: Arrays](#1-arrays)

[↑ Back to Table of Contents](#table-of-contents)

---

#### Approach 1 — Memoization (Top-Down)

```
USE WHEN:
  You think recursively — break big problem into smaller.
  Cache results so you never solve same sub-problem twice.
  Good when you do not need ALL sub-problems.
  Natural fit when recursive solution already exists.
```

**ALGORITHM:** → [Section C → Complete Algorithm Map](#complete-algorithm-map)

```
LOGIC:
  Write recursive solution first.
  Add a cache (Map or array).
  Before computing — check cache.
  After computing — store in cache.
  Return cached value on repeat calls.
```

```javascript
function memoization(n, cache = new Map()) {
  if (/* base case */) return /* base value */;
  if (cache.has(n)) return cache.get(n); // already solved
  const result = /* recursive formula */;
  cache.set(n, result); // store before returning
  return result;
}
```

**Example — "Climbing Stairs (memoization)"**

```javascript
function climbStairsMemo(n, cache = new Map()) {
  if (n <= 1) return 1;
  if (cache.has(n)) return cache.get(n);
  const result = climbStairsMemo(n-1, cache) + climbStairsMemo(n-2, cache);
  cache.set(n, result);
  return result;
}
// climbStairsMemo(5) → 8 ✓
```

```
Practice:
□ Climbing Stairs (#70)               □ House Robber (#198)
□ Word Break (#139)                   □ Coin Change (#322)
LeetCode: #70, #198, #139, #322
```

[↑ Back to Table of Contents](#table-of-contents)

---

#### Approach 2 — Tabulation (Bottom-Up)

```
USE WHEN:
  Build solution from smallest sub-problems upward.
  No recursion — just a loop filling an array.
  More efficient — no call stack overhead.
  Preferred in interviews — easier to read and explain.
```

**ALGORITHM:** → [Section C → Complete Algorithm Map](#complete-algorithm-map)

```
LOGIC:
  Create dp array of size n+1.
  Fill base cases first.
  Fill dp[i] using the recurrence relation.
  Return dp[n].

  Space optimization: if only need last 2 values
  use two variables instead of full array.
```

```javascript
function tabulation(n) {
  let prev2 = /* dp[0] */;
  let prev1 = /* dp[1] */;
  for (let i = 2; i <= n; i++) {
    const curr = /* formula using prev1 and prev2 */;
    prev2 = prev1;
    prev1 = curr;
  }
  return prev1;
}
```

**Example — "Climbing Stairs (tabulation)"**

```javascript
// DEFINE:  dp[i] = number of ways to reach step i
// RELATE:  dp[i] = dp[i-1] + dp[i-2]
// BASE:    dp[0] = 1, dp[1] = 1

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
  i=2: curr=1+1=2  prev2=1 prev1=2
  i=3: curr=2+1=3  prev2=2 prev1=3
  i=4: curr=3+2=5  prev2=3 prev1=5
  i=5: curr=5+3=8  prev2=5 prev1=8
  Return: 8 ✓
```

```
Practice:
□ Climbing Stairs (#70)               □ House Robber (#198)
□ Coin Change (#322)                  □ LIS (#300)
□ Max Product Subarray (#152)         □ LCS (#1143)
LeetCode: #70, #198, #322, #300, #139, #152, #1143, #416
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 13: 🔙 Backtracking

```
USE WHEN:
  Generate ALL valid combinations, permutations, or configurations.
```

```
KEYWORDS:
  "All subsets", "All permutations", "N-Queens", "Sudoku",
  "Word search", "Generate all..."
```

**ALGORITHM:** → [Section C → Complete Algorithm Map](#complete-algorithm-map)

**TOOLS:**
→ [Section A: Arrays](#1-arrays) or [Section A: Strings](#2-strings)

```
LOGIC:
  CHOOSE an option.
  EXPLORE that path fully (recurse).
  UN-CHOOSE (undo) → restore state.
  Try next option.
  Always in this exact order.

BUILT ON: 🌳 DFS + trial-and-error

WHY UN-CHOOSE MATTERS:
  Without undoing, the next path starts with leftover state.
  This corrupts all subsequent results.
  UN-CHOOSE restores state so each path starts clean.
```

```
DON'T USE WHEN:
  ✗ You only need ONE solution (use DFS instead)
  ✗ Problem has optimal substructure (use 🧩 DP instead)
```

```javascript
function backtrack(result, current, choices, start) {
  result.push([...current]); // save COPY of current state

  for (let i = start; i < choices.length; i++) {
    current.push(choices[i]);                    // CHOOSE
    backtrack(result, current, choices, i + 1);  // EXPLORE
    current.pop();                               // UN-CHOOSE
  }
}
```

**Example — "All subsets"**

```javascript
function subsets(nums) {
  const result = [];
  function bt(start, current) {
    result.push([...current]);
    for (let i = start; i < nums.length; i++) {
      current.push(nums[i]); // CHOOSE
      bt(i + 1, current);    // EXPLORE
      current.pop();         // UN-CHOOSE
    }
  }
  bt(0, []);
  return result;
}
// [1,2,3] → [[],[1],[1,2],[1,2,3],[1,3],[2],[2,3],[3]] ✓
```

```
Trace: [1,2]
  bt(0,[]):   push [] → result=[[]]
    choose 1: bt(1,[1])
      push [1] → result=[[],[1]]
      choose 2: bt(2,[1,2])
        push [1,2] → result=[[],[1],[1,2]]
        no more → return
      unchoose 2 → [1]
    return
    unchoose 1 → []
    choose 2: bt(2,[2])
      push [2] → result=[[],[1],[1,2],[2]]
      no more → return
    unchoose 2 → []
  Return: [[],[1],[1,2],[2]] ✓
```

```
Practice:
□ All subsets (#78)           □ All permutations (#46)
□ Combination Sum (#39)       □ Word Search (#79)
□ N-Queens (#51)              □ Palindrome Partitioning (#131)
LeetCode: #78, #46, #39, #79, #51, #131, #17
```

[↑ Back to Table of Contents](#table-of-contents)

---

### Pattern 14: 🏃 Greedy

```
USE WHEN:
  The best LOCAL choice at each step leads to the best GLOBAL result.
```

```
KEYWORDS:
  "Maximum events attended", "Minimum stops", "Jump game",
  "Schedule tasks", "Minimum intervals", "Earliest deadline"
```

**ALGORITHM:** → [Section C → Complete Algorithm Map](#complete-algorithm-map)

**TOOLS:**
→ [Section A: Arrays](#1-arrays)
→ [Section C: Sorting Algorithms](#sorting-algorithms) ← sort first almost always

```
LOGIC:
  Sort the data (almost always required).
  At each step pick the locally best option.
  Never go back. Never reconsider past choices.

BUILT ON: 🪣 Bucket + sorting
```

**WHY GREEDY WORKS (when it works):**

Some problems have a special property: the locally optimal choice at every step also happens to be globally optimal. Example: always attending the earliest-ending meeting leaves the most time for future meetings.

**WHY GREEDY FAILS (when it fails):**

When local best ≠ global best. Example: coin change with unusual denominations. Coins = [1, 3, 4], target = 6. Greedy picks 4 first → then 1+1 → 3 coins. But optimal is 3+3 → 2 coins. For coin change use DP not Greedy.

**RULE:** If unsure → use DP. DP is always correct. Only use Greedy when you can PROVE local = global.

```
DON'T USE WHEN:
  ✗ Local best ≠ global best (use 🧩 DP)
```

```javascript
function greedy(arr) {
  arr.sort(/* sort by relevant property */);
  let result = /* starting value */;
  for (const item of arr) {
    // pick best local option
    // update result
  }
  return result;
}
```

**Example — "Jump Game"**

```javascript
function canJump(nums) {
  let maxReach = 0;
  for (let i = 0; i < nums.length; i++) {
    if (i > maxReach) return false;             // cannot reach here
    maxReach = Math.max(maxReach, i + nums[i]); // update furthest reachable
  }
  return true;
}
```

**Example — "Buy and Sell Stock"**

```javascript
function maxProfit(prices) {
  let minPrice = Infinity, maxProfit = 0;
  for (const price of prices) {
    minPrice = Math.min(minPrice, price);        // track minimum seen
    maxProfit = Math.max(maxProfit, price - minPrice); // best profit so far
  }
  return maxProfit;
}
```

```
Trace canJump: [2,3,1,1,4]
  i=0: 0≤0 → ok → maxReach=max(0,0+2)=2
  i=1: 1≤2 → ok → maxReach=max(2,1+3)=4
  i=2: 2≤4 → ok → maxReach=max(4,2+1)=4
  i=3: 3≤4 → ok → maxReach=max(4,3+1)=4
  i=4: 4≤4 → ok → reached end → return true ✓

Trace canJump: [3,2,1,0,4]
  maxReach never goes past 3
  i=4: 4>3 → return false ✓
```

```
Practice:
□ Jump Game (#55)               □ Buy/Sell Stock (#121)
□ Non-overlapping Intervals (#435) □ Jump Game II (#45)
□ Gas Station (#134)            □ Task Scheduler (#621)
LeetCode: #55, #121, #435, #455, #45, #134, #621
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Level 4 — Specialist (Solves remaining 1%)

> Learn these in Days 39-48. Each has its own algorithm in Section C.

### ⛰️ Heap / Priority Queue — Pattern 15

```
USE WHEN:
  "Kth largest", "Top K frequent", "Running median"
  Any time you need repeated min/max access efficiently.
```

**ALGORITHM:** → [Section C → Heap Operations](#heap-operations)

**TOOLS:** → [Section A: Heap](#10-heap-priority-queue)

```
TIME: O(log n) insert/extract, O(1) peek
LeetCode: #215, #347, #295, #373
```

---

### 📚 Trie (Prefix Tree) — Pattern 16

```
USE WHEN:
  "Autocomplete", "Spell check", "Starts with prefix",
  "Word search in dictionary"
```

```
HOW IT WORKS:
  A tree where each NODE is a CHARACTER.
  Root → first letters. Each path = a word.
  "cat" and "car" share the path c→a then split.
TIME: O(m) per operation where m = word length
LeetCode: #208, #211, #212
```

---

### Prefix Sum — Pattern 17

```
USE WHEN:
  "Sum of range [i..j]", "Subarray sum equals K"
```

**ALGORITHM:** → [Section C → Prefix Sum](#prefix-sum)

```
TIME: O(n) build, O(1) query
LeetCode: #303, #560, #238
```

---

### Merge Intervals — Pattern 18

```
USE WHEN:
  "Overlapping meetings", "Merge time ranges"
```

```
APPROACH: Sort by start time. Merge if current start ≤ prev end.
TIME: O(n log n)
LeetCode: #56, #57, #252
```

---

### Union-Find — Pattern 19

```
USE WHEN:
  "Are nodes connected?", "Number of connected components"
```

**ALGORITHM:** → [Section C → Union-Find](#union-find)

```
TIME: Nearly O(1) per operation
LeetCode: #547, #684, #323
```

---

### Topological Sort — Pattern 20

```
USE WHEN:
  "Course prerequisites", "Build order", "Task scheduling"
```

**ALGORITHM:** → [Section C → Topological Sort](#topological-sort)

```
TIME: O(V+E)
LeetCode: #207, #210, #269
```

---

### Bit Manipulation — Pattern 21

```
USE WHEN:
  "Single number (XOR)", "Power of 2", "Count set bits"
```

```
KEY TRICKS:
  a XOR a = 0           (number XOR itself = 0)
  a XOR 0 = a           (number XOR 0 = number)
  n & (n-1) = 0         (check if power of 2)
TIME: O(1) or O(log n)
LeetCode: #136, #191, #338, #268
```

---

### Monotonic Stack — Pattern 22

```
USE WHEN:
  "Next greater element", "Next smaller element",
  "Largest rectangle in histogram", "Trapping rain water"
```

**ALGORITHM:** → [Section C → Monotonic Stack](#monotonic-stack)

```
TIME: O(n)
LeetCode: #496, #503, #84, #739, #42
```

[↑ Back to Table of Contents](#table-of-contents)
# Section E: Pattern Combos

> Most real interview problems use 2 patterns together.
> Recognizing the combo is a skill you build over time.

---

### Combo 1: 🔎 Memory + 🏆 Filter → "Remove Duplicates While Keeping Order"

**WHY TWO PATTERNS?**

[🔎 Memory](#pattern-5--memory-lookup--existence) answers: "Have I seen this item before?"
[🏆 Filter](#pattern-3--filter-predicate-selection) answers: "Should I keep this item?"

Together: "Keep only items I haven't seen before." Memory alone doesn't produce a list. Filter alone doesn't know what was seen. Together they solve it perfectly.

```javascript
function removeDuplicates(arr) {
  const seen = new Set();   // 🔎 Memory — tracks what was seen
  const result = [];        // 🏆 Filter — collects keepers
  for (const item of arr) {
    if (!seen.has(item)) {  // not seen before → keep
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

**WHY TWO PATTERNS?**

[🔎 Memory](#pattern-5--memory-lookup--existence) answers: "Have I seen this before?"
[🔍 Detective](#pattern-1--detective-boolean-validation) answers: "Is this true or false?"

Together: "Return true the moment I see something twice." Detective alone has no fast lookup. Memory alone doesn't produce true/false. Together they give early exit + fast lookup.

```javascript
function containsDuplicate(arr) {
  const seen = new Set();             // 🔎 Memory
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

**WHY TWO PATTERNS?**

[🪟 Sliding Window](#pattern-7--sliding-window) tracks the current window boundaries.
[🔎 Memory](#pattern-5--memory-lookup--existence) tracks which characters are currently IN the window.

Together: "Slide the window. Shrink it when a repeat enters." Window alone cannot check if a character is already inside. Memory alone cannot track a contiguous range.

```javascript
function lengthOfLongestSubstring(s) {
  const inWindow = new Set(); // 🔎 Memory — what's in current window
  let left = 0, best = 0;    // 🪟 Window boundaries

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

**WHY TWO PATTERNS?**

[🏗️ Stack](#pattern-9--stack-pattern) remembers previously seen opening brackets in LIFO order.
[🔍 Detective](#pattern-1--detective-boolean-validation) checks: does this closing bracket match what's on top?

Together: "Track open brackets. Return false the moment a mismatch is found." Stack alone doesn't produce true/false. Detective alone has no memory of what opened.

```javascript
function isValid(s) {
  const stack = [];           // 🏗️ Stack — remembers open brackets
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

**WHY TWO PATTERNS?**

[📊 Pointer Walker](#pattern-6--pointer-walker-two-pointers) moves two pointers from both ends.
[🪣 Bucket](#pattern-2--bucket-accumulation) tracks the best (maximum) area seen so far.

Together: "Compare all container candidates. Keep the best." Pointer Walker alone doesn't track max. Bucket alone doesn't know which pairs to compare.

```javascript
function maxArea(height) {
  let left = 0, right = height.length - 1; // 📊 Pointers from both ends
  let best = 0;                             // 🪣 Bucket for max area

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

### Combo 6: 🌳 Tree DFS + 🪣 Bucket → "Tree Path Problems"

**WHY TWO PATTERNS?**

[🌳 Tree DFS](#pattern-10--tree-dfs) explores every node recursively.
[🪣 Bucket](#pattern-2--bucket-accumulation) accumulates the answer (depth, sum, count) as recursion returns.

Together: "Go deep into the tree. Collect one value on the way back up." DFS alone just visits nodes. Bucket alone has no tree traversal. Together they compute answers across the whole tree.

```javascript
function maxDepth(node) {
  if (!node) return 0;                   // 🌳 base case
  const left  = maxDepth(node.left);
  const right = maxDepth(node.right);
  return 1 + Math.max(left, right);      // 🪣 accumulate max depth
}
// LeetCode: #104
```

---

### Combo 7: 🌊 BFS + 🔎 Memory → "Grid Shortest Path"

**WHY TWO PATTERNS?**

[🌊 BFS](#pattern-11--bfs) explores level by level — guarantees shortest path.
[🔎 Memory](#pattern-5--memory-lookup--existence) tracks visited nodes so you never revisit.

Together: "Find shortest path in a grid. Never revisit a cell." BFS alone loops forever on graphs with cycles. Memory alone has no traversal. Together they find shortest path safely.

```javascript
function bfsGrid(grid, start, end) {
  const visited = new Set([start]); // 🔎 Memory — visited cells
  const queue = [[start, 0]];       // 🌊 BFS — queue with distance
  while (queue.length > 0) {
    const [node, dist] = queue.shift();
    if (node === end) return dist;
    for (const neighbor of getNeighbors(node, grid)) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push([neighbor, dist + 1]);
      }
    }
  }
  return -1;
}
// LeetCode: #200, #994, #127
```

---

### Combo 8: 🔙 Backtracking + 🔍 Detective → "Generate Valid Configurations"

**WHY TWO PATTERNS?**

[🔙 Backtracking](#pattern-13--backtracking) generates all possible combinations.
[🔍 Detective](#pattern-1--detective-boolean-validation) prunes invalid branches early — returns false before going deeper.

Together: "Try all options. Skip invalid ones immediately." Backtracking alone generates everything including invalid. Detective alone has nothing to prune. Together they only explore valid paths.

```javascript
function generateValid(choices) {
  const result = [];
  function bt(current) {
    if (!isValid(current)) return; // 🔍 Detective — prune invalid
    if (/* complete */) { result.push([...current]); return; }
    for (const choice of choices) {
      current.push(choice);  // CHOOSE
      bt(current);           // EXPLORE
      current.pop();         // UN-CHOOSE
    }
  }
  bt([]);
  return result;
}
// LeetCode: #51 N-Queens, #37 Sudoku
```

---

```
Quick Combo Reference:

  Remove items based on lookup       → 🔎 Memory + 🏆 Filter
  Fast existence → true/false        → 🔎 Memory + 🔍 Detective
  Best contiguous + condition        → 🪟 Window + 🔎 Memory
  Matching/nesting + validation      → 🏗️ Stack + 🔍 Detective
  Sorted array optimization          → 📊 Pointer Walker + 🪣 Bucket
  Tree path / depth problems         → 🌳 DFS + 🪣 Bucket
  Grid shortest path                 → 🌊 BFS + 🔎 Memory
  Generate + filter valid configs    → 🔙 Backtracking + 🔍 Detective
```

[↑ Back to Table of Contents](#table-of-contents)
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

> Every advanced pattern is BUILT FROM Level 1 patterns.
> They are not separate skills. They are extensions.
> If Level 1 is weak → everything above breaks.

---

[🪟 Pattern 7: Sliding Window](#pattern-7--sliding-window)
= [📊 Pattern 6: Pointer Walker](#pattern-6--pointer-walker-two-pointers) (two index variables moving through data)
+ [🪣 Pattern 2: Bucket](#pattern-2--bucket-accumulation) (track best window total)

---

[🔪 Pattern 8: Binary Search](#pattern-8--binary-search)
= [🔍 Pattern 1: Detective](#pattern-1--detective-boolean-validation) (eliminate half each step — early exit logic)

---

[🏗️ Pattern 9: Stack](#pattern-9--stack-pattern)
= [🔍 Pattern 1: Detective](#pattern-1--detective-boolean-validation) (find mismatch → return false)
+ LIFO data structure (remembers what came before)

---

[🌳 Pattern 10: Tree DFS](#pattern-10--tree-dfs)
= [🪣 Pattern 2: Bucket](#pattern-2--bucket-accumulation) (accumulate answer while recursing back up)
+ Recursion (break into left and right sub-problems)

---

[🌊 Pattern 11: BFS](#pattern-11--bfs)
= [🪣 Pattern 2: Bucket](#pattern-2--bucket-accumulation) (accumulate level results)
+ Queue (process nodes in arrival order)

---

[🧩 Pattern 12: DP](#pattern-12--dynamic-programming)
= [🔎 Pattern 5: Memory](#pattern-5--memory-lookup--existence) (cache sub-problem answers so you never solve the same one twice)
+ [🪣 Pattern 2: Bucket](#pattern-2--bucket-accumulation) (build up to final answer from smaller answers)
+ Recursion (break into overlapping sub-problems)

---

[🔙 Pattern 13: Backtracking](#pattern-13--backtracking)
= [🌳 Pattern 10: Tree DFS](#pattern-10--tree-dfs) (go deep, then backtrack when stuck)
+ Trial-and-error (choose → explore → un-choose)

---

[🏃 Pattern 14: Greedy](#pattern-14--greedy)
= [🪣 Pattern 2: Bucket](#pattern-2--bucket-accumulation) (accumulate best local choices)
+ Sorting (to know which option is locally best at each step)

---

## What This Means Practically

```
LEVEL 1 IS THE FOUNDATION OF EVERYTHING.

  Cannot do 🪟 Sliding Window  → if 📊 Pointer Walker is weak
  Cannot do 🔪 Binary Search   → if 🔍 Detective is weak
  Cannot do 🌳 Tree DFS        → if 🪣 Bucket is weak
  Cannot do 🧩 DP              → if 🔎 Memory is weak
  Cannot do 🌊 BFS             → if 🪣 Bucket is weak
  Cannot do 🔙 Backtracking    → if 🌳 DFS is weak

BEFORE MOVING TO LEVEL 2:
  Pick any Level 1 pattern in under 30 seconds.
  Solve a Level 1 problem in under 10 minutes.
  Then and only then move to Level 2.
```

[↑ Back to Table of Contents](#table-of-contents)
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

## Complex Patterns Take Multiple Days

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

| Day | Pattern | Day Focus | Prepare A | Prepare C | Solve | Time | Space | LeetCode |
|-----|---------|-----------|-----------|-----------|-------|------|-------|----------|
| 1 | 🪣 Pattern 2: Bucket | Brand new pattern today. Learning to accumulate one value from the whole array. Template: start with 0, add each item, return result. | Arrays | Linear Search | Sum of array | O(n) | O(1) | — |
| 2 | 🔍 Pattern 1: Detective | Brand new pattern today. Learning to find ONE bad thing and stop immediately. Early exit saves time. Template: find bad → return false, survive → return true. | Arrays | Linear Search | Is array sorted? | O(n) | O(1) | #896 |
| 3 | 🏆 Pattern 3: Filter | Brand new pattern today. Learning to collect only items that pass a test. Template: empty winners list, test each item, push if passes. | Arrays | Linear Search | Get all even numbers | O(n) | O(n) | — |
| 4 | 🔄 Pattern 4: Transformer | Brand new pattern today. Learning to change every item. Result is always same length as input. Template: empty result list, push changed version of each item. | Arrays | Linear Search | Double every number | O(n) | O(n) | — |
| 5 | 🔎 Pattern 5: Memory Case 1 Basic | Brand new pattern today. Learning SET for instant yes/no lookup. This is Case 1 Basic — does something exist or repeat? SET replaces slow array search. | Set + Map | Hash-Based Search | Has duplicates? | O(n) | O(n) | #217 |
| 6 | 🔎 Pattern 5: Memory Case 1 Extended | Same Memory pattern. Now learning MAP for counting. Case 1 Extended — how many times does each item appear? MAP stores value → count. | Map | Hash-Based Search | Character frequency | O(n) | O(n) | #387 |
| 7 | 🔎 Pattern 5: Memory Case 2 | Same Memory pattern. Now learning the complement trick. Case 2 — find a PAIR of values that satisfy a formula. MAP stores value → index not count. Check before store. | Map | Hash-Based Search | Two Sum | O(n) | O(n) | #1 |
| 8 | 📊 Pattern 6: Pointer Walker | Not solving a pattern today — TOOL day. Learning JS Built-in Sort. This tool is required before solving Pattern 6 problems. Tomorrow you use it. | Arrays | JS Built-in Sort | Sort array of numbers | O(n log n) | O(n) | — |
| 9 | 📊 Pattern 6: Pointer Walker | Brand new pattern today. Learning Opposite Ends approach. Two pointers start at both ends and move toward each other. Applying to palindrome check. | Strings | Two Pointer Approaches | Is palindrome? | O(n) | O(1) | #125 |
| 10 | 🪣 Pattern 2: Bucket | Bucket — 2nd problem. Template is the same as Day 1. Different accumulation today — finding maximum instead of sum. Starting value is -Infinity not 0. | Arrays | Linear Search | Find maximum | O(n) | O(1) | — |
| 11 | 🔍 Pattern 1: Detective | Detective — 2nd problem. Template is the same as Day 2. Different bad condition today — check if any number is not positive. Early exit still applies. | Arrays | Linear Search | All numbers positive? | O(n) | O(1) | — |
| 12 | 🏆 Pattern 3: Filter | Filter — 2nd problem. Template is the same as Day 3. Different test today — keep only truthy values. Positive framing: keep if NOT falsy. | Arrays | Linear Search | Remove falsy values | O(n) | O(n) | — |
| 13 | 🔄 Pattern 4: Transformer | Transformer — 2nd problem. Template is the same as Day 4. Different formula today — apply temperature conversion to every item. | Arrays | Linear Search | Celsius to Fahrenheit | O(n) | O(n) | — |
| 14 | 📊 Pattern 6: Pointer Walker | Pointer Walker — 2nd problem. Yesterday was Opposite Ends. Today learning Same Direction Read/Write approach. Write pointer stays behind, read pointer scans ahead. | Arrays | Two Pointer Approaches | Reverse array in-place | O(n) | O(1) | #344 |

---

## Days 15-28: Level 2 Patterns

| Day | Pattern | Day Focus | Prepare A | Prepare C | Solve | Time | Space | LeetCode |
|-----|---------|-----------|-----------|-----------|-------|------|-------|----------|
| 15 | 🔎 Pattern 5: Memory + 🏆 Pattern 3: Filter | Reinforcing Week 1. Combining two patterns for the first time. Memory tracks what was seen. Filter decides what to keep. Together they remove duplicates while keeping order. | Arrays + Set | Hash-Based Search | Remove duplicates keeping order | O(n) | O(n) | #26 |
| 16 | 🔎 Pattern 5: Memory + 🔍 Pattern 1: Detective | Reinforcing Week 1. Second combo problem. Memory gives instant lookup. Detective gives early exit. Together they check for duplicate existence fast. | Arrays + Set | Hash-Based Search | Contains duplicate | O(n) | O(n) | #217 |
| 17 | 🪟 Pattern 7: Sliding Window | Brand new pattern today. Learning fixed window — window size k never changes. Add new item on right, remove old item on left. Track the best window seen. Built on Pointer Walker + Bucket. | Arrays | Sliding Window | Max sum subarray size K | O(n) | O(1) | — |
| 18 | 🪟 Pattern 7: Sliding Window | Sliding Window — 2nd problem. Yesterday was fixed window. Today learning variable window — size grows and shrinks based on condition. Window shrinks when a repeat enters. | Strings + Set | Sliding Window | Longest substring no repeat | O(n) | O(k) | #3 |
| 19 | 🔪 Pattern 8: Binary Search | Brand new pattern today. Learning to eliminate half the array each step. Data must be sorted first. Check middle — go left or right. Built on Detective elimination logic. | Arrays | Binary Search | Binary search sorted array | O(log n) | O(1) | #704 |
| 20 | 🔪 Pattern 8: Binary Search | Binary Search — 2nd problem. Yesterday found exact target. Today finding first occurrence. Template is same — condition changes to keep searching after finding. | Arrays | Binary Search | First occurrence in sorted array | O(log n) | O(1) | #34 |
| 21 | 🏗️ Pattern 9: Stack | PREP DAY — no problem solving today. Read Section A: Stack and Queue. Understand push, pop, LIFO order. Tomorrow you use Stack for Pattern 9. | **READ Stack + Queue** | Stack-based Matching | — PREP DAY — | — | — | — |
| 22 | 🏗️ Pattern 9: Stack | Brand new pattern today. Learning Stack-based Matching. Push open brackets. Pop and check when closing bracket arrives. Stack remembers what came before. | Stack | Stack-based Matching | Valid parentheses | O(n) | O(n) | #20 |
| 23 | 🏗️ Pattern 9: Stack | Stack — 2nd problem. Yesterday was matching pairs. Today learning Monotonic Stack variation. Stack stays decreasing. Pop when current breaks the order. | Stack | Monotonic Stack | Daily temperatures | O(n) | O(n) | #739 |
| 24 | 🌳 Pattern 10: Tree DFS | PREP DAY — no problem solving today. Read Section A: Tree. Understand nodes, left, right, null. Tomorrow you use Tree DFS for Pattern 10. | **READ Tree** | Tree Traversal DFS | — PREP DAY — | — | — | — |
| 25 | 🌳 Pattern 10: Tree DFS | Brand new pattern today. Learning Tree DFS skeleton. Base case null → return. Ask left → ask right → combine. Same skeleton works for almost every tree problem. | Tree | Tree Traversal DFS | Max depth binary tree | O(n) | O(h) | #104 |
| 26 | 🌳 Pattern 10: Tree DFS | Tree DFS — 2nd problem. Yesterday returned a number (depth). Today returning true/false (path sum exists). Same skeleton — only the combine step changes. | Tree | Tree Traversal DFS | Path sum | O(n) | O(h) | #112 |
| 27 | 🌊 Pattern 11: BFS | PREP DAY — no problem solving today. Read Section A: Graph. Understand nodes, edges, adjacency list. Tomorrow you use BFS for Pattern 11. | **READ Graph** | BFS Algorithm | — PREP DAY — | — | — | — |
| 28 | 🌊 Pattern 11: BFS | Brand new pattern today. Learning BFS — process level by level using a queue. Add root, process all at current level, add their children. Finds shortest path. | Graph + Queue | BFS Algorithm | Level order traversal | O(n) | O(w) | #102 |

---

## Days 29-38: Level 3 Patterns

| Day | Pattern | Day Focus | Prepare A | Prepare C | Solve | Time | Space | LeetCode |
|-----|---------|-----------|-----------|-----------|-------|------|-------|----------|
| 29 | 🧩 Pattern 12: DP | Brand new pattern today. Learning the 3-step recipe — Define, Relate, Base. dp[i] = number of ways to reach step i. Built on Memory + Bucket + Recursion. | Arrays | Memoization + Tabulation | Climbing Stairs | O(n) | O(1) | #70 |
| 30 | 🧩 Pattern 12: DP | DP — 2nd problem. Yesterday was additive formula. Today learning skip or take decision at each step. Same 3-step recipe applies. | Arrays | Memoization + Tabulation | House Robber | O(n) | O(1) | #198 |
| 31 | 🧩 Pattern 12: DP | DP — 3rd problem. More complex formula today. 2D decision — coin value and amount. Inner loop tries every coin at each amount. | Arrays | Memoization + Tabulation | Coin Change | O(n×amt) | O(amt) | #322 |
| 32 | 🔙 Pattern 13: Backtracking | Brand new pattern today. Learning Choose → Explore → Un-choose. Always in this exact order. Un-choose restores state so next path starts clean. | Arrays | Recursive Backtracking | All subsets | O(2^n) | O(n) | #78 |
| 33 | 🔙 Pattern 13: Backtracking | Backtracking — 2nd problem. Yesterday generated subsets — order did not matter. Today generating permutations — order matters. Every element can be picked at each position. | Arrays | Recursive Backtracking | All permutations | O(n!) | O(n) | #46 |
| 34 | 🏃 Pattern 14: Greedy | Brand new pattern today. Learning local best = global best. At each step pick the best option available. Never go back. Sort first almost always. | Arrays | Greedy Choice | Jump Game | O(n) | O(1) | #55 |
| 35 | 🏃 Pattern 14: Greedy | Greedy — 2nd problem. Yesterday maximized reachable index. Today maximizing profit. One pass — track minimum price seen, calculate max profit at each step. | Arrays | Greedy Choice | Buy and Sell Stock | O(n) | O(1) | #121 |
| 36 | 🏗️ Pattern 9: Stack | Stack — 3rd problem. Applying Stack to expression evaluation. Numbers go on stack. Operators pop two numbers, compute, push result back. | Stack | Stack-based Matching | Evaluate RPN | O(n) | O(n) | #150 |
| 37 | 🔙 Pattern 13: Backtracking | Backtracking — 3rd problem. Harder version today. Same number can be reused. Pass same index i not i+1 to allow reuse. Choose → Explore → Un-choose still applies. | Arrays | Recursive Backtracking | Combination Sum | O(2^n) | O(n) | #39 |
| 38 | 🏃 Pattern 14: Greedy | Greedy — 3rd problem. Interval scheduling today. Sort by end time. Always pick interval that ends earliest — leaves most room for future intervals. | Arrays | Greedy Choice | Non-overlapping Intervals | O(n log n) | O(1) | #435 |

---

## Days 39-48: New Algorithms (Level 4)

| Day | Pattern | Day Focus | Prepare A | Prepare C | Solve | Time | Space | LeetCode |
|-----|---------|-----------|-----------|-----------|-------|------|-------|----------|
| 39 | Level 4 — Prefix Sum | Brand new algorithm today. Build running totals once in O(n). Then answer any range sum in O(1). prefix[j+1] - prefix[i] gives sum of any range instantly. | Arrays | Prefix Sum | Range sum query | O(n)+O(1) | O(n) | #303 |
| 40 | Level 4 — Prefix Sum | Prefix Sum — 2nd problem. Harder variation today. Combine Prefix Sum with Memory Case 2. Check if complement of current sum was seen before. | Arrays | Prefix Sum | Subarray sum equals K | O(n) | O(n) | #560 |
| 41 | Level 4 — Monotonic Stack | Brand new algorithm today. Stack stays always decreasing. When current element breaks the order — pop and record answer. Used for next greater element problems. | Arrays | Monotonic Stack | Next greater element | O(n) | O(n) | #496 |
| 42 | Level 4 — Monotonic Stack | Monotonic Stack — 2nd problem. Same algorithm as Day 41. Different problem framing — find how many days until warmer temperature. Also read Linked List today for Day 43. | **READ Linked List** | Monotonic Stack | Daily temperatures | O(n) | O(n) | #739 |
| 43 | 📊 Pattern 6: Pointer Walker — Fast Slow | New approach inside Pointer Walker today. Fast pointer moves 2 steps, slow moves 1 step. When fast reaches end, slow is at middle. Works on Linked Lists only. | Linked List | Fast Slow Pointer | Find middle of list | O(n) | O(1) | #876 |
| 44 | 📊 Pattern 6: Pointer Walker — Fast Slow | Fast Slow — 2nd problem. Yesterday found the middle. Today detecting cycle entry point. When fast and slow meet — reset slow to head, both move 1 step until they meet again. | Linked List | Fast Slow Pointer | Linked list cycle II | O(n) | O(1) | #142 |
| 45 | 📊 Pattern 6: Pointer Walker — Dutch Flag | New approach inside Pointer Walker today. Three pointers — low, mid, high. Sort array with exactly 3 distinct values in one pass. Mid not incremented when swapping with high. | Arrays | Dutch National Flag | Sort colors | O(n) | O(1) | #75 |
| 46 | Level 4 — Heap | PREP DAY — no problem solving today. Read Section A: Heap and Deque. Understand min heap, insert bubbles up, extract bubbles down. Tomorrow you use Heap. | **READ Heap + Deque** | Heap Operations | — PREP DAY — | — | — | — |
| 47 | Level 4 — Heap | Brand new data structure today. MinHeap keeps smallest at top always. Insert O(log n). Peek O(1). Keep heap size at k — smallest of top k = kth largest. | Heap | Heap Operations | Kth largest element | O(n log k) | O(k) | #215 |
| 48 | Level 4 — Heap | Heap — 2nd problem. Yesterday found kth largest number. Today finding top k frequent elements. Build frequency map first, then use heap of size k on frequencies. | Heap | Heap Operations | Top K frequent elements | O(n log k) | O(n) | #347 |

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

| Day | Pattern | Day Focus | Prepare C | Solve | Time | Space |
|-----|---------|-----------|-----------|-------|------|-------|
| 49 | Interview Prep: Sorting | Learning only — never use in real problems. Compare adjacent elements, swap if wrong order. Each pass bubbles largest to its place. | Bubble Sort | Implement from scratch | O(n²) | O(1) |
| 50 | Interview Prep: Sorting | Learning only — never use in real problems. Find minimum in unsorted part, place it at front. Repeat for remaining unsorted section. | Selection Sort | Implement from scratch | O(n²) | O(1) |
| 51 | Interview Prep: Sorting | Use for nearly sorted data — best case O(n). Take each element, insert into correct position in sorted left portion. Like sorting cards in your hand. | Insertion Sort | Implement from scratch | O(n²) worst O(n) best | O(1) |
| 52 | Interview Prep: Sorting | Implement when interviewer asks. Divide array in half, sort each half, merge back in sorted order. Guaranteed O(n log n) always. Stable sort. | Merge Sort | Implement from scratch | O(n log n) | O(n) |
| 53 | Interview Prep: Sorting | Implement when interviewer asks. Pick pivot, put smaller left, larger right, recurse. Fastest in practice on average. Worst case O(n²) on bad pivot. | Quick Sort | Implement from scratch | O(n log n) avg | O(log n) |

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
  Give yourself 25 minutes maximum.
  If stuck after 25 minutes → look at the pattern hint only.
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
PATTERN    → 🪣 Pattern 2: Bucket
DAY FOCUS  → Brand new pattern today. Learning to accumulate one
             value from the whole array. Template: start with 0,
             add each item, return result.
PREPARE A  → Section A: Arrays
PREPARE C  → Section C: Linear Search
SOLVE      → Sum of array
TIME       → O(n)
SPACE      → O(1)

PHASE 1 → DECOMPOSE
  Input     → array of numbers, can be empty
  Output    → one number (the total sum)
  Rules     → add all numbers together
  Goal      → "I take an array and return the sum of all its numbers"
  Edge cases → [], [5], [-1,2,-3], [0,0,0]

PHASE 2 → IDENTIFY
  Return one value → 🪣 Pattern 2: Bucket
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
    if (arr.length === 0) return 0;
    let result = 0;
    for (let i = 0; i < arr.length; i++) {
      result += arr[i];
    }
    return result;
  }

PHASE 6 → EDGE CASES + BIG-O
  sumArray([])          → 0 ✓
  sumArray([5])         → 5 ✓
  sumArray([-1, 2, -3]) → -2 ✓
  sumArray([0, 0, 0])   → 0 ✓

  TIME  → O(n) — one loop through n items
  SPACE → O(1) — only one variable, no new arrays
```

[↑ Back to Table of Contents](#table-of-contents)
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

---

# Section M: Practice Questions — All Patterns

> Use this section when you want more problems for a pattern you already learned.
> Do NOT read this section upfront. Come here only when you need more practice.
> For each pattern: Easy → Medium → Hard in that order.

# Practice Questions — All Patterns

> For each pattern: Easy → Medium → Hard
> □ = not done yet   ✓ = completed
> Do Easy first. Move to Medium only when Easy feels automatic.

---

## Pattern 1: 🔍 Detective

```
WHAT TO LOOK FOR:
  Problem asks "Is it...?", "Are all...?", "Does every...?"
  Answer is always true or false.
  Find ONE bad thing → return false. Survive → return true.
```

**Easy — own problems (no LeetCode)**
```
□ Is the array sorted in ascending order?
  [1,3,5] → true    [1,5,3] → false

□ Are all numbers in the array positive?
  [1,2,3] → true    [1,-2,3] → false

□ Are all characters in the string digits?
  "1234" → true    "123a" → false

□ Does the string contain only lowercase letters?
  "hello" → true    "Hello" → false

□ Are all values in the array unique? (no duplicates)
  [1,2,3] → true    [1,2,2] → false

□ Is every number in the array even?
  [2,4,6] → true    [2,3,6] → false
```

**Medium — LeetCode**
```
□ #896  Monotonic Array
        Is array either entirely non-increasing or non-decreasing?

□ #1550 Three Consecutive Odds
        Are there 3 consecutive odd numbers in array?

□ #1827 Minimum Operations to Make Array Increasing
        (variant — check condition as you go)

□ #2000 Reverse Prefix of Word
        (variant — find position by condition)

□ #125  Valid Palindrome
        After removing non-alphanumeric, is it a palindrome?

□ #392  Is Subsequence
        Is string s a subsequence of string t?
```

**Hard**
```
□ #678  Valid Parenthesis String
        Can * be (, ) or empty to make it valid?

□ #1653 Minimum Deletions to Make String Balanced
        (variant — validate as you process)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 2: 🪣 Bucket

```
WHAT TO LOOK FOR:
  Problem asks for ONE value from the entire array.
  Sum, max, min, count, product, longest, shortest.
  Start with a value, update it as you loop.
```

**Easy — own problems**
```
□ Sum of all numbers in array
  [1,2,3,4] → 10

□ Find the maximum number
  [3,7,2,9] → 9

□ Find the minimum number
  [3,7,2,9] → 2

□ Count how many even numbers
  [1,2,3,4,5,6] → 3

□ Product of all numbers
  [1,2,3,4] → 24

□ Find the average of all numbers
  [1,2,3,4] → 2.5

□ Find the longest string in array
  ["hi","hello","hey"] → "hello"

□ Count how many strings start with vowel
  ["apple","banana","orange"] → 2
```

**Medium — LeetCode**
```
□ #53   Maximum Subarray (Kadane's)
        Find contiguous subarray with largest sum.

□ #121  Best Time to Buy and Sell Stock
        Find max profit from one buy and one sell.

□ #136  Single Number
        Every element appears twice except one. Find it.

□ #152  Maximum Product Subarray
        Find contiguous subarray with largest product.

□ #238  Product of Array Except Self
        For each position return product of all other elements.

□ #268  Missing Number
        Array has n distinct numbers in range [0,n]. Find missing.

□ #628  Maximum Product of Three Numbers
        Find three numbers with largest product.

□ #1480 Running Sum of 1D Array
        Return running sum at each position.
```

**Hard**
```
□ #42   Trapping Rain Water
        How much water can be trapped between bars?

□ #84   Largest Rectangle in Histogram
        Find largest rectangle in histogram.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 3: 🏆 Filter

```
WHAT TO LOOK FOR:
  Problem asks to "get all", "find all", "remove", "keep only".
  Result is a SHORTER list — only items that pass a test.
  Start with empty list, push items that pass.
```

**Easy — own problems**
```
□ Get all even numbers from array
  [1,2,3,4,5,6] → [2,4,6]

□ Remove all negative numbers
  [-1,2,-3,4] → [2,4]

□ Keep only strings longer than 3 characters
  ["hi","hello","hey","ok"] → ["hello","hey"]

□ Get all numbers greater than 5
  [1,3,7,9,2,8] → [7,9,8]

□ Remove all falsy values (0, false, null, undefined, "")
  [0,1,false,2,"",3] → [1,2,3]

□ Get all words starting with a vowel
  ["apple","banana","orange","grape"] → ["apple","orange"]

□ Keep only numbers divisible by 3
  [1,2,3,4,5,6,9] → [3,6,9]

□ Get all strings that contain the letter 'a'
  ["apple","cat","dog","banana"] → ["apple","cat","banana"]
```

**Medium — LeetCode**
```
□ #349  Intersection of Two Arrays
        Return elements that appear in both arrays.

□ #350  Intersection of Two Arrays II
        Return all matching elements including duplicates.

□ #1295 Find Numbers with Even Number of Digits
        Count numbers that have an even number of digits.

□ #2011 Final Value After Operations
        Filter and count specific operations.

□ #1313 Decompress Run-Length Encoded List
        Expand encoded list by repeating values.

□ #1431 Kids With the Greatest Number of Candies
        For each kid check if adding candies makes them greatest.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 4: 🔄 Transformer

```
WHAT TO LOOK FOR:
  Problem asks to "double each", "convert", "format", "extract".
  Result is SAME SIZE as input — every item is changed.
  Start with empty list, push transformed version of each item.
```

**Easy — own problems**
```
□ Double every number
  [1,2,3] → [2,4,6]

□ Square every number
  [1,2,3,4] → [1,4,9,16]

□ Convert Celsius to Fahrenheit (×9/5 + 32)
  [0,100] → [32,212]

□ Convert all strings to uppercase
  ["hello","world"] → ["HELLO","WORLD"]

□ Get the length of each string
  ["hi","hello","hey"] → [2,5,3]

□ Extract first letter of each word
  ["apple","banana","cherry"] → ["a","b","c"]

□ Add 10 to every number
  [1,2,3] → [11,12,13]

□ Reverse each string in array
  ["hello","world"] → ["olleh","dlrow"]
```

**Medium — LeetCode**
```
□ #977  Squares of Sorted Array
        Square each number and return sorted.

□ #1108 Defanging an IP Address
        Replace every "." with "[.]".

□ #1365 How Many Numbers Are Smaller Than the Current Number
        For each number count how many in array are smaller.

□ #1389 Create Target Array in the Given Order
        Insert elements at given indices.

□ #1920 Build Array from Permutation
        ans[i] = nums[nums[i]]

□ #2114 Maximum Number of Words Found in Sentences
        For each sentence count words, return max.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 5: 🔎 Memory — Case 1 Basic (SET)

```
WHAT TO LOOK FOR:
  "Have I seen this before?" → yes or no only.
  No counting. No position. Just existence.
  Use SET — O(1) lookup.
```

**Easy — own problems**
```
□ Does array have any duplicates?
  [1,2,3,1] → true

□ Are all values in array unique?
  [1,2,3,4] → true

□ Find the first repeated character in string
  "abcabd" → "a"

□ Remove duplicates keeping first occurrence
  [1,3,2,3,1] → [1,3,2]

□ Does array contain a specific value?
  [1,2,3], target=2 → true
```

**Medium — LeetCode**
```
□ #217  Contains Duplicate
        Return true if any value appears at least twice.

□ #219  Contains Duplicate II
        Duplicates within k positions of each other.

□ #349  Intersection of Two Arrays
        Elements that appear in both arrays.

□ #771  Jewels and Stones
        How many stones are jewels?

□ #1436 Destination City
        Find city that is not a source in any path.

□ #2351 First Letter to Appear Twice
        Find first character that appears twice.
```

**Hard**
```
□ #128  Longest Consecutive Sequence
        Find length of longest consecutive sequence.
        Put all in Set, find sequence starts.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 5: 🔎 Memory — Case 1 Extended (MAP count)

```
WHAT TO LOOK FOR:
  "How many times does each value appear?"
  You need the COUNT not just yes/no.
  Use MAP — stores value → count.
```

**Easy — own problems**
```
□ Character frequency of a string
  "hello" → {h:1, e:1, l:2, o:1}

□ Element frequency of an array
  [1,2,2,3,3,3] → {1:1, 2:2, 3:3}

□ Find most frequent element
  [1,2,2,3,3,3] → 3

□ Find elements appearing exactly twice
  [1,2,2,3,3,4] → [2,3]

□ Are two strings anagrams?
  "listen","silent" → true
```

**Medium — LeetCode**
```
□ #242  Valid Anagram
        Are two strings anagrams of each other?

□ #387  First Unique Character
        Find first character that appears only once.

□ #383  Ransom Note
        Can ransom note be built from magazine letters?

□ #49   Group Anagrams
        Group strings that are anagrams of each other.

□ #347  Top K Frequent Elements
        Return k most frequent elements.

□ #451  Sort Characters By Frequency
        Sort string by character frequency descending.

□ #1002 Find Common Characters
        Find characters common to all strings in array.

□ #1207 Unique Number of Occurrences
        Are all occurrence counts unique?
```

**Hard**
```
□ #692  Top K Frequent Words
        Return k most frequent words sorted.

□ #895  Maximum Frequency Stack
        Push and pop most frequent element.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 5: 🔎 Memory — Case 2 (MAP index)

```
WHAT TO LOOK FOR:
  Two values + one formula = find the pair.
  "Two numbers that add to target."
  Use MAP — stores value → index.
  CHECK complement first. STORE after.
```

**Easy — own problems**
```
□ Two Sum — return indices
  [2,7,11], target=9 → [0,1]

□ Two Sum — return true/false
  [1,4,7,2], target=9 → true

□ Find two numbers with difference K
  [1,5,3,4,2], k=2 → [1,3] or [2,4] or [3,5]
```

**Medium — LeetCode**
```
□ #1    Two Sum
        Return indices of two numbers that add to target.

□ #167  Two Sum II — Input Array is Sorted
        Same but array sorted — can use either Pattern 5 or 6.

□ #560  Subarray Sum Equals K
        Count subarrays with sum equal to K.
        Uses prefix sum + Case 2.

□ #1679 Max Number of K-Sum Pairs
        Count pairs that sum to K and remove them.

□ #1010 Pairs of Songs With Total Durations Divisible by 60
        Count pairs where sum of durations divisible by 60.

□ #2367 Number of Arithmetic Triplets
        Count triplets with equal difference.
```

**Hard**
```
□ #454  4Sum II
        Count quadruplets from 4 arrays that sum to 0.
        Use Case 2 twice.

□ #128  Longest Consecutive Sequence
        Find longest run of consecutive integers.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 6: 📊 Pointer Walker — Approach 1 (Opposite Ends)

```
WHAT TO LOOK FOR:
  Compare from both ends. Move toward middle.
  Palindrome, pair in sorted, container problems.
```

**Easy**
```
□ Is string a palindrome?
  "racecar" → true   "hello" → false

□ Reverse an array in-place
  [1,2,3,4,5] → [5,4,3,2,1]

□ Check if array reads same forwards and backwards
  [1,2,3,2,1] → true
```

**Medium — LeetCode**
```
□ #125  Valid Palindrome
        After cleaning non-alphanumeric, is it palindrome?

□ #167  Two Sum II — Input Array is Sorted
        Find pair that sums to target in sorted array.

□ #344  Reverse String
        Reverse character array in-place.

□ #345  Reverse Vowels of a String
        Swap only vowels, keep consonants in place.

□ #680  Valid Palindrome II
        Can you make palindrome by removing at most one char?

□ #977  Squares of Sorted Array
        Square each number, return sorted array.

□ #11   Container With Most Water
        Find two lines that hold most water.
```

**Hard**
```
□ #42   Trapping Rain Water
        How much water trapped between bars?

□ #15   3Sum
        All triplets that sum to zero.

□ #16   3Sum Closest
        Find triplet sum closest to target.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 6: 📊 Pointer Walker — Approach 2 (Same Direction R/W)

```
WHAT TO LOOK FOR:
  Modify array IN-PLACE.
  Write pointer builds result behind read pointer.
  Keep certain elements, skip others.
```

**Easy**
```
□ Move all zeroes to end keeping order
  [0,1,0,3,12] → [1,3,12,0,0]

□ Remove all occurrences of value in-place
  [3,2,2,3], val=3 → [2,2], return 2
```

**Medium — LeetCode**
```
□ #283  Move Zeroes
        Move all zeroes to end, keep order of non-zeroes.

□ #27   Remove Element
        Remove all occurrences of val in-place.

□ #26   Remove Duplicates from Sorted Array
        Remove duplicates in-place, return new length.

□ #80   Remove Duplicates from Sorted Array II
        Each element can appear at most twice.

□ #88   Merge Sorted Array
        Merge two sorted arrays into first one in-place.

□ #905  Sort Array By Parity
        Move even numbers to front, odd to back.

□ #1089 Duplicate Zeros
        Shift array right when zero encountered.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 6: 📊 Pointer Walker — Approach 3 (Fast and Slow)

```
WHAT TO LOOK FOR:
  Linked List problems.
  Find middle, detect cycle, find cycle entry.
  Slow moves 1 step, fast moves 2 steps.
```

**Medium — LeetCode**
```
□ #876  Middle of the Linked List
        Find middle node. If two middles return second.

□ #141  Linked List Cycle
        Does linked list have a cycle?

□ #142  Linked List Cycle II
        Return node where cycle begins.

□ #234  Palindrome Linked List
        Is linked list a palindrome?

□ #143  Reorder List
        Reorder list: first, last, second, second-last...

□ #202  Happy Number
        A number is happy if process eventually reaches 1.
        Use fast/slow on the sequence.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 6: 📊 Pointer Walker — Approach 4 (Sorted Array / 3Sum)

```
WHAT TO LOOK FOR:
  Find combinations of multiple numbers.
  Sort first. Fix one. Opposite ends for rest.
```

**Medium — LeetCode**
```
□ #15   3Sum
        All unique triplets that sum to zero.

□ #16   3Sum Closest
        Find triplet with sum closest to target.

□ #18   4Sum
        All unique quadruplets that sum to target.

□ #259  3Sum Smaller
        Count triplets with sum less than target.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 7: 🪟 Sliding Window

```
WHAT TO LOOK FOR:
  Contiguous subarray or substring.
  Best / longest / shortest window satisfying condition.
  Fixed size K or variable size.
```

**Easy**
```
□ Max sum of subarray of size K
  [1,3,2,6,-1,4,1,8,2], k=5 → 18

□ Min sum of subarray of size K
  [2,1,5,1,3,2], k=3 → 6
```

**Medium — LeetCode**
```
□ #3    Longest Substring Without Repeating Characters
        Longest window with all unique characters.

□ #209  Minimum Size Subarray Sum
        Smallest subarray with sum ≥ target.

□ #438  Find All Anagrams in a String
        All start positions where anagram of p found in s.

□ #567  Permutation in String
        Does s2 contain a permutation of s1?

□ #1004 Max Consecutive Ones III
        Max ones after flipping at most k zeros.

□ #1208 Get Equal Substrings Within Budget
        Longest substring where total char change ≤ maxCost.

□ #1343 Number of Sub-arrays of Size K and Avg ≥ Threshold
        Count subarrays of size k with average ≥ threshold.
```

**Hard**
```
□ #76   Minimum Window Substring
        Smallest window in s containing all chars of t.

□ #239  Sliding Window Maximum
        Max of every window of size k. Use Deque.

□ #424  Longest Repeating Character Replacement
        Longest substring with same char after k replacements.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 8: 🔪 Binary Search

```
WHAT TO LOOK FOR:
  Sorted data. Find position. Eliminate half each step.
  Also works on answer ranges — "minimum valid X".
```

**Easy**
```
□ Search for target in sorted array
  [1,3,5,7,9], target=5 → index 2

□ Find index to insert target to keep sorted
  [1,3,5,7], target=4 → index 2
```

**Medium — LeetCode**
```
□ #704  Binary Search
        Classic search in sorted array.

□ #35   Search Insert Position
        Find index where target belongs.

□ #34   Find First and Last Position
        Find leftmost and rightmost index of target.

□ #33   Search in Rotated Sorted Array
        Array was rotated. Find target.

□ #153  Find Minimum in Rotated Sorted Array
        Find minimum after rotation.

□ #162  Find Peak Element
        Find peak — element greater than neighbors.

□ #69   Sqrt(x)
        Integer square root using binary search on answer.

□ #74   Search a 2D Matrix
        Binary search in sorted 2D matrix.

□ #278  First Bad Version
        Find first bad version with minimal API calls.
```

**Hard**
```
□ #4    Median of Two Sorted Arrays
        Find median without merging.

□ #1011 Capacity to Ship Packages Within D Days
        Binary search on the answer (capacity).

□ #875  Koko Eating Bananas
        Binary search on eating speed.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 9: 🏗️ Stack — Template A (Stack Matching)

```
WHAT TO LOOK FOR:
  Matching pairs of opening and closing.
  What came just before matters.
  LIFO — last opened must be first closed.
```

**Medium — LeetCode**
```
□ #20   Valid Parentheses
        Are all brackets matched and in correct order?

□ #155  Min Stack
        Stack that also returns minimum in O(1).

□ #150  Evaluate Reverse Polish Notation
        Evaluate expression in RPN format.

□ #394  Decode String
        Decode k[encoded_string] format.

□ #856  Score of Parentheses
        Calculate score of balanced parentheses string.

□ #1047 Remove All Adjacent Duplicates in String
        Repeatedly remove adjacent duplicate pairs.

□ #1614 Maximum Nesting Depth of Parentheses
        Find max depth of nested parentheses.
```

**Hard**
```
□ #224  Basic Calculator
        Evaluate expression with +, -, parentheses.

□ #227  Basic Calculator II
        Evaluate expression with +, -, *, /.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 9: 🏗️ Stack — Template B (Monotonic Stack)

```
WHAT TO LOOK FOR:
  Next greater / smaller element for each position.
  Stack stays always increasing or always decreasing.
  Pop when current element breaks the order.
```

**Medium — LeetCode**
```
□ #496  Next Greater Element I
        For each element in nums1 find next greater in nums2.

□ #503  Next Greater Element II
        Circular array — next greater element.

□ #739  Daily Temperatures
        Days to wait for a warmer temperature.

□ #901  Online Stock Span
        How many consecutive days stock price was ≤ today?

□ #1019 Next Greater Node in Linked List
        Find next greater value for each node.
```

**Hard**
```
□ #84   Largest Rectangle in Histogram
        Find largest rectangle in histogram bars.

□ #42   Trapping Rain Water
        Calculate water trapped between bars.

□ #85   Maximal Rectangle
        Largest rectangle containing only 1s in matrix.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 10: 🌳 Tree DFS

```
WHAT TO LOOK FOR:
  Tree problems that need to explore every node.
  Return a value computed from subtrees.
  Base case: null → return something.
  Combine left + right + current.
```

**Easy**
```
□ Max depth of binary tree
  → return depth of deepest leaf

□ Count total nodes in tree
  → count = 1 + left count + right count
```

**Medium — LeetCode**
```
□ #104  Maximum Depth of Binary Tree
        Longest path from root to leaf.

□ #226  Invert Binary Tree
        Mirror the tree — swap left and right.

□ #112  Path Sum
        Does any root-to-leaf path sum equal target?

□ #100  Same Tree
        Are two trees identical in structure and values?

□ #101  Symmetric Tree
        Is tree a mirror of itself?

□ #543  Diameter of Binary Tree
        Longest path between any two nodes.

□ #572  Subtree of Another Tree
        Is one tree a subtree of another?

□ #617  Merge Two Binary Trees
        Merge by summing overlapping nodes.

□ #700  Search in a Binary Search Tree
        Find node with given value in BST.

□ #235  Lowest Common Ancestor of BST
        Find LCA in a BST.
```

**Hard**
```
□ #124  Binary Tree Maximum Path Sum
        Maximum sum path between any two nodes.

□ #236  Lowest Common Ancestor of Binary Tree
        Find LCA in any binary tree.

□ #98   Validate Binary Search Tree
        Is tree a valid BST?
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 11: 🌊 BFS — Template A (Level Order)

```
WHAT TO LOOK FOR:
  Process tree level by level.
  Collect results per level.
  Find nodes at specific depth.
```

**Medium — LeetCode**
```
□ #102  Binary Tree Level Order Traversal
        Return nodes grouped by level.

□ #107  Binary Tree Level Order Traversal II
        Bottom-up level order traversal.

□ #103  Binary Tree Zigzag Level Order Traversal
        Alternate left-to-right and right-to-left.

□ #199  Binary Tree Right Side View
        Rightmost node at each level.

□ #637  Average of Levels in Binary Tree
        Average value at each level.

□ #513  Find Bottom Left Tree Value
        Leftmost value of last row.

□ #116  Populating Next Right Pointers
        Connect each node to its next right node.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 11: 🌊 BFS — Template B (Graph BFS)

```
WHAT TO LOOK FOR:
  Shortest path in unweighted graph or grid.
  Explore all reachable nodes.
  Always mark visited to avoid cycles.
```

**Medium — LeetCode**
```
□ #200  Number of Islands
        Count connected groups of land.

□ #994  Rotting Oranges
        Minimum minutes for all oranges to rot.

□ #542  01 Matrix
        Distance from each cell to nearest 0.

□ #1091 Shortest Path in Binary Matrix
        Shortest clear path from top-left to bottom-right.

□ #133  Clone Graph
        Deep copy a graph.

□ #695  Max Area of Island
        Largest connected group of land.
```

**Hard**
```
□ #127  Word Ladder
        Fewest transformations from beginWord to endWord.

□ #126  Word Ladder II
        All shortest transformation sequences.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 11: 🌊 BFS — Template C (Topological Sort)

```
WHAT TO LOOK FOR:
  Tasks with prerequisites.
  Valid ordering. Detect cycles.
  inDegree count drives the queue.
```

**Medium — LeetCode**
```
□ #207  Course Schedule
        Can you finish all courses given prerequisites?

□ #210  Course Schedule II
        Return valid order to take all courses.

□ #310  Minimum Height Trees
        Find roots that give minimum height trees.

□ #1557 Minimum Number of Vertices to Reach All Nodes
        Find nodes with no incoming edges.
```

**Hard**
```
□ #269  Alien Dictionary
        Find character ordering from sorted alien words.

□ #444  Sequence Reconstruction
        Is there unique shortest supersequence?
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 12: 🧩 DP — Approach 1 (Memoization) + Approach 2 (Tabulation)

```
WHAT TO LOOK FOR:
  "How many ways", "minimum cost", "maximum value".
  Same sub-problem appears multiple times.
  3 steps: DEFINE dp[i], RELATE to smaller, BASE cases.
```

**Easy**
```
□ #70   Climbing Stairs
        1 or 2 steps at a time. How many ways to reach top?

□ #509  Fibonacci Number
        Return nth Fibonacci number.
```

**Medium — LeetCode**
```
□ #198  House Robber
        Max money without robbing adjacent houses.

□ #213  House Robber II
        Houses in a circle — cannot rob first and last both.

□ #322  Coin Change
        Fewest coins to make amount.

□ #300  Longest Increasing Subsequence
        Length of longest strictly increasing subsequence.

□ #139  Word Break
        Can string be segmented into dictionary words?

□ #416  Partition Equal Subset Sum
        Can array be split into two equal sum subsets?

□ #1143 Longest Common Subsequence
        Length of LCS of two strings.

□ #516  Longest Palindromic Subsequence
        Length of longest palindromic subsequence.

□ #62   Unique Paths
        How many paths from top-left to bottom-right?

□ #64   Minimum Path Sum
        Minimum sum path from top-left to bottom-right.
```

**Hard**
```
□ #72   Edit Distance
        Minimum operations to convert one string to another.

□ #312  Burst Balloons
        Maximum coins from bursting balloons.

□ #10   Regular Expression Matching
        Match string with pattern containing . and *.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 13: 🔙 Backtracking

```
WHAT TO LOOK FOR:
  Generate ALL valid combinations, permutations, configurations.
  CHOOSE → EXPLORE → UN-CHOOSE always in this order.
```

**Medium — LeetCode**
```
□ #78   Subsets
        All possible subsets of array.

□ #90   Subsets II
        All subsets with possible duplicates in input.

□ #46   Permutations
        All possible permutations of array.

□ #47   Permutations II
        All unique permutations with duplicates in input.

□ #39   Combination Sum
        All combinations that sum to target (reuse allowed).

□ #40   Combination Sum II
        All combinations that sum to target (no reuse).

□ #77   Combinations
        All combinations of k numbers from 1 to n.

□ #17   Letter Combinations of Phone Number
        All possible letter combinations from phone digits.

□ #22   Generate Parentheses
        All valid combinations of n pairs of parentheses.

□ #131  Palindrome Partitioning
        All ways to partition string into palindromes.
```

**Hard**
```
□ #51   N-Queens
        Place n queens so none attack each other.

□ #52   N-Queens II
        Count distinct solutions to N-Queens.

□ #37   Sudoku Solver
        Fill in the Sudoku board.

□ #79   Word Search
        Does word exist in grid?
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Pattern 14: 🏃 Greedy

```
WHAT TO LOOK FOR:
  Local best = global best.
  Sort first almost always.
  Pick best available option at each step.
  Never go back.
```

**Easy**
```
□ #455  Assign Cookies
        Greedily assign largest cookie to most greedy child.

□ #860  Lemonade Change
        Can you give correct change to every customer?
```

**Medium — LeetCode**
```
□ #55   Jump Game
        Can you reach the last index?

□ #45   Jump Game II
        Minimum jumps to reach last index.

□ #121  Best Time to Buy and Sell Stock
        Maximum profit from one transaction.

□ #122  Best Time to Buy and Sell Stock II
        Maximum profit from multiple transactions.

□ #435  Non-overlapping Intervals
        Minimum intervals to remove to make rest non-overlapping.

□ #452  Minimum Number of Arrows to Burst Balloons
        Minimum arrows to burst all balloon intervals.

□ #134  Gas Station
        Can you complete the circuit?

□ #738  Monotone Increasing Digits
        Largest number ≤ n with monotone increasing digits.

□ #1005 Maximize Sum of Array After K Negations
        Maximize sum by negating k elements.
```

**Hard**
```
□ #135  Candy
        Minimum candies: each child gets more than lower-rated neighbor.

□ #621  Task Scheduler
        Minimum intervals to execute all tasks with cooldown.

□ #630  Course Schedule III
        Maximum courses you can take given deadlines.
```

[↑ Back to Table of Contents](#table-of-contents)