# TypeScript Complete Interview & Learning Guide
## Part 3: Categories 15–20 (Modules → Build Tools)
### For React Developers Who Know JavaScript

---

# Table of Contents — Part 3

- [Category 15: Modules & Declaration Files](#category-15-modules--declaration-files)
- [Category 16: Type Narrowing Deep Dive](#category-16-type-narrowing-deep-dive)
- [Category 17: tsconfig Deep Dive](#category-17-tsconfig-deep-dive)
- [Category 18: State Management Typing](#category-18-state-management-typing)
- [Category 19: Testing with TypeScript](#category-19-testing-with-typescript)
- [Category 20: Build Tools & Integration](#category-20-build-tools--integration)
- [Quick Revision — Part 3 One-Liners](#quick-revision--part-3-one-liners)
- [Master Quick Revision — All 20 Categories](#master-quick-revision--all-20-categories)

---

# Category 15: Modules & Declaration Files

---

### 🔴 15.1 ES Modules in TypeScript: import/export with Types

**Explanation:** TypeScript uses ES module syntax with one addition: `import type` and `export type` are compile-time only — erased completely, producing no runtime JavaScript. Tools like Vite (which use `isolatedModules`) require this distinction.

```typescript
// Regular imports (values + types bundled together)
import { useState, useEffect } from "react";

// Type-only imports — compile-time only, zero runtime output
import type { User, ApiResponse } from "./types";
import type { FC, ReactNode }     from "react";

// Named exports
export function formatDate(date: Date): string { return date.toISOString(); }
export const API_URL = "https://api.example.com";
export interface Config { timeout: number; retries: number }
export type UserId = string | number;

// Type-only exports
export type { User };
export type { User, ApiResponse };

// Barrel exports (index.ts) — bundle a folder
export { Button }           from "./Button";
export { Input }            from "./Input";
export type { ButtonProps } from "./Button";

// Clean single import for consumers
import { Button, Input }   from "@/components";
import type { ButtonProps } from "@/components";

// Why import type matters for Vite/SWC (isolatedModules):
// These tools process files one at a time — can't determine if an export
// is type-only or a real value without full analysis.
// import type { User } makes it explicit — always safe to erase.
```

**Interview Q: What is `import type` and why does Vite require it for type imports?** 🔴
**A:** `import type` imports only TypeScript type information — completely erased at compile time, producing zero JavaScript output. Vite uses `isolatedModules` mode (processes files one at a time without full type analysis). It can't determine if an import is type-only, so it may try to import it at runtime. `import type` makes the intent explicit — erase without analysis.

**Aha moment:** `import type` is free — it adds zero bytes to your bundle. There's no downside to using it for type imports, and it makes your intent clear to every tool in the pipeline.

**Common mistake:** Not using `import type` in Vite projects and getting "The requested module does not provide an export named 'X'" runtime errors for type-only imports.

**One-liner:** `import type { T }` = compile-time only, zero runtime. Required for Vite/SWC `isolatedModules`. Always use for type-only imports.

---

### 🔴 15.2 @types/* Packages — How DefinitelyTyped Works

**Explanation:** Many JavaScript libraries don't ship TypeScript types. The community maintains type definitions at DefinitelyTyped, published as `@types/packagename` npm packages. Always install as dev dependencies.

```bash
# Library without built-in types — need @types
npm install lodash
npm install --save-dev @types/lodash

npm install express
npm install --save-dev @types/express

# Libraries WITH built-in types — NO @types needed
# axios, zod, @tanstack/react-query, zustand, @reduxjs/toolkit
# These ship their own .d.ts files in the package

# How to check: look for "types" or "typings" in package.json
# Or: node_modules/packagename/index.d.ts
```

```typescript
// After @types/lodash — full type safety
import _ from "lodash";
const grouped = _.groupBy(users, u => u.role);
// grouped: { [key: string]: User[] } ✅

// Error without @types: "Could not find declaration file for module 'lodash'"
// Fix 1: npm install --save-dev @types/lodash
// Fix 2: create a local .d.ts declaration
// Fix 3: declare module "lodash" { ... } as a quick stub

// tsconfig: control which @types are included
{
  "compilerOptions": {
    "types": ["node", "jest", "@testing-library/jest-dom"]
    // Without this, ALL @types in node_modules included automatically
  }
}
```

**Interview Q: How do you add TypeScript support for a JavaScript library with no built-in types?** 🔴
**A:** Check if `@types/libraryname` exists on npm — install it as a dev dependency. If no `@types` package exists, write a minimal `.d.ts` declaration file. Modern libraries increasingly bundle their own TypeScript types — check the package's `typings` or `types` field in `package.json`.

**Aha moment:** `@types/react` is entirely `.d.ts` files — zero JavaScript. It's a pure type description of React, maintained separately from React itself.

**Common mistake:** Installing `@types/react` as a regular dependency instead of `--save-dev`. Type packages are compile-time only — always `devDependencies`.

**One-liner:** `npm install --save-dev @types/lib` adds types for JS libs. Modern libraries ship own types. `@types` = devDependency always.

---

### 🟡 15.3 Writing .d.ts Declaration Files

**Explanation:** Declaration files contain only type information — no implementation. Written for JavaScript libraries with no `@types` package, for global variables injected by build tools, and for typing static asset imports.

```typescript
// Scenario 1: typing an untyped JS module
// Create: src/types/some-legacy-lib.d.ts
declare module "some-legacy-lib" {
  export function doThing(value: string): number;
  export const version: string;
  export interface Config { timeout: number; retry: boolean }
}

// Now TypeScript knows the shape
import { doThing } from "some-legacy-lib";
doThing("hello"); // ✅ returns number

// Scenario 2: global variables injected by Vite/webpack
// src/types/globals.d.ts
declare const __ENV__:      "development" | "production" | "test";
declare const __VERSION__:  string;
declare const __BUILD_DATE__: string;

if (__ENV__ === "development") {
  console.log(`v${__VERSION__}`); // ✅ TypeScript knows about these
}

// Scenario 3: typing static asset imports
// src/types/assets.d.ts
declare module "*.svg" {
  import React from "react";
  const ReactComponent: React.FC<React.SVGProps<SVGSVGElement>>;
  export default ReactComponent;
}
declare module "*.png"  { const url: string; export default url; }
declare module "*.jpg"  { const url: string; export default url; }
declare module "*.css"  {
  const styles: Record<string, string>;
  export default styles;
}

// Now these work:
import Logo      from "./logo.svg";   // React.FC<SVGProps> ✅
import heroImage from "./hero.png";   // string ✅
```

**Interview Q: When do you need to write `.d.ts` files and what goes in them?** 🟡
**A:** Write `.d.ts` files when: (1) a JS library has no `@types` package, (2) your build tool injects global variables TypeScript doesn't know about, (3) you import non-JS assets (images, CSS, SVGs), or (4) you're publishing a TypeScript library for consumers. They contain only type declarations — no implementation code, zero runtime output.

**Aha moment:** `.d.ts` files are TypeScript's equivalent of C header files — they describe shapes without implementing them, letting the type system understand code it didn't compile.

**Common mistake:** Editing auto-generated `.d.ts` files in `dist/`. They're regenerated on every build — your edits vanish. Put custom declarations in `src/types/`.

**One-liner:** `.d.ts` = types only, no implementation. Write for untyped libs, global vars, asset imports. Never edit auto-generated ones.

---

### 🟡 15.4 Module Augmentation — Extending Third-Party Types

**Explanation:** Module augmentation adds properties to existing types from third-party packages without modifying their source. Essential for Express (adding `user` to `Request`) and for extending browser globals.

```typescript
// Extending Express Request
// src/types/express.d.ts
import "express"; // side-effect import — REQUIRED to augment, not replace

declare module "express-serve-static-core" {
  interface Request {
    user?:      AuthenticatedUser;
    requestId?: string;
  }
}

// Every Express handler now has req.user typed:
app.get("/profile", (req, res) => {
  if (req.user) {
    res.json({ name: req.user.name }); // ✅ TypeScript knows req.user shape
  }
});

// Extending Window global
// src/types/global.d.ts
interface Window {
  analytics: {
    track:    (event: string, data?: Record<string, unknown>) => void;
    identify: (userId: string) => void;
  };
}
window.analytics.track("page_view"); // ✅

// Extending React's JSX attributes (add data-* to all elements)
declare namespace React {
  interface HTMLAttributes<T> {
    "data-testid"?: string;
    "data-cy"?:     string;
  }
}
<button data-testid="submit-btn">Submit</button> // ✅ no TypeScript error
```

**Interview Q: What is module augmentation and how is it different from declaration merging?** 🟡
**A:** Both use `interface` redeclaration to add properties. The difference is scope: declaration merging works within the same TypeScript project scope. Module augmentation extends types from an external module by targeting it with `declare module "moduleName"`. The key requirement: you must have a real `import` from that module in the file (even a side-effect import) — otherwise TypeScript creates a new module declaration instead of augmenting the existing one.

**Aha moment:** Module augmentation has zero runtime impact — it's purely type information. You extend a type without forking the library or touching its source code.

**Common mistake:** Forgetting the side-effect import (`import "express"`). Without it, the `declare module` creates a brand new module declaration, replacing the existing types instead of adding to them.

**One-liner:** Module augmentation = extend third-party types without modifying source. Must `import "module"` first, then `declare module "module" { interface X { ... } }`.

---

### 🟡 15.5 Ambient Declarations and the declare Keyword

**Explanation:** The `declare` keyword tells TypeScript "this exists at runtime, I'm not creating it here." Used for environment-injected variables, global libraries loaded via script tags, and in `.d.ts` files.

```typescript
// declare var/let/const — value exists globally at runtime
declare const API_BASE: string;    // injected by Vite define
declare let __DEV__:    boolean;   // injected by webpack

// declare function — exists globally
declare function gtag(command: string, ...args: unknown[]): void; // Google Analytics

// declare namespace — global object (describes script-tag libraries)
declare namespace MyLib {
  function create(): MyLibInstance;
  interface MyLibInstance {
    destroy(): void;
    render(el: HTMLElement): void;
  }
}
MyLib.create().render(document.getElementById("app")!); // ✅

// Where TypeScript's built-in ambient declarations come from:
// "lib": ["DOM"] — declares window, document, fetch, HTMLElement, etc.
// "lib": ["ES2020"] — declares Promise, Map, Set, Array.flat, etc.
// Without "DOM" in lib: document.getElementById(...) → TypeScript error!

// NodeJS process.env — declared in @types/node via namespace augmentation
declare global {
  namespace NodeJS {
    interface ProcessEnv {
      NODE_ENV:     "development" | "production" | "test";
      DATABASE_URL: string;
      PORT?:        string;
    }
  }
}
process.env.NODE_ENV; // "development" | "production" | "test" ✅
```

**Interview Q: What does the `declare` keyword do in TypeScript?** 🟡
**A:** `declare` tells TypeScript that a value, function, class, or namespace exists at runtime but wasn't created by TypeScript. TypeScript trusts the declaration without verifying it. Used for environment globals (injected by build tools), CDN-loaded libraries, and in `.d.ts` files. When you use `window`, `document`, or `console` in TypeScript without importing them, you're relying on ambient `declare` statements from TypeScript's built-in `lib.dom.d.ts`.

**Aha moment:** Every browser global (`window`, `document`, `fetch`, `console`) is an ambient declaration in TypeScript's built-in DOM lib. They're all just `declare var window: Window` somewhere.

**Common mistake:** Using `declare` for values you've created in your own code. `declare` is only for externally-provided values — TypeScript won't verify the implementation exists.

**One-liner:** `declare` = "this exists at runtime but TypeScript didn't create it." Used for globals, script-tag libs, and build-injected constants.

---

### 🟢 15.6 Namespaces (and Why to Avoid Them)

**Explanation:** TypeScript namespaces were the pre-module way to organize code. Modern TypeScript uses ES modules instead. Namespaces still appear in `.d.ts` files for global libraries and NodeJS type augmentation.

```typescript
// Old pattern (avoid in new application code)
namespace Validation {
  export interface StringValidator {
    isAcceptable(s: string): boolean;
  }
  export class LettersOnly implements StringValidator {
    isAcceptable(s: string): boolean {
      return /^[A-Za-z]+$/.test(s);
    }
  }
}
const v = new Validation.LettersOnly(); // must prefix with namespace name

// Modern replacement — use ES modules
// validators.ts
export interface StringValidator { isAcceptable(s: string): boolean }
export class LettersOnly implements StringValidator {
  isAcceptable(s: string): boolean { return /^[A-Za-z]+$/.test(s); }
}
// No prefix needed — just import
import { LettersOnly } from "./validators";

// Where namespaces STILL appear: .d.ts for global libs + @types/node
declare namespace NodeJS {
  interface ProcessEnv {
    NODE_ENV: "development" | "production" | "test";
    DATABASE_URL?: string;
  }
}
// process.env.NODE_ENV: "development" | "production" | "test" ✅

// Also used by @types/node, @types/jest, etc. for global type augmentation
declare namespace jest {
  interface Matchers<R> {
    toBeWithinRange(floor: number, ceiling: number): R;
  }
}
```

**Interview Q: What is the difference between TypeScript namespaces and ES modules?** 🟢
**A:** Namespaces compile to self-executing functions and global objects — TypeScript's legacy module system. ES modules use `import`/`export` — the JavaScript standard. In all new code, use ES modules. Namespaces still appear in `.d.ts` files for global libraries and for augmenting `NodeJS.ProcessEnv`, `jest.Matchers`, etc. Never use namespaces to organize new application code.

**Aha moment:** `@types/node` uses `declare namespace NodeJS { interface ProcessEnv { ... } }` — that's where typed `process.env` comes from. Namespaces survive in `.d.ts` files even though they're legacy in application code.

**Common mistake:** Using namespaces to organize React components instead of ES module imports/exports. In any modern bundler setup, use ES modules.

**One-liner:** Namespaces = legacy code organization. ES modules in all new code. Namespaces only in `.d.ts` for global library descriptions.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 16: Type Narrowing Deep Dive

---

### 🔴 16.1 Control Flow Analysis

**Explanation:** TypeScript traces every execution path and narrows types based on checks already made. After `if (x === null) return`, TypeScript knows `x` is not null for all remaining code. Fully automatic — no extra syntax required.

```typescript
function processInput(input: string | number | null | undefined): string {
  // Type: string | number | null | undefined

  if (input === null || input === undefined) {
    return "empty";
  }
  // After early return: string | number (null + undefined eliminated)

  if (typeof input === "number") {
    return input.toFixed(2); // input: number ✅
  }
  // After typeof check: only string remains

  return input.toUpperCase(); // input: string ✅
}

// Narrowing through assignments
function demo(value: string | number) {
  let result: string;
  if (typeof value === "string") {
    result = value.toUpperCase(); // value: string ✅
  } else {
    result = value.toFixed(2);    // value: number ✅
  }
  return result; // TypeScript knows result is always assigned
}

// Narrowing in loops
const items: Array<string | null> = getItems();
for (const item of items) {
  if (!item) continue;       // skip nulls/empty strings
  item.toUpperCase();        // ✅ item is string here
}

// Narrowing through try/catch
async function loadUser(id: number): Promise<User | null> {
  try {
    return await fetchUser(id); // User ✅
  } catch {
    return null;                // null ✅
  }
}
```

**Interview Q: What is TypeScript's control flow analysis?** 🔴
**A:** TypeScript traces execution paths and narrows types based on checks already made. After `if (x === null) return`, TypeScript removes `null` from `x`'s type for all subsequent code. This works through early returns, throw statements, loops, assignments, and switch cases — TypeScript builds a complete picture of what's possible at each line. No extra annotations needed.

**Aha moment:** TypeScript "reads" your if statements and understands what they prove. After `if (!user) return null`, TypeScript knows `user` is defined for the rest of the function — automatically.

**Common mistake:** Pulling a narrowed variable outside its narrowing scope. `if (x !== null) { /* x is string here */ } x.toUpperCase(); // ❌ x is back to string | null`

**One-liner:** TypeScript automatically narrows types through every code path — if/return/throw/assignment. No extra syntax. Narrow once, use freely after.

---

### 🔴 16.2 Truthy / Falsy Narrowing

**Explanation:** JavaScript's seven falsy values narrow types in boolean contexts. TypeScript understands `if (x)`, `!x`, `!!x`, and `Boolean(x)` — but watch out for valid `0` and `""` values being eliminated.

```typescript
// if (x) narrows away falsy types
function process(value: string | null | undefined) {
  if (value) {
    // value: string (null and undefined are falsy — eliminated)
    value.toUpperCase(); // ✅
  }
}

// if (!x) for early return / guard clauses
function requireString(value: string | null): string {
  if (!value) throw new Error("Value required");
  return value.toUpperCase(); // value: string ✅
}

// Falsy removes: false, 0, -0, 0n, "", null, undefined, NaN
// if (x) eliminates all of these from the type

// ⚠️ The 0 and "" pitfall — these may be valid values!
function display(count: number | null) {
  if (count) {
    console.log(count); // ⚠️ count = 0 is ALSO skipped! (0 is falsy)
  }
  // Fix: check for null specifically
  if (count !== null) {
    console.log(count); // ✅ 0 is included now
  }
}

// !! doesn't narrow as well as explicit checks
const hasUser: boolean = !!maybeUser; // converts, doesn't narrow type

// Boolean() in filter doesn't narrow properly
const values = ["a", null, "b", undefined, "c"];
const bad  = values.filter(Boolean);           // (string | null | undefined)[]
const good = values.filter((v): v is string => Boolean(v)); // string[] ✅
```

**Interview Q: What is the danger of truthy narrowing with numbers and empty strings?** 🔴
**A:** `if (count)` skips `0` because `0` is falsy — even though `0` is a valid number. Same with `""` — `if (name)` skips empty strings that may be intentional. For numbers that can be `0` (pagination, scores, indices), always use explicit checks: `if (count !== null)` or `count != null`. Truthy narrowing is safest for reference types (objects, arrays) where falsy means "not present."

**Aha moment:** `if (x)` is NOT the same as `if (x !== null && x !== undefined)`. The former also removes `0`, `""`, and `false`. In React, this causes silent `0` render bugs: `{count && <Badge />}` renders `0` when count is 0.

**Common mistake:** `if (count)` for numeric state where `0` is a valid value. Common React bug: `{items.length && <List />}` renders `0` when array is empty. Use `{items.length > 0 && <List />}`.

**One-liner:** Truthy `if (x)` removes null/undefined/0/""/false/NaN. Beware: 0 and "" may be valid. Use `!== null` for numbers/strings that can be falsy-valid.

---

### 🔴 16.3 Equality Narrowing

**Explanation:** TypeScript narrows based on equality checks. Most important: `=== null` removes null, `!= null` removes BOTH null and undefined. Switch cases narrow to their literal value.

```typescript
// === null narrows away null only
function processA(value: string | null | undefined) {
  if (value === null) {
    return "null"; // value: null
  }
  // value: string | undefined (null removed, undefined remains)
}

// !== undefined removes undefined only
function processB(x: string | undefined) {
  if (x !== undefined) {
    x.toUpperCase(); // x: string ✅
  }
}

// != null (loose) removes BOTH null AND undefined
function processC(x: string | null | undefined) {
  if (x != null) {    // loose != removes both
    x.toUpperCase();  // x: string ✅
  }
}
// x != null ≡ x !== null && x !== undefined
// This is the ONE acceptable use of == / != in TypeScript

// Switch narrows to each literal value
type Status = "active" | "inactive" | "pending";
function describe(status: Status): string {
  switch (status) {
    case "active":   return "Running";  // status: "active"
    case "inactive": return "Stopped"; // status: "inactive"
    case "pending":  return "Waiting"; // status: "pending"
  }
}

// Discriminated union narrowing via === check
type Shape =
  | { kind: "circle"; radius: number }
  | { kind: "square"; side: number };

function area(s: Shape): number {
  if (s.kind === "circle") {
    return Math.PI * s.radius ** 2; // s: { kind: "circle", radius: number } ✅
  }
  return s.side ** 2; // s: { kind: "square", side: number } ✅
}
```

**Interview Q: What does `x != null` check in TypeScript and why is it useful?** 🔴
**A:** `x != null` (loose inequality) is `true` when `x` is both `null` and `undefined` — it's one of the few acceptable uses of `==`/`!=`. `x !== null` (strict) only removes `null`, leaving `undefined` in the type. `x != null` removes both in a single concise check. TypeScript understands this and narrows accordingly — the idiomatic "check that a value is actually present."

**Aha moment:** `value != null` is not a typo. It's a deliberate choice: one check to remove both null AND undefined. Very common in TypeScript codebases — the idiomatic nullish guard.

**Common mistake:** Using `if (value !== null)` when the value could also be `undefined` — TypeScript still includes `undefined` after the check. Use `value != null` to handle both in one line.

**One-liner:** `=== null` removes null only. `!= null` removes BOTH null AND undefined (the one acceptable ==). Switch cases narrow to their literal.

---

### 🟡 16.4 Discriminated Unions — Deep Narrowing

**Explanation:** When a union's discriminant property is checked, TypeScript narrows to the exact variant with that discriminant value — giving full access to all properties of that specific type.

```typescript
// Multi-variant discriminated union
type ApiEvent =
  | { kind: "request";  method: "GET" | "POST"; url: string }
  | { kind: "response"; statusCode: number; body: unknown }
  | { kind: "error";    code: string; message: string; retryable: boolean };

function handleEvent(event: ApiEvent): void {
  switch (event.kind) {
    case "request":
      // event: { kind: "request", method: "GET"|"POST", url: string }
      console.log(`${event.method} ${event.url}`); // method + url available ✅
      break;
    case "response":
      // event: { kind: "response", statusCode: number, body: unknown }
      console.log(event.statusCode, event.body);   // statusCode + body ✅
      break;
    case "error":
      // event: { kind: "error", code: string, message: string, retryable: boolean }
      if (event.retryable) retry();                // retryable ✅
      break;
  }
}

// React component — discriminated state drives rendering
type ViewState =
  | { state: "loading" }
  | { state: "error";  message: string }
  | { state: "data";   users: User[]; totalCount: number };

function UserView({ viewState }: { viewState: ViewState }) {
  if (viewState.state === "loading") return <Spinner />;
  if (viewState.state === "error")   return <Error msg={viewState.message} />;
  // TypeScript narrows to data variant here
  return <UserList users={viewState.users} total={viewState.totalCount} />; // ✅
}
```

**Interview Q: How does TypeScript narrow discriminated union types so precisely?** 🟡
**A:** When TypeScript sees `event.kind === "request"`, it finds all union members where `kind` is the literal `"request"` — narrows to exactly those members. Since each discriminant value uniquely identifies one variant, TypeScript collapses the union to a single type, giving full access to all of that variant's properties without assertions.

**Aha moment:** Discriminated unions make impossible states unrepresentable. TypeScript won't let you access `event.body` in the "request" case — only the "response" variant has `body`.

**Common mistake:** Accessing properties before the discriminant check. `event.statusCode` outside a `kind === "response"` check fails — TypeScript doesn't know which variant you have.

**One-liner:** Discriminant check narrows to exact variant. After `kind === "response"`, ALL of that variant's properties are available — no assertions needed.

---

### 🟡 16.5 Type Predicates — User-Defined Type Guards

**Explanation:** A function with a `value is Type` return type (type predicate) narrows the type at the call site when it returns true. More powerful than a regular boolean check — the predicate communicates what was proved.

```typescript
// Type predicate — the "value is Type" return type is the key
function isString(value: unknown): value is string {
  return typeof value === "string";
}

// vs a regular boolean
function checkString(value: unknown): boolean {
  return typeof value === "string";
}

function process(value: unknown) {
  if (isString(value)) {
    value.toUpperCase(); // ✅ narrowed to string (type predicate worked)
  }
  if (checkString(value)) {
    value.toUpperCase(); // ❌ still unknown (regular boolean = no narrowing)
  }
}

// Interface type guard
interface User { id: number; name: string; email: string }

function isUser(val: unknown): val is User {
  return (
    typeof val === "object" && val !== null &&
    typeof (val as User).id === "number" &&
    typeof (val as User).name === "string"
  );
}

const data: unknown = await fetch("/api/user").then(r => r.json());
if (!isUser(data)) throw new Error("Invalid API response");
data.name; // ✅ User — narrowed after the guard

// ── The most practical use: typed .filter() ────────────────
const items: (string | null)[] = ["Alice", null, "Bob", null];

// Without predicate — TypeScript doesn't narrow the result
const bad  = items.filter(x => x !== null);
// bad: (string | null)[] — TypeScript doesn't know nulls were removed!

// With predicate — result is correctly typed
const good = items.filter((x): x is string => x !== null);
// good: string[] ✅
```

**Interview Q: What is a type predicate and why is it necessary for `.filter()`?** 🟡
**A:** A type predicate is a `value is Type` return annotation on a function. When it returns `true`, TypeScript narrows the caller's type. For `.filter()`, without a predicate, TypeScript can't narrow — `filter(x => x !== null)` still returns `(T | null)[]`. The predicate `(x): x is string => x !== null` tells TypeScript the filtered array has nulls removed, giving `string[]`. One annotation, properly typed result.

**Aha moment:** The difference between `(x) => x !== null` and `(x): x is string => x !== null` is one small annotation — but it's the difference between a correctly typed `string[]` and a useless `(string | null)[]`.

**Common mistake:** Writing `: boolean` instead of `: value is Type` as the return type. Without the predicate syntax, TypeScript treats it as a regular boolean — no narrowing happens.

**One-liner:** `(value): value is T => ...` = type predicate. Narrows call site. Essential for `.filter()` returning correctly typed arrays. Regular `: boolean` doesn't narrow.

---

### 🟡 16.6 Assertion Functions

**Explanation:** Assertion functions use `asserts value is T` as a return type. If they don't throw, TypeScript narrows the type for ALL subsequent code — not just inside a branch. Throw if invalid; narrow if valid.

```typescript
// Assertion function — narrows from the call point forward
function assertIsString(value: unknown): asserts value is string {
  if (typeof value !== "string") {
    throw new TypeError(`Expected string, got ${typeof value}`);
  }
}

function process(value: unknown) {
  assertIsString(value); // throws if not string
  // Everything AFTER this line: value is string ✅
  value.toUpperCase(); // ✅
  value.split(",");    // ✅
  return value.length; // ✅
}

// asserts condition (without is) — general assertion
function assert(condition: unknown, message?: string): asserts condition {
  if (!condition) throw new Error(message ?? "Assertion failed");
}

const maybeUser: User | null = getUser();
assert(maybeUser !== null, "User must be logged in");
maybeUser.name; // ✅ null removed, maybeUser is User

// asserts vs type guard
// Type guard:         if (isString(x)) { /* x is string INSIDE */ }
// Assertion function: assertIsString(x); // x is string FROM HERE FORWARD

// React: validate required config at app startup
function assertEnvVar(value: string | undefined, name: string): asserts value is string {
  if (!value) throw new Error(`Required env var ${name} is not set`);
}

assertEnvVar(process.env.DATABASE_URL, "DATABASE_URL");
process.env.DATABASE_URL; // string ✅ (not string | undefined)
```

**Interview Q: How do assertion functions differ from type guard functions?** 🟡
**A:** Type guards return `boolean` with a predicate — they narrow inside an `if` branch only. Assertion functions return `void` with `asserts x is T` — if they don't throw, TypeScript narrows the type for all code following the call, throughout the whole function scope. Use type guards for conditional branching; use assertion functions when invalid input should throw and you want narrowing to persist without extra `if` wrapping.

**Aha moment:** Assertion functions "thread" narrowing forward — after `assertIsString(x)`, narrowing persists for the rest of the function without any `if` block.

**Common mistake:** Using assertion functions where a type guard is more appropriate. If you want to branch on a type, use a guard. If you want to throw on invalid input and continue with a narrow type, use an assertion.

**One-liner:** `asserts value is T` = throw if not T, narrow everything after the call. Type guard = narrow inside if-block only. Assertion = narrow from here forward.

---

### 🟢 16.7 The satisfies Operator as Narrowing

**Explanation:** `satisfies` validates a value against a type at the point of declaration without widening the inferred type — it narrows at the source while preserving specific literal types.

```typescript
// Without satisfies — annotation widens types
const routes: Record<string, string> = { home: "/", about: "/about" };
routes.home;    // string (wide)
routes.unknown; // ✅ no error — any string key accepted

// With satisfies — validates shape + keeps literal key types
const routes = {
  home:  "/",
  about: "/about",
} satisfies Record<string, string>;
routes.home;    // string ✅ (key "home" known to exist)
routes.unknown; // ❌ Error: "unknown" not a key

// Narrows the discriminant in place
const myCircle = {
  kind: "circle" as const,
  radius: 5,
} satisfies Shape;
myCircle.radius; // ✅ TypeScript knows it's the circle variant

// Theme validation — validate without losing literal types
type Theme = { primary: string; secondary: string };
const darkTheme = {
  primary:   "#1a1a2e",
  secondary: "#16213e",
  extra:     "#0f3460", // ❌ Error: 'extra' not in Theme — validation works!
} satisfies Theme;
// primary type: "#1a1a2e" (literal) not "string" (wide annotation)
```

**Interview Q: How does `satisfies` combine validation with type preservation?** 🟢
**A:** `satisfies T` validates that a value matches type T at the declaration point (wrong properties = error) but doesn't change the inferred type to T. The value keeps its specific inferred types (literal strings, exact object keys). This combines the validation benefit of annotation (catches invalid structure) with the precision benefit of inference (keeps specific types). "Validate but don't widen."

**Aha moment:** Annotation = "make this type T, I accept the widening." `satisfies` = "validate against T, but keep my specific types." The difference matters when you need both safety and specificity.

**Common mistake:** Using `satisfies` on function parameters — it only works at declaration/assignment points, not on parameters.

**One-liner:** `satisfies T` = validate at declaration + keep specific types. Annotation widens to T; `satisfies` validates without widening.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 17: tsconfig Deep Dive

---

### 🔴 17.1 target and lib

**Explanation:** `target` controls which JavaScript version is emitted. `lib` controls which built-in type definitions are available. They're independent — you can target ES5 while using ES2020 lib types.

```json
{
  "compilerOptions": {
    "target": "ES2020",
    // ES5     = broadest compat (IE11), more transpilation
    // ES2020  = async/await, optional chaining, nullish coalescing native
    // ESNext  = latest — use with bundlers that handle compatibility

    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    // ES2020       = Promise, Map, Set, Array.flat, Object.entries
    // DOM          = window, document, HTMLElement, fetch
    // DOM.Iterable = NodeList.forEach, HTMLCollection iteration
    // ES2020.Promise = Promise.allSettled, Promise.any
  }
}
```

```typescript
// target affects WHAT IS EMITTED:
// target: ES5  — TypeScript compiles async/await → generator functions
// target: ES2017 — async/await kept as-is (native in ES2017+)

// lib affects WHAT TYPES ARE AVAILABLE (not emitted code):
// Without "DOM" in lib:
document.getElementById("app"); // ❌ Error: 'document' not found
// Without "ES2020" in lib:
Promise.allSettled([p1, p2]);   // ❌ Error: 'allSettled' not found on Promise

// Common combinations:
// Browser React app:    ["ES2020", "DOM", "DOM.Iterable"]
// Node.js app:          ["ES2020"] + @types/node for Node globals
// Library (no DOM):     ["ES2020"] — don't assume browser environment
```

**Interview Q: What is the difference between `target` and `lib` in tsconfig?** 🔴
**A:** `target` determines the JavaScript syntax emitted — lower targets mean more transpilation (arrow functions → regular functions). `lib` determines which built-in type definitions TypeScript includes for type-checking — it doesn't affect emitted code at all. Setting `lib: ["ES2020"]` doesn't add polyfills; it just tells TypeScript these APIs exist for type-checking purposes.

**Aha moment:** `lib` is purely about what TypeScript considers valid. If you include `ES2020` in `lib` but target `ES5`, TypeScript won't error on `Promise.allSettled` calls — but the code may fail in old browsers without runtime polyfills.

**Common mistake:** Forgetting `"DOM"` in `lib` for browser projects — TypeScript won't recognize `window`, `document`, `fetch`, or any DOM API without it.

**One-liner:** `target` = emitted JS version. `lib` = type definitions available (no output effect). Set both — they're independent.

---

### 🔴 17.2 strict and Its Sub-flags

**Explanation:** `strict: true` enables eight safety flags in one setting. Each can be enabled individually for gradual migration. The most impactful is `strictNullChecks`.

```json
{
  "compilerOptions": {
    "strict": true,
    // Enables all of these simultaneously:

    "strictNullChecks": true,
    // null/undefined are separate types — MOST IMPORTANT
    // Prevents "cannot read properties of null" crashes

    "noImplicitAny": true,
    // Untyped parameters become errors — no silent any

    "strictFunctionTypes": true,
    // Function parameter types checked correctly (contravariantly)

    "strictBindCallApply": true,
    // call/bind/apply arguments are type-checked

    "strictPropertyInitialization": true,
    // Class properties must be initialized in constructor

    "noImplicitThis": true,
    // 'this' in regular functions must be explicitly typed

    "alwaysStrict": true,
    // Emits 'use strict' in all output files

    "useUnknownInCatchVariables": true,
    // catch(e) is 'unknown' not 'any'

    // Extra flags NOT in strict (add separately):
    "noImplicitReturns": true,        // all code paths must return
    "noFallthroughCasesInSwitch": true, // switch must break/return
    "noUncheckedIndexedAccess": false, // arr[0] = T | undefined (aggressive)
  }
}
```

**Interview Q: What does `strictNullChecks` do and why is it the most impactful strict flag?** 🔴
**A:** `strictNullChecks` makes `null` and `undefined` separate types — not assignable to `string`, `number`, or any other type. TypeScript forces you to explicitly handle the "value might be absent" case. Without it, TypeScript silently allows null everywhere and provides almost no protection against the most common runtime error class. Every other strict flag is valuable, but this one prevents the most bugs.

**Aha moment:** With `strictNullChecks` off, TypeScript is basically JavaScript with syntax highlighting. With it on, you get real type safety for the most common bugs.

**Common mistake:** Enabling `noUncheckedIndexedAccess` without understanding the impact — `arr[0]` becomes `T | undefined` everywhere, requiring undefined checks on every array access. Enable it separately and fix intentionally.

**One-liner:** `strict: true` = 8 flags. Most critical: `strictNullChecks`. Add `noImplicitReturns` and `noFallthroughCasesInSwitch` separately.

---

### 🔴 17.3 module and moduleResolution

**Explanation:** `module` sets the module format in output. `moduleResolution` controls how TypeScript resolves imports. For Vite React projects, use `"ESNext"` + `"bundler"`.

```json
{
  "compilerOptions": {
    "module": "ESNext",
    // CommonJS  = require/module.exports (Node.js, Jest)
    // ESNext    = import/export          (modern bundlers, Vite)
    // NodeNext  = ESM + CJS in Node 16+
    // Preserve  = keep as written       (bundler handles it)

    "moduleResolution": "bundler",
    // node    = classic Node.js resolution
    // node16  = Node 16+ with package exports
    // bundler = Vite/webpack style (required for Vite + path aliases)

    "esModuleInterop": true,
    // Allows: import React from 'react'   (default import from CJS)
    // Without: import * as React from 'react' (namespace import required)

    "allowSyntheticDefaultImports": true,
    // Usually auto-enabled by esModuleInterop
    // Allows default imports from modules without default exports
  }
}
```

```typescript
// esModuleInterop: false (old way)
import * as React from "react";  // ✅ required namespace import
import React from "react";       // ❌ Error without esModuleInterop

// esModuleInterop: true (modern)
import React from "react";       // ✅ clean default import
import { useState } from "react"; // ✅ named imports unchanged
```

**Interview Q: What is `esModuleInterop` and why does most React code need it?** 🔴
**A:** CommonJS modules (like React pre-v18) export via `module.exports`, not `export default`. Without `esModuleInterop`, TypeScript requires namespace imports (`import * as React from 'react'`). With it, TypeScript allows default import syntax (`import React from 'react'`) by adding interop helpers. Nearly all modern React TypeScript projects enable this for cleaner import syntax.

**Aha moment:** The reason you write `import React from 'react'` instead of `import * as React from 'react'` is entirely because of `esModuleInterop: true`.

**Common mistake:** Using `"moduleResolution": "node"` in a Vite project. Vite requires `"bundler"` for proper path alias resolution and TypeScript 5+ features.

**One-liner:** `module: ESNext` + `moduleResolution: bundler` for Vite. `esModuleInterop: true` = clean default imports. All three needed together.

---

### 🟡 17.4 paths, baseUrl, and rootDir

**Explanation:** `baseUrl` sets the root for non-relative imports. `paths` maps import aliases. Must be mirrored in Vite's `resolve.alias` — TypeScript and the bundler are separate tools.

```json
{
  "compilerOptions": {
    "rootDir": "./src",
    "outDir": "./dist",
    "baseUrl": ".",
    "paths": {
      "@/*":          ["./src/*"],
      "@components/*": ["./src/components/*"],
      "@hooks/*":     ["./src/hooks/*"],
      "@utils/*":     ["./src/utils/*"]
    }
  }
}
```

```typescript
// vite.config.ts — mirror the tsconfig paths here
import { defineConfig } from "vite";
import path from "path";

export default defineConfig({
  resolve: {
    alias: {
      "@":           path.resolve(__dirname, "./src"),
      "@components": path.resolve(__dirname, "./src/components"),
      "@hooks":      path.resolve(__dirname, "./src/hooks"),
      "@utils":      path.resolve(__dirname, "./src/utils"),
    }
  }
});

// Before path aliases
import { Button } from "../../../components/ui/Button";  // messy

// After path aliases
import { Button } from "@/components/ui/Button";         // clean ✅
import { useAuth } from "@hooks/useAuth";                // semantic ✅
```

**Interview Q: Why do path aliases need configuration in both tsconfig AND Vite?** 🟡
**A:** tsconfig `paths` teaches TypeScript's type checker to resolve imports for type safety and autocomplete. Vite's `resolve.alias` teaches the bundler to resolve the same imports at build time. They're completely separate tools — TypeScript and Vite don't share configuration. Both are required: if only tsconfig is set, types work but the build fails at runtime. If only Vite is set, the build works but TypeScript shows errors.

**Aha moment:** When you add a path alias and autocomplete works but the page crashes at runtime — that's missing the Vite alias. When TypeScript shows errors but the page runs — that's missing the tsconfig paths.

**Common mistake:** Keeping them out of sync. When you add a new alias in tsconfig, immediately add it to Vite too. Treat them as a pair.

**One-liner:** `paths` in tsconfig = TypeScript resolves it. `alias` in Vite = bundler resolves it. Both required, must stay in sync.

---

### 🟡 17.5 declaration, sourceMap, and emitDeclarationOnly

**Explanation:** These flags control what TypeScript emits alongside JavaScript. `declaration` is for library authors; `sourceMap` is for debugging in DevTools.

```json
{
  "compilerOptions": {
    // For publishing a LIBRARY:
    "declaration": true,
    // Generates .d.ts files — library consumers get TypeScript types
    // Without: "no type information" for your library's users

    "declarationMap": true,
    // Generates .d.ts.map — "Go to Definition" shows your .ts source
    // Great DX for library consumers debugging into your code

    "emitDeclarationOnly": true,
    // Only emit .d.ts, no .js output
    // Use when bundler (Vite/Rollup) handles JS, tsc handles types

    // For DEBUGGING in any project:
    "sourceMap": true,
    // Generates .js.map — DevTools shows .ts source during debugging
    // Essential for any project you need to debug

    "inlineSourceMap": false,
    // Embeds source maps in .js files (larger files, simpler setup)
  }
}
```

**Interview Q: When do you need `declaration: true` in tsconfig?** 🟡
**A:** When publishing a TypeScript package as a library. `declaration: true` generates `.d.ts` files alongside your JS — these are the type definitions library consumers get. Without it, users of your library see no TypeScript types. For application projects (not libraries), you don't need `declaration` — only the final JS bundle matters. Add `declarationMap: true` for better "Go to Definition" DX.

**Aha moment:** `declaration: true` is what lets libraries skip the `@types/` package for their consumers — the library itself ships `.d.ts` files generated by `tsc`.

**Common mistake:** Setting `declaration: true` in an application project. It clutters your `dist/` directory with `.d.ts` files nobody will use. Only needed for published libraries.

**One-liner:** `declaration: true` = emit `.d.ts` for library consumers. `sourceMap: true` = TypeScript source in DevTools. Apps need sourceMap; libraries need declaration.

---

### 🟢 17.6 Project References and Composite Builds

**Explanation:** Project references split large TypeScript codebases into sub-projects. TypeScript rebuilds only changed packages — incremental compilation for monorepos.

```json
// Root tsconfig.json
{ "references": [
    { "path": "./packages/shared" },
    { "path": "./packages/frontend" },
    { "path": "./packages/backend" }
] }

// packages/shared/tsconfig.json
{
  "compilerOptions": {
    "composite":   true,  // Required for referenced projects
    "declaration": true,  // Must emit .d.ts for others to consume
    "outDir":      "./dist"
  }
}
```

```bash
# Build with project references — rebuilds only changed packages
npx tsc --build       # alias: npx tsc -b

# Clean all build outputs
npx tsc --build --clean

# Force full rebuild
npx tsc --build --force
```

**Interview Q: What are TypeScript project references and when would you use them?** 🟢
**A:** Project references split a codebase into sub-projects with explicit dependency relationships. TypeScript tracks which changed and only rebuilds those. Use them in monorepos where multiple packages share TypeScript code — e.g., shared types between frontend and backend. For a single-app project, they add complexity (`composite`, `declaration`, `.tsbuildinfo`) with no benefit.

**Aha moment:** Large monorepos without project references compile everything on every change. With references, only changed packages and their dependents rebuild — can be 10x faster.

**Common mistake:** Adding project references to a single-package project. The overhead (`composite: true`, extra `.d.ts` generation, `.tsbuildinfo` files) adds complexity with zero speed benefit at that scale.

**One-liner:** Project references = incremental builds in monorepos. Only compile what changed. `composite: true` required in each referenced sub-project.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 18: State Management Typing

---

### 🔴 18.1 Typing Zustand Stores

**Explanation:** Zustand is TypeScript-friendly by design. Define a store interface, pass it to `create<StoreType>()`. The entire store — state and actions — is fully typed. Selectors are automatically typed from the interface.

```typescript
import { create } from "zustand";

// Define the full store shape — state + actions together
interface UserStore {
  users:      User[];
  loading:    boolean;
  error:      string | null;
  fetchUsers:  () => Promise<void>;
  addUser:     (user: User) => void;
  removeUser:  (id: number) => void;
  clearError:  () => void;
}

const useUserStore = create<UserStore>((set, get) => ({
  users:   [],
  loading: false,
  error:   null,

  fetchUsers: async () => {
    set({ loading: true, error: null });
    try {
      const users = await typedFetch<User[]>("/api/users");
      set({ users, loading: false });
    } catch (e) {
      set({ error: getErrorMessage(e), loading: false });
    }
  },

  addUser:    (user)  => set(state => ({ users: [...state.users, user] })),
  removeUser: (id)    => set(state => ({ users: state.users.filter(u => u.id !== id) })),
  clearError: ()      => set({ error: null }),
}));

// Usage — fully typed
function UserList() {
  const users    = useUserStore(state => state.users);    // User[] ✅
  const loading  = useUserStore(state => state.loading);  // boolean ✅
  const fetchAll = useUserStore(state => state.fetchUsers); // () => Promise<void> ✅

  React.useEffect(() => { fetchAll(); }, [fetchAll]);

  return loading
    ? <Spinner />
    : <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}
```

**Interview Q: How do you type a Zustand store with both state and async actions?** 🔴
**A:** Define an interface that includes both state properties and action method signatures. Pass it as the type parameter: `create<StoreInterface>((set, get) => ({ ... }))`. TypeScript validates that your implementation satisfies the interface. The `set` function receives typed state, and selectors are typed from the interface automatically — `state => state.users` knows `users` is `User[]`.

**Aha moment:** With Zustand + TypeScript, selectors are fully typed without any annotation. `(state) => state.count` — TypeScript knows `state` is your full interface and `state.count` is `number`.

**Common mistake:** Defining state and actions in separate interfaces then intersecting them. Zustand's `create<T>` works best with a single flat interface — simpler and equally typed.

**One-liner:** `create<StoreInterface>((set) => ({ ... }))` — full interface typed. Selectors auto-typed. State + actions in one interface.

---

### 🔴 18.2 Typing Redux Toolkit (RTK)

**Explanation:** Redux Toolkit is TypeScript-first. `createSlice` infers action types from reducers. Derive `RootState` and `AppDispatch` from the store — never write these manually — and export typed hooks.

```typescript
import { createSlice, createAsyncThunk, configureStore } from "@reduxjs/toolkit";
import type { PayloadAction } from "@reduxjs/toolkit";
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";

// State type
interface UsersState { list: User[]; loading: boolean; error: string | null }
const initialState: UsersState = { list: [], loading: false, error: null };

// Async thunk: <ReturnType, ArgType, {rejectValue: ErrorType}>
const fetchUsers = createAsyncThunk<User[], void, { rejectValue: string }>(
  "users/fetchAll",
  async (_, { rejectWithValue }) => {
    try { return await typedFetch<User[]>("/api/users"); }
    catch (e) { return rejectWithValue(getErrorMessage(e)); }
  }
);

const usersSlice = createSlice({
  name: "users",
  initialState,
  reducers: {
    addUser:    (state, action: PayloadAction<User>)   => { state.list.push(action.payload); },
    removeUser: (state, action: PayloadAction<number>) => {
      state.list = state.list.filter(u => u.id !== action.payload);
    },
  },
  extraReducers: builder => {
    builder
      .addCase(fetchUsers.pending,   state => { state.loading = true; })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.loading = false;
        state.list    = action.payload; // action.payload: User[] ✅
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.loading = false;
        state.error   = action.payload ?? "Unknown"; // action.payload: string ✅
      });
  },
});

// ── Derive types from store — NEVER write these manually ──
const store = configureStore({ reducer: { users: usersSlice.reducer } });

export type RootState  = ReturnType<typeof store.getState>; // { users: UsersState }
export type AppDispatch = typeof store.dispatch;            // typed for thunks

// ── Export typed hooks — use EVERYWHERE in components ─────
export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;

// Usage
function UserList() {
  const users   = useAppSelector(s => s.users.list);    // User[] ✅
  const loading = useAppSelector(s => s.users.loading); // boolean ✅
  const dispatch = useAppDispatch();
  return <button onClick={() => dispatch(fetchUsers())}>Load</button>;
}
```

**Interview Q: What are `RootState` and `AppDispatch` in RTK and why derive them from the store?** 🔴
**A:** `RootState = ReturnType<typeof store.getState>` is the full Redux state tree type — used with `useSelector`. `AppDispatch = typeof store.dispatch` is the dispatch type that knows about async thunks. Both are derived, not manually written — add a new slice and `RootState` automatically includes it. Using the untyped `useSelector`/`useDispatch` gives you `unknown` state and untyped dispatch.

**Aha moment:** `RootState` and `AppDispatch` are the most important lines in an RTK TypeScript setup. They're never manually written — derived from the store and always up to date automatically.

**Common mistake:** Using `useSelector` and `useDispatch` directly instead of the typed wrappers. The untyped versions miss type safety for selectors and thunk dispatch.

**One-liner:** Derive `RootState` + `AppDispatch` from the store. Export `useAppSelector` + `useAppDispatch` typed wrappers. Use everywhere. Never manually write these types.

---

### 🟡 18.3 Context + useReducer Pattern

**Explanation:** Context combined with `useReducer` and discriminated union actions gives you typed state management without external libraries. Every `dispatch()` call is validated at compile time.

```typescript
interface CartState { items: CartItem[]; total: number }

type CartAction =
  | { type: "ADD_ITEM";    item: CartItem }
  | { type: "REMOVE_ITEM"; itemId: string }
  | { type: "UPDATE_QTY";  itemId: string; quantity: number }
  | { type: "CLEAR" };

function cartReducer(state: CartState, action: CartAction): CartState {
  switch (action.type) {
    case "ADD_ITEM":    return { ...state, items: [...state.items, action.item] };
    case "REMOVE_ITEM": return { ...state, items: state.items.filter(i => i.id !== action.itemId) };
    case "UPDATE_QTY":
      return {
        ...state,
        items: state.items.map(i =>
          i.id === action.itemId ? { ...i, quantity: action.quantity } : i
        ),
      };
    case "CLEAR":       return { items: [], total: 0 };
  }
}

interface CartContextValue { state: CartState; dispatch: React.Dispatch<CartAction> }
const CartContext = React.createContext<CartContextValue>(null!);

function CartProvider({ children }: { children: React.ReactNode }) {
  const [state, dispatch] = useReducer(cartReducer, { items: [], total: 0 });
  return <CartContext.Provider value={{ state, dispatch }}>{children}</CartContext.Provider>;
}

function useCart() {
  const ctx = React.useContext(CartContext);
  if (!ctx) throw new Error("useCart must be inside CartProvider");
  return ctx;
}

// Every dispatch is validated — missing payload = TypeScript error
function Item({ item }: { item: CartItem }) {
  const { dispatch } = useCart();
  return (
    <button onClick={() => dispatch({ type: "REMOVE_ITEM", itemId: item.id })}>
      Remove
    </button>
    // ❌ dispatch({ type: "UPDATE_QTY", itemId: item.id }) — missing quantity!
  );
}
```

**Interview Q: What are the TypeScript advantages of `useReducer` + Context over scattered `useState` calls?** 🟡
**A:** Every `dispatch()` call is validated at compile time — missing payloads are TypeScript errors. The state shape is one typed interface instead of scattered individual states. Context distributes both state and dispatch with full type information. Adding a new action type: define it in the union, TypeScript errors at every dispatch and every reducer case that doesn't handle it.

**Aha moment:** The discriminated union action type is the key — TypeScript validates that `dispatch({ type: "UPDATE_QTY" })` includes `itemId` and `quantity`. Forget either and it's a compile error.

**Common mistake:** Typing dispatch as `React.Dispatch<any>` to avoid writing the full action union. This removes all dispatch safety — every call is unchecked.

**One-liner:** `useReducer` + discriminated union actions + Context = typed state management, no library. Every dispatch payload validated at compile time.

---

### 🟡 18.4 Typing React Query State

**Explanation:** React Query has first-class TypeScript support. Use typed query key factories with `as const`, and type queries with `useQuery<TData, TError>` generics.

```typescript
// Typed query key factory — centralized + as const for precise types
const QUERY_KEYS = {
  users:  (filters?: UserFilters) => ["users", filters] as const,
  user:   (id: number)            => ["users", id]      as const,
  posts:  (userId: number)        => ["users", userId, "posts"] as const,
} as const;

// useQuery<TData, TError>
function useUsers(filters?: UserFilters) {
  return useQuery<User[], ApiError>({
    queryKey: QUERY_KEYS.users(filters),
    queryFn:  () => typedFetch<User[]>("/api/users"),
    staleTime: 5 * 60 * 1000,
  });
}

// useMutation<TData, TError, TVariables>
function useCreateUser() {
  const queryClient = useQueryClient();
  return useMutation<User, ApiError, CreateUserInput>({
    mutationFn: input => typedFetch<User>("/api/users", {
      method: "POST", body: JSON.stringify(input)
    }),
    onSuccess: newUser => {  // newUser: User ✅
      queryClient.invalidateQueries({ queryKey: QUERY_KEYS.users() });
      queryClient.setQueryData(QUERY_KEYS.user(newUser.id), newUser);
    },
  });
}

// Usage
function UserList() {
  const { data: users, isLoading, error } = useUsers();
  // users: User[] | undefined ✅   error: ApiError | null ✅

  const create = useCreateUser();
  // create.mutate: (input: CreateUserInput) => void ✅

  if (isLoading) return <Spinner />;
  return <ul>{users?.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}
```

**Interview Q: How do typed query keys improve TypeScript safety in React Query?** 🟡
**A:** `as const` on query key arrays makes TypeScript know the exact tuple type — `["users", 1]` not `(string | number)[]`. When using `setQueryData`, TypeScript can validate the data type matches the query's expected shape. A centralized `QUERY_KEYS` factory prevents key typos and makes invalidation patterns consistent and refactor-safe.

**Aha moment:** `queryClient.setQueryData(QUERY_KEYS.user(id), user)` is type-safe when keys are typed — TypeScript validates `user` matches the query's data type.

**Common mistake:** Scattering raw array query keys (`["users", id]`) throughout the codebase — no centralization, easy to make typos that silently miss cache invalidation.

**One-liner:** `QUERY_KEYS` factory with `as const`. `useQuery<Data, Error>` + `useMutation<Data, Error, Variables>`. Typed keys enable safe `setQueryData` and invalidation.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 19: Testing with TypeScript

---

### 🔴 19.1 Jest + TypeScript Setup

**Explanation:** Jest doesn't understand TypeScript natively — it needs a transformer. Options: `ts-jest` (type-checks tests, slower), `babel-jest` with `@babel/preset-typescript` (fast, no type-check), or Vitest (TypeScript-native, recommended for Vite projects).

```bash
# Option A: Vitest (recommended for Vite — zero config TypeScript)
npm install --save-dev vitest @vitest/ui jsdom
# Add to package.json: "test": "vitest", "test:ui": "vitest --ui"

# Option B: ts-jest (Jest with TypeScript type-checking)
npm install --save-dev ts-jest @types/jest
npx ts-jest config:init
```

```typescript
// jest.config.ts (with ts-jest)
import type { Config } from "jest";

const config: Config = {
  preset:          "ts-jest",
  testEnvironment: "jsdom",     // or "node" for non-React tests
  moduleNameMapper: {
    "^@/(.*)$":           "<rootDir>/src/$1",    // mirror tsconfig paths
    "^@components/(.*)$": "<rootDir>/src/components/$1",
  },
};
export default config;

// tsconfig.test.json (separate from main tsconfig)
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "module": "CommonJS",  // Jest requires CommonJS
    "types":  ["jest", "@testing-library/jest-dom"]
  }
}

// vitest.config.ts (Vite projects)
import { defineConfig } from "vitest/config";
export default defineConfig({
  test: {
    environment: "jsdom",
    globals:     true,
    setupFiles:  ["./src/test/setup.ts"],
  },
});
```

**Interview Q: What is the fastest way to add TypeScript support to a Jest test suite?** 🔴
**A:** For Vite projects, switch to Vitest — TypeScript-native, same Jest API, much faster, zero extra configuration. For existing Jest setups: `ts-jest` with `preset: "ts-jest"` is the straightforward path. Critical: add `"module": "CommonJS"` in your test tsconfig — Jest runs in Node.js and requires CommonJS, not ESNext modules.

**Aha moment:** Vitest is the modern drop-in replacement for Jest in Vite projects. Same `describe`, `it`, `expect` API, but TypeScript works out of the box with no configuration.

**Common mistake:** Forgetting `"module": "CommonJS"` in the test tsconfig. Jest can't run ES modules (`import/export`) natively — you get "SyntaxError: Cannot use import statement" without this.

**One-liner:** Vite → Vitest (zero config, TypeScript native). Jest → `ts-jest` preset. Always set `"module": "CommonJS"` in test tsconfig.

---

### 🔴 19.2 Typing Tests — describe, it, expect

**Explanation:** With `@types/jest` or Vitest's global types, all test functions are typed. Type your test data to catch discrepancies when application types change.

```typescript
import { describe, it, expect, beforeEach } from "vitest"; // or use globals

describe("formatCurrency", () => {
  it("formats USD correctly", () => {
    const result: string = formatCurrency(1234.56, "USD");
    expect(result).toBe("$1,234.56");
  });
});

// Typed test data — catches when User type changes
describe("createUser", () => {
  const validInput: CreateUserInput = {  // TypeScript validates this shape
    name:  "Alice",
    email: "alice@example.com",
    role:  "user",
  };

  it("returns correct shape", async () => {
    const user: User = await createUser(validInput);
    expect(user.id).toBeTypeOf("number");
    expect(user.name).toBe(validInput.name);
  });

  it("rejects invalid email", async () => {
    const badInput: CreateUserInput = { ...validInput, email: "bad" };
    await expect(createUser(badInput)).rejects.toThrow(ValidationError);
  });
});

// Component testing
describe("Button", () => {
  it("calls onClick when clicked", async () => {
    const handleClick = vi.fn(); // vi.Mock type — knows it's a mock
    const user = userEvent.setup();

    render(<Button label="Click" onClick={handleClick} />);
    await user.click(screen.getByRole("button"));

    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

**Interview Q: How does typing test data improve test resilience to type changes?** 🔴
**A:** When `CreateUserInput` adds a required field, every test that creates a `CreateUserInput` without that field becomes a TypeScript error. Tests that don't care about the new field still work with factory defaults. Typed test data means stale tests that test the wrong API shape are caught at compile time — not discovered when they silently pass against a changed API.

**Aha moment:** TypeScript errors in test files when you refactor a type = TypeScript catching tests that test the old API. That's the goal — tests should verify current behavior.

**Common mistake:** Using `as CreateUserInput` to cast incomplete objects in tests. This bypasses the type check — stale test data won't be caught when the type changes.

**One-liner:** Type test data with the actual types. TypeScript errors in tests = tests that test old API. Never `as T` to cast incomplete test objects.

---

### 🔴 19.3 Mocking with TypeScript

**Explanation:** Typed mocks let TypeScript validate that mock return values match the real function's return type. `vi.mocked(fn)` gives you typed access to mock methods.

```typescript
import { vi, expect, describe, it } from "vitest";

// Basic typed mock
const mockSave = vi.fn((user: User) => Promise.resolve(user));
mockSave({ id: 1, name: "Alice", email: "a@test.com" }); // ✅
mockSave({ id: "wrong" });                               // ❌ TypeScript error

// Mocking modules
vi.mock("./api", () => ({
  fetchUser: vi.fn(),
  saveUser:  vi.fn(),
}));

import { fetchUser } from "./api";

// vi.mocked() — gives typed access to mock methods
const mockedFetchUser = vi.mocked(fetchUser);
// mockedFetchUser: MockedFunction<typeof fetchUser>

mockedFetchUser.mockResolvedValue({
  id: 1, name: "Alice", email: "a@test.com"
});
// TypeScript validates: this object must match fetchUser's return type ✅

// ❌ Wrong return type — TypeScript error:
mockedFetchUser.mockResolvedValue({ wrong: "shape" });

// Spying on class methods
const service = new UserService();
const spy = vi.spyOn(service, "getUser");
spy.mockResolvedValue({ id: 1, name: "Mock", email: "m@test.com" });

// Asserting calls
expect(mockedFetchUser).toHaveBeenCalledWith(1);
expect(mockedFetchUser).toHaveBeenCalledTimes(1);

// Mock implementation
mockedFetchUser.mockImplementation(async (id: number) => ({
  id, name: `User ${id}`, email: `u${id}@test.com`,
})); // return type validated ✅
```

**Interview Q: How does `vi.mocked()` help with TypeScript-typed mocks?** 🔴
**A:** After `vi.mock("./module")`, TypeScript still treats the imports as their original (non-mock) types. `vi.mocked(fn)` casts the function to `MockedFunction<typeof fn>` — which extends the original type with mock-specific methods (`.mockResolvedValue`, `.mockImplementation`, etc.). This gives you type-checked mock values — `mockResolvedValue` with a wrong User shape is a TypeScript error.

**Aha moment:** `vi.mocked(fetchUser).mockResolvedValue(...)` validates the value matches `fetchUser`'s actual return type. Your mocks are as type-safe as your production code.

**Common mistake:** Not using `vi.mocked()` and calling mock methods on the original function type — TypeScript shows `.mockResolvedValue is not a function` on the type (works at runtime but loses type safety).

**One-liner:** `vi.mocked(fn)` = typed mock with `.mockResolvedValue()` etc. Return value is validated against the real function's return type.

---

### 🟡 19.4 Testing React Components with RTL

**Explanation:** React Testing Library with TypeScript provides typed queries, typed `userEvent`, and typed render. A typed `renderWithProviders` wrapper eliminates boilerplate and provides consistent provider setup.

```typescript
import { render, screen } from "@testing-library/react";
import userEvent              from "@testing-library/user-event";
import { vi, describe, it, expect } from "vitest";

// Typed render wrapper — one file, used by all tests
function renderWithProviders(
  ui: React.ReactElement,
  options?: { initialUser?: User | null }
) {
  return render(ui, {
    wrapper: ({ children }) => (
      <QueryClientProvider client={new QueryClient({ defaultOptions: {
        queries: { retry: false } // disable retries in tests
      }})}>
        <AuthProvider initialUser={options?.initialUser ?? null}>
          {children}
        </AuthProvider>
      </QueryClientProvider>
    ),
  });
}

describe("LoginForm", () => {
  it("submits with valid credentials", async () => {
    const mockLogin = vi.fn<[LoginCredentials], Promise<void>>();
    const user = userEvent.setup();

    renderWithProviders(<LoginForm onLogin={mockLogin} />);

    await user.type(screen.getByRole("textbox", { name: /email/i }), "a@test.com");
    await user.type(screen.getByLabelText(/password/i), "password123");
    await user.click(screen.getByRole("button", { name: /sign in/i }));

    expect(mockLogin).toHaveBeenCalledWith({
      email: "a@test.com", password: "password123"
    } satisfies LoginCredentials); // satisfies validates the shape ✅
  });
});
```

**Interview Q: Why use `satisfies LoginCredentials` in a `toHaveBeenCalledWith` assertion?** 🟡
**A:** `satisfies LoginCredentials` validates that the expected call argument object matches the `LoginCredentials` type. If `LoginCredentials` adds a required field and the test doesn't include it, TypeScript errors on the assertion — the test becomes a type-checked specification of what the function should receive. Without `satisfies`, plain object literals in assertions don't benefit from TypeScript validation.

**Aha moment:** Type checking extends into test assertions when you use `satisfies`. Your test expectations become typed specifications of function call contracts.

**Common mistake:** Not creating a `renderWithProviders` wrapper — manually wrapping every test with providers creates massive boilerplate and is hard to update when providers change.

**One-liner:** `renderWithProviders` wrapper = consistent typed provider setup. `satisfies T` in assertions = type-checked expected values.

---

### 🟡 19.5 Typed Test Factories

**Explanation:** Test factories create typed test data with sensible defaults, letting tests override only the fields they care about. The factory is the single update point when types change.

```typescript
// Factory with Partial<T> overrides — type-safe defaults
function createMockUser(overrides: Partial<User> = {}): User {
  return {
    id:        Math.floor(Math.random() * 1000),
    name:      "Test User",
    email:     "test@example.com",
    role:      "user",
    createdAt: new Date().toISOString(),
    ...overrides, // TypeScript validates overrides are valid User fields
  };
}

// Tests only specify what matters
describe("CartService", () => {
  it("applies 10% discount for admin users", () => {
    const admin = createMockUser({ role: "admin" }); // override only role ✅
    // TypeScript validates: "admin" is a valid role value
    expect(getDiscount(admin)).toBe(0.1);
  });

  it("applies no discount for regular users", () => {
    const regular = createMockUser(); // all defaults ✅
    expect(getDiscount(regular)).toBe(0);
  });
});

// Builder pattern for complex objects
class UserBuilder {
  private user: User = createMockUser();

  withRole(role: User["role"]): this { // User["role"] = indexed access!
    this.user = { ...this.user, role };
    return this;
  }
  withEmail(email: string): this {
    this.user = { ...this.user, email };
    return this;
  }
  build(): Readonly<User> { return Object.freeze({ ...this.user }); }
}

const admin = new UserBuilder()
  .withRole("admin")
  .withEmail("admin@company.com")
  .build();
```

**Interview Q: Why use `Partial<T>` for test factory overrides?** 🟡
**A:** `Partial<T>` makes all properties optional in the overrides parameter, so tests only specify what they care about — everything else gets sensible defaults from the factory. When `User` adds a required field, only the factory needs updating — all tests continue working unless they specifically care about that field. Without factories, every test that creates a `User` object must be updated.

**Aha moment:** When you add `createdBy: number` to `User` and only one factory function needs updating (not 50 test files) — that's the value. Centralized typed defaults.

**Common mistake:** Using `as User` to cast incomplete objects in test data. TypeScript no longer validates the shape — stale tests pass silently when types change.

**One-liner:** `createMockFactory(overrides: Partial<T>): T` = typed defaults + override only what matters. Single update point when types change.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 20: Build Tools & Integration

---

### 🔴 20.1 TypeScript with Vite

**Explanation:** Vite uses esbuild to strip TypeScript types (extremely fast, no type-checking) and optionally `tsc --noEmit` for type checking. The dev server starts instantly; type-checking happens separately.

```bash
# Create Vite + React + TypeScript
npm create vite@latest my-app -- --template react-ts
cd my-app && npm install
```

```typescript
// vite.config.ts — type-safe with defineConfig
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import path from "path";

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      "@":           path.resolve(__dirname, "./src"),
      "@components": path.resolve(__dirname, "./src/components"),
      "@hooks":      path.resolve(__dirname, "./src/hooks"),
    }
  },
  define: {
    __APP_VERSION__: JSON.stringify(process.env.npm_package_version),
  },
});

// src/vite-env.d.ts — extends Vite's types with your env vars
/// <reference types="vite/client" />

interface ImportMetaEnv {
  readonly VITE_API_URL:    string;
  readonly VITE_APP_TITLE:  string;
  readonly VITE_ANALYTICS?: string;
}
interface ImportMeta {
  readonly env: ImportMetaEnv;
}

// Usage — fully typed
const url = import.meta.env.VITE_API_URL;   // string ✅
const bad = import.meta.env.VITE_UNKNOWN;   // ❌ not declared
```

```json
// package.json scripts
{
  "scripts": {
    "dev":       "vite",
    "build":     "tsc && vite build",  // type-check THEN build
    "typecheck": "tsc --noEmit",       // type-check only, no output
    "preview":   "vite preview"
  }
}
```

**Interview Q: How does Vite handle TypeScript and what does `tsc && vite build` do?** 🔴
**A:** Vite uses esbuild to strip TypeScript types — extremely fast but no type-checking. `tsc && vite build` runs type-checking first (`tsc` with `noEmit`); only if it passes does the actual build run. The dev server uses esbuild only (fast), while CI and production builds always type-check first. Never skip the `tsc` step in the build script.

**Aha moment:** The dev server starts in milliseconds because Vite doesn't type-check. Your editor shows type errors; the dev server doesn't block on them. Type-check only happens during `build`.

**Common mistake:** Removing `tsc &&` from the build script to speed up CI. This allows type errors into production. Always keep it — type checking is fast compared to deployment failures.

**One-liner:** Vite = esbuild transpile (no type check, instant dev). `"build": "tsc && vite build"` = type-check first, always.

---

### 🔴 20.2 TypeScript with Next.js

**Explanation:** Next.js has built-in TypeScript — no extra configuration needed. App Router and Pages Router have different type patterns; params are always strings.

```typescript
// ── App Router (Next.js 13+) ───────────────────────────────
// app/users/[id]/page.tsx
interface PageProps {
  params:       { id: string };          // always string — URL segments
  searchParams: Record<string, string>;  // ?key=value query params
}

// Server components can be async — TypeScript supports this natively
export default async function UserPage({ params }: PageProps) {
  const user = await fetchUser(Number(params.id)); // convert string → number
  return <UserProfile user={user} />;
}

// App Router API route
// app/api/users/[id]/route.ts
import { NextRequest, NextResponse } from "next/server";

export async function GET(
  req: NextRequest,
  { params }: { params: { id: string } }
): Promise<NextResponse<User | { error: string }>> {
  try {
    const user = await fetchUser(Number(params.id));
    return NextResponse.json(user);
  } catch {
    return NextResponse.json({ error: "Not found" }, { status: 404 });
  }
}

// Typed metadata
import type { Metadata } from "next";
export const metadata: Metadata = { title: "User Profile" };

// ── Pages Router (Next.js 12 and below) ────────────────────
import type { GetServerSideProps, NextPage } from "next";

interface Props { user: User }

const UserPage: NextPage<Props> = ({ user }) => <UserProfile user={user} />;

export const getServerSideProps: GetServerSideProps<Props> = async context => {
  const id   = context.params?.id as string;
  const user = await fetchUser(Number(id));
  return { props: { user } };
};

export default UserPage;
```

**Interview Q: How do you type dynamic route parameters in Next.js App Router?** 🔴
**A:** Page components receive `params` and `searchParams` as props. Define `interface PageProps { params: { id: string }; searchParams: Record<string, string> }`. Both are always strings — URL segments are always strings regardless of content. Convert to the appropriate type in the component body: `Number(params.id)`, `new Date(params.date)`, etc.

**Aha moment:** App Router Server Components are just async functions — TypeScript fully supports `async` page components. `await fetchUser()` directly in the component is natural and type-safe.

**Common mistake:** Typing `params.id` as `number` — URL params are always strings. Always convert: `Number(params.id)` or `parseInt(params.id, 10)`.

**One-liner:** Next.js App Router: `params: { id: string }` — always strings, convert in component. Async server components are natively supported.

---

### 🔴 20.3 Environment Variables and Type Safety

**Explanation:** Env vars are always `string | undefined`. Extend `ImportMetaEnv` for TypeScript types, then add Zod validation at startup for runtime safety. Types alone don't validate — Zod does.

```typescript
// src/vite-env.d.ts — TypeScript types for env vars
/// <reference types="vite/client" />

interface ImportMetaEnv {
  readonly VITE_API_BASE_URL:  string;
  readonly VITE_APP_NAME:      string;
  readonly VITE_ANALYTICS_ID?: string; // optional
}
interface ImportMeta { readonly env: ImportMetaEnv; }

// Use — typed in all files
const url = import.meta.env.VITE_API_BASE_URL; // string ✅

// Node.js / Next.js — extend ProcessEnv
// src/types/env.d.ts
declare global {
  namespace NodeJS {
    interface ProcessEnv {
      NODE_ENV:     "development" | "production" | "test";
      DATABASE_URL: string;
      JWT_SECRET:   string;
      PORT?:        string;
    }
  }
}
process.env.NODE_ENV;     // "development" | "production" | "test" ✅
process.env.DATABASE_URL; // string ✅

// Best practice: Zod validation at startup (validates at RUNTIME too)
import { z } from "zod";

const EnvSchema = z.object({
  VITE_API_BASE_URL: z.string().url("Must be a valid URL"),
  VITE_APP_NAME:     z.string().min(1),
  VITE_ANALYTICS_ID: z.string().optional(),
});

// Throws immediately at app startup if env vars missing/invalid
export const env = EnvSchema.parse(import.meta.env);
// env.VITE_API_BASE_URL: string ✅ (both typed AND validated)
```

**Interview Q: Why is TypeScript typing alone insufficient for environment variable safety?** 🔴
**A:** TypeScript's `ImportMetaEnv` declarations say `VITE_API_BASE_URL: string` — but at runtime the `.env` file might be missing, making it actually `undefined`. TypeScript can't verify runtime values. Zod validation at startup catches this: if the var is missing or malformed, the app fails immediately with a clear error instead of crashing later with a cryptic null pointer deep in a request handler.

**Aha moment:** TypeScript type declarations for env vars are compile-time only. If the `.env` file is missing, TypeScript still says it's `string` — Zod is the runtime guardian.

**Common mistake:** Relying only on TypeScript types for env var safety. Types don't prevent `undefined` at runtime — Zod validation at startup does.

**One-liner:** Extend `ImportMetaEnv` for types. Add Zod validation at startup for runtime safety. Both needed — types prevent wrong usage, Zod prevents missing values.

---

### 🟡 20.4 Strict Mode Migration Strategy

**Explanation:** Adding strict TypeScript to an existing codebase is a process. Enable flags incrementally to avoid fixing 500 errors at once. Use `@ts-expect-error` (never `@ts-ignore`) for temporary suppressions.

```json
// Incremental migration — enable one flag at a time
{
  "compilerOptions": {
    // Phase 1: Non-breaking (enable immediately, minimal changes)
    "noImplicitReturns":          true,
    "noFallthroughCasesInSwitch": true,

    // Phase 2: Most impactful (most errors, most value)
    "strictNullChecks":           true,  // biggest fix effort, biggest payoff
    "noImplicitAny":              true,

    // Phase 3: After Phase 2 is done
    "strictFunctionTypes":              true,
    "strictPropertyInitialization":     true,
    "useUnknownInCatchVariables":       true,

    // Phase 4: Optional ambitious flag
    // "noUncheckedIndexedAccess": true, // arr[0] becomes T | undefined
  }
}
```

```typescript
// @ts-expect-error > @ts-ignore for temporary suppressions
// @ts-ignore     = silently ignores, no feedback when fixed
// @ts-expect-error = TypeScript ERRORS if the line no longer has an error

// ✅ Use @ts-expect-error with a tracking comment
// @ts-expect-error MIGRATION: type this properly (issue #234)
const result = legacyFunction(untyped as any);

// ❌ @ts-ignore (avoid)
// @ts-ignore
const result = legacyFunction(untyped); // silently stays even after fix

// JS → TS migration: file by file
// tsconfig: allowJs: true (accept .js and .ts together during migration)
// Rename leaf files first (files with no .ts dependencies)
// Work from the leaves toward the root

// Quick stub for untyped library during migration
// src/types/some-lib.d.ts
declare module "some-untyped-lib" {
  const lib: any; // typed as any during migration — improve later
  export = lib;
}
```

**Interview Q: What is the recommended strategy for enabling strict TypeScript in a large existing codebase?** 🟡
**A:** Enable strict flags one at a time rather than all at once. Start with non-breaking flags (`noImplicitReturns`), then tackle the high-value high-effort ones (`strictNullChecks`, `noImplicitAny`) separately. Use `@ts-expect-error` with tracking comments for temporary suppressions — it errors when the suppression becomes unnecessary, helping you track migration progress. Rename `.js` to `.ts` file by file, starting from leaf files.

**Aha moment:** `@ts-expect-error` actively helps your migration — when you fix a suppressed error and forget to remove the comment, TypeScript tells you. `@ts-ignore` is silent and permanent.

**Common mistake:** Using `@ts-ignore` for permanent suppression. Every `@ts-ignore` is a permanent hole. Use `@ts-expect-error` — TypeScript will notify you when the underlying issue is fixed.

**One-liner:** One strict flag at a time. `@ts-expect-error` + tracking comment for suppressions (not `@ts-ignore`). Rename `.js` → `.ts` file by file.

---

### 🟡 20.5 TypeScript with Node.js and Express

**Explanation:** TypeScript works well for Node.js APIs. Use `@types/node` for Node globals, `@types/express` for Express, and module augmentation to add custom properties to `Request`.

```typescript
// tsconfig for Node.js
{
  "compilerOptions": {
    "target":           "ES2022",
    "module":           "NodeNext",
    "moduleResolution": "NodeNext",
    "outDir":           "./dist",
    "rootDir":          "./src",
    "strict":           true,
    "esModuleInterop":  true,
    "types":            ["node"]
  }
}

// Install: npm install --save-dev @types/node @types/express
```

```typescript
import express, { Application, Request, Response, NextFunction } from "express";

const app: Application = express();
app.use(express.json());

// Typed route — Request<Params, ResBody, ReqBody, Query>
interface CreateUserBody { name: string; email: string; role?: "user" | "admin" }

app.post(
  "/users",
  async (req: Request<{}, User, CreateUserBody>, res: Response<User | { error: string }>) => {
    const { name, email, role = "user" } = req.body; // typed ✅
    try {
      const user = await createUser({ name, email, role });
      res.status(201).json(user);
    } catch (e) {
      res.status(400).json({ error: getErrorMessage(e) });
    }
  }
);

// Module augmentation — add req.user to all handlers
declare global {
  namespace Express {
    interface Request { user?: AuthUser }
  }
}

function authMiddleware(req: Request, res: Response, next: NextFunction): void {
  const token = req.headers.authorization?.replace("Bearer ", "");
  if (!token) { res.status(401).json({ error: "Unauthorized" }); return; }
  req.user = verifyToken(token); // req.user is now typed ✅
  next();
}

app.get("/profile", authMiddleware, (req, res) => {
  if (req.user) {
    res.json({ name: req.user.name }); // ✅ TypeScript knows req.user shape
  }
});
```

**Interview Q: How do you add `req.user` to Express request handlers in TypeScript?** 🟡
**A:** Use module augmentation: declare `namespace Express { interface Request { user?: AuthUser } }` in a `.d.ts` file. This extends Express's `Request` type globally — `req.user` is now typed in all handlers without modifying Express's source. The module augmentation is pure TypeScript — zero runtime impact.

**Aha moment:** Module augmentation for Express is one of the most practical `.d.ts` patterns — it adds `req.user` typing to every handler in your entire project from one declaration.

**Common mistake:** Forgetting `return` after `res.status(401).json(...)` in middleware. Without `return`, `next()` also runs — TypeScript doesn't catch this logic error.

**One-liner:** Express: `Request<Params, ResBody, ReqBody, Query>` for typed routes. `declare namespace Express { interface Request }` for `req.user`. Module augmentation = zero runtime impact.

---

### 🟢 20.6 Publishing a TypeScript Library

**Explanation:** Publishing a TypeScript library requires generating both JavaScript and declaration files, configuring `package.json` exports, and ensuring consumers get types automatically.

```json
// package.json — library configuration
{
  "name":    "my-typed-lib",
  "version": "1.0.0",
  "main":    "./dist/index.js",
  "module":  "./dist/index.mjs",
  "types":   "./dist/index.d.ts",
  "exports": {
    ".": {
      "import":  "./dist/index.mjs",
      "require": "./dist/index.js",
      "types":   "./dist/index.d.ts"
    }
  },
  "files": ["dist"],
  "scripts": { "build": "tsc -p tsconfig.build.json" }
}
```

```json
// tsconfig.build.json — library build config
{
  "compilerOptions": {
    "declaration":    true,   // emit .d.ts files for consumers
    "declarationMap": true,   // emit .d.ts.map for Go to Source
    "sourceMap":      true,
    "outDir":         "./dist",
    "rootDir":        "./src",
    "strict":         true
  },
  "include": ["src/**/*"],
  "exclude": ["src/**/*.test.ts"] // don't ship test files
}
```

```typescript
// src/index.ts — public API surface only
export { createThing }          from "./thing";
export { formatValue }          from "./utils";
export type { ThingOptions }    from "./types"; // type-only export
export type { FormatterConfig } from "./types";
// Internal utilities NOT exported — only exposed as types in .d.ts
```

**Interview Q: What is the minimum configuration to publish a TypeScript library with proper types?** 🟢
**A:** Set `"declaration": true` in tsconfig to emit `.d.ts` files. In `package.json`, set `"types": "./dist/index.d.ts"` to point consumers to your type declarations. Use `export type { ... }` for type-only exports. The `"exports"` field in package.json allows separate CJS/ESM/types entry points for maximum compatibility.

**Aha moment:** When a library consumer gets typed autocomplete without installing any `@types` package — that's because the library ships `.d.ts` files via the `"types"` field in package.json.

**Common mistake:** Not excluding test files from the library build. Shipping `*.test.ts` declaration files adds noise and unused types to consumers' type definitions.

**One-liner:** `declaration: true` + `"types": "./dist/index.d.ts"` = library ships own types. `export type` for type-only exports. Exclude test files from build.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Quick Revision — Part 3 One-Liners

```
CATEGORY 15: MODULES & DECLARATION FILES
  import type = compile-time only, zero runtime. Required for Vite/SWC isolatedModules
  @types/lib = devDependency always. Modern libs ship own types — check package.json
  .d.ts = types only. Write for untyped libs, globals, asset imports, never edit auto-generated
  Module augmentation: import "module" first, then extend. Extends without replacing
  declare = "exists at runtime, trust me." For globals, script-tag libs, env vars
  Namespaces = legacy. ES modules in all new code. Namespaces only in .d.ts for globals

CATEGORY 16: TYPE NARROWING DEEP DIVE
  Control flow: TS narrows automatically through if/return/throw/assignment. No annotation needed
  Truthy if (x) removes null/undefined/0/""/false/NaN. Beware: 0 and "" may be valid values!
  React: {count && <X />} renders 0 when count = 0. Use {count > 0 && <X />}
  x != null removes BOTH null AND undefined. The one acceptable ==
  Discriminant check = exact variant narrowed, all variant properties available
  (v): v is T = type predicate. Regular boolean doesn't narrow. Essential for .filter()
  asserts value is T = throw if invalid, narrow everything after call site
  satisfies T = validate + keep specific types. Annotation widens; satisfies doesn't

CATEGORY 17: TSCONFIG DEEP DIVE
  target = emitted JS version (syntax). lib = available type definitions (no output effect)
  strict: true = 8 safety flags. strictNullChecks is most critical. Enable all from day 1
  module: ESNext + moduleResolution: bundler for Vite. esModuleInterop = clean imports
  paths in tsconfig = TypeScript resolves. alias in Vite = bundler resolves. Both required
  declaration: true for libraries. sourceMap: true for debugging. Both shallow concerns
  Project references = incremental monorepo builds. composite: true per sub-project

CATEGORY 18: STATE MANAGEMENT TYPING
  Zustand: create<StoreInterface>() = whole store typed. Selectors auto-typed
  RTK: RootState + AppDispatch derived from store (never written manually). Export typed hooks
  useReducer + discriminated union Action = every dispatch payload validated at compile time
  QUERY_KEYS with as const = centralized typed keys. useQuery<Data, Error> generics

CATEGORY 19: TESTING WITH TYPESCRIPT
  Vite → Vitest (TypeScript native, zero config). Jest → ts-jest + module: CommonJS
  Type test data — catches stale tests when types change. Never use as T in test objects
  vi.mocked(fn) = typed mock. mockResolvedValue validates against real return type
  renderWithProviders = typed provider wrapper. satisfies T in toHaveBeenCalledWith
  createMockFactory(overrides: Partial<T>): T = centralized defaults, single update point

CATEGORY 20: BUILD TOOLS & INTEGRATION
  Vite: esbuild = fast transpile (no type check). "build": "tsc && vite build" = check first
  Next.js App Router: params: { id: string } always strings. Async server components OK
  ImportMetaEnv + Zod at startup = types AND runtime validation. Types alone insufficient
  Strict migration: one flag at a time. @ts-expect-error > @ts-ignore (notifies when fixed)
  Express: Request<P, ResBody, ReqBody, Q> typed routes. Namespace augmentation for req.user
  Library: declaration: true + "types" in package.json. export type for type-only exports
```

---

# Master Quick Revision — All 20 Categories

```
═══════════════════════════════════════════════════════════════════════
PART 1 — FOUNDATIONS + CORE TYPE SYSTEM (Categories 1–7)
═══════════════════════════════════════════════════════════════════════

SETUP (Cat 1)
  TypeScript = JavaScript + types. tsc strips types → plain JS. Zero runtime cost
  "strict": true non-negotiable. Set from day 1. ts-node = dev only
  Path aliases: tsconfig paths (types) + Vite alias (runtime). Both required

BASIC TYPES (Cat 2)
  string, number, boolean — always lowercase. any = avoid; unknown = safe alternative
  void = no return. never = never returns. never in switch default = exhaustiveness
  Annotate: params, empty arrays, null state. Infer: local vars, callbacks
  Tuples = fixed-length typed positions. useState returns a tuple!
  String enum preferred. as const = modern lightweight enum alternative

TYPE INFERENCE (Cat 3)
  TS infers from values and context. Over-annotating = code smell
  Annotate boundaries (params, public APIs). Infer internals
  Inline JSX handlers get event types automatically — never annotate them
  const = literal type. let = widens. as const = freeze object literal types

FUNCTIONS (Cat 4)
  Params always annotated. Return: public = annotate, private = infer
  param? = T | undefined inside. param = default = T always inside
  ...args: string[] = rest params. Overloads = when input type → return type varies
  Callback type: (param: T) => ReturnType. Inline in interfaces

INTERFACES & TYPES (Cat 5)
  interface = objects + declaration merging. type = everything else
  prop? = optional (T | undefined). Use ?. and ?? for safe access
  readonly = compile-time only, shallow. extends ComponentProps<'div'> + {...rest}
  A & B = AND (all props). A | B = OR (one type). Record<K, V> preferred
  Discriminated unions = literal discriminant enables precise narrowing per case

UNION & LITERAL TYPES (Cat 6)
  A | B = one type. Use common methods without narrowing first
  Literal types = exact values. Autocomplete + typo prevention
  typeof = primitives only. instanceof = classes. "prop" in obj = plain objects
  (v): v is T type predicate — essential for .filter() typed results
  never in switch default = catch unhandled union cases at compile time

GENERICS (Cat 7)
  <T> = type placeholder inferred at call site. Preserves types unlike any
  <T extends Constraint> = restricts T, enables accessing T's properties
  <T, U> for different input/output. <T = Default> = fallback when omitted
  T extends U ? X : Y distributes over unions. infer R extracts type from pattern

═══════════════════════════════════════════════════════════════════════
PART 2 — CLASSES, UTILITIES, REACT TYPING (Categories 8–14)
═══════════════════════════════════════════════════════════════════════

CLASSES (Cat 8)
  constructor(private name: string) = declare + assign in one. Eliminates boilerplate
  public/private/protected = compile-time only. # = runtime privacy
  implements = type check only, no code. extends = real inheritance
  Abstract = non-instantiable base with shared methods + required abstract overrides

UTILITY TYPES (Cat 9)
  Partial<T> = optional. Required<T> = mandatory. Readonly<T> = read-only (shallow)
  Pick<T, K> = keep listed. Omit<T, K> = remove listed
  Record<K, V> = typed dict. With union K = all keys required
  Exclude removes matching. Extract keeps matching. NonNullable removes null/undefined
  ReturnType<F> = return. Parameters<F> = param tuple. Awaited<T> = unwrap Promise

TYPING REACT (Cat 10)
  Plain typed function > React.FC in modern React
  ReactNode for children (accepts all). JSX.Element too restrictive
  Generic components: function List<T>. In .tsx arrow: <T,> (trailing comma)
  extends ComponentProps<'div'> + {...rest} = all native HTML attrs
  forwardRef<HTMLInputElement, Props> — ref element type first, props second

TYPING HOOKS (Cat 11)
  useState<User | null>(null) — always annotate null/empty array starts
  useRef(null) = DOM ref (RefObject). useRef(value) = mutable (MutableRefObject)
  useReducer + discriminated union = every dispatch payload validated
  createContext<Type>(null!) + custom hook null check = typed context
  [value, fn] as const = typed tuple return (not union array)

EVENTS & DOM (Cat 12)
  React.ChangeEvent<HTMLInputElement> for extracted handlers. Inline = auto-inferred
  e.preventDefault() always first in form onSubmit
  currentTarget = handler element (precise). target = triggering element (broad)
  querySelector<T>('.cls') for typed DOM queries

DATA FETCHING (Cat 13)
  typedFetch<T>() wrapper. Always check res.ok before .json()
  Catch errors are unknown. instanceof Error before .message
  getErrorMessage(error: unknown) = reusable utility. Zod = runtime + type safety
  useQuery<Data, Error>. data is T | undefined until loaded

MAPPED & TEMPLATE TYPES (Cat 14)
  [K in keyof T]: T[K] = iterate type keys. This IS Partial, Readonly, etc.
  keyof typeof obj = keys of runtime object (key idiom)
  Template literals: `${Union}suffix` = type pattern families
  T["key"] = type of property. T[number] = array element type

═══════════════════════════════════════════════════════════════════════
PART 3 — ADVANCED (Categories 15–20)
═══════════════════════════════════════════════════════════════════════

MODULES & DECLARATIONS (Cat 15)
  import type = compile-time only, zero runtime. Required for Vite isolatedModules
  @types/lib = devDependency. Modern libs ship own types
  .d.ts = types only. For untyped libs, globals, asset imports
  Module augmentation: import "module" first, then extend. Zero runtime impact
  declare = "exists at runtime." Namespaces = legacy, only in .d.ts

NARROWING DEEP DIVE (Cat 16)
  Control flow: TS narrows automatically. No annotation needed after checks
  Truthy if (x) removes 0/"" too — beware! React: {n && <X />} renders 0
  x != null removes BOTH null AND undefined — the one acceptable ==
  Discriminant check = exact variant, all properties available
  (v): v is T = predicate, narrows call site. asserts = narrows from call forward
  satisfies = validate + keep specific types (annotation widens)

TSCONFIG (Cat 17)
  target = emitted JS. lib = type definitions (independent!)
  strict: true = 8 flags. strictNullChecks = most critical
  moduleResolution: bundler for Vite. esModuleInterop = clean imports
  paths + Vite alias: both required, keep in sync
  declaration for libraries. sourceMap for debugging

STATE MANAGEMENT (Cat 18)
  Zustand: create<StoreInterface>() = whole store typed, selectors auto-typed
  RTK: derive RootState + AppDispatch. Export useAppSelector + useAppDispatch
  useReducer + discriminated union = typed dispatch, validated payloads
  QUERY_KEYS as const + useQuery<Data, Error> = typed React Query

TESTING (Cat 19)
  Vitest for Vite (native TS). Jest: ts-jest + module: CommonJS in test tsconfig
  Type test data. vi.mocked(fn) = typed mocks with validated return values
  renderWithProviders wrapper. satisfies T in assertions
  createMockFactory(Partial<T>) = centralized typed test data

BUILD TOOLS (Cat 20)
  Vite: esbuild strips types (fast). "tsc && vite build" = check first always
  Next.js: params always strings. Async server components supported
  ImportMetaEnv + Zod validation = types + runtime safety. Both needed
  @ts-expect-error > @ts-ignore. Strict flags one at a time
  Express: Request<P, ResBody, ReqBody, Q>. Namespace augmentation for req.user
  Library: declaration: true + "types" in package.json. export type for types
```

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

*End of TypeScript Complete Learning Guide — All 20 Categories.*

*Part 1 → Categories 1–7: Setup, Types, Inference, Functions, Interfaces, Union Types, Generics*
*Part 2 → Categories 8–14: Classes, Utility Types, React Components, Hooks, Events, Data Fetching, Mapped Types*
*Part 3 → Categories 15–20: Modules, Narrowing, tsconfig, State Management, Testing, Build Tools*