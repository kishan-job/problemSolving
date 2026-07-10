# JavaScript Methods Reference

Two worlds: **Core JavaScript (ECMAScript)** — works anywhere (browser, Node.js, Deno) — and **DOM / Web API** — requires a browser (or browser-like environment). Each method is marked **[S]** static (called on the class/constructor) or **[I]** instance (called on an object/value).

---

## 1. Array — Core JS

| Method | Type |
|---|---|
| `push()`, `pop()`, `shift()`, `unshift()`, `splice()` | I |
| `indexOf()`, `lastIndexOf()`, `includes()` | I |
| `find()`, `findIndex()`, `findLast()`, `findLastIndex()` | I |
| `forEach()`, `map()`, `filter()`, `reduce()`, `reduceRight()`, `some()`, `every()` | I |
| `slice()`, `concat()`, `join()`, `reverse()`, `sort()`, `flat()`, `flatMap()`, `fill()`, `copyWithin()`, `at()` | I |
| `toSorted()`, `toReversed()`, `toSpliced()`, `with()` (ES2023+) | I |
| `toString()` | I |
| `Array.from()`, `Array.of()`, `Array.isArray()` | S |

## 2. String — Core JS

| Method | Type |
|---|---|
| `indexOf()`, `lastIndexOf()`, `includes()`, `startsWith()`, `endsWith()` | I |
| `search()`, `match()`, `matchAll()` | I |
| `slice()`, `substring()`, `substr()`, `charAt()`, `charCodeAt()`, `codePointAt()`, `at()` | I |
| `toUpperCase()`, `toLowerCase()`, `trim()`, `trimStart()`, `trimEnd()` | I |
| `replace()`, `replaceAll()`, `split()`, `concat()`, `padStart()`, `padEnd()`, `repeat()`, `normalize()` | I |
| `toString()`, `valueOf()` | I |
| `String.fromCharCode()`, `String.raw()` | S |

## 3. Object — Core JS

| Method | Type |
|---|---|
| `Object.keys()`, `Object.values()`, `Object.entries()` | S |
| `Object.assign()`, `Object.freeze()`, `Object.isFrozen()`, `Object.seal()`, `Object.isSealed()` | S |
| `Object.create()`, `Object.getPrototypeOf()`, `Object.setPrototypeOf()` | S |
| `Object.defineProperty()`, `Object.defineProperties()` | S |
| `Object.getOwnPropertyNames()`, `Object.getOwnPropertyDescriptor()` | S |
| `Object.fromEntries()`, `Object.is()`, `Object.groupBy()` | S |
| `hasOwnProperty()`, `toString()`, `valueOf()`, `isPrototypeOf()`, `propertyIsEnumerable()` | I |

## 4. Number — Core JS

| Method | Type |
|---|---|
| `toFixed()`, `toPrecision()`, `toString()`, `toLocaleString()`, `valueOf()` | I |
| `Number.isInteger()`, `Number.isFinite()`, `Number.isNaN()`, `Number.isSafeInteger()` | S |
| `Number.parseFloat()`, `Number.parseInt()` | S |

## 5. Math — Core JS (all static)

| Method | Type |
|---|---|
| `Math.round()`, `Math.floor()`, `Math.ceil()`, `Math.trunc()`, `Math.abs()` | S |
| `Math.max()`, `Math.min()`, `Math.pow()`, `Math.sqrt()`, `Math.cbrt()` | S |
| `Math.random()`, `Math.log()`, `Math.log2()`, `Math.log10()`, `Math.exp()`, `Math.sign()` | S |
| `Math.sin()`, `Math.cos()`, `Math.tan()` (+ other trig) | S |

## 6. Function — Core JS

| Method | Type |
|---|---|
| `call()`, `apply()`, `bind()`, `toString()` | I |

## 7. JSON — Core JS (all static)

| Method | Type |
|---|---|
| `JSON.stringify()`, `JSON.parse()` | S |

## 8. Date — Core JS

| Method | Type |
|---|---|
| `getFullYear()`, `getMonth()`, `getDate()`, `getDay()`, `getHours()`, `getMinutes()`, `getSeconds()`, `getMilliseconds()`, `getTime()`, `getTimezoneOffset()` | I |
| `setFullYear()`, `setMonth()`, `setDate()`, `setHours()`, `setMinutes()`, `setSeconds()`, `setTime()` | I |
| `toISOString()`, `toLocaleDateString()`, `toLocaleTimeString()`, `toLocaleString()`, `toDateString()`, `toString()` | I |
| `Date.now()`, `Date.parse()`, `Date.UTC()` | S |

## 9. Map / Set / WeakMap / WeakSet — Core JS

| Method | Type |
|---|---|
| Map: `set()`, `get()`, `has()`, `delete()`, `clear()`, `keys()`, `values()`, `entries()`, `forEach()` | I |
| Set: `add()`, `has()`, `delete()`, `clear()`, `values()`, `forEach()` | I |
| WeakMap: `set()`, `get()`, `has()`, `delete()` | I |
| WeakSet: `add()`, `has()`, `delete()` | I |

