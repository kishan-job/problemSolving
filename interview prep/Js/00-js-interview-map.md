# JavaScript Interview Questions — Complete Map
## 16 Categories · 179 Sub-Topics · Classified by Frequency

---

# Table of Contents

- [How to Read This Map](#how-to-read-this-map)
- [Category 1: Variables & Data Types](#category-1-variables--data-types)
- [Category 2: Scope & Hoisting](#category-2-scope--hoisting)
- [Category 3: Functions](#category-3-functions)
- [Category 4: Closures](#category-4-closures)
- [Category 5: this Keyword](#category-5-this-keyword)
- [Category 6: Objects & Prototypes](#category-6-objects--prototypes)
- [Category 7: Arrays](#category-7-arrays)
- [Category 8: Asynchronous JavaScript](#category-8-asynchronous-javascript)
- [Category 9: Event Loop & Execution Context](#category-9-event-loop--execution-context)
- [Category 10: DOM Manipulation](#category-10-dom-manipulation)
- [Category 11: ES6+ Features](#category-11-es6-features)
- [Category 12: Error Handling](#category-12-error-handling)
- [Category 13: Storage & Browser APIs](#category-13-storage--browser-apis)
- [Category 14: Performance & Optimization](#category-14-performance--optimization)
- [Category 15: Coding Challenges (Implement These)](#category-15-coding-challenges)
- [Category 16: Tricky Output Questions (Predict the Output)](#category-16-tricky-output-questions)
- [Summary Table](#summary-table)
- [The 80/20 — Focus Here First](#the-8020--focus-here-first)
- [Prompt 2 — Ask for the Full Guide](#prompt-2--ask-for-the-full-guide)

---

# How to Read This Map

```
🔴 = Must-know   (asked in 90% of interviews — prepare these FIRST)
🟡 = Should-know  (asked in 50% of interviews — prepare after 🔴)
🟢 = Good-to-know (top companies / senior roles — prepare last)
```

This is ONLY the map. No explanations, no code.
Once you see the full picture, use [Prompt 2](#prompt-2--ask-for-the-full-guide) to get the complete guide.

---

# Category 1: Variables & Data Types

```
🔴 var vs let vs const
🔴 Primitive types (string, number, boolean, null, undefined, symbol, bigint)
🔴 Reference types (object, array, function)
🔴 typeof operator and its quirks (typeof null === "object")
🔴 == vs === (loose vs strict equality)
🔴 Type coercion (implicit conversion: "5" + 3, "5" - 3)
🟡 Symbol and its use cases
🟡 BigInt and when to use it
🟡 Pass by value vs pass by reference
🟡 Immutability of primitives
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 2: Scope & Hoisting

```
🔴 Global scope, function scope, block scope
🔴 Hoisting — var, let, const, function declarations
🔴 Temporal Dead Zone (TDZ) for let/const
🔴 Lexical scope (static scope)
🟡 Scope chain — how JS resolves variables
🟡 Variable shadowing
🟢 IIFE (Immediately Invoked Function Expression) and why it was used
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 3: Functions

```
🔴 Function declaration vs function expression
🔴 Arrow functions vs regular functions (differences)
🔴 Default parameters
🔴 Rest parameters (...args)
🔴 Spread operator (...)
🔴 Callback functions
🔴 Higher-order functions
🟡 Pure functions
🟡 First-class functions (functions as values)
🟡 Function composition
🟡 Currying
🟡 Partial application
🟡 Recursion
🟢 Generator functions (function*)
🟢 Memoization (caching function results)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 4: Closures

```
🔴 What is a closure? How does it work?
🔴 Practical uses (data privacy, factory functions)
🔴 Classic loop + closure problem (var in for loop)
🟡 Closure with setTimeout
🟡 Memory leaks from closures
🟡 Module pattern using closures
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 5: this Keyword

```
🔴 this in different contexts (global, object method, arrow function)
🔴 call(), apply(), bind() — what they do and differences
🔴 this in event handlers
🔴 Why arrow functions don't have their own this
🟡 this in class constructors
🟡 Explicit vs implicit vs default binding
🟢 this in setTimeout/setInterval
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 6: Objects & Prototypes

```
🔴 Object creation (literal, constructor, Object.create)
🔴 Object destructuring
🔴 Shallow copy vs deep copy (spread, Object.assign, structuredClone)
🔴 Object methods (keys, values, entries, freeze, assign)
🟡 Prototype chain — how inheritance works
🟡 __proto__ vs prototype
🟡 hasOwnProperty vs in operator
🟡 Property descriptors (writable, enumerable, configurable)
🟡 Object.freeze vs Object.seal
🟢 Proxy and Reflect
🟢 Getters and setters
🟢 WeakRef
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 7: Arrays

```
🔴 map, filter, reduce, forEach, find, findIndex
🔴 some, every, includes, indexOf
🔴 push, pop, shift, unshift
🔴 slice vs splice (most confused pair)
🔴 Array destructuring
🔴 Spread operator with arrays
🟡 flat, flatMap
🟡 sort (and its quirks with default lexicographic sort)
🟡 Array.from, Array.of
🟡 Chaining methods (map → filter → reduce)
🟢 Implement your own map, filter, reduce (polyfills)
🟢 Typed arrays
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 8: Asynchronous JavaScript

```
🔴 Synchronous vs Asynchronous
🔴 Callbacks and callback hell
🔴 Promises — resolve, reject, then, catch, finally
🔴 Promise.all, Promise.race, Promise.allSettled, Promise.any
🔴 async / await
🔴 try/catch with async/await
🔴 Error handling in promises
🟡 Chaining promises
🟡 Converting callbacks to promises
🟡 Microtasks vs Macrotasks
🟡 Fetch API and handling responses
🟡 AbortController (canceling fetch)
🟢 Custom Promise implementation (write your own Promise)
🟢 Async iterators and for-await-of
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 9: Event Loop & Execution Context

```
🔴 Call stack — what it is, how it works
🔴 Event loop — how JS handles async
🔴 Execution context (global, function, eval)
🔴 setTimeout / setInterval behavior
🟡 Microtask queue (Promise callbacks) vs Macrotask queue (setTimeout)
🟡 Order of execution questions (predict the output)
🟡 requestAnimationFrame
🟢 Web Workers
🟢 queueMicrotask
🟢 Starvation of macrotask queue
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 10: DOM Manipulation

```
🔴 Selecting elements (getElementById, querySelector, querySelectorAll)
🔴 Creating and appending elements
🔴 Modifying content (textContent, innerHTML, innerText)
🔴 Modifying styles and classes (classList, style)
🔴 Event listeners (addEventListener, removeEventListener)
🔴 Event delegation
🔴 Event bubbling vs capturing
🟡 event.target vs event.currentTarget
🟡 event.preventDefault() vs event.stopPropagation()
🟡 DOM traversal (parentNode, children, nextSibling, etc.)
🟡 Document fragment for performance
🟡 MutationObserver
🟢 Shadow DOM
🟢 Custom Elements (Web Components)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 11: ES6+ Features

```
🔴 let, const, arrow functions, template literals
🔴 Destructuring (object and array)
🔴 Spread and rest operators
🔴 Default parameters
🔴 Modules (import / export)
🔴 Classes (constructor, methods, extends, super)
🟡 Optional chaining (?.)
🟡 Nullish coalescing (??)
🟡 Map, Set, WeakMap, WeakSet
🟡 for...of vs for...in
🟡 Symbols
🟡 Iterators and iterables
🟢 Generators (function*, yield)
🟢 Tagged template literals
🟢 Decorators (proposal)
🟢 Private class fields (#)
🟢 Temporal API (date handling — emerging)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 12: Error Handling

```
🔴 try, catch, finally
🔴 throw and custom errors
🔴 Error types (TypeError, ReferenceError, SyntaxError, RangeError)
🟡 Error handling in async code
🟡 Global error handlers (window.onerror, unhandledrejection)
🟢 Custom error classes extending Error
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 13: Storage & Browser APIs

```
🔴 localStorage vs sessionStorage
🔴 Cookies — what they are, how they work
🟡 localStorage vs cookies vs sessionStorage (comparison)
🟡 JSON.parse / JSON.stringify (and their limitations)
🟡 Geolocation API
🟡 Intersection Observer
🟡 History API (pushState, replaceState)
🟡 Clipboard API
🟢 Service Workers
🟢 Web Workers
🟢 IndexedDB
🟢 WebSockets
🟢 Server-Sent Events (SSE)
🟢 Broadcast Channel API
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 14: Performance & Optimization

```
🔴 Debouncing vs Throttling (most asked optimization question)
🟡 Lazy loading
🟡 Code splitting
🟡 Memoization
🟡 Avoiding memory leaks
🟡 Reflow vs Repaint
🟡 Virtual scrolling (rendering large lists)
🟡 requestAnimationFrame for smooth animations
🟢 Tree shaking
🟢 Web Vitals (LCP, FID, CLS)
🟢 Performance API (performance.now(), PerformanceObserver)
🟢 Idle callback (requestIdleCallback)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 15: Coding Challenges

> "Implement this from scratch" — the most common coding round questions.

```
🔴 Implement debounce
🔴 Implement throttle
🔴 Implement Promise.all
🔴 Implement Array.prototype.map (polyfill)
🔴 Implement Array.prototype.filter (polyfill)
🔴 Implement Array.prototype.reduce (polyfill)
🔴 Implement Function.prototype.bind (polyfill)
🔴 Implement deep clone
🟡 Implement curry function
🟡 Implement event emitter (pub/sub)
🟡 Implement flat (flatten nested arrays)
🟡 Implement getElementsByClassName (from scratch)
🟡 Implement setInterval using setTimeout
🟡 Implement promise with retry
🟡 Implement memoize function
🟢 Implement custom Promise class
🟢 Implement JSON.stringify
🟢 Implement virtual DOM diff algorithm (concept)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 16: Tricky Output Questions

> "What does this code print?" — tests deep understanding.

```
🔴 Hoisting output questions
🔴 Closure in loop (var vs let)
🔴 this in different contexts
🔴 == type coercion surprises
🔴 setTimeout(fn, 0) execution order
🟡 Promise resolution order
🟡 Event loop output questions (mix of setTimeout, Promise, console.log)
🟡 typeof and instanceof tricks
🟡 NaN === NaN, null == undefined
🟢 Proxy traps and behavior
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Summary Table

| # | Category | Sub-topics | Priority |
|---|----------|-----------|----------|
| 1 | Variables & Data Types | 10 | 🔴 Must-know |
| 2 | Scope & Hoisting | 7 | 🔴 Must-know |
| 3 | Functions | 15 | 🔴 Must-know |
| 4 | Closures | 6 | 🔴 Must-know |
| 5 | this Keyword | 7 | 🔴 Must-know |
| 6 | Objects & Prototypes | 12 | 🔴🟡 Mixed |
| 7 | Arrays | 12 | 🔴 Must-know |
| 8 | Async JavaScript | 14 | 🔴 Must-know |
| 9 | Event Loop & Execution Context | 10 | 🔴🟡 Mixed |
| 10 | DOM Manipulation | 14 | 🔴 Must-know |
| 11 | ES6+ Features | 17 | 🔴🟡 Mixed |
| 12 | Error Handling | 6 | 🔴🟡 Mixed |
| 13 | Storage & Browser APIs | 13 | 🟡🟢 Mixed |
| 14 | Performance & Optimization | 12 | 🟡🟢 Mixed |
| 15 | Coding Challenges | 18 | 🔴🟡 Mixed |
| 16 | Tricky Output Questions | 10 | 🔴🟡 Mixed |
| | **TOTAL** | **183** | |

[↑ Back to Table of Contents](#table-of-contents)

---

# The 80/20 — Focus Here First

> These 8 areas cover ~80% of all JavaScript interview questions.
> If you only have 1-2 weeks, prepare ONLY these.

```
1. Closures               → asked in almost every interview
2. this keyword            → call, apply, bind, arrow function behavior
3. Promises + async/await  → resolve, reject, chaining, error handling
4. Event loop              → call stack, microtask vs macrotask, execution order
5. Array methods           → map, filter, reduce (use + implement from scratch)
6. Event delegation        → bubbling, capturing, event.target
7. Debounce + Throttle     → understand AND implement both
8. Hoisting + Scope        → var vs let vs const, TDZ, scope chain
```

```
PREPARATION TIME ESTIMATES:
  🔴 Must-know topics only        → 1-2 weeks (minimum viable prep)
  🔴 + 🟡 Must + Should know      → 3-4 weeks (solid prep)
  🔴 + 🟡 + 🟢 Everything         → 6-8 weeks (thorough prep)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Prompt 2 — Ask for the Full Guide

> Now that you see the complete map, copy-paste this prompt in a
> **new conversation** to get the full guide with explanations and code.

## Option A: Everything (6-8 weeks of content)

```
Based on these 16 categories of JavaScript interview topics
(183 sub-topics total), give me the COMPLETE preparation guide.

For EACH sub-topic include:
  - Clear explanation (2-3 sentences, simple language)
  - Code example showing how it works
  - The most common interview question about it (with answer)
  - Common mistake candidates make
  - One-liner summary for quick revision

Organize by category. Within each category, order by priority:
  🔴 Must-know first, then 🟡 Should-know, then 🟢 Good-to-know.

CATEGORIES TO COVER (all 16):
  1. Variables & Data Types
  2. Scope & Hoisting
  3. Functions
  4. Closures
  5. this Keyword
  6. Objects & Prototypes
  7. Arrays
  8. Asynchronous JavaScript
  9. Event Loop & Execution Context
  10. DOM Manipulation
  11. ES6+ Features
  12. Error Handling
  13. Storage & Browser APIs
  14. Performance & Optimization
  15. Coding Challenges (implement from scratch with full code)
  16. Tricky Output Questions (show code + explain step-by-step output)

FORMAT:
  - Single markdown document
  - Clickable table of contents at the top
  - Back-to-top links per section
  - All code in JavaScript with inline comments
  - Difficulty tag on each question (🔴🟡🟢)
  - Comparison tables where relevant

LEVEL: Know JS basics, preparing for interviews
STYLE: Practical, interview-focused. Each explanation should
connect to "how this gets asked in interviews."
```

## Option B: Must-Know Only (1-2 weeks of content)

```
Give me a JavaScript interview preparation guide covering
ONLY the must-know (🔴) topics from these categories:

  1. Variables & Data Types (var/let/const, types, == vs ===, coercion)
  2. Scope & Hoisting (scope types, hoisting, TDZ)
  3. Functions (declarations, arrow functions, callbacks, HOFs)
  4. Closures (what, why, loop problem)
  5. this Keyword (contexts, call/apply/bind)
  6. Arrays (map, filter, reduce, slice vs splice)
  7. Async JavaScript (promises, async/await, Promise.all)
  8. Event Loop (call stack, event loop, setTimeout behavior)
  9. DOM (selectors, event delegation, bubbling)
  10. Coding Challenges (debounce, throttle, Promise.all,
      map/filter/reduce polyfills, deep clone, bind polyfill)

For each topic: explanation + code + interview question + answer.
Single markdown, clickable TOC, JS code with comments.
Beginner-friendly. Interview-focused.
```

## Option C: One Category at a Time

```
Give me the complete JavaScript interview guide for ONLY
[CATEGORY NAME] (e.g., "Closures" or "Async JavaScript").

Cover every sub-topic in that category:
  - Clear explanation with code
  - Top 5 interview questions with detailed answers
  - Common mistakes
  - Tricky edge cases
  - One-liner revision notes

Markdown, code with comments, simple language.
```

[↑ Back to Table of Contents](#table-of-contents)
