# JavaScript Complete Interview Guide
## Part 1: Categories 1-6 (Core Language)

---

# Table of Contents — Part 1

- [Category 1: Variables & Data Types](#category-1-variables--data-types)
- [Category 2: Scope & Hoisting](#category-2-scope--hoisting)
- [Category 3: Functions](#category-3-functions)
- [Category 4: Closures](#category-4-closures)
- [Category 5: this Keyword](#category-5-this-keyword)
- [Category 6: Objects & Prototypes](#category-6-objects--prototypes)
- [Quick Revision — Part 1 One-Liners](#quick-revision--part-1-one-liners)

---

# Category 1: Variables & Data Types

---

### 🔴 1.1 var vs let vs const

**Explanation:** Three ways to declare variables. They differ in scope (where they're accessible), hoisting (moved to top before execution), and reassignment (can you change the value).

```javascript
// SCOPE
if (true) {
  var a = 1;    // function-scoped — leaks out of block
  let b = 2;    // block-scoped — stays inside {}
  const c = 3;  // block-scoped — stays inside {}
}
console.log(a);  // 1 (leaked)
// console.log(b); // ReferenceError
// console.log(c); // ReferenceError

// HOISTING
console.log(x);  // undefined (var hoisted, initialized as undefined)
var x = 10;
// console.log(y); // ReferenceError (let hoisted but in TDZ)
let y = 10;

// REASSIGNMENT
let count = 1;
count = 2;        // ✅ allowed

const name = "Alice";
// name = "Bob";  // ❌ TypeError

// ⚠️ const objects CAN be mutated
const obj = { a: 1 };
obj.a = 2;        // ✅ works (mutating, not reassigning)
// obj = {};      // ❌ fails (reassigning)
```

**Interview Q: What is the output?** 🔴
```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
```
**A:** `3, 3, 3` — `var` is function-scoped, one shared `i`. After loop, `i` is 3. Fix: use `let` (each iteration gets its own `i` → prints `0, 1, 2`).

**Mistake:** Thinking `const` makes objects immutable. It only prevents reassignment. Use `Object.freeze()` for immutability.

**One-liner:** var = function-scoped + hoisted. let = block-scoped + reassignable. const = block-scoped + no reassignment (but objects can mutate).

---

### 🔴 1.2 Primitive Types (7 types)

**Explanation:** JavaScript has 7 primitive types — simple, immutable values stored directly in memory. Everything else is an object.

```javascript
const str = "hello";           // string
const num = 42;                // number (includes NaN, Infinity)
const bool = true;             // boolean
const nothing = null;          // null (intentional "no value")
const notSet = undefined;      // undefined (not yet assigned)
const sym = Symbol("id");     // symbol (unique identifier)
const big = 9007199254740993n; // bigint (large integers)
```

**Interview Q: What is the difference between null and undefined?** 🔴

| | null | undefined |
|---|---|---|
| Meaning | YOU say "no value" | JS says "not assigned yet" |
| typeof | `"object"` (bug) | `"undefined"` |
| == | `null == undefined` → true | `null == undefined` → true |
| === | `null === undefined` → false | different types |

**Mistake:** Setting variables to `undefined` manually. Use `null` for intentional absence.

**One-liner:** 7 primitives: string, number, boolean, null, undefined, symbol, bigint. Immutable. Compared by value.

---

### 🔴 1.3 Reference Types (objects, arrays, functions)

**Explanation:** Non-primitives are stored as references (pointers to memory). Copying a reference copies the pointer, not the data — both variables point to the same object.

```javascript
// Primitives: copy the VALUE
let a = 5;
let b = a;
b = 10;
console.log(a); // 5 (independent)

// References: copy the POINTER
let obj1 = { name: "Alice" };
let obj2 = obj1;
obj2.name = "Bob";
console.log(obj1.name); // "Bob" (both point to same object!)

// Comparison
[1, 2] === [1, 2];  // false (different references)
{ a: 1 } === { a: 1 };  // false

// Shallow copy (one level)
const copy = { ...obj1 };
const arrCopy = [...arr];

// Deep copy (all levels)
const deep = structuredClone(obj1);
```

**Interview Q: What is the output?** 🟡
```javascript
let a = { x: 1 };
let b = a;
b.x = 2;
console.log(a.x);
```
**A:** `2` — `b = a` copies the reference. Both point to the same object.

**Mistake:** Using spread `{...obj}` and thinking it's a deep copy. Nested objects are still shared.

**One-liner:** Primitives = copy by value (independent). Objects/arrays = copy by reference (shared). Deep copy: structuredClone().

---

### 🔴 1.4 typeof and Its Quirks

**Explanation:** `typeof` returns a string indicating the type. But it has famous bugs that interviewers love.

```javascript
typeof "hello"       // "string"
typeof 42            // "number"
typeof true          // "boolean"
typeof undefined     // "undefined"
typeof Symbol()      // "symbol"
typeof 42n           // "bigint"
typeof function(){}  // "function"
typeof {}            // "object"
typeof []            // "object"     ← array is "object"!
typeof null          // "object"     ← famous bug from 1995!
typeof NaN           // "number"     ← NaN is type "number"!

// Proper checks:
Array.isArray([]);       // true (check array)
value === null;          // (check null)
Number.isNaN(NaN);       // true (check NaN)
```

**Interview Q: What is `typeof typeof 42`?** 🟡
**A:** `"string"` — `typeof 42` returns `"number"` (a string). `typeof "number"` returns `"string"`.

**Mistake:** Using `typeof` to check for null or arrays. `typeof null === "object"` and `typeof [] === "object"`.

**One-liner:** typeof null = "object" (bug). typeof [] = "object" (use Array.isArray). typeof NaN = "number". typeof typeof x = "string".

---

### 🔴 1.5 == vs === (Loose vs Strict)

**Explanation:** `==` converts types before comparing (coercion). `===` compares type AND value — no conversion.

```javascript
5 == "5"          // true  (string "5" becomes number 5)
5 === "5"         // false (different types)
0 == false        // true  (false becomes 0)
0 === false       // false
"" == false       // true  (both become 0)
null == undefined // true  (special rule)
null === undefined // false
NaN == NaN        // false (NaN is never equal to anything!)
NaN === NaN       // false
```

**Interview Q: What is the output?** 🔴
```javascript
console.log([] == ![]);
```
**A:** `true` — `![]` = `false` (array is truthy). Then `[] == false` → `[]` becomes `""` becomes `0`, `false` becomes `0`. `0 == 0` → `true`.

**Mistake:** Using `==` by default. Always use `===`. Only acceptable `==`: `value == null` (checks both null and undefined).

**One-liner:** == coerces types (5 == "5" true). === checks type + value (5 === "5" false). Always use ===.

---

### 🔴 1.6 Type Coercion

**Explanation:** JavaScript automatically converts types in operations. The `+` operator concatenates with strings; `-`, `*`, `/` force numbers.

```javascript
// String coercion (+ with string)
5 + "3"          // "53" (number → string, concatenated)
"5" + true       // "5true"

// Number coercion (-, *, /)
"5" - 3          // 2
"5" * "2"        // 10
true + true      // 2 (true = 1)
null + 1         // 1 (null = 0)
undefined + 1    // NaN (undefined = NaN)

// Boolean coercion (in if, !, &&, ||)
// Falsy: false, 0, -0, "", null, undefined, NaN
// Truthy: everything else (including "0", [], {})

!!"0"            // true  (non-empty string)
!![]             // true  (empty array is truthy!)
!!0              // false
```

**Interview Q: What is the output?** 🔴
```javascript
console.log(1 + "2" + 3);   // ?
console.log(1 + 2 + "3");   // ?
```
**A:** `"123"` and `"33"` — JS evaluates left to right. Once a string appears in `+`, everything after concatenates.

**Mistake:** Assuming `+` always adds numbers. If either side is a string, it concatenates.

**One-liner:** + with string = concatenation. - * / force numbers. 7 falsy values: false, 0, -0, "", null, undefined, NaN.

---

### 🟡 1.7 Symbol

**Explanation:** Symbol creates a guaranteed unique value. Used for unique object keys that won't collide with other properties.

```javascript
const id = Symbol("id");
const id2 = Symbol("id");
console.log(id === id2);  // false (always unique)

const user = { name: "Alice", [id]: 123 };
Object.keys(user);        // ["name"] — Symbol keys are hidden
JSON.stringify(user);      // '{"name":"Alice"}' — Symbols excluded

// Shared symbols via registry
Symbol.for("app") === Symbol.for("app"); // true
```

**Interview Q: Why use Symbols?** 🟡
**A:** Unique property keys that won't collide with other code. Hidden from `for...in`, `Object.keys()`, `JSON.stringify()`. Used in libraries to add metadata without polluting the object.

**One-liner:** Symbol() = always unique. Symbol keys are hidden from iteration/JSON. Symbol.for() = shared via global registry.

---

### 🟡 1.8 BigInt

**Explanation:** JavaScript numbers lose precision beyond `2^53 - 1`. BigInt handles arbitrarily large integers by appending `n`.

```javascript
9007199254740993     // 9007199254740992 (WRONG — precision lost!)
9007199254740993n    // 9007199254740993n (CORRECT with BigInt)

10n + 20n            // 30n ✅
// 10n + 5           // ❌ TypeError — can't mix BigInt and number
10n + BigInt(5)      // 15n ✅
10n == 10            // true (loose)
10n === 10           // false (different types)
```

**Interview Q: When would you use BigInt?** 🟡
**A:** Database IDs from 64-bit systems, cryptographic calculations, financial calculations with very large numbers. Anytime integers exceed `Number.MAX_SAFE_INTEGER`.

**One-liner:** BigInt = integers beyond 2^53-1. Append `n`. Can't mix with number in operations. JSON.stringify doesn't support it.

---

### 🟡 1.9 Pass by Value vs Reference

**Explanation:** Primitives are passed by value (copy). Objects are passed by reference (shared pointer). Mutation inside a function affects the original; reassignment doesn't.

```javascript
// Value — safe
function change(x) { x = 100; }
let num = 5;
change(num);
console.log(num); // 5 (unchanged)

// Reference — mutation affects original
function addProp(obj) { obj.new = true; }
let myObj = { a: 1 };
addProp(myObj);
console.log(myObj); // { a: 1, new: true } (changed!)

// Reference — reassignment does NOT affect original
function replace(obj) { obj = { replaced: true }; }
replace(myObj);
console.log(myObj); // { a: 1, new: true } (unchanged — local reassignment only)
```

**Interview Q: What is the output?** 🔴
```javascript
function modify(arr) {
  arr.push(4);
  arr = [10, 20];
  arr.push(30);
}
let a = [1, 2, 3];
modify(a);
console.log(a);
```
**A:** `[1, 2, 3, 4]` — `push(4)` mutates original. `arr = [10, 20]` reassigns the LOCAL variable. `push(30)` affects the new local array, not `a`.

**One-liner:** Primitives = copy (safe). Objects = shared pointer. Mutation affects original. Reassignment only affects local variable.

---

### 🟡 1.10 Immutability of Primitives

**Explanation:** Primitive values can never be changed in place. When you "modify" a primitive, you're creating a new value and reassigning.

```javascript
let str = "hello";
str[0] = "H";        // silently fails — strings are immutable
console.log(str);     // "hello" (unchanged)
str = "Hello";        // works — new string assigned to variable

// String methods return NEW strings
let upper = str.toUpperCase();
console.log(str);     // "Hello" (original unchanged)
console.log(upper);   // "HELLO" (new string)
```

**Interview Q: What is the output?** 🟢
```javascript
let s = "hello";
s.toUpperCase();
console.log(s);
```
**A:** `"hello"` — `toUpperCase()` returns a new string, but it's not assigned to anything. The original is unchanged.

**One-liner:** Primitives are immutable. str[0] = "X" does nothing. String methods return NEW strings — always assign the result.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 2: Scope & Hoisting

---

### 🔴 2.1 Global, Function, and Block Scope

**Explanation:** Scope determines where a variable is accessible. Global = everywhere. Function = inside the function. Block = inside `{}` (if, for, while).

```javascript
var g = "global";  // accessible everywhere

function myFunc() {
  var f = "function-scoped";  // only inside myFunc
  if (true) {
    let b = "block-scoped";   // only inside this {}
    const c = "also block";   // only inside this {}
    var d = "function-scoped"; // leaks out of block (var ignores blocks!)
  }
  console.log(f);  // ✅
  console.log(d);  // ✅ (var leaks out of block)
  // console.log(b); // ❌ ReferenceError
}
```

**Interview Q: What is the difference between function scope and block scope?** 🔴
**A:** Function scope: variable accessible anywhere inside the function (`var`). Block scope: variable accessible only inside the `{}` where it's declared (`let`, `const`). `var` doesn't respect block scope — it leaks out of `if`, `for`, `while`.

**One-liner:** var = function-scoped (leaks out of blocks). let/const = block-scoped (stays in {}). Always prefer let/const.

---

### 🔴 2.2 Hoisting

**Explanation:** JavaScript moves declarations to the top of their scope before executing code. `var` is hoisted and initialized as `undefined`. `let`/`const` are hoisted but NOT initialized (Temporal Dead Zone). Functions declarations are fully hoisted.

```javascript
// var — hoisted as undefined
console.log(a);   // undefined
var a = 5;

// let/const — hoisted but in TDZ (Temporal Dead Zone)
// console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 5;

// Function declaration — fully hoisted (can call before definition)
greet();           // "Hello!" ✅
function greet() { console.log("Hello!"); }

// Function expression — NOT fully hoisted
// sayHi();        // TypeError: sayHi is not a function
var sayHi = function() { console.log("Hi!"); };
// sayHi is hoisted as undefined (var), calling undefined() throws TypeError
```

**Interview Q: What is the output?** 🔴
```javascript
var x = 1;
function foo() {
  console.log(x);
  var x = 2;
}
foo();
```
**A:** `undefined` — Local `var x` is hoisted inside `foo`, shadowing the global `x`. The hoisted `x` is `undefined` before the assignment.

**Mistake:** Thinking `let` isn't hoisted. It IS hoisted (the engine knows about it), but it's in the TDZ until the declaration line — accessing it throws ReferenceError.

**One-liner:** var hoisted as undefined. let/const hoisted in TDZ (error if accessed early). Function declarations fully hoisted. Expressions are NOT.

---

### 🔴 2.3 Temporal Dead Zone (TDZ)

**Explanation:** The TDZ is the period between a scope starting and the `let`/`const` declaration line. The variable exists (hoisted) but can't be accessed.

```javascript
{
  // TDZ starts for x
  // console.log(x); // ❌ ReferenceError
  // console.log(x); // ❌ still in TDZ
  let x = 10;       // TDZ ends, x is initialized
  console.log(x);   // 10 ✅
}

// Even typeof is not safe in TDZ
// console.log(typeof undeclaredVar); // "undefined" (safe for undeclared)
// console.log(typeof tdzVar);       // ❌ ReferenceError
// let tdzVar = 5;
```

**Interview Q: What is the output?** 🔴
```javascript
let a = 1;
{
  console.log(a);
  let a = 2;
}
```
**A:** `ReferenceError` — The inner block has its own `let a`, creating a TDZ from the start of the block. Even though outer `a = 1` exists, the inner `a` shadows it and is in TDZ at the point of `console.log`.

**One-liner:** TDZ = period between block start and let/const declaration. Variable exists but can't be accessed. Prevents use-before-declare bugs.

---

### 🔴 2.4 Lexical Scope

**Explanation:** JavaScript uses lexical (static) scope — a function can access variables from the scope where it was DEFINED, not where it was CALLED. This is the foundation of closures.

```javascript
const outer = "I'm outer";

function outerFunc() {
  const inner = "I'm inner";

  function innerFunc() {
    console.log(outer);  // ✅ can access (defined in parent's parent scope)
    console.log(inner);  // ✅ can access (defined in parent scope)
  }

  innerFunc();
}

outerFunc();
// inner is NOT accessible here — only inside outerFunc
```

**Interview Q: What is lexical scope?** 🔴
**A:** A function's scope is determined by WHERE it's written in the code, not where it's called. Inner functions can access outer variables (scope chain), but outer functions can't access inner variables.

**One-liner:** Lexical scope = access determined by where function is written, not called. Inner sees outer. Outer can't see inner.

---

### 🟡 2.5 Scope Chain

**Explanation:** When JavaScript looks up a variable, it starts in the current scope and walks up through parent scopes until it finds it or reaches the global scope. This chain of scopes is the scope chain.

```javascript
const global = "global";

function outer() {
  const outerVar = "outer";

  function middle() {
    const middleVar = "middle";

    function inner() {
      const innerVar = "inner";
      // Lookup chain: inner → middle → outer → global
      console.log(innerVar);   // found in inner scope
      console.log(middleVar);  // found in middle scope
      console.log(outerVar);   // found in outer scope
      console.log(global);     // found in global scope
    }
    inner();
  }
  middle();
}
outer();
```

**Interview Q: What happens if a variable isn't found in any scope?** 🟡
**A:** In strict mode: `ReferenceError`. In non-strict mode: if you ASSIGN to it (e.g., `x = 5`), JavaScript creates it as a global variable (accidental global — very bad!). If you READ it, `ReferenceError`.

**One-liner:** Scope chain = current scope → parent → grandparent → ... → global. Lookup stops when variable is found.

---

### 🟡 2.6 Variable Shadowing

**Explanation:** When a variable in an inner scope has the same name as a variable in an outer scope, the inner one "shadows" (hides) the outer one within that scope.

```javascript
let x = 10;

function example() {
  let x = 20;        // shadows the outer x
  console.log(x);    // 20 (inner x)
}

example();
console.log(x);      // 10 (outer x, unaffected)

// var can shadow let (within a function)
let y = 1;
function test() {
  var y = 2;         // shadows outer y (different scope)
  console.log(y);    // 2
}
```

**Interview Q: What is variable shadowing?** 🟡
**A:** When a variable declared in an inner scope has the same name as one in an outer scope. The inner variable "shadows" the outer one — the outer variable is inaccessible within that inner scope.

**One-liner:** Shadowing = inner variable with same name hides outer variable. Outer is unaffected, just unreachable inside the inner scope.

---

### 🟢 2.7 IIFE (Immediately Invoked Function Expression)

**Explanation:** A function that runs immediately after being defined. Was used before `let`/`const` to create private scope and avoid polluting the global namespace.

```javascript
// IIFE syntax
(function() {
  var secret = "hidden";
  console.log(secret);  // "hidden"
})();
// console.log(secret); // ❌ ReferenceError — secret is private

// Arrow function IIFE
(() => {
  const data = "private";
  console.log(data);
})();

// With return value
const result = (function() {
  return 42;
})();
console.log(result); // 42

// Classic use: avoid polluting global scope in old JS
// Modern JS: just use let/const + modules instead
```

**Interview Q: Why were IIFEs used and are they still relevant?** 🟢
**A:** Before `let`/`const` and modules, IIFEs created private scope to avoid global variable pollution. Today, modules (`import`/`export`) and block scoping (`let`/`const`) largely replace IIFEs. Still seen in legacy code and some library patterns.

**One-liner:** IIFE = function that runs immediately. `(function(){})()`. Created private scope before modules existed. Mostly legacy now.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 3: Functions

---

### 🔴 3.1 Function Declaration vs Expression

**Explanation:** Declarations are hoisted (can call before definition). Expressions are assigned to variables and follow that variable's hoisting rules.

```javascript
// DECLARATION — fully hoisted
greet();  // ✅ "Hello"
function greet() { console.log("Hello"); }

// EXPRESSION — NOT fully hoisted
// sayHi(); // ❌ TypeError (var) or ReferenceError (let/const)
const sayHi = function() { console.log("Hi"); };
sayHi();  // ✅ "Hi" (only works after this line)

// NAMED expression (useful for recursion and stack traces)
const factorial = function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1);  // fact is available inside
};
// fact(5); // ❌ fact is not available outside
```

**Interview Q: What is the difference between function declaration and expression?** 🔴
**A:** Declaration is hoisted — you can call it before it appears in code. Expression is NOT hoisted — you must define it before calling. Declarations use `function name(){}`. Expressions assign a function to a variable: `const name = function(){}`.

**One-liner:** Declaration = hoisted, call anywhere. Expression = not hoisted, call only after definition.

---

### 🔴 3.2 Arrow Functions vs Regular Functions

**Explanation:** Arrow functions are shorter syntax AND have different behavior for `this`, `arguments`, and constructors.

```javascript
// Syntax
const regular = function(a, b) { return a + b; };
const arrow = (a, b) => a + b;  // implicit return for single expression

// KEY DIFFERENCES:
// 1. this — arrow inherits from parent scope, regular has its own
const obj = {
  name: "Alice",
  regular: function() { console.log(this.name); },  // "Alice" ✅
  arrow: () => { console.log(this.name); },          // undefined ❌ (window.name)
};

// 2. arguments — arrow doesn't have its own
function reg() { console.log(arguments); }  // ✅ [1, 2, 3]
const arr = () => { console.log(arguments); }; // ❌ ReferenceError (or parent's arguments)

// 3. Constructor — arrow can't be used with new
// new arrow(); // ❌ TypeError: arrow is not a constructor

// 4. No prototype property
// arrow.prototype // undefined
```

| Feature | Regular | Arrow |
|---|---|---|
| `this` | Own (dynamic, depends on call) | Inherited from parent (lexical) |
| `arguments` | Has its own | No (use ...rest) |
| `new` | Can be constructor | Cannot |
| Hoisting | Declaration = yes | No (expression) |
| Syntax | Longer | Shorter |

**Interview Q: Why can't you use arrow functions as object methods?** 🔴
**A:** Arrow functions don't have their own `this`. In an object method, `this` should refer to the object, but an arrow function inherits `this` from the surrounding scope (usually `window` or `undefined` in strict mode).

**One-liner:** Arrow = no own this (inherits), no arguments, no new. Use for callbacks. Regular = own this. Use for methods and constructors.

---

### 🔴 3.3 Default, Rest, and Spread

**Explanation:** Default parameters set fallback values. Rest (`...`) collects remaining arguments into an array. Spread (`...`) expands an array/object into individual items.

```javascript
// DEFAULT parameters
function greet(name = "World") {
  console.log(`Hello, ${name}!`);
}
greet();          // "Hello, World!"
greet("Alice");   // "Hello, Alice!"

// REST parameters (...args collects into array)
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}
sum(1, 2, 3, 4);  // 10

// SPREAD (expands array/object)
const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4];  // [1, 2, 3, 4]

const obj1 = { a: 1 };
const obj2 = { ...obj1, b: 2 }; // { a: 1, b: 2 }

// Spread in function call
Math.max(...[5, 10, 3]);  // 10
```

**Interview Q: What is the difference between rest and spread?** 🔴
**A:** They use the same syntax `...` but opposite purposes. Rest COLLECTS multiple items INTO an array (in function parameters). Spread EXPANDS an array/object INTO individual items.

**One-liner:** Rest (...args) = collect into array. Spread (...arr) = expand into items. Same syntax, opposite direction.

---

### 🔴 3.4 Callbacks and Higher-Order Functions

**Explanation:** A callback is a function passed as an argument to another function. A higher-order function (HOF) is a function that takes or returns a function.

```javascript
// Callback
function doSomething(callback) {
  console.log("Doing work...");
  callback();  // call the passed function
}
doSomething(() => console.log("Done!"));
// "Doing work..." then "Done!"

// Higher-order functions — take a function as argument
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);      // [2, 4, 6, 8, 10]
const evens = numbers.filter(n => n % 2 === 0); // [2, 4]
const sum = numbers.reduce((acc, n) => acc + n, 0); // 15

// Higher-order function — returns a function
function multiplier(factor) {
  return (number) => number * factor;  // returns a function
}
const double = multiplier(2);
const triple = multiplier(3);
double(5);  // 10
triple(5);  // 15
```

**Interview Q: What is a higher-order function? Give examples.** 🔴
**A:** A function that takes a function as argument OR returns a function. Built-in examples: `map`, `filter`, `reduce`, `forEach`, `sort`, `setTimeout`. Custom: factory functions like `multiplier` above.

**One-liner:** Callback = function passed as argument. HOF = function that takes/returns a function. map, filter, reduce are HOFs.

---

### 🟡 3.5 Pure Functions

**Explanation:** A pure function always returns the same output for the same input and has no side effects (doesn't modify external variables, make API calls, or change DOM).

```javascript
// PURE — same input → same output, no side effects
function add(a, b) {
  return a + b;
}

// IMPURE — modifies external state
let total = 0;
function addToTotal(n) {
  total += n;  // side effect: changes external variable
  return total;
}

// IMPURE — depends on external state
function getGreeting() {
  const hour = new Date().getHours();  // depends on current time
  return hour < 12 ? "Good morning" : "Good afternoon";
}

// IMPURE — side effect: API call
function saveUser(user) {
  fetch('/api/users', { method: 'POST', body: JSON.stringify(user) });
}
```

**Interview Q: Why are pure functions important in React?** 🟡
**A:** React components should be pure during rendering — same props → same output. This lets React skip re-renders (memoization), makes behavior predictable, and enables features like concurrent rendering where React may call your component multiple times.

**One-liner:** Pure = same input → same output + no side effects. Predictable, testable, memoizable. React components should be pure.

---

### 🟡 3.6 Currying

**Explanation:** Currying transforms a function that takes multiple arguments into a series of functions, each taking one argument. Enables partial application and function composition.

```javascript
// Normal function
function add(a, b, c) { return a + b + c; }
add(1, 2, 3); // 6

// Curried version
function curriedAdd(a) {
  return function(b) {
    return function(c) {
      return a + b + c;
    };
  };
}
curriedAdd(1)(2)(3); // 6

// Arrow syntax (cleaner)
const curriedAdd2 = a => b => c => a + b + c;

// Partial application — "pre-fill" some arguments
const add5 = curriedAdd(5);      // a=5, waiting for b and c
const add5and3 = add5(3);        // b=3, waiting for c
add5and3(2);                      // c=2 → 5+3+2 = 10

// Real-world: create specialized functions
const log = (level) => (message) => console.log(`[${level}] ${message}`);
const error = log("ERROR");
const info = log("INFO");
error("Something broke");  // [ERROR] Something broke
info("Server started");    // [INFO] Server started
```

**Interview Q: What is currying and how is it useful?** 🟡
**A:** Currying converts `f(a, b, c)` into `f(a)(b)(c)`. Useful for creating specialized functions from general ones (partial application), building middleware, and function composition in functional programming.

**One-liner:** Currying = f(a,b,c) → f(a)(b)(c). Each call returns a new function until all args received. Enables partial application.

---

### 🟡 3.7 Recursion

**Explanation:** A function that calls itself to solve smaller sub-problems. Must have a base case to stop, otherwise it causes infinite recursion (stack overflow).

```javascript
// Factorial: 5! = 5 × 4 × 3 × 2 × 1
function factorial(n) {
  if (n <= 1) return 1;        // base case — stops recursion
  return n * factorial(n - 1);  // recursive call
}
factorial(5); // 120

// Fibonacci
function fib(n) {
  if (n <= 1) return n;         // base case
  return fib(n - 1) + fib(n - 2); // two recursive calls
}
fib(6); // 8 (0,1,1,2,3,5,8)

// Flatten nested array
function flatten(arr) {
  const result = [];
  for (const item of arr) {
    if (Array.isArray(item)) {
      result.push(...flatten(item));  // recurse into nested arrays
    } else {
      result.push(item);
    }
  }
  return result;
}
flatten([1, [2, [3, 4]], 5]); // [1, 2, 3, 4, 5]
```

**Interview Q: What happens without a base case?** 🟡
**A:** Infinite recursion → `RangeError: Maximum call stack size exceeded`. Each recursive call adds to the call stack. Without a stopping condition, the stack overflows.

**One-liner:** Recursion = function calls itself. Must have base case (stop condition). Without it → stack overflow.

---

### 🟢 3.8 Memoization

**Explanation:** Caching the results of expensive function calls. If the same input appears again, return the cached result instead of recalculating.

```javascript
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      console.log('Cache hit!');
      return cache.get(key);
    }
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
}

const expensiveCalc = memoize((n) => {
  console.log('Computing...');
  return n * n;
});

expensiveCalc(5);  // "Computing..." → 25
expensiveCalc(5);  // "Cache hit!" → 25 (no recomputation)
expensiveCalc(3);  // "Computing..." → 9
```

**Interview Q: Implement a memoize function.** 🟢
**A:** See code above. Key points: use a Map/object as cache, serialize arguments as the key, check cache before computing, store result after computing.

**One-liner:** Memoization = cache function results. Same input → return cached result. Avoids redundant expensive calculations.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 4: Closures

---

### 🔴 4.1 What is a Closure?

**Explanation:** A closure is a function that remembers the variables from the scope where it was created, even after that scope has finished executing. It "closes over" its lexical environment.

```javascript
function createCounter() {
  let count = 0;  // this variable is "closed over"

  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count,
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
// count is NOT accessible directly — it's private!
// console.log(count); // ❌ ReferenceError
```

**Interview Q: What is a closure? Give an example.** 🔴
**A:** A closure is a function that has access to variables from its outer (enclosing) function's scope, even after the outer function has returned. In the example above, `increment`, `decrement`, and `getCount` all close over the same `count` variable, keeping it alive and private.

**One-liner:** Closure = function that remembers variables from where it was defined, even after that scope exits. Enables private state.

---

### 🔴 4.2 Classic Closure Problem (Loop + var)

**Explanation:** The most asked closure interview question. `var` in a `for` loop shares one variable across all iterations. Closures created in the loop all reference the same variable.

```javascript
// ❌ THE PROBLEM
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3 (not 0, 1, 2!)
// Why: var has one shared i. By the time setTimeout fires, i is 3.

// ✅ FIX 1: Use let (each iteration gets its own i)
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 0, 1, 2

// ✅ FIX 2: IIFE (creates new scope per iteration)
for (var i = 0; i < 3; i++) {
  (function(j) {
    setTimeout(() => console.log(j), 100);
  })(i);
}
// Output: 0, 1, 2

// ✅ FIX 3: Bind or additional closure
for (var i = 0; i < 3; i++) {
  setTimeout(console.log.bind(null, i), 100);
}
```

**Interview Q: What is the output and how do you fix it?** 🔴
**A:** `3, 3, 3` — `var` creates one shared `i`. All three `setTimeout` callbacks close over the SAME `i`, which is `3` when they execute. Fix: use `let` (block-scoped per iteration) or IIFE (new scope per iteration).

**One-liner:** var loop + setTimeout = all share one variable. Fix: let (block-scoped per iteration) or IIFE.

---

### 🔴 4.3 Practical Uses of Closures

**Explanation:** Closures power many real-world patterns: data privacy, factory functions, event handlers, memoization, and module pattern.

```javascript
// 1. DATA PRIVACY — hide internal state
function createBankAccount(initial) {
  let balance = initial;  // private variable
  return {
    deposit: (amount) => { balance += amount; return balance; },
    withdraw: (amount) => { balance -= amount; return balance; },
    getBalance: () => balance,
  };
}
const account = createBankAccount(100);
account.deposit(50);     // 150
// account.balance;      // undefined — balance is private!

// 2. FACTORY FUNCTIONS — create specialized functions
function createMultiplier(x) {
  return (y) => x * y;
}
const double = createMultiplier(2);
const triple = createMultiplier(3);
double(5);  // 10
triple(5);  // 15

// 3. EVENT HANDLERS — remember data from setup time
function setupButton(buttonId, message) {
  document.getElementById(buttonId).addEventListener('click', () => {
    alert(message);  // message is closed over
  });
}

// 4. DEBOUNCE — closure remembers timeoutId between calls
function debounce(fn, delay) {
  let timeoutId;  // closed over
  return (...args) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn(...args), delay);
  };
}
```

**Interview Q: Give 3 real-world uses of closures.** 🔴
**A:** (1) Data privacy/encapsulation — private variables in modules, (2) Function factories — create specialized functions, (3) Maintaining state — debounce/throttle functions remember their timer between calls.

**One-liner:** Closures enable: private data, function factories, stateful callbacks, memoization, debounce/throttle, module pattern.

---

### 🟡 4.4 Closure Memory Leaks

**Explanation:** Closures keep their entire lexical environment alive. If a closure holds a reference to a large object or DOM element, it can't be garbage collected even when you don't need it.

```javascript
// ❌ POTENTIAL LEAK — closure keeps large data alive
function processData() {
  const hugeArray = new Array(1000000).fill('data');

  return function getLength() {
    return hugeArray.length;  // closes over hugeArray — it can't be freed
  };
}
const getLen = processData();  // hugeArray stays in memory forever

// ✅ FIX — only close over what you need
function processData() {
  const hugeArray = new Array(1000000).fill('data');
  const length = hugeArray.length;  // extract needed data

  return function getLength() {
    return length;  // only closes over a single number
  };
  // hugeArray can now be garbage collected
}
```

**Interview Q: Can closures cause memory leaks?** 🟡
**A:** Yes. A closure keeps all variables from its outer scope alive, even ones it doesn't use. If those variables reference large objects, they can't be garbage collected. Fix: null out unused references or only close over the specific data you need.

**One-liner:** Closures keep their entire outer scope alive. If it holds large data you don't need, set it to null or extract only what's needed.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 5: this Keyword

---

### 🔴 5.1 this in Different Contexts

**Explanation:** `this` depends on HOW a function is called, not where it's defined. It has different values in different contexts.

```javascript
// 1. GLOBAL — window (browser) or global (Node), undefined in strict mode
console.log(this);  // window (browser)

// 2. OBJECT METHOD — this = the object calling the method
const user = {
  name: "Alice",
  greet() { console.log(this.name); }
};
user.greet();  // "Alice" — this = user

// 3. REGULAR FUNCTION — this = window (or undefined in strict mode)
function standalone() { console.log(this); }
standalone();  // window (sloppy mode) / undefined (strict mode)

// 4. ARROW FUNCTION — this = inherited from parent scope (lexical this)
const obj = {
  name: "Bob",
  greet: () => { console.log(this.name); }  // this = parent (window)
};
obj.greet();  // undefined (not "Bob"!)

// 5. EVENT HANDLER — this = the element that fired the event
button.addEventListener('click', function() {
  console.log(this);  // the button element
});
button.addEventListener('click', () => {
  console.log(this);  // window! (arrow inherits from parent)
});

// 6. CLASS/CONSTRUCTOR — this = the new instance
class Person {
  constructor(name) { this.name = name; }
}
const p = new Person("Charlie"); // this = the new Person instance
```

**Interview Q: What is `this` in an arrow function?** 🔴
**A:** Arrow functions don't have their own `this`. They inherit `this` from the enclosing lexical scope (where the arrow function is defined). This makes them great for callbacks but bad for object methods.

**One-liner:** this = who calls the function. Object method → object. Regular function → window/undefined. Arrow → inherits from parent. new → instance.

---

### 🔴 5.2 call(), apply(), bind()

**Explanation:** Methods to explicitly set `this`. `call` and `apply` invoke immediately. `bind` returns a new function with `this` permanently set.

```javascript
function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = { name: "Alice" };

// call — invoke with explicit this + individual args
greet.call(person, "Hello", "!");     // "Hello, Alice!"

// apply — invoke with explicit this + args as array
greet.apply(person, ["Hello", "!"]);  // "Hello, Alice!"

// bind — returns NEW function with this permanently set
const greetAlice = greet.bind(person, "Hey");
greetAlice("!!");                     // "Hey, Alice!!"
greetAlice("?");                      // "Hey, Alice?"

// DIFFERENCE:
// call  → immediate execution, args as list:    fn.call(ctx, a, b)
// apply → immediate execution, args as array:   fn.apply(ctx, [a, b])
// bind  → returns new function, doesn't execute: fn.bind(ctx, a)
```

**Interview Q: What is the difference between call, apply, and bind?** 🔴

| Method | Executes? | Arguments | Returns |
|---|---|---|---|
| call | Immediately | Comma-separated | Function result |
| apply | Immediately | Array | Function result |
| bind | No (later) | Comma-separated (partial) | New function |

**Trick to remember:** call = Commas. apply = Array. bind = Binds for later.

**One-liner:** call(ctx, a, b) = invoke now with args. apply(ctx, [a,b]) = invoke now with array. bind(ctx) = return new function for later.

---

### 🟡 5.3 this in Classes

**Explanation:** In class methods, `this` refers to the instance. But when you extract a method (assign it to a variable or pass as callback), `this` is lost.

```javascript
class Timer {
  constructor() {
    this.seconds = 0;
  }

  start() {
    // ❌ PROBLEM — regular function loses this in setInterval
    setInterval(function() {
      this.seconds++;  // this = window, not Timer instance!
    }, 1000);

    // ✅ FIX 1: Arrow function (inherits this from start())
    setInterval(() => {
      this.seconds++;  // this = Timer instance ✅
    }, 1000);

    // ✅ FIX 2: bind
    setInterval(function() {
      this.seconds++;
    }.bind(this), 1000);
  }
}

// Method extraction problem
class Logger {
  constructor(name) { this.name = name; }
  log() { console.log(this.name); }
}

const logger = new Logger("App");
logger.log();           // "App" ✅

const fn = logger.log;  // extracted method
fn();                   // undefined ❌ (this = window)

const boundFn = logger.log.bind(logger);
boundFn();              // "App" ✅
```

**Interview Q: Why does `this` become undefined when you extract a class method?** 🟡
**A:** When you assign `const fn = obj.method`, `fn` is just a function reference — it loses the connection to `obj`. When called as `fn()`, `this` is `window` (or `undefined` in strict mode). Fix: use `bind`, arrow functions, or call `obj.method()` directly.

**One-liner:** Class methods lose `this` when extracted. Fix: bind(this), arrow functions, or always call as obj.method().

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 6: Objects & Prototypes

---

### 🔴 6.1 Object Creation and Destructuring

**Explanation:** Objects are key-value pairs. You can create them with literals, constructors, or `Object.create()`. Destructuring extracts properties into variables.

```javascript
// Creation
const obj = { name: "Alice", age: 25 };             // literal
const obj2 = new Object();                           // constructor (rare)
const obj3 = Object.create({ greet() { return "hi"; } }); // from prototype

// Destructuring
const { name, age } = obj;                 // extract properties
const { name: userName } = obj;            // rename: userName = "Alice"
const { name, ...rest } = obj;            // rest: { age: 25 }
const { address = "Unknown" } = obj;      // default value

// Nested destructuring
const user = { name: "Alice", address: { city: "Delhi" } };
const { address: { city } } = user;       // city = "Delhi"

// Shorthand properties
const x = 1, y = 2;
const point = { x, y };  // same as { x: x, y: y }

// Computed property names
const key = "color";
const config = { [key]: "red" };  // { color: "red" }
```

**Interview Q: What is destructuring with default values?** 🔴
```javascript
const { a = 10, b = 20 } = { a: 5 };
// a = 5 (exists, so default ignored)
// b = 20 (doesn't exist, default used)
```

**One-liner:** Object destructuring: `const { a, b } = obj`. Rename: `{ a: x }`. Default: `{ a = 5 }`. Nested: `{ a: { b } }`. Rest: `{ a, ...rest }`.

---

### 🔴 6.2 Shallow Copy vs Deep Copy

**Explanation:** Shallow copy copies one level — nested objects are still shared. Deep copy creates fully independent copies at all levels.

```javascript
const original = { name: "Alice", scores: [1, 2, 3] };

// SHALLOW — nested array is shared
const shallow = { ...original };
shallow.name = "Bob";           // safe (primitive)
shallow.scores.push(4);         // ⚠️ CHANGES original!
console.log(original.scores);   // [1, 2, 3, 4]

// DEEP — fully independent
const deep = structuredClone(original);  // modern (2022+)
deep.scores.push(5);
console.log(original.scores);   // [1, 2, 3, 4] (unchanged)

// Other deep copy methods:
const deep2 = JSON.parse(JSON.stringify(original));
// ⚠️ Fails with: functions, undefined, Date, RegExp, circular refs
```

**Interview Q: What is the difference between shallow and deep copy?** 🔴

| | Shallow | Deep |
|---|---|---|
| First level | Independent copy | Independent copy |
| Nested objects | Shared reference | Independent copy |
| Methods | `{...obj}`, `Object.assign` | `structuredClone`, `JSON.parse(JSON.stringify)` |

**One-liner:** Shallow ({...obj}) = one level. Deep (structuredClone) = all levels. Spread only copies the first level.

---

### 🔴 6.3 Object Methods

**Explanation:** Built-in methods for working with objects.

```javascript
const user = { name: "Alice", age: 25, role: "admin" };

// Get keys, values, entries
Object.keys(user);     // ["name", "age", "role"]
Object.values(user);   // ["Alice", 25, "admin"]
Object.entries(user);  // [["name","Alice"], ["age",25], ["role","admin"]]

// Freeze — make immutable (shallow)
Object.freeze(user);
user.name = "Bob";     // silently fails (strict mode: TypeError)
console.log(user.name); // "Alice" (unchanged)

// Seal — can't add/remove, but CAN modify existing
const config = { debug: false };
Object.seal(config);
config.debug = true;   // ✅ works (modifying existing)
config.newProp = 1;    // ❌ fails (can't add)

// Assign — merge objects (shallow)
const merged = Object.assign({}, { a: 1 }, { b: 2 }, { a: 3 });
// { a: 3, b: 2 } — later properties override earlier ones
```

**Interview Q: What is the difference between freeze and seal?** 🟡

| | freeze | seal |
|---|---|---|
| Add properties | ❌ | ❌ |
| Remove properties | ❌ | ❌ |
| Modify existing | ❌ | ✅ |
| Both are shallow | ✅ | ✅ |

**One-liner:** Object.keys/values/entries for iteration. freeze = fully immutable. seal = can modify but not add/remove. Both shallow.

---

### 🟡 6.4 Prototype Chain

**Explanation:** Every object has an internal link (`[[Prototype]]`) to another object. When you access a property, JavaScript looks up the chain: current object → prototype → prototype's prototype → ... → `Object.prototype` → `null`.

```javascript
const animal = {
  type: "animal",
  speak() { console.log(`I'm a ${this.type}`); }
};

const dog = Object.create(animal);  // dog's prototype = animal
dog.type = "dog";
dog.bark = function() { console.log("Woof!"); };

dog.bark();    // "Woof!" — found on dog itself
dog.speak();   // "I'm a dog" — found on prototype (animal)

// Checking the chain
dog.hasOwnProperty('bark');   // true (own property)
dog.hasOwnProperty('speak');  // false (inherited from prototype)
'speak' in dog;                // true (checks entire chain)

// The chain: dog → animal → Object.prototype → null
console.log(Object.getPrototypeOf(dog) === animal); // true
```

**Interview Q: What is the prototype chain?** 🟡
**A:** Every object has a prototype (another object). When you access a property, JS first checks the object itself. If not found, it checks the prototype, then the prototype's prototype, until reaching `Object.prototype` (whose prototype is `null`). This chain is how inheritance works in JavaScript.

**One-liner:** Prototype chain: object → prototype → prototype's prototype → Object.prototype → null. Property lookup walks this chain.

---

### 🟡 6.5 hasOwnProperty vs in

**Explanation:** `hasOwnProperty` checks ONLY the object itself. `in` checks the entire prototype chain.

```javascript
const parent = { inherited: true };
const child = Object.create(parent);
child.own = true;

child.hasOwnProperty('own');        // true
child.hasOwnProperty('inherited');  // false (it's on the prototype)

'own' in child;        // true
'inherited' in child;  // true (found in prototype chain)

// Safe iteration (own properties only)
for (const key in child) {
  if (child.hasOwnProperty(key)) {
    console.log(key);  // "own" only
  }
}
// Or use Object.keys() which returns only own properties
Object.keys(child);  // ["own"]
```

**Interview Q: How do you check if a property belongs to the object itself (not inherited)?** 🟡
**A:** `obj.hasOwnProperty('prop')` or `Object.hasOwn(obj, 'prop')` (newer, safer). `in` operator checks the entire prototype chain. `Object.keys()` only returns own enumerable properties.

**One-liner:** hasOwnProperty = own only. `in` = own + inherited. Object.keys() = own enumerable only.

---

### 🟢 6.6 Proxy and Reflect

**Explanation:** Proxy wraps an object and intercepts operations (get, set, delete, etc.) with custom behavior. Reflect provides default implementations of those operations.

```javascript
const user = { name: "Alice", age: 25 };

const proxy = new Proxy(user, {
  // Intercept property read
  get(target, prop) {
    console.log(`Reading ${prop}`);
    return prop in target ? target[prop] : `Property ${prop} not found`;
  },

  // Intercept property write
  set(target, prop, value) {
    if (prop === 'age' && typeof value !== 'number') {
      throw new TypeError('Age must be a number');
    }
    target[prop] = value;
    return true;
  },

  // Intercept property deletion
  deleteProperty(target, prop) {
    if (prop === 'name') {
      throw new Error("Can't delete name");
    }
    delete target[prop];
    return true;
  }
});

proxy.name;         // "Reading name" → "Alice"
proxy.unknown;      // "Reading unknown" → "Property unknown not found"
proxy.age = 30;     // ✅ works
// proxy.age = "thirty"; // ❌ TypeError: Age must be a number
```

**Interview Q: What are real-world uses of Proxy?** 🟢
**A:** Validation (type checking on assignment), default values for missing properties, logging/debugging property access, reactive systems (Vue 3 uses Proxy for reactivity), access control, and API design.

**One-liner:** Proxy intercepts object operations (get, set, delete). Enables validation, defaults, logging, and reactivity (Vue 3 uses it).

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Quick Revision — Part 1 One-Liners

```
VARIABLES & DATA TYPES
  var = function-scoped. let/const = block-scoped. const can't reassign but CAN mutate objects
  7 primitives: string, number, boolean, null, undefined, symbol, bigint
  Primitives = copy by value. Objects = copy by reference (shared pointer)
  typeof null = "object" (bug). typeof [] = "object" (use Array.isArray)
  == coerces types. === checks type + value. Always use ===
  + with string = concat. 7 falsy: false, 0, -0, "", null, undefined, NaN
  Symbol = unique. BigInt = large integers (append n)
  Mutation in function affects original. Reassignment doesn't
  Primitives are immutable. String methods return new strings

SCOPE & HOISTING
  var = function scope (leaks). let/const = block scope (stays in {})
  var hoisted as undefined. let/const in TDZ. Function declarations fully hoisted
  TDZ = between block start and let/const declaration — ReferenceError if accessed
  Lexical scope = where defined, not where called. Basis of closures
  Scope chain: current → parent → grandparent → global
  IIFE = (function(){})() — private scope, mostly legacy now

FUNCTIONS
  Declaration = hoisted. Expression = not hoisted
  Arrow: no this, no arguments, no new. Regular: own this
  Rest (...args) collects. Spread (...arr) expands
  Callback = function as argument. HOF = takes/returns functions
  Pure = same input → same output + no side effects
  Currying = f(a,b) → f(a)(b). Enables partial application
  Recursion needs base case. No base case = stack overflow
  Memoization = cache results. Same input → cached result

CLOSURES
  Closure = function remembers outer scope variables after scope exits
  var loop + setTimeout = 3,3,3. Fix: let or IIFE
  Uses: private data, factories, debounce, memoization, module pattern
  Can cause memory leaks if holding large unused data

THIS KEYWORD
  this = who calls the function. Object.method → object. Regular → window/undefined
  Arrow = inherits this from parent. No own this
  call(ctx, a, b) = invoke now. apply(ctx, [a,b]) = invoke now. bind(ctx) = return new fn
  Class methods lose this when extracted. Fix: bind or arrow

OBJECTS & PROTOTYPES
  Destructuring: { a, b } = obj. Rename: { a: x }. Default: { a = 5 }
  Shallow ({...obj}) = one level. Deep (structuredClone) = all levels
  freeze = fully immutable. seal = can modify, not add/remove
  Prototype chain: obj → prototype → ... → Object.prototype → null
  hasOwnProperty = own only. in = own + inherited
  Proxy = intercept get/set/delete. Powers Vue 3 reactivity
```

[↑ Back to Table of Contents](#table-of-contents--part-1)
