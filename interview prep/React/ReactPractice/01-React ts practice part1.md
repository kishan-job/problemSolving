# React + TypeScript Practice Guide — Part 1
## Level 1: Foundation (80% of Interview Questions)


---

## How This Guide Is Structured

---

### Pattern Header

Every pattern starts with this:

- **What you will build** — 3 real features this pattern covers
- **Interview asks this as** — exact phrases interviewers use
- **TypeScript concepts** — specific TS skills used here
- **Most common mistake** — the one thing that fails interviews

---

### Scenario Structure

Every scenario contains these sections in order:

- 🏢 **Situation** — real company, real problem, 2-3 lines
- **Types** — all TypeScript interfaces defined before the code
- **Starter code** — broken or incomplete realistic code
- **Your tasks** — 3-4 specific things to fix or build
- 💡 **Hint** — thinking direction only, no answer *(click to open)*
- ✅ **Full Solution** — code + interviewer checks + TypeScript lesson *(click to open)*

---

### The 4 Scenario Types

Every pattern has exactly 4 scenarios, each testing a different skill:

**Scenario 1 — Build it properly**
Given basic untyped code → make it production-ready

**Scenario 2 — Find and fix the bug**
Given code that looks right but behaves wrong → find and fix it

**Scenario 3 — Add the missing feature**
Given working but incomplete code → add what is missing

**Scenario 4 — Handle the edge case**
Given code that works normally → make it survive the unusual condition

---

> ⚠️ Attempt every scenario yourself before opening the solution.
> One scenario done properly beats five scenarios skimmed.

---
---

# Table of Contents

