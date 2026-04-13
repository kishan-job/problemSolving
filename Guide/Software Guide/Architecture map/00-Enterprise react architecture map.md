# Enterprise React App — Architecture Map
> Complete 9-step guide from project setup to production support

---

## Step 1 · Project Foundation
> The build tool and framework that powers your app

| Tool | Use When |
|------|----------|
| **Vite** | New SPA (Single Page App) — fastest dev experience |
| **Next.js** | Need SSR (Server Side Rendering) or SSG (Static Site Generation) |
| **Remix** | Server-first data loading, progressive enhancement |
| ~~Create React App~~ | Avoid — unmaintained |

```bash
# Vite + React + TypeScript starter
npm create vite@latest my-app -- --template react-ts
```

---

## Step 2 · Language & Type Safety
> Tools that catch mistakes before they reach users

### TypeScript
- JavaScript with types — catches wrong prop types, missing fields at **compile time**
- Config: `tsconfig.json`
- Runs: in editor (red squiggles) + on build (`tsc`)

### ESLint
- Catches **code quality** issues — unused variables, broken React hooks rules
- Key plugins: `eslint-plugin-react-hooks`, `eslint-plugin-react-refresh`
- Config: `eslint.config.js`

### Prettier
- Auto-formats code on save — spacing, indentation, quotes, semicolons
- Everyone on the team gets identical-looking code
- Config: `.prettierrc`
- Add `eslint-config-prettier` to avoid ESLint conflicts

### Husky + lint-staged
- **Husky** — sets up a Git pre-commit hook (script runs before every commit)
- **lint-staged** — runs lint/format only on staged files (fast)
- Result: bad code is blocked before it enters the repo

```bash
# Install order
npm install -D eslint @eslint/js typescript-eslint eslint-plugin-react-hooks
npm install -D prettier eslint-config-prettier
npm install -D husky lint-staged && npx husky init
```

---

## Step 3 · Architecture Pattern
> How your codebase is organised and how components communicate

### Folder Structure — pick one

---

#### ✅ Feature-Sliced Design — Recommended for enterprise

Organise by **business domain**, not by file type. Everything related to a feature lives together.

```
src/app/
src/pages/
src/features/auth/
src/features/orders/
src/shared/ui/
src/entities/user/
```

What each folder contains:

```
src/
├── app/                    ← App-wide setup: router, global providers
│
├── pages/                  ← Route-level components (one per URL)
│     ├── HomePage.tsx
│     └── OrdersPage.tsx
│
├── features/               ← Business features — each fully self-contained
│     ├── auth/             ← Everything for login/logout
│     │     ├── ui/         ← Auth components (LoginForm etc.)
│     │     ├── api/        ← Auth API calls
│     │     ├── model/      ← Auth state, hooks
│     │     └── index.ts    ← Public API of this feature
│     └── orders/           ← Everything for orders
│           ├── ui/
│           ├── api/
│           └── model/
│
├── entities/               ← Business objects (pure data/model layer)
│     └── user/             ← User type, user API, user card component
│
└── shared/                 ← Truly reusable across entire app
      ├── ui/               ← Button, Input, Modal, Badge
      └── lib/              ← Utility functions, helpers
```

**Why FSD for enterprise:**
- New developer joins → they look in `features/orders/` and find everything for orders in one place
- Lower layers cannot import from higher layers → prevents spaghetti dependencies
- Teams can own separate features without stepping on each other

---

#### Atomic Design — UI-component focused

Best when building a design system. Organises by component complexity.

```
src/
├── atoms/        ← Smallest units: Button, Input, Badge, Icon
├── molecules/    ← Combinations: FormField (label + input + error)
├── organisms/    ← Complex sections: Header, ProductCard, Sidebar
├── templates/    ← Page layouts (no real data, just structure)
├── pages/        ← Route pages with real data
└── services/     ← API calls
```

---

#### Modular by type — simple default

Works for small apps, breaks down at large scale.

```
src/
├── components/   ← All UI components
├── pages/        ← Route pages
├── hooks/        ← Custom hooks
├── services/     ← API calls
├── store/        ← State management
└── utils/        ← Helper functions
```

---

### Component Communication — how state flows

#### 1. Smart / Dumb (Container / Presentational)

