# React + TypeScript Practice Guide — Part 3
## Level 3: Advanced (4% of Interview Questions)

> **Before starting Level 3:** You should be comfortable with Level 1 and Level 2.
> These scenarios are about things junior devs get wrong and seniors get right.
> The bugs here are subtle. The fixes reveal deep understanding.

---

# Table of Contents

- [Pattern 12: ⚡ Performance Optimization](#pattern-12--performance-optimization)
  - [Scenario 1 — The Search Box Destroying the Server](#-pattern-12--scenario-1--the-search-box-destroying-the-server)
  - [Scenario 2 — The Dashboard Re-rendering Everything](#-pattern-12--scenario-2--the-dashboard-re-rendering-everything)
  - [Scenario 3 — The Product List That Freezes the Browser](#-pattern-12--scenario-3--the-product-list-that-freezes-the-browser)
- [Pattern 13: 📡 Real-time / Polling](#pattern-13--real-time--polling)
  - [Scenario 1 — The Live Order Tracker That Never Updates](#-pattern-13--scenario-1--the-live-order-tracker-that-never-updates)
  - [Scenario 2 — The Notification Bell That Leaks Memory](#-pattern-13--scenario-2--the-notification-bell-that-leaks-memory)
  - [Scenario 3 — The Chat That Misses Messages](#-pattern-13--scenario-3--the-chat-that-misses-messages)
- [Pattern 14: 🔄 Complex Async](#pattern-14--complex-async)
  - [Scenario 1 — The Search That Shows Wrong Results](#-pattern-14--scenario-1--the-search-that-shows-wrong-results)
  - [Scenario 2 — The Form That Submits After You Leave](#-pattern-14--scenario-2--the-form-that-submits-after-you-leave)
  - [Scenario 3 — The Dashboard That Loads in the Wrong Order](#-pattern-14--scenario-3--the-dashboard-that-loads-in-the-wrong-order)
- [Interview Pattern Picker — Level 3](#interview-pattern-picker--level-3)

---

# Pattern 12: ⚡ Performance Optimization

> **Level 3 — Advanced**

🧠 **The Mental Model:**
Performance problems in React almost always come from one of three causes:
too many renders, too much work per render, or too much data per render.
Before reaching for `useMemo` or `React.memo`, ask: "Is this actually slow?
Have I measured it?" Premature optimization makes code harder to read for no gain.

⚡ **The One Rule:**
Measure first, optimize second — and when you do optimize, understand
exactly which of the three causes you are fixing.

🔷 **TypeScript focus in this pattern:**
Typed `useMemo` and `useCallback` with proper generic return types.
Typed `React.memo` with explicit props interface.
Typed debounce utility with generics.

🚨 **What junior devs always get wrong:**
Wrapping everything in `useMemo` and `useCallback` thinking it always helps.
These hooks have overhead — they cost memory and comparison work.
They only help when the work they save is more expensive than the comparison.

📍 **You need this pattern when:**
- A component re-renders when its props have not changed
- A search box fires an API call on every single keystroke
- A list with thousands of items causes the browser to freeze on scroll

📋 **Interview asks this as:**
- "This search is making too many API calls — how do you fix it?"
- "Why does this child component re-render when only the parent's unrelated state changes?"
- "How would you handle rendering a list of 10,000 items?"

📦 **Production-ready version always includes:**
- Debounce on any user input that triggers expensive work
- `React.memo` only on components that receive stable props and re-render often
- `useCallback` paired with `React.memo` — one without the other is useless
- Measured evidence that the optimization actually helped

---

### 🔴 Pattern 12 · Scenario 1 — The Search Box Destroying the Server

🏢 **Situation:**
You are working at a job portal. Your backend engineer sends you a Slack message:
"The search API is getting 3,000 requests per minute from a single user session.
Our server is struggling. Can you look at the frontend search component?"
You open the code and immediately see the problem.

🔷 **Types used in this scenario:**

```typescript
interface Job {
  id: string;
  title: string;
  company: string;
  location: string;
  salary: number;
  postedAt: string;
}

interface SearchResponse {
  jobs: Job[];
  total: number;
}
```

**Here is the current code:**

```tsx
import { useState, useEffect } from 'react';

interface Job {
  id: string;
  title: string;
  company: string;
  location: string;
  salary: number;
  postedAt: string;
}

interface SearchResponse {
  jobs: Job[];
  total: number;
}

function JobSearch(): JSX.Element {
  const [query, setQuery] = useState<string>('');
  const [jobs, setJobs] = useState<Job[]>([]);
  const [isLoading, setIsLoading] = useState<boolean>(false);

  // BUG: fires on every single keystroke
  useEffect(() => {
    const searchJobs = async (): Promise<void> => {
      if (!query) {
        setJobs([]);
        return;
      }
      setIsLoading(true);
      const res = await fetch(`/api/jobs/search?q=${encodeURIComponent(query)}`);
      const data = await res.json() as SearchResponse;
      setJobs(data.jobs);
      setIsLoading(false);
    };
    searchJobs();
  }, [query]); // runs on every character typed

  return (
    <div>
      <input
        type="text"
        value={query}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) => setQuery(e.target.value)}
        placeholder="Search jobs..."
      />
      {isLoading && <p>Searching...</p>}
      {jobs.map(job => (
        <div key={job.id}>
          <h3>{job.title}</h3>
          <p>{job.company} — {job.location}</p>
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Add debounce so the API only fires 400ms after the user stops typing
- ✅ Cancel the previous API call if a new one starts (avoid stale results)
- ✅ Write a reusable typed `useDebounce<T>` hook
- ✅ Show how many results were found

<details>
<summary>💡 Hint — How to think about this</summary>

There are two separate problems here — not one.
First: the API fires too often (fix with debounce — delay the search).
Second: if two searches fire close together, the slower one might
resolve last and show wrong results (fix with AbortController).
A typed `useDebounce<T>` hook keeps the debounce logic reusable
and out of the component.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useRef } from 'react';

// ─── Types ───────────────────────────────────────────────
interface Job {
  id: string;
  title: string;
  company: string;
  location: string;
  salary: number;
  postedAt: string;
}

interface SearchResponse {
  jobs: Job[];
  total: number;
}

// ─── Reusable Typed Debounce Hook ─────────────────────────
// Generic T means this hook works for any value type
function useDebounce<T>(value: T, delayMs: number): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value);

  useEffect(() => {
    // Set a timer to update debouncedValue after delay
    const timer = setTimeout(() => {
      setDebouncedValue(value);
    }, delayMs);

    // Cancel timer if value changes before delay completes
    return () => clearTimeout(timer);
  }, [value, delayMs]);

  return debouncedValue;
}

// ─── Component ────────────────────────────────────────────
function JobSearch(): JSX.Element {
  const [query, setQuery] = useState<string>('');
  const [jobs, setJobs] = useState<Job[]>([]);
  const [total, setTotal] = useState<number>(0);
  const [isLoading, setIsLoading] = useState<boolean>(false);
  const [error, setError] = useState<string | null>(null);

  // Only updates 400ms after the user stops typing
  const debouncedQuery = useDebounce<string>(query, 400);

  useEffect(() => {
    if (!debouncedQuery.trim()) {
      setJobs([]);
      setTotal(0);
      return;
    }

    // AbortController cancels the previous fetch if a new one starts
    const controller = new AbortController();

    const searchJobs = async (): Promise<void> => {
      try {
        setIsLoading(true);
        setError(null);

        const res = await fetch(
          `/api/jobs/search?q=${encodeURIComponent(debouncedQuery)}`,
          { signal: controller.signal } // attach abort signal
        );

        if (!res.ok) throw new Error(`Search failed: ${res.status}`);

        const data = await res.json() as SearchResponse;
        setJobs(data.jobs);
        setTotal(data.total);

      } catch (err: unknown) {
        // Ignore AbortError — it means a newer request replaced this one
        if (err instanceof Error && err.name === 'AbortError') return;
        setError(err instanceof Error ? err.message : 'Search failed');
      } finally {
        setIsLoading(false);
      }
    };

    searchJobs();

    // Cleanup: abort the fetch if query changes before it completes
    return () => controller.abort();

  }, [debouncedQuery]); // only fires when debouncedQuery changes

  return (
    <div>
      <input
        type="text"
        value={query}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setQuery(e.target.value)
        }
        placeholder="Search jobs..."
      />

      {/* Show query vs debouncedQuery to demonstrate the delay */}
      {query !== debouncedQuery && (
        <p style={{ color: '#999', fontSize: 12 }}>Waiting for you to stop typing...</p>
      )}

      {isLoading && <p>Searching...</p>}
      {error && <p style={{ color: 'red' }}>{error}</p>}

      {!isLoading && debouncedQuery && (
        <p>{total} jobs found for "{debouncedQuery}"</p>
      )}

      {jobs.map(job => (
        <div key={job.id}>
          <h3>{job.title}</h3>
          <p>{job.company} — {job.location}</p>
          <p>₹{job.salary.toLocaleString()}</p>
        </div>
      ))}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
They want the generic `useDebounce<T>` hook — not inline setTimeout logic.
They also check for `AbortController` to cancel stale requests.
Without abort, fast typists can see results from an old query appear after
the new one — a subtle but real bug called a race condition.

🔷 **TypeScript lesson:**
`useDebounce<T>` is a generic hook — the `T` makes it work for strings,
numbers, or any other type. The caller decides the type:
`useDebounce<string>(query, 400)`. This is how you write reusable
TypeScript utilities — generics over specifics.

⚡ **Why this matters in production:**
Without debounce: 10 characters typed = 10 API calls, server under load.
Without AbortController: fast typing shows results from old queries —
users see search results that do not match what they typed.

🔍 **What a senior dev spots in 10 seconds:**
`useEffect` with `[query]` dependency and a `fetch` inside — this is the
classic "fires on every keystroke" pattern. The fix is always the same:
debounce the value, not the effect.

✅ **What the interviewer is checking:**
Debounce implementation (not just "use lodash.debounce"), AbortController
for stale request cancellation, and a generic reusable hook rather than
inline setTimeout.

🚀 **Before you ship this, verify:**
- [ ] Does search fire only after user stops typing (not on every key)?
- [ ] Does typing fast not show results from an older query?
- [ ] Is the `useDebounce` hook reusable for non-string values?
- [ ] Does it handle empty query correctly (clear results)?
- [ ] Are all API errors shown to the user?

</details>

---

### 🔴 Pattern 12 · Scenario 2 — The Dashboard Re-rendering Everything

🏢 **Situation:**
You are at a SaaS analytics company. The main dashboard has a header
with the user's name, a sidebar with navigation, and a main content area
with charts. Every time any chart data updates, you notice in React DevTools
that the Header and Sidebar are re-rendering too — even though their data
has not changed at all. On slow devices, users can see a visual flicker.
Your tech lead asks you to fix the unnecessary re-renders.

🔷 **Types used in this scenario:**

```typescript
interface User {
  id: string;
  name: string;
  avatarUrl: string;
}

interface ChartData {
  label: string;
  value: number;
  date: string;
}

interface DashboardData {
  revenue: ChartData[];
  users: ChartData[];
  orders: ChartData[];
}
```

**Here is the current code:**

```tsx
import { useState, useEffect } from 'react';

interface User {
  id: string;
  name: string;
  avatarUrl: string;
}

interface ChartData {
  label: string;
  value: number;
  date: string;
}

interface DashboardData {
  revenue: ChartData[];
  users: ChartData[];
  orders: ChartData[];
}

// These components are expensive to render
function Header({ user }: { user: User }): JSX.Element {
  console.log('Header rendered'); // you can see this fires constantly
  return (
    <header>
      <img src={user.avatarUrl} alt={user.name} />
      <span>{user.name}</span>
    </header>
  );
}

function Sidebar({ onNavigate }: { onNavigate: (page: string) => void }): JSX.Element {
  console.log('Sidebar rendered');
  return (
    <nav>
      <button onClick={() => onNavigate('dashboard')}>Dashboard</button>
      <button onClick={() => onNavigate('reports')}>Reports</button>
      <button onClick={() => onNavigate('settings')}>Settings</button>
    </nav>
  );
}

function Dashboard(): JSX.Element {
  const [user, setUser] = useState<User | null>(null);
  const [data, setData] = useState<DashboardData | null>(null);
  const [currentPage, setCurrentPage] = useState<string>('dashboard');

  useEffect(() => {
    // Load user once
    fetch('/api/user/me')
      .then(r => r.json())
      .then((u: User) => setUser(u));

    // Refresh chart data every 10 seconds
    const interval = setInterval(() => {
      fetch('/api/dashboard/data')
        .then(r => r.json())
        .then((d: DashboardData) => setData(d));
    }, 10000);

    return () => clearInterval(interval);
  }, []);

  // This function is recreated on every render
  const handleNavigate = (page: string): void => {
    setCurrentPage(page);
  };

  if (!user) return <p>Loading...</p>;

  return (
    <div>
      {/* These re-render every time data refreshes */}
      <Header user={user} />
      <Sidebar onNavigate={handleNavigate} />
      <main>
        <p>Current page: {currentPage}</p>
        {data && <p>Revenue data points: {data.revenue.length}</p>}
      </main>
    </div>
  );
}
```

**Your tasks:**
- ✅ Prevent `Header` from re-rendering when chart data updates
- ✅ Prevent `Sidebar` from re-rendering when chart data updates
- ✅ Fix the `handleNavigate` function reference stability
- ✅ Explain why `React.memo` alone is not enough for `Sidebar`

<details>
<summary>💡 Hint — How to think about this</summary>

`React.memo` stops a component from re-rendering if its props
have not changed — but it uses shallow equality comparison.
For `Header`, the `user` object does not change, so memo alone works.
For `Sidebar`, the `onNavigate` prop is a function that gets
recreated on every render of `Dashboard`. Even with `React.memo`,
a new function reference === new prop === re-render.
You need `useCallback` to stabilize the function reference.
`React.memo` + `useCallback` together = the complete fix.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useCallback, memo } from 'react';

// ─── Types ────────────────────────────────────────────────
interface User {
  id: string;
  name: string;
  avatarUrl: string;
}

interface ChartData {
  label: string;
  value: number;
  date: string;
}

interface DashboardData {
  revenue: ChartData[];
  users: ChartData[];
  orders: ChartData[];
}

interface HeaderProps {
  user: User;
}

interface SidebarProps {
  onNavigate: (page: string) => void;
}

// ─── Memoized Components ──────────────────────────────────

// React.memo wraps the component — skips re-render if props unchanged
// For Header: user object reference stays the same → works perfectly
const Header = memo(function Header({ user }: HeaderProps): JSX.Element {
  console.log('Header rendered'); // now only logs once on load
  return (
    <header>
      <img src={user.avatarUrl} alt={user.name} />
      <span>{user.name}</span>
    </header>
  );
});

// For Sidebar: memo alone is NOT enough
// because onNavigate is a new function on every Dashboard render
// We also need useCallback in the parent to stabilize the reference
const Sidebar = memo(function Sidebar({ onNavigate }: SidebarProps): JSX.Element {
  console.log('Sidebar rendered'); // now only logs once
  return (
    <nav>
      <button onClick={() => onNavigate('dashboard')}>Dashboard</button>
      <button onClick={() => onNavigate('reports')}>Reports</button>
      <button onClick={() => onNavigate('settings')}>Settings</button>
    </nav>
  );
});

// ─── Parent Component ─────────────────────────────────────
function Dashboard(): JSX.Element {
  const [user, setUser] = useState<User | null>(null);
  const [data, setData] = useState<DashboardData | null>(null);
  const [currentPage, setCurrentPage] = useState<string>('dashboard');

  useEffect(() => {
    const loadUser = async (): Promise<void> => {
      const res = await fetch('/api/user/me');
      const u = await res.json() as User;
      setUser(u);
    };
    loadUser();

    const interval = setInterval(async () => {
      const res = await fetch('/api/dashboard/data');
      const d = await res.json() as DashboardData;
      setData(d);
    }, 10000);

    return () => clearInterval(interval);
  }, []);

  // useCallback stabilizes the function reference
  // Same function instance across renders → Sidebar memo comparison passes
  // Empty [] deps because setCurrentPage is stable (from useState)
  const handleNavigate = useCallback((page: string): void => {
    setCurrentPage(page);
  }, []); // no dependencies — setCurrentPage never changes

  if (!user) return <p>Loading dashboard...</p>;

  return (
    <div>
      {/* Header: user never changes → memo prevents re-renders */}
      <Header user={user} />

      {/* Sidebar: memo + useCallback prevents re-renders */}
      <Sidebar onNavigate={handleNavigate} />

      <main>
        <p>Current page: {currentPage}</p>
        {data && (
          <div>
            <p>Revenue points: {data.revenue.length}</p>
            <p>User points: {data.users.length}</p>
          </div>
        )}
      </main>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
They specifically look for the `React.memo` + `useCallback` combination —
and they want you to explain WHY both are needed. `React.memo` alone
fails for `Sidebar` because the function prop changes reference every render.

🔷 **TypeScript lesson:**
`memo` is typed automatically — it infers props from the wrapped component.
`useCallback` return type is inferred from the function body.
The explicit `SidebarProps` interface documents the contract clearly —
you can see at a glance that `Sidebar` depends on a stable function reference.

⚡ **Why this matters in production:**
Without memo + useCallback: every 10-second data refresh re-renders Header
and Sidebar — on slow devices this causes visible flicker and wasted CPU.
At scale with 20+ components, unnecessary re-renders degrade performance
significantly.

🔍 **What a senior dev spots in 10 seconds:**
A function defined inline inside a component being passed as a prop to
a child component — `const handleNavigate = (page) => ...`. This is always
a new reference every render. If the child uses `React.memo`, it is useless
without `useCallback` on the parent's function.

✅ **What the interviewer is checking:**
The explanation: "React.memo alone is not enough for Sidebar because
the function prop is a new reference every render. useCallback makes
the reference stable. Both together stop the re-render."

🚀 **Before you ship this, verify:**
- [ ] Does Header re-render when chart data updates? (Should not)
- [ ] Does Sidebar re-render when chart data updates? (Should not)
- [ ] Is `useCallback` only used where the function is passed to a memo'd component?
- [ ] Did you measure with React DevTools before adding memo?
- [ ] Is there actually a performance problem — or were you optimizing preemptively?

</details>

---

### 🔴 Pattern 12 · Scenario 3 — The Product List That Freezes the Browser

🏢 **Situation:**
You are at an e-commerce company. The product catalog page loads 5,000 products
from the API (the client insisted on "all products on one page — no pagination").
The browser freezes for 3-4 seconds on load and on every search filter change.
Users are leaving. Your PM says fix it without adding pagination — they want
all products visible. You need a different solution.

🔷 **Types used in this scenario:**

```typescript
interface Product {
  id: string;
  name: string;
  price: number;
  category: string;
  imageUrl: string;
  rating: number;
  stock: number;
}

interface VirtualListProps {
  items: Product[];
  itemHeight: number;
  containerHeight: number;
  renderItem: (item: Product, index: number) => JSX.Element;
}
```

**Here is the current code:**

```tsx
import { useState, useEffect, useMemo } from 'react';

interface Product {
  id: string;
  name: string;
  price: number;
  category: string;
  imageUrl: string;
  rating: number;
  stock: number;
}

function ProductCatalog(): JSX.Element {
  const [products, setProducts] = useState<Product[]>([]);
  const [searchTerm, setSearchTerm] = useState<string>('');
  const [isLoading, setIsLoading] = useState<boolean>(true);

  useEffect(() => {
    const load = async (): Promise<void> => {
      const res = await fetch('/api/products?limit=5000');
      const data = await res.json() as Product[];
      setProducts(data);
      setIsLoading(false);
    };
    load();
  }, []);

  // Filtering 5000 products on every render
  const filtered = products.filter(p =>
    p.name.toLowerCase().includes(searchTerm.toLowerCase())
  );

  if (isLoading) return <p>Loading products...</p>;

  return (
    <div>
      <input
        value={searchTerm}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setSearchTerm(e.target.value)
        }
        placeholder="Search products..."
      />
      <p>{filtered.length} products</p>
      {/* Renders ALL 5000 DOM nodes at once — this is the freeze */}
      {filtered.map(product => (
        <div key={product.id} style={{ height: 80, padding: 16, borderBottom: '1px solid #eee' }}>
          <h3>{product.name}</h3>
          <p>₹{product.price} — {product.category}</p>
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the 5000 DOM nodes problem using a virtual list
- ✅ Wrap the filter in `useMemo` so it does not recompute on every render
- ✅ Build a simple typed `VirtualList` component from scratch
- ✅ Keep search working with the virtual list

<details>
<summary>💡 Hint — How to think about this</summary>

There are two separate problems:
1. Filtering 5000 items on every render (even unrelated ones) — fix with `useMemo`
2. Rendering 5000 DOM nodes — fix with virtualization

Virtualization means: only render what is visible on screen.
If your container is 600px tall and each item is 80px,
only 8 items are visible. Render only those 8 + a few extra for scroll.
Use `scrollTop` to know which items are in the visible window.
The total height of the list is maintained with a spacer element
so scrollbar position stays correct.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useMemo, useRef, useCallback } from 'react';

// ─── Types ────────────────────────────────────────────────
interface Product {
  id: string;
  name: string;
  price: number;
  category: string;
  imageUrl: string;
  rating: number;
  stock: number;
}

interface VirtualListProps<T> {
  items: T[];
  itemHeight: number;        // fixed height of each item in px
  containerHeight: number;   // height of the scroll container in px
  renderItem: (item: T, index: number) => JSX.Element;
  overscan?: number;         // extra items to render above/below visible area
}

// ─── Virtual List Component ───────────────────────────────
// Generic T means this works for any item type
function VirtualList<T extends { id: string }>({
  items,
  itemHeight,
  containerHeight,
  renderItem,
  overscan = 3,
}: VirtualListProps<T>): JSX.Element {

  const [scrollTop, setScrollTop] = useState<number>(0);
  const containerRef = useRef<HTMLDivElement>(null);

  // Calculate which items are visible based on scroll position
  const visibleStart = Math.max(
    0,
    Math.floor(scrollTop / itemHeight) - overscan
  );
  const visibleEnd = Math.min(
    items.length - 1,
    Math.ceil((scrollTop + containerHeight) / itemHeight) + overscan
  );

  // Only the visible items (+ overscan buffer)
  const visibleItems = items.slice(visibleStart, visibleEnd + 1);

  const handleScroll = useCallback((e: React.UIEvent<HTMLDivElement>): void => {
    setScrollTop(e.currentTarget.scrollTop);
  }, []);

  const totalHeight = items.length * itemHeight;
  const offsetTop = visibleStart * itemHeight;

  return (
    // Scroll container — fixed height, overflow scroll
    <div
      ref={containerRef}
      onScroll={handleScroll}
      style={{
        height: containerHeight,
        overflowY: 'auto',
        position: 'relative',
      }}
    >
      {/* Spacer that gives the scrollbar its full height */}
      <div style={{ height: totalHeight, position: 'relative' }}>
        {/* Visible items positioned absolutely within the spacer */}
        <div style={{ position: 'absolute', top: offsetTop, width: '100%' }}>
          {visibleItems.map((item, index) =>
            renderItem(item, visibleStart + index)
          )}
        </div>
      </div>
    </div>
  );
}

// ─── Main Component ───────────────────────────────────────
function ProductCatalog(): JSX.Element {
  const [products, setProducts] = useState<Product[]>([]);
  const [searchTerm, setSearchTerm] = useState<string>('');
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const load = async (): Promise<void> => {
      try {
        const res = await fetch('/api/products?limit=5000');
        if (!res.ok) throw new Error('Failed to load products');
        const data = await res.json() as Product[];
        setProducts(data);
      } catch (err: unknown) {
        setError(err instanceof Error ? err.message : 'Failed to load');
      } finally {
        setIsLoading(false);
      }
    };
    load();
  }, []);

  // useMemo: only recompute filtered list when products or searchTerm changes
  // Without this: 5000-item filter runs on EVERY render
  const filteredProducts = useMemo<Product[]>(() => {
    const term = searchTerm.toLowerCase().trim();
    if (!term) return products;
    return products.filter(p =>
      p.name.toLowerCase().includes(term) ||
      p.category.toLowerCase().includes(term)
    );
  }, [products, searchTerm]);

  // renderItem is stable with useCallback — prevents VirtualList re-renders
  const renderProduct = useCallback((product: Product): JSX.Element => (
    <div
      key={product.id}
      style={{
        height: 80,
        padding: '12px 16px',
        borderBottom: '1px solid #eee',
        display: 'flex',
        alignItems: 'center',
        gap: 12,
        boxSizing: 'border-box',
      }}
    >
      <div style={{ flex: 1 }}>
        <h3 style={{ margin: 0, fontSize: 16 }}>{product.name}</h3>
        <p style={{ margin: 0, color: '#666', fontSize: 14 }}>
          ₹{product.price.toLocaleString()} — {product.category}
        </p>
      </div>
      <div style={{ textAlign: 'right' }}>
        <span style={{ color: product.stock > 0 ? 'green' : 'red', fontSize: 12 }}>
          {product.stock > 0 ? `${product.stock} in stock` : 'Out of stock'}
        </span>
      </div>
    </div>
  ), []);

  if (isLoading) return <p>Loading {5000} products...</p>;
  if (error) return <p style={{ color: 'red' }}>Error: {error}</p>;

  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setSearchTerm(e.target.value)
        }
        placeholder="Search products..."
        style={{ marginBottom: 16, padding: '8px 12px', width: '100%' }}
      />

      <p style={{ marginBottom: 8, color: '#666' }}>
        Showing {filteredProducts.length} of {products.length} products
        {/* Only ~8-12 DOM nodes rendered at any time — not 5000 */}
      </p>

      <VirtualList<Product>
        items={filteredProducts}
        itemHeight={80}
        containerHeight={600}
        renderItem={renderProduct}
        overscan={5}
      />
    </div>
  );
}
```

**✅ What the interviewer is checking:**
They want to see that you know the problem is DOM node count, not data size.
The virtual list solution — only rendering visible items — is the production
answer. They also check for `useMemo` on the filter so it does not recompute
on every unrelated render.

🔷 **TypeScript lesson:**
`VirtualList<T extends { id: string }>` uses a generic with a constraint.
`T extends { id: string }` means: T can be any type, as long as it has
an `id` field. This makes the component reusable while ensuring type safety.
This is a core TypeScript pattern for reusable components.

⚡ **Why this matters in production:**
Rendering 5000 DOM nodes at once: browser freezes 3-4 seconds, high memory use,
scroll is janky. With virtualization: browser renders 12 nodes, load is instant,
scroll is smooth. The data size is the same — only the rendered nodes change.

🔍 **What a senior dev spots in 10 seconds:**
`{filtered.map(product => <div>)}` rendering thousands of items with no
virtualization. The fix is always the same: virtual list or windowing library
(react-window, react-virtual).

✅ **What the interviewer is checking:**
Understanding WHY the browser freezes (DOM node count, layout/paint cost)
and the correct solution (virtual list). useMemo on the filter as a secondary
optimization.

🚀 **Before you ship this, verify:**
- [ ] Does the list scroll smoothly with 5000+ items?
- [ ] Does search filter correctly with the virtual list?
- [ ] Is each item's height consistent (virtual lists require fixed heights)?
- [ ] Is the `useMemo` dependency array correct?
- [ ] Did you test on a low-end device, not just your fast laptop?

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 13: 📡 Real-time / Polling

> **Level 3 — Advanced**

🧠 **The Mental Model:**
Real-time means the server tells the client something changed —
the client does not have to ask. Before reaching for WebSocket,
ask: does this truly need to be instant? If 30-second-old data
is acceptable, polling with `setInterval` is 10x simpler.
If you need instant push, WebSocket or SSE is the answer.

⚡ **The One Rule:**
Every connection you open must be closed on unmount —
every `setInterval`, every WebSocket, every EventSource.
Forgetting this is the most common memory leak in React.

🔷 **TypeScript focus in this pattern:**
Typed WebSocket message events with discriminated union message types.
Typed custom hooks for real-time connections with cleanup.
`useRef<WebSocket | null>` for stable connection references.

🚨 **What junior devs always get wrong:**
Opening a WebSocket connection inside `useState` initializer or
without a ref — creating multiple connections. Not cleaning up
on unmount. Not handling reconnection when the connection drops.

📍 **You need this pattern when:**
- Order status needs to update without the user refreshing
- A chat window needs to receive new messages instantly
- A dashboard needs live metrics that change every few seconds

📋 **Interview asks this as:**
- "Build a component that shows live order status updates"
- "Implement a notification bell that receives real-time push notifications"
- "How would you handle a WebSocket connection in React?"

📦 **Production-ready version always includes:**
- Cleanup on unmount — close connection, clear interval
- Reconnection logic when connection drops
- Loading state while connection establishes
- Error state if connection cannot be made

---

### 🔴 Pattern 13 · Scenario 1 — The Live Order Tracker That Never Updates

🏢 **Situation:**
You are at a food delivery app. The order tracking page shows the
order status — "Preparing", "Out for delivery", "Delivered". But
the status never updates. Users have to manually refresh to see
the latest status. Your backend team already built a polling endpoint
at `/api/orders/:id/status`. You need to wire it up correctly.

🔷 **Types used in this scenario:**

```typescript
type OrderStatus =
  | 'pending'
  | 'confirmed'
  | 'preparing'
  | 'out_for_delivery'
  | 'delivered'
  | 'cancelled';

interface OrderStatusResponse {
  orderId: string;
  status: OrderStatus;
  updatedAt: string;
  estimatedDelivery: string | null;
  driverName: string | null;
  driverPhone: string | null;
}
```

**Here is the current code:**

```tsx
import { useState, useEffect } from 'react';

type OrderStatus =
  | 'pending'
  | 'confirmed'
  | 'preparing'
  | 'out_for_delivery'
  | 'delivered'
  | 'cancelled';

interface OrderStatusResponse {
  orderId: string;
  status: OrderStatus;
  updatedAt: string;
  estimatedDelivery: string | null;
  driverName: string | null;
  driverPhone: string | null;
}

interface OrderTrackerProps {
  orderId: string;
}

function OrderTracker({ orderId }: OrderTrackerProps): JSX.Element {
  const [orderStatus, setOrderStatus] = useState<OrderStatusResponse | null>(null);

  useEffect(() => {
    // Only fetches once — never polls
    fetch(`/api/orders/${orderId}/status`)
      .then(r => r.json())
      .then((data: OrderStatusResponse) => setOrderStatus(data));
  }, [orderId]);

  if (!orderStatus) return <p>Loading...</p>;

  return (
    <div>
      <h2>Order #{orderStatus.orderId}</h2>
      <p>Status: {orderStatus.status}</p>
    </div>
  );
}
```

**Your tasks:**
- ✅ Add polling every 15 seconds to keep status updated
- ✅ Stop polling automatically when order is "delivered" or "cancelled"
- ✅ Show a "last updated" timestamp
- ✅ Add a manual refresh button
- ✅ Clean up the interval properly on unmount

<details>
<summary>💡 Hint — How to think about this</summary>

Extract the fetch into a `useCallback` function so both the
interval and the manual refresh button can call the same function.
For stopping when delivered: check the status inside the interval
callback — if delivered, `clearInterval`. Or put the status in
the `useEffect` dependency array and conditionally set up the interval.
The key rule: always return a cleanup function from `useEffect`
that calls `clearInterval`.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useCallback, useRef } from 'react';

// ─── Types ────────────────────────────────────────────────
type OrderStatus =
  | 'pending'
  | 'confirmed'
  | 'preparing'
  | 'out_for_delivery'
  | 'delivered'
  | 'cancelled';

interface OrderStatusResponse {
  orderId: string;
  status: OrderStatus;
  updatedAt: string;
  estimatedDelivery: string | null;
  driverName: string | null;
  driverPhone: string | null;
}

interface OrderTrackerProps {
  orderId: string;
}

// Orders that no longer need polling
const TERMINAL_STATUSES: OrderStatus[] = ['delivered', 'cancelled'];

const STATUS_LABELS: Record<OrderStatus, string> = {
  pending: '⏳ Order Placed',
  confirmed: '✅ Confirmed',
  preparing: '👨‍🍳 Being Prepared',
  out_for_delivery: '🚴 Out for Delivery',
  delivered: '📦 Delivered',
  cancelled: '❌ Cancelled',
};

// ─── Component ────────────────────────────────────────────
function OrderTracker({ orderId }: OrderTrackerProps): JSX.Element {
  const [orderStatus, setOrderStatus] = useState<OrderStatusResponse | null>(null);
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);
  const [lastRefreshed, setLastRefreshed] = useState<Date | null>(null);

  // useCallback: stable reference so useEffect deps don't change unnecessarily
  const fetchStatus = useCallback(async (): Promise<void> => {
    try {
      setError(null);
      const res = await fetch(`/api/orders/${orderId}/status`);
      if (!res.ok) throw new Error(`Failed to fetch: ${res.status}`);
      const data = await res.json() as OrderStatusResponse;
      setOrderStatus(data);
      setLastRefreshed(new Date());
    } catch (err: unknown) {
      setError(err instanceof Error ? err.message : 'Failed to load status');
    } finally {
      setIsLoading(false);
    }
  }, [orderId]);

  // Initial fetch
  useEffect(() => {
    fetchStatus();
  }, [fetchStatus]);

  // Polling — stops when order reaches terminal status
  useEffect(() => {
    // Do not poll if already in terminal status
    if (orderStatus && TERMINAL_STATUSES.includes(orderStatus.status)) {
      return;
    }

    // Poll every 15 seconds
    const intervalId = setInterval(() => {
      fetchStatus();
    }, 15_000);

    // CRITICAL: cleanup — stop polling on unmount or status change
    return () => clearInterval(intervalId);

  }, [fetchStatus, orderStatus?.status]); // re-evaluate when status changes

  if (isLoading && !orderStatus) return <p>Loading order status...</p>;
  if (error && !orderStatus) return <p style={{ color: 'red' }}>Error: {error}</p>;
  if (!orderStatus) return <p>Order not found.</p>;

  const isTerminal = TERMINAL_STATUSES.includes(orderStatus.status);

  return (
    <div style={{ padding: 24, border: '1px solid #eee', borderRadius: 8 }}>
      <h2>Order #{orderStatus.orderId}</h2>

      {/* Status display */}
      <p style={{ fontSize: 20, fontWeight: 'bold' }}>
        {STATUS_LABELS[orderStatus.status]}
      </p>

      {/* Driver info — only when out for delivery */}
      {orderStatus.status === 'out_for_delivery' && orderStatus.driverName && (
        <div style={{ background: '#f0f9ff', padding: 12, borderRadius: 6 }}>
          <p>🚴 Driver: {orderStatus.driverName}</p>
          {orderStatus.driverPhone && (
            <a href={`tel:${orderStatus.driverPhone}`}>
              Call {orderStatus.driverPhone}
            </a>
          )}
        </div>
      )}

      {/* ETA */}
      {orderStatus.estimatedDelivery && !isTerminal && (
        <p>Estimated delivery: {new Date(orderStatus.estimatedDelivery).toLocaleTimeString()}</p>
      )}

      {/* Last updated + manual refresh */}
      <div style={{ display: 'flex', alignItems: 'center', gap: 12, marginTop: 16 }}>
        {lastRefreshed && (
          <span style={{ color: '#999', fontSize: 12 }}>
            Last updated: {lastRefreshed.toLocaleTimeString()}
          </span>
        )}

        {/* Hide refresh button once terminal — no need to refresh */}
        {!isTerminal && (
          <button
            onClick={fetchStatus}
            disabled={isLoading}
            style={{ fontSize: 12 }}
          >
            {isLoading ? 'Refreshing...' : '↻ Refresh'}
          </button>
        )}
      </div>

      {/* Polling indicator */}
      {!isTerminal && (
        <p style={{ color: '#999', fontSize: 11, marginTop: 8 }}>
          Auto-refreshing every 15 seconds
        </p>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The interval cleanup in `useEffect` return function — this is the most
important line. They also check that polling stops when the order reaches
a terminal status, and that manual refresh and auto-refresh use the same
`fetchStatus` function.

🔷 **TypeScript lesson:**
`Record<OrderStatus, string>` types a lookup object where every key is
an `OrderStatus` and every value is a string. If you add a new status
to the union and forget to add it to `STATUS_LABELS`, TypeScript will
error — this is types preventing a runtime bug at compile time.

⚡ **Why this matters in production:**
Without cleanup: the interval runs after the user navigates away,
making API calls to a component that no longer exists. This causes
`setState` on unmounted component warnings and wastes server resources.

🔍 **What a senior dev spots in 10 seconds:**
`setInterval` or `fetch` inside `useEffect` without a `return () => clearInterval()`
cleanup. This is always a memory leak waiting to happen.

✅ **What the interviewer is checking:**
`return () => clearInterval(intervalId)` is the single most important
line in this scenario. Without it, the answer is wrong.

🚀 **Before you ship this, verify:**
- [ ] Does polling stop when the order is delivered or cancelled?
- [ ] Does the interval clean up when the user navigates away?
- [ ] Does manual refresh work alongside auto-refresh?
- [ ] Is the "last updated" time accurate?
- [ ] Does it handle API errors without breaking the polling loop?

</details>

---

### 🔴 Pattern 13 · Scenario 2 — The Notification Bell That Leaks Memory

🏢 **Situation:**
You are at a SaaS platform. The notification bell in the header
connects to a WebSocket server to receive real-time notifications.
Your on-call engineer reports that after users browse around the app
for 20-30 minutes, memory usage climbs to 800MB and the app becomes
sluggish. A heap snapshot shows thousands of WebSocket event listeners
piling up. You trace it to the notification component.

🔷 **Types used in this scenario:**

```typescript
type NotificationType = 'info' | 'success' | 'warning' | 'error';

interface Notification {
  id: string;
  type: NotificationType;
  title: string;
  message: string;
  createdAt: string;
  isRead: boolean;
}

type WebSocketMessage =
  | { event: 'notification'; payload: Notification }
  | { event: 'notification_read'; payload: { id: string } }
  | { event: 'ping' };
```

**Here is the current code:**

```tsx
import { useState, useEffect } from 'react';

type NotificationType = 'info' | 'success' | 'warning' | 'error';

interface Notification {
  id: string;
  type: NotificationType;
  title: string;
  message: string;
  createdAt: string;
  isRead: boolean;
}

interface NotificationBellProps {
  userId: string;
}

function NotificationBell({ userId }: NotificationBellProps): JSX.Element {
  const [notifications, setNotifications] = useState<Notification[]>([]);
  const [isOpen, setIsOpen] = useState<boolean>(false);

  useEffect(() => {
    // BUG 1: new WebSocket created on every render
    // BUG 2: no cleanup — connection never closes
    // BUG 3: event listener stacks up on every effect run
    const ws = new WebSocket(`wss://api.example.com/notifications?userId=${userId}`);

    ws.onmessage = (event: MessageEvent) => {
      const message = JSON.parse(event.data as string);
      if (message.event === 'notification') {
        setNotifications(prev => [message.payload, ...prev]);
      }
    };

    // No return cleanup function here — this is the leak
  }, [userId]); // re-runs when userId changes, stacking up connections

  const unreadCount = notifications.filter(n => !n.isRead).length;

  return (
    <div>
      <button onClick={() => setIsOpen(!isOpen)}>
        🔔 {unreadCount > 0 && <span>{unreadCount}</span>}
      </button>
      {isOpen && (
        <div>
          {notifications.length === 0 ? (
            <p>No notifications</p>
          ) : (
            notifications.map(n => (
              <div key={n.id}>
                <strong>{n.title}</strong>
                <p>{n.message}</p>
              </div>
            ))
          )}
        </div>
      )}
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the memory leak — close the WebSocket on unmount
- ✅ Use `useRef` to hold the WebSocket instance (not state)
- ✅ Handle the discriminated union message type safely
- ✅ Add connection status indicator
- ✅ Add basic reconnection when connection drops

<details>
<summary>💡 Hint — How to think about this</summary>

The WebSocket must be stored in a `useRef`, not created inline in the effect.
This gives you a stable reference to close it in the cleanup function.
The cleanup function `return () => ws.close()` is the fix for the leak.
For the discriminated union message type — use a switch statement on
`message.event` to get full type narrowing on each branch.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useRef, useCallback } from 'react';

// ─── Types ────────────────────────────────────────────────
type NotificationType = 'info' | 'success' | 'warning' | 'error';

interface Notification {
  id: string;
  type: NotificationType;
  title: string;
  message: string;
  createdAt: string;
  isRead: boolean;
}

// Discriminated union — each event type has its own payload shape
type WebSocketMessage =
  | { event: 'notification'; payload: Notification }
  | { event: 'notification_read'; payload: { id: string } }
  | { event: 'ping' };

type ConnectionStatus = 'connecting' | 'connected' | 'disconnected' | 'error';

interface NotificationBellProps {
  userId: string;
}

// ─── Component ────────────────────────────────────────────
function NotificationBell({ userId }: NotificationBellProps): JSX.Element {
  const [notifications, setNotifications] = useState<Notification[]>([]);
  const [isOpen, setIsOpen] = useState<boolean>(false);
  const [connectionStatus, setConnectionStatus] = useState<ConnectionStatus>('connecting');

  // useRef holds the WebSocket instance — does not cause re-renders
  const wsRef = useRef<WebSocket | null>(null);
  // Track if component is still mounted — prevents setState after unmount
  const isMountedRef = useRef<boolean>(true);

  const connect = useCallback((): void => {
    // Close existing connection before creating a new one
    if (wsRef.current) {
      wsRef.current.close();
    }

    setConnectionStatus('connecting');

    const ws = new WebSocket(
      `wss://api.example.com/notifications?userId=${userId}`
    );

    ws.onopen = (): void => {
      if (isMountedRef.current) setConnectionStatus('connected');
    };

    ws.onmessage = (event: MessageEvent<string>): void => {
      if (!isMountedRef.current) return;

      try {
        const message = JSON.parse(event.data) as WebSocketMessage;

        // Discriminated union switch — TypeScript narrows payload type per case
        switch (message.event) {
          case 'notification':
            // message.payload is Notification here
            setNotifications(prev => [message.payload, ...prev.slice(0, 49)]);
            break;

          case 'notification_read':
            // message.payload is { id: string } here
            setNotifications(prev =>
              prev.map(n =>
                n.id === message.payload.id ? { ...n, isRead: true } : n
              )
            );
            break;

          case 'ping':
            // Keepalive — no UI update needed
            break;
        }
      } catch {
        console.warn('Failed to parse WebSocket message');
      }
    };

    ws.onerror = (): void => {
      if (isMountedRef.current) setConnectionStatus('error');
    };

    ws.onclose = (): void => {
      if (!isMountedRef.current) return;
      setConnectionStatus('disconnected');

      // Reconnect after 5 seconds if component is still mounted
      setTimeout(() => {
        if (isMountedRef.current) connect();
      }, 5000);
    };

    // Store reference for cleanup
    wsRef.current = ws;
  }, [userId]);

  useEffect(() => {
    isMountedRef.current = true;
    connect();

    // CRITICAL CLEANUP — prevents the memory leak
    return (): void => {
      isMountedRef.current = false;

      if (wsRef.current) {
        // Remove onclose handler to prevent reconnection after unmount
        wsRef.current.onclose = null;
        wsRef.current.close();
        wsRef.current = null;
      }
    };
  }, [connect]);

  const markAllRead = (): void => {
    setNotifications(prev => prev.map(n => ({ ...n, isRead: true })));
  };

  const unreadCount = notifications.filter(n => !n.isRead).length;

  const statusColors: Record<ConnectionStatus, string> = {
    connecting: '#f59e0b',
    connected: '#10b981',
    disconnected: '#6b7280',
    error: '#ef4444',
  };

  return (
    <div style={{ position: 'relative' }}>
      <button
        onClick={() => setIsOpen(prev => !prev)}
        style={{ position: 'relative', background: 'none', border: 'none', cursor: 'pointer', fontSize: 24 }}
      >
        🔔
        {unreadCount > 0 && (
          <span style={{
            position: 'absolute', top: -4, right: -4,
            background: 'red', color: 'white',
            borderRadius: '50%', width: 18, height: 18,
            fontSize: 11, display: 'flex', alignItems: 'center',
            justifyContent: 'center',
          }}>
            {unreadCount > 9 ? '9+' : unreadCount}
          </span>
        )}
      </button>

      {/* Connection status dot */}
      <span style={{
        position: 'absolute', bottom: 0, right: 0,
        width: 8, height: 8, borderRadius: '50%',
        background: statusColors[connectionStatus],
      }}
        title={`WebSocket: ${connectionStatus}`}
      />

      {isOpen && (
        <div style={{
          position: 'absolute', right: 0, top: '100%',
          width: 320, background: 'white',
          border: '1px solid #e5e7eb', borderRadius: 8,
          boxShadow: '0 4px 16px rgba(0,0,0,0.1)',
          zIndex: 1000,
        }}>
          <div style={{
            padding: '12px 16px',
            borderBottom: '1px solid #e5e7eb',
            display: 'flex',
            justifyContent: 'space-between',
            alignItems: 'center',
          }}>
            <h3 style={{ margin: 0, fontSize: 16 }}>Notifications</h3>
            {unreadCount > 0 && (
              <button onClick={markAllRead} style={{ fontSize: 12, color: '#6366f1' }}>
                Mark all read
              </button>
            )}
          </div>

          <div style={{ maxHeight: 400, overflowY: 'auto' }}>
            {notifications.length === 0 ? (
              <p style={{ padding: 16, color: '#9ca3af', textAlign: 'center' }}>
                No notifications yet
              </p>
            ) : (
              notifications.map(n => (
                <div
                  key={n.id}
                  style={{
                    padding: '12px 16px',
                    borderBottom: '1px solid #f3f4f6',
                    background: n.isRead ? 'white' : '#f0f9ff',
                  }}
                >
                  <strong style={{ fontSize: 14 }}>{n.title}</strong>
                  <p style={{ margin: '4px 0 0', fontSize: 13, color: '#6b7280' }}>
                    {n.message}
                  </p>
                  <span style={{ fontSize: 11, color: '#9ca3af' }}>
                    {new Date(n.createdAt).toLocaleTimeString()}
                  </span>
                </div>
              ))
            )}
          </div>
        </div>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`wsRef.current.onclose = null` before `ws.close()` — this is the subtle
detail that prevents reconnection after unmount. `isMountedRef` prevents
`setState` on an unmounted component. The discriminated union switch shows
TypeScript-level message handling.

🔷 **TypeScript lesson:**
The discriminated union `WebSocketMessage` type means that inside
`case 'notification'`, TypeScript knows `message.payload` is `Notification`.
Inside `case 'notification_read'`, it knows it is `{ id: string }`.
This is type narrowing — TypeScript uses the `event` discriminant to
narrow the type automatically. No type casting needed.

⚡ **Why this matters in production:**
Without cleanup: each route change creates a new WebSocket and the old
one is never closed. After 20 routes, 20 connections are open, each
receiving messages and calling `setState` on ghost components.
Memory grows, the app slows, and eventually crashes.

🔍 **What a senior dev spots in 10 seconds:**
`new WebSocket()` inside `useEffect` without `return () => ws.close()`.
This is always a memory leak — no exceptions.

🚀 **Before you ship this, verify:**
- [ ] Does the WebSocket close when the component unmounts?
- [ ] Does it NOT reconnect after unmount?
- [ ] Does reconnection happen correctly after a real disconnect?
- [ ] Is `setState` guarded by `isMountedRef` to prevent warnings?
- [ ] Does the discriminated union handle all message types exhaustively?

</details>

---

### 🔴 Pattern 13 · Scenario 3 — The Chat That Misses Messages

🏢 **Situation:**
You are building an internal chat tool for a company. The chat window
uses a custom hook to connect to a WebSocket. The connection works —
messages arrive. But users report that sometimes messages are missing:
they can see "Alice: hey" but the next message from Alice never appears.
After debugging, you discover messages ARE arriving at the WebSocket —
but the state is not updating correctly. It is a stale closure bug
inside the WebSocket handler.

🔷 **Types used in this scenario:**

```typescript
interface ChatMessage {
  id: string;
  senderId: string;
  senderName: string;
  content: string;
  timestamp: string;
}

interface UseChatReturn {
  messages: ChatMessage[];
  connectionStatus: 'connecting' | 'connected' | 'disconnected';
  sendMessage: (content: string) => void;
}
```

**Here is the current code:**

```tsx
import { useState, useEffect, useRef } from 'react';

interface ChatMessage {
  id: string;
  senderId: string;
  senderName: string;
  content: string;
  timestamp: string;
}

function useChat(roomId: string) {
  const [messages, setMessages] = useState<ChatMessage[]>([]);
  const wsRef = useRef<WebSocket | null>(null);

  useEffect(() => {
    const ws = new WebSocket(`wss://chat.example.com/rooms/${roomId}`);

    ws.onmessage = (event: MessageEvent<string>) => {
      const newMessage = JSON.parse(event.data) as ChatMessage;

      // BUG: stale closure — messages captured at the time
      // this handler was created, never updated
      setMessages([...messages, newMessage]);
    };

    wsRef.current = ws;

    return () => {
      ws.close();
    };
  }, [roomId]); // messages not in deps — causes stale closure

  const sendMessage = (content: string): void => {
    if (wsRef.current?.readyState === WebSocket.OPEN) {
      wsRef.current.send(JSON.stringify({ type: 'message', content }));
    }
  };

  return { messages, sendMessage };
}
```

**Your tasks:**
- ✅ Fix the stale closure — messages must always show all history
- ✅ Add connection status tracking
- ✅ Add a proper typed return interface for the hook
- ✅ Make the hook handle room changes without stacking up connections

<details>
<summary>💡 Hint — How to think about this</summary>

The bug is `setMessages([...messages, newMessage])` — `messages`
is captured when the `onmessage` handler is created (when the
WebSocket opens) and never updates. If `messages` starts as `[]`,
it is always `[]` inside this handler no matter how many messages arrive.
The fix is the functional updater form: `setMessages(prev => [...prev, newMessage])`.
`prev` always gives you the current state, not the stale closure value.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useRef, useCallback } from 'react';

// ─── Types ────────────────────────────────────────────────
interface ChatMessage {
  id: string;
  senderId: string;
  senderName: string;
  content: string;
  timestamp: string;
}

type ConnectionStatus = 'connecting' | 'connected' | 'disconnected';

// Explicit return type for the hook — documents the contract
interface UseChatReturn {
  messages: ChatMessage[];
  connectionStatus: ConnectionStatus;
  sendMessage: (content: string) => void;
  clearMessages: () => void;
}

// Incoming WebSocket message types
type IncomingMessage =
  | { type: 'message'; payload: ChatMessage }
  | { type: 'history'; payload: ChatMessage[] }
  | { type: 'user_joined'; payload: { userId: string; name: string } };

// ─── Hook ─────────────────────────────────────────────────
function useChat(roomId: string): UseChatReturn {
  const [messages, setMessages] = useState<ChatMessage[]>([]);
  const [connectionStatus, setConnectionStatus] = useState<ConnectionStatus>('connecting');
  const wsRef = useRef<WebSocket | null>(null);
  const isMountedRef = useRef<boolean>(true);

  useEffect(() => {
    isMountedRef.current = true;

    // Close any existing connection before opening new one
    // (handles room changes — roomId in dependency array)
    if (wsRef.current) {
      wsRef.current.close();
    }

    // Clear messages when switching rooms
    setMessages([]);
    setConnectionStatus('connecting');

    const ws = new WebSocket(`wss://chat.example.com/rooms/${roomId}`);

    ws.onopen = (): void => {
      if (isMountedRef.current) setConnectionStatus('connected');
    };

    ws.onmessage = (event: MessageEvent<string>): void => {
      if (!isMountedRef.current) return;

      try {
        const incoming = JSON.parse(event.data) as IncomingMessage;

        switch (incoming.type) {
          case 'message':
            // FIX: functional updater — prev is always current state
            // Never stale because React provides it fresh each call
            setMessages(prev => [...prev, incoming.payload]);
            break;

          case 'history':
            // Replace messages with full history on connect
            setMessages(incoming.payload);
            break;

          case 'user_joined':
            // Could show a system message — not storing to keep example clean
            console.log(`${incoming.payload.name} joined the room`);
            break;
        }
      } catch {
        console.warn('Failed to parse chat message');
      }
    };

    ws.onclose = (): void => {
      if (isMountedRef.current) setConnectionStatus('disconnected');
    };

    wsRef.current = ws;

    return (): void => {
      isMountedRef.current = false;
      ws.close();
      wsRef.current = null;
    };
  }, [roomId]); // reconnects when room changes

  // sendMessage does NOT need messages in its closure
  const sendMessage = useCallback((content: string): void => {
    if (wsRef.current?.readyState === WebSocket.OPEN) {
      wsRef.current.send(
        JSON.stringify({ type: 'message', content })
      );
    }
  }, []); // no deps needed — wsRef.current is always current

  const clearMessages = useCallback((): void => {
    setMessages([]);
  }, []);

  return { messages, connectionStatus, sendMessage, clearMessages };
}

// ─── Chat Component using the hook ───────────────────────
interface ChatWindowProps {
  roomId: string;
  currentUserId: string;
}

function ChatWindow({ roomId, currentUserId }: ChatWindowProps): JSX.Element {
  const { messages, connectionStatus, sendMessage } = useChat(roomId);
  const [input, setInput] = useState<string>('');
  const bottomRef = useRef<HTMLDivElement>(null);

  // Auto-scroll to bottom on new messages
  useEffect(() => {
    bottomRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages]);

  const handleSend = (): void => {
    if (!input.trim()) return;
    sendMessage(input.trim());
    setInput('');
  };

  const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>): void => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      handleSend();
    }
  };

  const statusColors: Record<ConnectionStatus, string> = {
    connecting: '#f59e0b',
    connected: '#10b981',
    disconnected: '#ef4444',
  };

  return (
    <div style={{ display: 'flex', flexDirection: 'column', height: 500 }}>
      {/* Status bar */}
      <div style={{
        padding: '8px 16px',
        background: '#f9fafb',
        borderBottom: '1px solid #e5e7eb',
        display: 'flex',
        alignItems: 'center',
        gap: 8,
      }}>
        <span style={{
          width: 8, height: 8, borderRadius: '50%',
          background: statusColors[connectionStatus],
          display: 'inline-block',
        }} />
        <span style={{ fontSize: 13, color: '#6b7280', textTransform: 'capitalize' }}>
          {connectionStatus}
        </span>
      </div>

      {/* Messages */}
      <div style={{ flex: 1, overflowY: 'auto', padding: 16 }}>
        {messages.map(msg => (
          <div
            key={msg.id}
            style={{
              marginBottom: 12,
              display: 'flex',
              flexDirection: 'column',
              alignItems: msg.senderId === currentUserId ? 'flex-end' : 'flex-start',
            }}
          >
            <span style={{ fontSize: 11, color: '#9ca3af', marginBottom: 2 }}>
              {msg.senderName}
            </span>
            <div style={{
              maxWidth: '70%',
              padding: '8px 12px',
              borderRadius: 12,
              background: msg.senderId === currentUserId ? '#6366f1' : '#f3f4f6',
              color: msg.senderId === currentUserId ? 'white' : '#111',
            }}>
              {msg.content}
            </div>
          </div>
        ))}
        <div ref={bottomRef} />
      </div>

      {/* Input */}
      <div style={{
        padding: 12,
        borderTop: '1px solid #e5e7eb',
        display: 'flex',
        gap: 8,
      }}>
        <input
          value={input}
          onChange={(e: React.ChangeEvent<HTMLInputElement>) => setInput(e.target.value)}
          onKeyDown={handleKeyDown}
          placeholder="Type a message..."
          disabled={connectionStatus !== 'connected'}
          style={{ flex: 1, padding: '8px 12px', borderRadius: 6, border: '1px solid #d1d5db' }}
        />
        <button
          onClick={handleSend}
          disabled={!input.trim() || connectionStatus !== 'connected'}
          style={{ padding: '8px 16px', background: '#6366f1', color: 'white', border: 'none', borderRadius: 6 }}
        >
          Send
        </button>
      </div>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The single line `setMessages(prev => [...prev, incoming.payload])` —
the functional updater form. They want you to explain WHY: the `onmessage`
handler closes over the initial empty `messages` array and never sees
updates unless you use the updater form which gets fresh state from React.

🔷 **TypeScript lesson:**
The `UseChatReturn` interface explicitly types the hook's return value.
This is a production practice — it documents what the hook provides,
makes the return type visible in IDE autocomplete, and catches any
mismatch between what the hook returns and what it promises.

⚡ **Why this matters in production:**
The stale closure bug is invisible in tests with mock WebSockets that
deliver all messages at once. It only appears in production when messages
arrive over time. Users see the first message, then nothing — as if the
WebSocket stopped working.

🔍 **What a senior dev spots in 10 seconds:**
`setMessages([...messages, newMessage])` with `messages` NOT in the
useEffect dependency array. This is the stale closure pattern — the
handler captures `messages` at creation time and never updates.

🚀 **Before you ship this, verify:**
- [ ] Do messages accumulate correctly (not reset to the first message)?
- [ ] Does the connection close cleanly when switching rooms?
- [ ] Does the component handle room changes without stacking connections?
- [ ] Is `sendMessage` stable (wrapped in useCallback)?
- [ ] Is the stale closure test: can you send 10 messages and see all 10?

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 14: 🔄 Complex Async

> **Level 3 — Advanced**

🧠 **The Mental Model:**
Async code has three failure modes that demos never show:
race conditions (two requests, wrong one wins), abandoned effects
(component unmounts mid-request), and dependent sequences
(request B needs result from request A). Each needs a different fix.

⚡ **The One Rule:**
Every async operation started inside a component must either
complete safely, be cancelled, or be ignored if the component
unmounts before it finishes.

🔷 **TypeScript focus in this pattern:**
Typed `AbortController` and `AbortSignal`.
Generic async state type: `AsyncState<T>` discriminated union.
Typed sequential fetch utilities with proper error handling.

🚨 **What junior devs always get wrong:**
Not handling the case where a component unmounts while a fetch is
in progress. React warns: "Can't perform a React state update on an
unmounted component." This is almost always an abandoned async operation.

📍 **You need this pattern when:**
- A search input fires multiple API calls and shows the wrong result
- A form submits and the user navigates away before it completes
- A dashboard loads data from three APIs in sequence

📋 **Interview asks this as:**
- "Your search shows results from the previous query — how do you fix it?"
- "How do you handle dependent API calls (user → then their orders)?"
- "What is a race condition in React and how do you prevent it?"

📦 **Production-ready version always includes:**
- `AbortController` to cancel stale requests
- Mounted check or cleanup before calling `setState` after async work
- Loading sequence for dependent requests (show partial data while loading)

---

### 🔴 Pattern 14 · Scenario 1 — The Search That Shows Wrong Results

🏢 **Situation:**
You are at a recruitment platform. The candidate search shows results
from the PREVIOUS query, not the current one. You type "React developer",
wait a moment, then type "Python developer". Sometimes the results shown
say "React developer results" — even though you searched for Python.
Your team calls this a race condition. You need to fix it.

🔷 **Types used in this scenario:**

```typescript
interface Candidate {
  id: string;
  name: string;
  skills: string[];
  experience: number;
  location: string;
}

interface SearchResult {
  candidates: Candidate[];
  total: number;
  query: string;
}
```

**Here is the current code:**

```tsx
import { useState, useEffect } from 'react';

interface Candidate {
  id: string;
  name: string;
  skills: string[];
  experience: number;
  location: string;
}

interface SearchResult {
  candidates: Candidate[];
  total: number;
  query: string;
}

function CandidateSearch(): JSX.Element {
  const [query, setQuery] = useState<string>('');
  const [results, setResults] = useState<SearchResult | null>(null);
  const [isLoading, setIsLoading] = useState<boolean>(false);

  useEffect(() => {
    if (!query.trim()) {
      setResults(null);
      return;
    }

    const search = async (): Promise<void> => {
      setIsLoading(true);
      // If two searches fire and the slow one resolves last —
      // it overwrites the fast one with stale results
      const res = await fetch(`/api/candidates/search?q=${encodeURIComponent(query)}`);
      const data = await res.json() as SearchResult;
      setResults(data); // BUG: might be from an old query
      setIsLoading(false);
    };

    search();
  }, [query]);

  return (
    <div>
      <input
        value={query}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) => setQuery(e.target.value)}
        placeholder="Search candidates..."
      />
      {isLoading && <p>Searching...</p>}
      {results && (
        <div>
          <p>{results.total} results for "{results.query}"</p>
          {results.candidates.map(c => (
            <div key={c.id}>
              <strong>{c.name}</strong>
              <p>{c.skills.join(', ')}</p>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the race condition using `AbortController`
- ✅ Add debounce so search does not fire on every keystroke
- ✅ Show which query the current results are for
- ✅ Handle the AbortError separately from real errors

<details>
<summary>💡 Hint — How to think about this</summary>

Race condition: query A fires, then query B fires. If A is slow and
resolves AFTER B, A's results overwrite B's results on screen.
Fix: when a new search starts, cancel the previous one.
`AbortController` gives you a signal to attach to `fetch`.
When you call `controller.abort()`, the fetch throws an `AbortError`.
Catch it separately — it is not a real error, it means "intentionally cancelled".

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

// ─── Types ────────────────────────────────────────────────
interface Candidate {
  id: string;
  name: string;
  skills: string[];
  experience: number;
  location: string;
}

interface SearchResult {
  candidates: Candidate[];
  total: number;
  query: string;
}

// Discriminated union for async state — cleaner than separate booleans
type SearchState =
  | { status: 'idle' }
  | { status: 'loading'; query: string }
  | { status: 'success'; data: SearchResult }
  | { status: 'error'; message: string; query: string };

// ─── Debounce Hook ────────────────────────────────────────
function useDebounce<T>(value: T, delayMs: number): T {
  const [debounced, setDebounced] = useState<T>(value);
  useEffect(() => {
    const timer = setTimeout(() => setDebounced(value), delayMs);
    return () => clearTimeout(timer);
  }, [value, delayMs]);
  return debounced;
}

// ─── Component ────────────────────────────────────────────
function CandidateSearch(): JSX.Element {
  const [query, setQuery] = useState<string>('');
  const [searchState, setSearchState] = useState<SearchState>({ status: 'idle' });

  // Debounce: only search 400ms after user stops typing
  const debouncedQuery = useDebounce<string>(query, 400);

  useEffect(() => {
    if (!debouncedQuery.trim()) {
      setSearchState({ status: 'idle' });
      return;
    }

    // Create new AbortController for this search
    const controller = new AbortController();

    const search = async (): Promise<void> => {
      setSearchState({ status: 'loading', query: debouncedQuery });

      try {
        const res = await fetch(
          `/api/candidates/search?q=${encodeURIComponent(debouncedQuery)}`,
          { signal: controller.signal } // attach signal to this fetch
        );

        if (!res.ok) {
          throw new Error(`Search failed: ${res.status}`);
        }

        const data = await res.json() as SearchResult;

        // Only set results if this request was not aborted
        setSearchState({ status: 'success', data });

      } catch (err: unknown) {
        // AbortError = intentionally cancelled — not a real error
        if (err instanceof Error && err.name === 'AbortError') {
          // Silently ignore — a newer search has taken over
          return;
        }

        setSearchState({
          status: 'error',
          message: err instanceof Error ? err.message : 'Search failed',
          query: debouncedQuery,
        });
      }
    };

    search();

    // Cleanup: abort when query changes or component unmounts
    // This is what prevents the race condition —
    // when a new search starts, the old fetch is cancelled
    return (): void => {
      controller.abort();
    };

  }, [debouncedQuery]);

  return (
    <div>
      <input
        type="text"
        value={query}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
          setQuery(e.target.value)
        }
        placeholder="Search candidates..."
        style={{ padding: '8px 12px', width: '100%', marginBottom: 16 }}
      />

      {/* Waiting for debounce */}
      {query !== debouncedQuery && (
        <p style={{ color: '#9ca3af', fontSize: 13 }}>Waiting for you to stop typing...</p>
      )}

      {/* Render based on discriminated union status */}
      {searchState.status === 'loading' && (
        <p>Searching for "{searchState.query}"...</p>
      )}

      {searchState.status === 'error' && (
        <p style={{ color: 'red' }}>
          Search failed for "{searchState.query}": {searchState.message}
        </p>
      )}

      {searchState.status === 'success' && (
        <div>
          <p style={{ color: '#6b7280', marginBottom: 12 }}>
            {searchState.data.total} candidates for "{searchState.data.query}"
          </p>

          {searchState.data.candidates.length === 0 ? (
            <p>No candidates found. Try a different search.</p>
          ) : (
            searchState.data.candidates.map(c => (
              <div
                key={c.id}
                style={{
                  padding: '12px 16px',
                  borderBottom: '1px solid #f3f4f6',
                }}
              >
                <strong>{c.name}</strong>
                <p style={{ margin: '4px 0', color: '#6b7280', fontSize: 14 }}>
                  {c.experience} years · {c.location}
                </p>
                <p style={{ margin: 0, fontSize: 13 }}>
                  {c.skills.join(' · ')}
                </p>
              </div>
            ))
          )}
        </div>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`return () => controller.abort()` in the useEffect cleanup is the core answer.
They want you to explain the race condition clearly: "Two fetches fired.
The slower one resolved last and overwrote the newer results."
They also check that you handle `AbortError` separately from real errors.

🔷 **TypeScript lesson:**
The `SearchState` discriminated union means that when `searchState.status === 'loading'`,
TypeScript knows `searchState.query` exists. When `status === 'success'`,
TypeScript knows `searchState.data` exists. No type casting, no optional
chaining needed — the discriminant narrows the type automatically.

⚡ **Why this matters in production:**
Race conditions are invisible in dev (fast network). They appear in production
on slow connections or when users type quickly. Without the fix, users
see results that do not match their query — making search feel broken.

🔍 **What a senior dev spots in 10 seconds:**
`await fetch(...)` inside `useEffect` with a frequently-changing dependency
and no `AbortController`. This is a race condition waiting to happen
whenever the dependency changes before the previous fetch completes.

🚀 **Before you ship this, verify:**
- [ ] Does typing quickly always show results for the LATEST query?
- [ ] Is AbortError handled silently (not shown as an error to users)?
- [ ] Does clearing the input reset the search state?
- [ ] Does debounce prevent API calls on every keystroke?
- [ ] Does it handle slow network correctly (correct results always win)?

</details>

---

### 🔴 Pattern 14 · Scenario 2 — The Form That Submits After You Leave

🏢 **Situation:**
You are at a healthcare portal. A patient fills out an appointment form
and clicks Submit. Before the API responds (it is slow — 3 seconds),
they change their mind and navigate to a different page. Your React error
monitoring shows hundreds of warnings: "Warning: Can't perform a React
state update on an unmounted component." And some appointments are being
created with corrupted data. You need to fix the async cleanup.

🔷 **Types used in this scenario:**

```typescript
interface AppointmentFormData {
  doctorId: string;
  date: string;
  timeSlot: string;
  reason: string;
  patientNotes: string;
}

interface AppointmentResponse {
  appointmentId: string;
  confirmationCode: string;
  doctorName: string;
  scheduledAt: string;
}

type SubmitStatus = 'idle' | 'submitting' | 'success' | 'error';
```

**Here is the current code:**

```tsx
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';

interface AppointmentFormData {
  doctorId: string;
  date: string;
  timeSlot: string;
  reason: string;
  patientNotes: string;
}

interface AppointmentResponse {
  appointmentId: string;
  confirmationCode: string;
  doctorName: string;
  scheduledAt: string;
}

function AppointmentForm(): JSX.Element {
  const navigate = useNavigate();
  const [formData, setFormData] = useState<AppointmentFormData>({
    doctorId: '',
    date: '',
    timeSlot: '',
    reason: '',
    patientNotes: '',
  });
  const [status, setStatus] = useState<string>('idle');
  const [error, setError] = useState<string>('');

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>): Promise<void> => {
    e.preventDefault();
    setStatus('submitting');

    // BUG: if user navigates away during the 3-second API call,
    // setStatus and setError are called on unmounted component
    const res = await fetch('/api/appointments', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(formData),
    });

    if (!res.ok) {
      setStatus('error');        // called on unmounted component
      setError('Booking failed');
      return;
    }

    const data = await res.json() as AppointmentResponse;
    setStatus('success');        // called on unmounted component
    navigate(`/appointments/${data.appointmentId}`);
  };

  const handleChange = (
    e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement | HTMLSelectElement>
  ): void => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  return (
    <form onSubmit={handleSubmit}>
      <select name="doctorId" value={formData.doctorId} onChange={handleChange}>
        <option value="">Select Doctor</option>
        <option value="d1">Dr. Sharma</option>
        <option value="d2">Dr. Patel</option>
      </select>
      <input type="date" name="date" value={formData.date} onChange={handleChange} />
      <input name="timeSlot" value={formData.timeSlot} onChange={handleChange} placeholder="Time slot" />
      <textarea name="reason" value={formData.reason} onChange={handleChange} placeholder="Reason" />
      <button type="submit" disabled={status === 'submitting'}>
        {status === 'submitting' ? 'Booking...' : 'Book Appointment'}
      </button>
      {status === 'error' && <p style={{ color: 'red' }}>{error}</p>}
    </form>
  );
}
```

**Your tasks:**
- ✅ Fix the unmounted component setState warning using `AbortController`
- ✅ Add proper TypeScript `SubmitStatus` union type
- ✅ Cancel the in-flight request if the component unmounts
- ✅ Handle the case where booking succeeds but navigation already happened

<details>
<summary>💡 Hint — How to think about this</summary>

The `handleSubmit` function is a regular async function — it cannot
be cancelled once started. The fix has two parts:
1. Use `AbortController` to cancel the fetch if the component unmounts
2. Use a `useRef` to track whether the component is still mounted
   before calling `setState` after the async operation completes.
The `useEffect` cleanup is where you set `isMounted.current = false`
and call `controller.abort()`.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useRef } from 'react';
import { useNavigate } from 'react-router-dom';

// ─── Types ────────────────────────────────────────────────
interface AppointmentFormData {
  doctorId: string;
  date: string;
  timeSlot: string;
  reason: string;
  patientNotes: string;
}

interface AppointmentResponse {
  appointmentId: string;
  confirmationCode: string;
  doctorName: string;
  scheduledAt: string;
}

// Discriminated union — cleaner than a string type
type SubmitStatus = 'idle' | 'submitting' | 'success' | 'error';

// ─── Component ────────────────────────────────────────────
function AppointmentForm(): JSX.Element {
  const navigate = useNavigate();

  const [formData, setFormData] = useState<AppointmentFormData>({
    doctorId: '',
    date: '',
    timeSlot: '',
    reason: '',
    patientNotes: '',
  });
  const [status, setStatus] = useState<SubmitStatus>('idle');
  const [error, setError] = useState<string>('');
  const [confirmation, setConfirmation] = useState<AppointmentResponse | null>(null);

  // Track mounted state and abort controller
  const isMountedRef = useRef<boolean>(true);
  const abortControllerRef = useRef<AbortController | null>(null);

  // Cleanup on unmount
  useEffect(() => {
    isMountedRef.current = true;

    return (): void => {
      isMountedRef.current = false;
      // Cancel any in-flight request when component unmounts
      abortControllerRef.current?.abort();
    };
  }, []);

  const handleChange = (
    e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement | HTMLSelectElement>
  ): void => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>): Promise<void> => {
    e.preventDefault();

    if (!formData.doctorId || !formData.date || !formData.timeSlot) {
      setError('Please fill in all required fields');
      return;
    }

    // Create new AbortController for this submission
    const controller = new AbortController();
    abortControllerRef.current = controller;

    // Only update state if still mounted
    if (isMountedRef.current) {
      setStatus('submitting');
      setError('');
    }

    try {
      const res = await fetch('/api/appointments', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData),
        signal: controller.signal, // aborts if component unmounts
      });

      // Check mounted before EVERY setState after async work
      if (!isMountedRef.current) return;

      if (!res.ok) {
        const errorData = await res.json() as { message?: string };
        setStatus('error');
        setError(errorData.message ?? 'Booking failed. Please try again.');
        return;
      }

      const data = await res.json() as AppointmentResponse;

      if (!isMountedRef.current) return;

      setStatus('success');
      setConfirmation(data);

      // Navigate after a short delay to show confirmation
      setTimeout(() => {
        if (isMountedRef.current) {
          navigate(`/appointments/${data.appointmentId}`);
        }
      }, 2000);

    } catch (err: unknown) {
      // AbortError = component unmounted — not a real error
      if (err instanceof Error && err.name === 'AbortError') return;

      if (isMountedRef.current) {
        setStatus('error');
        setError(
          err instanceof Error ? err.message : 'Network error. Please try again.'
        );
      }
    }
  };

  // Success state
  if (status === 'success' && confirmation) {
    return (
      <div style={{ padding: 24, background: '#f0fdf4', borderRadius: 8 }}>
        <h2>✅ Appointment Confirmed!</h2>
        <p>Doctor: {confirmation.doctorName}</p>
        <p>Scheduled: {new Date(confirmation.scheduledAt).toLocaleString()}</p>
        <p>Confirmation: <strong>#{confirmation.confirmationCode}</strong></p>
        <p style={{ color: '#6b7280', fontSize: 13 }}>Redirecting to your appointments...</p>
      </div>
    );
  }

  return (
    <form onSubmit={handleSubmit}>
      {error && <p style={{ color: 'red', marginBottom: 16 }}>{error}</p>}

      <div style={{ marginBottom: 12 }}>
        <label>Doctor *</label>
        <select
          name="doctorId"
          value={formData.doctorId}
          onChange={handleChange}
          required
          style={{ display: 'block', width: '100%', padding: '8px 12px' }}
        >
          <option value="">Select a doctor</option>
          <option value="d1">Dr. Sharma — General Medicine</option>
          <option value="d2">Dr. Patel — Cardiology</option>
          <option value="d3">Dr. Gupta — Orthopedics</option>
        </select>
      </div>

      <div style={{ marginBottom: 12 }}>
        <label>Date *</label>
        <input
          type="date"
          name="date"
          value={formData.date}
          onChange={handleChange}
          min={new Date().toISOString().split('T')[0]}
          required
          style={{ display: 'block', width: '100%', padding: '8px 12px' }}
        />
      </div>

      <div style={{ marginBottom: 12 }}>
        <label>Time Slot *</label>
        <input
          name="timeSlot"
          value={formData.timeSlot}
          onChange={handleChange}
          placeholder="e.g. 10:00 AM"
          required
          style={{ display: 'block', width: '100%', padding: '8px 12px' }}
        />
      </div>

      <div style={{ marginBottom: 12 }}>
        <label>Reason for Visit</label>
        <textarea
          name="reason"
          value={formData.reason}
          onChange={handleChange}
          placeholder="Brief reason for your appointment"
          style={{ display: 'block', width: '100%', padding: '8px 12px' }}
        />
      </div>

      <button
        type="submit"
        disabled={status === 'submitting'}
        style={{
          padding: '12px 24px',
          background: status === 'submitting' ? '#9ca3af' : '#6366f1',
          color: 'white',
          border: 'none',
          borderRadius: 6,
          cursor: status === 'submitting' ? 'not-allowed' : 'pointer',
        }}
      >
        {status === 'submitting' ? 'Booking your appointment...' : 'Book Appointment'}
      </button>
    </form>
  );
}
```

**✅ What the interviewer is checking:**
`isMountedRef.current` check before every `setState` after async work,
and `controller.abort()` in the useEffect cleanup. They want to hear:
"If the component unmounts while the fetch is in progress, we cancel
the fetch and skip all setState calls — preventing the warning and
potential state corruption."

🔷 **TypeScript lesson:**
`SubmitStatus = 'idle' | 'submitting' | 'success' | 'error'` is a string
literal union. It is more type-safe than `string` — TypeScript will
error if you try to assign `setStatus('loading')` since 'loading' is
not in the union. This prevents typos and documents all valid states.

⚡ **Why this matters in production:**
The warning "Can't perform a React state update on an unmounted component"
is a memory leak indicator. It also causes subtle bugs: the success state
might render on a different page, or error state might corrupt a newly
mounted form on the same route.

🔍 **What a senior dev spots in 10 seconds:**
`async` function in `handleSubmit` with `setState` calls after `await`
and no mounted check. Any `await` is a point where the component could
have unmounted. Every `setState` after an `await` needs a mounted guard.

🚀 **Before you ship this, verify:**
- [ ] Does navigating away mid-submission prevent the warning?
- [ ] Does the in-flight request cancel on unmount?
- [ ] Is there a success confirmation before redirect?
- [ ] Does rapid double-click not submit twice?
- [ ] Does the error state clear when the user tries again?

</details>

---

### 🔴 Pattern 14 · Scenario 3 — The Dashboard That Loads in the Wrong Order

🏢 **Situation:**
You are at a fintech company. The user dashboard needs to load three things:
the user profile first (to get the userId), then their account list (needs userId),
then the transaction summary for each account. These are dependent requests —
each one needs the result of the previous. Currently the code fires all three
at once and crashes because the second and third calls have no userId yet.

🔷 **Types used in this scenario:**

```typescript
interface UserProfile {
  id: string;
  name: string;
  email: string;
  kycStatus: 'pending' | 'verified' | 'rejected';
}

interface BankAccount {
  id: string;
  userId: string;
  accountNumber: string;
  balance: number;
  currency: string;
  type: 'savings' | 'current' | 'fixed_deposit';
}

interface TransactionSummary {
  accountId: string;
  totalCredit: number;
  totalDebit: number;
  transactionCount: number;
  lastTransactionAt: string;
}

interface DashboardData {
  user: UserProfile;
  accounts: BankAccount[];
  summaries: TransactionSummary[];
}
```

**Here is the current code:**

```tsx
import { useState, useEffect } from 'react';

interface UserProfile { id: string; name: string; email: string; }
interface BankAccount { id: string; userId: string; balance: number; }
interface TransactionSummary { accountId: string; totalCredit: number; }

function FinanceDashboard(): JSX.Element {
  const [user, setUser] = useState<UserProfile | null>(null);
  const [accounts, setAccounts] = useState<BankAccount[]>([]);
  const [summaries, setSummaries] = useState<TransactionSummary[]>([]);
  const [isLoading, setIsLoading] = useState<boolean>(true);

  useEffect(() => {
    // BUG: all three fire simultaneously
    // accounts and summaries calls have no userId yet
    const loadAll = async (): Promise<void> => {
      const [userRes, accountsRes] = await Promise.all([
        fetch('/api/user/me'),
        fetch('/api/accounts?userId=undefined'), // userId not known yet!
        fetch('/api/transactions/summary?accountIds='), // accountIds not known!
      ]);

      const userData = await userRes.json() as UserProfile;
      const accountsData = await accountsRes.json() as BankAccount[];

      setUser(userData);
      setAccounts(accountsData);
      setIsLoading(false);
    };

    loadAll();
  }, []);

  if (isLoading) return <p>Loading...</p>;

  return (
    <div>
      <h1>Welcome, {user?.name}</h1>
      {accounts.map(acc => (
        <div key={acc.id}>Balance: ₹{acc.balance}</div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the dependent request sequence: user → accounts → summaries
- ✅ Show partial data as each step loads (not wait for all three)
- ✅ Use a typed `DashboardData` state to track loading progress
- ✅ Handle failure at each step independently

<details>
<summary>💡 Hint — How to think about this</summary>

Sequential dependent requests cannot use `Promise.all` —
that fires everything simultaneously. You need to `await` each
step and use the result for the next.
For partial loading: update state after each step, not at the end.
This lets you render user info immediately, then accounts when ready,
then summaries when ready — a progressive loading experience.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useRef } from 'react';

// ─── Types ────────────────────────────────────────────────
interface UserProfile {
  id: string;
  name: string;
  email: string;
  kycStatus: 'pending' | 'verified' | 'rejected';
}

interface BankAccount {
  id: string;
  userId: string;
  accountNumber: string;
  balance: number;
  currency: string;
  type: 'savings' | 'current' | 'fixed_deposit';
}

interface TransactionSummary {
  accountId: string;
  totalCredit: number;
  totalDebit: number;
  transactionCount: number;
  lastTransactionAt: string;
}

// Tracks what has loaded — partial data shows as it arrives
interface LoadingState {
  user: UserProfile | null;
  accounts: BankAccount[] | null;
  summaries: TransactionSummary[] | null;
  errors: {
    user: string | null;
    accounts: string | null;
    summaries: string | null;
  };
}

// ─── Typed Generic Fetch Utility ──────────────────────────
async function apiFetch<T>(url: string, signal: AbortSignal): Promise<T> {
  const res = await fetch(url, { signal });
  if (!res.ok) throw new Error(`${res.status}: ${res.statusText}`);
  return res.json() as Promise<T>;
}

// ─── Component ────────────────────────────────────────────
function FinanceDashboard(): JSX.Element {
  const [loadingState, setLoadingState] = useState<LoadingState>({
    user: null,
    accounts: null,
    summaries: null,
    errors: { user: null, accounts: null, summaries: null },
  });

  const isMountedRef = useRef<boolean>(true);

  useEffect(() => {
    isMountedRef.current = true;
    const controller = new AbortController();

    const loadDashboard = async (): Promise<void> => {

      // ── STEP 1: Load user profile ──────────────────────
      let user: UserProfile;
      try {
        user = await apiFetch<UserProfile>('/api/user/me', controller.signal);

        if (!isMountedRef.current) return;

        // Show user immediately — don't wait for accounts
        setLoadingState(prev => ({ ...prev, user }));

      } catch (err: unknown) {
        if (err instanceof Error && err.name === 'AbortError') return;
        if (!isMountedRef.current) return;

        setLoadingState(prev => ({
          ...prev,
          errors: {
            ...prev.errors,
            user: err instanceof Error ? err.message : 'Failed to load profile',
          },
        }));
        return; // Cannot proceed without user
      }

      // ── STEP 2: Load accounts (needs user.id) ─────────
      let accounts: BankAccount[];
      try {
        accounts = await apiFetch<BankAccount[]>(
          `/api/accounts?userId=${user.id}`,
          controller.signal
        );

        if (!isMountedRef.current) return;

        // Show accounts immediately — don't wait for summaries
        setLoadingState(prev => ({ ...prev, accounts }));

      } catch (err: unknown) {
        if (err instanceof Error && err.name === 'AbortError') return;
        if (!isMountedRef.current) return;

        setLoadingState(prev => ({
          ...prev,
          errors: {
            ...prev.errors,
            accounts: err instanceof Error ? err.message : 'Failed to load accounts',
          },
        }));
        return; // Cannot proceed without accounts
      }

      // ── STEP 3: Load summaries (needs account IDs) ────
      // This can run in parallel for all accounts since we have all IDs
      try {
        const accountIds = accounts.map(a => a.id).join(',');
        const summaries = await apiFetch<TransactionSummary[]>(
          `/api/transactions/summary?accountIds=${accountIds}`,
          controller.signal
        );

        if (!isMountedRef.current) return;

        setLoadingState(prev => ({ ...prev, summaries }));

      } catch (err: unknown) {
        if (err instanceof Error && err.name === 'AbortError') return;
        if (!isMountedRef.current) return;

        // Summaries failing does not break user/accounts display
        setLoadingState(prev => ({
          ...prev,
          errors: {
            ...prev.errors,
            summaries: 'Could not load transaction summary',
          },
        }));
      }
    };

    loadDashboard();

    return (): void => {
      isMountedRef.current = false;
      controller.abort();
    };
  }, []);

  const { user, accounts, summaries, errors } = loadingState;

  // Initial load — nothing yet
  if (!user && !errors.user) {
    return <p>Loading your profile...</p>;
  }

  if (errors.user) {
    return <p style={{ color: 'red' }}>Failed to load: {errors.user}</p>;
  }

  return (
    <div style={{ padding: 24 }}>
      {/* User section — shows immediately after step 1 */}
      {user && (
        <div style={{ marginBottom: 24 }}>
          <h1>Welcome, {user.name}</h1>
          <p style={{ color: '#6b7280' }}>{user.email}</p>
          {user.kycStatus !== 'verified' && (
            <p style={{ color: 'orange' }}>
              ⚠️ KYC Status: {user.kycStatus} — some features may be limited
            </p>
          )}
        </div>
      )}

      {/* Accounts section — shows after step 2 */}
      <div style={{ marginBottom: 24 }}>
        <h2>Your Accounts</h2>
        {!accounts && !errors.accounts && <p>Loading accounts...</p>}
        {errors.accounts && <p style={{ color: 'red' }}>{errors.accounts}</p>}
        {accounts && accounts.map(account => {
          // Find matching summary if loaded
          const summary = summaries?.find(s => s.accountId === account.id);

          return (
            <div
              key={account.id}
              style={{
                padding: 16,
                border: '1px solid #e5e7eb',
                borderRadius: 8,
                marginBottom: 12,
              }}
            >
              <div style={{ display: 'flex', justifyContent: 'space-between' }}>
                <div>
                  <p style={{ margin: 0, fontWeight: 'bold', textTransform: 'capitalize' }}>
                    {account.type.replace('_', ' ')} Account
                  </p>
                  <p style={{ margin: '4px 0', color: '#6b7280', fontSize: 13 }}>
                    ****{account.accountNumber.slice(-4)}
                  </p>
                </div>
                <div style={{ textAlign: 'right' }}>
                  <p style={{ margin: 0, fontSize: 20, fontWeight: 'bold' }}>
                    {account.currency} {account.balance.toLocaleString()}
                  </p>
                </div>
              </div>

              {/* Summaries section — shows inline after step 3 */}
              {summary ? (
                <div style={{
                  marginTop: 12, padding: 12, background: '#f9fafb', borderRadius: 6,
                  display: 'flex', gap: 24, fontSize: 13,
                }}>
                  <span style={{ color: 'green' }}>
                    ↑ ₹{summary.totalCredit.toLocaleString()} in
                  </span>
                  <span style={{ color: 'red' }}>
                    ↓ ₹{summary.totalDebit.toLocaleString()} out
                  </span>
                  <span style={{ color: '#6b7280' }}>
                    {summary.transactionCount} transactions
                  </span>
                </div>
              ) : !summaries && !errors.summaries ? (
                <p style={{ margin: '8px 0 0', fontSize: 13, color: '#9ca3af' }}>
                  Loading transaction summary...
                </p>
              ) : null}
            </div>
          );
        })}
        {errors.summaries && (
          <p style={{ color: '#f59e0b', fontSize: 13 }}>{errors.summaries}</p>
        )}
      </div>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
Sequential `await` for dependent calls, with UI updates after each step —
not at the end. They want to hear: "Each step's result is needed by the next.
I show partial data as soon as each step completes instead of making the user
wait for all three." They also check independent error handling per step.

🔷 **TypeScript lesson:**
`apiFetch<T>` is a generic fetch utility — `T` is the expected response type.
`apiFetch<UserProfile>(url, signal)` returns `Promise<UserProfile>`.
This pattern avoids repeating `res.json() as SomeType` everywhere and
centralizes error handling. Production codebases always have a typed
API utility like this.

⚡ **Why this matters in production:**
Banking apps with multiple dependent resources are extremely common.
Showing partial data progressively reduces perceived loading time
dramatically — users see their name and profile in 200ms instead of
waiting 1.5 seconds for all three requests to complete.

🔍 **What a senior dev spots in 10 seconds:**
`Promise.all` with URLs that contain undefined values from not-yet-loaded
data. `?userId=undefined` in the URL is the giveaway — the second fetch
started before the first one completed.

🚀 **Before you ship this, verify:**
- [ ] Does user info appear before accounts (progressive loading)?
- [ ] Does account list appear before summaries?
- [ ] Does a failure at step 2 still show step 1 data?
- [ ] Does the fetch cancel if the user navigates away mid-load?
- [ ] Does it handle accounts with no summaries gracefully?

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Interview Pattern Picker — Level 3

> **"Interviewer asks [X]" → "Go to Pattern [N]"**

| Problem or Question | Go To |
|---|---|
| "The search box is making too many API calls" | Pattern 12 · Scenario 1 — debounce |
| "Search shows results from the previous query" | Pattern 14 · Scenario 1 — race condition |
| "Why is the child component re-rendering when the parent's unrelated state changes?" | Pattern 12 · Scenario 2 — React.memo + useCallback |
| "The page freezes when I render 5000 items" | Pattern 12 · Scenario 3 — virtual list |
| "How do you handle a list of 10,000 items in React?" | Pattern 12 · Scenario 3 |
| "The order status never updates without refresh" | Pattern 13 · Scenario 1 — polling |
| "Memory usage climbs after browsing for a while" | Pattern 13 · Scenario 2 — WebSocket leak |
| "Chat messages are missing even though WebSocket is connected" | Pattern 13 · Scenario 3 — stale closure |
| "Can't perform a React state update on an unmounted component" | Pattern 14 · Scenario 2 — abort + isMounted |
| "The API is called with userId=undefined" | Pattern 14 · Scenario 3 — dependent requests |
| "The dashboard waits for all data before showing anything" | Pattern 14 · Scenario 3 — progressive loading |
| "How do you cancel a fetch when a component unmounts?" | Pattern 14 · Scenario 1 or 2 — AbortController |
| "What is a race condition in React?" | Pattern 14 · Scenario 1 |
| "When should you use useMemo vs useCallback?" | Pattern 12 · Scenario 2 |
| "How do you type a WebSocket message with multiple event types?" | Pattern 13 · Scenario 2 — discriminated union |
| "How do you write a reusable typed fetch utility?" | Pattern 14 · Scenario 3 — apiFetch<T> |
| "Why does React.memo not prevent re-renders for my component?" | Pattern 12 · Scenario 2 — missing useCallback |
| "How do you stop a WebSocket reconnecting after logout?" | Pattern 13 · Scenario 2 — onclose = null before close |
| "The form state updates after I navigate away" | Pattern 14 · Scenario 2 |
| "How do you load data that depends on the result of another API call?" | Pattern 14 · Scenario 3 |

---

> **Part 3 complete.**
> Say **"Generate Part 4 — Level 4, Patterns 15 to 19"** to continue.