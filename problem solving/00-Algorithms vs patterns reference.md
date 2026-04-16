# Algorithms vs Patterns — Complete Reference
## Which algorithms are free, which are tools, which are specialists

---

# Table of Contents

- [The Statement — What It Really Means](#the-statement--what-it-really-means)
- [Group 1 — Algorithms FREE Inside Pattern Templates](#group-1--algorithms-free-inside-pattern-templates)
- [Group 2 — Sorting Algorithms — Pre-Processing Tools](#group-2--sorting-algorithms--pre-processing-tools)
- [Group 3 — Specialist Algorithms — Level 4 Only](#group-3--specialist-algorithms--level-4-only)
- [The Complete Picture](#the-complete-picture)
- [How to Use This in Practice](#how-to-use-this-in-practice)

---

# The Statement — What It Really Means

> "When you pick a pattern, the algorithm comes FREE inside the template."

This statement is **true for Level 1, 2, and 3 patterns only.**

It means: when you identify the correct pattern, you already know exactly what algorithm to use — it is built into the template. You do not need to think separately about the algorithm.

**But this is NOT true for every algorithm in the blueprint.**

There are 3 groups:

```
Group 1 → FREE inside pattern templates     (Levels 1–3)
Group 2 → Pre-processing TOOLS              (used before patterns)
Group 3 → Specialist algorithms             (Level 4 only)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Group 1 — Algorithms FREE Inside Pattern Templates

> These come automatically when you pick the pattern.
> You never need to think about the algorithm separately.

| Algorithm | Lives In | How It Appears in Template |
|-----------|----------|---------------------------|
| Linear Scan | 🔍 Detective, 🪣 Bucket, 🏆 Filter, 🔄 Transformer | `for (let i = 0; i < arr.length; i++)` |
| Running Total | 🪣 Bucket | `result = combine(result, arr[i])` |
| Kadane's Algorithm | 🪣 Bucket | `currentSum = Math.max(arr[i], currentSum + arr[i])` |
| Hashing | 🔎 Memory | `seen.has(x)`, `freq.set(x, ...)` |
| Two Pointer Technique | 📊 Pointer Walker | `let left = 0, right = arr.length - 1` |
| Floyd's Cycle Detection | 📊 Pointer Walker | `slow = slow.next; fast = fast.next.next` |
| Sliding Window Technique | 🪟 Sliding Window | `windowSum += arr[i] - arr[i - k]` |
| Binary Search Algorithm | 🔪 Binary Search | `let lo = 0, hi = arr.length - 1` |
| Stack-based Matching | 🏗️ Stack | `stack.push(item); stack.pop()` |
| Monotonic Stack | 🏗️ Stack | `while stack not empty and condition: pop` |
| Inorder Traversal | 🌳 Tree DFS | `dfs(left) → process → dfs(right)` |
| Preorder Traversal | 🌳 Tree DFS | `process → dfs(left) → dfs(right)` |
| Postorder Traversal | 🌳 Tree DFS | `dfs(left) → dfs(right) → process` |
| Breadth-First Search | 🌊 BFS | `queue.push(node); queue.shift()` |
| Memoization | 🧩 DP | `memo[i] = result; if memo[i] exists return it` |
| Tabulation | 🧩 DP | `dp[i] = formula using dp[i-1], dp[i-2]` |
| Recursive Backtracking | 🔙 Backtracking | `CHOOSE → EXPLORE → UN-CHOOSE` |
| Greedy Choice | 🏃 Greedy | `sort → pick best now → never look back` |
| Activity Selection | 🏃 Greedy | `sort by end time → pick compatible` |

### What "FREE" means in practice:

```
Example:
  Problem: "Find the longest substring without repeating characters"

  Step 1 → You identify the pattern: 🪟 Sliding Window + 🔎 Memory
  Step 2 → You pick up the template

  The template already contains:
    - The sliding window technique (left/right pointers)
    - The hashing technique (Set to track characters in window)

  You never thought "what algorithm do I use?"
  Picking the pattern gave you the algorithm for FREE.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Group 2 — Sorting Algorithms — Pre-Processing Tools

> These are NOT inside any pattern template.
> They are used BEFORE you apply a pattern.
> They do not come free — you decide to use them as a separate step.

| Algorithm | Time | When to Use |
|-----------|------|-------------|
| Bubble Sort | O(n²) | Learning only — never in real problems |
| Selection Sort | O(n²) | Learning only — never in real problems |
| Insertion Sort | O(n²) best O(n) | Nearly sorted data only |
| Merge Sort | O(n log n) | When you need stable, guaranteed sort |
| Quick Sort | O(n log n) avg | When you need fast in-place sort |
| JS .sort() | O(n log n) | 99% of interview problems — just use this |

### How sorting fits into the process:

```
CORRECT FLOW:

  Step 1 → SORT the data first (pre-processing)
  Step 2 → NOW pick a pattern

  Example:
    Problem: "Two Sum in a sorted array"
    → Data is already sorted
    → Pick pattern: 📊 Pointer Walker
    → Algorithm comes FREE inside Pointer Walker template

  Example:
    Problem: "Find if any two numbers sum to target"
    → Data is NOT sorted
    → Option A: Sort first → then use 📊 Pointer Walker
    → Option B: Skip sort → use 🔎 Memory (Set) directly — faster!
```

### Why sorting is a tool, not a pattern:

```
A pattern answers: "What SHAPE is this problem?"
A sorting algorithm answers: "How do I ORDER this data first?"

Sorting never solves a problem by itself.
It prepares the data so a pattern CAN solve it.

Pattern  = the strategy
Sorting  = the preparation
```

### The rule:

```
❌ WRONG: "I'll use Merge Sort to solve this problem"
✅ RIGHT: "I'll sort first, then use 📊 Pointer Walker to solve this"

Always say: "I'll sort, THEN use [pattern]"
Never say: "I'll use [sort] to solve it"
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Group 3 — Specialist Algorithms — Level 4 Only

> These are in Section C of the blueprint as reference.
> They are NOT free inside any Level 1–3 pattern template.
> You learn these when you encounter Level 4 problems.

| Algorithm | Lives In | What It Solves |
|-----------|----------|----------------|
| Dijkstra's Algorithm | Level 4 — ⛰️ Heap + 🏃 Greedy | Shortest path in WEIGHTED graph |
| Topological Sort | Level 4 — 🌊 BFS variant | Task ordering with dependencies |
| Union-Find (Disjoint Set) | Level 4 — Specialist | Are nodes connected? Group membership |

### Why these are different:

```
Dijkstra's:
  You cannot just "pick the Greedy pattern" and get Dijkstra's free.
  Dijkstra's requires BOTH a Greedy approach AND a Min-Heap data structure.
  The Greedy pattern template alone does not give you Dijkstra's.
  You need to know this algorithm explicitly.

Topological Sort:
  It uses BFS internally but it is NOT just BFS.
  It requires tracking in-degrees of nodes.
  The BFS pattern template alone does not give you Topological Sort.

Union-Find:
  This is a completely standalone data structure + algorithm.
  It does not live inside any of the 14 patterns.
  It is its own thing entirely.
```

### When do you need these?

```
Dijkstra's       → "Shortest path" + graph has WEIGHTS on edges
                   Keywords: "weighted", "cost", "minimum distance"
                   LeetCode: #743, #1631, #787

Topological Sort → "Order tasks" + tasks have DEPENDENCIES
                   Keywords: "prerequisites", "build order", "course schedule"
                   LeetCode: #207, #210, #269

Union-Find       → "Are these connected?" + need to merge groups
                   Keywords: "connected components", "redundant connection"
                   LeetCode: #547, #684, #323
```

[↑ Back to Table of Contents](#table-of-contents)

---

# The Complete Picture

```
┌─────────────────────────────────────────────────────────────────┐
│                    ALL ALGORITHMS                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  GROUP 1 — FREE inside pattern templates (Level 1–3)           │
│  ─────────────────────────────────────────────────             │
│  Linear Scan, Running Total, Kadane's, Hashing,                │
│  Two Pointer, Floyd's Cycle, Sliding Window,                   │
│  Binary Search, Stack Matching, Monotonic Stack,               │
│  Inorder/Preorder/Postorder, BFS, Memoization,                 │
│  Tabulation, Recursive Backtracking, Greedy Choice             │
│                                                                 │
│  → Pick the pattern → algorithm is FREE                        │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  GROUP 2 — SORTING TOOLS (pre-processing, used before pattern) │
│  ─────────────────────────────────────────────────────────     │
│  Bubble Sort, Selection Sort, Insertion Sort,                  │
│  Merge Sort, Quick Sort, JS .sort()                            │
│                                                                 │
│  → Sort FIRST → then pick a pattern                            │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  GROUP 3 — SPECIALIST algorithms (Level 4, learn separately)   │
│  ─────────────────────────────────────────────────────────     │
│  Dijkstra's Algorithm                                          │
│  Topological Sort                                              │
│  Union-Find                                                    │
│                                                                 │
│  → Learn these explicitly when you reach Level 4               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

# How to Use This in Practice

### When you see a problem, ask in this order:

```
STEP 1 — Does the data need to be sorted first?
  YES → Sort it using .sort((a,b) => a-b), then go to Step 2
  NO  → Go to Step 2

STEP 2 — Which pattern is this? (Level 1–3)
  → Pick the pattern
  → The algorithm is FREE inside the template
  → Write the code

STEP 3 — Is this a special case?
  Weighted graph shortest path? → Dijkstra's (Level 4)
  Task ordering with deps?      → Topological Sort (Level 4)
  Group connectivity?           → Union-Find (Level 4)
```

### The corrected statement:

```
ORIGINAL (incomplete):
  "When you pick a pattern, the algorithm comes FREE inside the template."

COMPLETE VERSION:
  "When you pick a Level 1–3 pattern, the algorithm comes FREE
   inside the template.

   Sorting algorithms are PRE-PROCESSING tools used before patterns.

   Dijkstra's, Topological Sort, and Union-Find are Level 4 specialists
   that must be learned explicitly — they do not come free."
```

### Bottom line:

```
80% of problems  → Pick a Level 1 pattern → algorithm is FREE
15% of problems  → May need to sort first, then pick a pattern
4%  of problems  → Pick a Level 2–3 pattern → algorithm is FREE
1%  of problems  → Need a Level 4 specialist algorithm explicitly
```

[↑ Back to Table of Contents](#table-of-contents)