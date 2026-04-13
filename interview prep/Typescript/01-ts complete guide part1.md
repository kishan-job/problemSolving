# TypeScript Complete Interview & Learning Guide
## Part 1 of 3 — Categories 1–7: Foundations & Core Type System
### For React Developers Who Already Know JavaScript

---

# Table of Contents — Part 1

- [How to Use This Guide](#how-to-use-this-guide)
- [Category 1: TypeScript Setup & Tooling](#category-1-typescript-setup--tooling)
- [Category 2: Basic Types & Annotations](#category-2-basic-types--annotations)
- [Category 3: Type Inference](#category-3-type-inference)
- [Category 4: Functions in TypeScript](#category-4-functions-in-typescript)
- [Category 5: Interfaces & Type Aliases](#category-5-interfaces--type-aliases)
- [Category 6: Union & Literal Types](#category-6-union--literal-types)
- [Category 7: Generics](#category-7-generics)
- [Quick Revision — Part 1 One-Liners](#quick-revision--part-1-one-liners)

---

# How to Use This Guide

```
🔴 Must-know  — Daily use. Asked in 90% of TS interviews. Learn these FIRST.
🟡 Should-know — Common in real projects. Asked in ~50% of interviews.
🟢 Good-to-know — Senior / library-author level. Impresses in interviews.
```

Each topic follows this exact structure:
1. **Explanation** — plain English, 2–3 sentences
2. **Code** — TypeScript with inline comments
3. **Interview Q&A** — the most common question + answer
4. **Aha moment** — what's different from JavaScript
5. **Common mistake** — what trips developers up
6. **One-liner** — quick revision note

**The one mindset shift:** In JavaScript you discover type bugs at runtime when users hit them. In TypeScript you catch them at compile time before code runs. Every feature in this guide serves that single goal.

---

# Category 1: TypeScript Setup & Tooling

---

### 🔴 1.1 Installing TypeScript, Running tsc

**Explanation:** TypeScript is a superset of JavaScript — every valid JS file is also valid TS. You install it as a dev dependency and use the `tsc` compiler to check types and output plain `.js` files that browsers and Node can run. You never ship `.ts` files to production.

```typescript
// STEP 1: install TypeScript
// npm install --save-dev typescript

// STEP 2: create hello.ts
const message: string = "Hello TypeScript";
console.log(message);

// STEP 3: compile → produces hello.js (types erased)
// npx tsc hello.ts

// STEP 4: run the plain JS output
// node hello.js

// The output hello.js contains no types at all:
// const message = "Hello TypeScript";
// console.log(message);
// Types exist at compile time only — zero runtime cost

// Compile an entire project (reads tsconfig.json):
// npx tsc

// Type-check only, no output files:
// npx tsc --noEmit
```

**Interview Q: What is TypeScript and what problem does it solve?** 🔴
**A:** TypeScript is a statically typed superset of JavaScript that compiles to plain JS. It catches type errors at compile time — before code runs — provides editor autocomplete, makes large codebases maintainable, and self-documents APIs. At runtime it adds zero overhead because all types are stripped during compilation.

**Aha moment:** In JS, `user.nme` (a typo) silently returns `undefined` and crashes later. In TS that typo is a red squiggle in your editor before you even save the file.

**Common mistake:** Running `node hello.ts` directly. Node doesn't understand TypeScript syntax — always compile first with `tsc`, or use `ts-node` for development convenience.

**One-liner:** TypeScript = JavaScript + types. `tsc` checks types and outputs plain JS. Zero runtime cost — types are erased.

---

### 🔴 1.2 tsconfig.json — What It Is and Key Options

**Explanation:** `tsconfig.json` is the TypeScript compiler's configuration file. It tells `tsc` which files to compile, what JavaScript version to target, and how strict to be. Every TypeScript project needs one — without it `tsc` uses permissive defaults.

```json
{
  "compilerOptions": {
    "target": "ES2020",            // JS version output will use
    "lib": ["ES2020", "DOM"],      // type definitions available (DOM = browser APIs)
    "jsx": "react-jsx",            // how JSX transforms (react-jsx = React 17+)
    "module": "ESNext",            // module system for output
    "moduleResolution": "bundler", // how imports resolve (use "bundler" for Vite)
    "strict": true,                // ← THE MOST IMPORTANT OPTION
    "outDir": "./dist",            // where compiled JS files go
    "rootDir": "./src",            // where your .ts source files live
    "esModuleInterop": true,       // allows: import React from 'react' (not import * as)
    "skipLibCheck": true,          // skip checking .d.ts files (faster builds)
    "noUnusedLocals": true,        // error on unused variables
    "noUnusedParameters": true     // error on unused function parameters
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

**Interview Q: What does `"strict": true` do in tsconfig?** 🔴
**A:** `strict: true` is a master switch that enables a group of strict checks: `strictNullChecks` (null/undefined are separate types), `noImplicitAny` (no untyped variables), `strictFunctionTypes`, `strictBindCallApply`, and more. It's the single most important tsconfig option — always enable it from project start.

**Aha moment:** Without `strict`, TypeScript is surprisingly lenient and lets many unsafe patterns through. With it on, you get the full type safety TypeScript is known for.

**Common mistake:** Starting a project without `"strict": true` and enabling it six months later — you'll have hundreds of errors to fix all at once. Always set it on day one.

**One-liner:** tsconfig.json configures the compiler. `"strict": true` is non-negotiable — set it first, everything else is secondary.

---

### 🔴 1.3 strict Mode — What It Enables

**Explanation:** `strict: true` activates a bundle of safety rules. The two that matter most: `strictNullChecks` makes null/undefined their own types (not assignable to `string`/`number`), and `noImplicitAny` prevents variables from silently becoming `any`.

```typescript
// ── WITHOUT strict (dangerous defaults) ────────────────────
function greet(name) {        // name is silently 'any' — no error!
  return "Hello " + name;
}
let user: string = null;      // allowed — will crash at runtime!

// ── WITH strict: true (what you always want) ────────────────
function greet(name: string) { // must annotate — no implicit any
  return "Hello " + name;
}
let user: string = null;       // ❌ Error: null not assignable to string
let user: string | null = null; // ✅ must be explicit about null

// What strict: true enables under the hood:
// • strictNullChecks        — null/undefined are their own types
// • noImplicitAny           — all variables must have knowable types
// • strictFunctionTypes     — function parameters checked correctly
// • strictBindCallApply     — call/bind/apply are type-checked
// • strictPropertyInitialization — class props must be initialized
// • noImplicitThis          — 'this' must be typed in functions
// • alwaysStrict            — emits 'use strict' in all output

// Most impactful: strictNullChecks
const user = getUser(); // returns User | null
user.name;              // ❌ Error: user might be null
if (user !== null) {
  user.name;            // ✅ TypeScript knows user is User here
}
```

**Interview Q: What is `strictNullChecks` and why is it TypeScript's most important feature?** 🔴
**A:** With `strictNullChecks`, `null` and `undefined` are not automatically assignable to other types. Before using a value that might be null, you must check it. This prevents the most common JavaScript runtime error — "Cannot read properties of null/undefined" — at compile time instead of in production.

**Aha moment:** Without `strictNullChecks`, TypeScript lets you write `user.name` when `user` might be null — the same bug as vanilla JS. Turning it on is the single change that prevents the most real-world crashes.

**Common mistake:** Disabling `strict` to "fix" errors faster. Every error you suppress this way is a real bug you're choosing to ignore.

**One-liner:** `strict: true` = full safety. Key rules: null/undefined are separate types, no implicit any. Never disable it.

---

### 🔴 1.4 ts-node — Running TypeScript Directly

**Explanation:** `ts-node` executes TypeScript files directly without a manual compile step. It transpiles in memory and runs immediately. Use it during development for scripts and Node servers — never in production because it adds startup overhead.

```bash
# Install ts-node
npm install --save-dev ts-node

# Run a .ts file directly (no compile step!)
npx ts-node src/seed.ts

# With nodemon for auto-restart on save (Node.js backend dev)
npm install --save-dev nodemon
# package.json script: "dev": "nodemon --exec ts-node src/index.ts"

# Type-check only, no execution
npx tsc --noEmit
```

```typescript
// src/seed.ts — runs with: npx ts-node src/seed.ts
import { createUser } from "./db";

const users: string[] = ["Alice", "Bob", "Carol"];

for (const name of users) {
  await createUser({ name }); // full TypeScript type safety here
}
console.log("Seeded!");
```

**Interview Q: What is the difference between `tsc` and `ts-node`?** 🔴
**A:** `tsc` is the official TypeScript compiler — it type-checks code and writes `.js` output files for production. `ts-node` transpiles TypeScript in memory and executes it immediately, without writing files — for development convenience. Production builds always use `tsc`; `ts-node` is a dev tool only.

**Aha moment:** You can prototype and run TypeScript scripts exactly like JavaScript scripts during development — no manual build step.

**Common mistake:** Using `ts-node` in production Docker containers. It's significantly slower than running pre-compiled JavaScript and adds unnecessary startup time.

**One-liner:** `ts-node` = run `.ts` directly in dev, no output files. Use `tsc` for production builds. Never `ts-node` in prod.

---

### 🟡 1.5 ESLint + @typescript-eslint

**Explanation:** TypeScript checks type correctness. ESLint checks code quality and style. The `@typescript-eslint` package lets ESLint parse TypeScript syntax and apply TS-specific rules. Both tools are needed — they solve different problems.

```bash
# Install
npm install --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint

# .eslintrc.json
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": { "project": "./tsconfig.json" },
  "plugins": ["@typescript-eslint"],
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    "@typescript-eslint/no-unused-vars": "error",
    "@typescript-eslint/no-explicit-any": "warn",    // warn when you use 'any'
    "@typescript-eslint/explicit-function-return-type": "off" // too noisy
  }
}
```

**Interview Q: Why use ESLint alongside TypeScript? Doesn't the compiler already catch errors?** 🟡
**A:** They solve different problems. TypeScript catches type errors. ESLint catches code quality issues: unused variables, banned patterns like `no-explicit-any`, code style. You can write type-correct code that still has quality problems only ESLint catches. Both are needed for a well-maintained codebase.

**Aha moment:** `@typescript-eslint/no-explicit-any` is the most valuable rule — it warns whenever you bypass TypeScript's safety with `any`, which is the #1 beginner mistake.

**Common mistake:** Skipping `@typescript-eslint/parser` installation and wondering why ESLint throws parse errors on every TypeScript file.

**One-liner:** TypeScript = type correctness. ESLint = code quality. Both needed. `@typescript-eslint` bridges them.

---

### 🟡 1.6 Watch Mode

**Explanation:** Watch mode recompiles TypeScript automatically on every file save. In React projects (Vite, CRA, Next.js) this happens transparently as part of the dev server. For standalone Node projects you run `tsc --watch` explicitly.

```bash
# tsc watch mode — recompiles on every .ts file save
npx tsc --watch

# ts-node + nodemon — recompiles AND restarts Node process
npx nodemon --exec ts-node src/index.ts

# Vite (React projects) — handles TS compilation automatically
# Running "npm run dev" starts a dev server with built-in TS support
# Type errors appear as browser overlay + terminal warnings
```

**Interview Q: How does TypeScript compilation work in a Vite React project?** 🟡
**A:** Vite uses esbuild to transpile TypeScript files (strips types only — no type checking) for maximum build speed. Separately, running `tsc --noEmit` does the actual type checking. This means the dev server is fast, but type errors don't block hot reload — they appear as warnings in the terminal and browser overlay.

**Aha moment:** In React projects you experience TypeScript primarily through your editor's red squiggles and the terminal — not by running `tsc` manually.

**Common mistake:** Running `tsc --watch` inside a Vite project. Vite already handles compilation — this just creates duplicate work and confusing duplicate errors.

**One-liner:** `tsc --watch` for Node projects. Vite/Next.js handle it automatically in React. Check with `tsc --noEmit` to see all errors.

---

### 🟢 1.7 Path Aliases (baseUrl + paths)

**Explanation:** Path aliases let you import from `@/components/Button` instead of `../../../components/Button`. They must be configured in both tsconfig.json (so TypeScript understands them) AND your bundler config (so the build tool resolves them at runtime).

```json
// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*":            ["./src/*"],
      "@components/*":  ["./src/components/*"],
      "@hooks/*":       ["./src/hooks/*"],
      "@utils/*":       ["./src/utils/*"],
      "@types/*":       ["./src/types/*"]
    }
  }
}
```

```typescript
// vite.config.ts — bundler ALSO needs to know about aliases
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import path from "path";

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: { "@": path.resolve(__dirname, "./src") }
  }
});

// Before path aliases — ugly deep relative imports
import { Button }  from "../../../components/ui/Button";
import { useAuth } from "../../hooks/useAuth";
import { User }    from "../../../../types/user";

// After path aliases — clean and refactor-proof
import { Button }  from "@/components/ui/Button";
import { useAuth } from "@/hooks/useAuth";
import { User }    from "@/types/user";
```

**Interview Q: Why do path aliases need to be configured in both tsconfig AND the bundler?** 🟢
**A:** tsconfig paths teach the TypeScript type checker how to resolve imports — for type safety and editor autocomplete. The bundler's alias config (Vite, webpack) teaches the build tool how to resolve the same imports at bundle time. They're separate tools that don't share configuration. Missing either one breaks the feature.

**Aha moment:** TypeScript and your bundler are two completely separate tools running independently. TypeScript knows about types; the bundler knows about files. Both need to understand your aliases.

**Common mistake:** Configuring tsconfig paths but forgetting the Vite alias config — TypeScript gives you correct types and autocomplete, but the app crashes at runtime with "Module not found."

**One-liner:** Path aliases: configure in tsconfig.json (for types) AND vite/webpack config (for runtime). Both required — one alone breaks things.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 2: Basic Types & Annotations

---

### 🔴 2.1 Primitive Types: string, number, boolean

**Explanation:** The three daily primitives match JavaScript's primitive values exactly. You annotate with a colon after the variable name. TypeScript usually infers these from assigned values — explicit annotations are mainly needed at function parameters, which TypeScript cannot infer from inside the function.

```typescript
// Explicit annotations — you write the type
const name: string    = "Alice";
const age: number     = 30;
const active: boolean = true;

// Inferred — TypeScript figures it out (preferred when obvious)
const name   = "Alice"; // TypeScript infers: string
const age    = 30;      // TypeScript infers: number
const active = true;    // TypeScript infers: boolean

// TypeScript catches type mismatches at compile time
let score: number = 100;
score = "high";   // ❌ Error: Type 'string' is not assignable to type 'number'

// Function parameters — ALWAYS annotate, TS can't infer these
function greet(name: string, age: number): string {
  return `${name} is ${age}`;
}
greet("Alice", 30);  // ✅
greet(30, "Alice");  // ❌ Error: caught at compile time — args in wrong order!
greet("Alice");      // ❌ Error: Expected 2 arguments, got 1

// ⚠️ Critical: ALWAYS use lowercase types
const bad1: String  = "hello"; // ❌ wrong — String wrapper Object (avoid)
const bad2: Number  = 42;      // ❌ wrong — Number wrapper Object (avoid)
const good1: string = "hello"; // ✅ primitive type
const good2: number = 42;      // ✅ primitive type
```

**Interview Q: What is the difference between `string` (lowercase) and `String` (uppercase) in TypeScript?** 🔴
**A:** Lowercase `string` is the TypeScript primitive type — always use this. Uppercase `String` is the JavaScript String object wrapper class. They behave differently and `String` causes subtle bugs. TypeScript itself warns you to avoid the wrapper types. Always use lowercase.

**Aha moment:** TypeScript's primitive type names match exactly what JavaScript's `typeof` operator returns — `"string"`, `"number"`, `"boolean"`. This is intentional. There's no new syntax to learn, just annotations on what JavaScript already has.

**Common mistake:** Writing `String`, `Number`, `Boolean` (uppercase) as type annotations. The compiler accepts them but they refer to the wrapper object types — always use lowercase.

**One-liner:** Three daily primitives: `string`, `number`, `boolean` — always lowercase. Function params require explicit annotation; variables infer from assigned value.

---

### 🔴 2.2 null and undefined — and strictNullChecks

**Explanation:** With `strictNullChecks: true`, `null` and `undefined` are not assignable to `string` or `number` — they're their own distinct types. This is the single change that prevents the most JavaScript runtime crashes in TypeScript codebases.

```typescript
// ── WITHOUT strictNullChecks (dangerous — avoid) ────────────
let name: string = null;      // TS allows — crashes at runtime!
let age: number = undefined;  // TS allows — crashes at runtime!

// ── WITH strictNullChecks: true (always use this) ───────────
let name: string = null;         // ❌ Error: null not assignable to string
let name: string | null = null;  // ✅ explicit about nullable

// Accessing nullable values — TypeScript forces you to check
const user: User | null = getUser();
user.name;              // ❌ Error: user might be null
if (user !== null) {
  user.name;            // ✅ TypeScript knows user is User here
}
user?.name;             // ✅ optional chaining — returns undefined if null

// Common React pattern: async data starts null
const [user, setUser] = useState<User | null>(null); // starts null
// After fetch: setUser(fetchedUser) — now User

// undefined vs null — subtle difference
let declared: string | undefined;      // declared, not yet assigned
let intentional: string | null = null; // intentionally "no value"

// Nullish coalescing — handle null/undefined with a default
const displayName = user?.name ?? "Guest"; // "Guest" if user is null/undefined
```

**Interview Q: What is the difference between `null` and `undefined` in TypeScript?** 🔴

| | `null` | `undefined` |
|---|---|---|
| Meaning | Intentional absence of value | Not assigned yet |
| Convention | You set it deliberately | TypeScript/JS sets it |
| Optional props (`name?`) | Uses `undefined` | Uses `undefined` |
| Best practice | Use for "intentionally empty" | Let TS/JS assign naturally |

**A:** Both mean "no value" but with different intent. `null` is intentional absence — you explicitly assign it. `undefined` means not yet assigned — TypeScript uses it for optional properties and uninitialized variables. With `strictNullChecks`, both are separate types requiring explicit handling.

**Aha moment:** This is where TypeScript prevents the most real bugs. "Cannot read properties of undefined" becomes a compile-time error you fix before shipping, not a runtime crash your users hit.

**Common mistake:** `const [user, setUser] = useState(null)` without a generic type. TypeScript infers the state as permanently `null` — you can never set it to a `User`. Always write `useState<User | null>(null)`.

**One-liner:** `strictNullChecks` = null/undefined are their own types. Must handle explicitly. Use `?.` for safe access, `?? default` for fallbacks.

---

### 🔴 2.3 any vs unknown — The Most Important Difference

**Explanation:** `any` completely disables TypeScript's type checking for a value — it's an escape hatch that makes that variable behave like plain JavaScript. `unknown` is the type-safe alternative: it accepts any value but forces you to check the type before using it.

```typescript
// ── any — turns off TypeScript completely (AVOID) ──────────
let data: any = fetchSomeData();
data.user.name;       // no TS error — but crashes if data is null!
data.toUpperCase();   // no TS error — but crashes if data is a number!
data = 42;            // no error
data = "hello";       // no error
// TypeScript gives up entirely and trusts you blindly

// ── unknown — the safe version ──────────────────────────────
let response: unknown = fetchSomeData();
response.user.name;    // ❌ Error: can't access property of 'unknown'
response.toUpperCase(); // ❌ Error: can't call method on 'unknown'

// Must narrow the type first
if (typeof response === "string") {
  response.toUpperCase(); // ✅ TypeScript knows it's string here
}
if (response !== null && typeof response === "object" && "user" in response) {
  // ✅ safe to access
}

// ── Real-world use: catch block errors ──────────────────────
try {
  doRiskyOperation();
} catch (error: unknown) {
  // With strict: catch errors are 'unknown' (not 'any')
  error.message;  // ❌ Error: must check first
  if (error instanceof Error) {
    console.error(error.message); // ✅ safe
  }
}

// ── Real-world use: API responses ───────────────────────────
async function fetchUser(): Promise<unknown> {
  const res = await fetch("/api/user");
  return res.json(); // json() returns 'any' — we widen to unknown for safety
}
```

**Interview Q: What is the difference between `any` and `unknown`?** 🔴

| | `any` | `unknown` |
|---|---|---|
| Accepts any value | ✅ | ✅ |
| Use without checking | ✅ (no errors!) | ❌ (must narrow first) |
| Type safety level | None — disables TS | Full — forces checking |
| When to reach for it | Almost never | External/untyped data |

**A:** Both accept any value. The difference is what you can do with it. `any` disables all type checking — you can call methods, access properties, pass it anywhere with no errors. `unknown` is fully type-safe — TypeScript forces you to narrow the type before using it. `any` is a lie to the compiler; `unknown` is "I don't know yet, but I'll prove it before I use it."

**Aha moment:** Every `any` in your codebase is a hole in your type safety. A codebase with zero explicit `any` is one where TypeScript catches every catchable bug. Treat `any` like a code smell.

**Common mistake:** Reaching for `any` whenever TypeScript shows an error. Use `unknown` for truly untyped data (API responses, catch blocks), and fix the actual types everywhere else.

**One-liner:** `any` = disable type checking entirely (avoid). `unknown` = safe "could be anything" — must narrow before use. Always prefer `unknown`.

---

### 🔴 2.4 void and never

**Explanation:** `void` is the return type of functions that don't return a useful value — they run, finish, but the return value is meaningless (like event handlers or `console.log`). `never` is for functions that literally never finish — they always throw an error or run forever.

```typescript
// ── void: function finishes, returns nothing useful ─────────
function logMessage(msg: string): void {
  console.log(msg);
  // implicit return undefined — that's fine for void
}

// All React event handlers return void
const handleClick = (): void => {
  setCount(prev => prev + 1);
};

useEffect((): void => {
  document.title = "App";
}, []);

// ── never: function never reaches its end ───────────────────
function throwError(msg: string): never {
  throw new Error(msg);
  // execution stops here — never returns
}

function infiniteLoop(): never {
  while (true) { /* polls server forever */ }
}

// ── The practical magic of never: exhaustiveness checking ───
type Shape = "circle" | "square" | "triangle";

function describeShape(shape: Shape): string {
  switch (shape) {
    case "circle":   return "Round";
    case "square":   return "Four sides";
    case "triangle": return "Three sides";
    default:
      // shape is type 'never' here — all cases handled
      // Add "pentagon" to Shape and forget this switch?
      // TypeScript errors RIGHT HERE before your code ships
      const _exhaustiveCheck: never = shape;
      throw new Error(`Unhandled shape: ${shape}`);
  }
}
```

**Interview Q: What is the `never` type and what is its most practical use?** 🔴
**A:** `never` is the type of values that can never occur — functions that always throw or never return. Its most practical use is exhaustiveness checking: in a switch statement that handles every case of a union type, the `default` branch can only be reached if a new value was added without updating the switch. Assigning it to `never` there makes TypeScript error at compile time whenever a case is missing.

**Aha moment:** `void` means "returns nothing useful." `never` means "execution never reaches the return." They look similar but represent very different execution paths.

**Common mistake:** Using `void` as the return type for functions that always throw. They should be typed `never` — `void` implies the function could return normally (it just doesn't return a useful value).

**One-liner:** `void` = runs and finishes, no useful return. `never` = never reaches end (throws/infinite loop). `never` in switch default = exhaustiveness check.

---

### 🔴 2.5 Type Annotations on Variables

**Explanation:** The `: Type` colon syntax explicitly tells TypeScript what type a variable holds. In practice, you need this far less than beginners expect — TypeScript infers types from initialized values. Annotate where TypeScript can't figure it out on its own.

```typescript
// ── When TypeScript CAN infer — don't annotate ─────────────
const name    = "Alice";            // inferred: string ✅
const count   = 0;                  // inferred: number ✅
const scores  = [1, 2, 3];         // inferred: number[] ✅
const user    = { name: "Alice" };  // inferred: { name: string } ✅
const doubled = [1,2,3].map(n => n * 2); // inferred: number[] ✅

// ── When you MUST annotate — TypeScript can't infer ─────────

// 1. Declared without a value
let username: string;               // will be assigned later

// 2. Empty array — TypeScript has nothing to infer element type from
const tags: string[] = [];          // annotate! (otherwise: never[])
const tags = [];                    // ❌ inferred as never[] — nothing can go in!

// 3. Union types — especially nullable state
let currentUser: User | null = null;  // starts null, becomes User

// 4. Wider type than the literal
let status: "active" | "inactive" = "active"; // can be reassigned to "inactive"

// ── React: annotate initial state that doesn't match final ──
const [user, setUser]       = useState<User | null>(null);   // null initially
const [items, setItems]     = useState<Product[]>([]);       // empty array
const [error, setError]     = useState<string | null>(null); // null initially
```

**Interview Q: When should you write explicit type annotations vs. letting TypeScript infer?** 🔴
**A:** Let TypeScript infer when the type is obvious from the assigned value (`const x = 5`). Explicitly annotate when: the variable is declared without a value, an array starts empty, state can be null but will hold data later, or you want a union/wider type than what the literal would infer. Over-annotating is a sign of TS inexperience.

**Aha moment:** Good TypeScript code uses fewer annotations than beginners expect. `const name: string = "Alice"` — the `: string` is pure noise. TypeScript already knows from `"Alice"`.

**Common mistake:** Annotating every single variable with its obvious type. `const count: number = 0` is redundant. Let TypeScript infer and annotate only where it needs help.

**One-liner:** Annotate: empty arrays, uninitialized vars, nullable state, function params. Infer: everything initialized with a clear value.

---

### 🔴 2.6 Array Types: number[] and Array&lt;number&gt;

**Explanation:** Two syntaxes for typed arrays — identical in behavior, different in appearance. Both say "array where every element must be this type." TypeScript checks element types when you push, access, or pass them to functions.

```typescript
// Two identical syntaxes — pick one and be consistent
const scores: number[]      = [1, 2, 3]; // shorthand — most common
const scores: Array<number> = [1, 2, 3]; // generic — same thing

// TypeScript catches wrong element types
const names: string[] = ["Alice", "Bob"];
names.push(42);        // ❌ Error: Argument of type 'number' not assignable to 'string[]'
names[0].toUpperCase(); // ✅ TypeScript knows every element has string methods

// Array of objects
interface User { id: number; name: string }
const users: User[] = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
];
users[0].name; // ✅ TypeScript knows .name is a string