```
Smart component (Container)
  → Fetches data, holds state, passes down as props

        ↓ props

Dumb component (Presentational)
  → Only renders UI, no logic, fully reusable
```

```jsx
// Smart — knows about API
function UserListContainer() {
  const { data: users } = useQuery({ queryKey: ['users'], queryFn: fetchUsers })
  return <UserList users={users} />
}

// Dumb — only renders, easy to test
function UserList({ users }) {
  return users.map(u => <UserCard key={u.id} name={u.name} />)
}
```

---

#### 2. Prop drilling — avoid past 2 levels

```
Page (has the data)
  └── Section (doesn't need it, just passes down)
        └── Widget (doesn't need it, just passes down)
              └── Button (finally uses it)  ← BAD

Problem: middle components carry data they don't care about.
Every level must be touched when the data shape changes.
```

---

#### 3. Context + Provider — solves prop drilling

Wrap the tree once. Any child reads directly — no passing through layers.

```jsx
<UserContext.Provider value={user}>
  <Header />       {/* useContext(UserContext) — reads directly */}
  <Dashboard />    {/* useContext(UserContext) — reads directly */}
  <Sidebar />      {/* useContext(UserContext) — reads directly */}
</UserContext.Provider>
```

> ✅ Use for: theme, logged-in user, language — values that rarely change
> ❌ Avoid for: cart, modals, notifications — values that change often (re-renders everything)

---

#### 4. Children as prop — composition

Parent provides the shell. Caller decides what goes inside. No props needed.

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

<Layout>
  <Dashboard />   {/* Layout doesn't need to know what this is */}
</Layout>
```

---

## Step 4 · State Management
> Right tool for right kind of state — this is where most confusion happens

---

### The history — why everyone used Redux and why that changed

**Before 2020**, React had no good answer for "how do I fetch data and share it across the app."
So developers used Redux for **everything** — both API data and UI state.

To fetch a simple list of users with Redux, you had to write:

```
Step 1 — Action type constants
  FETCH_USERS_REQUEST
  FETCH_USERS_SUCCESS
  FETCH_USERS_FAILURE

Step 2 — A reducer with 3 cases
  case REQUEST  → loading: true
  case SUCCESS  → data: action.payload
  case FAILURE  → error: action.error

Step 3 — A thunk (async action creator)
  dispatch(FETCH_USERS_REQUEST)
  const data = await fetch('/api/users')
  dispatch(FETCH_USERS_SUCCESS, data)

Step 4 — Connect in component
  const users = useSelector(state => state.users.list)
  const dispatch = useDispatch()
  useEffect(() => { dispatch(fetchUsers()) }, [])
```

That is **4 files and ~60 lines** just to show a list on screen.

And even after all that, Redux gave you nothing special for server data:
- No caching — navigating back to a page refetches everything
- No background sync — if server data changed, your UI has no idea
- No deduplication — 2 components needing same data = 2 network requests
- You had to build all of this manually on top of Redux

**In 2020**, TanStack Query arrived with one key insight:

> "API data is fundamentally different from UI state.
>  It has a lifecycle — it goes stale, needs refreshing, can fail, should be cached.
>  Redux was never designed for this."

Everything developers were painfully building on top of Redux — caching, loading states,
error states, background refetching, deduplication — TanStack Query made automatic.
60 lines became 3.

**Redux Toolkit** (2019) fixed Redux's boilerplate with `createSlice`. But it could not fix
the fundamental issue — Redux is a UI state container, not a server cache. You still had to
write `createAsyncThunk` for every API call and manually track loading/error per slice.

**Result today:**
- TanStack Query took away the main reason people used Redux (storing API data)
- Zustand took away the second reason (shared UI state with less boilerplate)
- Redux is now only used in large existing codebases or for time-travel debugging

---

### The key split — only 3 kinds of state exist

| State type | Definition | Example | Tool |
|------------|-----------|---------|------|
| **Server state** | Comes from API/backend | users, products, orders | TanStack Query |
| **Shared UI state** | Shared, changes often, no API | modal, cart, tabs | Zustand |
| **Global config** | App-wide, rarely changes | theme, language, auth user | useContext |
| **Local state** | One component only | input value, toggle | useState |

---

### The Golden Rule

```
❌ WRONG — Never put API data into Redux or Zustand
   fetch('/api/users') → store in Zustand → now you have 2 sources of truth → bugs

