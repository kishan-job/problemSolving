# React.js Complete Interview Guide
## Part 2: Categories 7-12 (Core Skills)

---

# Table of Contents — Part 2

- [Category 7: Component Lifecycle](#category-7-component-lifecycle)
- [Category 8: Event Handling](#category-8-event-handling)
- [Category 9: Conditional Rendering & Lists](#category-9-conditional-rendering--lists)
- [Category 10: Forms](#category-10-forms)
- [Category 11: State Management](#category-11-state-management)
- [Category 12: React Router](#category-12-react-router)
- [Quick Revision — Part 2 One-Liners](#quick-revision--part-2-one-liners)

---

# Category 7: Component Lifecycle

---

### 🔴 7.1 The 3 Phases: Mounting → Updating → Unmounting

**Explanation:** Every React component goes through three phases. Mounting = component is created and inserted into DOM. Updating = component re-renders due to state/prop changes. Unmounting = component is removed from DOM.

```jsx
function LifecycleDemo() {
  const [count, setCount] = useState(0);

  // MOUNTING — runs once after first render
  useEffect(() => {
    console.log('Component MOUNTED');

    // UNMOUNTING — cleanup runs when component is removed
    return () => {
      console.log('Component UNMOUNTED');
    };
  }, []);

  // UPDATING — runs every time count changes
  useEffect(() => {
    console.log('Count UPDATED to:', count);
  }, [count]);

  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

**Interview Q: How do you replicate componentDidMount, componentDidUpdate, and componentWillUnmount with hooks?**

```jsx
// componentDidMount → useEffect with []
useEffect(() => { /* runs once on mount */ }, []);

// componentDidUpdate → useEffect with [dep]
useEffect(() => { /* runs when dep changes */ }, [dep]);

// componentWillUnmount → return cleanup from useEffect
useEffect(() => {
  return () => { /* runs on unmount */ };
}, []);

// All three combined:
useEffect(() => {
  console.log('mount or update');
  return () => console.log('cleanup before next run or unmount');
}, [dep]);
```

**Common Mistake:** Thinking `useEffect(() => {}, [])` is EXACTLY like componentDidMount. It's close but runs after paint (asynchronous), while componentDidMount runs synchronously before paint.

**One-liner:** Mount = created + inserted. Update = re-render on state/prop change. Unmount = removed. useEffect handles all three.

---

### 🔴 7.2 Cleanup in useEffect

**Explanation:** The return function inside useEffect is the cleanup. It runs before the next effect execution and when the component unmounts. Essential for preventing memory leaks from subscriptions, timers, and event listeners.

```jsx
function WindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    // SETUP — add listener
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', handleResize);

    // CLEANUP — remove listener (prevents memory leak)
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []); // run once on mount, cleanup on unmount

  return <p>Window width: {width}px</p>;
}

// Timer example
function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);

    // Without cleanup, interval runs FOREVER even after component unmounts
    return () => clearInterval(id);
  }, []);

  return <p>{seconds}s</p>;
}
```

**Interview Q: What happens if you don't return a cleanup function from useEffect?**
**A:** Resources like event listeners, timers, or subscriptions keep running after the component unmounts, causing memory leaks. The browser accumulates these ghost listeners, degrading performance over time.

**Common Mistake:** Forgetting to clean up `setInterval`, `addEventListener`, or WebSocket connections.

**One-liner:** useEffect cleanup = the return function. Runs before next effect and on unmount. Always clean up timers, listeners, subscriptions.

---

### 🟡 7.3 Error Boundaries

**Explanation:** Error boundaries catch JavaScript errors in their child component tree, log them, and display a fallback UI. They prevent the entire app from crashing. Currently, error boundaries must be class components — there's no hook equivalent yet.

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };  // update state to show fallback
  }

  componentDidCatch(error, errorInfo) {
    console.error('Error caught:', error, errorInfo);
    // Send to error reporting service (Sentry, etc.)
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;  // fallback UI
    }
    return this.props.children;
  }
}

// Usage — wrap any part of your app
function App() {
  return (
    <ErrorBoundary>
      <Dashboard />   {/* if this crashes, fallback shows */}
    </ErrorBoundary>
  );
}
```

**Interview Q: Do error boundaries catch all errors?**
**A:** No. They don't catch: event handler errors, async code (setTimeout, promises), server-side rendering errors, or errors in the error boundary itself. For event handlers, use regular try/catch.

**Common Mistake:** Thinking error boundaries replace try/catch. They only catch render-time errors and lifecycle method errors in the component tree below them.

**One-liner:** Error boundary = class component that catches child render errors. Shows fallback UI instead of crashing the whole app.

---

### 🟡 7.4 Render Phase vs Commit Phase

**Explanation:** React rendering happens in two phases. The Render phase is where React calculates what changed (pure, no side effects, can be paused). The Commit phase is where React actually updates the DOM and runs effects.

```
RENDER PHASE (can be paused/restarted by Fiber):
  - Calls your component function
  - Creates new Virtual DOM
  - Diffs old vs new Virtual DOM
  - NO side effects allowed here (no DOM updates, no API calls)

COMMIT PHASE (synchronous, can't be interrupted):
  - Updates the Real DOM with calculated changes
  - Runs useLayoutEffect
  - Browser paints the screen
  - Runs useEffect
```

**Interview Q: Why shouldn't you have side effects in the render phase?**
**A:** The render phase can run multiple times (React Fiber can pause and restart it). If you have side effects (API calls, DOM manipulation) in the render body, they'll execute multiple times unpredictably. Put side effects in useEffect (commit phase).

**Common Mistake:** Fetching data or modifying DOM directly inside the component body instead of useEffect.

**One-liner:** Render phase = calculate changes (pure, no side effects). Commit phase = apply changes to DOM + run effects.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 8: Event Handling

---

### 🔴 8.1 How Events Work in React (Synthetic Events)

**Explanation:** React wraps native browser events in a SyntheticEvent object. This provides a consistent API across all browsers. React attaches one event listener at the root (event delegation), not on each element.

```jsx
function Button() {
  const handleClick = (e) => {
    // e is a SyntheticEvent, not a native browser event
    console.log(e);                  // SyntheticEvent object
    console.log(e.nativeEvent);      // actual browser event (if you need it)
    console.log(e.target);           // the element that was clicked
    console.log(e.currentTarget);    // the element the handler is attached to
  };

  return <button onClick={handleClick}>Click me</button>;
}
```

**Interview Q: What is a SyntheticEvent?**
**A:** It's React's cross-browser wrapper around the native browser event. It has the same interface as native events (stopPropagation, preventDefault, target, etc.) but works identically across all browsers. React pools and reuses these objects for performance (before React 17).

**Common Mistake:** Trying to access event properties asynchronously (e.g., inside setTimeout). In React 16, events were pooled and nullified. In React 17+, this was removed — events persist normally.

**One-liner:** SyntheticEvent = React's cross-browser event wrapper. Same API as native events. React 17+ no longer pools them.

---

### 🔴 8.2 Handling onClick, onChange, onSubmit

**Explanation:** React uses camelCase for event names (onClick not onclick) and passes functions, not strings.

```jsx
function Form() {
  const [name, setName] = useState('');

  // onClick — button clicks
  const handleClick = () => {
    alert(`Hello, ${name}!`);
  };

  // onChange — input changes
  const handleChange = (e) => {
    setName(e.target.value);
  };

  // onSubmit — form submission
  const handleSubmit = (e) => {
    e.preventDefault();  // prevent page reload!
    console.log('Submitted:', name);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={name} onChange={handleChange} />
      <button type="submit">Submit</button>
      <button type="button" onClick={handleClick}>Say Hi</button>
    </form>
  );
}
```

**Interview Q: Why do you need e.preventDefault() in form onSubmit?**
**A:** By default, HTML forms reload the entire page when submitted. `e.preventDefault()` stops this default behavior so React can handle the submission with JavaScript (SPA behavior).

**Common Mistake:** Forgetting `e.preventDefault()` in form handlers — the page reloads and state is lost.

**One-liner:** onClick, onChange, onSubmit — camelCase, pass function reference. Always preventDefault on forms.

---

### 🔴 8.3 Passing Arguments to Event Handlers

**Explanation:** You can't call a function directly in JSX props (it would execute immediately). Use an arrow function wrapper or `.bind()`.

```jsx
function ItemList({ items, onDelete }) {
  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>
          {item.name}

          {/* ✅ Arrow function — creates new function per render */}
          <button onClick={() => onDelete(item.id)}>Delete</button>

          {/* ✅ Bind — also creates new function per render */}
          <button onClick={onDelete.bind(null, item.id)}>Delete</button>

          {/* ❌ WRONG — calls immediately on render, not on click */}
          <button onClick={onDelete(item.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}
```

**Interview Q: What's wrong with `onClick={handleDelete(id)}`?**
**A:** It CALLS the function immediately during render, not on click. `handleDelete(id)` executes and its return value becomes the handler (probably undefined). Use `onClick={() => handleDelete(id)}` to wrap it in a function that executes on click.

**Common Mistake:** Writing `onClick={fn()}` instead of `onClick={() => fn()}` or `onClick={fn}`.

**One-liner:** Pass function reference: `onClick={fn}` or `onClick={() => fn(arg)}`. Never `onClick={fn()}` — that calls it immediately.

---

### 🟡 8.4 event.target vs event.currentTarget

**Explanation:** `e.target` is the actual element that was clicked (the deepest element). `e.currentTarget` is the element the event handler is attached to.

```jsx
function Parent() {
  const handleClick = (e) => {
    console.log('target:', e.target);          // the <span> that was clicked
    console.log('currentTarget:', e.currentTarget); // the <div> with the handler
  };

  return (
    <div onClick={handleClick} style={{ padding: 20, background: '#eee' }}>
      <span>Click this text</span>
    </div>
  );
}
// If you click the <span>:
//   target = <span>        (what you actually clicked)
//   currentTarget = <div>  (where the handler lives)
```

**Interview Q: When is this distinction important?**
**A:** In event delegation — when a parent handles clicks for many children. You use `e.target` to identify WHICH child was clicked and `e.currentTarget` to reference the parent with the handler.

**One-liner:** target = element clicked. currentTarget = element with the handler. Different when event bubbles up.

---

### 🟡 8.5 Debouncing and Throttling in React

**Explanation:** Debounce = wait until user STOPS doing something for X ms. Throttle = allow action at most once every X ms. Both prevent excessive function calls (search, scroll, resize).

```jsx
function SearchInput() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);

  // Debounced search — only fires 300ms after user STOPS typing
  const debouncedSearch = useMemo(
    () => debounce((q) => {
      fetch(`/api/search?q=${q}`)
        .then(res => res.json())
        .then(data => setResults(data));
    }, 300),
    []
  );

  // Cleanup on unmount
  useEffect(() => {
    return () => debouncedSearch.cancel();
  }, [debouncedSearch]);

  const handleChange = (e) => {
    setQuery(e.target.value);          // update input immediately
    debouncedSearch(e.target.value);   // search after user stops
  };

  return <input value={query} onChange={handleChange} />;
}

// Simple debounce implementation
function debounce(fn, delay) {
  let timeoutId;
  const debounced = (...args) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn(...args), delay);
  };
  debounced.cancel = () => clearTimeout(timeoutId);
  return debounced;
}
```

**Interview Q: Debounce vs Throttle — when to use which?**
**A:**
- **Debounce** — search input, form validation, auto-save. "Wait until they stop."
- **Throttle** — scroll handler, resize handler, game input. "Allow at most once per X ms."

**Common Mistake:** Creating a new debounced function on every render (defeats the purpose). Use useMemo or useRef to persist it.

**One-liner:** Debounce = fires after pause. Throttle = fires at fixed intervals. Both prevent excessive calls. Persist with useMemo/useRef.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 9: Conditional Rendering & Lists

---

### 🔴 9.1 Conditional Rendering

**Explanation:** React has no special directive for conditions (no v-if like Vue). You use plain JavaScript: ternary, &&, if/else outside return.

```jsx
function Dashboard({ isLoggedIn, isAdmin, notifications }) {
  // Method 1: if/else OUTSIDE return
  if (!isLoggedIn) {
    return <LoginPage />;
  }

  // Method 2: Ternary INSIDE return
  // Method 3: && INSIDE return
  return (
    <div>
      {isAdmin ? <AdminPanel /> : <UserPanel />}

      {notifications.length > 0 && (
        <p>You have {notifications.length} notifications</p>
      )}
    </div>
  );
}

// Method 4: Return null to render nothing
function MaybeVisible({ show }) {
  if (!show) return null;
  return <div>I'm visible!</div>;
}
```

**Interview Q: What are the different ways to conditionally render in React?**
**A:** Four ways: (1) if/else before return, (2) ternary `? :` in JSX, (3) logical AND `&&` in JSX, (4) return `null` to render nothing. No `v-if` or `ng-if` — just JavaScript.

**One-liner:** No special syntax — use ternary (? :), && , if/else outside return, or return null.

---

### 🔴 9.2 Rendering Lists with .map()

**Explanation:** Use `.map()` to transform an array of data into an array of JSX elements. Always provide a unique `key` prop.

```jsx
function UserList({ users }) {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>
          {user.name} — {user.email}
        </li>
      ))}
    </ul>
  );
}

// With objects
function ProductGrid({ products }) {
  return (
    <div className="grid">
      {products.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}
```

**Interview Q: Why do you use .map() instead of for loops in JSX?**
**A:** JSX accepts expressions, not statements. `.map()` is an expression that returns an array of elements. `for` loops are statements and can't be used directly inside JSX `{}`.

**One-liner:** Use .map() to convert data arrays to JSX arrays. Always add a unique key prop.

---

### 🔴 9.3 The Key Prop

**Explanation:** `key` is a special prop that helps React identify which list items changed, were added, or removed. React uses keys during reconciliation to match old and new elements efficiently.

```jsx
// ✅ GOOD — unique, stable ID
{users.map(user => <UserCard key={user.id} user={user} />)}

// ⚠️ BAD — index as key (causes bugs with reordering)
{users.map((user, index) => <UserCard key={index} user={user} />)}

// ❌ WORST — no key (React warns in console)
{users.map(user => <UserCard user={user} />)}
```

**Interview Q: Why is using array index as key a problem?**
**A:** When items are reordered, added, or removed, index keys shift. React thinks the component at index 0 is the same component as before (same key) even though it's a different item. This causes incorrect state, broken animations, and input values appearing in wrong elements.

**When index is OK:** Static lists that never reorder, filter, or add/remove items. But using a unique ID is always safer.

**Common Mistake:** Using random values (`key={Math.random()}`) — this creates a new key every render, destroying and recreating every element. Never do this.

**One-liner:** key = unique stable ID for each list item. Helps React track changes. Never use Math.random(). Avoid index unless list is static.

---

### 🟡 9.4 Short-Circuit Gotcha (0 && Component)

**Explanation:** When using `&&` for conditional rendering, if the left side is `0` (falsy but a number), React renders `0` on screen instead of nothing.

```jsx
function Notifications({ count }) {
  return (
    <div>
      {/* ❌ BUG — when count is 0, renders "0" on screen */}
      {count && <p>You have {count} notifications</p>}

      {/* ✅ FIX — explicitly use boolean check */}
      {count > 0 && <p>You have {count} notifications</p>}

      {/* ✅ FIX — double negation to boolean */}
      {!!count && <p>You have {count} notifications</p>}

      {/* ✅ FIX — ternary */}
      {count ? <p>You have {count} notifications</p> : null}
    </div>
  );
}
```

**Interview Q: What does `{0 && <Component />}` render?**
**A:** It renders `0` on screen. Because `0 && anything` evaluates to `0` (the left side), and React renders numbers. `false && anything` evaluates to `false`, which React doesn't render. So `0` is the problem, not `false`.

**One-liner:** `0 && <X/>` renders "0" on screen. Fix: use `count > 0 &&` or ternary.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 10: Forms

---

### 🔴 10.1 Controlled Components

**Explanation:** In a controlled component, React state is the "single source of truth" for the input value. Every keystroke updates state via onChange, and the input displays the state value.

```jsx
function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Login:', { email, password });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}                              // state drives the value
        onChange={(e) => setEmail(e.target.value)}  // every keystroke updates state
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button type="submit">Login</button>
    </form>
  );
}
```

**Interview Q: Why use controlled components?**
**A:** You have full control over the input value at all times — you can validate on every keystroke, transform input (uppercase), conditionally prevent input, and the form data is always in sync with React state.

**Common Mistake:** Setting `value` without `onChange` — this makes the input read-only. React warns: "You provided a `value` prop without an `onChange` handler."

**One-liner:** Controlled = `value={state}` + `onChange={setState}`. React state is the single source of truth.

---

### 🔴 10.2 Handling Multiple Inputs with One Handler

**Explanation:** Instead of one handler per input, use the input's `name` attribute as the key in state.

```jsx
function SignupForm() {
  const [form, setForm] = useState({
    name: '',
    email: '',
    age: '',
  });

  // ONE handler for ALL inputs
  const handleChange = (e) => {
    const { name, value } = e.target;
    setForm(prev => ({
      ...prev,          // keep all other fields
      [name]: value,    // update only the changed field (computed property name)
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(form);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="name" value={form.name} onChange={handleChange} />
      <input name="email" value={form.email} onChange={handleChange} />
      <input name="age" value={form.age} onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Interview Q: How do you handle multiple form inputs without separate handlers?**
**A:** Store all fields in one state object. Use the `name` attribute of each input as the key. In the handler, use `[e.target.name]: e.target.value` with computed property names to update the right field.

**Common Mistake:** Forgetting to spread the previous state (`...prev`), which erases all other fields.

**One-liner:** One state object + one handler. Use `[e.target.name]: e.target.value` with spread to update only the changed field.

---

### 🔴 10.3 Uncontrolled Components

**Explanation:** In uncontrolled components, the DOM manages the input value. You access it via a ref when needed (usually on submit). Use `defaultValue` instead of `value`.

```jsx
function QuickForm() {
  const nameRef = useRef();
  const emailRef = useRef();

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log({
      name: nameRef.current.value,    // read value from DOM
      email: emailRef.current.value,
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <input ref={nameRef} defaultValue="" />
      <input ref={emailRef} defaultValue="" />
      <button type="submit">Submit</button>
    </form>
  );
}

// File inputs are ALWAYS uncontrolled (can't set value programmatically)
function FileUpload() {
  const fileRef = useRef();
  const handleUpload = () => {
    console.log(fileRef.current.files[0]);
  };
  return <input type="file" ref={fileRef} onChange={handleUpload} />;
}
```

**Interview Q: Controlled vs Uncontrolled — when to use each?**

| | Controlled | Uncontrolled |
|---|---|---|
| Value managed by | React state | DOM |
| Access value | From state (anytime) | From ref (on demand) |
| Validation | On every keystroke | On submit |
| Best for | Complex forms, real-time validation | Simple forms, file inputs, third-party libraries |

**One-liner:** Uncontrolled = DOM owns value, read with ref. Use `defaultValue`. Best for simple forms and file inputs.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 11: State Management

---

### 🔴 11.1 Context API

**Explanation:** Context provides a way to share data across the component tree without prop drilling. You create a context, wrap a section of the tree with a Provider, and consume the value with useContext.

```jsx
// 1. CREATE context
const ThemeContext = React.createContext('light');

// 2. PROVIDE value at the top
function App() {
  const [theme, setTheme] = useState('dark');

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Header />
      <Main />
    </ThemeContext.Provider>
  );
}

// 3. CONSUME anywhere below (no prop drilling!)
function Header() {
  const { theme, setTheme } = useContext(ThemeContext);

  return (
    <header className={theme}>
      <button onClick={() => setTheme(theme === 'dark' ? 'light' : 'dark')}>
        Toggle: {theme}
      </button>
    </header>
  );
}
```

**Interview Q: When should you use Context API?**
**A:** For data that many components at different nesting levels need: themes, auth/user data, locale/language, feature flags. NOT for all state — it causes re-renders of ALL consumers when value changes.

**Common Mistake:** Putting everything in Context. Context re-renders every consumer when the value changes. Use it for infrequently changing data, not for rapidly changing state like form inputs.

**One-liner:** Context = createContext → Provider wraps tree → useContext consumes. Avoids prop drilling. Don't overuse — causes re-renders.

---

### 🔴 11.2 Context Re-rendering Problem

**Explanation:** When a Context Provider's value changes, ALL components using `useContext` for that context re-render — even if they only use a part of the value that didn't change.

```jsx
// ❌ PROBLEM — new object every render → all consumers re-render
function App() {
  const [user, setUser] = useState({ name: 'Alice' });
  const [theme, setTheme] = useState('dark');

  return (
    // { user, theme } is a NEW object every render
    <AppContext.Provider value={{ user, theme, setTheme }}>
      <Children />
    </AppContext.Provider>
  );
}

// ✅ FIX 1: useMemo the value
function App() {
  const [user, setUser] = useState({ name: 'Alice' });
  const [theme, setTheme] = useState('dark');

  const value = useMemo(() => ({ user, theme, setTheme }), [user, theme]);

  return (
    <AppContext.Provider value={value}>
      <Children />
    </AppContext.Provider>
  );
}

// ✅ FIX 2: Split into separate contexts
const UserContext = React.createContext();
const ThemeContext = React.createContext();

function App() {
  const [user, setUser] = useState({ name: 'Alice' });
  const [theme, setTheme] = useState('dark');

  return (
    <UserContext.Provider value={user}>
      <ThemeContext.Provider value={{ theme, setTheme }}>
        <Children />
      </ThemeContext.Provider>
    </UserContext.Provider>
  );
}
// Now changing theme only re-renders ThemeContext consumers, not UserContext consumers
```

**Interview Q: How do you prevent unnecessary re-renders with Context?**
**A:** Three strategies: (1) useMemo the provider value, (2) split into separate contexts so changes are isolated, (3) use React.memo on consumer components with careful prop extraction.

**One-liner:** Context value change → ALL consumers re-render. Fix: useMemo the value, or split into separate contexts.

---

### 🔴 11.3 Redux Basics

**Explanation:** Redux is a predictable state container. It has one global store. State changes only through dispatching actions, which are processed by pure reducer functions. One-way flow: dispatch(action) → reducer(state, action) → new state → re-render.

```jsx
// 1. ACTION — what happened
const increment = { type: 'INCREMENT' };
const decrement = { type: 'DECREMENT' };
const addBy = (amount) => ({ type: 'ADD', payload: amount });

// 2. REDUCER — how state changes (pure function)
function counterReducer(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT': return state + 1;
    case 'DECREMENT': return state - 1;
    case 'ADD':       return state + action.payload;
    default:          return state;
  }
}

