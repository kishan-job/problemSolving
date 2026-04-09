# React.js Complete Interview Guide
## Part 1: Categories 1-6 (Foundation)

---

# Table of Contents — Part 1

- [Category 1: React Fundamentals](#category-1-react-fundamentals)
- [Category 2: JSX](#category-2-jsx)
- [Category 3: Components](#category-3-components)
- [Category 4: Props](#category-4-props)
- [Category 5: State](#category-5-state)
- [Category 6: Hooks](#category-6-hooks)

---

# Category 1: React Fundamentals

---

### 🔴 1.1 What is React? Why use it?

**Explanation:** React is an open-source JavaScript library built by Facebook for building user interfaces. It uses a component-based architecture where you build small, reusable pieces of UI and compose them together.

```jsx
// React lets you build UI from components
function App() {
  return (
    <div>
      <Header />
      <Content />
      <Footer />
    </div>
  );
}
```

**Interview Q: Why would you choose React over vanilla JavaScript?**
**A:** Component reusability, Virtual DOM for performance, declarative syntax (describe WHAT you want, not HOW), large ecosystem, and one-way data flow for predictable state management.

**Common Mistake:** Saying React is a "framework." It's a library — it handles only the view layer. You need separate tools for routing, state management, etc.

**One-liner:** React is a UI library that lets you build interfaces from reusable components with efficient updates via Virtual DOM.

---

### 🔴 1.2 Virtual DOM — What it is, how it works

**Explanation:** The Virtual DOM is a lightweight JavaScript copy of the real DOM. When state changes, React creates a new Virtual DOM tree, compares it with the previous one (diffing), and updates only the changed parts in the real DOM (patching).

```
State changes → New Virtual DOM created
→ Diff with old Virtual DOM (find changes)
→ Patch only changed nodes in Real DOM
```

**Interview Q: Why is Virtual DOM faster than directly updating the Real DOM?**
**A:** Direct DOM manipulation is expensive because the browser must recalculate layout, styles, and repaint. Virtual DOM batches changes and updates only what's necessary in a single operation, minimizing browser reflows and repaints.

**Common Mistake:** Saying "Virtual DOM is faster than Real DOM." The Virtual DOM isn't faster — the STRATEGY of batching minimal updates is faster than manually updating every DOM node.

**One-liner:** Virtual DOM is a JS copy of the real DOM — React diffs old vs new, then patches only the changes.

---

### 🔴 1.3 Real DOM vs Virtual DOM

**Explanation:** Real DOM is the actual browser document. It's slow to update because any change can trigger reflow and repaint. Virtual DOM is a plain JS object representation that React uses for efficient diffing.

| | Real DOM | Virtual DOM |
|---|---|---|
| What | Browser's actual document | JS object copy |
| Update speed | Slow (reflow + repaint) | Fast (just JS objects) |
| Update method | Direct manipulation | Diff → patch only changes |
| Memory | Heavy | Lightweight |

**Interview Q: Does React ever touch the Real DOM?**
**A:** Yes. After diffing the Virtual DOM, React applies the minimal set of changes to the Real DOM. React doesn't eliminate Real DOM updates — it minimizes them.

**Common Mistake:** Thinking React never uses the Real DOM. It always updates the Real DOM — just intelligently.

**One-liner:** Real DOM = browser's actual UI (slow to change). Virtual DOM = React's JS blueprint for smart, minimal updates.

---

### 🔴 1.4 Reconciliation

**Explanation:** Reconciliation is React's algorithm for determining which parts of the UI need to change. It compares two Virtual DOM trees and figures out the minimum number of operations to update the Real DOM.

```
Two key rules React uses:
1. Different element types → destroy old tree, build new tree
2. Same element type → compare attributes, update only what changed
3. Keys → help React identify which list items moved/added/removed
```

**Interview Q: What role does the key prop play in reconciliation?**
**A:** Keys help React identify which items in a list have changed, been added, or removed. Without keys (or with index as key), React can't efficiently reorder elements and may re-render the entire list.

**Common Mistake:** Not knowing that reconciliation is O(n) complexity thanks to heuristics — not the theoretical O(n³) of generic tree diffing.

**One-liner:** Reconciliation = React's diffing algorithm that compares old and new Virtual DOM to find the minimum updates needed.

---

### 🔴 1.5 React Fiber

**Explanation:** Fiber is React's internal reconciliation engine (introduced in React 16). It breaks rendering work into small units called "fibers" that can be paused, resumed, and prioritized — enabling concurrent rendering.

```
Before Fiber (React 15):
  Rendering was synchronous → large updates blocked the main thread → UI froze

After Fiber (React 16+):
  Rendering is broken into chunks → high-priority updates (user input) go first
  → low-priority updates (data fetching) happen when idle
```

**Interview Q: What problem does React Fiber solve?**
**A:** In React 15, rendering was synchronous — a large component tree update would block the browser from responding to user input. Fiber enables interruptible rendering, so React can pause work, handle urgent updates (like typing), then resume.

**Common Mistake:** Thinking Fiber is a separate feature you import or use. It's an internal architecture change — you benefit from it automatically.

**One-liner:** Fiber = React's internal engine that breaks rendering into small chunks so the UI stays responsive.

---

### 🔴 1.6 One-way Data Flow

**Explanation:** In React, data flows in one direction: parent → child via props. A child cannot directly modify its parent's data. If a child needs to communicate up, the parent passes a callback function as a prop.

```jsx
// Parent → passes data DOWN via props
function Parent() {
  const [count, setCount] = useState(0);
  return <Child count={count} onIncrement={() => setCount(count + 1)} />;
}

// Child → reads props, calls parent's function to communicate UP
function Child({ count, onIncrement }) {
  return <button onClick={onIncrement}>Count: {count}</button>;
}
```

**Interview Q: Why does React use one-way data flow?**
**A:** It makes data predictable and easier to debug. You always know where data comes from (the parent) and where it changes (the state owner). Two-way binding (like Angular's ngModel) can cause unexpected cascading updates.

**Common Mistake:** Trying to modify props inside a child component. Props are read-only.

**One-liner:** Data flows parent → child via props. Child communicates back via callback functions. Never the other way around.

---

### 🔴 1.7 Declarative vs Imperative

**Explanation:** Imperative = you tell the computer HOW to do it step by step. Declarative = you describe WHAT you want, and the library figures out how. React is declarative.

```javascript
// IMPERATIVE (vanilla JS): step-by-step DOM manipulation
const el = document.createElement('h1');
el.textContent = 'Hello';
el.className = 'greeting';
document.body.appendChild(el);

// DECLARATIVE (React): describe what you want
function App() {
  return <h1 className="greeting">Hello</h1>;
}
// React handles the "how" (creating DOM elements, updating, etc.)
```

**Interview Q: Why is declarative better for UI?**
**A:** You describe the final state of the UI, and React handles updates. You don't manually track what changed — when state updates, React re-renders the component and efficiently updates only what's different.

**Common Mistake:** Mixing imperative DOM manipulation (document.querySelector) with React. Use refs instead.

**One-liner:** Declarative = describe WHAT you want. Imperative = describe HOW to do it. React is declarative.

---

### 🟡 1.8 React vs Angular vs Vue

| | React | Angular | Vue |
|---|---|---|---|
| Type | Library | Framework | Framework |
| Language | JavaScript/JSX | TypeScript | JavaScript/HTML templates |
| DOM | Virtual DOM | Real DOM (with change detection) | Virtual DOM |
| Data flow | One-way | Two-way | Both |
| Learning curve | Medium | Steep | Easy |
| Size | ~42KB | ~143KB | ~33KB |
| Created by | Meta (Facebook) | Google | Evan You |

**Interview Q: When would you choose React over Angular?**
**A:** React for flexibility and when you want to choose your own tools (routing, state management). Angular when you want an all-in-one opinionated framework with built-in solutions for everything.

**One-liner:** React = flexible library (you choose tools). Angular = full framework (everything built-in). Vue = middle ground.

---

### 🟡 1.9 Library vs Framework

**Explanation:** A library provides tools YOU call when needed (React). A framework calls YOUR code within its structure (Angular). This is called "Inversion of Control."

**Interview Q: Why is React called a library, not a framework?**
**A:** React only handles the view layer (rendering UI). It doesn't provide routing, HTTP calls, form handling, or state management out of the box. You pick separate libraries for each. Frameworks like Angular provide all of these built-in.

**One-liner:** Library = you call it. Framework = it calls you. React is a library — it handles only the view layer.

---

### 🟡 1.10 SPA (Single Page Application)

**Explanation:** An SPA loads a single HTML page and dynamically updates content using JavaScript without full page reloads. React is commonly used to build SPAs with client-side routing.

**Interview Q: What are the pros and cons of SPAs?**
**A:** Pros: fast navigation (no page reload), smooth UX, easy to build rich interactions. Cons: poor initial load time (large JS bundle), SEO challenges (content rendered client-side), browser back button can be tricky.

**One-liner:** SPA = one HTML page, JS handles navigation. No full page reloads. Fast UX but needs attention for SEO and initial load.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 2: JSX

---

### 🔴 2.1 What is JSX?

**Explanation:** JSX (JavaScript XML) is a syntax extension that lets you write HTML-like code inside JavaScript. It's not actual HTML — it gets compiled to `React.createElement()` calls by tools like Babel.

```jsx
// What you write (JSX):
const element = <h1 className="title">Hello World</h1>;

// What React sees after compilation:
const element = React.createElement('h1', { className: 'title' }, 'Hello World');
```

**Interview Q: Is JSX required to use React?**
**A:** No. You can use `React.createElement()` directly. But JSX is preferred because it's more readable and looks like the UI you're building.

**Common Mistake:** Thinking JSX IS HTML. It's not — it's syntactic sugar for `React.createElement()`.

**One-liner:** JSX = HTML-like syntax in JavaScript that compiles to React.createElement() calls.

---

### 🔴 2.2 JSX is NOT HTML — Key Differences

**Explanation:** JSX looks like HTML but has important differences. Since it's JavaScript, some HTML attributes have different names.

```jsx
// HTML vs JSX differences:
// class      → className
// for        → htmlFor
// onclick    → onClick (camelCase)
// tabindex   → tabIndex
// style=""   → style={{}} (object, not string)

// HTML:
<label for="name" class="label" style="color: red;">Name</label>

// JSX:
<label htmlFor="name" className="label" style={{ color: 'red' }}>Name</label>
```

**Interview Q: Why does JSX use className instead of class?**
**A:** Because `class` is a reserved word in JavaScript (used for class declarations). Since JSX is JavaScript, it uses `className` to avoid conflicts.

**Common Mistake:** Writing `class` instead of `className`. It might work but shows React warnings.

**One-liner:** JSX uses className (not class), htmlFor (not for), camelCase events, and style as objects.

---

### 🔴 2.3 How JSX Gets Compiled

**Explanation:** Babel transpiles JSX into `React.createElement(type, props, ...children)`. This is why you needed to import React in every file before React 17. After React 17, the new JSX transform handles it automatically.

```jsx
// JSX:
<div id="box">
  <span>Hello</span>
</div>

// Compiles to:
React.createElement('div', { id: 'box' },
  React.createElement('span', null, 'Hello')
);
```

**Interview Q: Why did we need `import React from 'react'` in every file before React 17?**
**A:** Because JSX compiled to `React.createElement()`, which required React to be in scope. React 17's new JSX transform auto-imports the necessary functions, so the manual import is no longer needed.

**Common Mistake:** Still writing `import React from 'react'` in every file with React 17+. Not harmful, but unnecessary.

**One-liner:** JSX compiles to React.createElement(). React 17+ auto-imports this, so manual React import isn't required.

---

### 🔴 2.4 Embedding JavaScript in JSX

**Explanation:** Use curly braces `{}` to embed any JavaScript expression inside JSX. Expressions (things that produce a value) work — statements (if/else, for) do not.

```jsx
function Greeting({ name, age }) {
  return (
    <div>
      {/* Variable */}
      <h1>Hello, {name}!</h1>

      {/* Expression */}
      <p>Next year you'll be {age + 1}</p>

      {/* Ternary (conditional expression) */}
      <p>{age >= 18 ? 'Adult' : 'Minor'}</p>

      {/* Function call */}
      <p>{name.toUpperCase()}</p>

      {/* ❌ This would FAIL — if/else is a statement, not an expression */}
      {/* {if (age > 18) { return 'Adult' }} */}
    </div>
  );
}
```

**Interview Q: Can you use if/else inside JSX?**
**A:** No, because JSX only accepts expressions (things that return a value). Use ternary operators (`? :`), logical AND (`&&`), or move the logic outside the return.

**Common Mistake:** Trying to write `if/else` or `for` loops inside `{}` in JSX. Use `.map()` for loops and ternary for conditions.

**One-liner:** Use {} for JS expressions in JSX. Ternary and && work. if/else and for loops don't — they're statements, not expressions.

---

### 🔴 2.5 JSX Must Return a Single Parent

**Explanation:** A component can only return one root element. To return multiple elements without adding an extra DOM node, use React Fragment (`<>...</>` or `<React.Fragment>`).

```jsx
// ❌ WRONG — two root elements
function App() {
  return (
    <h1>Title</h1>
    <p>Content</p>
  );
}

// ✅ CORRECT — wrap in a div
function App() {
  return (
    <div>
      <h1>Title</h1>
      <p>Content</p>
    </div>
  );
}

// ✅ BETTER — Fragment adds no extra DOM node
function App() {
  return (
    <>
      <h1>Title</h1>
      <p>Content</p>
    </>
  );
}
```

**Interview Q: What is a React Fragment and why use it?**
**A:** A Fragment lets you group multiple elements without adding an extra DOM node. `<div>` wrappers pollute the DOM and can break CSS layouts (flexbox, grid). Fragments (`<>...</>`) solve this.

**Common Mistake:** Always using `<div>` wrappers instead of Fragments, which adds unnecessary DOM nodes.

**One-liner:** JSX must return one root element. Use `<>...</>` (Fragment) to group elements without extra DOM nodes.

---

### 🔴 2.6 React Fragments

**Explanation:** Two ways to use Fragments. Short syntax `<>` can't have keys. Full syntax `<React.Fragment>` can accept a `key` prop — useful in lists.

```jsx
// Short syntax — no key
<>
  <h1>Title</h1>
  <p>Content</p>
</>

// Full syntax — can have key (needed in lists)
{items.map(item => (
  <React.Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.definition}</dd>
  </React.Fragment>
))}
```

**Interview Q: When do you need `<React.Fragment>` instead of `<>`?**
**A:** When you need to pass a `key` prop — the short syntax `<>` doesn't support attributes. This is common when rendering lists that need to return multiple elements per item.

**One-liner:** `<>` = Fragment shorthand (no props). `<React.Fragment key={}>` = when you need key in lists.

---

### 🟡 2.7 Conditional Expressions in JSX

**Explanation:** Since if/else doesn't work in JSX, use ternary (`? :`), logical AND (`&&`), or logical OR (`||`).

```jsx
function Status({ isLoggedIn, count }) {
  return (
    <div>
      {/* Ternary — show one OR the other */}
      {isLoggedIn ? <Dashboard /> : <Login />}

      {/* && — show only if true */}
      {isLoggedIn && <WelcomeMessage />}

      {/* ⚠️ GOTCHA with && and 0 */}
      {count && <p>You have {count} items</p>}
      {/* If count is 0, this renders "0" on screen! */}

      {/* FIX: explicitly check boolean */}
      {count > 0 && <p>You have {count} items</p>}
    </div>
  );
}
```

**Interview Q: What is the gotcha with `&&` in conditional rendering?**
**A:** If the left side is `0` (falsy but a number), React renders `0` on screen instead of nothing. Because `0 && <Component />` evaluates to `0`, and React renders numbers. Fix: use `count > 0 &&` or a ternary.

**Common Mistake:** Writing `{count && <Component />}` when count could be 0.

**One-liner:** Use ternary (? :) or && for JSX conditions. Watch out: `0 && <X />` renders "0" on screen.

---

### 🟡 2.8 JSX Spread Attributes

**Explanation:** You can spread an object's properties as props onto a component using `{...obj}`. Useful for passing many props without listing each one.

```jsx
// Without spread — verbose
const props = { name: "Alice", age: 25, role: "admin" };
<UserCard name={props.name} age={props.age} role={props.role} />

// With spread — clean
<UserCard {...props} />

// You can override specific props after spreading
<UserCard {...props} role="viewer" />  // role becomes "viewer"
```

**Interview Q: What are the risks of spreading props?**
**A:** You might pass unwanted or unexpected props to DOM elements, causing React warnings. It also makes it harder to see what props a component receives. Use it carefully and intentionally.

**One-liner:** `{...props}` spreads all object properties as component props. Clean but use carefully — can pass unwanted props.

---

### 🟡 2.9 dangerouslySetInnerHTML

**Explanation:** React escapes all values in JSX to prevent XSS attacks. To render raw HTML, use `dangerouslySetInnerHTML` — named intentionally scary to discourage misuse.

```jsx
function Article({ htmlContent }) {
  // ⚠️ DANGEROUS — only use with trusted/sanitized content
  return <div dangerouslySetInnerHTML={{ __html: htmlContent }} />;
}

// Usage:
<Article htmlContent="<strong>Bold</strong> and <em>italic</em>" />
```

**Interview Q: Why is it called "dangerously"?**
**A:** Because injecting raw HTML exposes your app to XSS (Cross-Site Scripting) attacks. If `htmlContent` comes from user input and isn't sanitized, an attacker could inject `<script>` tags. Always sanitize HTML (use libraries like DOMPurify) before rendering.

**Common Mistake:** Using it with unsanitized user input. Always sanitize first.

**One-liner:** dangerouslySetInnerHTML renders raw HTML. Named scary on purpose — sanitize content first or risk XSS.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 3: Components

---

### 🔴 3.1 Functional vs Class Components

**Explanation:** Functional components are plain JavaScript functions that return JSX. Class components are ES6 classes that extend `React.Component`. Since React 16.8 (hooks), functional components can do everything class components can.

```jsx
// FUNCTIONAL (modern, preferred)
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}

// CLASS (legacy, still works)
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

**Interview Q: Why are functional components preferred over class components?**
**A:** Simpler syntax, no `this` keyword confusion, hooks make state and lifecycle easy, easier to test, better performance (slightly less overhead), and the React team recommends them for all new code.

**Common Mistake:** Using class components in new projects. Use functional components with hooks for everything new.

**One-liner:** Functional = function that returns JSX (preferred). Class = ES6 class with render() method (legacy).

---

### 🔴 3.2 Controlled vs Uncontrolled Components

**Explanation:** Controlled = React state drives the input value (via `value` + `onChange`). Uncontrolled = the DOM handles the input value (via `ref` + `defaultValue`).

```jsx
// CONTROLLED — React owns the value
function ControlledInput() {
  const [text, setText] = useState('');
  return <input value={text} onChange={(e) => setText(e.target.value)} />;
}

// UNCONTROLLED — DOM owns the value
function UncontrolledInput() {
  const inputRef = useRef();
  const handleSubmit = () => console.log(inputRef.current.value);
  return <input ref={inputRef} defaultValue="" />;
}
```

**Interview Q: When would you use an uncontrolled component?**
**A:** For simple forms where you only need the value on submit (not on every keystroke), file inputs (`<input type="file">` is always uncontrolled), or when integrating with non-React libraries.

**Common Mistake:** Using both `value` and `defaultValue` on the same input — React will warn you. Pick one approach.

**One-liner:** Controlled = React state owns input value. Uncontrolled = DOM owns it, accessed via ref.

---

### 🔴 3.3 Pure Components

**Explanation:** A pure component renders the same output for the same props and state — no side effects in rendering. React can skip re-rendering pure components if their inputs haven't changed.

```jsx
// Pure — same input always gives same output
function PureGreeting({ name }) {
  return <h1>Hello, {name}</h1>;  // no side effects, no randomness
}

// NOT pure — different output each time
function ImpureGreeting({ name }) {
  return <h1>Hello, {name} at {new Date().toLocaleTimeString()}</h1>;
}

// React.memo makes a functional component skip re-render if props unchanged
const MemoizedGreeting = React.memo(PureGreeting);
```

**Interview Q: What does React.memo do?**
**A:** It's a higher-order component that wraps a functional component and prevents re-renders if the props haven't changed (shallow comparison). It's the functional equivalent of class-based `PureComponent`.

**Common Mistake:** Wrapping EVERY component in React.memo. It adds overhead for the comparison. Only use it when re-renders are actually a performance problem.

**One-liner:** Pure component = same props → same output. React.memo skips re-render if props unchanged.

---

### 🟡 3.4 Higher-Order Components (HOC)

**Explanation:** A HOC is a function that takes a component and returns a new component with added behavior. It's a pattern for reusing component logic. Think of it as a wrapper.

```jsx
// HOC that adds loading behavior
function withLoading(WrappedComponent) {
  return function EnhancedComponent({ isLoading, ...props }) {
    if (isLoading) return <p>Loading...</p>;
    return <WrappedComponent {...props} />;
  };
}

// Usage
const UserListWithLoading = withLoading(UserList);
<UserListWithLoading isLoading={true} users={[]} />
```

**Interview Q: What are the drawbacks of HOCs?**
**A:** Wrapper hell (many nested HOCs become hard to debug), naming collisions in props, and difficulty tracing where props come from. Custom hooks have largely replaced HOCs for logic reuse.

**Common Mistake:** Using HOCs when custom hooks would be simpler and more readable.

**One-liner:** HOC = function(Component) → EnhancedComponent. Adds behavior. Largely replaced by custom hooks.

---

### 🟡 3.5 Render Props

**Explanation:** A component accepts a function as a prop (or children) and calls that function to determine what to render. The function receives data as arguments.

```jsx
// Render prop pattern
function MouseTracker({ render }) {
  const [pos, setPos] = useState({ x: 0, y: 0 });

  const handleMouseMove = (e) => {
    setPos({ x: e.clientX, y: e.clientY });
  };

  return <div onMouseMove={handleMouseMove}>{render(pos)}</div>;
}

// Usage — YOU decide what to render with the data
<MouseTracker render={({ x, y }) => <p>Mouse at {x}, {y}</p>} />
```

**Interview Q: Render props vs custom hooks — which to use?**
**A:** Custom hooks are simpler and more readable for most cases. Render props are still useful when you need to control what JSX is rendered based on shared logic, but hooks have largely replaced this pattern.

**One-liner:** Render prop = pass a function that receives data and returns JSX. Replaced by custom hooks in most cases.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 4: Props

---

### 🔴 4.1 What Are Props?

**Explanation:** Props (properties) are inputs passed from a parent component to a child component. They are read-only — a child can never modify the props it receives.

```jsx
// Parent passes props
function App() {
  return <UserCard name="Alice" age={25} isAdmin={true} />;
}

// Child receives props
function UserCard({ name, age, isAdmin }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      {isAdmin && <span>Admin</span>}
    </div>
  );
}
```

**Interview Q: Can a child modify its props?**
**A:** No. Props are read-only. If a child needs to change data, the parent should pass a callback function as a prop, and the child calls that function.

**Common Mistake:** Trying to do `props.name = "Bob"` inside a child. This doesn't work and violates React's one-way data flow.

**One-liner:** Props = read-only inputs from parent to child. Child can never modify them.

---

### 🔴 4.2 Passing Functions as Props

**Explanation:** Parents pass callback functions as props so children can communicate back up. This is how React achieves child → parent communication.

```jsx
function Parent() {
  const [items, setItems] = useState([]);

  // Pass this function DOWN to child
  const addItem = (item) => {
    setItems([...items, item]);
  };

  return <ChildForm onAdd={addItem} />;
}

function ChildForm({ onAdd }) {
  const [text, setText] = useState('');

  const handleSubmit = () => {
    onAdd(text);      // call parent's function
    setText('');        // clear input
  };

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={handleSubmit}>Add</button>
    </div>
  );
}
```

**Interview Q: How does a child communicate with its parent in React?**
**A:** The parent passes a callback function as a prop. The child calls that function with data as arguments. Data flows up through function calls, not direct mutation.

**One-liner:** Parent passes function as prop → child calls it to send data up.

---

### 🔴 4.3 Children Prop

**Explanation:** `props.children` contains whatever JSX is placed between a component's opening and closing tags. It lets you create wrapper/layout components.

```jsx
// Card component uses children to render anything inside it
function Card({ title, children }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-body">{children}</div>
    </div>
  );
}

// Usage — anything between <Card> tags becomes children
<Card title="Profile">
  <img src="avatar.jpg" />
  <p>Hello, I'm Alice!</p>
</Card>
```

**Interview Q: What is props.children and when do you use it?**
**A:** It's a special prop that contains the elements between a component's opening and closing tags. Use it for layout components (Card, Modal, Sidebar), wrapper components, and any component that needs to be flexible about its content.

**Common Mistake:** Forgetting that `children` can be anything — a string, an element, multiple elements, or even a function (render props).

**One-liner:** props.children = whatever you put between `<Component>...here...</Component>`. Perfect for wrappers and layouts.

---

### 🟡 4.4 Prop Drilling

**Explanation:** Prop drilling is when you pass props through multiple intermediate components that don't use them, just to reach a deeply nested component that does. It makes code harder to maintain.

```jsx
// PROP DRILLING — App passes "user" through 3 levels
function App() {
  const user = { name: "Alice" };
  return <Layout user={user} />;      // Layout doesn't use user
}
function Layout({ user }) {
  return <Sidebar user={user} />;     // Sidebar doesn't use user
}
function Sidebar({ user }) {
  return <UserInfo user={user} />;    // Only UserInfo needs user!
}
function UserInfo({ user }) {
  return <p>{user.name}</p>;
}
```

**Interview Q: How do you solve prop drilling?**
**A:** Three main solutions:
1. **Context API** — share data without passing through every level
2. **Component composition** — restructure so the data owner renders the consumer directly
3. **State management** (Redux, Zustand) — global store accessible anywhere

**Common Mistake:** Using Context for everything to avoid any prop drilling. Simple 1-2 level prop passing is fine and more explicit than Context.

**One-liner:** Prop drilling = passing props through components that don't need them. Fix with Context, composition, or state management.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 5: State

---

### 🔴 5.1 State vs Props

| | State | Props |
|---|---|---|
| Owned by | The component itself | Parent component |
| Mutable? | Yes (via setState/useState) | No (read-only) |
| Triggers re-render? | Yes | Yes (when parent re-renders with new props) |
| Where declared | Inside the component | Passed from outside |

```jsx
function Counter() {
  const [count, setCount] = useState(0); // STATE — owned by this component

  return <Display count={count} />;       // count becomes a PROP for Display
}

function Display({ count }) {             // PROP — received from parent
  return <p>{count}</p>;
}
```

**Interview Q: What is the difference between state and props?**
**A:** State is internal data owned and managed by the component. Props are external data passed by the parent. State can be changed by the component; props cannot.

**One-liner:** State = component's own data (mutable). Props = parent's data passed down (read-only).

---

### 🔴 5.2 useState Hook

**Explanation:** `useState` lets functional components hold state. It returns an array with the current value and a setter function. When you call the setter, the component re-renders.

```jsx
function Counter() {
  // count = current value, setCount = function to update it
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

**Interview Q: What does useState return?**
**A:** An array with exactly two elements: `[currentValue, setterFunction]`. We destructure them with array destructuring. The argument to useState is the initial value (used only on first render).

**Common Mistake:** Calling the setter directly in render body (causes infinite re-render loop):
```jsx
// ❌ WRONG — causes infinite loop
function Bad() {
  const [count, setCount] = useState(0);
  setCount(1); // called on every render → re-render → called again → loop
  return <p>{count}</p>;
}
```

**One-liner:** `const [value, setValue] = useState(initial)` — value holds state, setValue triggers re-render.

---

### 🔴 5.3 State is Asynchronous (Batching)

**Explanation:** React batches state updates for performance. Multiple `setState` calls in the same event handler are batched into a single re-render. You don't get the updated value immediately after calling setState.

```jsx
function Example() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
    // count is still 0 here — updates are batched
    // Result: count becomes 1 (not 3!) because all three use the same stale "count"
    console.log(count); // 0 — not updated yet!
  };

  // FIX: Use functional updater form
  const handleClickFixed = () => {
    setCount(prev => prev + 1);  // prev is 0, returns 1
    setCount(prev => prev + 1);  // prev is 1, returns 2
    setCount(prev => prev + 1);  // prev is 2, returns 3
    // Result: count becomes 3 ✓
  };

  return <button onClick={handleClickFixed}>{count}</button>;
}
```

**Interview Q: Why does calling setCount three times with `count + 1` only increment by 1?**
**A:** All three calls use the same stale `count` value from the current render. React batches them, so only the last value is applied. Use the functional form `setCount(prev => prev + 1)` to get the latest state.

**Common Mistake:** Relying on the state value immediately after setting it. State updates are scheduled, not instant.

**One-liner:** State updates are batched. Use `setState(prev => newValue)` when the new value depends on the previous.

---

### 🔴 5.4 State Immutability

**Explanation:** Never mutate state directly. Always create a new object/array and pass it to setState. React compares old and new state by reference — if you mutate, the reference stays the same and React won't re-render.

```jsx
function TodoApp() {
  const [todos, setTodos] = useState([{ id: 1, text: 'Learn React' }]);

  // ❌ WRONG — mutating state directly
  const addTodoBad = () => {
    todos.push({ id: 2, text: 'New' }); // mutates the same array
    setTodos(todos); // same reference → React won't re-render!
  };

  // ✅ CORRECT — create new array
  const addTodoGood = () => {
    setTodos([...todos, { id: 2, text: 'New' }]); // new array reference
  };

  // ✅ Updating an object in state
  const [user, setUser] = useState({ name: 'Alice', age: 25 });
  const updateName = () => {
    setUser({ ...user, name: 'Bob' }); // spread old, override name
  };
}
```

**Interview Q: Why must state be treated as immutable in React?**
**A:** React uses reference comparison to detect changes. If you mutate an object/array, the reference stays the same, so React thinks nothing changed and skips re-rendering. Creating a new reference tells React "something changed, re-render."

**Common Mistake:** Using `array.push()`, `array.splice()`, or `object.property = value` directly on state.

**One-liner:** Never mutate state. Always spread into a new object/array. Same reference = React won't re-render.

---

### 🔴 5.5 Lifting State Up

**Explanation:** When two sibling components need to share state, move the state up to their closest common parent. The parent holds the state and passes it down to both children via props.

```jsx
// ❌ BEFORE — siblings can't share state
function TempInput() {
  const [temp, setTemp] = useState(''); // each has its own state
  return <input value={temp} onChange={e => setTemp(e.target.value)} />;
}

// ✅ AFTER — state lifted to parent
function Calculator() {
  const [temp, setTemp] = useState(''); // parent owns the state

  return (
    <div>
      <CelsiusInput temp={temp} onTempChange={setTemp} />
      <FahrenheitDisplay temp={temp} />
    </div>
  );
}

function CelsiusInput({ temp, onTempChange }) {
  return <input value={temp} onChange={e => onTempChange(e.target.value)} />;
}

function FahrenheitDisplay({ temp }) {
  const fahrenheit = (parseFloat(temp) * 9/5 + 32) || '';
  return <p>{fahrenheit}°F</p>;
}
```

**Interview Q: When should you lift state up?**
**A:** When two or more components need access to the same state. Move it to their closest common ancestor so it can be shared through props.

**One-liner:** Shared state → move it to the closest common parent → pass down as props to all who need it.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Category 6: Hooks

---

### 🔴 6.1 Rules of Hooks

**Explanation:** Hooks have two strict rules that must never be broken. React relies on the ORDER hooks are called — breaking these rules corrupts React's internal state tracking.

```jsx
// ✅ RULE 1: Only call hooks at the TOP LEVEL
// Never inside if/else, loops, or nested functions
function Good() {
  const [count, setCount] = useState(0);   // ✅ top level
  useEffect(() => {}, []);                  // ✅ top level
  return <p>{count}</p>;
}

function Bad() {
  if (someCondition) {
    const [count, setCount] = useState(0);  // ❌ inside condition!
  }
}

// ✅ RULE 2: Only call hooks in React COMPONENTS or CUSTOM HOOKS
// Never in regular JavaScript functions
function notAComponent() {
  const [x, setX] = useState(0);  // ❌ not a React component!
}

function useCustomHook() {
  const [x, setX] = useState(0);  // ✅ custom hook (starts with "use")
  return x;
}
```

**Interview Q: Why can't you call hooks inside conditions or loops?**
**A:** React tracks hooks by the ORDER they're called on each render. If a hook is inside a condition, it might not run on some renders, shifting the order of all subsequent hooks and corrupting React's state tracking.

**Common Mistake:** Wrapping useState or useEffect inside an if statement.

**One-liner:** Hooks: always at top level, only in components or custom hooks. Order matters — never conditionally call them.

---

### 🔴 6.2 useEffect

**Explanation:** `useEffect` runs side effects in functional components: fetching data, subscriptions, timers, DOM manipulation. It runs AFTER the render is committed to the screen.

```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  // Runs after render, re-runs when userId changes
  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(data => setUser(data));

    // CLEANUP — runs before next effect or on unmount
    return () => {
      console.log('Cleaning up previous effect');
    };
  }, [userId]); // dependency array — only re-run when userId changes

  return user ? <h1>{user.name}</h1> : <p>Loading...</p>;
}
```

**Three dependency array behaviors:**
```jsx
useEffect(() => { /* ... */ });          // runs after EVERY render
useEffect(() => { /* ... */ }, []);      // runs ONCE after first render (mount)
useEffect(() => { /* ... */ }, [a, b]);  // runs when a OR b changes
```

**Interview Q: What does the empty dependency array `[]` mean in useEffect?**
**A:** It tells React to run the effect only once, after the initial render (like componentDidMount). It won't re-run on any state or prop change.

**Common Mistake:** Missing dependencies in the array, causing stale closures or bugs. ESLint's `exhaustive-deps` rule helps catch this.

**One-liner:** useEffect(fn, deps) — runs side effects after render. [] = once. [dep] = when dep changes. No array = every render.

---

### 🔴 6.3 useRef

**Explanation:** `useRef` creates a mutable reference that persists across renders WITHOUT causing re-renders when changed. Two main uses: accessing DOM elements and storing values that shouldn't trigger re-renders.

```jsx
function TextInput() {
  const inputRef = useRef(null); // create ref

  const focusInput = () => {
    inputRef.current.focus();    // access DOM element directly
  };

  return (
    <div>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}

// Use case 2: Persist value across renders without re-rendering
function Timer() {
  const intervalRef = useRef(null);       // stores interval ID
  const [count, setCount] = useState(0);

  const start = () => {
    intervalRef.current = setInterval(() => {
      setCount(prev => prev + 1);
    }, 1000);
  };

  const stop = () => {
    clearInterval(intervalRef.current);   // access stored interval ID
  };

  return (
    <div>
      <p>{count}</p>
      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
    </div>
  );
}
```

**Interview Q: What is the difference between useRef and useState?**
**A:** Both persist values across renders. But `useState` triggers a re-render when updated; `useRef` does NOT. Use `useRef` for values you need to track but don't need to display (interval IDs, previous values, DOM elements).

**One-liner:** useRef = mutable box that persists across renders. Changing it does NOT re-render. Use for DOM access and storing non-rendered values.

---

### 🔴 6.4 useMemo

**Explanation:** `useMemo` memoizes the RESULT of an expensive calculation. It only recalculates when its dependencies change. Use it to avoid redoing heavy computations on every render.

```jsx
function ProductList({ products, filter }) {
  // ❌ Without useMemo — filters on EVERY render, even if products/filter unchanged
  const filtered = products.filter(p => p.name.includes(filter));

  // ✅ With useMemo — only recalculates when products or filter change
  const filteredMemo = useMemo(() => {
    return products.filter(p => p.name.includes(filter));
  }, [products, filter]);

  return filteredMemo.map(p => <div key={p.id}>{p.name}</div>);
}
```

**Interview Q: When should you use useMemo?**
**A:** When you have an expensive computation (filtering large arrays, complex math, formatting data) that doesn't need to rerun if its inputs haven't changed. Don't use it for simple operations — the memoization overhead isn't worth it.

**Common Mistake:** Using useMemo everywhere. It adds memory overhead (stores the cached value). Only use it when you've identified an actual performance bottleneck.

**One-liner:** useMemo(fn, [deps]) = cache a computed VALUE. Only recomputes when deps change.

---

### 🔴 6.5 useCallback

**Explanation:** `useCallback` memoizes a FUNCTION reference. It returns the same function instance between renders unless its dependencies change. Useful when passing callbacks to memoized child components.

```jsx
function Parent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  // ❌ Without useCallback — new function every render
  // Child re-renders even if only "text" changed
  const increment = () => setCount(prev => prev + 1);

  // ✅ With useCallback — same function reference unless deps change
  const incrementMemo = useCallback(() => {
    setCount(prev => prev + 1);
  }, []); // no deps — function never changes

  return (
    <div>
      <input value={text} onChange={e => setText(e.target.value)} />
      <MemoizedChild onIncrement={incrementMemo} />
    </div>
  );
}

const MemoizedChild = React.memo(({ onIncrement }) => {
  console.log('Child rendered');
  return <button onClick={onIncrement}>+</button>;
});
```

**Interview Q: What is the difference between useMemo and useCallback?**
**A:**
- `useMemo(() => value, [deps])` — caches the RETURN VALUE of a function
- `useCallback(fn, [deps])` — caches the FUNCTION ITSELF

`useCallback(fn, deps)` is the same as `useMemo(() => fn, deps)`.

**Common Mistake:** Using useCallback without React.memo on the child. Without memo, the child re-renders regardless of whether the function reference changed.

**One-liner:** useCallback(fn, [deps]) = cache a FUNCTION reference. Pair with React.memo on child to prevent unnecessary re-renders.

---

### 🔴 6.6 Custom Hooks

**Explanation:** A custom hook is a function starting with "use" that encapsulates reusable stateful logic. It can use other hooks inside it. It lets you share logic between components without changing their structure.

```jsx
// Custom hook — reusable fetch logic
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    setLoading(true);
    fetch(url)
      .then(res => res.json())
      .then(data => { setData(data); setLoading(false); })
      .catch(err => { setError(err); setLoading(false); });
  }, [url]);

  return { data, loading, error };
}

// Usage — ANY component can use this hook
function UserPage() {
  const { data, loading, error } = useFetch('/api/users');
  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error!</p>;
  return <ul>{data.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}

function ProductPage() {
  const { data, loading } = useFetch('/api/products');  // reused!
  // ...
}
```

**Interview Q: What is a custom hook and when would you create one?**
**A:** A custom hook extracts reusable stateful logic from components. Create one when two or more components share the same logic (data fetching, form handling, window resize tracking, auth status, etc.). It must start with "use" so React knows to apply hook rules.

**Common Mistake:** Not starting the function name with "use". Without the "use" prefix, React won't enforce the Rules of Hooks and linting won't catch mistakes.

**One-liner:** Custom hook = "use" function that encapsulates reusable logic with hooks. Share logic, not UI.

---

### 🟡 6.7 useReducer

**Explanation:** `useReducer` is an alternative to useState for complex state logic. It works like Redux — dispatch an action, a reducer function processes it and returns new state.

```jsx
// Reducer function — pure function: (state, action) → newState
function counterReducer(state, action) {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    case 'reset':     return { count: 0 };
    default: throw new Error(`Unknown action: ${action.type}`);
  }
}

function Counter() {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });

  return (
    <div>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}
```

**Interview Q: When should you use useReducer instead of useState?**
**A:** When state logic is complex (multiple related values, actions depend on previous state), when the next state depends on the previous one in non-trivial ways, or when you want to centralize state logic in one function for testing.

**One-liner:** useReducer(reducer, initial) = dispatch actions to a reducer function. Better than useState for complex state logic.

---

### 🟡 6.8 useLayoutEffect vs useEffect

**Explanation:** Both run side effects, but at different times. `useEffect` runs AFTER the browser paints. `useLayoutEffect` runs BEFORE the browser paints — synchronously after DOM mutations.

```jsx
// useEffect — runs AFTER paint (async, non-blocking)
// Use for: data fetching, subscriptions, logging
useEffect(() => {
  console.log('After paint');
}, []);

// useLayoutEffect — runs BEFORE paint (sync, blocking)
// Use for: measuring DOM, preventing visual flicker
useLayoutEffect(() => {
  const height = elementRef.current.getBoundingClientRect().height;
  // set position based on measured height — happens before user sees anything
}, []);
```

**Interview Q: When would you use useLayoutEffect?**
**A:** When you need to measure DOM elements and adjust the UI before the user sees it — like positioning tooltips, measuring element sizes, or preventing visual flicker. For everything else, use useEffect.

**Common Mistake:** Using useLayoutEffect for data fetching. It blocks painting, making the UI feel slow. Use useEffect for async operations.

**One-liner:** useEffect = after paint (async). useLayoutEffect = before paint (sync). Use layout only for DOM measurements to avoid flicker.

---

### 🟡 6.9 Stale Closure Problem

**Explanation:** A stale closure happens when a function inside useEffect or useCallback "captures" an old value of state because the dependency array is missing that state variable.

```jsx
function StaleExample() {
  const [count, setCount] = useState(0);

  // ❌ STALE CLOSURE — count is always 0 inside this effect
  useEffect(() => {
    const id = setInterval(() => {
      console.log(count);  // always logs 0! (captured old count)
    }, 1000);
    return () => clearInterval(id);
  }, []); // empty deps → effect never re-runs → count is stale

  // ✅ FIX 1: Add count to deps (effect re-runs each time)
  useEffect(() => {
    const id = setInterval(() => {
      console.log(count);  // now gets updated count
    }, 1000);
    return () => clearInterval(id);
  }, [count]);

  // ✅ FIX 2: Use functional updater (no dependency on count)
  useEffect(() => {
    const id = setInterval(() => {
      setCount(prev => prev + 1);  // doesn't need count in closure
    }, 1000);
    return () => clearInterval(id);
  }, []);

  return <p>{count}</p>;
}
```

**Interview Q: What is a stale closure in React hooks?**
**A:** It's when a function created in a previous render still references old values of state/props because it "closed over" those values. It happens when the dependency array of useEffect or useCallback is missing a variable it uses.

**One-liner:** Stale closure = hook function captures old state. Fix: add to deps array, or use functional updater `prev => ...`.

---

### 🟢 6.10 useTransition (React 18)

**Explanation:** `useTransition` marks state updates as "non-urgent." React prioritizes urgent updates (user typing) over transition updates (filtering a list), keeping the UI responsive.

```jsx
function SearchList({ items }) {
  const [query, setQuery] = useState('');
  const [filtered, setFiltered] = useState(items);
  const [isPending, startTransition] = useTransition();

  const handleChange = (e) => {
    setQuery(e.target.value);  // urgent — update input immediately

    startTransition(() => {
      // non-urgent — can be interrupted if user types more
      setFiltered(items.filter(item => item.includes(e.target.value)));
    });
  };

  return (
    <div>
      <input value={query} onChange={handleChange} />
      {isPending && <p>Updating...</p>}
      {filtered.map(item => <p key={item}>{item}</p>)}
    </div>
  );
}
```

**Interview Q: What problem does useTransition solve?**
**A:** Large state updates (filtering 10,000 items) can freeze the UI because React processes them synchronously. useTransition tells React "this update can wait" — so user input stays responsive while the expensive update happens in the background.

**One-liner:** useTransition marks updates as non-urgent. React keeps UI responsive by prioritizing user interactions over heavy re-renders.

---

[↑ Back to Table of Contents](#table-of-contents--part-1)

---

# Quick Revision — Part 1 One-Liners

```
REACT FUNDAMENTALS
  React = UI library, component-based, Virtual DOM
  Virtual DOM = JS copy, diff old vs new, patch only changes
  Fiber = interruptible rendering, breaks work into chunks
  One-way flow = data parent→child via props, child→parent via callbacks
  Declarative = describe WHAT, not HOW

JSX
  JSX = HTML-like syntax that compiles to React.createElement()
  className not class, htmlFor not for, camelCase events
  {} for expressions. No if/else — use ternary or &&
  Must return single root. Use <> Fragment for no extra DOM
  0 && <X/> renders "0" — use count > 0 && <X/>

COMPONENTS
  Functional > Class. Always use functional + hooks
  Controlled = React owns value. Uncontrolled = DOM owns value
  React.memo = skip re-render if props unchanged
  HOC = function(Component) → EnhancedComponent (replaced by hooks)

PROPS
  Props = read-only inputs from parent
  children = content between <Component>...</Component>
  Prop drilling = passing through components that don't need it → use Context

STATE
  State = component's own mutable data. Props = parent's read-only data
  useState returns [value, setter]. Setter triggers re-render
  State updates are batched. Use prev => for dependent updates
  Never mutate. Always spread into new object/array
  Shared state → lift to closest common parent

HOOKS
  Rules: top level only, components/custom hooks only
  useEffect(fn, [deps]) = side effects. [] = mount only
  useRef = persistent mutable box, no re-render on change
  useMemo = cache computed value. useCallback = cache function reference
  Custom hook = reusable "use" function sharing stateful logic
  useReducer = dispatch actions to reducer for complex state
  Stale closure = hook captures old state → fix with deps or prev =>
```
