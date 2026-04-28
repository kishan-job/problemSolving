# React Testing Crash Course — 3 Hour Complete Guide

> **Jest vs Vitest — Quick Reference**
> ```
> Jest      →  jest.fn()   jest.mock()   jest.spyOn()   jest.useFakeTimers()
> Vitest    →  vi.fn()     vi.mock()     vi.spyOn()     vi.useFakeTimers()
> Everything else — describe, it, test, expect, RTL queries — 100% identical
> ```

---

## Table of Contents

* **[Block 1 — 🏗️ Foundation (20 min)](#block-1--️-foundation-20-min)**
  * [What is React Testing & Why](#what-is-react-testing--why)
  * [Types of Testing — Unit, Integration, E2E](#types-of-testing--unit-integration-e2e)
  * [Setup — Jest + RTL / Vitest + RTL](#setup--jest--rtl--vitest--rtl)
  * [First Test — what a test looks like](#first-test--what-a-test-looks-like)
  * [Jest vs Vitest Difference](#jest-vs-vitest-difference)

* **[Block 2 — 🔍 Querying Elements (20 min)](#block-2--querying-elements-20-min)**
  * [getBy vs queryBy vs findBy](#getby-vs-queryby-vs-findby)
  * [ByRole — most recommended](#byrole--most-recommended)
  * [ByText, ByLabelText, ByPlaceholderText](#bytext-bylabeltext-byplaceholdertext)
  * [ByTestId — last resort](#bytestid--last-resort)
  * [Query Priority Order](#query-priority-order)

* **[Block 3 — 🖱️ User Interactions (20 min)](#block-3--️-user-interactions-20-min)**
  * [fireEvent vs userEvent](#fireevent-vs-userevent)
  * [userEvent — click, type, clear](#userevent--click-type-clear)
  * [selectOptions, checkbox, radio](#selectoptions-checkbox-radio)
  * [Form Interactions](#form-interactions)

* **[Block 4 — ⏳ Async Testing (20 min)](#block-4--async-testing-20-min)**
  * [waitFor](#waitfor)
  * [findBy — built-in async queries](#findby--built-in-async-queries)
  * [waitForElementToBeRemoved](#waitforelementtoberemoved)
  * [Testing Loading & Success States](#testing-loading--success-states)
  * [Testing API Calls](#testing-api-calls)

* **[Block 5 — 🧩 Component Testing Patterns (25 min)](#block-5--component-testing-patterns-25-min)**
  * [Testing Props](#testing-props)
  * [Testing Conditional Rendering](#testing-conditional-rendering)
  * [Testing Lists](#testing-lists)
  * [Testing Forms & Validation](#testing-forms--validation)
  * [Testing Error States](#testing-error-states)

* **[Block 6 — 🎭 Mocking (20 min)](#block-6--mocking-20-min)**
  * [jest.fn() / vi.fn() — Mock Functions](#jestfn--vifn--mock-functions)
  * [Mocking Modules](#mocking-modules)
  * [Mocking API Calls — fetch, axios](#mocking-api-calls--fetch-axios)
  * [Mocking Hooks](#mocking-hooks)
  * [MSW — Mock Service Worker](#msw--mock-service-worker)

* **[Block 7 — 🪝 Testing Hooks (15 min)](#block-7--testing-hooks-15-min)**
  * [renderHook](#renderhook)
  * [Testing useState & useEffect](#testing-usestate--useeffect)
  * [Testing Custom Hooks](#testing-custom-hooks)
  * [act() — when and why](#act--when-and-why)

* **[Block 8 — ⚙️ Setup & Config (10 min)](#block-8--️-setup--config-10-min)**
  * [jest.config / vitest.config](#jestconfig--vitestconfig)
  * [Setup Files](#setup-files)
  * [beforeEach, afterEach, beforeAll, afterAll](#beforeeach-aftereach-beforeall-afterall)
  * [describe blocks](#describe-blocks)
  * [screen.debug()](#screendebug)
  * [Common Gotchas & Fixes](#common-gotchas--fixes)

* **[Block 9 — 📌 Topics Not Covered](#block-9--topics-not-covered)**

---

## Block 1 — 🏗️ Foundation (20 min)

### What is React Testing & Why

Testing means writing code that **checks your code works correctly** — automatically.

```
Without tests:               With tests:
change code → manually       change code → run tests →
check in browser →           pass ✅ or fail ❌
hope nothing broke           know immediately what broke
```

**Why test React components:**
- Catch bugs before users do
- Safely refactor without fear
- Document how components should behave
- Confidence when adding new features

```jsx
// This is your component
function Button({ label, onClick }) {
  return <button onClick={onClick}>{label}</button>
}

// This is your test — checks it works
test("renders button with label", () => {
  render(<Button label="Click me" onClick={() => {}} />)
  expect(screen.getByText("Click me")).toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Types of Testing — Unit, Integration, E2E

```
Unit Test
└── Tests one component in isolation
└── Fast, simple, lots of them
└── Example: "Does Button render correctly?"

Integration Test
└── Tests multiple components working together
└── Example: "Does Form submit and show success message?"

E2E Test (End to End)
└── Tests entire app in real browser
└── Slow, fewer of them
└── Tools: Cypress, Playwright
└── Example: "User logs in, adds item to cart, checks out"
```

**Testing Trophy — how many of each:**

```
        /\
       /E2E\          ← few
      /──────\
     /  Integ \       ← some
    /──────────\
   /    Unit    \     ← most
  /______________\
```

> React Testing Library pushes you toward **Integration tests** — test how components work together, not implementation details.

[⬆ Back to Table of Contents](#table-of-contents)

---

### Setup — Jest + RTL / Vitest + RTL

#### Option 1 — Jest + React Testing Library (CRA / older projects)

```bash
# If using Create React App — already set up!
npx create-react-app my-app

# Manual setup
npm install --save-dev jest @testing-library/react @testing-library/jest-dom @testing-library/user-event
npm install --save-dev jest-environment-jsdom babel-jest @babel/core @babel/preset-env @babel/preset-react
```

```js
// jest.config.js
module.exports = {
  testEnvironment: "jsdom",
  setupFilesAfterFramework: ["@testing-library/jest-dom"],
  transform: {
    "^.+\\.(js|jsx)$": "babel-jest"
  }
}
```

```js
// babel.config.js
module.exports = {
  presets: [
    "@babel/preset-env",
    ["@babel/preset-react", { runtime: "automatic" }]
  ]
}
```

#### Option 2 — Vitest + React Testing Library (Vite / modern projects)

```bash
# If using Vite
npm create vite@latest my-app -- --template react

# Install testing deps
npm install --save-dev vitest @testing-library/react @testing-library/jest-dom @testing-library/user-event jsdom
```

```js
// vite.config.js
import { defineConfig } from "vite"
import react from "@vitejs/plugin-react"

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    setupFiles: ["./src/setupTests.js"],
    globals: true   // so you don't need to import describe/it/expect
  }
})
```

#### Both — setup file (same for Jest and Vitest)

```js
// src/setupTests.js
import "@testing-library/jest-dom"
// this gives you extra matchers like toBeInTheDocument, toHaveText etc.
```

#### Run tests

```bash
# Jest
npx jest
npx jest --watch        # watch mode

# Vitest
npx vitest
npx vitest --watch      # watch mode (default)
npx vitest run          # run once
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### First Test — what a test looks like

```jsx
// Button.jsx — component to test
function Button({ label, onClick, disabled }) {
  return (
    <button onClick={onClick} disabled={disabled}>
      {label}
    </button>
  )
}
export default Button
```

```jsx
// Button.test.jsx — test file
import { render, screen } from "@testing-library/react"
import userEvent from "@testing-library/user-event"
import Button from "./Button"

// describe — groups related tests
describe("Button", () => {

  // test / it — single test case (both work same)
  test("renders with label", () => {
    // 1. render — put component in fake DOM
    render(<Button label="Click me" onClick={() => {}} />)

    // 2. screen — query the DOM
    const button = screen.getByText("Click me")

    // 3. expect — assert something is true
    expect(button).toBeInTheDocument()
  })

  it("calls onClick when clicked", async () => {
    const handleClick = jest.fn()  // or vi.fn() for Vitest
    render(<Button label="Click me" onClick={handleClick} />)

    // userEvent — simulate real user interactions
    await userEvent.click(screen.getByText("Click me"))

    expect(handleClick).toHaveBeenCalledTimes(1)
  })

  it("is disabled when disabled prop is true", () => {
    render(<Button label="Click me" onClick={() => {}} disabled={true} />)
    expect(screen.getByText("Click me")).toBeDisabled()
  })

})
```

**Structure of every test:**

```
1. render    → put component in fake DOM
2. query     → find elements with screen.getBy...
3. interact  → userEvent.click / type / etc
4. assert    → expect(...).toBe...
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Jest vs Vitest Difference

Everything is the same **except** mock-related APIs:

```js
// JEST
jest.fn()
jest.mock("./module")
jest.spyOn(object, "method")
jest.useFakeTimers()
jest.clearAllMocks()
jest.resetAllMocks()

// VITEST — just replace jest with vi
vi.fn()
vi.mock("./module")
vi.spyOn(object, "method")
vi.useFakeTimers()
vi.clearAllMocks()
vi.resetAllMocks()
```

```js
// Vitest — import vi at top of file
import { vi } from "vitest"

// Jest — jest is global, no import needed
```

> For the rest of this crash course — all examples use `jest.*`. If you're using Vitest just replace `jest` with `vi` — everything else is identical.

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 2 — 🔍 Querying Elements (20 min)

### getBy vs queryBy vs findBy

Three families of queries — each for a different situation:

```
getBy...
├── finds element — throws error if NOT found
├── throws error if MORE than one found
└── use when: element SHOULD be there

queryBy...
├── finds element — returns NULL if not found (no error)
├── throws error if MORE than one found
└── use when: element SHOULD NOT be there (testing absence)

findBy...
├── finds element — returns PROMISE
├── waits up to 1000ms for element to appear
└── use when: element appears ASYNC (after fetch, setTimeout)
```

```jsx
// getBy — element should exist
const button = screen.getByText("Submit")           // ✅ found
const button = screen.getByText("Not here")         // ❌ throws error

// queryBy — testing absence
const error = screen.queryByText("Error message")   // null if not found — no error
expect(error).not.toBeInTheDocument()               // ✅ use queryBy for this

// findBy — async element
const message = await screen.findByText("Loaded!")  // waits for it to appear
```

```jsx
// Common mistake:
expect(screen.getByText("Error")).not.toBeInTheDocument()  // ❌ throws before expect runs!
expect(screen.queryByText("Error")).not.toBeInTheDocument() // ✅ correct
```

**All three also have getAllBy, queryAllBy, findAllBy — for multiple elements:**

```jsx
screen.getAllByRole("listitem")      // returns array, throws if none
screen.queryAllByRole("listitem")   // returns array, empty array if none
screen.findAllByRole("listitem")    // async, returns array
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### ByRole — most recommended

Query by ARIA role — most accessible and recommended way.

```jsx
// Common roles
screen.getByRole("button")
screen.getByRole("textbox")
screen.getByRole("checkbox")
screen.getByRole("radio")
screen.getByRole("combobox")      // select dropdown
screen.getByRole("heading")       // h1-h6
screen.getByRole("link")          // <a>
screen.getByRole("img")           // <img>
screen.getByRole("list")          // ul, ol
screen.getByRole("listitem")      // li
screen.getByRole("dialog")        // modal
screen.getByRole("alert")         // error messages
screen.getByRole("navigation")    // nav

// With name option — when multiple same roles exist
screen.getByRole("button", { name: "Submit" })
screen.getByRole("button", { name: "Cancel" })
screen.getByRole("textbox", { name: "Email" })    // matches label text
screen.getByRole("heading", { name: "Welcome" })
screen.getByRole("heading", { level: 1 })         // h1 specifically
```

```jsx
// Example
function LoginForm() {
  return (
    <form>
      <label htmlFor="email">Email</label>
      <input id="email" type="email" />
      <button type="submit">Login</button>
    </form>
  )
}

test("renders login form", () => {
  render(<LoginForm />)

  expect(screen.getByRole("textbox", { name: "Email" })).toBeInTheDocument()
  expect(screen.getByRole("button", { name: "Login" })).toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### ByText, ByLabelText, ByPlaceholderText

```jsx
// ByText — find by visible text content
screen.getByText("Welcome back!")
screen.getByText(/welcome/i)              // regex — case insensitive
screen.getByText("Hello", { exact: false }) // partial match

// ByLabelText — find input by its label
// <label htmlFor="email">Email Address</label>
// <input id="email" />
screen.getByLabelText("Email Address")
screen.getByLabelText(/email/i)

// ByPlaceholderText — find by placeholder
// <input placeholder="Enter your email" />
screen.getByPlaceholderText("Enter your email")
screen.getByPlaceholderText(/email/i)

// ByDisplayValue — find input by current value
// <input value="Alice" />
screen.getByDisplayValue("Alice")

// ByAltText — find image by alt text
// <img alt="User avatar" />
screen.getByAltText("User avatar")

// ByTitle — find by title attribute
// <span title="Close">X</span>
screen.getByTitle("Close")
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### ByTestId — last resort

Use **only** when no other query works.

```jsx
// Add data-testid to element
function Spinner() {
  return <div data-testid="loading-spinner" className="spinner" />
}

// Query by testId
screen.getByTestId("loading-spinner")
```

> Use `data-testid` only when the element has no text, role, or label to query by. Overusing testId makes tests brittle and less meaningful.

[⬆ Back to Table of Contents](#table-of-contents)

---

### Query Priority Order

RTL recommends this priority — use the highest one possible:

```
1. getByRole           ← best — mirrors how users/screen readers see page
2. getByLabelText      ← great for form fields
3. getByPlaceholderText
4. getByText           ← good for non-interactive elements
5. getByDisplayValue   ← for inputs with current value
6. getByAltText        ← for images
7. getByTitle          ← for title attributes
8. getByTestId         ← last resort only
```

```jsx
// ❌ Avoid — too implementation-specific
document.querySelector(".submit-btn")
container.querySelector("#form button")

// ✅ Preferred — mirrors user experience
screen.getByRole("button", { name: "Submit" })
screen.getByLabelText("Password")
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 3 — 🖱️ User Interactions (20 min)

### fireEvent vs userEvent

```
fireEvent
├── fires a single DOM event directly
├── simpler, synchronous
└── doesn't simulate real user behavior fully

userEvent
├── simulates real user behavior
├── typing fires: focus → keydown → keypress → input → keyup
├── clicking fires: mouseover → mousemove → mousedown → mouseup → click
└── always prefer userEvent over fireEvent
```

```jsx
import { render, screen, fireEvent } from "@testing-library/react"
import userEvent from "@testing-library/user-event"

// fireEvent — simple but incomplete
fireEvent.click(button)
fireEvent.change(input, { target: { value: "hello" } })

// userEvent — real user simulation (preferred)
await userEvent.click(button)
await userEvent.type(input, "hello")

// userEvent setup — recommended pattern
const user = userEvent.setup()
await user.click(button)
await user.type(input, "hello")
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### userEvent — click, type, clear

```jsx
import userEvent from "@testing-library/user-event"

describe("userEvent examples", () => {
  it("click", async () => {
    const user = userEvent.setup()
    const handleClick = jest.fn()  // vi.fn() for Vitest

    render(<button onClick={handleClick}>Click me</button>)
    await user.click(screen.getByRole("button"))

    expect(handleClick).toHaveBeenCalledTimes(1)
  })

  it("type into input", async () => {
    const user = userEvent.setup()
    render(<input type="text" />)

    const input = screen.getByRole("textbox")
    await user.type(input, "Hello World")

    expect(input).toHaveValue("Hello World")
  })

  it("clear input then type", async () => {
    const user = userEvent.setup()
    render(<input type="text" defaultValue="old value" />)

    const input = screen.getByRole("textbox")
    await user.clear(input)
    await user.type(input, "new value")

    expect(input).toHaveValue("new value")
  })

  it("double click", async () => {
    const user = userEvent.setup()
    const handleDoubleClick = jest.fn()

    render(<button onDoubleClick={handleDoubleClick}>Double click</button>)
    await user.dblClick(screen.getByRole("button"))

    expect(handleDoubleClick).toHaveBeenCalledTimes(1)
  })

  it("hover", async () => {
    const user = userEvent.setup()
    render(
      <button onMouseEnter={() => {}}>Hover me</button>
    )
    await user.hover(screen.getByRole("button"))
    await user.unhover(screen.getByRole("button"))
  })
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### selectOptions, checkbox, radio

```jsx
it("select dropdown option", async () => {
  const user = userEvent.setup()
  render(
    <select>
      <option value="dog">Dog</option>
      <option value="cat">Cat</option>
      <option value="bird">Bird</option>
    </select>
  )

  await user.selectOptions(screen.getByRole("combobox"), "cat")
  expect(screen.getByRole("combobox")).toHaveValue("cat")
})

it("check checkbox", async () => {
  const user = userEvent.setup()
  render(<input type="checkbox" />)

  const checkbox = screen.getByRole("checkbox")
  expect(checkbox).not.toBeChecked()

  await user.click(checkbox)
  expect(checkbox).toBeChecked()

  await user.click(checkbox)
  expect(checkbox).not.toBeChecked()
})

it("select radio button", async () => {
  const user = userEvent.setup()
  render(
    <>
      <input type="radio" name="size" value="small" id="small" />
      <label htmlFor="small">Small</label>
      <input type="radio" name="size" value="large" id="large" />
      <label htmlFor="large">Large</label>
    </>
  )

  await user.click(screen.getByLabelText("Large"))
  expect(screen.getByLabelText("Large")).toBeChecked()
  expect(screen.getByLabelText("Small")).not.toBeChecked()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Form Interactions

```jsx
// Component
function LoginForm({ onSubmit }) {
  const [email, setEmail] = useState("")
  const [password, setPassword] = useState("")

  return (
    <form onSubmit={(e) => { e.preventDefault(); onSubmit({ email, password }) }}>
      <label htmlFor="email">Email</label>
      <input id="email" type="email" value={email} onChange={e => setEmail(e.target.value)} />

      <label htmlFor="password">Password</label>
      <input id="password" type="password" value={password} onChange={e => setPassword(e.target.value)} />

      <button type="submit">Login</button>
    </form>
  )
}

// Test
test("submits form with email and password", async () => {
  const user = userEvent.setup()
  const handleSubmit = jest.fn()  // vi.fn() for Vitest

  render(<LoginForm onSubmit={handleSubmit} />)

  await user.type(screen.getByLabelText("Email"), "alice@example.com")
  await user.type(screen.getByLabelText("Password"), "password123")
  await user.click(screen.getByRole("button", { name: "Login" }))

  expect(handleSubmit).toHaveBeenCalledWith({
    email: "alice@example.com",
    password: "password123"
  })
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 4 — ⏳ Async Testing (20 min)

### waitFor

Use `waitFor` when something happens **after** an event — state update, API call, timeout.

```jsx
import { render, screen, waitFor } from "@testing-library/react"

test("shows success message after submit", async () => {
  const user = userEvent.setup()
  render(<ContactForm />)

  await user.type(screen.getByLabelText("Message"), "Hello!")
  await user.click(screen.getByRole("button", { name: "Send" }))

  // waitFor — keeps checking until assertion passes or timeout
  await waitFor(() => {
    expect(screen.getByText("Message sent!")).toBeInTheDocument()
  })
})

// waitFor with timeout
await waitFor(() => {
  expect(screen.getByText("Loaded")).toBeInTheDocument()
}, { timeout: 3000 })  // default is 1000ms

// waitFor — multiple assertions
await waitFor(() => {
  expect(screen.getByText("Success")).toBeInTheDocument()
  expect(screen.queryByText("Loading...")).not.toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### findBy — built-in async queries

`findBy` = `getBy` + `waitFor` combined — simpler for async elements.

```jsx
// findBy waits automatically — no need for waitFor
test("shows user name after loading", async () => {
  render(<UserProfile userId={1} />)

  // automatically waits up to 1000ms
  const name = await screen.findByText("Alice")
  expect(name).toBeInTheDocument()
})

// findAllBy — multiple async elements
const items = await screen.findAllByRole("listitem")
expect(items).toHaveLength(3)

// findBy vs waitFor — when to use which
// findBy   → waiting for a specific element to appear
// waitFor  → waiting for any assertion to pass (more flexible)

// ✅ use findBy
const heading = await screen.findByRole("heading", { name: "Dashboard" })

// ✅ use waitFor
await waitFor(() => {
  expect(mockFn).toHaveBeenCalledTimes(1)
  expect(screen.getByText("Done")).toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### waitForElementToBeRemoved

Use when you want to wait for something to **disappear** — like a loading spinner.

```jsx
import { waitForElementToBeRemoved } from "@testing-library/react"

test("hides loading spinner after data loads", async () => {
  render(<UserList />)

  // wait for spinner to disappear
  await waitForElementToBeRemoved(() => screen.queryByTestId("loading-spinner"))

  // now data should be visible
  expect(screen.getByText("Alice")).toBeInTheDocument()
  expect(screen.getByText("Bob")).toBeInTheDocument()
})

// Or pass element directly
const spinner = screen.getByTestId("loading-spinner")
await waitForElementToBeRemoved(spinner)
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Testing Loading & Success States

```jsx
// Component
function UserProfile({ userId }) {
  const [user, setUser] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(r => r.json())
      .then(data => { setUser(data); setLoading(false) })
      .catch(err => { setError(err.message); setLoading(false) })
  }, [userId])

  if (loading) return <div>Loading...</div>
  if (error)   return <div>Error: {error}</div>
  return <div>{user.name}</div>
}

// Test loading → success
test("shows loading then user name", async () => {
  // mock fetch
  global.fetch = jest.fn().mockResolvedValue({  // vi.fn() for Vitest
    json: () => Promise.resolve({ name: "Alice" })
  })

  render(<UserProfile userId={1} />)

  // loading state
  expect(screen.getByText("Loading...")).toBeInTheDocument()

  // wait for data
  await waitForElementToBeRemoved(() => screen.queryByText("Loading..."))

  // success state
  expect(screen.getByText("Alice")).toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Testing API Calls

```jsx
// Mock fetch globally
global.fetch = jest.fn()  // vi.fn() for Vitest

beforeEach(() => {
  fetch.mockClear()
})

test("fetches and displays users", async () => {
  // mock successful response
  fetch.mockResolvedValueOnce({
    ok: true,
    json: async () => ([
      { id: 1, name: "Alice" },
      { id: 2, name: "Bob" }
    ])
  })

  render(<UserList />)

  await screen.findByText("Alice")  // wait for data

  expect(screen.getByText("Alice")).toBeInTheDocument()
  expect(screen.getByText("Bob")).toBeInTheDocument()
  expect(fetch).toHaveBeenCalledWith("/api/users")
  expect(fetch).toHaveBeenCalledTimes(1)
})

test("shows error when fetch fails", async () => {
  fetch.mockRejectedValueOnce(new Error("Network error"))

  render(<UserList />)

  await screen.findByText(/error/i)
  expect(screen.getByText(/network error/i)).toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 5 — 🧩 Component Testing Patterns (25 min)

### Testing Props

```jsx
// Component
function Greeting({ name, role = "user" }) {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>Role: {role}</p>
    </div>
  )
}

// Tests
test("renders with required props", () => {
  render(<Greeting name="Alice" />)
  expect(screen.getByText("Hello, Alice!")).toBeInTheDocument()
})

test("renders default role when not provided", () => {
  render(<Greeting name="Alice" />)
  expect(screen.getByText("Role: user")).toBeInTheDocument()
})

test("renders custom role when provided", () => {
  render(<Greeting name="Alice" role="admin" />)
  expect(screen.getByText("Role: admin")).toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Testing Conditional Rendering

```jsx
// Component
function Alert({ type, message }) {
  if (!message) return null

  return (
    <div role="alert" className={`alert-${type}`}>
      {type === "error" && <span>❌</span>}
      {type === "success" && <span>✅</span>}
      {message}
    </div>
  )
}

// Tests
test("renders nothing when no message", () => {
  render(<Alert type="error" message="" />)
  expect(screen.queryByRole("alert")).not.toBeInTheDocument()
})

test("renders error alert with icon", () => {
  render(<Alert type="error" message="Something went wrong" />)

  const alert = screen.getByRole("alert")
  expect(alert).toBeInTheDocument()
  expect(screen.getByText("❌")).toBeInTheDocument()
  expect(screen.getByText("Something went wrong")).toBeInTheDocument()
})

test("renders success alert with icon", () => {
  render(<Alert type="success" message="Saved!" />)
  expect(screen.getByText("✅")).toBeInTheDocument()
  expect(screen.queryByText("❌")).not.toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Testing Lists

```jsx
// Component
function TodoList({ todos }) {
  if (todos.length === 0) return <p>No todos yet</p>

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id} style={{ textDecoration: todo.done ? "line-through" : "none" }}>
          {todo.text}
        </li>
      ))}
    </ul>
  )
}

// Tests
test("shows empty state when no todos", () => {
  render(<TodoList todos={[]} />)
  expect(screen.getByText("No todos yet")).toBeInTheDocument()
})

test("renders all todos", () => {
  const todos = [
    { id: 1, text: "Buy milk", done: false },
    { id: 2, text: "Walk dog", done: false },
    { id: 3, text: "Read book", done: true }
  ]
  render(<TodoList todos={todos} />)

  const items = screen.getAllByRole("listitem")
  expect(items).toHaveLength(3)
  expect(screen.getByText("Buy milk")).toBeInTheDocument()
  expect(screen.getByText("Walk dog")).toBeInTheDocument()
  expect(screen.getByText("Read book")).toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Testing Forms & Validation

```jsx
// Component
function SignupForm({ onSubmit }) {
  const [email, setEmail] = useState("")
  const [error, setError] = useState("")

  const handleSubmit = (e) => {
    e.preventDefault()
    if (!email.includes("@")) {
      setError("Invalid email address")
      return
    }
    setError("")
    onSubmit({ email })
  }

  return (
    <form onSubmit={handleSubmit}>
      <label htmlFor="email">Email</label>
      <input id="email" value={email} onChange={e => setEmail(e.target.value)} />
      {error && <p role="alert">{error}</p>}
      <button type="submit">Sign Up</button>
    </form>
  )
}

// Tests
test("shows error for invalid email", async () => {
  const user = userEvent.setup()
  render(<SignupForm onSubmit={jest.fn()} />)  // vi.fn() for Vitest

  await user.type(screen.getByLabelText("Email"), "notanemail")
  await user.click(screen.getByRole("button", { name: "Sign Up" }))

  expect(screen.getByRole("alert")).toHaveTextContent("Invalid email address")
})

test("does not submit with invalid email", async () => {
  const user = userEvent.setup()
  const handleSubmit = jest.fn()  // vi.fn() for Vitest
  render(<SignupForm onSubmit={handleSubmit} />)

  await user.type(screen.getByLabelText("Email"), "notanemail")
  await user.click(screen.getByRole("button", { name: "Sign Up" }))

  expect(handleSubmit).not.toHaveBeenCalled()
})

test("submits with valid email", async () => {
  const user = userEvent.setup()
  const handleSubmit = jest.fn()  // vi.fn() for Vitest
  render(<SignupForm onSubmit={handleSubmit} />)

  await user.type(screen.getByLabelText("Email"), "alice@example.com")
  await user.click(screen.getByRole("button", { name: "Sign Up" }))

  expect(handleSubmit).toHaveBeenCalledWith({ email: "alice@example.com" })
  expect(screen.queryByRole("alert")).not.toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Testing Error States

```jsx
test("shows error boundary message when component crashes", () => {
  // suppress console.error for this test
  jest.spyOn(console, "error").mockImplementation(() => {})  // vi.spyOn for Vitest

  const BrokenComponent = () => { throw new Error("Crash!") }

  render(
    <ErrorBoundary fallback={<p>Something went wrong</p>}>
      <BrokenComponent />
    </ErrorBoundary>
  )

  expect(screen.getByText("Something went wrong")).toBeInTheDocument()
  console.error.mockRestore()
})

test("shows error message from API", async () => {
  global.fetch = jest.fn().mockRejectedValueOnce(new Error("Server error"))  // vi.fn() for Vitest

  render(<UserProfile userId={1} />)

  await screen.findByText(/error/i)
  expect(screen.getByText(/server error/i)).toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 6 — 🎭 Mocking (20 min)

> **Jest vs Vitest reminder:**
> ```
> jest.fn()     →  vi.fn()
> jest.mock()   →  vi.mock()
> jest.spyOn()  →  vi.spyOn()
> ```

### jest.fn() / vi.fn() — Mock Functions

Mock functions let you **track calls, arguments, and control return values**.

```jsx
// Create a mock function
const mockFn = jest.fn()  // vi.fn() for Vitest

// Call it
mockFn("hello", 42)
mockFn("world")

// Check calls
expect(mockFn).toHaveBeenCalled()
expect(mockFn).toHaveBeenCalledTimes(2)
expect(mockFn).toHaveBeenCalledWith("hello", 42)
expect(mockFn).toHaveBeenLastCalledWith("world")

// Control return value
const mockAdd = jest.fn().mockReturnValue(10)   // vi.fn() for Vitest
mockAdd(3, 4)  // returns 10

// Return different values each call
const mockFetch = jest.fn()   // vi.fn() for Vitest
  .mockReturnValueOnce("first")
  .mockReturnValueOnce("second")
  .mockReturnValue("default")   // all subsequent calls

// Return resolved promise
const mockAsync = jest.fn().mockResolvedValue({ data: "hello" })  // vi.fn() for Vitest
const mockFail  = jest.fn().mockRejectedValue(new Error("fail"))  // vi.fn() for Vitest

// Clear mock history
mockFn.mockClear()    // clears calls, instances
mockFn.mockReset()    // clears + removes return value
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Mocking Modules

```jsx
// utils.js
export function formatDate(date) { return date.toISOString() }
export function generateId() { return Math.random().toString(36) }

// Component.test.jsx
import { formatDate, generateId } from "./utils"

// Mock entire module — jest
jest.mock("./utils")
// Mock entire module — vitest
vi.mock("./utils")

// Now all exports are jest.fn() / vi.fn() automatically
test("uses mocked utils", () => {
  formatDate.mockReturnValue("2024-01-01")   // control return value
  generateId.mockReturnValue("abc123")

  render(<MyComponent />)
  expect(screen.getByText("2024-01-01")).toBeInTheDocument()
})

// Mock with implementation
jest.mock("./utils", () => ({   // vi.mock for Vitest
  formatDate: jest.fn(() => "2024-01-01"),   // vi.fn() for Vitest
  generateId: jest.fn(() => "abc123")        // vi.fn() for Vitest
}))

// Partial mock — keep some real, mock some
jest.mock("./utils", () => ({   // vi.mock for Vitest
  ...jest.requireActual("./utils"),   // vi.importActual for Vitest
  generateId: jest.fn(() => "abc123") // vi.fn() for Vitest
}))
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Mocking API Calls — fetch, axios

```jsx
// Mock fetch globally
beforeEach(() => {
  global.fetch = jest.fn()  // vi.fn() for Vitest
})

afterEach(() => {
  jest.restoreAllMocks()  // vi.restoreAllMocks() for Vitest
})

test("fetches data successfully", async () => {
  fetch.mockResolvedValueOnce({
    ok: true,
    json: async () => ({ name: "Alice", age: 30 })
  })

  render(<UserProfile userId={1} />)

  expect(await screen.findByText("Alice")).toBeInTheDocument()
  expect(fetch).toHaveBeenCalledWith("/api/users/1")
})

// Mock axios
jest.mock("axios")  // vi.mock for Vitest
import axios from "axios"

test("fetches with axios", async () => {
  axios.get.mockResolvedValueOnce({ data: { name: "Alice" } })

  render(<UserProfile userId={1} />)

  expect(await screen.findByText("Alice")).toBeInTheDocument()
  expect(axios.get).toHaveBeenCalledWith("/api/users/1")
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Mocking Hooks

```jsx
// useAuth.js — custom hook
export function useAuth() {
  return { user: null, login: () => {}, logout: () => {} }
}

// Component.test.jsx
jest.mock("./useAuth")  // vi.mock for Vitest
import { useAuth } from "./useAuth"

test("shows username when logged in", () => {
  useAuth.mockReturnValue({
    user: { name: "Alice" },
    login: jest.fn(),   // vi.fn() for Vitest
    logout: jest.fn()   // vi.fn() for Vitest
  })

  render(<Header />)
  expect(screen.getByText("Alice")).toBeInTheDocument()
})

test("shows login button when logged out", () => {
  useAuth.mockReturnValue({
    user: null,
    login: jest.fn(),   // vi.fn() for Vitest
    logout: jest.fn()   // vi.fn() for Vitest
  })

  render(<Header />)
  expect(screen.getByRole("button", { name: "Login" })).toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### MSW — Mock Service Worker

MSW intercepts actual network requests — most realistic API mocking.

```bash
npm install --save-dev msw
```

```jsx
// src/mocks/handlers.js
import { http, HttpResponse } from "msw"

export const handlers = [
  http.get("/api/users", () => {
    return HttpResponse.json([
      { id: 1, name: "Alice" },
      { id: 2, name: "Bob" }
    ])
  }),

  http.post("/api/users", async ({ request }) => {
    const body = await request.json()
    return HttpResponse.json({ id: 3, ...body }, { status: 201 })
  }),

  http.get("/api/users/:id", ({ params }) => {
    if (params.id === "999") {
      return new HttpResponse(null, { status: 404 })
    }
    return HttpResponse.json({ id: params.id, name: "Alice" })
  })
]

// src/mocks/server.js
import { setupServer } from "msw/node"
import { handlers } from "./handlers"

export const server = setupServer(...handlers)

// src/setupTests.js
import { server } from "./mocks/server"

beforeAll(() => server.listen())
afterEach(() => server.resetHandlers())
afterAll(() => server.close())

// In tests — override for specific test
import { http, HttpResponse } from "msw"
import { server } from "./mocks/server"

test("handles error response", async () => {
  server.use(
    http.get("/api/users", () => {
      return new HttpResponse(null, { status: 500 })
    })
  )

  render(<UserList />)
  expect(await screen.findByText(/error/i)).toBeInTheDocument()
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 7 — 🪝 Testing Hooks (15 min)

### renderHook

`renderHook` lets you test hooks **without** wrapping them in a component.

```jsx
import { renderHook, act } from "@testing-library/react"

// Simple hook
function useCounter(initial = 0) {
  const [count, setCount] = useState(initial)
  const increment = () => setCount(c => c + 1)
  const decrement = () => setCount(c => c - 1)
  const reset = () => setCount(initial)
  return { count, increment, decrement, reset }
}

// Test
test("initializes with default value", () => {
  const { result } = renderHook(() => useCounter())
  expect(result.current.count).toBe(0)
})

test("initializes with custom value", () => {
  const { result } = renderHook(() => useCounter(10))
  expect(result.current.count).toBe(10)
})

test("increments count", () => {
  const { result } = renderHook(() => useCounter())

  act(() => {
    result.current.increment()
  })

  expect(result.current.count).toBe(1)
})

test("resets to initial value", () => {
  const { result } = renderHook(() => useCounter(5))

  act(() => {
    result.current.increment()
    result.current.increment()
  })
  expect(result.current.count).toBe(7)

  act(() => {
    result.current.reset()
  })
  expect(result.current.count).toBe(5)
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Testing useState & useEffect

```jsx
// Hook with useEffect
function useFetchUser(userId) {
  const [user, setUser] = useState(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState(null)

  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(r => r.json())
      .then(data => { setUser(data); setLoading(false) })
      .catch(err => { setError(err.message); setLoading(false) })
  }, [userId])

  return { user, loading, error }
}

// Test
test("fetches user data", async () => {
  global.fetch = jest.fn().mockResolvedValueOnce({  // vi.fn() for Vitest
    json: async () => ({ id: 1, name: "Alice" })
  })

  const { result } = renderHook(() => useFetchUser(1))

  // initial state
  expect(result.current.loading).toBe(true)
  expect(result.current.user).toBe(null)

  // wait for fetch to complete
  await waitFor(() => {
    expect(result.current.loading).toBe(false)
  })

  expect(result.current.user).toEqual({ id: 1, name: "Alice" })
  expect(result.current.error).toBe(null)
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Testing Custom Hooks

```jsx
// Hook with context dependency
function useTheme() {
  const context = useContext(ThemeContext)
  if (!context) throw new Error("useTheme must be inside ThemeProvider")
  return context
}

// Provide wrapper for hooks that need context
test("returns theme from context", () => {
  const wrapper = ({ children }) => (
    <ThemeContext.Provider value={{ theme: "dark", toggle: jest.fn() }}>  {/* vi.fn() for Vitest */}
      {children}
    </ThemeContext.Provider>
  )

  const { result } = renderHook(() => useTheme(), { wrapper })
  expect(result.current.theme).toBe("dark")
})

// rerender — test hook with changing props
test("updates when userId changes", async () => {
  global.fetch = jest.fn()  // vi.fn() for Vitest
    .mockResolvedValueOnce({ json: async () => ({ name: "Alice" }) })
    .mockResolvedValueOnce({ json: async () => ({ name: "Bob" }) })

  const { result, rerender } = renderHook(({ id }) => useFetchUser(id), {
    initialProps: { id: 1 }
  })

  await waitFor(() => expect(result.current.user?.name).toBe("Alice"))

  rerender({ id: 2 })

  await waitFor(() => expect(result.current.user?.name).toBe("Bob"))
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### act() — when and why

`act()` wraps code that causes React state updates — ensures React processes them before assertions.

```jsx
import { act } from "@testing-library/react"

// RTL's userEvent and waitFor wrap act() automatically
// You only need act() manually when:
// 1. Calling hook functions directly in renderHook
// 2. Triggering state updates outside RTL utilities

// ✅ Need act() — calling hook function directly
test("increments counter", () => {
  const { result } = renderHook(() => useCounter())

  act(() => {
    result.current.increment()  // causes state update
  })

  expect(result.current.count).toBe(1)
})

// ✅ Need act() — triggering state update with setTimeout
test("updates after timeout", async () => {
  jest.useFakeTimers()  // vi.useFakeTimers() for Vitest

  render(<DelayedMessage />)

  act(() => {
    jest.runAllTimers()  // vi.runAllTimers() for Vitest
  })

  expect(screen.getByText("Hello!")).toBeInTheDocument()
  jest.useRealTimers()  // vi.useRealTimers() for Vitest
})

// ❌ Don't need act() — RTL handles it
await userEvent.click(button)     // act() handled internally
await waitFor(() => { ... })      // act() handled internally
await screen.findByText("...")    // act() handled internally
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 8 — ⚙️ Setup & Config (10 min)

### jest.config / vitest.config

```js
// jest.config.js
module.exports = {
  testEnvironment: "jsdom",
  setupFilesAfterFramework: ["<rootDir>/src/setupTests.js"],
  moduleNameMapper: {
    "\\.(css|scss)$": "<rootDir>/__mocks__/styleMock.js",  // mock CSS
    "\\.(jpg|png|svg)$": "<rootDir>/__mocks__/fileMock.js" // mock images
  },
  transform: {
    "^.+\\.(js|jsx|ts|tsx)$": "babel-jest"
  },
  collectCoverageFrom: ["src/**/*.{js,jsx}", "!src/index.js"],
  coverageThreshold: {
    global: { branches: 80, functions: 80, lines: 80 }
  }
}

// vitest.config.js
import { defineConfig } from "vite"
import react from "@vitejs/plugin-react"

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    setupFiles: ["./src/setupTests.js"],
    globals: true,
    css: false,                    // ignore CSS
    coverage: {
      provider: "v8",
      reporter: ["text", "html"],
      exclude: ["src/index.jsx"]
    }
  }
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Setup Files

```js
// src/setupTests.js — runs before every test file
import "@testing-library/jest-dom"   // adds toBeInTheDocument, toHaveValue etc.

// Global mocks
global.ResizeObserver = jest.fn().mockImplementation(() => ({  // vi.fn() for Vitest
  observe: jest.fn(),   // vi.fn() for Vitest
  unobserve: jest.fn(), // vi.fn() for Vitest
  disconnect: jest.fn() // vi.fn() for Vitest
}))

// Mock matchMedia (not available in jsdom)
Object.defineProperty(window, "matchMedia", {
  value: jest.fn().mockImplementation(query => ({  // vi.fn() for Vitest
    matches: false,
    media: query,
    addEventListener: jest.fn(),   // vi.fn() for Vitest
    removeEventListener: jest.fn() // vi.fn() for Vitest
  }))
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### beforeEach, afterEach, beforeAll, afterAll

```jsx
describe("UserList", () => {
  // runs once before ALL tests in this describe
  beforeAll(() => {
    server.listen()
  })

  // runs once after ALL tests in this describe
  afterAll(() => {
    server.close()
  })

  // runs before EACH test
  beforeEach(() => {
    global.fetch = jest.fn()  // vi.fn() for Vitest
  })

  // runs after EACH test
  afterEach(() => {
    jest.clearAllMocks()   // vi.clearAllMocks() for Vitest
    server.resetHandlers()
  })

  test("renders users", async () => { ... })
  test("handles error", async () => { ... })
})
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### describe blocks

```jsx
// Organize tests with describe
describe("Button component", () => {
  describe("when enabled", () => {
    test("calls onClick when clicked", async () => { ... })
    test("shows correct label", () => { ... })
  })

  describe("when disabled", () => {
    test("does not call onClick", async () => { ... })
    test("has disabled attribute", () => { ... })
  })
})

// Skip tests
test.skip("work in progress", () => { ... })
describe.skip("entire suite", () => { ... })

// Run only this test
test.only("focus on this", () => { ... })
describe.only("focus on suite", () => { ... })

// test vs it — identical, just style preference
test("renders correctly", () => { ... })
it("renders correctly", () => { ... })    // reads like english: "it renders correctly"
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### screen.debug()

Use `screen.debug()` to see what's in the DOM when tests fail.

```jsx
test("debugging example", () => {
  render(<MyComponent />)

  // print entire DOM
  screen.debug()

  // print specific element
  screen.debug(screen.getByRole("button"))

  // print with more depth (default is 7)
  screen.debug(undefined, 30000)
})

// prettyDOM — same as debug but returns string
import { prettyDOM } from "@testing-library/react"
console.log(prettyDOM(container))

// logRoles — shows all available roles in component
import { logRoles } from "@testing-library/react"
const { container } = render(<MyComponent />)
logRoles(container)  // shows: button, textbox, heading etc.
```

[⬆ Back to Table of Contents](#table-of-contents)

---

### Common Gotchas & Fixes

```jsx
// ❌ Gotcha 1 — not wrapping async in await
test("wrong", () => {
  render(<AsyncComponent />)
  expect(screen.getByText("Loaded")).toBeInTheDocument()  // fails — not loaded yet
})
// ✅ Fix
test("correct", async () => {
  render(<AsyncComponent />)
  expect(await screen.findByText("Loaded")).toBeInTheDocument()
})

// ❌ Gotcha 2 — using getBy for absent elements
expect(screen.getByText("Error")).not.toBeInTheDocument()  // throws before expect!
// ✅ Fix
expect(screen.queryByText("Error")).not.toBeInTheDocument()

// ❌ Gotcha 3 — not clearing mocks between tests
// previous test's mock bleeds into next test
// ✅ Fix
afterEach(() => {
  jest.clearAllMocks()  // vi.clearAllMocks() for Vitest
})

// ❌ Gotcha 4 — testing implementation details
expect(component.state.count).toBe(1)   // testing internal state
// ✅ Fix — test what user sees
expect(screen.getByText("Count: 1")).toBeInTheDocument()

// ❌ Gotcha 5 — act() warning
// "not wrapped in act()" warning = state update happening after test ends
// ✅ Fix — use waitFor or findBy to wait for updates
await waitFor(() => expect(screen.getByText("Done")).toBeInTheDocument())

// ❌ Gotcha 6 — missing await on userEvent
userEvent.click(button)   // missing await!
// ✅ Fix
await userEvent.click(button)
```

[⬆ Back to Table of Contents](#table-of-contents)

---

## Block 9 — 📌 Topics Not Covered

These are valid testing tools/concepts but either advanced, niche, or separate tools:

- **Cypress** — E2E testing in real browser, separate tool entirely
- **Playwright** — E2E testing, faster than Cypress, multi-browser
- **Snapshot Testing** — `toMatchSnapshot()` — brittle, not recommended for most cases
- **Visual Regression Testing** — tools like Percy, Chromatic, Storybook
- **Testing React Native** — uses `@testing-library/react-native`, different setup
- **Storybook Interaction Testing** — `@storybook/test` for testing stories
- **Coverage Reports** — `jest --coverage` / `vitest --coverage` — measures % of code tested
- **Testing Context API** — straightforward once you know `wrapper` option in renderHook
- **Testing Redux** — `redux-mock-store` or test connected components with real store
- **Testing React Router** — wrap with `MemoryRouter` in tests
- **Performance Testing** — `@testing-library/user-event` timing, React Profiler
- **Accessibility Testing** — `jest-axe` for automated a11y checks

[⬆ Back to Table of Contents](#table-of-contents)

---

*React Testing Library version reference: v14+ | Jest: v29+ | Vitest: v1+ | Estimated time: ~3 hours*