// ⚠️ CRITICAL: always annotate empty arrays
const tags: string[] = []; // ✅ TypeScript knows what goes in
const tags = [];            // ❌ TypeScript infers never[] — nothing can be pushed!

// Inference from initialization — works for non-empty arrays
const nums = [1, 2, 3]; // TypeScript infers: number[] ✅

// Readonly arrays — prevents mutation
const fixed: readonly number[] = [1, 2, 3];
fixed.push(4);  // ❌ Error: Property 'push' does not exist on readonly array
```

**Interview Q: What is the difference between `number[]` and `Array<number>` in TypeScript?** 🔴
**A:** They are exactly equivalent — just two syntaxes for the same type. `number[]` is the shorthand bracket syntax; `Array<number>` is the generic syntax. Most codebases prefer `number[]` for simple types. The important thing is consistency.

**Aha moment:** An empty array `[]` has no elements to infer from, so TypeScript infers `never[]` — an array that can never hold anything. Always annotate empty arrays or TypeScript won't let you add elements later.

**Common mistake:** Writing `const items = []` without a type annotation. The `never[]` type error appears later when you try to push items, which is confusing if you don't know why.

**One-liner:** `string[]` and `Array<string>` are identical. ALWAYS annotate empty arrays: `const items: string[] = []` — otherwise TypeScript infers `never[]`.

---

### 🔴 2.7 Tuple Types

**Explanation:** A tuple is a fixed-length array where each position has a specific type. TypeScript knows what type is at each index. React's `useState`, `useReducer`, and most custom hooks return tuples — you've been using them without knowing the term.

```typescript
// Tuple: exactly [string, number] in that order
const person: [string, number] = ["Alice", 30];  // ✅
const wrong: [string, number]  = [30, "Alice"];  // ❌ wrong order!

// Destructuring tuples — how React hooks work!
const [name, age] = person;
// name: string ✅   age: number ✅
// TypeScript knows which type is at which position

// useState IS a tuple return under the hood
const [count, setCount] = useState(0);
// TypeScript infers: [number, Dispatch<SetStateAction<number>>]
// count: number   setCount: function — perfectly typed!

// Custom hook returning a tuple
function useToggle(initial = false): [boolean, () => void] {
  const [value, setValue] = useState(initial);
  const toggle = () => setValue(v => !v);
  return [value, toggle]; // returns a typed tuple
}
const [isOpen, toggleOpen] = useToggle();
// isOpen: boolean   toggleOpen: () => void ✅

// Named tuple elements (TypeScript 4.0+) — clearer docs
type Point = [x: number, y: number];
const origin: Point = [0, 0];