// 3. STORE — holds the state
// (with Redux Toolkit — the modern way)
import { configureStore, createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: 0,
  reducers: {
    increment: (state) => state + 1,
    decrement: (state) => state - 1,
    addBy: (state, action) => state + action.payload,
  },
});

const store = configureStore({
  reducer: { counter: counterSlice.reducer },
});

// 4. USE IN COMPONENT
import { useSelector, useDispatch } from 'react-redux';

function Counter() {
  const count = useSelector((state) => state.counter);
  const dispatch = useDispatch();

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => dispatch(counterSlice.actions.increment())}>+</button>
      <button onClick={() => dispatch(counterSlice.actions.addBy(5))}>+5</button>
    </div>
  );
}
```

**Interview Q: What is the Redux data flow?**
**A:** Component dispatches action → action goes to reducer → reducer returns new state → store updates → subscribed components re-render with new state. State is never mutated directly — always replaced.

**Common Mistake:** Mutating state in reducers. In classic Redux, you must return new objects. Redux Toolkit uses Immer internally, which lets you "mutate" safely (it creates a new object behind the scenes).

**One-liner:** Redux: dispatch(action) → reducer(state, action) → new state → re-render. One store, one-way flow, pure reducers.

---

### 🟡 11.4 Redux vs Context — When to Use Which

| | Context API | Redux |
|---|---|---|
| Complexity | Simple | More boilerplate (less with RTK) |
| Best for | Theme, auth, locale (low-frequency updates) | Complex app state, frequent updates |
| DevTools | No | Yes (Redux DevTools — time travel debugging) |
| Middleware | No | Yes (async, logging, etc.) |
| Performance | Re-renders all consumers | Selective re-renders with useSelector |
| Learning curve | Low | Medium |

**Interview Q: When would you choose Redux over Context?**
**A:** When you have complex state logic, many state interactions, need middleware for async operations, want DevTools for debugging, or have performance concerns with frequent updates. Context is fine for simple, infrequently changing data.

**One-liner:** Context = simple shared data (theme, auth). Redux = complex state with DevTools, middleware, and selective re-renders.

---

### 🟡 11.5 Zustand (Lightweight Alternative)

**Explanation:** Zustand is a small, fast state management library. No providers, no boilerplate. Create a store with a hook, use it anywhere.

```jsx
import { create } from 'zustand';

