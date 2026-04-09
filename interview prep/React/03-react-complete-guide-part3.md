# React.js Complete Interview Guide
## Part 3: Categories 13-18 (Advanced + Machine Coding)

---

# Table of Contents — Part 3

- [Category 13: Performance Optimization](#category-13-performance-optimization)
- [Category 14: Patterns & Architecture](#category-14-patterns--architecture)
- [Category 15: Styling](#category-15-styling)
- [Category 16: Testing](#category-16-testing)
- [Category 17: React Internals](#category-17-react-internals)
- [Category 18: Machine Coding — Build These](#category-18-machine-coding)
- [Quick Revision — Part 3 One-Liners](#quick-revision--part-3-one-liners)

---

# Category 13: Performance Optimization

---

### 🔴 13.1 React.memo

**Explanation:** `React.memo` is a higher-order component that wraps a functional component and prevents it from re-rendering if its props haven't changed (shallow comparison).

```jsx
// Without memo — re-renders every time Parent re-renders
function ExpensiveList({ items }) {
  console.log('ExpensiveList rendered');
  return <ul>{items.map(i => <li key={i.id}>{i.name}</li>)}</ul>;
}

// With memo — skips re-render if items prop is the same reference
const MemoizedList = React.memo(ExpensiveList);

function Parent() {
  const [count, setCount] = useState(0);
  const items = useMemo(() => [{ id: 1, name: 'A' }], []); // stable reference

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
      {/* MemoizedList won't re-render when count changes */}
      <MemoizedList items={items} />
    </div>
  );
}

// Custom comparison function
const CustomMemo = React.memo(MyComponent, (prevProps, nextProps) => {
  return prevProps.id === nextProps.id; // return true to SKIP re-render
});
```

**Interview Q: When should you use React.memo?**
**A:** When a component re-renders frequently but its props rarely change, especially if the component is expensive to render (large lists, complex calculations, charts). Don't wrap everything — the comparison itself has overhead.

**Common Mistake:** Using React.memo but passing a new object/function as props every render — memo becomes useless because the reference changes. Pair with useMemo/useCallback.

**One-liner:** React.memo(Component) = skip re-render if props unchanged (shallow compare). Pair with useMemo/useCallback for object/function props.

---

### 🔴 13.2 Code Splitting with React.lazy

**Explanation:** Code splitting breaks your app into smaller bundles that load on demand. `React.lazy` loads a component only when it's rendered for the first time.

```jsx
import { lazy, Suspense } from 'react';

// These components load ONLY when needed
const HeavyChart = lazy(() => import('./components/HeavyChart'));
const AdminPanel = lazy(() => import('./components/AdminPanel'));

function App() {
  const [showChart, setShowChart] = useState(false);

  return (
    <div>
      <button onClick={() => setShowChart(true)}>Show Chart</button>

      <Suspense fallback={<p>Loading chart...</p>}>
        {showChart && <HeavyChart />}
      </Suspense>
    </div>
  );
}
```

**Interview Q: What is the benefit of code splitting?**
**A:** The user downloads only the code they need for the current page/feature. Without splitting, a large app might have a 5MB bundle that ALL downloads upfront. With splitting, the initial load might be 500KB, and other features load on demand.

**Common Mistake:** Forgetting Suspense — `React.lazy` requires a `<Suspense>` boundary with a fallback, or React throws an error.

**One-liner:** React.lazy(() => import()) = load component on demand. Wrap in Suspense. Reduces initial bundle size.

---

### 🔴 13.3 Why Immutability Helps Performance

**Explanation:** React detects changes by comparing references (===). If you mutate an object, the reference stays the same — React thinks nothing changed and skips the update. Immutable updates create new references, making changes detectable instantly.

```jsx
// ❌ MUTATION — same reference, React can't detect change
const handleAddBad = () => {
  items.push(newItem);       // mutates original array
  setItems(items);           // same reference → React won't re-render
};

// ✅ IMMUTABLE — new reference, React detects change
const handleAddGood = () => {
  setItems([...items, newItem]);  // new array → new reference → re-render ✓
};

// Performance benefit with React.memo:
// React.memo does shallow compare: prevProps.items === nextProps.items
// With immutability: old array !== new array → knows to re-render
// With mutation: old array === old array → thinks nothing changed
```

**Interview Q: How does immutability help React's performance?**
**A:** React uses reference equality (===) to detect changes. Creating new references on updates makes change detection O(1) — just compare the pointer, not deep-compare every property. This is why React.memo, useSelector, and shouldComponentUpdate work efficiently.

**One-liner:** Immutability = new reference on change → React detects it instantly via === comparison → efficient re-rendering.

---

### 🟡 13.4 Avoiding Unnecessary Re-renders

**Explanation:** The most common causes of unnecessary re-renders and how to fix them.

```jsx
// CAUSE 1: Parent re-renders → all children re-render
// FIX: React.memo on expensive children

// CAUSE 2: New object/array/function created every render
function Parent() {
  // ❌ New object every render → child always sees "new" props
  const style = { color: 'red' };
  const handleClick = () => console.log('clicked');

  // ✅ FIX: memoize
  const styleMemo = useMemo(() => ({ color: 'red' }), []);
  const handleClickMemo = useCallback(() => console.log('clicked'), []);

  return <MemoizedChild style={styleMemo} onClick={handleClickMemo} />;
}

// CAUSE 3: Context value changes → all consumers re-render
// FIX: Split contexts, useMemo the value

// CAUSE 4: State stored too high in the tree
// FIX: Keep state close to where it's used (state colocation)

// CAUSE 5: Updating state that doesn't affect UI
// FIX: Use useRef instead of useState for non-rendered values
```

**Interview Q: How do you find unnecessary re-renders?**
**A:** Use React DevTools Profiler — it highlights which components re-rendered and why. You can also add `console.log('rendered')` temporarily. The Profiler shows render duration and frequency for each component.

**One-liner:** Common causes: parent re-renders, new object/function refs, Context changes, state too high. Fix: memo, useMemo, useCallback, colocation.

---

### 🟡 13.5 Windowing / Virtualization

**Explanation:** When rendering thousands of items, the browser struggles because all DOM nodes exist simultaneously. Virtualization renders ONLY the items visible in the viewport, dramatically improving performance.

```jsx
// Using react-window (lightweight virtualization library)
import { FixedSizeList } from 'react-window';

function VirtualizedList({ items }) {
  const Row = ({ index, style }) => (
    <div style={style}>
      {items[index].name}
    </div>
  );

  return (
    <FixedSizeList
      height={400}          // viewport height
      itemCount={items.length}  // total items (could be 100,000)
      itemSize={35}         // each row height in px
      width="100%"
    >
      {Row}
    </FixedSizeList>
  );
  // Only ~12 rows rendered at a time (400/35), not all 100,000
}
```

**Interview Q: How would you efficiently render a list of 10,000 items?**
**A:** Use virtualization (react-window or react-virtualized). These libraries render only the visible rows (plus a small buffer), replacing DOM nodes as the user scrolls. Instead of 10,000 DOM nodes, you have ~20-30 at any time.

**One-liner:** Virtualization = render only visible items. 10,000 items but only ~20 DOM nodes. Use react-window or react-virtualized.

---

### 🟢 13.6 React.memo vs useMemo vs useCallback

**Explanation:** These three are often confused. They memoize different things.

```
React.memo    → memoizes a COMPONENT (skips re-render if props unchanged)
useMemo       → memoizes a VALUE (recalculates only when deps change)
useCallback   → memoizes a FUNCTION (same reference unless deps change)
```

```jsx
// React.memo — wraps a component
const MemoChild = React.memo(function Child({ data }) {
  return <p>{data.name}</p>;
});

function Parent() {
  const [count, setCount] = useState(0);

  // useMemo — cache computed value
  const expensiveResult = useMemo(() => {
    return heavyCalculation(count);
  }, [count]);

  // useCallback — cache function reference
  const handleClick = useCallback(() => {
    console.log(count);
  }, [count]);

  return (
    <div>
      <p>{expensiveResult}</p>
      <MemoChild data={{ name: 'Alice' }} onClick={handleClick} />
    </div>
  );
}
```

**Interview Q: Can useMemo replace useCallback?**
**A:** Yes technically. `useCallback(fn, deps)` is identical to `useMemo(() => fn, deps)`. useCallback is just syntactic sugar for memoizing functions specifically.

**One-liner:** memo = skip component re-render. useMemo = cache value. useCallback = cache function. Three tools, three purposes.

---

### 🟢 13.7 Concurrent Features (React 18)

**Explanation:** React 18 introduced concurrent rendering — React can prepare multiple versions of the UI simultaneously and prioritize urgent updates (user typing) over non-urgent ones (search results filtering).

```jsx
import { useTransition, useDeferredValue } from 'react';

// useTransition — mark updates as non-urgent
function SearchPage() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();

  const handleChange = (e) => {
    setQuery(e.target.value);  // URGENT — update input now

    startTransition(() => {
      setResults(filterLargeList(e.target.value));  // NON-URGENT — can wait
    });
  };

  return (
    <div>
      <input value={query} onChange={handleChange} />
      {isPending ? <p>Updating...</p> : <ResultsList results={results} />}
    </div>
  );
}

// useDeferredValue — defer a value to a lower priority
function SearchResults({ query }) {
  const deferredQuery = useDeferredValue(query);
  // deferredQuery lags behind query — React renders the old value
  // while calculating the new one in the background

  const results = useMemo(() => filterLargeList(deferredQuery), [deferredQuery]);

  return <ResultsList results={results} />;
}
```

**Interview Q: What is the difference between useTransition and useDeferredValue?**
**A:** `useTransition` wraps the STATE UPDATE as non-urgent (you control when to trigger it). `useDeferredValue` wraps a VALUE as non-urgent (the value itself lags behind). useTransition = "delay this update." useDeferredValue = "delay this value."

**One-liner:** Concurrent React = prioritize urgent updates. useTransition = defer state update. useDeferredValue = defer a value. UI stays responsive.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 14: Patterns & Architecture

---

### 🔴 14.1 Composition Over Inheritance

**Explanation:** React strongly favors composition (building components from other components) over class inheritance. You combine components using props and children, not by extending base classes.

```jsx
// ✅ COMPOSITION — flexible, reusable
function Dialog({ title, children, footer }) {
  return (
    <div className="dialog">
      <h2>{title}</h2>
      <div className="body">{children}</div>
      {footer && <div className="footer">{footer}</div>}
    </div>
  );
}

// Different dialogs via composition
function ConfirmDialog() {
  return (
    <Dialog title="Are you sure?" footer={<button>Confirm</button>}>
      <p>This action cannot be undone.</p>
    </Dialog>
  );
}

function ErrorDialog({ message }) {
  return (
    <Dialog title="Error">
      <p style={{ color: 'red' }}>{message}</p>
    </Dialog>
  );
}

// ❌ INHERITANCE — rigid, React discourages this
class BaseDialog extends React.Component { /* ... */ }
class ConfirmDialog extends BaseDialog { /* ... */ }  // DON'T do this
```

**Interview Q: Why does React prefer composition over inheritance?**
**A:** Composition is more flexible — you can change behavior by passing different props and children. Inheritance creates rigid hierarchies that are hard to change. The React team says they've never found a use case where inheritance is better than composition.

**One-liner:** Build complex components by COMBINING simple components (composition), not by extending base classes (inheritance).

---

### 🟡 14.2 Custom Hooks for Logic Reuse

**Explanation:** Custom hooks are the primary way to share stateful logic between components. They replace HOCs and render props with a simpler, more composable pattern.

```jsx
// Custom hook: reusable "toggle" logic
function useToggle(initial = false) {
  const [isOn, setIsOn] = useState(initial);
  const toggle = useCallback(() => setIsOn(prev => !prev), []);
  const setOn = useCallback(() => setIsOn(true), []);
  const setOff = useCallback(() => setIsOn(false), []);
  return { isOn, toggle, setOn, setOff };
}

// Custom hook: reusable "local storage" logic
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}

// Usage — any component can use these
function Settings() {
  const { isOn, toggle } = useToggle(false);
  const [name, setName] = useLocalStorage('username', '');

  return (
    <div>
      <p>Dark mode: {isOn ? 'ON' : 'OFF'}</p>
      <button onClick={toggle}>Toggle</button>
      <input value={name} onChange={e => setName(e.target.value)} />
    </div>
  );
}
```

**Interview Q: When should you extract a custom hook?**
**A:** When two or more components share the same stateful logic (fetching, form handling, auth checks, subscriptions, toggle state, window size tracking). Extract the logic into a `useX` function and reuse it.

**One-liner:** Custom hooks = reusable stateful logic in a "use" function. Replaced HOCs and render props as the primary reuse pattern.

---

### 🟡 14.3 Container vs Presentational Components

**Explanation:** A pattern that separates logic from UI. Container components handle data/state/logic. Presentational components only handle display — they receive everything via props.

```jsx
// CONTAINER — handles logic, data fetching, state
function UserListContainer() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch('/api/users')
      .then(res => res.json())
      .then(data => { setUsers(data); setLoading(false); });
  }, []);

  const handleDelete = (id) => {
    setUsers(users.filter(u => u.id !== id));
  };

  // Passes everything down — doesn't render UI itself
  return <UserList users={users} loading={loading} onDelete={handleDelete} />;
}

// PRESENTATIONAL — only renders UI from props
function UserList({ users, loading, onDelete }) {
  if (loading) return <p>Loading...</p>;
  return (
    <ul>
      {users.map(u => (
        <li key={u.id}>
          {u.name} <button onClick={() => onDelete(u.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}
```

**Interview Q: Is this pattern still relevant with hooks?**
**A:** The strict separation is less necessary since custom hooks can extract logic without a container component. But the concept of separating concerns (logic vs UI) is still valuable. Many teams use custom hooks as the "container" and the component as "presentational."

**One-liner:** Container = logic + state. Presentational = pure UI via props. Still useful concept, but custom hooks often replace containers.

---

### 🟡 14.4 Compound Components

**Explanation:** A pattern where a parent component shares state implicitly with specific children. The children don't work outside the parent. Think of `<select>` and `<option>` — option doesn't make sense without select.

```jsx
// Compound component: Tabs
const TabsContext = React.createContext();

function Tabs({ children, defaultTab }) {
  const [activeTab, setActiveTab] = useState(defaultTab);

  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      <div className="tabs">{children}</div>
    </TabsContext.Provider>
  );
}

function TabList({ children }) {
  return <div className="tab-list">{children}</div>;
}

function Tab({ value, children }) {
  const { activeTab, setActiveTab } = useContext(TabsContext);
  return (
    <button
      className={activeTab === value ? 'active' : ''}
      onClick={() => setActiveTab(value)}
    >
      {children}
    </button>
  );
}

function TabPanel({ value, children }) {
  const { activeTab } = useContext(TabsContext);
  return activeTab === value ? <div>{children}</div> : null;
}

// Usage — clean, declarative API
function App() {
  return (
    <Tabs defaultTab="tab1">
      <TabList>
        <Tab value="tab1">Profile</Tab>
        <Tab value="tab2">Settings</Tab>
      </TabList>
      <TabPanel value="tab1"><p>Profile content</p></TabPanel>
      <TabPanel value="tab2"><p>Settings content</p></TabPanel>
    </Tabs>
  );
}
```

**Interview Q: What is the compound components pattern?**
**A:** A set of components that share implicit state via Context. They work together as a group (like `<select>` + `<option>`). The parent manages state, children consume it. Creates clean, declarative APIs for complex UI components.

**One-liner:** Compound components = parent + children share state via Context. Clean API: `<Tabs><Tab>...</Tab></Tabs>`. Like select/option.

---

### 🟡 14.5 Folder Structure

**Explanation:** Two common approaches for organizing React projects.

```
TYPE-BASED (group by file type):
  src/
    components/
      Button.jsx
      Header.jsx
      UserCard.jsx
    hooks/
      useFetch.js
      useAuth.js
    pages/
      Home.jsx
      Dashboard.jsx
    services/
      api.js
    utils/
      format.js

FEATURE-BASED (group by feature — preferred for large apps):
  src/
    features/
      auth/
        LoginForm.jsx
        useAuth.js
        authService.js
        auth.test.js
      users/
        UserList.jsx
        UserCard.jsx
        useUsers.js
        userService.js
      dashboard/
        Dashboard.jsx
        DashboardChart.jsx
        useDashboard.js
    shared/
      components/
        Button.jsx
        Modal.jsx
      hooks/
        useFetch.js
      utils/
        format.js
```

**Interview Q: Type-based vs feature-based — which is better?**
**A:** Feature-based scales better for large apps because related files live together. When working on the "auth" feature, everything is in one folder. Type-based works fine for small apps but becomes hard to navigate in large codebases.

**One-liner:** Small app → type-based (components/, hooks/, pages/). Large app → feature-based (features/auth/, features/users/). Feature-based scales better.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 15: Styling

---

### 🔴 15.1 Inline Styles in React

**Explanation:** React inline styles use a JavaScript object, not a string. Property names are camelCase. Values are strings (or numbers for pixel values).

```jsx
// ❌ HTML style (string)
<div style="color: red; font-size: 16px;">

// ✅ React style (object, camelCase)
<div style={{ color: 'red', fontSize: 16, backgroundColor: '#eee' }}>

// Dynamic styles
function Alert({ type }) {
  const styles = {
    padding: 16,
    borderRadius: 8,
    backgroundColor: type === 'error' ? '#fee' : '#efe',
    color: type === 'error' ? 'red' : 'green',
  };
  return <div style={styles}>Alert message</div>;
}
```

**Interview Q: What are the limitations of inline styles?**
**A:** No pseudo-classes (:hover, :focus), no media queries, no animations/keyframes, no cascading, harder to maintain for complex styling. Good for dynamic styles, bad for static styling.

**One-liner:** React inline style = object with camelCase keys. `style={{ color: 'red' }}`. No hover/media queries — use CSS for complex styling.

---

### 🟡 15.2 CSS Modules

**Explanation:** CSS Modules scope CSS class names locally to the component by auto-generating unique class names at build time. No global naming conflicts.

```css
/* Button.module.css */
.button {
  padding: 10px 20px;
  border-radius: 8px;
}
.primary {
  background: blue;
  color: white;
}
```

```jsx
import styles from './Button.module.css';

function Button({ variant, children }) {
  return (
    <button className={`${styles.button} ${styles[variant]}`}>
      {children}
    </button>
  );
}

// Rendered HTML: <button class="Button_button_x7ht2 Button_primary_k9d3f">
// Unique class names → no conflicts with other components
```

**Interview Q: How do CSS Modules prevent naming conflicts?**
**A:** The build tool (Webpack/Vite) transforms class names into unique hashes like `Button_primary_k9d3f`. Two components can both have a `.button` class without conflict because the actual class names are different.

**One-liner:** CSS Modules = locally scoped CSS. Import as object: `styles.button`. Auto-generated unique class names prevent conflicts.

---

### 🟡 15.3 CSS-in-JS (styled-components)

**Explanation:** CSS-in-JS writes CSS directly in JavaScript files. styled-components is the most popular library. It creates actual React components with styles attached.

```jsx
import styled from 'styled-components';

// Create styled components
const Button = styled.button`
  padding: 10px 20px;
  border-radius: 8px;
  border: none;
  cursor: pointer;
  background: ${props => props.primary ? 'blue' : 'gray'};
  color: white;

  &:hover {
    opacity: 0.8;
  }

  @media (max-width: 768px) {
    width: 100%;
  }
`;

const Title = styled.h1`
  font-size: 2rem;
  color: ${props => props.theme.primary};
`;

// Usage
function App() {
  return (
    <div>
      <Title>Hello</Title>
      <Button primary>Save</Button>
      <Button>Cancel</Button>
    </div>
  );
}
```

**Interview Q: Pros and cons of CSS-in-JS?**

| Pros | Cons |
|---|---|
| Scoped by default (no conflicts) | Runtime overhead (generates CSS at runtime) |
| Dynamic styles based on props | Larger bundle size |
| Co-located with component | Learning curve |
| Full CSS support (hover, media) | Harder to cache separately |
| Theming support built-in | |

**One-liner:** styled-components = write CSS in JS, creates styled React components. Scoped, dynamic, full CSS. But adds runtime cost.

---

### 🟡 15.4 Tailwind CSS with React

**Explanation:** Tailwind is a utility-first CSS framework. Instead of writing custom CSS, you apply pre-built utility classes directly in JSX. Highly popular in the React ecosystem.

```jsx
function Card({ title, description, featured }) {
  return (
    <div className={`
      p-6 rounded-lg shadow-md
      ${featured ? 'bg-blue-100 border-2 border-blue-500' : 'bg-white'}
    `}>
      <h2 className="text-xl font-bold mb-2">{title}</h2>
      <p className="text-gray-600">{description}</p>
      <button className="mt-4 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
        Read More
      </button>
    </div>
  );
}

// For complex conditional classes, use clsx or classnames library
import clsx from 'clsx';

function Button({ primary, disabled, children }) {
  return (
    <button className={clsx(
      'px-4 py-2 rounded font-medium',
      primary ? 'bg-blue-500 text-white' : 'bg-gray-200 text-gray-800',
      disabled && 'opacity-50 cursor-not-allowed'
    )}>
      {children}
    </button>
  );
}
```

**Interview Q: CSS Modules vs Tailwind vs styled-components — when to use each?**
**A:** Tailwind for rapid development and utility-first teams. CSS Modules for traditional CSS fans who want scoping. styled-components for dynamic styles heavily based on props. Team preference matters most — all three are production-ready.

**One-liner:** Tailwind = utility classes in JSX (p-4, bg-blue-500). Fast to build. Use clsx for conditional classes.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 16: Testing

---

### 🔴 16.1 React Testing Library Basics

**Explanation:** React Testing Library (RTL) tests components the way users interact with them — by text, role, label — not by implementation details (state, props, class names).

```jsx
// Component to test
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

// Test file: Counter.test.jsx
import { render, screen, fireEvent } from '@testing-library/react';

test('renders initial count of 0', () => {
  render(<Counter />);
  expect(screen.getByText('Count: 0')).toBeInTheDocument();
});

test('increments count when button is clicked', () => {
  render(<Counter />);
  const button = screen.getByText('Increment');

  fireEvent.click(button);

  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

**Interview Q: Why use React Testing Library instead of Enzyme?**
**A:** RTL tests USER BEHAVIOR (what the user sees and does). Enzyme tests IMPLEMENTATION DETAILS (component state, methods). RTL's philosophy: "The more your tests resemble the way your software is used, the more confidence they give you."

**Common Mistake:** Testing implementation details like `expect(component.state().count).toBe(1)`. Test what the user sees: `expect(screen.getByText('Count: 1'))`.

**One-liner:** RTL = test what users see (getByText, getByRole), not internals. render → find → interact → assert.

---

### 🔴 16.2 Testing User Interactions

**Explanation:** Use `fireEvent` for basic events or `userEvent` (more realistic) to simulate clicks, typing, and form submissions.

```jsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

function LoginForm({ onSubmit }) {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  return (
    <form onSubmit={(e) => { e.preventDefault(); onSubmit({ email, password }); }}>
      <label htmlFor="email">Email</label>
      <input id="email" value={email} onChange={e => setEmail(e.target.value)} />

      <label htmlFor="password">Password</label>
      <input id="password" type="password" value={password}
             onChange={e => setPassword(e.target.value)} />

      <button type="submit">Login</button>
    </form>
  );
}

// Test
test('submits form with email and password', async () => {
  const handleSubmit = jest.fn();
  render(<LoginForm onSubmit={handleSubmit} />);

  // Type into inputs (userEvent is more realistic than fireEvent)
  await userEvent.type(screen.getByLabelText('Email'), 'alice@test.com');
  await userEvent.type(screen.getByLabelText('Password'), 'secret123');

  // Click submit
  await userEvent.click(screen.getByText('Login'));

  // Assert
  expect(handleSubmit).toHaveBeenCalledWith({
    email: 'alice@test.com',
    password: 'secret123',
  });
});
```

**Interview Q: What is the difference between fireEvent and userEvent?**
**A:** `fireEvent` dispatches a single DOM event. `userEvent` simulates full user interactions (typing fires keyDown, keyPress, keyUp, input, change for each character). userEvent is more realistic and catches more bugs.

**One-liner:** userEvent > fireEvent. userEvent simulates real user behavior (typing = multiple events per keystroke). More realistic tests.

---

### 🟡 16.3 Testing Async Operations

**Explanation:** For components that fetch data or have delayed updates, use `waitFor` or `findBy` queries that wait for elements to appear.

```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(data => setUser(data));
  }, [userId]);

  if (!user) return <p>Loading...</p>;
  return <h1>{user.name}</h1>;
}

// Test with mocked fetch
test('displays user name after loading', async () => {
  // Mock the API call
  global.fetch = jest.fn(() =>
    Promise.resolve({
      json: () => Promise.resolve({ name: 'Alice' }),
    })
  );

  render(<UserProfile userId={1} />);

  // Initially shows loading
  expect(screen.getByText('Loading...')).toBeInTheDocument();

  // Wait for the async update — findByText waits automatically
  const heading = await screen.findByText('Alice');
  expect(heading).toBeInTheDocument();

  // Cleanup
  global.fetch.mockRestore();
});
```

**Interview Q: What is the difference between getBy, queryBy, and findBy?**

| Method | Returns | Throws? | Async? | Use When |
|---|---|---|---|---|
| getBy | Element | Yes (if not found) | No | Element should be present |
| queryBy | Element or null | No | No | Element might not be present |
| findBy | Promise<Element> | Yes (timeout) | Yes | Element will appear after async |

**One-liner:** getBy = must exist now. queryBy = might not exist (returns null). findBy = will appear later (async, waits).

---

### 🟡 16.4 Mocking API Calls

**Explanation:** Never call real APIs in tests. Mock them with jest.fn() or libraries like MSW (Mock Service Worker).

```jsx
// Method 1: Jest mock
test('fetches and displays data', async () => {
  jest.spyOn(global, 'fetch').mockResolvedValue({
    json: async () => [{ id: 1, name: 'Alice' }],
  });

  render(<UserList />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();

  global.fetch.mockRestore();
});

// Method 2: MSW (Mock Service Worker) — recommended for complex apps
import { rest } from 'msw';
import { setupServer } from 'msw/node';

const server = setupServer(
  rest.get('/api/users', (req, res, ctx) => {
    return res(ctx.json([{ id: 1, name: 'Alice' }]));
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

test('displays users', async () => {
  render(<UserList />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();
});
```

**Interview Q: Why use MSW over jest.fn() for API mocking?**
**A:** MSW intercepts actual network requests at the service worker level, so your component code is tested exactly as it runs in production. jest.fn() requires you to know and mock the exact fetch call, which is more brittle.

**One-liner:** Never call real APIs in tests. Use jest.spyOn(global, 'fetch') for simple cases, MSW for realistic API mocking.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 17: React Internals

---

### 🔴 17.1 How Diffing Works

**Explanation:** React's diffing algorithm compares two Virtual DOM trees with O(n) complexity using two heuristics: (1) different element types produce different trees, (2) keys identify which items in a list moved.

```
RULE 1: Different type → tear down old tree, build new tree
  <div> → <span>    // React destroys <div> and its children, creates <span>

RULE 2: Same type → compare attributes, update changed ones
  <div className="old"> → <div className="new">
  // React only updates className, keeps the DOM node

RULE 3: Lists — use keys to match items
  Before: [A, B, C]
  After:  [A, C, B]
  With keys: React knows B and C just swapped → moves DOM nodes
  Without keys: React re-renders B as C and C as B → wasteful
```

**Interview Q: Why is React's diffing O(n) instead of O(n³)?**
**A:** Generic tree diffing is O(n³). React reduces this to O(n) by assuming: (1) elements of different types produce completely different trees (skip deep comparison), and (2) keys help identify stable items in lists. These heuristics work well in practice because UI updates are usually small and localized.

**One-liner:** Diffing = compare old/new Virtual DOM. Different type → rebuild. Same type → update attributes. Keys match list items. O(n) via heuristics.

---

### 🟡 17.2 Batching (React 17 vs React 18)

**Explanation:** Batching combines multiple state updates into one re-render for performance. React 17 only batched inside event handlers. React 18 batches everywhere (promises, setTimeout, native events too).

```jsx
function Example() {
  const [count, setCount] = useState(0);
  const [flag, setFlag] = useState(false);

  // REACT 17: batched (inside event handler)
  const handleClick = () => {
    setCount(c => c + 1);
    setFlag(f => !f);
    // ONE re-render (batched ✓)
  };

  // REACT 17: NOT batched (inside setTimeout/promise)
  // REACT 18: ALSO batched (automatic batching everywhere)
  const handleAsync = () => {
    fetch('/api').then(() => {
      setCount(c => c + 1);
      setFlag(f => !f);
      // React 17: TWO re-renders ✗
      // React 18: ONE re-render ✓ (auto-batched)
    });
  };

  // React 18: Opt OUT of batching (rare)
  import { flushSync } from 'react-dom';
  const handleFlush = () => {
    flushSync(() => setCount(c => c + 1)); // re-renders immediately
    flushSync(() => setFlag(f => !f));      // re-renders immediately
    // TWO separate re-renders (batching bypassed)
  };
}
```

**Interview Q: What changed with batching in React 18?**
**A:** React 17 only batched state updates inside React event handlers. In setTimeout, promises, and native event handlers, each setState caused a separate re-render. React 18 automatically batches ALL updates everywhere by default, improving performance.

**One-liner:** React 17: batches only in event handlers. React 18: batches everywhere (promises, timeouts, etc.) automatically. Use flushSync to opt out.

---

### 🟡 17.3 Concurrent Rendering

**Explanation:** Before React 18, rendering was synchronous — once started, it couldn't be interrupted. Concurrent rendering lets React pause rendering work, handle urgent updates (user input), then resume. The UI stays responsive even during heavy updates.

```
SYNCHRONOUS (React 17):
  Start rendering heavy list → BLOCKS → user types → nothing happens → finishes → input updates
  User experience: UI feels frozen

CONCURRENT (React 18):
  Start rendering heavy list → user types → PAUSE list → update input → RESUME list
  User experience: Input responds instantly, list updates in background
```

**Interview Q: Is concurrent rendering automatic in React 18?**
**A:** The concurrent engine is always on in React 18, but concurrent FEATURES are opt-in. You must use `useTransition`, `useDeferredValue`, or `<Suspense>` to trigger concurrent behavior. Without these, React 18 behaves like React 17 (synchronous rendering).

**One-liner:** Concurrent rendering = React can pause/resume rendering. Urgent updates (typing) go first, heavy updates wait. Opt-in via useTransition/useDeferredValue.

---

### 🟢 17.4 React Server Components (RSC)

**Explanation:** Server Components render on the server and send only HTML to the client — zero JavaScript for that component. They can directly access databases, file systems, and APIs without exposing them to the client.

```jsx
// Server Component (runs on server only, no JS shipped to client)
// Filename: UserList.server.jsx (or in Next.js App Router, default is server)
async function UserList() {
  // Direct database access — impossible in client components
  const users = await db.query('SELECT * FROM users');

  return (
    <ul>
      {users.map(u => <li key={u.id}>{u.name}</li>)}
    </ul>
  );
}

// Client Component (runs in browser, has interactivity)
'use client';  // Next.js directive to mark as client component

function SearchInput() {
  const [query, setQuery] = useState('');  // useState = needs client JS
  return <input value={query} onChange={e => setQuery(e.target.value)} />;
}

// Mix: Server component renders client component
async function Page() {
  const users = await getUsers();  // server-side
  return (
    <div>
      <SearchInput />              {/* client component — interactive */}
      <UserList users={users} />   {/* server component — no JS shipped */}
    </div>
  );
}
```

**Interview Q: What's the difference between Server Components and SSR?**
**A:** SSR renders the full page to HTML on the server, then the client downloads ALL JavaScript and "hydrates" (attaches event listeners). Server Components render specific components on the server and ship ZERO JavaScript for them. The client only downloads JS for interactive (client) components.

**One-liner:** Server Components = render on server, zero JS shipped. Can access DB directly. Client Components = interactive, have hooks. Mix both.

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Category 18: Machine Coding

> "Build this component in 30-45 minutes" — the most practical interview round.
> Full working code for the most commonly asked challenges.

---

### 🔴 18.1 Todo App

```jsx
function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState('');
  const [editId, setEditId] = useState(null);
  const [editText, setEditText] = useState('');

  const addTodo = () => {
    if (!input.trim()) return;
    setTodos([...todos, { id: Date.now(), text: input, done: false }]);
    setInput('');
  };

  const deleteTodo = (id) => {
    setTodos(todos.filter(t => t.id !== id));
  };

  const toggleDone = (id) => {
    setTodos(todos.map(t => t.id === id ? { ...t, done: !t.done } : t));
  };

  const startEdit = (todo) => {
    setEditId(todo.id);
    setEditText(todo.text);
  };

  const saveEdit = () => {
    setTodos(todos.map(t => t.id === editId ? { ...t, text: editText } : t));
    setEditId(null);
    setEditText('');
  };

  return (
    <div>
      <h1>Todo App</h1>
      <div>
        <input
          value={input}
          onChange={e => setInput(e.target.value)}
          onKeyDown={e => e.key === 'Enter' && addTodo()}
          placeholder="Add a todo..."
        />
        <button onClick={addTodo}>Add</button>
      </div>

      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            {editId === todo.id ? (
              <>
                <input value={editText} onChange={e => setEditText(e.target.value)} />
                <button onClick={saveEdit}>Save</button>
                <button onClick={() => setEditId(null)}>Cancel</button>
              </>
            ) : (
              <>
                <span
                  style={{ textDecoration: todo.done ? 'line-through' : 'none' }}
                  onClick={() => toggleDone(todo.id)}
                >
                  {todo.text}
                </span>
                <button onClick={() => startEdit(todo)}>Edit</button>
                <button onClick={() => deleteTodo(todo.id)}>Delete</button>
              </>
            )}
          </li>
        ))}
      </ul>

      <p>{todos.filter(t => !t.done).length} remaining</p>
    </div>
  );
}
```

**What interviewers evaluate:** State management, immutable updates, conditional rendering, list rendering with keys, event handling, controlled inputs.

---

### 🔴 18.2 Debounced Search Input

```jsx
function DebouncedSearch() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  const [loading, setLoading] = useState(false);

  // Debounced fetch — only fires 300ms after user stops typing
  useEffect(() => {
    if (!query.trim()) {
      setResults([]);
      return;
    }

    setLoading(true);
    const timeoutId = setTimeout(() => {
      fetch(`https://api.example.com/search?q=${query}`)
        .then(res => res.json())
        .then(data => {
          setResults(data);
          setLoading(false);
        })
        .catch(() => setLoading(false));
    }, 300);

    // Cleanup: cancel the timeout if query changes before 300ms
    return () => clearTimeout(timeoutId);
  }, [query]);

  return (
    <div>
      <input
        value={query}
        onChange={e => setQuery(e.target.value)}
        placeholder="Search..."
      />
      {loading && <p>Searching...</p>}
      <ul>
        {results.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

**What interviewers evaluate:** useEffect cleanup for debouncing, controlled input, loading states, API handling.

---

### 🟡 18.3 Tabs Component

```jsx
function Tabs({ tabs, defaultTab = 0 }) {
  const [activeIndex, setActiveIndex] = useState(defaultTab);

  return (
    <div>
      <div role="tablist" style={{ display: 'flex', gap: 0, borderBottom: '2px solid #ddd' }}>
        {tabs.map((tab, index) => (
          <button
            key={index}
            role="tab"
            aria-selected={index === activeIndex}
            onClick={() => setActiveIndex(index)}
            style={{
              padding: '10px 20px',
              border: 'none',
              borderBottom: index === activeIndex ? '2px solid blue' : '2px solid transparent',
              background: 'none',
              fontWeight: index === activeIndex ? 'bold' : 'normal',
              cursor: 'pointer',
            }}
          >
            {tab.label}
          </button>
        ))}
      </div>
      <div role="tabpanel" style={{ padding: 16 }}>
        {tabs[activeIndex].content}
      </div>
    </div>
  );
}

// Usage
function App() {
  const tabs = [
    { label: 'Profile', content: <p>Profile content here</p> },
    { label: 'Settings', content: <p>Settings content here</p> },
    { label: 'Notifications', content: <p>Notifications content here</p> },
  ];

  return <Tabs tabs={tabs} defaultTab={0} />;
}
```

**What interviewers evaluate:** Dynamic rendering, active state management, accessibility (ARIA roles), clean prop API design.

---

### 🟡 18.4 Modal / Dialog

```jsx
function Modal({ isOpen, onClose, title, children }) {
  // Close on Escape key
  useEffect(() => {
    const handleEsc = (e) => {
      if (e.key === 'Escape') onClose();
    };
    if (isOpen) document.addEventListener('keydown', handleEsc);
    return () => document.removeEventListener('keydown', handleEsc);
  }, [isOpen, onClose]);

  // Prevent body scroll when modal is open
  useEffect(() => {
    if (isOpen) document.body.style.overflow = 'hidden';
    else document.body.style.overflow = '';
    return () => { document.body.style.overflow = ''; };
  }, [isOpen]);

  if (!isOpen) return null;

  return ReactDOM.createPortal(
    <div
      style={{
        position: 'fixed', top: 0, left: 0, right: 0, bottom: 0,
        backgroundColor: 'rgba(0,0,0,0.5)',
        display: 'flex', alignItems: 'center', justifyContent: 'center',
      }}
      onClick={onClose}  // close on backdrop click
    >
      <div
        style={{
          background: 'white', borderRadius: 8, padding: 24,
          minWidth: 300, maxWidth: 500,
        }}
        onClick={e => e.stopPropagation()}  // prevent closing on content click
      >
        <div style={{ display: 'flex', justifyContent: 'space-between' }}>
          <h2>{title}</h2>
          <button onClick={onClose} style={{ border: 'none', fontSize: 20, cursor: 'pointer' }}>
            ×
          </button>
        </div>
        <div>{children}</div>
      </div>
    </div>,
    document.body  // Portal renders outside the component tree
  );
}

// Usage
function App() {
  const [showModal, setShowModal] = useState(false);
  return (
    <div>
      <button onClick={() => setShowModal(true)}>Open Modal</button>
      <Modal isOpen={showModal} onClose={() => setShowModal(false)} title="Confirm">
        <p>Are you sure?</p>
        <button onClick={() => setShowModal(false)}>Yes</button>
      </Modal>
    </div>
  );
}
```

**What interviewers evaluate:** Portal usage, keyboard handling (Escape), backdrop click, preventing event propagation, body scroll lock, cleanup.

---

### 🟡 18.5 Star Rating Component

```jsx
function StarRating({ totalStars = 5, value = 0, onChange }) {
  const [hovered, setHovered] = useState(0);

  return (
    <div style={{ display: 'flex', gap: 4, cursor: 'pointer' }}>
      {Array.from({ length: totalStars }, (_, index) => {
        const starValue = index + 1;
        const isFilled = starValue <= (hovered || value);

        return (
          <span
            key={index}
            style={{ fontSize: 32, color: isFilled ? '#ffc107' : '#e0e0e0' }}
            onClick={() => onChange(starValue)}
            onMouseEnter={() => setHovered(starValue)}
            onMouseLeave={() => setHovered(0)}
          >
            ★
          </span>
        );
      })}
      <span style={{ marginLeft: 8 }}>{value} / {totalStars}</span>
    </div>
  );
}

// Usage
function App() {
  const [rating, setRating] = useState(0);
  return <StarRating value={rating} onChange={setRating} />;
}
```

**What interviewers evaluate:** Controlled component pattern, hover state separate from selected state, Array.from for dynamic rendering.

---

### 🟡 18.6 Dark Mode Toggle with Context

```jsx
const ThemeContext = React.createContext();

function ThemeProvider({ children }) {
  const [isDark, setIsDark] = useState(false);
  const toggle = useCallback(() => setIsDark(prev => !prev), []);

  const theme = useMemo(() => ({
    isDark,
    toggle,
    colors: {
      bg: isDark ? '#1a1a1a' : '#ffffff',
      text: isDark ? '#ffffff' : '#1a1a1a',
      primary: isDark ? '#4dabf7' : '#1971c2',
    },
  }), [isDark, toggle]);

  return (
    <ThemeContext.Provider value={theme}>
      <div style={{ background: theme.colors.bg, color: theme.colors.text, minHeight: '100vh' }}>
        {children}
      </div>
    </ThemeContext.Provider>
  );
}

function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) throw new Error('useTheme must be used within ThemeProvider');
  return context;
}

// Usage
function Header() {
  const { isDark, toggle } = useTheme();
  return (
    <header style={{ padding: 16 }}>
      <button onClick={toggle}>
        {isDark ? '☀️ Light Mode' : '🌙 Dark Mode'}
      </button>
    </header>
  );
}

function App() {
  return (
    <ThemeProvider>
      <Header />
      <main style={{ padding: 16 }}>
        <h1>Hello World</h1>
        <p>This respects the theme automatically.</p>
      </main>
    </ThemeProvider>
  );
}
```

**What interviewers evaluate:** Context API full flow (create, provide, consume), custom hook with error boundary, useMemo for performance, theming pattern.

---

### 🟡 18.7 Infinite Scroll

```jsx
function InfiniteScrollList() {
  const [items, setItems] = useState([]);
  const [page, setPage] = useState(1);
  const [loading, setLoading] = useState(false);
  const [hasMore, setHasMore] = useState(true);
  const observerRef = useRef();
  const lastItemRef = useRef();

  // Fetch data
  const fetchItems = useCallback(async (pageNum) => {
    setLoading(true);
    const res = await fetch(`/api/items?page=${pageNum}&limit=20`);
    const data = await res.json();

    setItems(prev => [...prev, ...data.items]);
    setHasMore(data.items.length === 20);
    setLoading(false);
  }, []);

  useEffect(() => {
    fetchItems(page);
  }, [page, fetchItems]);

  // Intersection Observer — detect when last item is visible
  useEffect(() => {
    if (loading) return;

    // Disconnect previous observer
    if (observerRef.current) observerRef.current.disconnect();

    observerRef.current = new IntersectionObserver(entries => {
      if (entries[0].isIntersecting && hasMore) {
        setPage(prev => prev + 1);  // trigger next page fetch
      }
    });

    if (lastItemRef.current) {
      observerRef.current.observe(lastItemRef.current);
    }

    return () => observerRef.current?.disconnect();
  }, [loading, hasMore]);

  return (
    <div>
      {items.map((item, index) => (
        <div
          key={item.id}
          ref={index === items.length - 1 ? lastItemRef : null}
          style={{ padding: 16, borderBottom: '1px solid #eee' }}
        >
          {item.name}
        </div>
      ))}
      {loading && <p>Loading more...</p>}
      {!hasMore && <p>No more items</p>}
    </div>
  );
}
```

**What interviewers evaluate:** IntersectionObserver API, ref management, pagination, cleanup, loading/end states.

---

### 🔴 18.8 Shopping Cart

```jsx
function ShoppingCart() {
  const [cart, setCart] = useState([]);

  const products = [
    { id: 1, name: 'Laptop', price: 999 },
    { id: 2, name: 'Phone', price: 699 },
    { id: 3, name: 'Headphones', price: 199 },
  ];

  const addToCart = (product) => {
    setCart(prev => {
      const existing = prev.find(item => item.id === product.id);
      if (existing) {
        return prev.map(item =>
          item.id === product.id ? { ...item, qty: item.qty + 1 } : item
        );
      }
      return [...prev, { ...product, qty: 1 }];
    });
  };

  const removeFromCart = (id) => {
    setCart(prev => prev.filter(item => item.id !== id));
  };

  const updateQty = (id, delta) => {
    setCart(prev => prev
      .map(item => item.id === id ? { ...item, qty: item.qty + delta } : item)
      .filter(item => item.qty > 0)
    );
  };

  const total = cart.reduce((sum, item) => sum + item.price * item.qty, 0);
  const totalItems = cart.reduce((sum, item) => sum + item.qty, 0);

  return (
    <div style={{ display: 'flex', gap: 32 }}>
      {/* Products */}
      <div>
        <h2>Products</h2>
        {products.map(p => (
          <div key={p.id} style={{ padding: 8, borderBottom: '1px solid #eee' }}>
            <span>{p.name} — ${p.price}</span>
            <button onClick={() => addToCart(p)} style={{ marginLeft: 8 }}>
              Add to Cart
            </button>
          </div>
        ))}
      </div>

      {/* Cart */}
      <div>
        <h2>Cart ({totalItems} items)</h2>
        {cart.length === 0 ? (
          <p>Cart is empty</p>
        ) : (
          <>
            {cart.map(item => (
              <div key={item.id} style={{ padding: 8, borderBottom: '1px solid #eee' }}>
                <span>{item.name}</span>
                <button onClick={() => updateQty(item.id, -1)}>−</button>
                <span style={{ margin: '0 8px' }}>{item.qty}</span>
                <button onClick={() => updateQty(item.id, 1)}>+</button>
                <span style={{ marginLeft: 8 }}>${item.price * item.qty}</span>
                <button onClick={() => removeFromCart(item.id)} style={{ marginLeft: 8 }}>
                  Remove
                </button>
              </div>
            ))}
            <h3>Total: ${total}</h3>
          </>
        )}
      </div>
    </div>
  );
}
```

**What interviewers evaluate:** Complex state updates, immutable array operations (add, update, filter), derived state (total, count), handling edge cases (quantity 0 removes item, duplicate adds increase quantity).

---

[↑ Back to Table of Contents](#table-of-contents--part-3)

---

# Quick Revision — Part 3 One-Liners

```
PERFORMANCE OPTIMIZATION
  React.memo = skip re-render if props unchanged. Pair with useMemo/useCallback
  React.lazy + Suspense = load component on demand (code splitting)
  Immutability = new reference → React detects change via === → efficient diffing
  Causes of re-renders: parent renders, new object/function refs, Context changes
  Virtualization = render only visible items. react-window for large lists
  memo = cache component. useMemo = cache value. useCallback = cache function
  useTransition = defer heavy updates. useDeferredValue = defer a value

PATTERNS & ARCHITECTURE
  Composition > Inheritance. Build from small components, not class hierarchies
  Custom hooks = primary reuse pattern. Replaced HOCs and render props
  Container = logic. Presentational = UI. Or just use custom hooks for logic
  Compound components = parent + children share state via Context
  Small app → type-based folders. Large app → feature-based folders

STYLING
  Inline: style={{ camelCase }}. No hover/media queries
  CSS Modules: scoped, import styles from './X.module.css'
  styled-components: CSS-in-JS, dynamic props, full CSS. Runtime overhead
  Tailwind: utility classes in JSX. Fast. Use clsx for conditionals

TESTING
  RTL: test user behavior, not internals. render → find → interact → assert
  userEvent > fireEvent (more realistic interactions)
  getBy = must exist. queryBy = might not. findBy = will appear (async)
  Mock APIs with jest.spyOn or MSW. Never call real APIs in tests

REACT INTERNALS
  Diffing: different type → rebuild. Same type → update attrs. Keys for lists
  React 18: auto-batching everywhere (not just event handlers)
  Concurrent rendering: pause/resume. useTransition, useDeferredValue opt in
  Server Components: render on server, zero JS shipped. Can access DB directly

MACHINE CODING
  Todo: add, delete, edit, toggle done. Immutable state updates
  Debounced search: useEffect cleanup with setTimeout
  Modal: Portal, Escape key, backdrop click, stopPropagation, body scroll lock
  Shopping cart: add (handle duplicates), quantity +/-, remove, computed total
  Infinite scroll: IntersectionObserver + pagination
  Star rating: hover state separate from selected state
  Dark mode: Context + Provider + useTheme custom hook
```

[↑ Back to Table of Contents](#table-of-contents--part-3)
