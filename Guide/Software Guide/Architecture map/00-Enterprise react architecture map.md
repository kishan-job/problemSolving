# Enterprise React App — Architecture Map
> Complete guide from project setup to production support

---

## Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Recommended |
| 🔵 | Alternative |
| ⭐ | Added — was missing from original |
| ⚠️ | Situational |
| ~~strikethrough~~ | Avoid |
| 🔴 | Legacy only |

---

## Table of Contents

- [Step 1 · Project Setup](#step-1--project-setup)
- [Step 2 · Language & Code Quality](#step-2--language--code-quality)
- [Step 3 · Architecture & Component Patterns](#step-3--architecture--component-patterns)
- [Step 4 · State Management](#step-4--state-management)
- [Step 5 · Routing](#step-5--routing)
- [Step 6 · UI Layer](#step-6--ui-layer)
- [Step 7 · API Communication](#step-7--api-communication)
- [Step 8 · Forms ⭐](#step-8--forms-)
- [Step 9 · Testing](#step-9--testing)
- [Step 10 · CI/CD & Observability](#step-10--cicd--observability)
- [Step 11 · Security ⭐](#step-11--security-)
- [Step 12 · Accessibility (a11y) ⭐](#step-12--accessibility-a11y-)
- [Step 13 · Internationalisation (i18n) ⭐](#step-13--internationalisation-i18n-)
- [Step 14 · Performance ⭐](#step-14--performance-)
- [Step 15 · Auth & Identity ⭐](#step-15--auth--identity-)
- [Step 16 · Documentation & DX ⭐](#step-16--documentation--dx-)
- [Quick Reference — Full Recommended Stack](#quick-reference--full-recommended-stack)

---

## Step 1 · Project Setup
> The build tool and framework that powers your app

### In this step

**⚙️ Build Tools** — how your app is compiled and served locally
- Vite ✅
- ~~Create React App~~

**🏗️ Frameworks** — what sits on top of React for routing and rendering
- Next.js 🔵
- Remix 🔵
- TanStack Start ⭐

---

### ⚙️ Build Tools

| Tool | Status | Use When |
|------|--------|----------|
| **Vite** | ✅ | New SPA — fastest dev experience |
| ~~Create React App~~ | Avoid | Unmaintained |

```bash
npm create vite@latest my-app -- --template react-ts
```

### 🏗️ Frameworks

| Tool | Status | Use When |
|------|--------|----------|
| **Next.js** | 🔵 | Need SSR or SSG |
| **Remix** | 🔵 | Server-first data loading, progressive enhancement |
| **TanStack Start** | ⭐ | Full-stack React with type-safe routing, emerging alternative to Next.js |

[↑ Back to top](#table-of-contents)

---

## Step 2 · Language & Code Quality
> Tools that catch mistakes before they reach users

### In this step

**🔒 Type Safety** — catch wrong types and missing fields before runtime
- TypeScript ✅

**🔍 Linting & Formatting** — enforce code style and catch bad patterns automatically
- ESLint ✅
- Prettier ✅
- Biome ⭐

**🚦 Git Hooks** — block bad code before it enters the repo
- Husky + lint-staged ✅

**🧹 Dead Code Detection** — find unused exports and dependencies before they pile up
- Knip ⭐

---

### 🔒 Type Safety

**TypeScript** ✅
- Catches wrong prop types and missing fields at compile time
- Config: `tsconfig.json`
- Runs in editor (red squiggles) + on build (`tsc`)

### 🔍 Linting & Formatting

**ESLint** ✅
- Catches code quality issues — unused variables, broken hook rules
- Key plugins: `eslint-plugin-react-hooks`, `eslint-plugin-react-refresh`
- Config: `eslint.config.js`

**Prettier** ✅
- Auto-formats code on save — spacing, indentation, quotes
- Config: `.prettierrc`
- Add `eslint-config-prettier` to avoid ESLint conflicts

**Biome** ⭐
- Single tool that replaces both ESLint and Prettier
- Significantly faster (written in Rust), zero config
- Good choice for new projects that want less setup overhead

### 🚦 Git Hooks

**Husky + lint-staged** ✅
- **Husky** — runs a script before every `git commit`
- **lint-staged** — runs lint/format only on staged files (fast)
- Result: bad code is blocked before it enters the repo

### 🧹 Dead Code Detection

**Knip** ⭐
- Finds unused exports, files, and bloated `package.json` dependencies
- Run in CI to prevent dead code accumulation over time

```bash
npm install -D eslint @eslint/js typescript-eslint eslint-plugin-react-hooks
npm install -D prettier eslint-config-prettier
npm install -D husky lint-staged && npx husky init
# or replace ESLint + Prettier with:
npm install -D @biomejs/biome
# dead code detection:
npm install -D knip
```

[↑ Back to top](#table-of-contents)

---

## Step 3 · Architecture & Component Patterns
> How your codebase is organised, how components are built, and how they communicate

### In this step

**📁 Folder Structure** — how to organise files so the codebase scales
- FSD ✅, Atomic Design 🔵, Modular 🔵, Vertical Slices ⭐

**🧩 Component Patterns** — reusable ways to structure and compose components
- Smart / Dumb (Container / Presentational)
- Compound components ⭐
- Render props ⭐
- Controlled vs uncontrolled ⭐
- Forward refs ⭐
- Polymorphic components ⭐
- Children as prop (composition)

**🪝 Hook Patterns** — extract and reuse logic cleanly across components
- Custom hooks as abstraction layer ⭐
- useReducer for complex local state ⭐
- useCallback / useMemo ⭐
- useTransition / useDeferredValue ⭐

**🔀 Data Flow Patterns** — how state and data move through the component tree
- Prop drilling — what it is and how to avoid it
- Context + Provider pattern
- Colocation ⭐
- Lifting state up ⭐
- Derived state vs stored state ⭐
- Optimistic updates ⭐

**🗂️ Code Organisation Patterns** — keep modules clean, decoupled, and testable
- Barrel files and public API pattern ⭐
- Dependency inversion in components ⭐

**🖥️ Rendering Patterns** — control what renders, when, and how failures are handled
- Portal pattern ⭐
- Error boundaries ⭐
- Suspense boundaries ⭐
- List rendering and stable keys ⭐
- Conditional rendering strategies ⭐

---

### 📁 Folder Structure

#### ✅ Feature-Sliced Design — Recommended for enterprise

```
src/
├── app/                    ← App-wide setup: router, global providers
├── pages/                  ← Route-level components (one per URL)
├── features/               ← Business features, each fully self-contained
│     ├── auth/
│     │     ├── ui/
│     │     ├── api/
│     │     ├── model/
│     │     └── index.ts    ← public API of this feature
│     └── orders/
├── entities/               ← Business objects (pure data/model layer)
└── shared/                 ← Truly reusable across entire app
      ├── ui/
      └── lib/
```

**Why FSD:** new developers find everything for a feature in one place; lower layers cannot import from higher layers; teams can own separate features independently.

#### 🔵 Atomic Design — UI-component focused

```
src/
├── atoms/        ← Button, Input, Badge
├── molecules/    ← FormField (label + input + error)
├── organisms/    ← Header, ProductCard
├── templates/    ← Page layouts
├── pages/        ← Route pages with real data
└── services/
```

#### 🔵 Modular by type — simple default

```
src/
├── components/
├── pages/
├── hooks/
├── services/
├── store/
└── utils/
```

#### ⭐ Vertical Slice Architecture

Organises code by feature end-to-end (UI + logic + API + tests in one folder). Closer to FSD but without strict layer rules. Good for teams that find FSD too rigid.

---

### 🧩 Component Patterns

#### Smart / Dumb (Container / Presentational)

```jsx
// Smart — fetches data, holds state, passes down as props
function UserListContainer() {
  const { data: users } = useQuery({ queryKey: ['users'], queryFn: fetchUsers })
  return <UserList users={users} />
}

// Dumb — only renders, no logic, fully reusable and easy to test
function UserList({ users }) {
  return users.map(u => <UserCard key={u.id} name={u.name} />)
}
```

#### Compound Components ⭐

Parent and children share implicit state through context. The caller composes the pieces — no prop drilling needed.

```jsx
// Usage — caller decides the structure
<Tabs defaultValue="orders">
  <Tabs.List>
    <Tabs.Tab value="orders">Orders</Tabs.Tab>
    <Tabs.Tab value="returns">Returns</Tabs.Tab>
  </Tabs.List>
  <Tabs.Panel value="orders"><OrdersTable /></Tabs.Panel>
  <Tabs.Panel value="returns"><ReturnsTable /></Tabs.Panel>
</Tabs>

// Implementation — parent shares state via context
const TabsContext = createContext(null)

function Tabs({ defaultValue, children }) {
  const [active, setActive] = useState(defaultValue)
  return (
    <TabsContext.Provider value={{ active, setActive }}>
      {children}
    </TabsContext.Provider>
  )
}
Tabs.Tab   = function Tab({ value, children }) {
  const { active, setActive } = useContext(TabsContext)
  return <button role="tab" aria-selected={active === value} onClick={() => setActive(value)}>{children}</button>
}
Tabs.Panel = function Panel({ value, children }) {
  const { active } = useContext(TabsContext)
  return active === value ? <div role="tabpanel">{children}</div> : null
}
```

> Use for: Tabs, Accordion, Select, Dropdown, Modal — any component with multiple coordinated sub-parts.

#### Render Props ⭐

A component shares its internal state by calling a function prop instead of rendering children directly.

```jsx
function MouseTracker({ render }) {
  const [pos, setPos] = useState({ x: 0, y: 0 })
  return (
    <div onMouseMove={e => setPos({ x: e.clientX, y: e.clientY })}>
      {render(pos)}
    </div>
  )
}

<MouseTracker render={({ x, y }) => <p>Mouse at {x}, {y}</p>} />
```

> Mostly replaced by custom hooks today, but still used in libraries like React Hook Form's `Controller`.

#### Controlled vs Uncontrolled Components ⭐

```jsx
// Controlled — React owns the value
function Controlled() {
  const [value, setValue] = useState('')
  return <input value={value} onChange={e => setValue(e.target.value)} />
}

// Uncontrolled — DOM owns the value, read via ref
function Uncontrolled() {
  const ref = useRef(null)
  return <input ref={ref} defaultValue="" />
}
```

> Prefer controlled for form validation. Use uncontrolled for file inputs or integrating with non-React code.

#### Forward Refs ⭐

Pass a ref through a component to a DOM element inside it.

```jsx
const Input = forwardRef(function Input({ label, ...props }, ref) {
  return (
    <label>
      {label}
      <input ref={ref} {...props} />
    </label>
  )
})

const inputRef = useRef(null)
<Input ref={inputRef} label="Email" />
inputRef.current.focus()
```

> Use when a parent needs direct DOM access into a child — focus management, scroll, animation libraries.

#### Polymorphic Components ⭐

A component that renders as different HTML elements depending on an `as` prop.

```jsx
function Text({ as: Tag = 'p', children, ...props }) {
  return <Tag {...props}>{children}</Tag>
}

<Text as="h1">Page title</Text>
<Text as="span">Inline label</Text>
<Text as={Link} to="/home">Go home</Text>
```

> Common in design systems — Button, Text, Box components that need to change their element while keeping consistent styles.

#### Children as Prop (Composition)

Parent provides the shell. Caller decides what goes inside.

```jsx
function Layout({ children }) {
  return (
    <div className="page-wrapper">
      <Navbar />
      <main>{children}</main>
      <Footer />
    </div>
  )
}

<Layout><Dashboard /></Layout>
```

---

### 🪝 Hook Patterns

#### Custom Hooks as Abstraction Layer ⭐

Extract logic into custom hooks before reaching for state management libraries.

```jsx
function useOrders(userId) {
  return useQuery({
    queryKey: ['orders', userId],
    queryFn: () => fetchOrders(userId)
  })
}

function OrdersPage() {
  const { data, isLoading } = useOrders(currentUser.id)
}
```

#### useReducer for Complex Local State ⭐

When `useState` requires multiple related state updates in one action, use `useReducer`.

```jsx
const initialState = { step: 1, data: {}, errors: {} }

function reducer(state, action) {
  switch (action.type) {
    case 'NEXT_STEP': return { ...state, step: state.step + 1, data: { ...state.data, ...action.payload } }
    case 'PREV_STEP': return { ...state, step: state.step - 1 }
    case 'SET_ERROR': return { ...state, errors: action.payload }
    default: return state
  }
}

const [state, dispatch] = useReducer(reducer, initialState)
```

#### useCallback / useMemo — When to Use vs Skip ⭐

```jsx
// useMemo — cache an expensive computation
const sorted = useMemo(() => expensiveSort(items), [items])

// useCallback — stable function reference for React.memo children
const handleDelete = useCallback((id) => dispatch({ type: 'DELETE', id }), [dispatch])

// Only use when:
//   (1) passing to React.memo children
//   (2) value is in a dependency array
//   (3) computation is genuinely expensive
```

#### useTransition / useDeferredValue ⭐

Keep the UI responsive during expensive state updates.

```jsx
// useTransition — mark an update as non-urgent so typing stays responsive
const [isPending, startTransition] = useTransition()
startTransition(() => setSearchQuery(input))

// useDeferredValue — defer re-rendering a slow part of the tree
const deferredQuery = useDeferredValue(searchQuery)
```

---

### 🔀 Data Flow Patterns

#### Prop Drilling — Avoid Past 2 Levels

```
Page (has the data)
  └── Section (passes it down, doesn't use it)
        └── Widget (passes it down, doesn't use it)
              └── Button (finally uses it)  ← BAD
```

Solve with Context + Provider for rarely-changing values, or Zustand for frequently-changing ones.

#### Context + Provider Pattern

```jsx
<UserContext.Provider value={user}>
  <Header />      {/* reads directly via useContext */}
  <Dashboard />   {/* reads directly via useContext */}
</UserContext.Provider>
```

> ✅ Use for: theme, logged-in user, language — values that rarely change
> ❌ Avoid for: cart, modals, notifications — values that change often (re-renders everything)

#### Colocation ⭐

Keep state as close to where it's used as possible. Don't lift state higher than necessary.

```jsx
// ❌ Lifted too high — App re-renders when modal opens
function App() {
  const [modalOpen, setModalOpen] = useState(false)
  return <OrdersPage modalOpen={modalOpen} setModalOpen={setModalOpen} />
}

// ✅ Colocated — only OrdersPage re-renders
function OrdersPage() {
  const [modalOpen, setModalOpen] = useState(false)
  return <>{modalOpen && <OrderModal />}</>
}
```

#### Lifting State Up ⭐

When two siblings need to share state, lift it to their closest common parent.

```jsx
function Parent() {
  const [selected, setSelected] = useState(null)
  return (
    <>
      <ProductList onSelect={setSelected} />
      <ProductDetail product={selected} />
    </>
  )
}
```

#### Derived State vs Stored State ⭐

Compute values from existing state instead of storing them separately. Two sources of truth always drift.

```jsx
// ❌ Stored — total can get out of sync with items
const [items, setItems] = useState([])
const [total, setTotal] = useState(0)

// ✅ Derived — always correct, zero extra state
const [items, setItems] = useState([])
const total = items.reduce((sum, item) => sum + item.price, 0)
```

#### Optimistic Updates ⭐

Update the UI before the server confirms — makes the app feel instant, roll back on failure.

```jsx
const mutation = useMutation({
  mutationFn: (id) => deleteOrder(id),
  onMutate: async (id) => {
    await queryClient.cancelQueries({ queryKey: ['orders'] })
    const previous = queryClient.getQueryData(['orders'])
    queryClient.setQueryData(['orders'], old => old.filter(o => o.id !== id))
    return { previous }
  },
  onError: (err, id, context) => {
    queryClient.setQueryData(['orders'], context.previous)
  }
})
```

---

### 🗂️ Code Organisation Patterns

#### Barrel Files (`index.ts`) ⭐

```ts
// features/auth/index.ts
export { LoginForm } from './ui/LoginForm'
export { useAuth }   from './model/useAuth'
export type { AuthUser } from './model/types'

// Consumer — clean import, no internal path knowledge needed
import { LoginForm, useAuth } from '@/features/auth'
```

> Tradeoff: convenient imports but can hurt tree-shaking. Keep barrel files shallow — one level only.

#### Public API Pattern ⭐

Each feature exposes only what other modules need. Everything else stays internal.

```
features/auth/
  ui/LoginForm.tsx       ← internal
  model/useAuth.ts       ← internal
  index.ts               ← only public exports live here
```

#### Dependency Inversion in Components ⭐

Inject dependencies via props instead of hardcoding them. Makes components testable without mocking modules.

```jsx
// ❌ Hard dependency — test must mock the fetch module
function UserList() {
  const { data } = useQuery({ queryFn: () => fetch('/api/users') })
}

// ✅ Injected — swap the fetcher in tests with no mocking needed
function UserList({ fetchUsers = defaultFetchUsers }) {
  const { data } = useQuery({ queryFn: fetchUsers })
}
```

---

### 🖥️ Rendering Patterns

#### Portal Pattern ⭐

Render a component outside its parent DOM tree — required for modals and tooltips to escape `overflow` / `z-index` constraints.

```jsx
import { createPortal } from 'react-dom'

function Modal({ children, isOpen }) {
  if (!isOpen) return null
  return createPortal(
    <div className="modal-overlay">{children}</div>,
    document.getElementById('modal-root')
  )
}
```

#### Error Boundaries ⭐

Catch JavaScript errors in a component tree and show a fallback UI instead of crashing the whole app.

```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false }
  static getDerivedStateFromError() { return { hasError: true } }
  componentDidCatch(error, info) { reportToSentry(error, info) }
  render() {
    return this.state.hasError
      ? <p>Something went wrong. <button onClick={() => this.setState({ hasError: false })}>Retry</button></p>
      : this.props.children
  }
}

// Wrap each major section separately so one crash doesn't kill the whole page
<ErrorBoundary><OrdersSection /></ErrorBoundary>
<ErrorBoundary><NotificationsSection /></ErrorBoundary>
```

> Use `react-error-boundary` library for a hooks-friendly version.

#### Suspense Boundaries ⭐

Show a fallback while a component is loading data or code.

```jsx
<Suspense fallback={<Skeleton />}>
  <UserProfile />     {/* lazy-loaded component */}
</Suspense>

<Suspense fallback={<TableSkeleton />}>
  <OrdersTable />     {/* component that suspends while fetching */}
</Suspense>
```

> Place Suspense boundaries close to the suspending component — not at the top of the tree.

#### List Rendering and Stable Keys ⭐

```jsx
// ❌ Index as key — breaks when list reorders or items are removed
{items.map((item, i) => <Row key={i} item={item} />)}

// ✅ Stable unique ID
{items.map(item => <Row key={item.id} item={item} />)}
```

#### Conditional Rendering Strategies ⭐

```jsx
// Short-circuit — simple show/hide
{isLoggedIn && <Dashboard />}

// Ternary — either/or
{isLoading ? <Spinner /> : <Content />}

// Early return — cleaner for multiple guards
function Page() {
  if (isLoading) return <Spinner />
  if (error)     return <ErrorMessage />
  if (!data)     return null
  return <Content data={data} />
}

// Component map — avoids long if/else for role-based UI
const dashboardMap = { admin: AdminDash, user: UserDash, guest: GuestDash }
const Dashboard = dashboardMap[role] ?? GuestDash
```

[↑ Back to top](#table-of-contents)

---

## Step 4 · State Management
> Right tool for the right kind of state

### In this step

**💡 Concepts** — understand the problem before picking a tool
- Why Redux was overused and what replaced it
- The 3 kinds of state and the right tool for each
- The Golden Rule — never duplicate server state
- Decision flow
- Real e-commerce example — all tools together

**🌐 Server State Tools** — data that lives on the backend and needs caching, syncing, and refetching
- TanStack Query ✅
- SWR 🔵
- Apollo Client 🔵

**🖥️ Client State Tools** — UI state that lives only in the browser
- Zustand ✅
- Jotai 🔵
- Valtio ⭐
- XState ⭐
- Redux Toolkit 🔴

---

### 💡 Concepts

#### Why Redux Was Overused and What Replaced It

Before 2020, React had no good answer for sharing API data across the app. Developers used Redux for everything — both server data and UI state. Fetching a simple user list required action types, a reducer, a thunk, and a selector — ~60 lines for one API call, with no caching, no background sync, and no deduplication built in.

In 2020, TanStack Query arrived with one key insight: API data is fundamentally different from UI state. It has a lifecycle — it goes stale, needs refreshing, can fail, should be cached. Everything developers were painfully building on top of Redux — caching, loading states, background refetching — TanStack Query made automatic.

**Result today:** TanStack Query replaced Redux for server data. Zustand replaced Redux for shared UI state. Redux now only appears in large existing codebases.

#### The 3 Kinds of State

| State type | Definition | Example | Tool |
|------------|-----------|---------|------|
| **Server state** | Comes from API / backend | users, products, orders | TanStack Query |
| **Shared UI state** | Shared, changes often, no API | modal, cart, tabs | Zustand |
| **Global config** | App-wide, rarely changes | theme, language, auth user | useContext |
| **Local state** | One component only | input value, toggle | useState |
| **Complex machines** | Many transitions, lifecycle | wizard, websocket | XState ⭐ |

#### The Golden Rule

```
❌ Never put API data into Redux or Zustand
   fetch('/api/users') → store in Zustand → two sources of truth → bugs

✅ TanStack Query owns all server data
   It already caches it. Storing it again causes sync issues.
```

#### Decision Flow

```
Is the data from an API?
  YES → TanStack Query. Always.

  NO  → Is it shared across components?

    YES → Changes frequently? (cart, modal, notifications)
            YES → Zustand
            NO  → useContext (theme, auth user, language)
          Complex lifecycle / many transitions?
            YES → XState ⭐

    NO  → useState (or useReducer if multiple related fields ⭐)
```

#### Real E-commerce Example — All Tools Together

```
TanStack Query owns:        Zustand owns:           Context owns:
  product list                cart items              logged-in user
  user orders                 modal open/closed       dark/light theme
  search results              sidebar collapsed       selected language
  notifications               checkout step           feature flags
```

---

### 🌐 Server State Tools

#### TanStack Query ✅

```jsx
const { data: users, isLoading, isError } = useQuery({
  queryKey: ['users'],
  queryFn: () => fetch('/api/users').then(r => r.json())
})

const mutation = useMutation({
  mutationFn: (newUser) => fetch('/api/users', { method: 'POST', body: JSON.stringify(newUser) }),
  onSuccess: () => queryClient.invalidateQueries({ queryKey: ['users'] })
})
```

10 components using the same `queryKey` = 1 network request, all share the result.

#### SWR 🔵

```jsx
const { data, isLoading } = useSWR('/api/users', fetcher)
```

Simpler API, fewer features — good for small/medium Next.js apps.

#### Apollo Client 🔵

Only use if your backend exposes a GraphQL API. Normalised cache means the same entity across 10 queries is stored once and stays in sync.

---

### 🖥️ Client State Tools

#### Zustand ✅

```jsx
const useCartStore = create((set) => ({
  items: [],
  addItem:    (item) => set((s) => ({ items: [...s.items, item] })),
  removeItem: (id)   => set((s) => ({ items: s.items.filter(i => i.id !== id) })),
  clearCart:  ()     => set({ items: [] })
}))
```

No Provider needed. Only re-renders components subscribed to the changed slice.

#### Jotai 🔵

```jsx
const modalOpenAtom = atom(false)
function Button() {
  const [isOpen, setIsOpen] = useAtom(modalOpenAtom)
  return <button onClick={() => setIsOpen(true)}>Open</button>
}
```

#### Valtio ⭐

```jsx
const state = proxy({ count: 0 })
function Counter() {
  const snap = useSnapshot(state)
  return <button onClick={() => state.count++}>{snap.count}</button>
}
```

Mutable proxy model — feels closer to plain JavaScript than Zustand's functional style.

#### XState ⭐

State machines for complex UI flows — multi-step forms, websocket lifecycle, wizard flows. Prevents impossible states by explicitly modelling every transition.

```bash
npm install xstate @xstate/react
```

#### Redux Toolkit 🔴

Only for existing codebases already using Redux. TanStack Query + Zustand replaces it completely for new projects.

[↑ Back to top](#table-of-contents)

---

## Step 5 · Routing
> How users navigate between pages

### In this step

**💡 Concepts** — routing patterns every React app needs
- Route-level code splitting ⭐
- Protected routes ⭐
- File-based routes ⭐

**🛠️ Tools** — router libraries and their trade-offs
- React Router v6 ✅
- TanStack Router 🔵
- Next.js App Router 🔵
- Remix Router 🔵

---

### 💡 Concepts

#### Route-level Code Splitting ⭐

Each page becomes its own chunk loaded on demand — the most impactful single performance improvement for most SPAs.

```jsx
const OrdersPage  = lazy(() => import('./pages/OrdersPage'))
const ProfilePage = lazy(() => import('./pages/ProfilePage'))

<Suspense fallback={<PageSpinner />}>
  <Routes>
    <Route path="/orders"  element={<OrdersPage />} />
    <Route path="/profile" element={<ProfilePage />} />
  </Routes>
</Suspense>
```

#### Protected Routes ⭐

```jsx
function ProtectedRoute({ children }) {
  const { isSignedIn, isLoaded } = useAuth()
  if (!isLoaded)   return <Spinner />
  if (!isSignedIn) return <Navigate to="/login" replace />
  return children
}

<Route path="/orders" element={<ProtectedRoute><OrdersPage /></ProtectedRoute>} />
```

#### File-based Routes ⭐

Automatic route generation from the file structure — no manual route config needed. Supported natively in Next.js, TanStack Start, and via `generouted` in Vite.

---

### 🛠️ Tools

| Tool | Status | Use When |
|------|--------|----------|
| **React Router v6** | ✅ | Standard for Vite SPA |
| **TanStack Router** | 🔵 | Full TypeScript type-safety on route params |
| **Next.js App Router** | 🔵 | If using Next.js — built in |
| **Remix Router** | 🔵 | If using Remix — built in |

```bash
npm install react-router-dom
```

[↑ Back to top](#table-of-contents)

---

## Step 6 · UI Layer
> What components look like and how they are styled

### In this step

**💡 Concepts** — UI patterns that go beyond picking a library
- Component memoisation with React.memo ⭐
- Design tokens ⭐
- CVA for component variants ⭐

**🧱 Component Libraries** — pre-built accessible component sets
- shadcn/ui ✅
- Radix UI 🔵
- Ark UI ⭐
- MUI 🔵
- Ant Design 🔵

**🎨 Styling Tools** — how CSS is written and scoped
- Tailwind CSS ✅
- CVA ⭐
- CSS Modules 🔵
- Panda CSS ⭐
- Styled Components 🔵
- Emotion 🔵

---

### 💡 Concepts

#### Component Memoisation — React.memo ⭐

Prevent a child component from re-rendering when its parent re-renders but props haven't changed.

```jsx
// Without memo — re-renders every time parent renders
function ProductCard({ product }) { ... }

// With memo — only re-renders when product prop changes
const ProductCard = React.memo(function ProductCard({ product }) { ... })
```

> Use when: (1) the component renders often, (2) with the same props, (3) and the render is expensive. Don't wrap everything — memoisation itself has a cost.

#### Design Tokens ⭐

Centralise all design decisions (colors, spacing, typography, radii) as tokens — not scattered Tailwind classes or hardcoded values. Use **Style Dictionary** to transform tokens into Tailwind config, CSS variables, and mobile values from one source.

#### CVA — Component Variants ⭐

Type-safe component variants on top of Tailwind — replaces messy string conditionals.

```jsx
import { cva } from 'class-variance-authority'

const button = cva('rounded font-medium', {
  variants: {
    intent: {
      primary:     'bg-blue-600 text-white hover:bg-blue-700',
      destructive: 'bg-red-600 text-white hover:bg-red-700',
    },
    size: {
      sm: 'px-3 py-1 text-sm',
      md: 'px-4 py-2 text-base',
    }
  },
  defaultVariants: { intent: 'primary', size: 'md' }
})

<button className={button({ intent: 'destructive', size: 'sm' })}>Delete</button>
```

---

### 🧱 Component Libraries

| Library | Status | When to use |
|---------|--------|-------------|
| **shadcn/ui** | ✅ | Copy-pasted into your project — you own it, fully customisable |
| **Radix UI** | 🔵 | Headless, unstyled, accessible primitives |
| **Ark UI** | ⭐ | Headless alternative to Radix — broader component set |
| **MUI** | 🔵 | Full featured, Google Material design |
| **Ant Design** | 🔵 | Full featured, popular in enterprise |

---

### 🎨 Styling Tools

| Tool | Status | When to use |
|------|--------|-------------|
| **Tailwind CSS** | ✅ | Utility-first, pairs perfectly with shadcn/ui |
| **CVA** | ⭐ | Type-safe variants on top of Tailwind |
| **CSS Modules** | 🔵 | Scoped CSS per component, traditional approach |
| **Panda CSS** | ⭐ | Type-safe CSS-in-JS with zero runtime |
| **Styled Components** | 🔵 | CSS-in-JS, has runtime cost |
| **Emotion** | 🔵 | CSS-in-JS, used inside MUI |

> **Recommended combo: shadcn/ui + Tailwind CSS + CVA + design tokens**

[↑ Back to top](#table-of-contents)

---

## Step 7 · API Communication
> How React talks to the backend

### In this step

**💡 Concepts** — patterns for reliable, typed API communication
- Axios interceptor pattern ⭐
- Zod for runtime API validation ⭐

**🔗 REST Tools** — fetching from REST APIs
- Axios ✅
- Fetch API 🔵
- tRPC 🔵
- OpenAPI codegen ⭐

**📡 GraphQL Tools** — fetching from GraphQL APIs
- Apollo Client 🔵
- urql 🔵

**⚡ Real-time Tools** — live data from the server
- WebSockets ⭐
- SSE (Server-Sent Events) ⭐

---

### 💡 Concepts

#### Axios Interceptor Pattern ⭐

Centralise auth headers, error handling, and token refresh in one place.

```ts
const client = axios.create({ baseURL: import.meta.env.VITE_API_URL })

client.interceptors.request.use(config => {
  const token = getAccessToken()
  if (token) config.headers.Authorization = `Bearer ${token}`
  return config
})

client.interceptors.response.use(
  response => response,
  async error => {
    if (error.response?.status === 401) {
      await refreshToken()
      return client(error.config)
    }
    return Promise.reject(error)
  }
)
```

#### Zod for API Validation ⭐

Validate API responses at the boundary so bad data never reaches your UI silently.

```ts
const UserSchema = z.object({
  id:    z.string().uuid(),
  email: z.string().email(),
  role:  z.enum(['admin', 'user', 'guest'])
})

const fetchUser = async (id: string) => {
  const data = await client.get(`/users/${id}`).then(r => r.data)
  return UserSchema.parse(data) // throws if shape is wrong
}
```

---

### 🔗 REST Tools

| Tool | Status | When to use |
|------|--------|-------------|
| **Axios** | ✅ | Most common — interceptors, global error handling |
| **Fetch API** | 🔵 | Built-in browser, no install needed |
| **tRPC** | 🔵 | Full end-to-end TypeScript — no separate API schema |
| **OpenAPI codegen** | ⭐ | Auto-generate typed clients from an OpenAPI spec |

### 📡 GraphQL Tools

| Tool | Status | When to use |
|------|--------|-------------|
| **Apollo Client** | 🔵 | Full-featured, normalised cache |
| **urql** | 🔵 | Lightweight alternative to Apollo |

### ⚡ Real-time Tools

| Tool | Status | When to use |
|------|--------|-------------|
| **WebSockets** | ⭐ | Bidirectional — chat, live collaboration, gaming |
| **SSE** | ⭐ | One-way server push — notifications, live feeds, simpler than WebSockets |

> TanStack Query wraps whichever fetcher you choose.

[↑ Back to top](#table-of-contents)

---

## Step 8 · Forms ⭐
> Form state, validation, and multi-step patterns — missing from the original guide

### In this step

**💡 Concepts** — form patterns that scale in enterprise apps
- Multi-step form pattern ⭐
- Field array pattern (dynamic rows) ⭐

**🛠️ Tools** — form libraries and validation
- React Hook Form ✅
- Zod + React Hook Form ✅

---

### 💡 Concepts

#### Multi-step Form Pattern ⭐

Hold form state across steps in Zustand or `useReducer`. Don't reset state on step change.

```jsx
const useCheckoutStore = create((set) => ({
  step: 1,
  data: {},
  nextStep: (stepData) => set(s => ({ step: s.step + 1, data: { ...s.data, ...stepData } })),
  prevStep: () => set(s => ({ step: s.step - 1 })),
}))

function CheckoutWizard() {
  const { step } = useCheckoutStore()
  return (
    <>
      {step === 1 && <ShippingStep />}
      {step === 2 && <PaymentStep />}
      {step === 3 && <ReviewStep />}
    </>
  )
}
```

#### Field Array Pattern ⭐

Dynamic lists of form fields — add and remove rows at runtime.

```jsx
const { fields, append, remove } = useFieldArray({ control, name: 'items' })

{fields.map((field, i) => (
  <div key={field.id}>
    <input {...register(`items.${i}.name`)} placeholder="Item name" />
    <button type="button" onClick={() => remove(i)}>Remove</button>
  </div>
))}
<button type="button" onClick={() => append({ name: '', qty: 1 })}>Add item</button>
```

---

### 🛠️ Tools

#### React Hook Form ✅

```bash
npm install react-hook-form
```

```jsx
function LoginForm() {
  const { register, handleSubmit, formState: { errors } } = useForm()
  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register('email', { required: 'Email is required' })} />
      {errors.email && <p>{errors.email.message}</p>}
      <button type="submit">Log in</button>
    </form>
  )
}
```

#### Zod + React Hook Form ✅

Define the schema once — get TypeScript types and runtime validation for free.

```bash
npm install @hookform/resolvers zod
```

```tsx
const schema = z.object({
  email:    z.string().email('Invalid email'),
  password: z.string().min(8, 'Min 8 characters'),
})

type FormData = z.infer<typeof schema>

const { register, handleSubmit } = useForm<FormData>({
  resolver: zodResolver(schema)
})
```

[↑ Back to top](#table-of-contents)

---

## Step 9 · Testing
> Making sure the app works before it reaches users

### In this step

**💡 Concepts** — testing philosophy and what to test
- Test behaviour, not implementation ⭐
- Test pyramid for React ⭐

**🔬 Unit & Integration Tools** — fast tests for components, hooks, and logic
- Vitest ✅
- React Testing Library ✅
- MSW ⭐
- Jest 🔵

**🌐 E2E & Visual Tools** — tests that run a real browser
- Playwright ✅
- Cypress 🔵
- Storybook 🔵
- Storybook Interaction Tests ⭐
- Chromatic 🔵

**♿ Quality & Contract Tools** — catch a11y issues and API contract breaks in CI
- axe-core / jest-axe ⭐
- Contract testing (Pact) ⭐

---

### 💡 Concepts

#### Test Behaviour, Not Implementation ⭐

```
✅ "When the user clicks delete, the item disappears"
❌ "When deleteItem() is called, state.items has one fewer element"
```

Test what the user sees and does — not internal function calls or state shape.

#### Test Pyramid for React ⭐

```
Many   → unit tests for custom hooks and pure utility functions
Some   → integration tests for components with React Testing Library
Few    → E2E tests for critical user journeys (checkout, login, signup)
```

---

### 🔬 Unit & Integration Tools

| Tool | Status | Purpose |
|------|--------|---------|
| **Vitest** | ✅ | Unit tests — fast, Vite-native |
| **React Testing Library** | ✅ | Test components the way users use them |
| **MSW** | ⭐ | Intercept network requests — same handlers work in Vitest, Storybook, and the browser |
| **Jest** | 🔵 | Alternative to Vitest (older projects) |

```ts
export const handlers = [
  http.get('/api/users',   () => HttpResponse.json([{ id: '1', name: 'Alice' }])),
  http.post('/api/orders', () => HttpResponse.json({ id: 'ord_1' }, { status: 201 }))
]
```

---

### 🌐 E2E & Visual Tools

| Tool | Status | Purpose |
|------|--------|---------|
| **Playwright** | ✅ | E2E tests — real browser, most reliable |
| **Cypress** | 🔵 | E2E alternative — easier to start |
| **Storybook** | 🔵 | Develop and document components in isolation |
| **Storybook Interaction Tests** | ⭐ | Click / type / assert inside Storybook stories |
| **Chromatic** | 🔵 | Visual regression testing on top of Storybook |

---

### ♿ Quality & Contract Tools

| Tool | Status | Purpose |
|------|--------|---------|
| **axe-core / jest-axe** | ⭐ | Automated a11y testing — catches ~30% of issues, runs in CI |
| **Contract testing (Pact)** | ⭐ | Verify frontend/backend API contract — catches breaking changes before production |

```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom msw jest-axe
```

[↑ Back to top](#table-of-contents)

---

## Step 10 · CI/CD & Observability
> Getting code to production automatically and watching it once it's live

### In this step

**💡 Concepts** — environments, release cycle, and hotfix workflow
- The 3 environments (dev, staging, production)
- The release cycle — code freeze to live
- Hotfix workflow — step by step

**⚙️ CI/CD Tools** — automate testing, quality gates, and deployment
- GitHub Actions ✅
- Bundle budgets ⭐
- Feature flags ⭐

**🚀 Deployment Platforms** — where the app runs
- Vercel ✅
- AWS (S3 + CloudFront) 🔵
- Docker + Kubernetes ⭐
- GCP / Netlify 🔵

**📊 Observability Tools** — see what is happening in production
- Sentry ✅
- Source Maps ✅
- Datadog 🔵
- Splunk 🔵
- LogRocket 🔵
- PagerDuty 🔵
- Web Vitals ⭐
- OpenTelemetry ⭐

---

### 💡 Concepts

#### The 3 Environments

```
DEVELOPMENT         STAGING              PRODUCTION
localhost:5173      staging.yourapp.com  yourapp.com
Only you see it     Team + QA sees it    Real users
Safe to break       Last check           Real money
```

#### The Release Cycle

```
Code Freeze → QA on Staging → Release → Production Support on Watch
```

#### Hotfix Workflow

```
1. Sentry alert fires → PagerDuty wakes on-call dev
2. Read error in Sentry — exact file, line, release version
3. Check API logs in Datadog / Splunk at the same timestamp
4. Watch session replay in LogRocket — confirm exact steps to reproduce
5. git checkout -b hotfix/fix-name v2.4.1  ← branch from production tag, NOT main
6. Fix the specific bug
7. Fast PR → one senior reviewer → merge
8. GitHub Actions runs → tests pass → auto deploys
9. Sentry confirms error rate drops to zero
```

---

### ⚙️ CI/CD Tools

#### GitHub Actions ✅

```
git push
  ├── ESLint / Biome check
  ├── TypeScript check (tsc --noEmit)
  ├── Unit tests (vitest run)
  ├── E2E tests (playwright)
  ├── Bundle size check ⭐
  └── PASS → vite build → deploy
```

#### Feature Flags ⭐

Decouple deployment from release. Ship code dark, turn it on with a flag.

Tools: **LaunchDarkly**, **Unleash** (self-hosted), **Flagsmith**, **GrowthBook**

#### Bundle Budgets ⭐

Fail the build if bundle size exceeds a threshold — prevents silent performance regressions.

```bash
npm install -D bundlemon
```

---

### 🚀 Deployment Platforms

| Platform | Status | Use When |
|----------|--------|----------|
| **Vercel** | ✅ | Easiest — connect GitHub, auto deploy, preview URL per PR |
| **AWS** (S3 + CloudFront) | 🔵 | Enterprise — custom security, compliance |
| **Docker + Kubernetes** | ⭐ | Self-hosted enterprise, multi-region, custom infra |
| **GCP / Netlify** | 🔵 | Cloud-specific or Vercel alternative |

---

### 📊 Observability Tools

| Situation | Tool |
|-----------|------|
| JS error in production | **Sentry** — exact file, line, users affected |
| Error looks like gibberish | **Source maps** — makes minified errors readable |
| API calls failing or slow | **Datadog** — filter by endpoint and time range |
| Search logs by keyword | **Splunk** — enterprise log search |
| Cannot reproduce the bug | **LogRocket** — watch the user's exact session replay |
| Nobody responding at 2am | **PagerDuty** — calls the on-call dev's phone |
| Track real user page speed | **Web Vitals** ⭐ — LCP, INP, CLS from real users |
| Vendor-neutral tracing | **OpenTelemetry** ⭐ — instrument once, export anywhere |

[↑ Back to top](#table-of-contents)

---

## Step 11 · Security ⭐
> Protecting users and the app in production — missing from the original guide

### In this step

**💡 Concepts** — understand the attack vectors before adding tools
- XSS — how it happens and how to prevent it
- Token storage — where to put auth tokens safely
- CORS — what it is and where it is configured

**🛠️ Tools & Practices** — concrete things to add to every project
- DOMPurify for HTML sanitisation ✅
- Content Security Policy (CSP) ⭐
- httpOnly cookies for auth tokens ⭐
- npm audit / Snyk for dependency scanning ⭐

---

### 💡 Concepts

#### XSS — Cross-Site Scripting

React escapes JSX by default. The dangerous case is `dangerouslySetInnerHTML`.

```jsx
// ❌ Never do this with untrusted input
<div dangerouslySetInnerHTML={{ __html: userContent }} />

// ✅ Sanitise first if you must render HTML
import DOMPurify from 'dompurify'
<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(userContent) }} />
```

#### Token Storage

| Location | XSS risk | CSRF risk | Recommendation |
|----------|----------|-----------|----------------|
| `localStorage` | High — JS readable | None | ❌ Avoid for auth tokens |
| `sessionStorage` | High — JS readable | None | ❌ Avoid for auth tokens |
| `httpOnly` cookie | None — not JS readable | Medium | ✅ Preferred for auth tokens |
| Memory (JS variable) | Low — lost on refresh | None | ✅ For short-lived access tokens |

#### CORS

Configured on the **backend**, not the frontend. For local dev, proxy via Vite:

```ts
server: {
  proxy: { '/api': { target: 'http://localhost:3000', changeOrigin: true } }
}
```

---

### 🛠️ Tools & Practices

#### Content Security Policy (CSP) ⭐

Restricts which scripts and resources the browser can load. Prevents injected scripts from executing even if XSS occurs.

```
Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'
```

Configure on your CDN — Vercel headers, CloudFront response policies, or nginx.

#### Dependency Vulnerability Scanning ⭐

```bash
npm audit --audit-level=high    # add as a CI step
npx snyk test                   # more detailed, integrates with GitHub
```

[↑ Back to top](#table-of-contents)

---

## Step 12 · Accessibility (a11y) ⭐
> Building apps that work for everyone — missing from the original guide

### In this step

**💡 Concepts** — the a11y model and why it matters in enterprise
- Semantic HTML vs ARIA — when each applies
- Keyboard navigation rules
- Focus management for modals and dialogs
- WCAG 2.1 AA key requirements

**🛠️ Tools** — test and enforce accessibility automatically
- axe-core / jest-axe ⭐
- Radix UI / Ark UI — handle a11y primitives for you ✅

---

### 💡 Concepts

#### Semantic HTML First

```jsx
// ❌ Div soup — screen readers cannot interpret this
<div onClick={handleClick}>Submit</div>

// ✅ Semantic HTML — meaning is built in
<button type="submit">Submit</button>

// ARIA only when semantics are not enough
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
</div>
```

#### Keyboard Navigation

- All interactive elements must be reachable by Tab
- Tab order must follow visual order
- Use `tabIndex={0}` for custom interactive elements, `tabIndex={-1}` for programmatic focus only
- Never `outline: none` without a visible replacement

#### Focus Management

When a modal opens, move focus inside it. When it closes, return focus to the trigger.

```jsx
function Modal({ isOpen, onClose, children }) {
  const firstFocusable = useRef(null)
  useEffect(() => { if (isOpen) firstFocusable.current?.focus() }, [isOpen])
  return isOpen ? (
    <div role="dialog" aria-modal="true">
      <button ref={firstFocusable} onClick={onClose}>Close</button>
      {children}
    </div>
  ) : null
}
```

Radix UI and Ark UI handle focus trapping automatically in their dialog primitives.

#### WCAG 2.1 AA Key Requirements

- Colour contrast ≥ 4.5:1 for text, ≥ 3:1 for large text
- All functionality available from keyboard
- No content flashing more than 3 times per second
- Text resizable to 200% without loss of content
- Error messages must identify the field in error

---

### 🛠️ Tools

#### axe-core / jest-axe ⭐

```jsx
test('has no a11y violations', async () => {
  const { container } = render(<MyComponent />)
  expect(await axe(container)).toHaveNoViolations()
})
```

Automated tools catch ~30% of issues. Manual keyboard and screen reader testing is still required for full coverage.

[↑ Back to top](#table-of-contents)

---

## Step 13 · Internationalisation (i18n) ⭐
> Supporting multiple languages and locales — missing from the original guide

### In this step

**💡 Concepts** — i18n patterns to build in from day one
- RTL support — logical CSS properties ⭐
- Locale-aware formatting with the Intl API ⭐

**🛠️ Tools** — translation and localisation libraries
- i18next + react-i18next ✅
- react-intl (FormatJS) 🔵

---

### 💡 Concepts

#### RTL Support ⭐

Arabic, Hebrew, Urdu, and Persian read right-to-left. Use logical CSS properties so layouts flip automatically.

```jsx
<html dir={isRTL ? 'rtl' : 'ltr'} lang={lang}>

// ✅ Logical property — flips in RTL
<div className="ms-4">    // margin-inline-start

// ❌ Physical property — does not flip
<div className="ml-4">    // margin-left
```

#### Locale-aware Formatting — Intl API ⭐

Built into the browser — no library needed for numbers, dates, and relative time.

```js
new Intl.NumberFormat('en-IN', { style: 'currency', currency: 'INR' }).format(1234567)
// → ₹12,34,567.00

new Intl.RelativeTimeFormat('en', { numeric: 'auto' }).format(-1, 'day')
// → "yesterday"
```

---

### 🛠️ Tools

#### i18next + react-i18next ✅

```bash
npm install i18next react-i18next
```

```jsx
const { t } = useTranslation()
return <h1>{t('welcome')}</h1>
```

#### react-intl (FormatJS) 🔵

Good for apps that need heavy date / number / currency formatting alongside translations.

[↑ Back to top](#table-of-contents)

---

## Step 14 · Performance ⭐
> Making the app fast and keeping it fast — missing from the original guide

### In this step

**💡 Concepts** — performance patterns every React app needs
- Code splitting with React.lazy + Suspense
- Avoiding re-render cascades from context ⭐
- Image optimisation
- Web Vitals — the metrics that actually matter ⭐

**🛠️ Tools** — libraries that solve specific performance problems
- TanStack Virtual ⭐
- vite-plugin-pwa / Service Workers ⭐
- web-vitals ⭐
- Bundle analysis tools ⭐

---

### 💡 Concepts

#### Code Splitting

```jsx
const OrdersPage = lazy(() => import('./pages/OrdersPage'))
<Suspense fallback={<Spinner />}><OrdersPage /></Suspense>
```

Split at the route level first — each page becomes its own chunk loaded on demand.

#### Avoiding Re-render Cascades from Context ⭐

Context re-renders every consumer when its value changes. Split contexts by concern.

```jsx
// ❌ One fat context — everything re-renders when cart changes
<AppContext.Provider value={{ user, cart, theme }}>

// ✅ Split contexts — isolated re-renders per concern
<UserContext.Provider value={user}>
  <CartContext.Provider value={cart}>
    <ThemeContext.Provider value={theme}>
```

#### Image Optimisation

- `loading="lazy"` on all below-the-fold images
- Serve WebP / AVIF with `<picture>` fallbacks
- Specify `width` and `height` to prevent layout shift (CLS)
- Use Next.js `<Image>` if on Next.js — handles all of this automatically

#### Web Vitals ⭐

| Metric | Measures | Good threshold |
|--------|----------|----------------|
| **LCP** | How fast main content loads | < 2.5s |
| **INP** | Responsiveness to clicks/taps | < 200ms |
| **CLS** | Visual stability — no jumping content | < 0.1 |

---

### 🛠️ Tools

#### TanStack Virtual ⭐

Renders only the rows the user can see — essential for tables with thousands of rows.

```bash
npm install @tanstack/react-virtual
```

```jsx
const rowVirtualizer = useVirtualizer({
  count: 10000,
  getScrollElement: () => parentRef.current,
  estimateSize: () => 35,
})
```

#### Service Workers / PWA ⭐

```bash
npm install -D vite-plugin-pwa
```

Offline support, installable app, background sync, push notifications.

#### web-vitals ⭐

```bash
npm install web-vitals
```

```js
import { onLCP, onINP, onCLS } from 'web-vitals'
onLCP(console.log); onINP(console.log); onCLS(console.log)
```

#### Bundle Analysis ⭐

```bash
npx vite build --report
```

Run in CI. Set budgets and fail the build if exceeded.

[↑ Back to top](#table-of-contents)

---

## Step 15 · Auth & Identity ⭐
> Authentication and authorisation patterns — missing from the original guide

### In this step

**💡 Concepts** — auth patterns and their trade-offs
- Session vs JWT vs OAuth2 / PKCE — when to use which
- RBAC — Role-Based Access Control ⭐

**🛠️ Tools** — auth libraries and hosted services
- Auth.js (NextAuth) ⭐
- Clerk ⭐

---

### 💡 Concepts

#### Auth Patterns

| Approach | When to use |
|----------|-------------|
| **httpOnly cookie + session** | Traditional server-side auth — most secure, simple |
| **OAuth2 + PKCE** | SPA logging in via third-party IdP (Google, GitHub, Okta) |
| **JWT + refresh token rotation** | Stateless API auth — access token short-lived, refresh token in httpOnly cookie |

Never use the implicit OAuth2 flow in a SPA. Always use **PKCE** (Proof Key for Code Exchange).

#### RBAC — Role-Based Access Control ⭐

```jsx
function usePermission(permission) {
  const { user } = useAuth()
  return user?.permissions?.includes(permission) ?? false
}

const canDeleteOrders = usePermission('orders:delete')
{canDeleteOrders && <DeleteButton />}
```

---

### 🛠️ Tools

#### Auth.js (NextAuth) ⭐

Full-featured auth for Next.js — OAuth2, credentials, magic links, session management, database adapters.

```bash
npm install next-auth
```

#### Clerk ⭐

Hosted auth-as-a-service with drop-in React components. Handles MFA, social login, organisation management.

```bash
npm install @clerk/nextjs
```

[↑ Back to top](#table-of-contents)

---

## Step 16 · Documentation & DX ⭐
> Keeping the codebase understandable as it grows — missing from the original guide

### In this step

**💡 Concepts** — practices that pay off as the team and codebase grow
- Architecture Decision Records (ADRs) ⭐
- Component versioning strategy ⭐

**🛠️ Tools** — DX tooling for documentation and monorepos
- Storybook 🔵
- Turborepo ⭐
- Nx ⭐
- Changesets ⭐

---

### 💡 Concepts

#### Architecture Decision Records (ADRs) ⭐

Record significant technical decisions in a short markdown file so future team members understand the why.

```
docs/adr/
  001-state-management.md      ← why Zustand over Redux
  002-folder-structure.md      ← why FSD
  003-auth-approach.md         ← why Clerk
```

Format: **Context → Decision → Consequences**

#### Component Versioning Strategy ⭐

If you extract shared components into a package, use **Changesets** to manage versioning and changelogs. Publish to a private npm registry for internal libraries.

```bash
npm install -D @changesets/cli && npx changeset init
```

---

### 🛠️ Tools

#### Storybook 🔵

Stories serve as living documentation, a design-engineering collaboration layer, and a test harness via Interaction Tests.

```bash
npm create storybook@latest
```

```jsx
export const Primary:     Story = { args: { variant: 'primary',     label: 'Submit' } }
export const Destructive: Story = { args: { variant: 'destructive', label: 'Delete' } }
```

#### Turborepo ⭐

Simple pipeline caching for monorepos — easy to set up, great for small-to-medium codebases.

```bash
npx create-turbo@latest
```

#### Nx ⭐

Full monorepo platform — code generation, dependency graph, affected-only CI. Better for large enterprise codebases.

```bash
npx create-nx-workspace
```

[↑ Back to top](#table-of-contents)

---

## Quick Reference — Full Recommended Stack

```
Foundation         →  Vite + React + TypeScript
Code quality       →  ESLint + Prettier (or Biome ⭐) + Husky + Knip ⭐
Architecture       →  Feature-Sliced Design
Component patterns →  Smart/Dumb, Compound ⭐, Forward refs ⭐, Polymorphic ⭐
Rendering patterns →  Error boundaries ⭐, Suspense ⭐, Portals ⭐, Stable keys ⭐
Hook patterns      →  Custom hooks ⭐, useReducer ⭐, useTransition ⭐, useMemo/useCallback ⭐
Data flow          →  Colocation ⭐, Derived state ⭐, Optimistic updates ⭐
Code organisation  →  Barrel files ⭐, Public API pattern ⭐, Dependency inversion ⭐
Server state       →  TanStack Query
Client state       →  Zustand
Complex flows      →  XState ⭐
Local state        →  useState / useReducer ⭐
Global config      →  useContext (split by concern ⭐)
Routing            →  React Router v6 + protected routes ⭐ + code splitting ⭐
UI components      →  shadcn/ui + Radix UI
Styling            →  Tailwind CSS + CVA ⭐
Design tokens      →  Style Dictionary ⭐
Component memo     →  React.memo ⭐ (when needed)
Forms              →  React Hook Form + Zod ⭐
API fetcher        →  Axios + interceptor pattern ⭐
API validation     →  Zod ⭐
API codegen        →  OpenAPI codegen ⭐
Real-time          →  WebSockets / SSE ⭐
Auth               →  Clerk or Auth.js ⭐ + OAuth2/PKCE ⭐ + RBAC ⭐
Security           →  DOMPurify + CSP ⭐ + httpOnly cookies ⭐
a11y               →  Radix primitives + jest-axe ⭐ + WCAG 2.1 AA ⭐
i18n               →  i18next + Intl API ⭐
Performance        →  React.lazy + TanStack Virtual ⭐ + Web Vitals ⭐
Testing            →  Vitest + RTL + MSW ⭐ + Playwright + axe-core ⭐
CI/CD              →  GitHub Actions + bundle budgets ⭐
Deploy             →  Vercel → scale to AWS / Docker ⭐
Feature flags      →  LaunchDarkly or Unleash ⭐
Error tracking     →  Sentry + Source Maps
Performance mon    →  Datadog + Web Vitals ⭐
Observability      →  OpenTelemetry ⭐
Session replay     →  LogRocket
Alerting           →  PagerDuty
Docs               →  Storybook + ADRs ⭐
Monorepo           →  Turborepo or Nx ⭐
```

> ⭐ = Added to the original guide
> Redux is NOT in this stack. TanStack Query handles server data. Zustand handles shared UI state.

---

*Each layer depends on the one before it. The ⭐ additions are not optional extras — they are the difference between a codebase that scales and one that doesn't.*