// Tuple vs Array — key distinction
type Pair  = [string, number]; // fixed: 2 elements, typed per position
type Items = string[];          // flexible: any length, all same type
```

**Interview Q: What is a tuple in TypeScript? Where does React use tuples?** 🔴
**A:** A tuple is a fixed-length typed array where each position has a specific type. React's `useState` hook returns a tuple `[value, setter]` — TypeScript knows position 0 is the state value and position 1 is the dispatch function. That's why `const [count, setCount] = useState(0)` gives you perfectly typed variables without any extra annotation.

**Aha moment:** You've been using tuples every single day with React hooks! `const [value, setValue] = useState(...)` is tuple destructuring. TypeScript knows exactly what type each position holds.

**Common mistake:** Confusing tuples with arrays. `[string, number]` is a tuple — 2 elements, typed individually. `string[]` is an array — unlimited elements, all the same type.

**One-liner:** Tuple = fixed-length array with typed positions. `useState` returns a `[value, setter]` tuple. `const [a, b]: [string, number]`.

---

### 🔴 2.8 Enums — String vs Numeric

**Explanation:** Enums define named sets of constants. String enums are preferred because they produce readable runtime values. Numeric enums produce mystery numbers in logs. Many modern codebases replace enums entirely with `const` object + `as const`.

```typescript
// ── Numeric enum (default — AVOID in most cases) ────────────
enum Direction { Up, Down, Left, Right } // = 0, 1, 2, 3
const d = Direction.Up; // d = 0 at runtime — what does 0 mean?!

// ── String enum (PREFERRED — readable in logs/debugger) ─────
enum Status {
  Pending  = "PENDING",
  Active   = "ACTIVE",
  Inactive = "INACTIVE",
}
const s = Status.Active; // s = "ACTIVE" — readable everywhere

// React usage
function StatusBadge({ status }: { status: Status }) {
  return (
    <span className={status === Status.Active ? "green" : "gray"}>
      {status} {/* renders "ACTIVE" — readable! */}
    </span>
  );
}

// ── Modern alternative: const + as const (RECOMMENDED) ──────
// Generates no extra JS code — just plain JavaScript object
const STATUS = {
  Pending:  "PENDING",
  Active:   "ACTIVE",
  Inactive: "INACTIVE",
} as const;

// Extract the union type from values
type Status = typeof STATUS[keyof typeof STATUS];
// = "PENDING" | "ACTIVE" | "INACTIVE"

function setStatus(status: Status) { /* ... */ }
setStatus(STATUS.Active); // ✅ "ACTIVE"
setStatus("ACTIVE");       // ✅ the literal string also works
setStatus("invalid");      // ❌ TypeScript error
```

**Interview Q: What is the difference between string and numeric enums? When would you skip enums entirely?** 🔴
**A:** Numeric enums assign incrementing numbers (0, 1, 2) — unreadable in logs. String enums require explicit string values — readable and debuggable. Many modern codebases skip enums entirely in favor of `const` objects with `as const`: they generate no extra JavaScript code, work as plain objects at runtime, and avoid TypeScript-specific syntax that some teams dislike.

**Aha moment:** Enums are one of very few TypeScript features that actually generate JavaScript code in the output. Almost everything else is erased. This is why some teams prefer `as const` objects — they're invisible at runtime.

**Common mistake:** Using numeric enums and then being confused seeing `2` instead of `"Left"` in console logs or server logs. String enums or `as const` objects are almost always better.

**One-liner:** String enums > numeric enums (readable values). Modern alternative: `const OBJ = {...} as const` + `type T = typeof OBJ[keyof typeof OBJ]`.

---

### 🟡 2.9 Type Assertions: as and !

**Explanation:** Type assertions tell TypeScript "trust me, I know the type better than you do here." The `as` keyword asserts a specific type; the `!` non-null assertion tells TypeScript a value is definitely not null or undefined. Both override TypeScript's safety — use sparingly and only when you're certain.

```typescript
// ── as: override TypeScript's inferred type ─────────────────
// getElementById returns HTMLElement | null — TypeScript doesn't know which
const input = document.getElementById("email"); // HTMLElement | null
input.value; // ❌ Error: 'value' doesn't exist on HTMLElement

// Tell TypeScript it's specifically an input element
const input = document.getElementById("email") as HTMLInputElement;
input.value; // ✅ TypeScript trusts you it's an input

// ── ! (non-null assertion): remove null/undefined ───────────
const el = document.getElementById("app")!; // removes | null
el.innerHTML = "Hello"; // ✅ no null check needed (but risky!)

// React ref pattern — the most common use of !
const inputRef = useRef<HTMLInputElement>(null);
function handleFocus() {
  // Inside a handler, ref is definitely attached — ! is safe here
  inputRef.current!.focus();
}

// ── as const — completely different use of "as" ─────────────
// Freezes types as literals (covered more in inference section)
const config = { host: "localhost", port: 8080 } as const;
// type: { readonly host: "localhost"; readonly port: 8080 }

// ── What NOT to do ───────────────────────────────────────────
// Double assertion — always a red flag
const risky = (someValue as unknown) as string; // forces TS to accept wrong types

// Using ! everywhere to silence errors
const dangerous = maybeNull!.property; // crashes if maybeNull is null
```

**Interview Q: What is a type assertion (`as`) and how is it different from a type annotation?** 🟡
**A:** A type annotation (`: Type`) tells TypeScript the type during declaration — TypeScript enforces it. A type assertion (`as Type`) overrides TypeScript's inferred type at a specific point — TypeScript trusts you. Importantly, `as` does NOT convert values at runtime — if the value is actually a different type, you'll still get a runtime error. Annotations are proactive safety; assertions are overrides.

**Aha moment:** `as string` doesn't convert a number to a string at runtime like a cast in other languages. It only changes how TypeScript sees the value. The actual runtime value is unchanged.

**Common mistake:** Using `!` (non-null assertion) everywhere to silence TypeScript errors. Each `!` is a promise you're making — "I guarantee this is not null." If you're wrong, it crashes silently. Fix the actual type instead.

**One-liner:** `as Type` = override TS type opinion (no runtime effect). `!` = "trust me it's not null" (crashes if wrong). Both bypass safety — use sparingly.

---

### 🟡 2.10 The satisfies Operator (TS 4.9+)

**Explanation:** `satisfies` validates that a value matches a type without changing the inferred type. Unlike a direct type annotation (which widens the type), `satisfies` gives you validation AND preserves the specific literal types TypeScript infers.

```typescript
// ── Problem: annotation widens types ────────────────────────
const palette: Record<string, string> = {
  red:   "#FF0000",
  green: "#00FF00",
};
palette.red;      // type: string (wide)
palette.unknown;  // ✅ no error — any string key accepted (bad!)

// ── satisfies: validates AND preserves specific types ────────
const palette = {
  red:   "#FF0000",
  green: "#00FF00",
} satisfies Record<string, string>;

palette.red;      // type: "#FF0000" | string (specific) ✅
palette.unknown;  // ❌ Error: 'unknown' is not a key

// ── Practical React: typed theme config ─────────────────────
type ThemeColors = {
  primary:   string;
  secondary: string;
  danger:    string;
};

const theme = {
  primary:   "#3B82F6",
  secondary: "#1D4ED8",
  danger:    "#EF4444",
  typo:      "#000", // ❌ Error: 'typo' not in ThemeColors — caught!
} satisfies ThemeColors;

theme.primary; // type: string ✅ (not widened to any)
```

**Interview Q: What is the `satisfies` operator and how does it differ from a type annotation?** 🟡

| | Annotation `: Type` | `satisfies Type` |
|---|---|---|
| Validates shape | ✅ | ✅ |
| Keeps specific types | ❌ (widens to Type) | ✅ (preserves inference) |
| Unknown key check | ❌ | ✅ |

**A:** A type annotation widens the type to the annotated type, losing specific literal types. `satisfies` validates against the type but keeps the more specific inferred types. You get both validation and specificity — the best of both worlds.

**Aha moment:** `satisfies` is "validate against this type, but keep my specific inferred types." A direct annotation says "treat this exactly as this type." The difference matters when you need both safety and access to specific subtypes.

**Common mistake:** Not knowing `satisfies` exists and writing complex type assertions or duplicate type definitions to work around the limitations of plain annotations.

**One-liner:** `satisfies T` = validates shape matches T AND keeps specific inferred types. Better than annotation when you want both safety and specificity.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 3: Type Inference

---

### 🔴 3.1 How TypeScript Infers Types Automatically

**Explanation:** TypeScript's inference engine reads your code and determines types from context — from initialized values, return statements, array contents, and how values are used. You don't need to annotate everything; TypeScript fills in the gaps intelligently.

```typescript
// TypeScript infers ALL of these — no annotations needed
const name   = "Alice";           // inferred: string
const age    = 30;                // inferred: number
const active = true;              // inferred: boolean
const scores = [1, 2, 3];        // inferred: number[]
const user   = { name: "Alice", age: 30 }; // inferred: { name: string; age: number }

// Function return type — inferred from the return statement
function double(n: number) {
  return n * 2; // return type inferred: number
}
const result = double(5); // result inferred: number

// Array method callbacks — deeply inferred
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);    // n: number, doubled: number[]
const evens   = numbers.filter(n => n % 2 === 0); // evens: number[]
const sum     = numbers.reduce((acc, n) => acc + n, 0); // sum: number

// Destructuring — types flow through
const { name, age } = user; // name: string, age: number

// Where inference FAILS — annotate these:
const items = [];    // ❌ inferred as never[] (no elements to infer from)
const data  = null;  // ❌ inferred as null (can't become User later)
```

**Interview Q: What is type inference in TypeScript? When does it fail?** 🔴
**A:** Type inference means TypeScript automatically determines types from context without explicit annotations. It works from initialized values, return statements, and usage context. It fails when there's no initialization context: function parameters (TypeScript can't see future callers), empty arrays (no elements to learn from), and variables declared without initial values. Those cases require explicit annotations.

**Aha moment:** Professional TypeScript code uses surprisingly few explicit type annotations. TypeScript infers through complex chains of operations. The annotation-heavy code you see in tutorials is often over-engineered.

**Common mistake:** Annotating every single variable. `const count: number = 0` has redundant noise — TypeScript already knows from `0`. Over-annotating slows you down and creates maintenance burden.

**One-liner:** TypeScript infers types from values, returns, and usage. Only annotate where it can't: function params, empty arrays, late-assigned variables.

---

### 🔴 3.2 When to Annotate vs Let TypeScript Infer

**Explanation:** A practical rule: let TypeScript infer local variables and most return types. Explicitly annotate function parameters (always), empty arrays, nullable state, and public API boundaries where the type is documentation.

```typescript
// ✅ LET TypeScript INFER these
const count    = 0;                          // obvious: number
const message  = "hello";                   // obvious: string
const isActive = user !== null;             // inferred: boolean from expression
const doubled  = [1,2,3].map(n => n * 2);  // inferred: number[]

// Callback parameters — inferred from the array's type
["a", "b"].forEach(s => s.toUpperCase()); // s inferred as string
[1, 2, 3].map(n => n * 2);               // n inferred as number

// ✅ EXPLICITLY ANNOTATE these
// 1. Function parameters — TS can't infer what callers will pass
function greet(name: string, age: number): string {
  return `${name} is ${age}`;
}

// 2. Empty arrays — no elements to learn element type from
const tags: string[] = [];
const users: User[]  = [];

// 3. State that starts null but becomes typed
const [user, setUser]   = useState<User | null>(null);
const [error, setError] = useState<string | null>(null);

// 4. Public API function return types — acts as documentation
export function parseUser(json: string): User {
  return JSON.parse(json); // TS verifies the return matches User
}

// 5. When you want a wider type than the literal infers
let direction: "left" | "right" = "left"; // can be reassigned to "right"
// vs: const direction = "left" → inferred as literal "left"
```

**Interview Q: When do you need explicit type annotations in TypeScript?** 🔴
**A:** You need explicit annotations when TypeScript can't infer: function parameters (TypeScript doesn't know what callers will pass), empty arrays (no elements to infer from), variables declared without initialization, and state that starts as null but will hold typed data. For public/exported functions, explicit return types are good documentation practice even when TypeScript could infer them.

**Aha moment:** The rule experienced developers use: "annotate at boundaries, infer in the middle." Boundaries are function parameters and public API return types. Everything else should infer.

**Common mistake:** Treating TypeScript like Java — annotating every variable. Real TypeScript is light on annotations and relies heavily on inference.

**One-liner:** Annotate: function params, empty arrays, nullable state, public APIs. Infer: local variables, callbacks, return types of simple functions.

---

### 🔴 3.3 Contextual Typing — Callbacks and Event Handlers

**Explanation:** TypeScript can infer types from how a value is used, not just from what it's initialized to. The most important case: callback function parameters automatically get their types from the function that calls them. React event handlers in JSX get their types for free.

```typescript
// Array method callbacks — parameters inferred from the array type
const numbers = [1, 2, 3];

numbers.forEach(num => {
  num.toFixed(2);    // ✅ TypeScript knows num is number — no annotation needed
  num.toUpperCase(); // ❌ Error — TypeScript knows number has no .toUpperCase()
});

numbers.map(n => n * 2);        // n: number — inferred
numbers.filter(n => n > 0);     // n: number — inferred
numbers.reduce((acc, n) => acc + n, 0); // acc: number, n: number — inferred

// ── React JSX — the most useful contextual typing ────────────
function SearchInput() {
  return (
    <input
      onChange={e => {
        // e is AUTOMATICALLY inferred as React.ChangeEvent<HTMLInputElement>
        // because <input> onChange implies this type
        console.log(e.target.value); // ✅ TypeScript knows .target.value is string
        e.target.checked; // ❌ Error — .checked is for checkboxes, not inputs
      }}
      onKeyDown={e => {
        // e automatically inferred as React.KeyboardEvent<HTMLInputElement>
        console.log(e.key);     // ✅ .key exists on KeyboardEvent
        console.log(e.clientX); // ❌ Error — .clientX is on MouseEvent, not KeyboardEvent
      }}
    />
  );
}
// You NEVER need to annotate inline JSX event handlers!
```

**Interview Q: What is contextual typing? Why does it matter for React developers?** 🔴
**A:** Contextual typing is when TypeScript infers the type of an expression based on how it's used — specifically from the expected type provided by the surrounding context. For React, this means inline event handlers automatically get the correct event type from the JSX element. You never need to write `(e: React.ChangeEvent<HTMLInputElement>) =>` inline — TypeScript already knows.

**Aha moment:** You almost never need to manually type React event handlers when they're written inline in JSX. This is TypeScript's contextual typing working silently for you. It's one of the most practically useful TypeScript features in React development.

**Common mistake:** Manually annotating inline JSX callbacks. Writing `onChange={(e: React.ChangeEvent<HTMLInputElement>) => ...}` is redundant — TypeScript already knows the type from the JSX element context.

**One-liner:** Contextual typing = TypeScript infers callback types from the calling context. Inline JSX event handlers get types automatically — no annotation needed.

---

### 🟡 3.4 Type Widening — Why "left" Becomes string

**Explanation:** When TypeScript infers a type from a literal value, it often "widens" it to the general type. `"left"` assigned to a `let` variable becomes `string`, not the literal `"left"`. This causes errors when you try to pass it to a function expecting a specific literal type.

```typescript
// const — TypeScript infers the LITERAL type (value can't change)
const direction = "left"; // type: "left" (literal)