✅ RIGHT — TanStack Query owns all server data
   TanStack Query already caches it. Storing it again causes sync issues.
```

---

### When to use which — plain English

**`useState`** — state that only one component needs. An input field, a local toggle.
Nobody else cares about it. No library needed.

**`useContext`** — values many components need but change rarely. Theme (dark/light), current
logged-in user, selected language. Wrap the app once in a Provider, any child reads it.
Downside: every consumer re-renders when the value changes — don't put frequently-updating
state here.

**`Zustand`** — UI state shared across multiple components that changes often. Shopping cart,
modal open/closed, sidebar collapsed. Zustand only re-renders the specific components that
subscribed to the changed piece of state — unlike Context which re-renders everything.

**`TanStack Query`** — anything that comes from a server. It is not "state management" — it is
a server cache. Give it a URL and a fetcher, it handles caching, loading, errors, refetching
automatically. Multiple components with same `queryKey` share one network request.

**`Redux Toolkit`** — only in existing codebases already using it, or if you specifically need
time-travel debugging. For any new project today, TanStack Query + Zustand + Context
replaces it completely.

---

### 4a · Server State Tools

#### TanStack Query ✅ Most used — replaces useEffect fetch

**The old way everyone wrote in every component:**
```jsx
const [users, setUsers] = useState([])
const [loading, setLoading] = useState(false)
const [error, setError] = useState(null)

useEffect(() => {
  setLoading(true)
  fetch('/api/users')
    .then(res => res.json())
    .then(data => { setUsers(data); setLoading(false) })
    .catch(err => { setError(err); setLoading(false) })
}, [])
// Problems: no caching, duplicate requests, same boilerplate everywhere
```

**The TanStack Query way:**
```jsx
const { data: users, isLoading, isError } = useQuery({
  queryKey: ['users'],
  queryFn: () => fetch('/api/users').then(r => r.json())
})
// isLoading, isError, data — all free. Caching, deduplication — free.
```

**How the cache works:**
- `queryKey: ['users']` is the cache identifier
- 10 components using same key = only 1 network request, all 10 share the result
- Navigate away and come back → cached data shown instantly, quiet background refetch

**For mutations (POST / PUT / DELETE):**
```jsx
const mutation = useMutation({
  mutationFn: (newUser) => fetch('/api/users', {
    method: 'POST',
    body: JSON.stringify(newUser)
  }),
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ['users'] }) // auto refetch list
  }
})
mutation.mutate({ name: 'John' })
```

**Setup:**
```bash
npm install @tanstack/react-query
```
```jsx
// main.tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
const queryClient = new QueryClient()

<QueryClientProvider client={queryClient}>
  <App />
</QueryClientProvider>
```

---

#### SWR — lighter REST alternative
```jsx
const { data, isLoading } = useSWR('/api/users', fetcher)
```
- SWR = Stale While Revalidate — shows old cached data instantly, fetches fresh in background
- Made by Vercel/Next.js team — integrates naturally with Next.js
- Simpler API, fewer features (no pagination, no infinite scroll, no offline)
- Good for: small/medium Next.js apps

---

#### Apollo Client — GraphQL only

GraphQL replaces many endpoints with one:
```
REST:     GET /api/users  +  GET /api/orders/1/items  +  GET /api/users/1/profile
GraphQL:  POST /graphql   — you describe exactly what you need in one request
```

```jsx
const { data } = useQuery(GET_USERS)  // GET_USERS is a gql query string
```
- Normalised cache — same entity in 10 queries = stored once, all stay in sync
- Only use if your backend team says "we have a GraphQL API"
- If your API is REST → Apollo is irrelevant, use TanStack Query

---

### 4b · Client State Tools

#### Zustand ✅ Recommended — replaces Redux for UI state

```jsx
import { create } from 'zustand'

// Define store once
const useCartStore = create((set) => ({
  items: [],
  addItem:   (item) => set((s) => ({ items: [...s.items, item] })),
  removeItem: (id)  => set((s) => ({ items: s.items.filter(i => i.id !== id) })),
  clearCart:  ()    => set({ items: [] })
}))