- [Pattern 1: 📋 Fetch & Display Data](#pattern-1--fetch--display-data)
  - [Scenario 1 — The Dashboard Nobody Can Use](#pattern-1--scenario-1--the-dashboard-nobody-can-use)
  - [Scenario 2 — The User Table That Shows Nothing](#pattern-1--scenario-2--the-user-table-that-shows-nothing)
  - [Scenario 3 — The Profile Page That Crashes](#pattern-1--scenario-3--the-profile-page-that-crashes)
  - [Scenario 4 — The Data That Goes Stale](#pattern-1--scenario-4--the-data-that-goes-stale)
- [Pattern 2: 📝 Form + Validation](#pattern-2--form--validation)
  - [Scenario 1 — The Login Form That Lets Anyone In](#pattern-2--scenario-1--the-login-form-that-lets-anyone-in)
  - [Scenario 2 — The Signup Form That Submits Three Times](#pattern-2--scenario-2--the-signup-form-that-submits-three-times)
  - [Scenario 3 — The Form That Loses Your Data](#pattern-2--scenario-3--the-form-that-loses-your-data)
  - [Scenario 4 — The Form With No Feedback](#pattern-2--scenario-4--the-form-with-no-feedback)
- [Pattern 3: 🔁 List + Search + Filter](#pattern-3--list--search--filter)
  - [Scenario 1 — The Product Page Nobody Can Search](#pattern-3--scenario-1--the-product-page-nobody-can-search)
  - [Scenario 2 — The Employee List That Filters Wrong](#pattern-3--scenario-2--the-employee-list-that-filters-wrong)
  - [Scenario 3 — The Job Board That Sorts Incorrectly](#pattern-3--scenario-3--the-job-board-that-sorts-incorrectly)
  - [Scenario 4 — The Search That Breaks on Special Characters](#pattern-3--scenario-4--the-search-that-breaks-on-special-characters)
- [Pattern 4: 💾 localStorage Usage](#pattern-4--localstorage-usage)
  - [Scenario 1 — The Cart That Empties on Refresh](#pattern-4--scenario-1--the-cart-that-empties-on-refresh)
  - [Scenario 2 — The Settings That Reset Every Visit](#pattern-4--scenario-2--the-settings-that-reset-every-visit)
  - [Scenario 3 — The Draft That Belongs to the Wrong Post](#pattern-4--scenario-3--the-draft-that-belongs-to-the-wrong-post)
  - [Scenario 4 — The localStorage That Crashes the App](#pattern-4--scenario-4--the-localstorage-that-crashes-the-app)
- [Pattern 5: 🔀 Show/Hide UI](#pattern-5--showhide-ui)
  - [Scenario 1 — The Modal That Never Closes](#pattern-5--scenario-1--the-modal-that-never-closes)
  - [Scenario 2 — The Accordion That Opens Everything](#pattern-5--scenario-2--the-accordion-that-opens-everything)
  - [Scenario 3 — The Dropdown That Stays Open](#pattern-5--scenario-3--the-dropdown-that-stays-open)
  - [Scenario 4 — The Tabs That Lose Their State](#pattern-5--scenario-4--the-tabs-that-lose-their-state)
- [Pattern 6: 🧭 Page Navigation](#pattern-6--page-navigation)
  - [Scenario 1 — The Protected Page Anyone Can Visit](#pattern-6--scenario-1--the-protected-page-anyone-can-visit)
  - [Scenario 2 — The Login That Does Not Redirect](#pattern-6--scenario-2--the-login-that-does-not-redirect)
  - [Scenario 3 — The Back Button That Breaks Auth](#pattern-6--scenario-3--the-back-button-that-breaks-auth)
  - [Scenario 4 — The Page That Flashes Before Redirecting](#pattern-6--scenario-4--the-page-that-flashes-before-redirecting)
- [Interview Pattern Picker](#interview-pattern-picker)

---

# Pattern 1: 📋 Fetch & Display Data

> **Level 1 — Foundation**

**What you will build in this pattern:**
- A dashboard that fetches typed data from an API and displays it in a table
- A profile page that fetches a single typed record by ID with error handling
- Any data-loading page with loading, error, and empty states — all typed

**Interview asks this as:**
- "Fetch a list of users from this API and display them in a table"
- "Build a product listing page that loads data on mount"
- "Show loading and error states while fetching — using TypeScript"

**TypeScript concepts used in this pattern:**
- Typed `useState` with generics: `useState<User[]>([])`
- Discriminated union status type: `'idle' | 'loading' | 'success' | 'error'`
- Generic fetch utility with typed response
- `interface` for API response shapes
- Optional chaining and nullish coalescing on nested data

**Most common mistake in interviews:**
Showing only the happy path — no loading state, no error state, no empty state.
In TypeScript interviews: leaving `useState([])` untyped or using `any` for the API response.

---

### Pattern 1 · Scenario 1 — The Dashboard Nobody Can Use

🏢 **Situation:**
You joined a fintech startup. The analytics dashboard shows a blank white screen
while the API loads and stays blank when the API fails. Users are calling support
thinking the app is broken. Your job: make it production-ready with proper TypeScript types.

**Types used in this scenario:**
```typescript
interface Transaction {
  id: string;
  description: string;
  amount: number;
  currency: string;
  createdAt: string;
}

type FetchStatus = 'loading' | 'success' | 'error';
```

**Here is the starter code:**
```tsx
import { useState, useEffect } from 'react';

function TransactionDashboard() {
  const [transactions, setTransactions] = useState([]);  // untyped

  useEffect(() => {
    fetch('/api/transactions')
      .then(res => res.json())
      .then(data => setTransactions(data));  // no error handling
  }, []);

  return (
    <div>
      <h1>Transactions</h1>
      {transactions.map(tx => (           // tx is any — no types
        <div key={tx.id}>
          <span>{tx.description}</span>
          <span>{tx.amount}</span>
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Add TypeScript interfaces for `Transaction` and type the state
- ✅ Add loading, error, and empty states with typed status
- ✅ Use `async/await` instead of `.then()` chains
- ✅ Check `res.ok` — fetch does not throw on 4xx/5xx errors

<details>
<summary>💡 Hint</summary>

You need three state variables: `transactions`, `status`, and `error`.
Type `transactions` as `Transaction[]` and `status` as the union type.
Remember: `fetch()` only throws on network failure — a 500 response
does not throw. You must check `res.ok` manually and throw yourself.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

// Define the shape of a transaction from the API
interface Transaction {
  id: string;
  description: string;
  amount: number;
  currency: string;
  createdAt: string;
}

// Discriminated union — better than three separate booleans
type FetchStatus = 'loading' | 'success' | 'error';

function TransactionDashboard(): JSX.Element {
  // Typed state — TypeScript knows exactly what is in the array
  const [transactions, setTransactions] = useState<Transaction[]>([]);
  const [status, setStatus] = useState<FetchStatus>('loading');
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchTransactions = async (): Promise<void> => {
      try {
        setStatus('loading');
        setError(null);

        const res = await fetch('/api/transactions');

        // fetch does NOT throw on 4xx/5xx — must check manually
        if (!res.ok) {
          throw new Error(`Server error: ${res.status}`);
        }

        // Cast the response to the expected type
        const data = await res.json() as Transaction[];
        setTransactions(data);
        setStatus('success');

      } catch (err: unknown) {
        // Narrow the unknown error to get the message
        const message = err instanceof Error ? err.message : 'Unknown error';
        setError(message);
        setStatus('error');
      }
    };

    fetchTransactions();
  }, []);

  if (status === 'loading') {
    return <div>Loading transactions...</div>;
  }

  if (status === 'error') {
    return <div>Error: {error}. Please try again.</div>;
  }

  if (transactions.length === 0) {
    return <div>No transactions found.</div>;
  }

  return (
    <div>
      <h1>Transactions</h1>
      {transactions.map((tx: Transaction) => (
        <div key={tx.id}>
          <span>{tx.description}</span>
          {/* TypeScript knows tx.amount is a number */}
          <span>{tx.currency} {tx.amount.toFixed(2)}</span>
        </div>
      ))}
    </div>
  );
}

export default TransactionDashboard;
```

**What the interviewer is checking:**
Logic: all three states handled, `res.ok` checked, `async/await` used.
TypeScript: `useState<Transaction[]>`, typed status union, `err: unknown` narrowed with `instanceof`.

**TypeScript lesson:**
`err: unknown` in catch blocks is the correct TypeScript approach — not `err: any`.
You must narrow it with `instanceof Error` before accessing `.message`.
This prevents runtime errors from assuming the caught value has a certain shape.

</details>

---

### Pattern 1 · Scenario 2 — The User Table That Shows Nothing

🏢 **Situation:**
You are at an HR tool company. The employee directory shows 200 OK in the network tab
with data, but the table is completely empty. Your senior dev asks you to find
and fix the bug in 15 minutes. There is also a TypeScript error being ignored.

**Types used in this scenario:**
```typescript
interface Employee {
  id: string;
  name: string;
  department: string;
  email: string;
  role: string;
}

interface EmployeesApiResponse {
  employees: Employee[];
  total: number;
  page: number;
}
```

**Here is the broken code:**
```tsx
import { useState, useEffect } from 'react';

function EmployeeDirectory() {
  const [employees, setEmployees] = useState<any[]>([]);  // any — weak type
  const [isLoading, setIsLoading] = useState(false);      // should start true

  useEffect(() => {
    const load = async () => {
      setIsLoading(true);
      const res = await fetch('/api/employees');
      const data = await res.json();
      // API returns: { employees: [...], total: 120, page: 1 }
      setEmployees(data);   // BUG: storing the whole object, not the array
      setIsLoading(false);
    };
    load();
  }, []);

  if (isLoading) return <p>Loading...</p>;

  return (
    <table>
      <thead>
        <tr><th>Name</th><th>Department</th><th>Email</th></tr>
      </thead>
      <tbody>
        {employees.map(emp => (    // emp is any — no type safety
          <tr key={emp.id}>
            <td>{emp.name}</td>
            <td>{emp.department}</td>
            <td>{emp.email}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

**Your tasks:**
- ✅ Find the data shape bug causing the empty table
- ✅ Fix the `any` type — use proper interfaces
- ✅ Fix `isLoading` initial value
- ✅ Add error handling and typed error state

<details>
<summary>💡 Hint</summary>

The API returns `{ employees: [...], total: 120, page: 1 }` — an object,
not an array. `setEmployees(data)` stores the whole object.
When you map over an object, you get nothing.
Use the `EmployeesApiResponse` interface to type the response,
then extract `data.employees`.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

interface Employee {
  id: string;
  name: string;
  department: string;
  email: string;
  role: string;
}

// Typing the full API response shape
interface EmployeesApiResponse {
  employees: Employee[];
  total: number;
  page: number;
}

function EmployeeDirectory(): JSX.Element {
  // FIX: use Employee[] not any[]
  const [employees, setEmployees] = useState<Employee[]>([]);
  // FIX: starts true — data loads on mount immediately
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const load = async (): Promise<void> => {
      try {
        setIsLoading(true);
        const res = await fetch('/api/employees');

        if (!res.ok) throw new Error('Failed to load employees');

        // Type the response as the full API shape
        const data = await res.json() as EmployeesApiResponse;

        // FIX: extract the array from the response object
        setEmployees(data.employees);

      } catch (err: unknown) {
        const message = err instanceof Error ? err.message : 'Unknown error';
        setError(message);
      } finally {
        setIsLoading(false);
      }
    };

    load();
  }, []);

  if (isLoading) return <p>Loading employees...</p>;
  if (error) return <p>Could not load employees: {error}</p>;
  if (employees.length === 0) return <p>No employees found.</p>;

  return (
    <table>
      <thead>
        <tr><th>Name</th><th>Department</th><th>Email</th></tr>
      </thead>
      <tbody>
        {/* TypeScript now knows emp is Employee — full autocomplete */}
        {employees.map((emp: Employee) => (
          <tr key={emp.id}>
            <td>{emp.name}</td>
            <td>{emp.department}</td>
            <td>{emp.email}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}

export default EmployeeDirectory;
```

**What the interviewer is checking:**
Logic: `data.employees` instead of `data`, `isLoading` starts `true`.
TypeScript: `EmployeesApiResponse` interface for the full response, `Employee[]` not `any[]`.

**TypeScript lesson:**
Always type your API response to match its actual shape — including wrapper objects.
If the API returns `{ data: T[], total: number }`, type that wrapper.
`any[]` hides bugs like this one at compile time that only appear at runtime.

</details>

---

### Pattern 1 · Scenario 3 — The Profile Page That Crashes

🏢 **Situation:**
You are at a social platform. The user profile page works for most users
but crashes with "Cannot read properties of undefined" when a user
navigates to a profile before the data loads. Fix it with proper
TypeScript null safety.

**Types used in this scenario:**
```typescript
interface UserFollowers {
  count: number;
  preview: string[];
}

interface UserProfile {
  id: string;
  name: string;
  bio: string;
  avatar: string;
  joinedAt: string;
  followers: UserFollowers;
}

interface UserProfileProps {
  userId: string;
}
```

**Here is the broken code:**
```tsx
import { useState, useEffect } from 'react';
import { useParams } from 'react-router-dom';

function UserProfile() {
  const { userId } = useParams();
  const [user, setUser] = useState(null);   // untyped null

  useEffect(() => {
    const loadUser = async () => {
      const res = await fetch(`/api/users/${userId}`);
      const data = await res.json();
      setUser(data);
    };
    loadUser();
  }, [userId]);

  // CRASHES — user is null on first render
  return (
    <div>
      <img src={user.avatar} alt={user.name} />
      <h1>{user.name}</h1>
      <p>{user.bio}</p>
      <p>Followers: {user.followers.count}</p>
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the null crash with proper TypeScript typing
- ✅ Add loading, error, and 404 states
- ✅ Use optional chaining where appropriate
- ✅ Reset state when `userId` changes (navigating between profiles)

<details>
<summary>💡 Hint</summary>

Type the state as `useState<UserProfile | null>(null)`.
With this type, TypeScript will actually prevent you from
accessing `user.avatar` when `user` is `null` — the type
system catches the crash at compile time.
Guard the render: check `isLoading`, then `error`, then `!user`
before rendering the profile content.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';
import { useParams } from 'react-router-dom';

interface UserFollowers {
  count: number;
  preview: string[];
}

interface UserProfile {
  id: string;
  name: string;
  bio: string;
  avatar: string;
  joinedAt: string;
  followers: UserFollowers;
}

// useParams returns Record<string, string | undefined>
// so we type userId explicitly
function UserProfilePage(): JSX.Element {
  const { userId } = useParams<{ userId: string }>();

  // TypeScript knows user is UserProfile or null — not unknown
  const [user, setUser] = useState<UserProfile | null>(null);
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    if (!userId) return;

    const loadUser = async (): Promise<void> => {
      try {
        // Reset when userId changes — switching between profiles
        setIsLoading(true);
        setError(null);
        setUser(null);

        const res = await fetch(`/api/users/${userId}`);

        if (res.status === 404) {
          throw new Error('User not found');
        }
        if (!res.ok) {
          throw new Error(`Server error: ${res.status}`);
        }

        const data = await res.json() as UserProfile;
        setUser(data);

      } catch (err: unknown) {
        const message = err instanceof Error ? err.message : 'Failed to load profile';
        setError(message);
      } finally {
        setIsLoading(false);
      }
    };

    loadUser();
  }, [userId]); // re-fetch when userId changes

  if (isLoading) return <div>Loading profile...</div>;
  if (error) return <div>{error}</div>;

  // TypeScript narrows: after this check, user is UserProfile (not null)
  if (!user) return <div>No user data.</div>;

  // From here TypeScript knows user is UserProfile — full type safety
  return (
    <div>
      <img src={user.avatar} alt={user.name} />
      <h1>{user.name}</h1>
      <p>{user.bio}</p>
      {/* Optional chaining for nested objects */}
      <p>Followers: {user.followers?.count ?? 0}</p>
      <p>Joined: {new Date(user.joinedAt).toLocaleDateString()}</p>
    </div>
  );
}

export default UserProfilePage;
```

**What the interviewer is checking:**
Logic: null guard before render, state reset on userId change, 404 handled.
TypeScript: `useState<UserProfile | null>`, type narrowing after `if (!user)`,
`useParams<{ userId: string }>()` for typed params.

**TypeScript lesson:**
Type narrowing via `if (!user) return` tells TypeScript that below that line,
`user` is guaranteed to be `UserProfile`. This is called control flow analysis.
TypeScript tracks what types are possible at each point in your code.

</details>

---

### Pattern 1 · Scenario 4 — The Data That Goes Stale

🏢 **Situation:**
You are building a food delivery app. The order status page shows
"Preparing" and never updates. Users have to refresh manually to see
if their order moved to "On the way". Add a refresh button and
auto-refresh every 30 seconds — all fully typed.

**Types used in this scenario:**
```typescript
type OrderStatus = 'pending' | 'preparing' | 'on_the_way' | 'delivered';

interface Order {
  id: string;
  status: OrderStatus;
  eta: string;
  restaurantName: string;
  items: OrderItem[];
}

interface OrderItem {
  name: string;
  quantity: number;
  price: number;
}

interface OrderStatusProps {
  orderId: string;
}
```

**Here is the starter code:**
```tsx
import { useState, useEffect } from 'react';

function OrderStatus({ orderId }) {      // untyped props
  const [order, setOrder] = useState(null);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    const loadOrder = async () => {
      const res = await fetch(`/api/orders/${orderId}`);
      const data = await res.json();
      setOrder(data);
      setIsLoading(false);
    };
    loadOrder();
  }, [orderId]);

  if (isLoading) return <p>Loading...</p>;
  if (!order) return null;

  return (
    <div>
      <h2>Order #{order.id}</h2>
      <p>Status: {order.status}</p>
      <p>ETA: {order.eta}</p>
    </div>
  );
}
```

**Your tasks:**
- ✅ Add `OrderStatusProps` interface and type the component props
- ✅ Type all state with proper interfaces
- ✅ Add a manual refresh button (typed click handler)
- ✅ Add auto-refresh every 30 seconds — stop when status is `'delivered'`
- ✅ Clean up the interval on unmount

<details>
<summary>💡 Hint</summary>

Use `useCallback` to create a stable `fetchOrder` function so it can be
called by both the button and the interval without causing extra re-renders.
Type the `useCallback` return as `() => Promise<void>`.
For the interval cleanup, store the ID in `useRef<ReturnType<typeof setInterval> | null>`.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useCallback, useRef } from 'react';

type OrderStatus = 'pending' | 'preparing' | 'on_the_way' | 'delivered';

interface OrderItem {
  name: string;
  quantity: number;
  price: number;
}

interface Order {
  id: string;
  status: OrderStatus;
  eta: string;
  restaurantName: string;
  items: OrderItem[];
}

interface OrderStatusProps {
  orderId: string;
}

function OrderStatusPage({ orderId }: OrderStatusProps): JSX.Element {
  const [order, setOrder] = useState<Order | null>(null);
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  // Store interval ID in ref — changing ref does not cause re-render
  const intervalRef = useRef<ReturnType<typeof setInterval> | null>(null);

  // useCallback gives fetchOrder a stable reference
  // without it, the useEffect below would re-run every render
  const fetchOrder = useCallback(async (): Promise<void> => {
    try {
      setIsLoading(true);
      setError(null);

      const res = await fetch(`/api/orders/${orderId}`);
      if (!res.ok) throw new Error(`Failed to load order: ${res.status}`);

      const data = await res.json() as Order;
      setOrder(data);

    } catch (err: unknown) {
      const message = err instanceof Error ? err.message : 'Failed to load order';
      setError(message);
    } finally {
      setIsLoading(false);
    }
  }, [orderId]);

  // Initial fetch
  useEffect(() => {
    fetchOrder();
  }, [fetchOrder]);

  // Auto-refresh — stop when delivered
  useEffect(() => {
    // Do not start interval if already delivered
    if (order?.status === 'delivered') return;

    intervalRef.current = setInterval(() => {
      fetchOrder();
    }, 30_000); // 30 seconds

    // CLEANUP — runs on unmount or when status becomes 'delivered'
    return () => {
      if (intervalRef.current !== null) {
        clearInterval(intervalRef.current);
      }
    };
  }, [fetchOrder, order?.status]);

  // Typed click handler
  const handleRefreshClick = (e: React.MouseEvent<HTMLButtonElement>): void => {
    e.preventDefault();
    fetchOrder();
  };

  if (isLoading && !order) return <p>Loading order...</p>;
  if (error) return <p>Error: {error}</p>;
  if (!order) return <p>Order not found.</p>;

  return (
    <div>
      <h2>Order #{order.id}</h2>
      <p>Restaurant: {order.restaurantName}</p>
      {/* TypeScript knows status is OrderStatus — autocomplete works */}
      <p>Status: {order.status.replace('_', ' ')}</p>
      <p>ETA: {order.eta}</p>

      {order.status !== 'delivered' && (
        <button onClick={handleRefreshClick} disabled={isLoading}>
          {isLoading ? 'Refreshing...' : 'Refresh Status'}
        </button>
      )}

      {order.status === 'delivered' && (
        <p>Your order has been delivered!</p>
      )}
    </div>
  );
}

export default OrderStatusPage;
```

**What the interviewer is checking:**
Logic: interval cleanup in useEffect return, stop when delivered, manual refresh.
TypeScript: `OrderStatus` union type (prevents invalid status values),
`useRef<ReturnType<typeof setInterval> | null>`, typed click handler.

**TypeScript lesson:**
`ReturnType<typeof setInterval>` is the correct way to type a timer ID.
It resolves to `number` in browsers and `NodeJS.Timeout` in Node.
Using this utility type makes your code work correctly in both environments
without hardcoding either type.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 2: 📝 Form + Validation

> **Level 1 — Foundation**

**What you will build in this pattern:**
- A typed login form with email/password validation and API submission
- A signup form with a typed form state object and real-time validation
- A settings form with typed success/error feedback states

**Interview asks this as:**
- "Build a login form with validation in TypeScript"
- "Create a signup form — validate all fields, type the state"
- "Build a contact form that calls an API and shows typed feedback"

**TypeScript concepts used in this pattern:**
- Typed form state with `interface FormData {}`
- Typed errors with `Record<string, string>` or specific error interface
- Typed event handlers: `React.ChangeEvent<HTMLInputElement>`
- Typed form submit: `React.FormEvent<HTMLFormElement>`
- Status state machine: `'idle' | 'submitting' | 'success' | 'error'`

**Most common mistake in interviews:**
Forgetting `e.preventDefault()` on the submit handler — page reloads and React state is lost.
In TypeScript: leaving the form state as `any` or not typing event handlers.

---

### Pattern 2 · Scenario 1 — The Login Form That Lets Anyone In

🏢 **Situation:**
You are at a SaaS company. The login form submits with blank fields.
Your QA engineer found that completely empty credentials hit the API.
Fix the validation and type everything properly before the security audit.

**Types used in this scenario:**
```typescript
interface LoginFormData {
  email: string;
  password: string;
}

interface LoginFormErrors {
  email?: string;
  password?: string;
}

interface LoginApiResponse {
  token: string;
  user: {
    id: string;
    name: string;
    email: string;
  };
}

type FormStatus = 'idle' | 'submitting' | 'error';
```

**Here is the broken code:**
```tsx
import { useState } from 'react';

function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = async (e) => {   // untyped event
    const res = await fetch('/api/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password }),
    });
    const data = await res.json();      // untyped response
    if (data.token) {
      localStorage.setItem('token', data.token);
      window.location.href = '/dashboard';
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={email} onChange={e => setEmail(e.target.value)}
        placeholder="Email" />
      <input type="password" value={password}
        onChange={e => setPassword(e.target.value)} placeholder="Password" />
      <button type="submit">Login</button>
    </form>
  );
}
```

**Your tasks:**
- ✅ Add `e.preventDefault()` and type the submit handler
- ✅ Add typed validation with `LoginFormErrors` interface
- ✅ Type the API response with `LoginApiResponse`
- ✅ Add `isSubmitting` state and disabled button during submission
- ✅ Show typed inline errors under each field

<details>
<summary>💡 Hint</summary>

Use `React.FormEvent<HTMLFormElement>` to type the submit event.
Use `React.ChangeEvent<HTMLInputElement>` for input changes.
For errors, `Record<keyof LoginFormData, string>` gives you a
typed object where only valid form field names are allowed as keys.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState } from 'react';

interface LoginFormData {
  email: string;
  password: string;
}

// Partial — errors are optional, not all fields will have errors
type LoginFormErrors = Partial<Record<keyof LoginFormData, string>>;

interface LoginApiResponse {
  token: string;
  user: {
    id: string;
    name: string;
    email: string;
  };
}

type FormStatus = 'idle' | 'submitting' | 'error';

function LoginForm(): JSX.Element {
  const [formData, setFormData] = useState<LoginFormData>({
    email: '',
    password: '',
  });
  const [errors, setErrors] = useState<LoginFormErrors>({});
  const [status, setStatus] = useState<FormStatus>('idle');
  const [apiError, setApiError] = useState<string | null>(null);

  // Typed change handler — works for all text inputs
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>): void => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    // Clear error for this field when user starts typing
    setErrors(prev => ({ ...prev, [name]: undefined }));
  };

  const validate = (): LoginFormErrors => {
    const newErrors: LoginFormErrors = {};

    if (!formData.email) {
      newErrors.email = 'Email is required';
    } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.email)) {
      newErrors.email = 'Enter a valid email address';
    }

    if (!formData.password) {
      newErrors.password = 'Password is required';
    } else if (formData.password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }

    return newErrors;
  };

  // Typed submit handler
  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>): Promise<void> => {
    e.preventDefault(); // CRITICAL — stops page reload

    const validationErrors = validate();
    if (Object.keys(validationErrors).length > 0) {
      setErrors(validationErrors);
      return;
    }

    try {
      setStatus('submitting');
      setApiError(null);

      const res = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData),
      });

      // Type the response — TypeScript now knows what data contains
      const data = await res.json() as LoginApiResponse;

      if (!res.ok) {
        throw new Error('Invalid email or password');
      }

      localStorage.setItem('token', data.token);
      window.location.href = '/dashboard';

    } catch (err: unknown) {
      const message = err instanceof Error ? err.message : 'Login failed';
      setApiError(message);
      setStatus('error');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      {apiError && <p style={{ color: 'red' }}>{apiError}</p>}

      <div>
        <input
          name="email"
          type="email"
          placeholder="Email"
          value={formData.email}
          onChange={handleChange}
          disabled={status === 'submitting'}
        />
        {errors.email && <p style={{ color: 'red' }}>{errors.email}</p>}
      </div>

      <div>
        <input
          name="password"
          type="password"
          placeholder="Password"
          value={formData.password}
          onChange={handleChange}
          disabled={status === 'submitting'}
        />
        {errors.password && <p style={{ color: 'red' }}>{errors.password}</p>}
      </div>

      <button type="submit" disabled={status === 'submitting'}>
        {status === 'submitting' ? 'Logging in...' : 'Login'}
      </button>
    </form>
  );
}

export default LoginForm;
```

**What the interviewer is checking:**
Logic: `e.preventDefault()`, validate before API call, disabled during submit.
TypeScript: `React.FormEvent<HTMLFormElement>`, `Partial<Record<keyof LoginFormData, string>>`,
`LoginApiResponse` typed response, `err: unknown` narrowed.

**TypeScript lesson:**
`Partial<Record<keyof LoginFormData, string>>` is a utility type combo.
`keyof LoginFormData` gives `'email' | 'password'`.
`Record<'email' | 'password', string>` maps both to string.
`Partial<...>` makes both optional — since not all fields will have errors at once.

</details>

---

### Pattern 2 · Scenario 2 — The Signup Form That Submits Three Times

🏢 **Situation:**
Users on your e-commerce platform receive three welcome emails after signing up.
The backend says the signup API is called three times per submission.
You find the bug in the code below. Fix it and add proper TypeScript types.

**Types used in this scenario:**
```typescript
interface SignupFormData {
  name: string;
  email: string;
  password: string;
}

interface SignupApiResponse {
  userId: string;
  message: string;
}

type SignupStatus = 'idle' | 'submitting' | 'success' | 'error';
```

**Here is the broken code:**
```tsx
import { useState } from 'react';

function SignupForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = async () => {    // no event parameter — no preventDefault
    if (!name || !email || !password) return;
    const res = await fetch('/api/auth/signup', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ name, email, password }),
    });
    if (res.ok) alert('Account created!');
  };

  return (
    <form>
      <input value={name} onChange={e => setName(e.target.value)} placeholder="Name" />
      <input value={email} onChange={e => setEmail(e.target.value)} placeholder="Email" />
      <input type="password" value={password}
        onChange={e => setPassword(e.target.value)} placeholder="Password" />
      {/* BUG: button with no type inside a form + onClick = double/triple submit */}
      <button onClick={handleSubmit}>Create Account</button>
    </form>
  );
}
```

**Your tasks:**
- ✅ Find and fix why the API is called multiple times
- ✅ Add `SignupFormData` interface and use a single typed form state object
- ✅ Add `isSubmitting` guard typed as `boolean`
- ✅ Show a typed success state after signup

<details>
<summary>💡 Hint</summary>

A button with no `type` attribute inside a `<form>` defaults to `type="submit"`.
When you also add an `onClick` handler AND the form has an `onSubmit`,
all three fire. Fix: put `onSubmit` on the `<form>` and make the button
`type="submit"`. Type the handler as `React.FormEvent<HTMLFormElement>`.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState } from 'react';

interface SignupFormData {
  name: string;
  email: string;
  password: string;
}

interface SignupApiResponse {
  userId: string;
  message: string;
}

type SignupStatus = 'idle' | 'submitting' | 'success' | 'error';

function SignupForm(): JSX.Element {
  // Single typed state object — cleaner than 3 separate useState calls
  const [formData, setFormData] = useState<SignupFormData>({
    name: '',
    email: '',
    password: '',
  });
  const [status, setStatus] = useState<SignupStatus>('idle');
  const [error, setError] = useState<string | null>(null);

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>): void => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  // FIX: typed form submit handler — not button click handler
  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>): Promise<void> => {
    e.preventDefault(); // FIX: stops default form submission

    // FIX: guard against duplicate submissions
    if (status === 'submitting') return;

    const { name, email, password } = formData;
    if (!name || !email || !password) {
      setError('All fields are required');
      return;
    }

    try {
      setStatus('submitting');
      setError(null);

      const res = await fetch('/api/auth/signup', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData),
      });

      if (!res.ok) {
        const data = await res.json() as { message?: string };
        throw new Error(data.message ?? 'Signup failed');
      }

      // Type the success response
      const _data = await res.json() as SignupApiResponse;
      setStatus('success');
      setFormData({ name: '', email: '', password: '' });

    } catch (err: unknown) {
      const message = err instanceof Error ? err.message : 'Signup failed';
      setError(message);
      setStatus('error');
    }
  };

  // Typed success state — replaces the form
  if (status === 'success') {
    return (
      <div>
        <h2>Account Created!</h2>
        <p>Please check your email to verify your account.</p>
      </div>
    );
  }

  return (
    // FIX: onSubmit on form, NOT onClick on button
    <form onSubmit={handleSubmit}>
      {error && <p style={{ color: 'red' }}>{error}</p>}

      <input
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Full Name"
        disabled={status === 'submitting'}
      />
      <input
        name="email"
        type="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
        disabled={status === 'submitting'}
      />
      <input
        name="password"
        type="password"
        value={formData.password}
        onChange={handleChange}
        placeholder="Password"
        disabled={status === 'submitting'}
      />

      {/* FIX: type="submit" — explicit, no onClick needed */}
      <button type="submit" disabled={status === 'submitting'}>
        {status === 'submitting' ? 'Creating Account...' : 'Create Account'}
      </button>
    </form>
  );
}

export default SignupForm;
```

**What the interviewer is checking:**
Logic: `onSubmit` on form not `onClick` on button, `status === 'submitting'` guard.
TypeScript: `React.FormEvent<HTMLFormElement>`, `SignupFormData` for state,
`SignupStatus` union type instead of boolean flags.

**TypeScript lesson:**
`SignupStatus = 'idle' | 'submitting' | 'success' | 'error'` is a discriminated union
used as a state machine. It is strictly better than boolean flags like `isLoading`, `isError`
because you cannot be in two states at once — TypeScript enforces that.

</details>

---

### Pattern 2 · Scenario 3 — The Form That Loses Your Data

🏢 **Situation:**
You are building a job portal. Candidates fill a long application form.
If they accidentally close the tab, all their work is lost.
Your PM wants auto-save with localStorage. Type the whole feature properly.

**Types used in this scenario:**
```typescript
interface ApplicationFormData {
  fullName: string;
  coverLetter: string;
  experience: string;
  skills: string;
}

interface ApplicationDraft {
  data: ApplicationFormData;
  savedAt: string;
}

interface JobApplicationFormProps {
  jobId: string;
}
```

**Here is the starter code:**
```tsx
import { useState } from 'react';

function JobApplicationForm({ jobId }) {   // untyped props
  const [formData, setFormData] = useState({   // untyped state
    fullName: '',
    coverLetter: '',
    experience: '',
    skills: '',
  });

  const handleChange = (e) => {   // untyped event
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = async (e) => {   // untyped event
    e.preventDefault();
    await fetch(`/api/jobs/${jobId}/apply`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(formData),
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="fullName" value={formData.fullName} onChange={handleChange} />
      <textarea name="coverLetter" value={formData.coverLetter} onChange={handleChange} />
      <textarea name="experience" value={formData.experience} onChange={handleChange} />
      <input name="skills" value={formData.skills} onChange={handleChange} />
      <button type="submit">Submit Application</button>
    </form>
  );
}
```

**Your tasks:**
- ✅ Type all props, state, and event handlers
- ✅ Auto-save draft to localStorage as the user types (debounced)
- ✅ Restore draft from localStorage on mount (with typed parse)
- ✅ Clear the draft after successful submission

<details>
<summary>💡 Hint</summary>

Use `React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>` to type
the change handler — it handles both `<input>` and `<textarea>` elements.
For localStorage, use a typed parse helper that takes a type parameter
and returns `T | null` — this is the safe pattern for reading storage.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

interface ApplicationFormData {
  fullName: string;
  coverLetter: string;
  experience: string;
  skills: string;
}

interface ApplicationDraft {
  data: ApplicationFormData;
  savedAt: string;
}

interface JobApplicationFormProps {
  jobId: string;
}

// Generic typed localStorage reader — reusable utility
function getFromStorage<T>(key: string): T | null {
  try {
    const item = localStorage.getItem(key);
    return item ? (JSON.parse(item) as T) : null;
  } catch {
    return null;
  }
}

const EMPTY_FORM: ApplicationFormData = {
  fullName: '',
  coverLetter: '',
  experience: '',
  skills: '',
};

function JobApplicationForm({ jobId }: JobApplicationFormProps): JSX.Element {
  const DRAFT_KEY = `application-draft-${jobId}`;

  // Restore draft from localStorage on mount
  const [formData, setFormData] = useState<ApplicationFormData>(() => {
    const draft = getFromStorage<ApplicationDraft>(DRAFT_KEY);
    return draft?.data ?? EMPTY_FORM;
  });

  const [savedAt, setSavedAt] = useState<string | null>(null);
  const [isSubmitting, setIsSubmitting] = useState<boolean>(false);
  const [submitError, setSubmitError] = useState<string | null>(null);

  // Auto-save with debounce — saves 1 second after user stops typing
  useEffect(() => {
    const timer = setTimeout(() => {
      const draft: ApplicationDraft = {
        data: formData,
        savedAt: new Date().toLocaleTimeString(),
      };
      localStorage.setItem(DRAFT_KEY, JSON.stringify(draft));
      setSavedAt(draft.savedAt);
    }, 1000);

    return () => clearTimeout(timer); // cancel if user types again
  }, [formData, DRAFT_KEY]);

  // Typed for both input and textarea
  const handleChange = (
    e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
  ): void => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>): Promise<void> => {
    e.preventDefault();

    try {
      setIsSubmitting(true);
      setSubmitError(null);

      const res = await fetch(`/api/jobs/${jobId}/apply`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData),
      });

      if (!res.ok) throw new Error('Submission failed. Please try again.');

      // Clear draft on success
      localStorage.removeItem(DRAFT_KEY);
      setSavedAt(null);
      alert('Application submitted!');

    } catch (err: unknown) {
      const message = err instanceof Error ? err.message : 'Submission failed';
      setSubmitError(message);
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      {savedAt && (
        <p style={{ color: 'green', fontSize: '12px' }}>
          Draft saved at {savedAt}
        </p>
      )}
      {submitError && <p style={{ color: 'red' }}>{submitError}</p>}

      <input
        name="fullName"
        value={formData.fullName}
        onChange={handleChange}
        placeholder="Full Name"
        disabled={isSubmitting}
      />
      <textarea
        name="coverLetter"
        value={formData.coverLetter}
        onChange={handleChange}
        placeholder="Cover Letter"
        disabled={isSubmitting}
      />
      <textarea
        name="experience"
        value={formData.experience}
        onChange={handleChange}
        placeholder="Work Experience"
        disabled={isSubmitting}
      />
      <input
        name="skills"
        value={formData.skills}
        onChange={handleChange}
        placeholder="Skills (comma separated)"
        disabled={isSubmitting}
      />

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Submitting...' : 'Submit Application'}
      </button>
    </form>
  );
}

export default JobApplicationForm;
```

**What the interviewer is checking:**
Logic: debounced auto-save, restore on mount, clear on submit success.
TypeScript: `React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>` union type,
generic `getFromStorage<T>` utility, `ApplicationDraft` interface for typed storage.

**TypeScript lesson:**
`React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>` is a union type for
event handlers that work on both inputs and textareas. This is more precise than
using `React.ChangeEvent<HTMLElement>` which would lose type information about
`.target.value` and `.target.name`.

</details>

---

### Pattern 2 · Scenario 4 — The Form With No Feedback

🏢 **Situation:**
You are at a healthcare portal. Patients submit appointment requests and nothing
happens visually — no spinner, no confirmation, no error. They click 5 times.
Your backend gets 5 duplicate appointments. Fix the UX and type everything
with a proper status state machine.

**Types used in this scenario:**
```typescript
interface AppointmentFormData {
  doctorId: string;
  date: string;
  reason: string;
}

interface AppointmentConfirmation {
  appointmentId: string;
  doctorName: string;
  date: string;
  confirmationNumber: string;
}

type AppointmentFormStatus = 'idle' | 'submitting' | 'success' | 'error';
```

**Here is the starter code:**
```tsx
import { useState } from 'react';

function AppointmentForm() {
  const [doctorId, setDoctorId] = useState('');
  const [date, setDate] = useState('');
  const [reason, setReason] = useState('');

  const handleSubmit = async (e) => {  // untyped
    e.preventDefault();
    await fetch('/api/appointments', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ doctorId, date, reason }),
    });
    // No feedback — user does not know if it worked
  };

  return (
    <form onSubmit={handleSubmit}>
      <select value={doctorId} onChange={e => setDoctorId(e.target.value)}>
        <option value="">Select Doctor</option>
        <option value="d1">Dr. Smith</option>
        <option value="d2">Dr. Jones</option>
      </select>
      <input type="date" value={date} onChange={e => setDate(e.target.value)} />
      <textarea value={reason} onChange={e => setReason(e.target.value)} />
      <button type="submit">Book Appointment</button>
    </form>
  );
}
```

**Your tasks:**
- ✅ Refactor to use typed `AppointmentFormData` state object
- ✅ Add `AppointmentFormStatus` state machine — no boolean flags
- ✅ Show the `AppointmentConfirmation` data in a typed success state
- ✅ Disable the submit button during submission
- ✅ Add basic typed validation

<details>
<summary>💡 Hint</summary>

Use `React.ChangeEvent<HTMLSelectElement>` for the select handler —
it is different from `HTMLInputElement`. For the status state machine,
once `status === 'success'` you can safely narrow to show
the confirmation — TypeScript can help if you store confirmation
separately as `AppointmentConfirmation | null`.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState } from 'react';

interface AppointmentFormData {
  doctorId: string;
  date: string;
  reason: string;
}

interface AppointmentConfirmation {
  appointmentId: string;
  doctorName: string;
  date: string;
  confirmationNumber: string;
}

type AppointmentFormStatus = 'idle' | 'submitting' | 'success' | 'error';

const EMPTY_FORM: AppointmentFormData = {
  doctorId: '',
  date: '',
  reason: '',
};

function AppointmentForm(): JSX.Element {
  const [formData, setFormData] = useState<AppointmentFormData>(EMPTY_FORM);
  const [status, setStatus] = useState<AppointmentFormStatus>('idle');
  const [errorMessage, setErrorMessage] = useState<string | null>(null);
  const [confirmation, setConfirmation] = useState<AppointmentConfirmation | null>(null);

  // Typed handler for input and textarea
  const handleInputChange = (
    e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
  ): void => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  // Typed handler for select — different element type
  const handleSelectChange = (e: React.ChangeEvent<HTMLSelectElement>): void => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const validate = (): string | null => {
    if (!formData.doctorId) return 'Please select a doctor';
    if (!formData.date) return 'Please select a date';
    if (!formData.reason.trim()) return 'Please describe your reason for visiting';
    return null;
  };

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>): Promise<void> => {
    e.preventDefault();

    const validationError = validate();
    if (validationError) {
      setErrorMessage(validationError);
      return;
    }

    try {
      setStatus('submitting');
      setErrorMessage(null);

      const res = await fetch('/api/appointments', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData),
      });

      if (!res.ok) {
        const data = await res.json() as { message?: string };
        throw new Error(data.message ?? 'Booking failed. Please try again.');
      }

      // Type the confirmation response
      const data = await res.json() as AppointmentConfirmation;
      setConfirmation(data);
      setStatus('success');

    } catch (err: unknown) {
      const message = err instanceof Error ? err.message : 'Booking failed';
      setErrorMessage(message);
      setStatus('error');
    }
  };

  // Success state — TypeScript knows confirmation is not null here
  if (status === 'success' && confirmation !== null) {
    return (
      <div>
        <h2>Appointment Confirmed!</h2>
        <p>Doctor: {confirmation.doctorName}</p>
        <p>Date: {confirmation.date}</p>
        <p>Confirmation #: {confirmation.confirmationNumber}</p>
        <button
          onClick={() => {
            setStatus('idle');
            setFormData(EMPTY_FORM);
            setConfirmation(null);
          }}
        >
          Book Another
        </button>
      </div>
    );
  }

  return (
    <form onSubmit={handleSubmit}>
      {errorMessage && (
        <p style={{ color: 'red' }}>{errorMessage}</p>
      )}

      <select
        name="doctorId"
        value={formData.doctorId}
        onChange={handleSelectChange}
        disabled={status === 'submitting'}
      >
        <option value="">Select Doctor</option>
        <option value="d1">Dr. Smith</option>
        <option value="d2">Dr. Jones</option>
      </select>

      <input
        type="date"
        name="date"
        value={formData.date}
        onChange={handleInputChange}
        min={new Date().toISOString().split('T')[0]}
        disabled={status === 'submitting'}
      />

      <textarea
        name="reason"
        value={formData.reason}
        onChange={handleInputChange}
        placeholder="Reason for visit"
        disabled={status === 'submitting'}
      />

      <button type="submit" disabled={status === 'submitting'}>
        {status === 'submitting' ? 'Booking...' : 'Book Appointment'}
      </button>
    </form>
  );
}

export default AppointmentForm;
```

**What the interviewer is checking:**
Logic: status state machine, disabled during submit, success state replaces form.
TypeScript: `React.ChangeEvent<HTMLSelectElement>` vs `HTMLInputElement`,
`AppointmentConfirmation | null` typed separately from status.

**TypeScript lesson:**
`React.ChangeEvent<HTMLSelectElement>` is distinct from `React.ChangeEvent<HTMLInputElement>`.
Trying to use one handler for both with `HTMLInputElement | HTMLTextAreaElement` works —
but `HTMLSelectElement` is not included because select elements behave differently
(no `type` attribute, different value handling). Separate handlers make this explicit.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 3: 🔁 List + Search + Filter

> **Level 1 — Foundation**

**What you will build in this pattern:**
- A typed product listing with search input and category filter working together
- A typed employee directory with search, department filter, and sort
- A job board with multiple typed simultaneous filters

**Interview asks this as:**
- "Add typed search and filter to this product list"
- "Build a filterable table — all state and props in TypeScript"
- "Make this list searchable by name and filterable by status"

**TypeScript concepts used in this pattern:**
- Generic `Array.filter()` with typed predicates
- `useMemo` with typed return value
- `Set` constructor for typed unique value extraction
- Typed select and input change handlers
- Computed derived state typed explicitly

**Most common mistake in interviews:**
Storing filtered results in state instead of computing them.
In TypeScript: mapping over an untyped array and losing autocomplete.

---

### Pattern 3 · Scenario 1 — The Product Page Nobody Can Search

🏢 **Situation:**
You are building an e-commerce site. 120 products load fine.
Users cannot find anything — no search, no filter.
Your PM says ship search + filter by category by end of day.
Build it with full TypeScript types.

**Types used in this scenario:**
```typescript
interface Product {
  id: string;
  name: string;
  price: number;
  category: string;
  image: string;
  inStock: boolean;
}
```

**Here is the starter code:**
```tsx
import { useState, useEffect } from 'react';

function ProductListing() {
  const [products, setProducts] = useState([]);   // untyped
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    const load = async () => {
      const res = await fetch('/api/products');
      const data = await res.json();
      setProducts(data);
      setIsLoading(false);
    };
    load();
  }, []);

  if (isLoading) return <p>Loading products...</p>;

  return (
    <div>
      <h1>Products</h1>
      {products.map(product => (    // product is any
        <div key={product.id}>
          <h3>{product.name}</h3>
          <p>₹{product.price}</p>
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Type `products` as `Product[]` and add the `Product` interface
- ✅ Add a typed search input that filters by name
- ✅ Add a typed category select that filters by category
- ✅ Both filters must work together — derived state, not stored state

<details>
<summary>💡 Hint</summary>

The filtered list is derived state — compute it with `useMemo`.
Type the `useMemo` return explicitly: `useMemo<Product[]>(() => ..., [])`.
For categories, extract unique values: `[...new Set(products.map(p => p.category))]`
— TypeScript will infer this as `string[]` automatically.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useMemo } from 'react';

interface Product {
  id: string;
  name: string;
  price: number;
  category: string;
  image: string;
  inStock: boolean;
}

function ProductListing(): JSX.Element {
  // Typed state — TypeScript knows what's in the array
  const [products, setProducts] = useState<Product[]>([]);
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);
  const [searchTerm, setSearchTerm] = useState<string>('');
  const [selectedCategory, setSelectedCategory] = useState<string>('all');

  useEffect(() => {
    const load = async (): Promise<void> => {
      try {
        const res = await fetch('/api/products');
        if (!res.ok) throw new Error('Failed to load products');
        const data = await res.json() as Product[];
        setProducts(data);
      } catch (err: unknown) {
        setError(err instanceof Error ? err.message : 'Unknown error');
      } finally {
        setIsLoading(false);
      }
    };
    load();
  }, []);

  // DERIVED STATE — typed with useMemo, never stored in state
  const filteredProducts = useMemo<Product[]>(() => {
    return products.filter((product: Product) => {
      const matchesSearch = product.name
        .toLowerCase()
        .includes(searchTerm.toLowerCase());

      const matchesCategory =
        selectedCategory === 'all' || product.category === selectedCategory;

      return matchesSearch && matchesCategory;
    });
  }, [products, searchTerm, selectedCategory]);

  // Typed unique category list from actual data
  const categories: string[] = useMemo(
    () => ['all', ...new Set(products.map((p: Product) => p.category))],
    [products]
  );

  const handleSearchChange = (e: React.ChangeEvent<HTMLInputElement>): void => {
    setSearchTerm(e.target.value);
  };

  const handleCategoryChange = (e: React.ChangeEvent<HTMLSelectElement>): void => {
    setSelectedCategory(e.target.value);
  };

  if (isLoading) return <p>Loading products...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <h1>Products</h1>

      <input
        type="text"
        placeholder="Search products..."
        value={searchTerm}
        onChange={handleSearchChange}
      />

      <select value={selectedCategory} onChange={handleCategoryChange}>
        {categories.map((cat: string) => (
          <option key={cat} value={cat}>
            {cat === 'all' ? 'All Categories' : cat}
          </option>
        ))}
      </select>

      <p>{filteredProducts.length} of {products.length} products</p>

      {filteredProducts.length === 0 ? (
        <p>No products match your search.</p>
      ) : (
        <div className="product-grid">
          {filteredProducts.map((product: Product) => (
            <div key={product.id}>
              <img src={product.image} alt={product.name} />
              <h3>{product.name}</h3>
              {/* TypeScript knows price is number — toLocaleString is safe */}
              <p>₹{product.price.toLocaleString()}</p>
              <p>{product.category}</p>
              {/* TypeScript knows inStock is boolean */}
              {!product.inStock && <span>Out of Stock</span>}
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

export default ProductListing;
```

**What the interviewer is checking:**
Logic: derived `filteredProducts` via `useMemo`, both filters working together.
TypeScript: `useState<Product[]>`, typed `useMemo<Product[]>`,
`React.ChangeEvent<HTMLInputElement>` vs `React.ChangeEvent<HTMLSelectElement>`.

**TypeScript lesson:**
`useMemo<Product[]>(() => products.filter(...), [products, searchTerm, selectedCategory])`
explicitly types the return value of memo. Without the generic, TypeScript infers it,
but being explicit makes the intent clear and prevents accidental type widening.

</details>

---

### Pattern 3 · Scenario 2 — The Employee List That Filters Wrong

🏢 **Situation:**
You are at an HR tool. The employee directory has a department filter.
Selecting "Engineering" works. But typing in search after that searches
the full list — ignoring the department filter. The bug is in how
filter state is managed. Fix it with TypeScript.

**Types used in this scenario:**
```typescript
interface Employee {
  id: string;
  name: string;
  department: string;
  email: string;
  position: string;
}

type Department = 'all' | 'engineering' | 'design' | 'marketing' | 'hr';
```

**Here is the broken code:**
```tsx
import { useState, useEffect } from 'react';

function EmployeeDirectory() {
  const [employees, setEmployees] = useState<any[]>([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [department, setDepartment] = useState('all');
  // BUG: storing filtered results in state
  const [displayedEmployees, setDisplayedEmployees] = useState<any[]>([]);

  useEffect(() => {
    const load = async () => {
      const res = await fetch('/api/employees');
      const data = await res.json();
      setEmployees(data);
      setDisplayedEmployees(data);
    };
    load();
  }, []);

  const handleSearch = (e: React.ChangeEvent<HTMLInputElement>) => {
    const term = e.target.value;
    setSearchTerm(term);
    // BUG: ignores department filter
    setDisplayedEmployees(employees.filter(emp =>
      emp.name.toLowerCase().includes(term.toLowerCase())
    ));
  };

  const handleDepartment = (e: React.ChangeEvent<HTMLSelectElement>) => {
    const dept = e.target.value;
    setDepartment(dept);
    // BUG: ignores search term
    setDisplayedEmployees(
      dept === 'all' ? employees : employees.filter(emp => emp.department === dept)
    );
  };

  return (
    <div>
      <input value={searchTerm} onChange={handleSearch} placeholder="Search..." />
      <select value={department} onChange={handleDepartment}>
        <option value="all">All Departments</option>
        <option value="engineering">Engineering</option>
        <option value="design">Design</option>
        <option value="marketing">Marketing</option>
      </select>
      {displayedEmployees.map(emp => (   // emp is any
        <div key={emp.id}>{emp.name} — {emp.department}</div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Remove `displayedEmployees` state — replace with derived computation
- ✅ Fix types: `any[]` → `Employee[]`, `Department` union type
- ✅ Make both filters work together
- ✅ Add a typed "Clear filters" button

<details>
<summary>💡 Hint</summary>

Remove `displayedEmployees` entirely. Remove `setDisplayedEmployees` from both handlers.
Make handlers only update `searchTerm` and `department` state.
Then compute `filteredEmployees` as a `useMemo` that reads both state values.
Type `department` as `Department` — TypeScript will catch invalid values.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useMemo } from 'react';

interface Employee {
  id: string;
  name: string;
  department: string;
  email: string;
  position: string;
}

// Union type — only valid department values allowed
type Department = 'all' | 'engineering' | 'design' | 'marketing' | 'hr';

function EmployeeDirectory(): JSX.Element {
  // FIX: properly typed array, no any
  const [employees, setEmployees] = useState<Employee[]>([]);
  const [isLoading, setIsLoading] = useState<boolean>(true);
  // FIX: typed department using the union type
  const [searchTerm, setSearchTerm] = useState<string>('');
  const [department, setDepartment] = useState<Department>('all');

  // REMOVED: displayedEmployees state — that was the bug

  useEffect(() => {
    const load = async (): Promise<void> => {
      try {
        const res = await fetch('/api/employees');
        if (!res.ok) throw new Error('Failed to load');
        const data = await res.json() as Employee[];
        setEmployees(data);
      } finally {
        setIsLoading(false);
      }
    };
    load();
  }, []);

  // SINGLE computed result — both filters always applied together
  const filteredEmployees = useMemo<Employee[]>(() => {
    return employees.filter((emp: Employee) => {
      const matchesSearch = emp.name
        .toLowerCase()
        .includes(searchTerm.toLowerCase());

      const matchesDept =
        department === 'all' || emp.department === department;

      return matchesSearch && matchesDept;
    });
  }, [employees, searchTerm, department]);

  const hasActiveFilters: boolean = searchTerm !== '' || department !== 'all';

  const clearFilters = (): void => {
    setSearchTerm('');
    setDepartment('all');
  };

  const handleSearchChange = (e: React.ChangeEvent<HTMLInputElement>): void => {
    // FIX: only update searchTerm — no manual filtering
    setSearchTerm(e.target.value);
  };

  const handleDepartmentChange = (e: React.ChangeEvent<HTMLSelectElement>): void => {
    // FIX: only update department — no manual filtering
    // Type assertion needed: select value is string, but we know it's Department
    setDepartment(e.target.value as Department);
  };

  if (isLoading) return <p>Loading employees...</p>;

  return (
    <div>
      <input
        value={searchTerm}
        onChange={handleSearchChange}
        placeholder="Search by name..."
      />

      <select value={department} onChange={handleDepartmentChange}>
        <option value="all">All Departments</option>
        <option value="engineering">Engineering</option>
        <option value="design">Design</option>
        <option value="marketing">Marketing</option>
        <option value="hr">HR</option>
      </select>

      {hasActiveFilters && (
        <button onClick={clearFilters}>Clear Filters</button>
      )}

      <p>
        Showing {filteredEmployees.length} of {employees.length} employees
      </p>

      {filteredEmployees.length === 0 ? (
        <p>No employees match your filters.</p>
      ) : (
        filteredEmployees.map((emp: Employee) => (
          <div key={emp.id}>
            <strong>{emp.name}</strong> — {emp.department}
            <span style={{ color: '#666' }}> ({emp.position})</span>
          </div>
        ))
      )}
    </div>
  );
}

export default EmployeeDirectory;
```

**What the interviewer is checking:**
Logic: `displayedEmployees` state removed, both filters computed together.
TypeScript: `Department` union type, `e.target.value as Department` type assertion
explained, `Employee[]` replacing `any[]`.

**TypeScript lesson:**
`setDepartment(e.target.value as Department)` uses a type assertion.
This is acceptable here because the select only has `Department` values as options —
the HTML constrains what `e.target.value` can be. The assertion documents this intent.
For stricter safety you could validate: `if (['all','engineering',...].includes(e.target.value))`.

</details>

---

### Pattern 3 · Scenario 3 — The Job Board That Sorts Incorrectly

🏢 **Situation:**
You are at a job portal. The listings have a sort dropdown.
After sorting, searching by company name shows nothing.
The bug: `setJobs(sorted)` overwrites the original data.
Fix it and add TypeScript's help to prevent future mutations.

**Types used in this scenario:**
```typescript
interface Job {
  id: string;
  title: string;
  company: string;
  salary: number;
  postedAt: string;
  type: 'full-time' | 'part-time' | 'contract';
}

type SortOption = 'newest' | 'oldest' | 'salary_high' | 'salary_low';
```

**Here is the broken code:**
```tsx
import { useState, useEffect } from 'react';

function JobBoard() {
  const [jobs, setJobs] = useState<any[]>([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [sortBy, setSortBy] = useState('newest');

  useEffect(() => {
    const load = async () => {
      const res = await fetch('/api/jobs');
      setJobs(await res.json());
    };
    load();
  }, []);

  const handleSort = (e: React.ChangeEvent<HTMLSelectElement>) => {
    const value = e.target.value;
    setSortBy(value);
    // BUG: sorts and overwrites original data — search breaks
    const sorted = [...jobs].sort((a, b) => {
      if (value === 'newest') return new Date(b.postedAt).getTime() - new Date(a.postedAt).getTime();
      if (value === 'salary') return b.salary - a.salary;
      return 0;
    });
    setJobs(sorted);   // destroys original order
  };

  const filteredJobs = jobs.filter(job =>
    job.company.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    <div>
      <input value={searchTerm} onChange={e => setSearchTerm(e.target.value)}
        placeholder="Search by company..." />
      <select value={sortBy} onChange={handleSort}>
        <option value="newest">Newest First</option>
        <option value="oldest">Oldest First</option>
        <option value="salary_high">Salary High to Low</option>
      </select>
      {filteredJobs.map(job => (
        <div key={job.id}>
          <h3>{job.title}</h3>
          <p>{job.company} — ₹{job.salary}</p>
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the sort bug — never call `setJobs(sorted)`
- ✅ Replace `any[]` with `Job[]` and `SortOption` union type
- ✅ Use `useMemo` to compute the filtered + sorted result
- ✅ Add `readonly` to the `Job` interface to prevent mutation

<details>
<summary>💡 Hint</summary>

`sortBy` should just be state. The sort logic moves into `useMemo`
alongside the filter logic. The sort comparator reads `sortBy` state directly.
Add `readonly` to every field in the `Job` interface — TypeScript will
then error if any code tries to mutate a job object directly.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useMemo } from 'react';

// readonly fields — TypeScript prevents direct mutation of job objects
interface Job {
  readonly id: string;
  readonly title: string;
  readonly company: string;
  readonly salary: number;
  readonly postedAt: string;
  readonly type: 'full-time' | 'part-time' | 'contract';
}

type SortOption = 'newest' | 'oldest' | 'salary_high' | 'salary_low';

function JobBoard(): JSX.Element {
  // FIX: properly typed, readonly objects
  const [jobs, setJobs] = useState<readonly Job[]>([]);
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [searchTerm, setSearchTerm] = useState<string>('');
  // FIX: typed as SortOption — not plain string
  const [sortBy, setSortBy] = useState<SortOption>('newest');

  useEffect(() => {
    const load = async (): Promise<void> => {
      try {
        const res = await fetch('/api/jobs');
        if (!res.ok) throw new Error('Failed to load jobs');
        const data = await res.json() as Job[];
        setJobs(data);
      } finally {
        setIsLoading(false);
      }
    };
    load();
  }, []);

  const handleSortChange = (e: React.ChangeEvent<HTMLSelectElement>): void => {
    // FIX: only update sortBy — no manual sorting, no setJobs
    setSortBy(e.target.value as SortOption);
  };

  // Step 1: filter. Step 2: sort. Both derived from original jobs.
  const processedJobs = useMemo<Job[]>(() => {
    const filtered = jobs.filter((job: Job) => {
      const term = searchTerm.toLowerCase();
      return (
        job.company.toLowerCase().includes(term) ||
        job.title.toLowerCase().includes(term)
      );
    });

    // Sort the filtered result — using SortOption union for exhaustive check
    return [...filtered].sort((a: Job, b: Job): number => {
      switch (sortBy) {
        case 'newest':
          return new Date(b.postedAt).getTime() - new Date(a.postedAt).getTime();
        case 'oldest':
          return new Date(a.postedAt).getTime() - new Date(b.postedAt).getTime();
        case 'salary_high':
          return b.salary - a.salary;
        case 'salary_low':
          return a.salary - b.salary;
        // TypeScript exhaustiveness — no default needed if all cases covered
      }
    });
  }, [jobs, searchTerm, sortBy]);

  if (isLoading) return <p>Loading jobs...</p>;

  return (
    <div>
      <input
        value={searchTerm}
        onChange={e => setSearchTerm(e.target.value)}
        placeholder="Search by company or title..."
      />

      <select value={sortBy} onChange={handleSortChange}>
        <option value="newest">Newest First</option>
        <option value="oldest">Oldest First</option>
        <option value="salary_high">Salary High to Low</option>
        <option value="salary_low">Salary Low to High</option>
      </select>

      <p>{processedJobs.length} jobs found</p>

      {processedJobs.map((job: Job) => (
        <div key={job.id}>
          <h3>{job.title}</h3>
          {/* TypeScript knows type is the union — safe to use directly */}
          <span>{job.type}</span>
          <p>{job.company} — ₹{job.salary.toLocaleString()}</p>
          <p>Posted: {new Date(job.postedAt).toLocaleDateString()}</p>
        </div>
      ))}
    </div>
  );
}

export default JobBoard;
```

**What the interviewer is checking:**
Logic: `setJobs(sorted)` removed, filter and sort both in `useMemo`.
TypeScript: `readonly` fields on `Job` prevent mutation, `SortOption` union type,
`switch` exhaustiveness with all cases covered.

**TypeScript lesson:**
`readonly` on interface fields makes TypeScript error if any code tries to do
`job.salary = 0` or similar mutations. This is excellent for API response types —
data from the server should never be mutated directly. Combined with `useState<readonly Job[]>`,
the entire data flow is mutation-safe.

</details>

---

### Pattern 3 · Scenario 4 — The Search That Breaks on Special Characters

🏢 **Situation:**
You are at a SaaS company. The customer directory search crashes with
"Invalid regular expression" when users type `(`, `+`, or `[`.
A customer named "O'Brien" cannot find themselves.
Fix the crash and add debounce — all typed correctly.

**Types used in this scenario:**
```typescript
interface Customer {
  id: string;
  name: string;
  email: string;
  plan: 'free' | 'pro' | 'enterprise';
  createdAt: string;
}

interface CustomerSearchProps {
  customers: Customer[];
}

interface HighlightedTextProps {
  text: string;
  highlight: string;
}
```

**Here is the broken code:**
```tsx
import { useState } from 'react';

function CustomerSearch({ customers }) {   // untyped props
  const [searchTerm, setSearchTerm] = useState('');

  const filtered = customers.filter(customer => {
    // CRASH: user input used directly in RegExp
    const regex = new RegExp(searchTerm, 'i');
    return regex.test(customer.name) || regex.test(customer.email);
  });

  return (
    <div>
      <input value={searchTerm} onChange={e => setSearchTerm(e.target.value)}
        placeholder="Search customers..." />
      {filtered.map(customer => (    // customer is any
        <div key={customer.id}>
          <strong>{customer.name}</strong>
          <span>{customer.email}</span>
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Type the component with `CustomerSearchProps`
- ✅ Fix the regex crash — use `.includes()` instead
- ✅ Add typed debounce (two states: `inputValue` and `searchTerm`)
- ✅ Build a typed `HighlightedText` component that highlights matches

<details>
<summary>💡 Hint</summary>

Two separate state values: `inputValue` (updates on every keystroke, controls the input)
and `searchTerm` (updates 300ms after typing stops, used for filtering).
The `HighlightedText` component takes `HighlightedTextProps` and returns `JSX.Element`.
Use `.includes()` for safe string search — it treats every character as a literal.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useMemo } from 'react';

interface Customer {
  id: string;
  name: string;
  email: string;
  plan: 'free' | 'pro' | 'enterprise';
  createdAt: string;
}

interface CustomerSearchProps {
  customers: Customer[];
}

interface HighlightedTextProps {
  text: string;
  highlight: string;
}

// Typed sub-component for text highlighting
function HighlightedText({ text, highlight }: HighlightedTextProps): JSX.Element {
  if (!highlight.trim()) {
    return <span>{text}</span>;
  }

  const index = text.toLowerCase().indexOf(highlight.toLowerCase());

  if (index === -1) {
    return <span>{text}</span>;
  }

  return (
    <span>
      {text.slice(0, index)}
      <mark>{text.slice(index, index + highlight.length)}</mark>
      {text.slice(index + highlight.length)}
    </span>
  );
}

function CustomerSearch({ customers }: CustomerSearchProps): JSX.Element {
  // Two separate states for debounce
  const [inputValue, setInputValue] = useState<string>('');    // controls input
  const [searchTerm, setSearchTerm] = useState<string>('');    // used for filtering

  // Debounce — update searchTerm 300ms after user stops typing
  useEffect(() => {
    const timer = setTimeout(() => {
      setSearchTerm(inputValue);
    }, 300);

    return () => clearTimeout(timer);
  }, [inputValue]);

  // FIX: use .includes() — safe with ALL characters, no regex
  const filteredCustomers = useMemo<Customer[]>(() => {
    if (!searchTerm.trim()) return customers;

    const term = searchTerm.toLowerCase();
    return customers.filter((customer: Customer) =>
      customer.name.toLowerCase().includes(term) ||
      customer.email.toLowerCase().includes(term)
    );
  }, [customers, searchTerm]);

  const handleInputChange = (e: React.ChangeEvent<HTMLInputElement>): void => {
    setInputValue(e.target.value);
  };

  return (
    <div>
      <input
        value={inputValue}
        onChange={handleInputChange}
        placeholder="Search customers..."
      />

      <p>{filteredCustomers.length} customers found</p>

      {filteredCustomers.length === 0 && searchTerm ? (
        <p>No customers match "{searchTerm}"</p>
      ) : (
        filteredCustomers.map((customer: Customer) => (
          <div key={customer.id}>
            <strong>
              {/* Typed highlight component */}
              <HighlightedText text={customer.name} highlight={searchTerm} />
            </strong>
            <span>
              <HighlightedText text={customer.email} highlight={searchTerm} />
            </span>
            {/* TypeScript knows plan is the union — no typos possible */}
            <span className={`badge badge-${customer.plan}`}>
              {customer.plan}
            </span>
          </div>
        ))
      )}
    </div>
  );
}

export default CustomerSearch;
```

**What the interviewer is checking:**
Logic: `.includes()` instead of `RegExp`, debounce with two state values.
TypeScript: `CustomerSearchProps` interface, `HighlightedTextProps` typed component,
`Customer['plan']` union type for badge class.

**TypeScript lesson:**
The `HighlightedText` sub-component with `HighlightedTextProps` shows how to build
small, well-typed utility components. Having an explicit props interface makes the
component self-documenting — any caller knows exactly what is required.
It also prevents accidentally passing a number as `text` or `highlight`.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 4: 💾 localStorage Usage

> **Level 1 — Foundation**

**What you will build in this pattern:**
- A typed shopping cart that persists across page refreshes
- A settings page with typed preferences that restore on reload
- A typed draft saver that handles storage errors gracefully

**Interview asks this as:**
- "Make this cart persist on refresh — with TypeScript types"
- "Save user preferences to localStorage — type the storage helpers"
- "Build a typed localStorage utility that handles all error cases"

**TypeScript concepts used in this pattern:**
- Generic typed `localStorage` helpers: `getFromStorage<T>()`
- `useState` with lazy initializer function for reading storage
- `useEffect` with typed deps for saving on change
- Type assertions with `as T` for parsed JSON
- Try/catch with `unknown` error type

**Most common mistake in interviews:**
Not wrapping `JSON.parse()` in try/catch.
In TypeScript: using `any` for the parsed value instead of a generic or specific type.

---

### Pattern 4 · Scenario 1 — The Cart That Empties on Refresh

🏢 **Situation:**
You are at an e-commerce startup. Users add items to the cart and refresh the page —
everything is gone. You are losing sales. Fix cart persistence with
full TypeScript types for the cart items and storage utilities.

**Types used in this scenario:**
```typescript
interface Product {
  id: string;
  name: string;
  price: number;
  image: string;
}

interface CartItem extends Product {
  quantity: number;
}
```

**Here is the starter code:**
```tsx
import { useState } from 'react';

function ShoppingCart() {
  const [cartItems, setCartItems] = useState([]);   // untyped, not persisted

  const addItem = (product) => {   // untyped param
    setCartItems(prev => {
      const exists = prev.find(item => item.id === product.id);
      if (exists) {
        return prev.map(item =>
          item.id === product.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...prev, { ...product, quantity: 1 }];
    });
  };

  const removeItem = (productId) => {
    setCartItems(prev => prev.filter(item => item.id !== productId));
  };

  return (
    <div>
      {cartItems.map(item => (
        <div key={item.id}>
          <span>{item.name}</span>
          <button onClick={() => removeItem(item.id)}>Remove</button>
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Build a generic `getFromStorage<T>` utility with try/catch
- ✅ Initialize `cartItems` from localStorage using lazy `useState`
- ✅ Save to localStorage whenever `cartItems` changes
- ✅ Type `addItem` and `removeItem` with proper param types

<details>
<summary>💡 Hint</summary>

A generic storage helper: `function getFromStorage<T>(key: string, fallback: T): T`
returns either the parsed value cast to `T`, or the `fallback` if parsing fails.
For the `useState` lazy initializer, call `getFromStorage<CartItem[]>` inside the
function so it only runs once on mount, not on every render.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

interface Product {
  id: string;
  name: string;
  price: number;
  image: string;
}

// CartItem extends Product — adds quantity field
interface CartItem extends Product {
  quantity: number;
}

const CART_KEY = 'shopping-cart';

// Generic typed storage utility — reusable across the whole app
function getFromStorage<T>(key: string, fallback: T): T {
  try {
    const item = localStorage.getItem(key);
    return item ? (JSON.parse(item) as T) : fallback;
  } catch {
    // JSON.parse failed or localStorage unavailable
    localStorage.removeItem(key);
    return fallback;
  }
}

function ShoppingCart(): JSX.Element {
  // Lazy initializer — reads localStorage once on mount
  const [cartItems, setCartItems] = useState<CartItem[]>(() =>
    getFromStorage<CartItem[]>(CART_KEY, [])
  );

  // Save to localStorage whenever cart changes
  useEffect(() => {
    if (cartItems.length === 0) {
      localStorage.removeItem(CART_KEY);
    } else {
      localStorage.setItem(CART_KEY, JSON.stringify(cartItems));
    }
  }, [cartItems]);

  const addItem = (product: Product): void => {
    setCartItems(prev => {
      const existing = prev.find((item: CartItem) => item.id === product.id);
      if (existing) {
        return prev.map((item: CartItem) =>
          item.id === product.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      // Spread product + add quantity — TypeScript ensures CartItem shape
      const newItem: CartItem = { ...product, quantity: 1 };
      return [...prev, newItem];
    });
  };

  const removeItem = (productId: string): void => {
    setCartItems(prev => prev.filter((item: CartItem) => item.id !== productId));
  };

  const updateQuantity = (productId: string, quantity: number): void => {
    if (quantity <= 0) {
      removeItem(productId);
      return;
    }
    setCartItems(prev =>
      prev.map((item: CartItem) =>
        item.id === productId ? { ...item, quantity } : item
      )
    );
  };

  const total: number = cartItems.reduce(
    (sum: number, item: CartItem) => sum + item.price * item.quantity,
    0
  );

  if (cartItems.length === 0) {
    return <div>Your cart is empty.</div>;
  }

  return (
    <div>
      <h2>Cart ({cartItems.length} items)</h2>
      {cartItems.map((item: CartItem) => (
        <div key={item.id}>
          <img src={item.image} alt={item.name} width={60} />
          <span>{item.name}</span>
          <input
            type="number"
            value={item.quantity}
            min={1}
            onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
              updateQuantity(item.id, parseInt(e.target.value, 10))
            }
          />
          {/* TypeScript knows price and quantity are numbers */}
          <span>₹{(item.price * item.quantity).toFixed(2)}</span>
          <button onClick={() => removeItem(item.id)}>Remove</button>
        </div>
      ))}
      <p>Total: ₹{total.toFixed(2)}</p>
    </div>
  );
}

export default ShoppingCart;
```

**What the interviewer is checking:**
Logic: lazy `useState` initializer, `useEffect` saves on change, remove when empty.
TypeScript: generic `getFromStorage<T>`, `CartItem extends Product`, `CartItem[]` state.

**TypeScript lesson:**
`interface CartItem extends Product { quantity: number }` uses interface extension.
This means `CartItem` has all `Product` fields plus `quantity`. When you do
`{ ...product, quantity: 1 }`, TypeScript verifies the result satisfies `CartItem`.
This is better than redefining all fields — changes to `Product` automatically
propagate to `CartItem`.

</details>

---

### Pattern 4 · Scenario 2 — The Settings That Reset Every Visit

🏢 **Situation:**
Your SaaS dashboard has user preferences — items per page, date range, language.
They reset every visit. Users configure them every session. Fix with
localStorage persistence and full TypeScript safety.

**Types used in this scenario:**
```typescript
interface UserSettings {
  itemsPerPage: 10 | 25 | 50;
  dateRange: '7days' | '30days' | '90days';
  emailNotifications: boolean;
  language: 'en' | 'hi' | 'ta';
}

type SaveStatus = 'idle' | 'saving' | 'saved' | 'error';
```

**Here is the starter code:**
```tsx
import { useState } from 'react';

const DEFAULT_SETTINGS = {
  itemsPerPage: 10,
  dateRange: '7days',
  emailNotifications: true,
  language: 'en',
};

function UserSettingsPage() {
  // Resets every visit — no localStorage
  const [settings, setSettings] = useState(DEFAULT_SETTINGS);  // untyped

  const handleSave = () => {
    fetch('/api/user/settings', {
      method: 'PUT',
      body: JSON.stringify(settings),
    });
  };

  return (
    <div>
      <select value={settings.itemsPerPage}
        onChange={e => setSettings(prev => ({
          ...prev, itemsPerPage: Number(e.target.value)
        }))}>
        <option value={10}>10</option>
        <option value={25}>25</option>
        <option value={50}>50</option>
      </select>
      <button onClick={handleSave}>Save</button>
    </div>
  );
}
```

**Your tasks:**
- ✅ Add the `UserSettings` interface and type all state
- ✅ Initialize from localStorage using typed lazy `useState`
- ✅ Save to localStorage AND API when user clicks Save
- ✅ Use `SaveStatus` for the save button state machine

<details>
<summary>💡 Hint</summary>

When reading settings from localStorage, merge with defaults:
`{ ...DEFAULT_SETTINGS, ...parsedSettings }`.
This ensures that if a new setting key is added later, it gets its default value
even if the stored data does not have it. TypeScript will enforce the final
object satisfies `UserSettings`.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState } from 'react';

interface UserSettings {
  itemsPerPage: 10 | 25 | 50;
  dateRange: '7days' | '30days' | '90days';
  emailNotifications: boolean;
  language: 'en' | 'hi' | 'ta';
}

type SaveStatus = 'idle' | 'saving' | 'saved' | 'error';

const SETTINGS_KEY = 'user-settings';

const DEFAULT_SETTINGS: UserSettings = {
  itemsPerPage: 10,
  dateRange: '7days',
  emailNotifications: true,
  language: 'en',
};

function UserSettingsPage(): JSX.Element {
  // Typed lazy initialization — merge saved with defaults
  const [settings, setSettings] = useState<UserSettings>(() => {
    try {
      const saved = localStorage.getItem(SETTINGS_KEY);
      if (!saved) return DEFAULT_SETTINGS;
      const parsed = JSON.parse(saved) as Partial<UserSettings>;
      // Merge: defaults fill in any missing keys from saved data
      return { ...DEFAULT_SETTINGS, ...parsed };
    } catch {
      return DEFAULT_SETTINGS;
    }
  });

  const [saveStatus, setSaveStatus] = useState<SaveStatus>('idle');

  // Typed update helper — key must be a valid UserSettings key
  const updateSetting = <K extends keyof UserSettings>(
    key: K,
    value: UserSettings[K]
  ): void => {
    setSettings(prev => ({ ...prev, [key]: value }));
    setSaveStatus('idle');
  };

  const handleSave = async (): Promise<void> => {
    try {
      setSaveStatus('saving');

      // Save locally first — instant
      localStorage.setItem(SETTINGS_KEY, JSON.stringify(settings));

      // Then sync to server
      const res = await fetch('/api/user/settings', {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(settings),
      });

      if (!res.ok) throw new Error('Server save failed');

      setSaveStatus('saved');
      setTimeout(() => setSaveStatus('idle'), 3000);

    } catch (err: unknown) {
      console.warn('Settings save error:', err instanceof Error ? err.message : err);
      setSaveStatus('error');
    }
  };

  return (
    <div>
      <h2>Settings</h2>

      <label>
        Items per page:
        <select
          value={settings.itemsPerPage}
          onChange={(e: React.ChangeEvent<HTMLSelectElement>) =>
            // Type assertion: select value is string, we know it's the union
            updateSetting('itemsPerPage', Number(e.target.value) as 10 | 25 | 50)
          }
        >
          <option value={10}>10</option>
          <option value={25}>25</option>
          <option value={50}>50</option>
        </select>
      </label>

      <label>
        Language:
        <select
          value={settings.language}
          onChange={(e: React.ChangeEvent<HTMLSelectElement>) =>
            updateSetting('language', e.target.value as UserSettings['language'])
          }
        >
          <option value="en">English</option>
          <option value="hi">Hindi</option>
          <option value="ta">Tamil</option>
        </select>
      </label>

      <label>
        Email Notifications:
        <input
          type="checkbox"
          checked={settings.emailNotifications}
          onChange={(e: React.ChangeEvent<HTMLInputElement>) =>
            updateSetting('emailNotifications', e.target.checked)
          }
        />
      </label>

      <button onClick={handleSave} disabled={saveStatus === 'saving'}>
        {saveStatus === 'saving' ? 'Saving...' : 'Save Settings'}
      </button>

      {saveStatus === 'saved' && <p style={{ color: 'green' }}>Settings saved!</p>}
      {saveStatus === 'error' && <p style={{ color: 'red' }}>Save failed. Saved locally.</p>}
    </div>
  );
}

export default UserSettingsPage;
```

**What the interviewer is checking:**
Logic: merge with defaults on init, save locally then to API, status machine.
TypeScript: generic `updateSetting<K extends keyof UserSettings>`, `Partial<UserSettings>`
for parsed data, `UserSettings['language']` indexed access type for value constraint.

**TypeScript lesson:**
`<K extends keyof UserSettings>(key: K, value: UserSettings[K])` is a generic
with a constraint. `K` is constrained to valid keys of `UserSettings`. `UserSettings[K]`
is the indexed access type — it gives you the type of that specific field.
This means `updateSetting('itemsPerPage', 'hello')` is a TypeScript error —
`itemsPerPage` only accepts `10 | 25 | 50`.

</details>

---

### Pattern 4 · Scenario 3 — The Draft That Belongs to the Wrong Post

🏢 **Situation:**
You are building a blogging platform. Writers have two tabs open —
one for each post draft. They keep overwriting each other because
the storage key is the same for all posts. Fix the key collision
using typed storage with per-post keys.

**Types used in this scenario:**
```typescript
interface PostDraft {
  title: string;
  content: string;
  savedAt: string;
}

interface PostEditorProps {
  postId: string;
  initialTitle?: string;
  initialContent?: string;
}
```

**Here is the broken code:**
```tsx
import { useState, useEffect } from 'react';

function PostEditor({ postId }) {    // untyped props
  const [title, setTitle] = useState('');
  const [content, setContent] = useState('');

  useEffect(() => {
    // BUG: same key for all posts — tabs overwrite each other
    const saved = localStorage.getItem('post-draft');
    if (saved) {
      const { title, content } = JSON.parse(saved);  // untyped parse
      setTitle(title);
      setContent(content);
    }
  }, []);

  useEffect(() => {
    // BUG: same key again
    localStorage.setItem('post-draft', JSON.stringify({ title, content }));
  }, [title, content]);

  return (
    <div>
      <input value={title} onChange={e => setTitle(e.target.value)} />
      <textarea value={content} onChange={e => setContent(e.target.value)} />
    </div>
  );
}
```

**Your tasks:**
- ✅ Type the component with `PostEditorProps`
- ✅ Fix key collision: use `post-draft-${postId}` as the storage key
- ✅ Type the parsed draft as `PostDraft`
- ✅ Reset state when `postId` changes, then restore the correct draft
- ✅ Add a typed "last saved" timestamp

<details>
<summary>💡 Hint</summary>

The storage key must include `postId` so each post has its own slot.
Build the key as a `const DRAFT_KEY = \`post-draft-${postId}\`` inside the component
so it updates when `postId` changes. Include `DRAFT_KEY` in the `useEffect`
dependency arrays so effects re-run when switching between posts.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

interface PostDraft {
  title: string;
  content: string;
  savedAt: string;
}

interface PostEditorProps {
  postId: string;
  initialTitle?: string;
  initialContent?: string;
}

function PostEditor({
  postId,
  initialTitle = '',
  initialContent = '',
}: PostEditorProps): JSX.Element {
  // FIX: unique key per post — no more tab collision
  const DRAFT_KEY = `post-draft-${postId}`;

  const [title, setTitle] = useState<string>(initialTitle);
  const [content, setContent] = useState<string>(initialContent);
  const [lastSaved, setLastSaved] = useState<string | null>(null);
  const [isPublishing, setIsPublishing] = useState<boolean>(false);

  // Restore the correct draft when postId changes
  useEffect(() => {
    // Reset first — prevents old post data bleeding in
    setTitle(initialTitle);
    setContent(initialContent);
    setLastSaved(null);

    try {
      const saved = localStorage.getItem(DRAFT_KEY);
      if (saved) {
        // Typed parse — we know the shape of what we saved
        const draft = JSON.parse(saved) as PostDraft;
        setTitle(draft.title ?? initialTitle);
        setContent(draft.content ?? initialContent);
        setLastSaved(draft.savedAt ?? null);
      }
    } catch {
      localStorage.removeItem(DRAFT_KEY);
    }
  }, [DRAFT_KEY, initialTitle, initialContent]); // re-run when post changes

  // Auto-save with debounce
  useEffect(() => {
    const timer = setTimeout(() => {
      const draft: PostDraft = {
        title,
        content,
        savedAt: new Date().toLocaleTimeString(),
      };
      localStorage.setItem(DRAFT_KEY, JSON.stringify(draft));
      setLastSaved(draft.savedAt);
    }, 1000);

    return () => clearTimeout(timer);
  }, [title, content, DRAFT_KEY]);

  const handlePublish = async (): Promise<void> => {
    try {
      setIsPublishing(true);
      const res = await fetch(`/api/posts/${postId}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ title, content, status: 'published' }),
      });

      if (!res.ok) throw new Error('Publish failed');

      // Clear THIS post's draft only
      localStorage.removeItem(DRAFT_KEY);
      setLastSaved(null);
      alert('Post published!');

    } catch (err: unknown) {
      const message = err instanceof Error ? err.message : 'Publish failed';
      alert(`${message}. Your draft is saved.`);
    } finally {
      setIsPublishing(false);
    }
  };

  return (
    <div>
      {lastSaved && (
        <p style={{ color: 'gray', fontSize: '12px' }}>
          Draft saved at {lastSaved}
        </p>
      )}
      <input
        value={title}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) => setTitle(e.target.value)}
        placeholder="Post title"
      />
      <textarea
        value={content}
        onChange={(e: React.ChangeEvent<HTMLTextAreaElement>) => setContent(e.target.value)}
        placeholder="Write your post..."
        rows={10}
      />
      <button onClick={handlePublish} disabled={isPublishing}>
        {isPublishing ? 'Publishing...' : 'Publish'}
      </button>
    </div>
  );
}

export default PostEditor;
```

**What the interviewer is checking:**
Logic: `postId` in storage key, state reset before restore, only clear own draft on publish.
TypeScript: `PostEditorProps` with optional fields using `?`, `PostDraft` typed parse,
`React.ChangeEvent<HTMLTextAreaElement>` specifically for textarea.

**TypeScript lesson:**
`React.ChangeEvent<HTMLTextAreaElement>` is distinct from `React.ChangeEvent<HTMLInputElement>`.
In JavaScript you might get away with `e.target.value` on both, but TypeScript forces you
to be explicit. This distinction matters for accessibility attributes and behavior
that differ between the two element types.

</details>

---

### Pattern 4 · Scenario 4 — The localStorage That Crashes the App

🏢 **Situation:**
You are on call at a banking app. An urgent alert: "White screen for some users."
The crash is in localStorage. Some users are in private browsing. Others have
full storage. Build a typed safe storage utility that never crashes.

**Types used in this scenario:**
```typescript
interface StorageResult<T> {
  success: boolean;
  value: T | null;
  error?: string;
}

interface SafeStorage {
  get: <T>(key: string, fallback: T) => T;
  set: (key: string, value: unknown) => boolean;
  remove: (key: string) => void;
  isAvailable: () => boolean;
}
```

**Here is the broken code:**
```tsx
import { useState } from 'react';

function App() {
  // CRASHES in private browsing — localStorage throws SecurityError
  const savedPrefs = JSON.parse(localStorage.getItem('user-preferences'));

  const [preferences, setPreferences] = useState(
    savedPrefs || { theme: 'light', currency: 'INR' }
  );

  const savePreference = (key, value) => {   // untyped
    const updated = { ...preferences, [key]: value };
    setPreferences(updated);
    // CRASHES when storage is full — QuotaExceededError
    localStorage.setItem('user-preferences', JSON.stringify(updated));
  };

  return <div>{/* ... */}</div>;
}
```

**Your tasks:**
- ✅ Build the typed `SafeStorage` object with proper interfaces
- ✅ Handle three crash scenarios: parse error, SecurityError, QuotaExceededError
- ✅ Type preferences with an interface and use the safe storage
- ✅ Never crash — always degrade gracefully

<details>
<summary>💡 Hint</summary>

Three distinct failure modes need three separate try/catch locations.
The `isAvailable()` check wraps `localStorage.setItem('test', 'test')` in a try/catch —
if it throws, storage is disabled. The generic `get<T>` returns the fallback on any error.
The `set` function returns `boolean` — callers can check if saving succeeded.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState } from 'react';

interface UserPreferences {
  theme: 'light' | 'dark';
  currency: 'INR' | 'USD' | 'EUR';
  language: 'en' | 'hi';
}

// Typed safe storage — wraps localStorage with full error handling
const safeStorage = {
  // Check if localStorage is available (fails in some private browsing)
  isAvailable(): boolean {
    try {
      const testKey = '__storage_test__';
      localStorage.setItem(testKey, 'test');
      localStorage.removeItem(testKey);
      return true;
    } catch {
      return false;
    }
  },

  // Generic get — returns fallback on ANY error
  get<T>(key: string, fallback: T): T {
    try {
      const item = localStorage.getItem(key);
      // null means key does not exist — return fallback
      if (item === null) return fallback;
      return JSON.parse(item) as T;
    } catch (err: unknown) {
      // Catches: JSON.parse error, SecurityError (private browsing)
      console.warn(`Storage read error for "${key}":`, err);
      return fallback;
    }
  },

  // Returns true if saved successfully, false if not
  set(key: string, value: unknown): boolean {
    try {
      localStorage.setItem(key, JSON.stringify(value));
      return true;
    } catch (err: unknown) {
      // Catches: QuotaExceededError (storage full), SecurityError
      console.warn(`Storage write error for "${key}":`, err);
      return false;
    }
  },

  remove(key: string): void {
    try {
      localStorage.removeItem(key);
    } catch (err: unknown) {
      console.warn(`Storage remove error for "${key}":`, err);
    }
  },
};

const PREFS_KEY = 'user-preferences';

const DEFAULT_PREFS: UserPreferences = {
  theme: 'light',
  currency: 'INR',
  language: 'en',
};

function App(): JSX.Element {
  // FIX: safe read — never throws, returns defaults on any failure
  const [preferences, setPreferences] = useState<UserPreferences>(() => {
    const saved = safeStorage.get<Partial<UserPreferences>>(PREFS_KEY, {});
    // Merge with defaults — handles missing keys
    return { ...DEFAULT_PREFS, ...saved };
  });

  const [storageWarning, setStorageWarning] = useState<string | null>(null);

  const savePreference = <K extends keyof UserPreferences>(
    key: K,
    value: UserPreferences[K]
  ): void => {
    const updated: UserPreferences = { ...preferences, [key]: value };
    setPreferences(updated);

    // FIX: safe write — returns false if full or blocked, never throws
    const saved = safeStorage.set(PREFS_KEY, updated);
    if (!saved) {
      setStorageWarning(
        'Could not save preferences. They will reset on next visit.'
      );
    } else {
      setStorageWarning(null);
    }
  };

  return (
    <div className={`app theme-${preferences.theme}`}>
      {storageWarning && (
        <div style={{ background: 'orange', padding: 8 }}>
          ⚠️ {storageWarning}
        </div>
      )}

      <button
        onClick={() =>
          savePreference('theme', preferences.theme === 'light' ? 'dark' : 'light')
        }
      >
        Toggle Theme ({preferences.theme})
      </button>

      <select
        value={preferences.currency}
        onChange={(e: React.ChangeEvent<HTMLSelectElement>) =>
          savePreference('currency', e.target.value as UserPreferences['currency'])
        }
      >
        <option value="INR">INR</option>
        <option value="USD">USD</option>
        <option value="EUR">EUR</option>
      </select>
    </div>
  );
}

export default App;
```

**What the interviewer is checking:**
Logic: three crash scenarios handled, graceful degradation, warning shown to user.
TypeScript: generic `get<T>` method, `<K extends keyof UserPreferences>` for `savePreference`,
`UserPreferences['currency']` indexed access for value type constraint.

**TypeScript lesson:**
`safeStorage.get<Partial<UserPreferences>>(key, {})` uses `Partial<UserPreferences>`
because stored data might be incomplete (written by an older app version with fewer fields).
The merge `{ ...DEFAULT_PREFS, ...saved }` then fills in any missing fields.
This pattern — `Partial<T>` for stored data merged with a complete default — is
the production-safe way to read any persisted state.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 5: 🔀 Show/Hide UI

> **Level 1 — Foundation**

**What you will build in this pattern:**
- A typed modal with outside-click and Escape key close handlers
- A typed accordion with exclusive open state
- A typed dropdown that closes on outside click using `useRef`

**Interview asks this as:**
- "Build a modal component in TypeScript — close on outside click"
- "Create a typed accordion where only one item is open"
- "Build a dropdown that closes when clicking outside — use useRef"

**TypeScript concepts used in this pattern:**
- `useRef<HTMLDivElement>(null)` for DOM references
- `useRef<ReturnType<typeof setTimeout> | null>` for timers
- Typed event listeners: `MouseEvent` vs `React.MouseEvent`
- `React.KeyboardEvent` and `KeyboardEvent` for Escape key
- `React.ReactNode` for children prop typing

**Most common mistake in interviews:**
Not using `e.stopPropagation()` on the modal content div — clicks inside close the modal.
In TypeScript: typing the ref as `useRef<HTMLElement>` instead of the specific element type.

---

### Pattern 5 · Scenario 1 — The Modal That Never Closes

🏢 **Situation:**
You are at a recruitment platform. Clicking a candidate row opens a detail modal.
The only close mechanism is the X button. Users expect Escape key and outside-click.
Your UX designer filed a bug report. Fix it with typed event handlers.

**Types used in this scenario:**
```typescript
interface Candidate {
  id: string;
  name: string;
  email: string;
  role: string;
  experience: number;
}

interface CandidateTableProps {
  candidates: Candidate[];
}

interface CandidateModalProps {
  candidate: Candidate;
  onClose: () => void;
}
```

**Here is the broken code:**
```tsx
import { useState } from 'react';

function CandidateTable({ candidates }) {  // untyped
  const [selected, setSelected] = useState(null);  // untyped null

  return (
    <div>
      <table>
        <tbody>
          {candidates.map(c => (
            <tr key={c.id} onClick={() => setSelected(c)}>
              <td>{c.name}</td><td>{c.role}</td>
            </tr>
          ))}
        </tbody>
      </table>
      {selected && (
        <div style={{ position: 'fixed', inset: 0, background: 'rgba(0,0,0,0.5)' }}>
          <div style={{ background: 'white', padding: 24 }}>
            <button onClick={() => setSelected(null)}>✕</button>
            <h2>{selected.name}</h2>
          </div>
        </div>
        // No outside-click close, no Escape key close
      )}
    </div>
  );
}
```

**Your tasks:**
- ✅ Type `selected` as `Candidate | null`
- ✅ Extract a typed `CandidateModal` component with `CandidateModalProps`
- ✅ Close on Escape key — typed `KeyboardEvent` listener
- ✅ Close on overlay click — typed `React.MouseEvent` with `stopPropagation`
- ✅ Prevent body scroll while modal is open

<details>
<summary>💡 Hint</summary>

The overlay `onClick` closes the modal. The inner content `onClick` must call
`e.stopPropagation()` to prevent the click from bubbling to the overlay.
`e` here is `React.MouseEvent<HTMLDivElement>` — not a native `MouseEvent`.
The Escape listener on `document` uses native `KeyboardEvent`, not the React synthetic version.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

interface Candidate {
  id: string;
  name: string;
  email: string;
  role: string;
  experience: number;
}

interface CandidateTableProps {
  candidates: Candidate[];
}

interface CandidateModalProps {
  candidate: Candidate;
  onClose: () => void;
}

// Extracted typed modal component
function CandidateModal({ candidate, onClose }: CandidateModalProps): JSX.Element {
  // Escape key + scroll lock
  useEffect(() => {
    document.body.style.overflow = 'hidden';

    // Native DOM event — KeyboardEvent not React.KeyboardEvent
    const handleKeyDown = (e: KeyboardEvent): void => {
      if (e.key === 'Escape') onClose();
    };

    document.addEventListener('keydown', handleKeyDown);

    return () => {
      document.body.style.overflow = '';
      document.removeEventListener('keydown', handleKeyDown);
    };
  }, [onClose]);

  // Typed overlay click handler
  const handleOverlayClick = (e: React.MouseEvent<HTMLDivElement>): void => {
    onClose();
  };

  // Typed content click handler — stop propagation
  const handleContentClick = (e: React.MouseEvent<HTMLDivElement>): void => {
    e.stopPropagation(); // prevents click reaching overlay
  };

  return (
    <div
      onClick={handleOverlayClick}
      style={{
        position: 'fixed', inset: 0,
        background: 'rgba(0,0,0,0.5)',
        display: 'flex', alignItems: 'center', justifyContent: 'center',
        zIndex: 1000,
      }}
    >
      <div
        onClick={handleContentClick}
        style={{
          background: 'white', padding: 32,
          borderRadius: 8, minWidth: 400, maxWidth: 600,
        }}
      >
        <button onClick={onClose}>✕ Close</button>
        <h2>{candidate.name}</h2>
        <p>Role: {candidate.role}</p>
        <p>Email: {candidate.email}</p>
        {/* TypeScript knows experience is number */}
        <p>Experience: {candidate.experience} years</p>
      </div>
    </div>
  );
}

function CandidateTable({ candidates }: CandidateTableProps): JSX.Element {
  // Typed selected state — Candidate or null
  const [selected, setSelected] = useState<Candidate | null>(null);

  return (
    <div>
      <table>
        <tbody>
          {candidates.map((candidate: Candidate) => (
            <tr
              key={candidate.id}
              onClick={() => setSelected(candidate)}
              style={{ cursor: 'pointer' }}
            >
              <td>{candidate.name}</td>
              <td>{candidate.role}</td>
            </tr>
          ))}
        </tbody>
      </table>

      {/* TypeScript narrows: selected is Candidate here (not null) */}
      {selected !== null && (
        <CandidateModal
          candidate={selected}
          onClose={() => setSelected(null)}
        />
      )}
    </div>
  );
}

export default CandidateTable;
```

**What the interviewer is checking:**
Logic: `stopPropagation` on content, `KeyboardEvent` listener with cleanup, scroll lock.
TypeScript: `React.MouseEvent<HTMLDivElement>` for synthetic events, native `KeyboardEvent`
for document listeners, `Candidate | null` state, typed `CandidateModalProps`.

**TypeScript lesson:**
`React.MouseEvent<HTMLDivElement>` is the React synthetic event type.
`KeyboardEvent` (no `React.` prefix) is the native DOM event type — used for
`document.addEventListener`. These are different types! React events have
extra properties like `nativeEvent`. Native events work directly with the DOM.
Mixing them up causes TypeScript errors that reveal architectural mistakes.

</details>

---

### Pattern 5 · Scenario 2 — The Accordion That Opens Everything

🏢 **Situation:**
You are building an FAQ section for a healthcare app with 10 questions.
Only one should be open at a time. Currently all can be open simultaneously.
Fix it with a single typed `openId` state and typed accordion components.

**Types used in this scenario:**
```typescript
interface FaqItem {
  id: string;
  question: string;
  answer: string;
  category: string;
}

interface FaqAccordionProps {
  faqs: FaqItem[];
}

interface AccordionItemProps {
  faq: FaqItem;
  isOpen: boolean;
  onToggle: (id: string) => void;
}
```

**Here is the broken code:**
```tsx
import { useState } from 'react';

function FAQ({ faqs }) {    // untyped
  // BUG: object with boolean per item — allows multiple open
  const [openItems, setOpenItems] = useState({});

  const toggle = (id) => {
    setOpenItems(prev => ({ ...prev, [id]: !prev[id] }));
  };

  return (
    <div>
      {faqs.map(faq => (
        <div key={faq.id}>
          <button onClick={() => toggle(faq.id)}>
            {faq.question}
          </button>
          {openItems[faq.id] && <div>{faq.answer}</div>}
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Replace `openItems` object with a single typed `openId: string | null`
- ✅ Extract a typed `AccordionItem` component
- ✅ Type the `onToggle` callback with proper signature
- ✅ Add typed ARIA attributes for accessibility

<details>
<summary>💡 Hint</summary>

One state value: `const [openId, setOpenId] = useState<string | null>(null)`.
The toggle logic: `setOpenId(prev => prev === id ? null : id)`.
This automatically closes the previous item when a new one opens.
TypeScript enforces `openId` can only be `string | null` — no invalid states possible.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState } from 'react';

interface FaqItem {
  id: string;
  question: string;
  answer: string;
  category: string;
}

interface FaqAccordionProps {
  faqs: FaqItem[];
}

interface AccordionItemProps {
  faq: FaqItem;
  isOpen: boolean;
  onToggle: (id: string) => void;
}

// Extracted typed accordion item component
function AccordionItem({ faq, isOpen, onToggle }: AccordionItemProps): JSX.Element {
  const handleClick = (): void => {
    onToggle(faq.id);
  };

  return (
    <div style={{ borderBottom: '1px solid #eee' }}>
      <button
        onClick={handleClick}
        style={{
          width: '100%',
          padding: '16px',
          display: 'flex',
          justifyContent: 'space-between',
          alignItems: 'center',
          background: 'none',
          border: 'none',
          cursor: 'pointer',
          fontWeight: isOpen ? 'bold' : 'normal',
        }}
        // Typed ARIA attribute — boolean is correct type here
        aria-expanded={isOpen}
        aria-controls={`faq-answer-${faq.id}`}
      >
        <span>{faq.question}</span>
        <span
          style={{
            transform: isOpen ? 'rotate(180deg)' : 'rotate(0deg)',
            transition: 'transform 0.2s ease',
            display: 'inline-block',
          }}
        >
          ▼
        </span>
      </button>

      {isOpen && (
        <div
          id={`faq-answer-${faq.id}`}
          style={{ padding: '0 16px 16px', color: '#555' }}
        >
          {faq.answer}
        </div>
      )}
    </div>
  );
}

function FaqAccordion({ faqs }: FaqAccordionProps): JSX.Element {
  // ONE state — id of open item, null means all closed
  const [openId, setOpenId] = useState<string | null>(null);

  const handleToggle = (id: string): void => {
    // If clicking open item → close it. If clicking closed → open it.
    setOpenId(prev => prev === id ? null : id);
  };

  return (
    <div>
      {faqs.map((faq: FaqItem) => (
        <AccordionItem
          key={faq.id}
          faq={faq}
          isOpen={openId === faq.id}
          onToggle={handleToggle}
        />
      ))}
    </div>
  );
}

export default FaqAccordion;
```

**What the interviewer is checking:**
Logic: single `openId` state replacing the object, toggle closes previous item automatically.
TypeScript: `string | null` state, `(id: string) => void` callback type in props,
`AccordionItemProps` interface for the extracted component.

**TypeScript lesson:**
`onToggle: (id: string) => void` in the props interface precisely defines the callback contract.
The parent provides `handleToggle` which matches this signature.
If someone passes a callback that takes a number instead of a string,
TypeScript catches it at compile time — before any runtime errors occur.

</details>

---

### Pattern 5 · Scenario 3 — The Dropdown That Stays Open

🏢 **Situation:**
You are building a user avatar menu. It opens on click but never closes
unless you click the toggle button again. Users expect it to close when
clicking anywhere else on the page. Fix it using `useRef` and
typed document event listeners.

**Types used in this scenario:**
```typescript
interface UserMenuUser {
  name: string;
  email: string;
  avatar: string;
  role: 'admin' | 'user';
}

interface UserMenuProps {
  user: UserMenuUser;
  onLogout: () => void;
}
```

**Here is the broken code:**
```tsx
import { useState } from 'react';

function UserMenu({ user, onLogout }) {   // untyped
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div style={{ position: 'relative' }}>
      <button onClick={() => setIsOpen(!isOpen)}>
        <img src={user.avatar} alt={user.name} />
      </button>
      {isOpen && (
        <div style={{ position: 'absolute', right: 0, background: 'white' }}>
          <p>{user.name}</p>
          <button onClick={onLogout}>Logout</button>
        </div>
        // Never closes on outside click
      )}
    </div>
  );
}
```

**Your tasks:**
- ✅ Type with `UserMenuProps` interface
- ✅ Add `useRef<HTMLDivElement>(null)` for the container
- ✅ Add typed outside-click detection using `document.addEventListener`
- ✅ Add typed Escape key close handler
- ✅ Only attach listeners when the dropdown is open

<details>
<summary>💡 Hint</summary>

`containerRef.current.contains(e.target as Node)` checks if a DOM click
happened inside the container. The native `MouseEvent` type has `e.target`
typed as `EventTarget | null`, not `Node` — use `as Node` to cast.
The `useEffect` should return early `if (!isOpen)` so listeners are
only added when needed.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect, useRef } from 'react';

interface UserMenuUser {
  name: string;
  email: string;
  avatar: string;
  role: 'admin' | 'user';
}

interface UserMenuProps {
  user: UserMenuUser;
  onLogout: () => void;
}

function UserMenu({ user, onLogout }: UserMenuProps): JSX.Element {
  const [isOpen, setIsOpen] = useState<boolean>(false);
  // Typed ref — specific to div elements
  const containerRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    // Only add listeners when dropdown is open
    if (!isOpen) return;

    // Native MouseEvent — not React.MouseEvent
    const handleOutsideClick = (e: MouseEvent): void => {
      // e.target is EventTarget | null — cast to Node for contains()
      if (
        containerRef.current !== null &&
        !containerRef.current.contains(e.target as Node)
      ) {
        setIsOpen(false);
      }
    };

    // Native KeyboardEvent
    const handleEscape = (e: KeyboardEvent): void => {
      if (e.key === 'Escape') setIsOpen(false);
    };

    // mousedown fires before click — avoids race conditions
    document.addEventListener('mousedown', handleOutsideClick);
    document.addEventListener('keydown', handleEscape);

    return () => {
      document.removeEventListener('mousedown', handleOutsideClick);
      document.removeEventListener('keydown', handleEscape);
    };
  }, [isOpen]);

  const handleToggle = (e: React.MouseEvent<HTMLButtonElement>): void => {
    e.stopPropagation();
    setIsOpen(prev => !prev);
  };

  return (
    // ref attached to the container div
    <div ref={containerRef} style={{ position: 'relative', display: 'inline-block' }}>
      <button onClick={handleToggle}>
        <img
          src={user.avatar}
          alt={user.name}
          style={{ width: 40, height: 40, borderRadius: '50%', cursor: 'pointer' }}
        />
      </button>

      {isOpen && (
        <div
          style={{
            position: 'absolute', right: 0, top: '100%',
            background: 'white', border: '1px solid #ddd',
            borderRadius: 8, minWidth: 200, padding: 8,
            boxShadow: '0 4px 12px rgba(0,0,0,0.1)',
            zIndex: 1000,
          }}
        >
          <div style={{ padding: '8px 12px', borderBottom: '1px solid #eee' }}>
            <p style={{ margin: 0, fontWeight: 'bold' }}>{user.name}</p>
            <p style={{ margin: 0, fontSize: 13, color: '#666' }}>{user.email}</p>
            {/* TypeScript knows role is 'admin' | 'user' */}
            {user.role === 'admin' && (
              <span style={{ fontSize: 11, color: 'blue' }}>Admin</span>
            )}
          </div>
          <button
            onClick={onLogout}
            style={{ display: 'block', width: '100%', padding: '8px 12px', color: 'red' }}
          >
            Logout
          </button>
        </div>
      )}
    </div>
  );
}

export default UserMenu;
```

**What the interviewer is checking:**
Logic: `containerRef.current.contains(e.target)` pattern, listeners only when open, cleanup.
TypeScript: `useRef<HTMLDivElement>(null)`, native `MouseEvent` vs `React.MouseEvent`,
`e.target as Node` cast explained.

**TypeScript lesson:**
Native DOM `MouseEvent.target` is typed as `EventTarget | null` — a very broad type.
`Node.contains()` requires a `Node` argument. The cast `e.target as Node` is safe here
because any element that receives a click event is a `Node`. TypeScript cannot verify
this automatically because `EventTarget` is broader than `Node` (audio contexts,
workers, etc. are also EventTargets but not Nodes).

</details>

---

### Pattern 5 · Scenario 4 — The Tabs That Lose Their State

🏢 **Situation:**
You are building a user profile page with Overview, Posts, and Settings tabs.
The Settings tab has a form. Switching to Posts and back to Settings loses all
unsaved form data. Fix with CSS hide/show and typed tab components.

**Types used in this scenario:**
```typescript
interface TabItem {
  id: string;
  label: string;
  hasUnsavedChanges?: boolean;
}

interface ProfileTabsProps {
  user: UserProfile;
}

interface UserProfile {
  id: string;
  name: string;
  bio: string;
  email: string;
}

interface SettingsTabProps {
  user: UserProfile;
  onUnsavedChange: (hasChanges: boolean) => void;
}
```

**Here is the broken code:**
```tsx
import { useState } from 'react';

function ProfileTabs({ user }) {    // untyped
  const [activeTab, setActiveTab] = useState('overview');

  const renderTab = () => {
    // BUG: only mounts active tab — unmounts others, losing state
    if (activeTab === 'overview') return <OverviewTab user={user} />;
    if (activeTab === 'posts') return <PostsTab userId={user.id} />;
    if (activeTab === 'settings') return <SettingsTab user={user} />;
  };

  return (
    <div>
      <div>
        {['overview', 'posts', 'settings'].map(tab => (
          <button key={tab} onClick={() => setActiveTab(tab)}>{tab}</button>
        ))}
      </div>
      <div>{renderTab()}</div>
    </div>
  );
}
```

**Your tasks:**
- ✅ Type everything with proper interfaces
- ✅ Fix state loss — render all tabs, hide with `display: 'none'`
- ✅ Build typed `SettingsTab` that reports unsaved changes to parent
- ✅ Show a typed unsaved-changes badge on the Settings tab button

<details>
<summary>💡 Hint</summary>

Render all three tab panels but toggle `display: 'none'` vs `display: 'block'`
based on `activeTab`. The `SettingsTab` receives `onUnsavedChange: (hasChanges: boolean) => void`
as a prop and calls it via `useEffect` when form data diverges from the original.
Use `useEffect(() => { onUnsavedChange(hasChanges) }, [hasChanges, onUnsavedChange])`.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';

interface UserProfile {
  id: string;
  name: string;
  bio: string;
  email: string;
}

interface TabDefinition {
  id: string;
  label: string;
}

interface ProfileTabsProps {
  user: UserProfile;
}

interface SettingsTabProps {
  user: UserProfile;
  onUnsavedChange: (hasChanges: boolean) => void;
}

// The settings form — stays mounted, reports unsaved changes
function SettingsTab({ user, onUnsavedChange }: SettingsTabProps): JSX.Element {
  const [name, setName] = useState<string>(user.name);
  const [bio, setBio] = useState<string>(user.bio);
  const [isSaving, setIsSaving] = useState<boolean>(false);

  const hasChanges: boolean = name !== user.name || bio !== user.bio;

  // Notify parent whenever unsaved status changes
  useEffect(() => {
    onUnsavedChange(hasChanges);
  }, [hasChanges, onUnsavedChange]);

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>): Promise<void> => {
    e.preventDefault();
    try {
      setIsSaving(true);
      const res = await fetch(`/api/users/${user.id}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ name, bio }),
      });
      if (!res.ok) throw new Error('Save failed');
      onUnsavedChange(false);
    } catch (err: unknown) {
      console.error(err instanceof Error ? err.message : err);
    } finally {
      setIsSaving(false);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      {hasChanges && (
        <p style={{ color: 'orange' }}>You have unsaved changes</p>
      )}
      <input
        value={name}
        onChange={(e: React.ChangeEvent<HTMLInputElement>) => setName(e.target.value)}
        placeholder="Display name"
      />
      <textarea
        value={bio}
        onChange={(e: React.ChangeEvent<HTMLTextAreaElement>) => setBio(e.target.value)}
        placeholder="Bio"
      />
      <button type="submit" disabled={isSaving || !hasChanges}>
        {isSaving ? 'Saving...' : 'Save Changes'}
      </button>
    </form>
  );
}

function ProfileTabs({ user }: ProfileTabsProps): JSX.Element {
  const [activeTab, setActiveTab] = useState<string>('overview');
  const [hasUnsavedSettings, setHasUnsavedSettings] = useState<boolean>(false);

  const tabs: TabDefinition[] = [
    { id: 'overview', label: 'Overview' },
    { id: 'posts', label: 'Posts' },
    { id: 'settings', label: 'Settings' },
  ];

  return (
    <div>
      {/* Tab buttons */}
      <div style={{ display: 'flex', borderBottom: '2px solid #eee' }}>
        {tabs.map((tab: TabDefinition) => (
          <button
            key={tab.id}
            onClick={() => setActiveTab(tab.id)}
            style={{
              padding: '12px 20px',
              border: 'none',
              background: 'none',
              cursor: 'pointer',
              fontWeight: activeTab === tab.id ? 'bold' : 'normal',
              borderBottom: activeTab === tab.id ? '2px solid blue' : 'none',
              position: 'relative',
            }}
          >
            {tab.label}
            {/* Unsaved changes badge — only on settings */}
            {tab.id === 'settings' && hasUnsavedSettings && (
              <span
                style={{
                  position: 'absolute', top: 8, right: 8,
                  width: 8, height: 8, borderRadius: '50%',
                  background: 'orange',
                }}
              />
            )}
          </button>
        ))}
      </div>

      {/* FIX: render all tabs, hide with CSS — state is preserved */}
      <div style={{ display: activeTab === 'overview' ? 'block' : 'none' }}>
        <div>
          <h2>{user.name}</h2>
          <p>{user.bio}</p>
        </div>
      </div>

      <div style={{ display: activeTab === 'posts' ? 'block' : 'none' }}>
        <div>Posts for {user.name}</div>
      </div>

      <div style={{ display: activeTab === 'settings' ? 'block' : 'none' }}>
        {/* SettingsTab stays mounted — reports unsaved status */}
        <SettingsTab
          user={user}
          onUnsavedChange={setHasUnsavedSettings}
        />
      </div>
    </div>
  );
}

export default ProfileTabs;
```

**What the interviewer is checking:**
Logic: `display: 'none'` pattern keeps all tabs mounted, `onUnsavedChange` callback lifts state.
TypeScript: `SettingsTabProps` with `onUnsavedChange: (hasChanges: boolean) => void`,
`TabDefinition` interface for the tab list, `React.FormEvent<HTMLFormElement>`.

**TypeScript lesson:**
The `onUnsavedChange: (hasChanges: boolean) => void` callback type in `SettingsTabProps`
is a function type. The parent passes `setHasUnsavedSettings` (from `useState<boolean>`)
which has type `(value: boolean) => void` — it matches perfectly.
TypeScript verifies this compatibility at compile time — if the parent passes the wrong
function signature, you get an error immediately.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 6: 🧭 Page Navigation

> **Level 1 — Foundation**

**What you will build in this pattern:**
- A typed `ProtectedRoute` component that redirects unauthenticated users
- A typed login page that redirects back to the attempted URL after login
- A typed logout handler that prevents back-button history abuse

**Interview asks this as:**
- "Protect this route so only logged-in users can access it — TypeScript"
- "After login, redirect to the page the user was trying to visit"
- "Build a typed ProtectedRoute with role-based access"

**TypeScript concepts used in this pattern:**
- Typed `useParams<{ id: string }>()`
- Typed `useLocation` with `state` casting
- Typed `useNavigate` return value
- Interface for route state: `LocationState`
- Type narrowing for auth check results

**Most common mistake in interviews:**
Rendering protected content before the auth check completes — causes a flash.
In TypeScript: not typing `useParams` and getting `string | undefined` errors.

---

### Pattern 6 · Scenario 1 — The Protected Page Anyone Can Visit

🏢 **Situation:**
You are at a SaaS company. The admin dashboard at `/admin` is visible
without authentication. Direct URL access bypasses all checks.
Build a typed `ProtectedRoute` component that fixes this.

**Types used in this scenario:**
```typescript
interface LocationState {
  from?: string;
}

interface ProtectedRouteProps {
  allowedRoles?: UserRole[];
}

type UserRole = 'admin' | 'manager' | 'user';

interface AuthUser {
  id: string;
  name: string;
  role: UserRole;
  token: string;
}
```

**Here is the broken code:**
```tsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/login" element={<Login />} />
        {/* No protection — anyone can visit */}
        <Route path="/admin" element={<AdminDashboard />} />
      </Routes>
    </BrowserRouter>
  );
}

function AdminDashboard() {
  const token = localStorage.getItem('token');
  if (!token) alert('Please login');  // shows content briefly, then alerts
  return <div><h1>Admin Dashboard</h1><p>Secret data</p></div>;
}
```

**Your tasks:**
- ✅ Build a typed `ProtectedRoute` component using the interfaces above
- ✅ Redirect unauthenticated users to `/login` with `from` state
- ✅ Add role-based access with `allowedRoles` prop
- ✅ Use `<Navigate replace />` so back button does not loop

<details>
<summary>💡 Hint</summary>

`ProtectedRoute` renders `<Outlet />` when authorized, and
`<Navigate to="/login" state={{ from: location.pathname }} replace />` when not.
Type `location.state` by casting: `const state = location.state as LocationState | null`.
The `allowedRoles` check compares `user.role` against the array using `.includes()`.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import {
  BrowserRouter, Routes, Route,
  Navigate, Outlet, useNavigate, useLocation
} from 'react-router-dom';

type UserRole = 'admin' | 'manager' | 'user';

interface AuthUser {
  id: string;
  name: string;
  role: UserRole;
  token: string;
}

interface LocationState {
  from?: string;
}

interface ProtectedRouteProps {
  allowedRoles?: UserRole[];
}

// Auth helper — returns typed user or null
function getAuthUser(): AuthUser | null {
  try {
    const token = localStorage.getItem('token');
    const userStr = localStorage.getItem('user');
    if (!token || !userStr) return null;
    return JSON.parse(userStr) as AuthUser;
  } catch {
    return null;
  }
}

// Typed ProtectedRoute — handles auth + role checks
function ProtectedRoute({ allowedRoles }: ProtectedRouteProps): JSX.Element {
  const location = useLocation();
  const user = getAuthUser();

  // Not authenticated — redirect to login with return URL
  if (user === null) {
    return (
      <Navigate
        to="/login"
        state={{ from: location.pathname } satisfies LocationState}
        replace
      />
    );
  }

  // Role check — if allowedRoles specified, user must have one of them
  if (allowedRoles !== undefined && !allowedRoles.includes(user.role)) {
    return <Navigate to="/403" replace />;
  }

  // Authenticated and authorized — render the child route
  return <Outlet />;
}

function App(): JSX.Element {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<div>Home</div>} />
        <Route path="/login" element={<LoginPage />} />
        <Route path="/403" element={<div>Forbidden</div>} />

        {/* Protected routes — all users */}
        <Route element={<ProtectedRoute />}>
          <Route path="/dashboard" element={<div>Dashboard</div>} />
        </Route>

        {/* Admin only */}
        <Route element={<ProtectedRoute allowedRoles={['admin']} />}>
          <Route path="/admin" element={<div>Admin Dashboard</div>} />
        </Route>

        {/* Admin or manager */}
        <Route element={<ProtectedRoute allowedRoles={['admin', 'manager']} />}>
          <Route path="/reports" element={<div>Reports</div>} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

function LoginPage(): JSX.Element {
  const navigate = useNavigate();
  const location = useLocation();

  // Cast location.state — it is unknown by default in React Router
  const state = location.state as LocationState | null;
  const from = state?.from ?? '/dashboard';

  const handleLogin = async (e: React.FormEvent<HTMLFormElement>): Promise<void> => {
    e.preventDefault();
    try {
      const res = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email: 'user@example.com', password: 'pass123' }),
      });
      const data = await res.json() as { token: string; user: AuthUser };
      localStorage.setItem('token', data.token);
      localStorage.setItem('user', JSON.stringify(data.user));
      navigate(from, { replace: true });
    } catch (err: unknown) {
      console.error(err instanceof Error ? err.message : 'Login failed');
    }
  };

  return (
    <form onSubmit={handleLogin}>
      <h1>Login</h1>
      <button type="submit">Login</button>
    </form>
  );
}

export default App;
```

**What the interviewer is checking:**
Logic: `<Navigate replace />` before content renders, role check, `from` state for redirect-back.
TypeScript: `ProtectedRouteProps` with optional `allowedRoles?: UserRole[]`,
`location.state as LocationState | null` cast, `AuthUser` typed from storage.

**TypeScript lesson:**
`location.state as LocationState | null` is necessary because React Router types
`location.state` as `unknown` — you must cast it. Using `| null` handles the case
where state was never set. The optional chaining `state?.from ?? '/dashboard'`
safely handles both cases. The `satisfies` operator on the Navigate state
verifies the object matches `LocationState` without widening the type.

</details>

---

### Pattern 6 · Scenario 2 — The Login That Does Not Redirect

🏢 **Situation:**
Users at your food delivery app log in — they see "Login successful!" but nothing
happens. They sit on the login page staring at it. Fix the redirect using
`useNavigate` and add an "already logged in" check — all typed.

**Types used in this scenario:**
```typescript
interface LoginCredentials {
  email: string;
  password: string;
}

interface LoginApiResponse {
  token: string;
  user: AuthUser;
}

type LoginStatus = 'idle' | 'submitting' | 'error';
```

**Here is the broken code:**
```tsx
import { useState } from 'react';

function LoginPage() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [message, setMessage] = useState('');

  const handleSubmit = async (e) => {   // untyped
    e.preventDefault();
    const res = await fetch('/api/auth/login', {
      method: 'POST',
      body: JSON.stringify({ email, password }),
    });
    const data = await res.json();    // untyped
    if (res.ok && data.token) {
      localStorage.setItem('token', data.token);
      setMessage('Login successful!');
      // Forgot to redirect
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={email} onChange={e => setEmail(e.target.value)} />
      <input type="password" value={password} onChange={e => setPassword(e.target.value)} />
      <button>Login</button>
      {message && <p>{message}</p>}
    </form>
  );
}
```

**Your tasks:**
- ✅ Add `useNavigate` and redirect to `/dashboard` after login
- ✅ Type credentials, response, and status state with interfaces
- ✅ Add "already logged in" redirect in `useEffect`
- ✅ Explain in a comment why `useNavigate` not `window.location.href`

<details>
<summary>💡 Hint</summary>

`useNavigate` returns a typed `NavigateFunction`. Call `navigate('/dashboard', { replace: true })`
after storing the token. For "already logged in", check the token in a `useEffect` with `[]`
dependency and call `navigate` if it exists. This runs synchronously on mount before the
user sees the login form.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';
import { useNavigate, useLocation } from 'react-router-dom';

type UserRole = 'admin' | 'manager' | 'user';

interface AuthUser {
  id: string;
  name: string;
  role: UserRole;
}

interface LoginCredentials {
  email: string;
  password: string;
}

interface LoginApiResponse {
  token: string;
  user: AuthUser;
}

interface LocationState {
  from?: string;
}

type LoginStatus = 'idle' | 'submitting' | 'error';

function LoginPage(): JSX.Element {
  const [credentials, setCredentials] = useState<LoginCredentials>({
    email: '',
    password: '',
  });
  const [status, setStatus] = useState<LoginStatus>('idle');
  const [errorMessage, setErrorMessage] = useState<string | null>(null);

  // useNavigate returns NavigateFunction — typed by React Router
  const navigate = useNavigate();
  const location = useLocation();
  const state = location.state as LocationState | null;
  const from = state?.from ?? '/dashboard';

  // Already logged in — redirect immediately
  useEffect(() => {
    const token = localStorage.getItem('token');
    if (token) {
      navigate('/dashboard', { replace: true });
    }
  }, [navigate]);

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>): void => {
    const { name, value } = e.target;
    setCredentials(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>): Promise<void> => {
    e.preventDefault();

    if (!credentials.email || !credentials.password) {
      setErrorMessage('Please fill in all fields');
      return;
    }

    try {
      setStatus('submitting');
      setErrorMessage(null);

      const res = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(credentials),
      });

      if (!res.ok) {
        throw new Error('Invalid credentials');
      }

      // Typed response
      const data = await res.json() as LoginApiResponse;

      localStorage.setItem('token', data.token);
      localStorage.setItem('user', JSON.stringify(data.user));

      // FIX: useNavigate keeps SPA alive — window.location.href reloads the page
      // navigate() updates URL client-side, preserving React state and avoiding full reload
      navigate(from, { replace: true });

    } catch (err: unknown) {
      const message = err instanceof Error ? err.message : 'Login failed';
      setErrorMessage(message);
      setStatus('error');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      {errorMessage && <p style={{ color: 'red' }}>{errorMessage}</p>}

      <input
        name="email"
        type="email"
        value={credentials.email}
        onChange={handleChange}
        placeholder="Email"
        disabled={status === 'submitting'}
      />
      <input
        name="password"
        type="password"
        value={credentials.password}
        onChange={handleChange}
        placeholder="Password"
        disabled={status === 'submitting'}
      />

      <button type="submit" disabled={status === 'submitting'}>
        {status === 'submitting' ? 'Logging in...' : 'Login'}
      </button>
    </form>
  );
}

export default LoginPage;
```

**What the interviewer is checking:**
Logic: `navigate(from, { replace: true })` after login, already-logged-in redirect on mount.
TypeScript: `LoginCredentials` interface, `LoginApiResponse` typed response,
`LoginStatus` union, `location.state as LocationState | null` cast.

**TypeScript lesson:**
`useNavigate()` returns `NavigateFunction` — a typed function from React Router.
`NavigateFunction` accepts `string | number | To` as the first argument and
`NavigateOptions` as the second. TypeScript catches if you pass invalid options.
`window.location.href = '/dashboard'` has no TypeScript benefit — it is a plain
string assignment with no type checking on the URL or options.

</details>

---

### Pattern 6 · Scenario 3 — The Back Button That Breaks Auth

🏢 **Situation:**
At your banking app, users click Logout. They land on `/login`.
They press browser back. They are back on the dashboard, still logged out
but seeing protected content. Your security team wants this fixed immediately
with proper TypeScript typing.

**Types used in this scenario:**
```typescript
interface HeaderProps {
  user: AuthUser;
  onLogout?: () => void;
}
```

**Here is the broken code:**
```tsx
import { useNavigate } from 'react-router-dom';

function Header({ user, onLogout }) {   // untyped
  const navigate = useNavigate();

  const handleLogout = () => {
    localStorage.removeItem('token');
    localStorage.removeItem('user');
    navigate('/login');  // BUG: does not use replace — back button returns to dashboard
  };

  return (
    <header>
      <span>Hello, {user.name}</span>
      <button onClick={handleLogout}>Logout</button>
    </header>
  );
}
```

**Your tasks:**
- ✅ Type with `HeaderProps` interface
- ✅ Fix the back button issue using `navigate('/login', { replace: true })`
- ✅ Add a typed API logout call to invalidate the server session
- ✅ Add a typed loading state during logout

<details>
<summary>💡 Hint</summary>

`navigate('/login', { replace: true })` replaces the current history entry
instead of adding a new one. When the user presses back, the dashboard entry
is gone. Also call the logout API — even if it fails, proceed with
client-side logout. Use `finally` to ensure navigation always happens.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState } from 'react';
import { useNavigate } from 'react-router-dom';

type UserRole = 'admin' | 'manager' | 'user';

interface AuthUser {
  id: string;
  name: string;
  role: UserRole;
}

interface HeaderProps {
  user: AuthUser;
  onLogout?: () => void;
}

function Header({ user, onLogout }: HeaderProps): JSX.Element {
  const navigate = useNavigate();
  const [isLoggingOut, setIsLoggingOut] = useState<boolean>(false);

  const handleLogout = async (): Promise<void> => {
    try {
      setIsLoggingOut(true);

      // 1. Tell server to invalidate the token
      const token = localStorage.getItem('token');
      if (token) {
        await fetch('/api/auth/logout', {
          method: 'POST',
          headers: { Authorization: `Bearer ${token}` },
        }).catch((err: unknown) => {
          // If API fails, still proceed with client logout
          console.warn('Server logout failed:', err instanceof Error ? err.message : err);
        });
      }

    } finally {
      // 2. Always clear client auth data
      localStorage.removeItem('token');
      localStorage.removeItem('user');
      sessionStorage.clear();

      // 3. Notify parent if provided
      onLogout?.();

      // FIX: replace — removes dashboard from history stack
      // Back button now has no dashboard to return to
      navigate('/login', { replace: true });

      setIsLoggingOut(false);
    }
  };

  return (
    <header style={{ display: 'flex', alignItems: 'center', gap: 16 }}>
      <span>Hello, {user.name}</span>
      {/* TypeScript knows role is UserRole — can safely compare */}
      {user.role === 'admin' && (
        <span style={{ fontSize: 12, color: 'blue' }}>Admin</span>
      )}
      <button onClick={handleLogout} disabled={isLoggingOut}>
        {isLoggingOut ? 'Logging out...' : 'Logout'}
      </button>
    </header>
  );
}

export default Header;
```

**What the interviewer is checking:**
Logic: `replace: true` on navigate, API logout call in try block, client clear in finally.
TypeScript: `HeaderProps` with optional `onLogout?: () => void`, typed `AuthUser` with role union,
`isLoggingOut` state typed as `boolean`.

**TypeScript lesson:**
`onLogout?: () => void` in props uses the optional property `?` modifier.
This means the prop can be omitted entirely. Calling it: `onLogout?.()` uses optional
chaining on a function — this only calls it if it is not `undefined`.
Without optional chaining, you would need: `if (onLogout) onLogout()`.
Both are valid TypeScript but `onLogout?.()` is more concise.

</details>

---

### Pattern 6 · Scenario 4 — The Page That Flashes Before Redirecting

🏢 **Situation:**
Your banking app authenticates via an API check on load. Logged-in users visiting
`/login` briefly see the login form before being redirected — a split-second flash.
Users report "the app looks broken." Fix with a typed `isChecking` state.

**Types used in this scenario:**
```typescript
type AuthCheckStatus = 'checking' | 'authenticated' | 'unauthenticated';

interface MeApiResponse {
  user: AuthUser;
  token: string;
}
```

**Here is the broken code:**
```tsx
import { useState, useEffect } from 'react';
import { Navigate } from 'react-router-dom';

function LoginPage() {
  const [user, setUser] = useState(null);   // untyped

  useEffect(() => {
    const check = async () => {
      const token = localStorage.getItem('token');
      if (token) {
        const res = await fetch('/api/auth/me', {
          headers: { Authorization: `Bearer ${token}` },
        });
        if (res.ok) setUser(await res.json());
      }
    };
    check();
  }, []);

  // BUG: user starts null — login form renders first, then Navigate
  if (user) return <Navigate to="/dashboard" replace />;

  return (
    <div>
      <h1>Login</h1>
      <form>...</form>
    </div>
  );
}
```

**Your tasks:**
- ✅ Add `AuthCheckStatus` union state — replace `user` check with status check
- ✅ Show nothing (or spinner) while `status === 'checking'`
- ✅ Only show the login form when `status === 'unauthenticated'`
- ✅ Type the `/api/auth/me` response with `MeApiResponse`

<details>
<summary>💡 Hint</summary>

`type AuthCheckStatus = 'checking' | 'authenticated' | 'unauthenticated'`
starts as `'checking'`. Only show the login form when it becomes `'unauthenticated'`.
While `'checking'`, return a loading div or `null`. TypeScript's exhaustive check
on the status union ensures you handle all three states.

</details>

<details>
<summary>✅ Full Solution</summary>

```tsx
import { useState, useEffect } from 'react';
import { Navigate, useNavigate, useLocation } from 'react-router-dom';

type UserRole = 'admin' | 'manager' | 'user';

interface AuthUser {
  id: string;
  name: string;
  role: UserRole;
}

interface MeApiResponse {
  user: AuthUser;
  token: string;
}

interface LocationState {
  from?: string;
}

// Three states — no more flash
type AuthCheckStatus = 'checking' | 'authenticated' | 'unauthenticated';

function LoginPage(): JSX.Element {
  // FIX: status state machine instead of just user/null
  const [status, setStatus] = useState<AuthCheckStatus>('checking');

  const navigate = useNavigate();
  const location = useLocation();
  const state = location.state as LocationState | null;
  const from = state?.from ?? '/dashboard';

  // Auth check on mount
  useEffect(() => {
    const checkAuth = async (): Promise<void> => {
      const token = localStorage.getItem('token');

      if (!token) {
        // No token — definitely not logged in
        setStatus('unauthenticated');
        return;
      }

      try {
        const res = await fetch('/api/auth/me', {
          headers: { Authorization: `Bearer ${token}` },
        });

        if (res.ok) {
          const data = await res.json() as MeApiResponse;
          // Update stored token if refreshed
          localStorage.setItem('token', data.token);
          setStatus('authenticated');
        } else {
          // Token invalid — clear it
          localStorage.removeItem('token');
          localStorage.removeItem('user');
          setStatus('unauthenticated');
        }
      } catch {
        // Network error — assume unauthenticated
        setStatus('unauthenticated');
      }
    };

    checkAuth();
  }, []);

  // 1. CHECKING — show nothing (prevents flash)
  if (status === 'checking') {
    return <div style={{ display: 'flex', justifyContent: 'center', padding: 40 }}>
      Verifying session...
    </div>;
  }

  // 2. AUTHENTICATED — redirect
  if (status === 'authenticated') {
    return <Navigate to={from} replace />;
  }

  // 3. UNAUTHENTICATED — show login form
  // TypeScript exhaustiveness: all three union members handled above
  return (
    <div>
      <h1>Login</h1>
      <form onSubmit={async (e: React.FormEvent<HTMLFormElement>) => {
        e.preventDefault();
        try {
          const res = await fetch('/api/auth/login', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ email: 'user@example.com', password: 'pass' }),
          });
          const data = await res.json() as { token: string; user: AuthUser };
          localStorage.setItem('token', data.token);
          localStorage.setItem('user', JSON.stringify(data.user));
          navigate(from, { replace: true });
        } catch (err: unknown) {
          console.error(err instanceof Error ? err.message : 'Login failed');
        }
      }}>
        <button type="submit">Login</button>
      </form>
    </div>
  );
}

export default LoginPage;
```

**What the interviewer is checking:**
Logic: three-state check (checking/authenticated/unauthenticated), no flash because
login form only renders in `unauthenticated` state, token cleared on invalid response.
TypeScript: `AuthCheckStatus` union, `MeApiResponse` typed response, exhaustive state handling.

**TypeScript lesson:**
`type AuthCheckStatus = 'checking' | 'authenticated' | 'unauthenticated'` is a
discriminated union used as a state machine. TypeScript's exhaustiveness checking
means if you add a new status value like `'expired'` to the union, the compiler
will warn you everywhere that doesn't handle the new case. This prevents bugs
from forgetting to handle new states — especially valuable when auth requirements evolve.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Interview Pattern Picker

> **"Interviewer asks [X]" → "Go to Pattern [N]"**

| Interviewer Says | Go To |
|---|---|
| "Fetch data and display it — use TypeScript" | Pattern 1 — Fetch & Display |
| "Show loading, error, and empty states" | Pattern 1 — Scenario 1 |
| "The table is empty but the API returns data" | Pattern 1 — Scenario 2 |
| "Type the API response — no any allowed" | Pattern 1 — all scenarios |
| "How do you handle errors in async TypeScript?" | Pattern 1 — `err: unknown` + narrowing |
| "Auto-refresh data every 30 seconds" | Pattern 1 — Scenario 4 |
| "Build a login form with TypeScript" | Pattern 2 — Form + Validation |
| "Type the form submit event handler" | Pattern 2 — `React.FormEvent<HTMLFormElement>` |
| "The form submits multiple times" | Pattern 2 — Scenario 2 |
| "Auto-save form data to localStorage" | Pattern 2 — Scenario 3 |
| "What is Partial<T> and when do you use it?" | Pattern 2 — Scenario 1 errors type |
| "Add search and filter to this product list" | Pattern 3 — List + Search |
| "Filter and sort stop working together" | Pattern 3 — Scenario 3 |
| "The search crashes on special characters" | Pattern 3 — Scenario 4 |
| "How do you type a select onChange handler?" | Pattern 3 — `React.ChangeEvent<HTMLSelectElement>` |
| "Persist cart data across page refreshes" | Pattern 4 — localStorage |
| "Build a generic localStorage utility" | Pattern 4 — Scenario 4 |
| "localStorage crashes for some users" | Pattern 4 — Scenario 4 |
| "How do you type JSON.parse safely?" | Pattern 4 — generic `getFromStorage<T>` |
| "Build a modal that closes on outside click" | Pattern 5 — Show/Hide |
| "Close dropdown when clicking outside — useRef" | Pattern 5 — Scenario 3 |
| "How do you type a useRef for a DOM element?" | Pattern 5 — `useRef<HTMLDivElement>(null)` |
| "Tab content resets when switching tabs" | Pattern 5 — Scenario 4 |
| "Protect a route so only logged-in users see it" | Pattern 6 — Page Navigation |
| "Redirect to the page user tried to visit" | Pattern 6 — Scenario 1 + 2 |
| "How do you type useParams in React Router?" | Pattern 6 — `useParams<{ id: string }>()` |
| "User sees dashboard after logout via back button" | Pattern 6 — Scenario 3 |
| "Login page flashes before redirecting" | Pattern 6 — Scenario 4 |

---

> **Part 1 complete.**
> Say **"Generate Part 2 — Level 2, Patterns 7 to 11"** to continue.