// let — TypeScript WIDENS to the general type (value can change)
let direction = "left";   // type: string (wide — could be "anything" later)

// Why widening causes problems
type Direction = "left" | "right" | "up" | "down";
function move(dir: Direction) { /* ... */ }

const d1 = "left";    // type: "left" (literal)
move(d1);              // ✅ "left" is assignable to Direction

let d2 = "left";       // type: string (widened!)
move(d2);              // ❌ Error: 'string' is not assignable to '"left" | "right" | ...'

// Fixes:
// 1. Use const
const d3 = "left";
move(d3); // ✅

// 2. Explicit annotation
let d4: Direction = "left";
move(d4); // ✅

// 3. as const on objects (even const objects widen property types!)
const config = { direction: "left" };        // { direction: string } — widened!
const config2 = { direction: "left" } as const; // { readonly direction: "left" } — literal!
move(config.direction);  // ❌ string is not Direction
move(config2.direction); // ✅ "left" is Direction
```

**Interview Q: What is type widening and when does it cause problems?** 🟡
**A:** Widening is when TypeScript infers a broader type than the literal value. `let x = "left"` gives `x` type `string` (not `"left"`) because a `let` variable could be reassigned to any string. This causes errors when passing to functions expecting literal types. Fix: use `const`, add an explicit literal type annotation, or use `as const`.

**Aha moment:** Prefer `const` over `let` in TypeScript — not just for JavaScript best practices, but because `const` preserves literal types that are essential for typed APIs like component variant props.

**Common mistake:** Creating a `let` variable to pass to a function expecting a literal union type, then being confused by the error. The fix is almost always: use `const`.

**One-liner:** `const` = preserves literal types. `let` = widens to general type. Object properties widen too — use `as const` to freeze them.

---

### 🟡 3.5 as const — Const Assertions

**Explanation:** `as const` tells TypeScript to treat a value as deeply immutable with the most specific (literal) types. Object properties become `readonly`, strings stay as their exact literal type. This is the modern alternative to enums and the key to creating typed constant maps.

```typescript
// Without as const — wide, mutable types
const config = {
  host: "localhost",
  port: 8080,
  env:  "development",
};
// TypeScript infers: { host: string; port: number; env: string }
// Properties are mutable and types are wide
config.env = "production"; // ✅ TypeScript allows this

// With as const — literal, readonly types
const config = {
  host: "localhost",
  port: 8080,
  env:  "development",
} as const;
// TypeScript infers: { readonly host: "localhost"; readonly port: 8080; readonly env: "development" }
config.env = "production"; // ❌ Error: readonly

// ── The real power: extracting union types from objects ─────
const ROUTES = {
  home:     "/",
  about:    "/about",
  profile:  "/profile",
  settings: "/settings",
} as const;

type Route = typeof ROUTES[keyof typeof ROUTES];
// = "/" | "/about" | "/profile" | "/settings"

function navigate(path: Route) { /* ... */ }
navigate(ROUTES.home); // ✅ "/"
navigate("/about");    // ✅ literal also works
navigate("/invalid");  // ❌ TypeScript error — not a valid route

// ── As const on arrays ───────────────────────────────────────
const SIZES = ["sm", "md", "lg", "xl"] as const;
type Size = typeof SIZES[number]; // "sm" | "md" | "lg" | "xl"
```

**Interview Q: What does `as const` do and why is it preferred over enums?** 🟡
**A:** `as const` makes all properties readonly and preserves literal types instead of widening. It's preferred over enums because: it generates no extra JavaScript code (pure type-level), the object is still plain JS at runtime, the values are directly usable as strings, and you can extract a union type with `typeof OBJ[keyof typeof OBJ]`. Enums generate an actual object + reverse mapping in the compiled output.

**Aha moment:** `as const` + `typeof OBJ[keyof typeof OBJ]` is the modern TypeScript idiom for typed constant sets. Many real codebases have eliminated all enums in favor of this pattern.

**Common mistake:** Forgetting `as const` on objects and being confused why the properties have type `string` instead of specific literal types when you try to use them as discriminants.

**One-liner:** `as const` = all properties readonly + literal types frozen. Use `typeof OBJ[keyof typeof OBJ]` to extract the value union type.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 4: Functions in TypeScript

---

### 🔴 4.1 Typing Function Parameters and Return Types

**Explanation:** Every function parameter needs a type annotation — TypeScript cannot infer what callers will pass. Return types are inferred from the `return` statement but should be explicit for public/exported functions as documentation.

```typescript
// Parameters REQUIRED — return type inferred
function add(a: number, b: number) {
  return a + b; // TypeScript infers return: number
}

// Explicit return type — good practice for public functions
function formatCurrency(amount: number, currency: string): string {
  return `${currency}${amount.toFixed(2)}`;
}

// TypeScript catches wrong argument types AND wrong counts
add(1, 2);          // ✅
add("1", 2);        // ❌ Error: 'string' not assignable to 'number'
add(1, 2, 3);       // ❌ Error: Expected 2 arguments, got 3
add(1);             // ❌ Error: Expected 2 arguments, got 1

// Arrow function — same type annotation syntax
const multiply = (a: number, b: number): number => a * b;

// Typing the function variable itself
const greet: (name: string) => string = name => `Hello, ${name}`;

// Async function — return type wraps in Promise automatically
async function fetchUser(id: number): Promise<User> {
  const res = await fetch(`/api/users/${id}`);
  return res.json();
}
// Calling it: const user = await fetchUser(1) — user: User ✅
```

**Interview Q: Do you always need to annotate return types in TypeScript?** 🔴
**A:** No — TypeScript infers return types from `return` statements. But explicitly annotating is a good practice for exported/public functions because it: acts as documentation for callers, forces the implementation to match the contract, and catches bugs where you accidentally return the wrong type from one code path. For private/internal functions, letting TypeScript infer is fine.

**Aha moment:** In JavaScript, calling a function with the wrong number of arguments is silently fine. In TypeScript, it's a compile error. This catches entire categories of "forgot a required argument" bugs.

**Common mistake:** Forgetting that ALL parameters are required by default in TypeScript. Unlike JavaScript where missing arguments become `undefined`, TypeScript will error if you don't pass every non-optional parameter.

**One-liner:** Params always annotated. Return type: annotate public functions (documentation + contract), infer private functions.

---

### 🔴 4.2 Optional Parameters (?) and Default Parameters

**Explanation:** Add `?` after a parameter name to make it optional — callers can omit it, and TypeScript types it as `T | undefined` inside the function. Default parameters work the same as JavaScript and automatically make the parameter optional.

```typescript
// Optional parameter — ? makes it optional
function greet(name: string, title?: string): string {
  // title is: string | undefined inside the function
  return title ? `Hello, ${title} ${name}` : `Hello, ${name}`;
}
greet("Alice");          // ✅ "Hello, Alice"
greet("Alice", "Dr.");   // ✅ "Hello, Dr. Alice"

// Default parameter — optional for callers, always has value inside
function createUser(name: string, role: string = "user") {
  // role is always string here — default fills in if omitted
  return { name, role };
}
createUser("Alice");          // ✅ role = "user"
createUser("Alice", "admin"); // ✅ role = "admin"

// ⚠️ Required parameters must come BEFORE optional/default
function bad(a?: string, b: string) {}   // ❌ Error
function good(a: string, b?: string) {}  // ✅

// React component pattern — optional props with defaults
interface ButtonProps {
  label:     string;           // required
  onClick:   () => void;       // required
  disabled?: boolean;          // optional
  variant?:  "primary" | "secondary" | "danger"; // optional
}

function Button({
  label,
  onClick,
  disabled = false,     // default: false (never undefined in body)
  variant  = "primary", // default: "primary"
}: ButtonProps) {
  // disabled: boolean ✅   variant: "primary" | "secondary" | "danger" ✅
  return (
    <button onClick={onClick} disabled={disabled} className={`btn-${variant}`}>
      {label}
    </button>
  );
}
```

**Interview Q: What is the difference between `param?` and `param = defaultValue` in TypeScript?** 🔴

| | `param?` (optional) | `param = value` (default) |
|---|---|---|
| Caller can omit | ✅ | ✅ |
| Type inside function | `T \| undefined` | `T` (always has value) |
| Must handle undefined | ✅ Yes | ❌ No |
| React convention | Use in interface props | Use in destructuring |

**A:** `param?` makes the parameter optional — inside the function its type is `T | undefined` and you must handle both cases. `param = value` also makes it optional for callers, but the default ensures the function body always sees a defined `T`. For React prop interfaces, use `?`. For function bodies, use destructured defaults.

**Aha moment:** React's `function Button({ variant = "primary" }: ButtonProps)` is TypeScript's default parameter syntax. You use it every day in React components.

**Common mistake:** Using `?` and then accessing the value without handling `undefined`. TypeScript will flag `title.toUpperCase()` when `title: string | undefined`.

**One-liner:** `?` = optional (T | undefined inside). `= default` = optional (T always inside). `?` in interfaces, defaults in destructuring.

---

### 🔴 4.3 Rest Parameters: ...args: string[]

**Explanation:** Rest parameters collect all remaining arguments into a typed array. TypeScript annotates the rest parameter as an array type. It must always be the last parameter, and it replaces the untyped `arguments` object from JavaScript.

```typescript
// Rest parameter — collects remaining args into typed array
function joinStrings(separator: string, ...parts: string[]): string {
  // parts is string[] — all extra arguments
  return parts.join(separator);
}
joinStrings(", ", "Alice", "Bob", "Carol"); // "Alice, Bob, Carol"
joinStrings("-");                           // "" (empty array for parts — fine)

// Different element type
function logMessages(level: "info" | "warn" | "error", ...messages: string[]): void {
  console[level](...messages);
}

// Spreading an array into a rest function
const names = ["Alice", "Bob", "Carol"];
joinStrings(", ", ...names); // ✅ spread array into rest

// ── Why rest > arguments ─────────────────────────────────────
// Old JavaScript:
function oldSum() {
  // arguments exists but has type 'IArguments' — no proper typing!
  // doesn't work in arrow functions at all
  return Array.from(arguments).reduce((a, b) => a + b, 0);
}

// Modern TypeScript:
function sum(...numbers: number[]): number {
  // numbers is a real number[] array — fully typed!
  return numbers.reduce((acc, n) => acc + n, 0);
}
sum(1, 2, 3, 4, 5); // 15 ✅
```

**Interview Q: What is the difference between rest parameters and the `arguments` object?** 🔴
**A:** Rest parameters (`...args: T[]`) are a proper typed array — all array methods work and TypeScript types them fully. The `arguments` object is array-like (not a real array), doesn't work in arrow functions, and has no TypeScript type information. Rest parameters replace `arguments` completely in modern TypeScript.

**Aha moment:** Rest parameters fix one of JavaScript's oldest rough edges. TypeScript makes them even better by knowing exactly what type the collected elements will be.

**Common mistake:** Annotating rest params as a single value instead of an array: `...args: string` is wrong. It must be `...args: string[]`.

**One-liner:** `...args: string[]` collects remaining args into typed array. Must be last param. Replaces untyped `arguments` object.

---

### 🔴 4.4 Function Overloads

**Explanation:** Function overloads let you define multiple call signatures for a single function, where different input types produce different output types. TypeScript selects the correct signature at each call site — callers get precisely typed return values.

```typescript
// Overload signatures (no body — just type contracts)
function format(value: string): string;
function format(value: number): string;
function format(value: Date):   string;
// Implementation signature — must handle all overloads
function format(value: string | number | Date): string {
  if (typeof value === "string") return value.toUpperCase();
  if (typeof value === "number") return value.toFixed(2);
  return value.toLocaleDateString();
}

format("hello"); // TypeScript knows: returns string
format(3.14);    // TypeScript knows: returns string
format(new Date()); // TypeScript knows: returns string
format(true);    // ❌ no matching overload

// More useful: different inputs → different return SHAPES
function createElement(tag: "a"):      HTMLAnchorElement;
function createElement(tag: "canvas"): HTMLCanvasElement;
function createElement(tag: "input"):  HTMLInputElement;
function createElement(tag: string):   HTMLElement {
  return document.createElement(tag);
}

const link   = createElement("a");     // TypeScript knows: HTMLAnchorElement
const canvas = createElement("canvas"); // TypeScript knows: HTMLCanvasElement
link.href = "/page"; // ✅ .href only exists on HTMLAnchorElement — TypeScript knows!

// React hook overloads — optional parameter changes return type
function useLocalStorage(key: string):                  [string | null, (v: string) => void];
function useLocalStorage(key: string, initial: string): [string, (v: string) => void];
function useLocalStorage(key: string, initial?: string) {
  /* implementation */
}
const [val1] = useLocalStorage("theme");          // val1: string | null
const [val2] = useLocalStorage("theme", "dark");  // val2: string (never null!)
```

**Interview Q: When would you use function overloads in TypeScript?** 🔴
**A:** Use overloads when a function's return type genuinely depends on the type or presence of an input argument. Without overloads, callers would get a union return type and need to narrow it themselves on every call. Common real-world cases: DOM API functions (where the tag string determines the element type), hooks where optional arguments change the return type, and utility functions that handle multiple input formats differently.

**Aha moment:** `document.createElement("canvas")` returning `HTMLCanvasElement` is TypeScript overloads in action — that's how the DOM library is typed. Every time you get specific element types from DOM methods, overloads are responsible.

**Common mistake:** Using overloads when a generic function would be simpler and more general. If input and output have a consistent relationship, generics are cleaner.

**One-liner:** Overloads = multiple type signatures for one function. Use when input type determines return type. Implementation signature is private.

---

### 🟡 4.5 Typing Callbacks and Higher-Order Functions

**Explanation:** Functions that accept other functions need the callback's type specified. The type syntax mirrors JavaScript arrow function syntax but describes the shape instead of implementing it.

```typescript
// Callback type: (param: Type) => ReturnType
function applyToAll(arr: number[], transform: (n: number) => number): number[] {
  return arr.map(transform);
}
applyToAll([1, 2, 3], n => n * 2);  // [2, 4, 6] — callback inferred ✅

// Callback in an interface — very common in React
interface FormFieldProps {
  value:    string;
  onChange: (newValue: string) => void;     // callback type
  onBlur?:  () => void;                     // optional callback
  validate?: (value: string) => string | null; // returns error or null
}

// Higher-order function that returns a function
function createLogger(prefix: string): (message: string) => void {
  return message => console.log(`[${prefix}] ${message}`);
}
const warn = createLogger("WARN");
warn("something odd"); // [WARN] something odd

