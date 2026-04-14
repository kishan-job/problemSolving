# React.js Interview Guide — Missing & Partial Categories
## Categories 21–36 · Complete Coverage with Code + Q&A

> These categories are **completely absent or only partially mentioned** in the original 18-category guide.
> All are asked regularly in senior and enterprise React interviews.
> Same format as the original guide: explanation · code · interview Q&A · common mistake · one-liner.

---

## Priority Table — What to Study and When

| Priority | Category | Status in original guide | Asked in | Study order |
|----------|----------|--------------------------|----------|-------------|
| 🔴 Critical | **Cat 21 · TypeScript with React** | ❌ Completely missing | Every enterprise interview | 1st |
| 🔴 Critical | **Cat 22 · Accessibility (a11y)** | ❌ Completely missing | Google, Microsoft, Swiggy, Flipkart | 2nd |
| 🔴 Critical | **Cat 23 · API Layer / Axios** | ❌ Completely missing | Every enterprise interview | 3rd |
| 🔴 Critical | **Cat 24 · Security (XSS/CSRF/CSP)** | ❌ Completely missing | Every senior interview | 4th |
| 🔴 Critical | **Cat 25 · Environment Variables** | ❌ Completely missing | Every Vite/React project interview | 5th |
| 🔴 Critical | **Cat 32 · SSR / Next.js** | ⚠️ 2 lines only in Cat 29 | Frontend-heavy, product companies | 6th |
| 🔴 Critical | **Cat 33 · Error Handling** | ⚠️ Error Boundary mentioned once | Every enterprise interview | 7th |
| 🟡 Important | **Cat 26 · i18n** | ❌ Completely missing | Product companies with global users | 8th |
| 🟡 Important | **Cat 27 · Animations** | ❌ Completely missing | UI-heavy product companies | 9th |
| 🟡 Important | **Cat 28 · Form Validation (Zod+RHF)** | ⚠️ RHF named, no explanation | Every form-heavy app interview | 10th |
| 🟡 Important | **Cat 34 · GraphQL** | ⚠️ Apollo named, no explanation | Companies using GraphQL backend | 11th |
| 🟡 Important | **Cat 35 · Bundle Optimization** | ⚠️ Code splitting only | Senior / performance interviews | 12th |
| 🟡 Important | **Cat 36 · Portals** | ⚠️ createPortal in machine coding only | Modal/tooltip architecture questions | 13th |
| 🟢 Good to know | **Cat 29 · SEO** | ❌ Completely missing | Public-facing app companies | 14th |
| 🟢 Good to know | **Cat 30 · Browser Storage** | ❌ Completely missing | Offline / PWA roles | 15th |
| 🟢 Good to know | **Cat 31 · Feature Flags** | ❌ Completely missing | Senior / platform roles | 16th |

---

## Gap Summary

```
Original guide:     18 categories · 230 topics
Authentication:      1 category added (Cat 19) ✅
WebSocket:           1 category added (Cat 20) ✅
This document:      16 categories added (Cat 21–36)

Completely missing:  19 topics → now covered (Cat 21-31 + Cat 34-36)
Partially mentioned:  5 topics → now fully covered (Cat 32, 33, 34, 35, 36)
```

---

# Table of Contents

