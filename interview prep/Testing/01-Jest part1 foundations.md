# Category 1: Jest Fundamentals & Setup — Question Bank

```
Category 1 covers Phases 1 & 2 partially

Phase 1 — Foundations (Jest setup, config, running tests, file conventions)
Phase 2 start — Pure functions, matchers like toBe, toEqual
```

## 40 Interview Questions with Answers
## Organized by Sub-Topic · Tagged by Difficulty

---

# Table of Contents

- [1.1 What is Jest & Why Use It (3 Questions)](#11-what-is-jest--why-use-it)
- [1.2 Installing Jest (3 Questions)](#12-installing-jest)
- [1.3 jest.config.js Configuration (4 Questions)](#13-jestconfigjs-configuration)
- [1.4 Running Tests & Watch Mode (4 Questions)](#14-running-tests--watch-mode)
- [1.5 File Naming Conventions (3 Questions)](#15-file-naming-conventions)
- [1.6 CLI Flags (3 Questions)](#16-cli-flags)
- [1.7 Transform & Module Mapper (3 Questions)](#17-transform--module-mapper)
- [2.1 describe() (3 Questions)](#21-describe)
- [2.2 it() vs test() (2 Questions)](#22-it-vs-test)
- [2.3 AAA Pattern (3 Questions)](#23-aaa-pattern)
- [2.4 beforeEach / afterEach (4 Questions)](#24-beforeeach--aftereach)
- [2.5 beforeAll / afterAll (2 Questions)](#25-beforeall--afterall)
- [2.6 test.each() (2 Questions)](#26-testeach)
- [2.7 test.only, test.skip, test.todo (2 Questions)](#27-testonly-testskip-testtodo)
- [3.1 toBe() (3 Questions)](#31-tobe)
- [3.2 toEqual() (3 Questions)](#32-toequal)

---

```
Difficulty Tags:
  🟢 Easy    — Freshers / Junior (0-1 years)
  🟡 Medium  — Mid-level (2-4 years)
  🔴 Hard    — Senior (5+ years) / Tricky
```

---

# 1.1 What is Jest & Why Use It

---

### Q1 🟢 What is Jest and why is it the default choice for React testing?

**Answer:**

Jest is a JavaScript testing framework built by Meta (Facebook).

| Feature | Why It Matters |
|---|---|
| Zero config | Works out of the box with CRA |
| All-in-one | Test runner + assertions + mocking — no extra packages |
| Fast | Parallel tests, only re-runs changed files |
| Snapshots | Built-in component snapshot testing |
| Coverage | Built-in reports without extra tools |

Unlike Mocha or Jasmine, you don't need Chai (assertions) or Sinon (mocking) separately.

---

### Q2 🟢 How does Jest differ from other test frameworks?

**Answer:**

| | Jest | Mocha | Jasmine | Vitest |
|---|---|---|---|---|
| Assertions | Built-in | Needs Chai | Built-in | Built-in |
| Mocking | Built-in | Needs Sinon | Limited | Built-in |
| Snapshots | ✅ | ❌ | ❌ | ✅ |
| Speed | Fast | Medium | Medium | Fastest |
| ESM support | Limited | ✅ Native | ✅ Native | ✅ Native |
| Best for | CRA / Webpack | Node.js | Angular | Vite projects |

Rule of thumb: Jest for CRA/Webpack. Vitest for Vite.

---

### Q3 🟡 What does "test runner" vs "assertion library" vs "mocking library" mean?

**Answer:**

```javascript
// Jest does ALL three in one:
test('example', () => {          // ← Test runner (finds & runs this)
  const fn = jest.fn();          // ← Mocking library (creates fake function)
  fn(42);
  expect(fn).toHaveBeenCalledWith(42);  // ← Assertion library (checks result)
});
```

- **Test runner** → finds and executes test files, reports pass/fail
- **Assertion library** → provides `expect()` to check values
- **Mocking library** → creates fake functions/modules to isolate code

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.2 Installing Jest

---

### Q4 🟢 How do you set up Jest in a Create React App project?

**Answer:**

You don't — Jest comes pre-installed with CRA.

```bash
npm test    # Runs react-scripts test (uses Jest internally)
```

CRA includes out of the box:
- `jest` — test runner
- `@testing-library/react` — component rendering
- `@testing-library/jest-dom` — DOM matchers (`toBeInTheDocument`)
- `@testing-library/user-event` — user interaction simulation

No `jest.config.js` needed.

---

### Q5 🟡 How do you set up Jest manually (without CRA)?

**Answer:**

```bash
npm install --save-dev jest @testing-library/react @testing-library/jest-dom
```

```javascript
// jest.config.js
module.exports = {
  testEnvironment: 'jsdom',        // React needs DOM (default is 'node')
  setupFilesAfterSetup: ['./jest.setup.js'],
  transform: {
    '^.+\\.(js|jsx)$': 'babel-jest',
  },
  moduleNameMapper: {
    '\\.(css|scss)$': 'identity-obj-proxy',
    '\\.(jpg|png|svg)$': '<rootDir>/__mocks__/fileMock.js',
  },
};
```

```javascript
// jest.setup.js
import '@testing-library/jest-dom';
```

**Common mistake:** Forgetting `testEnvironment: 'jsdom'`. Without it, `document` is undefined and all React tests crash.

---

### Q6 🟡 What is `@testing-library/jest-dom` and why do we need it?

**Answer:**

It adds DOM-specific matchers to Jest's `expect`:

```javascript
// ❌ WITHOUT jest-dom (ugly)
expect(element.textContent).toBe('Hello');
expect(element.getAttribute('disabled')).not.toBeNull();

// ✅ WITH jest-dom (clean)
expect(element).toHaveTextContent('Hello');
expect(element).toBeDisabled();
```

Key matchers it adds:

| Matcher | Checks |
|---|---|
| `toBeInTheDocument()` | Element exists in DOM |
| `toBeVisible()` | Element is visible |
| `toBeDisabled()` | Element is disabled |
| `toHaveTextContent()` | Text content matches |
| `toHaveValue()` | Input has value |
| `toHaveClass()` | Has CSS class |
| `toBeChecked()` | Checkbox is checked |

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.3 jest.config.js Configuration

---

### Q7 🟢 What are the most important jest.config.js options?

**Answer:**

```javascript
module.exports = {
  testEnvironment: 'jsdom',                  // 'jsdom' for React, 'node' for backend
  roots: ['<rootDir>/src'],                  // Where to look for tests
  testMatch: ['**/*.test.js'],               // Pattern for test files
  setupFilesAfterSetup: ['./jest.setup.js'], // Runs after Jest loads
  transform: {
    '^.+\\.jsx?$': 'babel-jest',             // Compile JSX
  },
  moduleNameMapper: {
    '\\.(css|scss)$': 'identity-obj-proxy',  // Mock CSS imports
  },
  collectCoverageFrom: ['src/**/*.{js,jsx}'],
  coverageThreshold: {
    global: { branches: 80, functions: 80, lines: 80 },
  },
};
```

---

### Q8 🟡 What is the difference between `setupFiles` and `setupFilesAfterSetup`?

**Answer:**

| | `setupFiles` | `setupFilesAfterSetup` |
|---|---|---|
| When | Before Jest is installed | After Jest is installed |
| Has `expect`? | ❌ No | ✅ Yes |
| Has `jest` object? | ❌ No | ✅ Yes |
| Use for | Polyfills, env vars | Custom matchers, jest-dom |

```javascript
// setupFiles runs FIRST
process.env.API_URL = 'http://localhost:3000';

// setupFilesAfterSetup runs SECOND (Jest ready)
import '@testing-library/jest-dom';  // ✅ needs expect()
```

**Common mistake:** Putting `@testing-library/jest-dom` in `setupFiles` — it needs `expect` which isn't available yet.

---

### Q9 🟡 What does `moduleNameMapper` do?

**Answer:**

Jest runs in Node.js which can't parse CSS or images. `moduleNameMapper` tells Jest what to return instead:

```javascript
// Your component:
import styles from './Button.module.css';  // ❌ Jest can't parse CSS
import logo from './logo.svg';             // ❌ Jest can't parse SVG

// Fix:
moduleNameMapper: {
  '\\.module\\.(css|scss)$': 'identity-obj-proxy', // styles.active → "active"
  '\\.(css|scss)$': '<rootDir>/__mocks__/styleMock.js',
  '\\.(jpg|png|svg)$': '<rootDir>/__mocks__/fileMock.js',
  '^@/(.*)$': '<rootDir>/src/$1',  // Path alias: @/ → src/
}
```

```javascript
// __mocks__/styleMock.js
module.exports = {};

// __mocks__/fileMock.js
module.exports = 'test-file-stub';
```

---

### Q10 🔴 What does `testEnvironment: 'jsdom'` actually do under the hood?

**Answer:**

Jest runs in Node.js — no browser APIs exist. `jsdom` creates a fake browser:

```javascript
// testEnvironment: 'node' (default)
console.log(typeof document);  // "undefined"
console.log(typeof window);    // "undefined"

// testEnvironment: 'jsdom'
console.log(typeof document);  // "object" ← fake DOM
console.log(typeof window);    // "object" ← fake window
```

**jsdom provides:** `document`, `window`, `HTMLElement`, `localStorage`, events, `MutationObserver`

**jsdom does NOT provide:** Canvas, `window.matchMedia`, `IntersectionObserver`, CSS layout — you must mock these manually.

Per-file override:

```javascript
/**
 * @jest-environment jsdom
 */
// This file uses jsdom even if global config says 'node'
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.4 Running Tests & Watch Mode

---

### Q11 🟢 What are the different ways to run Jest tests?

**Answer:**

```bash
npx jest                          # Run all tests once
npx jest --watch                  # Re-run only changed files' tests
npx jest --watchAll               # Re-run ALL tests on any change
npx jest Button.test.js           # Run specific file
npx jest -t "should render"       # Run tests matching name pattern
npx jest --verbose                # Show each test name
```

In CRA:

```bash
npm test                           # jest --watch (default)
npm test -- --coverage             # Pass flags after --
npm test -- Button.test.js         # Specific file
```

---

### Q12 🟢 What is watch mode?

**Answer:**

Watch mode keeps Jest running and re-runs tests when you save a file.

```
Watch Usage:
  › Press a to run all tests
  › Press f to run only failing tests
  › Press p to filter by filename
  › Press t to filter by test name
  › Press q to quit
```

| | `--watch` | `--watchAll` |
|---|---|---|
| Requires Git | ✅ Yes | ❌ No |
| What re-runs | Only changed files' tests | ALL tests |
| Speed | Faster | Slower |

---

### Q13 🟡 What is the output?

```javascript
describe('math', () => {
  test('adds', () => { expect(1 + 2).toBe(3); });
  test.skip('subtracts', () => { expect(5 - 3).toBe(2); });
  test.only('multiplies', () => { expect(2 * 3).toBe(6); });
});
```

**Answer:**

```
math
  ○ skipped: adds            ← skipped because .only exists
  ○ skipped: subtracts       ← explicitly skipped
  ✓ multiplies               ← only this runs
```

`test.only` runs ONLY that test — all others in the file are skipped. **Never commit `test.only`** — it silently skips tests in CI.

---

### Q14 🟡 How do you run Jest in CI/CD?

**Answer:**

```bash
npx jest --ci --coverage --forceExit
```

`--ci` does three things:
1. Fails if no tests found (instead of passing)
2. Prevents interactive prompts
3. Fails on new snapshots (must be committed first)

```json
{
  "scripts": {
    "test": "jest --watch",
    "test:ci": "jest --ci --coverage --forceExit"
  }
}
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.5 File Naming Conventions

---

### Q15 🟢 How does Jest find test files?

**Answer:**

Jest looks for files matching ANY of these:

```
1. Inside __tests__/ folders  → src/__tests__/Button.js
2. Ending with .test.js       → src/Button.test.js
3. Ending with .spec.js       → src/Button.spec.js
```

```
src/
├── components/
│   ├── Button.jsx
│   ├── Button.test.jsx       ← co-located (preferred)
│   └── __tests__/
│       └── Button.test.jsx   ← also works
```

Best practice: co-locate tests next to the file they test.

---

### Q16 🟢 What is the difference between `.test.js` and `.spec.js`?

**Answer:** Nothing. They are identical. Jest treats both the same.

- `.test.js` — more common in React
- `.spec.js` — comes from BDD, more common in Angular

Pick one and be consistent.

---

### Q17 🟡 How do you customize which files Jest considers as tests?

**Answer:**

```javascript
// jest.config.js
module.exports = {
  // Option 1: testMatch (glob patterns)
  testMatch: ['<rootDir>/src/**/*.test.{js,jsx,ts,tsx}'],

  // Option 2: testRegex (regex — can't use both!)
  testRegex: '(/__tests__/.*|(\\.|/)(test|spec))\\.[jt]sx?$',

  // Ignore paths
  testPathIgnorePatterns: ['/node_modules/', '/build/', '/e2e/'],
};
```

**Common mistake:** Setting both `testMatch` AND `testRegex` — they conflict. Use one or the other.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.6 CLI Flags

---

### Q18 🟡 What are the most useful Jest CLI flags?

**Answer:**

```bash
--coverage               # Generate coverage report
--bail                   # Stop after first failure
--bail=3                 # Stop after 3 failures
--runInBand              # Run tests serially (not parallel)
--maxWorkers=50%         # Limit CPU usage
--verbose                # Show each test name
--detectOpenHandles      # Find what's preventing exit
--changedSince=main      # Only test files changed since main branch
```

---

### Q19 🟡 What does `--runInBand` do and when would you use it?

**Answer:**

Normally Jest runs files in **parallel**. `--runInBand` forces **serial** execution.

| Use when | Why |
|---|---|
| Debugging | Console.log output is ordered |
| CI with low memory | Parallel workers use too much RAM |
| Tests share global state | Prevents race conditions |
| Docker containers | Limited CPU makes parallel slower |

```json
{
  "scripts": {
    "test:debug": "jest --runInBand --verbose"
  }
}
```

---

### Q20 🔴 What's the difference between `--watch`, `--watchAll`, `--onlyChanged`, and `--changedSince`?

**Answer:**

| Flag | Watch? | What runs | Git needed? |
|---|---|---|---|
| `--watch` | ✅ Keeps running | Changed files' tests | ✅ |
| `--watchAll` | ✅ Keeps running | All tests | ❌ |
| `--onlyChanged` | ❌ Runs once | Changed files' tests | ✅ |
| `--changedSince=main` | ❌ Runs once | Tests for files in your PR | ✅ |

**CI pro tip:** `--changedSince=main` in PR checks — only test what changed, much faster.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 1.7 Transform & Module Mapper

---

### Q21 🟡 Why does Jest need `transform` configuration?

**Answer:**

Node.js doesn't understand JSX. `transform` tells Jest how to compile code:

```javascript
// Your component:
const Button = () => <button>Click</button>;  // ❌ Node can't parse JSX

// transform compiles JSX → JS before running:
transform: {
  '^.+\\.jsx?$': 'babel-jest',   // Babel (common)
  '^.+\\.tsx?$': 'ts-jest',      // TypeScript
  '^.+\\.jsx?$': '@swc/jest',    // SWC (10x faster than Babel)
}
```

Speed comparison:

```
babel-jest  →  ~5s for 100 files
ts-jest     →  ~5s for 100 files
@swc/jest   →  ~1s for 100 files (fastest)
```

---

### Q22 🟡 What is `identity-obj-proxy`?

**Answer:**

CSS Modules return class name objects. Jest can't parse CSS. `identity-obj-proxy` echoes property names:

```javascript
// In your component:
import styles from './Button.module.css';
// styles.primary → "primary" (in browser, it's a hashed name)

// Without identity-obj-proxy: ❌ Jest crashes
// With identity-obj-proxy: styles.primary → "primary" ✅
```

```bash
npm install --save-dev identity-obj-proxy
```

This lets you still assert on class names: `expect(button).toHaveClass('primary')`.

---

### Q23 🔴 Your test fails with "SyntaxError: Unexpected token '<'". What's wrong?

**Answer:**

Jest tried to run JSX without a transformer:

```
SyntaxError: Unexpected token '<'
> 2 | const App = () => <div>Hello</div>;
  |                    ^
```

**Cause → Fix:**

```javascript
// 1. No transform configured
transform: { '^.+\\.jsx?$': 'babel-jest' }

// 2. Extension not covered (.js only, missing .jsx)
// ❌  '^.+\\.js$'
// ✅  '^.+\\.jsx?$'

// 3. node_modules package uses JSX
transformIgnorePatterns: [
  'node_modules/(?!(problematic-package)/)'
]

// 4. Missing Babel preset
// .babelrc needs:
{ "presets": ["@babel/preset-env", "@babel/preset-react"] }
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 2.1 describe()

---

### Q24 🟢 What is `describe()` and how do you use it?

**Answer:**

`describe()` groups related tests — like a folder for tests:

```javascript
describe('TodoList', () => {
  describe('rendering', () => {
    test('shows the title', () => { /* ... */ });
    test('shows empty message when no todos', () => { /* ... */ });
  });

  describe('adding todos', () => {
    test('adds todo on form submit', () => { /* ... */ });
    test('clears input after adding', () => { /* ... */ });
  });
});
```

Output:

```
TodoList
  rendering
    ✓ shows the title
    ✓ shows empty message when no todos
  adding todos
    ✓ adds todo on form submit
    ✓ clears input after adding
```

---

### Q25 🟡 Can you nest `describe()` blocks? When should you?

**Answer:**

Yes. Nest when you have distinct states:

```javascript
describe('ShoppingCart', () => {
  describe('when empty', () => {
    test('shows empty message', () => { /* ... */ });
    test('disables checkout button', () => { /* ... */ });
  });

  describe('when has items', () => {
    test('shows item count', () => { /* ... */ });
    test('calculates total', () => { /* ... */ });
  });
});
```

**Rule:** Don't go deeper than 3 levels. Too much nesting makes tests hard to find.

---

### Q26 🔴 What is the execution order?

```javascript
describe('outer', () => {
  console.log('1: outer describe');
  beforeAll(() => console.log('2: outer beforeAll'));
  beforeEach(() => console.log('3: outer beforeEach'));

  describe('inner', () => {
    console.log('4: inner describe');
    beforeAll(() => console.log('5: inner beforeAll'));
    beforeEach(() => console.log('6: inner beforeEach'));
    test('test one', () => console.log('7: test one'));
  });

  test('test two', () => console.log('8: test two'));
});
```

**Answer:**

```
1: outer describe      ← describe callbacks run FIRST (collection phase)
4: inner describe      ← nested describes also run during collection
2: outer beforeAll     ← then setup hooks run
5: inner beforeAll
3: outer beforeEach    ← outer runs first for inner test
6: inner beforeEach
7: test one
3: outer beforeEach    ← outer runs again for outer test
8: test two
```

**Key insight:** `describe` callbacks run immediately (collection phase). `beforeAll`, `beforeEach`, `test` are registered and run later.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 2.2 it() vs test()

---

### Q27 🟢 What is the difference between `it()` and `test()`?

**Answer:** Nothing. `it` is an alias for `test`. They are identical.

```javascript
// Style preference:
test('renders the button', () => { /* ... */ });  // "test that it renders"
it('should render the button', () => { /* ... */ });  // "it should render"
```

Most React projects use `test()`. Pick one, be consistent.

---

### Q28 🟡 How should you name your tests?

**Answer:**

Describe WHAT it does, not HOW:

```javascript
// ❌ BAD — describes implementation
test('sets state to true', () => {});
test('calls handleClick', () => {});

// ✅ GOOD — describes behavior
test('shows success message after submission', () => {});
test('disables button while loading', () => {});
test('displays error for invalid email', () => {});
```

**Pattern:** `[action/state] + [expected outcome]`

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 2.3 AAA Pattern

---

### Q29 🟢 What is the AAA pattern?

**Answer:**

**Arrange → Act → Assert** — the standard structure for every test:

```javascript
test('adds a todo when form is submitted', async () => {
  // ARRANGE — set up
  render(<TodoApp />);
  const input = screen.getByRole('textbox');
  const button = screen.getByRole('button', { name: /add/i });

  // ACT — do the thing
  await userEvent.type(input, 'Buy groceries');
  await userEvent.click(button);

  // ASSERT — check result
  expect(screen.getByText('Buy groceries')).toBeInTheDocument();
  expect(input).toHaveValue('');
});
```

Each test should have ONE act phase. Two actions = two tests.

---

### Q30 🟡 What is wrong with this test?

```javascript
test('todo operations', async () => {
  render(<TodoApp />);

  await userEvent.type(screen.getByRole('textbox'), 'Buy milk');
  await userEvent.click(screen.getByRole('button', { name: /add/i }));
  expect(screen.getByText('Buy milk')).toBeInTheDocument();

  await userEvent.click(screen.getByRole('button', { name: /delete/i }));
  expect(screen.queryByText('Buy milk')).not.toBeInTheDocument();

  expect(screen.getByText('No todos yet')).toBeInTheDocument();
});
```

**Answer:** It tests THREE things in one test. If delete fails, you can't tell if add works.

**Fix:** Split into three separate tests:

```javascript
test('adds todo on submit', async () => { /* arrange, act, assert */ });
test('removes todo on delete click', async () => { /* arrange, act, assert */ });
test('shows empty message when all deleted', async () => { /* arrange, act, assert */ });
```

---

### Q31 🔴 Should you have only ONE assertion per test?

**Answer:** One **ACT** per test, but multiple assertions about that action are fine.

```javascript
// ❌ TOO STRICT — duplicated setup for related checks
test('renders username', () => {
  render(<Profile user={mockUser} />);
  expect(screen.getByText('Alice')).toBeInTheDocument();
});
test('renders email', () => {
  render(<Profile user={mockUser} />);  // Same setup duplicated!
  expect(screen.getByText('alice@test.com')).toBeInTheDocument();
});

// ✅ BETTER — multiple assertions about one render
test('renders user profile info', () => {
  render(<Profile user={mockUser} />);
  expect(screen.getByText('Alice')).toBeInTheDocument();
  expect(screen.getByText('alice@test.com')).toBeInTheDocument();
  expect(screen.getByRole('img', { name: /avatar/i })).toBeInTheDocument();
});
```

Split when assertions test **different behaviors**, not different parts of the same behavior.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 2.4 beforeEach / afterEach

---

### Q32 🟢 What are `beforeEach()` and `afterEach()`?

**Answer:**

They run code before/after EACH test:

```javascript
describe('TodoList', () => {
  let user;

  beforeEach(() => {
    user = userEvent.setup();
    render(<TodoList />);       // Fresh render for each test
  });

  afterEach(() => {
    jest.restoreAllMocks();     // Clean up after each test
  });

  test('shows input field', () => {
    expect(screen.getByRole('textbox')).toBeInTheDocument();
  });

  test('adds todo on submit', async () => {
    await user.type(screen.getByRole('textbox'), 'New todo');
    await user.click(screen.getByRole('button', { name: /add/i }));
    expect(screen.getByText('New todo')).toBeInTheDocument();
  });
});
```

---

### Q33 🟡 What is the output?

```javascript
describe('counter', () => {
  let count = 0;

  beforeEach(() => { count += 1; });
  afterEach(() => { console.log(`After: count = ${count}`); });

  test('first', () => { console.log(`Test 1: count = ${count}`); });
  test('second', () => { count += 10; console.log(`Test 2: count = ${count}`); });
  test('third', () => { console.log(`Test 3: count = ${count}`); });
});
```

**Answer:**

```
Test 1: count = 1      ← beforeEach: 0 → 1
After: count = 1
Test 2: count = 12     ← beforeEach: 1 → 2, test: 2 → 12
After: count = 12
Test 3: count = 13     ← beforeEach: 12 → 13
After: count = 13
```

**Warning:** `count` is shared mutable state. Tests depend on each other's side effects. Bad practice — reset in `beforeEach`: `count = 0; count += 1;`.

---

### Q34 🟡 Why should you render in `beforeEach`, not in `describe`?

**Answer:**

```javascript
// ❌ BAD — render runs during collection phase
describe('Button', () => {
  render(<Button />);  // Runs before any hooks, gets cleaned up before tests!
  test('shows text', () => { /* DOM is empty! */ });
});

// ✅ GOOD — fresh render per test
describe('Button', () => {
  beforeEach(() => { render(<Button />); });
  test('shows text', () => { /* DOM has Button */ });
  test('handles click', () => { /* Clean DOM, no leaks */ });
});
```

RTL calls `cleanup` after each test, unmounting components. Rendering in `describe` means the component is gone before any test runs.

---

### Q35 🔴 What is the execution order?

```javascript
beforeEach(() => console.log('outer beforeEach'));
afterEach(() => console.log('outer afterEach'));

describe('group', () => {
  beforeEach(() => console.log('inner beforeEach'));
  afterEach(() => console.log('inner afterEach'));
  test('test A', () => console.log('test A'));
});
```

**Answer:**

```
outer beforeEach     ← outside → inside
inner beforeEach
test A
inner afterEach      ← inside → outside
outer afterEach
```

`beforeEach` runs outside → inside. `afterEach` runs inside → outside. Like opening and closing brackets.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 2.5 beforeAll / afterAll

---

### Q36 🟢 What is the difference between `beforeEach` and `beforeAll`?

**Answer:**

| | beforeEach | beforeAll |
|---|---|---|
| Runs | Before EVERY test | Once before ALL tests |
| Use for | Fresh state per test | Expensive one-time setup |
| Example | `render(<Component />)` | Database connection |

```javascript
describe('ProductList', () => {
  let products;
  beforeAll(async () => { products = await loadFixtures(); }); // Once
  beforeEach(() => { render(<ProductList products={products} />); }); // Every test
  afterAll(async () => { await cleanup(); }); // Once at end
});
```

Default to `beforeEach`. Use `beforeAll` only for expensive setup that doesn't change.

---

### Q37 🟡 What happens if `beforeAll` throws an error?

**Answer:**

All tests in that `describe` are skipped and marked **failed**:

```
FAIL
  API tests
    ✕ test 1 — Setup failed!
    ✕ test 2 — Setup failed!
    ✕ test 3 — Setup failed!
```

`afterAll` still runs even if `beforeAll` fails — use it for cleanup.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 2.6 test.each()

---

### Q38 🟡 What is `test.each` and when do you use it?

**Answer:**

Runs the same test with different data — avoids copy-pasting:

```javascript
// ❌ WITHOUT test.each — repetitive
test('formats $0', () => { expect(formatPrice(0)).toBe('$0.00'); });
test('formats $10', () => { expect(formatPrice(10)).toBe('$10.00'); });

// ✅ WITH test.each — clean
test.each([
  [0, '$0.00'],
  [10, '$10.00'],
  [99.9, '$99.90'],
  [-5, '-$5.00'],
])('formats %s as %s', (input, expected) => {
  expect(formatPrice(input)).toBe(expected);
});
```

Output:

```
✓ formats 0 as $0.00
✓ formats 10 as $10.00
✓ formats 99.9 as $99.90
✓ formats -5 as -$5.00
```

---

### Q39 🟡 How do you use `test.each` with objects?

**Answer:**

```javascript
test.each([
  { input: '',       valid: false, error: 'Email required' },
  { input: 'abc',    valid: false, error: 'Invalid format' },
  { input: 'a@b.co', valid: true,  error: null },
])('validates "$input" → valid: $valid', ({ input, error, valid }) => {
  const result = validateEmail(input);
  expect(result.valid).toBe(valid);
  if (error) expect(result.error).toBe(error);
});
```

Also works with `describe.each` for parameterized suites:

```javascript
describe.each([
  { role: 'admin', canDelete: true },
  { role: 'user', canDelete: false },
])('$role permissions', ({ role, canDelete }) => {
  test(`can${canDelete ? '' : 'not'} delete`, () => { /* ... */ });
});
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 2.7 test.only, test.skip, test.todo

---

### Q40 🟢 When should you use `test.only`, `test.skip`, and `test.todo`?

**Answer:**

```javascript
test.only('debug this one', () => { /* only this runs */ });
// ⚠️ NEVER commit — silently skips other tests in CI

test.skip('broken, fix later', () => { /* skipped */ });
// ⚠️ Don't let skipped tests pile up

test.todo('should handle network timeout');
// ✅ Good for planning — no callback needed
```

Output:

```
✓ debug this one
○ skipped: broken, fix later
✎ todo: should handle network timeout
```

---

### Q41 🟡 What is the output?

```javascript
describe('auth', () => {
  test.only('login', () => { console.log('login'); });
  test.only('logout', () => { console.log('logout'); });
  test('signup', () => { console.log('signup'); });
});

describe('profile', () => {
  test('renders name', () => { console.log('name'); });
});
```

**Answer:**

```
login
logout
```

Only the two `.only` tests run. ALL other tests in the **entire file** are skipped — including the "profile" describe block.

Use `describe.only` to focus on a whole group instead.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 3.1 toBe()

---

### Q42 🟢 What does `toBe()` check?

**Answer:**

`toBe()` uses `Object.is()` — essentially `===` (strict equality). Checks the exact same thing:

```javascript
// ✅ Works for primitives
expect(2 + 2).toBe(4);
expect('hello').toBe('hello');
expect(true).toBe(true);

// ❌ Fails for objects/arrays (different references)
expect({ a: 1 }).toBe({ a: 1 });  // FAIL! Different objects in memory
expect([1, 2]).toBe([1, 2]);       // FAIL! Different arrays
```

**Rule:** `toBe` for primitives. `toEqual` for objects and arrays.

---

### Q43 🟡 What is the output?

```javascript
expect(0.1 + 0.2).toBe(0.3);   // ?
expect(NaN).toBe(NaN);          // ?
expect(+0).toBe(-0);            // ?
```

**Answer:**

```javascript
expect(0.1 + 0.2).toBe(0.3);  // ❌ FAIL — 0.30000000000000004
expect(NaN).toBe(NaN);         // ✅ PASS — Object.is(NaN, NaN) is true!
expect(+0).toBe(-0);           // ❌ FAIL — Object.is(+0, -0) is false!
```

`toBe` uses `Object.is()`, not `===`:
- `Object.is(NaN, NaN)` → `true` (but `NaN === NaN` → `false`)
- `Object.is(+0, -0)` → `false` (but `+0 === -0` → `true`)

For floats: use `toBeCloseTo(0.3, 5)`.

---

### Q44 🔴 Why does this test fail?

```javascript
const user = { name: 'Alice' };
const getUser = () => user;

test('returns the user', () => {
  expect(getUser()).toBe({ name: 'Alice' });  // ❌ FAILS
});
```

**Answer:**

`toBe` compares by reference. `{ name: 'Alice' }` in the test is a NEW object — different reference.

```javascript
expect(getUser()).toEqual({ name: 'Alice' });  // ✅ compares values
expect(getUser()).toBe(user);                   // ✅ same reference
```

**One-liner:** `toBe` = "same object in memory." `toEqual` = "same shape and values."

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 3.2 toEqual()

---

### Q45 🟢 What is the difference between `toBe()` and `toEqual()`?

**Answer:**

| | toBe() | toEqual() |
|---|---|---|
| Comparison | `Object.is()` (reference) | Deep value comparison |
| Primitives | ✅ Works | ✅ Works |
| Objects | ❌ Fails | ✅ Compares all properties |
| Arrays | ❌ Fails | ✅ Compares all elements |
| Nested | ❌ | ✅ Recursive |

```javascript
expect({ a: 1 }).toBe({ a: 1 });     // ❌ FAIL (different references)
expect({ a: 1 }).toEqual({ a: 1 });   // ✅ PASS (same values)
expect({ x: { y: 1 } }).toEqual({ x: { y: 1 } });  // ✅ PASS (deep)
```

---

### Q46 🟡 What is the output?

```javascript
expect({ a: 1, b: undefined }).toEqual({ a: 1 });          // ?
expect({ a: 1 }).toStrictEqual({ a: 1, b: undefined });    // ?
```

**Answer:**

```javascript
expect({ a: 1, b: undefined }).toEqual({ a: 1 });          // ✅ PASS
expect({ a: 1 }).toStrictEqual({ a: 1, b: undefined });    // ❌ FAIL
```

- `toEqual` — ignores `undefined` properties
- `toStrictEqual` — checks `undefined` properties and class instances strictly

```javascript
// toEqual treats these as equal:
{ a: 1, b: undefined }  ≈  { a: 1 }

// toStrictEqual says they're different
```

---

### Q47 🔴 When should you use `toStrictEqual` over `toEqual`?

**Answer:**

When you care about exact object shape:

```javascript
test('returns user without password', () => {
  const user = sanitize({ id: 1, name: 'Alice', password: 'hash' });

  // ❌ toEqual PASSES even if password is undefined (not deleted)
  expect(user).toEqual({ id: 1, name: 'Alice' });

  // ✅ toStrictEqual catches the leftover undefined field
  expect(user).toStrictEqual({ id: 1, name: 'Alice' });
});
```

Also catches class mismatches:

```javascript
class User { constructor(name) { this.name = name; } }

expect(new User('Alice')).toEqual({ name: 'Alice' });        // ✅ PASS
expect(new User('Alice')).toStrictEqual({ name: 'Alice' });   // ❌ FAIL
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# Quick Reference — All 47 Questions at a Glance

| # | Topic | Question Summary | Difficulty |
|---|-------|-----------------|------------|
| 1 | Jest Intro | What is Jest, why for React | 🟢 |
| 2 | Jest Intro | Jest vs Mocha vs Jasmine vs Vitest | 🟢 |
| 3 | Jest Intro | Runner vs assertion vs mocking lib | 🟡 |
| 4 | Setup | Jest in CRA | 🟢 |
| 5 | Setup | Manual Jest setup | 🟡 |
| 6 | Setup | What is jest-dom | 🟡 |
| 7 | Config | Essential config options | 🟢 |
| 8 | Config | setupFiles vs setupFilesAfterSetup | 🟡 |
| 9 | Config | moduleNameMapper | 🟡 |
| 10 | Config | testEnvironment jsdom | 🔴 |
| 11 | Running | Ways to run Jest | 🟢 |
| 12 | Running | Watch mode | 🟢 |
| 13 | Running | test.only and test.skip output | 🟡 |
| 14 | Running | Jest in CI/CD | 🟡 |
| 15 | Files | How Jest finds tests | 🟢 |
| 16 | Files | .test.js vs .spec.js | 🟢 |
| 17 | Files | Custom testMatch | 🟡 |
| 18 | CLI | Useful CLI flags | 🟡 |
| 19 | CLI | --runInBand | 🟡 |
| 20 | CLI | --watch vs --watchAll vs --changedSince | 🔴 |
| 21 | Transform | Why Jest needs transform | 🟡 |
| 22 | Transform | identity-obj-proxy | 🟡 |
| 23 | Transform | SyntaxError Unexpected token '<' | 🔴 |
| 24 | describe | What is describe() | 🟢 |
| 25 | describe | Nested describe | 🟡 |
| 26 | describe | Execution order with nested hooks | 🔴 |
| 27 | it/test | it() vs test() | 🟢 |
| 28 | it/test | How to name tests | 🟡 |
| 29 | AAA | Arrange-Act-Assert | 🟢 |
| 30 | AAA | Multi-action test problem | 🟡 |
| 31 | AAA | One assertion per test rule | 🔴 |
| 32 | beforeEach | What is beforeEach/afterEach | 🟢 |
| 33 | beforeEach | Shared mutable state output | 🟡 |
| 34 | beforeEach | Render in beforeEach not describe | 🟡 |
| 35 | beforeEach | Nested before/afterEach order | 🔴 |
| 36 | beforeAll | beforeAll vs beforeEach | 🟢 |
| 37 | beforeAll | beforeAll throws error | 🟡 |
| 38 | test.each | What is test.each | 🟡 |
| 39 | test.each | test.each with objects | 🟡 |
| 40 | only/skip | test.only, test.skip, test.todo | 🟢 |
| 41 | only/skip | test.only scope across describes | 🟡 |
| 42 | toBe | What does toBe check | 🟢 |
| 43 | toBe | NaN, +0/-0, float edge cases | 🟡 |
| 44 | toBe | Why toBe fails for objects | 🔴 |
| 45 | toEqual | toBe vs toEqual | 🟢 |
| 46 | toEqual | toEqual vs toStrictEqual | 🟡 |
| 47 | toEqual | When to use toStrictEqual | 🔴 |