// Generic HOF — preserve the type through the transformation
function withLogging<T, R>(fn: (arg: T) => R): (arg: T) => R {
  return arg => {
    console.log("Calling with:", arg);
    const result = fn(arg);
    console.log("Result:", result);
    return result;
  };
}
const loggedDouble = withLogging((n: number) => n * 2);
loggedDouble(5); // logs then returns 10
```

**Interview Q: How do you type a function that accepts a callback in TypeScript?** 🟡
**A:** Use function type syntax as the parameter type: `callback: (param: Type) => ReturnType`. You can inline it directly in the parameter list or extract it as a named type alias: `type Handler = (event: MouseEvent) => void`. For generic callbacks that work with multiple types, use a generic type parameter.

**Aha moment:** Callback type syntax looks like an arrow function but it's a type description. `onChange: (value: string) => void` is NOT a function implementation — it describes what shape the onChange function must have.

**Common mistake:** Confusing function type syntax with an actual arrow function. `onChange: (value: string) => void` in an interface is a type. `onChange = (value) => console.log(value)` in a component is an actual function.

**One-liner:** Callback type: `(paramName: Type) => ReturnType`. Inline in parameter or extract as type alias. Same arrow syntax as a real function but it's only a type.

---

### 🟡 4.6 void vs never as Return Types

**Explanation:** `void` means the function runs to completion but doesn't return a useful value — its return is ignored. `never` means the function cannot complete normally at all. These look similar but represent fundamentally different execution models.

```typescript
// void — runs, finishes, no useful return value
function logError(message: string): void {
  console.error(`[ERROR] ${message}`);
  // returns undefined implicitly — that's fine for void
}

// All React event handlers return void
const handleSubmit: React.FormEventHandler<HTMLFormElement> = (e): void => {
  e.preventDefault();
  submitForm();
};

// never — execution never reaches the function's end
function assertNever(value: never, message: string): never {
  throw new Error(`${message}: ${JSON.stringify(value)}`);
}

function exhaustiveRedux(state: State, action: Action): State {
  switch (action.type) {
    case "INCREMENT": return { ...state, count: state.count + 1 };
    case "DECREMENT": return { ...state, count: state.count - 1 };
    default:
      // action.type is 'never' here — all cases handled
      // Add a new action type without a case? Error fires HERE
      return assertNever(action.type, "Unhandled action");
  }
}

// void vs never in practice:
const runForever = (): never => { while(true) {} };
const doNothing  = (): void  => { /* completes */ };
```

**Interview Q: What is the difference between `void` and `never` as function return types?** 🟡
**A:** `void` means the function returns normally but the return value is meaningless (implicitly `undefined`). `never` means the function cannot return normally at all — it always throws or loops forever. Practically, `never` in switch/if-else defaults is used for exhaustiveness checking: TypeScript errors there when a union case isn't handled.

**Aha moment:** `void` and `never` look similar but are opposite ideas. `void` = "got to the end, nothing to return." `never` = "never got to the end."

**Common mistake:** Annotating error-throwing helper functions as `void`. They should be `never` — `void` implies the function could return normally (it just doesn't return a useful value).

**One-liner:** `void` = runs, no useful return. `never` = never returns (throws/loops). Use `never` in switch defaults for exhaustiveness checks.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 5: Interfaces & Type Aliases

---

### 🔴 5.1 interface vs type — When to Use Which

**Explanation:** Both `interface` and `type` define the shapes of objects and are mostly interchangeable. The real differences are: `type` can describe any type (unions, primitives, tuples), while `interface` supports declaration merging. Most teams pick a convention and stick with it.

```typescript
// Both define the same object shape
interface User { id: number; name: string; email: string }
type User    = { id: number; name: string; email: string }

// Both are used identically
const user: User = { id: 1, name: "Alice", email: "a@test.com" };

// ── KEY DIFFERENCE 1: type handles non-object types ─────────
type ID       = string | number;        // union — interface can't do this
type Handler  = (event: Event) => void; // function type
type Pair     = [string, number];       // tuple type
type Nullable<T> = T | null;            // generic utility

// ── KEY DIFFERENCE 2: interface supports declaration merging ─
interface Window { myApp: AppConfig } // extends the existing Window type!
type Window = { myApp: AppConfig }   // ❌ Error: duplicate identifier

// ── KEY DIFFERENCE 3: extension syntax differs ────────────────
interface Animal    { name: string }
interface Dog extends Animal { breed: string }  // interface: extends

type Animal = { name: string }
type Dog    = Animal & { breed: string }          // type: intersection &

// ── Popular React convention ─────────────────────────────────
// interface → for object shapes: props, API responses, class contracts
// type → for unions, function types, utility types, and aliases
interface ButtonProps { label: string; onClick: () => void }
type ButtonVariant = "primary" | "secondary" | "danger"
type EventHandler  = (e: React.MouseEvent) => void
```

**Interview Q: What is the difference between `interface` and `type` in TypeScript?** 🔴

| Feature | `interface` | `type` |
|---|---|---|
| Object shapes | ✅ | ✅ |
| Union types | ❌ | ✅ |
| Primitive aliases | ❌ | ✅ |
| Declaration merging | ✅ | ❌ |
| Extension syntax | `extends` | `&` (intersection) |
| Best for | Object shapes, class contracts | Everything else |

**A:** For object shapes, they're equivalent — mostly a style choice. Key differences: `type` handles non-object types (unions, primitives, tuples) while `interface` supports declaration merging (you can reopen and extend it, which is how `@types` packages work). Convention: `interface` for object/class shapes, `type` for unions, functions, and utility types.

**Aha moment:** The `interface` vs `type` debate is largely a style preference for basic object shapes. The cases where the difference actually matters — declaration merging, non-object types — come up infrequently. Pick a convention and be consistent.

**Common mistake:** Randomly mixing `interface` and `type` for object shapes with no consistent reasoning. Pick one for objects and stick with it across the codebase.

**One-liner:** interface = object shapes + declaration merging. type = everything (unions, functions, utilities). For objects: pick one, be consistent.

---

### 🔴 5.2 Optional Properties: prop?: Type

**Explanation:** Adding `?` after a property name makes it optional in an interface or type. TypeScript types it as `T | undefined` when read, so you must handle the undefined case before using it.

```typescript
interface UserProfile {
  id:       number;  // required — always present
  name:     string;  // required — always present
  bio?:     string;  // optional — string | undefined when read
  avatar?:  string;  // optional
  age?:     number;  // optional
}

// Both are valid — optional props can be omitted
const minimal: UserProfile = { id: 1, name: "Alice" };
const full: UserProfile    = { id: 1, name: "Alice", bio: "Dev", age: 30 };

// Reading optional props — MUST handle undefined
function getBioLength(user: UserProfile): number {
  // return user.bio.length; // ❌ Error: user.bio might be undefined
  return user.bio?.length ?? 0; // ✅ optional chain + nullish coalescing
}

// React component with optional props
interface CardProps {
  title:       string;        // required
  subtitle?:   string;        // optional
  imageUrl?:   string;        // optional
  onDelete?:   () => void;    // optional callback
  className?:  string;        // optional styling
}

function Card({ title, subtitle, imageUrl, onDelete, className }: CardProps) {
  return (
    <div className={className}>
      {imageUrl && <img src={imageUrl} alt={title} />}
      <h2>{title}</h2>
      {subtitle && <p>{subtitle}</p>}
      {onDelete && <button onClick={onDelete}>Delete</button>}
    </div>
  );
}

<Card title="Hello" />  {/* ✅ — all optionals omitted */}
<Card title="Hello" subtitle="World" className="featured" />  {/* ✅ */}
```

**Interview Q: How do optional properties work, and how do you safely access them?** 🔴
**A:** Optional properties (`prop?`) are typed as `T | undefined` when read. Safe access patterns: optional chaining `user.bio?.toUpperCase()` returns `undefined` if `bio` is undefined; nullish coalescing `user.bio ?? "default"` provides a fallback; or explicit `if (user.bio)` guards. TypeScript errors if you access an optional property without handling the undefined case first.

**Aha moment:** Every prop with `?` in your React component interfaces is an optional property. TypeScript ensures callers can omit them, and that you handle the undefined case inside the component.

**Common mistake:** Accessing optional properties directly without checking: `user.bio.toUpperCase()`. This compiles to code that crashes when `bio` is undefined. Always use `?.` or check first.

**One-liner:** `prop?` = optional (T | undefined). Use `?.` for safe access, `?? "default"` for fallbacks, or `if (prop)` guards.

---

### 🔴 5.3 Readonly Properties

**Explanation:** The `readonly` modifier prevents a property from being reassigned after creation. It's a compile-time check — TypeScript warns about reassignment but the runtime JavaScript doesn't enforce it.

```typescript
interface User {
  readonly id:   number; // cannot change after creation
  name:          string; // can change
  readonly role: "user" | "admin"; // cannot change
}

const user: User = { id: 1, name: "Alice", role: "user" };
user.name = "Bob";    // ✅ fine — name is mutable
user.id   = 2;        // ❌ Error: Cannot assign to 'id' — it is read-only
user.role = "admin";  // ❌ Error: read-only

// Readonly<T> utility — applies readonly to ALL properties
type ReadonlyUser = Readonly<User>;
// { readonly id: number; readonly name: string; readonly role: "user" | "admin" }

// ⚠️ IMPORTANT: readonly is SHALLOW
interface Config {
  readonly settings: { darkMode: boolean }; // reference is readonly...
}
const config: Config = { settings: { darkMode: false } };
config.settings = { darkMode: true };  // ❌ can't replace the object
config.settings.darkMode = true;       // ✅ but CAN mutate the object's properties!

// For true immutability, use readonly recursively:
interface DeepConfig {
  readonly settings: Readonly<{ darkMode: boolean }>; // now truly immutable
}

// React: props are conceptually readonly — never mutate them
// This is enforced by convention, but you can make it explicit:
interface ButtonProps {
  readonly label:   string;
  readonly onClick: () => void;
}
```

**Interview Q: What does `readonly` do in TypeScript and what is its key limitation?** 🔴
**A:** `readonly` prevents property reassignment at compile time — TypeScript flags any code that tries to change a readonly property. Its key limitation is that it's shallow: `readonly arr: string[]` means you can't replace `arr` with a different array, but you can still `.push()` to it. For deep immutability, recursively apply `Readonly<T>` or use `readonly T[]` for arrays.

**Aha moment:** TypeScript's `readonly` is purely a compile-time concept — it has zero effect on the generated JavaScript. It's like `const` for object properties — a type-level constraint.

**Common mistake:** Thinking `readonly obj: SomeType` makes the entire object tree immutable. It only prevents reassigning `obj` — the inner properties are still mutable.

**One-liner:** `readonly` = no reassignment (compile-time only, shallow). Use `Readonly<T>` for all-properties. Use `readonly T[]` for immutable arrays.

---

### 🔴 5.4 Extending Interfaces and React's extends Pattern

**Explanation:** Interfaces can inherit properties from other interfaces using `extends`. The most important React pattern: extend `React.HTMLAttributes` or element-specific attribute types to make your component accept all native HTML attributes.

```typescript
// Basic extension
interface Animal { name: string; age: number }
interface Dog extends Animal { breed: string } // Dog has: name, age, breed

const dog: Dog = { name: "Rex", age: 3, breed: "Labrador" }; // all required ✅

// Extending multiple interfaces
interface Named  { name: string }
interface Dated  { createdAt: Date }
interface Tagged { tags: string[] }
interface BlogPost extends Named, Dated, Tagged {
  content: string;
}

// ── The most important React pattern ───────────────────────
// Extend native HTML attributes → accept ALL native button props automatically
interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  // Native button attributes included: disabled, onClick, type, form, name, value, aria-*, data-*...
  // Add only your custom props:
  variant?:   "primary" | "secondary" | "danger";
  isLoading?: boolean;
}

function Button({
  variant   = "primary",
  isLoading = false,
  children,
  ...rest   // all native button attributes
}: ButtonProps) {
  return (
    <button
      {...rest}                    // pass ALL native attrs through
      disabled={isLoading || rest.disabled}
      className={`btn btn-${variant}`}
    >
      {isLoading ? "Loading..." : children}
    </button>
  );
}

// Callers can now use any native button attribute
<Button onClick={save} disabled={pending} form="myForm" aria-label="Save" variant="primary" />
//       ↑ native       ↑ native          ↑ native       ↑ native         ↑ custom
```

**Interview Q: Why do component libraries extend `React.ButtonHTMLAttributes` and what does it enable?** 🔴
**A:** `React.ButtonHTMLAttributes<HTMLButtonElement>` contains every valid attribute a native `<button>` can receive. Extending it means your custom Button component automatically accepts `onClick`, `disabled`, `type`, `form`, `aria-*`, `data-*`, and every other native attribute — without listing them manually. Combined with `...rest` spreading, users get the full power of native buttons through your component.

**Aha moment:** This is the pattern every real component library uses. Once you understand it, you understand how design system components work — they extend native element types and spread the rest through.

**Common mistake:** Forgetting to spread `...rest` onto the underlying element after extending. The types are correct (callers CAN pass native attrs) but the values are lost (the attrs don't actually reach the element).

**One-liner:** `extends React.ButtonHTMLAttributes<HTMLButtonElement>` = all native button attrs included. Always spread `...rest` to forward them to the element.

---

### 🔴 5.5 Intersection Types: A & B

**Explanation:** Intersection types combine multiple types using `&` — the result must satisfy ALL properties from all types. Union is "one of these"; intersection is "all of these at once."

```typescript
// & combines — result must have ALL properties from both
type Named  = { name: string };
type Aged   = { age: number };
type Person = Named & Aged;     // must have name AND age

const person: Person = {
  name: "Alice",  // required (from Named)
  age:  30,       // required (from Aged)
  // missing either → ❌ Error
};

// Practical: combining feature sets
type Serializable = { serialize(): string };
type Loggable     = { log(): void };
type ApiService   = { name: string } & Serializable & Loggable;

// React HOC (Higher-Order Component) pattern
type WithAuthentication = { currentUser: User | null; isAdmin: boolean };
type EnhancedProps<T>   = T & WithAuthentication;

// ── Union vs Intersection — the key mental model ─────────────
type AB_Union        = { a: string } | { b: number }; // must have A OR B
type AB_Intersection = { a: string } & { b: number }; // must have A AND B

const u: AB_Union = { a: "hello" };     // ✅ just A
const i: AB_Intersection = { a: "hello", b: 42 }; // ✅ must have both