// Create store — one line
const useStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));

// Use in any component — no Provider needed!
function Counter() {
  const { count, increment, decrement } = useStore();
  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}
```

**Interview Q: Why would you use Zustand over Redux?**
**A:** Much less boilerplate, no Provider wrapper needed, simpler API, smaller bundle size (~1KB), and no action types or switch statements. Good for small to medium apps. Redux is better for large apps needing DevTools and middleware.

**One-liner:** Zustand = tiny state management. create(set => state) → use anywhere. No Provider, no boilerplate.

---

### 🟢 11.6 React Query / TanStack Query

**Explanation:** React Query manages server state — data fetched from APIs. It handles caching, background refetching, stale data, loading/error states, and pagination automatically. It separates "server state" from "client state."

```jsx
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';

function UserList() {
  // Fetch data — automatic caching, loading, error handling
  const { data, isLoading, error } = useQuery({
    queryKey: ['users'],
    queryFn: () => fetch('/api/users').then(res => res.json()),
    staleTime: 5 * 60 * 1000,  // data is fresh for 5 minutes
  });

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error!</p>;

  return (
    <ul>
      {data.map(user => <li key={user.id}>{user.name}</li>)}
    </ul>
  );
}

// Mutations (POST, PUT, DELETE)
function AddUser() {
  const queryClient = useQueryClient();
  const mutation = useMutation({
    mutationFn: (newUser) => fetch('/api/users', {
      method: 'POST',
      body: JSON.stringify(newUser),
    }),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] }); // refetch users
    },
  });

  return <button onClick={() => mutation.mutate({ name: 'Bob' })}>Add User</button>;
}
```

**Interview Q: What's the difference between client state and server state?**
**A:** Client state = UI state owned by the app (theme, modal open/closed, form input). Server state = data from an external source (API, database) that's asynchronous, shared, and can be stale. React Query handles server state; useState/Redux handle client state.

**One-liner:** React Query = server state management. Handles caching, refetching, loading/error automatically. Separate from client state (useState/Redux).

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Category 12: React Router

---

### 🔴 12.1 Setting Up Routes

**Explanation:** React Router enables client-side routing — navigation without full page reloads. You define routes that map URL paths to components.

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/users">Users</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/users" element={<Users />} />
        <Route path="/users/:id" element={<UserProfile />} />
        <Route path="*" element={<NotFound />} />  {/* 404 catch-all */}
      </Routes>
    </BrowserRouter>
  );
}
```

