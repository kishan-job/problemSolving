# JavaScript Complete Interview Guide
## Part 3: Categories 13-16 (Browser APIs, Performance, Coding Challenges, Tricky Questions)

---

# Table of Contents — Part 3

- [Category 13: Storage & Browser APIs](#category-13-storage--browser-apis)
- [Category 14: Performance & Optimization](#category-14-performance--optimization)
- [Category 15: Coding Challenges — Implement From Scratch](#category-15-coding-challenges)
- [Category 16: Tricky Output Questions](#category-16-tricky-output-questions)
- [Quick Revision — Part 3 One-Liners](#quick-revision--part-3-one-liners)

---

# Category 13: Storage & Browser APIs

---

### 🔴 13.1 localStorage vs sessionStorage

**Explanation:** Both store key-value pairs in the browser. `localStorage` persists forever (until cleared). `sessionStorage` persists only for the tab's session (cleared when tab closes).

```javascript
// localStorage — persists across sessions
localStorage.setItem('theme', 'dark');
localStorage.getItem('theme');           // "dark"
localStorage.removeItem('theme');
localStorage.clear();                    // remove everything

// sessionStorage — same API, cleared when tab closes
sessionStorage.setItem('token', 'abc123');
sessionStorage.getItem('token');         // "abc123"

// ⚠️ Both store STRINGS only
localStorage.setItem('user', { name: 'Alice' }); // stores "[object Object]" — WRONG!

// FIX: serialize/deserialize with JSON
localStorage.setItem('user', JSON.stringify({ name: 'Alice' }));
const user = JSON.parse(localStorage.getItem('user')); // { name: 'Alice' }

// Check if key exists
if (localStorage.getItem('theme') !== null) {
  // key exists
}
```

**Interview Q: localStorage vs sessionStorage vs cookies?** 🔴

| | localStorage | sessionStorage | Cookies |
|---|---|---|---|
| Persistence | Until cleared manually | Until tab closes | Expiry date (or session) |
| Size | ~5-10 MB | ~5-10 MB | ~4 KB |
| Sent to server? | No | No | Yes (every HTTP request!) |
| Access | Client-side JS | Client-side JS | Client + Server |
| Scope | Same origin | Same origin + same tab | Same origin (configurable) |

**Mistake:** Storing sensitive data (tokens, passwords) in localStorage — it's accessible via JS and vulnerable to XSS attacks. Use httpOnly cookies for auth tokens.

**One-liner:** localStorage = persists forever, 5-10MB, client only. sessionStorage = tab session only. Cookies = 4KB, sent to server. All store strings only.

---

### 🔴 13.2 Cookies

**Explanation:** Cookies are small pieces of data stored in the browser and automatically sent with every HTTP request to the same origin. Used for auth, tracking, and preferences.

```javascript
// SET a cookie
document.cookie = "theme=dark; max-age=86400; path=/";
// max-age = seconds until expiry (86400 = 1 day)
// path = which URLs the cookie applies to

// SET with expiry date
document.cookie = "user=Alice; expires=Fri, 31 Dec 2026 23:59:59 GMT";

// SET secure + httpOnly (httpOnly can only be set by server, not JS!)
// document.cookie = "token=abc; Secure; SameSite=Strict";

// READ all cookies (returns ONE string with all cookies)
console.log(document.cookie);  // "theme=dark; user=Alice"

// Parse a specific cookie (no built-in method — must parse manually)
function getCookie(name) {
  const cookies = document.cookie.split('; ');
  const found = cookies.find(c => c.startsWith(name + '='));
  return found ? found.split('=')[1] : null;
}
getCookie('theme');  // "dark"

// DELETE a cookie (set max-age to 0 or past expiry)
document.cookie = "theme=; max-age=0";
```

**Interview Q: What are httpOnly cookies and why are they important?** 🟡
**A:** httpOnly cookies can't be accessed by JavaScript (`document.cookie` won't show them). They can only be set and read by the server. This prevents XSS attacks from stealing auth tokens — even if an attacker injects JS, they can't read the cookie.

**One-liner:** Cookies = sent with every request. 4KB limit. httpOnly = invisible to JS (XSS-safe). Secure = HTTPS only. SameSite = CSRF protection.

---

### 🟡 13.3 JSON.parse / JSON.stringify Limitations

**Explanation:** `JSON.stringify` converts JS values to JSON strings. `JSON.parse` converts JSON strings back. But they have limitations.

```javascript
// Basic usage
JSON.stringify({ name: "Alice", age: 25 }); // '{"name":"Alice","age":25}'
JSON.parse('{"name":"Alice","age":25}');      // { name: "Alice", age: 25 }

// ❌ LIMITATIONS — these are LOST during stringify:
const obj = {
  fn: function() {},     // functions → REMOVED
  undef: undefined,       // undefined → REMOVED
  sym: Symbol("x"),      // symbols → REMOVED
  date: new Date(),       // Date → becomes string (not parsed back as Date)
  regex: /hello/,         // RegExp → becomes {}
  infinity: Infinity,     // Infinity → becomes null
  nan: NaN,               // NaN → becomes null
  bigint: 10n,            // BigInt → ❌ THROWS TypeError!
};

JSON.stringify(obj);
// '{"date":"2026-04-09T...","regex":{},"infinity":null,"nan":null}'
// fn, undef, sym are GONE. date is a string. regex is empty object.

// ❌ CIRCULAR REFERENCES — throws error
const a = {};
a.self = a;
// JSON.stringify(a); // ❌ TypeError: Converting circular structure

// ✅ Deep clone alternative: structuredClone (handles more types)
const clone = structuredClone(obj); // handles Date, RegExp, Map, Set, circular refs
// But still can't clone functions or DOM elements
```

**Interview Q: What does JSON.stringify lose?** 🟡
**A:** Functions, undefined, Symbols are silently removed. Date becomes string. RegExp becomes `{}`. Infinity and NaN become `null`. BigInt throws TypeError. Circular references throw TypeError. For reliable deep cloning, use `structuredClone()`.

**One-liner:** JSON.stringify loses: functions, undefined, Symbol, Date accuracy, RegExp, Infinity/NaN (→ null), BigInt (throws). Use structuredClone for reliable deep copy.

---

### 🟡 13.4 Intersection Observer

**Explanation:** Detects when an element enters or exits the viewport. Used for lazy loading images, infinite scroll, scroll-triggered animations, and analytics (tracking impressions).

```javascript
// Create observer
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      console.log('Element is visible!', entry.target);
      // entry.intersectionRatio — 0 to 1 (how much is visible)
    }
  });
}, {
  root: null,        // viewport (null = browser viewport)
  rootMargin: '0px', // margin around root (e.g., '100px' to trigger before visible)
  threshold: 0.5,    // trigger when 50% visible (0 = any pixel, 1 = fully visible)
});

// Observe an element
const element = document.querySelector('.lazy-image');
observer.observe(element);

// Stop observing
observer.unobserve(element);
observer.disconnect();  // stop all observations

// PRACTICAL: Lazy load images
const imgObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;  // load real image
      imgObserver.unobserve(img);  // stop watching (loaded)
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => {
  imgObserver.observe(img);
});
```

**Interview Q: How would you implement lazy loading for images?** 🟡
**A:** Use Intersection Observer. Set `data-src` with the real URL and leave `src` empty/placeholder. When the image enters the viewport, copy `data-src` to `src` and unobserve. More efficient than scroll event listeners (no continuous listening).

**One-liner:** IntersectionObserver = detect when element enters viewport. For lazy loading, infinite scroll, animations. Better than scroll events.

---

### 🟢 13.5 Web Workers

**Explanation:** Web Workers run JavaScript in a background thread, separate from the main thread. This prevents heavy computations from blocking the UI. Workers can't access the DOM.

```javascript
// === main.js ===
const worker = new Worker('worker.js');

// Send data TO worker
worker.postMessage({ numbers: [1, 2, 3, 4, 5] });

// Receive data FROM worker
worker.onmessage = (event) => {
  console.log('Sum:', event.data.sum);  // 15
};

// Handle errors
worker.onerror = (error) => {
  console.error('Worker error:', error.message);
};

// Terminate worker
worker.terminate();


// === worker.js ===
self.onmessage = (event) => {
  const { numbers } = event.data;

  // Heavy computation (doesn't block main thread!)
  const sum = numbers.reduce((a, b) => a + b, 0);

  // Send result back
  self.postMessage({ sum });
};
```

```
MAIN THREAD                    WORKER THREAD
┌──────────────┐               ┌──────────────┐
│ UI rendering │               │ Heavy calc   │
│ Event handling│  postMessage  │ No DOM access│
│ DOM access   │ ←──────────→  │ No window    │
│ User input   │               │ Has fetch,   │
│              │               │ setTimeout   │
└──────────────┘               └──────────────┘
```

**Interview Q: What can't Web Workers do?** 🟢
**A:** Can't access: DOM (`document`), `window` object, parent scope variables. Can access: `fetch`, `setTimeout`, `WebSocket`, `IndexedDB`, `importScripts`. Communication only via `postMessage` (data is copied, not shared).

**One-liner:** Web Workers = background thread for heavy computation. Can't access DOM. Communicate via postMessage. Prevents UI freezing.

---

### 🟢 13.6 WebSockets

**Explanation:** WebSocket creates a persistent two-way connection between browser and server. Unlike HTTP (request → response), WebSocket keeps the connection open for real-time data.

```javascript
// Create connection
const ws = new WebSocket('wss://example.com/chat');

// Connection opened
ws.addEventListener('open', () => {
  console.log('Connected!');
  ws.send(JSON.stringify({ type: 'join', room: 'general' }));
});

// Receive message
ws.addEventListener('message', (event) => {
  const data = JSON.parse(event.data);
  console.log('Received:', data);
});

// Connection closed
ws.addEventListener('close', (event) => {
  console.log('Disconnected:', event.code, event.reason);
});

// Error
ws.addEventListener('error', (error) => {
  console.error('WebSocket error:', error);
});

// Send message
ws.send(JSON.stringify({ text: 'Hello!' }));

// Close connection
ws.close();
```

**Interview Q: WebSocket vs HTTP — when to use each?** 🟢

| | HTTP | WebSocket |
|---|---|---|
| Connection | New connection per request | Persistent (stays open) |
| Direction | Client → Server (request/response) | Both ways (full-duplex) |
| Overhead | Headers sent every request | Headers only on handshake |
| Use case | REST APIs, page loads | Chat, gaming, live data, notifications |
| Protocol | http:// / https:// | ws:// / wss:// |

**One-liner:** WebSocket = persistent two-way connection. For real-time: chat, gaming, live updates. HTTP = request/response (one-way per request).

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 14: Performance & Optimization

---

### 🔴 14.1 Debouncing vs Throttling

**Explanation:** Both limit how often a function runs. Debounce = wait until user STOPS (fires once after pause). Throttle = allow at most once per interval (fires at regular rate).

```javascript
// === DEBOUNCE === "Wait until they stop, then fire"
// Use for: search input, form validation, auto-save, resize
function debounce(fn, delay) {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn.apply(this, args), delay);
  };
}

const debouncedSearch = debounce((query) => {
  console.log('Searching:', query);
}, 300);

// User types: h → he → hel → hello
// Only ONE search fires: "hello" (300ms after last keystroke)


// === THROTTLE === "Fire at most once every X ms"
// Use for: scroll handler, resize handler, mouse move, game input
function throttle(fn, limit) {
  let inThrottle = false;
  return function(...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

const throttledScroll = throttle(() => {
  console.log('Scroll position:', window.scrollY);
}, 200);

window.addEventListener('scroll', throttledScroll);
// Fires at most once every 200ms during scroll
```

**Interview Q: Debounce vs Throttle — when to use which?** 🔴

| | Debounce | Throttle |
|---|---|---|
| Fires when | After user STOPS for X ms | At most once every X ms |
| Pattern | Wait → fire once at the end | Fire → wait → fire → wait |
| Use for | Search, validation, auto-save | Scroll, resize, mouse move |
| User types fast | Fires once (after they stop) | Fires multiple times (at rate) |

**One-liner:** Debounce = fire after pause ("wait till they stop"). Throttle = fire at fixed rate ("at most once per X ms").

---

### 🟡 14.2 Lazy Loading

**Explanation:** Load resources (images, components, routes) only when needed, not upfront. Reduces initial page load time.

```javascript
// LAZY LOAD IMAGES — native browser support
<img src="placeholder.jpg" loading="lazy" alt="..." />

// LAZY LOAD IMAGES — Intersection Observer (more control)
const observer = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.src = entry.target.dataset.src;
      observer.unobserve(entry.target);
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => observer.observe(img));

// LAZY LOAD JS MODULES — dynamic import
button.addEventListener('click', async () => {
  const { heavyFunction } = await import('./heavyModule.js');
  heavyFunction();
});

// LAZY LOAD REACT COMPONENTS
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));
```

**Interview Q: What are different ways to lazy load?** 🟡
**A:** (1) `loading="lazy"` on images (native browser). (2) Intersection Observer for images/elements. (3) Dynamic `import()` for JS modules. (4) `React.lazy()` for React components. (5) Route-based code splitting.

**One-liner:** Lazy loading = load only when needed. Images: loading="lazy" or IntersectionObserver. JS: dynamic import(). React: React.lazy().

---

### 🟡 14.3 Memoization for Performance

**Explanation:** Cache expensive function results. If the same input appears again, return the cached result instead of recomputing.

```javascript
// Generic memoize function
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
}

// Without memoization — O(2^n) recursive calls
function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}
fib(40); // ~1.5 seconds (millions of redundant calls)

// With memoization — O(n) only
const memoFib = memoize(fib);
memoFib(40); // instant (each value computed once)

// Or manual memoization
function fibMemo(n, memo = {}) {
  if (n in memo) return memo[n];
  if (n <= 1) return n;
  memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
  return memo[n];
}
```

**Interview Q: When is memoization beneficial?** 🟡
**A:** For pure functions (same input → same output) that are called multiple times with the same arguments, especially expensive computations (recursive algorithms, API responses, heavy calculations). Not useful for functions with side effects or always-unique inputs.

**One-liner:** Memoization = cache results. Same input → cached result (skip recomputation). Use for expensive pure functions. Map for cache.

---

### 🟡 14.4 Reflow vs Repaint

**Explanation:** Repaint = visual change only (color, shadow). Reflow = layout change (size, position) — the browser recalculates geometry for ALL affected elements. Reflow is MUCH more expensive.

```javascript
// REPAINT — visual only, cheaper
element.style.color = 'red';
element.style.backgroundColor = 'blue';
element.style.visibility = 'hidden';

// REFLOW — layout change, expensive (triggers repaint too)
element.style.width = '100px';
element.style.height = '200px';
element.style.margin = '10px';
element.style.display = 'none';
element.style.fontSize = '20px';
// Also triggered by: reading offsetHeight, scrollTop, getBoundingClientRect()

// ❌ BAD — multiple reflows
element.style.width = '100px';   // reflow
element.style.height = '200px';  // reflow
element.style.margin = '10px';   // reflow

// ✅ GOOD — batch changes with class
element.className = 'new-styles';  // one reflow

// ✅ GOOD — batch with cssText
element.style.cssText = 'width:100px; height:200px; margin:10px';  // one reflow

// ✅ GOOD — batch with fragment (for multiple DOM additions)
const fragment = document.createDocumentFragment();
for (let i = 0; i < 1000; i++) {
  const div = document.createElement('div');
  div.textContent = i;
  fragment.appendChild(div);  // no reflow yet
}
document.body.appendChild(fragment);  // ONE reflow
```

**Interview Q: How do you minimize reflows?** 🟡
**A:** (1) Batch DOM changes (class toggle, cssText, DocumentFragment). (2) Avoid reading layout properties (offsetHeight) between writes. (3) Use `display: none`, make all changes, then show again. (4) Use CSS transforms instead of top/left for animations. (5) Use `requestAnimationFrame` for animations.

**One-liner:** Repaint = visual only (cheap). Reflow = layout recalculation (expensive). Batch changes with class/cssText/Fragment. Avoid read-write-read pattern.

---

### 🟢 14.5 Web Vitals (LCP, FID, CLS)

**Explanation:** Core Web Vitals are Google's metrics for user experience. They affect SEO ranking.

```
LCP (Largest Contentful Paint) — Loading performance
  "How fast does the main content appear?"
  Target: < 2.5 seconds
  Fix: Optimize images, preload fonts, reduce server time, lazy load below-fold

FID (First Input Delay) — Interactivity
  "How fast can the user interact?" (replaced by INP in 2024)
  Target: < 100ms
  Fix: Break long JS tasks, use web workers, defer non-critical scripts

CLS (Cumulative Layout Shift) — Visual stability
  "Does the page jump around while loading?"
  Target: < 0.1
  Fix: Set image dimensions, reserve space for ads/embeds, avoid inserting content above existing

INP (Interaction to Next Paint) — Replaced FID in 2024
  "How fast does the page respond to ANY interaction?"
  Target: < 200ms
```

```javascript
// Measure with Performance API
new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    console.log(`${entry.name}: ${entry.startTime}ms`);
  }
}).observe({ type: 'largest-contentful-paint', buffered: true });

// Or use web-vitals library
import { onLCP, onFID, onCLS } from 'web-vitals';
onLCP(console.log);
onFID(console.log);
onCLS(console.log);
```

**Interview Q: What are Core Web Vitals?** 🟢
**A:** Google's metrics for UX: LCP (loading speed, < 2.5s), CLS (visual stability, < 0.1), INP (responsiveness, < 200ms). They affect SEO. Measured with Chrome DevTools Lighthouse, web-vitals library, or PageSpeed Insights.

**One-liner:** LCP = how fast main content loads (< 2.5s). CLS = how much page jumps (< 0.1). INP = interaction responsiveness (< 200ms). Affects SEO.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 15: Coding Challenges

> "Implement this from scratch" — the most common coding round questions.
> Full working code with comments explaining every step.

---

### 🔴 15.1 Implement Debounce

```javascript
function debounce(fn, delay) {
  let timeoutId;

  return function(...args) {
    // Clear previous timer (reset the wait)
    clearTimeout(timeoutId);

    // Start new timer
    timeoutId = setTimeout(() => {
      fn.apply(this, args);  // preserve context and arguments
    }, delay);
  };
}

// Usage
const log = debounce((msg) => console.log(msg), 300);
log("a");  // timer starts
log("ab"); // timer resets
log("abc"); // timer resets
// 300ms later... logs "abc" (only once)

// WITH IMMEDIATE (leading edge) — fires on first call, then waits
function debounceImmediate(fn, delay, immediate = false) {
  let timeoutId;
  return function(...args) {
    const callNow = immediate && !timeoutId;
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
      timeoutId = null;
      if (!immediate) fn.apply(this, args);
    }, delay);
    if (callNow) fn.apply(this, args);
  };
}
```

**What they evaluate:** setTimeout/clearTimeout, closures (timeoutId persists), `this` context preservation with apply, rest arguments.

---

### 🔴 15.2 Implement Throttle

```javascript
function throttle(fn, limit) {
  let inThrottle = false;

  return function(...args) {
    if (!inThrottle) {
      fn.apply(this, args);     // execute immediately
      inThrottle = true;         // lock
      setTimeout(() => {
        inThrottle = false;      // unlock after limit
      }, limit);
    }
  };
}

// WITH TRAILING CALL — fires last call after throttle period
function throttleWithTrailing(fn, limit) {
  let lastArgs = null;
  let inThrottle = false;

  return function(...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;
      setTimeout(() => {
        inThrottle = false;
        if (lastArgs) {
          fn.apply(this, lastArgs);  // fire the last skipped call
          lastArgs = null;
        }
      }, limit);
    } else {
      lastArgs = args;  // save latest args for trailing call
    }
  };
}
```

**What they evaluate:** Boolean flag pattern, closure over `inThrottle`, understanding of leading vs trailing execution.

---

### 🔴 15.3 Implement Promise.all

```javascript
function promiseAll(promises) {
  return new Promise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      return reject(new TypeError('Argument must be an array'));
    }

    const results = [];
    let completed = 0;

    if (promises.length === 0) {
      return resolve([]);
    }

    promises.forEach((promise, index) => {
      // Wrap in Promise.resolve in case it's not a promise
      Promise.resolve(promise)
        .then(value => {
          results[index] = value;  // maintain order!
          completed++;

          if (completed === promises.length) {
            resolve(results);  // all done
          }
        })
        .catch(reject);  // reject immediately on first failure
    });
  });
}

// Test
promiseAll([
  Promise.resolve(1),
  Promise.resolve(2),
  Promise.resolve(3),
]).then(console.log);  // [1, 2, 3]

promiseAll([
  Promise.resolve(1),
  Promise.reject('Error'),
  Promise.resolve(3),
]).catch(console.log);  // "Error"
```

**What they evaluate:** Promise constructor, maintaining result ORDER (use index, not push), handling non-promise values, short-circuit rejection, edge case (empty array).

---

### 🔴 15.4 Implement Array.prototype.map

```javascript
Array.prototype.myMap = function(callback, thisArg) {
  if (typeof callback !== 'function') {
    throw new TypeError(callback + ' is not a function');
  }

  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (i in this) {  // skip holes in sparse arrays
      result.push(callback.call(thisArg, this[i], i, this));
    }
  }
  return result;
};

// Test
[1, 2, 3].myMap(n => n * 2);           // [2, 4, 6]
[1, 2, 3].myMap((n, i) => n + i);      // [1, 3, 5]
```

---

### 🔴 15.5 Implement Array.prototype.filter

```javascript
Array.prototype.myFilter = function(callback, thisArg) {
  if (typeof callback !== 'function') {
    throw new TypeError(callback + ' is not a function');
  }

  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (i in this && callback.call(thisArg, this[i], i, this)) {
      result.push(this[i]);
    }
  }
  return result;
};

// Test
[1, 2, 3, 4].myFilter(n => n % 2 === 0);  // [2, 4]
```

---

### 🔴 15.6 Implement Array.prototype.reduce

```javascript
Array.prototype.myReduce = function(callback, initialValue) {
  if (typeof callback !== 'function') {
    throw new TypeError(callback + ' is not a function');
  }

  let acc = initialValue;
  let startIndex = 0;

  if (acc === undefined) {
    if (this.length === 0) {
      throw new TypeError('Reduce of empty array with no initial value');
    }
    acc = this[0];
    startIndex = 1;
  }

  for (let i = startIndex; i < this.length; i++) {
    if (i in this) {
      acc = callback(acc, this[i], i, this);
    }
  }

  return acc;
};

// Test
[1, 2, 3].myReduce((acc, n) => acc + n, 0);   // 6
[1, 2, 3].myReduce((acc, n) => acc + n);       // 6 (no initial: first element is acc)
```

---

### 🔴 15.7 Implement Function.prototype.bind

```javascript
Function.prototype.myBind = function(context, ...boundArgs) {
  const fn = this;  // the original function

  return function(...callArgs) {
    return fn.apply(context, [...boundArgs, ...callArgs]);
  };
};

// Test
function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}

const boundGreet = greet.myBind({ name: 'Alice' }, 'Hello');
boundGreet('!');   // "Hello, Alice!"
boundGreet('?');   // "Hello, Alice?"
```

**What they evaluate:** Understanding of `this`, closure, apply, partial application (pre-filled args + later args).

---

### 🔴 15.8 Implement Deep Clone

```javascript
function deepClone(obj) {
  // Handle primitives and null
  if (obj === null || typeof obj !== 'object') {
    return obj;
  }

  // Handle Date
  if (obj instanceof Date) {
    return new Date(obj.getTime());
  }

  // Handle RegExp
  if (obj instanceof RegExp) {
    return new RegExp(obj.source, obj.flags);
  }

  // Handle Array
  if (Array.isArray(obj)) {
    return obj.map(item => deepClone(item));
  }

  // Handle Map
  if (obj instanceof Map) {
    const map = new Map();
    obj.forEach((val, key) => map.set(deepClone(key), deepClone(val)));
    return map;
  }

  // Handle Set
  if (obj instanceof Set) {
    const set = new Set();
    obj.forEach(val => set.add(deepClone(val)));
    return set;
  }

  // Handle Object
  const cloned = {};
  for (const key of Object.keys(obj)) {
    cloned[key] = deepClone(obj[key]);  // recursive
  }
  return cloned;
}

// Test
const original = {
  name: 'Alice',
  scores: [1, 2, 3],
  nested: { deep: { value: 42 } },
  date: new Date(),
};

const cloned = deepClone(original);
cloned.scores.push(4);
cloned.nested.deep.value = 99;
console.log(original.scores);            // [1, 2, 3] (unchanged)
console.log(original.nested.deep.value);  // 42 (unchanged)
```

**What they evaluate:** Recursion, type checking (instanceof), handling edge cases (null, Date, RegExp, Array, Map, Set), not mutating original.

---

### 🟡 15.9 Implement Curry Function

```javascript
function curry(fn) {
  return function curried(...args) {
    // If enough arguments, call the original function
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    }

    // Otherwise, return a function that collects more arguments
    return function(...moreArgs) {
      return curried.apply(this, [...args, ...moreArgs]);
    };
  };
}

// Test
function add(a, b, c) { return a + b + c; }

const curriedAdd = curry(add);
curriedAdd(1)(2)(3);     // 6
curriedAdd(1, 2)(3);     // 6
curriedAdd(1)(2, 3);     // 6
curriedAdd(1, 2, 3);     // 6
```

**What they evaluate:** fn.length (number of expected parameters), recursion, collecting arguments across calls, apply for context.

---

### 🟡 15.10 Implement Event Emitter (Pub/Sub)

```javascript
class EventEmitter {
  constructor() {
    this.events = {};
  }

  // Subscribe to an event
  on(event, listener) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(listener);
    return this;  // for chaining
  }

  // Subscribe once (auto-remove after first call)
  once(event, listener) {
    const wrapper = (...args) => {
      listener(...args);
      this.off(event, wrapper);
    };
    this.on(event, wrapper);
    return this;
  }

  // Unsubscribe
  off(event, listener) {
    if (this.events[event]) {
      this.events[event] = this.events[event].filter(l => l !== listener);
    }
    return this;
  }

  // Emit event (call all listeners)
  emit(event, ...args) {
    if (this.events[event]) {
      this.events[event].forEach(listener => listener(...args));
    }
    return this;
  }
}

// Test
const emitter = new EventEmitter();

const handler = (name) => console.log(`Hello, ${name}!`);
emitter.on('greet', handler);
emitter.emit('greet', 'Alice');  // "Hello, Alice!"
emitter.off('greet', handler);
emitter.emit('greet', 'Bob');    // (nothing — unsubscribed)

emitter.once('login', (user) => console.log(`${user} logged in`));
emitter.emit('login', 'Alice');  // "Alice logged in"
emitter.emit('login', 'Bob');    // (nothing — once removes itself)
```

**What they evaluate:** Object for event storage, array of listeners, on/off/emit/once pattern, removing correct listener.

---

### 🟡 15.11 Implement Flatten (Nested Arrays)

```javascript
// Recursive approach
function flatten(arr, depth = Infinity) {
  const result = [];

  for (const item of arr) {
    if (Array.isArray(item) && depth > 0) {
      result.push(...flatten(item, depth - 1));  // recurse with reduced depth
    } else {
      result.push(item);
    }
  }

  return result;
}

// Test
flatten([1, [2, [3, [4]]]]);          // [1, 2, 3, 4] (all levels)
flatten([1, [2, [3, [4]]]], 1);       // [1, 2, [3, [4]]] (one level)
flatten([1, [2, [3, [4]]]], 2);       // [1, 2, 3, [4]] (two levels)

// Iterative approach (using stack)
function flattenIterative(arr) {
  const stack = [...arr];
  const result = [];

  while (stack.length) {
    const item = stack.pop();
    if (Array.isArray(item)) {
      stack.push(...item);   // spread nested items back onto stack
    } else {
      result.unshift(item);  // add to front (maintains order)
    }
  }

  return result;
}
```

---

### 🟡 15.12 Implement setInterval Using setTimeout

```javascript
function mySetInterval(fn, delay) {
  let cancelled = false;

  function repeat() {
    if (!cancelled) {
      fn();
      setTimeout(repeat, delay);  // schedule next AFTER completion
    }
  }

  setTimeout(repeat, delay);

  // Return cancel function
  return {
    clear() { cancelled = true; }
  };
}

// Test
const timer = mySetInterval(() => console.log('tick'), 1000);
// logs "tick" every ~1 second

setTimeout(() => timer.clear(), 5000);  // stop after 5 seconds

// WHY THIS IS BETTER THAN REAL setInterval:
// setInterval can queue up callbacks if fn takes longer than delay
// This approach waits for fn to finish, THEN schedules next call
```

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 16: Tricky Output Questions

> "What does this code print?" — tests deep understanding of JS internals.
> For each: code → your prediction → step-by-step explanation.

---

### 🔴 16.1 Hoisting Trick

```javascript
console.log(a);
console.log(b);
var a = 1;
let b = 2;
```

**Output:**
```
undefined
ReferenceError: Cannot access 'b' before initialization
```

**Why:** `var a` is hoisted and initialized as `undefined`. `let b` is hoisted but in the TDZ — accessing it before the declaration throws ReferenceError. The code stops at the error (line 2 never executes).

---

### 🔴 16.2 var in Loop

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

**Output:** `3, 3, 3`

**Why:** `var i` is function-scoped — one shared `i` for all iterations. All three `setTimeout` callbacks reference the same `i`. By the time they fire (after the sync loop finishes), `i` is `3`.

---

### 🔴 16.3 Closure Value Capture

```javascript
function createFunctions() {
  const result = [];
  for (let i = 0; i < 3; i++) {
    result.push(() => i);
  }
  return result;
}

const fns = createFunctions();
console.log(fns[0]());
console.log(fns[1]());
console.log(fns[2]());
```

**Output:** `0, 1, 2`

**Why:** `let` creates a new scope per iteration. Each closure captures its own `i`. If this were `var`, all would print `3`.

---

### 🔴 16.4 == Coercion Surprise

```javascript
console.log([] == false);
console.log([] == ![]);
console.log('' == false);
console.log('0' == false);
console.log(0 == '');
```

**Output:** `true, true, true, true, true`

**Why step by step:**
- `[] == false` → `[] → "" → 0`, `false → 0` → `0 == 0` → true
- `[] == ![]` → `![]` is `false` → `[] == false` → same as above → true
- `'' == false` → `'' → 0`, `false → 0` → `0 == 0` → true
- `'0' == false` → `'0' → 0`, `false → 0` → `0 == 0` → true
- `0 == ''` → `'' → 0` → `0 == 0` → true

---

### 🔴 16.5 setTimeout(fn, 0) Order

```javascript
console.log('start');
setTimeout(() => console.log('timeout'), 0);
Promise.resolve().then(() => console.log('promise'));
console.log('end');
```

**Output:** `start, end, promise, timeout`

**Why:** Sync first (`start`, `end`). Microtask next (`promise` — Promise.then). Macrotask last (`timeout` — setTimeout). Even with 0ms delay, setTimeout is a macrotask and waits.

---

### 🟡 16.6 Promise Chain Order

```javascript
Promise.resolve()
  .then(() => console.log(1))
  .then(() => console.log(2));

Promise.resolve()
  .then(() => console.log(3))
  .then(() => console.log(4));
```

**Output:** `1, 3, 2, 4`

**Why:** First `.then` of each chain runs (1 and 3 — both are microtasks queued). Then second `.then` of each chain runs (2 and 4 — queued after first `.then` completes). Microtasks interleave: 1 → 3 → 2 → 4.

---

### 🟡 16.7 this in Object Method

```javascript
const obj = {
  name: 'Alice',
  greet: function() { console.log(this.name); },
  greetArrow: () => { console.log(this.name); },
};

obj.greet();
obj.greetArrow();

const fn = obj.greet;
fn();
```

**Output:**
```
Alice
undefined
undefined
```

**Why:**
- `obj.greet()` — regular function called as method → `this = obj` → "Alice"
- `obj.greetArrow()` — arrow function → `this` inherited from outer scope (module/window) → `this.name` is `undefined`
- `fn()` — extracted method, called as regular function → `this = window` (or `undefined` in strict) → `undefined`

---

### 🟡 16.8 Complex Event Loop

```javascript
console.log('A');

setTimeout(() => console.log('B'), 0);

Promise.resolve().then(() => {
  console.log('C');
  setTimeout(() => console.log('D'), 0);
});

Promise.resolve().then(() => console.log('E'));

console.log('F');
```

**Output:** `A, F, C, E, D, B`

Wait — actually: `A, F, C, E, B, D`

**Step by step:**
1. Sync: `A`, `F`
2. Microtasks: `C` (first Promise.then), `E` (second Promise.then)
3. During `C`, a new setTimeout `D` is queued to macrotask
4. Macrotasks in order: `B` (queued first), then `D` (queued during microtask)

---

### 🟡 16.9 typeof Tricks

```javascript
console.log(typeof undefined);
console.log(typeof null);
console.log(typeof NaN);
console.log(typeof typeof 42);
console.log(typeof (() => {}));
```

**Output:**
```
"undefined"
"object"       ← famous bug
"number"       ← NaN is a number!
"string"       ← typeof 42 = "number" (string), typeof "number" = "string"
"function"
```

---

### 🟡 16.10 NaN Comparisons

```javascript
console.log(NaN === NaN);
console.log(NaN == NaN);
console.log(isNaN('hello'));
console.log(Number.isNaN('hello'));
console.log(Object.is(NaN, NaN));
```

**Output:**
```
false            ← NaN is never equal to itself
false            ← even with loose equality
true             ← isNaN coerces "hello" → NaN, then checks
false            ← Number.isNaN doesn't coerce (recommended)
true             ← Object.is treats NaN as equal to NaN
```

---

### 🔴 16.11 this Inside setTimeout

```javascript
const user = {
  name: 'Alice',
  greet() {
    console.log(`Hi, ${this.name}`);

    setTimeout(function() {
      console.log(`Delayed: ${this.name}`);
    }, 100);

    setTimeout(() => {
      console.log(`Arrow: ${this.name}`);
    }, 100);
  }
};

user.greet();
```

**Output:**
```
Hi, Alice
Delayed: undefined     ← regular function in setTimeout: this = window
Arrow: Alice           ← arrow function inherits this from greet()
```

---

### 🔴 16.12 Implicit Type Conversion

```javascript
console.log(1 + '2' + 3);
console.log(1 + 2 + '3');
console.log('3' - 1);
console.log('3' + 1);
console.log(true + true);
console.log([] + {});
console.log(+[]);
console.log(+!![]);
```

**Output:**
```
"123"               ← 1+"2"="12", "12"+3="123" (concat)
"33"                ← 1+2=3, 3+"3"="33" (math then concat)
2                   ← "3"-1=2 (- forces number)
"31"                ← "3"+1="31" (+ concatenates with string)
2                   ← true=1, 1+1=2
"[object Object]"   ← [].toString()="" + {}.toString()="[object Object]"
0                   ← +[] → +""  → 0
1                   ← !![] = true, +true = 1
```

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Quick Revision — Part 3 One-Liners

```
STORAGE & BROWSER APIs
  localStorage = forever, 5-10MB. sessionStorage = tab only. Cookies = 4KB, sent to server
  Cookies: httpOnly (no JS access), Secure (HTTPS), SameSite (CSRF protection)
  JSON.stringify loses: functions, undefined, Symbol, Date accuracy, BigInt (throws)
  IntersectionObserver = detect viewport entry. For lazy load, infinite scroll
  Web Workers = background thread, no DOM. Communicate via postMessage
  WebSocket = persistent two-way connection. For chat, gaming, live data

PERFORMANCE
  Debounce = fire after pause. Throttle = fire at fixed rate
  Lazy loading: loading="lazy", IntersectionObserver, dynamic import(), React.lazy
  Memoization = cache results of pure functions. Same input → cached result
  Reflow (layout) is expensive. Repaint (visual) is cheaper. Batch DOM changes
  Web Vitals: LCP < 2.5s (loading), CLS < 0.1 (stability), INP < 200ms (responsiveness)

CODING CHALLENGES
  Debounce: clearTimeout + setTimeout. Closure holds timeoutId
  Throttle: boolean flag + setTimeout. Closure holds inThrottle
  Promise.all: counter + results[index]. Reject on first failure
  Polyfills: loop this, callback(el, i, arr). map→push result. filter→push if true
  bind: return function that calls fn.apply(context, args)
  Deep clone: recursion + instanceof checks for Date/RegExp/Array/Map/Set
  Curry: if args.length >= fn.length → call, else → return collecting function
  Event emitter: on/off/emit/once using object of event → listener arrays
  Flatten: recursion with depth parameter. Base: not array or depth 0

TRICKY OUTPUT
  var loop + setTimeout = all same value (shared var). let = each iteration unique
  Sync → microtasks (Promise.then) → macrotasks (setTimeout)
  typeof null = "object". typeof NaN = "number". NaN !== NaN
  Arrow functions inherit this. Regular functions get their own this
  + with string = concat. - with string = number. Left to right evaluation
  [] == false → true. [] == ![] → true. The most cursed JS behavior
```

[↑ Back to Table of Contents](#table-of-contents--part-3)