// ⚠️ Primitive intersections produce never
type Impossible = string & number; // never — can't be both simultaneously
```

**Interview Q: What is the difference between union types (`|`) and intersection types (`&`) in TypeScript?** 🔴

| | Union `A \| B` | Intersection `A & B` |
|---|---|---|
| Meaning | One of these types | All of these types |
| Mental model | OR | AND |
| Properties available | Only shared properties | ALL properties from both |
| Object must have | Properties of A or B | Properties of both A and B |

**A:** Union (`A | B`) means the value can be type A or type B. Intersection (`A & B`) means the value must simultaneously satisfy all properties of both A and B. Counterintuitively, intersection gives you MORE properties (stricter), while union gives you FEWER guarantees (more flexible).

**Aha moment:** The naming is confusing — "intersection" sounds like less but gives more properties. Think of it as AND (must have everything from all types).

**Common mistake:** `string & number` expecting some middle ground. There's no value that's simultaneously both — the result is `never` (impossible type).

**One-liner:** `A & B` = must satisfy ALL types (AND). `A | B` = must satisfy ONE type (OR). Intersection gives more properties, union gives more flexibility.

---

### 🟡 5.6 Index Signatures: [key: string]: Type

**Explanation:** Index signatures type objects where you know the shape of keys and values but not the exact property names. TypeScript's `Record<K, V>` is the cleaner modern alternative for most cases.

```typescript
// Index signature — dynamic property names
interface StringToNumber {
  [key: string]: number; // any string key maps to a number
}
const scores: StringToNumber = { alice: 95, bob: 87 };
scores["carol"] = 92; // ✅ any string key

// Mixing specific properties + index signature
// ⚠️ All named properties must match the index signature's value type!
interface Config {
  timeout: number;        // specific (must be number to match index)
  retries: number;        // specific
  [key: string]: number;  // any other keys also must be number
}

// ── Record<K, V> — cleaner modern alternative ────────────────
type ScoreMap    = Record<string, number>;
type ButtonStyles = Record<string, string>;

// With literal union key type — TypeScript requires ALL keys to exist
type Status = "active" | "inactive" | "pending";
const statusColors: Record<Status, string> = {
  active:   "#00FF00",
  inactive: "#888888",
  pending:  "#FFA500",
  // missing "active"? → ❌ TypeScript error
  // extra "unknown"?  → ❌ TypeScript error
};
statusColors.active;  // type: string — TypeScript knows this key exists

// React: CSS class mapping, theme tokens
const variantClasses: Record<"primary" | "secondary" | "danger", string> = {
  primary:   "bg-blue-500 text-white",
  secondary: "bg-gray-200 text-gray-800",
  danger:    "bg-red-500 text-white",
};
```

**Interview Q: When would you use an index signature vs `Record<K, V>`?** 🟡
**A:** `Record<K, V>` is cleaner and preferred for most cases. When the key type is a literal union, `Record` enforces that all keys must be present — you can't forget one. Index signatures are for truly dynamic key sets where you can't enumerate all possible keys ahead of time. If you know all the possible keys, use `Record` with a union key.

**Aha moment:** `Record<"active" | "inactive", string>` is a total map — TypeScript errors if you miss a key or add a key not in the union. It's a type-safe lookup table for known sets of values.

**Common mistake:** Having named properties that conflict with the index signature type: `[key: string]: number` but also `name: string` — TypeScript rejects this because `string` doesn't match `number`.

**One-liner:** `Record<K, V>` preferred. With literal K = all keys required + no extras. Index signatures for truly dynamic keys.

---

### 🟡 5.7 Discriminated Union Pattern

**Explanation:** A discriminated union is a union of types where each member has a common literal property (the "tag" or "discriminant"). TypeScript uses that property to narrow to the exact type inside conditional branches, giving you access to type-specific properties.

```typescript
// Each type has a 'kind' property with a unique literal value
type LoadingState = { kind: "loading" };
type SuccessState = { kind: "success"; data: User[] };
type ErrorState   = { kind: "error";   message: string };
type EmptyState   = { kind: "empty" };

type AppState = LoadingState | SuccessState | ErrorState | EmptyState;

// TypeScript narrows to the exact type inside each case
function renderState(state: AppState) {
  switch (state.kind) {
    case "loading":
      return <Spinner />;
    case "success":
      return <UserList users={state.data} />; // ✅ state.data available
    case "error":
      return <ErrorBanner msg={state.message} />; // ✅ state.message available
    case "empty":
      return <EmptyView />;
  }
}

// React: perfect for form state machines
type FormState =
  | { status: "idle" }
  | { status: "submitting" }
  | { status: "success"; submittedAt: Date }
  | { status: "error";   error: string };

function ContactForm() {
  const [state, setState] = useState<FormState>({ status: "idle" });

  return (
    <form>
      {state.status === "error"   && <p className="error">{state.error}</p>}
      {state.status === "success" && <p>Sent at {state.submittedAt.toLocaleString()}</p>}
      <button disabled={state.status === "submitting"}>
        {state.status === "submitting" ? "Sending..." : "Send"}
      </button>
    </form>
  );
}
```

**Interview Q: What is a discriminated union and why is it better than boolean flags for state?** 🟡
**A:** A discriminated union uses a shared literal property to identify each state variant. Inside `state.status === "error"`, TypeScript narrows the entire state object to the error variant, giving typed access to `state.error`. Compare this to boolean flags (`isLoading`, `isError`, `isSuccess`) where TypeScript can't tell you which combination is valid. Discriminated unions also make impossible states unrepresentable — you can't be simultaneously `loading` and `error`.

**Aha moment:** Discriminated unions model state machines. The type system makes impossible states impossible to represent in code. This is one of the most powerful TypeScript patterns for building reliable UI components.

**Common mistake:** Using a non-literal for the discriminant. `status: string` doesn't work for narrowing — it must be specific literals like `"loading"`, `"error"`.

**One-liner:** Discriminated union = shared literal property enables precise narrowing per case. Makes impossible states unrepresentable. Best pattern for React component state.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 6: Union & Literal Types

---

### 🔴 6.1 Union Types: string | number

**Explanation:** Union types say "this value can be one of these types." You can only use methods and properties that exist on ALL types in the union without first narrowing. Narrowing (with `typeof`, `instanceof`, etc.) unlocks the full type-specific API.

```typescript
// Basic union
let id: string | number;
id = "abc123"; // ✅
id = 42;       // ✅
id = true;     // ❌ Error: boolean not in the union

// Before narrowing: only shared methods available
function formatId(id: string | number): string {
  id.toString();    // ✅ both string and number have .toString()
  id.toUpperCase(); // ❌ Error: number doesn't have .toUpperCase()
  id.toFixed(2);    // ❌ Error: string doesn't have .toFixed()
}

// After narrowing: full type-specific API
function formatId(id: string | number): string {
  if (typeof id === "string") {
    return id.toUpperCase(); // ✅ TypeScript knows: string
  }
  return id.toFixed(2); // ✅ TypeScript knows: number
}

// The most common union: nullable
function getDisplayName(user: User | null): string {
  if (user === null) return "Guest";
  return user.name; // ✅ TypeScript knows user is User (not null) here
}
const name = user?.name ?? "Guest"; // optional chaining shorthand

// React literal unions for typed props
interface AlertProps {
  type:     "info" | "warning" | "error" | "success"; // 4 valid values only
  message:  string;
  onClose?: () => void;
}
```

**Interview Q: What can you do with a union type value before narrowing it?** 🔴
**A:** Before narrowing, you can only use properties and methods that exist on ALL types in the union — the intersection of their APIs. After narrowing (with `typeof`, `instanceof`, equality checks, or type guards), TypeScript lets you use the full API of the specific narrowed type. This forces you to handle all possible types explicitly.

**Aha moment:** Union types are TypeScript's safe "this could be several things" — unlike JavaScript's anything-goes approach. TypeScript won't let you call a string method on something that might be a number. You must prove the type first.

**Common mistake:** Calling a type-specific method on a union without narrowing first. `id.toUpperCase()` on `string | number` errors because `number` doesn't have `.toUpperCase`. Narrow with `typeof` first.

**One-liner:** `A | B` = value is one of these. Only shared methods available before narrowing. Narrow with typeof/instanceof/in to unlock specific APIs.

---

### 🔴 6.2 Literal Types

**Explanation:** Literal types restrict a value to exact specific values — not just "a string" but "specifically this string." Combined with unions, they create a set of allowed values. This is the most common pattern in React component prop design.

```typescript
// String literal types — exact values only
type Direction   = "left" | "right" | "up" | "down";
type Variant     = "primary" | "secondary" | "danger" | "ghost";
type Environment = "development" | "staging" | "production";
type HttpMethod  = "GET" | "POST" | "PUT" | "DELETE" | "PATCH";

// TypeScript enforces exact values
function move(dir: Direction) { /* ... */ }
move("left");      // ✅
move("diagonal");  // ❌ Error: '"diagonal"' not assignable to Direction
// IDE autocomplete shows exactly 4 options — no guessing

// Numeric literal types
type DiceRoll   = 1 | 2 | 3 | 4 | 5 | 6;
type HttpStatus = 200 | 201 | 400 | 401 | 403 | 404 | 500;

// React: typed component APIs
interface ButtonProps {
  variant: "primary" | "secondary" | "danger";
  size:    "sm" | "md" | "lg";
}

const variantStyles = {
  primary:   "bg-blue-600 text-white",
  secondary: "bg-gray-200 text-gray-800",
  danger:    "bg-red-600 text-white",
} satisfies Record<ButtonProps["variant"], string>;

function Button({ variant, size }: ButtonProps) {
  return (
    <button className={`${variantStyles[variant]} px-${size === "lg" ? 6 : 4}`}>
      {/* TypeScript knows variant is one of 3 strings — safe indexing */}
    </button>
  );
}

// Caller gets autocomplete — TypeScript suggests "primary", "secondary", "danger"
<Button variant="primary" size="lg" />
<Button variant="invalid" size="lg" /> // ❌ Error — typo caught at compile time!
```

**Interview Q: What are literal types and what advantages do they provide over using plain `string`?** 🔴
**A:** Literal types restrict values to exact specific strings or numbers. Benefits over `string`: editor autocomplete shows valid options, typos are caught at compile time, the type itself documents valid values, and object indexing with the type is guaranteed safe. `variant: "primary" | "secondary" | "danger"` is infinitely more useful than `variant: string` for both type safety and documentation.

**Aha moment:** Literal types turn TypeScript into a built-in validation system for string constants. No more runtime errors from typos like "Primray" — TypeScript catches them when you type them.

**Common mistake:** Assigning a literal value to a `let` variable and passing it to a function expecting a literal type — `let` widens to `string`. Use `const` to preserve the literal type.

**One-liner:** Literal types restrict to exact values. Prevents typos, enables autocomplete, self-documents valid options. Use for props, state, config.

---

### 🔴 6.3 Narrowing with typeof

**Explanation:** TypeScript understands JavaScript's `typeof` operator at the type level. Inside a `typeof` check, TypeScript narrows the variable to that specific primitive type. Essential for handling union types containing different primitives.

```typescript
function process(value: string | number | boolean | null | undefined) {
  // value is: string | number | boolean | null | undefined

  if (typeof value === "string") {
    value.toUpperCase(); // ✅ TypeScript knows: string
    return `"${value.toUpperCase()}"`;
  }

  if (typeof value === "number") {
    value.toFixed(2);  // ✅ TypeScript knows: number
    return value.toFixed(2);
  }

  if (typeof value === "boolean") {
    return value ? "yes" : "no"; // ✅ TypeScript knows: boolean
  }

  // value is: null | undefined here
  return "empty";
}

// ── CRITICAL: typeof limitations ────────────────────────────
// typeof only narrows to these 7 type strings:
// "string" | "number" | "boolean" | "bigint" | "symbol" | "undefined" | "function"

typeof null      // "object" ← the famous JS bug — does NOT narrow to null!
typeof []        // "object" ← arrays are "object"!
typeof {}        // "object" ← plain objects too
typeof new Date() // "object" ← class instances too

// For these cases, use other narrowing:
value === null;      // for null specifically
Array.isArray(val);  // for arrays
val instanceof Date; // for class instances
"prop" in val;       // for specific object shapes
```

**Interview Q: What are the limitations of `typeof` for type narrowing?** 🔴
**A:** `typeof` only narrows to 7 primitive type strings. It cannot distinguish between `null`, arrays, plain objects, class instances, or Date objects — all return `"object"`. For these, use: `=== null` for null, `Array.isArray()` for arrays, `instanceof` for class instances, and the `in` operator for object shapes.

**Aha moment:** TypeScript's `typeof` narrowing is built on exactly what JavaScript's `typeof` returns at runtime — no new behavior, just TypeScript understanding existing JS patterns. The famous `typeof null === "object"` bug exists in TypeScript too.

**Common mistake:** `typeof value === "object"` expecting it to safely mean "it's an object (not null, not array)." It doesn't — `null` returns `"object"`. Always pair with `value !== null`.

**One-liner:** `typeof` narrows to 7 primitives. Does NOT distinguish: null, arrays, objects (all "object"). Use `=== null`, `Array.isArray()`, `instanceof` for those.

---

### 🔴 6.4 Narrowing with instanceof

**Explanation:** `instanceof` checks whether an object was created from a specific class constructor. TypeScript narrows to that class inside the check. The most critical use in React/TypeScript: narrowing errors in catch blocks.

```typescript
class Dog { breed: string; bark() { return "Woof!"; } }
class Cat { color:  string; meow() { return "Meow!"; } }

function interact(animal: Dog | Cat) {
  if (animal instanceof Dog) {
    animal.bark();  // ✅ TypeScript knows: Dog
    animal.breed;   // ✅ .breed available
  } else {
    animal.meow();  // ✅ TypeScript knows: Cat
    animal.color;   // ✅ .color available
  }
}

// ── Most important use: catch block error handling ───────────
async function saveUser(user: User) {
  try {
    await api.save(user);
  } catch (error: unknown) {
    // error is 'unknown' with strict mode — TypeScript forces you to check
    error.message;  // ❌ Error: cannot access .message on unknown

    if (error instanceof Error) {
      console.error(error.message); // ✅ TypeScript knows: Error
      console.error(error.stack);   // ✅ .stack available
    } else {
      console.error("Unknown error:", String(error));
    }
  }
}

// Custom error classes with instanceof
class ApiError extends Error {
  constructor(public statusCode: number, message: string) {
    super(message);
    this.name = "ApiError";
  }
}