**Interview Q: What does `<Route path="*">` do?**
**A:** It's a catch-all route that matches any URL not matched by other routes. Used to display a 404 "Not Found" page.

**One-liner:** BrowserRouter wraps app → Routes contains Route elements → each Route maps a path to a component.

---

### 🔴 12.2 Link vs NavLink

**Explanation:** `Link` renders an anchor tag that navigates without page reload. `NavLink` is the same but adds "active" styling when the link matches the current URL.

```jsx
import { Link, NavLink } from 'react-router-dom';

// Link — simple navigation
<Link to="/about">About</Link>

// NavLink — adds "active" class when URL matches
<NavLink
  to="/about"
  className={({ isActive }) => isActive ? 'active-link' : ''}
>
  About
</NavLink>

// ❌ WRONG — using <a> tag causes full page reload
<a href="/about">About</a>
```

**Interview Q: Why use Link instead of `<a>` tags?**
**A:** `<a>` causes a full page reload (requests the HTML from the server). `Link` uses JavaScript to update the URL and re-render the matching component without reloading — SPA behavior.

**One-liner:** Link = navigate without reload. NavLink = same + active styling. Never use `<a>` for internal navigation.

---

### 🔴 12.3 useNavigate and useParams

**Explanation:** `useNavigate` gives you programmatic navigation (redirect after form submit). `useParams` reads URL parameters defined in the route path.

