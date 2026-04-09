# How to Get the Perfect Answer in One Shot
## A Guide for Getting Exactly What You Want from AI

---

# Table of Contents

- [The Problem](#the-problem)
- [The Solution: 2-Prompt Approach](#the-solution-2-prompt-approach)
  - [Prompt 1: Discovery — "What exists?"](#prompt-1-discovery)
  - [Prompt 2: Full Guide — "Now teach me all of it"](#prompt-2-full-guide)
- [How It Works (The Blueprint Example)](#how-it-works-the-blueprint-example)
- [The 1-Prompt Approach (When You Already Know the Scope)](#the-1-prompt-approach)
  - [The 6 Elements](#the-6-elements)
  - [The Reusable Template](#the-reusable-template)
- [Real Examples](#real-examples)
  - [Example 1: Learning Problem Solving (New Topic)](#example-1-learning-problem-solving)
  - [Example 2: Learning React (Partial Knowledge)](#example-2-learning-react)
  - [Example 3: Learning SQL (Complete Beginner)](#example-3-learning-sql)
  - [Example 4: System Design (You Know the Scope)](#example-4-system-design)
  - [Example 5: Any New Topic You Know Nothing About](#example-5-any-new-topic)
- [When to Use Which Approach](#when-to-use-which-approach)
- [Common Mistakes to Avoid](#common-mistakes-to-avoid)
- [Quick Cheat Sheet](#quick-cheat-sheet)

---

# The Problem

When you want to learn something new, you face a chicken-and-egg problem:

```
To get a good answer → you need to tell the AI what topics to cover
To know what topics to cover → you need to already know the subject
But you DON'T know the subject → that's WHY you're asking
```

So what happens:

```
You say:     "Give me a complete guide on problem solving"
AI guesses:  gives you something incomplete or wrong focus
You say:     "No, add this"
AI rebuilds: still not quite right
You say:     "No, I also need this"
...10 rounds later you finally get what you wanted
```

**The fix is simple: Ask TWO questions instead of ONE.**

---

# The Solution: 2-Prompt Approach

---

## Prompt 1: Discovery

> **Purpose:** You don't know what topics exist. So ASK the AI to tell you.

**The template:**

```
I want to learn [TOPIC] using [LANGUAGE/TOOL].
I'm starting from [YOUR CURRENT LEVEL].
I don't know what topics exist in this field.

Tell me:
1. What are ALL the topics, sub-topics, and concepts that
   exist in this field?
2. Organize them in the order I should learn them
3. How long each topic takes (days/weeks)
4. What depends on what (what must I learn before what?)
5. Group them into phases or milestones

Don't teach me anything yet. Just show me the COMPLETE MAP
of everything that exists so I can see the full picture.
```

**What you get back:** A table of contents of the entire field. Now you can SEE what exists, how big it is, and how long it takes.

**Now you know the scope — without needing to know it beforehand.**

---

## Prompt 2: Full Guide

> **Purpose:** Now that you see the map, ask for the full guide.

**The template:**

```
Now give me the COMPLETE guide covering ALL of those topics.

Put it in a single markdown document with:
- Clickable table of contents at the top
- Back-to-top links in each section
- Code in [LANGUAGE] with comments explaining every line
- Solved examples with step-by-step trace tables
- Practice problems with specific references (LeetCode numbers, etc.)
- Comparison tables where relevant
- "When to use" AND "when NOT to use" for each concept

LEVEL: [Absolute beginner / knows basics / intermediate]
STYLE: [Simple and friendly / concise / detailed and exhaustive]
```

**What you get back:** The complete guide in one response.

**Total: 2 prompts. No guessing. No 10 rounds of correction.**

---

# How It Works (The Blueprint Example)

Here's exactly how our problem-solving blueprint conversation SHOULD have gone:

### Prompt 1 (What I should have asked you first):

```
I want to learn problem solving as a software engineer using JavaScript.
I'm starting from absolute zero — no programming knowledge at all.

Tell me:
1. What are ALL the topics I need to learn?
2. In what order?
3. How long each takes?
4. What depends on what?

Don't teach anything yet. Just show me the complete map.
```

### What AI responds with:

```
Here's the complete map:

Phase 1: Data Structure Prerequisites (Week 1)
  You need these BEFORE anything else:
  - Array, String, Set, Map (built into JS)
  - Stack, Queue, Linked List, Tree, Graph (you build these)
  Time: 5-7 days

Phase 2: Problem-Solving Process (Day 1-2)
  The 6 steps you follow for EVERY problem:
  - Decompose → Identify → Pseudo-code → Dry Run → Code → Refine
  Time: 2 days

Phase 3: Foundation Patterns — 6 patterns (Week 2-3)
  These solve 80% of all problems:
  - Detective, Bucket, Filter, Transformer, Memory, Pointer Walker
  Time: 14 days (1 problem/day)

Phase 4: Algorithms (Week 2-3, alongside patterns)
  The procedures that live inside patterns:
  - Sorting: Bubble, Selection, Insertion, Merge, Quick
  - Searching: Linear, Binary, Hash
  - Traversal: Array, Tree DFS, Tree BFS, Graph DFS, Graph BFS
  - Classic: Kadane's, Floyd's, Dijkstra's, Topological Sort, Union-Find
  Time: 7 days

Phase 5: Intermediate Patterns — 5 patterns (Week 3-4)
  - Sliding Window, Binary Search, Stack, Tree DFS, BFS
  Time: 14 days

Phase 6: Advanced Patterns — 3 patterns (Week 5)
  - Dynamic Programming, Backtracking, Greedy
  Time: 7 days

Phase 7: Specialist Patterns — 8 patterns (Ongoing)
  - Heap, Trie, Prefix Sum, Merge Intervals, Union-Find,
    Topological Sort, Bit Manipulation, Monotonic Stack
  Time: Learn as needed

Supporting Concepts (woven throughout):
  - Big-O complexity analysis
  - Pattern combos (how 2 patterns work together)
  - How patterns connect to each other
  - Key insights and golden rules

Total: ~35 days for Phases 1-6 at 15 min/day
```

### Prompt 2 (Now you see the scope):

```
Give me the COMPLETE guide covering ALL of those topics.

Single markdown document with:
- Clickable table of contents
- Back-to-top links per section
- Every data structure: full JS code with create, read, update,
  delete, search, traverse
- Every pattern: when to use, when NOT to use, template code,
  solved example with trace table, practice problems, LeetCode numbers
- Every algorithm: full JS implementation with trace
- Pattern combos, Big-O cheat sheet, key insights
- 35-day roadmap with specific problems per day

LEVEL: Absolute beginner, zero knowledge
STYLE: Simple, like explaining to a friend, use emojis for pattern names
```

**→ One response. Complete blueprint. No rounds of correction.**

---

# The 1-Prompt Approach

> **Use this when you ALREADY know the scope.** You've done the topic before,
> or you got the scope from Prompt 1 in a previous conversation.

---

## The 6 Elements

When you already know what you want, include these 6 things:

```
1. ROLE     → "Who is this for?"
2. GOAL     → "What should they be able to DO after reading?"
3. SCOPE    → "What topics must be covered?"
4. FORMAT   → "What structure?" (markdown, TOC, code, tables)
5. LEVEL    → "Beginner? Intermediate? Advanced?"
6. STYLE    → "Simple? Academic? Concise? Exhaustive?"
```

**Why each matters:**

| # | Element | If You Skip It, AI Will... |
|---|---------|---------------------------|
| 1 | ROLE | Not know if it's for a kid, student, or senior engineer |
| 2 | GOAL | Not know if you want to learn, teach, or just reference |
| 3 | SCOPE | Include too much or miss sections you need |
| 4 | FORMAT | Give prose when you wanted tables, or a cheat sheet when you wanted a course |
| 5 | LEVEL | Explain too much (boring) or too little (confusing) |
| 6 | STYLE | Write academically when you wanted simple, or vice versa |

---

## The Reusable Template

```
I need a [TYPE OF DOCUMENT] about [TOPIC].

ROLE: This is for [WHO — their current knowledge level].

GOAL: After reading, they should be able to:
  - [Outcome 1]
  - [Outcome 2]
  - [Outcome 3]

SCOPE — Must include these sections:
  1. [Section 1]
  2. [Section 2]
  3. [Section 3]
  ... (list every section)

FORMAT:
  - [Document type: markdown, code file, cheat sheet]
  - [Structure: clickable TOC? back-to-top links?]
  - [Code: which language? with comments?]
  - [Visuals: tables? trace tables?]

LEVEL: [Absolute beginner / intermediate / advanced]

STYLE: [Simple / academic / concise / exhaustive]
```

---

# Real Examples

---

## Example 1: Learning Problem Solving

> **Situation:** Completely new topic. Don't know what exists.
> **Approach:** 2-prompt

**Prompt 1:**
```
I want to master problem solving as a software engineer
using JavaScript. Starting from zero — no programming knowledge.

What are ALL the topics, sub-topics, and concepts that exist
in this field? Organize by learning order with time estimates
and dependencies. Don't teach yet, just show the complete map.
```

**Prompt 2 (after seeing the map):**
```
Give me the complete guide for ALL those topics.
Single markdown, clickable TOC, full JS code with comments,
trace tables, practice problems with LeetCode numbers.
Beginner level. Simple friendly style.
```

---

## Example 2: Learning React

> **Situation:** Know JavaScript, but new to React.
> **Approach:** 2-prompt (you know JS scope, but not React scope)

**Prompt 1:**
```
I know JavaScript well. I've never used React.
I want to build production web apps with React.

What are ALL the React topics I need to learn?
Order them from first to last. Include time estimates.
Show me the full map before teaching anything.
```

**Prompt 2:**
```
Give me the complete React guide for all those topics.
Single markdown, clickable TOC. Every topic should have:
explanation, syntax, 3 real examples, common mistakes,
when to use vs not. Code in React/JSX with comments.
Level: Knows JS, new to React. Style: Practical, not theoretical.
```

---

## Example 3: Learning SQL

> **Situation:** Complete beginner to SQL.
> **Approach:** 2-prompt

**Prompt 1:**
```
I want to learn SQL for data-heavy roles and interviews.
I've never written a SQL query. What are ALL the SQL topics
that exist? Order them by learning sequence with time estimates.
Just the map, don't teach yet.
```

**Prompt 2:**
```
Now give me the complete SQL guide for all those topics.
Markdown, clickable TOC. Every concept: explanation → syntax →
2-3 examples with sample data showing input and output.
Include 20 practice questions (easy → hard) with solutions.
Beginner level. Practical style — show data, show query, show result.
```

---

## Example 4: System Design

> **Situation:** You're experienced, you know WHAT you need.
> **Approach:** 1-prompt (you already know the scope)

```
I need a system design interview preparation guide.

ROLE: For a mid-level engineer (3-5 years) preparing for
FAANG system design interviews.

GOAL: Handle any system design question confidently.

SCOPE:
  1. 7-step framework for answering system design questions
  2. Core concepts: load balancing, caching, CDN, sharding,
     replication, message queues, microservices, API gateway
  3. SQL vs NoSQL with decision guide
  4. CAP theorem simply explained
  5. 10 fully designed systems: URL shortener, Twitter feed,
     Chat, YouTube, Uber, Rate limiter, Notifications,
     Search autocomplete, Distributed cache, Payment system
  6. Back-of-envelope estimation cheat sheet
  7. Common mistakes
  8. Glossary

FORMAT: Markdown, clickable TOC, each system design should have:
requirements → high-level design → deep dive → bottlenecks.
Comparison tables for tech choices.

LEVEL: Intermediate. Knows basic web architecture.
STYLE: Concise, interview-focused. No fluff.
```

---

## Example 5: Any New Topic

> **Situation:** You know NOTHING about the topic.
> **Approach:** Always 2-prompt

**The universal Prompt 1:**
```
I want to learn [TOPIC].
I currently know [WHAT YOU KNOW].
I don't know what I don't know about this field.

Before teaching me anything, show me:
1. Every topic and sub-topic that exists in this field
2. In the order I should learn them
3. How long each takes
4. What depends on what
5. Grouped into phases/milestones with a total timeline

Just the map. I'll ask for the full guide next.
```

**The universal Prompt 2:**
```
Now give me the complete guide covering ALL of those topics.
Single markdown document with clickable table of contents.
[Add any format preferences: code language, tables, examples, etc.]
Level: [Your level]
Style: [Your preference]
```

---

# When to Use Which Approach

```
┌─────────────────────────────────────────┬──────────────────────┐
│ Situation                               │ Use                  │
├─────────────────────────────────────────┼──────────────────────┤
│ I know NOTHING about the topic          │ 2-prompt approach    │
│ "I want to learn machine learning"      │ (discover → guide)   │
│                                         │                      │
├─────────────────────────────────────────┼──────────────────────┤
│ I know SOME things but not the full     │ 2-prompt approach    │
│ scope                                   │ (discover → guide)   │
│ "I know JS, want to learn React"        │                      │
│                                         │                      │
├─────────────────────────────────────────┼──────────────────────┤
│ I KNOW the scope, just need it          │ 1-prompt approach    │
│ documented / organized                  │ (full template)      │
│ "I need a guide covering X, Y, Z"      │                      │
│                                         │                      │
├─────────────────────────────────────────┼──────────────────────┤
│ Quick question, not a guide             │ Just ask normally    │
│ "How does useEffect work?"              │ No template needed   │
└─────────────────────────────────────────┴──────────────────────┘
```

---

# Common Mistakes to Avoid

### ❌ Mistake 1: Giving a vague prompt for a big topic

```
BAD:  "Tell me about JavaScript"
WHY:  Too broad. I'll guess wrong about what you need.

FIX:  Use Prompt 1 first: "What are all the JS topics?
      Show me the map." Then Prompt 2: "Now teach me all of it."
```

### ❌ Mistake 2: Not specifying who it's for

```
BAD:  "Explain closures"
WHY:  I don't know if you're a beginner or expert.

FIX:  "Explain closures to someone who knows basic JS functions
       but has never heard of closures. Use a real-world analogy,
       then 3 code examples from simple to complex."
```

### ❌ Mistake 3: Correcting one thing at a time

```
BAD:  "Give me a guide"
      → "Add more detail"
      → "Add code examples"
      → "Add a table of contents"
      → "Change the format"
      (5 rounds of back-and-forth)

FIX:  Say everything upfront in Prompt 2:
      "Complete guide, markdown, TOC, code with comments,
       trace tables, practice problems. Beginner. Simple style."
```

### ❌ Mistake 4: Trying to guess the scope yourself

```
BAD:  "Give me a guide on problem solving covering
       arrays and loops" (you missed 90% of the field)

FIX:  Prompt 1 first: "What ALL exists in problem solving?"
      Then you see the full picture before asking for the guide.
```

### ❌ Mistake 5: Not specifying the format

```
BAD:  "Explain sorting algorithms"
WHY:  I might give you 3 paragraphs when you wanted code + tables.

FIX:  "Explain sorting algorithms in markdown with:
       clickable TOC, JS code for each, trace table,
       Big-O comparison table, practice problems."
```

---

# Quick Cheat Sheet

### For NEW topics (you don't know the scope):

```
PROMPT 1: "I want to learn [X]. I know [Y]. Show me the
           complete map of everything that exists. Don't teach yet."

PROMPT 2: "Now give me the full guide for all of it.
           Markdown, TOC, code in [language], [your preferences]."
```

### For KNOWN topics (you know the scope):

```
PROMPT:
  ROLE:   "For a [WHO] who knows [WHAT] but not [WHAT]"
  GOAL:   "After reading, they can [DO WHAT]"
  SCOPE:  [Numbered list of every section]
  FORMAT: "Markdown, TOC, code in [LANGUAGE], tables"
  LEVEL:  "Beginner / Intermediate / Advanced"
  STYLE:  "Simple / Concise / Exhaustive"
```

### The exact prompt for the problem-solving blueprint (if you knew the scope):

```
I need a COMPLETE blueprint document for mastering problem
solving in programming using JavaScript.

ROLE: This is for someone teaching a person with ZERO
programming skills.

GOAL: After reading this one document, the person should:
  - Know all prerequisite data structures with full JS code
  - Know the step-by-step process to solve any coding problem
  - Know all problem-solving patterns from beginner to advanced
  - Know all key algorithms with implementations
  - Have a daily practice roadmap with specific problems

SCOPE — Must include ALL of these as separate sections:
  1. Data structure prerequisites (Array, String, Set, Map,
     Stack, Queue, Linked List, Tree, Graph) — each with
     full JavaScript code showing create, read, update,
     delete, search, traverse. Note which ones are built
     into JS and which must be built manually.
  2. The problem-solving process (6 phases to follow every
     time) with a translation dictionary from English to JS
  3. All patterns across 4 levels (foundation → specialist)
     — each with: when to use, when NOT to use, template
     code, solved example with trace table, practice
     problems with LeetCode numbers
  4. All algorithms (sorting, searching, traversal, classic
     named algorithms like Kadane's, Dijkstra's, Floyd's,
     Topological Sort, Union-Find) — each with full JS
     implementation and trace tables
  5. How patterns and algorithms relate (mapping table
     showing which algorithm lives inside which pattern)
  6. Pattern combos showing how 2 patterns work together
     with real problem examples and code
  7. Big-O cheat sheet for all patterns and data structures
  8. How patterns connect (which advanced patterns are
     built from which foundation patterns)
  9. Key insights and golden rules
  10. 35-day practice roadmap with specific problems,
      patterns used, and LeetCode numbers per day

FORMAT:
  - One single markdown document
  - Table of contents at the top with clickable links to
    every section and sub-section
  - "Back to Table of Contents" links at end of each section
  - All code in JavaScript with inline comments
  - Trace tables for all examples
  - Comparison tables where relevant

LEVEL: Absolute beginner. Assume zero knowledge.

STYLE: Simple, easy to read. Like explaining to a friend.
Use emojis for pattern names to make them memorable.
Use analogies and visuals where possible.
```

### The golden rule:

```
┌──────────────────────────────────────────────┐
│                                              │
│   Don't know the scope? → 2 prompts          │
│   Know the scope?       → 1 prompt           │
│                                              │
│   Spend 2 minutes on a clear prompt.         │
│   Save 20 minutes of back-and-forth.         │
│                                              │
└──────────────────────────────────────────────┘
```
