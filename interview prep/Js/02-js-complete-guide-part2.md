# JavaScript Complete Interview Guide
## Part 2: Categories 7-12 (Core Skills)

---

# Table of Contents — Part 2

- [Category 7: Arrays](#category-7-arrays)
- [Category 8: Asynchronous JavaScript](#category-8-asynchronous-javascript)
- [Category 9: Event Loop & Execution Context](#category-9-event-loop--execution-context)
- [Category 10: DOM Manipulation](#category-10-dom-manipulation)
- [Category 11: ES6+ Features](#category-11-es6-features)
- [Category 12: Error Handling](#category-12-error-handling)
- [Quick Revision — Part 2 One-Liners](#quick-revision--part-2-one-liners)

---

# Category 7: Arrays

---

### 🔴 7.1 map, filter, reduce

**Explanation:** The three most important array methods. `map` transforms every item. `filter` keeps items passing a test. `reduce` combines all items into one value.

```javascript
const nums = [1, 2, 3, 4, 5];

// MAP — transform each item. Returns SAME-SIZE array.
const doubled = nums.map(n => n * 2);         // [2, 4, 6, 8, 10]

// FILTER — keep items passing test. Returns SHORTER (or equal) array.
const evens = nums.filter(n => n % 2 === 0);  // [2, 4]

// REDUCE — combine all items into ONE value.
const sum = nums.reduce((acc, n) => acc + n, 0);  // 15
//                       accumulator  current  initial

// Chaining
const result = nums
  .filter(n => n % 2 === 0)    // [2, 4]
  .map(n => n * 10)            // [20, 40]
  .reduce((acc, n) => acc + n, 0); // 60
```

**Interview Q: Difference between map, filter, and reduce?** 🔴

| Method | Input Size → Output Size | Returns | Purpose |
|---|---|---|---|
| map | n → n (same) | New array | Transform each item |
| filter | n → 0..n (fewer or equal) | New array | Keep items passing test |
| reduce | n → 1 | Single value | Combine into one result |

**Mistake:** Using `forEach` when you need a return value. `forEach` returns `undefined`. Use `map` to get a new array.

**One-liner:** map = transform all (same size). filter = keep matching (shorter). reduce = combine into one value. All return new data, don't mutate.

---

### 🔴 7.2 forEach, find, findIndex, some, every, includes

**Explanation:** Utility methods for common array operations.

```javascript
const users = [
  { id: 1, name: "Alice", active: true },
  { id: 2, name: "Bob", active: false },
  { id: 3, name: "Charlie", active: true },
];

// forEach — loop, no return value
users.forEach(u => console.log(u.name));  // Alice, Bob, Charlie

// find — first item matching condition (or undefined)
const bob = users.find(u => u.name === "Bob");  // { id: 2, name: "Bob", ... }

// findIndex — index of first match (or -1)
const bobIndex = users.findIndex(u => u.name === "Bob");  // 1

// some — does ANY item pass? (returns boolean, stops early)
const hasInactive = users.some(u => !u.active);  // true

// every — do ALL items pass? (returns boolean, stops early)
const allActive = users.every(u => u.active);     // false

// includes — is this VALUE in the array? (for primitives)
[1, 2, 3].includes(2);    // true
[1, 2, 3].includes(99);   // false

// indexOf — position of value (or -1)
[10, 20, 30].indexOf(20);  // 1
[10, 20, 30].indexOf(99);  // -1
```

**Interview Q: Difference between find and filter?** 🔴
**A:** `find` returns the FIRST matching item (or undefined). `filter` returns ALL matching items in an array. Use `find` when you need one result, `filter` when you need all.

**Mistake:** Using `filter` when you only need one item — it scans the entire array unnecessarily.

**One-liner:** find = first match. filter = all matches. some = any pass? every = all pass? includes = value exists? forEach = loop (no return).

---

### 🔴 7.3 slice vs splice

**Explanation:** The most confused pair. `slice` copies a portion WITHOUT modifying. `splice` modifies the original array (insert, remove, replace).

```javascript
const arr = [1, 2, 3, 4, 5];

// SLICE — does NOT modify original. Returns a copy of a section.
arr.slice(1, 3);     // [2, 3] (index 1 to 3, exclusive)
arr.slice(2);        // [3, 4, 5] (from index 2 to end)
arr.slice(-2);       // [4, 5] (last 2 items)
arr.slice();         // [1, 2, 3, 4, 5] (shallow copy)
console.log(arr);    // [1, 2, 3, 4, 5] (UNCHANGED)

// SPLICE — MODIFIES original. Removes/inserts items.
const arr2 = [1, 2, 3, 4, 5];

arr2.splice(1, 2);          // removes 2 items starting at index 1
// arr2 = [1, 4, 5]          // returns removed: [2, 3]

arr2.splice(1, 0, 10, 20);  // at index 1, remove 0, insert 10, 20
// arr2 = [1, 10, 20, 4, 5]

arr2.splice(0, 1, 99);      // replace first item with 99
// arr2 = [99, 10, 20, 4, 5]
```

**Interview Q: Slice vs splice?** 🔴

| | slice | splice |
|---|---|---|
| Modifies original? | ❌ No | ✅ Yes |
| Returns | New array (copy of section) | Array of removed items |
| Arguments | (start, end) | (start, deleteCount, ...items) |
| Use for | Copying, extracting | Inserting, removing, replacing |

**One-liner:** slice = copy without modifying (immutable). splice = modify original (insert/remove/replace). Slice is safe, splice mutates.

---

### 🔴 7.4 push, pop, shift, unshift

**Explanation:** Add/remove items from the start or end of an array. All MUTATE the original.

```javascript
const arr = [2, 3, 4];

// END operations
arr.push(5);       // add to end    → [2, 3, 4, 5]    returns new length: 4
arr.pop();         // remove from end → [2, 3, 4]       returns removed: 5

// START operations
arr.unshift(1);    // add to start   → [1, 2, 3, 4]    returns new length: 4
arr.shift();       // remove from start → [2, 3, 4]     returns removed: 1
```

```
PERFORMANCE:
  push/pop   → O(1) — fast (no shifting needed)
  shift/unshift → O(n) — slow (shifts all elements)
```

**Interview Q: Why is shift/unshift slower than push/pop?** 🟡
**A:** push/pop operate at the end — no other elements need to move. shift/unshift operate at the start — every remaining element must be re-indexed (shifted one position). This is O(n) for an array of n elements.

**One-liner:** push/pop = end (fast O(1)). shift/unshift = start (slow O(n)). All mutate original. Use spread [...arr, item] for immutable adds.

---

### 🔴 7.5 sort and Its Quirks

**Explanation:** The built-in sort has a MAJOR gotcha — it sorts lexicographically (as strings) by default, not numerically.

```javascript
// ❌ DEFAULT SORT IS ALPHABETICAL (LEXICOGRAPHIC)
[10, 2, 30, 1].sort();            // [1, 10, 2, 30] — WRONG!
// Sorts as strings: "1" < "10" < "2" < "30"

// ✅ NUMERIC SORT — pass a comparison function
[10, 2, 30, 1].sort((a, b) => a - b);  // [1, 2, 10, 30] — ascending
[10, 2, 30, 1].sort((a, b) => b - a);  // [30, 10, 2, 1] — descending

// How the comparator works:
//   negative → a comes first
//   positive → b comes first
//   zero → keep order

// Sort objects by property
const users = [{ name: "Charlie" }, { name: "Alice" }, { name: "Bob" }];
users.sort((a, b) => a.name.localeCompare(b.name));
// [{ name: "Alice" }, { name: "Bob" }, { name: "Charlie" }]

// ⚠️ sort MUTATES the original array!
const original = [3, 1, 2];
const sorted = original.sort((a, b) => a - b);
console.log(original); // [1, 2, 3] — MUTATED!
// Fix: sort a copy
const safeSorted = [...original].sort((a, b) => a - b);
```

**Interview Q: Why does [10, 2, 30].sort() return [10, 2, 30]?** 🔴
**A:** Default sort is lexicographic (string comparison). "10" < "2" because "1" < "2" (first character comparison). Fix: `.sort((a, b) => a - b)` for numeric sort.

**Mistake:** Forgetting that sort mutates the original array. Always sort a copy if you need immutability.

**One-liner:** Default sort is alphabetical (10 < 2 as strings!). Always use .sort((a, b) => a - b) for numbers. sort() MUTATES — copy first.

---

### 🟡 7.6 flat, flatMap

**Explanation:** `flat` flattens nested arrays. `flatMap` maps then flattens one level (more efficient than `.map().flat()`).

```javascript
// flat — flatten nested arrays
[1, [2, 3], [4, [5]]].flat();     // [1, 2, 3, 4, [5]] — one level
[1, [2, [3, [4]]]].flat(2);       // [1, 2, 3, [4]] — two levels
[1, [2, [3, [4]]]].flat(Infinity); // [1, 2, 3, 4] — all levels

// flatMap — map + flat(1) in one step
const sentences = ["hello world", "foo bar"];
sentences.map(s => s.split(" "));     // [["hello","world"], ["foo","bar"]]
sentences.flatMap(s => s.split(" ")); // ["hello", "world", "foo", "bar"]

// Practical: filter and map simultaneously
[1, 2, 3, 4].flatMap(n => n % 2 === 0 ? [n * 2] : []);
// [4, 8] — even numbers doubled, odds removed
```

**Interview Q: What does flatMap do?** 🟡
**A:** It maps each element using a function then flattens the result one level. It's equivalent to `.map(fn).flat(1)` but more efficient (single pass). Useful for splitting strings into words, filtering + transforming simultaneously, etc.

**One-liner:** flat(depth) = flatten nested arrays. flatMap = map + flat(1) in one pass. Efficient for split, filter+map combos.

---

### 🟢 7.7 Implement map, filter, reduce (Polyfills)

**Explanation:** Interviewers ask you to implement these from scratch to test understanding of how they work internally.

```javascript
// IMPLEMENT map
Array.prototype.myMap = function(callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    result.push(callback(this[i], i, this));  // (element, index, array)
  }
  return result;
};
[1, 2, 3].myMap(n => n * 2);  // [2, 4, 6]

// IMPLEMENT filter
Array.prototype.myFilter = function(callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (callback(this[i], i, this)) {  // if truthy, keep it
      result.push(this[i]);
    }
  }
  return result;
};
[1, 2, 3, 4].myFilter(n => n % 2 === 0);  // [2, 4]

// IMPLEMENT reduce
Array.prototype.myReduce = function(callback, initialValue) {
  let acc = initialValue;
  let startIndex = 0;

  if (acc === undefined) {
    acc = this[0];       // first element becomes initial accumulator
    startIndex = 1;      // start from second element
  }

  for (let i = startIndex; i < this.length; i++) {
    acc = callback(acc, this[i], i, this);
  }
  return acc;
};
[1, 2, 3].myReduce((acc, n) => acc + n, 0);  // 6
```

**Interview Q: Implement Array.prototype.map from scratch.** 🟢
**A:** See code above. Key points: create empty result array, loop through `this` (the array), call callback with `(element, index, array)`, push each result, return new array. Never modify the original.

**One-liner:** Polyfills: loop through `this`, call callback(element, index, array). map → push result. filter → push if truthy. reduce → accumulate.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 8: Asynchronous JavaScript

---

### 🔴 8.1 Synchronous vs Asynchronous

**Explanation:** Synchronous code runs line by line — each line waits for the previous one. Asynchronous code starts an operation and moves on — the result comes later via callbacks, promises, or async/await.

```javascript
// SYNCHRONOUS — blocks until done
console.log("1");
console.log("2");  // waits for "1" to finish
console.log("3");
// Output: 1, 2, 3

// ASYNCHRONOUS — doesn't block
console.log("1");
setTimeout(() => console.log("2"), 0);  // scheduled for later
console.log("3");
// Output: 1, 3, 2 — "2" runs after current code finishes!
```

**Interview Q: Why is JavaScript single-threaded but can handle async?** 🔴
**A:** JS has ONE call stack (single-threaded) but uses the Event Loop + Web APIs (setTimeout, fetch, etc.) to handle async. The browser runs async operations in the background, then puts callbacks on the queue when done. The event loop picks them up when the call stack is empty.

**One-liner:** Sync = line by line, blocks. Async = starts operation, moves on, gets result later. JS is single-threaded but uses event loop for async.

---

### 🔴 8.2 Promises

**Explanation:** A Promise represents a value that will be available in the future. It has three states: pending, fulfilled (resolved), or rejected.

```javascript
// Creating a promise
const myPromise = new Promise((resolve, reject) => {
  const success = true;
  if (success) {
    resolve("Data loaded!");   // fulfilled
  } else {
    reject("Error occurred");  // rejected
  }
});

// Consuming a promise
myPromise
  .then(data => console.log(data))     // "Data loaded!"
  .catch(err => console.error(err))    // handles rejection
  .finally(() => console.log("Done")); // runs regardless

// Chaining — each .then returns a new promise
fetch('/api/user')
  .then(res => res.json())        // returns promise
  .then(user => fetch(`/api/posts/${user.id}`))  // returns promise
  .then(res => res.json())
  .then(posts => console.log(posts))
  .catch(err => console.error(err));  // catches ANY error in the chain
```

**Interview Q: What are the three states of a Promise?** 🔴
**A:** (1) **Pending** — initial state, neither fulfilled nor rejected. (2) **Fulfilled** — operation completed, `.then()` runs. (3) **Rejected** — operation failed, `.catch()` runs. Once settled (fulfilled or rejected), a promise can't change state.

**Mistake:** Forgetting `.catch()` — unhandled rejections cause warnings/crashes. Always handle errors.

**One-liner:** Promise = future value. pending → fulfilled (.then) or rejected (.catch). Chain with .then(). Always .catch() errors.

---

### 🔴 8.3 Promise.all, race, allSettled, any

**Explanation:** Methods for handling multiple promises concurrently.

```javascript
const p1 = Promise.resolve(1);
const p2 = Promise.resolve(2);
const p3 = Promise.reject("Error");

// all — ALL must succeed. Rejects if ANY fails.
Promise.all([p1, p2])
  .then(results => console.log(results));  // [1, 2]
Promise.all([p1, p3])
  .catch(err => console.log(err));         // "Error" (p3 failed)

// allSettled — waits for ALL, never rejects. Returns status of each.
Promise.allSettled([p1, p2, p3])
  .then(results => console.log(results));
// [{ status: "fulfilled", value: 1 },
//  { status: "fulfilled", value: 2 },
//  { status: "rejected", reason: "Error" }]

// race — first to settle (resolve OR reject) wins.
Promise.race([
  new Promise(r => setTimeout(() => r("slow"), 200)),
  new Promise(r => setTimeout(() => r("fast"), 100)),
]).then(winner => console.log(winner));  // "fast"

// any — first to RESOLVE wins. Rejects only if ALL reject.
Promise.any([p3, p1, p2])
  .then(first => console.log(first));  // 1 (first resolved)
```

**Interview Q: Difference between Promise.all and Promise.allSettled?** 🔴

| | all | allSettled |
|---|---|---|
| Fails if one rejects? | Yes (short-circuits) | No (waits for all) |
| Returns | Array of values | Array of {status, value/reason} |
| Use when | Need ALL to succeed | Need results regardless of failure |

**One-liner:** all = all succeed or fail fast. allSettled = wait for all regardless. race = first to settle. any = first to resolve.

---

### 🔴 8.4 async / await

**Explanation:** Syntactic sugar over Promises. `async` functions always return a Promise. `await` pauses execution until the Promise resolves. Makes async code look synchronous.

```javascript
// Without async/await (promise chains)
function getUser() {
  return fetch('/api/user')
    .then(res => res.json())
    .then(user => {
      return fetch(`/api/posts/${user.id}`)
        .then(res => res.json());
    });
}

// With async/await (clean, readable)
async function getUser() {
  const res = await fetch('/api/user');
  const user = await res.json();

  const postsRes = await fetch(`/api/posts/${user.id}`);
  const posts = await postsRes.json();

  return posts;
}

// Error handling with try/catch
async function fetchData() {
  try {
    const res = await fetch('/api/data');
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    const data = await res.json();
    return data;
  } catch (err) {
    console.error('Failed:', err);
    throw err;  // re-throw if caller needs to handle it
  }
}

// Parallel with async/await (don't await sequentially if independent)
async function loadDashboard() {
  // ❌ SEQUENTIAL — slow (one after another)
  const users = await fetchUsers();    // 1 second
  const posts = await fetchPosts();    // 1 second
  // Total: 2 seconds

  // ✅ PARALLEL — fast (both at once)
  const [users, posts] = await Promise.all([
    fetchUsers(),   // starts immediately
    fetchPosts(),   // starts immediately
  ]);
  // Total: 1 second (max of both)
}
```

**Interview Q: How do you run async operations in parallel with async/await?** 🔴
**A:** Use `Promise.all()`. Start all promises without awaiting them individually, then await them all at once: `const [a, b] = await Promise.all([fetchA(), fetchB()])`. If you `await` each sequentially, they run one after another (slower).

**Mistake:** Awaiting in a loop — `for (const url of urls) { await fetch(url); }` is sequential! Use `Promise.all(urls.map(url => fetch(url)))` for parallel.

**One-liner:** async function returns Promise. await pauses until resolved. Use try/catch for errors. Promise.all for parallel. Don't await in loops.

---

### 🟡 8.5 Microtasks vs Macrotasks

**Explanation:** Async callbacks go into two different queues. Microtasks (Promises) have HIGHER priority than macrotasks (setTimeout). After each task, ALL microtasks run before the next macrotask.

```javascript
console.log("1");                          // sync

setTimeout(() => console.log("2"), 0);     // macrotask

Promise.resolve().then(() => console.log("3"));  // microtask

console.log("4");                          // sync

// Output: 1, 4, 3, 2
// Why: sync first (1, 4), then microtask (3), then macrotask (2)
```

```
EXECUTION ORDER:
  1. All synchronous code (call stack)
  2. ALL microtasks (Promise.then, queueMicrotask, MutationObserver)
  3. ONE macrotask (setTimeout, setInterval, I/O, UI events)
  4. ALL microtasks again
  5. Next macrotask
  ...repeat
```

**Interview Q: What is the output?** 🟡
```javascript
setTimeout(() => console.log('A'), 0);
Promise.resolve().then(() => console.log('B'));
Promise.resolve().then(() => {
  console.log('C');
  setTimeout(() => console.log('D'), 0);
});
console.log('E');
```
**A:** `E, B, C, A, D` — Sync first (E). Microtasks next (B, C). Macrotask A was queued before D. D was added during microtask C, goes to macrotask queue after A.

**One-liner:** Microtasks (Promises) run before macrotasks (setTimeout). After EACH macrotask, drain ALL microtasks before next macrotask.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 9: Event Loop & Execution Context

---

### 🔴 9.1 Call Stack

**Explanation:** The call stack tracks which function is currently running. When a function is called, it's pushed onto the stack. When it returns, it's popped off. JS is single-threaded — one stack, one thing at a time.

```javascript
function third()  { console.log("third"); }
function second() { third(); console.log("second"); }
function first()  { second(); console.log("first"); }
first();

// Stack trace:
// 1. first() pushed
// 2.   second() pushed
// 3.     third() pushed
// 4.     third() prints "third", popped
// 5.   second() prints "second", popped
// 6. first() prints "first", popped
// Output: third, second, first
```

**Interview Q: What happens if the call stack overflows?** 🔴
**A:** `RangeError: Maximum call stack size exceeded`. This happens with infinite recursion — the stack keeps growing until it hits the limit (~10,000-25,000 frames depending on the browser).

**One-liner:** Call stack = LIFO (last in, first out). Tracks running functions. One stack = single threaded. Overflow = infinite recursion.

---

### 🔴 9.2 Event Loop

**Explanation:** The event loop is the mechanism that coordinates the call stack, microtask queue, and macrotask queue. It continuously checks: "Is the stack empty? If yes, process microtasks, then pick one macrotask."

```
┌─────────────────────────────────────────────┐
│                                             │
│   CALL STACK (runs synchronous code)        │
│   ↓ (when empty)                            │
│   MICROTASK QUEUE (Promise.then, etc.)      │
│   ↓ (when empty)                            │
│   MACROTASK QUEUE (setTimeout, events, etc.)│
│   ↓ (picks ONE, puts on call stack)         │
│   → back to top                             │
│                                             │
└─────────────────────────────────────────────┘
```

**Interview Q: What is the output?** 🔴
```javascript
console.log('start');
setTimeout(() => console.log('timeout'), 0);
Promise.resolve().then(() => console.log('promise'));
console.log('end');
```
**A:** `start, end, promise, timeout`
1. Sync: `start`, `end` (call stack)
2. Microtask: `promise` (Promise.then)
3. Macrotask: `timeout` (setTimeout)

**One-liner:** Event loop: run all sync → drain all microtasks → pick one macrotask → repeat. This is why setTimeout(fn, 0) isn't truly instant.

---

### 🔴 9.3 Execution Context

**Explanation:** Every time JS runs code, it creates an execution context — an environment containing the variables, scope chain, and `this` value. There are three types: Global, Function, and Eval.

```javascript
// GLOBAL execution context — created when script starts
var globalVar = "I'm global";
// this = window (browser) or global (Node)

function outer() {
  // FUNCTION execution context — created when outer() is called
  var outerVar = "I'm outer";
  // this = depends on how outer() was called

  function inner() {
    // FUNCTION execution context — created when inner() is called
    var innerVar = "I'm inner";
    console.log(globalVar, outerVar, innerVar);
  }
  inner();
}
outer();

// Each context goes through 2 phases:
// 1. CREATION — sets up variables (hoisting), scope chain, this
// 2. EXECUTION — runs the code line by line
```

**Interview Q: What is an execution context?** 🔴
**A:** An environment created when code runs. It contains: (1) Variable Environment (hoisted vars/functions), (2) Scope Chain (current scope + parent scopes), (3) `this` binding. Global context is created first. Each function call creates a new context pushed onto the call stack.

**One-liner:** Execution context = environment with variables + scope chain + this. Created for each function call. Two phases: creation (hoisting) → execution.

---

### 🔴 9.4 setTimeout / setInterval Behavior

**Explanation:** `setTimeout` schedules a callback after a MINIMUM delay — not an exact delay. The callback goes to the macrotask queue and waits for the call stack to be empty.

```javascript
// setTimeout(fn, 0) doesn't mean "run immediately"
console.log("before");
setTimeout(() => console.log("timeout"), 0);
console.log("after");
// Output: before, after, timeout
// "timeout" waits for sync code AND microtasks to finish

// setInterval — repeats at interval
const id = setInterval(() => {
  console.log("tick");
}, 1000);

// Stop it
clearInterval(id);

// ⚠️ setInterval drift — if callback takes longer than interval,
// callbacks queue up. Use recursive setTimeout instead:
function accurateTimer() {
  console.log("tick");
  setTimeout(accurateTimer, 1000);  // schedules AFTER completion
}
```

**Interview Q: Is setTimeout(fn, 0) truly zero delay?** 🔴
**A:** No. The minimum is usually 4ms (browser throttling), and the callback still waits for: (1) current synchronous code to finish, (2) all microtasks to drain. It only means "run as soon as possible after the current work."

**One-liner:** setTimeout(fn, 0) ≠ instant. Callback waits for sync + microtasks. Minimum ~4ms. setInterval can drift — use recursive setTimeout.

---

### 🟡 9.5 requestAnimationFrame

**Explanation:** `requestAnimationFrame` runs a callback before the next browser repaint (~60fps = every ~16ms). Better than setTimeout for animations because it syncs with the display refresh rate.

```javascript
// Smooth animation
function animate() {
  element.style.left = `${position}px`;
  position += 2;

  if (position < 300) {
    requestAnimationFrame(animate);  // schedule next frame
  }
}
requestAnimationFrame(animate);

// Cancel
const frameId = requestAnimationFrame(callback);
cancelAnimationFrame(frameId);

// rAF runs AFTER microtasks, BEFORE paint (between microtask and macrotask)
```

**Interview Q: Why use requestAnimationFrame instead of setTimeout for animations?** 🟡
**A:** rAF syncs with the browser's refresh rate (usually 60fps). setTimeout might fire too early or too late, causing janky animations. rAF is also paused when the tab is inactive (saves battery/CPU). setTimeout keeps firing.

**One-liner:** rAF = before next paint (~60fps). Better than setTimeout for animations. Syncs with display. Pauses in background tabs.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 10: DOM Manipulation

---

### 🔴 10.1 Selecting Elements

**Explanation:** Methods to find elements in the DOM.

```javascript
// By ID — returns single element
document.getElementById('header');

// By selector — returns first match
document.querySelector('.card');          // first element with class "card"
document.querySelector('#main > .title'); // CSS selector

// By selector — returns ALL matches (NodeList)
document.querySelectorAll('.card');       // all elements with class "card"

// Older methods (still work)
document.getElementsByClassName('card');  // HTMLCollection (live)
document.getElementsByTagName('div');     // HTMLCollection (live)

// querySelectorAll returns STATIC NodeList (snapshot)
// getElementsBy* returns LIVE HTMLCollection (auto-updates)
```

**Interview Q: What is the difference between querySelector and getElementById?** 🔴
**A:** `getElementById` only finds by ID and is slightly faster. `querySelector` accepts any CSS selector (class, tag, attribute, nested selectors) and is more versatile. For IDs, both work; for anything else, use querySelector.

**One-liner:** getElementById = by ID only. querySelector = any CSS selector. querySelectorAll = all matches (static). getElementsBy* = live collection.

---

### 🔴 10.2 Event Delegation

**Explanation:** Instead of attaching listeners to many child elements, attach ONE listener to the parent. When a child is clicked, the event bubbles up to the parent, which handles it.

```javascript
// ❌ BAD — one listener per button (100 buttons = 100 listeners)
document.querySelectorAll('.btn').forEach(btn => {
  btn.addEventListener('click', handleClick);
});

// ✅ GOOD — one listener on parent (handles all current AND future buttons)
document.getElementById('button-container').addEventListener('click', (e) => {
  if (e.target.matches('.btn')) {   // check if clicked element is a button
    console.log('Button clicked:', e.target.textContent);
  }
});

// Works for dynamically added elements too!
// New buttons added later are automatically handled by the parent listener.
```

**Interview Q: What is event delegation and why use it?** 🔴
**A:** Attaching a single event listener to a parent element instead of many listeners on children. Benefits: (1) Better performance (fewer listeners), (2) Works with dynamically added elements, (3) Less memory usage. Uses event bubbling — click on child bubbles up to parent.

**One-liner:** Event delegation = one listener on parent handles all children. Uses bubbling. Better performance. Works for dynamic elements.

---

### 🔴 10.3 Event Bubbling vs Capturing

**Explanation:** When an event occurs on an element, it travels in two phases: Capturing (top → target) then Bubbling (target → top). By default, listeners fire during bubbling.

```
CLICK on <span> inside <div> inside <body>:

  CAPTURING (top → down):   body → div → span
  TARGET:                    span (event fires on target)
  BUBBLING (bottom → up):   span → div → body  ← default phase

  Event handlers fire during BUBBLING by default.
```

```javascript
// Bubbling (default)
parent.addEventListener('click', () => console.log('parent'));
child.addEventListener('click', () => console.log('child'));
// Click child → "child", then "parent" (bubbles up)

// Capturing — add { capture: true } or just true as third arg
parent.addEventListener('click', () => console.log('parent'), true);
// Now parent fires BEFORE child during capturing phase

// Stop bubbling
child.addEventListener('click', (e) => {
  e.stopPropagation();  // prevents event from reaching parent
  console.log('child only');
});

// Prevent default behavior (e.g., link navigation, form submit)
link.addEventListener('click', (e) => {
  e.preventDefault();    // stops the <a> from navigating
});
```

**Interview Q: What is the difference between stopPropagation and preventDefault?** 🔴

| | stopPropagation | preventDefault |
|---|---|---|
| Does | Stops event from bubbling to parent | Stops browser's default action |
| Example | Click on child doesn't reach parent | Link doesn't navigate, form doesn't submit |
| Affects other listeners? | Other elements' listeners don't fire | The element's own action is cancelled |

**One-liner:** Bubbling = child → parent (default). Capturing = parent → child. stopPropagation stops bubbling. preventDefault stops browser action.

---

### 🟡 10.4 Creating and Modifying Elements

**Explanation:** Methods to create elements, set content, and add them to the DOM.

```javascript
// CREATE
const div = document.createElement('div');
div.className = 'card';
div.id = 'new-card';
div.textContent = 'Hello World';  // safe (no HTML parsing)
// div.innerHTML = '<strong>Bold</strong>';  // parses HTML (XSS risk!)

// MODIFY attributes
div.setAttribute('data-id', '42');
div.getAttribute('data-id');       // "42"
div.style.backgroundColor = 'blue';
div.classList.add('active');
div.classList.remove('hidden');
div.classList.toggle('selected');
div.classList.contains('active');   // true

// ADD to DOM
document.body.appendChild(div);              // add as last child
document.body.prepend(div);                  // add as first child
parentEl.insertBefore(newEl, referenceEl);   // insert before reference

// REMOVE
div.remove();                          // modern way
parentEl.removeChild(div);             // older way

// CLONE
const clone = div.cloneNode(true);     // true = deep clone (with children)
```

**Interview Q: textContent vs innerHTML vs innerText?** 🟡

| | textContent | innerHTML | innerText |
|---|---|---|---|
| Returns | All text (including hidden) | HTML markup as string | Visible text only |
| Parses HTML? | No (safe) | Yes (XSS risk!) | No |
| Performance | Fast | Slower (parses HTML) | Slowest (considers CSS) |
| Use for | Setting text safely | Setting HTML content | Getting visible text |

**One-liner:** textContent = safe text (no HTML). innerHTML = renders HTML (XSS risk). innerText = visible text only (slow). Use textContent for safety.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 11: ES6+ Features

---

### 🔴 11.1 Modules (import / export)

**Explanation:** ES6 modules split code into files. `export` makes values available. `import` brings them into another file. Modules have their own scope (no global pollution).

```javascript
// === math.js ===
// Named exports (can have many)
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export function multiply(a, b) { return a * b; }

// Default export (one per file)
export default class Calculator {
  // ...
}

// === app.js ===
// Import named exports (must match names, use {})
import { add, multiply, PI } from './math.js';
import { add as addition } from './math.js';  // rename

// Import default export (any name works, no {})
import Calculator from './math.js';
import MyCalc from './math.js';  // same thing, different name

// Import everything
import * as math from './math.js';
math.add(1, 2);
math.PI;
```

**Interview Q: What is the difference between named and default exports?** 🔴

| | Named Export | Default Export |
|---|---|---|
| Syntax | `export const x` / `export { x }` | `export default x` |
| Import | `import { x } from` (must match name) | `import anyName from` |
| Per file | Multiple allowed | Only one |
| Renaming | `import { x as y }` | Automatic (any name) |

**One-liner:** Named: `export { x }` → `import { x }`. Default: `export default x` → `import anyName`. Named needs {}, default doesn't.

---

### 🔴 11.2 Classes

**Explanation:** ES6 classes are syntactic sugar over prototype-based inheritance. They provide cleaner syntax for constructors, methods, and inheritance.

```javascript
class Animal {
  // Constructor — runs when new instance created
  constructor(name, sound) {
    this.name = name;
    this.sound = sound;
  }

  // Method
  speak() {
    console.log(`${this.name} says ${this.sound}`);
  }

  // Static method — called on class, not instances
  static create(name, sound) {
    return new Animal(name, sound);
  }

  // Getter
  get info() {
    return `${this.name}: ${this.sound}`;
  }
}

const cat = new Animal("Cat", "Meow");
cat.speak();        // "Cat says Meow"
cat.info;           // "Cat: Meow" (getter, no parentheses)
Animal.create("Dog", "Woof"); // static method

// Inheritance with extends
class Dog extends Animal {
  constructor(name) {
    super(name, "Woof");  // call parent constructor
  }

  fetch(item) {
    console.log(`${this.name} fetches ${item}`);
  }
}

const dog = new Dog("Rex");
dog.speak();       // "Rex says Woof" (inherited)
dog.fetch("ball"); // "Rex fetches ball" (own method)
dog instanceof Dog;    // true
dog instanceof Animal; // true
```

**Interview Q: What does `super` do?** 🔴
**A:** `super()` calls the parent class constructor — required in a child constructor before using `this`. `super.method()` calls a parent method from within an overriding child method.

**Mistake:** Forgetting `super()` in a child constructor. JS throws `ReferenceError: Must call super constructor before accessing 'this'`.

**One-liner:** class = cleaner syntax for prototypes. constructor() for init. extends + super for inheritance. static for class-level methods.

---

### 🟡 11.3 Optional Chaining (?.) and Nullish Coalescing (??)

**Explanation:** `?.` safely accesses nested properties (returns `undefined` instead of crashing on null/undefined). `??` provides a default only for `null`/`undefined` (not for `0` or `""`).

```javascript
const user = {
  name: "Alice",
  address: {
    city: "Delhi"
  }
};

// ❌ WITHOUT optional chaining — crashes if address is null
// user.address.zip.code  // TypeError if zip is undefined

// ✅ WITH optional chaining — returns undefined safely
user.address?.zip?.code;       // undefined (no error)
user.profile?.avatar;          // undefined (no error)
user.getName?.();              // undefined (safe function call)
users?.[0]?.name;              // safe array access

// NULLISH COALESCING (??) vs OR (||)
const count = 0;
count || 10;    // 10 — || treats 0 as falsy!
count ?? 10;    // 0  — ?? only defaults for null/undefined

const text = "";
text || "default";   // "default" — || treats "" as falsy
text ?? "default";   // ""        — ?? keeps "" (not null/undefined)

null ?? "default";   // "default"
undefined ?? "default"; // "default"
```

**Interview Q: When would you use ?? instead of ||?** 🟡
**A:** When `0`, `""`, or `false` are valid values that should NOT be replaced. `||` treats all falsy values as "missing." `??` only treats `null` and `undefined` as "missing." Example: `user.age ?? 0` — if age is `0`, `||` would give the default, but `??` correctly keeps `0`.

**One-liner:** ?. = safe property access (undefined instead of crash). ?? = default only for null/undefined (keeps 0 and ""). || defaults for ALL falsy.

---

### 🟡 11.4 Map, Set, WeakMap, WeakSet

**Explanation:** Map = key-value pairs with any key type. Set = unique values. Weak versions allow garbage collection of keys.

```javascript
// MAP — any type as key, ordered, has .size
const map = new Map();
map.set("name", "Alice");
map.set(1, "one");
map.set(true, "yes");
map.get("name");    // "Alice"
map.has(1);         // true
map.size;           // 3

// SET — unique values only
const set = new Set([1, 2, 2, 3, 3, 3]);
console.log(set);   // {1, 2, 3}
set.add(4);
set.has(2);         // true
set.size;           // 4

// WEAKMAP — keys must be objects, garbage-collectible
const weakMap = new WeakMap();
let obj = { data: "secret" };
weakMap.set(obj, "hidden value");
obj = null;  // obj can now be garbage collected, and its WeakMap entry too

// WEAKSET — values must be objects, garbage-collectible
const weakSet = new WeakSet();
```

| | Map | Object | WeakMap |
|---|---|---|---|
| Key types | Any | String/Symbol | Objects only |
| Ordered | Yes | Sort of (since ES2015) | No |
| Size | .size | Manual count | N/A |
| Iterable | Yes | Yes (Object.keys) | No |
| GC of keys | No | No | Yes |

**Interview Q: When would you use Map over a plain Object?** 🟡
**A:** When keys are not strings (numbers, objects, etc.), when you need insertion order, when you need `.size`, when you add/remove keys frequently (Map is optimized for this), or when you don't want prototype-inherited properties.

**One-liner:** Map = any-type keys + .size + ordered. Set = unique values. WeakMap/WeakSet = garbage-collectible keys (objects only, not iterable).

---

### 🟡 11.5 for...of vs for...in

**Explanation:** `for...of` iterates over VALUES (arrays, strings, maps, sets — anything iterable). `for...in` iterates over KEYS (object property names, including inherited ones).

```javascript
const arr = [10, 20, 30];

// for...of — VALUES (use for arrays, strings, maps, sets)
for (const value of arr) {
  console.log(value);  // 10, 20, 30
}

// for...in — KEYS/INDICES (use for objects, NOT arrays)
for (const index in arr) {
  console.log(index);  // "0", "1", "2" (strings, not numbers!)
}

// for...in on objects
const obj = { a: 1, b: 2, c: 3 };
for (const key in obj) {
  console.log(key, obj[key]);  // a 1, b 2, c 3
}

// ⚠️ for...in includes inherited properties!
const child = Object.create({ inherited: true });
child.own = "mine";
for (const key in child) {
  console.log(key);  // "own", "inherited" — includes prototype!
}
// Fix: filter with hasOwnProperty
for (const key in child) {
  if (child.hasOwnProperty(key)) console.log(key);  // "own" only
}
```

**Interview Q: Why shouldn't you use for...in with arrays?** 🟡
**A:** (1) It iterates over indices as STRINGS ("0", "1"), not numbers. (2) It includes inherited properties from the prototype. (3) Order is not guaranteed for all engines. Use `for...of` or `.forEach()` for arrays.

**One-liner:** for...of = values (arrays, strings, iterables). for...in = keys (objects). Never use for...in on arrays. for...in includes inherited.

---

### 🟢 11.6 Generators

**Explanation:** Generator functions can pause and resume. They use `function*` and `yield` keyword. Each call to `.next()` runs until the next `yield` and returns `{ value, done }`.

```javascript
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = numberGenerator();
gen.next();  // { value: 1, done: false }
gen.next();  // { value: 2, done: false }
gen.next();  // { value: 3, done: false }
gen.next();  // { value: undefined, done: true }

// Generators are iterable
for (const num of numberGenerator()) {
  console.log(num);  // 1, 2, 3
}

// Practical: infinite sequence
function* ids() {
  let id = 1;
  while (true) {
    yield id++;
  }
}
const idGen = ids();
idGen.next().value;  // 1
idGen.next().value;  // 2
idGen.next().value;  // 3 (can go forever, generates on demand)

// Practical: lazy evaluation
function* range(start, end) {
  for (let i = start; i <= end; i++) {
    yield i;  // generates one at a time, not all at once
  }
}
```

**Interview Q: What are generators used for?** 🟢
**A:** Lazy evaluation (generate values on demand), infinite sequences, implementing iterators, controlling async flow (before async/await), and custom iterable objects.

**One-liner:** function* + yield = pausable function. .next() resumes. Returns { value, done }. For lazy evaluation, infinite sequences, custom iterables.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 12: Error Handling

---

### 🔴 12.1 try, catch, finally

**Explanation:** `try` wraps code that might throw errors. `catch` handles the error. `finally` runs regardless of error or not — used for cleanup.

```javascript
try {
  const data = JSON.parse('invalid json');
} catch (error) {
  console.error('Type:', error.name);      // "SyntaxError"
  console.error('Message:', error.message); // "Unexpected token i..."
  console.error('Stack:', error.stack);     // full stack trace
} finally {
  console.log('Cleanup here');  // runs no matter what
}

// Selective error handling
try {
  riskyOperation();
} catch (err) {
  if (err instanceof TypeError) {
    console.log('Type error:', err.message);
  } else if (err instanceof RangeError) {
    console.log('Range error:', err.message);
  } else {
    throw err;  // re-throw unknown errors
  }
}
```

**Interview Q: Does finally run even if catch throws?** 🔴
**A:** Yes. `finally` ALWAYS runs — after `try` succeeds, after `catch` handles error, and even if `catch` throws a new error or `try`/`catch` has a `return` statement. It's guaranteed cleanup.

**One-liner:** try = risky code. catch(err) = handle error. finally = always runs (cleanup). finally runs even if catch throws or try returns.

---

### 🔴 12.2 throw and Custom Errors

**Explanation:** `throw` creates an error manually. You can throw any value, but best practice is to throw Error objects.

```javascript
// Throw built-in errors
function divide(a, b) {
  if (b === 0) throw new Error("Division by zero");
  return a / b;
}

// Custom error classes
class ValidationError extends Error {
  constructor(field, message) {
    super(message);
    this.name = 'ValidationError';
    this.field = field;
  }
}

class NotFoundError extends Error {
  constructor(resource) {
    super(`${resource} not found`);
    this.name = 'NotFoundError';
    this.statusCode = 404;
  }
}

// Usage
function validateAge(age) {
  if (typeof age !== 'number') {
    throw new ValidationError('age', 'Age must be a number');
  }
  if (age < 0 || age > 150) {
    throw new ValidationError('age', 'Age must be between 0 and 150');
  }
}

try {
  validateAge("twenty");
} catch (err) {
  if (err instanceof ValidationError) {
    console.log(`Field: ${err.field}, Message: ${err.message}`);
  }
}
```

**Interview Q: Why create custom error classes?** 🟡
**A:** To distinguish between different types of errors (validation vs not-found vs auth), add custom properties (field, statusCode), and use `instanceof` for selective handling. Built-in Error types are too generic.

**One-liner:** throw new Error("msg") for manual errors. Extend Error for custom types with extra properties. Use instanceof to handle selectively.

---

### 🟡 12.3 Error Handling in Async Code

**Explanation:** Promises use `.catch()`. async/await uses `try/catch`. Unhandled rejections cause warnings/crashes — always handle them.

```javascript
// Promise errors — .catch()
fetch('/api/data')
  .then(res => res.json())
  .then(data => processData(data))
  .catch(err => console.error('Failed:', err));  // catches ANY error in chain

// Async/await errors — try/catch
async function loadData() {
  try {
    const res = await fetch('/api/data');
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    return await res.json();
  } catch (err) {
    console.error('Failed:', err);
  }
}

// Global handlers (last resort — catch unhandled errors)
// Browser
window.addEventListener('unhandledrejection', (event) => {
  console.error('Unhandled promise rejection:', event.reason);
});

window.addEventListener('error', (event) => {
  console.error('Uncaught error:', event.error);
});

// Node.js
process.on('unhandledRejection', (reason) => {
  console.error('Unhandled rejection:', reason);
});
```

**Interview Q: How do you handle errors in async/await?** 🟡
**A:** Wrap await calls in try/catch. For multiple independent awaits, use Promise.allSettled to get results regardless of individual failures. For global safety, add `unhandledrejection` event listeners.

**One-liner:** Promises: .catch(). async/await: try/catch. Global: window 'unhandledrejection'. ALWAYS handle errors — unhandled rejections crash Node.

---

### 🔴 12.4 Error Types

**Explanation:** JavaScript has several built-in error types that indicate what went wrong.

```javascript
// TypeError — wrong type operation
null.toString();           // TypeError: Cannot read properties of null
undefined.foo;             // TypeError in strict mode

// ReferenceError — using undeclared variable
console.log(undeclared);   // ReferenceError: undeclared is not defined

// SyntaxError — invalid code structure
// eval('if(');             // SyntaxError: Unexpected end of input
// JSON.parse('{invalid}'); // SyntaxError

// RangeError — value out of valid range
new Array(-1);              // RangeError: Invalid array length
function recurse() { recurse(); }
// recurse();               // RangeError: Maximum call stack size exceeded

// URIError — invalid URI
decodeURIComponent('%');    // URIError
```

**Interview Q: What are the common JavaScript error types?** 🔴

| Error Type | When It Occurs |
|---|---|
| TypeError | Wrong type (calling non-function, accessing null property) |
| ReferenceError | Using undeclared variable |
| SyntaxError | Invalid code syntax |
| RangeError | Value out of allowed range (stack overflow, invalid array length) |
| URIError | Invalid URI encoding/decoding |

**One-liner:** TypeError = wrong type. ReferenceError = undeclared var. SyntaxError = bad syntax. RangeError = out of range / stack overflow.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Quick Revision — Part 2 One-Liners

```
ARRAYS
  map = transform (same size). filter = keep matches (shorter). reduce = combine into one
  find = first match. some = any? every = all? includes = value exists?
  slice = copy section (immutable). splice = insert/remove (mutates)
  push/pop = end (O(1)). shift/unshift = start (O(n))
  sort() is ALPHABETICAL by default! Use .sort((a,b) => a-b) for numbers. Mutates!
  flat(depth) = flatten. flatMap = map + flat(1)
  Polyfills: loop through this, callback(el, i, arr), push result

ASYNC JAVASCRIPT
  Sync = blocking. Async = non-blocking. JS uses event loop
  Promise: pending → fulfilled (.then) or rejected (.catch). Always .catch()
  all = all succeed. allSettled = all finish. race = first settled. any = first resolved
  async/await = sugar over Promises. try/catch for errors. Promise.all for parallel
  Microtasks (Promise) run before macrotasks (setTimeout)

EVENT LOOP
  Call stack = LIFO, tracks running functions. Overflow = infinite recursion
  Event loop: sync code → all microtasks → one macrotask → repeat
  Execution context = variables + scope chain + this. Created per function call
  setTimeout(fn, 0) ≠ instant. Waits for sync + microtasks
  requestAnimationFrame = before paint, syncs with 60fps

DOM MANIPULATION
  querySelector = any CSS selector. querySelectorAll = all matches
  Event delegation = one listener on parent. Uses bubbling. Handles dynamic elements
  Bubbling = child → parent (default). Capturing = parent → child
  stopPropagation = stop bubbling. preventDefault = stop browser action
  textContent = safe text. innerHTML = HTML (XSS risk). createElement + appendChild

ES6+ FEATURES
  Named export: export { x } → import { x }. Default: export default → import anyName
  class + constructor + extends + super. Static methods on class, not instances
  ?. = safe access. ?? = default for null/undefined only (keeps 0 and "")
  Map = any-type keys. Set = unique values. WeakMap/WeakSet = GC-friendly
  for...of = values (arrays). for...in = keys (objects, includes inherited)
  Generators: function* + yield. Pausable. Lazy evaluation

ERROR HANDLING
  try/catch/finally. finally ALWAYS runs
  throw new Error("msg"). Extend Error for custom types
  Promises: .catch(). async/await: try/catch. Global: unhandledrejection event
  TypeError, ReferenceError, SyntaxError, RangeError
```

[↑ Back to Table of Contents](#table-of-contents--part-2)
