# React Practice Guide — Part 1
## Level 1: Foundation (80% of Interview Questions)

---

# Table of Contents

- [Pattern 1: 📋 Fetch & Display Data](#pattern-1--fetch--display-data)
  - [Scenario 1 — The Dashboard Nobody Can Use](#-pattern-1--scenario-1--the-dashboard-nobody-can-use)
  - [Scenario 2 — The User Table That Shows Nothing](#-pattern-1--scenario-2--the-user-table-that-shows-nothing)
  - [Scenario 3 — The Profile Page That Crashes](#-pattern-1--scenario-3--the-profile-page-that-crashes)
  - [Scenario 4 — The Data That Goes Stale](#-pattern-1--scenario-4--the-data-that-goes-stale)
- [Pattern 2: 📝 Form + Validation](#pattern-2--form--validation)
  - [Scenario 1 — The Login Form That Lets Anyone In](#-pattern-2--scenario-1--the-login-form-that-lets-anyone-in)
  - [Scenario 2 — The Signup Form That Submits Three Times](#-pattern-2--scenario-2--the-signup-form-that-submits-three-times)
  - [Scenario 3 — The Form That Loses Your Data](#-pattern-2--scenario-3--the-form-that-loses-your-data)
  - [Scenario 4 — The Form With No Feedback](#-pattern-2--scenario-4--the-form-with-no-feedback)
- [Pattern 3: 🔁 List + Search + Filter](#pattern-3--list--search--filter)
  - [Scenario 1 — The Product Page Nobody Can Search](#-pattern-3--scenario-1--the-product-page-nobody-can-search)
  - [Scenario 2 — The Employee List That Filters Wrong](#-pattern-3--scenario-2--the-employee-list-that-filters-wrong)
  - [Scenario 3 — The Job Board That Sorts Incorrectly](#-pattern-3--scenario-3--the-job-board-that-sorts-incorrectly)
  - [Scenario 4 — The Search That Breaks on Special Characters](#-pattern-3--scenario-4--the-search-that-breaks-on-special-characters)
- [Pattern 4: 💾 localStorage Usage](#pattern-4--localstorage-usage)
  - [Scenario 1 — The Cart That Empties on Refresh](#-pattern-4--scenario-1--the-cart-that-empties-on-refresh)
  - [Scenario 2 — The Settings That Reset Every Visit](#-pattern-4--scenario-2--the-settings-that-reset-every-visit)
  - [Scenario 3 — The Form Draft That Disappears](#-pattern-4--scenario-3--the-form-draft-that-disappears)
  - [Scenario 4 — The localStorage That Crashes the App](#-pattern-4--scenario-4--the-localstorage-that-crashes-the-app)
- [Pattern 5: 🔀 Show/Hide UI](#pattern-5--showhide-ui)
  - [Scenario 1 — The Modal That Never Closes](#-pattern-5--scenario-1--the-modal-that-never-closes)
  - [Scenario 2 — The Accordion That Opens Everything](#-pattern-5--scenario-2--the-accordion-that-opens-everything)
  - [Scenario 3 — The Dropdown That Stays Open](#-pattern-5--scenario-3--the-dropdown-that-stays-open)
  - [Scenario 4 — The Tabs That Lose Their State](#-pattern-5--scenario-4--the-tabs-that-lose-their-state)
- [Pattern 6: 🧭 Page Navigation](#pattern-6--page-navigation)
  - [Scenario 1 — The Protected Page Anyone Can Visit](#-pattern-6--scenario-1--the-protected-page-anyone-can-visit)
  - [Scenario 2 — The Login That Does Not Redirect](#-pattern-6--scenario-2--the-login-that-does-not-redirect)
  - [Scenario 3 — The Back Button That Breaks Auth](#-pattern-6--scenario-3--the-back-button-that-breaks-auth)
  - [Scenario 4 — The Page That Flashes Before Redirecting](#-pattern-6--scenario-4--the-page-that-flashes-before-redirecting)
- [Interview Pattern Picker](#interview-pattern-picker)

---

# Pattern 1: 📋 Fetch & Display Data

> **Level 1 — Foundation**

🎯 **What you will build in this pattern:**
- A dashboard that loads users/products from an API and displays them in a table
- A profile page that fetches a single record by ID
- Any page that loads data when it mounts and shows it to the user

📋 **Interview asks this as:**
- "Fetch a list of users from this API and display them in a table"
- "Build a product listing page that loads data on mount"
- "Show loading and error states while fetching data"

🚨 **Most common mistake in interviews:**
Showing the data but forgetting loading state, error state, and empty state.
Interviewers want to see all three — not just the happy path.

---

### 🔴 Pattern 1 · Scenario 1 — The Dashboard Nobody Can Use

🏢 **Situation:**
You joined a fintech startup. The analytics dashboard just shows
a blank white screen while the API loads. When the API fails,
it still shows a blank screen. Users are calling support thinking
the app is broken. Your job: make it usable.

**Here is the current code:**

```jsx
import { useState, useEffect } from 'react';

function TransactionDashboard() {
  const [transactions, setTransactions] = useState([]);

  useEffect(() => {
    fetch('/api/transactions')
      .then(res => res.json())
      .then(data => setTransactions(data));
  }, []);

  return (
    <div>
      <h1>Transactions</h1>
      {transactions.map(tx => (
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
- ✅ Add a loading state that shows a spinner while data is fetching
- ✅ Add an error state that shows a message when the API fails
- ✅ Add an empty state when the API returns an empty array
- ✅ Use async/await instead of .then() chains

<details>
<summary>💡 Hint</summary>

You need three pieces of state, not one.
Ask yourself: what are the three things that can happen
when you call an API?
It is loading, it succeeded, or it failed.
Each needs its own state variable and its own UI.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect } from 'react';

function TransactionDashboard() {
  const [transactions, setTransactions] = useState([]);
  const [isLoading, setIsLoading] = useState(true);   // start true — loading on mount
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchTransactions = async () => {
      try {
        setIsLoading(true);
        setError(null); // clear previous errors before new fetch

        const res = await fetch('/api/transactions');

        // fetch does NOT throw on 4xx/5xx — you must check manually
        if (!res.ok) {
          throw new Error(`Server error: ${res.status}`);
        }

        const data = await res.json();
        setTransactions(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false); // always stop loading, success or fail
      }
    };

    fetchTransactions();
  }, []); // empty array = run once on mount

  // Render loading state
  if (isLoading) {
    return <div>Loading transactions...</div>;
  }

  // Render error state
  if (error) {
    return <div>Error: {error}. Please try again.</div>;
  }

  // Render empty state
  if (transactions.length === 0) {
    return <div>No transactions found.</div>;
  }

  // Render data
  return (
    <div>
      <h1>Transactions</h1>
      {transactions.map(tx => (
        <div key={tx.id}>
          <span>{tx.description}</span>
          <span>₹{tx.amount}</span>
        </div>
      ))}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
They want to see that you handle all three API states — loading,
error, and empty — not just the success case. They also want
to see that you know fetch does not throw on 4xx/5xx errors
and that you check `res.ok` manually.

</details>

---

### 🔴 Pattern 1 · Scenario 2 — The User Table That Shows Nothing

🏢 **Situation:**
You are working at an HR tool company. The employee directory
loads data fine in the network tab — you can see 200 OK with
data. But the table on screen is completely empty. Your senior
dev asks you to find and fix the bug in 15 minutes.

**Here is the current code:**

```jsx
import { useState, useEffect } from 'react';

function EmployeeDirectory() {
  const [employees, setEmployees] = useState([]);
  const [isLoading, setIsLoading] = useState(false);

  useEffect(() => {
    const loadEmployees = async () => {
      setIsLoading(true);
      const res = await fetch('/api/employees');
      const data = await res.json();
      // API returns: { employees: [...], total: 120, page: 1 }
      setEmployees(data);
      setIsLoading(false);
    };

    loadEmployees();
  }, []);

  if (isLoading) return <p>Loading...</p>;

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Department</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        {employees.map(emp => (
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
- ✅ Find the bug causing the empty table
- ✅ Fix it so employees display correctly
- ✅ Add error handling that is currently missing

<details>
<summary>💡 Hint</summary>

Look at the comment in the code — the API returns an object
with a key called `employees`, not a plain array.
When you call `setEmployees(data)`, what are you actually storing?
Map the response to the right shape before storing it.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect } from 'react';

function EmployeeDirectory() {
  const [employees, setEmployees] = useState([]);
  const [isLoading, setIsLoading] = useState(true); // bug 2: should start true
  const [error, setError] = useState(null);

  useEffect(() => {
    const loadEmployees = async () => {
      try {
        setIsLoading(true);
        const res = await fetch('/api/employees');

        if (!res.ok) throw new Error('Failed to load employees');

        const data = await res.json();

        // BUG FIX: API returns { employees: [...], total, page }
        // You must extract the array, not store the whole object
        setEmployees(data.employees);

      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false);
      }
    };

    loadEmployees();
  }, []);

  if (isLoading) return <p>Loading employees...</p>;
  if (error) return <p>Could not load employees: {error}</p>;
  if (employees.length === 0) return <p>No employees found.</p>;

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Department</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        {employees.map(emp => (
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

**✅ What the interviewer is checking:**
They want to see that you can debug a data shape mismatch —
one of the most common real-world bugs. They also check that
you notice `isLoading` starts as `false` when it should start
as `true`, since data is loading immediately on mount.

</details>

---

### 🔴 Pattern 1 · Scenario 3 — The Profile Page That Crashes

🏢 **Situation:**
You are at a social platform company. The user profile page
works for most users but randomly crashes with "Cannot read
properties of undefined". It happens when users click a profile
before the data has fully loaded. Your PM wants it fixed before
the weekend launch.

**Here is the current code:**

```jsx
import { useState, useEffect } from 'react';
import { useParams } from 'react-router-dom';

function UserProfile() {
  const { userId } = useParams();
  const [user, setUser] = useState(null);

  useEffect(() => {
    const loadUser = async () => {
      const res = await fetch(`/api/users/${userId}`);
      const data = await res.json();
      setUser(data);
    };
    loadUser();
  }, [userId]);

  return (
    <div>
      <img src={user.avatar} alt={user.name} />
      <h1>{user.name}</h1>
      <p>{user.bio}</p>
      <p>Followers: {user.followers.count}</p>
      <p>Joined: {user.joinedAt}</p>
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the crash that happens before data loads
- ✅ Add proper loading state
- ✅ Handle the case where a user ID does not exist (404)
- ✅ Handle when userId changes (user navigates to another profile)

<details>
<summary>💡 Hint</summary>

When the component first renders, `user` is `null`.
But the JSX immediately tries to access `user.avatar`,
`user.name`, `user.followers.count` — all on a null object.
You need to guard against this. Also think about what happens
when `userId` changes — do you need to reset state?

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect } from 'react';
import { useParams } from 'react-router-dom';

function UserProfile() {
  const { userId } = useParams();
  const [user, setUser] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const loadUser = async () => {
      try {
        // Reset state when userId changes (navigating between profiles)
        setIsLoading(true);
        setError(null);
        setUser(null);

        const res = await fetch(`/api/users/${userId}`);

        // Handle 404 — user does not exist
        if (res.status === 404) {
          throw new Error('User not found');
        }

        if (!res.ok) {
          throw new Error('Failed to load profile');
        }

        const data = await res.json();
        setUser(data);

      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false);
      }
    };

    loadUser();
  }, [userId]); // re-run when userId changes

  if (isLoading) return <div>Loading profile...</div>;
  if (error) return <div>{error}</div>;
  if (!user) return null; // safety guard

  // Only reach here when user data is fully loaded
  return (
    <div>
      <img src={user.avatar} alt={user.name} />
      <h1>{user.name}</h1>
      <p>{user.bio}</p>
      {/* Optional chaining for nested objects */}
      <p>Followers: {user.followers?.count ?? 0}</p>
      <p>Joined: {user.joinedAt}</p>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
They want to see that you understand why the crash happens —
accessing properties on null — and that you guard the render
until data arrives. They also look for `userId` in the dependency
array and state reset when the ID changes.

</details>

---

### 🔴 Pattern 1 · Scenario 4 — The Data That Goes Stale

🏢 **Situation:**
You are building a food delivery app. The order status page
fetches order data when it loads. But users complain that the
status never updates — they have to refresh the page manually
to see if their order moved from "Preparing" to "On the way".
Your tech lead says: add a refresh button AND auto-refresh
every 30 seconds.

**Here is the current code:**

```jsx
import { useState, useEffect } from 'react';

function OrderStatus({ orderId }) {
  const [order, setOrder] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const loadOrder = async () => {
      try {
        setIsLoading(true);
        const res = await fetch(`/api/orders/${orderId}`);
        if (!res.ok) throw new Error('Failed to load order');
        const data = await res.json();
        setOrder(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setIsLoading(false);
      }
    };
    loadOrder();
  }, [orderId]);

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;
  if (!order) return null;

  return (
    <div>
      <h2>Order #{order.id}</h2>
      <p>Status: {order.status}</p>
      <p>Estimated arrival: {order.eta}</p>
    </div>
  );
}
```

**Your tasks:**
- ✅ Add a manual refresh button that re-fetches order data
- ✅ Add auto-refresh every 30 seconds
- ✅ Stop auto-refresh when the order status is "delivered"
- ✅ Clean up the interval when the component unmounts

<details>
<summary>💡 Hint</summary>

Extract the fetch logic into a separate function so both
the manual button and the auto-refresh interval can call
the same function. For the interval, think about what
happens when the component unmounts — you need to clean
it up in your useEffect return function or it keeps running.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect, useCallback } from 'react';

function OrderStatus({ orderId }) {
  const [order, setOrder] = useState(null);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState(null);

  // useCallback so fetchOrder has a stable reference
  // and can be used safely in useEffect dependency array
  const fetchOrder = useCallback(async () => {
    try {
      setIsLoading(true);
      setError(null);
      const res = await fetch(`/api/orders/${orderId}`);
      if (!res.ok) throw new Error('Failed to load order');
      const data = await res.json();
      setOrder(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setIsLoading(false);
    }
  }, [orderId]);

  // Initial fetch
  useEffect(() => {
    fetchOrder();
  }, [fetchOrder]);

  // Auto-refresh every 30 seconds
  // Stop if order is delivered
  useEffect(() => {
    // Do not start interval if order is already delivered
    if (order?.status === 'delivered') return;

    const intervalId = setInterval(() => {
      fetchOrder();
    }, 30000); // 30 seconds

    // CLEANUP — critical: stop interval when component unmounts
    // or when order becomes delivered
    return () => clearInterval(intervalId);

  }, [fetchOrder, order?.status]); // re-evaluate when status changes

  if (isLoading && !order) return <p>Loading order...</p>;
  if (error) return <p>Error: {error}</p>;
  if (!order) return null;

  return (
    <div>
      <h2>Order #{order.id}</h2>
      <p>Status: {order.status}</p>
      <p>Estimated arrival: {order.eta}</p>

      {/* Manual refresh button */}
      <button onClick={fetchOrder} disabled={isLoading}>
        {isLoading ? 'Refreshing...' : 'Refresh Status'}
      </button>

      {order.status === 'delivered' && (
        <p>Your order has been delivered!</p>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
They want to see cleanup of `setInterval` in the useEffect
return — this is the most common memory leak in React.
They also look for the auto-stop when status is "delivered"
and the disabled state on the button while loading.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 2: 📝 Form + Validation

> **Level 1 — Foundation**

🎯 **What you will build in this pattern:**
- A login form with email/password validation and API submission
- A signup form with multiple fields, real-time validation, and error messages
- A settings form that updates user data and shows success/failure feedback

📋 **Interview asks this as:**
- "Build a login form with validation"
- "Create a signup form — validate all fields before submitting"
- "Build a contact form that calls an API and shows a success message"

🚨 **Most common mistake in interviews:**
Submitting the form without preventing the default page reload.
Forgetting `e.preventDefault()` in the submit handler crashes
the entire SPA and loses all state.

---

### 🔴 Pattern 2 · Scenario 1 — The Login Form That Lets Anyone In

🏢 **Situation:**
You are at a SaaS company. The login form submits even when
the email field is empty and the password is two characters.
Your QA engineer found that you can log in with blank fields.
Fix the validation before the security audit tomorrow.

**Here is the current code:**

```jsx
import { useState } from 'react';

function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = async (e) => {
    const res = await fetch('/api/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password }),
    });
    const data = await res.json();
    if (data.token) {
      localStorage.setItem('token', data.token);
      window.location.href = '/dashboard';
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Email"
        value={email}
        onChange={e => setEmail(e.target.value)}
      />
      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={e => setPassword(e.target.value)}
      />
      <button type="submit">Login</button>
    </form>
  );
}
```

**Your tasks:**
- ✅ Add `e.preventDefault()` to stop page reload on submit
- ✅ Validate email format (must contain @ and a domain)
- ✅ Validate password minimum length (at least 6 characters)
- ✅ Show inline error messages under each field
- ✅ Show a general error if the API returns invalid credentials

<details>
<summary>💡 Hint</summary>

Store errors in a separate state object — one key per field.
Validate before the API call and return early if invalid.
Also: fetch does not throw on 401 responses — you need to
check `res.ok` or `res.status` manually.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState } from 'react';

function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [apiError, setApiError] = useState('');

  const validate = () => {
    const newErrors = {};

    // Email validation
    if (!email) {
      newErrors.email = 'Email is required';
    } else if (!email.includes('@') || !email.includes('.')) {
      newErrors.email = 'Enter a valid email address';
    }

    // Password validation
    if (!password) {
      newErrors.password = 'Password is required';
    } else if (password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }

    return newErrors;
  };

  const handleSubmit = async (e) => {
    e.preventDefault(); // CRITICAL — stops page reload

    // Clear previous API error
    setApiError('');

    // Run validation
    const validationErrors = validate();
    if (Object.keys(validationErrors).length > 0) {
      setErrors(validationErrors);
      return; // stop here — do not call API
    }

    setErrors({});

    try {
      setIsSubmitting(true);

      const res = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email, password }),
      });

      const data = await res.json();

      if (!res.ok) {
        // API returned 401 or other error
        setApiError(data.message || 'Invalid email or password');
        return;
      }

      localStorage.setItem('token', data.token);
      window.location.href = '/dashboard';

    } catch (err) {
      setApiError('Network error. Please try again.');
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* General API error */}
      {apiError && <p style={{ color: 'red' }}>{apiError}</p>}

      <div>
        <input
          type="email"
          placeholder="Email"
          value={email}
          onChange={e => setEmail(e.target.value)}
        />
        {/* Inline field error */}
        {errors.email && <p style={{ color: 'red' }}>{errors.email}</p>}
      </div>

      <div>
        <input
          type="password"
          placeholder="Password"
          value={password}
          onChange={e => setPassword(e.target.value)}
        />
        {errors.password && <p style={{ color: 'red' }}>{errors.password}</p>}
      </div>

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Logging in...' : 'Login'}
      </button>
    </form>
  );
}
```

**✅ What the interviewer is checking:**
They look for `e.preventDefault()` first — if it is missing,
everything else fails. Then they check for validation before
the API call, disabled submit button during submission,
and separate handling of validation errors vs API errors.

</details>

---

### 🔴 Pattern 2 · Scenario 2 — The Signup Form That Submits Three Times

🏢 **Situation:**
Users on your e-commerce platform are receiving three welcome
emails after signup. Your backend team says the signup API is
being called three times per submission. They suspect a frontend
bug. You look at the signup form and find the problem in 2 minutes.

**Here is the current code:**

```jsx
import { useState } from 'react';

function SignupForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = async () => {
    if (!name || !email || !password) return;

    const res = await fetch('/api/auth/signup', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ name, email, password }),
    });

    if (res.ok) {
      alert('Account created!');
    }
  };

  return (
    <form>
      <input value={name} onChange={e => setName(e.target.value)}
        placeholder="Full Name" />
      <input value={email} onChange={e => setEmail(e.target.value)}
        placeholder="Email" />
      <input type="password" value={password}
        onChange={e => setPassword(e.target.value)}
        placeholder="Password" />
      <button onClick={handleSubmit}>Create Account</button>
    </form>
  );
}
```

**Your tasks:**
- ✅ Find and fix why the API is called three times
- ✅ Add an `isSubmitting` guard to prevent duplicate submissions
- ✅ Show a loading state on the button during submission
- ✅ Clear the form after successful signup

<details>
<summary>💡 Hint</summary>

Look at the button type and how the handler is attached.
When a button inside a form has no type, it defaults to
`type="submit"` — which triggers the form's submit event.
If you also have an `onClick` handler calling the same
function, and the form itself submits... how many times
does the function run?

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState } from 'react';

function SignupForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    password: '',
  });
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [error, setError] = useState('');
  const [success, setSuccess] = useState(false);

  // One handler for all fields using name attribute
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = async (e) => {
    e.preventDefault(); // FIX 1: prevent default form submission

    // FIX 2: guard against duplicate submissions
    if (isSubmitting) return;

    if (!formData.name || !formData.email || !formData.password) {
      setError('All fields are required');
      return;
    }

    try {
      setIsSubmitting(true);
      setError('');

      const res = await fetch('/api/auth/signup', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData),
      });

      if (!res.ok) {
        const data = await res.json();
        throw new Error(data.message || 'Signup failed');
      }

      // Clear form on success
      setFormData({ name: '', email: '', password: '' });
      setSuccess(true);

    } catch (err) {
      setError(err.message);
    } finally {
      setIsSubmitting(false);
    }
  };

  if (success) {
    return <div>Account created! Please check your email.</div>;
  }

  return (
    // FIX 3: use onSubmit on form, not onClick on button
    <form onSubmit={handleSubmit}>
      {error && <p style={{ color: 'red' }}>{error}</p>}

      <input
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Full Name"
      />
      <input
        name="email"
        type="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <input
        name="password"
        type="password"
        value={formData.password}
        onChange={handleChange}
        placeholder="Password"
      />

      {/* FIX 4: type="submit" is explicit and clear */}
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Creating Account...' : 'Create Account'}
      </button>
    </form>
  );
}
```

**✅ What the interviewer is checking:**
The core bug — button with no type inside a form + onClick
causes double submission. They also check for the `isSubmitting`
guard, the disabled button, and that you use `onSubmit` on
the form not `onClick` on the button.

</details>

---

### 🔴 Pattern 2 · Scenario 3 — The Form That Loses Your Data

🏢 **Situation:**
You are building a job portal. Candidates fill out a long
application form — name, cover letter, work experience,
skills. Halfway through, they accidentally close the tab
or their browser crashes. All their work is gone.
Your PM wants the form to auto-save as the user types.

**Here is the current code:**

```jsx
import { useState } from 'react';

function JobApplicationForm({ jobId }) {
  const [formData, setFormData] = useState({
    fullName: '',
    coverLetter: '',
    experience: '',
    skills: '',
  });

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    await fetch(`/api/jobs/${jobId}/apply`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(formData),
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="fullName" value={formData.fullName}
        onChange={handleChange} placeholder="Full Name" />
      <textarea name="coverLetter" value={formData.coverLetter}
        onChange={handleChange} placeholder="Cover Letter" />
      <textarea name="experience" value={formData.experience}
        onChange={handleChange} placeholder="Work Experience" />
      <input name="skills" value={formData.skills}
        onChange={handleChange} placeholder="Skills" />
      <button type="submit">Submit Application</button>
    </form>
  );
}
```

**Your tasks:**
- ✅ Auto-save form data to localStorage as the user types
- ✅ Restore saved data when the component mounts
- ✅ Clear saved data from localStorage after successful submission
- ✅ Show a "Draft saved" indicator when auto-save happens

<details>
<summary>💡 Hint</summary>

You need two useEffects — one that runs on mount to restore
saved data, and one that runs whenever formData changes to
save it. Use a unique key in localStorage that includes
the jobId so different job applications do not overwrite
each other.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect } from 'react';

function JobApplicationForm({ jobId }) {
  const STORAGE_KEY = `job-application-draft-${jobId}`;

  const [formData, setFormData] = useState({
    fullName: '',
    coverLetter: '',
    experience: '',
    skills: '',
  });
  const [savedAt, setSavedAt] = useState(null);
  const [isSubmitting, setIsSubmitting] = useState(false);

  // RESTORE saved draft on mount
  useEffect(() => {
    const saved = localStorage.getItem(STORAGE_KEY);
    if (saved) {
      try {
        const parsed = JSON.parse(saved);
        setFormData(parsed);
      } catch (err) {
        // Corrupted data — remove it
        localStorage.removeItem(STORAGE_KEY);
      }
    }
  }, [STORAGE_KEY]);

  // AUTO-SAVE whenever formData changes
  useEffect(() => {
    const timer = setTimeout(() => {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(formData));
      setSavedAt(new Date().toLocaleTimeString());
    }, 1000); // debounce — save 1 second after user stops typing

    return () => clearTimeout(timer); // cancel previous timer
  }, [formData, STORAGE_KEY]);

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      setIsSubmitting(true);
      const res = await fetch(`/api/jobs/${jobId}/apply`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData),
      });

      if (!res.ok) throw new Error('Submission failed');

      // Clear draft from localStorage after successful submit
      localStorage.removeItem(STORAGE_KEY);
      alert('Application submitted!');

    } catch (err) {
      alert('Failed to submit. Your draft is saved.');
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* Draft saved indicator */}
      {savedAt && (
        <p style={{ color: 'green', fontSize: '12px' }}>
          Draft saved at {savedAt}
        </p>
      )}

      <input name="fullName" value={formData.fullName}
        onChange={handleChange} placeholder="Full Name" />
      <textarea name="coverLetter" value={formData.coverLetter}
        onChange={handleChange} placeholder="Cover Letter" />
      <textarea name="experience" value={formData.experience}
        onChange={handleChange} placeholder="Work Experience" />
      <input name="skills" value={formData.skills}
        onChange={handleChange} placeholder="Skills" />

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Submitting...' : 'Submit Application'}
      </button>
    </form>
  );
}
```

**✅ What the interviewer is checking:**
They want to see the two-useEffect pattern — one for restore
on mount, one for save on change. The debounce (setTimeout
in useEffect with cleanup) shows you know not to hammer
localStorage on every single keystroke.

</details>

---

### 🔴 Pattern 2 · Scenario 4 — The Form With No Feedback

🏢 **Situation:**
You are at a healthcare portal. Patients fill out an appointment
request form and click submit. Nothing happens visually — no
spinner, no success message, no error message. They click submit
again. And again. Your backend team is getting 5-10 duplicate
appointments per user. Fix the UX so users know what is happening.

**Here is the current code:**

```jsx
import { useState } from 'react';

function AppointmentForm() {
  const [doctorId, setDoctorId] = useState('');
  const [date, setDate] = useState('');
  const [reason, setReason] = useState('');

  const handleSubmit = async (e) => {
    e.preventDefault();
    await fetch('/api/appointments', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ doctorId, date, reason }),
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <select value={doctorId} onChange={e => setDoctorId(e.target.value)}>
        <option value="">Select Doctor</option>
        <option value="d1">Dr. Smith</option>
        <option value="d2">Dr. Jones</option>
      </select>
      <input type="date" value={date} onChange={e => setDate(e.target.value)} />
      <textarea value={reason} onChange={e => setReason(e.target.value)}
        placeholder="Reason for visit" />
      <button type="submit">Book Appointment</button>
    </form>
  );
}
```

**Your tasks:**
- ✅ Disable the submit button while the form is submitting
- ✅ Show a loading spinner or text on the button during submission
- ✅ Show a success state after booking (replace form with confirmation)
- ✅ Show an error message if the booking fails
- ✅ Add basic validation — all fields must be filled

<details>
<summary>💡 Hint</summary>

Think about the three states this form can be in:
idle (waiting for user), submitting (API call in progress),
and done (success or error). Each state needs different UI.
The success state should replace the form entirely —
not just show a message above it.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState } from 'react';

function AppointmentForm() {
  const [formData, setFormData] = useState({
    doctorId: '',
    date: '',
    reason: '',
  });
  const [status, setStatus] = useState('idle'); // idle | submitting | success | error
  const [errorMessage, setErrorMessage] = useState('');
  const [appointment, setAppointment] = useState(null);

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const validate = () => {
    if (!formData.doctorId) return 'Please select a doctor';
    if (!formData.date) return 'Please select a date';
    if (!formData.reason.trim()) return 'Please enter a reason for your visit';
    return null; // null means valid
  };

  const handleSubmit = async (e) => {
    e.preventDefault();

    const validationError = validate();
    if (validationError) {
      setErrorMessage(validationError);
      return;
    }

    try {
      setStatus('submitting');
      setErrorMessage('');

      const res = await fetch('/api/appointments', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData),
      });

      if (!res.ok) {
        const data = await res.json();
        throw new Error(data.message || 'Booking failed. Please try again.');
      }

      const data = await res.json();
      setAppointment(data);
      setStatus('success');

    } catch (err) {
      setErrorMessage(err.message);
      setStatus('error');
    }
  };

  // Success state — replace form with confirmation
  if (status === 'success' && appointment) {
    return (
      <div>
        <h2>Appointment Confirmed!</h2>
        <p>Date: {appointment.date}</p>
        <p>Doctor: {appointment.doctorName}</p>
        <p>Confirmation: #{appointment.confirmationNumber}</p>
        <button onClick={() => setStatus('idle')}>
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
        onChange={handleChange}
      >
        <option value="">Select Doctor</option>
        <option value="d1">Dr. Smith</option>
        <option value="d2">Dr. Jones</option>
      </select>

      <input
        type="date"
        name="date"
        value={formData.date}
        onChange={handleChange}
        min={new Date().toISOString().split('T')[0]} // no past dates
      />

      <textarea
        name="reason"
        value={formData.reason}
        onChange={handleChange}
        placeholder="Reason for visit"
      />

      <button
        type="submit"
        disabled={status === 'submitting'}
      >
        {status === 'submitting' ? 'Booking...' : 'Book Appointment'}
      </button>
    </form>
  );
}
```

**✅ What the interviewer is checking:**
The `status` state machine approach (idle/submitting/success/error)
is the pattern they want to see — not separate boolean flags.
They also look for replacing the form with a success state,
not just showing a message above the form.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 3: 🔁 List + Search + Filter

> **Level 1 — Foundation**

🎯 **What you will build in this pattern:**
- A product listing page with a search bar and category filter
- An employee directory with search, department filter, and sort
- A job board with multiple simultaneous filters

📋 **Interview asks this as:**
- "Add search and filter to this list"
- "Build a product page where users can filter by category and search by name"
- "Make this table searchable and sortable"

🚨 **Most common mistake in interviews:**
Storing the filtered list in state instead of computing it.
When you store filtered results in state, you lose the original
data and cannot reset filters. Always filter from the original list.

---

### 🔴 Pattern 3 · Scenario 1 — The Product Page Nobody Can Search

🏢 **Situation:**
You are building an e-commerce site. The product list loads
fine and shows 120 products. But users cannot find anything —
there is no search, no filter, nothing. Your PM says:
"Ship search and filter by category by end of day."

**Here is the current code:**

```jsx
import { useState, useEffect } from 'react';

function ProductListing() {
  const [products, setProducts] = useState([]);
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
      <div className="product-grid">
        {products.map(product => (
          <div key={product.id}>
            <img src={product.image} alt={product.name} />
            <h3>{product.name}</h3>
            <p>₹{product.price}</p>
            <p>{product.category}</p>
          </div>
        ))}
      </div>
    </div>
  );
}
```

**Your tasks:**
- ✅ Add a search input that filters products by name
- ✅ Add a category dropdown that filters by category
- ✅ Both filters must work together at the same time
- ✅ Show a "No products found" message when filters match nothing

<details>
<summary>💡 Hint</summary>

Do NOT store the filtered results in state.
Store only the search term and selected category in state.
Then compute the filtered list directly in the render —
`const filtered = products.filter(...)`.
This way you always have the full list and can reset filters.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect } from 'react';

function ProductListing() {
  const [products, setProducts] = useState([]);   // original — never modified
  const [isLoading, setIsLoading] = useState(true);
  const [searchTerm, setSearchTerm] = useState('');
  const [selectedCategory, setSelectedCategory] = useState('all');

  useEffect(() => {
    const load = async () => {
      const res = await fetch('/api/products');
      const data = await res.json();
      setProducts(data);
      setIsLoading(false);
    };
    load();
  }, []);

  // DERIVED STATE — compute on every render, no need to store in state
  // Both filters apply together
  const filteredProducts = products.filter(product => {
    const matchesSearch = product.name
      .toLowerCase()
      .includes(searchTerm.toLowerCase());

    const matchesCategory =
      selectedCategory === 'all' || product.category === selectedCategory;

    return matchesSearch && matchesCategory;
  });

  // Build category list from actual data (no hardcoding)
  const categories = ['all', ...new Set(products.map(p => p.category))];

  if (isLoading) return <p>Loading products...</p>;

  return (
    <div>
      <h1>Products</h1>

      {/* Search input */}
      <input
        type="text"
        placeholder="Search products..."
        value={searchTerm}
        onChange={e => setSearchTerm(e.target.value)}
      />

      {/* Category filter */}
      <select
        value={selectedCategory}
        onChange={e => setSelectedCategory(e.target.value)}
      >
        {categories.map(cat => (
          <option key={cat} value={cat}>
            {cat === 'all' ? 'All Categories' : cat}
          </option>
        ))}
      </select>

      {/* Result count */}
      <p>{filteredProducts.length} products found</p>

      {/* Empty state */}
      {filteredProducts.length === 0 ? (
        <p>No products match your search. Try different filters.</p>
      ) : (
        <div className="product-grid">
          {filteredProducts.map(product => (
            <div key={product.id}>
              <img src={product.image} alt={product.name} />
              <h3>{product.name}</h3>
              <p>₹{product.price}</p>
              <p>{product.category}</p>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The key insight they want: filter is derived state, not stored state.
`products` never changes — only `searchTerm` and `selectedCategory`
change. They also look for both filters working together with `&&`.

</details>

---

### 🔴 Pattern 3 · Scenario 2 — The Employee List That Filters Wrong

🏢 **Situation:**
You are at an HR tool company. The employee directory has a
department filter dropdown. When you select "Engineering",
it shows Engineering employees. But when you then type
in the search box, it searches the full list — ignoring
the department filter you already selected. Fix it.

**Here is the current code:**

```jsx
import { useState, useEffect } from 'react';

function EmployeeDirectory() {
  const [employees, setEmployees] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [department, setDepartment] = useState('all');
  const [displayedEmployees, setDisplayedEmployees] = useState([]);

  useEffect(() => {
    const load = async () => {
      const res = await fetch('/api/employees');
      const data = await res.json();
      setEmployees(data);
      setDisplayedEmployees(data);
    };
    load();
  }, []);

  const handleSearch = (e) => {
    const term = e.target.value;
    setSearchTerm(term);
    // Filters only by search, ignores department
    const filtered = employees.filter(emp =>
      emp.name.toLowerCase().includes(term.toLowerCase())
    );
    setDisplayedEmployees(filtered);
  };

  const handleDepartmentChange = (e) => {
    const dept = e.target.value;
    setDepartment(dept);
    // Filters only by department, ignores search
    if (dept === 'all') {
      setDisplayedEmployees(employees);
    } else {
      const filtered = employees.filter(emp => emp.department === dept);
      setDisplayedEmployees(filtered);
    }
  };

  return (
    <div>
      <input value={searchTerm} onChange={handleSearch}
        placeholder="Search by name..." />
      <select value={department} onChange={handleDepartmentChange}>
        <option value="all">All Departments</option>
        <option value="engineering">Engineering</option>
        <option value="design">Design</option>
        <option value="marketing">Marketing</option>
      </select>
      {displayedEmployees.map(emp => (
        <div key={emp.id}>
          <strong>{emp.name}</strong> — {emp.department}
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix filters so search and department always work together
- ✅ Remove `displayedEmployees` state — it is the wrong pattern
- ✅ Add an employee count showing how many match current filters
- ✅ Add a "Clear filters" button that resets both filters

<details>
<summary>💡 Hint</summary>

The problem is storing filtered results in state.
Each handler only applies its own filter and ignores the other.
Remove `displayedEmployees` state entirely.
Compute the filtered list from `employees` using both
`searchTerm` and `department` together in one place.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect } from 'react';

function EmployeeDirectory() {
  const [employees, setEmployees] = useState([]);
  const [isLoading, setIsLoading] = useState(true);
  const [searchTerm, setSearchTerm] = useState('');
  const [department, setDepartment] = useState('all');

  // REMOVED: displayedEmployees state — that was the bug

  useEffect(() => {
    const load = async () => {
      const res = await fetch('/api/employees');
      const data = await res.json();
      setEmployees(data);
      setIsLoading(false);
    };
    load();
  }, []);

  // SINGLE place where both filters apply together
  const filteredEmployees = employees.filter(emp => {
    const matchesSearch = emp.name
      .toLowerCase()
      .includes(searchTerm.toLowerCase());

    const matchesDepartment =
      department === 'all' || emp.department === department;

    return matchesSearch && matchesDepartment;
  });

  const clearFilters = () => {
    setSearchTerm('');
    setDepartment('all');
  };

  const hasActiveFilters = searchTerm !== '' || department !== 'all';

  if (isLoading) return <p>Loading...</p>;

  return (
    <div>
      <input
        value={searchTerm}
        onChange={e => setSearchTerm(e.target.value)}
        placeholder="Search by name..."
      />

      <select
        value={department}
        onChange={e => setDepartment(e.target.value)}
      >
        <option value="all">All Departments</option>
        <option value="engineering">Engineering</option>
        <option value="design">Design</option>
        <option value="marketing">Marketing</option>
      </select>

      {/* Clear filters button */}
      {hasActiveFilters && (
        <button onClick={clearFilters}>Clear Filters</button>
      )}

      <p>
        Showing {filteredEmployees.length} of {employees.length} employees
      </p>

      {filteredEmployees.length === 0 ? (
        <p>No employees match your filters.</p>
      ) : (
        filteredEmployees.map(emp => (
          <div key={emp.id}>
            <strong>{emp.name}</strong> — {emp.department}
          </div>
        ))
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
They specifically want to see you remove `displayedEmployees`
and replace it with a computed variable. This is the single
most important concept in filtering — derived state vs stored state.

</details>

---

### 🔴 Pattern 3 · Scenario 3 — The Job Board That Sorts Incorrectly

🏢 **Situation:**
You are working on a job portal. The job listings page has
a sort dropdown — "Newest First", "Oldest First", "Salary High to Low".
Users are complaining that after sorting, the filter stops working.
After you sort, searching by company name shows no results.

**Here is the current code:**

```jsx
import { useState, useEffect } from 'react';

function JobBoard() {
  const [jobs, setJobs] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [sortBy, setSortBy] = useState('newest');

  useEffect(() => {
    const load = async () => {
      const res = await fetch('/api/jobs');
      const data = await res.json();
      setJobs(data);
    };
    load();
  }, []);

  const handleSort = (e) => {
    const value = e.target.value;
    setSortBy(value);
    // BUG: sorts the jobs array in state — mutates original
    const sorted = [...jobs].sort((a, b) => {
      if (value === 'newest') return new Date(b.postedAt) - new Date(a.postedAt);
      if (value === 'oldest') return new Date(a.postedAt) - new Date(b.postedAt);
      if (value === 'salary') return b.salary - a.salary;
      return 0;
    });
    setJobs(sorted); // overwrites original data!
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
        <option value="salary">Salary High to Low</option>
      </select>
      {filteredJobs.map(job => (
        <div key={job.id}>
          <h3>{job.title}</h3>
          <p>{job.company} — ₹{job.salary}</p>
          <p>Posted: {job.postedAt}</p>
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the bug where sorting destroys the original data
- ✅ Make sort and search work together correctly
- ✅ Add a "jobs found" count

<details>
<summary>💡 Hint</summary>

The bug is `setJobs(sorted)` — this replaces the original
data with a sorted copy. Once you sort, the original order
is gone and search now only searches the sorted (possibly
filtered) set. Store `sortBy` in state, then compute
both filter and sort together without touching `jobs`.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect } from 'react';

function JobBoard() {
  const [jobs, setJobs] = useState([]);       // NEVER modified after load
  const [isLoading, setIsLoading] = useState(true);
  const [searchTerm, setSearchTerm] = useState('');
  const [sortBy, setSortBy] = useState('newest');

  useEffect(() => {
    const load = async () => {
      const res = await fetch('/api/jobs');
      const data = await res.json();
      setJobs(data);
      setIsLoading(false);
    };
    load();
  }, []);

  // Step 1: Filter
  // Step 2: Sort the filtered result
  // Both derived — never stored in state
  const processedJobs = jobs
    .filter(job =>
      job.company.toLowerCase().includes(searchTerm.toLowerCase()) ||
      job.title.toLowerCase().includes(searchTerm.toLowerCase())
    )
    .sort((a, b) => {
      if (sortBy === 'newest') return new Date(b.postedAt) - new Date(a.postedAt);
      if (sortBy === 'oldest') return new Date(a.postedAt) - new Date(b.postedAt);
      if (sortBy === 'salary') return b.salary - a.salary;
      return 0;
    });

  if (isLoading) return <p>Loading jobs...</p>;

  return (
    <div>
      <input
        value={searchTerm}
        onChange={e => setSearchTerm(e.target.value)}
        placeholder="Search by company or title..."
      />

      <select
        value={sortBy}
        onChange={e => setSortBy(e.target.value)}
        // FIX: just update sortBy state — no manual sorting here
      >
        <option value="newest">Newest First</option>
        <option value="oldest">Oldest First</option>
        <option value="salary">Salary High to Low</option>
      </select>

      <p>{processedJobs.length} jobs found</p>

      {processedJobs.length === 0 ? (
        <p>No jobs found. Try a different search.</p>
      ) : (
        processedJobs.map(job => (
          <div key={job.id}>
            <h3>{job.title}</h3>
            <p>{job.company} — ₹{job.salary.toLocaleString()}</p>
            <p>Posted: {new Date(job.postedAt).toLocaleDateString()}</p>
          </div>
        ))
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
Never mutate the source data array. Sort and filter are both
derived operations. They chain — filter first, then sort the
filtered result. The interviewer specifically looks for the
absence of `setJobs(sorted)`.

</details>

---

### 🔴 Pattern 3 · Scenario 4 — The Search That Breaks on Special Characters

🏢 **Situation:**
You are at a SaaS company. The customer directory has a search
feature. It works for most names. But when a user searches for
"O'Brien" or "John+Smith" or types "(admin)" the browser crashes
with an error: "Invalid regular expression". A customer complained
after they could not find their own account.

**Here is the current code:**

```jsx
import { useState } from 'react';

function CustomerSearch({ customers }) {
  const [searchTerm, setSearchTerm] = useState('');

  const filtered = customers.filter(customer => {
    const regex = new RegExp(searchTerm, 'i');
    return regex.test(customer.name) || regex.test(customer.email);
  });

  return (
    <div>
      <input
        value={searchTerm}
        onChange={e => setSearchTerm(e.target.value)}
        placeholder="Search customers..."
      />
      {filtered.map(customer => (
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
- ✅ Fix the crash caused by special characters in the search term
- ✅ Keep the case-insensitive search working
- ✅ Add debounce so it does not filter on every single keystroke
- ✅ Highlight the matched text in the results

<details>
<summary>💡 Hint</summary>

The crash happens because user input like `(` or `+` or `[`
are special regex characters. Using them directly in
`new RegExp(searchTerm)` throws. You have two options:
escape the special characters, or switch from regex to
simple `.includes()` which treats everything as a plain string.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect, useMemo } from 'react';

function CustomerSearch({ customers }) {
  const [inputValue, setInputValue] = useState('');
  const [searchTerm, setSearchTerm] = useState(''); // debounced value

  // Debounce — update searchTerm 300ms after user stops typing
  useEffect(() => {
    const timer = setTimeout(() => {
      setSearchTerm(inputValue);
    }, 300);
    return () => clearTimeout(timer);
  }, [inputValue]);

  // FIX: use .includes() instead of RegExp — safe with all characters
  const filtered = useMemo(() => {
    if (!searchTerm.trim()) return customers;

    const term = searchTerm.toLowerCase();
    return customers.filter(customer =>
      customer.name.toLowerCase().includes(term) ||
      customer.email.toLowerCase().includes(term)
    );
  }, [customers, searchTerm]);

  // Highlight matched text in a string
  const highlight = (text, term) => {
    if (!term.trim()) return text;
    const index = text.toLowerCase().indexOf(term.toLowerCase());
    if (index === -1) return text;

    return (
      <>
        {text.slice(0, index)}
        <mark>{text.slice(index, index + term.length)}</mark>
        {text.slice(index + term.length)}
      </>
    );
  };

  return (
    <div>
      <input
        value={inputValue}
        onChange={e => setInputValue(e.target.value)}
        placeholder="Search customers..."
      />

      <p>{filtered.length} customers found</p>

      {filtered.length === 0 ? (
        <p>No customers match "{searchTerm}"</p>
      ) : (
        filtered.map(customer => (
          <div key={customer.id}>
            <strong>{highlight(customer.name, searchTerm)}</strong>
            <span>{highlight(customer.email, searchTerm)}</span>
          </div>
        ))
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
They want to see that you know RegExp with user input is
dangerous and that `.includes()` is the safe alternative.
The debounce and highlight are bonus points that show
production-level thinking.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 4: 💾 localStorage Usage

> **Level 1 — Foundation**

🎯 **What you will build in this pattern:**
- A shopping cart that persists across page refreshes
- A settings/preferences page that remembers user choices
- A form that auto-saves drafts and restores them on reload

📋 **Interview asks this as:**
- "Make this cart persist when the user refreshes the page"
- "Save the user's theme preference so it loads correctly on next visit"
- "Store this form data so users do not lose their work"

🚨 **Most common mistake in interviews:**
Not wrapping `JSON.parse(localStorage.getItem())` in a try/catch.
If localStorage has corrupted data, this throws and crashes the app.
Always parse defensively.

---

### 🔴 Pattern 4 · Scenario 1 — The Cart That Empties on Refresh

🏢 **Situation:**
You are at an e-commerce startup. Users add items to their cart,
then refresh the page to check something — and their entire cart
is empty. They have to add everything again. You are losing sales.
Your PM wants this fixed today.

**Here is the current code:**

```jsx
import { useState } from 'react';

function ShoppingCart() {
  const [cartItems, setCartItems] = useState([]);

  const addItem = (product) => {
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

  const total = cartItems.reduce(
    (sum, item) => sum + item.price * item.quantity, 0
  );

  return (
    <div>
      <h2>Cart ({cartItems.length} items)</h2>
      {cartItems.map(item => (
        <div key={item.id}>
          <span>{item.name}</span>
          <span>x{item.quantity}</span>
          <span>₹{item.price * item.quantity}</span>
          <button onClick={() => removeItem(item.id)}>Remove</button>
        </div>
      ))}
      <p>Total: ₹{total}</p>
    </div>
  );
}
```

**Your tasks:**
- ✅ Save cartItems to localStorage whenever it changes
- ✅ Load cartItems from localStorage when the component mounts
- ✅ Handle corrupted localStorage data safely (try/catch)
- ✅ Clear localStorage when the cart is emptied

<details>
<summary>💡 Hint</summary>

You need two useEffects — one runs on mount to read from
localStorage (with `[]` dependency), and one runs whenever
`cartItems` changes to write to localStorage.
Initialize state with a function that reads localStorage —
this is called lazy initialization.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect } from 'react';

const CART_KEY = 'shopping-cart';

function ShoppingCart() {
  // Lazy initialization — reads localStorage ONCE on first render
  const [cartItems, setCartItems] = useState(() => {
    try {
      const saved = localStorage.getItem(CART_KEY);
      return saved ? JSON.parse(saved) : [];
    } catch (err) {
      // Corrupted data — start fresh
      localStorage.removeItem(CART_KEY);
      return [];
    }
  });

  // Save to localStorage whenever cart changes
  useEffect(() => {
    if (cartItems.length === 0) {
      localStorage.removeItem(CART_KEY); // clean up when empty
    } else {
      localStorage.setItem(CART_KEY, JSON.stringify(cartItems));
    }
  }, [cartItems]);

  const addItem = (product) => {
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

  const clearCart = () => {
    setCartItems([]);
    // useEffect above will call localStorage.removeItem
  };

  const total = cartItems.reduce(
    (sum, item) => sum + item.price * item.quantity, 0
  );

  return (
    <div>
      <h2>Cart ({cartItems.length} items)</h2>
      {cartItems.length === 0 ? (
        <p>Your cart is empty.</p>
      ) : (
        <>
          {cartItems.map(item => (
            <div key={item.id}>
              <span>{item.name}</span>
              <span>x{item.quantity}</span>
              <span>₹{item.price * item.quantity}</span>
              <button onClick={() => removeItem(item.id)}>Remove</button>
            </div>
          ))}
          <p>Total: ₹{total.toFixed(2)}</p>
          <button onClick={clearCart}>Clear Cart</button>
        </>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
They look for lazy initialization of `useState` with a function —
this is the correct pattern for reading localStorage on startup.
They also check for try/catch around `JSON.parse` and cleanup
of localStorage when the cart empties.

</details>

---

### 🔴 Pattern 4 · Scenario 2 — The Settings That Reset Every Visit

🏢 **Situation:**
Your SaaS dashboard lets users configure their preferences —
items per page (10/25/50), default date range, and notification
settings. But every time they visit, everything resets to defaults.
Users are angry. They set their preferences every single session.

**Here is the current code:**

```jsx
import { useState } from 'react';

const DEFAULT_SETTINGS = {
  itemsPerPage: 10,
  dateRange: '7days',
  emailNotifications: true,
  language: 'en',
};

function UserSettings() {
  const [settings, setSettings] = useState(DEFAULT_SETTINGS);
  const [saved, setSaved] = useState(false);

  const handleChange = (key, value) => {
    setSettings(prev => ({ ...prev, [key]: value }));
    setSaved(false);
  };

  const handleSave = () => {
    // Saves to API but not localStorage
    fetch('/api/user/settings', {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(settings),
    });
    setSaved(true);
  };

  return (
    <div>
      <h2>Settings</h2>
      <label>
        Items per page:
        <select value={settings.itemsPerPage}
          onChange={e => handleChange('itemsPerPage', Number(e.target.value))}>
          <option value={10}>10</option>
          <option value={25}>25</option>
          <option value={50}>50</option>
        </select>
      </label>
      <label>
        Language:
        <select value={settings.language}
          onChange={e => handleChange('language', e.target.value)}>
          <option value="en">English</option>
          <option value="hi">Hindi</option>
        </select>
      </label>
      <button onClick={handleSave}>Save Settings</button>
      {saved && <p>Settings saved!</p>}
    </div>
  );
}
```

**Your tasks:**
- ✅ Load settings from localStorage on mount (fall back to defaults)
- ✅ Save settings to localStorage when user clicks Save
- ✅ Also save to API (already done — keep it)
- ✅ Apply settings immediately — do not wait for page reload

<details>
<summary>💡 Hint</summary>

Initialize state with a function that merges localStorage
data with defaults using spread operator. This way if a
new setting is added later, it gets its default value
even if it is not in the user's saved data.
`{ ...DEFAULT_SETTINGS, ...savedSettings }`

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState } from 'react';

const SETTINGS_KEY = 'user-settings';

const DEFAULT_SETTINGS = {
  itemsPerPage: 10,
  dateRange: '7days',
  emailNotifications: true,
  language: 'en',
};

function UserSettings() {
  // Lazy init — merge saved settings with defaults
  const [settings, setSettings] = useState(() => {
    try {
      const saved = localStorage.getItem(SETTINGS_KEY);
      if (!saved) return DEFAULT_SETTINGS;
      // Merge with defaults — handles new settings added in future
      return { ...DEFAULT_SETTINGS, ...JSON.parse(saved) };
    } catch {
      return DEFAULT_SETTINGS;
    }
  });

  const [saveStatus, setSaveStatus] = useState('idle'); // idle | saving | saved | error

  const handleChange = (key, value) => {
    setSettings(prev => ({ ...prev, [key]: value }));
    setSaveStatus('idle'); // mark as unsaved when changed
  };

  const handleSave = async () => {
    try {
      setSaveStatus('saving');

      // Save to localStorage immediately
      localStorage.setItem(SETTINGS_KEY, JSON.stringify(settings));

      // Also sync to API
      const res = await fetch('/api/user/settings', {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(settings),
      });

      if (!res.ok) throw new Error('Failed to save to server');

      setSaveStatus('saved');

      // Reset status after 3 seconds
      setTimeout(() => setSaveStatus('idle'), 3000);

    } catch (err) {
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
          onChange={e => handleChange('itemsPerPage', Number(e.target.value))}
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
          onChange={e => handleChange('language', e.target.value)}
        >
          <option value="en">English</option>
          <option value="hi">Hindi</option>
        </select>
      </label>

      <label>
        Email Notifications:
        <input
          type="checkbox"
          checked={settings.emailNotifications}
          onChange={e => handleChange('emailNotifications', e.target.checked)}
        />
      </label>

      <button
        onClick={handleSave}
        disabled={saveStatus === 'saving'}
      >
        {saveStatus === 'saving' ? 'Saving...' : 'Save Settings'}
      </button>

      {saveStatus === 'saved' && <p style={{ color: 'green' }}>Settings saved!</p>}
      {saveStatus === 'error' && <p style={{ color: 'red' }}>Failed to save to server. Saved locally.</p>}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The merge pattern `{ ...DEFAULT_SETTINGS, ...JSON.parse(saved) }`
is what separates a thoughtful answer from a basic one. It ensures
settings always have valid values even when new keys are added.
They also look for the status state machine.

</details>

---

### 🔴 Pattern 4 · Scenario 3 — The Form Draft That Disappears

*(Covered in Pattern 2 Scenario 3 — The Form That Loses Your Data.
This scenario focuses on a different aspect: multiple drafts.)*

🏢 **Situation:**
You are building a blogging platform. Writers create new posts
using a rich text editor. The editor auto-saves drafts to
localStorage. But if a writer has two browser tabs open — one
for each draft — they keep overwriting each other. The wrong
draft data appears in the wrong post editor.

**Here is the current code:**

```jsx
import { useState, useEffect } from 'react';

function PostEditor({ postId }) {
  const [title, setTitle] = useState('');
  const [content, setContent] = useState('');

  // BUG: same key for all posts
  useEffect(() => {
    const saved = localStorage.getItem('post-draft');
    if (saved) {
      const { title, content } = JSON.parse(saved);
      setTitle(title);
      setContent(content);
    }
  }, []);

  useEffect(() => {
    localStorage.setItem('post-draft', JSON.stringify({ title, content }));
  }, [title, content]);

  const handlePublish = async () => {
    await fetch(`/api/posts/${postId}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ title, content, status: 'published' }),
    });
    localStorage.removeItem('post-draft');
  };

  return (
    <div>
      <input value={title} onChange={e => setTitle(e.target.value)}
        placeholder="Post title" />
      <textarea value={content} onChange={e => setContent(e.target.value)}
        placeholder="Write your post..." />
      <button onClick={handlePublish}>Publish</button>
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the key collision — each post must have its own draft key
- ✅ Restore the correct draft for the current postId on mount
- ✅ Clear only the current post's draft after publishing
- ✅ Add a "last saved" timestamp to the draft indicator

<details>
<summary>💡 Hint</summary>

The fix is simple but important: use `postId` in the localStorage
key. `post-draft-${postId}` means each post gets its own storage
slot. Make sure the useEffect that restores data depends on `postId`
so it runs again if the editor switches to a different post.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect } from 'react';

function PostEditor({ postId }) {
  // FIX: unique key per post
  const DRAFT_KEY = `post-draft-${postId}`;

  const [title, setTitle] = useState('');
  const [content, setContent] = useState('');
  const [lastSaved, setLastSaved] = useState(null);
  const [isPublishing, setIsPublishing] = useState(false);

  // Restore draft when postId changes
  useEffect(() => {
    // Reset form first (important when switching between posts)
    setTitle('');
    setContent('');
    setLastSaved(null);

    try {
      const saved = localStorage.getItem(DRAFT_KEY);
      if (saved) {
        const { title, content, savedAt } = JSON.parse(saved);
        setTitle(title || '');
        setContent(content || '');
        setLastSaved(savedAt || null);
      }
    } catch {
      localStorage.removeItem(DRAFT_KEY);
    }
  }, [DRAFT_KEY]); // re-run when postId changes

  // Auto-save draft with debounce
  useEffect(() => {
    const timer = setTimeout(() => {
      const draft = {
        title,
        content,
        savedAt: new Date().toLocaleTimeString(),
      };
      localStorage.setItem(DRAFT_KEY, JSON.stringify(draft));
      setLastSaved(draft.savedAt);
    }, 1000);

    return () => clearTimeout(timer);
  }, [title, content, DRAFT_KEY]);

  const handlePublish = async () => {
    try {
      setIsPublishing(true);
      const res = await fetch(`/api/posts/${postId}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ title, content, status: 'published' }),
      });

      if (!res.ok) throw new Error('Publish failed');

      // Clear only THIS post's draft
      localStorage.removeItem(DRAFT_KEY);
      setLastSaved(null);
      alert('Post published!');

    } catch (err) {
      alert('Publish failed. Draft is saved.');
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
        onChange={e => setTitle(e.target.value)}
        placeholder="Post title"
      />
      <textarea
        value={content}
        onChange={e => setContent(e.target.value)}
        placeholder="Write your post..."
      />
      <button onClick={handlePublish} disabled={isPublishing}>
        {isPublishing ? 'Publishing...' : 'Publish'}
      </button>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The `postId` in the storage key is the core fix. They also
look for the state reset at the start of the restore useEffect —
without it, old state from a previous post bleeds in before
the saved data loads.

</details>

---

### 🔴 Pattern 4 · Scenario 4 — The localStorage That Crashes the App

🏢 **Situation:**
You are on call at a banking app. An urgent bug report comes in:
"App completely blank for some users, white screen on load."
You trace it back to localStorage. The app crashes on startup
for certain users. No errors in your dev environment.
You need to find and fix this in production.

**Here is the current code:**

```jsx
import { useState } from 'react';

const PREFS_KEY = 'user-preferences';

function App() {
  // This crashes for some users
  const savedPrefs = JSON.parse(localStorage.getItem(PREFS_KEY));

  const [preferences, setPreferences] = useState(
    savedPrefs || { theme: 'light', currency: 'INR' }
  );

  const savePreference = (key, value) => {
    const updated = { ...preferences, [key]: value };
    setPreferences(updated);
    localStorage.setItem(PREFS_KEY, JSON.stringify(updated));
  };

  return (
    <div className={`app theme-${preferences.theme}`}>
      <button onClick={() =>
        savePreference('theme', preferences.theme === 'light' ? 'dark' : 'light')
      }>
        Toggle Theme
      </button>
    </div>
  );
}
```

**Your tasks:**
- ✅ Find all the ways this code can crash
- ✅ Add safe localStorage reading with fallback
- ✅ Handle the case where localStorage is full (quota exceeded)
- ✅ Handle the case where localStorage is disabled (private browsing)

<details>
<summary>💡 Hint</summary>

There are three ways this crashes:
1. `JSON.parse` on corrupted/null data throws
2. `localStorage.getItem` itself throws in private browsing mode
   on some browsers (Safari) or when storage is blocked
3. `localStorage.setItem` throws when storage is full (QuotaExceededError)
Each needs its own try/catch in a different place.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState } from 'react';

const PREFS_KEY = 'user-preferences';
const DEFAULT_PREFS = { theme: 'light', currency: 'INR' };

// Safe localStorage utilities — use these everywhere
const storage = {
  get: (key, fallback = null) => {
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : fallback;
    } catch (err) {
      // Catches: JSON.parse errors, localStorage disabled errors
      console.warn(`localStorage.get failed for key "${key}":`, err);
      return fallback;
    }
  },

  set: (key, value) => {
    try {
      localStorage.setItem(key, JSON.stringify(value));
      return true;
    } catch (err) {
      // Catches: QuotaExceededError (storage full)
      // Catches: SecurityError (private browsing, storage blocked)
      console.warn(`localStorage.set failed for key "${key}":`, err);
      return false;
    }
  },

  remove: (key) => {
    try {
      localStorage.removeItem(key);
    } catch (err) {
      console.warn(`localStorage.remove failed for key "${key}":`, err);
    }
  },
};

function App() {
  // FIX: use safe getter with default fallback
  const [preferences, setPreferences] = useState(() => {
    const saved = storage.get(PREFS_KEY, DEFAULT_PREFS);
    // Merge with defaults to handle missing keys
    return { ...DEFAULT_PREFS, ...saved };
  });

  const savePreference = (key, value) => {
    const updated = { ...preferences, [key]: value };
    setPreferences(updated);

    // FIX: safe setter — will not crash if storage is full
    const saved = storage.set(PREFS_KEY, updated);
    if (!saved) {
      console.warn('Could not save preferences — storage may be full or disabled');
      // App still works — just does not persist
    }
  };

  return (
    <div className={`app theme-${preferences.theme}`}>
      <button onClick={() =>
        savePreference('theme', preferences.theme === 'light' ? 'dark' : 'light')
      }>
        Toggle Theme ({preferences.theme})
      </button>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
They want to see that you know localStorage can throw in THREE
different ways: parsing error, access error (private browsing),
and quota error (storage full). Wrapping operations in a utility
object shows production-level defensive coding.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 5: 🔀 Show/Hide UI

> **Level 1 — Foundation**

🎯 **What you will build in this pattern:**
- A modal/dialog that opens and closes correctly
- An accordion where only one section is open at a time
- A dropdown that closes when you click outside it

📋 **Interview asks this as:**
- "Build a modal component that opens when a button is clicked"
- "Create an accordion where only one item is open at a time"
- "Build a dropdown menu that closes when clicking outside"

🚨 **Most common mistake in interviews:**
Not closing the modal/dropdown when clicking outside it or
pressing Escape. Real users expect this behavior and interviewers
specifically look for it.

---

### 🔴 Pattern 5 · Scenario 1 — The Modal That Never Closes

🏢 **Situation:**
You are at a recruitment platform. There is a modal that shows
candidate details when you click a row in the table. The modal
opens fine. But the only way to close it is clicking the X button.
Users expect to close it by clicking outside or pressing Escape.
Your UX designer filed a bug report.

**Here is the current code:**

```jsx
import { useState } from 'react';

function CandidateTable({ candidates }) {
  const [selectedCandidate, setSelectedCandidate] = useState(null);

  return (
    <div>
      <table>
        <tbody>
          {candidates.map(candidate => (
            <tr key={candidate.id}
              onClick={() => setSelectedCandidate(candidate)}
              style={{ cursor: 'pointer' }}>
              <td>{candidate.name}</td>
              <td>{candidate.role}</td>
            </tr>
          ))}
        </tbody>
      </table>

      {selectedCandidate && (
        <div style={{
          position: 'fixed', inset: 0,
          background: 'rgba(0,0,0,0.5)',
          display: 'flex', alignItems: 'center', justifyContent: 'center'
        }}>
          <div style={{ background: 'white', padding: 24, borderRadius: 8 }}>
            <button onClick={() => setSelectedCandidate(null)}>✕</button>
            <h2>{selectedCandidate.name}</h2>
            <p>{selectedCandidate.email}</p>
            <p>{selectedCandidate.experience} years experience</p>
          </div>
        </div>
      )}
    </div>
  );
}
```

**Your tasks:**
- ✅ Close the modal when clicking the dark overlay (outside the modal)
- ✅ Close the modal when pressing the Escape key
- ✅ Prevent the page from scrolling when the modal is open
- ✅ Make sure clicking inside the modal does NOT close it

<details>
<summary>💡 Hint</summary>

For outside click: the overlay and modal share the same click event.
You need `e.stopPropagation()` on the inner modal div to prevent
clicks inside from bubbling to the overlay.
For Escape key: add a `keydown` event listener in useEffect —
remember to clean it up on unmount.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect } from 'react';

function CandidateTable({ candidates }) {
  const [selectedCandidate, setSelectedCandidate] = useState(null);

  const closeModal = () => setSelectedCandidate(null);

  // Close on Escape key + prevent body scroll
  useEffect(() => {
    if (!selectedCandidate) return;

    // Prevent background scroll when modal is open
    document.body.style.overflow = 'hidden';

    const handleKeyDown = (e) => {
      if (e.key === 'Escape') closeModal();
    };

    document.addEventListener('keydown', handleKeyDown);

    // Cleanup: restore scroll and remove listener
    return () => {
      document.body.style.overflow = '';
      document.removeEventListener('keydown', handleKeyDown);
    };
  }, [selectedCandidate]);

  return (
    <div>
      <table>
        <tbody>
          {candidates.map(candidate => (
            <tr
              key={candidate.id}
              onClick={() => setSelectedCandidate(candidate)}
              style={{ cursor: 'pointer' }}
            >
              <td>{candidate.name}</td>
              <td>{candidate.role}</td>
            </tr>
          ))}
        </tbody>
      </table>

      {selectedCandidate && (
        // Overlay — clicking here closes modal
        <div
          onClick={closeModal}
          style={{
            position: 'fixed', inset: 0,
            background: 'rgba(0,0,0,0.5)',
            display: 'flex', alignItems: 'center', justifyContent: 'center'
          }}
        >
          {/* Modal content — stopPropagation prevents closing when clicking inside */}
          <div
            onClick={e => e.stopPropagation()}
            style={{ background: 'white', padding: 24, borderRadius: 8, minWidth: 400 }}
          >
            <button onClick={closeModal}>✕ Close</button>
            <h2>{selectedCandidate.name}</h2>
            <p>Email: {selectedCandidate.email}</p>
            <p>Experience: {selectedCandidate.experience} years</p>
            <p>Role: {selectedCandidate.role}</p>
          </div>
        </div>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`e.stopPropagation()` on the inner div is the key answer —
without it, clicking inside the modal closes it. They also
look for the Escape key handler with proper cleanup and
`document.body.style.overflow = 'hidden'` for scroll prevention.

</details>

---

### 🔴 Pattern 5 · Scenario 2 — The Accordion That Opens Everything

🏢 **Situation:**
You are building an FAQ section for a healthcare app. There
is an accordion with 10 questions. The requirement says only
one question should be open at a time — opening a new question
should close the previous one. But right now all questions
can be open at the same time.

**Here is the current code:**

```jsx
import { useState } from 'react';

function FAQ({ faqs }) {
  // Each item tracks its own open state
  const [openItems, setOpenItems] = useState({});

  const toggle = (id) => {
    setOpenItems(prev => ({
      ...prev,
      [id]: !prev[id],
    }));
  };

  return (
    <div>
      {faqs.map(faq => (
        <div key={faq.id}>
          <button onClick={() => toggle(faq.id)}>
            {faq.question}
            <span>{openItems[faq.id] ? '▲' : '▼'}</span>
          </button>
          {openItems[faq.id] && (
            <div>{faq.answer}</div>
          )}
        </div>
      ))}
    </div>
  );
}
```

**Your tasks:**
- ✅ Change it so only one FAQ can be open at a time
- ✅ Clicking an open item closes it (toggle behavior)
- ✅ Keep track of which item is open with a single state value
- ✅ Add smooth open/close animation

<details>
<summary>💡 Hint</summary>

Instead of an object with multiple booleans, you only need
ONE state value — the ID of the currently open item
(or null if none is open). When you click an item:
if it is already open, set state to null. If it is closed,
set state to its ID. This automatically closes all others.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState } from 'react';

function FAQ({ faqs }) {
  // ONE state — ID of the open item, or null if all closed
  const [openId, setOpenId] = useState(null);

  const toggle = (id) => {
    // If clicking the open item → close it (set to null)
    // If clicking a closed item → open it (and close the previous)
    setOpenId(prev => prev === id ? null : id);
  };

  return (
    <div>
      {faqs.map(faq => {
        const isOpen = openId === faq.id;

        return (
          <div key={faq.id} style={{ borderBottom: '1px solid #eee' }}>
            <button
              onClick={() => toggle(faq.id)}
              style={{
                width: '100%',
                padding: '16px',
                display: 'flex',
                justifyContent: 'space-between',
                background: 'none',
                border: 'none',
                cursor: 'pointer',
                fontWeight: isOpen ? 'bold' : 'normal',
              }}
              aria-expanded={isOpen}
            >
              {faq.question}
              {/* Arrow rotates based on open state */}
              <span style={{
                transform: isOpen ? 'rotate(180deg)' : 'rotate(0deg)',
                transition: 'transform 0.2s ease',
                display: 'inline-block',
              }}>
                ▼
              </span>
            </button>

            {/* Answer — only renders when open */}
            {isOpen && (
              <div style={{
                padding: '0 16px 16px',
                color: '#555',
              }}>
                {faq.answer}
              </div>
            )}
          </div>
        );
      })}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
Single `openId` state instead of an object of booleans —
this is the cleaner, correct pattern for exclusive selection.
The toggle logic `prev === id ? null : id` is the key line
they want to see.

</details>

---

### 🔴 Pattern 5 · Scenario 3 — The Dropdown That Stays Open

🏢 **Situation:**
You are building a user menu dropdown (avatar in top-right corner).
It opens when clicked. But it never closes unless you click the
button again. Users are frustrated — they expect it to close when
they click anywhere else on the page, the same as every other
dropdown on the internet.

**Here is the current code:**

```jsx
import { useState } from 'react';

function UserMenu({ user }) {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div style={{ position: 'relative' }}>
      <button onClick={() => setIsOpen(!isOpen)}>
        <img src={user.avatar} alt={user.name} style={{ borderRadius: '50%', width: 40 }} />
      </button>

      {isOpen && (
        <div style={{
          position: 'absolute', right: 0, top: '100%',
          background: 'white', border: '1px solid #ccc',
          borderRadius: 8, minWidth: 200,
        }}>
          <p>{user.name}</p>
          <p>{user.email}</p>
          <hr />
          <button>Profile</button>
          <button>Settings</button>
          <button>Logout</button>
        </div>
      )}
    </div>
  );
}
```

**Your tasks:**
- ✅ Close the dropdown when clicking anywhere outside it
- ✅ Close the dropdown when pressing Escape
- ✅ Use useRef to detect outside clicks correctly
- ✅ Keep it open when clicking inside the dropdown

<details>
<summary>💡 Hint</summary>

Use a `ref` on the container div. In a useEffect, add a
`mousedown` listener to `document`. When a click happens,
check if `containerRef.current.contains(e.target)` — if the
click was outside the container, close the dropdown.
This pattern works for any "close on outside click" scenario.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect, useRef } from 'react';

function UserMenu({ user }) {
  const [isOpen, setIsOpen] = useState(false);
  const containerRef = useRef(null); // ref to the entire component

  // Close on outside click and Escape key
  useEffect(() => {
    if (!isOpen) return; // only add listeners when open

    const handleOutsideClick = (e) => {
      // If click is outside the container div — close
      if (containerRef.current && !containerRef.current.contains(e.target)) {
        setIsOpen(false);
      }
    };

    const handleEscape = (e) => {
      if (e.key === 'Escape') setIsOpen(false);
    };

    // mousedown fires before click — prevents race conditions
    document.addEventListener('mousedown', handleOutsideClick);
    document.addEventListener('keydown', handleEscape);

    return () => {
      document.removeEventListener('mousedown', handleOutsideClick);
      document.removeEventListener('keydown', handleEscape);
    };
  }, [isOpen]);

  return (
    // ref attached to container
    <div ref={containerRef} style={{ position: 'relative' }}>
      <button onClick={() => setIsOpen(prev => !prev)}>
        <img
          src={user.avatar}
          alt={user.name}
          style={{ borderRadius: '50%', width: 40, cursor: 'pointer' }}
        />
      </button>

      {isOpen && (
        <div style={{
          position: 'absolute', right: 0, top: '100%',
          background: 'white', border: '1px solid #ccc',
          borderRadius: 8, minWidth: 200, padding: 8,
          boxShadow: '0 4px 12px rgba(0,0,0,0.1)',
          zIndex: 1000,
        }}>
          <div style={{ padding: '8px 12px', borderBottom: '1px solid #eee' }}>
            <p style={{ margin: 0, fontWeight: 'bold' }}>{user.name}</p>
            <p style={{ margin: 0, color: '#666', fontSize: 14 }}>{user.email}</p>
          </div>
          <button style={{ display: 'block', width: '100%', padding: '8px 12px' }}>
            Profile
          </button>
          <button style={{ display: 'block', width: '100%', padding: '8px 12px' }}>
            Settings
          </button>
          <button style={{ display: 'block', width: '100%', padding: '8px 12px', color: 'red' }}>
            Logout
          </button>
        </div>
      )}
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The `containerRef.contains(e.target)` pattern — this is the
standard way to detect outside clicks and they specifically
look for it. They also check that you only add listeners
when the dropdown is open and clean them up properly.

</details>

---

### 🔴 Pattern 5 · Scenario 4 — The Tabs That Lose Their State

🏢 **Situation:**
You are building a user profile page with three tabs:
Overview, Posts, and Settings. The Settings tab has a form
that users are filling out. When they switch to Posts and
back to Settings — all their unsaved form data is gone.
Users are angry. Fix it.

**Here is the current code:**

```jsx
import { useState } from 'react';

function ProfileTabs({ user }) {
  const [activeTab, setActiveTab] = useState('overview');

  const tabs = ['overview', 'posts', 'settings'];

  const renderTab = () => {
    if (activeTab === 'overview') return <OverviewTab user={user} />;
    if (activeTab === 'posts') return <PostsTab userId={user.id} />;
    if (activeTab === 'settings') return <SettingsTab user={user} />;
  };

  return (
    <div>
      <div>
        {tabs.map(tab => (
          <button
            key={tab}
            onClick={() => setActiveTab(tab)}
            style={{ fontWeight: activeTab === tab ? 'bold' : 'normal' }}
          >
            {tab}
          </button>
        ))}
      </div>
      <div>{renderTab()}</div>
    </div>
  );
}

function SettingsTab({ user }) {
  const [name, setName] = useState(user.name);
  const [bio, setBio] = useState(user.bio);

  return (
    <form>
      <input value={name} onChange={e => setName(e.target.value)} />
      <textarea value={bio} onChange={e => setBio(e.target.value)} />
      <button type="submit">Save</button>
    </form>
  );
}
```

**Your tasks:**
- ✅ Fix state loss when switching tabs
- ✅ Keep all tab components mounted (just hide/show with CSS)
- ✅ Show an "unsaved changes" warning dot on the Settings tab
- ✅ Make the active tab visually distinct

<details>
<summary>💡 Hint</summary>

The problem is that `renderTab()` only mounts the active tab —
when you switch away, the component unmounts and its state is
destroyed. The fix is to render ALL tabs but use CSS to
`display: none` the inactive ones. This keeps them mounted
and their state intact.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState } from 'react';

function ProfileTabs({ user }) {
  const [activeTab, setActiveTab] = useState('overview');
  const [hasUnsavedSettings, setHasUnsavedSettings] = useState(false);

  const tabs = [
    { id: 'overview', label: 'Overview' },
    { id: 'posts', label: 'Posts' },
    {
      id: 'settings',
      label: 'Settings',
      badge: hasUnsavedSettings, // show dot when unsaved changes exist
    },
  ];

  return (
    <div>
      {/* Tab buttons */}
      <div style={{ display: 'flex', borderBottom: '2px solid #eee' }}>
        {tabs.map(tab => (
          <button
            key={tab.id}
            onClick={() => setActiveTab(tab.id)}
            style={{
              padding: '12px 20px',
              border: 'none',
              background: 'none',
              cursor: 'pointer',
              borderBottom: activeTab === tab.id ? '2px solid blue' : 'none',
              fontWeight: activeTab === tab.id ? 'bold' : 'normal',
              position: 'relative',
            }}
          >
            {tab.label}
            {/* Unsaved changes indicator */}
            {tab.badge && (
              <span style={{
                position: 'absolute', top: 8, right: 8,
                width: 8, height: 8, borderRadius: '50%',
                background: 'orange',
              }} />
            )}
          </button>
        ))}
      </div>

      {/* FIX: render ALL tabs, hide with CSS — state is preserved */}
      <div style={{ display: activeTab === 'overview' ? 'block' : 'none' }}>
        <OverviewTab user={user} />
      </div>

      <div style={{ display: activeTab === 'posts' ? 'block' : 'none' }}>
        <PostsTab userId={user.id} />
      </div>

      <div style={{ display: activeTab === 'settings' ? 'block' : 'none' }}>
        <SettingsTab
          user={user}
          onUnsavedChange={setHasUnsavedSettings}
        />
      </div>
    </div>
  );
}

function SettingsTab({ user, onUnsavedChange }) {
  const [name, setName] = useState(user.name);
  const [bio, setBio] = useState(user.bio);

  const hasChanges = name !== user.name || bio !== user.bio;

  // Notify parent about unsaved changes
  useEffect(() => {
    onUnsavedChange(hasChanges);
  }, [hasChanges, onUnsavedChange]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    await fetch(`/api/users/${user.id}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ name, bio }),
    });
    onUnsavedChange(false);
  };

  return (
    <form onSubmit={handleSubmit}>
      {hasChanges && <p style={{ color: 'orange' }}>You have unsaved changes</p>}
      <input value={name} onChange={e => setName(e.target.value)} />
      <textarea value={bio} onChange={e => setBio(e.target.value)} />
      <button type="submit">Save Changes</button>
    </form>
  );
}
```

**✅ What the interviewer is checking:**
The `display: none` pattern for preserving tab state —
this shows you understand when to keep components mounted
vs unmount them. The unsaved changes badge is a bonus that
shows product thinking.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Pattern 6: 🧭 Page Navigation

> **Level 1 — Foundation**

🎯 **What you will build in this pattern:**
- A protected route that redirects unauthenticated users to login
- A login page that redirects to dashboard after successful auth
- A navigation guard that prevents users from leaving a page with unsaved changes

📋 **Interview asks this as:**
- "Protect this route so only logged-in users can see it"
- "After login, redirect the user to the page they were trying to visit"
- "Build a protected route component using React Router"

🚨 **Most common mistake in interviews:**
Checking auth after rendering the protected content.
Always check auth BEFORE rendering — use early return or redirect.
Never flash the protected content for a fraction of a second.

---

### 🔴 Pattern 6 · Scenario 1 — The Protected Page Anyone Can Visit

🏢 **Situation:**
You are at a SaaS company. The admin dashboard at `/admin`
should only be visible to logged-in users. But you just
discovered that if you go directly to `/admin` in the browser
without logging in, you can see the entire dashboard.
Your security team is alarmed.

**Here is the current code:**

```jsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/login" element={<Login />} />
        <Route path="/admin" element={<AdminDashboard />} />
      </Routes>
    </BrowserRouter>
  );
}

function AdminDashboard() {
  const token = localStorage.getItem('token');

  // Shows dashboard briefly, then maybe redirects?
  if (!token) {
    alert('Please login first');
  }

  return (
    <div>
      <h1>Admin Dashboard</h1>
      <p>Secret admin data here</p>
    </div>
  );
}
```

**Your tasks:**
- ✅ Build a `ProtectedRoute` component that checks authentication
- ✅ Redirect unauthenticated users to `/login`
- ✅ Never show the protected content to unauthenticated users
- ✅ After login, redirect back to the page they tried to visit

<details>
<summary>💡 Hint</summary>

Build a wrapper component called `ProtectedRoute`.
It checks if the user is logged in. If yes, render `<Outlet />`.
If no, render `<Navigate to="/login" />` — this redirects
immediately, the protected content never renders.
Use `replace` so the back button does not loop.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import {
  BrowserRouter, Routes, Route,
  Navigate, Outlet, useNavigate, useLocation
} from 'react-router-dom';

// Auth helper — checks if user is logged in
const isAuthenticated = () => {
  const token = localStorage.getItem('token');
  return !!token; // true if token exists
};

// Protected Route component
function ProtectedRoute() {
  const location = useLocation();

  if (!isAuthenticated()) {
    // Redirect to login, but remember where they were going
    // state.from lets login redirect back after success
    return (
      <Navigate
        to="/login"
        state={{ from: location.pathname }}
        replace // replace history — back button will not loop
      />
    );
  }

  // Authenticated — render the child route
  return <Outlet />;
}

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/login" element={<Login />} />

        {/* Wrap protected routes */}
        <Route element={<ProtectedRoute />}>
          <Route path="/admin" element={<AdminDashboard />} />
          <Route path="/dashboard" element={<Dashboard />} />
          <Route path="/settings" element={<Settings />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

function Login() {
  const navigate = useNavigate();
  const location = useLocation();

  // Where did the user come from? Default to /dashboard
  const from = location.state?.from || '/dashboard';

  const handleLogin = async (e) => {
    e.preventDefault();
    const res = await fetch('/api/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email: 'user@example.com', password: 'pass' }),
    });
    const data = await res.json();
    if (data.token) {
      localStorage.setItem('token', data.token);
      // Redirect back to where they came from
      navigate(from, { replace: true });
    }
  };

  return (
    <form onSubmit={handleLogin}>
      <h1>Login</h1>
      <button type="submit">Login</button>
    </form>
  );
}

function AdminDashboard() {
  return (
    <div>
      <h1>Admin Dashboard</h1>
      <p>Only visible to authenticated users</p>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`<Navigate replace />` before the content renders — the content
must never flash. They also look for `state={{ from: location.pathname }}`
to redirect back after login, and `replace` on the navigate
so the back button does not create a redirect loop.

</details>

---

### 🔴 Pattern 6 · Scenario 2 — The Login That Does Not Redirect

🏢 **Situation:**
Users at your food delivery app log in successfully — they see
the success message "Login successful!" — but nothing happens.
They sit on the login page. They have to manually navigate to
the homepage. Your PM says this is making users think the login
broke. Fix the redirect.

**Here is the current code:**

```jsx
import { useState } from 'react';

function LoginPage() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [message, setMessage] = useState('');

  const handleSubmit = async (e) => {
    e.preventDefault();

    const res = await fetch('/api/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password }),
    });

    const data = await res.json();

    if (res.ok && data.token) {
      localStorage.setItem('token', data.token);
      localStorage.setItem('user', JSON.stringify(data.user));
      setMessage('Login successful!');
      // Supposed to redirect but does not
    } else {
      setMessage(data.message || 'Login failed');
    }
  };

  return (
    <div>
      <h1>Login</h1>
      {message && <p>{message}</p>}
      <form onSubmit={handleSubmit}>
        <input type="email" value={email}
          onChange={e => setEmail(e.target.value)} placeholder="Email" />
        <input type="password" value={password}
          onChange={e => setPassword(e.target.value)} placeholder="Password" />
        <button type="submit">Login</button>
      </form>
    </div>
  );
}
```

**Your tasks:**
- ✅ Redirect to `/dashboard` after successful login
- ✅ Use React Router's `useNavigate` (not `window.location.href`)
- ✅ Add loading state during the API call
- ✅ If user is already logged in and visits `/login`, redirect to `/dashboard`

<details>
<summary>💡 Hint</summary>

Use `useNavigate` from React Router — call `navigate('/dashboard')`
after storing the token. For the "already logged in" check,
use `useEffect` — if a token exists when the component mounts,
redirect immediately with `navigate('/dashboard', { replace: true })`.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useState, useEffect } from 'react';
import { useNavigate, useLocation } from 'react-router-dom';

function LoginPage() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');
  const [isLoading, setIsLoading] = useState(false);

  const navigate = useNavigate();
  const location = useLocation();
  const from = location.state?.from || '/dashboard';

  // If already logged in, redirect immediately
  useEffect(() => {
    const token = localStorage.getItem('token');
    if (token) {
      navigate('/dashboard', { replace: true });
    }
  }, [navigate]);

  const handleSubmit = async (e) => {
    e.preventDefault();

    if (!email || !password) {
      setError('Please fill in all fields');
      return;
    }

    try {
      setIsLoading(true);
      setError('');

      const res = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email, password }),
      });

      const data = await res.json();

      if (!res.ok) {
        throw new Error(data.message || 'Invalid credentials');
      }

      // Store auth data
      localStorage.setItem('token', data.token);
      localStorage.setItem('user', JSON.stringify(data.user));

      // FIX: redirect using navigate, not window.location.href
      // navigate keeps the SPA alive — window.location reloads the page
      navigate(from, { replace: true });

    } catch (err) {
      setError(err.message);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div>
      <h1>Login</h1>
      {error && <p style={{ color: 'red' }}>{error}</p>}
      <form onSubmit={handleSubmit}>
        <input
          type="email"
          value={email}
          onChange={e => setEmail(e.target.value)}
          placeholder="Email"
          disabled={isLoading}
        />
        <input
          type="password"
          value={password}
          onChange={e => setPassword(e.target.value)}
          placeholder="Password"
          disabled={isLoading}
        />
        <button type="submit" disabled={isLoading}>
          {isLoading ? 'Logging in...' : 'Login'}
        </button>
      </form>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
`useNavigate` vs `window.location.href` — they specifically
ask why you use navigate. Answer: `window.location.href`
reloads the entire page and destroys React state. `navigate`
updates the URL client-side and keeps the app running.

</details>

---

### 🔴 Pattern 6 · Scenario 3 — The Back Button That Breaks Auth

🏢 **Situation:**
At your SaaS platform, users click Logout. They are redirected
to `/login`. They press the browser back button. They land back
on the dashboard and can see protected data — even though they
just logged out. Your security team calls this a session fixation
issue and wants it fixed immediately.

**Here is the current code:**

```jsx
import { useNavigate } from 'react-router-dom';

function Header({ user }) {
  const navigate = useNavigate();

  const handleLogout = () => {
    localStorage.removeItem('token');
    localStorage.removeItem('user');
    navigate('/login'); // user can press back and return to dashboard
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
- ✅ Fix logout so pressing back does not return to the dashboard
- ✅ Clear all auth data on logout
- ✅ Call the logout API to invalidate the server session
- ✅ Use `replace` so the back button cannot return to protected pages

<details>
<summary>💡 Hint</summary>

The fix is `navigate('/login', { replace: true })`.
`replace` removes the current history entry instead of adding
a new one. So when the user presses back, there is no dashboard
entry to go back to. Also consider clearing any cached data.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useNavigate } from 'react-router-dom';

function Header({ user, onLogout }) {
  const navigate = useNavigate();

  const handleLogout = async () => {
    try {
      // 1. Tell the server to invalidate the session/token
      await fetch('/api/auth/logout', {
        method: 'POST',
        headers: {
          Authorization: `Bearer ${localStorage.getItem('token')}`,
        },
      });
    } catch (err) {
      // Even if API fails, proceed with client-side logout
      console.warn('Server logout failed:', err);
    }

    // 2. Clear ALL auth data from client
    localStorage.removeItem('token');
    localStorage.removeItem('user');
    localStorage.removeItem('shopping-cart'); // clear sensitive data too
    sessionStorage.clear(); // clear session storage too

    // 3. Notify parent component if needed
    if (onLogout) onLogout();

    // 4. FIX: use replace — removes dashboard from history
    // Back button now has nothing to go back to
    navigate('/login', { replace: true });
  };

  return (
    <header>
      <span>Hello, {user.name}</span>
      <button onClick={handleLogout}>Logout</button>
    </header>
  );
}
```

**✅ What the interviewer is checking:**
`navigate('/login', { replace: true })` is the exact answer.
They want you to explain what `replace` does: it replaces the
current history entry, so the back button skips over it.
They also look for the API logout call to invalidate
the server-side session.

</details>

---

### 🔴 Pattern 6 · Scenario 4 — The Page That Flashes Before Redirecting

🏢 **Situation:**
On your banking app, logged-in users visiting `/login` briefly
see the login form for a split second before being redirected
to `/dashboard`. Users are reporting this flash as "the app
looks broken". It happens because auth state loads asynchronously
from an API check on startup.

**Here is the current code:**

```jsx
import { useEffect, useState } from 'react';
import { Navigate } from 'react-router-dom';

function LoginPage() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Check if user is already logged in via API
    const checkAuth = async () => {
      const token = localStorage.getItem('token');
      if (token) {
        const res = await fetch('/api/auth/me', {
          headers: { Authorization: `Bearer ${token}` },
        });
        if (res.ok) {
          const data = await res.json();
          setUser(data.user);
        }
      }
    };
    checkAuth();
  }, []);

  // Problem: user starts as null, so login form renders first
  // Then user gets set and Navigate renders — causing the flash
  if (user) {
    return <Navigate to="/dashboard" replace />;
  }

  return (
    <div>
      <h1>Login</h1>
      <form>...</form>
    </div>
  );
}
```

**Your tasks:**
- ✅ Fix the flash — never show login form to authenticated users
- ✅ Add an `isChecking` state that shows a loading screen during auth check
- ✅ Only show the login form once auth check is complete and user is not logged in

<details>
<summary>💡 Hint</summary>

Add a third state: `isChecking` — starts as `true`, becomes
`false` after the auth check completes. While `isChecking` is true,
show nothing (or a loading spinner). This prevents the login form
from rendering before you know if the user is logged in.

</details>

<details>
<summary>✅ Full Solution</summary>

```jsx
import { useEffect, useState } from 'react';
import { Navigate } from 'react-router-dom';

function LoginPage() {
  const [user, setUser] = useState(null);
  const [isChecking, setIsChecking] = useState(true); // key: starts as true

  useEffect(() => {
    const checkAuth = async () => {
      const token = localStorage.getItem('token');

      if (!token) {
        // No token — definitely not logged in
        setIsChecking(false);
        return;
      }

      try {
        const res = await fetch('/api/auth/me', {
          headers: { Authorization: `Bearer ${token}` },
        });

        if (res.ok) {
          const data = await res.json();
          setUser(data.user);
        } else {
          // Token invalid — clear it
          localStorage.removeItem('token');
        }
      } catch (err) {
        // Network error — assume not logged in
        console.warn('Auth check failed:', err);
      } finally {
        setIsChecking(false); // always stop checking
      }
    };

    checkAuth();
  }, []);

  // While checking — show nothing (prevents flash)
  if (isChecking) {
    return <div>Loading...</div>; // or null for invisible check
  }

  // Check done — user is logged in, redirect
  if (user) {
    return <Navigate to="/dashboard" replace />;
  }

  // Check done — user is NOT logged in, show login form
  return (
    <div>
      <h1>Login</h1>
      <form>
        {/* login form */}
      </form>
    </div>
  );
}
```

**✅ What the interviewer is checking:**
The `isChecking` state is the exact answer to this problem.
They want to see three states: checking (show nothing),
authenticated (redirect), and unauthenticated (show form).
Most candidates only handle two states and miss the flash.

</details>

[↑ Back to Table of Contents](#table-of-contents)

---

# Interview Pattern Picker

> **"Interviewer asks [X]" → "Go to Pattern [N]"**

| Interviewer Says | Go To |
|---|---|
| "Fetch data from an API and display it" | Pattern 1 — Fetch & Display |
| "Show loading and error states" | Pattern 1 — Scenario 1 |
| "The data is not showing even though API returns 200" | Pattern 1 — Scenario 2 |
| "Handle a 404 when a user doesn't exist" | Pattern 1 — Scenario 3 |
| "Auto-refresh data every 30 seconds" | Pattern 1 — Scenario 4 |
| "Build a login form with validation" | Pattern 2 — Form + Validation |
| "The form is submitting multiple times" | Pattern 2 — Scenario 2 |
| "Save form data so it persists on reload" | Pattern 2 — Scenario 3 |
| "Show success/error feedback after submit" | Pattern 2 — Scenario 4 |
| "Add search to this product list" | Pattern 3 — List + Search |
| "The filter stops working after sorting" | Pattern 3 — Scenario 3 |
| "Search crashes when user types special characters" | Pattern 3 — Scenario 4 |
| "Make this cart persist on page refresh" | Pattern 4 — localStorage |
| "User preferences reset on every visit" | Pattern 4 — Scenario 2 |
| "localStorage crashes the app for some users" | Pattern 4 — Scenario 4 |
| "Build a modal that closes on outside click" | Pattern 5 — Show/Hide |
| "Only one accordion item should be open at a time" | Pattern 5 — Scenario 2 |
| "Dropdown won't close when clicking outside" | Pattern 5 — Scenario 3 |
| "Tab content resets when switching tabs" | Pattern 5 — Scenario 4 |
| "Only logged-in users should see this page" | Pattern 6 — Page Navigation |
| "After login, redirect to dashboard" | Pattern 6 — Scenario 2 |
| "User can press back after logout and see dashboard" | Pattern 6 — Scenario 3 |
| "Login form flashes before redirect" | Pattern 6 — Scenario 4 |

---

> **Part 1 complete.**
> Say **"Generate Part 2 — Level 2, Patterns 7 to 11"** to continue.