function handleError(error: unknown) {
  if (error instanceof ApiError) {
    // TypeScript knows: statusCode, message, stack
    if (error.statusCode === 401) redirectToLogin();
    if (error.statusCode === 404) showNotFound();
  } else if (error instanceof Error) {
    showGenericError(error.message);
  }
}
```

**Interview Q: Why is `instanceof Error` essential in modern TypeScript error handling?** 🔴
**A:** With `useUnknownInCatchVariables` (enabled by `strict`), catch block errors are typed as `unknown` — not `any`. TypeScript won't let you access `.message` without first proving it's an `Error` instance. `error instanceof Error` narrows from `unknown` to `Error`, enabling safe property access. This prevents a whole class of "error handler itself throws an error" bugs.

**Aha moment:** In older TypeScript (and many tutorials), catch errors were typed as `any`. Modern TypeScript with `strict` makes them `unknown` — a much safer default that forces you to handle errors properly.

**Common mistake:** `instanceof` doesn't catch thrown strings. `throw "Something failed"` creates a string error, not an `Error` instance. Always handle both cases: `instanceof Error` for proper errors, and `else` for everything else.

**One-liner:** `instanceof` narrows class instances. Essential for `catch (error: unknown)` — must `instanceof Error` before accessing `.message`.

---

### 🔴 6.5 Narrowing with the in Operator

**Explanation:** The `in` operator checks whether a property exists on an object. TypeScript narrows between union types that have different properties — perfect for plain object types where `instanceof` doesn't apply.

```typescript
interface Cat { name: string; meow(): void }
interface Dog { name: string; bark(): void }

function interact(animal: Cat | Dog) {
  animal.name; // ✅ both have name — no narrowing needed

  if ("meow" in animal) {
    animal.meow(); // ✅ TypeScript knows: Cat
  } else {
    animal.bark(); // ✅ TypeScript knows: Dog
  }
}

// Real-world: API response discrimination
type ApiSuccess = { success: true;  data: User[] };
type ApiError   = { success: false; error: string };
type ApiResult  = ApiSuccess | ApiError;

function handleResult(result: ApiResult) {
  if (result.success) {
    // TypeScript narrows to ApiSuccess
    console.log(result.data.length); // ✅ .data available
  } else {
    // TypeScript narrows to ApiError
    console.log(result.error); // ✅ .error available
  }
}

// React: checking for optional render props
interface WithChildren { children: React.ReactNode }
interface WithRender   { render: () => React.ReactNode }
type Renderable = WithChildren | WithRender;

function Layout(props: Renderable) {
  if ("render" in props) {
    return <div>{props.render()}</div>;  // ✅ TypeScript knows: WithRender
  }
  return <div>{props.children}</div>;  // ✅ TypeScript knows: WithChildren
}
```

**Interview Q: When would you use `in` for narrowing vs `typeof` or `instanceof`?** 🔴
**A:** Use `in` when narrowing between union types made of plain objects (not class instances). `typeof` works for primitives; `instanceof` works for class instances; `in` works for any object type with distinguishing properties. It's ideal for API response types, discriminated union alternatives, and render prop patterns.

**Aha moment:** `in` is the answer to "how do I tell these two plain object types apart when they both have some properties in common?" The distinguishing property narrows to the specific type.

**Common mistake:** Checking for a property that exists on ALL types in the union — `"name" in animal` won't narrow between Cat and Dog if both have `name`. The checked property must uniquely identify one type.

**One-liner:** `"prop" in obj` narrows object union types by property presence. Use when typeof/instanceof don't apply (plain objects, not class instances).

---

### 🔴 6.6 Type Guards — The is Keyword

**Explanation:** A type guard function uses the `value is Type` return type to tell TypeScript "if this function returns true, the argument is narrowed to this specific type." This lets you extract and reuse complex narrowing logic.

```typescript
// Basic type guard
function isString(value: unknown): value is string {
  return typeof value === "string";
}

function process(value: unknown) {
  if (isString(value)) {
    value.toUpperCase(); // ✅ TypeScript knows: string inside the block
  }
}

// Type guard for an interface
interface User { id: number; name: string; email: string }

function isUser(value: unknown): value is User {
  return (
    typeof value === "object" &&
    value !== null &&
    "id" in value &&
    "name" in value &&
    typeof (value as User).id === "number"
  );
}

async function loadUser(id: number) {
  const data = await fetch(`/api/users/${id}`).then(r => r.json());
  if (!isUser(data)) throw new Error("Invalid user response from API");
  return data.name; // ✅ TypeScript knows data is User here
}

// ── Most practical React use: typed array filtering ──────────
const items: (string | null | undefined)[] = ["Alice", null, "Bob", undefined, "Carol"];

// Without type guard — still (string | null | undefined)[]
const filtered1 = items.filter(item => item != null);
// TypeScript doesn't know that filter removed the nulls!

// With type guard — correctly narrows to string[]
const filtered2 = items.filter((item): item is string => item != null);
// filtered2: string[] ✅ — TypeScript knows all nulls/undefineds removed
```

**Interview Q: What is a type guard and what makes it different from a regular boolean check?** 🔴
**A:** A type guard is a function with return type `value is SomeType` (a type predicate). When called in a conditional and it returns `true`, TypeScript narrows the type inside that branch. A regular boolean function just returns `true`/`false` — TypeScript doesn't narrow based on the result. The `value is Type` syntax is what signals to TypeScript that a truthy return means type narrowing.

**Aha moment:** The filter pattern `items.filter((item): item is string => item != null)` is one of the most useful TypeScript tricks. Without the type predicate, `filter` returns the same union type — TypeScript doesn't automatically know nulls were removed.

**Common mistake:** Forgetting the type predicate syntax. Writing `(item): boolean => item != null` instead of `(item): item is string => item != null` — TypeScript won't narrow the array type without the predicate.

**One-liner:** Type guard `(x): x is Type => boolean` = reusable narrowing. TypeScript narrows in the true branch. Essential for typed `.filter()` results.

---

### 🟡 6.7 Exhaustiveness Checking with never

**Explanation:** By assigning the "leftover" type to `never` in a switch's default branch, TypeScript errors if any union case is unhandled. This is compile-time safety — add a new union member and forget to handle it, TypeScript tells you exactly where before you ship.

```typescript
type Shape = "circle" | "square" | "triangle";

function getArea(shape: Shape, size: number): number {
  switch (shape) {
    case "circle":   return Math.PI * size ** 2;
    case "square":   return size ** 2;
    case "triangle": return (size ** 2) / 2;
    default:
      // shape is 'never' here — all cases are handled
      // The assignment ONLY fails if a case is MISSING
      const _exhaustiveCheck: never = shape;
      throw new Error(`Unhandled shape: ${_exhaustiveCheck}`);
  }
}

// Now add "pentagon" to the union:
type Shape = "circle" | "square" | "triangle" | "pentagon";
// ❌ Error: Type '"pentagon"' is not assignable to type 'never'
// TypeScript tells you EXACTLY which switch to update!

// ── Reusable exhaustiveness utility ─────────────────────────
function assertNever(value: never, message?: string): never {
  throw new Error(message ?? `Unhandled case: ${JSON.stringify(value)}`);
}

// Usage in React discriminated union state
type LoadState = "idle" | "loading" | "success" | "error";
function renderIcon(state: LoadState) {
  switch (state) {
    case "idle":    return <IdleIcon />;
    case "loading": return <Spinner />;
    case "success": return <CheckIcon />;
    case "error":   return <ErrorIcon />;
    default:        return assertNever(state); // compile error if a case is missing
  }
}
```

**Interview Q: What is exhaustiveness checking and how do you implement it?** 🟡
**A:** Exhaustiveness checking ensures every case of a union type is handled in a switch. Assign the `default` branch value to `never` — TypeScript errors there if any union member is unhandled (because a non-exhausted union member can't be assigned to `never`). When you add a new union member later and forget to update all your switches, TypeScript tells you the exact locations at compile time.

**Aha moment:** This turns the TypeScript type system into your changelog enforcer. Extend a union type and TypeScript automatically tells you every switch statement that needs updating. No more "I added a new status but forgot to handle it in the dashboard" bugs.

**Common mistake:** Forgetting to put the `never` assignment or `assertNever()` call in the `default` branch. Without it, unhandled cases silently fall through to `undefined` behavior.

**One-liner:** Assign to `never` in default branch → compile error when new union cases unhandled. `assertNever(value)` utility makes it elegant.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 7: Generics

---

### 🔴 7.1 Generic Functions: function identity&lt;T&gt;

**Explanation:** Generics let you write functions that work with multiple types while preserving full type information. The `<T>` is a type parameter — a placeholder filled in at each call site. TypeScript infers `T` from the arguments, so you rarely specify it explicitly.

```typescript
// Without generics — loses type information
function getFirst(arr: any[]): any {
  return arr[0];
}
const first = getFirst([1, 2, 3]); // first: any — completely useless!

// With generics — preserves the type through the function
function getFirst<T>(arr: T[]): T | undefined {
  return arr[0];
}
const num  = getFirst([1, 2, 3]);         // num:  number | undefined ✅
const name = getFirst(["Alice", "Bob"]);  // name: string | undefined ✅

// TypeScript INFERS T — you almost never write it explicitly
getFirst<number>([1, 2, 3]); // explicit — valid but redundant
getFirst([1, 2, 3]);          // inferred — TypeScript sees it's number[]

// Multiple operations — types flow through
function identity<T>(value: T): T { return value; }
function swap<A, B>(pair: [A, B]): [B, A] { return [pair[1], pair[0]]; }

const swapped = swap([1, "hello"]); // type: [string, number] — perfectly inferred!

// React: generic list component
function List<T>({
  items,
  renderItem,
}: {
  items:       T[];
  renderItem:  (item: T, index: number) => React.ReactNode;
  emptyText?:  string;
}) {
  return (
    <ul>
      {items.map((item, i) => (
        <li key={i}>{renderItem(item, i)}</li>
      ))}
    </ul>
  );
}

// TypeScript infers T as User from items prop
<List
  items={users}
  renderItem={(user, i) => <span key={i}>{user.name}</span>}
  // user is User — TypeScript knows! No annotation needed ✅
/>
```

**Interview Q: What are generics and why are they better than using `any`?** 🔴
**A:** Generics are type parameters that let you write reusable code while preserving type safety. Unlike `any` which erases all type information, generics preserve the specific type through the function — the return type is correctly known based on what was passed in. TypeScript infers generic types from arguments, so they're ergonomic to use. `any` is a type-safety hole; generics are type-safe generalization.

**Aha moment:** `useState<T>`, `useRef<T>`, `useContext<T>`, Array's `.map()`, `.filter()`, `.reduce()` — all generic functions. You've been benefiting from generics every single day in React. Now you can write your own.

**Common mistake:** Using `any[]` as parameter type for "flexible" functions. This throws away type safety. Use `T[]` with a generic to be flexible AND type-safe.

**One-liner:** Generics `<T>` = type placeholder filled in at call site. Preserves types unlike `any`. TypeScript infers T from arguments — rarely need to write it explicitly.

---

### 🔴 7.2 Generic Interfaces and Type Aliases

**Explanation:** Interfaces and types can also be generic — their structure is parameterized by one or more types. This creates reusable type shapes. The most common React example: `ApiResponse<T>` wraps any data type with the same metadata structure.

```typescript
// Generic interface — reusable wrapper structure
interface ApiResponse<T> {
  data:      T;
  status:    number;
  message:   string;
  timestamp: string;
}

// Specialize for each endpoint
type UserResponse     = ApiResponse<User>;       // data: User
type UserListResponse = ApiResponse<User[]>;     // data: User[]
type PagedResponse<T> = ApiResponse<{ items: T[]; total: number; page: number }>;

// Usage
async function fetchUser(id: number): Promise<ApiResponse<User>> {
  const res = await fetch(`/api/users/${id}`);
  return res.json();
}
const response = await fetchUser(1);
response.data.name; // ✅ TypeScript knows data is User

// Generic utility types
type Maybe<T>     = T | null | undefined;
type Nullable<T>  = T | null;
type Optional<T>  = T | undefined;

// Result pattern — functional error handling
type Result<T, E = Error> =
  | { ok: true;  value: T }
  | { ok: false; error: E };

async function fetchUserSafe(id: number): Promise<Result<User>> {
  try {
    const res = await fetch(`/api/users/${id}`);
    if (!res.ok) throw new ApiError(res.status, "Fetch failed");
    return { ok: true, value: await res.json() };
  } catch (error) {
    return { ok: false, error: error instanceof Error ? error : new Error("Unknown") };
  }
}

const result = await fetchUserSafe(1);
if (result.ok) {
  console.log(result.value.name); // TypeScript knows: User ✅
} else {
  console.log(result.error.message); // TypeScript knows: Error ✅
}
```

**Interview Q: How does a generic `ApiResponse<T>` improve over separate response types for each endpoint?** 🔴
**A:** Without generics, you'd define `UserResponse`, `ProductResponse`, `OrderResponse`, etc. — all with the same metadata structure but different data types. With `ApiResponse<T>`, you define the structure once and instantiate it for each endpoint. If you add a field like `requestId` to responses, you add it in one place instead of many. TypeScript maintains the connection between the generic parameter and every property that uses it.

**Aha moment:** `Promise<T>`, `Array<T>`, `Partial<T>`, `Record<K, V>` — all the utility types you use every day are generic interfaces. Understanding how to write your own generics means you understand how TypeScript's entire utility type ecosystem works.

**Common mistake:** Defining nearly identical types for each endpoint instead of one generic `ApiResponse<T>`. This is the classic "copy-paste types" smell that generics solve.

**One-liner:** Generic interfaces `interface Foo<T>` create reusable shapes. `ApiResponse<User>` and `ApiResponse<string>` from one definition. Eliminates copy-paste types.

---

### 🔴 7.3 Generic Constraints: T extends SomeType

**Explanation:** Constraints limit which types can be used as a generic parameter. `<T extends SomeType>` means T must be, or extend, `SomeType`. Without constraints, TypeScript doesn't know what properties T has — you can't access any properties on an unconstrained T.

```typescript
// Without constraint — can't access any properties on T
function getLength<T>(arg: T): number {
  return arg.length; // ❌ Error: Property 'length' doesn't exist on type 'T'
}

// With constraint — T must have a .length property
function getLength<T extends { length: number }>(arg: T): number {
  return arg.length; // ✅ TypeScript knows T has .length
}
getLength("hello");    // ✅ T = string — strings have .length
getLength([1, 2, 3]);  // ✅ T = number[] — arrays have .length
getLength(42);         // ❌ Error: number doesn't have .length

// keyof constraint — THE most important pattern
function getProperty<T extends object, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key]; // perfectly type-safe — return type matches the property type
}
const user = { name: "Alice", age: 30, email: "a@test.com" };
const name  = getProperty(user, "name");  // name: string ✅
const age   = getProperty(user, "age");   // age: number ✅
const email = getProperty(user, "email"); // email: string ✅
getProperty(user, "phone"); // ❌ Error: "phone" is not a key of typeof user

// Extending an interface — reusable across similar shapes
interface HasId { id: number }

