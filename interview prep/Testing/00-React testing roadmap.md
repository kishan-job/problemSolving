# React Testing — Complete Learning Roadmap

> **Total estimated time:** 12–14 weeks (full-time study)
> Phases 1–3 make you job-ready. Phases 4–6 bring senior-level expertise.

---

## 📋 Table of Contents

- [Phase 1 — Foundations](#phase-1--foundations-15-weeks)
  - [Testing Fundamentals](#-testing-fundamentals-23-days)
  - [Jest — Test Runner & Assertions](#-jest--test-runner--assertions-34-days)
  - [Testing Pure Functions & Utilities](#-testing-pure-functions--utilities-2-days)
- [Phase 2 — Component Testing Core](#phase-2--component-testing-core-2-weeks)
  - [React Testing Library (RTL) — Basics](#-react-testing-library-rtl--basics-45-days)
  - [User Interactions — user-event](#-user-interactions--user-event-34-days)
  - [Async Testing](#-async-testing-34-days)
- [Phase 3 — Mocking & Isolation](#phase-3--mocking--isolation-2-weeks)
  - [Jest Mocking Deep Dive](#-jest-mocking-deep-dive-45-days)
  - [API Mocking — MSW](#-api-mocking--msw-57-days)
  - [Mocking React Ecosystem](#-mocking-react-ecosystem-34-days)
- [Phase 4 — Advanced Component Patterns](#phase-4--advanced-component-patterns-25-weeks)
  - [Testing Hooks](#-testing-hooks-34-days)
  - [Testing State Management](#-testing-state-management-57-days)
  - [Testing Routing & Navigation](#-testing-routing--navigation-23-days)
  - [Snapshot & Visual Testing](#-snapshot--visual-testing-23-days)
- [Phase 5 — End-to-End & Integration](#phase-5--end-to-end--integration-25-weeks)
  - [Cypress](#-cypress-710-days)
  - [Playwright](#-playwright-57-days)
  - [Accessibility Testing](#-accessibility-testing-34-days)
- [Phase 6 — Professional Practices & CI/CD](#phase-6--professional-practices--cicd-2-weeks)
  - [Test Architecture & Strategy](#-test-architecture--strategy-34-days)
  - [CI/CD Integration](#-cicd-integration-45-days)
  - [Performance & Advanced Testing](#-performance--advanced-testing-35-days)
  - [Vitest (Modern Alternative)](#-vitest-modern-alternative-23-days)
- [Dependency Flow](#dependency-flow)
- [Quick Reference](#quick-reference)

---

## 🟢 Easy (1–3 days) | 🟠 Medium (4–7 days) | 🔴 Hard (1–3 weeks)

---

## Phase 1 — Foundations (~1.5 weeks)

### 🟢 Testing Fundamentals (2–3 days)

- Why we test — confidence, regression, documentation
- Testing pyramid: unit → integration → E2E
- Testing trophy (Kent C. Dodds model)
- TDD vs BDD — red/green/refactor
- What to test vs what NOT to test
- Test coverage — what it means and its limits

> **Prereq:** Basic JavaScript & React knowledge

### 🟢 Jest — Test Runner & Assertions (3–4 days)

- Setup & configuration (CRA vs Vite vs manual)
- `describe`, `it`, `test` blocks
- Matchers: `toBe`, `toEqual`, `toContain`, `toThrow`, etc.
- Setup/teardown: `beforeEach`, `afterEach`, `beforeAll`, `afterAll`
- Test grouping and organization patterns
- Running tests: watch mode, filtering, verbose output
- Code coverage reports (`--coverage`)

> **Prereq:** Testing fundamentals

### 🟢 Testing Pure Functions & Utilities (2 days)

- Testing helper/util functions in isolation
- Edge cases & boundary values
- Testing data transformations
- Testing validators & formatters

> **Prereq:** Jest basics

---

## Phase 2 — Component Testing Core (~2 weeks)

### 🟠 React Testing Library (RTL) — Basics (4–5 days)

- Philosophy: test behavior, not implementation
- `render()` and `screen` object
- Query priority: `getByRole` → `getByLabelText` → `getByText` → `getByTestId`
- Query variants: `getBy`, `queryBy`, `findBy` (and `*AllBy`)
- Debug tools: `screen.debug()`, `logRoles()`
- `cleanup` and auto-cleanup

> **Prereq:** Jest, React component basics

### 🟠 User Interactions — user-event (3–4 days)

- `fireEvent` vs `userEvent` — why `userEvent` is better
- `click`, `type`, `clear`, `selectOptions`, `tab`, `hover`
- Keyboard navigation testing
- Testing forms: input, submit, validation messages
- File upload simulation

> **Prereq:** RTL basics

### 🟠 Async Testing (3–4 days)

- `waitFor`, `findBy*` queries
- `act()` — when and why
- Testing loading / error / success states
- Fake timers: `jest.useFakeTimers()`, `advanceTimersByTime`
- Testing debounced/throttled functions
- Handling promises in tests

> **Prereq:** RTL basics, user-event

---

## Phase 3 — Mocking & Isolation (~2 weeks)

### 🟠 Jest Mocking Deep Dive (4–5 days)

- `jest.fn()` — mock functions, return values, implementations
- `jest.mock()` — module mocking (auto & manual)
- `jest.spyOn()` — spying on existing methods
- Mocking ES modules vs CommonJS
- Partial mocking (mock some exports, keep others)
- `__mocks__` directory convention
- Clearing, resetting, restoring mocks

> **Prereq:** Jest basics, RTL basics

### 🔴 API Mocking — MSW (5–7 days)

- Why MSW over `jest.mock(fetch)`
- Setting up MSW handlers (REST & GraphQL)
- Request interception at network level
- Per-test handler overrides
- Simulating error responses (4xx, 5xx, network errors)
- Testing loading states with delayed responses
- MSW v2 migration and new API

> **Prereq:** Async testing, Jest mocking

### 🟠 Mocking React Ecosystem (3–4 days)

- Mocking React Router (`useNavigate`, `useParams`)
- Mocking child components
- Mocking browser APIs (`localStorage`, `IntersectionObserver`, `matchMedia`)
- Mocking third-party libraries (`date-fns`, `axios`, etc.)
- Mocking images, CSS modules, static assets

> **Prereq:** Jest mocking deep dive

---

## Phase 4 — Advanced Component Patterns (~2.5 weeks)

### 🟠 Testing Hooks (3–4 days)

- `renderHook` from `@testing-library/react`
- Testing custom hooks with state & effects
- Testing hooks that call APIs
- Testing hook return values and updates
- `act()` with hooks

> **Prereq:** RTL, async testing, mocking

### 🔴 Testing State Management (5–7 days)

- Testing Context API providers & consumers
- Custom render with providers (`wrapper` option)
- Testing Redux: store, reducers, actions, selectors
- Testing Redux Toolkit slices & thunks
- Testing Zustand / Jotai / Recoil stores
- Testing React Query / TanStack Query

> **Prereq:** Hooks testing, mocking ecosystem

### 🟠 Testing Routing & Navigation (2–3 days)

- `MemoryRouter` for tests
- Testing route params & query strings
- Testing navigation on user actions
- Testing protected/private routes
- Testing 404/error routes

> **Prereq:** RTL, mocking React ecosystem

### 🟠 Snapshot & Visual Testing (2–3 days)

- Jest snapshots: `toMatchSnapshot`, `toMatchInlineSnapshot`
- When snapshots help vs when they become noise
- Updating snapshots safely
- Intro to visual regression: Chromatic, Percy, Loki

> **Prereq:** RTL basics

---

## Phase 5 — End-to-End & Integration (~2.5 weeks)

### 🔴 Cypress (7–10 days)

- Setup and project structure
- Selectors and commands (`cy.get`, `cy.contains`, `cy.find`)
- Assertions and chaining
- Intercepting API calls (`cy.intercept`)
- Fixtures and test data
- Custom commands
- Page Object Model pattern
- Testing auth flows (login, session, tokens)
- Component testing in Cypress
- Cypress dashboard & CI/CD integration

> **Prereq:** All of Phases 1–3

### 🔴 Playwright (5–7 days)

- Setup and configuration
- Locators and auto-waiting
- Multi-browser testing (Chromium, Firefox, WebKit)
- API request mocking (`route`)
- Codegen — record and replay
- Parallel test execution
- Visual comparisons (screenshot assertions)
- Trace viewer for debugging

> **Prereq:** All of Phases 1–3 (can learn alongside or instead of Cypress)

### 🟠 Accessibility Testing (3–4 days)

- `jest-axe` for automated a11y checks
- Testing ARIA roles, labels, and live regions
- Keyboard navigation testing
- Screen reader behavior simulation
- Color contrast and focus indicator testing
- Cypress / Playwright a11y plugins (`axe-core`)

> **Prereq:** RTL, one E2E tool

---

## Phase 6 — Professional Practices & CI/CD (~2 weeks)

### 🟠 Test Architecture & Strategy (3–4 days)

- Structuring test files and folders
- Test naming conventions
- Shared test utilities & custom render functions
- Factory functions for test data (`faker.js`, `fishery`)
- Deciding what to unit test vs integration test vs E2E test
- Cost vs value of each test type

> **Prereq:** All previous phases

### 🔴 CI/CD Integration (4–5 days)

- Running tests in GitHub Actions / GitLab CI
- Pre-commit hooks (`husky` + `lint-staged`)
- Coverage thresholds and gates
- Parallelizing test suites
- Flaky test detection and management
- Test reporting (JUnit XML, coverage reports)

> **Prereq:** Test architecture, at least one E2E tool

### 🟠 Performance & Advanced Testing (3–5 days)

- Testing `React.memo`, `useMemo`, `useCallback`
- React Profiler in tests
- Lighthouse CI for performance budgets
- Testing Suspense and lazy-loaded components
- Testing error boundaries
- Testing portals and modals
- Testing SSR / Next.js components

> **Prereq:** All previous phases

### 🟢 Vitest (Modern Alternative) (2–3 days)

- Vitest vs Jest — differences and migration
- Native ESM support & Vite integration
- In-source testing
- UI mode for interactive debugging

> **Prereq:** Jest knowledge (maps directly)

---

## Dependency Flow

```
JavaScript & React basics
    └── Testing Fundamentals
          └── Jest
                └── Pure Functions & Utilities
                      └── React Testing Library (RTL)
                            ├── user-event
                            │     └── Async Testing
                            │           └── Jest Mocking
                            │                 ├── MSW (API Mocking)
                            │                 └── Mocking React Ecosystem
                            │                       ├── Testing Hooks
                            │                       │     └── Testing State Management
                            │                       ├── Testing Routing
                            │                       └── Snapshot Testing
                            └── Cypress / Playwright (E2E)
                                  └── Accessibility Testing
                                        └── Test Architecture
                                              └── CI/CD Integration
                                                    └── Performance & Advanced Testing
```

---

## Quick Reference

| Phase | Focus | Time | Job Readiness |
|-------|-------|------|---------------|
| Phase 1 | Foundations | ~1.5 weeks | Can write basic tests |
| Phase 2 | Component Testing | ~2 weeks | Can test React components |
| Phase 3 | Mocking & Isolation | ~2 weeks | **Job-ready for most roles** |
| Phase 4 | Advanced Patterns | ~2.5 weeks | Can handle complex codebases |
| Phase 5 | E2E Testing | ~2.5 weeks | Full-stack testing capability |
| Phase 6 | Professional Practices | ~2 weeks | **Senior-level expertise** |