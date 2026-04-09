# Category 1: Variables & Data Types
## Complete Interview Guide — Every Sub-Topic with Explanation, Code, Questions & Answers

---

# Table of Contents

- [1.1 var vs let vs const](#11-var-vs-let-vs-const)
- [1.2 Primitive Types](#12-primitive-types)
- [1.3 Reference Types](#13-reference-types)
- [1.4 typeof Operator and Its Quirks](#14-typeof-operator-and-its-quirks)
- [1.5 == vs === (Loose vs Strict Equality)](#15--vs--loose-vs-strict-equality)
- [1.6 Type Coercion (Implicit Conversion)](#16-type-coercion-implicit-conversion)
- [1.7 Symbol and Its Use Cases](#17-symbol-and-its-use-cases)
- [1.8 BigInt and When to Use It](#18-bigint-and-when-to-use-it)
- [1.9 Pass by Value vs Pass by Reference](#19-pass-by-value-vs-pass-by-reference)
- [1.10 Immutability of Primitives](#110-immutability-of-primitives)
- [Quick Revision — One-Liners](#quick-revision--one-liners)

---

# 1.1 var vs let vs const

> 🔴 **Must-know** — Asked in almost every interview.

## Explanation

JavaScript has 3 ways to declare variables. They differ in scope, hoisting, and reassignment.

```
var  → function-scoped, hoisted (initialized as undefined), CAN be reassigned
let  → block-scoped, hoisted (but in TDZ, NOT initialized), CAN be reassigned
const → block-scoped, hoisted (but in TDZ, NOT initialized), CANNOT be reassigned
```

## Code Example

```javascript
// === SCOPE DIFFERENCE ===

// var is FUNCTION scoped — leaks out of blocks
if (true) {
  var x = 10;
}
console.log(x); // 10 ← var leaked out of the if block!

// let and const are BLOCK scoped — stay inside {}
if (true) {
  let y = 20;
  const z = 30;
}
// console.log(y); // ❌ ReferenceError: y is not defined
// console.log(z); // ❌ ReferenceError: z is not defined


// === HOISTING DIFFERENCE ===

console.log(a); // undefined ← var is hoisted and initialized as undefined
var a = 5;

// console.log(b); // ❌ ReferenceError ← let is hoisted but in TDZ
let b = 5;

// console.log(c); // ❌ ReferenceError ← const is hoisted but in TDZ
const c = 5;


// === REASSIGNMENT DIFFERENCE ===

var name = "Alice";
name = "Bob";       // ✅ works

let age = 25;
age = 26;           // ✅ works

const city = "Delhi";
// city = "Mumbai"; // ❌ TypeError: Assignment to constant variable

// ⚠️ TRICKY: const objects CAN be mutated, just not reassigned
const person = { name: "Alice" };
person.name = "Bob";     // ✅ works! (mutating the object, not reassigning)
// person = { name: "Bob" }; // ❌ this would fail (reassigning the variable)


// === REDECLARATION ===

var x = 1;
var x = 2;   // ✅ var allows redeclaration

let y = 1;
// let y = 2; // ❌ SyntaxError: 'y' has already been declared

const z = 1;
// const z = 2; // ❌ SyntaxError: 'z' has already been declared
```

## Interview Questions

**Q1: What are the differences between var, let, and const?**

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Hoisting | Yes (undefined) | Yes (TDZ) | Yes (TDZ) |
| Reassign | ✅ Yes | ✅ Yes | ❌ No |
| Redeclare | ✅ Yes | ❌ No | ❌ No |

**Q2: What is the output?**

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
```

**Answer:** `3, 3, 3` — Because `var` is function-scoped. By the time setTimeout runs, the loop is done and `i` is 3. With `let` it would print `0, 1, 2` because each iteration gets its own block-scoped `i`.

**Q3: Can you change a property of a const object?**

**Answer:** Yes. `const` prevents reassigning the variable, not mutating the object. `const obj = {a: 1}; obj.a = 2;` works fine. `obj = {}` would fail.

## Common Mistake

```
Thinking const makes the VALUE immutable.
const only makes the BINDING (the variable) immutable.
Objects and arrays declared with const CAN still be changed inside.
To truly freeze an object: Object.freeze(obj)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.2 Primitive Types

> 🔴 **Must-know** — Foundation of everything in JS.

## Explanation

JavaScript has **7 primitive types**. They are simple, immutable values stored directly in memory.

```
string    → "hello", 'world', `template`
number    → 42, 3.14, -7, NaN, Infinity
boolean   → true, false
null      → intentional absence of value
undefined → variable declared but not assigned
symbol    → unique identifier (ES6)
bigint    → numbers larger than 2^53 - 1 (ES2020)
```

## Code Example

```javascript
// === ALL 7 PRIMITIVES ===

const str = "hello";           // string
const num = 42;                // number
const bool = true;             // boolean
const nothing = null;          // null (intentional empty)
const notAssigned = undefined; // undefined (not yet set)
const sym = Symbol("id");     // symbol (unique)
const big = 9007199254740992n; // bigint (the 'n' at the end)

// === KEY PROPERTIES ===

// Primitives are IMMUTABLE — you can't change them in place
let name = "hello";
name[0] = "H";         // does nothing — strings are immutable
console.log(name);      // "hello" (unchanged)
name = "Hello";         // this works — you're REPLACING, not mutating

// Primitives are compared BY VALUE
const a = 5;
const b = 5;
console.log(a === b);   // true — same value

// Each primitive has a wrapper object for methods
"hello".toUpperCase();  // "HELLO" — JS auto-boxes to String object
(42).toFixed(2);        // "42.00" — JS auto-boxes to Number object
```

## Interview Questions

**Q1: How many primitive types are there in JavaScript? Name them.**

**Answer:** 7 — string, number, boolean, null, undefined, symbol, bigint.

**Q2: What is the difference between null and undefined?**

```javascript
let a;
console.log(a);        // undefined — declared but not assigned

let b = null;
console.log(b);        // null — intentionally set to "nothing"

// Both are falsy
console.log(!null);      // true
console.log(!undefined); // true

// But they're different types
console.log(typeof null);      // "object" ← famous JS bug!
console.log(typeof undefined); // "undefined"

// Loose equality: they're equal to each other
console.log(null == undefined);  // true
// Strict equality: they're NOT equal
console.log(null === undefined); // false
```

**Answer:**
- `undefined` = variable exists but has no value assigned yet (JS does this)
- `null` = developer intentionally says "this has no value" (YOU do this)

**Q3: Is NaN a number?**

```javascript
console.log(typeof NaN); // "number" ← yes, NaN is type "number"!
console.log(NaN === NaN); // false ← NaN is not equal to itself!
console.log(isNaN(NaN));  // true ← use isNaN() to check
console.log(Number.isNaN(NaN)); // true ← stricter check (recommended)
console.log(Number.isNaN("hello")); // false (isNaN("hello") would be true)
```

**Answer:** `typeof NaN` is `"number"` — it's a special numeric value meaning "not a valid number." It's the only value in JS that is not equal to itself.

## Common Mistake

```
Confusing null and undefined.
  undefined → JS's default "not assigned yet"
  null → YOUR explicit "no value"

Never set a variable to undefined yourself.
If you want "no value," use null.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.3 Reference Types

> 🔴 **Must-know** — Understanding this prevents 90% of mutation bugs.

## Explanation

Everything that is NOT a primitive is a **reference type**. The variable doesn't hold the value — it holds a pointer (reference) to the value in memory.

```
The 3 main reference types:
  Object   → { key: value }
  Array    → [1, 2, 3]
  Function → function() {}
```

## Code Example

```javascript
// === PRIMITIVES: COPY THE VALUE ===
let a = 10;
let b = a;       // b gets a COPY of 10
b = 20;
console.log(a);  // 10 — unchanged. a and b are independent.


// === REFERENCES: COPY THE POINTER ===
let obj1 = { name: "Alice" };
let obj2 = obj1;             // obj2 points to the SAME object
obj2.name = "Bob";
console.log(obj1.name);     // "Bob" ← obj1 is ALSO changed!

// Same with arrays
let arr1 = [1, 2, 3];
let arr2 = arr1;             // arr2 points to the SAME array
arr2.push(4);
console.log(arr1);           // [1, 2, 3, 4] ← arr1 is ALSO changed!


// === HOW TO MAKE A REAL COPY ===

// Shallow copy (one level deep)
let original = { name: "Alice", scores: [1, 2, 3] };
let copy1 = { ...original };           // spread operator
let copy2 = Object.assign({}, original); // Object.assign

copy1.name = "Bob";
console.log(original.name); // "Alice" ← safe, name is a primitive

copy1.scores.push(4);
console.log(original.scores); // [1, 2, 3, 4] ← NOT safe! Nested reference shared!

// Deep copy (all levels)
let deepCopy = structuredClone(original); // modern way (2022+)
let deepCopy2 = JSON.parse(JSON.stringify(original)); // older way (has limitations)

deepCopy.scores.push(5);
console.log(original.scores); // [1, 2, 3, 4] ← safe! Fully independent.
```

## Interview Questions

**Q1: What is the difference between primitive and reference types?**

| Feature | Primitive | Reference |
|---------|-----------|-----------|
| Stored as | Value directly | Pointer to memory location |
| Copy behavior | Copies the value | Copies the pointer (same object) |
| Comparison | By value | By reference |
| Mutation | Immutable | Mutable |
| Examples | string, number, boolean | object, array, function |

**Q2: What is the output?**

```javascript
let a = [1, 2, 3];
let b = [1, 2, 3];
console.log(a === b);
```

**Answer:** `false` — Even though they look the same, `a` and `b` point to two DIFFERENT arrays in memory. Reference types are compared by their memory address, not their content.

**Q3: How do you deep clone an object?**

**Answer:**
- `structuredClone(obj)` — modern, handles most types (recommended)
- `JSON.parse(JSON.stringify(obj))` — works but fails with functions, Date, undefined, circular references
- Libraries like Lodash `_.cloneDeep(obj)`
- Write a recursive function manually

## Common Mistake

```
Thinking { ...obj } is a deep copy. It's NOT.
Spread only copies ONE level. Nested objects are still shared.

const original = { user: { name: "Alice" } };
const copy = { ...original };
copy.user.name = "Bob";
console.log(original.user.name); // "Bob" ← OOPS! Both changed.

Use structuredClone() for true deep copy.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.4 typeof Operator and Its Quirks

> 🔴 **Must-know** — typeof has bugs that interviewers love to test.

## Explanation

`typeof` returns a string telling you the type of a value. But it has famous quirks.

## Code Example

```javascript
// === NORMAL BEHAVIOR ===
console.log(typeof "hello");     // "string"
console.log(typeof 42);          // "number"
console.log(typeof true);        // "boolean"
console.log(typeof undefined);   // "undefined"
console.log(typeof Symbol());    // "symbol"
console.log(typeof 42n);         // "bigint"
console.log(typeof function(){}); // "function"
console.log(typeof {});          // "object"
console.log(typeof []);          // "object" ← array is "object"!

// === THE FAMOUS QUIRKS ===

// QUIRK 1: typeof null === "object" (this is a BUG from 1995!)
console.log(typeof null);        // "object" ← WRONG! null is not an object.
// This is a historical bug that can never be fixed (would break the web).

// QUIRK 2: typeof array === "object"
console.log(typeof [1, 2, 3]);   // "object" ← doesn't tell you it's an array!
// Fix: use Array.isArray()
console.log(Array.isArray([1, 2, 3])); // true
console.log(Array.isArray({}));         // false

// QUIRK 3: typeof NaN === "number"
console.log(typeof NaN);         // "number" ← NaN is technically a number!

// QUIRK 4: typeof function
console.log(typeof function(){}); // "function" ← not "object"
// Functions ARE objects, but typeof gives them a special type.

// === HOW TO PROPERLY CHECK TYPES ===
// For null:        value === null
// For array:       Array.isArray(value)
// For NaN:         Number.isNaN(value)
// For everything:  Object.prototype.toString.call(value)

console.log(Object.prototype.toString.call(null));      // "[object Null]"
console.log(Object.prototype.toString.call([]));        // "[object Array]"
console.log(Object.prototype.toString.call({}));        // "[object Object]"
console.log(Object.prototype.toString.call("hi"));      // "[object String]"
console.log(Object.prototype.toString.call(42));        // "[object Number]"
```

## Interview Questions

**Q1: What is typeof null and why?**

**Answer:** `"object"` — This is a known bug from the first version of JavaScript (1995). In the original implementation, values were tagged with a type code, and `null` used the same tag as objects (0). It was never fixed because changing it would break existing code across the web.

**Q2: How do you check if a value is an array?**

**Answer:** Use `Array.isArray(value)` — because `typeof []` returns `"object"` which doesn't distinguish arrays from regular objects.

**Q3: What is the output?**

```javascript
console.log(typeof typeof 42);
```

**Answer:** `"string"` — `typeof 42` returns `"number"` (a string). Then `typeof "number"` returns `"string"`.

## Common Mistake

```
Using typeof to check for null:
  if (typeof value === "null") → NEVER WORKS! typeof null is "object"
  if (value === null) → ✅ correct way
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.5 == vs === (Loose vs Strict Equality)

> 🔴 **Must-know** — One of the most tested JS concepts.

## Explanation

```
==  (loose equality)  → compares VALUES after type coercion
=== (strict equality) → compares VALUES AND TYPES, no coercion
```

## Code Example

```javascript
// === LOOSE EQUALITY (==) — converts types before comparing ===

console.log(5 == "5");          // true ← string "5" becomes number 5
console.log(0 == false);        // true ← false becomes 0
console.log("" == false);       // true ← both become 0
console.log(null == undefined); // true ← special rule: they're equal to each other
console.log(0 == null);         // false ← null only equals undefined, not 0
console.log("" == null);        // false ← null only equals undefined
console.log(NaN == NaN);        // false ← NaN is not equal to anything, even itself!

// === STRICT EQUALITY (===) — NO type conversion ===

console.log(5 === "5");          // false ← different types (number vs string)
console.log(0 === false);        // false ← different types (number vs boolean)
console.log("" === false);       // false ← different types (string vs boolean)
console.log(null === undefined); // false ← different types
console.log(NaN === NaN);        // false ← still false even with ===!

// === THE RULE ===
// ALWAYS use === in your code. Use == only when you INTENTIONALLY want coercion.
// The only acceptable use of == is: value == null (checks both null and undefined)

function process(value) {
  if (value == null) {
    // This catches BOTH null AND undefined
    console.log("No value provided");
  }
}
process(null);      // "No value provided"
process(undefined); // "No value provided"
process(0);         // (nothing — 0 is not null)
```

## Interview Questions

**Q1: What is the difference between == and ===?**

**Answer:**
- `==` converts types before comparing (type coercion). `5 == "5"` is true.
- `===` compares both value AND type without conversion. `5 === "5"` is false.
- Always use `===` except when checking `value == null` (shorthand for null or undefined).

**Q2: What is the output?**

```javascript
console.log([] == false);   // ?
console.log([] == ![]);     // ?
```

**Answer:**
- `[] == false` → `true` — `[]` coerces to `""`, then to `0`. `false` coerces to `0`. `0 == 0` is true.
- `[] == ![]` → `true` — `![]` is `false` (any object is truthy, negation is false). Then `[] == false` → same as above → true. This is the most famous JS coercion trick.

**Q3: When is it OK to use ==?**

**Answer:** Only one case: `value == null` — this checks for both `null` and `undefined` in a single comparison. Everything else should use `===`.

## Common Mistake

```
Using == by habit and getting unexpected results:
  "" == 0       → true  (both coerce to 0)
  " " == 0      → true  (space string coerces to 0)
  "0" == false   → true  ("0" → 0, false → 0)
  [] == 0        → true  ([] → "" → 0)

Fix: Always use ===
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.6 Type Coercion (Implicit Conversion)

> 🔴 **Must-know** — Powers behind ==, + operator, and if() checks.

## Explanation

Type coercion is when JavaScript automatically converts one type to another. It happens in 3 situations: comparisons, operations, and boolean checks.

## Code Example

```javascript
// === STRING COERCION (+ with a string) ===

console.log(5 + "3");         // "53" ← number becomes string, concatenated
console.log("5" + 3);         // "53"
console.log("5" + true);      // "5true"
console.log("5" + null);      // "5null"
console.log("5" + undefined); // "5undefined"

// === NUMBER COERCION (-, *, /, or == with number) ===

console.log("5" - 3);         // 2 ← string becomes number
console.log("5" * 2);         // 10
console.log("5" / 2);         // 2.5
console.log("abc" - 1);       // NaN ← "abc" can't become a number
console.log(true + 1);        // 2 ← true becomes 1
console.log(false + 1);       // 1 ← false becomes 0
console.log(null + 1);        // 1 ← null becomes 0
console.log(undefined + 1);   // NaN ← undefined becomes NaN

// === BOOLEAN COERCION (in if, !, &&, ||, ternary) ===

// FALSY values (become false):
//   false, 0, -0, "", null, undefined, NaN

// TRUTHY values (become true):
//   everything else, including: "0", " ", [], {}, function(){}

if ("hello") console.log("truthy");  // runs ✅
if ("") console.log("truthy");       // doesn't run (empty string is falsy)
if (0) console.log("truthy");        // doesn't run (0 is falsy)
if ([]) console.log("truthy");       // runs ✅ (empty array is truthy!)
if ({}) console.log("truthy");       // runs ✅ (empty object is truthy!)

// === EXPLICIT CONVERSION (you do it yourself) ===

// To String
String(123);        // "123"
(123).toString();   // "123"
123 + "";           // "123" (implicit)

// To Number
Number("42");       // 42
parseInt("42px");   // 42 (stops at non-number)
parseFloat("3.14"); // 3.14
+"42";              // 42 (unary + operator)

// To Boolean
Boolean(0);         // false
Boolean("");        // false
Boolean("hello");   // true
Boolean([]);        // true ← surprise!
!!value;            // double negation = explicit boolean conversion
```

## Interview Questions

**Q1: What is the output?**

```javascript
console.log(1 + "2" + 3);
```

**Answer:** `"123"` — Step by step: `1 + "2"` = `"12"` (number coerced to string). Then `"12" + 3` = `"123"` (3 coerced to string).

**Q2: What are the falsy values in JavaScript?**

**Answer:** There are exactly 7 falsy values:
`false`, `0`, `-0`, `""` (empty string), `null`, `undefined`, `NaN`

Everything else is truthy — including `"0"`, `" "`, `[]`, `{}`.

**Q3: What is the output?**

```javascript
console.log([] + []);       // ?
console.log([] + {});       // ?
console.log({} + []);       // ?
```

**Answer:**
- `[] + []` → `""` — Both arrays convert to empty strings. `"" + ""` = `""`.
- `[] + {}` → `"[object Object]"` — `[]` → `""`, `{}` → `"[object Object]"`.
- `{} + []` → `0` (in console) — `{}` is interpreted as empty block, then `+[]` = `+""` = `0`. (In an expression context, it gives `"[object Object]"` like above.)

## Common Mistake

```
Assuming + always adds numbers:
  5 + "3" = "53" NOT 8
  
The rule: if EITHER side of + is a string, it concatenates.
For math with strings: use Number(), parseInt(), or unary +
  Number("3") + 5 = 8
  +"3" + 5 = 8
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.7 Symbol and Its Use Cases

> 🟡 **Should-know** — Asked at mid-level and senior interviews.

## Explanation

A Symbol is a unique, immutable primitive. No two symbols are the same, even with the same description. Used for creating unique keys that can't be accidentally overwritten.

## Code Example

```javascript
// === CREATING SYMBOLS ===

const sym1 = Symbol("id");
const sym2 = Symbol("id");
console.log(sym1 === sym2);   // false ← every Symbol is unique!
console.log(sym1.toString());  // "Symbol(id)"

// === USE CASE 1: Unique object keys (no accidental collision) ===

const ID = Symbol("id");
const user = {
  name: "Alice",
  [ID]: 12345      // Symbol as key
};

console.log(user.name);  // "Alice"
console.log(user[ID]);   // 12345

// Symbols are hidden from normal iteration
console.log(Object.keys(user));         // ["name"] ← Symbol key not shown!
console.log(JSON.stringify(user));       // {"name":"Alice"} ← Symbol key not included!
console.log(Object.getOwnPropertySymbols(user)); // [Symbol(id)] ← special method to find them

// === USE CASE 2: Symbol.for() — global symbol registry ===

const s1 = Symbol.for("shared");
const s2 = Symbol.for("shared");
console.log(s1 === s2);  // true ← same key in global registry → same symbol

// === USE CASE 3: Well-known symbols (customize JS behavior) ===

// Symbol.iterator — make your object iterable
// Symbol.toPrimitive — customize type conversion
// Symbol.hasInstance — customize instanceof
```

## Interview Questions

**Q1: What is a Symbol and why would you use it?**

**Answer:** A Symbol is a unique, immutable primitive value. Use it when you need guaranteed unique property keys that won't collide with other keys, won't appear in `JSON.stringify`, and won't show up in `for...in` or `Object.keys()`. Useful for library code and meta-programming.

**Q2: Are two Symbols with the same description equal?**

**Answer:** No. `Symbol("a") === Symbol("a")` is `false`. Every `Symbol()` creates a unique value. Use `Symbol.for("a")` if you want shared symbols.

## Common Mistake

```
Trying to create Symbol with new:
  new Symbol() → ❌ TypeError
  Symbol()     → ✅ correct (no new keyword)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.8 BigInt and When to Use It

> 🟡 **Should-know** — Asked when discussing large numbers or precision.

## Explanation

JavaScript `number` type is a 64-bit float. It can safely represent integers up to `2^53 - 1` (9,007,199,254,740,991). Beyond that, precision is lost. `BigInt` handles arbitrarily large integers.

## Code Example

```javascript
// === THE PROBLEM: number loses precision ===

console.log(9007199254740991);     // 9007199254740991 ← OK (Number.MAX_SAFE_INTEGER)
console.log(9007199254740992);     // 9007199254740992 ← OK
console.log(9007199254740993);     // 9007199254740992 ← WRONG! Same as above!

console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
console.log(Number.isSafeInteger(9007199254740993)); // false


// === THE SOLUTION: BigInt ===

const big = 9007199254740993n;     // add 'n' at the end
console.log(big);                   // 9007199254740993n ← correct!

const also = BigInt("9007199254740993");  // from string
console.log(also);                  // 9007199254740993n


// === OPERATIONS ===

console.log(10n + 20n);     // 30n ✅
console.log(10n * 3n);      // 30n ✅
// console.log(10n + 5);    // ❌ TypeError: can't mix BigInt and number!
console.log(10n + BigInt(5)); // 15n ✅ — convert number to BigInt first


// === COMPARISON ===

console.log(10n === 10);    // false ← different types
console.log(10n == 10);     // true ← loose equality coerces
console.log(10n > 5);       // true ← comparison works across types
```

## Interview Questions

**Q1: When would you use BigInt?**

**Answer:** When dealing with integers larger than `Number.MAX_SAFE_INTEGER` (2^53 - 1), such as database IDs from systems that use 64-bit integers, cryptographic calculations, or financial calculations requiring exact precision with very large numbers.

**Q2: Can you mix BigInt and Number in operations?**

**Answer:** No. `10n + 5` throws TypeError. You must explicitly convert: `10n + BigInt(5)` or `Number(10n) + 5` (but Number conversion may lose precision).

## Common Mistake

```
Assuming all numbers are safe:
  const id = 9007199254740999; // WRONG — precision lost silently!
  const id = 9007199254740999n; // CORRECT — use BigInt for large IDs
  
Also: JSON.stringify() doesn't support BigInt.
  JSON.stringify({ id: 10n }) → ❌ TypeError
  Fix: JSON.stringify({ id: 10n.toString() })
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.9 Pass by Value vs Pass by Reference

> 🟡 **Should-know** — Explains mutation bugs that trip up many developers.

## Explanation

```
Primitives → passed by VALUE (a copy is made, original is safe)
Objects/Arrays → passed by REFERENCE (pointer is copied, both point to same data)
```

## Code Example

```javascript
// === PASS BY VALUE (primitives) ===

function changeValue(x) {
  x = 100;
  console.log("Inside:", x);  // 100
}

let num = 5;
changeValue(num);
console.log("Outside:", num);  // 5 ← unchanged! Copy was modified, not original.


// === PASS BY REFERENCE (objects/arrays) ===

function changeName(obj) {
  obj.name = "Bob";
  console.log("Inside:", obj.name);  // "Bob"
}

let person = { name: "Alice" };
changeName(person);
console.log("Outside:", person.name);  // "Bob" ← CHANGED! Same object in memory.


// === TRICKY: Reassignment vs Mutation ===

function replaceObject(obj) {
  obj = { name: "Charlie" };  // this creates a NEW object, doesn't affect original
  console.log("Inside:", obj.name);  // "Charlie"
}

let user = { name: "Alice" };
replaceObject(user);
console.log("Outside:", user.name);  // "Alice" ← unchanged!
// Why? Because obj = { ... } reassigned the LOCAL variable, not the original.

// But MUTATION works:
function mutateObject(obj) {
  obj.name = "Charlie";  // this CHANGES the original object
}

mutateObject(user);
console.log("Outside:", user.name);  // "Charlie" ← changed!
```

## Interview Questions

**Q1: Is JavaScript pass by value or pass by reference?**

**Answer:** Technically, JavaScript is always "pass by value." But for objects, the VALUE being passed IS a reference (memory address). So primitives get a copy of the value, and objects get a copy of the reference — meaning both the original and the copy point to the same object in memory.

**Q2: What is the output?**

```javascript
function modify(arr) {
  arr.push(4);
  arr = [10, 20, 30];
  arr.push(40);
}

let myArr = [1, 2, 3];
modify(myArr);
console.log(myArr);
```

**Answer:** `[1, 2, 3, 4]` — `arr.push(4)` mutates the original array. Then `arr = [10, 20, 30]` reassigns the LOCAL variable to a new array — the original `myArr` is no longer affected. `arr.push(40)` adds to the new local array, not `myArr`.

## Common Mistake

```
Thinking reassignment inside a function changes the original:
  function change(obj) { obj = { new: true }; }
  → This NEVER changes the original. It only reassigns the local copy.

To change the original, MUTATE it:
  function change(obj) { obj.new = true; }
  → This DOES change the original.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.10 Immutability of Primitives

> 🟡 **Should-know** — Explains why string methods return new strings.

## Explanation

Primitive values cannot be changed after creation. When you "change" a primitive, you're actually creating a new value and reassigning the variable.

## Code Example

```javascript
// === STRINGS ARE IMMUTABLE ===

let str = "hello";
str[0] = "H";           // does nothing — no error, just silently fails
console.log(str);        // "hello" — unchanged!

// To "change" a string, you create a NEW one
str = "H" + str.slice(1);
console.log(str);        // "Hello" — new string assigned to str

// All string methods return NEW strings
let greeting = "hello world";
let upper = greeting.toUpperCase();
console.log(greeting);   // "hello world" — original unchanged
console.log(upper);      // "HELLO WORLD" — new string


// === NUMBERS ARE IMMUTABLE ===

let x = 5;
// You can't change the value 5 itself.
// x++ creates the value 6 and assigns it to x.
x++;
console.log(x);   // 6 — x now points to a new value (6), not a modified 5


// === WHY THIS MATTERS ===

// Primitives: safe to pass around — no one can change YOUR copy
let a = "hello";
let b = a;
b = "world";
console.log(a);   // "hello" — completely safe, independent copies

// Objects: NOT safe — shared reference means changes affect everyone
let obj1 = { name: "Alice" };
let obj2 = obj1;
obj2.name = "Bob";
console.log(obj1.name);  // "Bob" — BOTH changed because same reference
```

## Interview Questions

**Q1: Are strings mutable in JavaScript?**

**Answer:** No. Strings are immutable primitives. You cannot change individual characters (`str[0] = "X"` does nothing). All string methods (toUpperCase, slice, replace, etc.) return a NEW string — the original is never modified.

**Q2: What is the output?**

```javascript
let s = "hello";
s.toUpperCase();
console.log(s);
```

**Answer:** `"hello"` — `toUpperCase()` returns a new string `"HELLO"` but it's not assigned to anything. The original `s` is unchanged because strings are immutable.

## Common Mistake

```
Forgetting to ASSIGN the result of string methods:
  
  BAD:
  let name = "alice";
  name.toUpperCase();     // result thrown away!
  console.log(name);       // "alice"

  GOOD:
  let name = "alice";
  name = name.toUpperCase();  // assign the new string back
  console.log(name);           // "ALICE"
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Quick Revision — One-Liners

> Glance through these before your interview for a fast refresh.

```
1.1  var = function-scoped, let/const = block-scoped. const can't reassign but CAN mutate objects.
1.2  7 primitives: string, number, boolean, null, undefined, symbol, bigint.
1.3  Primitives copy values. Objects copy references (pointers to same data).
1.4  typeof null = "object" (bug). typeof [] = "object" (use Array.isArray).
1.5  == coerces types. === doesn't. Always use === except for value == null.
1.6  + with string = concatenation. Falsy: false, 0, "", null, undefined, NaN.
1.7  Symbol = unique value. Symbol("a") !== Symbol("a"). Used for unique keys.
1.8  BigInt handles integers beyond 2^53-1. Can't mix with Number in operations.
1.9  Primitives pass by value (safe). Objects pass by reference (shared mutation).
1.10 Primitives are immutable. String methods return NEW strings, don't change original.
```

[↑ Back to Table of Contents](#table-of-contents)
