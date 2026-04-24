# React Practice Guide — All Prompts + Study Plans (TypeScript)

---

## Table of Contents

- [Quick Decision — Which Prompt to Use](#quick-decision--which-prompt-to-use)
- [Prompt 1 — Interview Prep](#prompt-1--interview-prep)
- [Prompt 2 — Senior / Architecture](#prompt-2--senior--architecture)
- [Prompt 3 — Real Product Builder](#prompt-3--real-product-builder)
- [Master Prompt — All Purposes Combined](#master-prompt--all-purposes-combined)
- [What Each Prompt Includes](#what-each-prompt-includes)
- [How Many Days Each Level Takes](#how-many-days-each-level-takes)
- [Realistic Study Plans](#realistic-study-plans)
- [The Honest Truth](#the-honest-truth)
- [How to Generate Parts](#how-to-generate-parts)

---

## Quick Decision — Which Prompt to Use

| Your Situation | Use This |
|---|---|
| I have an interview coming up | Prompt 1 — Interview Prep |
| I want to think like a senior dev | Prompt 2 — Senior / Architecture |
| I want to build real portfolio features | Prompt 3 — Product Builder |
| I want one guide covering everything | Master Prompt |

[↑ Back to Table of Contents](#table-of-contents)

---

## Prompt 1 — Interview Prep

> **Use when:** Preparing for interviews, learning React with TypeScript, need to practice building specific typed features. You know exactly what you are practicing. "I need to build a typed form" → go to Pattern 2.

### How to Use This Prompt

Copy the prompt text block below and paste it into a new conversation. Then add your generation command at the end.

---

### Prompt 1 — Full Text

```
You are building a complete scenario-based React + TypeScript
practice guide for developers preparing for interviews.
Category names are task-based — what you BUILD, not theory.
All code must be written in TypeScript with proper types,
interfaces, and type safety throughout.

TYPESCRIPT REQUIREMENTS — APPLY TO ALL CODE

ALWAYS include:
- Interface or type for every prop
- Interface or type for every API response shape
- Return types on all functions and async functions
- Typed useState: useState<User | null>(null)
- Typed useRef: useRef<HTMLInputElement>(null)
- Typed event handlers: (e: React.ChangeEvent<HTMLInputElement>)
- Typed fetch responses with generic or cast
- Union types for status: 'idle' | 'loading' | 'success' | 'error'
- Optional chaining and nullish coalescing where appropriate
- No use of `any` — use `unknown` and narrow it

NEVER use:
- Plain JavaScript without types
- `any` type
- Untyped state: useState([]) without generic
- Untyped refs: useRef() without generic
- Untyped event handlers: (e) => without type annotation

FULL STRUCTURE — ALL 4 LEVELS

LEVEL 1 — FOUNDATION (80% of interview questions)
  Pattern 1:  Fetch & Display Data
  Pattern 2:  Form + Validation
  Pattern 3:  List + Search + Filter
  Pattern 4:  localStorage Usage
  Pattern 5:  Show/Hide UI
  Pattern 6:  Page Navigation

LEVEL 2 — INTERMEDIATE (15% of interview questions)
  Pattern 7:  Pagination / Infinite Scroll
  Pattern 8:  Toast / Notifications
  Pattern 9:  Shopping Cart
  Pattern 10: Dark Mode / User Preferences
  Pattern 11: Auth Flow

LEVEL 3 — ADVANCED (4% of interview questions)
  Pattern 12: Performance Optimization
  Pattern 13: Real-time / Polling
  Pattern 14: Complex Async

LEVEL 4 — SPECIALIST (1% — senior interviews only)
  Pattern 15: Memory Leaks
  Pattern 16: Stale Closures
  Pattern 17: Infinite Render Loops
  Pattern 18: Token Expiry + Silent Refresh
  Pattern 19: Context Performance at Scale

FOR EACH PATTERN HEADER USE THIS FORMAT:

## Pattern [N]: [Emoji] [Name]
> Level [1/2/3/4]

What you will build in this pattern:
- [real feature 1]
- [real feature 2]
- [real feature 3]

Interview asks this as:
- [exact phrasing 1]
- [exact phrasing 2]
- [exact phrasing 3]

TypeScript concepts used in this pattern:
- [TS concept 1]
- [TS concept 2]

Most common mistake in interviews:
[The one thing that makes interviewers mark you down]

FOR EACH SCENARIO USE THIS FORMAT:

### Pattern [N] · Scenario [N] — [Problem Title]

Situation:
[2-3 lines. Real company. Real task.]

Types used in this scenario:
[All interfaces and types defined before the code]

Here is the starter code / broken code:
[Realistic TypeScript + React code]

Your tasks:
- [specific thing to build or fix]
- [specific TypeScript type to add or fix]
- [specific thing to build or fix]

<details>
<summary>Hint</summary>
[One key insight. Logic + TypeScript direction.]
</details>

<details>
<summary>Full Solution</summary>
[Complete working TypeScript + React code]

What the interviewer is checking:
[Logic they want + Types they want]

TypeScript lesson:
[The specific TS concept this scenario teaches]
</details>

SCENARIOS PER PATTERN:
  Level 1 → 4 scenarios each
  Level 2 → 4 scenarios each
  Level 3 → 3 scenarios each
  Level 4 → 2 scenarios each

RULES:
- All code is TypeScript — zero plain JavaScript
- No `any` type anywhere
- Scenarios go: easy → medium → hard → tricky edge case
- React functional components + hooks only
- async/await not .then() chains
- Clickable TOC at top of generated output
- Back to TOC link after every pattern
- Level dividers between levels
- Each scenario shows at least one real TypeScript pattern
- End with Interview Pattern Picker — at least 20 entries

GENERATION COMMANDS:
To start generating, add one of these at the end:
"Generate Part 1 — Level 1, Patterns 1 to 6"
"Generate Part 2 — Level 2, Patterns 7 to 11"
"Generate Part 3 — Level 3, Patterns 12 to 14"
"Generate Part 4 — Level 4, Patterns 15 to 19"

Or one pattern only:
"Generate only Pattern [N] — [Name]. All scenarios. Complete. No placeholders."

OUTPUT: Complete markdown. No placeholders.
Every scenario fully written in TypeScript.
Every solution working with full type safety.
```

---

### Structure Overview — Prompt 1

**Level 1 — Foundation** *(80% of interview questions)*

| Pattern | Name |
|---|---|
| Pattern 1 | 📋 Fetch & Display Data |
| Pattern 2 | 📝 Form + Validation |
| Pattern 3 | 🔁 List + Search + Filter |
| Pattern 4 | 💾 localStorage Usage |
| Pattern 5 | 🔀 Show/Hide UI |
| Pattern 6 | 🧭 Page Navigation |

**Level 2 — Intermediate** *(15% of interview questions)*

| Pattern | Name |
|---|---|
| Pattern 7 | ♾️ Pagination / Infinite Scroll |
| Pattern 8 | 🔔 Toast / Notifications |
| Pattern 9 | 🛒 Shopping Cart |
| Pattern 10 | 🌙 Dark Mode / User Preferences |
| Pattern 11 | 🔐 Auth Flow |

**Level 3 — Advanced** *(4% of interview questions)*

| Pattern | Name |
|---|---|
| Pattern 12 | ⚡ Performance Optimization |
| Pattern 13 | 📡 Real-time / Polling |
| Pattern 14 | 🔄 Complex Async |

**Level 4 — Specialist** *(1% — senior interviews only)*

| Pattern | Name |
|---|---|
| Pattern 15 | 🕳️ Memory Leaks |
| Pattern 16 | 👻 Stale Closures |
| Pattern 17 | 🔁 Infinite Render Loops |
| Pattern 18 | 🔑 Token Expiry + Silent Refresh |
| Pattern 19 | 📦 Context Performance at Scale |

[↑ Back to Table of Contents](#table-of-contents)

---

## Prompt 2 — Senior / Architecture

> **Use when:** Thinking about system design, preparing for senior roles, want to build mental models with TypeScript. Builds the WHY behind typed code, not just the HOW.

---

### Prompt 2 — Full Text

```
You are building a complete scenario-based React + TypeScript
practice guide that teaches developers to THINK in patterns.
Not just what to build — how to THINK about what to build.
All code must be TypeScript with full type safety.
Types and interfaces are part of the architecture, not afterthoughts.

TYPESCRIPT REQUIREMENTS — APPLY TO ALL CODE

ALWAYS include:
- Interface or type for every data shape and prop
- Discriminated union types for state machines
- Generic functions for reusable utilities
- Typed context with createContext<Type | null>(null)
- Strict typed reducers with Action union types
- Return types on all functions
- No `any` — use `unknown`, generics, or proper types

ARCHITECTURE THINKING WITH TYPES:
- Types should reflect your domain model
- If a type needs `any`, the architecture is wrong
- Union types over boolean flags:
  BAD:  isLoading: boolean, isError: boolean, isSuccess: boolean
  GOOD: status: 'idle' | 'loading' | 'success' | 'error'
- Types document intent — name them to explain purpose
- Shared types belong in a types.ts file, not scattered

FULL STRUCTURE — ALL 4 LEVELS

LEVEL 1 — FOUNDATION (80% of real work)
  Pattern 1:  Data Display Pattern
  Pattern 2:  Form Flow Pattern
  Pattern 3:  Conditional UI Pattern
  Pattern 4:  List Operations Pattern
  Pattern 5:  State Sync Pattern
  Pattern 6:  Navigation Flow Pattern

LEVEL 2 — INTERMEDIATE (15% of real work)
  Pattern 7:  Performance Pattern
  Pattern 8:  Async Pattern
  Pattern 9:  Component Architecture Pattern
  Pattern 10: Global State Pattern
  Pattern 11: Side Effect Pattern

LEVEL 3 — ADVANCED (4% of real work)
  Pattern 12: Auth Flow Pattern
  Pattern 13: Reusability Pattern
  Pattern 14: Real-time Pattern

LEVEL 4 — SPECIALIST (1% of real work)
  Pattern 15: Memory Leak Pattern
  Pattern 16: Stale Closure Pattern
  Pattern 17: Infinite Loop Pattern
  Pattern 18: Token Management Pattern
  Pattern 19: Context Scale Pattern

FOR EACH PATTERN HEADER USE THIS FORMAT:

## Pattern [N]: [Emoji] [Name]
> Level [1/2/3/4] — [Foundation/Intermediate/Advanced/Specialist]

The Mental Model:
[How a senior dev THINKS about this pattern.
Not a definition — a thinking approach.
The question they ask themselves. 2-3 lines.]

The One Rule:
[Single sentence. Separates junior from senior thinking.]

The TypeScript Mental Model:
[How types REVEAL the architecture.
What good types look like. What bad types signal. 2-3 lines.]

What junior devs always get wrong:
[Wrong thinking in both logic AND types.]

You need this pattern when:
- [Real situation 1]
- [Real situation 2]
- [Real situation 3]

This pattern connects to:
[Which other patterns this one builds on]

FOR EACH SCENARIO USE THIS FORMAT:

### Pattern [N] · Scenario [N] — [Problem Title]

Situation:
[2-3 lines. Real company. Senior-level decision.]

Here is the current approach:
[TypeScript + React code that WORKS but is architecturally wrong
OR has types that reveal wrong thinking]

What is wrong with this approach — in both logic AND types?

Your tasks:
- [architectural fix]
- [TypeScript fix — better types enforcing the right pattern]
- [architectural fix]

<details>
<summary>Hint — How to think about this</summary>
[Thinking direction. What do the types tell you? 2-3 lines.]
</details>

<details>
<summary>Full Solution + Architecture Explanation</summary>
[Complete TypeScript + React code. Full interfaces. No any.]

The principle at work:
[Connects fix to the One Rule. 1-2 lines.]

How types enforce the architecture:
[How the TypeScript types PREVENT the wrong pattern from recurring.]

How this scales:
[What happens when types + pattern are correct as app grows.]

What a senior dev spots in 10 seconds:
[The signal in the TYPES that reveals the wrong architecture.]

The alternative that looks correct but isn't:
[The tempting wrong solution and why it fails later.]
</details>

SCENARIOS PER PATTERN:
  Level 1 → 4 scenarios each (naive → scaled thinking)
  Level 2 → 4 scenarios each (works now → breaks at scale)
  Level 3 → 3 scenarios each (wrong architecture → right one)
  Level 4 → 2 scenarios each (invisible bug → production incident)

RULES:
- All code is TypeScript — zero plain JavaScript
- No `any` — if `any` seems needed, the architecture is wrong
- Problems must be about WRONG THINKING in both logic AND types
- Solutions must explain the PRINCIPLE and how types enforce it
- React functional components + hooks only
- End with Mental Model Picker — at least 15 entries
  including TypeScript-specific mental model mistakes

GENERATION COMMANDS:
"Generate Part 1 — Level 1, Patterns 1 to 6"
"Generate Part 2 — Level 2, Patterns 7 to 11"
"Generate Part 3 — Level 3, Patterns 12 to 14"
"Generate Part 4 — Level 4, Patterns 15 to 19"

Or one pattern only:
"Generate only Pattern [N] — [Name]. All scenarios. Complete. No placeholders."

OUTPUT: Complete markdown. No placeholders.
Every scenario fully written in TypeScript.
Types must enforce architecture, not just decorate it.
```

---

### TypeScript Patterns Used in Prompt 2

**Discriminated Union Status:**

```typescript
type AsyncState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; error: string };
```

**Generic Custom Hook:**

```typescript
interface UseFetchResult<T> {
  data: T | null;
  isLoading: boolean;
  error: string | null;
  refetch: () => void;
}

function useFetch<T>(url: string): UseFetchResult<T> {}
```

**Typed Reducer:**

```typescript
type CartAction =
  | { type: 'ADD_ITEM'; payload: Product }
  | { type: 'REMOVE_ITEM'; payload: { id: string } }
  | { type: 'UPDATE_QUANTITY'; payload: { id: string; quantity: number } }
  | { type: 'CLEAR_CART' };

function cartReducer(state: CartItem[], action: CartAction): CartItem[] {}
```

**Typed Context:**

```typescript
interface ThemeContextType {
  theme: 'light' | 'dark';
  toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextType | null>(null);
```

---

### Structure Overview — Prompt 2

**Level 1 — Foundation** *(80% of real work)*

| Pattern | Name |
|---|---|
| Pattern 1 | 📋 Data Display Pattern |
| Pattern 2 | 📝 Form Flow Pattern |
| Pattern 3 | 🔀 Conditional UI Pattern |
| Pattern 4 | 🔁 List Operations Pattern |
| Pattern 5 | 💾 State Sync Pattern |
| Pattern 6 | 🧭 Navigation Flow Pattern |

**Level 2 — Intermediate** *(15% of real work)*

| Pattern | Name |
|---|---|
| Pattern 7 | ⚡ Performance Pattern |
| Pattern 8 | 🔄 Async Pattern |
| Pattern 9 | 🧩 Component Architecture Pattern |
| Pattern 10 | 🌐 Global State Pattern |
| Pattern 11 | 🔌 Side Effect Pattern |

**Level 3 — Advanced** *(4% of real work)*

| Pattern | Name |
|---|---|
| Pattern 12 | 🔐 Auth Flow Pattern |
| Pattern 13 | ♻️ Reusability Pattern |
| Pattern 14 | 📡 Real-time Pattern |

**Level 4 — Specialist** *(1% of real work)*

| Pattern | Name |
|---|---|
| Pattern 15 | 🕳️ Memory Leak Pattern |
| Pattern 16 | 👻 Stale Closure Pattern |
| Pattern 17 | 🔁 Infinite Loop Pattern |
| Pattern 18 | 🔑 Token Management Pattern |
| Pattern 19 | 📦 Context Scale Pattern |

[↑ Back to Table of Contents](#table-of-contents)

---

## Prompt 3 — Real Product Builder

> **Use when:** You want to practice building complete real features end to end with TypeScript. Full typed features a real company ships to real users. Good for portfolio projects.

---

### Prompt 3 — Full Text

```
You are building a complete scenario-based React + TypeScript
practice guide where every scenario is a REAL PRODUCT FEATURE
— not a concept demo, not an isolated exercise.

Every scenario = something a real company ships to real users.
Every line of code is TypeScript with full type safety.

TYPESCRIPT REQUIREMENTS — APPLY TO ALL CODE

ALWAYS include:
- A types section at the top of each scenario showing
  all interfaces and types used in that feature
- Typed props for every component
- Generic fetch utilities
- Discriminated union status types
- Typed custom hooks with explicit return types
- No `any` — production code never uses `any`

PRODUCTION TYPESCRIPT PATTERNS:
- Custom hook return types are explicit interfaces
- API response types mirror the actual API contract
- Form state uses a typed interface, not loose objects
- Error types distinguish network errors from API errors
- Event types are specific, not generic React.SyntheticEvent

FULL STRUCTURE — ALL 4 LEVELS

LEVEL 1 — FOUNDATION (80% of real product features)
  Pattern 1:  Data Loading Feature
              (product page, user list, dashboard widget)
  Pattern 2:  Form Feature
              (login, signup, contact, settings update)
  Pattern 3:  List Feature
              (search, filter, sort, paginate)
  Pattern 4:  Persistence Feature
              (cart, preferences, draft saving)
  Pattern 5:  Interactive UI Feature
              (modal, drawer, tabs, accordion, tooltip)
  Pattern 6:  Navigation Feature
              (protected pages, redirects, breadcrumbs)

LEVEL 2 — INTERMEDIATE (15% of product features)
  Pattern 7:  Load More Feature
              (pagination, infinite scroll, load more button)
  Pattern 8:  Feedback Feature
              (toasts, banners, inline errors, confirmations)
  Pattern 9:  Cart Feature
              (add, remove, update, persist, calculate)
  Pattern 10: Theme Feature
              (dark mode, font size, color scheme, persist)
  Pattern 11: Auth Feature
              (full login → protected → refresh → logout flow)

LEVEL 3 — ADVANCED (4% of product features)
  Pattern 12: Optimization Feature
              (debounce, virtualize, memo, code split)
  Pattern 13: Live Feature
              (real-time updates, polling, websocket)
  Pattern 14: Resilient Async Feature
              (retry, race conditions, loading states)

LEVEL 4 — SPECIALIST (1% — breaks production)
  Pattern 15: Leak Fix
  Pattern 16: Closure Fix
  Pattern 17: Loop Fix
  Pattern 18: Token Fix
  Pattern 19: Scale Fix

FOR EACH PATTERN HEADER USE THIS FORMAT:

## Pattern [N]: [Emoji] [Name]
> Level [1/2/3/4]

Real products that have this feature:
[3 real apps and how they use this pattern with TypeScript]

What a complete TypeScript version includes:
- [checklist item 1]
- [checklist item 2 — TypeScript specific]
- [checklist item 3]
- [checklist item 4]
- [checklist item 5]

What makes the demo version different from the real version:
[The 2-3 things demos skip — including TypeScript gaps]

FOR EACH SCENARIO USE THIS FORMAT:

### Pattern [N] · Scenario [N] — [Feature Title]

Product Brief:
[Real product requirement. Company type. What done looks like. 3-4 lines.]

Types for this feature:
[ALL interfaces and types defined here — like a real types.ts file]

Current state of the code:
[Realistic incomplete or broken TypeScript + React code]

Definition of Done:
- [specific requirement]
- [specific requirement]
- [TypeScript requirement]
- [edge case]

<details>
<summary>Hint</summary>
[What is missing including TypeScript gaps]
</details>

<details>
<summary>Complete Feature Implementation</summary>
[Complete production-ready TypeScript + React code]

Production checklist:
  - Does it show a loading state?
  - Are all API response shapes typed?
  - Is there any `any` type to remove?
  - Are all component props typed?
  - Does it handle empty data?

How this connects to other features:
[Which other patterns this works with]

TypeScript patterns used:
[Specific TS patterns: discriminated unions, generics, utility types]
</details>

SCENARIOS PER PATTERN:
  Level 1 → 4 scenarios (basic → production-ready)
  Level 2 → 4 scenarios (incomplete → complete feature)
  Level 3 → 3 scenarios (demo version → real version)
  Level 4 → 2 scenarios (silent bug → fixed feature)

RULES:
- Every scenario is a real shippable typed feature
- All code TypeScript — no plain JavaScript
- No `any` anywhere
- Types section shown before each scenario code
- Code handles: loading, error, empty state, edge cases
- React functional components + hooks only
- async/await throughout
- End with Feature Readiness Checklist — for every pattern,
  5-6 questions including TypeScript checks

GENERATION COMMANDS:
"Generate Part 1 — Level 1, Patterns 1 to 6"
"Generate Part 2 — Level 2, Patterns 7 to 11"
"Generate Part 3 — Level 3, Patterns 12 to 14"
"Generate Part 4 — Level 4, Patterns 15 to 19"

Or one pattern only:
"Generate only Pattern [N] — [Name]. All scenarios. Complete. No placeholders."

OUTPUT: Complete markdown. No placeholders.
Every scenario fully written in TypeScript.
Every feature complete with proper types.
```

---

### Typed Utility Patterns Used in Prompt 3

**Typed localStorage:**

```typescript
function getFromStorage<T>(key: string, fallback: T): T {
  try {
    const item = localStorage.getItem(key);
    return item ? (JSON.parse(item) as T) : fallback;
  } catch {
    return fallback;
  }
}

function setToStorage<T>(key: string, value: T): boolean {
  try {
    localStorage.setItem(key, JSON.stringify(value));
    return true;
  } catch {
    return false;
  }
}
```

---

### Structure Overview — Prompt 3

**Level 1 — Foundation** *(80% of real product features)*

| Pattern | Name | Examples |
|---|---|---|
| Pattern 1 | 📋 Data Loading Feature | product page, user list, dashboard widget |
| Pattern 2 | 📝 Form Feature | login, signup, contact, settings update |
| Pattern 3 | 🔁 List Feature | search, filter, sort, paginate |
| Pattern 4 | 💾 Persistence Feature | cart, preferences, draft saving |
| Pattern 5 | 🔀 Interactive UI Feature | modal, drawer, tabs, accordion |
| Pattern 6 | 🧭 Navigation Feature | protected pages, redirects, breadcrumbs |

**Level 2 — Intermediate** *(15% of product features)*

| Pattern | Name | Examples |
|---|---|---|
| Pattern 7 | ♾️ Load More Feature | pagination, infinite scroll, load more |
| Pattern 8 | 🔔 Feedback Feature | toasts, banners, inline errors |
| Pattern 9 | 🛒 Cart Feature | add, remove, update, persist, calculate |
| Pattern 10 | 🎨 Theme Feature | dark mode, font size, color scheme |
| Pattern 11 | 🔐 Auth Feature | login → protected → refresh → logout |

**Level 3 — Advanced** *(4% of product features)*

| Pattern | Name | Examples |
|---|---|---|
| Pattern 12 | ⚡ Optimization Feature | debounce, virtualize, memo, code split |
| Pattern 13 | 📡 Live Feature | real-time updates, polling, websocket |
| Pattern 14 | 🔄 Resilient Async Feature | retry, race conditions, loading states |

**Level 4 — Specialist** *(1% — breaks production)*

| Pattern | Name |
|---|---|
| Pattern 15 | 🕳️ Leak Fix |
| Pattern 16 | 👻 Closure Fix |
| Pattern 17 | 🔁 Loop Fix |
| Pattern 18 | 🔑 Token Fix |
| Pattern 19 | 📦 Scale Fix |

[↑ Back to Table of Contents](#table-of-contents)

---

## Master Prompt — All Purposes Combined

> **Use when:** You want one guide covering everything — interview prep + mental models + real product features + production bugs. All in TypeScript with full type safety.

---

### Master Prompt — Full Text

```
You are building a complete scenario-based React + TypeScript
practice guide. This is the master version — it combines
interview prep, mental model building, and real product
feature practice in one guide.

All code is TypeScript. Types are first-class citizens —
not optional annotations added at the end, but integral
to understanding the architecture and preventing bugs.

TYPESCRIPT REQUIREMENTS — NON-NEGOTIABLE

ALWAYS:
- Define interfaces for all data shapes before the component
- Type all useState with generics: useState<User | null>(null)
- Type all useRef with generics: useRef<HTMLDivElement>(null)
- Type all event handlers explicitly
- Use discriminated unions for state machines
- Type all async functions with return types
- Use generic fetch utilities
- Type all custom hook return values

NEVER:
- Use `any` type — use `unknown` and narrow it
- Leave useState untyped: useState([])
- Leave props untyped: function Card({ title, onClick })
- Use type assertions carelessly: as any
- Skip typing API responses

TYPESCRIPT PATTERNS TO TEACH IN SCENARIOS:

Pattern A — Discriminated Union Status:
  type Status =
    | { kind: 'idle' }
    | { kind: 'loading' }
    | { kind: 'success'; data: User[] }
    | { kind: 'error'; message: string };

Pattern B — Generic Fetch:
  async function apiFetch<T>(url: string, options?: RequestInit): Promise<T> {
    const res = await fetch(url, options);
    if (!res.ok) throw new Error(`${res.status}: ${res.statusText}`);
    return res.json() as Promise<T>;
  }

Pattern C — Typed Custom Hook:
  interface UseProductsReturn {
    products: Product[];
    isLoading: boolean;
    error: string | null;
    refetch: () => void;
  }
  function useProducts(categoryId: string): UseProductsReturn {}

Pattern D — Typed Reducer:
  type CartAction =
    | { type: 'ADD'; payload: Product }
    | { type: 'REMOVE'; payload: string }
    | { type: 'CLEAR' };
  function cartReducer(state: CartItem[], action: CartAction): CartItem[] {}

Pattern E — Typed Context with Safe Hook:
  interface ThemeContextType {
    theme: 'light' | 'dark';
    toggleTheme: () => void;
  }
  const ThemeContext = createContext<ThemeContextType | null>(null);
  function useTheme(): ThemeContextType {
    const ctx = useContext(ThemeContext);
    if (!ctx) throw new Error('useTheme must be inside ThemeProvider');
    return ctx;
  }

Pattern F — Utility Types in Props:
  type UpdateUserPayload = Partial<Omit<User, 'id' | 'createdAt'>>;
  interface UpdateUserFormProps {
    userId: string;
    onSuccess: (updated: User) => void;
  }

FULL STRUCTURE — ALL 4 LEVELS

LEVEL 1 — FOUNDATION (80% of real work)
  Pattern 1:  Fetch & Display Data
  Pattern 2:  Form + Validation
  Pattern 3:  List + Search + Filter
  Pattern 4:  localStorage / Persistence
  Pattern 5:  Show/Hide UI
  Pattern 6:  Page Navigation

LEVEL 2 — INTERMEDIATE (15% of real work)
  Pattern 7:  Pagination / Infinite Scroll
  Pattern 8:  Toast / Notifications
  Pattern 9:  Shopping Cart
  Pattern 10: Dark Mode / User Preferences
  Pattern 11: Auth Flow

LEVEL 3 — ADVANCED (4% of real work)
  Pattern 12: Performance Optimization
  Pattern 13: Real-time / Polling
  Pattern 14: Complex Async

LEVEL 4 — SPECIALIST (1% — production incidents)
  Pattern 15: Memory Leaks
  Pattern 16: Stale Closures
  Pattern 17: Infinite Render Loops
  Pattern 18: Token Expiry + Silent Refresh
  Pattern 19: Context Performance at Scale

FOR EACH PATTERN HEADER USE THIS FORMAT:

## Pattern [N]: [Emoji] [Name]
> Level [1/2/3/4] — [Foundation/Intermediate/Advanced/Specialist]

The Mental Model:
[How a senior dev thinks about this. 2-3 lines.]

The One Rule:
[Single sentence. Solves 80% of this pattern's problems.]

TypeScript focus in this pattern:
[Which TS concepts appear here. What good types look like. 2 lines.]

What junior devs always get wrong:
[Wrong thinking in both logic AND types.]

You need this pattern when:
- [real situation 1]
- [real situation 2]
- [real situation 3]

Interview asks this as:
- [exact interview phrasing 1]
- [exact interview phrasing 2]

Production-ready version always includes:
- [thing demos skip 1]
- [thing demos skip 2 — may be TypeScript specific]
- [thing demos skip 3]

FOR EACH SCENARIO USE THIS FORMAT:

### Pattern [N] · Scenario [N] — [Problem Title]

Situation:
[2-3 lines. Real company. Real person affected.
Real consequence. Feels like Monday morning at a real job.]

Types used in this scenario:
[All interfaces and types defined before the code]

Here is the current code:
[Realistic broken or incomplete TypeScript + React code.
Has either logic bugs OR TypeScript weaknesses OR both.]

Your tasks:
- [specific logic fix or feature]
- [specific logic fix or feature]
- [TypeScript improvement — add types, remove any, fix generics]
- [edge case to handle]

<details>
<summary>Hint — How to think about this</summary>
[Thinking direction. What do the types tell you? 2-3 lines.]
</details>

<details>
<summary>Full Solution</summary>
[Complete TypeScript + React code. No any. Every edge case handled.
Comments explain both LOGIC and TYPE decisions.]

The principle:
[1 line — connects fix back to the One Rule.]

TypeScript lesson:
[Specific TS concept. How types enforce correct behavior. 1-2 lines.]

Why this matters in production:
[What actually breaks without this fix. 2 lines.]

What a senior dev spots in 10 seconds:
[Root cause signal — in the code AND in the types.]

What the interviewer is checking:
[Logic they want to see AND TypeScript they want to see.]

Before you ship this, verify:
  - [logic checklist item]
  - [TypeScript checklist item — no any, typed responses]
  - [edge case checklist item]
</details>

SCENARIOS PER PATTERN:
  Level 1 → 4 scenarios (easy → medium → hard → edge case)
  Level 2 → 4 scenarios (incomplete → broken → scaled → edge case)
  Level 3 → 3 scenarios (wrong approach → right approach → production)
  Level 4 → 2 scenarios (subtle bug → full production fix)

COMPANY TYPES — ROTATE THROUGH THESE:
  - E-commerce (products, cart, orders, reviews)
  - SaaS dashboard (analytics, users, billing, reports)
  - Food delivery (orders, tracking, menu, drivers)
  - Social platform (posts, comments, likes, feed)
  - Fintech / Banking (transactions, accounts, transfers)
  - HR / Recruitment (jobs, candidates, interviews)
  - Job portal (listings, applications, profiles)
  - Healthcare (appointments, records, doctors)

TYPESCRIPT TEACHING PROGRESSION:
  Level 1: Basic generics, typed state, typed events, typed props
  Level 2: Discriminated unions, typed reducers, typed context
  Level 3: Generic hooks, utility types, conditional types
  Level 4: Advanced narrowing, branded types, module patterns

RULES:
- All code TypeScript — zero plain JavaScript
- No `any` type — if needed, the architecture is wrong
- React functional components + hooks only
- async/await not .then() chains
- Clickable TOC at very top
- Level dividers between each level
- Back to TOC after every pattern
- Types section shown in every scenario

END WITH TWO SECTIONS:

SECTION 1 — Pattern Picker (at least 25 entries):
  Format: "My [problem]" → "Pattern [N] — [reason]"
  Include TypeScript-specific problems like:
  "My API response is typed as any everywhere"
    → Pattern 1 — generic fetch utility
  "My state has boolean flags for loading/error/success"
    → Pattern 1 — discriminated union status type

SECTION 2 — Production Readiness Checklist:
  For every pattern, 5-6 questions including TypeScript checks.
  Format per pattern:
  Pattern [N] — [Name]
    - [logic question]
    - [TypeScript question]
    - [edge case question]
    - [TypeScript question — no any, typed responses?]
    - [production question]

GENERATION COMMANDS:
After finishing a part, say:
"Part [N] complete. Say 'Generate Part [N+1]' to continue."

"Generate Part 1 — Level 1, Patterns 1 to 6"
"Generate Part 2 — Level 2, Patterns 7 to 11"
"Generate Part 3 — Level 3, Patterns 12 to 14"
"Generate Part 4 — Level 4, Patterns 15 to 19"

One pattern only:
"Generate only Pattern [N] — [Name]. All scenarios. Complete. No placeholders."

Regenerate a scenario:
"Regenerate Pattern [N] Scenario [N].
Make it harder / more realistic / different company."

OUTPUT: Single complete markdown per part.
No placeholders. No "add your code here".
Every scenario fully written in TypeScript.
Every solution complete with full type safety.
No `any` type anywhere in any solution.
All interfaces defined before the component that uses them.
```

---

### TypeScript Patterns Reference — Master Prompt

| Pattern Name | When to Use |
|---|---|
| Discriminated Union Status | Any async operation with loading/error/success states |
| Generic Fetch Utility | Any API call — type the response instead of casting |
| Typed Custom Hook | Any reusable logic extracted to a hook |
| Typed Reducer | Cart, complex form state, multi-step flows |
| Typed Context + Safe Hook | Any global state shared across components |
| Utility Types in Props | Update payloads, display-safe versions of full types |

---

### Structure Overview — Master Prompt

**Level 1 — Foundation** *(80% of real work)*

| Pattern | Name |
|---|---|
| Pattern 1 | 📋 Fetch & Display Data |
| Pattern 2 | 📝 Form + Validation |
| Pattern 3 | 🔁 List + Search + Filter |
| Pattern 4 | 💾 localStorage / Persistence |
| Pattern 5 | 🔀 Show/Hide UI |
| Pattern 6 | 🧭 Page Navigation |

**Level 2 — Intermediate** *(15% of real work)*

| Pattern | Name |
|---|---|
| Pattern 7 | ♾️ Pagination / Infinite Scroll |
| Pattern 8 | 🔔 Toast / Notifications |
| Pattern 9 | 🛒 Shopping Cart |
| Pattern 10 | 🌙 Dark Mode / User Preferences |
| Pattern 11 | 🔐 Auth Flow |

**Level 3 — Advanced** *(4% of real work)*

| Pattern | Name |
|---|---|
| Pattern 12 | ⚡ Performance Optimization |
| Pattern 13 | 📡 Real-time / Polling |
| Pattern 14 | 🔄 Complex Async |

**Level 4 — Specialist** *(1% — production incidents)*

| Pattern | Name |
|---|---|
| Pattern 15 | 🕳️ Memory Leaks |
| Pattern 16 | 👻 Stale Closures |
| Pattern 17 | 🔁 Infinite Render Loops |
| Pattern 18 | 🔑 Token Expiry + Silent Refresh |
| Pattern 19 | 📦 Context Performance at Scale |

[↑ Back to Table of Contents](#table-of-contents)

---

## What Each Prompt Includes

| Feature | Prompt 1 | Prompt 2 | Prompt 3 | Master |
|---|:---:|:---:|:---:|:---:|
| Build from scratch | ✅ | ❌ | ✅ | ✅ |
| Fix broken code | ✅ | ❌ | ✅ | ✅ |
| Fix broken types | ✅ | ✅ | ✅ | ✅ |
| Why it breaks | ❌ | ✅ | ❌ | ✅ |
| Mental models | ❌ | ✅ | ❌ | ✅ |
| TypeScript mental models | ❌ | ✅ | ❌ | ✅ |
| Architecture fix | ❌ | ✅ | ❌ | ✅ |
| Full feature end to end | ❌ | ❌ | ✅ | ✅ |
| Types section per scenario | ❌ | ❌ | ✅ | ✅ |
| Production checklist | ❌ | ❌ | ✅ | ✅ |
| Interview criteria | ✅ | ❌ | ❌ | ✅ |
| Senior thinking | ❌ | ✅ | ❌ | ✅ |
| TS concept per scenario | ✅ | ✅ | ✅ | ✅ |
| All 4 levels | ✅ | ✅ | ✅ | ✅ |
| Pattern picker | ✅ | ✅ | ✅ | ✅ |
| **Best for** | **Interview** | **Senior** | **Portfolio** | **Everything** |

[↑ Back to Table of Contents](#table-of-contents)

---

## How Many Days Each Level Takes

> TypeScript adds approximately 20–30% more time than plain JavaScript because you must understand the data shapes, not just the logic.

### Time Per Scenario

| Prompt | Hours Per Scenario |
|---|---|
| Prompt 1 — Interview Prep | 1.5 hours |
| Prompt 2 — Senior / Architecture | 2.0 hours |
| Prompt 3 — Product Builder | 2.5 hours |
| Master Prompt | 3.0 hours |

---

### Prompt 1 — Interview Prep

| Level | Patterns | Scenarios | Total Hours | 2 hrs/day | 3 hrs/day |
|---|---|---|---|---|---|
| Level 1 — Foundation | 1 to 6 | 24 | 36h | 18 days | 12 days |
| Level 2 — Intermediate | 7 to 11 | 20 | 30h | 15 days | 10 days |
| Level 3 — Advanced | 12 to 14 | 12 | 18h | 9 days | 6 days |
| Level 4 — Specialist | 15 to 19 | 10 | 20h | 10 days | 7 days |
| **Total** | — | **66** | **104h** | **52 days** | **35 days** |

---

### Prompt 2 — Senior / Architecture

| Level | Patterns | Scenarios | Total Hours | 2 hrs/day | 3 hrs/day |
|---|---|---|---|---|---|
| Level 1 — Foundation | 1 to 6 | 24 | 48h | 24 days | 16 days |
| Level 2 — Intermediate | 7 to 11 | 20 | 40h | 20 days | 14 days |
| Level 3 — Advanced | 12 to 14 | 12 | 24h | 12 days | 8 days |
| Level 4 — Specialist | 15 to 19 | 10 | 20h | 10 days | 7 days |
| **Total** | — | **66** | **132h** | **66 days** | **44 days** |

---

### Prompt 3 — Product Builder

| Level | Patterns | Scenarios | Total Hours | 2 hrs/day | 3 hrs/day |
|---|---|---|---|---|---|
| Level 1 — Foundation | 1 to 6 | 24 | 60h | 30 days | 20 days |
| Level 2 — Intermediate | 7 to 11 | 20 | 50h | 25 days | 17 days |
| Level 3 — Advanced | 12 to 14 | 12 | 30h | 15 days | 10 days |
| Level 4 — Specialist | 15 to 19 | 10 | 25h | 13 days | 9 days |
| **Total** | — | **66** | **165h** | **83 days** | **56 days** |

---

### Master Prompt

| Level | Patterns | Scenarios | Total Hours | 2 hrs/day | 3 hrs/day |
|---|---|---|---|---|---|
| Level 1 — Foundation | 1 to 6 | 24 | 72h | 36 days | 24 days |
| Level 2 — Intermediate | 7 to 11 | 20 | 60h | 30 days | 20 days |
| Level 3 — Advanced | 12 to 14 | 12 | 36h | 18 days | 12 days |
| Level 4 — Specialist | 15 to 19 | 10 | 30h | 15 days | 10 days |
| **Total** | — | **66** | **198h** | **99 days** | **66 days** |

---

### All Prompts Side By Side

| | Prompt 1 | Prompt 2 | Prompt 3 | Master |
|---|---|---|---|---|
| **Level 1 — 2 hrs/day** | 18 days | 24 days | 30 days | 36 days |
| **Level 1 — 3 hrs/day** | 12 days | 16 days | 20 days | 24 days |
| **Level 2 — 2 hrs/day** | 15 days | 20 days | 25 days | 30 days |
| **Level 2 — 3 hrs/day** | 10 days | 14 days | 17 days | 20 days |
| **Level 3 — 2 hrs/day** | 9 days | 12 days | 15 days | 18 days |
| **Level 3 — 3 hrs/day** | 6 days | 8 days | 10 days | 12 days |
| **Level 4 — 2 hrs/day** | 10 days | 10 days | 13 days | 15 days |
| **Level 4 — 3 hrs/day** | 7 days | 7 days | 9 days | 10 days |
| **Total — 2 hrs/day** | **52 days** | **66 days** | **83 days** | **99 days** |
| **Total — 3 hrs/day** | **35 days** | **44 days** | **56 days** | **66 days** |

[↑ Back to Table of Contents](#table-of-contents)

---

## Realistic Study Plans

### Plan A — Interview in 30 Days

| | Detail |
|---|---|
| **Use** | Prompt 1 only |
| **Cover** | Level 1 only — all 6 patterns |
| **Skip** | Level 2, 3, 4 for now |
| **Time** | 3 hours/day |
| **Schedule** | 12 days Level 1, remaining 18 days redo weak patterns |
| **Result** | Ready for most React + TypeScript interviews |

---

### Plan B — Interview in 60 Days

| | Detail |
|---|---|
| **Use** | Prompt 1 for Level 1 + Level 2, Master Prompt for Level 3 + 4 |
| **Cover** | All 4 levels |
| **Time** | 2–3 hours/day |
| **Schedule** | ~50 days total |
| **Result** | Ready for mid to senior React + TypeScript interviews |

---

### Plan C — No Interview, Want to Grow

| | Detail |
|---|---|
| **Use** | Master Prompt |
| **Cover** | All 4 levels, no rush |
| **Time** | 1–2 hours/day |
| **Schedule** | 99–198 days |
| **Result** | Solid senior-level React + TypeScript thinking |

---

### Plan D — Weekend Warrior

| | Detail |
|---|---|
| **Use** | Any prompt |
| **Cover** | 1 pattern per weekend (4 scenarios) |
| **Time** | 5–6 hours Saturday |
| **Schedule** | 19 weekends = about 5 months |
| **Result** | Steady deep progress |

---

### Plan E — TypeScript First Timer

| | Detail |
|---|---|
| **Use** | Prompt 1 only, Level 1 only |
| **Cover** | Patterns 1 to 6 |
| **Focus** | Get comfortable with TypeScript + React before going deeper |
| **Time** | 2 hours/day |
| **Schedule** | 18 days, then move to Level 2 or Master Prompt |
| **Result** | Solid foundation in typed React |

[↑ Back to Table of Contents](#table-of-contents)

---

## The Honest Truth

### What These Time Estimates Assume

| Habit | What It Does |
|---|---|
| Try the scenario yourself before looking at the solution | Builds real understanding |
| Understand the types, not just copy them | Compounds into architectural thinking |
| Close the solution and write it again yourself | Where actual skill is built |
| Take notes on TypeScript patterns you did not know | Compounds over sessions |
| Go back and replace every `any` in your solution | Scenario is truly complete |

---

### If You Skip the Doing

| Approach | Time Used | Benefit Gained |
|---|---|---|
| Read and do every scenario properly | Full estimate | 100% |
| Read without doing | Half the time | 20% |
| Skim solutions only | Quarter the time | 5% |

---

### The Two Rules That Matter Most

> **30 minutes struggling with the types yourself is worth more than 2 hours reading typed solutions.**

> **If you copy the solution and it has `any` somewhere — you have not finished the scenario. Your job is not done until zero `any`.**

[↑ Back to Table of Contents](#table-of-contents)

---

## How to Generate Parts

### Generation Commands

| Command | What It Generates |
|---|---|
| `Generate Part 1 — Level 1, Patterns 1 to 6` | Foundation — 24 scenarios |
| `Generate Part 2 — Level 2, Patterns 7 to 11` | Intermediate — 20 scenarios |
| `Generate Part 3 — Level 3, Patterns 12 to 14` | Advanced — 12 scenarios |
| `Generate Part 4 — Level 4, Patterns 15 to 19` | Specialist — 10 scenarios |

---

### One Pattern Only

```
Generate only Pattern 3 — List + Search + Filter.
All scenarios. Complete. No placeholders.
Full TypeScript with no any.
```

---

### Regenerate a Specific Scenario

```
Regenerate Pattern 2 Scenario 3.
Make the TypeScript harder — use discriminated unions.

Regenerate Pattern 5 Scenario 1.
Use a food delivery company. Add generic typed modal hook.

Regenerate Pattern 12 Scenario 2.
Focus on typed debounce hook with generics.
```

---

### After Each Part

| Finished | Say This |
|---|---|
| Part 1 done | `Generate Part 2` |
| Part 2 done | `Generate Part 3` |
| Part 3 done | `Generate Part 4` |

---

### Recommended Weekly Order

| Week | Part | TypeScript Focus |
|---|---|---|
| Week 1–2 | Part 1 — Level 1 Foundation | Basic generics, typed state, typed events |
| Week 3–4 | Part 2 — Level 2 Intermediate | Discriminated unions, typed reducers, typed context |
| Week 5–6 | Part 3 — Level 3 Advanced | Generic hooks, utility types, conditional types |
| Week 7+ | Part 4 — Level 4 Specialist | Advanced narrowing, module patterns |

[↑ Back to Table of Contents](#table-of-contents)