## 10. Promise — Core JS

| Method | Type |
|---|---|
| `then()`, `catch()`, `finally()` | I |
| `Promise.all()`, `Promise.allSettled()`, `Promise.race()`, `Promise.any()`, `Promise.resolve()`, `Promise.reject()` | S |

## 11. RegExp — Core JS

| Method | Type |
|---|---|
| `test()`, `exec()`, `toString()` | I |

## 12. Symbol — Core JS

| Method | Type |
|---|---|
| `toString()` | I |
| `Symbol.for()`, `Symbol.keyFor()` | S |
| Well-known: `Symbol.iterator`, `Symbol.asyncIterator`, `Symbol.hasInstance` | S (properties) |

## 13. Reflect — Core JS (all static)

| Method | Type |
|---|---|
| `Reflect.get()`, `Reflect.set()`, `Reflect.has()`, `Reflect.deleteProperty()` | S |
| `Reflect.ownKeys()`, `Reflect.apply()`, `Reflect.construct()` | S |
| `Reflect.defineProperty()`, `Reflect.getPrototypeOf()`, `Reflect.setPrototypeOf()` | S |
| `Reflect.isExtensible()`, `Reflect.preventExtensions()` | S |

## 14. Proxy — Core JS (handler traps, instance-level)

| Trap | Type |
|---|---|
| `get`, `set`, `has`, `deleteProperty`, `apply`, `construct` | I |
| `ownKeys`, `getPrototypeOf`, `setPrototypeOf`, `defineProperty`, `getOwnPropertyDescriptor` | I |

## 15. Generators / Iterators — Core JS

| Method | Type |
|---|---|
| `next()`, `return()`, `throw()` | I |

## 16. Error — Core JS

| Method | Type |
|---|---|
| `toString()` | I |
| Properties: `message`, `name`, `stack`, `cause` | I |

## 17. TypedArray / ArrayBuffer / DataView — Core JS

| Method | Type |
|---|---|
| `ArrayBuffer.isView()` | S |
| `slice()`, `set()`, `subarray()` | I |
| `getInt8()`, `getUint8()`, `setInt8()`, `getFloat32()`, etc. | I |

## 18. BigInt — Core JS

| Method | Type |
|---|---|
| `BigInt.asIntN()`, `BigInt.asUintN()` | S |
| `toString()`, `valueOf()` | I |

## 19. Intl — Core JS

| Method | Type |
|---|---|
| `Intl.NumberFormat()`, `Intl.DateTimeFormat()`, `Intl.Collator()` | S (constructors) |
| `Intl.RelativeTimeFormat()`, `Intl.ListFormat()`, `Intl.PluralRules()` | S (constructors) |

## 20. Global Functions — Core JS

| Method | Type |
|---|---|
| `parseInt()`, `parseFloat()`, `isNaN()`, `isFinite()` | Global |
| `encodeURIComponent()`, `decodeURIComponent()` | Global |
| `setTimeout()`, `setInterval()`, `clearTimeout()`, `clearInterval()` | Global |
| `structuredClone()`, `queueMicrotask()` | Global |

---

## 21. DOM / Web API — Event System

### Static (constructors)
| Method | Type |
|---|---|
| `new Event()`, `new CustomEvent()`, `new EventTarget()` | S |

### Instance — on any `EventTarget` (elements, `window`, `document`)
| Method | Type |
|---|---|
| `addEventListener()`, `removeEventListener()`, `dispatchEvent()` | I |

### Instance — on the `Event` object inside a handler
| Method | Type |
|---|---|
| `preventDefault()`, `stopPropagation()`, `stopImmediatePropagation()`, `composedPath()` | I |

### Instance — on specific HTML elements
| Method | Element | Type |
|---|---|---|
| `focus()`, `blur()` | any focusable element | I |
| `submit()`, `reset()`, `requestSubmit()` | `<form>` | I |
| `checkValidity()`, `reportValidity()`, `setCustomValidity()` | form controls | I |
| `play()`, `pause()`, `load()` | `<audio>`, `<video>` | I |
| `setPointerCapture()`, `releasePointerCapture()`, `hasPointerCapture()` | any element | I |

### Instance — on `dataTransfer` / `clipboardData`
| Method | Type |
|---|---|
| `setData()`, `getData()`, `clearData()`, `setDragImage()` | I |

### Instance — `AbortController`
| Method | Type |
|---|---|
| `abort()` | I |

---

## Quick rule of thumb

- **Core JS** methods work in Node.js with zero browser involvement.
- **DOM/Web API** methods require `window`, `document`, or an HTML element — almost all **instance** methods; the only static pieces are the event constructors.
- **[S] static** = called on the class/constructor itself (e.g. `Array.from()`, `Object.keys()`, `Math.max()`).
- **[I] instance** = called on a specific value/object (e.g. `myArray.push()`, `myString.trim()`, `button.addEventListener()`).
