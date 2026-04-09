# Category 1: Variables & Data Types — Question Bank
## 40 Interview Questions with Answers
## Organized by Sub-Topic · Tagged by Difficulty

---

# Table of Contents

- [1.1 var vs let vs const (8 Questions)](#11-var-vs-let-vs-const)
- [1.2 Primitive Types (5 Questions)](#12-primitive-types)
- [1.3 Reference Types (5 Questions)](#13-reference-types)
- [1.4 typeof Operator (4 Questions)](#14-typeof-operator)
- [1.5 == vs === (5 Questions)](#15--vs-)
- [1.6 Type Coercion (5 Questions)](#16-type-coercion)
- [1.7 Symbol (2 Questions)](#17-symbol)
- [1.8 BigInt (2 Questions)](#18-bigint)
- [1.9 Pass by Value vs Reference (2 Questions)](#19-pass-by-value-vs-reference)
- [1.10 Immutability of Primitives (2 Questions)](#110-immutability-of-primitives)

---

```
Difficulty Tags:
  🟢 Easy    — Freshers / Junior (0-1 years)
  🟡 Medium  — Mid-level (2-4 years)
  🔴 Hard    — Senior (5+ years) / Tricky
```

---

# 1.1 var vs let vs const

---

### Q1 🟢 What are the differences between var, let, and const?

**Answer:**

| | var | let | const |
|---|---|---|---|
| Scope | Function | Block | Block |
| Hoisting | Yes, initialized as `undefined` | Yes, but in Temporal Dead Zone | Yes, but in Temporal Dead Zone |
| Reassignment | ✅ Allowed | ✅ Allowed | ❌ Not allowed |
| Redeclaration | ✅ Allowed | ❌ Not allowed | ❌ Not allowed |

---

### Q2 🟢 What is the output?

```javascript
console.log(a);
var a = 10;
```

**Answer:** `undefined`

`var` is hoisted to the top and initialized as `undefined`. So it becomes:

```javascript
var a;           // hoisted
console.log(a);  // undefined
a = 10;
```

---

### Q3 🟢 What is the output?

```javascript
console.log(b);
let b = 10;
```

**Answer:** `ReferenceError: Cannot access 'b' before initialization`

`let` is hoisted but stays in the Temporal Dead Zone (TDZ) until the declaration line. Accessing it before that throws an error.

---

### Q4 🟡 What is the output?

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
```

**Answer:** `3, 3, 3`

`var` is function-scoped, so there's only ONE `i` shared across all iterations. By the time `setTimeout` fires, the loop has finished and `i` is 3.

**Follow-up: How to fix it?**

Use `let` instead — each iteration gets its own block-scoped `i`:

```javascript
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 0, 1, 2
```

---

### Q5 🟡 What is the output?

```javascript
const arr = [1, 2, 3];
arr.push(4);
console.log(arr);
```

**Answer:** `[1, 2, 3, 4]`

`const` prevents reassignment of the variable, not mutation of the value. The array itself can be modified (push, pop, splice, etc.). Only `arr = [...]` would fail.

---

### Q6 🟡 What is the output?

```javascript
const obj = { a: 1 };
obj.b = 2;
obj.a = 100;
console.log(obj);
```

**Answer:** `{ a: 100, b: 2 }`

Same reason as Q5. `const` doesn't make the object immutable. It only prevents `obj = somethingElse`.

**Follow-up: How to make an object truly immutable?**

`Object.freeze(obj)` — prevents adding, removing, or changing properties (shallow freeze only).

---

### Q7 🔴 What is the output?

```javascript
var x = 1;
function foo() {
  console.log(x);
  var x = 2;
  console.log(x);
}
foo();
```

**Answer:** `undefined`, then `2`

The local `var x` is hoisted inside `foo()`. So `foo` becomes:

```javascript
function foo() {
  var x;            // hoisted (local x shadows global x)
  console.log(x);   // undefined (local x, not yet assigned)
  x = 2;
  console.log(x);   // 2
}
```

The local `x` shadows the global `x = 1`. The first `console.log` sees the hoisted local `x` which is `undefined`.

---

### Q8 🔴 What is the output?

```javascript
let a = 1;
{
  console.log(a);
  let a = 2;
}
```

**Answer:** `ReferenceError: Cannot access 'a' before initialization`

Even though there's an outer `a = 1`, the inner block has its own `let a`. This inner `a` enters the TDZ from the start of the block until the `let a = 2` line. Accessing it before that line causes a ReferenceError.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.2 Primitive Types

---

### Q9 🟢 How many primitive types are there in JavaScript? Name them.

**Answer:** 7 primitive types:

1. `string` — `"hello"`
2. `number` — `42`, `3.14`, `NaN`, `Infinity`
3. `boolean` — `true`, `false`
4. `null` — intentional absence of value
5. `undefined` — not yet assigned
6. `symbol` — unique identifier
7. `bigint` — large integers (`100n`)

---

### Q10 🟢 What is the difference between null and undefined?

**Answer:**

| | null | undefined |
|---|---|---|
| Meaning | Developer says "no value" | JS says "not assigned yet" |
| typeof | `"object"` (bug) | `"undefined"` |
| Set by | You (intentionally) | JavaScript (automatically) |
| == comparison | `null == undefined` is `true` | `null == undefined` is `true` |
| === comparison | `null === undefined` is `false` | Different types |

Rule of thumb: Use `null` when YOU want to say "empty." Never set a variable to `undefined` manually.

---

### Q11 🟡 What is the output?

```javascript
console.log(typeof NaN);
console.log(NaN === NaN);
console.log(NaN == NaN);
console.log(isNaN("hello"));
console.log(Number.isNaN("hello"));
```

**Answer:**

```
"number"    — NaN is technically of type number
false       — NaN is NOT equal to itself (unique in JS)
false       — even with loose equality
true        — isNaN() coerces "hello" to NaN first, then checks
false       — Number.isNaN() doesn't coerce, "hello" is not NaN
```

`Number.isNaN()` is the stricter, recommended version.

---

### Q12 🟡 What is the output?

```javascript
console.log(0.1 + 0.2 === 0.3);
console.log(0.1 + 0.2);
```

**Answer:**

```
false
0.30000000000000004
```

JavaScript uses 64-bit floating point (IEEE 754). Some decimal numbers can't be represented exactly in binary, causing tiny precision errors. To compare: `Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON`.

---

### Q13 🔴 What is the output?

```javascript
console.log(null + 1);
console.log(undefined + 1);
console.log("" + 1);
console.log(true + true);
console.log(false + null);
```

**Answer:**

```
1         — null converts to 0, so 0 + 1 = 1
NaN       — undefined converts to NaN, so NaN + 1 = NaN
"1"       — "" is a string, + concatenates → "1"
2         — true converts to 1, so 1 + 1 = 2
0         — false is 0, null is 0, so 0 + 0 = 0
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.3 Reference Types

---

### Q14 🟢 What is the difference between primitive and reference types?

**Answer:**

| | Primitive | Reference |
|---|---|---|
| Types | string, number, boolean, null, undefined, symbol, bigint | object, array, function |
| Stored as | Value directly in variable | Pointer to location in memory |
| Copy behavior | Creates independent copy | Copies the pointer (both point to same data) |
| Comparison | By value (`5 === 5` → true) | By reference (`{} === {}` → false) |
| Mutability | Immutable | Mutable |

---

### Q15 🟡 What is the output?

```javascript
let a = { x: 1 };
let b = a;
b.x = 2;
console.log(a.x);
```

**Answer:** `2`

`b = a` copies the reference, not the object. Both `a` and `b` point to the same object in memory. Changing `b.x` changes `a.x` too.

---

### Q16 🟡 What is the output?

```javascript
let a = [1, 2, 3];
let b = [1, 2, 3];
console.log(a === b);
console.log(a == b);
```

**Answer:** `false` and `false`

Even though the contents are identical, `a` and `b` are two separate arrays in memory. Reference types are compared by their memory address, not their content.

---

### Q17 🟡 What are 3 ways to shallow copy an object?

**Answer:**

```javascript
const original = { a: 1, b: 2 };

// Way 1: Spread operator
const copy1 = { ...original };

// Way 2: Object.assign
const copy2 = Object.assign({}, original);

// Way 3: Manual loop
const copy3 = {};
for (const key in original) {
  copy3[key] = original[key];
}
```

All three create a shallow copy — nested objects are still shared.

---

### Q18 🔴 What is the output?

```javascript
function modify(obj) {
  obj.a = 100;
  obj = { a: 999 };
  obj.b = 200;
}

let myObj = { a: 1 };
modify(myObj);
console.log(myObj);
```

**Answer:** `{ a: 100 }`

Step by step:
1. `obj.a = 100` — mutates the original object. `myObj` is now `{ a: 100 }`.
2. `obj = { a: 999 }` — reassigns the local `obj` to a NEW object. `myObj` is no longer affected.
3. `obj.b = 200` — adds to the new local object, not `myObj`.

So `myObj` only got the first mutation: `{ a: 100 }`.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.4 typeof Operator

---

### Q19 🟢 What is `typeof null`?

**Answer:** `"object"`

This is a famous bug from JavaScript's first version (1995). Internally, null had the same type tag as objects. It was never fixed because changing it would break millions of websites.

To properly check for null: `value === null`.

---

### Q20 🟢 How do you check if a value is an array?

**Answer:** `Array.isArray(value)`

You can't use `typeof` because `typeof [] === "object"` — it doesn't distinguish arrays from objects.

```javascript
Array.isArray([1, 2, 3]);  // true
Array.isArray({});          // false
Array.isArray("hello");     // false
```

---

### Q21 🟡 What is the output?

```javascript
console.log(typeof undefined);
console.log(typeof null);
console.log(typeof NaN);
console.log(typeof []);
console.log(typeof {});
console.log(typeof function(){});
console.log(typeof typeof 42);
```

**Answer:**

```
"undefined"   — correct
"object"      — bug (null is not an object)
"number"      — NaN is type number
"object"      — arrays are objects in JS
"object"      — correct
"function"    — special case (functions ARE objects but get their own type)
"string"      — typeof 42 returns "number" (a string), typeof "number" returns "string"
```

---

### Q22 🔴 How can you reliably get the exact type of any value?

**Answer:** `Object.prototype.toString.call(value)`

```javascript
Object.prototype.toString.call(null);        // "[object Null]"
Object.prototype.toString.call(undefined);   // "[object Undefined]"
Object.prototype.toString.call([]);          // "[object Array]"
Object.prototype.toString.call({});          // "[object Object]"
Object.prototype.toString.call(42);          // "[object Number]"
Object.prototype.toString.call("hi");        // "[object String]"
Object.prototype.toString.call(true);        // "[object Boolean]"
Object.prototype.toString.call(new Date());  // "[object Date]"
Object.prototype.toString.call(/regex/);     // "[object RegExp]"
```

This is the most reliable type check in JavaScript.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.5 == vs ===

---

### Q23 🟢 What is the difference between == and ===?

**Answer:**

- `==` (loose equality) — converts types before comparing. `5 == "5"` is `true`.
- `===` (strict equality) — compares value AND type. No conversion. `5 === "5"` is `false`.

Always use `===`. The only acceptable use of `==` is `value == null` (checks both null and undefined).

---

### Q24 🟡 What is the output?

```javascript
console.log(0 == false);
console.log(0 === false);
console.log("" == false);
console.log("" === false);
console.log(null == undefined);
console.log(null === undefined);
```

**Answer:**

```
true    — false becomes 0, 0 == 0
false   — different types (number vs boolean)
true    — both become 0
false   — different types (string vs boolean)
true    — special rule: null and undefined are == to each other
false   — different types (object vs undefined)
```

---

### Q25 🔴 What is the output?

```javascript
console.log([] == false);
console.log([] == 0);
console.log([] == "");
console.log([1] == 1);
console.log(["1"] == 1);
```

**Answer:**

```
true    — [] → "" → 0, false → 0, 0 == 0
true    — [] → "" → 0, 0 == 0
true    — [] → "", "" == ""
true    — [1] → "1" → 1, 1 == 1
true    — ["1"] → "1" → 1, 1 == 1
```

Arrays convert to strings first (using `.toString()`), then to numbers for comparison.

---

### Q26 🔴 What is the output?

```javascript
console.log([] == ![]);
```

**Answer:** `true`

This is JavaScript's most infamous coercion trick. Step by step:

1. `![]` — `[]` is truthy, so `![]` is `false`
2. Now it's `[] == false`
3. `false` → `0`
4. `[]` → `""` → `0`
5. `0 == 0` → `true`

---

### Q27 🔴 What is the output?

```javascript
console.log(NaN == NaN);
console.log(NaN === NaN);
```

**Answer:** `false` and `false`

NaN is the only value in JavaScript that is not equal to itself. To check for NaN, use `Number.isNaN(value)`.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.6 Type Coercion

---

### Q28 🟢 What is the output?

```javascript
console.log(5 + "3");
console.log("5" - 3);
console.log("5" * "2");
```

**Answer:**

```
"53"    — + with string = concatenation (5 becomes "5")
2       — - forces numeric conversion ("5" becomes 5)
10      — * forces numeric conversion (both become numbers)
```

**Rule:** `+` is the ONLY operator that concatenates with strings. All others (`-`, `*`, `/`, `%`) force numbers.

---

### Q29 🟢 What are the falsy values in JavaScript?

**Answer:** There are exactly 7:

```
false
0
-0
"" (empty string)
null
undefined
NaN
```

Everything else is truthy — including `"0"`, `"false"`, `[]`, `{}`, `function(){}`.

---

### Q30 🟡 What is the output?

```javascript
console.log(1 + "2" + 3);
console.log(1 + 2 + "3");
console.log("1" + 2 + 3);
```

**Answer:**

```
"123"   — 1 + "2" = "12" (concat), "12" + 3 = "123" (concat)
"33"    — 1 + 2 = 3 (math), 3 + "3" = "33" (concat)
"123"   — "1" + 2 = "12" (concat), "12" + 3 = "123" (concat)
```

**Rule:** JS evaluates left to right. Once a string appears in a `+` chain, everything after becomes concatenation.

---

### Q31 🔴 What is the output?

```javascript
console.log([] + []);
console.log([] + {});
console.log({} + []);
console.log(true + true + "3");
console.log("3" + true + true);
```

**Answer:**

```
""                  — both [] become "", "" + "" = ""
"[object Object]"   — [] becomes "", {} becomes "[object Object]"
0 (in console)      — {} is parsed as empty block, +[] becomes +""  becomes 0
                      (in expression context: "[object Object]")
"23"                — true + true = 2 (math), 2 + "3" = "23" (concat)
"3truetrue"         — "3" + true = "3true" (concat), + true = "3truetrue"
```

---

### Q32 🔴 What is the output?

```javascript
console.log(!!"");
console.log(!!"0");
console.log(!!"false");
console.log(!!0);
console.log(!!null);
console.log(!!undefined);
console.log(!![]);
console.log(!!{});
```

**Answer:**

```
false    — "" is falsy
true     — "0" is a non-empty string → truthy!
true     — "false" is a non-empty string → truthy!
false    — 0 is falsy
false    — null is falsy
false    — undefined is falsy
true     — [] is truthy (empty array is NOT falsy!)
true     — {} is truthy (empty object is NOT falsy!)
```

**Tricky part:** `"0"` and `"false"` are truthy because they're non-empty strings. `[]` and `{}` are truthy because they're objects.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.7 Symbol

---

### Q33 🟡 What is a Symbol and why would you use it?

**Answer:**

A Symbol is a unique, immutable primitive. Every `Symbol()` creates a value guaranteed to be different from every other Symbol.

Use cases:
1. **Unique object keys** — won't collide with other property names
2. **Hidden properties** — Symbols don't show up in `for...in`, `Object.keys()`, or `JSON.stringify()`
3. **Well-known Symbols** — customize JS behavior (`Symbol.iterator`, `Symbol.toPrimitive`)

```javascript
const id = Symbol("id");
const user = { name: "Alice", [id]: 123 };

Object.keys(user);      // ["name"] — Symbol key hidden
JSON.stringify(user);    // '{"name":"Alice"}' — Symbol key not included
user[id];                // 123 — accessible only with the Symbol reference
```

---

### Q34 🟡 What is the output?

```javascript
console.log(Symbol("a") === Symbol("a"));
console.log(Symbol.for("a") === Symbol.for("a"));
```

**Answer:**

```
false   — every Symbol() creates a UNIQUE value, description doesn't matter
true    — Symbol.for() uses a global registry; same key = same symbol
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.8 BigInt

---

### Q35 🟡 When would you use BigInt?

**Answer:**

When you need integers larger than `Number.MAX_SAFE_INTEGER` (9,007,199,254,740,991 or 2^53 - 1). Beyond this, JavaScript's `number` type loses precision silently.

Common scenarios: database IDs from 64-bit systems, cryptographic calculations, financial calculations with very large numbers.

```javascript
console.log(9007199254740993);     // 9007199254740992 — WRONG! Precision lost.
console.log(9007199254740993n);    // 9007199254740993n — CORRECT with BigInt.
```

---

### Q36 🟡 What is the output?

```javascript
console.log(10n + 5n);
console.log(10n === 10);
console.log(10n == 10);
// console.log(10n + 5);  // what happens?
```

**Answer:**

```
15n     — BigInt arithmetic works normally
false   — different types (bigint vs number)
true    — loose equality coerces
```

`10n + 5` would throw `TypeError: Cannot mix BigInt and other types`. You must convert explicitly: `10n + BigInt(5)` or `Number(10n) + 5`.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.9 Pass by Value vs Reference

---

### Q37 🟡 What is the output?

```javascript
let x = 10;
function change(val) {
  val = 20;
}
change(x);
console.log(x);
```

**Answer:** `10`

Primitives are passed by value. `val` gets a COPY of `10`. Changing `val` inside the function doesn't affect `x`.

---

### Q38 🔴 What is the output?

```javascript
function modify(arr, obj) {
  arr.push(4);
  arr = [10, 20];
  arr.push(30);

  obj.name = "Bob";
  obj = { name: "Charlie" };
  obj.age = 25;
}

let myArr = [1, 2, 3];
let myObj = { name: "Alice" };
modify(myArr, myObj);
console.log(myArr);
console.log(myObj);
```

**Answer:**

```
[1, 2, 3, 4]
{ name: "Bob" }
```

Step by step:
- `arr.push(4)` — mutates original → `myArr` is `[1, 2, 3, 4]`
- `arr = [10, 20]` — reassigns LOCAL variable. `myArr` is no longer affected.
- `arr.push(30)` — pushes to the new local array, not `myArr`.
- `obj.name = "Bob"` — mutates original → `myObj` is `{ name: "Bob" }`
- `obj = { name: "Charlie" }` — reassigns LOCAL variable. `myObj` unaffected.
- `obj.age = 25` — adds to new local object, not `myObj`.

**Key rule:** Mutation (`.push`, `.name = ...`) affects the original. Reassignment (`= new value`) only affects the local variable.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.10 Immutability of Primitives

---

### Q39 🟢 What is the output?

```javascript
let str = "hello";
str[0] = "H";
console.log(str);
```

**Answer:** `"hello"` — unchanged.

Strings are immutable in JavaScript. You cannot change individual characters. `str[0] = "H"` silently fails (no error, but no effect). To change a string, you must create a new one: `str = "H" + str.slice(1)`.

---

### Q40 🟡 What is the output?

```javascript
let greeting = "hello";
let result = greeting.toUpperCase();
console.log(greeting);
console.log(result);
```

**Answer:**

```
"hello"   — original is unchanged (strings are immutable)
"HELLO"   — toUpperCase() returns a NEW string
```

All string methods (`toUpperCase`, `slice`, `replace`, `trim`, etc.) return new strings. They never modify the original. If you don't assign the result, it's lost.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# Quick Reference — All 40 Questions at a Glance

| # | Topic | Question Summary | Difficulty |
|---|-------|-----------------|------------|
| 1 | var/let/const | Differences table | 🟢 |
| 2 | var/let/const | console.log before var | 🟢 |
| 3 | var/let/const | console.log before let | 🟢 |
| 4 | var/let/const | var in for loop + setTimeout | 🟡 |
| 5 | var/let/const | const array push | 🟡 |
| 6 | var/let/const | const object mutation | 🟡 |
| 7 | var/let/const | var hoisting inside function | 🔴 |
| 8 | var/let/const | let TDZ in nested block | 🔴 |
| 9 | Primitives | Name all 7 types | 🟢 |
| 10 | Primitives | null vs undefined | 🟢 |
| 11 | Primitives | NaN quirks (5 outputs) | 🟡 |
| 12 | Primitives | 0.1 + 0.2 === 0.3 | 🟡 |
| 13 | Primitives | null/undefined/true/false in math | 🔴 |
| 14 | Reference | Primitive vs reference table | 🟢 |
| 15 | Reference | Object mutation via shared reference | 🟡 |
| 16 | Reference | Array comparison [1,2] === [1,2] | 🟡 |
| 17 | Reference | 3 ways to shallow copy | 🟡 |
| 18 | Reference | Mutation vs reassignment in function | 🔴 |
| 19 | typeof | typeof null | 🟢 |
| 20 | typeof | Check if array | 🟢 |
| 21 | typeof | 7 typeof outputs | 🟡 |
| 22 | typeof | Object.prototype.toString.call | 🔴 |
| 23 | == vs === | Difference explained | 🟢 |
| 24 | == vs === | 6 comparison outputs | 🟡 |
| 25 | == vs === | Array loose equality tricks | 🔴 |
| 26 | == vs === | [] == ![] | 🔴 |
| 27 | == vs === | NaN == NaN | 🔴 |
| 28 | Coercion | + and - with strings | 🟢 |
| 29 | Coercion | List all falsy values | 🟢 |
| 30 | Coercion | Mixed string + number chains | 🟡 |
| 31 | Coercion | [] + {}, {} + [], true + "3" | 🔴 |
| 32 | Coercion | !! double negation outputs | 🔴 |
| 33 | Symbol | What and why | 🟡 |
| 34 | Symbol | Symbol() vs Symbol.for() | 🟡 |
| 35 | BigInt | When to use | 🟡 |
| 36 | BigInt | BigInt operations and comparison | 🟡 |
| 37 | Pass by val/ref | Primitive in function | 🟡 |
| 38 | Pass by val/ref | Array + object mutation vs reassign | 🔴 |
| 39 | Immutability | String character assignment | 🟢 |
| 40 | Immutability | String method returns new string | 🟡 |