- [Category 21: TypeScript with React](#category-21-typescript-with-react)
- [Category 22: Accessibility (a11y)](#category-22-accessibility-a11y)
- [Category 23: API Layer and Axios Setup](#category-23-api-layer-and-axios-setup)
- [Category 24: Security — XSS, CSRF, CSP](#category-24-security--xss-csrf-csp)
- [Category 25: Environment Variables](#category-25-environment-variables)
- [Category 26: Internationalization (i18n)](#category-26-internationalization-i18n)
- [Category 27: Animations](#category-27-animations)
- [Category 28: Form Validation — Zod + React Hook Form](#category-28-form-validation--zod--react-hook-form)
- [Category 29: SEO in React Apps](#category-29-seo-in-react-apps)
- [Category 30: Browser Storage APIs](#category-30-browser-storage-apis)
- [Category 31: Feature Flags](#category-31-feature-flags)
- [Category 32: SSR and Next.js](#category-32-ssr-and-nextjs)
- [Category 33: Error Handling](#category-33-error-handling)
- [Category 34: GraphQL in React](#category-34-graphql-in-react)
- [Category 35: Bundle Optimization](#category-35-bundle-optimization)
- [Category 36: Portals](#category-36-portals)
- [Quick Revision — All Categories One-Liners](#quick-revision--all-categories-one-liners)

---

# Category 21: TypeScript with React
> 🔴 Critical — asked in every enterprise interview · Study 1st

---

### 🔴 21.1 Typing component props

**Explanation:** Every component's props should have an explicit TypeScript interface. This gives you autocomplete, catches wrong prop types at compile time, and makes components self-documenting.

```tsx
// Define props with an interface
interface ButtonProps {
  label: string
  onClick: () => void
  variant?: 'primary' | 'secondary' | 'danger'  // optional union type
  disabled?: boolean
  children?: React.ReactNode
}

// Plain function — NOT React.FC (preferred)
function Button({ label, onClick, variant = 'primary', disabled = false }: ButtonProps) {
  return (
    <button onClick={onClick} disabled={disabled} className={`btn-${variant}`}>
      {label}
    </button>
  )
}

// TypeScript catches mistakes at compile time
<Button label="Save" onClick={() => save()} />             // ✅
<Button label="Save" onClick={() => save()} variant="danger" /> // ✅
<Button onClick={() => save()} />                          // ❌ label is required
<Button label="Save" onClick={() => save()} variant="xl" /> // ❌ xl not in union
```

**Interview Q: Should you use `React.FC` or plain function for components?**
**A:** Plain function is now preferred. `React.FC` was popular before but it implicitly adds `children` to props even when unwanted, and the extra wrapper makes types less clear. Just type the props directly: `function MyComp({ name }: { name: string })`.

**Common Mistake:** Using `any` for props you are unsure about. Use `unknown` and narrow the type instead. `any` disables all TypeScript checks on that value.

**One-liner:** Type props with an interface. Plain function > React.FC. Never use `any` — use `unknown` and narrow.

---

### 🔴 21.2 Typing useState

**Explanation:** TypeScript infers state type from the initial value. But when initial value is `null` or an empty array, you must provide the type explicitly with a generic.

```tsx
// TypeScript infers these automatically — no generic needed
const [count, setCount] = useState(0)        // number
const [name, setName] = useState('')         // string
const [isOpen, setIsOpen] = useState(false)  // boolean

// Initial value is null — TypeScript cannot infer what it will become
// Must use generic to tell TypeScript the future shape
interface User {
  id: string
  name: string
  email: string
  role: 'admin' | 'manager' | 'user'
}

const [user, setUser] = useState<User | null>(null)
// After login: setUser({ id:'1', name:'Alice', email:'...', role:'admin' }) ✅

// Initial value is empty array — must type explicitly
const [items, setItems] = useState<string[]>([])
const [users, setUsers] = useState<User[]>([])

// TypeScript narrows after null check
if (user) {
  console.log(user.name)  // ✅ TypeScript knows user is User here (not null)
}
```

**Interview Q: Why do you need a generic on useState for null initial values?**
**A:** TypeScript infers `useState(null)` as `null` type permanently — it doesn't know what the state will eventually become. Without `useState<User | null>(null)`, trying to set the state to a User object later is a type error. The generic tells TypeScript "this will be User or null."

**Common Mistake:** Writing `useState<User>(null)` — errors because `null` is not assignable to `User`. Always write `useState<User | null>(null)`.

**One-liner:** TypeScript infers from initial value. For null or empty array use generic: `useState<User | null>(null)`, `useState<string[]>([])`.

---

### 🔴 21.3 Typing event handlers

**Explanation:** React events have specific TypeScript types. These are the ones you must know by memory.

```tsx
function Form() {
  // Input change — most common
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value)  // string
  }

  // Textarea
  const handleTextarea = (e: React.ChangeEvent<HTMLTextAreaElement>) => {
    console.log(e.target.value)
  }

  // Select dropdown
  const handleSelect = (e: React.ChangeEvent<HTMLSelectElement>) => {
    console.log(e.target.value)
  }

  // Form submit — always needs preventDefault
  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault()
    // handle submit
  }

  // Button click
  const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
    console.log(e.currentTarget)
  }

  // Keyboard
  const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
    if (e.key === 'Enter') submit()
  }

  return (
    <form onSubmit={handleSubmit}>
      <input onChange={handleChange} onKeyDown={handleKeyDown} />
      <button onClick={handleClick}>Submit</button>
    </form>
  )
}
```

**Interview Q: What is the difference between `React.MouseEvent` and `MouseEvent`?**
**A:** `React.MouseEvent` is React's synthetic event wrapper — it's what you receive in React JSX event handlers. `MouseEvent` (no React prefix) is the native browser event used in `addEventListener`. Use `React.MouseEvent` in JSX props.

**Common Mistake:** Using `any` for event types because you don't remember the exact type. `React.ChangeEvent<HTMLInputElement>` for inputs and `React.FormEvent<HTMLFormElement>` for form submit covers 90% of cases.

**One-liner:** Input: `React.ChangeEvent<HTMLInputElement>`. Form: `React.FormEvent<HTMLFormElement>`. Click: `React.MouseEvent<HTMLButtonElement>`. Keyboard: `React.KeyboardEvent<HTMLInputElement>`.

---

### 🔴 21.4 Typing useRef

**Explanation:** `useRef` is used two ways — DOM reference and storing a mutable value. Each needs a different generic.

```tsx
// DOM ref — type is the HTML element, initial value is null
// React fills current after component mounts
const inputRef = useRef<HTMLInputElement>(null)
const divRef = useRef<HTMLDivElement>(null)
const buttonRef = useRef<HTMLButtonElement>(null)

function FocusInput() {
  const inputRef = useRef<HTMLInputElement>(null)

  const handleFocus = () => {
    inputRef.current?.focus()  // ?. required — current could be null before mount
  }

  return <input ref={inputRef} />
}

// Mutable value ref — NOT null, stores an actual value
// No null check needed since you initialise with real value
const intervalRef = useRef<ReturnType<typeof setInterval> | null>(null)
const countRef = useRef<number>(0)
const prevValueRef = useRef<string>('')
```

**Interview Q: Why is the initial value `null` for DOM refs but not for value refs?**
**A:** DOM refs start as null because the DOM element doesn't exist until the component mounts — React fills `current` after mounting. For value refs storing an interval ID or counter, you initialise with the actual value so TypeScript doesn't force null checks on every use.

**One-liner:** DOM ref: `useRef<HTMLInputElement>(null)`. Value ref: `useRef<number>(0)`. Use `?.` when accessing DOM ref `.current`.

---

### 🔴 21.5 Typing useContext

**Explanation:** Context needs a TypeScript type so all consumers get proper autocomplete and type safety.

```tsx
interface ThemeContextType {
  theme: 'light' | 'dark'
  toggleTheme: () => void
}

// createContext needs a default value — use null and handle in hook
const ThemeContext = createContext<ThemeContextType | null>(null)

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setTheme] = useState<'light' | 'dark'>('light')
  const toggleTheme = () => setTheme(prev => prev === 'light' ? 'dark' : 'light')

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}

// Custom hook — handles the null check once, all consumers are clean
export function useTheme(): ThemeContextType {
  const ctx = useContext(ThemeContext)
  if (!ctx) throw new Error('useTheme must be inside ThemeProvider')
  return ctx  // TypeScript now knows ctx is ThemeContextType (not null)
}

// Consumer — fully typed, no null checks needed
function Header() {
  const { theme, toggleTheme } = useTheme()  // theme is 'light' | 'dark'
  return <button onClick={toggleTheme}>Mode: {theme}</button>
}
```

**One-liner:** Type `createContext` with your interface. Use `null` as default. Custom hook throws if null — consumers always get clean non-null types.

---

### 🔴 21.6 Typing custom hooks with generics

**Explanation:** Generic hooks work with any data type — define the type once, reuse for any response shape.

```tsx
interface UseFetchResult<T> {
  data: T | null
  isLoading: boolean
  error: string | null
  refetch: () => void
}

function useFetch<T>(url: string): UseFetchResult<T> {
  const [data, setData] = useState<T | null>(null)
  const [isLoading, setIsLoading] = useState(true)
  const [error, setError] = useState<string | null>(null)

  const fetchData = useCallback(async () => {
    setIsLoading(true)
    setError(null)
    try {
      const res = await fetch(url)
      if (!res.ok) throw new Error(`HTTP ${res.status}`)
      const json: T = await res.json()
      setData(json)
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Unknown error')
    } finally {
      setIsLoading(false)
    }
  }, [url])

  useEffect(() => { fetchData() }, [fetchData])
  return { data, isLoading, error, refetch: fetchData }
}

// Usage — TypeScript knows the exact shape of data
interface User { id: number; name: string; email: string }

function UserPage() {
  const { data, isLoading } = useFetch<User[]>('/api/users')
  // data is User[] | null — TypeScript knows every field
  return isLoading ? <p>Loading...</p> : (
    <ul>{data?.map(u => <li key={u.id}>{u.name}</li>)}</ul>
  )
}
```

**One-liner:** Generic hooks: `function useFetch<T>(url)` — works for any data shape. Return type interface documents the hook. TypeScript infers from generic at call site.

---

### 🟡 21.7 TypeScript utility types in React

**Explanation:** Built-in TypeScript utility types are commonly used to derive new types from existing ones without duplication.

```tsx
interface User {
  id: string
  name: string
  email: string
  role: 'admin' | 'user'
  createdAt: string
}

// Partial — all properties optional (update payloads)
type UserUpdate = Partial<User>
// { id?: string, name?: string, email?: string, ... }

// Pick — keep only specific properties
type UserCard = Pick<User, 'id' | 'name'>
// { id: string, name: string }

// Omit — remove specific properties (form types)
type CreateUserForm = Omit<User, 'id' | 'createdAt'>
// { name: string, email: string, role: 'admin' | 'user' }

// Record — key-value type
type RolePermissions = Record<User['role'], string[]>
// { admin: string[], user: string[] }

// ReturnType — extract function return type
const useAuth = () => ({ user: null as User | null, login: async () => {} })
type AuthState = ReturnType<typeof useAuth>
```

**Interview Q: When do you use Partial vs Omit?**
**A:** `Partial<T>` makes all fields optional — for PATCH/update payloads where only changed fields are sent. `Omit<T, K>` removes specific fields entirely — for form types that exclude server-generated fields like `id` or `createdAt`.

**One-liner:** `Partial` = all optional. `Pick` = keep only these. `Omit` = remove these. `Record<K,V>` = key-value map. Never duplicate interfaces — derive from existing ones.

---

# Category 22: Accessibility (a11y)
> 🔴 Critical — asked heavily at Google, Microsoft, Swiggy, Flipkart · Study 2nd

---

### 🔴 22.1 ARIA roles and attributes

**Explanation:** ARIA (Accessible Rich Internet Applications) attributes describe UI element purpose and state to screen readers. Always prefer semantic HTML first — it has ARIA built in.

```tsx
// ✅ Semantic HTML — implicit roles, best choice
<button onClick={save}>Save</button>      // role="button" implicit
<nav>...</nav>                             // role="navigation" implicit
<main>...</main>                           // role="main" implicit
<header>...</header>                       // role="banner" implicit

// ✅ ARIA for custom interactive widgets
function CustomDropdown({ isOpen, onToggle, label, children }) {
  return (
    <div>
      <button
        aria-haspopup="listbox"        // announces: opens a listbox
        aria-expanded={isOpen}         // announces: is it open or closed
        aria-label={label}
        onClick={onToggle}
      >
        {label}
      </button>
      {isOpen && (
        <ul role="listbox" aria-label={`${label} options`}>
          {children}
        </ul>
      )}
    </div>
  )
}

// aria-label vs aria-labelledby
<button aria-label="Close dialog">×</button>     // text provided directly

<h2 id="dialog-title">Confirm Delete</h2>
<dialog aria-labelledby="dialog-title">...</dialog>  // points to existing element

// aria-describedby — additional description
<input aria-describedby="email-hint" />
<p id="email-hint">We will never share your email</p>
```

**Interview Q: When should you use ARIA roles?**
**A:** Only when native HTML elements cannot convey the semantics — custom dropdowns, modals, tabs, or sliders built from divs. The first rule of ARIA: "don't use ARIA if a native HTML element works." `<button>` is always better than `<div role="button">` because button has keyboard focus, click events, and correct semantics built in.

**Common Mistake:** Adding ARIA to elements that already have implicit roles. `<button role="button">` is redundant.

**One-liner:** Semantic HTML first. ARIA only for custom widgets. `aria-label` = text. `aria-expanded` = state. `aria-describedby` = description. Never add ARIA to already-semantic elements.

---

### 🔴 22.2 Keyboard navigation

**Explanation:** Every interactive element must be reachable and operable by keyboard only. Tab navigates between elements. Enter/Space activates. Arrow keys navigate within components.

```tsx
function AccessibleTabs({ tabs }: { tabs: Tab[] }) {
  const [activeIndex, setActiveIndex] = useState(0)

  const handleKeyDown = (e: React.KeyboardEvent, index: number) => {
    if (e.key === 'ArrowRight') setActiveIndex((index + 1) % tabs.length)
    if (e.key === 'ArrowLeft') setActiveIndex((index - 1 + tabs.length) % tabs.length)
    if (e.key === 'Home') setActiveIndex(0)
    if (e.key === 'End') setActiveIndex(tabs.length - 1)
  }

  return (
    <div>
      <div role="tablist">
        {tabs.map((tab, i) => (
          <button
            key={tab.id}
            role="tab"
            aria-selected={i === activeIndex}
            aria-controls={`panel-${tab.id}`}
            tabIndex={i === activeIndex ? 0 : -1}  // only active tab in tab order
            onClick={() => setActiveIndex(i)}
            onKeyDown={(e) => handleKeyDown(e, i)}
          >
            {tab.label}
          </button>
        ))}
      </div>
      {tabs.map((tab, i) => (
        <div
          key={tab.id}
          id={`panel-${tab.id}`}
          role="tabpanel"
          hidden={i !== activeIndex}
        >
          {tab.content}
        </div>
      ))}
    </div>
  )
}
```

**tabIndex rules:**
```
tabIndex={0}   → in natural tab order
tabIndex={-1}  → focusable programmatically but NOT in tab order
tabIndex={1+}  → avoid — creates unpredictable tab order
```

**Interview Q: How do you make a custom component keyboard accessible?**
**A:** Three steps: (1) ensure it can receive focus — use a native focusable element or add `tabIndex={0}`, (2) handle keyboard events — Enter/Space to activate, Escape to close, Arrow keys inside, (3) manage focus — when modal opens move focus inside, when it closes return focus to the trigger element.

**Common Mistake:** Building interactive elements with `<div onClick>` and no `tabIndex` or `onKeyDown`. Keyboard users cannot reach or activate divs without these.

**One-liner:** Tab = navigate. Enter/Space = activate. Arrow keys = within component. `tabIndex={0}` = in tab order. `tabIndex={-1}` = programmatic focus only. Never interactive div without these.

---

### 🔴 22.3 Focus management

**Explanation:** When UI changes significantly — modal opens, route changes — focus must move programmatically so keyboard users know something changed.

```tsx
function Modal({ isOpen, onClose, children }: ModalProps) {
  const modalRef = useRef<HTMLDivElement>(null)
  const previousFocusRef = useRef<HTMLElement | null>(null)

  useEffect(() => {
    if (isOpen) {
      // Save where focus was
      previousFocusRef.current = document.activeElement as HTMLElement
      // Move focus into modal
      modalRef.current?.focus()
    } else {
      // Return focus when modal closes
      previousFocusRef.current?.focus()
    }
  }, [isOpen])

  if (!isOpen) return null

  return (
    <div
      ref={modalRef}
      role="dialog"
      aria-modal="true"
      aria-labelledby="modal-title"
      tabIndex={-1}              // makes div programmatically focusable
      onKeyDown={(e) => { if (e.key === 'Escape') onClose() }}
      style={{ outline: 'none' }}
    >
      <h2 id="modal-title">Confirm</h2>
      {children}
    </div>
  )
}
```

**One-liner:** Modal opens → focus inside. Modal closes → focus returns to trigger. Route changes → focus to page heading. `tabIndex={-1}` makes non-interactive elements programmatically focusable.

---

### 🟡 22.4 Accessible forms

```tsx
function AccessibleForm() {
  const [errors, setErrors] = useState<Record<string, string>>({})

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="email">
          Email address
          <span aria-hidden="true"> *</span>  {/* visual asterisk, hidden from SR */}
        </label>
        <input
          id="email"
          type="email"
          required
          aria-required="true"
          aria-invalid={!!errors.email}
          aria-describedby={errors.email ? 'email-error' : 'email-hint'}
        />
        <p id="email-hint" style={{ fontSize: 12 }}>We'll never share your email</p>
        {errors.email && (
          <p id="email-error" role="alert" style={{ color: 'red' }}>
            {errors.email}
          </p>
        )}
      </div>

      {/* Group related radio/checkbox with fieldset + legend */}
      <fieldset>
        <legend>Preferred contact method</legend>
        <label><input type="radio" name="contact" value="email" /> Email</label>
        <label><input type="radio" name="contact" value="phone" /> Phone</label>
      </fieldset>
    </form>
  )
}
```

**One-liner:** Every input needs `<label htmlFor>`. `aria-invalid` + `aria-describedby` for errors. `role="alert"` announces error messages to screen readers immediately. `fieldset+legend` for groups.

---

# Category 23: API Layer and Axios Setup
> 🔴 Critical — asked in every enterprise interview · Study 3rd

---

### 🔴 23.1 Creating a centralised Axios instance

**Explanation:** Instead of calling Axios directly everywhere, create one configured instance with `baseURL`, headers, and timeout. All API calls go through it.

```ts
// src/shared/lib/apiClient.ts
import axios from 'axios'

export const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_URL,   // e.g. https://api.myapp.com
  timeout: 10000,                           // 10 second timeout
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
  },
})
```

**Why one instance:**
```
Without: fetch('/api/users')  ← no base URL
         fetch('https://api.example.com/api/orders', { headers: {...} })  ← hardcoded + repeated

With:    apiClient.get('/users')    ← baseURL auto-prepended
         apiClient.get('/orders')   ← headers auto-added
         Change baseURL in ONE place → works everywhere
```

**One-liner:** `axios.create({ baseURL, timeout, headers })` — one configured instance. Change URL once. All calls automatically get the base config.

---

### 🔴 23.2 Request and response interceptors

```ts
// ── REQUEST INTERCEPTOR — attach auth token automatically
apiClient.interceptors.request.use((config) => {
  const token = getAccessToken()
  if (token) config.headers.Authorization = `Bearer ${token}`
  return config
})

// ── RESPONSE INTERCEPTOR — global error handling
apiClient.interceptors.response.use(
  (response) => response,  // success — pass through

  async (error) => {
    const status = error.response?.status

    if (status === 401) {
      // Token expired → silent refresh (see Category 19)
      // or redirect to login
    }
    if (status === 403) {
      toast.error('You do not have permission for this action')
    }
    if (status === 429) {
      toast.error('Too many requests — please slow down')
    }
    if (status >= 500) {
      toast.error('Server error — our team has been notified')
      // reportToSentry(error)
    }
    if (!error.response) {
      toast.error('No internet connection')
    }

    return Promise.reject(error)
  }
)
```

**One-liner:** Request interceptor = auto-attach token. Response interceptor = handle 401/403/500 globally once. Components never write boilerplate error handling for common HTTP errors.

---

### 🔴 23.3 Typed API service functions

```ts
// src/features/users/api/userService.ts
import { apiClient } from '@/shared/lib/apiClient'

interface User {
  id: string; name: string; email: string; role: 'admin' | 'user'
}
type CreateUserPayload = Omit<User, 'id'>

export async function getUsers(): Promise<User[]> {
  const { data } = await apiClient.get<User[]>('/users')
  return data
}
export async function getUserById(id: string): Promise<User> {
  const { data } = await apiClient.get<User>(`/users/${id}`)
  return data
}
export async function createUser(payload: CreateUserPayload): Promise<User> {
  const { data } = await apiClient.post<User>('/users', payload)
  return data
}
export async function updateUser(id: string, payload: Partial<CreateUserPayload>): Promise<User> {
  const { data } = await apiClient.put<User>(`/users/${id}`, payload)
  return data
}
export async function deleteUser(id: string): Promise<void> {
  await apiClient.delete(`/users/${id}`)
}
```

**One-liner:** One service file per feature. Typed functions. Components call `getUsers()` — never `apiClient.get('/users')` directly. Clear separation: service = HTTP layer, component = UI layer.

---

### 🟡 23.4 Error type narrowing

```ts
import axios, { AxiosError } from 'axios'

interface ApiError {
  message: string
  code: string
  details?: Record<string, string[]>  // validation errors per field
}

function isAxiosError(error: unknown): error is AxiosError<ApiError> {
  return axios.isAxiosError(error)
}

// In TanStack Query mutation
const mutation = useMutation({
  mutationFn: createUser,
  onError: (error) => {
    if (isAxiosError(error)) {
      const status = error.response?.status
      if (status === 422) {
        // Validation errors — show per-field
        setFormErrors(error.response?.data?.details ?? {})
      } else if (status === 409) {
        toast.error('Email already exists')
      }
      // 401/403/500 handled globally by interceptor
    }
  }
})
```

**One-liner:** `axios.isAxiosError(error)` narrows unknown to AxiosError. 422 = validation (show per-field). 409 = conflict. 401/403/500 handled by interceptor globally.

---

# Category 24: Security — XSS, CSRF, CSP
> 🔴 Critical — asked in every senior enterprise interview · Study 4th

---

### 🔴 24.1 XSS — Cross-Site Scripting

**Explanation:** XSS is when an attacker injects malicious JavaScript into your page. React escapes all JSX values by default — this is the primary built-in protection.

```tsx
// ✅ React escapes automatically — safe
const userInput = '<script>alert("xss")</script>'
return <p>{userInput}</p>
// Renders as text: <script>... is displayed, NOT executed

// ❌ dangerouslySetInnerHTML bypasses React's escaping
return <div dangerouslySetInnerHTML={{ __html: userInput }} />
// This EXECUTES the script — never use with unsanitized user input

// ✅ If you must render HTML (e.g. rich text editor) — sanitize with DOMPurify first
import DOMPurify from 'dompurify'

function RichContent({ html }: { html: string }) {
  const clean = DOMPurify.sanitize(html)  // strips all scripts and event handlers
  return <div dangerouslySetInnerHTML={{ __html: clean }} />
}

// ❌ XSS via href — javascript: protocol
const url = 'javascript:alert("xss")'
return <a href={url}>Click me</a>  // executes JS on click

// ✅ Validate URLs before using in href
function SafeLink({ url, children }: { url: string; children: React.ReactNode }) {
  const isSafe = url.startsWith('https://') || url.startsWith('/')
  return isSafe ? <a href={url}>{children}</a> : <span>{children}</span>
}
```

**Interview Q: How does React protect against XSS by default?**
**A:** React escapes all values rendered in JSX — `<script>` becomes `&lt;script&gt;` displayed as text, never executed. The only way to inject raw HTML is `dangerouslySetInnerHTML`, which is deliberately named to warn developers. Always sanitize with DOMPurify before using it with any user-generated content.

**Common Mistake:** Using `dangerouslySetInnerHTML` with database content without sanitizing. Even content from your own DB should be sanitized if it was originally user-generated.

**One-liner:** React auto-escapes JSX. `dangerouslySetInnerHTML` bypasses it — always sanitize with DOMPurify. Never `href="javascript:"` — validate URLs. `localStorage` token + XSS = stolen token.

---

### 🔴 24.2 CSRF — Cross-Site Request Forgery

**Explanation:** CSRF tricks an authenticated user's browser into making an unwanted request. The browser sends cookies automatically — attackers exploit this.

```
CSRF attack:
  1. User logs into bank.com (session cookie set)
  2. User visits evil.com
  3. evil.com has hidden form: <form action="https://bank.com/transfer" method="POST">
  4. Form auto-submits → browser sends bank.com cookie automatically
  5. Bank processes the transfer as if user initiated it

Protections:
  SameSite=Strict on cookies → browser only sends cookie for same-origin requests
  CSRF token → server issues secret token, every mutation must include it
  Custom headers → browsers don't send custom headers cross-origin
```

```ts
// CSRF token pattern — server sets readable cookie, frontend sends as header
function getCsrfToken(): string {
  return document.cookie
    .split('; ')
    .find(row => row.startsWith('csrf='))
    ?.split('=')[1] ?? ''
}

// Add to all mutating requests in Axios interceptor
apiClient.interceptors.request.use((config) => {
  const method = config.method?.toUpperCase()
  if (['POST', 'PUT', 'PATCH', 'DELETE'].includes(method ?? '')) {
    config.headers['X-CSRF-Token'] = getCsrfToken()
  }
  return config
})
```

**One-liner:** CSRF = malicious site uses your cookies to make requests. Fix: `SameSite=Strict` cookie flag, CSRF token header on mutations, or custom request headers.

---

### 🟡 24.3 Content Security Policy (CSP)

**Explanation:** CSP is an HTTP header whitelisting which sources can load scripts, styles, and other resources. It prevents XSS by blocking inline scripts and unauthorised external scripts.

```
// Set by server/nginx:
Content-Security-Policy:
  default-src 'self';
  script-src 'self' cdn.myapp.com;
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: https:;
  connect-src 'self' api.myapp.com;
  frame-ancestors 'none';

// In Next.js next.config.js:
async headers() {
  return [{
    source: '/(.*)',
    headers: [
      { key: 'X-Frame-Options', value: 'DENY' },
      { key: 'X-Content-Type-Options', value: 'nosniff' },
      { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
    ]
  }]
}
```

**One-liner:** CSP = HTTP header whitelisting allowed resource sources. Blocks XSS and clickjacking. Set in nginx/server config. React production builds are CSP-compatible (no `eval()`).

---

### 🟡 24.4 Never put secrets in the frontend

```ts
// ❌ NEVER in frontend code
const API_SECRET = 'sk-prod-abc123'   // secret API key
const JWT_SECRET = 'my-jwt-secret'    // signing key

// ✅ Public keys are fine
const STRIPE_PUBLIC_KEY = 'pk_live_abc'  // public key — designed to be exposed

// ✅ VITE_ vars are in the bundle — only non-secrets
VITE_API_URL=https://api.example.com        // fine
VITE_STRIPE_PUBLIC_KEY=pk_live_abc          // fine — it's a public key

// Sensitive operations MUST go through your backend
// Frontend → Your Backend → Third Party API  (secret key on backend only)
```

**One-liner:** Frontend code is public — anyone can read your JS bundle. Never put secrets, private keys, or credentials in frontend code or VITE_ env vars.

---

# Category 25: Environment Variables
> 🔴 Critical — asked in every Vite/React project interview · Study 5th

---

### 🔴 25.1 How .env files work in Vite

**Explanation:** Vite uses `.env` files to inject values at build time. Only `VITE_` prefixed variables are bundled into your app — everything else is for build scripts only.

```bash
# .env — shared defaults (lowest priority)
VITE_APP_NAME=MyApp

# .env.development — only when npm run dev
VITE_API_URL=http://localhost:3000
VITE_DEBUG=true

# .env.production — only when npm run build
VITE_API_URL=https://api.myapp.com
VITE_DEBUG=false

# .env.local — NEVER committed to git (overrides everything)
VITE_API_URL=http://localhost:4000   # your personal dev override
```

**Priority order (highest → lowest):**
```
.env.local
.env.[mode].local   (e.g. .env.development.local)
.env.[mode]         (e.g. .env.production)
.env
```

```ts
// Access with import.meta.env — NOT process.env in Vite
const apiUrl = import.meta.env.VITE_API_URL
const isDev = import.meta.env.DEV   // built-in boolean
const isProd = import.meta.env.PROD // built-in boolean
const mode = import.meta.env.MODE   // 'development' | 'production'

// TypeScript — add types in vite-env.d.ts
interface ImportMetaEnv {
  readonly VITE_API_URL: string
  readonly VITE_APP_NAME: string
  readonly VITE_STRIPE_PUBLIC_KEY: string
}
interface ImportMeta {
  readonly env: ImportMetaEnv
}

// Validate required vars on startup — fail fast with clear message
function requireEnv(key: keyof ImportMetaEnv): string {
  const value = import.meta.env[key]
  if (!value) throw new Error(`Missing required env var: ${key}`)
  return value
}
export const env = {
  apiUrl: requireEnv('VITE_API_URL'),
}
```

**Interview Q: What happens if you forget the `VITE_` prefix?**
**A:** The variable is not injected into the browser bundle. `import.meta.env.MY_VAR` returns `undefined` at runtime. Non-prefixed vars are accessible in `vite.config.ts` via Node's `process.env` during the build process — but not in your app code.

**Common Mistake:** Using `process.env.REACT_APP_URL` (Create React App syntax) in a Vite project. Vite uses `import.meta.env.VITE_URL`.

**One-liner:** `VITE_` prefix = in browser bundle. No prefix = build scripts only. `import.meta.env.VITE_X`. `.env.local` is gitignored personal override. Never commit secrets.

---

### 🟡 25.2 Multiple environments

```bash
# .env.staging
VITE_API_URL=https://staging-api.myapp.com
VITE_LOG_LEVEL=warn
```

```json
// package.json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "build:staging": "vite build --mode staging"
  }
}
```

**One-liner:** `--mode staging` uses `.env.staging`. Different API URLs per environment. Never commit `.env.local` — add to `.gitignore`.

# Category 26: Internationalization (i18n)
> 🟡 Important — product companies with global users · Study 8th

---

### 🔴 26.1 What is i18n and why it matters

**Explanation:** i18n prepares your app to work in multiple languages and locales — translations, date/number formatting, pluralization, RTL text direction.

```tsx
// ❌ Without i18n — strings hardcoded in components
function Greeting({ name }: { name: string }) {
  return <p>Hello, {name}! You have 3 new messages.</p>
}

// ✅ With i18n — strings in external JSON files
// en.json: { "greeting": "Hello, {{name}}!", "messages_one": "{{count}} message", "messages_other": "{{count}} messages" }
// hi.json: { "greeting": "नमस्ते, {{name}}!", ... }

function Greeting({ name, count }: { name: string; count: number }) {
  const { t } = useTranslation()
  return <p>{t('greeting', { name })} {t('messages', { count })}</p>
}
```

---

### 🟡 26.2 react-i18next setup

```ts
// src/shared/lib/i18n.ts
import i18n from 'i18next'
import { initReactI18next } from 'react-i18next'
import LanguageDetector from 'i18next-browser-languagedetector'
import en from './locales/en.json'
import hi from './locales/hi.json'

i18n
  .use(LanguageDetector)   // detects browser language
  .use(initReactI18next)
  .init({
    resources: {
      en: { translation: en },
      hi: { translation: hi },
    },
    fallbackLng: 'en',
    interpolation: { escapeValue: false },  // React already escapes
  })
```

```json
// locales/en.json
{
  "nav": { "home": "Home", "orders": "Orders" },
  "orders": {
    "title": "Your orders",
    "count_one": "{{count}} order",
    "count_other": "{{count}} orders"
  }
}
```

```tsx
// Using in components
function OrdersPage() {
  const { t, i18n } = useTranslation()
  return (
    <div>
      <h1>{t('orders.title')}</h1>
      <p>{t('orders.count', { count: 5 })}</p>  {/* auto-pluralizes */}
      <select value={i18n.language} onChange={e => i18n.changeLanguage(e.target.value)}>
        <option value="en">English</option>
        <option value="hi">हिंदी</option>
      </select>
    </div>
  )
}
```

**One-liner:** `useTranslation()` → `t('key')`. JSON files per language. Auto-pluralization with `_one`/`_other` suffixes. `i18n.changeLanguage()` to switch at runtime.

---

### 🟡 26.3 RTL support

```tsx
function App() {
  const { i18n } = useTranslation()
  const isRTL = ['ar', 'he', 'ur', 'fa'].includes(i18n.language)

  useEffect(() => {
    document.documentElement.dir = isRTL ? 'rtl' : 'ltr'
    document.documentElement.lang = i18n.language
  }, [i18n.language, isRTL])

  return <RouterProvider router={router} />
}
```

```css
/* ❌ Physical properties — don't flip in RTL */
.card { margin-left: 16px; }

/* ✅ Logical properties — flip automatically in RTL */
.card { margin-inline-start: 16px; }
/* margin-inline-start = left in LTR, right in RTL */
```

**One-liner:** RTL: `document.dir = 'rtl'`. Use CSS logical properties (`margin-inline-start` not `margin-left`). Tailwind has `rtl:` modifier.

---

# Category 27: Animations
> 🟡 Important — UI-heavy product companies · Study 9th

---

### 🔴 27.1 CSS transitions in React

**Explanation:** Simplest animations — CSS transitions triggered by state changes. No library needed.

```tsx
function Toast({ message, isVisible }: ToastProps) {
  return (
    <div style={{
      opacity: isVisible ? 1 : 0,
      transform: isVisible ? 'translateY(0)' : 'translateY(-20px)',
      transition: 'opacity 300ms ease, transform 300ms ease',
    }}>
      {message}
    </div>
  )
}

// Class toggle approach
function Accordion({ isOpen, children }: AccordionProps) {
  return <div className={`accordion ${isOpen ? 'accordion--open' : ''}`}>{children}</div>
}
// CSS: .accordion { max-height: 0; overflow: hidden; transition: max-height 300ms; }
// CSS: .accordion--open { max-height: 500px; }
```

**One-liner:** CSS transitions = simplest approach. Toggle class or inline style on state change. No library needed for simple fade/slide/show-hide.

---

### 🟡 27.2 Framer Motion

```tsx
import { motion, AnimatePresence } from 'framer-motion'

// Animate on mount
function Card() {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
    >
      Content
    </motion.div>
  )
}

// AnimatePresence — animate components when they unmount (exit animation)
function NotificationList({ notifications }: NotificationListProps) {
  return (
    <AnimatePresence>
      {notifications.map(n => (
        <motion.div
          key={n.id}
          initial={{ opacity: 0, x: 50 }}
          animate={{ opacity: 1, x: 0 }}
          exit={{ opacity: 0, x: -50 }}    // plays when removed from list
          transition={{ duration: 0.2 }}
        >
          {n.message}
        </motion.div>
      ))}
    </AnimatePresence>
  )
}
```

**Interview Q: When do you use Framer Motion vs CSS transitions?**
**A:** CSS transitions for simple state-based changes (hover, show/hide, color). Framer Motion for mount/unmount transitions (`AnimatePresence`), drag interactions, staggered list animations, physics-based spring animations, or layout animations between route changes.

**One-liner:** Framer Motion: `<motion.div initial animate exit>`. `AnimatePresence` handles unmount animations (exit). CSS for simple cases. Framer Motion for complex enter/exit/drag.

---

# Category 28: Form Validation — Zod + React Hook Form
> 🟡 Important — every form-heavy app interview · Study 10th

---

### 🔴 28.1 React Hook Form basics

**Explanation:** RHF uses uncontrolled inputs under the hood — no re-render on every keystroke — making it significantly more performant than controlled forms for complex forms.

```tsx
import { useForm } from 'react-hook-form'

interface LoginFormValues {
  email: string
  password: string
}

function LoginForm() {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
    reset,
  } = useForm<LoginFormValues>()

  const onSubmit = async (data: LoginFormValues) => {
    // data is typed — only called when validation passes
    await login(data.email, data.password)
    reset()
  }

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        {...register('email', {
          required: 'Email is required',
          pattern: { value: /^\S+@\S+$/i, message: 'Invalid email' }
        })}
        type="email"
      />
      {errors.email && <p>{errors.email.message}</p>}

      <input
        {...register('password', {
          required: 'Password is required',
          minLength: { value: 8, message: 'At least 8 characters' }
        })}
        type="password"
      />
      {errors.password && <p>{errors.password.message}</p>}

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Logging in...' : 'Login'}
      </button>
    </form>
  )
}
```

**One-liner:** RHF: `register` connects inputs. `handleSubmit` validates before calling your fn. `formState.errors` for display. Uncontrolled = performant, no re-render per keystroke.

---

### 🔴 28.2 Zod schema validation with React Hook Form

```tsx
import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import { z } from 'zod'

// Schema = TypeScript type + validation rules in ONE place
const registerSchema = z.object({
  name: z.string().min(2, 'At least 2 characters').max(50),
  email: z.string().email('Invalid email'),
  password: z.string()
    .min(8, 'At least 8 characters')
    .regex(/[A-Z]/, 'Must contain uppercase')
    .regex(/[0-9]/, 'Must contain a number'),
  confirmPassword: z.string(),
  age: z.number().min(18, 'Must be 18 or older'),
}).refine(
  (data) => data.password === data.confirmPassword,
  { message: 'Passwords do not match', path: ['confirmPassword'] }
)

// z.infer automatically generates TypeScript type — no duplication
type RegisterFormValues = z.infer<typeof registerSchema>

function RegisterForm() {
  const { register, handleSubmit, formState: { errors } } = useForm<RegisterFormValues>({
    resolver: zodResolver(registerSchema)  // Zod validates on submit
  })

  return (
    <form onSubmit={handleSubmit(data => registerUser(data))}>
      <input {...register('name')} />
      {errors.name && <p>{errors.name.message}</p>}
      <input {...register('email')} type="email" />
      {errors.email && <p>{errors.email.message}</p>}
      <input {...register('password')} type="password" />
      {errors.password && <p>{errors.password.message}</p>}
      <input {...register('confirmPassword')} type="password" />
      {errors.confirmPassword && <p>{errors.confirmPassword.message}</p>}
      <button type="submit">Register</button>
    </form>
  )
}
```

**Interview Q: Why use Zod with React Hook Form instead of RHF's built-in validation?**
**A:** Three reasons. First, the schema is both the TypeScript type and the validation rules — no duplication. Second, `z.infer<typeof schema>` generates the type automatically. Third, Zod supports complex cross-field validation (passwords match) more cleanly. And the same Zod schema can be shared with your backend for identical validation.

**One-liner:** Zod schema = TypeScript type + validation in one. `z.infer` = auto type. `zodResolver` connects to RHF. Share schema between frontend and backend for identical rules.

---

# Category 29: SEO in React Apps
> 🟢 Good to know — public-facing app companies · Study 14th

---

### 🔴 29.1 The SPA SEO problem

```
Default Vite SPA:
  HTML served: <div id="root"></div>  ← empty shell
  Search crawler sees: empty page
  JavaScript runs later → content appears → too late for some crawlers

Solutions ranked by effectiveness:
  1. SSR (Next.js)  → full HTML per request, always fresh
  2. SSG            → pre-built HTML, fastest load
  3. ISR            → SSG + scheduled regeneration
  4. Prerendering   → build-time HTML for static routes
  5. Dynamic render → detect crawlers, serve pre-rendered HTML
```

**One-liner:** SPA = empty HTML shell → bad SEO by default. Fix with SSR (Next.js), SSG, or prerendering. Googlebot runs JS but with delays and limits.

---

### 🟡 29.2 Meta tags and Open Graph

```tsx
// react-helmet-async for Vite SPA
import { Helmet } from 'react-helmet-async'

function ProductPage({ product }: { product: Product }) {
  return (
    <>
      <Helmet>
        <title>{product.name} | MyShop</title>
        <meta name="description" content={product.description.slice(0, 160)} />
        <meta property="og:title" content={product.name} />
        <meta property="og:description" content={product.description} />
        <meta property="og:image" content={product.imageUrl} />
        <meta property="og:url" content={`https://myshop.com/products/${product.id}`} />
        <link rel="canonical" href={`https://myshop.com/products/${product.id}`} />
      </Helmet>
      <ProductDetail product={product} />
    </>
  )
}

// Next.js App Router — built-in metadata API
export const metadata = {
  title: 'Product Name | MyShop',
  description: 'Product description',
  openGraph: { title: 'Product Name', images: ['/product.jpg'] },
}
```

**One-liner:** react-helmet-async for SPAs. Next.js has built-in `metadata` export. Set unique title + description + og: tags per page. Canonical URL prevents duplicate content penalty.

---

# Category 30: Browser Storage APIs
> 🟢 Good to know · Study 15th

---

### 🔴 30.1 localStorage vs sessionStorage vs cookies

| | localStorage | sessionStorage | Cookie |
|--|-------------|----------------|--------|
| Capacity | ~5MB | ~5MB | ~4KB |
| Persists | Until cleared | Until tab closes | Until expiry |
| JS readable | ✅ Yes | ✅ Yes | ✅ (unless httpOnly) |
| Sent with requests | ❌ No | ❌ No | ✅ Yes |
| Use for | User preferences | Temp wizard state | Auth (httpOnly) |

```tsx
// useLocalStorage custom hook — React state + localStorage sync
function useLocalStorage<T>(key: string, initialValue: T) {
  const [value, setValue] = useState<T>(() => {
    try {
      const stored = localStorage.getItem(key)
      return stored ? JSON.parse(stored) : initialValue
    } catch { return initialValue }
  })

  const setStoredValue = useCallback((newValue: T | ((val: T) => T)) => {
    setValue(prev => {
      const toStore = newValue instanceof Function ? newValue(prev) : newValue
      try { localStorage.setItem(key, JSON.stringify(toStore)) }
      catch (e) { console.error('localStorage error:', e) }
      return toStore
    })
  }, [key])

  return [value, setStoredValue] as const
}

// Usage
const [theme, setTheme] = useLocalStorage<'light' | 'dark'>('theme', 'light')
```

**One-liner:** localStorage = persistent. sessionStorage = tab only. httpOnly cookie = sent with requests, JS can't read. Never store auth tokens in localStorage — XSS steals them.

---

# Category 31: Feature Flags
> 🟢 Good to know — senior/platform roles · Study 16th

---

### 🔴 31.1 Feature flags — what and why

```
Without flags:  Feature complete → deploy → all users see it
                Bug found → rollback deployment → downtime

With flags:     Feature behind disabled flag → deploy → nobody sees it
                Enable for 1% of users → monitor → enable for 10% → 100%
                Bug found → toggle flag off → instant, no deployment needed
```

---

### 🟡 31.2 Feature flag implementation

```tsx
// Simple context-based feature flags
const FeatureFlagContext = createContext<Record<string, boolean>>({})

export function FeatureFlagProvider({ children }: { children: React.ReactNode }) {
  const [flags, setFlags] = useState({
    newCheckoutFlow: false,
    aiRecommendations: false,
    betaDashboard: false,
  })

  useEffect(() => {
    // Fetch from LaunchDarkly, Flagsmith, or your own API
    fetch('/api/feature-flags')
      .then(r => r.json())
      .then(setFlags)
  }, [])

  return (
    <FeatureFlagContext.Provider value={flags}>
      {children}
    </FeatureFlagContext.Provider>
  )
}

export function useFeatureFlag(flag: string): boolean {
  return useContext(FeatureFlagContext)[flag] ?? false
}

// Feature component for clean conditional rendering
function Feature({ flag, children, fallback = null }: {
  flag: string
  children: React.ReactNode
  fallback?: React.ReactNode
}) {
  return useFeatureFlag(flag) ? <>{children}</> : <>{fallback}</>
}

// Usage
function CheckoutPage() {
  return (
    <Feature flag="newCheckoutFlow" fallback={<OldCheckout />}>
      <NewCheckout />
    </Feature>
  )
}
```

**Interview Q: How do feature flags enable safer deployments?**
**A:** Code ships behind a disabled flag — zero user impact. The flag is then enabled for a small percentage of users to monitor. If issues appear, the flag is toggled off instantly — no rollback deployment needed. This separates code deployment from feature release.

**One-liner:** Feature flags = toggle features without deployment. Ship behind flag → gradual rollout → instant rollback. Use LaunchDarkly, Flagsmith, or build simple with Context.

# Category 32: SSR and Next.js
> 🔴 Critical — partially mentioned (2 lines in Cat 29) → now fully covered · Study 6th

---

### 🔴 32.1 CSR vs SSR vs SSG vs ISR — the four rendering strategies

**Explanation:** These strategies differ in WHEN and WHERE HTML is generated. Understanding the trade-offs is a standard senior interview question.

```
CSR — Client Side Rendering (default Vite/SPA)
  WHERE: Browser
  WHEN:  After JS downloads and runs
  HTML:  <div id="root"></div>  ← empty shell
  ✅ Good for: dashboards behind login, highly interactive apps
  ❌ Bad for:  SEO, public pages, slow devices

SSR — Server Side Rendering
  WHERE: Server (every request)
  WHEN:  At request time
  HTML:  Full HTML with real content
  ✅ Good for: SEO pages with dynamic data (product pages, news)
  ❌ Bad for:  Server load at scale, slow server = slow page

SSG — Static Site Generation
  WHERE: Build time
  WHEN:  Once at build, never again until next deploy
  HTML:  Pre-built static files served from CDN
  ✅ Good for: blogs, docs, marketing pages
  ❌ Bad for:  Frequently changing data (real-time prices)

ISR — Incremental Static Regeneration (Next.js)
  WHERE: Build time + background server regeneration
  WHEN:  At build, then re-generated after time window expires
  HTML:  Cached static, updated periodically (e.g. every hour)
  ✅ Good for: e-commerce (prices updated hourly is acceptable)
  ✅ Best of: SSG speed + SSR freshness
```

**Timeline comparison:**
```
CSR:  Request → empty HTML → JS downloads → React runs → content visible
SSR:  Request → server renders → full HTML → JS downloads → hydrate → interactive
SSG:  (build) → Request → CDN returns HTML → JS downloads → hydrate
ISR:  (build+schedule) → Request → CDN HTML (may be stale) → background regen
```

**Interview Q: When would you choose SSR over SSG?**
**A:** SSG when content changes rarely and can rebuild on deploy (blog posts, marketing). SSR when content changes per-request or per-user (personalised pages, real-time prices, user-specific dashboards). ISR is the middle ground — build once, regenerate on a schedule, serve cached to all users between regenerations.

**Common Mistake:** Using SSR for everything. SSG served from CDN is significantly faster (no server round-trip). Only use SSR when you need per-request dynamic content.

**One-liner:** CSR = browser renders. SSR = server per request. SSG = built at build time. ISR = SSG + scheduled regen. SEO-sensitive pages need SSR or SSG.

---

### 🔴 32.2 Hydration — what it is and why it matters

**Explanation:** Hydration is React attaching event listeners to server-rendered HTML. Before hydration the page looks correct but is not interactive.

```
SSR flow:
  1. Server renders full HTML (React runs on Node.js server)
  2. Browser receives HTML → user sees content immediately ← FCP is fast
  3. Browser downloads JS bundle
  4. React runs in browser → attaches event handlers to existing HTML
     ← this is HYDRATION
  5. Page becomes interactive ← TTI

The hydration gap (step 2 → step 4):
  User SEES the page but clicking DOES NOTHING
  Frustrating if JS bundle is large (slow devices / slow network)
```

```tsx
// Next.js App Router — Server Component (no hydration, zero JS shipped)
// All components are Server Components by default
async function ProductList() {
  const products = await db.getProducts()  // direct DB — server only
  return (
    <ul>{products.map(p => <li key={p.id}>{p.name}</li>)}</ul>
  )
  // ZERO JavaScript shipped to browser for this component
}

// Client Component — needs interactivity
'use client'  // marks as client component — will hydrate

function AddToCartButton({ productId }: { productId: string }) {
  const [added, setAdded] = useState(false)  // useState = needs browser JS
  return (
    <button onClick={() => setAdded(true)}>
      {added ? 'Added!' : 'Add to Cart'}
    </button>
  )
}

// Mix server + client
async function ProductPage({ id }: { id: string }) {
  const product = await db.getProduct(id)  // server-side
  return (
    <div>
      <ProductDetails product={product} />   {/* server — no JS */}
      <AddToCartButton productId={id} />     {/* client — interactive */}
    </div>
  )
}
```

**Interview Q: What is a hydration mismatch and how do you fix it?**
**A:** A mismatch happens when the HTML rendered on the server differs from what React renders on the client. Common causes: `new Date()` (different server vs client time), `Math.random()`, accessing `window` or `localStorage` during render (server doesn't have these). Fix: put browser-only code in `useEffect`, or check `typeof window !== 'undefined'`.

**Common Mistake:** Using `window`, `localStorage`, or `document` directly in a component body — this crashes on the server. Always wrap in `useEffect` or use Next.js client components.

**One-liner:** Hydration = React attaches events to server HTML. Mismatch = server ≠ client render → error. Never use window/localStorage outside useEffect in SSR apps.

---

### 🔴 32.3 Next.js App Router key concepts

```
app/ folder conventions:
  layout.tsx     ← shared UI for segment + children (persists across routes)
  page.tsx       ← unique UI for a route (required to be publicly accessible)
  loading.tsx    ← auto loading UI while page.tsx loads (Suspense boundary)
  error.tsx      ← auto error boundary for the segment (must be 'use client')
  not-found.tsx  ← 404 UI for the segment
  route.ts       ← API route handler (replaces pages/api/)
```

```tsx
// app/products/[id]/page.tsx
interface PageProps { params: { id: string } }

export default async function ProductPage({ params }: PageProps) {
  const product = await fetch(`${process.env.API_URL}/products/${params.id}`, {
    next: { revalidate: 3600 }  // ISR — revalidate every hour
  }).then(r => r.json())

  return <ProductDetail product={product} />
}

// Dynamic metadata
export async function generateMetadata({ params }: PageProps) {
  const product = await getProduct(params.id)
  return {
    title: `${product.name} | MyShop`,
    description: product.description,
  }
}

// app/products/[id]/loading.tsx
export default function Loading() { return <ProductSkeleton /> }

// app/products/[id]/error.tsx — must be client component
'use client'
export default function Error({ error, reset }: { error: Error; reset: () => void }) {
  return (
    <div>
      <p>Failed to load: {error.message}</p>
      <button onClick={reset}>Try again</button>
    </div>
  )
}
```

**Interview Q: What is the difference between Server and Client Components?**
**A:** Server Components run only on the server — can access databases directly, zero JS shipped to browser, cannot use hooks or browser APIs. Client Components run in browser — support useState, useEffect, event handlers. Mark with `'use client'` at top. Rule: push interactivity to the leaves, keep data fetching in server components.

**One-liner:** App Router: server components by default (no hooks, zero JS). `'use client'` = browser (hooks, events). `loading.tsx` = auto Suspense. `error.tsx` = auto Error Boundary.

---

### 🟡 32.4 Data fetching patterns

```tsx
// Parallel fetching — avoid waterfall
async function Dashboard() {
  // ❌ Sequential waterfall
  const users = await fetchUsers()
  const orders = await fetchOrders()  // waits for users to finish first

  // ✅ Parallel — both start at the same time
  const [users, orders] = await Promise.all([fetchUsers(), fetchOrders()])
  return <DashboardView users={users} orders={orders} />
}

// Streaming with Suspense — show parts as they load
export default function Page() {
  return (
    <div>
      <h1>Dashboard</h1>                  {/* renders immediately */}
      <Suspense fallback={<Spinner />}>
        <SlowComponent />                 {/* streams in when ready */}
      </Suspense>
      <Suspense fallback={<Spinner />}>
        <AnotherSlowComponent />          {/* streams independently */}
      </Suspense>
    </div>
  )
}
```

**One-liner:** Use `Promise.all` for parallel fetches — avoid waterfall. `Suspense` streaming = show parts of page as each loads. Nest Suspense boundaries for independent loading states.

---

# Category 33: Error Handling
> 🔴 Critical — partially covered (Error Boundary mentioned once) → now fully covered · Study 7th

---

### 🔴 33.1 The four error handling layers

```
Layer 1: Error Boundaries
  Catches: render errors, lifecycle errors in child components
  Does NOT catch: event handlers, async code, server errors

Layer 2: Event handler try/catch
  Catches: errors inside onClick, onChange, onSubmit handlers
  Why separate: Error Boundaries never catch event handler errors

Layer 3: API error handling (TanStack Query onError + Axios interceptors)
  Global interceptors: 401, 403, 500 handled once for whole app
  Component level: specific messages for specific status codes

Layer 4: Global window error listeners
  Catches: anything that slipped through all other layers
  Last resort: log to Sentry, show fallback
```

---

### 🔴 33.2 Error Boundaries — full implementation

```tsx
// src/shared/ui/ErrorBoundary.tsx
import React from 'react'

interface Props {
  children: React.ReactNode
  fallback?: React.ReactNode
  onError?: (error: Error, info: React.ErrorInfo) => void
}
interface State { hasError: boolean; error: Error | null }

export class ErrorBoundary extends React.Component<Props, State> {
  state: State = { hasError: false, error: null }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error }
  }

  componentDidCatch(error: Error, info: React.ErrorInfo) {
    console.error('Boundary caught:', error)
    this.props.onError?.(error, info)
    // Sentry.captureException(error, { extra: info })
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback ?? (
        <div style={{ padding: 24, textAlign: 'center' }}>
          <h2>Something went wrong</h2>
          <p>{this.state.error?.message}</p>
          <button onClick={() => this.setState({ hasError: false, error: null })}>
            Try again
          </button>
        </div>
      )
    }
    return this.props.children
  }
}

// Wrap sections independently — granular recovery
function App() {
  return (
    <ErrorBoundary fallback={<p>App crashed — please refresh</p>}>
      <ErrorBoundary fallback={<p>Header failed</p>}>
        <Header />
      </ErrorBoundary>
      <ErrorBoundary fallback={<p>Dashboard failed — try refreshing</p>}>
        <Dashboard />
      </ErrorBoundary>
      <Footer />
    </ErrorBoundary>
  )
}
```

**Interview Q: What errors do Error Boundaries NOT catch?**
**A:** They only catch render-time and lifecycle errors in children. They do NOT catch: errors in event handlers (use try/catch), async errors (setTimeout, promises), errors in the boundary itself, and SSR errors. For async/event errors use try/catch and `setState` to show error UI.

**Common Mistake:** Expecting Error Boundary to catch errors inside `onClick` or `useEffect`. It won't. Use try/catch in those places.

**One-liner:** Error Boundary catches render errors → shows fallback. Does NOT catch event handlers or async. Must be a class component. Wrap each feature section separately for granular recovery.

---

### 🔴 33.3 API error handling with TanStack Query

```tsx
// Global in Axios interceptor
apiClient.interceptors.response.use(
  response => response,
  async error => {
    const status = error.response?.status
    if (status === 403) toast.error('Permission denied')
    if (status >= 500) toast.error('Server error — team notified')
    if (!error.response) toast.error('No internet connection')
    return Promise.reject(error)
  }
)

// Component-level for specific cases
function DeleteButton({ userId }: { userId: string }) {
  const mutation = useMutation({
    mutationFn: () => deleteUser(userId),
    onSuccess: () => {
      toast.success('User deleted')
      queryClient.invalidateQueries({ queryKey: ['users'] })
    },
    onError: (error) => {
      if (isAxiosError(error) && error.response?.status === 409) {
        toast.error('Cannot delete — user has active orders')
      }
      // 500 already handled globally by interceptor
    }
  })

  return (
    <button onClick={() => mutation.mutate()} disabled={mutation.isPending}>
      {mutation.isPending ? 'Deleting...' : 'Delete'}
    </button>
  )
}
```

**One-liner:** Interceptor = global 401/403/500 handling once. `onError` in mutation = component-specific cases. `isAxiosError()` narrows the error type for safe status checking.

---

### 🟡 33.4 useEffect error handling + Global handlers

```tsx
// useEffect — must use try/catch inside
function DataComponent() {
  const [data, setData] = useState(null)
  const [error, setError] = useState<string | null>(null)

  useEffect(() => {
    let cancelled = false
    async function load() {
      try {
        const result = await fetchData()
        if (!cancelled) setData(result)
      } catch (err) {
        if (!cancelled) setError(err instanceof Error ? err.message : 'Failed')
      }
    }
    load()
    return () => { cancelled = true }  // prevent state update after unmount
  }, [])

  if (error) return <p>Error: {error}</p>
  return <div>{data}</div>
}

// src/main.tsx — global last resort handlers
window.addEventListener('error', (event) => {
  // Sentry.captureException(event.error)
})
window.addEventListener('unhandledrejection', (event) => {
  // Sentry.captureException(event.reason)
  event.preventDefault()
})
```

**One-liner:** `useEffect` errors need try/catch inside — Error Boundary won't catch them. `cancelled` flag prevents state update on unmounted component. Global handlers catch anything that slipped through.

---

# Category 34: GraphQL in React
> 🟡 Important — partially covered (Apollo named, no explanation) → now fully covered · Study 11th

---

### 🔴 34.1 GraphQL vs REST

```
REST — many endpoints, fixed response shapes:
  GET /api/users/123           → ALL user fields returned
  GET /api/users/123/orders    → separate request
  GET /api/orders/456/items    → separate request
  Total: 3 requests, lots of unused data (over-fetching)

GraphQL — one endpoint, you describe exactly what you need:
  POST /graphql
  {
    user(id: "123") {
      name email                  ← only these fields
      orders(last: 5) {           ← related data in same request
        id total
        items { name price }
      }
    }
  }
  Total: 1 request, exactly the data needed
```

**Three operation types:**
```graphql
query GetUser($id: ID!) { user(id: $id) { name email } }      # read
mutation CreateUser($input: CreateUserInput!) { createUser(input: $input) { id } }  # write
subscription OnOrderUpdated($id: ID!) { orderUpdated(orderId: $id) { status } }     # real-time
```

**One-liner:** One endpoint. Client describes exactly what it needs — no over/under-fetching. Query = read. Mutation = write. Subscription = real-time via WebSocket.

---

### 🔴 34.2 Apollo Client setup and useQuery

```tsx
// main.tsx
import { ApolloClient, InMemoryCache, ApolloProvider } from '@apollo/client'

const client = new ApolloClient({
  uri: import.meta.env.VITE_GRAPHQL_URL,
  cache: new InMemoryCache(),
})

// Wrap app
<ApolloProvider client={client}><App /></ApolloProvider>

// Define queries with gql tag
import { gql, useQuery } from '@apollo/client'

const GET_USERS = gql`
  query GetUsers {
    users { id name email role }
  }
`

interface User { id: string; name: string; email: string }
interface GetUsersData { users: User[] }

function UserList() {
  const { data, loading, error, refetch } = useQuery<GetUsersData>(GET_USERS, {
    fetchPolicy: 'cache-first',
  })

  if (loading) return <Spinner />
  if (error) return <p>Error: {error.message}</p>
  return <ul>{data?.users.map(u => <li key={u.id}>{u.name}</li>)}</ul>
}
```

**One-liner:** ApolloProvider wraps app. `gql` tag defines queries. `useQuery(GET_USERS)` → `{ data, loading, error }`. Apollo normalises cache by `__typename + id` automatically.

---

### 🟡 34.3 useMutation

```tsx
const CREATE_USER = gql`
  mutation CreateUser($name: String!, $email: String!) {
    createUser(name: $name, email: $email) { id name email }
  }
`

function CreateUserForm() {
  const [createUser, { loading, error }] = useMutation(CREATE_USER, {
    refetchQueries: [{ query: GET_USERS }],  // refetch list after create
    onCompleted: () => toast.success('User created'),
    onError: (err) => toast.error(err.message),
  })

  return (
    <button
      disabled={loading}
      onClick={() => createUser({ variables: { name, email } })}
    >
      {loading ? 'Creating...' : 'Create'}
    </button>
  )
}
```

**One-liner:** `useMutation` returns `[mutate, { loading, error }]`. `refetchQueries` re-fetches affected queries. `update` function for direct cache manipulation (more efficient, no extra request).

---

### 🟡 34.4 Apollo normalised cache vs TanStack Query

```
Apollo InMemoryCache normalises by __typename + id:
  Query 1 returns User:1 { name: 'Alice' }
  Query 2 returns User:1 { name: 'Alice', email: '...' }
  Cache stores ONE User:1 entry — both queries share it
  Update User:1 anywhere → EVERY query showing that user updates automatically

TanStack Query caches by queryKey (full response):
  ['users'] and ['user', 1] are separate cache entries
  Must invalidate manually after mutations

Choose Apollo when:
  Deeply nested data, same entities in many queries
  Normalised cache saves manual invalidation

Choose TanStack Query for GraphQL when:
  Simpler setup, comfortable with manual invalidation
  Already using TanStack Query for some REST endpoints
```

**One-liner:** Apollo normalised cache: entity stored once, all queries auto-sync. TanStack Query: whole response per key, manual invalidation. Apollo = better for complex nested data.

---

# Category 35: Bundle Optimization
> 🟡 Important — partially covered (code splitting only) → now fully covered · Study 12th

---

### 🔴 35.1 Analysing your bundle

```ts
// vite.config.ts
import { visualizer } from 'rollup-plugin-visualizer'

export default defineConfig({
  plugins: [
    react(),
    visualizer({
      filename: 'dist/bundle-analysis.html',
      open: true,       // auto-open after build
      gzipSize: true,
    })
  ],
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          'vendor-react': ['react', 'react-dom'],
          'vendor-router': ['react-router-dom'],
          'vendor-charts': ['recharts', 'd3'],  // heavy lib in own chunk
        }
      }
    }
  }
})
```

```bash
npm run build
# Opens dist/bundle-analysis.html — treemap of every file in bundle
# Look for: unexpectedly large packages, duplicates, unused code
```

**One-liner:** `rollup-plugin-visualizer` = treemap of bundle. Run after build. Find large unexpected packages. `manualChunks` splits heavy libraries into separate files.

---

### 🔴 35.2 Code splitting

```tsx
// Route-level — load page code only when route is visited
const Dashboard = lazy(() => import('./pages/Dashboard'))
const Reports = lazy(() => import('./pages/Reports'))
const AdminPanel = lazy(() => import('./pages/AdminPanel'))

function App() {
  return (
    <Suspense fallback={<PageSpinner />}>
      <Routes>
        <Route path="/" element={<Home />} />          {/* not lazy */}
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/admin" element={<AdminPanel />} />
      </Routes>
    </Suspense>
  )
}

// Component-level — load heavy features only when needed
function ProductPage() {
  const [showReviews, setShowReviews] = useState(false)
  const ReviewSection = lazy(() => import('./ReviewSection'))

  return (
    <div>
      <ProductDetails />
      <button onClick={() => setShowReviews(true)}>Show Reviews</button>
      {showReviews && (
        <Suspense fallback={<ReviewsSkeleton />}>
          <ReviewSection />
        </Suspense>
      )}
    </div>
  )
}
```

**One-liner:** `React.lazy(() => import('./Page'))` + Suspense = load code on demand. Route-level splitting is the biggest win. Component-level for heavy features triggered by user action.

---

### 🔴 35.3 Tree shaking — removing unused code

```ts
// ❌ Imports ENTIRE lodash (~70KB)
import _ from 'lodash'
const result = _.pick(obj, ['name'])

// ✅ Named import — only pick is bundled
import { pick } from 'lodash-es'  // lodash-es = tree-shakeable ES module version
const result = pick(obj, ['name'])

// ❌ moment.js — not tree-shakeable, always ~330KB
import moment from 'moment'

// ✅ date-fns — tree-shakeable, only import what you use
import { format, parseISO } from 'date-fns'

// Verify tree-shaking: bundle analysis should show only what you actually use
// If you see entire libraries for one function → tree shaking is broken for that pkg
// Likely reason: library uses CommonJS (require()) not ES modules (import/export)
```

**One-liner:** Tree shaking removes unused exports. Needs ES modules. Use named imports. Replace moment.js with date-fns, lodash with lodash-es. Check bundle analysis to verify it's working.

---

### 🟡 35.4 Performance checklist

```
Bundle:
  ✅ Bundle analysis (rollup-plugin-visualizer)
  ✅ Route-level code splitting (React.lazy)
  ✅ Named imports — not whole libraries
  ✅ Replace moment.js → date-fns, lodash → lodash-es
  ✅ manualChunks for heavy libraries

Images:
  ✅ loading="lazy" on all below-fold images
  ✅ WebP/AVIF format (50-80% smaller than PNG/JPEG)
  ✅ Explicit width + height (prevents layout shift / CLS)
  ✅ Preload hero images: <link rel="preload" as="image">

React-specific:
  ✅ React.memo for expensive components
  ✅ useMemo for expensive calculations
  ✅ useCallback for stable function references
  ✅ Virtualise long lists (react-window)
  ✅ React DevTools Profiler to find bottlenecks

Build/server:
  ✅ gzip/brotli compression (nginx)
  ✅ Content-hash filenames for long-term cache (Vite default)
  ✅ Serve from CDN
```

**One-liner:** Measure first (Lighthouse + bundle analysis). Biggest wins: code splitting, image optimisation, tree shaking. Verify with before/after Lighthouse performance score.

---

# Category 36: Portals
> 🟡 Important — partially covered (createPortal in machine coding only) → now fully covered · Study 13th

---

### 🔴 36.1 What are Portals and why you need them

**Explanation:** `ReactDOM.createPortal` renders component output into a different DOM node — typically `document.body`. This solves the `overflow: hidden` and `z-index` CSS problem.

```
The problem:
  <div style={{ overflow: 'hidden', position: 'relative', zIndex: 1 }}>
    <Modal />   ← Modal is rendered INSIDE this clipping div
  </div>
  Even with zIndex: 9999 on the modal, it's clipped by overflow: hidden
  The modal is visually trapped inside its parent's CSS context

This affects:
  · Modals inside cards with overflow: hidden
  · Tooltips inside scrollable containers
  · Dropdowns inside table cells
  · Any fixed/absolute UI inside a relative container

Portal solution:
  Modal's DOM output moves to document.body — outside all containers
  No clipping. No z-index battles. No CSS inheritance issues.
```

---

### 🔴 36.2 createPortal — full modal implementation

```tsx
import { useEffect, useRef } from 'react'
import ReactDOM from 'react-dom'

interface ModalProps {
  isOpen: boolean
  onClose: () => void
  title: string
  children: React.ReactNode
}

function Modal({ isOpen, onClose, title, children }: ModalProps) {
  const previousFocusRef = useRef<HTMLElement | null>(null)
  const modalRef = useRef<HTMLDivElement>(null)

  useEffect(() => {
    if (isOpen) {
      previousFocusRef.current = document.activeElement as HTMLElement
      document.body.style.overflow = 'hidden'  // prevent background scroll
      modalRef.current?.focus()
    } else {
      document.body.style.overflow = ''
      previousFocusRef.current?.focus()         // return focus on close
    }
    return () => { document.body.style.overflow = '' }
  }, [isOpen])

  if (!isOpen) return null

  // Portal renders into document.body — outside any overflow: hidden containers
  return ReactDOM.createPortal(
    <div
      style={{
        position: 'fixed', top: 0, left: 0, right: 0, bottom: 0,
        backgroundColor: 'rgba(0,0,0,0.5)',
        display: 'flex', alignItems: 'center', justifyContent: 'center',
        zIndex: 1000,
      }}
      onClick={onClose}  // close on backdrop click
    >
      <div
        ref={modalRef}
        role="dialog"
        aria-modal="true"
        aria-labelledby="modal-title"
        tabIndex={-1}
        style={{
          background: 'white', borderRadius: 8, padding: 24,
          minWidth: 320, maxWidth: 500, outline: 'none',
        }}
        onClick={e => e.stopPropagation()}  // prevent closing on content click
        onKeyDown={e => { if (e.key === 'Escape') onClose() }}
      >
        <div style={{ display: 'flex', justifyContent: 'space-between', marginBottom: 16 }}>
          <h2 id="modal-title" style={{ margin: 0 }}>{title}</h2>
          <button onClick={onClose} aria-label="Close dialog">×</button>
        </div>
        {children}
      </div>
    </div>,
    document.body  // ← renders HERE, outside component tree DOM position
  )
}

// Usage
function App() {
  const [showModal, setShowModal] = useState(false)
  return (
    <div style={{ overflow: 'hidden' }}>  {/* no longer clips the modal */}
      <button onClick={() => setShowModal(true)}>Open Modal</button>
      <Modal isOpen={showModal} onClose={() => setShowModal(false)} title="Confirm">
        <p>Are you sure you want to delete this item?</p>
        <button onClick={() => setShowModal(false)}>Cancel</button>
        <button onClick={confirm}>Confirm</button>
      </Modal>
    </div>
  )
}
```

**Interview Q: Why use a Portal for modals instead of just a div with high z-index?**
**A:** High z-index alone doesn't work if any ancestor has `overflow: hidden` or `overflow: auto` — the modal is visually clipped regardless of z-index. Portal moves the modal's DOM output to `document.body`, completely outside the component's DOM tree, so no ancestor CSS can affect it. The React component tree (props, context, events) still works normally — only the DOM position changes.

**Common Mistake:** Forgetting `e.stopPropagation()` on the modal content div. Without it, clicking anywhere inside the modal triggers the backdrop's `onClick` and closes the modal immediately.

**One-liner:** Portal = render into `document.body` — solves overflow/z-index CSS conflicts. React tree still works (context, events). `stopPropagation` on content prevents backdrop click from closing.

---

### 🟡 36.3 Tooltip with Portal

```tsx
function Tooltip({ text, children }: { text: string; children: React.ReactElement }) {
  const [visible, setVisible] = useState(false)
  const [position, setPosition] = useState({ top: 0, left: 0 })
  const triggerRef = useRef<HTMLElement>(null)

  const showTooltip = () => {
    if (triggerRef.current) {
      const rect = triggerRef.current.getBoundingClientRect()
      setPosition({
        top: rect.bottom + window.scrollY + 8,   // below the trigger
        left: rect.left + window.scrollX,
      })
    }
    setVisible(true)
  }

  return (
    <>
      {React.cloneElement(children, {
        ref: triggerRef,
        onMouseEnter: showTooltip,
        onMouseLeave: () => setVisible(false),
      })}
      {visible && ReactDOM.createPortal(
        <div style={{
          position: 'absolute',
          top: position.top,
          left: position.left,
          background: '#333',
          color: 'white',
          padding: '4px 8px',
          borderRadius: 4,
          fontSize: 13,
          zIndex: 9999,
          pointerEvents: 'none',
        }}>
          {text}
        </div>,
        document.body
      )}
    </>
  )
}

// Usage
<Tooltip text="Delete this item">
  <button>Delete</button>
</Tooltip>
```

**One-liner:** Tooltip portals to body = no clipping by `overflow: hidden` table cells. Calculate position from `getBoundingClientRect()`. `pointerEvents: none` so tooltip doesn't interfere with hover.

---

# Quick Revision — All Categories One-Liners

```
CATEGORY 21 — TYPESCRIPT WITH REACT  (🔴 Study 1st)
  Props: interface + plain function. Never React.FC. Never any — use unknown
  useState<User | null>(null) — generic needed when initial value is null
  DOM ref: useRef<HTMLInputElement>(null). Value ref: useRef<number>(0)
  Events: React.ChangeEvent<HTMLInputElement>, React.FormEvent<HTMLFormElement>
  Context: type createContext with interface. Custom hook handles null check once
  Generic hooks: useFetch<T>(url) — one hook, any data type
  Utility types: Partial (all optional), Pick (keep these), Omit (remove these), Record

CATEGORY 22 — ACCESSIBILITY  (🔴 Study 2nd)
  Semantic HTML first. ARIA only for custom widgets that HTML can't express
  aria-label = text. aria-expanded = open/closed state. aria-describedby = description
  Tab = navigate. Enter/Space = activate. Arrow keys = within component
  tabIndex={0} = in tab order. tabIndex={-1} = programmatic focus only
  Modal: focus inside on open, return focus on close. role="dialog" aria-modal="true"
  Forms: label htmlFor = input id. aria-invalid for errors. role="alert" for messages
  4.5:1 color contrast. Never color-only — add icon or text too

CATEGORY 23 — API LAYER / AXIOS  (🔴 Study 3rd)
  axios.create({ baseURL, timeout, headers }) = one configured instance for whole app
  Request interceptor = auto-attach auth token to every request
  Response interceptor = handle 401/403/500 globally once, not in every component
  One service file per feature with typed async functions
  axios.isAxiosError(error) narrows type. 422 = validation. 409 = conflict

CATEGORY 24 — SECURITY  (🔴 Study 4th)
  React auto-escapes JSX. dangerouslySetInnerHTML bypasses it — sanitize with DOMPurify
  Never href="javascript:" — validate URLs before using
  CSRF: SameSite=Strict cookie + CSRF token header on mutations
  CSP: HTTP header whitelisting allowed script/style sources — set in nginx
  Frontend code is public — never put secrets. VITE_ vars are in the bundle

CATEGORY 25 — ENVIRONMENT VARIABLES  (🔴 Study 5th)
  VITE_ prefix = in browser bundle. No prefix = build scripts only
  import.meta.env.VITE_X — NOT process.env.X in Vite apps
  .env.local = gitignored personal overrides (highest priority)
  --mode staging flag uses .env.staging
  requireEnv() on startup = fail fast with clear message if var missing

CATEGORY 32 — SSR / NEXT.JS  (🔴 Study 6th)
  CSR = browser renders. SSR = server per request. SSG = built at build time. ISR = SSG + regen
  Hydration = React attaches events to server HTML. Gap between FCP and TTI
  Mismatch = server HTML ≠ client render. Never use window/localStorage outside useEffect
  App Router: server components by default (no hooks, zero JS shipped)
  'use client' = browser component. loading.tsx = auto Suspense. error.tsx = auto Error Boundary
  Promise.all for parallel fetches — avoid waterfall

CATEGORY 33 — ERROR HANDLING  (🔴 Study 7th)
  4 layers: Error Boundary → event try/catch → API interceptors → global window handlers
  Error Boundary catches render errors only. NOT event handlers or async — use try/catch there
  Wrap each feature section in separate Error Boundary for granular recovery
  Interceptor = 401/403/500 globally once. onError mutation = component-specific cases
  useEffect: try/catch inside + cancelled flag to prevent state update after unmount

CATEGORY 26 — i18n  (🟡 Study 8th)
  react-i18next: useTranslation() → t('key'). JSON files per language
  Auto-pluralization with _one/_other suffixes. i18n.changeLanguage() to switch
  RTL: document.dir = 'rtl'. CSS logical properties (margin-inline-start not margin-left)

CATEGORY 27 — ANIMATIONS  (🟡 Study 9th)
  CSS transitions: toggle class/style on state change. No library needed for simple cases
  Framer Motion: motion.div initial/animate/exit. AnimatePresence for unmount exit animations
  CSS for simple hover/show-hide. Framer Motion for complex enter/exit/drag/layout

CATEGORY 28 — FORM VALIDATION  (🟡 Study 10th)
  React Hook Form: register connects inputs. handleSubmit validates. formState.errors for display
  Uncontrolled inputs = no re-render per keystroke = performant for large forms
  Zod: schema = TypeScript type + validation rules in one. z.infer = auto type
  zodResolver connects Zod to RHF. Share schema with backend for identical validation

CATEGORY 34 — GRAPHQL  (🟡 Study 11th)
  One endpoint, client describes exactly what it needs — no over/under-fetching
  Query = read. Mutation = write. Subscription = real-time WebSocket
  ApolloProvider wraps app. useQuery(GET_USERS) → { data, loading, error }
  Normalised cache: User:1 stored once, all queries auto-sync on update
  Choose Apollo for complex nested data. TanStack Query for simpler GraphQL setup

CATEGORY 35 — BUNDLE OPTIMIZATION  (🟡 Study 12th)
  Measure first: rollup-plugin-visualizer treemap after npm run build
  Code splitting: React.lazy + Suspense. Route-level = biggest win
  Tree shaking: named imports, ES modules, lodash-es not lodash
  Replace moment.js with date-fns. Check bundle analysis to verify tree shaking works
  manualChunks = split heavy libs (charts, maps) into separate files

CATEGORY 36 — PORTALS  (🟡 Study 13th)
  ReactDOM.createPortal(children, document.body) = renders outside component DOM position
  Solves overflow:hidden and z-index CSS conflicts for modals, tooltips, dropdowns
  React tree (context, events, props) still works normally — only DOM position changes
  Always stopPropagation on modal content to prevent backdrop click from closing it
  Focus trap: focus inside on open, return to trigger on close. Escape key to close

CATEGORY 29 — SEO  (🟢 Study 14th)
  SPA default = empty HTML shell → bad SEO. Fix: SSR (Next.js), SSG, or prerendering
  react-helmet-async for SPA meta tags. Next.js has built-in metadata export
  Set unique title + description + og: per page. Canonical URL prevents duplicate penalty

CATEGORY 30 — BROWSER STORAGE  (🟢 Study 15th)
  localStorage = persistent. sessionStorage = tab only. httpOnly cookie = sent with requests
  Never store auth tokens in localStorage — XSS can steal them instantly
  useLocalStorage custom hook for React state + localStorage sync with JSON parsing
  IndexedDB = client-side database for large data (drafts, offline cache). Use idb library

CATEGORY 31 — FEATURE FLAGS  (🟢 Study 16th)
  Feature flags = toggle features without deployment. Separate code release from feature release
  Ship behind disabled flag → enable 1% → monitor → 10% → 100%
  Instant rollback: toggle flag off. No deployment needed.
  useFeatureFlag hook + Feature component for clean conditional rendering
  Use LaunchDarkly, Flagsmith, or simple context-based approach
```

[↑ Back to Table of Contents](#table-of-contents)