// Use in any component — no Provider needed
function CartIcon() {
  const items = useCartStore(state => state.items)
  return <span>{items.length}</span>
}

function ProductPage() {
  const addItem = useCartStore(state => state.addItem)
  return <button onClick={() => addItem(product)}>Add to cart</button>
}
```

- No Provider, no boilerplate — ~1kb bundle size
- Only re-renders components that use the changed slice (unlike Context)
- Use for: cart, modals, sidebar, multi-step forms, notifications

---

#### Jotai — atomic model
```jsx
const modalOpenAtom = atom(false)

function Button() {
  const [isOpen, setIsOpen] = useAtom(modalOpenAtom) // like useState but global
  return <button onClick={() => setIsOpen(true)}>Open</button>
}
```
- Each `atom()` is independent — components only re-render for their specific atom
- Good for: many small independent state pieces

---

#### Redux Toolkit — only for existing apps or special cases
```jsx
const usersSlice = createSlice({
  name: 'users',
  initialState: { list: [], loading: false },
  reducers: {
    setUsers: (state, action) => { state.list = action.payload }
  }
})
const users = useSelector(state => state.users.list)
const dispatch = useDispatch()
```

**Why it is skipped in new projects:**
- TanStack Query removed the main reason (storing API data)
- Zustand removed the second reason (shared UI state with less code)
- Still valid for: large existing codebases, time-travel debugging with Redux DevTools

---

### Decision flow

```
Is the data coming from an API or backend?
  YES → TanStack Query. Always. No exceptions.

  NO  → Is it shared across more than one component?

    YES → Does it change frequently? (cart, modal, notifications)
            YES → Zustand
            NO  → useContext  (theme, auth user, language)

    NO  → useState  (stays inside one component only)
```

---

### Real e-commerce example — all tools together

```
TanStack Query owns:          Zustand owns:             Context owns:
  fetch product list            cart items (local UI)     logged-in user
  fetch user orders             modal open / closed       dark/light theme
  POST add to cart API          sidebar collapsed         selected language
  search results                checkout step 1/2/3       feature flags
  notifications feed            active filter tabs
```

When user clicks "Add to Cart":
1. TanStack Query has product data cached → shown instantly, no loading
2. Zustand `addItem()` runs → cart badge updates immediately
3. Context provides current user → Header shows correct username
4. Three tools, zero overlap, no Redux anywhere

---

## Step 5 · Routing
> How users navigate between pages

| Tool | Use When |
|------|----------|
| **React Router v6** | Standard for Vite SPA |
| **TanStack Router** | Full TypeScript type-safety on route params |
| **Next.js App Router** | If using Next.js — built in |
| **Remix Router** | If using Remix — built in |

```bash
npm install react-router-dom
```

---

## Step 6 · UI Layer
> What components look like and how they are styled

### 6a · Component Libraries

| Library | When to use |
|---------|-------------|
| **shadcn/ui** ✅ | Copy-pasted into your project — you own it, fully customisable |
| **Radix UI** | Headless, unstyled, accessible — build your own design on top |
| **MUI (Material UI)** | Full featured, Google Material design |
| **Ant Design** | Full featured, popular in enterprise |

### 6b · Styling

| Tool | When to use |
|------|-------------|
| **Tailwind CSS** ✅ | Utility-first, pairs perfectly with shadcn/ui |
| **CSS Modules** | Scoped CSS per component, traditional approach |
| **Styled Components** | CSS-in-JS, component-scoped |
| **Emotion** | CSS-in-JS, used inside MUI |

> **Recommended combo: shadcn/ui + Tailwind CSS**

---

## Step 7 · API Communication
> How React talks to the backend

| Tool | When to use |
|------|-------------|
| **Axios** | REST — most common, interceptors, global error handling |
| **Fetch API** | Built-in browser, no install needed |
| **tRPC** | Full end-to-end TypeScript — no separate API schema needed |
| **Apollo Client** | GraphQL backend |
| **urql** | Lightweight GraphQL alternative to Apollo |

> TanStack Query **wraps** whichever fetcher you choose

---

## Step 8 · Testing
> Making sure the app works before it reaches users

### 8a · Unit & Integration

| Tool | Purpose |
|------|---------|
| **Vitest** ✅ | Unit tests — fast, Vite-native |
| **React Testing Library** | Test components the way users use them |
| **Jest** | Alternative to Vitest (older projects) |

```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom
```

### 8b · End-to-End & Visual

| Tool | Purpose |
|------|---------|
| **Playwright** ✅ | E2E tests — real browser, most reliable |
| **Cypress** | E2E alternative — easier to start |
| **Storybook** | Develop and document components in isolation |
| **Chromatic** | Visual regression testing on top of Storybook |

---

## Step 9 · CI/CD & Observability
> Getting code to production automatically, and watching it once it's live

---

### What "production" means — the 3 environments

Every app lives in 3 different places:

```
DEVELOPMENT              STAGING                  PRODUCTION
(your laptop)            (test server)            (the live app)

