# React.js Interview Questions — Complete Map
## 18 Categories · 200+ Sub-Topics · Classified by Frequency

---

# Table of Contents

- [How to Read This Map](#how-to-read-this-map)
- [Category 1: React Fundamentals](#category-1-react-fundamentals)
- [Category 2: JSX](#category-2-jsx)
- [Category 3: Components](#category-3-components)
- [Category 4: Props](#category-4-props)
- [Category 5: State](#category-5-state)
- [Category 6: Hooks](#category-6-hooks)
- [Category 7: Component Lifecycle](#category-7-component-lifecycle)
- [Category 8: Event Handling](#category-8-event-handling)
- [Category 9: Conditional Rendering & Lists](#category-9-conditional-rendering--lists)
- [Category 10: Forms](#category-10-forms)
- [Category 11: State Management](#category-11-state-management)
- [Category 12: React Router](#category-12-react-router)
- [Category 13: Performance Optimization](#category-13-performance-optimization)
- [Category 14: Patterns & Architecture](#category-14-patterns--architecture)
- [Category 15: Styling](#category-15-styling)
- [Category 16: Testing](#category-16-testing)
- [Category 17: React Internals (How React Works)](#category-17-react-internals)
- [Category 18: Machine Coding / Build This](#category-18-machine-coding--build-this)
- [Summary Table](#summary-table)
- [The 80/20 — Focus Here First](#the-8020--focus-here-first)
- [Prompt 2 — Ask for the Full Guide](#prompt-2--ask-for-the-full-guide)

---

# How to Read This Map

```
🔴 = Must-know   (asked in 90% of interviews — prepare FIRST)
🟡 = Should-know  (asked in 50% of interviews — prepare after 🔴)
🟢 = Good-to-know (top companies / senior roles — prepare last)
```

This is ONLY the map — no explanations, no code.
Use [Prompt 2](#prompt-2--ask-for-the-full-guide) at the bottom to get the full guide.

---

# Category 1: React Fundamentals

```
🔴 What is React? Why use it?
🔴 Virtual DOM — what it is, how it works, why it's fast
🔴 Real DOM vs Virtual DOM
🔴 Reconciliation — how React updates the DOM efficiently
🔴 React Fiber — what it is (high-level)
🔴 One-way data flow (unidirectional data binding)
🔴 Declarative vs Imperative UI
🟡 React vs Angular vs Vue (comparison)
🟡 React library vs framework — why React is a "library"
🟡 SPA (Single Page Application) — what it means
🟢 History of React — versions and major changes
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 2: JSX

```
🔴 What is JSX?
🔴 JSX is NOT HTML — key differences (className, htmlFor, camelCase)
🔴 How JSX gets compiled (React.createElement under the hood)
🔴 Embedding JavaScript expressions in JSX ({})
🔴 JSX must return a single parent element (Fragment or div)
🔴 React Fragments — <></> and <React.Fragment>
🟡 Conditional expressions in JSX (ternary, &&, ||)
🟡 JSX spread attributes ({...props})
🟡 Why JSX expressions can't use if/else (only expressions, not statements)
🟡 Rendering raw HTML (dangerouslySetInnerHTML)
🟢 JSX without React (new JSX transform in React 17+)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 3: Components

```
🔴 Functional components vs Class components
🔴 Why functional components are preferred now
🔴 Component composition (nesting components)
🔴 Component reusability
🔴 Controlled vs Uncontrolled components
🔴 Pure components — what makes a component "pure"
🟡 Higher-Order Components (HOCs) — pattern and use cases
🟡 Render Props pattern
🟡 Container vs Presentational components
🟡 Compound components pattern
🟡 When to split a component into smaller ones
🟢 Headless components
🟢 Polymorphic components (as prop)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 4: Props

```
🔴 What are props? How to pass them?
🔴 Props are read-only (immutable)
🔴 Passing functions as props (callback props)
🔴 Children prop (props.children)
🔴 Default props / default parameter values
🔴 Destructuring props
🟡 Prop drilling — what it is, why it's a problem
🟡 Solutions to prop drilling (Context, composition, state management)
🟡 PropTypes for type checking
🟡 Spread props ({...props}) — when to use, when to avoid
🟢 Render props pattern (function as children)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 5: State

```
🔴 What is state? How is it different from props?
🔴 useState hook — syntax, usage, updater function
🔴 State is asynchronous (batching)
🔴 State immutability — why you must never mutate state directly
🔴 Updating objects and arrays in state (spread operator)
🔴 Lifting state up — when and why
🟡 Functional updater form: setState(prev => prev + 1)
🟡 Lazy initialization of state: useState(() => expensiveCalc())
🟡 State vs Ref — when to use which
🟡 Derived state — computing values from state instead of storing them
🟢 State colocation — keeping state close to where it's used
🟢 Why not to sync state with props (anti-pattern)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 6: Hooks

```
🔴 What are hooks? Rules of hooks (only top level, only in components/custom hooks)
🔴 useState — managing state in functional components
🔴 useEffect — side effects, cleanup, dependency array
🔴 useEffect dependency array — [], [dep], no array
🔴 useRef — DOM access, persisting values without re-render
🔴 useContext — consuming context
🔴 useMemo — memoize expensive calculations
🔴 useCallback — memoize functions (prevent unnecessary re-renders)
🔴 Custom hooks — creating reusable logic
🟡 useReducer — complex state logic, dispatch actions
🟡 useReducer vs useState — when to pick which
🟡 useLayoutEffect vs useEffect — what's the difference
🟡 useId — generating unique IDs (React 18+)
🟡 Cleanup function in useEffect — when and why
🟡 Stale closure problem in hooks
🟢 useImperativeHandle — exposing methods to parent
🟢 useSyncExternalStore — subscribing to external stores
🟢 useTransition — marking non-urgent updates (React 18)
🟢 useDeferredValue — deferring re-renders (React 18)
🟢 use() hook — React 19 (reading promises/context)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 7: Component Lifecycle

```
🔴 Mounting → Updating → Unmounting (the 3 phases)
🔴 useEffect as lifecycle replacement (mount, update, unmount)
🔴 Cleanup on unmount (return function in useEffect)
🟡 Class lifecycle methods (componentDidMount, componentDidUpdate, componentWillUnmount)
🟡 getDerivedStateFromProps — why it's rarely needed
🟡 shouldComponentUpdate — manual optimization
🟡 componentDidCatch / Error Boundaries
🟢 getSnapshotBeforeUpdate
🟢 Render phase vs Commit phase
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 8: Event Handling

```
🔴 How events work in React (synthetic events)
🔴 Handling onClick, onChange, onSubmit
🔴 Passing arguments to event handlers
🔴 Preventing default behavior (e.preventDefault())
🟡 SyntheticEvent — what it is, why React uses it
🟡 Event pooling (React 16, removed in React 17)
🟡 Keyboard events, form events, mouse events
🟡 Debouncing and throttling in event handlers
🟢 Custom events between components
🟢 Capture vs Bubble phase in React
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 9: Conditional Rendering & Lists

```
🔴 Conditional rendering (if/else, ternary, &&)
🔴 Rendering lists with .map()
🔴 The key prop — why it matters, what happens without it
🔴 Why index as key is bad (and when it's OK)
🟡 Short-circuit rendering pitfall (0 && <Component />)
🟡 Rendering null to hide a component
🟡 Switch/case in render
🟢 Virtualized lists for performance (react-window, react-virtualized)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 10: Forms

```
🔴 Controlled components — value + onChange
🔴 Uncontrolled components — ref + defaultValue
🔴 Handling form submission (onSubmit, preventDefault)
🔴 Controlled vs Uncontrolled — when to use which
🟡 Form validation approaches
🟡 Handling multiple inputs with a single handler
🟡 File input handling
🟡 React Hook Form (library) — when and why to use
🟢 Formik (library)
🟢 Server actions with forms (React 19 / Next.js)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 11: State Management

```
🔴 Context API — createContext, Provider, useContext
🔴 When to use Context vs prop drilling
🔴 Context re-rendering problem — why context causes unnecessary re-renders
🔴 Redux basics — store, actions, reducers, dispatch
🔴 Redux flow: dispatch(action) → reducer → new state → re-render
🟡 Redux Toolkit (RTK) — createSlice, configureStore
🟡 useSelector, useDispatch hooks
🟡 Redux middleware (thunk for async)
🟡 Redux vs Context API — when to use which
🟡 Zustand — lightweight alternative
🟡 Jotai / Recoil — atomic state management
🟢 State machines (XState)
🟢 Server state vs Client state
🟢 React Query / TanStack Query — server state management
🟢 SWR — stale-while-revalidate
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 12: React Router

```
🔴 Setting up routes (BrowserRouter, Routes, Route)
🔴 Link vs NavLink
🔴 useNavigate — programmatic navigation
🔴 useParams — reading URL parameters
🔴 Nested routes and Outlet
🟡 useSearchParams — query parameters
🟡 useLocation — current location object
🟡 Protected routes / Auth guards
🟡 Lazy loading routes with React.lazy + Suspense
🟡 404 Not Found route (catch-all)
🟢 Loaders and Actions (React Router v6.4+)
🟢 Data fetching in routes
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 13: Performance Optimization

```
🔴 React.memo — prevent re-renders of unchanged components
🔴 useMemo — memoize expensive calculations
🔴 useCallback — memoize callback functions
🔴 Key prop for list optimization
🔴 Lazy loading with React.lazy + Suspense
🔴 Code splitting
🟡 Why immutability helps React detect changes
🟡 Avoiding unnecessary re-renders (common causes)
🟡 React DevTools Profiler — finding performance bottlenecks
🟡 Windowing / Virtualization for large lists
🟡 Debouncing input handlers
🟡 Image optimization (lazy loading images)
🟢 React.memo vs useMemo vs useCallback — exact differences
🟢 Bundle analysis (webpack-bundle-analyzer)
🟢 Web Workers with React
🟢 Concurrent features (startTransition, useTransition, useDeferredValue)
🟢 React Server Components (RSC) — zero client-side JS
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 14: Patterns & Architecture

```
🔴 Component composition over inheritance
🔴 Lifting state up
🔴 Custom hooks — extracting reusable logic
🟡 Higher-Order Components (HOC)
🟡 Render Props pattern
🟡 Container / Presentational pattern
🟡 Compound components
🟡 Provider pattern
🟡 Feature-based folder structure vs type-based
🟢 Atomic Design (atoms, molecules, organisms, templates, pages)
🟢 Module Federation (micro-frontends)
🟢 Monorepo strategies for React apps
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 15: Styling

```
🔴 Inline styles in React (style={{ }})
🔴 CSS Modules
🔴 className (not class)
🟡 CSS-in-JS libraries (styled-components, Emotion)
🟡 Tailwind CSS with React
🟡 Conditional classnames (clsx / classnames library)
🟡 Global vs scoped styles
🟢 CSS Variables with React
🟢 Theme switching (dark mode implementation)
🟢 Comparison: CSS Modules vs styled-components vs Tailwind
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 16: Testing

```
🔴 Why testing matters
🔴 Jest — basics (describe, it, expect)
🔴 React Testing Library — render, screen, fireEvent
🔴 Testing component rendering
🔴 Testing user interactions (click, type, submit)
🟡 Testing async operations (waitFor, findBy)
🟡 Mocking API calls
🟡 Testing custom hooks (renderHook)
🟡 Snapshot testing — what it is, pros/cons
🟡 Testing with context and providers
🟢 End-to-end testing (Cypress, Playwright)
🟢 Integration tests vs Unit tests vs E2E
🟢 Test coverage and what % to aim for
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 17: React Internals

```
🔴 Virtual DOM — how diffing works
🔴 Reconciliation algorithm
🟡 React Fiber architecture — what problem it solves
🟡 Render phase vs Commit phase
🟡 Batching state updates (React 17 vs React 18 automatic batching)
🟡 Concurrent rendering — what it means
🟡 Why keys matter for reconciliation
🟢 React Compiler (React Forget) — automatic memoization
🟢 Server Components vs Client Components
🟢 Streaming SSR
🟢 Selective hydration
🟢 How Suspense works internally
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Category 18: Machine Coding / Build This

> "Build this component in 30-45 minutes" — the most practical interview round.

```
🔴 Build a Todo app (add, delete, edit, mark complete)
🔴 Build a Counter (increment, decrement, reset)
🔴 Build a Search/Filter list
🔴 Build a Form with validation
🔴 Build a Toggle (show/hide content)
🟡 Build a Tabs component
🟡 Build an Accordion component
🟡 Build a Modal/Dialog
🟡 Build a Pagination component
🟡 Build an Infinite scroll list
🟡 Build a Debounced search input
🟡 Build a Star rating component
🟡 Build a Carousel/Slider
🟡 Build a Dropdown/Select component
🟡 Build a Dark mode toggle with Context
🟡 Build a Timer / Stopwatch
🔴 Build a Shopping cart (add, remove, quantity, total)
🟢 Build a Drag and drop list
🟢 Build a Multi-step form/wizard
🟢 Build a File uploader with progress
🟢 Build a Chat interface
🟢 Build an Autocomplete/Typeahead
🟢 Build a Kanban board (like Trello)
🟢 Build a Data table with sort and filter
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Summary Table

| # | Category | Sub-topics | Priority |
|---|----------|-----------|----------|
| 1 | React Fundamentals | 11 | 🔴 Must-know |
| 2 | JSX | 11 | 🔴 Must-know |
| 3 | Components | 13 | 🔴🟡 Mixed |
| 4 | Props | 11 | 🔴 Must-know |
| 5 | State | 12 | 🔴 Must-know |
| 6 | Hooks | 20 | 🔴 Must-know |
| 7 | Component Lifecycle | 9 | 🔴🟡 Mixed |
| 8 | Event Handling | 10 | 🔴🟡 Mixed |
| 9 | Conditional Rendering & Lists | 8 | 🔴 Must-know |
| 10 | Forms | 10 | 🔴🟡 Mixed |
| 11 | State Management | 15 | 🔴🟡 Mixed |
| 12 | React Router | 12 | 🔴🟡 Mixed |
| 13 | Performance Optimization | 17 | 🔴🟡🟢 Mixed |
| 14 | Patterns & Architecture | 12 | 🟡🟢 Mixed |
| 15 | Styling | 10 | 🟡 Should-know |
| 16 | Testing | 13 | 🟡🟢 Mixed |
| 17 | React Internals | 12 | 🟡🟢 Mixed |
| 18 | Machine Coding / Build This | 24 | 🔴🟡 Mixed |
| | **TOTAL** | **230** | |

[↑ Back to Table of Contents](#table-of-contents)

---

# The 80/20 — Focus Here First

> These 10 areas cover ~80% of all React interview questions.

```
 1. Hooks (useState, useEffect, useRef, useMemo, useCallback, custom hooks)
 2. State management (useState, Context API, lifting state, Redux basics)
 3. Component lifecycle (mount, update, unmount via useEffect)
 4. Virtual DOM & Reconciliation (how React updates efficiently)
 5. Props & State difference (immutability, one-way data flow)
 6. Controlled vs Uncontrolled components (forms)
 7. Key prop (why it matters in lists)
 8. Performance (React.memo, useMemo, useCallback, lazy loading)
 9. React Router basics (routes, params, navigation)
10. Build a component (Todo, Form, Search — machine coding)
```

```
PREPARATION TIME ESTIMATES:
  🔴 Must-know only           → 1-2 weeks (minimum viable prep)
  🔴 + 🟡 Must + Should       → 3-4 weeks (solid prep)
  🔴 + 🟡 + 🟢 Everything     → 6-8 weeks (thorough prep for FAANG)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Prompt 2 — Ask for the Full Guide

> Copy-paste one of these into a new conversation after reviewing the map.

## Option A: Everything

```
Based on these 18 categories of React.js interview topics
(230 sub-topics), give me the COMPLETE preparation guide.

For EACH sub-topic include:
  - Clear explanation (2-3 sentences, simple)
  - Code example showing how it works
  - Most common interview question about it (with answer)
  - Common mistake candidates make
  - One-liner summary for quick revision

Organize by category. 🔴 first, then 🟡, then 🟢.

CATEGORIES (all 18):
  1. React Fundamentals
  2. JSX
  3. Components
  4. Props
  5. State
  6. Hooks
  7. Component Lifecycle
  8. Event Handling
  9. Conditional Rendering & Lists
  10. Forms
  11. State Management
  12. React Router
  13. Performance Optimization
  14. Patterns & Architecture
  15. Styling
  16. Testing
  17. React Internals
  18. Machine Coding (build these components with full code)

FORMAT: Single markdown, clickable TOC, React/JSX code with
comments, difficulty tags (🔴🟡🟢).
LEVEL: Know JS and basic React, preparing for interviews.
STYLE: Practical, interview-focused.
```

## Option B: Must-Know Only (Quick Prep)

```
Give me a React interview guide covering ONLY 🔴 must-know topics:

  1. Virtual DOM & Reconciliation
  2. JSX basics and rules
  3. Functional vs Class components
  4. Props (passing, children, destructuring)
  5. State (useState, immutability, lifting state)
  6. Core hooks (useState, useEffect, useRef, useMemo, useCallback)
  7. Custom hooks
  8. Component lifecycle via useEffect
  9. Event handling (onClick, onChange, synthetic events)
  10. Conditional rendering & lists with keys
  11. Controlled vs Uncontrolled forms
  12. Context API
  13. Redux basics
  14. React Router basics
  15. React.memo, code splitting, lazy loading
  16. Build: Todo app, Search filter, Form with validation

For each: explanation + code + interview question + answer.
Markdown, TOC, React code with comments. Interview-focused.
```

## Option C: One Category at a Time

```
Give me the complete React interview guide for ONLY
[CATEGORY NAME] (e.g., "Hooks" or "State Management").

Cover every sub-topic with:
  - Explanation with code
  - Top 5 interview questions with answers
  - Common mistakes
  - One-liner revision notes

Markdown, code with comments, simple language.
```

## Option D: Question Bank Only (No Teaching)

```
Give me a React.js interview QUESTION BANK for all 18 categories.
Only questions and answers, no teaching.
40-50 questions total, tagged by difficulty (🟢🟡🔴).
Include "What is the output?" questions where relevant.
Markdown format with a summary table at the end.
```

[↑ Back to Table of Contents](#table-of-contents)
