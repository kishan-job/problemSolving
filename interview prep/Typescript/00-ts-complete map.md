# TypeScript Learning Map — For React Developers
## 20 Categories · 155 Sub-Topics · Classified by Priority

---

# Table of Contents

- [How to Read This Map](#how-to-read-this-map)
- [Phase 1: Foundations — JS → TS Mindset](#phase-1-foundations--js--ts-mindset)
  - [Category 1: TypeScript Setup & Tooling](#category-1-typescript-setup--tooling)
  - [Category 2: Basic Types & Annotations](#category-2-basic-types--annotations)
  - [Category 3: Type Inference](#category-3-type-inference)
  - [Category 4: Functions in TypeScript](#category-4-functions-in-typescript)
- [Phase 2: Core TypeScript — The Type System](#phase-2-core-typescript--the-type-system)
  - [Category 5: Interfaces & Type Aliases](#category-5-interfaces--type-aliases)
  - [Category 6: Union & Literal Types](#category-6-union--literal-types)
  - [Category 7: Generics](#category-7-generics)
  - [Category 8: Classes in TypeScript](#category-8-classes-in-typescript)
  - [Category 9: Utility Types](#category-9-utility-types)
- [Phase 3: React + TypeScript](#phase-3-react--typescript)
  - [Category 10: Typing React Components](#category-10-typing-react-components)
  - [Category 11: Typing Hooks](#category-11-typing-hooks)
  - [Category 12: Event & DOM Typing](#category-12-event--dom-typing)
  - [Category 13: Data Fetching & API Typing](#category-13-data-fetching--api-typing)
- [Phase 4: Advanced TypeScript](#phase-4-advanced-typescript)
  - [Category 14: Mapped & Template Types](#category-14-mapped--template-types)
  - [Category 15: Modules & Declaration Files](#category-15-modules--declaration-files)
  - [Category 16: Type Narrowing Deep Dive](#category-16-type-narrowing-deep-dive)
  - [Category 17: tsconfig Deep Dive](#category-17-tsconfig-deep-dive)
- [Phase 5: Ecosystem & Real-World Patterns](#phase-5-ecosystem--real-world-patterns)
  - [Category 18: State Management Typing](#category-18-state-management-typing)
  - [Category 19: Testing with TypeScript](#category-19-testing-with-typescript)
  - [Category 20: Build Tools & Integration](#category-20-build-tools--integration)
- [Summary Table](#summary-table)
- [The 80/20 — Learn These First](#the-8020--learn-these-first)
- [Prompt 2 — Ask for the Full Guide](#prompt-2--ask-for-the-full-guide)

---

# How to Read This Map

```
🔴 = Must-know   (daily use + asked in 90% of interviews — learn these FIRST)
🟡 = Should-know  (common projects + 50% of interviews — learn after 🔴)
🟢 = Good-to-know (senior / library author / architecture work — learn last)
```

> You already know JavaScript and React.
> This map shows you what TypeScript ADDS on top.
> Use [Prompt 2](#prompt-2--ask-for-the-full-guide) to get the full guide with explanations and code.

---

# Phase 1: Foundations — JS → TS Mindset

> **Goal:** Get TS running, learn to label things you already know, understand what "types" mean in practice.
> **Time:** ~2–3 weeks
> **Unlock:** Lets you write `.tsx` files without red squiggles.

---

## Category 1: TypeScript Setup & Tooling

```
🔴 Installing TypeScript, running tsc
🔴 tsconfig.json — what it is and the key options
🔴 strict mode — what it enables and why you need it
🔴 ts-node for running TS directly
🟡 ESLint + @typescript-eslint/parser plugin
🟡 Watch mode (tsc --watch, ts-node-dev)
🟢 Project references and composite builds
🟢 Path aliases (baseUrl + paths in tsconfig)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 2: Basic Types & Annotations

```
🔴 Primitive types: string, number, boolean
🔴 null and undefined — and how strictNullChecks changes everything
🔴 any vs unknown — the most important difference to learn first
🔴 void and never — what they mean and when they appear
🔴 Type annotations on variables: const name: string = "Alice"
🔴 Array types: number[] and Array<number>
🔴 Tuple types: [string, number]
🔴 Enum — string enums vs numeric enums
🟡 Type assertions: as keyword and the ! non-null assertion
🟡 satisfies operator (TS 4.9+)
🟡 object type vs Object vs {} — the differences
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 3: Type Inference

```
🔴 How TypeScript infers types automatically (you don't always need to annotate)
🔴 When to annotate vs when to let TS infer
🔴 Contextual typing — how callbacks and event handlers get typed automatically
🟡 Type widening — why "hello" becomes string
🟡 const assertions (as const) — freeze a value's type
🟡 Narrowing vs widening in conditional branches
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 4: Functions in TypeScript

```
🔴 Typing function parameters and return types
🔴 Optional parameters with ?
🔴 Default parameter values typed
🔴 Rest parameters: ...args: string[]
🔴 Function overloads — multiple signatures for one function
🟡 void vs never as return types
🟡 Typing callback functions and higher-order functions
🟡 The never return type — functions that throw or run forever
🟢 this parameter annotation in functions
🟢 Generic functions (preview — covered fully in Category 7)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Phase 2: Core TypeScript — The Type System

> **Goal:** Understand the constructs that make TypeScript powerful — interfaces, generics, unions, and utility types.
> **Time:** ~2–3 weeks
> **Unlock:** You can type any data shape and most interview questions live here.

---

## Category 5: Interfaces & Type Aliases

```
🔴 interface vs type — when to use which (the real answer)
🔴 Optional properties: name?: string
🔴 Readonly properties: readonly id: number
🔴 Extending interfaces with extends
🔴 Intersection types: TypeA & TypeB
🔴 Index signatures: [key: string]: number
🟡 Declaration merging — adding to an interface across files
🟡 Discriminated unions — a pattern, not a keyword
🟡 interface for objects, type for everything else (rule of thumb)
🟢 Recursive types — e.g., a tree node that contains itself
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 6: Union & Literal Types

```
🔴 Union types: string | number
🔴 Literal types: "left" | "right" | "center"
🔴 Narrowing with typeof: if (typeof x === "string")
🔴 Narrowing with instanceof: if (err instanceof Error)
🔴 Narrowing with in: if ("name" in obj)
🔴 Type guards — custom functions using the is keyword
🟡 Discriminated union pattern for state machines and API responses
🟡 Exhaustiveness checking: using never to catch unhandled cases
🟢 Assertion functions: asserts x is string
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 7: Generics

```
🔴 Generic functions: function identity<T>(arg: T): T
🔴 Generic interfaces and type aliases
🔴 Generic constraints: <T extends object>
🔴 Multiple type parameters: <T, K extends keyof T>
🔴 Default type parameters: <T = string>
🟡 Generic classes
🟡 Conditional types: T extends U ? X : Y
🟡 The infer keyword inside conditional types
🟡 Distributive conditional types
🟢 Variadic tuple types
🟢 Template literal types combined with generics
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 8: Classes in TypeScript

```
🔴 Class property types and constructor typing
🔴 Access modifiers: public, private, protected
🔴 readonly on class properties
🔴 Implementing an interface: implements MyInterface
🔴 Abstract classes and abstract methods
🟡 Parameter properties shorthand: constructor(private name: string)
🟡 Static members typed correctly
🟡 Class expressions typed
🟢 Mixins pattern in TypeScript
🟢 Decorators (experimental — used in NestJS, Angular)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 9: Utility Types

```
🔴 Partial<T> — make all properties optional
🔴 Required<T> — make all properties required
🔴 Readonly<T> — make all properties readonly
🔴 Pick<T, K> — keep only certain properties
🔴 Omit<T, K> — remove certain properties
🔴 Record<K, V> — object with known key/value types
🔴 Exclude<T, U> and Extract<T, U> — filter union types
🔴 NonNullable<T> — remove null and undefined
🟡 ReturnType<T> — extract a function's return type
🟡 Parameters<T> — extract a function's parameter types
🟡 Awaited<T> — unwrap a Promise type
🟡 ConstructorParameters<T>
🟢 InstanceType<T>
🟢 Building your own custom utility types
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Phase 3: React + TypeScript

> **Goal:** Apply your type knowledge directly to React — props, hooks, events.
> **Time:** ~2–3 weeks
> **Unlock:** You can work on any React+TS codebase confidently.

---

## Category 10: Typing React Components

```
🔴 Typing props with an interface or type alias
🔴 Function component return type: JSX.Element vs ReactElement vs ReactNode
🔴 children prop: ReactNode (the correct type)
🔴 Optional vs required props
🔴 Generic components: function List<T>({ items }: { items: T[] })
🔴 Default props with TypeScript
🟡 forwardRef with TypeScript: forwardRef<HTMLDivElement, Props>
🟡 ComponentProps<'div'> — extending native HTML element props
🟡 PropsWithChildren<T> utility
🟢 Polymorphic components (the "as" prop pattern)
🟢 Component prop patterns for compound components
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 11: Typing Hooks

```
🔴 useState<T> — when TS can't infer and you need to annotate
🔴 useRef — the three different types (DOM ref, mutable ref, null ref)
🔴 useReducer — typing state and action with discriminated unions
🔴 useContext — creating and typing a context correctly
🔴 Custom hooks — typing parameters and return values
🟡 useMemo and useCallback — when to type them explicitly
🟡 useImperativeHandle — typing the exposed handle
🟢 Generic custom hooks
🟢 Typing third-party hook libraries (react-hook-form, etc.)
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 12: Event & DOM Typing

```
🔴 React.ChangeEvent<HTMLInputElement> for input onChange
🔴 React.MouseEvent<HTMLButtonElement> for onClick
🔴 React.FormEvent<HTMLFormElement> for form onSubmit
🔴 React.KeyboardEvent for keyboard handlers
🔴 Typing event handler props: onClick: (e: React.MouseEvent) => void
🟡 RefObject<T> vs MutableRefObject<T> — when to use which
🟡 React.CSSProperties for inline style objects
🟡 Event.target vs currentTarget — typing the difference
🟢 Custom synthetic event types
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 13: Data Fetching & API Typing

```
🔴 Typing fetch: const res = await fetch<User>('/api/user')
🔴 Async function return types: Promise<User>
🔴 Error handling types in async/try-catch
🟡 Typing React Query (useQuery, useMutation) with generics
🟡 Axios with typed responses: axios.get<User>('/user')
🟡 Zod for runtime validation + automatic type inference
🟡 Type-safe API clients (tRPC concept)
🟢 OpenAPI → TypeScript type generation (openapi-typescript)
🟢 GraphQL + TypeScript (codegen)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Phase 4: Advanced TypeScript

> **Goal:** Write types that enforce architecture, build utilities, understand library typings.
> **Time:** ~2–3 weeks
> **Unlock:** Senior-level TS, reading complex library types, writing reusable typed utilities.

---

## Category 14: Mapped & Template Types

```
🟡 Mapped types: { [K in keyof T]: string }
🟡 keyof operator — get keys of a type as a union
🟡 typeof operator — get the type of a value
🟡 Template literal types: `on${Capitalize<string>}`
🟡 Intrinsic string manipulation: Uppercase, Lowercase, Capitalize, Uncapitalize
🟡 Modifiers in mapped types: +/- for readonly and optional
🟢 Remapping keys with the as clause
🟢 Deep Partial and DeepReadonly — building recursive utilities
🟢 Recursive types for nested data structures
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 15: Modules & Declaration Files

```
🔴 ES modules in TypeScript: import/export with types
🟡 Writing .d.ts declaration files for JS libraries
🟡 @types/* packages — how DefinitelyTyped works
🟡 Module augmentation — adding to an existing module's types
🟡 Ambient declarations: declare const, declare module
🟢 Publishing a typed npm package correctly
🟢 Triple-slash directives: /// <reference types="..." />
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 16: Type Narrowing Deep Dive

```
🔴 Control flow analysis — TS tracks types across if/else branches
🔴 User-defined type guards: function isUser(x: unknown): x is User
🟡 Assertion functions: function assert(x: unknown): asserts x is string
🟡 in operator narrowing for object shapes
🟡 Truthiness narrowing — how TS handles if (x)
🟢 never narrowing — exhaustive switch/case
🟢 Branded types — extra type safety with phantom types
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 17: tsconfig Deep Dive

```
🔴 strict — what the umbrella flag actually turns on
🔴 noImplicitAny — no untyped values allowed
🔴 strictNullChecks — null and undefined are separate types
🟡 target and lib — controlling JS output version
🟡 module and moduleResolution — how imports resolve
🟡 include, exclude, files — controlling which files TS compiles
🟡 baseUrl and paths — setting up import aliases
🟢 incremental and composite — faster builds
🟢 isolatedModules — required for Vite and SWC
🟢 exactOptionalPropertyTypes — stricter optional property handling
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Phase 5: Ecosystem & Real-World Patterns

> **Goal:** Connect TypeScript to the libraries and tools in your actual projects.
> **Time:** ~2–3 weeks (learn on-demand based on your stack)
> **Unlock:** Production-ready TypeScript across any React project.

---

## Category 18: State Management Typing

```
🔴 Redux Toolkit with TypeScript — RootState, AppDispatch, typed slices
🔴 Zustand — typing store state and actions
🟡 Jotai — typing atoms and derived atoms
🟡 Context + useReducer — the fully typed local state pattern
🟢 XState — typing state machines and events
🟢 Immer with TypeScript — typing draft state
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 19: Testing with TypeScript

```
🔴 Jest + ts-jest — running TypeScript tests
🔴 Typing mocks: jest.fn() and jest.mocked()
🟡 Vitest with TypeScript (faster, native ESM)
🟡 @testing-library/react types and typed queries
🟢 Type-level tests: tsd and expect-type
🟢 Mock Service Worker (MSW) with typed handlers
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Category 20: Build Tools & Integration

```
🔴 Vite + React + TypeScript (vite --template react-ts)
🟡 Next.js App Router with TypeScript
🟡 Webpack + ts-loader or babel-loader with @babel/preset-typescript
🟡 Setting up path aliases in Vite + tsconfig together
🟢 esbuild and SWC — transpile-only (skip type checking for speed)
🟢 Turborepo / monorepo TypeScript setup
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Summary Table

| # | Category | Sub-topics | Priority | Phase |
|---|----------|-----------|----------|-------|
| 1 | TypeScript Setup & Tooling | 8 | 🔴 Must-know | 1 |
| 2 | Basic Types & Annotations | 11 | 🔴 Must-know | 1 |
| 3 | Type Inference | 6 | 🔴 Must-know | 1 |
| 4 | Functions in TypeScript | 9 | 🔴 Must-know | 1 |
| 5 | Interfaces & Type Aliases | 10 | 🔴 Must-know | 2 |
| 6 | Union & Literal Types | 9 | 🔴 Must-know | 2 |
| 7 | Generics | 11 | 🔴🟡 Mixed | 2 |
| 8 | Classes in TypeScript | 10 | 🔴🟡 Mixed | 2 |
| 9 | Utility Types | 14 | 🔴🟡 Mixed | 2 |
| 10 | Typing React Components | 10 | 🔴 Must-know | 3 |
| 11 | Typing Hooks | 9 | 🔴🟡 Mixed | 3 |
| 12 | Event & DOM Typing | 9 | 🔴🟡 Mixed | 3 |
| 13 | Data Fetching & API Typing | 9 | 🔴🟡 Mixed | 3 |
| 14 | Mapped & Template Types | 9 | 🟡🟢 Mixed | 4 |
| 15 | Modules & Declaration Files | 7 | 🟡🟢 Mixed | 4 |
| 16 | Type Narrowing Deep Dive | 7 | 🔴🟡 Mixed | 4 |
| 17 | tsconfig Deep Dive | 10 | 🔴🟡 Mixed | 4 |
| 18 | State Management Typing | 6 | 🔴🟡 Mixed | 5 |
| 19 | Testing with TypeScript | 6 | 🔴🟡 Mixed | 5 |
| 20 | Build Tools & Integration | 6 | 🔴🟡 Mixed | 5 |
| | **TOTAL** | **175** | | 5 phases |

[↑ Back to Table of Contents](#table-of-contents)

---

# The 80/20 — Learn These First

> These 8 concepts unlock ~80% of real TypeScript React work.
> If you only have 1–2 weeks, learn ONLY these.

```
1. any vs unknown          → the single most important TS concept to grasp first
2. interface vs type       → how to describe data shapes (your daily tool)
3. Union types + narrowing → string | number, typeof guards, is keyword
4. Generics <T>            → the concept that unlocks everything else
5. Utility types           → Partial, Pick, Omit, Record (used in every codebase)
6. Typing props + children → ReactNode, optional props, extending HTML props
7. useState / useRef typed → the hooks you use in every component
8. Event handler types     → ChangeEvent, MouseEvent, FormEvent (daily React work)
```

```
PREPARATION TIME ESTIMATES:
  🔴 Must-know topics only              → 4–6 weeks  (minimum viable TS)
  🔴 + 🟡 Must + Should know (Phase 1–3) → 6–8 weeks  (solid working TS)
  🔴 + 🟡 + 🟢 Everything               → 10–14 weeks (thorough / senior level)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Prompt 2 — Ask for the Full Guide

> Now that you see the complete map, copy-paste a prompt below into a
> **new conversation** to get the full guide with explanations and code.

## Option A: Everything (full 20-category guide)

```
Based on these 20 categories of TypeScript learning topics
(175 sub-topics total), give me the COMPLETE learning guide.

I'm a React developer who knows HTML, CSS, JavaScript, and React.
I'm learning TypeScript to use it with React in real projects.

For EACH sub-topic include:
  - Clear explanation (2-3 sentences, simple language)
  - Code example showing how it works (TypeScript)
  - The most common interview question about it (with answer)
  - The "aha moment" — how this differs from plain JavaScript
  - Common mistake developers make when learning this
  - One-liner summary for quick revision

Organize by category. Within each category, order by priority:
  🔴 Must-know first, then 🟡 Should-know, then 🟢 Good-to-know.

CATEGORIES TO COVER (all 20):
  1. TypeScript Setup & Tooling
  2. Basic Types & Annotations
  3. Type Inference
  4. Functions in TypeScript
  5. Interfaces & Type Aliases
  6. Union & Literal Types
  7. Generics
  8. Classes in TypeScript
  9. Utility Types
  10. Typing React Components
  11. Typing Hooks
  12. Event & DOM Typing
  13. Data Fetching & API Typing
  14. Mapped & Template Types
  15. Modules & Declaration Files
  16. Type Narrowing Deep Dive
  17. tsconfig Deep Dive
  18. State Management Typing
  19. Testing with TypeScript
  20. Build Tools & Integration

FORMAT:
  - Single markdown document
  - Clickable table of contents at the top
  - Back-to-top links per section
  - All code in TypeScript with inline comments
  - Difficulty tag on each topic (🔴🟡🟢)
  - JS vs TS comparison tables where relevant

LEVEL: Know JS + React basics, learning TypeScript
STYLE: Practical, interview-focused. Each explanation should
connect to "how this gets asked in interviews" and
"how you'll use this in a real React project."
```

## Option B: Phase 1–3 Only (React-ready in 6–8 weeks)

```
Give me a TypeScript learning guide covering ONLY
Phases 1–3 (Categories 1–13) — the topics needed to
work confidently in a React + TypeScript codebase.

I'm a React developer. I know JavaScript well.

For each topic include:
  - Plain-English explanation
  - TypeScript code example with comments
  - How it's used in a React component (practical context)
  - Common mistake to avoid

CATEGORIES TO COVER (13):
  1. TypeScript Setup & Tooling
  2. Basic Types & Annotations
  3. Type Inference
  4. Functions in TypeScript
  5. Interfaces & Type Aliases
  6. Union & Literal Types
  7. Generics
  8. Classes in TypeScript
  9. Utility Types
  10. Typing React Components
  11. Typing Hooks
  12. Event & DOM Typing
  13. Data Fetching & API Typing

Single markdown, clickable TOC, TS code with comments.
React-developer perspective throughout.
```

## Option C: One Category at a Time

```
Give me the complete TypeScript learning guide for ONLY
[CATEGORY NAME] (e.g., "Generics" or "Typing Hooks").

Cover every sub-topic in that category:
  - Clear explanation with TypeScript code
  - React-specific example (where applicable)
  - Top 5 interview questions with detailed answers
  - Common mistakes and how to fix them
  - Tricky edge cases
  - One-liner revision notes

Markdown, code with comments, simple language.
Perspective: React developer learning TypeScript.
```

## Option D: The 80/20 Fast Track (1–2 weeks)

```
Give me a fast-track TypeScript guide covering ONLY
the 8 most important concepts for a React developer:

  1. any vs unknown
  2. interface vs type
  3. Union types + type narrowing (typeof, is)
  4. Generics <T> — functions, interfaces, components
  5. Utility types (Partial, Pick, Omit, Record)
  6. Typing React props and children
  7. Typing useState and useRef
  8. Typing event handlers (ChangeEvent, MouseEvent, FormEvent)

For each:
  - 2-sentence explanation
  - TypeScript code example
  - React usage example
  - The #1 interview question about it (with answer)
  - One-liner to remember it

Short, focused, practical. React developer perspective.
```

[↑ Back to Table of Contents](#table-of-contents)