function findById<T extends HasId>(items: T[], id: number): T | undefined {
  return items.find(item => item.id === id);
}
// Works with ANY type that has an id property
const user    = findById(users, 1);     // User | undefined ✅
const product = findById(products, 5);  // Product | undefined ✅
const order   = findById(orders, 99);   // Order | undefined ✅
```

**Interview Q: What are generic constraints and why are they necessary?** 🔴
**A:** Constraints restrict what types a generic parameter can be. Without constraints, TypeScript treats T as potentially any type — you can't access any properties on it. `<T extends HasId>` means "T must have at least the properties defined in HasId." The `keyof` constraint pattern (`<T extends object, K extends keyof T>`) creates perfectly type-safe property accessors where the return type exactly matches the property's type.

**Aha moment:** `<T extends object, K extends keyof T>` is one of TypeScript's most powerful patterns. It creates a function that's safe for any object shape — and TypeScript knows the exact return type based on which key you access.

**Common mistake:** Over-constraining: `<T extends string>` when you could just use `string`. Constraints should be the minimum required to access what you need. Under-constraining: using `T` with no constraint and then trying to access properties — TypeScript (correctly) errors.

**One-liner:** `<T extends SomeType>` restricts T to types satisfying SomeType. Required whenever you need to access properties on T inside the function.

---

### 🔴 7.4 Multiple Type Parameters: &lt;T, K&gt;

**Explanation:** Generic functions can have multiple type parameters when you need to express relationships between different types — an input transforms into a different output type, or one type parameter must be a key of another.

```typescript
// Two type parameters: input T, output U (can be different)
function mapArray<T, U>(array: T[], transform: (item: T) => U): U[] {
  return array.map(transform);
}
const lengths = mapArray(["hello", "world"], str => str.length);
// T = string, U = number → lengths: number[] ✅

// Type-safe object merge
function merge<T extends object, U extends object>(first: T, second: U): T & U {
  return { ...first, ...second };
}
const merged = merge({ name: "Alice" }, { age: 30 });
merged.name; // ✅  merged.age; // ✅
// TypeScript knows: { name: string } & { age: number }

// T and K with dependent constraint
function pick<T extends object, K extends keyof T>(obj: T, keys: K[]): Pick<T, K> {
  const result = {} as Pick<T, K>;
  keys.forEach(k => { result[k] = obj[k]; });
  return result;
}
const user = { name: "Alice", age: 30, email: "a@test.com" };
const pub   = pick(user, ["name", "email"]);
// pub: { name: string; email: string } ✅
pick(user, ["invalid"]); // ❌ Error: "invalid" is not a key of user

// React: generic select component
interface SelectProps<T> {
  options:    T[];
  value:      T | null;
  onChange:   (value: T) => void;
  getLabel:   (option: T) => string;
  getValue:   (option: T) => string | number;
}
function Select<T>({ options, value, onChange, getLabel, getValue }: SelectProps<T>) {
  // T can be User, Product, or any type — stays fully typed
}
```

**Interview Q: When do you need multiple type parameters in a generic function?** 🔴
**A:** Use multiple type parameters when you need to express relationships between different types: the input and output are different types (`T` and `U` in `map`), or one type must be constrained by another (`K extends keyof T`). Array's built-in `.map()` uses `<T, U>` — the element type `T` transforms to result type `U`, which can be completely different.

**Aha moment:** Array's `.map()` is defined as `map<U>(fn: (value: T) => U): U[]`. That's why `[1, 2, 3].map(n => String(n))` correctly returns `string[]` instead of `number[]` — TypeScript infers `U = string` from the callback's return type.

**Common mistake:** Using a single `<T>` when input and output types should be different. `function transform<T>(input: T): T` means input and output are the same type. Use `<T, U>` when the transformation produces a different type.

**One-liner:** `<T, U>` for different input/output types. `<T, K extends keyof T>` for key/value relationships. TypeScript infers all params from arguments.

---

### 🟡 7.5 Default Type Parameters: &lt;T = DefaultType&gt;

**Explanation:** Generic type parameters can have defaults, used when the caller doesn't specify the type argument. This makes APIs more ergonomic — you can omit the type parameter when the default is sensible.

```typescript
// Generic with default
interface ApiResponse<T = unknown> {
  data:    T;
  status:  number;
  message: string;
}

// Specify T → uses it
const typed: ApiResponse<User> = { data: user, status: 200, message: "OK" };
typed.data.name; // TypeScript knows: User ✅

// Omit T → uses default (unknown)
const generic: ApiResponse = { data: anything, status: 200, message: "OK" };
// generic.data is unknown — must narrow before use

// State type with default
interface LoadState<T = undefined> {
  data:    T;
  loading: boolean;
  error:   string | null;
}
type EmptyState    = LoadState;        // data: undefined (default)
type UserState     = LoadState<User>;  // data: User
type UserListState = LoadState<User[]>;// data: User[]

// Hook with default type parameter
function useFetch<T = unknown>(url: string): { data: T | null; loading: boolean } {
  // implementation
}

const { data: userData }  = useFetch<User>("/api/user");   // data: User | null
const { data: genericData } = useFetch("/api/unknown");    // data: unknown | null (default)
```

**Interview Q: What are default type parameters and when are they useful?** 🟡
**A:** Default type parameters let callers omit the type argument and fall back to a sensible default. They're used in library APIs for ergonomics: `ApiResponse<User>` when you know the type, `ApiResponse` (defaults to `unknown`) when you don't. React's `Component<P = {}, S = {}>` uses defaults so you can extend it without specifying empty prop/state types.

**Aha moment:** React's class component signature `class Component<P = {}, S = {}>` uses default type parameters. That's why `class MyComponent extends React.Component` works without type arguments — P and S default to empty objects.

**Common mistake:** Expecting the default to apply when TypeScript CAN infer T from arguments. Defaults only apply when T is completely omitted. If TypeScript can infer T from a function argument, it uses the inferred type, not the default.

**One-liner:** `<T = DefaultType>` = fallback type when T not specified. Makes APIs ergonomic. React.Component uses `<P = {}, S = {}>` for this.

---

### 🟡 7.6 Conditional Types: T extends U ? X : Y

**Explanation:** Conditional types choose between two types based on a type-level condition. They work like ternary operators for types and are the mechanism behind TypeScript's built-in utility types like `NonNullable`, `ReturnType`, and `Awaited`.

```typescript
// Basic conditional type
type IsString<T> = T extends string ? "yes" : "no";
type A = IsString<string>;   // "yes"
type B = IsString<number>;   // "no"
type C = IsString<"hello">;  // "yes" — string literal extends string

// ── The utility types you already use are conditional types ──

// NonNullable — built from conditional types
type NonNullable<T> = T extends null | undefined ? never : T;
type X = NonNullable<string | null>;  // string (null removed)
type Y = NonNullable<User | null | undefined>; // User

// ReturnType — extract what a function returns
type ReturnType<T extends (...args: any[]) => any> =
  T extends (...args: any[]) => infer R ? R : never;

function getUser(): User { return user; }
type UserReturn = ReturnType<typeof getUser>; // User ✅

// Awaited — unwrap Promise
type Awaited<T> = T extends Promise<infer U> ? Awaited<U> : T;
type R = Awaited<Promise<User>>;         // User
type S = Awaited<Promise<Promise<string>>>; // string (recursive)

// ── Distribution: applies to each union member separately ───
type ToArray<T> = T extends any ? T[] : never;
type Result = ToArray<string | number>;
// = string[] | number[] (distributed — NOT (string | number)[])
```

**Interview Q: What are conditional types and how are they used in TypeScript's utility types?** 🟡
**A:** Conditional types select between types based on `T extends U ? X : Y`. TypeScript's `NonNullable<T>`, `ReturnType<T>`, `Parameters<T>`, and `Awaited<T>` are all implemented with conditional types. When applied to a union type, conditional types "distribute" — the condition is checked for each union member separately, which is what makes them powerful for type transformations.

**Aha moment:** Once you understand conditional types, you can understand — and build — any of TypeScript's utility types. They're the functions of the type system.

**Common mistake:** Not understanding distribution. `T extends any ? T[] : never` applied to `string | number` gives `string[] | number[]` (distributed individually), NOT `(string | number)[]`. This is often surprising.

**One-liner:** `T extends U ? X : Y` = type-level ternary. Distributes over unions. Powers NonNullable, ReturnType, Awaited, and all the utility types you use daily.

---

### 🟡 7.7 The infer Keyword

**Explanation:** `infer` declares a type variable inside a conditional type that TypeScript fills in from the pattern match — like destructuring at the type level. It's how TypeScript "reads inside" complex types to extract their component types.

```typescript
// Extract the return type of any function
type GetReturnType<T extends (...args: any[]) => any> =
  T extends (...args: any[]) => infer R ? R : never;

function fetchUsers(): Promise<User[]> { return api.getUsers(); }
type FetchResult = GetReturnType<typeof fetchUsers>; // Promise<User[]>

// Unwrap a Promise (what Awaited does)
type Unwrap<T> = T extends Promise<infer U> ? Unwrap<U> : T;
type A = Unwrap<Promise<string>>;           // string
type B = Unwrap<Promise<Promise<number>>>;  // number (recursive!)

// Extract array element type
type ElementType<T extends readonly any[]> = T extends readonly (infer E)[] ? E : never;
type NumEl = ElementType<number[]>;  // number
type StrEl = ElementType<string[]>;  // string

// Extract first argument type
type FirstParam<T extends (...args: any[]) => any> =
  T extends (first: infer F, ...rest: any[]) => any ? F : never;

function greet(name: string, age: number): void {}
type NameParam = FirstParam<typeof greet>; // string

// React: extract props type from a component
type ComponentProps<T extends React.ComponentType<any>> =
  T extends React.ComponentType<infer P> ? P : never;

import { Button } from "./Button";
type ButtonProps = ComponentProps<typeof Button>; // Button's props ✅
```

**Interview Q: What does the `infer` keyword do and where can it be used?** 🟡
**A:** `infer` declares a type variable within a conditional type that TypeScript infers from the matched pattern. It's like "pattern matching" for types — `T extends Promise<infer U>` says "if T is a Promise of something, call that something U." It can only be used in the `extends` clause of a conditional type. It's the mechanism behind `ReturnType`, `Parameters`, `Awaited`, and most sophisticated type utilities.

**Aha moment:** `infer` is how TypeScript "opens" complex types. `T extends Promise<infer U>` literally means "peek inside the Promise and capture what's inside as U." It's incredibly powerful for type-level programming.

**Common mistake:** Trying to use `infer` outside a conditional type. It only works inside `T extends SomeWrapper<infer R> ? R : fallback`. It's not a general type extraction mechanism.

**One-liner:** `infer R` in a conditional type = TypeScript fills in R from the pattern match. Used to extract inner types from wrappers like Promise, Array, or function signatures.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Quick Revision — Part 1 One-Liners

```
CATEGORY 1: TYPESCRIPT SETUP & TOOLING
  TypeScript = JavaScript + types. tsc checks + emits plain JS. Zero runtime cost
  tsconfig.json configures the compiler. "strict": true is non-negotiable — set on day 1
  strict: true = null safety + no implicit any + function type checks + more
  ts-node = run .ts directly in dev (no output files). Never use in production
  TypeScript = type correctness. ESLint = code quality. Both needed, different jobs
  Path aliases: configure in tsconfig.json (types) AND vite/webpack (runtime). Both required

CATEGORY 2: BASIC TYPES & ANNOTATIONS
  Three primitives: string, number, boolean — always lowercase. String/Number/Boolean = wrong
  strictNullChecks = null/undefined are separate types. Forces explicit null handling
  any = disables TypeScript entirely (avoid). unknown = safe "could be anything" (use this)
  void = function returns nothing useful. never = function never returns. never = exhaustiveness
  Annotate where TS can't infer: function params, empty arrays, nullable state
  string[] = Array<string> — identical. ALWAYS annotate empty arrays: const items: string[] = []
  Tuple = fixed-length typed positions. useState returns [value, setter] tuple
  String enums > numeric enums. as const object = modern enum alternative (no TS-specific code)
  as Type overrides TS opinion. ! removes null/undefined. Both bypass safety — use sparingly
  satisfies T = validates against T AND keeps specific inferred types

CATEGORY 3: TYPE INFERENCE
  TypeScript infers from values, returns, and context. Only annotate where it can't
  Annotate boundaries: function params, public APIs. Infer internals: local vars, callbacks
  Contextual typing: inline JSX handlers get event types automatically — no annotation needed
  const = preserves literal types. let = widens to general type. as const freezes object literals

CATEGORY 4: FUNCTIONS IN TYPESCRIPT
  Function params: always annotate. Return types: annotate public functions, infer private ones
  param? = optional (T | undefined inside). param = default = optional (T always inside)
  ...args: string[] = rest params as typed array. Must be last. Replaces untyped arguments
  Overloads = multiple type signatures, one implementation. Use when input type → return type
  void = runs and finishes, no useful return. never = never returns. Use never in switch default
  Callback type: (paramName: Type) => ReturnType. Same syntax as arrow fn but it's a type

CATEGORY 5: INTERFACES & TYPE ALIASES
  interface = objects + declaration merging. type = everything (unions, functions, tuples)
  prop? = optional (T | undefined). Use ?. for access, ?? for fallbacks, if() for guards
  readonly = no reassignment (compile-time, shallow). Use readonly T[] for immutable arrays
  extends React.ButtonHTMLAttributes = all native button attrs included. Always spread ...rest
  A & B = AND (must satisfy both, more properties). A | B = OR (one of, more flexible)
  Record<K, V> preferred over index signatures. Literal K = all keys required + no extras
  Discriminated unions = shared literal discriminant enables precise narrowing. Best for state

CATEGORY 6: UNION & LITERAL TYPES
  A | B = one of these types. Only shared methods before narrowing. Narrow for specific methods
  Literal types restrict to exact values. Prevents typos, enables autocomplete, self-documents
  typeof narrows 7 primitives only. typeof null = "object" (bug). Use === null for null checks
  instanceof narrows class instances. Essential: instanceof Error before accessing .message
  "prop" in obj narrows object unions. Use when typeof/instanceof don't apply
  Type guard (v): v is T = reusable narrowing. Essential for typed array .filter() results
  never in switch default = compile error when new union cases unhandled. Use assertNever()

CATEGORY 7: GENERICS
  <T> = type placeholder filled at call site. Preserves types unlike any. TS infers from args
  Generic interfaces create reusable shapes. ApiResponse<User> and ApiResponse<string> from one
  <T extends SomeType> restricts T. Required to access properties on T inside the function
  <T, U> for different input/output types. <T, K extends keyof T> for key/value relationships
  <T = Default> provides fallback type. React.Component<P = {}, S = {}> uses this pattern
  T extends U ? X : Y = type-level ternary. Distributes over unions. Powers utility types
  infer R in conditional type = TypeScript fills R from pattern match. Powers ReturnType/Awaited
```

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

*→ Continue to **Part 2**: Categories 8–14 (Classes, Utility Types, Typing React, Hooks, Events, Data Fetching, Mapped Types)*