```jsx
import { useNavigate, useParams } from 'react-router-dom';

// useNavigate — redirect programmatically
function LoginForm() {
  const navigate = useNavigate();

  const handleSubmit = async (e) => {
    e.preventDefault();
    await login();
    navigate('/dashboard');         // redirect after login
    // navigate('/dashboard', { replace: true }); // replace history (can't go back)
    // navigate(-1);                // go back one page
  };

  return <form onSubmit={handleSubmit}>...</form>;
}

// useParams — read URL parameters
// Route: <Route path="/users/:id" element={<UserProfile />} />
// URL:   /users/42
function UserProfile() {
  const { id } = useParams();   // id = "42" (always a string!)

  return <p>User ID: {id}</p>;
}
```

**Interview Q: Are useParams values strings or numbers?**
**A:** Always strings. `/users/42` gives `{ id: "42" }`. You must parse them yourself: `const userId = parseInt(id)` or `Number(id)`.

**Common Mistake:** Using `useParams` outside of a route component. It only works inside a component rendered by `<Route>`.

**One-liner:** useNavigate() = redirect in code. useParams() = read URL params (always strings). Both must be inside Router.

---

### 🟡 12.4 Nested Routes and Outlet

**Explanation:** Nested routes let you define routes inside routes. The parent renders an `<Outlet>` component where the child route's element appears.

