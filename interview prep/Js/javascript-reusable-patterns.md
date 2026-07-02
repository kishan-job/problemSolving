# JavaScript Reusable Patterns

## Check Object (excluding null)

```javascript
if (value !== null && typeof value === "object") {
}
```

---

## Check Plain Object

```javascript
if (
  Object.prototype.toString.call(value) === "[object Object]"
) {
}
```

---

## Check Array

```javascript
if (Array.isArray(value)) {
}
```

---

## Check Empty Array

```javascript
if (Array.isArray(value) && value.length === 0) {
}
```

---

## Check Empty Object

```javascript
if (
  Object.prototype.toString.call(value) === "[object Object]" &&
  Object.keys(value).length === 0
) {
}
```

---

## Check Date

```javascript
if (value instanceof Date) {
}
```

---

## Check RegExp

```javascript
if (value instanceof RegExp) {
}
```

---

## Check Null or Undefined

```javascript
if (value == null) {
}
```

Equivalent:

```javascript
if (value === null || value === undefined) {
}
```

---

## Check NaN

```javascript
Number.isNaN(value);
```

---

## Check Function

```javascript
typeof value === "function";
```

---

## Check Truthy

```javascript
if (value) {
}
```

---

## Check Falsy

```javascript
if (!value) {
}
```

---

## Optional Chaining

```javascript
user?.address?.city;
```

Avoids:

```javascript
Cannot read properties of undefined
```

---

## Nullish Coalescing

```javascript
const name = value ?? "Default";
```

Uses default only for:

```javascript
null
undefined
```

---

## Remove Duplicates

```javascript
const unique = [...new Set(arr)];
```

---

## Deep Clone

```javascript
const clone = structuredClone(obj);
```

Alternative:

```javascript
const clone = JSON.parse(JSON.stringify(obj));
```

Limitation:

- Loses Date
- Loses Functions
- Loses undefined

---

## Debounce

```javascript
function debounce(fn, delay) {
  let timer;

  return function (...args) {
    clearTimeout(timer);

    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}
```

Use Cases:

- Search box
- API calls
- Input handlers

---

## Throttle

```javascript
function throttle(fn, delay) {
  let lastCall = 0;

  return function (...args) {
    const now = Date.now();

    if (now - lastCall >= delay) {
      lastCall = now;
      fn.apply(this, args);
    }
  };
}
```

Use Cases:

- Scroll events
- Resize events
- Mouse move events

---

## Safe Object Property Check

```javascript
Object.hasOwn(obj, "key");
```

Alternative:

```javascript
Object.prototype.hasOwnProperty.call(obj, "key");
```

---

## Promise.all

```javascript
const result = await Promise.all([
  promise1,
  promise2,
  promise3
]);
```

Runs concurrently.

Fails if any promise rejects.

---

## Promise.allSettled

```javascript
const result = await Promise.allSettled([
  promise1,
  promise2
]);
```

Returns all results even if some fail.

---

## Destructuring Default Value

```javascript
const { name = "Guest" } = user;
```

---

## Swap Variables

```javascript
[a, b] = [b, a];
```

---

## Convert String to Number

```javascript
Number(value);
```

or

```javascript
+value;
```

---

## Convert Number to String

```javascript
String(value);
```

---

## Accurate Type Detection

```javascript
Object.prototype.toString.call(value);
```

Examples:

```javascript
"[object Object]"
"[object Array]"
"[object Date]"
"[object RegExp]"
"[object Null]"
"[object Undefined]"
```

---

## Equality Notes

```javascript
null == undefined; // true

null === undefined; // false
```

---

## typeof null

```javascript
typeof null;
```

Result:

```javascript
"object"
```

(Historical JavaScript bug)