npm run dev              staging.yourapp.com      yourapp.com
localhost:5173

Only YOU see it          Only TEAM sees it        Real users use it
Safe to break            QA signs off here        Real money involved
Hot reload               Looks like production     Breaking it = users
Full error messages      Last check before         see broken screen
Large file (~5MB)        release                  Minified file (~200KB)
```

**What `vite build` does for production:**
- Minifies code — `userOrderData` becomes just `a`
- 5MB dev bundle → ~200KB production bundle
- Removes dev error messages — users never see stack traces
- This is why React DevTools cannot work in production — code is unreadable

---

### The release cycle — code freeze to live

```
Code Freeze
  ↓  No new features — only bug fixes from this point
  ↓  Gives QA time to test the exact code going live

QA / Staging
  ↓  Team tests on staging.yourapp.com
  ↓  QA signs off

Release to Production
  ↓  yourapp.com updated — real users on new version

Production Support goes ON WATCH
  ↓  Team watches dashboards: is the live app working?
  ↓  First 2-4 hours after release = highest risk window
```

---

### GitHub Actions — CI/CD pipeline

Triggered on every `git push` — everything runs automatically:

```
git push
  ↓
GitHub Actions wakes up
  ↓
  ├── ESLint check          (code quality)
  ├── TypeScript check      (tsc --noEmit)
  ├── Unit tests            (vitest run)
  └── E2E tests             (playwright)
       ↓
  FAIL → deploy blocked → team notified in Slack
  PASS → vite build runs → deployed to Vercel / AWS
```

Config: `.github/workflows/deploy.yml`

Humans forget to run tests. The robot never does. No bad code reaches production from a forgotten step.

---

### Deployment platforms

| Platform | Use When |
|----------|----------|
| **Vercel** ✅ | Easiest — connect GitHub, auto deploy, preview URL per PR, free tier |
| **AWS** (S3 + CloudFront) | Enterprise — custom security, compliance, existing AWS infra |
| **GCP** | Already on Google Cloud |
| **Netlify** | Vercel alternative |

---

### Observability — what production support actually uses

> ⚠️ React DevTools does NOT work in production — code is minified, names are gone.
> These tools below are what production support uses instead.

---

#### Sentry — Error tracking (first screen to check)

The moment a real user hits a JavaScript error, Sentry captures it instantly:

```
TypeError: Cannot read properties of undefined (reading 'userId')