```jsx
function App() {
  return (
    <Routes>
      <Route path="/dashboard" element={<DashboardLayout />}>
        {/* These render inside DashboardLayout's <Outlet> */}
        <Route index element={<DashboardHome />} />         {/* /dashboard */}
        <Route path="settings" element={<Settings />} />     {/* /dashboard/settings */}
        <Route path="profile" element={<Profile />} />       {/* /dashboard/profile */}
      </Route>
    </Routes>
  );
}

function DashboardLayout() {
  return (
    <div>
      <h1>Dashboard</h1>
      <nav>
        <Link to="/dashboard">Home</Link>
        <Link to="/dashboard/settings">Settings</Link>
        <Link to="/dashboard/profile">Profile</Link>
      </nav>
      <Outlet />  {/* child route renders HERE */}
    </div>
  );
}
```

**Interview Q: What is an Outlet in React Router?**
**A:** `<Outlet>` is a placeholder in a parent route component where the matched child route's element renders. It's like `{children}` but for routes.

**One-liner:** Nested routes = routes inside routes. Parent uses `<Outlet>` to render the matched child route component.

---

### 🟡 12.5 Protected Routes

**Explanation:** Protected (or private) routes check if the user is authenticated before rendering the component. If not, redirect to login.

```jsx
function ProtectedRoute({ children }) {
  const { isAuthenticated } = useAuth(); // your auth hook/context

  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }

  return children;
}

// Usage in routes
function App() {
  return (
    <Routes>
      <Route path="/login" element={<Login />} />
      <Route
        path="/dashboard"
        element={
          <ProtectedRoute>
            <Dashboard />
          </ProtectedRoute>
        }
      />
    </Routes>
  );
}

// OR — as a layout route
function App() {
  return (
    <Routes>
      <Route path="/login" element={<Login />} />
      <Route element={<ProtectedRoute />}>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/settings" element={<Settings />} />
      </Route>
    </Routes>
  );
}

function ProtectedRoute() {
  const { isAuthenticated } = useAuth();
  return isAuthenticated ? <Outlet /> : <Navigate to="/login" replace />;
}
```

