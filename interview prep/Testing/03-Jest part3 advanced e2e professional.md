# Category 3: Advanced Patterns, E2E & Professional Practices — Question Bank
## 70 Interview Questions with Answers
## Organized by Sub-Topic · Tagged by Difficulty

---

# Table of Contents

- [10.1 Testing Custom Hooks (4 Questions)](#101-testing-custom-hooks)
- [10.2 Testing Context API (3 Questions)](#102-testing-context-api)
- [10.3 Testing Redux (4 Questions)](#103-testing-redux)
- [10.4 Testing React Query / TanStack Query (3 Questions)](#104-testing-react-query--tanstack-query)
- [10.5 Testing Routing & Navigation (3 Questions)](#105-testing-routing--navigation)
- [10.6 Testing Error Boundaries (2 Questions)](#106-testing-error-boundaries)
- [10.7 Testing Portals, Modals, Suspense (3 Questions)](#107-testing-portals-modals-suspense)
- [11.1 Cypress Basics (4 Questions)](#111-cypress-basics)
- [11.2 Cypress Commands & Assertions (3 Questions)](#112-cypress-commands--assertions)
- [11.3 cy.intercept & Fixtures (3 Questions)](#113-cyintercept--fixtures)
- [11.4 Cypress Auth & Custom Commands (2 Questions)](#114-cypress-auth--custom-commands)
- [12.1 Playwright Basics (3 Questions)](#121-playwright-basics)
- [12.2 Playwright Locators & Auto-Wait (3 Questions)](#122-playwright-locators--auto-wait)
- [12.3 Playwright API Mocking & Codegen (2 Questions)](#123-playwright-api-mocking--codegen)
- [13.1 Code Coverage (4 Questions)](#131-code-coverage)
- [13.2 Test Architecture & Strategy (3 Questions)](#132-test-architecture--strategy)
- [13.3 CI/CD Integration (3 Questions)](#133-cicd-integration)
- [13.4 Debugging & Performance (4 Questions)](#134-debugging--performance)
- [13.5 Vitest (2 Questions)](#135-vitest)
- [14.1 Tricky Jest Output Questions (15 Questions)](#141-tricky-jest-output-questions)

---

```
Difficulty Tags:
  🟢 Easy    — Freshers / Junior (0-1 years)
  🟡 Medium  — Mid-level (2-4 years)
  🔴 Hard    — Senior (5+ years) / Tricky
```

---

# 10.1 Testing Custom Hooks

---

### Q1 🟢 How do you test a custom hook?

**Answer:**

Hooks can't run outside components. `renderHook` from RTL creates a wrapper component for you:

```javascript
import { renderHook, act } from '@testing-library/react';
import useCounter from './useCounter';

test('increments counter', () => {
  const { result } = renderHook(() => useCounter());

  // Initial value
  expect(result.current.count).toBe(0);

  // Call the hook's function
  act(() => { result.current.increment(); });

  // Updated value
  expect(result.current.count).toBe(1);
});
```

`result.current` always has the latest return value of the hook.

---

### Q2 🟡 How do you test a hook with arguments?

**Answer:**

```javascript
// useCounter(initialValue)
test('starts at custom initial value', () => {
  const { result } = renderHook(() => useCounter(10));
  expect(result.current.count).toBe(10);
});

// To change arguments after render, use rerender:
test('resets when initialValue changes', () => {
  const { result, rerender } = renderHook(
    ({ initial }) => useCounter(initial),
    { initialProps: { initial: 5 } }
  );

  expect(result.current.count).toBe(5);

  rerender({ initial: 20 });
  expect(result.current.count).toBe(20);
});
```

---

### Q3 🟡 How do you test a hook that calls an API?

**Answer:**

```javascript
// useFetchUser(id) — fetches user data
import { renderHook, waitFor } from '@testing-library/react';

// Mock the API (or use MSW)
global.fetch = jest.fn();

test('fetches user by id', async () => {
  fetch.mockResolvedValueOnce({
    ok: true,
    json: async () => ({ id: 1, name: 'Alice' }),
  });

  const { result } = renderHook(() => useFetchUser(1));

  // Initially loading
  expect(result.current.loading).toBe(true);
  expect(result.current.user).toBeNull();

  // Wait for data
  await waitFor(() => {
    expect(result.current.loading).toBe(false);
  });

  expect(result.current.user).toEqual({ id: 1, name: 'Alice' });
  expect(fetch).toHaveBeenCalledWith('/api/users/1');
});
```

---

### Q4 🔴 How do you test a hook that needs a Provider (Context/Redux)?

**Answer:**

Pass a `wrapper` option to `renderHook`:

```javascript
import { ThemeContext } from './ThemeContext';

const wrapper = ({ children }) => (
  <ThemeContext.Provider value={{ theme: 'dark', toggle: jest.fn() }}>
    {children}
  </ThemeContext.Provider>
);

test('returns current theme', () => {
  const { result } = renderHook(() => useTheme(), { wrapper });
  expect(result.current.theme).toBe('dark');
});
```

For Redux:

```javascript
import { Provider } from 'react-redux';
import { configureStore } from '@reduxjs/toolkit';
import userReducer from './userSlice';

const createWrapper = (preloadedState) => {
  const store = configureStore({
    reducer: { user: userReducer },
    preloadedState,
  });
  return ({ children }) => <Provider store={store}>{children}</Provider>;
};

test('returns user from Redux store', () => {
  const wrapper = createWrapper({ user: { name: 'Alice', role: 'admin' } });
  const { result } = renderHook(() => useCurrentUser(), { wrapper });
  expect(result.current.name).toBe('Alice');
});
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 10.2 Testing Context API

---

### Q5 🟡 How do you test a component that consumes Context?

**Answer:**

Wrap the component in its Provider with test values:

```javascript
import { AuthContext } from './AuthContext';

const renderWithAuth = (ui, { user = null } = {}) => {
  return render(
    <AuthContext.Provider value={{ user, login: jest.fn(), logout: jest.fn() }}>
      {ui}
    </AuthContext.Provider>
  );
};

test('shows username when logged in', () => {
  renderWithAuth(<Navbar />, { user: { name: 'Alice' } });
  expect(screen.getByText('Alice')).toBeInTheDocument();
});

test('shows login button when logged out', () => {
  renderWithAuth(<Navbar />, { user: null });
  expect(screen.getByRole('button', { name: /login/i })).toBeInTheDocument();
});
```

---

### Q6 🟡 How do you create a custom render function with all Providers?

**Answer:**

```javascript
// test-utils.js
import { render } from '@testing-library/react';
import { BrowserRouter } from 'react-router-dom';
import { ThemeProvider } from './ThemeContext';
import { AuthProvider } from './AuthContext';
import { Provider } from 'react-redux';
import { store } from './store';

const AllProviders = ({ children }) => (
  <Provider store={store}>
    <AuthProvider>
      <ThemeProvider>
        <BrowserRouter>
          {children}
        </BrowserRouter>
      </ThemeProvider>
    </AuthProvider>
  </Provider>
);

const customRender = (ui, options) =>
  render(ui, { wrapper: AllProviders, ...options });

// Re-export everything
export * from '@testing-library/react';
export { customRender as render };
```

```javascript
// In tests — import from test-utils instead of @testing-library/react
import { render, screen } from '../test-utils';

test('renders with all providers', () => {
  render(<Dashboard />);  // Automatically wrapped in all providers
  expect(screen.getByText('Dashboard')).toBeInTheDocument();
});
```

---

### Q7 🔴 How do you test that a Context Provider updates correctly?

**Answer:**

```javascript
test('toggles theme from light to dark', async () => {
  const user = userEvent.setup();

  // Render provider WITH a consumer that shows the value
  render(
    <ThemeProvider>
      <ThemeConsumerTestComponent />
    </ThemeProvider>
  );

  // Initially light
  expect(screen.getByText('Current: light')).toBeInTheDocument();

  // Click toggle
  await user.click(screen.getByRole('button', { name: /toggle/i }));

  // Now dark
  expect(screen.getByText('Current: dark')).toBeInTheDocument();
});

// Helper component that exposes context values
const ThemeConsumerTestComponent = () => {
  const { theme, toggle } = useTheme();
  return (
    <div>
      <p>Current: {theme}</p>
      <button onClick={toggle}>Toggle</button>
    </div>
  );
};
```

**Key insight:** Test Context through a real consumer component, not by inspecting Context internals. Test behavior, not implementation.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 10.3 Testing Redux

---

### Q8 🟢 How do you test a Redux reducer?

**Answer:**

Reducers are pure functions — test them directly, no rendering needed:

```javascript
import todoReducer, { addTodo, removeTodo, toggleTodo } from './todoSlice';

test('adds a todo', () => {
  const initialState = { todos: [] };
  const nextState = todoReducer(initialState, addTodo('Buy milk'));

  expect(nextState.todos).toHaveLength(1);
  expect(nextState.todos[0]).toMatchObject({
    text: 'Buy milk',
    completed: false,
  });
});

test('toggles a todo', () => {
  const initialState = {
    todos: [{ id: 1, text: 'Buy milk', completed: false }],
  };
  const nextState = todoReducer(initialState, toggleTodo(1));

  expect(nextState.todos[0].completed).toBe(true);
});
```

---

### Q9 🟡 How do you test a component connected to Redux?

**Answer:**

Create a real store with test state and wrap the component:

```javascript
import { configureStore } from '@reduxjs/toolkit';
import { Provider } from 'react-redux';
import todoReducer from './todoSlice';

const renderWithStore = (ui, { preloadedState = {} } = {}) => {
  const store = configureStore({
    reducer: { todos: todoReducer },
    preloadedState,
  });
  return render(<Provider store={store}>{ui}</Provider>);
};

test('shows todos from store', () => {
  renderWithStore(<TodoList />, {
    preloadedState: {
      todos: {
        todos: [
          { id: 1, text: 'Buy milk', completed: false },
          { id: 2, text: 'Walk dog', completed: true },
        ],
      },
    },
  });

  expect(screen.getByText('Buy milk')).toBeInTheDocument();
  expect(screen.getByText('Walk dog')).toBeInTheDocument();
});
```

**Rule:** Use a real store, not mock `useSelector`/`useDispatch`. Tests closer to production.

---

### Q10 🟡 How do you test Redux async thunks?

**Answer:**

```javascript
import { configureStore } from '@reduxjs/toolkit';
import userReducer, { fetchUser } from './userSlice';

test('fetchUser thunk populates store', async () => {
  // Mock the API
  global.fetch = jest.fn().mockResolvedValue({
    ok: true,
    json: async () => ({ id: 1, name: 'Alice' }),
  });

  const store = configureStore({ reducer: { user: userReducer } });

  // Dispatch the thunk
  await store.dispatch(fetchUser(1));

  // Check store state
  const state = store.getState().user;
  expect(state.data).toEqual({ id: 1, name: 'Alice' });
  expect(state.loading).toBe(false);
  expect(state.error).toBeNull();
});
```

Or test through the UI (integration style):

```javascript
test('shows user after fetch', async () => {
  fetch.mockResolvedValueOnce({
    ok: true,
    json: async () => ({ id: 1, name: 'Alice' }),
  });

  renderWithStore(<UserProfile userId={1} />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();
});
```

---

### Q11 🔴 How do you test Redux selectors?

**Answer:**

Selectors are pure functions — test directly:

```javascript
import { selectCompletedTodos, selectTodoCount } from './todoSelectors';

const state = {
  todos: {
    todos: [
      { id: 1, text: 'Buy milk', completed: true },
      { id: 2, text: 'Walk dog', completed: false },
      { id: 3, text: 'Read book', completed: true },
    ],
  },
};

test('selectCompletedTodos returns only completed', () => {
  const result = selectCompletedTodos(state);
  expect(result).toHaveLength(2);
  expect(result.every(t => t.completed)).toBe(true);
});

test('selectTodoCount returns total count', () => {
  expect(selectTodoCount(state)).toBe(3);
});
```

For memoized selectors (Reselect), also test memoization:

```javascript
test('selector is memoized', () => {
  const result1 = selectCompletedTodos(state);
  const result2 = selectCompletedTodos(state);
  expect(result1).toBe(result2);  // Same reference = memoized
});
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 10.4 Testing React Query / TanStack Query

---

### Q12 🟡 How do you test components using React Query?

**Answer:**

Wrap in `QueryClientProvider` with a fresh client per test:

```javascript
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const createTestQueryClient = () => new QueryClient({
  defaultOptions: {
    queries: {
      retry: false,        // Don't retry failed queries in tests
      gcTime: Infinity,    // Don't garbage collect
    },
  },
});

const renderWithQuery = (ui) => {
  const client = createTestQueryClient();
  return render(
    <QueryClientProvider client={client}>
      {ui}
    </QueryClientProvider>
  );
};

test('shows users from API', async () => {
  // MSW handles the API mock
  renderWithQuery(<UserList />);
  expect(await screen.findByText('Alice')).toBeInTheDocument();
});
```

---

### Q13 🟡 Why set `retry: false` in test QueryClient?

**Answer:**

```javascript
// Default React Query behavior:
// Failed request → wait → retry → wait → retry → wait → retry → FAIL
// This takes ~6 seconds in tests!

// With retry: false:
// Failed request → FAIL immediately

const client = new QueryClient({
  defaultOptions: {
    queries: {
      retry: false,  // ← Fail fast in tests
    },
  },
});
```

Without `retry: false`, error state tests are painfully slow because React Query retries 3 times with exponential backoff.

---

### Q14 🔴 How do you test loading, error, and success states with React Query?

**Answer:**

```javascript
test('shows loading → success', async () => {
  renderWithQuery(<UserList />);

  // Loading state
  expect(screen.getByText(/loading/i)).toBeInTheDocument();

  // Success state
  expect(await screen.findByText('Alice')).toBeInTheDocument();
  expect(screen.queryByText(/loading/i)).not.toBeInTheDocument();
});

test('shows error on API failure', async () => {
  // Override MSW handler for this test
  server.use(
    http.get('/api/users', () => {
      return HttpResponse.json({ error: 'Server error' }, { status: 500 });
    })
  );

  renderWithQuery(<UserList />);

  expect(await screen.findByText(/error/i)).toBeInTheDocument();
});

test('refetches on button click', async () => {
  const user = userEvent.setup();
  renderWithQuery(<UserList />);

  await screen.findByText('Alice');

  // Change API response
  server.use(
    http.get('/api/users', () => {
      return HttpResponse.json([{ id: 3, name: 'Charlie' }]);
    })
  );

  await user.click(screen.getByRole('button', { name: /refresh/i }));
  expect(await screen.findByText('Charlie')).toBeInTheDocument();
});
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 10.5 Testing Routing & Navigation

---

### Q15 🟡 How do you test that navigation works?

**Answer:**

```javascript
import { MemoryRouter } from 'react-router-dom';

test('navigates from home to about', async () => {
  const user = userEvent.setup();

  render(
    <MemoryRouter initialEntries={['/']}>
      <App />
    </MemoryRouter>
  );

  // On home page
  expect(screen.getByText('Welcome Home')).toBeInTheDocument();

  // Click about link
  await user.click(screen.getByRole('link', { name: /about/i }));

  // Now on about page
  expect(screen.getByText('About Us')).toBeInTheDocument();
  expect(screen.queryByText('Welcome Home')).not.toBeInTheDocument();
});
```

---

### Q16 🟡 How do you test protected/private routes?

**Answer:**

```javascript
test('redirects to login when not authenticated', () => {
  render(
    <MemoryRouter initialEntries={['/dashboard']}>
      <AuthContext.Provider value={{ user: null }}>
        <App />
      </AuthContext.Provider>
    </MemoryRouter>
  );

  // Should redirect to login
  expect(screen.getByText('Please log in')).toBeInTheDocument();
  expect(screen.queryByText('Dashboard')).not.toBeInTheDocument();
});

test('shows dashboard when authenticated', () => {
  render(
    <MemoryRouter initialEntries={['/dashboard']}>
      <AuthContext.Provider value={{ user: { name: 'Alice' } }}>
        <App />
      </AuthContext.Provider>
    </MemoryRouter>
  );

  expect(screen.getByText('Dashboard')).toBeInTheDocument();
});
```

---

### Q17 🔴 How do you test route params?

**Answer:**

```javascript
// ProductPage uses useParams() to get { id }
test('shows product details for given ID', async () => {
  // MSW returns product for ID 42
  server.use(
    http.get('/api/products/42', () => {
      return HttpResponse.json({ id: 42, name: 'Laptop', price: 999 });
    })
  );

  render(
    <MemoryRouter initialEntries={['/products/42']}>
      <Routes>
        <Route path="/products/:id" element={<ProductPage />} />
      </Routes>
    </MemoryRouter>
  );

  expect(await screen.findByText('Laptop')).toBeInTheDocument();
  expect(screen.getByText('$999')).toBeInTheDocument();
});
```

**Key:** Set `initialEntries` to the URL with the param value, and define the `Route` with `:id` so `useParams` works.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 10.6 Testing Error Boundaries

---

### Q18 🟡 How do you test an error boundary?

**Answer:**

```javascript
// Suppress console.error for cleaner test output
const BrokenComponent = () => { throw new Error('Boom!'); };

test('shows fallback when child throws', () => {
  const spy = jest.spyOn(console, 'error').mockImplementation(() => {});

  render(
    <ErrorBoundary fallback={<p>Something went wrong</p>}>
      <BrokenComponent />
    </ErrorBoundary>
  );

  expect(screen.getByText('Something went wrong')).toBeInTheDocument();

  spy.mockRestore();
});
```

---

### Q19 🔴 How do you test that the error boundary catches async errors?

**Answer:**

Error boundaries don't catch async errors or event handler errors. You must test differently:

```javascript
// ❌ Error boundary does NOT catch this:
const AsyncBroken = () => {
  useEffect(() => {
    setTimeout(() => { throw new Error('Async boom'); }, 100);
  }, []);
  return <p>Hello</p>;
};

// ✅ For async errors, test error STATE instead:
const AsyncComponent = () => {
  const [error, setError] = useState(null);
  useEffect(() => {
    fetchData().catch(setError);
  }, []);
  if (error) return <p>Error: {error.message}</p>;
  return <p>Data loaded</p>;
};

test('shows error for failed fetch', async () => {
  fetch.mockRejectedValueOnce(new Error('Network failure'));
  render(<AsyncComponent />);
  expect(await screen.findByText(/network failure/i)).toBeInTheDocument();
});
```

**Key insight:** Error boundaries only catch errors during rendering, lifecycle methods, and constructors — NOT in event handlers, async code, or setTimeout.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 10.7 Testing Portals, Modals, Suspense

---

### Q20 🟡 How do you test a modal rendered via Portal?

**Answer:**

Portals render outside the parent DOM tree, but RTL queries the entire `document.body`, so they work normally:

```javascript
test('opens and closes modal', async () => {
  const user = userEvent.setup();
  render(<ConfirmDialog />);

  // Modal not visible
  expect(screen.queryByRole('dialog')).not.toBeInTheDocument();

  // Open modal
  await user.click(screen.getByRole('button', { name: /delete/i }));
  expect(screen.getByRole('dialog')).toBeInTheDocument();
  expect(screen.getByText('Are you sure?')).toBeInTheDocument();

  // Close modal
  await user.click(screen.getByRole('button', { name: /cancel/i }));
  expect(screen.queryByRole('dialog')).not.toBeInTheDocument();
});
```

**No special setup needed.** RTL's `screen` queries the entire document, including portal targets.

---

### Q21 🟡 How do you test `React.lazy` and `Suspense`?

**Answer:**

```javascript
// App uses React.lazy:
// const Dashboard = React.lazy(() => import('./Dashboard'));

test('shows fallback then lazy component', async () => {
  render(
    <Suspense fallback={<p>Loading...</p>}>
      <Dashboard />
    </Suspense>
  );

  // Suspense fallback shown while chunk loads
  expect(screen.getByText('Loading...')).toBeInTheDocument();

  // Wait for lazy component to load
  expect(await screen.findByText('Dashboard')).toBeInTheDocument();
  expect(screen.queryByText('Loading...')).not.toBeInTheDocument();
});
```

If you need to mock the lazy import:

```javascript
jest.mock('./Dashboard', () => {
  return { __esModule: true, default: () => <div>Mocked Dashboard</div> };
});
```

---

### Q22 🔴 How do you test a component using `useRef` and `forwardRef`?

**Answer:**

```javascript
// FocusInput uses forwardRef to expose focus()
const FocusInput = forwardRef((props, ref) => (
  <input ref={ref} placeholder="Type here" {...props} />
));

test('parent can focus the input via ref', () => {
  const ref = React.createRef();
  render(<FocusInput ref={ref} />);

  // Programmatically focus via ref
  act(() => { ref.current.focus(); });

  expect(screen.getByPlaceholderText('Type here')).toHaveFocus();
});

// Testing a component that uses useRef internally:
test('scrolls to bottom on new message', async () => {
  const user = userEvent.setup();
  const scrollSpy = jest.fn();

  // Mock scrollIntoView (jsdom doesn't implement it)
  Element.prototype.scrollIntoView = scrollSpy;

  render(<ChatWindow messages={['Hello']} />);
  await user.click(screen.getByRole('button', { name: /send/i }));

  expect(scrollSpy).toHaveBeenCalled();
});
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 11.1 Cypress Basics

---

### Q23 🟢 What is Cypress and how is it different from Jest + RTL?

**Answer:**

| | Jest + RTL | Cypress |
|---|---|---|
| Type | Unit / Integration | End-to-End (E2E) |
| Runs in | Node.js (jsdom) | Real browser (Chrome, Firefox, Edge) |
| Speed | Fast (~ms per test) | Slower (~seconds per test) |
| Tests | Component behavior | Full user journeys |
| Network | Mocked (MSW / jest.mock) | Real or intercepted |
| Sees | Fake DOM | Real rendered pixels |

```bash
npm install --save-dev cypress
npx cypress open   # Interactive mode
npx cypress run    # Headless CI mode
```

---

### Q24 🟢 What does a basic Cypress test look like?

**Answer:**

```javascript
// cypress/e2e/login.cy.js
describe('Login', () => {
  beforeEach(() => {
    cy.visit('/login');
  });

  it('logs in with valid credentials', () => {
    cy.get('[data-testid="email"]').type('alice@test.com');
    cy.get('[data-testid="password"]').type('password123');
    cy.get('button[type="submit"]').click();

    cy.url().should('include', '/dashboard');
    cy.contains('Welcome, Alice').should('be.visible');
  });

  it('shows error for wrong password', () => {
    cy.get('[data-testid="email"]').type('alice@test.com');
    cy.get('[data-testid="password"]').type('wrong');
    cy.get('button[type="submit"]').click();

    cy.contains('Invalid credentials').should('be.visible');
    cy.url().should('include', '/login');
  });
});
```

---

### Q25 🟡 What is Cypress's automatic retry / auto-wait?

**Answer:**

Cypress automatically retries assertions until they pass or timeout (4 seconds default):

```javascript
// You do NOT need waitFor or sleep:
cy.contains('Data loaded').should('be.visible');
// Cypress retries finding "Data loaded" for up to 4 seconds

cy.get('.spinner').should('not.exist');
// Retries until spinner is gone

// Custom timeout:
cy.contains('Slow data', { timeout: 10000 }).should('be.visible');
```

**Difference from Jest:** In Jest, you write `await screen.findByText(...)`. In Cypress, every command auto-waits. No async/await needed.

---

### Q26 🟡 Cypress vs Playwright — when to use which?

**Answer:**

| | Cypress | Playwright |
|---|---|---|
| Browsers | Chrome, Firefox, Edge | Chrome, Firefox, WebKit (Safari) |
| Language | JavaScript only | JS, TS, Python, Java, C# |
| Architecture | Runs inside browser | Controls browser from outside |
| Multi-tab | ❌ Not supported | ✅ Supported |
| iframes | Limited | ✅ Full support |
| Speed | Medium | Faster (parallel by default) |
| Community | Larger, more plugins | Growing fast |
| DX | Interactive runner, time travel | Trace viewer, codegen |

**Choose Cypress** when: team is JS-only, want great interactive debugging, simpler API.
**Choose Playwright** when: need Safari testing, multi-tab/iframe support, faster CI.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 11.2 Cypress Commands & Assertions

---

### Q27 🟡 What are the most common Cypress commands?

**Answer:**

```javascript
// Navigation
cy.visit('/products');
cy.go('back');
cy.reload();

// Finding elements
cy.get('button');                        // CSS selector
cy.get('[data-testid="submit"]');        // Test ID
cy.contains('Add to Cart');              // By text content
cy.get('form').find('input');            // Scoped query

// Interactions
cy.get('input').type('hello');
cy.get('input').clear();
cy.get('button').click();
cy.get('select').select('Option A');
cy.get('input[type="checkbox"]').check();

// Assertions
cy.get('h1').should('have.text', 'Welcome');
cy.get('.error').should('be.visible');
cy.get('.spinner').should('not.exist');
cy.url().should('include', '/dashboard');
cy.get('li').should('have.length', 3);
```

---

### Q28 🟡 What is chaining in Cypress?

**Answer:**

Every Cypress command yields a subject that the next command operates on:

```javascript
cy.get('form')               // Yields <form>
  .find('input[name="email"]') // Yields <input> inside form
  .type('alice@test.com')      // Types into that input
  .should('have.value', 'alice@test.com'); // Asserts on that input

// ⚠️ Gotcha: some commands change the subject
cy.get('ul')
  .find('li')                 // Subject is now <li> elements
  .first()                    // Subject is now first <li>
  .should('contain', 'Item 1');
```

---

### Q29 🔴 What is the difference between `cy.get()` and `cy.contains()`?

**Answer:**

```javascript
// cy.get() — finds by CSS selector, returns ALL matches
cy.get('button');            // All <button> elements
cy.get('.btn-primary');      // By class
cy.get('[data-testid="x"]'); // By attribute

// cy.contains() — finds by TEXT content, returns first match
cy.contains('Submit');            // First element with "Submit" text
cy.contains('button', 'Submit');  // First <button> with "Submit" text

// Key difference:
cy.get('button').contains('Submit');  // Find <button>, then check text
cy.contains('button', 'Submit');      // Find <button> WITH text (one command)
```

`cy.contains` is more readable for user-facing text. `cy.get` is for structural queries.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 11.3 cy.intercept & Fixtures

---

### Q30 🟡 How do you mock API calls in Cypress?

**Answer:**

```javascript
// cy.intercept — intercepts network requests
cy.intercept('GET', '/api/users', {
  statusCode: 200,
  body: [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' },
  ],
}).as('getUsers');  // Give it an alias

cy.visit('/users');
cy.wait('@getUsers');  // Wait for the request to complete
cy.contains('Alice').should('be.visible');
```

---

### Q31 🟡 What are fixtures and how do you use them?

**Answer:**

Fixtures are static JSON files for test data:

```json
// cypress/fixtures/users.json
[
  { "id": 1, "name": "Alice", "email": "alice@test.com" },
  { "id": 2, "name": "Bob", "email": "bob@test.com" }
]
```

```javascript
// Use in test:
cy.intercept('GET', '/api/users', { fixture: 'users.json' }).as('getUsers');

cy.visit('/users');
cy.wait('@getUsers');
cy.contains('Alice').should('be.visible');
```

**Why fixtures:** Reusable across tests, version controlled, consistent test data.

---

### Q32 🔴 How do you test error and loading states with cy.intercept?

**Answer:**

```javascript
// Simulate slow response
cy.intercept('GET', '/api/users', (req) => {
  req.reply({
    delay: 3000,  // 3 second delay
    body: [{ id: 1, name: 'Alice' }],
  });
}).as('slowRequest');

cy.visit('/users');
cy.contains('Loading...').should('be.visible');
cy.wait('@slowRequest');
cy.contains('Loading...').should('not.exist');

// Simulate server error
cy.intercept('GET', '/api/users', {
  statusCode: 500,
  body: { error: 'Internal Server Error' },
}).as('failedRequest');

cy.visit('/users');
cy.wait('@failedRequest');
cy.contains('Something went wrong').should('be.visible');

// Simulate network failure
cy.intercept('GET', '/api/users', { forceNetworkError: true });
cy.visit('/users');
cy.contains('Network error').should('be.visible');
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 11.4 Cypress Auth & Custom Commands

---

### Q33 🟡 How do you handle authentication in Cypress tests?

**Answer:**

```javascript
// ❌ SLOW — log in through the UI every test
beforeEach(() => {
  cy.visit('/login');
  cy.get('#email').type('alice@test.com');
  cy.get('#password').type('password123');
  cy.get('button[type="submit"]').click();
  cy.url().should('include', '/dashboard');
});

// ✅ FAST — set auth token directly
beforeEach(() => {
  cy.window().then((win) => {
    win.localStorage.setItem('authToken', 'fake-jwt-token');
  });
  // OR intercept the auth API:
  cy.intercept('GET', '/api/me', { id: 1, name: 'Alice' });
  cy.visit('/dashboard');
});
```

**Rule:** Log in through UI in ONE test. All other tests bypass the UI and set tokens directly.

---

### Q34 🟡 What are Cypress custom commands?

**Answer:**

```javascript
// cypress/support/commands.js
Cypress.Commands.add('login', (email, password) => {
  cy.window().then((win) => {
    win.localStorage.setItem('authToken', 'fake-token');
  });
  cy.intercept('GET', '/api/me', { id: 1, name: 'Alice', email });
});

Cypress.Commands.add('getBySel', (selector) => {
  return cy.get(`[data-testid="${selector}"]`);
});

// Usage in tests:
beforeEach(() => {
  cy.login('alice@test.com', 'password123');
  cy.visit('/dashboard');
});

it('shows user profile', () => {
  cy.getBySel('profile-name').should('have.text', 'Alice');
});
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 12.1 Playwright Basics

---

### Q35 🟢 What is Playwright?

**Answer:**

Playwright is an E2E testing framework by Microsoft that controls real browsers:

```bash
npm install --save-dev @playwright/test
npx playwright install   # Download browsers
npx playwright test      # Run tests
npx playwright test --ui # Interactive UI mode
```

```javascript
// tests/login.spec.js
import { test, expect } from '@playwright/test';

test('logs in successfully', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill('alice@test.com');
  await page.getByLabel('Password').fill('password123');
  await page.getByRole('button', { name: 'Sign In' }).click();

  await expect(page).toHaveURL(/dashboard/);
  await expect(page.getByText('Welcome, Alice')).toBeVisible();
});
```

---

### Q36 🟡 What makes Playwright different from Cypress?

**Answer:**

```javascript
// Playwright uses async/await (standard JS)
await page.click('button');
await expect(page.locator('h1')).toHaveText('Welcome');

// Cypress uses chaining (custom API)
cy.get('button').click();
cy.get('h1').should('have.text', 'Welcome');
```

| Feature | Playwright | Cypress |
|---|---|---|
| Async model | async/await | Command chaining |
| Multi-browser | Chromium, Firefox, WebKit | Chrome, Firefox, Edge |
| Multi-tab | ✅ | ❌ |
| Parallel | Built-in | Needs Dashboard/plugin |
| Codegen | `npx playwright codegen` | No built-in |
| Trace viewer | ✅ Built-in | Time travel debugger |

---

### Q37 🟡 How does Playwright's auto-wait work?

**Answer:**

Playwright auto-waits for elements to be actionable before interacting:

```javascript
// Playwright waits for button to be:
// 1. Attached to DOM
// 2. Visible
// 3. Stable (not animating)
// 4. Enabled
// 5. Not obscured by other elements
await page.getByRole('button', { name: 'Submit' }).click();

// No need for sleep or explicit waits!
// If button appears after 2 seconds, Playwright waits automatically.

// Custom timeout:
await page.getByText('Slow content').click({ timeout: 10000 });
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 12.2 Playwright Locators & Auto-Wait

---

### Q38 🟡 What are Playwright locators?

**Answer:**

Locators are Playwright's way of finding elements — similar to RTL queries:

```javascript
// Recommended locators (accessible, resilient):
page.getByRole('button', { name: 'Submit' });
page.getByLabel('Email');
page.getByText('Welcome');
page.getByPlaceholder('Search...');
page.getByTestId('submit-btn');

// CSS / XPath (less recommended):
page.locator('button.primary');
page.locator('#submit-form');
page.locator('xpath=//button[@type="submit"]');

// Filtering:
page.getByRole('listitem').filter({ hasText: 'Alice' });
page.locator('tr').filter({ has: page.getByText('Admin') });
```

**Priority is the same as RTL:** Role → Label → Text → TestId.

---

### Q39 🟡 How do you assert in Playwright?

**Answer:**

```javascript
// Element assertions (auto-retry):
await expect(page.getByText('Hello')).toBeVisible();
await expect(page.getByRole('button')).toBeEnabled();
await expect(page.getByRole('textbox')).toHaveValue('alice@test.com');
await expect(page.getByRole('heading')).toHaveText('Dashboard');
await expect(page.getByTestId('items')).toHaveCount(5);

// Page assertions:
await expect(page).toHaveURL(/dashboard/);
await expect(page).toHaveTitle('My App - Dashboard');

// Negative assertions:
await expect(page.getByText('Error')).not.toBeVisible();
await expect(page.getByRole('button')).not.toBeDisabled();

// Screenshot assertions (visual regression):
await expect(page).toHaveScreenshot('dashboard.png');
```

All `expect` assertions auto-retry for up to 5 seconds by default.

---

### Q40 🔴 How do you test across multiple pages/tabs?

**Answer:**

```javascript
// Playwright can handle multiple tabs — Cypress cannot
test('opens link in new tab', async ({ page, context }) => {
  await page.goto('/');

  // Listen for new page (tab) event
  const [newPage] = await Promise.all([
    context.waitForEvent('page'),
    page.getByRole('link', { name: 'Open Docs' }).click(),
  ]);

  // Assert on the new tab
  await expect(newPage).toHaveURL(/docs/);
  await expect(newPage.getByText('Documentation')).toBeVisible();

  // Original page is still accessible
  await expect(page.getByText('Home')).toBeVisible();
});
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 12.3 Playwright API Mocking & Codegen

---

### Q41 🟡 How do you mock API calls in Playwright?

**Answer:**

```javascript
test('shows mocked users', async ({ page }) => {
  // Intercept API call
  await page.route('/api/users', (route) => {
    route.fulfill({
      status: 200,
      contentType: 'application/json',
      body: JSON.stringify([{ id: 1, name: 'Alice' }]),
    });
  });

  await page.goto('/users');
  await expect(page.getByText('Alice')).toBeVisible();
});

// Simulate error:
await page.route('/api/users', (route) => {
  route.fulfill({ status: 500, body: 'Server Error' });
});

// Simulate network failure:
await page.route('/api/users', (route) => { route.abort(); });
```

---

### Q42 🟡 What is Playwright Codegen?

**Answer:**

Codegen records your browser actions and generates test code:

```bash
npx playwright codegen http://localhost:3000
```

A browser opens. You click, type, navigate. Playwright writes the test:

```javascript
// Auto-generated code:
test('test', async ({ page }) => {
  await page.goto('http://localhost:3000/');
  await page.getByLabel('Email').fill('alice@test.com');
  await page.getByLabel('Password').fill('password123');
  await page.getByRole('button', { name: 'Sign In' }).click();
  await expect(page.getByText('Welcome')).toBeVisible();
});
```

**Use Codegen for:** Getting started quickly, discovering correct locators, generating boilerplate. Then refine the generated code manually.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 13.1 Code Coverage

---

### Q43 🟢 How do you generate a coverage report?

**Answer:**

```bash
npx jest --coverage
```

Output:

```
----------|---------|----------|---------|---------|
File      | % Stmts | % Branch | % Funcs | % Lines |
----------|---------|----------|---------|---------|
Button.js |   95.00 |    80.00 |  100.00 |   95.00 |
utils.js  |  100.00 |   100.00 |  100.00 |  100.00 |
----------|---------|----------|---------|---------|
All files |   97.50 |    90.00 |  100.00 |   97.50 |
```

| Column | Measures |
|---|---|
| Statements | Total executable statements run |
| Branches | if/else, ternary, switch paths taken |
| Functions | Functions that were called |
| Lines | Lines of code executed |

---

### Q44 🟢 What is a good coverage threshold?

**Answer:**

```javascript
// jest.config.js
coverageThreshold: {
  global: {
    branches: 80,
    functions: 80,
    lines: 80,
    statements: 80,
  },
},
```

| Level | Meaning |
|---|---|
| 80%+ | Good for most projects |
| 90%+ | High quality, slower to maintain |
| 100% | Unrealistic — diminishing returns, brittle tests |

**80% is the sweet spot.** It catches most bugs without creating test maintenance burden.

**What coverage does NOT tell you:** That your tests are good. You can have 100% coverage with zero meaningful assertions.

---

### Q45 🟡 What does low branch coverage mean?

**Answer:**

Branch coverage tracks if/else, ternary, and switch paths. Low branch means untested paths:

```javascript
// This function has 2 branches:
function getDiscount(user) {
  if (user.isPremium) {
    return 0.20;  // Branch 1
  }
  return 0;        // Branch 2
}

// If you only test isPremium = true → 50% branch coverage
// You need BOTH cases for 100% branch coverage
test('premium gets 20%', () => { expect(getDiscount({ isPremium: true })).toBe(0.20); });
test('regular gets 0%', () => { expect(getDiscount({ isPremium: false })).toBe(0); });
```

**Common gaps:** Error handling branches, early returns, default switch cases, null checks.

---

### Q46 🟡 How do you exclude files from coverage?

**Answer:**

```javascript
// jest.config.js
collectCoverageFrom: [
  'src/**/*.{js,jsx,ts,tsx}',
  '!src/index.js',              // Exclude entry point
  '!src/**/*.stories.{js,jsx}', // Exclude Storybook
  '!src/**/*.d.ts',             // Exclude type declarations
  '!src/mocks/**',              // Exclude test mocks
  '!src/test-utils/**',         // Exclude test utilities
],

coveragePathIgnorePatterns: [
  '/node_modules/',
  '/build/',
  'setupTests.js',
],
```

Or per-file with comments:

```javascript
/* istanbul ignore next */
function debugOnly() {
  // This function won't count toward coverage
}

/* istanbul ignore file */
// Entire file excluded
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 13.2 Test Architecture & Strategy

---

### Q47 🟡 How should you structure test files in a React project?

**Answer:**

```
src/
├── components/
│   ├── Button/
│   │   ├── Button.jsx
│   │   ├── Button.test.jsx      ← co-located unit/integration test
│   │   └── Button.module.css
│   └── TodoList/
│       ├── TodoList.jsx
│       ├── TodoList.test.jsx
│       └── TodoList.module.css
├── hooks/
│   ├── useAuth.js
│   └── useAuth.test.js
├── utils/
│   ├── formatDate.js
│   └── formatDate.test.js
├── test-utils/
│   └── index.js                  ← custom render with providers
├── mocks/
│   ├── handlers.js               ← MSW handlers
│   └── server.js                 ← MSW server setup
└── __mocks__/
    ├── styleMock.js
    └── fileMock.js

cypress/
├── e2e/
│   ├── auth.cy.js                ← E2E tests (separate from unit)
│   └── checkout.cy.js
└── fixtures/
    └── users.json
```

**Rule:** Co-locate unit tests. Separate E2E tests in their own folder.

---

### Q48 🟡 How do you decide what to unit test vs integration test vs E2E test?

**Answer:**

| Type | Tests | Speed | Confidence | Example |
|---|---|---|---|---|
| Unit | One function/hook in isolation | Fastest | Low | `formatPrice(99.9) → "$99.90"` |
| Integration | Component + children + state + API | Fast | High | Render TodoApp, add item, see list |
| E2E | Full app in real browser | Slowest | Highest | Login → add todo → check persisted |

**What to put where:**

```
Unit tests:
  ✅ Utility functions (formatDate, validateEmail)
  ✅ Reducers, selectors (pure functions)
  ✅ Custom hooks (complex logic)

Integration tests (RTL):
  ✅ Component renders correctly with props
  ✅ User interactions (click, type, submit)
  ✅ API fetching (with MSW)
  ✅ Context/Redux integration

E2E tests (Cypress/Playwright):
  ✅ Critical user flows (login, checkout, signup)
  ✅ Multi-page navigation
  ✅ Auth persistence across pages
```

**80/20 rule:** 80% integration tests, 10% unit, 10% E2E.

---

### Q49 🔴 What are test data factories and why use them?

**Answer:**

Factories generate consistent test data, avoiding copy-paste:

```javascript
// test-utils/factories.js
let nextId = 1;

export const createUser = (overrides = {}) => ({
  id: nextId++,
  name: 'Alice',
  email: 'alice@test.com',
  role: 'user',
  createdAt: '2024-01-01',
  ...overrides,  // Override any field
});

export const createTodo = (overrides = {}) => ({
  id: nextId++,
  text: 'Buy milk',
  completed: false,
  userId: 1,
  ...overrides,
});

// Usage in tests:
test('shows admin badge', () => {
  const admin = createUser({ role: 'admin', name: 'Bob' });
  render(<UserCard user={admin} />);
  expect(screen.getByText('Admin')).toBeInTheDocument();
});

test('shows completed style', () => {
  const todo = createTodo({ completed: true });
  render(<TodoItem todo={todo} />);
  expect(screen.getByText('Buy milk')).toHaveClass('completed');
});
```

**Benefits:** DRY, consistent defaults, explicit overrides, easy to maintain.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 13.3 CI/CD Integration

---

### Q50 🟡 How do you run Jest in GitHub Actions?

**Answer:**

```yaml
# .github/workflows/test.yml
name: Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
      - run: npm ci
      - run: npm test -- --ci --coverage --forceExit
      - uses: actions/upload-artifact@v4
        with:
          name: coverage
          path: coverage/
```

**Key flags for CI:** `--ci` (fail on new snapshots), `--coverage`, `--forceExit` (don't hang).

---

### Q51 🟡 What are pre-commit hooks and how do they help?

**Answer:**

Run tests automatically before every commit to catch bugs early:

```bash
npm install --save-dev husky lint-staged
npx husky init
```

```json
// package.json
{
  "lint-staged": {
    "src/**/*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "jest --bail --findRelatedTests"
    ]
  }
}
```

```bash
# .husky/pre-commit
npx lint-staged
```

`--findRelatedTests` runs ONLY tests related to staged files — fast, targeted.

**Flow:** `git commit` → husky triggers → lint-staged runs ESLint + Jest on changed files → commit proceeds or fails.

---

### Q52 🔴 How do you handle flaky tests in CI?

**Answer:**

Flaky tests pass sometimes and fail sometimes — CI's worst enemy.

**Common causes and fixes:**

```javascript
// 1. Race conditions — missing await
// ❌ Flaky:
user.click(button);
expect(screen.getByText('Done')).toBeInTheDocument();
// ✅ Fixed:
await user.click(button);
await waitFor(() => expect(screen.getByText('Done')).toBeInTheDocument());

// 2. Shared state between tests
// ❌ Flaky: tests depend on execution order
let count = 0;
test('first', () => { count++; expect(count).toBe(1); });
test('second', () => { count++; expect(count).toBe(2); });
// ✅ Fixed: reset in beforeEach
beforeEach(() => { count = 0; });

// 3. Timer-dependent tests
// ❌ Flaky: real timers are unpredictable
await new Promise(r => setTimeout(r, 1000));
// ✅ Fixed: use fake timers
jest.useFakeTimers();
act(() => jest.advanceTimersByTime(1000));

// 4. Date-dependent tests
// ❌ Flaky: different results on different days
expect(formatDate(new Date())).toBe('April 14, 2026');
// ✅ Fixed: mock the date
jest.setSystemTime(new Date('2026-04-14'));
```

**Detection:** Run tests with `--runInBand` (serial). If they pass serial but fail parallel, you have shared state.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 13.4 Debugging & Performance

---

### Q53 🟢 How do you read Jest error messages?

**Answer:**

```
FAIL  src/Button.test.jsx
  ● Button › shows text

    expect(element).toHaveTextContent()

    Expected element to have text content:
      Save Changes            ← What you expected
    Received:
      Save                    ← What was actually rendered

      12 |   render(<Button label="Save" />);
    > 13 |   expect(screen.getByRole('button')).toHaveTextContent('Save Changes');
         |                                      ^
      14 | });

    at Object.<anonymous> (src/Button.test.jsx:13:38)
```

**Read bottom to top:**
1. **File and line** → `Button.test.jsx:13` — where it failed
2. **Expected vs Received** → you expected "Save Changes" but got "Save"
3. **Matcher** → `toHaveTextContent` — what check failed
4. **Test name** → `Button › shows text` — which test

---

### Q54 🟡 Why are my tests slow and how do I speed them up?

**Answer:**

```bash
# 1. Find slow tests:
npx jest --verbose --logHeapUsage

# 2. Common speed fixes:
```

| Problem | Fix |
|---|---|
| Too many `render()` calls | Combine related assertions into fewer tests |
| Slow transforms (Babel) | Switch to `@swc/jest` (10x faster) |
| Real timers in tests | Use `jest.useFakeTimers()` |
| Large snapshot files | Delete stale snapshots, use inline |
| Running all tests | Use `--watch` or `--changedSince=main` |
| Sequential execution | Default parallel is fine, `--maxWorkers=50%` if OOM |
| Heavy imports | Mock large modules you don't test |

```javascript
// jest.config.js — speed optimizations
module.exports = {
  transform: { '^.+\\.jsx?$': '@swc/jest' },  // Fast transforms
  maxWorkers: '50%',                            // Don't use all CPUs
  testTimeout: 5000,                            // Fail fast on hanging tests
};
```

---

### Q55 🟡 How do you debug tests in VS Code?

**Answer:**

```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug Jest Tests",
      "type": "node",
      "request": "launch",
      "runtimeExecutable": "npx",
      "runtimeArgs": ["jest", "--runInBand", "--no-coverage",
                       "${relativeFile}"],
      "console": "integratedTerminal"
    }
  ]
}
```

**Steps:**
1. Set breakpoints in test file or component
2. Open the test file
3. Press F5 (Start Debugging)
4. Jest runs in debug mode — stops at breakpoints

Also useful: `screen.debug()` prints the DOM, `logRoles(container)` shows available roles.

---

### Q56 🔴 What does `--detectOpenHandles` do?

**Answer:**

Jest exits after all tests finish. If something keeps Node.js alive (open connection, timer, listener), Jest hangs. `--detectOpenHandles` finds the culprit:

```bash
npx jest --detectOpenHandles
```

```
Jest has detected the following 1 open handle:
  ●  TCPSERVERWRAP
      at createServer (net.js:1130:10)
      at new Server (http.js:282:22)
      at setupServer (src/mocks/server.js:5:10)
```

**Common causes:**

| Open handle | Fix |
|---|---|
| MSW server | `afterAll(() => server.close())` |
| Database connection | `afterAll(() => db.disconnect())` |
| setInterval not cleared | `afterEach(() => jest.clearAllTimers())` |
| Event listener | `afterEach(() => window.removeEventListener(...))` |
| WebSocket | `afterAll(() => ws.close())` |

If you can't fix it immediately: `npx jest --forceExit` (kills process, not recommended long-term).

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 13.5 Vitest

---

### Q57 🟡 What is Vitest and how is it different from Jest?

**Answer:**

Vitest is a Vite-native test runner — same API as Jest but faster:

| | Jest | Vitest |
|---|---|---|
| Bundler | Webpack / Babel | Vite / esbuild |
| Speed | ~5s startup | ~1s startup |
| ESM | Experimental | Native |
| Config | `jest.config.js` | Inside `vite.config.js` |
| API | `jest.fn()`, `jest.mock()` | `vi.fn()`, `vi.mock()` |
| Watch mode | Good | Faster (Vite HMR) |

```javascript
// Vitest — almost identical API:
import { describe, test, expect, vi } from 'vitest';

test('adds numbers', () => {
  expect(1 + 2).toBe(3);
});

const mock = vi.fn();  // vi instead of jest
vi.mock('./api');       // vi instead of jest
```

---

### Q58 🟢 How do you migrate from Jest to Vitest?

**Answer:**

```bash
npm install --save-dev vitest
```

```javascript
// vite.config.js
import { defineConfig } from 'vite';

export default defineConfig({
  test: {
    environment: 'jsdom',
    globals: true,         // No need to import describe/test/expect
    setupFiles: './jest.setup.js',  // Reuse existing setup
  },
});
```

**Search and replace:**

```
jest.fn()    →  vi.fn()
jest.mock()  →  vi.mock()
jest.spyOn() →  vi.spyOn()
jest.useFakeTimers() → vi.useFakeTimers()
```

Everything else (`describe`, `test`, `expect`, `beforeEach`) is identical. Most tests work without any changes.

---

[↑ Back to Table of Contents](#table-of-contents)

---

# 14.1 Tricky Jest Output Questions

---

### Q59 🔴 What is the output? (Mock hoisting)

```javascript
import { fetchUsers } from './api';

jest.mock('./api', () => ({
  fetchUsers: jest.fn(() => 'mocked'),
}));

test('hoisting', () => {
  console.log(fetchUsers());
});
```

**Answer:** `"mocked"`

`jest.mock()` is hoisted ABOVE the `import` statement. So by the time `fetchUsers` is imported, it's already the mock. The execution order is:

```javascript
// 1. jest.mock('./api', ...) — hoisted to top
// 2. import { fetchUsers } from './api' — gets the mocked version
// 3. test runs — fetchUsers() returns 'mocked'
```

---

### Q60 🔴 What is the output? (mockClear vs mockReset)

```javascript
const fn = jest.fn().mockReturnValue('hello');

fn('a');
fn('b');

fn.mockClear();
console.log(fn.mock.calls.length);  // ?
console.log(fn());                   // ?

fn.mockReset();
console.log(fn());                   // ?
```

**Answer:**

```
0          ← mockClear removed call history
"hello"    ← mockClear did NOT remove mockReturnValue
undefined  ← mockReset removed mockReturnValue too
```

---

### Q61 🔴 What is the output? (toBe vs toEqual)

```javascript
const a = { x: 1, y: [2, 3] };
const b = { x: 1, y: [2, 3] };
const c = a;

test('comparisons', () => {
  expect(a).toBe(b);       // ?
  expect(a).toEqual(b);    // ?
  expect(a).toBe(c);       // ?
  expect(a.y).toBe(b.y);   // ?
  expect(a.y).toEqual(b.y); // ?
});
```

**Answer:**

```javascript
expect(a).toBe(b);       // ❌ FAIL — different references
expect(a).toEqual(b);    // ✅ PASS — same values (deep)
expect(a).toBe(c);       // ✅ PASS — c IS a (same reference)
expect(a.y).toBe(b.y);   // ❌ FAIL — different array references
expect(a.y).toEqual(b.y); // ✅ PASS — same array values
```

---

### Q62 🔴 What is the output? (act warning)

```javascript
test('act warning', () => {
  render(<AsyncCounter />);

  // AsyncCounter fetches data in useEffect
  // and calls setState when data arrives

  expect(screen.getByText('Count: 0')).toBeInTheDocument();
});
// What happens after the test?
```

**Answer:**

The test PASSES but prints a warning:

```
Warning: An update to AsyncCounter inside a test was not wrapped in act(...).
```

The fetch resolves AFTER the test finishes, causing a state update on an unmounted component. Fix:

```javascript
test('act warning fixed', async () => {
  render(<AsyncCounter />);
  expect(await screen.findByText('Count: 5')).toBeInTheDocument();
  // Now the test waits for the async update before finishing
});
```

---

### Q63 🔴 What is wrong? (Fake timer gotcha)

```javascript
test('debounce', async () => {
  jest.useFakeTimers();
  const user = userEvent.setup();
  render(<SearchBar onSearch={mockSearch} />);

  await user.type(screen.getByRole('textbox'), 'react');
  jest.advanceTimersByTime(500);

  expect(mockSearch).toHaveBeenCalledWith('react');
  jest.useRealTimers();
});
// Test hangs and times out!
```

**Answer:**

`userEvent` uses real delays internally (`setTimeout`). With fake timers, those delays never resolve, so `user.type()` hangs.

```javascript
// ✅ FIX: Tell userEvent to use fake timers
const user = userEvent.setup({ advanceTimers: jest.advanceTimersByTime });
```

---

### Q64 🔴 What is the output? (Async false positive)

```javascript
test('this test has a bug', () => {
  render(<App />);

  fetch('/api/data').then(data => {
    expect(data.items).toHaveLength(5);
  });
});
```

**Answer:** Test **PASSES** even if the assertion is wrong!

The `.then()` runs asynchronously AFTER the test finishes. Jest sees zero assertions, no errors, marks as passed. The assertion inside `.then()` runs later and its failure is swallowed.

```javascript
// ✅ FIX 1: Use async/await
test('fixed', async () => {
  render(<App />);
  const data = await fetch('/api/data');
  expect(data.items).toHaveLength(5);
});

// ✅ FIX 2: Use expect.assertions
test('fixed', () => {
  expect.assertions(1);  // MUST have exactly 1 assertion
  return fetch('/api/data').then(data => {
    expect(data.items).toHaveLength(5);
  });
});
```

---

### Q65 🔴 What is the output? (Module mock scope)

```javascript
// file: math.test.js
import { add } from './math';

jest.mock('./math', () => ({
  add: jest.fn(() => 999),
}));

test('test 1', () => {
  console.log(add(1, 2));  // ?
  add.mockReturnValue(0);
});

test('test 2', () => {
  console.log(add(1, 2));  // ?
});
```

**Answer:**

```
999    ← test 1: uses the factory mock implementation
0      ← test 2: mockReturnValue persists across tests!
```

`jest.mock()` applies to the entire file. Mock state (like `mockReturnValue`) persists between tests unless you clear it.

```javascript
// ✅ FIX: Clear mocks between tests
afterEach(() => { jest.clearAllMocks(); });
// OR in jest.config.js: clearMocks: true
```

---

### Q66 🔴 What is the output? (Snapshot confusion)

```javascript
test('snapshot', () => {
  const { container } = render(<Button />);
  expect(container).toMatchSnapshot();
});
// First run: PASS, creates snapshot
// You change Button's class from "btn" to "btn-primary"
// Second run: ???
```

**Answer:** Test **FAILS**:

```
Snapshot failed:
- <button class="btn">Click</button>
+ <button class="btn-primary">Click</button>
```

You must decide: Is this change intentional?
- **Yes:** `npx jest -u` to update the snapshot
- **No:** Fix the code

**Gotcha:** Developers often run `jest -u` without looking. This defeats the purpose of snapshot testing. Always review snapshot diffs in code review.

---

### Q67 🔴 What is wrong? (Testing implementation)

```javascript
test('increments count', () => {
  const setState = jest.fn();
  jest.spyOn(React, 'useState').mockReturnValue([0, setState]);

  render(<Counter />);
  fireEvent.click(screen.getByText('+'));

  expect(setState).toHaveBeenCalledWith(1);
});
```

**Answer:** This is a terrible test. It tests **implementation** (useState was called) not **behavior** (count showed 1).

Problems:
1. Mocking `React.useState` is fragile — breaks if component refactors
2. Doesn't test the actual DOM update
3. Tests React internals, not your component

```javascript
// ✅ CORRECT — test behavior
test('increments count', async () => {
  const user = userEvent.setup();
  render(<Counter />);

  expect(screen.getByText('Count: 0')).toBeInTheDocument();
  await user.click(screen.getByRole('button', { name: '+' }));
  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

---

### Q68 🔴 What is the output? (queryBy returns null, not throws)

```javascript
test('element check', () => {
  render(<App />);

  const error = screen.getByText('Error');    // ?
  const error2 = screen.queryByText('Error'); // ?
});
// Assume "Error" text does NOT exist in the DOM
```

**Answer:**

```javascript
screen.getByText('Error');    // ❌ THROWS: Unable to find element with text "Error"
screen.queryByText('Error');  // Returns null (test never reaches this line)
```

The test crashes at line 1. It never reaches line 2.

---

### Q69 🔴 What is the output? (beforeEach order)

```javascript
const order = [];

beforeAll(() => order.push('A'));
beforeEach(() => order.push('B'));

describe('outer', () => {
  beforeAll(() => order.push('C'));
  beforeEach(() => order.push('D'));

  test('test 1', () => order.push('T1'));

  describe('inner', () => {
    beforeAll(() => order.push('E'));
    beforeEach(() => order.push('F'));

    test('test 2', () => order.push('T2'));
  });
});

afterAll(() => console.log(order.join(', ')));
```

**Answer:**

```
A, C, B, D, T1, E, B, D, F, T2
```

Step by step:
1. `A` — root beforeAll
2. `C` — outer beforeAll
3. `B, D` — root + outer beforeEach for test 1
4. `T1` — test 1 runs
5. `E` — inner beforeAll
6. `B, D, F` — root + outer + inner beforeEach for test 2
7. `T2` — test 2 runs

---

### Q70 🔴 What is wrong? (Partial mock missing requireActual)

```javascript
jest.mock('./api', () => ({
  fetchUsers: jest.fn(() => []),
}));

import { fetchUsers, fetchProducts } from './api';

test('fetch products', () => {
  console.log(fetchProducts());  // ?
});
```

**Answer:** `TypeError: fetchProducts is not a function`

The mock factory replaces the ENTIRE module. Since you only defined `fetchUsers`, `fetchProducts` doesn't exist.

```javascript
// ✅ FIX: Use jest.requireActual to keep other exports
jest.mock('./api', () => ({
  ...jest.requireActual('./api'),
  fetchUsers: jest.fn(() => []),
}));
```

---

[↑ Back to Table of Contents](#table-of-contents)

---

# Quick Reference — All 70 Questions at a Glance

| # | Topic | Question Summary | Difficulty |
|---|-------|-----------------|------------|
| 1 | Hooks | Testing custom hooks with renderHook | 🟢 |
| 2 | Hooks | Hook with arguments and rerender | 🟡 |
| 3 | Hooks | Hook that calls an API | 🟡 |
| 4 | Hooks | Hook with Provider wrapper | 🔴 |
| 5 | Context | Testing Context consumer | 🟡 |
| 6 | Context | Custom render with all Providers | 🟡 |
| 7 | Context | Testing Provider updates | 🔴 |
| 8 | Redux | Testing reducers | 🟢 |
| 9 | Redux | Testing connected component | 🟡 |
| 10 | Redux | Testing async thunks | 🟡 |
| 11 | Redux | Testing selectors | 🔴 |
| 12 | React Query | Components with QueryClient | 🟡 |
| 13 | React Query | Why retry: false in tests | 🟡 |
| 14 | React Query | Loading/error/success states | 🔴 |
| 15 | Routing | Testing navigation | 🟡 |
| 16 | Routing | Testing protected routes | 🟡 |
| 17 | Routing | Testing route params | 🔴 |
| 18 | Error Boundaries | Testing error boundary fallback | 🟡 |
| 19 | Error Boundaries | Async errors vs boundary | 🔴 |
| 20 | Portals/Modals | Testing modal open/close | 🟡 |
| 21 | Suspense | Testing React.lazy + Suspense | 🟡 |
| 22 | Refs | Testing useRef and forwardRef | 🔴 |
| 23 | Cypress | What is Cypress, Jest vs Cypress | 🟢 |
| 24 | Cypress | Basic Cypress test | 🟢 |
| 25 | Cypress | Auto-retry / auto-wait | 🟡 |
| 26 | Cypress | Cypress vs Playwright comparison | 🟡 |
| 27 | Cypress | Common Cypress commands | 🟡 |
| 28 | Cypress | Chaining in Cypress | 🟡 |
| 29 | Cypress | cy.get vs cy.contains | 🔴 |
| 30 | Cypress | cy.intercept for API mocking | 🟡 |
| 31 | Cypress | Fixtures for test data | 🟡 |
| 32 | Cypress | Error/loading with cy.intercept | 🔴 |
| 33 | Cypress | Authentication in Cypress | 🟡 |
| 34 | Cypress | Custom commands | 🟡 |
| 35 | Playwright | What is Playwright | 🟢 |
| 36 | Playwright | Playwright vs Cypress | 🟡 |
| 37 | Playwright | Auto-wait behavior | 🟡 |
| 38 | Playwright | Locators | 🟡 |
| 39 | Playwright | Assertions | 🟡 |
| 40 | Playwright | Multi-tab testing | 🔴 |
| 41 | Playwright | API mocking with page.route | 🟡 |
| 42 | Playwright | Codegen | 🟡 |
| 43 | Coverage | Generating coverage reports | 🟢 |
| 44 | Coverage | Good coverage threshold | 🟢 |
| 45 | Coverage | Low branch coverage meaning | 🟡 |
| 46 | Coverage | Excluding files from coverage | 🟡 |
| 47 | Architecture | Test file structure | 🟡 |
| 48 | Architecture | Unit vs integration vs E2E | 🟡 |
| 49 | Architecture | Test data factories | 🔴 |
| 50 | CI/CD | Jest in GitHub Actions | 🟡 |
| 51 | CI/CD | Pre-commit hooks with Husky | 🟡 |
| 52 | CI/CD | Handling flaky tests | 🔴 |
| 53 | Debugging | Reading Jest error messages | 🟢 |
| 54 | Debugging | Speeding up slow tests | 🟡 |
| 55 | Debugging | Debug tests in VS Code | 🟡 |
| 56 | Debugging | --detectOpenHandles | 🔴 |
| 57 | Vitest | Vitest vs Jest | 🟡 |
| 58 | Vitest | Migrating Jest to Vitest | 🟢 |
| 59 | Tricky | Mock hoisting | 🔴 |
| 60 | Tricky | mockClear vs mockReset output | 🔴 |
| 61 | Tricky | toBe vs toEqual for objects | 🔴 |
| 62 | Tricky | act() warning cause | 🔴 |
| 63 | Tricky | Fake timer + userEvent hang | 🔴 |
| 64 | Tricky | Async false positive | 🔴 |
| 65 | Tricky | Module mock scope persistence | 🔴 |
| 66 | Tricky | Snapshot update confusion | 🔴 |
| 67 | Tricky | Testing implementation vs behavior | 🔴 |
| 68 | Tricky | getBy throws vs queryBy returns null | 🔴 |
| 69 | Tricky | Nested beforeAll/beforeEach order | 🔴 |
| 70 | Tricky | Partial mock missing requireActual | 🔴 |