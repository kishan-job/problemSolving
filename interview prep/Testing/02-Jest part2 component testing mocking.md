# Category 2: Component Testing & Mocking — Question Bank
## 65 Interview Questions with Answers
## Organized by Sub-Topic · Tagged by Difficulty

---

# Table of Contents

- [4.1 render() & screen (4 Questions)](#41-render--screen)
- [4.2 getByRole (3 Questions)](#42-getbyrole)
- [4.3 getByText, getByLabelText, getByTestId (3 Questions)](#43-getbytext-getbylabeltext-getbytestid)
- [4.4 queryBy vs findBy vs getBy (4 Questions)](#44-queryby-vs-findby-vs-getby)
- [4.5 within() & screen.debug() (3 Questions)](#45-within--screendebug)
- [5.1 userEvent.setup(), click, type, clear (4 Questions)](#51-usereventsetup-click-type-clear)
- [5.2 fireEvent vs userEvent (2 Questions)](#52-fireevent-vs-userevent)
- [5.3 Testing Forms (4 Questions)](#53-testing-forms)
- [5.4 selectOptions, tab, hover, keyboard (3 Questions)](#54-selectoptions-tab-hover-keyboard)
- [6.1 async/await in Tests (3 Questions)](#61-asyncawait-in-tests)
- [6.2 waitFor() (4 Questions)](#62-waitfor)
- [6.3 act() (4 Questions)](#63-act)
- [6.4 Fake Timers (4 Questions)](#64-fake-timers)
- [7.1 jest.fn() (4 Questions)](#71-jestfn)
- [7.2 jest.mock() (4 Questions)](#72-jestmock)
- [7.3 jest.spyOn() (3 Questions)](#73-jestspyon)
- [7.4 mockClear vs mockReset vs mockRestore (2 Questions)](#74-mockclear-vs-mockreset-vs-mockrestore)
- [8.1 Mocking fetch / axios (3 Questions)](#81-mocking-fetch--axios)
- [8.2 MSW — Mock Service Worker (4 Questions)](#82-msw--mock-service-worker)
- [8.3 Mocking React Router (2 Questions)](#83-mocking-react-router)
- [8.4 Mocking Browser APIs (2 Questions)](#84-mocking-browser-apis)
- [9.1 Snapshot Testing (3 Questions)](#91-snapshot-testing)

---

```
Difficulty Tags:
  🟢 Easy    — Freshers / Junior (0-1 years)
  🟡 Medium  — Mid-level (2-4 years)
  🔴 Hard    — Senior (5+ years) / Tricky
```

---

# 4.1 render() & screen

---

### Q1 🟢 What does `render()` do in React Testing Library?

**Answer:**

`render()` mounts your component into a fake DOM (jsdom) so you can test it:

```javascript
import { render, screen } from '@testing-library/react';
import Button from './Button';

test('renders button text', () => {
  render(<Button label="Save" />);

  // Component is now in the DOM — query it with screen
  expect(screen.getByText('Save')).toBeInTheDocument();
});
```

`render()` returns utilities like `container`, `unmount`, `rerender`, but you should almost always use `screen` instead of destructuring from `render`.

---

### Q2 🟢 What is the `screen` object?

**Answer:**

`screen` is a global reference to the rendered DOM. It has every query method attached:

```javascript
// ❌ OLD WAY — destructuring from render (still works, not preferred)
const { getByText, getByRole } = render(<App />);
getByText('Hello');

// ✅ PREFERRED — use screen (cleaner, no destructuring)
render(<App />);
screen.getByText('Hello');
screen.getByRole('button');
screen.debug();  // Prints the current DOM to console
```

**Why `screen` is better:**
1. No need to destructure — less boilerplate
2. If you render in `beforeEach`, queries still work in each test
3. `screen.debug()` always available

---

### Q3 🟡 What does `rerender()` do and when would you use it?

**Answer:**

`rerender()` updates the component with new props without unmounting:

```javascript
test('updates greeting when name changes', () => {
  const { rerender } = render(<Greeting name="Alice" />);
  expect(screen.getByText('Hello, Alice')).toBeInTheDocument();

  rerender(<Greeting name="Bob" />);
  expect(screen.getByText('Hello, Bob')).toBeInTheDocument();
});
```

**Use when:** Testing how a component responds to prop changes. Without `rerender`, you'd need to re-render the parent which is more complex.

**Common mistake:** Calling `render()` again instead of `rerender()` — that creates a SECOND component instance in the DOM.

---

### Q4 🟡 What does `unmount()` do?

**Answer:**

`unmount()` removes the component from the DOM. Useful for testing cleanup effects:

```javascript
test('unsubscribes on unmount', () => {
  const unsubscribe = jest.fn();
  jest.spyOn(api, 'subscribe').mockReturnValue(unsubscribe);

  const { unmount } = render(<LiveFeed />);
  expect(api.subscribe).toHaveBeenCalled();

  unmount();
  expect(unsubscribe).toHaveBeenCalled();  // Cleanup ran
});
```

RTL calls `cleanup` (which unmounts) automatically after each test, so you rarely call `unmount()` manually.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 4.2 getByRole

---

### Q5 🟢 Why is `getByRole` the preferred query?

**Answer:**

`getByRole` queries the accessibility tree — the same way screen readers see your app. If `getByRole` can't find your element, **your app has an accessibility problem**.

```javascript
// ✅ getByRole — tests accessibility AND finds element
screen.getByRole('button', { name: /save/i });
screen.getByRole('heading', { name: /welcome/i });
screen.getByRole('textbox', { name: /email/i });
screen.getByRole('checkbox', { name: /agree/i });
screen.getByRole('link', { name: /sign up/i });

// Common HTML → Role mapping:
// <button>        → button
// <a href="">     → link
// <input>         → textbox
// <h1>-<h6>       → heading
// <select>        → combobox
// <input checkbox> → checkbox
// <input radio>    → radio
// <img>           → img
// <ul>, <ol>      → list
// <li>            → listitem
```

---

### Q6 🟡 What is the `name` option in `getByRole`?

**Answer:**

`name` matches the **accessible name** — the text a screen reader would announce:

```javascript
// <button>Save Changes</button>
screen.getByRole('button', { name: 'Save Changes' });  // ✅
screen.getByRole('button', { name: /save/i });          // ✅ regex, case-insensitive

// <label for="email">Email Address</label>
// <input id="email" />
screen.getByRole('textbox', { name: /email address/i }); // ✅ label text

// <button aria-label="Close dialog">✕</button>
screen.getByRole('button', { name: /close dialog/i });   // ✅ aria-label
```

The accessible name comes from: text content, `<label>`, `aria-label`, `aria-labelledby`, `alt` text, or `title`.

---

### Q7 🔴 How do you find elements that don't have an obvious role?

**Answer:**

```javascript
// Use logRoles to discover what roles exist:
import { logRoles } from '@testing-library/react';

const { container } = render(<MyComponent />);
logRoles(container);
// Prints:
// heading:    [<h1>Welcome</h1>]
// textbox:    [<input />]
// button:     [<button>Submit</button>]
// separator:  [<hr />]

// If an element has NO implicit role, add one:
// <div role="alert">Error occurred</div>
screen.getByRole('alert');

// RTL query priority (use in this order):
// 1. getByRole        ← accessible, best
// 2. getByLabelText   ← form elements
// 3. getByPlaceholderText ← if no label exists
// 4. getByText        ← non-interactive elements
// 5. getByDisplayValue ← current input value
// 6. getByAltText     ← images
// 7. getByTitle       ← title attribute
// 8. getByTestId      ← LAST RESORT
```

**Rule:** If you reach for `getByTestId`, ask yourself: "Can I add a proper label or role instead?"

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 4.3 getByText, getByLabelText, getByTestId

---

### Q8 🟢 When do you use `getByText` vs `getByLabelText` vs `getByTestId`?

**Answer:**

```javascript
// getByText — for non-interactive elements (headings, paragraphs, spans)
screen.getByText('Welcome back!');
screen.getByText(/welcome/i);  // regex, case-insensitive

// getByLabelText — for form elements (inputs, selects, textareas)
// <label>Email</label> <input />
screen.getByLabelText('Email');
screen.getByLabelText(/email/i);

// getByTestId — LAST RESORT when no semantic query works
// <div data-testid="custom-loader">Loading...</div>
screen.getByTestId('custom-loader');
```

| Query | Use for | Example |
|---|---|---|
| `getByRole` | Anything with a role | Buttons, links, headings, inputs |
| `getByLabelText` | Form fields with labels | `<label>Name</label><input>` |
| `getByText` | Static text, no role | Paragraphs, spans, error messages |
| `getByTestId` | Last resort | Complex components, icons |

---

### Q9 🟡 What is the output?

```javascript
// Component:
const Form = () => (
  <form>
    <label htmlFor="user">Username</label>
    <input id="user" placeholder="Enter username" value="alice" />
  </form>
);

test('queries', () => {
  render(<Form />);
  console.log(screen.getByLabelText('Username').tagName);
  console.log(screen.getByPlaceholderText('Enter username').tagName);
  console.log(screen.getByDisplayValue('alice').tagName);
});
```

**Answer:**

```
INPUT     ← getByLabelText finds the <input> linked to <label>
INPUT     ← getByPlaceholderText finds by placeholder attr
INPUT     ← getByDisplayValue finds by current input value
```

All three find the same `<input>`, but via different attributes. `getByLabelText` is preferred because it tests the label-input connection (accessibility).

---

### Q10 🟡 What are the exact and regex matching rules?

**Answer:**

```javascript
// Exact string match (default):
screen.getByText('Hello World');   // ✅ exact match
screen.getByText('Hello');         // ❌ FAIL — partial match doesn't work
screen.getByText('hello world');   // ❌ FAIL — case-sensitive

// Regex match:
screen.getByText(/hello/i);       // ✅ case-insensitive, partial
screen.getByText(/^Hello World$/); // ✅ exact with regex
screen.getByText(/hello world/i); // ✅ case-insensitive full match

// exact: false option:
screen.getByText('Hello', { exact: false });  // ✅ partial, case-insensitive

// Custom function:
screen.getByText((content, element) => {
  return content.startsWith('Error') && element.tagName === 'SPAN';
});
```

**Rule of thumb:** Use regex with `/i` flag for most queries. It's more resilient to minor text changes.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 4.4 queryBy vs findBy vs getBy

---

### Q11 🟢 What is the difference between `getBy`, `queryBy`, and `findBy`?

**Answer:**

| | `getBy` | `queryBy` | `findBy` |
|---|---|---|---|
| Returns | Element | Element or `null` | Promise<Element> |
| Not found | ❌ Throws error | Returns `null` | ❌ Throws after timeout |
| Multiple found | ❌ Throws error | ❌ Throws error | ❌ Throws error |
| Use when | Element SHOULD exist | Element MIGHT not exist | Element appears LATER (async) |

```javascript
// getBy — element must be there right now
screen.getByText('Hello');  // ✅ or throws

// queryBy — check if element is NOT there
screen.queryByText('Error');  // null if not found
expect(screen.queryByText('Error')).not.toBeInTheDocument();

// findBy — element will appear asynchronously
await screen.findByText('Data loaded');  // Waits up to 1000ms
```

---

### Q12 🟢 Why does this test fail?

```javascript
test('error message is not shown', () => {
  render(<Form />);
  expect(screen.getByText('Invalid email')).not.toBeInTheDocument();
});
```

**Answer:**

`getByText` throws an error if the element doesn't exist. The test crashes BEFORE reaching `.not.toBeInTheDocument()`.

```javascript
// ❌ WRONG — getBy throws when not found
expect(screen.getByText('Invalid email')).not.toBeInTheDocument();

// ✅ CORRECT — queryBy returns null when not found
expect(screen.queryByText('Invalid email')).not.toBeInTheDocument();
```

**One-liner:** Use `queryBy` when asserting something does NOT exist.

---

### Q13 🟡 When do you use `findBy` vs `waitFor`?

**Answer:**

```javascript
// findBy — shortcut for waiting for ONE element to appear
const heading = await screen.findByText('Welcome');
// Equivalent to:
const heading = await waitFor(() => screen.getByText('Welcome'));

// waitFor — for MORE complex async assertions
await waitFor(() => {
  expect(screen.getByText('Welcome')).toBeInTheDocument();
  expect(screen.getByText('Alice')).toBeInTheDocument();
  expect(mockApi).toHaveBeenCalledTimes(1);
});
```

| | `findBy` | `waitFor` |
|---|---|---|
| Returns | The element | Whatever callback returns |
| Assertions | Single element | Multiple assertions |
| Timeout | 1000ms default | 1000ms default |
| Use when | Waiting for one element | Waiting for any condition |

---

### Q14 🔴 What is the output?

```javascript
// Component loads data asynchronously:
const UserProfile = () => {
  const [user, setUser] = useState(null);
  useEffect(() => {
    fetchUser().then(setUser);
  }, []);
  if (!user) return <p>Loading...</p>;
  return <p>{user.name}</p>;
};

test('shows user name after loading', async () => {
  render(<UserProfile />);

  // Which queries work?
  screen.getByText('Loading...');             // ?
  screen.getByText('Alice');                   // ?
  screen.queryByText('Alice');                 // ?
  await screen.findByText('Alice');            // ?
});
```

**Answer:**

```javascript
screen.getByText('Loading...');          // ✅ PASS — shown immediately
screen.getByText('Alice');                // ❌ THROWS — not in DOM yet
screen.queryByText('Alice');              // null — not in DOM yet (no throw)
await screen.findByText('Alice');         // ✅ PASS — waits for it to appear
```

After `fetchUser` resolves, "Loading..." disappears and "Alice" appears. Only `findBy` waits for this async change.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 4.5 within() & screen.debug()

---

### Q15 🟡 What does `within()` do?

**Answer:**

`within()` scopes queries to a specific container — like a sub-`screen`:

```javascript
// Problem: two sections both have a "Delete" button
// screen.getByRole('button', { name: /delete/i }) → ERROR (multiple found!)

// Solution: scope to a specific section
const cartSection = screen.getByRole('region', { name: /cart/i });
const wishlistSection = screen.getByRole('region', { name: /wishlist/i });

within(cartSection).getByRole('button', { name: /delete/i });      // Cart's delete
within(wishlistSection).getByRole('button', { name: /delete/i });  // Wishlist's delete
```

```javascript
// Also useful for table rows:
const rows = screen.getAllByRole('row');
expect(within(rows[1]).getByText('Alice')).toBeInTheDocument();
expect(within(rows[1]).getByText('Admin')).toBeInTheDocument();
```

---

### Q16 🟡 How do you use `screen.debug()` for debugging?

**Answer:**

```javascript
test('debugging example', () => {
  render(<TodoList todos={[{ id: 1, text: 'Buy milk' }]} />);

  screen.debug();
  // Prints full DOM to console:
  // <body>
  //   <div>
  //     <ul>
  //       <li>Buy milk</li>
  //     </ul>
  //   </div>
  // </body>

  // Debug a specific element:
  screen.debug(screen.getByRole('list'));
  // Prints only the <ul> and its children

  // Increase output length (default truncates at 7000 chars):
  screen.debug(undefined, 30000);
});
```

**Pro tip:** When a query fails, RTL automatically prints the DOM in the error message. `screen.debug()` is for when you want to inspect BEFORE the failure.

---

### Q17 🔴 What does `logRoles()` do and when is it useful?

**Answer:**

```javascript
import { logRoles } from '@testing-library/react';

test('discover roles', () => {
  const { container } = render(<Navigation />);
  logRoles(container);
});

// Output:
// navigation:
//   <nav />
// list:
//   <ul />
// listitem:
//   <li />
//   <li />
// link:
//   <a href="/home">Home</a>
//   <a href="/about">About</a>
```

**Use when:** You don't know what `role` to use in `getByRole`. Run `logRoles`, see what's available, then write your query.

This is better than guessing roles or falling back to `getByTestId`.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 5.1 userEvent.setup(), click, type, clear

---

### Q18 🟢 How do you simulate user interactions with `userEvent`?

**Answer:**

```javascript
import userEvent from '@testing-library/user-event';

test('adds a todo', async () => {
  const user = userEvent.setup();  // Always call setup() first
  render(<TodoApp />);

  // Type into input
  await user.type(screen.getByRole('textbox'), 'Buy groceries');

  // Click a button
  await user.click(screen.getByRole('button', { name: /add/i }));

  // Assert
  expect(screen.getByText('Buy groceries')).toBeInTheDocument();
});
```

**Key rules:**
1. Always call `userEvent.setup()` before `render()`
2. Every `userEvent` method is async — always `await` it
3. Use `user.type()` not `user.keyboard()` for typing into inputs

---

### Q19 🟢 What does `userEvent.clear()` do?

**Answer:**

`clear()` selects all text in an input and deletes it — like Ctrl+A then Backspace:

```javascript
test('clears and retypes input', async () => {
  const user = userEvent.setup();
  render(<SearchBar defaultValue="old query" />);

  const input = screen.getByRole('textbox');
  expect(input).toHaveValue('old query');

  await user.clear(input);
  expect(input).toHaveValue('');

  await user.type(input, 'new query');
  expect(input).toHaveValue('new query');
});
```

**Without `clear()`:** `user.type()` appends text. So typing "new" into an input with "old" gives "oldnew".

---

### Q20 🟡 What is the output?

```javascript
test('type behavior', async () => {
  const handleChange = jest.fn();
  const user = userEvent.setup();
  render(<input onChange={handleChange} />);

  await user.type(screen.getByRole('textbox'), 'Hi');
  console.log(handleChange.mock.calls.length);
});
```

**Answer:** `2`

`userEvent.type()` simulates real typing — one keystroke at a time. Typing "Hi" fires:
1. `onChange` for "H"
2. `onChange` for "i"

Each character triggers a separate event, just like a real user. This is different from `fireEvent.change` which fires once.

---

### Q21 🔴 What is wrong with this code?

```javascript
test('interaction test', () => {
  render(<Counter />);
  const button = screen.getByRole('button', { name: /increment/i });

  userEvent.click(button);  // No await!

  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

**Answer:**

Missing `await` and missing `userEvent.setup()`. Two bugs:

```javascript
// ❌ WRONG
userEvent.click(button);  // Returns a promise — not awaited!

// ✅ CORRECT
test('interaction test', async () => {  // async
  const user = userEvent.setup();       // setup first
  render(<Counter />);

  await user.click(screen.getByRole('button', { name: /increment/i }));

  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

Without `await`, the click fires but the assertion runs BEFORE React updates the DOM. Test may pass randomly (race condition).

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 5.2 fireEvent vs userEvent

---

### Q22 🟢 What is the difference between `fireEvent` and `userEvent`?

**Answer:**

| | `fireEvent` | `userEvent` |
|---|---|---|
| Realism | Fires ONE event | Fires ALL events a real user would |
| Typing | One `change` event | `keyDown`, `keyPress`, `keyUp`, `input`, `change` per key |
| Clicking | One `click` | `pointerDown`, `mouseDown`, `pointerUp`, `mouseUp`, `click` |
| Async | Synchronous | Async (returns Promise) |
| Setup | None | `userEvent.setup()` required |

```javascript
// fireEvent.click fires ONLY a click event
fireEvent.click(button);

// userEvent.click fires the full chain:
// pointerEnter → pointerDown → mouseDown → focus →
// pointerUp → mouseUp → click
await user.click(button);
```

**Rule:** Default to `userEvent`. Use `fireEvent` only for edge cases (`scroll`, `resize`, custom events) that `userEvent` doesn't support.

---

### Q23 🟡 When would you use `fireEvent` over `userEvent`?

**Answer:**

```javascript
// 1. Events userEvent doesn't support:
fireEvent.scroll(container, { target: { scrollTop: 500 } });
fireEvent.resize(window);

// 2. Custom/non-standard events:
fireEvent(element, new CustomEvent('myEvent', { detail: { id: 1 } }));

// 3. Testing specific event properties:
fireEvent.keyDown(input, { key: 'Escape', code: 'Escape' });

// 4. When you need synchronous execution (rare):
fireEvent.change(input, { target: { value: 'test' } });
// No await needed — but less realistic
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 5.3 Testing Forms

---

### Q24 🟢 How do you test a form submission?

**Answer:**

```javascript
test('submits contact form with user data', async () => {
  const handleSubmit = jest.fn();
  const user = userEvent.setup();
  render(<ContactForm onSubmit={handleSubmit} />);

  // Fill form fields
  await user.type(screen.getByLabelText(/name/i), 'Alice');
  await user.type(screen.getByLabelText(/email/i), 'alice@test.com');
  await user.type(screen.getByLabelText(/message/i), 'Hello!');

  // Submit
  await user.click(screen.getByRole('button', { name: /send/i }));

  // Assert
  expect(handleSubmit).toHaveBeenCalledWith({
    name: 'Alice',
    email: 'alice@test.com',
    message: 'Hello!',
  });
});
```

---

### Q25 🟡 How do you test form validation error messages?

**Answer:**

```javascript
test('shows error for empty email', async () => {
  const user = userEvent.setup();
  render(<SignupForm />);

  // Submit without filling email
  await user.click(screen.getByRole('button', { name: /sign up/i }));

  // Error should appear
  expect(screen.getByText(/email is required/i)).toBeInTheDocument();

  // Error should disappear after filling
  await user.type(screen.getByLabelText(/email/i), 'alice@test.com');
  expect(screen.queryByText(/email is required/i)).not.toBeInTheDocument();
});
```

**Pattern:** Submit empty → check error appears → fill field → check error disappears.

---

### Q26 🟡 How do you test a controlled input?

**Answer:**

```javascript
test('updates input value as user types', async () => {
  const user = userEvent.setup();
  render(<SearchBar />);

  const input = screen.getByRole('textbox', { name: /search/i });

  // Initially empty
  expect(input).toHaveValue('');

  // Type
  await user.type(input, 'react testing');
  expect(input).toHaveValue('react testing');

  // Clear
  await user.clear(input);
  expect(input).toHaveValue('');
});
```

`toHaveValue()` checks the current value of an input, select, or textarea. Part of `@testing-library/jest-dom`.

---

### Q27 🔴 How do you test a form with select, checkbox, and radio?

**Answer:**

```javascript
test('fills complete form', async () => {
  const user = userEvent.setup();
  render(<RegistrationForm />);

  // Text input
  await user.type(screen.getByLabelText(/name/i), 'Alice');

  // Select dropdown
  await user.selectOptions(
    screen.getByRole('combobox', { name: /country/i }),
    'India'
  );
  expect(screen.getByRole('combobox', { name: /country/i })).toHaveValue('IN');

  // Checkbox
  await user.click(screen.getByRole('checkbox', { name: /agree/i }));
  expect(screen.getByRole('checkbox', { name: /agree/i })).toBeChecked();

  // Radio
  await user.click(screen.getByRole('radio', { name: /premium/i }));
  expect(screen.getByRole('radio', { name: /premium/i })).toBeChecked();
  expect(screen.getByRole('radio', { name: /free/i })).not.toBeChecked();
});
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 5.4 selectOptions, tab, hover, keyboard

---

### Q28 🟡 How do you test keyboard navigation?

**Answer:**

```javascript
test('tabs through form fields in order', async () => {
  const user = userEvent.setup();
  render(<LoginForm />);

  // Tab to first field
  await user.tab();
  expect(screen.getByLabelText(/email/i)).toHaveFocus();

  // Tab to second field
  await user.tab();
  expect(screen.getByLabelText(/password/i)).toHaveFocus();

  // Tab to button
  await user.tab();
  expect(screen.getByRole('button', { name: /login/i })).toHaveFocus();
});
```

`toHaveFocus()` is from `jest-dom` — checks which element currently has focus.

---

### Q29 🟡 How do you test hover effects?

**Answer:**

```javascript
test('shows tooltip on hover', async () => {
  const user = userEvent.setup();
  render(<IconButton tooltip="Delete item" />);

  // Tooltip not visible initially
  expect(screen.queryByText('Delete item')).not.toBeInTheDocument();

  // Hover
  await user.hover(screen.getByRole('button'));
  expect(screen.getByText('Delete item')).toBeInTheDocument();

  // Unhover
  await user.unhover(screen.getByRole('button'));
  expect(screen.queryByText('Delete item')).not.toBeInTheDocument();
});
```

---

### Q30 🟡 How do you simulate complex keyboard shortcuts?

**Answer:**

```javascript
test('Ctrl+S saves the document', async () => {
  const handleSave = jest.fn();
  const user = userEvent.setup();
  render(<Editor onSave={handleSave} />);

  // Simulate Ctrl+S
  await user.keyboard('{Control>}s{/Control}');

  expect(handleSave).toHaveBeenCalledTimes(1);
});

// Keyboard syntax:
// {Enter}           → Enter key
// {Escape}          → Escape key
// {Control>}s{/Control} → Hold Ctrl, press s, release Ctrl
// {Shift>}A{/Shift}    → Shift + A
// {Backspace}       → Backspace
// {ArrowDown}       → Down arrow
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 6.1 async/await in Tests

---

### Q31 🟢 How do you test async operations?

**Answer:**

```javascript
// Component fetches data on mount:
const UserList = () => {
  const [users, setUsers] = useState([]);
  useEffect(() => {
    fetch('/api/users').then(r => r.json()).then(setUsers);
  }, []);
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
};

test('shows users after loading', async () => {
  render(<UserList />);

  // Wait for async data to appear
  const alice = await screen.findByText('Alice');
  expect(alice).toBeInTheDocument();
  expect(screen.getByText('Bob')).toBeInTheDocument();
});
```

`findBy` = `getBy` + `waitFor`. It retries until the element appears or timeout (1000ms default).

---

### Q32 🟡 What is wrong with this test?

```javascript
test('loads data', () => {  // ← no async!
  render(<UserList />);
  expect(screen.findByText('Alice')).toBeInTheDocument();
});
```

**Answer:** Two bugs:

1. `test` is not `async` — `findByText` returns a Promise that's never awaited
2. `expect(Promise).toBeInTheDocument()` — checking a Promise, not an element

```javascript
// ✅ CORRECT
test('loads data', async () => {
  render(<UserList />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();
  // OR:
  const alice = await screen.findByText('Alice');
  expect(alice).toBeInTheDocument();
});
```

This is a common source of **false positives** — the test passes because `expect(Promise).toBeInTheDocument()` doesn't throw.

---

### Q33 🔴 How do you test both loading and loaded states?

**Answer:**

```javascript
test('shows loading then data', async () => {
  render(<UserProfile userId={1} />);

  // 1. Assert loading state (synchronous — shown immediately)
  expect(screen.getByText(/loading/i)).toBeInTheDocument();
  expect(screen.queryByText('Alice')).not.toBeInTheDocument();

  // 2. Wait for data to appear (async)
  expect(await screen.findByText('Alice')).toBeInTheDocument();

  // 3. Assert loading is gone
  expect(screen.queryByText(/loading/i)).not.toBeInTheDocument();
});
```

**Order matters:** Check loading with `getBy` (sync) first, then use `findBy` (async) to wait for data.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 6.2 waitFor()

---

### Q34 🟢 What does `waitFor()` do?

**Answer:**

`waitFor()` repeatedly runs a callback until it passes or times out:

```javascript
await waitFor(() => {
  expect(screen.getByText('Success')).toBeInTheDocument();
});
// Retries every 50ms for up to 1000ms
```

```javascript
// Custom timeout and interval:
await waitFor(
  () => { expect(mockFn).toHaveBeenCalled(); },
  { timeout: 3000, interval: 100 }  // Wait up to 3s, check every 100ms
);
```

---

### Q35 🟡 What is wrong with this `waitFor` usage?

```javascript
test('updates count', async () => {
  const user = userEvent.setup();
  render(<Counter />);

  await waitFor(async () => {
    await user.click(screen.getByRole('button'));
    expect(screen.getByText('Count: 1')).toBeInTheDocument();
  });
});
```

**Answer:**

**Never put side effects (clicks, types) inside `waitFor`.** `waitFor` retries the callback multiple times. Each retry would click the button again!

```javascript
// ❌ WRONG — clicks button multiple times
await waitFor(async () => {
  await user.click(button);  // Fires on EVERY retry!
  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});

// ✅ CORRECT — click once, then wait for result
await user.click(screen.getByRole('button'));
await waitFor(() => {
  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

**Rule:** `waitFor` is for **assertions only**. Do actions BEFORE `waitFor`.

---

### Q36 🟡 What is `waitForElementToBeRemoved()`?

**Answer:**

Waits until an element disappears from the DOM:

```javascript
test('hides loading spinner after data loads', async () => {
  render(<Dashboard />);

  // Loading spinner is shown
  expect(screen.getByText(/loading/i)).toBeInTheDocument();

  // Wait for it to disappear
  await waitForElementToBeRemoved(() => screen.queryByText(/loading/i));

  // Now data is shown
  expect(screen.getByText('Dashboard')).toBeInTheDocument();
});
```

**Important:** Pass `queryBy` (not `getBy`) inside the callback. If the element is already gone, `getBy` throws. `queryBy` returns `null` which `waitForElementToBeRemoved` handles gracefully.

---

### Q37 🔴 What is the output?

```javascript
test('waitFor timing', async () => {
  let count = 0;
  render(<div>{count}</div>);

  setTimeout(() => { count = 5; }, 500);

  await waitFor(() => {
    expect(count).toBe(5);
  });

  console.log('passed!');
});
```

**Answer:** The test **FAILS** with timeout.

`waitFor` retries the callback, but `count` is a local variable — React doesn't re-render the component when it changes. Even though `count` becomes 5 after 500ms, `waitFor` checks the **DOM**, and the DOM still shows `0`.

`waitFor` is for waiting on DOM changes, not JavaScript variable changes. For this to work, `count` would need to be React state that triggers a re-render.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 6.3 act()

---

### Q38 🟢 What is `act()` and why does it exist?

**Answer:**

`act()` ensures all state updates, effects, and re-renders are flushed before you assert. React batches updates — without `act()`, you might assert on stale DOM.

```javascript
// You rarely need to call act() directly because:
// ✅ render() wraps in act() automatically
// ✅ userEvent methods wrap in act() automatically
// ✅ findBy and waitFor wrap in act() automatically

// You NEED act() when triggering state updates manually:
import { act } from '@testing-library/react';

test('timer updates count', () => {
  jest.useFakeTimers();
  render(<AutoCounter />);

  act(() => {
    jest.advanceTimersByTime(1000);  // Triggers state update
  });

  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

---

### Q39 🟡 What does the warning "not wrapped in act(...)" mean?

**Answer:**

This warning means a state update happened OUTSIDE of `act()`:

```
Warning: An update to Counter inside a test was not wrapped in act(...).
When testing, code that causes React state updates should be wrapped in act(...).
```

**Common causes:**

```javascript
// 1. Async state update after test ends
// Fix: await async operations or use findBy
test('loads data', async () => {
  render(<UserList />);
  // ❌ Test ends before fetch resolves → warning
  // ✅ Wait for it:
  await screen.findByText('Alice');
});

// 2. Timer-based state update
// Fix: wrap timer advancement in act()
act(() => { jest.advanceTimersByTime(1000); });

// 3. Manual promise resolution
// Fix: use waitFor
await waitFor(() => {
  expect(screen.getByText('Done')).toBeInTheDocument();
});
```

---

### Q40 🟡 When do you need `act()` explicitly?

**Answer:**

Almost never. RTL wraps everything for you. You need it only when:

```javascript
// 1. Advancing fake timers
act(() => { jest.advanceTimersByTime(5000); });

// 2. Manually resolving promises (rare)
let resolve;
const promise = new Promise(r => { resolve = r; });
await act(async () => { resolve('data'); });

// 3. Testing hooks with renderHook
const { result } = renderHook(() => useCounter());
act(() => { result.current.increment(); });
expect(result.current.count).toBe(1);
```

**Rule:** If you're writing `act()` a lot, you're probably not using RTL's async utilities correctly. `findBy`, `waitFor`, and `userEvent` already handle it.

---

### Q41 🔴 What is the output?

```javascript
test('counter with act', () => {
  jest.useFakeTimers();
  render(<TimerCounter />); // Increments count every 1 second

  // Without act:
  jest.advanceTimersByTime(3000);
  console.log(screen.getByTestId('count').textContent);  // ?

  // With act:
  act(() => { jest.advanceTimersByTime(3000); });
  console.log(screen.getByTestId('count').textContent);  // ?
});
```

**Answer:**

```
"0"   ← Without act: timers fired but React didn't flush updates to DOM
"6"   ← With act: React processed all state updates, DOM is current
```

Without `act()`, `advanceTimersByTime` triggers the interval callbacks, but React's state updates are queued and never flushed. The DOM stays stale. `act()` forces React to process all pending updates.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 6.4 Fake Timers

---

### Q42 🟡 How do you test `setTimeout` and `setInterval`?

**Answer:**

```javascript
// Component: shows message after 3 seconds
const DelayedMessage = () => {
  const [show, setShow] = useState(false);
  useEffect(() => { setTimeout(() => setShow(true), 3000); }, []);
  return show ? <p>Hello!</p> : null;
};

test('shows message after 3 seconds', () => {
  jest.useFakeTimers();
  render(<DelayedMessage />);

  // Nothing yet
  expect(screen.queryByText('Hello!')).not.toBeInTheDocument();

  // Fast-forward 3 seconds
  act(() => { jest.advanceTimersByTime(3000); });

  // Now it shows
  expect(screen.getByText('Hello!')).toBeInTheDocument();

  jest.useRealTimers();  // Restore real timers
});
```

---

### Q43 🟡 What is the difference between `advanceTimersByTime`, `runAllTimers`, and `runOnlyPendingTimers`?

**Answer:**

```javascript
jest.useFakeTimers();

setTimeout(() => console.log('A'), 1000);
setTimeout(() => console.log('B'), 2000);
setTimeout(() => console.log('C'), 5000);

// advanceTimersByTime(3000) — fast-forward exactly 3 seconds
jest.advanceTimersByTime(3000);
// Output: 'A', 'B' (C hasn't fired yet)

// runAllTimers() — run ALL pending timers (no matter how far ahead)
jest.runAllTimers();
// Output: 'A', 'B', 'C'

// runOnlyPendingTimers() — run only currently scheduled timers
// (Useful when timers schedule NEW timers — prevents infinite loop)
jest.runOnlyPendingTimers();
```

| Method | What it does |
|---|---|
| `advanceTimersByTime(ms)` | Move clock forward by exact ms |
| `runAllTimers()` | Execute ALL timers (even future ones) |
| `runOnlyPendingTimers()` | Execute only currently queued timers |
| `jest.now()` | Get current fake time |

---

### Q44 🟡 How do you test a debounced search?

**Answer:**

```javascript
test('searches after user stops typing for 500ms', async () => {
  jest.useFakeTimers();
  const user = userEvent.setup({ advanceTimers: jest.advanceTimersByTime });
  render(<SearchBar onSearch={mockSearch} />);

  await user.type(screen.getByRole('textbox'), 'react');

  // Debounce hasn't fired yet
  expect(mockSearch).not.toHaveBeenCalled();

  // Fast-forward past debounce delay
  act(() => { jest.advanceTimersByTime(500); });

  // Now it fires
  expect(mockSearch).toHaveBeenCalledWith('react');
  expect(mockSearch).toHaveBeenCalledTimes(1);

  jest.useRealTimers();
});
```

**Critical:** When using fake timers with `userEvent`, pass `advanceTimers` to `setup()`. Otherwise `userEvent` delays hang forever.

---

### Q45 🔴 What is wrong with this test?

```javascript
test('auto-save', () => {
  jest.useFakeTimers();
  render(<Editor />);

  // User types, component auto-saves every 5 seconds
  fireEvent.change(screen.getByRole('textbox'), { target: { value: 'Hello' } });

  jest.runAllTimers();  // ← Problem?

  expect(mockSave).toHaveBeenCalled();
});
// Test never completes — hangs!
```

**Answer:**

If the auto-save uses `setInterval`, `runAllTimers()` tries to run ALL future intervals — which is infinite. It hangs forever.

```javascript
// ❌ WRONG — infinite loop with setInterval
jest.runAllTimers();

// ✅ FIX 1: Use runOnlyPendingTimers (one tick)
jest.runOnlyPendingTimers();

// ✅ FIX 2: Use advanceTimersByTime (exact control)
act(() => { jest.advanceTimersByTime(5000); });

// ✅ FIX 3: Limit recursive timers
jest.useFakeTimers({ timerLimit: 10 });
```

**Rule:** Never use `runAllTimers()` with `setInterval`. Use `advanceTimersByTime` or `runOnlyPendingTimers` instead.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 7.1 jest.fn()

---

### Q46 🟢 What is `jest.fn()` and how do you use it?

**Answer:**

`jest.fn()` creates a mock function — a fake that tracks how it's called:

```javascript
const mockCallback = jest.fn();
mockCallback('hello', 42);

// What was it called with?
expect(mockCallback).toHaveBeenCalledWith('hello', 42);
expect(mockCallback).toHaveBeenCalledTimes(1);

// Default return value is undefined
console.log(mockCallback()); // undefined
```

**Most common use:** Passing as props to test callbacks:

```javascript
test('calls onDelete when delete button clicked', async () => {
  const handleDelete = jest.fn();
  const user = userEvent.setup();
  render(<TodoItem text="Buy milk" onDelete={handleDelete} />);

  await user.click(screen.getByRole('button', { name: /delete/i }));

  expect(handleDelete).toHaveBeenCalledTimes(1);
});
```

---

### Q47 🟢 How do you make a mock return a value?

**Answer:**

```javascript
const mock = jest.fn();

// Return same value every time
mock.mockReturnValue(42);
console.log(mock()); // 42
console.log(mock()); // 42

// Return different values on consecutive calls
mock.mockReturnValueOnce('first').mockReturnValueOnce('second');
console.log(mock()); // 'first'
console.log(mock()); // 'second'
console.log(mock()); // undefined (no more "once" values)

// Return a promise (async functions)
mock.mockResolvedValue({ id: 1, name: 'Alice' });
const result = await mock(); // { id: 1, name: 'Alice' }

// Return a rejected promise
mock.mockRejectedValue(new Error('Failed'));
await expect(mock()).rejects.toThrow('Failed');
```

---

### Q48 🟡 What is `mockImplementation`?

**Answer:**

`mockImplementation` gives the mock custom logic — like writing a function body:

```javascript
// Simple return:
const mock = jest.fn().mockImplementation((a, b) => a + b);
console.log(mock(2, 3)); // 5

// One-time override:
mock.mockImplementationOnce(() => 999);
console.log(mock(2, 3)); // 999 (once)
console.log(mock(2, 3)); // 5 (back to original)

// Simulating an API:
const fetchUser = jest.fn().mockImplementation(async (id) => {
  if (id === 1) return { name: 'Alice' };
  throw new Error('Not found');
});
```

**`mockReturnValue` vs `mockImplementation`:**
- `mockReturnValue(42)` — always returns 42, ignores arguments
- `mockImplementation((x) => x * 2)` — uses arguments, has logic

---

### Q49 🔴 What is the output?

```javascript
const fn = jest.fn()
  .mockReturnValueOnce(1)
  .mockReturnValueOnce(2)
  .mockReturnValue(99);

console.log(fn());  // ?
console.log(fn());  // ?
console.log(fn());  // ?
console.log(fn());  // ?
```

**Answer:**

```
1    ← first call: uses first mockReturnValueOnce
2    ← second call: uses second mockReturnValueOnce
99   ← third call: "once" values exhausted, falls back to mockReturnValue
99   ← fourth call: same fallback
```

`mockReturnValueOnce` is consumed in order. After all "once" values are used, `mockReturnValue` is the permanent fallback.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 7.2 jest.mock()

---

### Q50 🟢 What does `jest.mock()` do?

**Answer:**

`jest.mock()` replaces an entire module with a fake version:

```javascript
// api.js
export const fetchUsers = () => fetch('/api/users').then(r => r.json());

// UserList.test.js
import { fetchUsers } from './api';
import UserList from './UserList';

jest.mock('./api');  // All exports become jest.fn()

test('shows users', async () => {
  fetchUsers.mockResolvedValue([
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' },
  ]);

  render(<UserList />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();
});
```

After `jest.mock('./api')`, every exported function becomes `jest.fn()` automatically.

---

### Q51 🟡 How do you mock a default export vs named exports?

**Answer:**

```javascript
// Module: utils.js
export default function formatDate(d) { /* ... */ }
export function formatPrice(p) { /* ... */ }

// --- Mocking named export ---
jest.mock('./utils', () => ({
  ...jest.requireActual('./utils'),    // Keep other exports real
  formatPrice: jest.fn(() => '$0.00'), // Mock only this one
}));

// --- Mocking default export ---
jest.mock('./utils', () => ({
  __esModule: true,                          // Required for ES modules
  default: jest.fn(() => '2024-01-01'),      // Mock the default
  formatPrice: jest.fn(() => '$0.00'),       // Mock named too
}));

// --- Mocking entire module (auto-mock) ---
jest.mock('./utils');
// All exports become jest.fn() with no implementation
```

**`__esModule: true`** tells Jest this is an ES module with a `default` export. Without it, `import formatDate from './utils'` gets the whole mock object instead of the default.

---

### Q52 🟡 What does `jest.requireActual()` do?

**Answer:**

It imports the REAL module, even inside a `jest.mock()` factory. Used for partial mocking:

```javascript
// You want to mock ONE function but keep everything else real:
jest.mock('./utils', () => {
  const actual = jest.requireActual('./utils');
  return {
    ...actual,                         // Keep all real exports
    fetchData: jest.fn(() => 'mocked'), // Override just this one
  };
});

// Now:
import { fetchData, formatDate } from './utils';
fetchData();    // 'mocked' — fake
formatDate();   // Real implementation — untouched
```

**Without `jest.requireActual`:** You'd have to manually recreate every export you want to keep.

---

### Q53 🔴 Why is `jest.mock()` hoisted?

**Answer:**

Jest automatically moves `jest.mock()` calls to the TOP of the file — above all imports:

```javascript
// What you write:
import { fetchUsers } from './api';
jest.mock('./api');

// What Jest actually runs:
jest.mock('./api');               // ← HOISTED to top
import { fetchUsers } from './api'; // Now imports the mocked version
```

**Why it matters:**

```javascript
// ❌ WRONG — can't use imported variable in mock factory
import { API_URL } from './config';
jest.mock('./api', () => ({
  fetch: jest.fn(() => API_URL),  // ERROR: API_URL is not defined yet!
}));

// ✅ FIX — use require inside factory (runs at call time, not import time)
jest.mock('./api', () => ({
  fetch: jest.fn(() => require('./config').API_URL),
}));

// ✅ FIX 2 — variables starting with "mock" are allowed
const mockUrl = 'http://test.com';  // "mock" prefix = hoisted too
jest.mock('./api', () => ({
  fetch: jest.fn(() => mockUrl),
}));
```

**Rule:** Inside `jest.mock()` factory, you can't reference outer scope variables UNLESS they start with "mock".

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 7.3 jest.spyOn()

---

### Q54 🟡 What is `jest.spyOn()` and how is it different from `jest.fn()`?

**Answer:**

| | `jest.fn()` | `jest.spyOn()` |
|---|---|---|
| Creates | New fake function | Wraps an EXISTING method |
| Original | No original exists | Original still callable |
| Restore | Nothing to restore | `mockRestore()` brings back original |
| Use when | Props, callbacks | Spying on module/object methods |

```javascript
// jest.fn() — create a brand new fake
const onClick = jest.fn();
render(<Button onClick={onClick} />);

// jest.spyOn() — spy on an existing method
const spy = jest.spyOn(console, 'error');
render(<BrokenComponent />);
expect(spy).toHaveBeenCalledWith(expect.stringContaining('Warning'));
spy.mockRestore();  // Put console.error back to normal
```

---

### Q55 🟡 How do you spy on and mock a method?

**Answer:**

```javascript
import * as api from './api';

// Spy AND override implementation:
const spy = jest.spyOn(api, 'fetchUsers').mockResolvedValue([
  { id: 1, name: 'Alice' },
]);

test('shows users', async () => {
  render(<UserList />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();
  expect(spy).toHaveBeenCalledTimes(1);
});

afterEach(() => {
  spy.mockRestore();  // Restore real fetchUsers
});
```

**`spyOn` without mock** — just tracks calls without changing behavior:

```javascript
const spy = jest.spyOn(api, 'fetchUsers');
// fetchUsers still works normally, but spy tracks calls
render(<UserList />);
expect(spy).toHaveBeenCalled();
```

---

### Q56 🔴 What is the output?

```javascript
const obj = {
  greet(name) { return `Hello, ${name}`; }
};

const spy = jest.spyOn(obj, 'greet');

console.log(obj.greet('Alice'));   // ?

spy.mockReturnValue('Mocked!');
console.log(obj.greet('Bob'));     // ?

spy.mockRestore();
console.log(obj.greet('Charlie')); // ?
```

**Answer:**

```
"Hello, Alice"    ← spyOn without mock: original still runs
"Mocked!"         ← after mockReturnValue: overridden
"Hello, Charlie"  ← after mockRestore: original restored
```

`spyOn` preserves the original by default. `mockRestore()` puts it back after you've overridden it. This is impossible with `jest.fn()`.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 7.4 mockClear vs mockReset vs mockRestore

---

### Q57 🟡 What is the difference between `mockClear`, `mockReset`, and `mockRestore`?

**Answer:**

| | `mockClear()` | `mockReset()` | `mockRestore()` |
|---|---|---|---|
| Clears call history | ✅ | ✅ | ✅ |
| Removes implementation | ❌ | ✅ | ✅ |
| Restores original | ❌ | ❌ | ✅ (spyOn only) |

```javascript
const mock = jest.fn().mockReturnValue(42);
mock('a'); mock('b');

// mockClear — reset call tracking only
mock.mockClear();
console.log(mock.mock.calls);     // [] — calls cleared
console.log(mock());              // 42 — implementation still works

// mockReset — clear calls AND remove implementation
mock.mockReset();
console.log(mock.mock.calls);     // [] — calls cleared
console.log(mock());              // undefined — implementation gone

// mockRestore — only works with spyOn
const spy = jest.spyOn(console, 'log');
spy.mockRestore();                // console.log is real again
```

**In `afterEach`:**

```javascript
afterEach(() => {
  jest.clearAllMocks();    // mockClear on ALL mocks
  jest.resetAllMocks();    // mockReset on ALL mocks
  jest.restoreAllMocks();  // mockRestore on ALL mocks (best for spyOn)
});
```

---

### Q58 🔴 What is the output?

```javascript
const fn = jest.fn().mockReturnValue('hello');

fn('a');
fn('b');
console.log(fn.mock.calls.length);  // ?

fn.mockClear();
console.log(fn.mock.calls.length);  // ?
console.log(fn());                  // ?

fn.mockReset();
console.log(fn());                  // ?
```

**Answer:**

```
2           ← called twice ('a' and 'b')
0           ← mockClear cleared call history
"hello"     ← mockClear did NOT remove mockReturnValue
undefined   ← mockReset removed mockReturnValue too
```

**One-liner:** `mockClear` = forget calls. `mockReset` = forget everything. `mockRestore` = go back to original.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 8.1 Mocking fetch / axios

---

### Q59 🟢 How do you mock `fetch` in tests?

**Answer:**

```javascript
// Method 1: Global mock
global.fetch = jest.fn();

test('fetches and displays users', async () => {
  fetch.mockResolvedValueOnce({
    ok: true,
    json: async () => [{ id: 1, name: 'Alice' }],
  });

  render(<UserList />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();
  expect(fetch).toHaveBeenCalledWith('/api/users');
});

afterEach(() => { fetch.mockClear(); });
```

```javascript
// Method 2: jest.spyOn
jest.spyOn(global, 'fetch').mockResolvedValue({
  ok: true,
  json: async () => [{ id: 1, name: 'Alice' }],
});
```

**Common mistake:** Forgetting to mock `response.json()` — `fetch` returns a Response object, not data directly.

---

### Q60 🟡 How do you mock error responses?

**Answer:**

```javascript
// Network error (fetch itself fails)
fetch.mockRejectedValueOnce(new Error('Network error'));

// HTTP error (server returns 4xx/5xx)
fetch.mockResolvedValueOnce({
  ok: false,
  status: 404,
  json: async () => ({ message: 'Not found' }),
});

test('shows error message on API failure', async () => {
  fetch.mockRejectedValueOnce(new Error('Network error'));
  render(<UserList />);

  expect(await screen.findByText(/error/i)).toBeInTheDocument();
  expect(screen.queryByText('Alice')).not.toBeInTheDocument();
});
```

**Test both:** Network errors (`mockRejectedValue`) AND HTTP errors (`ok: false`). They're different failure modes.

---

### Q61 🟡 How do you mock axios?

**Answer:**

```javascript
import axios from 'axios';
jest.mock('axios');

test('fetches users with axios', async () => {
  axios.get.mockResolvedValue({
    data: [{ id: 1, name: 'Alice' }],
  });

  render(<UserList />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();
  expect(axios.get).toHaveBeenCalledWith('/api/users');
});
```

`jest.mock('axios')` auto-mocks all methods (`get`, `post`, `put`, etc.) as `jest.fn()`.

**axios vs fetch mocking difference:**
- `fetch` → mock returns `{ ok, json() }`
- `axios` → mock returns `{ data }` directly

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 8.2 MSW — Mock Service Worker

---

### Q62 🟢 What is MSW and why use it over `jest.mock(fetch)`?

**Answer:**

MSW intercepts HTTP requests at the **network level** — your component code runs exactly like production:

| | `jest.mock(fetch)` | MSW |
|---|---|---|
| Intercepts at | Function level | Network level |
| Component code | Knows about mock | No changes needed |
| Tests | Tightly coupled | Realistic |
| Covers | Only `fetch` OR `axios` | All HTTP (fetch, axios, XMLHttpRequest) |
| Reusable | Per test file | Shared across tests |

```bash
npm install --save-dev msw
```

---

### Q63 🟡 How do you set up MSW for testing?

**Answer:**

```javascript
// src/mocks/handlers.js
import { http, HttpResponse } from 'msw';

export const handlers = [
  http.get('/api/users', () => {
    return HttpResponse.json([
      { id: 1, name: 'Alice' },
      { id: 2, name: 'Bob' },
    ]);
  }),

  http.post('/api/users', async ({ request }) => {
    const body = await request.json();
    return HttpResponse.json({ id: 3, ...body }, { status: 201 });
  }),
];
```

```javascript
// src/mocks/server.js
import { setupServer } from 'msw/node';
import { handlers } from './handlers';
export const server = setupServer(...handlers);
```

```javascript
// jest.setup.js (or setupFilesAfterSetup)
import { server } from './mocks/server';

beforeAll(() => server.listen());           // Start intercepting
afterEach(() => server.resetHandlers());    // Reset to default handlers
afterAll(() => server.close());             // Stop intercepting
```

```javascript
// UserList.test.js — no mocking needed!
test('shows users from API', async () => {
  render(<UserList />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();
  expect(screen.getByText('Bob')).toBeInTheDocument();
});
```

---

### Q64 🟡 How do you override handlers for a specific test?

**Answer:**

```javascript
import { http, HttpResponse } from 'msw';
import { server } from '../mocks/server';

test('shows error when API fails', async () => {
  // Override the default handler for THIS test only
  server.use(
    http.get('/api/users', () => {
      return HttpResponse.json(
        { message: 'Internal Server Error' },
        { status: 500 }
      );
    })
  );

  render(<UserList />);
  expect(await screen.findByText(/error/i)).toBeInTheDocument();
});

// After this test, afterEach → server.resetHandlers()
// restores the default success handler
```

**Pattern:**
1. Default handlers return success responses (happy path)
2. Individual tests use `server.use()` to override for error/edge cases
3. `resetHandlers()` in `afterEach` restores defaults

---

### Q65 🔴 How do you simulate a network delay with MSW?

**Answer:**

```javascript
import { http, HttpResponse, delay } from 'msw';

test('shows loading state during slow API call', async () => {
  server.use(
    http.get('/api/users', async () => {
      await delay(2000);  // 2 second delay
      return HttpResponse.json([{ id: 1, name: 'Alice' }]);
    })
  );

  render(<UserList />);

  // Loading state shown immediately
  expect(screen.getByText(/loading/i)).toBeInTheDocument();

  // Wait for data
  expect(await screen.findByText('Alice', {}, { timeout: 3000 })).toBeInTheDocument();

  // Loading gone
  expect(screen.queryByText(/loading/i)).not.toBeInTheDocument();
});
```

**delay options:**
- `delay(ms)` — exact delay
- `delay('infinite')` — never responds (test timeout, abort controller)
- `delay('real')` — realistic random delay

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 8.3 Mocking React Router

---

### Q66 🟡 How do you test components that use React Router?

**Answer:**

```javascript
import { MemoryRouter } from 'react-router-dom';

// Wrap component in MemoryRouter with initial route:
test('shows dashboard at /dashboard', () => {
  render(
    <MemoryRouter initialEntries={['/dashboard']}>
      <App />
    </MemoryRouter>
  );

  expect(screen.getByText('Dashboard')).toBeInTheDocument();
});

test('shows 404 for unknown route', () => {
  render(
    <MemoryRouter initialEntries={['/nonexistent']}>
      <App />
    </MemoryRouter>
  );

  expect(screen.getByText('Page Not Found')).toBeInTheDocument();
});
```

`MemoryRouter` keeps history in memory — no real browser URL needed.

---

### Q67 🔴 How do you mock `useNavigate` and `useParams`?

**Answer:**

```javascript
const mockNavigate = jest.fn();

jest.mock('react-router-dom', () => ({
  ...jest.requireActual('react-router-dom'),
  useNavigate: () => mockNavigate,
  useParams: () => ({ id: '42' }),
}));

test('navigates to home on logout', async () => {
  const user = userEvent.setup();
  render(
    <MemoryRouter>
      <Header />
    </MemoryRouter>
  );

  await user.click(screen.getByRole('button', { name: /logout/i }));
  expect(mockNavigate).toHaveBeenCalledWith('/');
});

test('shows product with ID from params', () => {
  render(
    <MemoryRouter>
      <ProductPage />
    </MemoryRouter>
  );

  // useParams returns { id: '42' }
  expect(screen.getByText('Product #42')).toBeInTheDocument();
});
```

**Key:** Use `jest.requireActual` to keep real router components (MemoryRouter, Link, etc.) while mocking only hooks.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 8.4 Mocking Browser APIs

---

### Q68 🟡 How do you mock `localStorage`?

**Answer:**

```javascript
// jest jsdom includes a basic localStorage, but you can mock it:
beforeEach(() => {
  jest.spyOn(Storage.prototype, 'getItem');
  jest.spyOn(Storage.prototype, 'setItem');
});

afterEach(() => {
  jest.restoreAllMocks();
  localStorage.clear();
});

test('saves theme preference', async () => {
  const user = userEvent.setup();
  render(<ThemeToggle />);

  await user.click(screen.getByRole('button', { name: /dark mode/i }));

  expect(localStorage.setItem).toHaveBeenCalledWith('theme', 'dark');
});

test('loads saved theme', () => {
  Storage.prototype.getItem.mockReturnValue('dark');
  render(<ThemeToggle />);

  expect(screen.getByText('Dark Mode: ON')).toBeInTheDocument();
});
```

---

### Q69 🔴 How do you mock `window.matchMedia`?

**Answer:**

jsdom doesn't implement `matchMedia`. You must mock it:

```javascript
// jest.setup.js or beforeAll
Object.defineProperty(window, 'matchMedia', {
  writable: true,
  value: jest.fn().mockImplementation(query => ({
    matches: false,                     // Default: light mode
    media: query,
    onchange: null,
    addListener: jest.fn(),
    removeListener: jest.fn(),
    addEventListener: jest.fn(),
    removeEventListener: jest.fn(),
    dispatchEvent: jest.fn(),
  })),
});

// Override for specific test:
test('renders dark mode styles', () => {
  window.matchMedia.mockImplementation(query => ({
    matches: query === '(prefers-color-scheme: dark)',
    media: query,
    onchange: null,
    addListener: jest.fn(),
    removeListener: jest.fn(),
    addEventListener: jest.fn(),
    removeEventListener: jest.fn(),
    dispatchEvent: jest.fn(),
  }));

  render(<App />);
  expect(screen.getByTestId('app')).toHaveClass('dark');
});
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 9.1 Snapshot Testing

---

### Q70 🟢 What is snapshot testing?

**Answer:**

Jest saves a rendered component's output and compares future renders against it:

```javascript
test('renders correctly', () => {
  const { container } = render(<Button label="Save" />);
  expect(container).toMatchSnapshot();
});
// First run: creates __snapshots__/Button.test.js.snap
// Next runs: compares current output to saved snapshot
```

If the output changes, the test fails:

```
Snapshot failed:
- <button class="btn-primary">Save</button>
+ <button class="btn-blue">Save</button>
```

Update snapshots: `npx jest --updateSnapshot` or `npx jest -u`.

---

### Q71 🟡 When should you use snapshot testing and when should you avoid it?

**Answer:**

| ✅ Good for | ❌ Bad for |
|---|---|
| Catching unintended UI changes | Large, deeply nested components |
| Simple, stable components | Components with dynamic data (dates, IDs) |
| Detecting CSS class changes | Testing behavior/interactions |
| Quick coverage for static content | Replacing behavioral tests |

```javascript
// ✅ Good snapshot candidate — small, stable
test('renders logo', () => {
  const { container } = render(<Logo size="large" />);
  expect(container).toMatchSnapshot();
});

// ❌ Bad snapshot candidate — too large, too dynamic
test('renders dashboard', () => {
  const { container } = render(<Dashboard />);
  expect(container).toMatchSnapshot();
  // Snapshot is 500+ lines, changes with every API response
});
```

**Rule:** Snapshots catch **unintended** changes. Behavioral tests verify **intended** behavior. Use both, but lean toward behavioral.

---

### Q72 🔴 What is `toMatchInlineSnapshot` and how is it different?

**Answer:**

```javascript
// toMatchSnapshot — saves to external .snap file
expect(container).toMatchSnapshot();

// toMatchInlineSnapshot — saves INSIDE the test file
expect(container).toMatchInlineSnapshot();
// First run, Jest writes the snapshot into your test:
expect(container).toMatchInlineSnapshot(`
  <div>
    <button class="btn-primary">Save</button>
  </div>
`);
```

| | `toMatchSnapshot` | `toMatchInlineSnapshot` |
|---|---|---|
| Stored in | `__snapshots__/` folder | Inside the test file itself |
| Readability | Must open separate file | Visible right in the test |
| Best for | Large snapshots | Small 1-5 line outputs |
| Review | Harder to review in PRs | Easy to review in PRs |

**Pro tip for dynamic values:**

```javascript
expect(user).toMatchInlineSnapshot(
  { id: expect.any(Number), createdAt: expect.any(String) },
  `
    Object {
      "id": Any<Number>,
      "name": "Alice",
      "createdAt": Any<String>,
    }
  `
);
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# Quick Reference — All 72 Questions at a Glance

| # | Topic | Question Summary | Difficulty |
|---|-------|-----------------|------------|
| 1 | render/screen | What does render() do | 🟢 |
| 2 | render/screen | What is the screen object | 🟢 |
| 3 | render/screen | rerender() usage | 🟡 |
| 4 | render/screen | unmount() usage | 🟡 |
| 5 | getByRole | Why getByRole is preferred | 🟢 |
| 6 | getByRole | name option and accessible name | 🟡 |
| 7 | getByRole | logRoles and query priority | 🔴 |
| 8 | Queries | getByText vs getByLabelText vs getByTestId | 🟢 |
| 9 | Queries | Multiple queries finding same element | 🟡 |
| 10 | Queries | Exact vs regex matching | 🟡 |
| 11 | getBy/queryBy/findBy | Three query variants compared | 🟢 |
| 12 | getBy/queryBy/findBy | getBy throws on not found | 🟢 |
| 13 | getBy/queryBy/findBy | findBy vs waitFor | 🟡 |
| 14 | getBy/queryBy/findBy | Async component query output | 🔴 |
| 15 | within/debug | within() for scoped queries | 🟡 |
| 16 | within/debug | screen.debug() usage | 🟡 |
| 17 | within/debug | logRoles() for discovering roles | 🔴 |
| 18 | userEvent | setup, click, type, clear basics | 🟢 |
| 19 | userEvent | userEvent.clear() | 🟢 |
| 20 | userEvent | type fires per-character events | 🟡 |
| 21 | userEvent | Missing await and setup bugs | 🔴 |
| 22 | fireEvent | fireEvent vs userEvent | 🟢 |
| 23 | fireEvent | When to use fireEvent over userEvent | 🟡 |
| 24 | Forms | Testing form submission | 🟢 |
| 25 | Forms | Testing validation errors | 🟡 |
| 26 | Forms | Testing controlled inputs | 🟡 |
| 27 | Forms | Select, checkbox, radio testing | 🔴 |
| 28 | Keyboard | Tab navigation testing | 🟡 |
| 29 | Hover | Tooltip hover testing | 🟡 |
| 30 | Keyboard | Complex keyboard shortcuts | 🟡 |
| 31 | Async | Testing async data fetching | 🟢 |
| 32 | Async | Missing async/await bugs | 🟡 |
| 33 | Async | Loading then loaded state | 🔴 |
| 34 | waitFor | What waitFor does | 🟢 |
| 35 | waitFor | Side effects inside waitFor bug | 🟡 |
| 36 | waitFor | waitForElementToBeRemoved | 🟡 |
| 37 | waitFor | waitFor timing gotcha | 🔴 |
| 38 | act() | What is act() | 🟢 |
| 39 | act() | "not wrapped in act" warning | 🟡 |
| 40 | act() | When you need explicit act() | 🟡 |
| 41 | act() | act vs no-act DOM difference | 🔴 |
| 42 | Timers | Testing setTimeout/setInterval | 🟡 |
| 43 | Timers | advanceTimers vs runAll vs runPending | 🟡 |
| 44 | Timers | Testing debounced search | 🟡 |
| 45 | Timers | runAllTimers infinite loop bug | 🔴 |
| 46 | jest.fn | What is jest.fn() | 🟢 |
| 47 | jest.fn | mockReturnValue and mockResolvedValue | 🟢 |
| 48 | jest.fn | mockImplementation | 🟡 |
| 49 | jest.fn | mockReturnValueOnce chaining output | 🔴 |
| 50 | jest.mock | What jest.mock() does | 🟢 |
| 51 | jest.mock | Default vs named export mocking | 🟡 |
| 52 | jest.mock | jest.requireActual for partial mocking | 🟡 |
| 53 | jest.mock | Why jest.mock is hoisted | 🔴 |
| 54 | jest.spyOn | spyOn vs jest.fn | 🟡 |
| 55 | jest.spyOn | Spy and mock a method | 🟡 |
| 56 | jest.spyOn | spyOn → mockReturnValue → mockRestore | 🔴 |
| 57 | Clear/Reset/Restore | mockClear vs mockReset vs mockRestore | 🟡 |
| 58 | Clear/Reset/Restore | Output difference between clear and reset | 🔴 |
| 59 | fetch mocking | How to mock fetch | 🟢 |
| 60 | fetch mocking | Mocking error responses | 🟡 |
| 61 | axios mocking | How to mock axios | 🟡 |
| 62 | MSW | What is MSW, why use it | 🟢 |
| 63 | MSW | Setting up MSW for tests | 🟡 |
| 64 | MSW | Per-test handler overrides | 🟡 |
| 65 | MSW | Simulating network delay | 🔴 |
| 66 | React Router | Testing with MemoryRouter | 🟡 |
| 67 | React Router | Mocking useNavigate and useParams | 🔴 |
| 68 | Browser APIs | Mocking localStorage | 🟡 |
| 69 | Browser APIs | Mocking window.matchMedia | 🔴 |
| 70 | Snapshots | What is snapshot testing | 🟢 |
| 71 | Snapshots | When to use vs avoid snapshots | 🟡 |
| 72 | Snapshots | toMatchInlineSnapshot | 🔴 |