**Interview Q: How do you protect routes in React?**
**A:** Create a wrapper component that checks auth status. If authenticated, render children (or Outlet). If not, redirect to login using `<Navigate>`. Can be used as a wrapper or as a layout route.

**One-liner:** Protected route = check auth → render children if yes → redirect to login if no. Use Navigate + Outlet.

---

### 🟡 12.6 Lazy Loading Routes

**Explanation:** Use `React.lazy()` and `<Suspense>` to load route components only when the user navigates to them. Reduces initial bundle size.

```jsx
import { lazy, Suspense } from 'react';

// Lazy load — component code loads only when route is visited
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Settings = lazy(() => import('./pages/Settings'));
const Profile = lazy(() => import('./pages/Profile'));

function App() {
  return (
    <Suspense fallback={<p>Loading page...</p>}>
      <Routes>
        <Route path="/" element={<Home />} />  {/* not lazy — loads immediately */}
        <Route path="/dashboard" element={<Dashboard />} />  {/* lazy */}
        <Route path="/settings" element={<Settings />} />    {/* lazy */}
        <Route path="/profile" element={<Profile />} />      {/* lazy */}
      </Routes>
    </Suspense>
  );
}
```

**Interview Q: What is code splitting and why use it with routes?**
**A:** Code splitting breaks your bundle into smaller chunks. Without it, the user downloads ALL page code upfront (even pages they'll never visit). With lazy loading, each page's code downloads only when navigated to, making the initial load much faster.

**Common Mistake:** Forgetting `<Suspense>` around lazy components — React throws an error because it needs a fallback while the chunk loads.

**One-liner:** React.lazy(() => import()) + Suspense = load route code only when visited. Faster initial load. Always wrap in Suspense.

---

[↑ Back to Table of Contents](#table-of-contents--part-2)

---

# Quick Revision — Part 2 One-Liners

```
COMPONENT LIFECYCLE
  3 phases: Mount → Update → Unmount
  useEffect(fn, []) = mount. useEffect(fn, [dep]) = update. return () => = unmount
  Always clean up timers, listeners, subscriptions
  Error boundaries = class components that catch child render errors
  Render phase = calculate (pure). Commit phase = apply to DOM + effects

EVENT HANDLING
  SyntheticEvent = React's cross-browser event wrapper
  camelCase: onClick, onChange, onSubmit. Pass functions, not strings
  onClick={() => fn(arg)} — never onClick={fn(arg)}
  target = what was clicked. currentTarget = where handler lives
  Debounce = after pause. Throttle = at fixed rate. Persist with useMemo/useRef

CONDITIONAL RENDERING & LISTS
  Ternary (? :), && , if/else outside return, return null
  .map() for lists. Always add unique key prop
  Index as key = bad for reorderable lists. Use stable IDs
  0 && <X/> renders "0" — use count > 0 &&

FORMS
  Controlled = value + onChange (React owns state)
  Uncontrolled = ref + defaultValue (DOM owns state)
  One handler for multiple inputs: [e.target.name]: e.target.value
  File inputs are always uncontrolled

STATE MANAGEMENT
  Context: createContext → Provider → useContext. Avoid for frequent updates
  Context re-render fix: useMemo value, or split contexts
  Redux: dispatch(action) → reducer → new state. One store, one-way flow
  Redux for complex state + DevTools. Context for simple shared data
  Zustand: tiny alternative, no Provider, minimal boilerplate
  React Query: server state (caching, refetch). useState/Redux: client state

REACT ROUTER
  BrowserRouter → Routes → Route (path + element)
  Link/NavLink for navigation (not <a>). NavLink adds active styling
  useNavigate = redirect in code. useParams = read URL params (strings!)
  Nested routes use Outlet. Protected routes check auth before rendering
  React.lazy + Suspense = load route code on demand
```

[↑ Back to Table of Contents](#table-of-contents--part-2)