File:           OrderSummary.tsx   ← readable because of source maps
Line:           42
Affected users: 1,243
First seen:     2 minutes ago
Release:        v2.4.1             ← which version introduced this
Browser:        Chrome 120
OS:             Windows 11
URL:            /checkout/summary
```

Without Sentry — you find out when users call support. That is too late.

**Source maps — why they matter:**

Production code is minified. Without source maps:
```
Error at: a.js line 1 col 4521   ← unreadable, useless
```

With source maps uploaded to Sentry:
```
Error at: OrderSummary.tsx line 42   ← readable, actionable
```

Vite generates source map files during `vite build`. You upload them to Sentry.
Now every minified production error becomes readable original TypeScript.

---

#### Datadog — Logs, API performance, dashboards

When an API call fails or something is slow, Sentry may not catch it — it is a network issue, not a JS crash.

Datadog collects:
- All browser console logs
- API response times — spikes show backend problems
- Error rates over time — is the error growing or stable?
- Custom dashboards — overall health at a glance
- Alerts when metrics cross thresholds

Production support opens Datadog: "show me all failed API calls in the last 30 minutes"
→ instantly sees every `/api/checkout` call that returned 500.

---

#### Splunk — Enterprise log search

Same role as Datadog for logs. Very common in banks, insurance, large enterprises.
Search millions of log lines instantly:

```
status=500 AND url=/api/orders AND timestamp>now-30m
```

Shows every order API failure in the last 30 minutes. Used alongside Sentry in large orgs.

---

#### LogRocket — Session replay

Records exactly what every user did — every click, scroll, network request — like a screen recording.

When user says "checkout broke for me":
1. Support opens LogRocket, finds that user's session
2. Presses play — watches exactly what they did before the crash
3. No more "I cannot reproduce it" — you watch exactly what happened

Most powerful tool for front-end bug reproduction.

---

#### PagerDuty — On-call alerting

If a P1 (critical) issue hits production at 2am:
- PagerDuty calls the on-call developer's phone
- Sends SMS
- Pings Slack
- Escalates to next person if unanswered in 5 minutes

Connected to Sentry and Datadog — fires automatically when error rate crosses threshold.

---

### The hotfix workflow — step by step

```
1.  Sentry alert fires → PagerDuty wakes on-call dev
         ↓
2.  Dev opens Sentry
    → reads exact error, file, line, which release (e.g. v2.4.1)
         ↓
3.  Dev opens Datadog / Splunk
    → checks API logs at same timestamp
    → is it a frontend bug or a backend API failure?
         ↓
4.  Dev opens LogRocket
    → watches session replay of affected user
    → confirms exact steps to reproduce
         ↓
5.  Dev creates hotfix branch FROM the production tag
    git checkout -b hotfix/fix-checkout-crash v2.4.1
    (NOT from main — main has unreleased features)
         ↓
6.  Fix the specific bug
         ↓
7.  Fast PR → quick review from one senior dev → merge
         ↓
8.  GitHub Actions runs → tests pass → auto deploys
         ↓
9.  Sentry confirms error rate drops to zero
    PagerDuty alert resolved
```

---

### Hotfix vs regular release

| | Regular Release | Hotfix |
|--|----------------|--------|
| Branch from | `main` or `develop` | production tag (e.g. `v2.4.1`) |
| QA cycle | Full — days or weeks | Fast — hours |
| Code freeze | Yes | Skips feature freeze |
| Who approves | Full team | 1-2 senior devs |
| When used | Planned sprint | Production is broken right now |

---

### Production support — tool cheatsheet

| Situation | Tool to open |
|-----------|-------------|
| App threw a JS error | **Sentry** — exact file, line, users affected |
| Error looks like gibberish | **Source maps** — makes minified errors readable |
| API calls failing or slow | **Datadog** — filter by endpoint and time |
| Search logs by keyword | **Splunk** — enterprise log search |
| Cannot reproduce the bug | **LogRocket** — watch the user's session replay |
| Nobody responding at 2am | **PagerDuty** — calls the on-call dev's phone |
| Need to deploy fix now | **GitHub Actions** — hotfix branch → PR → auto deploy |

---

## Quick Reference — Recommended Stack

```
Foundation     →  Vite + React + TypeScript
Code Quality   →  ESLint + Prettier + Husky + lint-staged
Architecture   →  Feature-Sliced Design
Server State   →  TanStack Query   (replaces Redux for API data)
Client State   →  Zustand          (replaces Redux for shared UI state)
Local State    →  useState
Global Config  →  useContext       (theme, auth user, language)
Routing        →  React Router v6
UI             →  shadcn/ui + Tailwind CSS
API Fetcher    →  Axios
Testing        →  Vitest + React Testing Library + Playwright
CI/CD          →  GitHub Actions
Deploy         →  Vercel → scale to AWS
Error tracking →  Sentry + Source Maps
Performance    →  Datadog
Log search     →  Splunk (enterprise)
Session replay →  LogRocket
Alerting       →  PagerDuty
```

> Redux is NOT in this stack.
> TanStack Query handles server data. Zustand handles shared UI state.
> Redux is only for existing codebases that already use it.

---

*Built step by step — each layer depends on the one before it.*