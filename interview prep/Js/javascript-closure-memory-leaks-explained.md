# Closures vs Memory Leaks

A closure is **not automatically a memory leak**.

A memory leak occurs only when memory is retained **after it is no longer needed**.

---

# Example 1: NOT a Memory Leak

```javascript
function outer() {
    const data = new Array(1000000).fill("hello");

    function inner() {
        console.log(data.length);
    }

    return inner;
}

let ren = outer();

ren();
```

### Memory

```text
ren
 ↓
inner
 ↓
data
```

After `ren()` executes:

```text
ren
 ↓
inner
 ↓
data
```

The closure is STILL alive.

Why?

Because `ren` still points to it.

You can still do:

```javascript
ren();
ren();
ren();
```

at any time.

Therefore JavaScript keeps `inner` and `data` in memory.

This is NOT a memory leak because the application is intentionally keeping the function for future use.

---

### Reassigning `ren`

```javascript
let ren = outer(); // inner1 -> data1

ren = outer();     // inner2 -> data2
```

After reassignment:

```text
ren
 ↓
inner2
 ↓
data2
```

The old closure:

```text
inner1
 ↓
data1
```

is no longer referenced.

Therefore it becomes eligible for Garbage Collection.

No memory leak occurs.

---

# Example 2: Memory Leak

```javascript
function createPage() {
    const data = new Array(1000000).fill("hello");

    function handleResize() {
        console.log(data.length);
    }

    window.addEventListener("resize", handleResize);
}
```

### First Page Load

```javascript
createPage();
```

Memory:

```text
window
 ↓
handleResize1
 ↓
data1
```

---

### User Reloads / Revisits Page

```javascript
createPage();
```

Memory:

```text
window
 ├── handleResize1 -> data1
 └── handleResize2 -> data2
```

---

### User Reloads Again

```javascript
createPage();
```

Memory:

```text
window
 ├── handleResize1 -> data1
 ├── handleResize2 -> data2
 └── handleResize3 -> data3
```

---

# What Is the Problem?

Each page load creates:

- A new `data`
- A new `handleResize`
- A new closure

Even though the old page/component is gone, the old listeners are still attached to `window`.

```text
window
 ├── handleResize1 -> data1
 ├── handleResize2 -> data2
 ├── handleResize3 -> data3
 └── ...
```

The application no longer needs:

```text
handleResize1
data1

handleResize2
data2
```

but they remain alive because `window` still references them.

---

# Why Garbage Collector Can't Clean Them

Garbage Collector only removes objects that are **unreachable**.

For example:

```text
window
 ↓
handleResize1
 ↓
data1
```

Since `window` is still referencing `handleResize1`:

```text
data1 is reachable
```

Therefore Garbage Collector cannot remove:

```text
handleResize1
data1
```

The same applies to all subsequent listeners.

---

# Why Memory Keeps Growing

Every page reload creates another closure:

```text
Load 1 -> Closure1 -> Data1
Load 2 -> Closure2 -> Data2
Load 3 -> Closure3 -> Data3
Load 4 -> Closure4 -> Data4
...
```

Result:

```text
window
 ├── Closure1 -> LargeData1
 ├── Closure2 -> LargeData2
 ├── Closure3 -> LargeData3
 ├── Closure4 -> LargeData4
 └── ...
```

None of the old closures are collected because they are still referenced by `window`.

As more pages are loaded:

```text
Memory Usage
↑
|
|        /
|      /
|    /
|  /
|/
+-----------------> Time
```

Memory continuously grows.

This is the actual memory leak.

---

# Fix

```javascript
function createPage() {
    const data = new Array(1000000).fill("hello");

    function handleResize() {
        console.log(data.length);
    }

    window.addEventListener("resize", handleResize);

    return () => {
        window.removeEventListener("resize", handleResize);
    };
}
```

When the page/component is destroyed:

```javascript
const cleanup = createPage();

cleanup();
```

Now:

```text
window
  X handleResize removed
```

No references remain:

```text
handleResize
 ↓
data
```

Both become unreachable and can be garbage collected.

---

# The REAL Difference

## `ren` Example

```text
Object is referenced
AND
Application still needs it
```

✅ Not a Memory Leak

---

## `window` Example

```text
Object is referenced
AND
Application no longer needs it
```

❌ Memory Leak

---

# Interview One-Liner

> A closure itself is not a memory leak. A memory leak occurs when multiple closures are created over time and remain referenced by long-lived objects (such as event listeners, timers, caches, global objects, or DOM references) even though the application no longer needs them. Because those closures remain reachable, the garbage collector cannot free the captured data, causing memory usage to keep growing.
