# Debugging Guide: Source Maps & React DevTools Across App Architectures

A deep, step-by-step reference for why source maps and React DevTools break in different app setups (plain apps, production builds, iframes, Next.js, Module Federation, micro-frontends), and exactly how a senior developer debugs each one.

## Navigation

| # | Topic |
|---|-------|
| 1 | [Scenario Reference Table](#1-scenario-reference-table) |
| 2 | [Types of Debugging (The Toolbox)](#2-types-of-debugging-the-toolbox) |
| 3 | [Scenario 1: Plain Vite/CRA Dev](#3-scenario-1-plain-vitecra-dev) |
| 4 | [Scenario 2: Production Build (No Sourcemap Flag)](#4-scenario-2-production-build-no-sourcemap-flag) |
| 5 | [Scenario 3: devtool false / GENERATE_SOURCEMAP false](#5-scenario-3-devtool-false--generate_sourcemapfalse) |
| 6 | [Scenario 4: Cross-Origin Iframe](#6-scenario-4-cross-origin-iframe) |
| 7 | [Scenario 5: Next.js Server Components](#7-scenario-5-nextjs-server-components) |
| 8 | [Scenario 6: App Disables the DevTools Hook](#8-scenario-6-app-disables-the-devtools-hook) |
| 9 | [Module Federation — What It Is & Why It Matters](#9-module-federation--what-it-is--why-it-matters) |
| 10 | [Scenario 7: Module Federation (Misconfigured)](#10-scenario-7-module-federation-misconfigured) |
| 11 | [Scenario 8: Micro-Frontends (Iframe or MF) Generally](#11-scenario-8-micro-frontends-iframe-or-mf-generally) |
| 12 | [The Senior Dev's Default Workflow](#12-the-senior-devs-default-workflow) |
| 13 | [Quick-Reference Command Cheatsheet](#13-quick-reference-command-cheatsheet) |

---

## 1. Scenario Reference Table

This is the master table — every scenario below refers back to a row here.

| Scenario | Source maps affected? | React DevTools affected? | Why |
|---|---|---|---|
| Plain Vite/CRA dev | No (both work) | No | Normal happy path |
| Production build (no sourcemap flag) | Yes — minified blob | Sometimes (names minified) | Maps off by default in prod |
| `devtool: false` / `GENERATE_SOURCEMAP=false` | Yes | No (tree still works) | Maps explicitly disabled |
| Cross-origin iframe | Yes (often) | Yes | Isolated `window`, extension can't reach in |
| Next.js Server Components | Partially | Yes (those parts invisible) | No client-side React for them |
| App disables hook (`isDisabled=true`) | No | Yes | Enterprise security choice |
| Module Federation (misconfigured) | Yes (remotes' `.map` not served) | Yes/partial (multiple React) | Remotes load from other origins |
| Micro-frontends (iframe or MF) | Often | Often | Combination of the above two rows |

**How to read this table:** the two right-hand columns are independent failure modes. A scenario can break source maps only, DevTools only, both, or neither — knowing *which* combination you're facing is the first diagnostic step, before you open a single breakpoint.

**[⬆ Back to navigation](#navigation)**

---

## 2. Types of Debugging (The Toolbox)

Before working through scenarios, here are the ten categories of debugging technique referenced throughout this guide. A senior dev doesn't pick just one — they layer several depending on what the scenario table tells them is broken.

| Type | What it means | When you reach for it |
|---|---|---|
| **A. Source-level debugging** | Breakpoints in your actual `.jsx`/`.ts` files via clean source maps | Default, happiest path |
| **B. Binary/minified debugging** | Breakpoints in raw bundled code, pretty-printed | When maps are broken/missing |
| **C. Statement-based debugging** | `debugger;`, `console.log/table/trace` inserted in source | Fastest, map-independent |
| **D. Runtime/state inspection** | React DevTools Components/Profiler, reading Fiber directly | React-specific, map-independent |
| **E. Network-level debugging** | Network tab, XHR/fetch breakpoints, request inspection | API/data flow issues |
| **F. DOM-level debugging** | Elements tab, `$0`, DOM breakpoints (attribute/subtree change) | UI/rendering issues |
| **G. Cross-context debugging** | Multiple DevTools instances, `chrome://inspect`, iframe-specific inspection | iframes, multi-window, workers |
| **H. Bundle/dependency-level debugging** | Inspecting `renderers`, checking `node_modules`, bundle analyzers | Multiple React copies, Module Federation issues |
| **I. Server-side debugging** | Node inspector, server logs, SSR-specific tracing | Server Components, SSR bugs |
| **J. Production/remote debugging** | Sentry/LogRocket-style tools, privately uploaded source maps | Live incidents you can't reproduce locally |

Below is the step-by-step "how to actually do this" for each type, one at a time.

### A. Source-level debugging — step by step

1. Open DevTools → **Sources** tab.
2. In the left file tree, locate your file under the page's origin (e.g. `localhost:5173` → `src/components/Header.jsx`).
3. Click the line number where you want to pause — a blue marker appears, confirming the breakpoint is set.
4. Trigger the code path in the app (click the button, submit the form, etc.).
5. Execution pauses at that line. Use the right-hand panel to inspect:
   - **Scope** — local variables, closures, `this`.
   - **Watch** — add any expression to track continuously.
   - **Call Stack** — see exactly how execution got here.
6. Use the step controls at the top of the panel: **Step over** (F10), **Step into** (F11), **Step out** (Shift+F11), **Resume** (F8).
7. Right-click a breakpoint → **Edit breakpoint** to add a condition (e.g. `userId === 'abc123'`) so it only pauses when relevant.

### B. Binary/minified debugging — step by step

1. Open DevTools → **Sources** tab, find the minified bundle (e.g. `main.abc123.js`) even though it appears as one long line.
2. Click the **{ }** "Pretty print" icon at the bottom-left of the code pane — Chrome reformats it with indentation.
3. Use **Ctrl+F** (inside that file, not the page) to search for a distinctive string — an error message, a unique class name, a literal value you know appears near the bug.
4. Click the line number next to the relevant (now-readable) line to set a breakpoint, same as Type A — variable names will be minified (`e`, `t`, `n`), but logic flow is still traceable.
5. Optional: right-click the file tab → **Save for overrides** (see Type C below) if you need the breakpoint to persist across reloads.

### C. Statement-based debugging — step by step

1. Open the actual source file in your editor (not DevTools).
2. Insert a `debugger;` statement directly at the point you want to pause:
   ```js
   function handleSubmit() {
     debugger;
     // rest of logic
   }
   ```
3. Save the file — if using `npm run dev`, the change hot-reloads automatically.
4. Open DevTools (must be open for `debugger;` to trigger) and re-run the action — execution pauses exactly like a manual breakpoint.
5. For non-pausing visibility, use `console.log`/`console.table`/`console.trace` instead:
   ```js
   console.log('handleSubmit called', { formData, userId });
   console.table(listOfItems);       // readable table for arrays/objects
   console.trace();                   // prints the call stack to this point
   ```
6. Remove all `debugger;` statements before committing — add the ESLint rule `no-debugger` to catch forgotten ones automatically in CI.
7. For production bundles you can't edit directly, use **DevTools → Sources → Overrides**: right-click the file → **Save for overrides** → edit the served copy directly in DevTools → your `debugger;`/log persists across reloads without touching the deployed source.

### D. Runtime/state inspection (React DevTools) — step by step

1. Install/enable the **React Developer Tools** browser extension.
2. Open DevTools → switch to the **Components** tab.
3. Click any component in the tree — the right panel shows its live **props**, **state**, and **hooks** values.
4. Edit a prop or state value directly in that panel to test behavior without changing code (useful for reproducing edge cases).
5. Use the search bar at the top of the Components tab to jump straight to a component by name in a large tree.
6. Right-click a component → **"Log this component's data to console"** — dumps its full props/state as an inspectable object in Console.
7. Switch to the **Profiler** tab → click **Record** → interact with the app → **Stop** — see a flame graph of what re-rendered and, when you click a bar, *why* (which prop/state changed).

### E. Network-level debugging — step by step

1. Open DevTools → **Network** tab, and reload the page to capture everything from the start.
2. Filter by type (`Fetch/XHR`, `JS`, `Doc`) using the filter bar to cut noise.
3. Click any request to inspect **Headers**, **Payload** (what was sent), **Response** (what came back), and **Timing**.
4. To pause execution when a specific request fires: DevTools → **Sources** → left sidebar → **XHR/Fetch Breakpoints** panel → click **+** → enter a URL substring (or leave blank to pause on *any* request) → execution now pauses right before that request is sent.
5. Right-click any request → **Copy → Copy as fetch** to replay it independently in Console with modified parameters.
6. Use the **Throttling** dropdown (top of Network tab) to simulate slow connections and catch race conditions.

### F. DOM-level debugging — step by step

1. Open DevTools → **Elements** tab.
2. Click the element-picker icon (top-left, arrow-in-box) then click any element on the page to jump straight to its HTML.
3. In Console, the last-inspected element is always available as `$0` — type `$0` to reference it directly in expressions.
4. Right-click any DOM node in the Elements tree → **Break on** → choose **Subtree modifications**, **Attribute modifications**, or **Node removal** — execution pauses in Sources exactly when that DOM change happens, even if you don't know which script caused it.
5. Use the **Styles** and **Computed** panels alongside this to check whether a bug is visual/CSS rather than logic-related.

### G. Cross-context debugging — step by step

1. For iframes: open DevTools on the parent page, then look for the **context/frame dropdown** near the top of the **Console** or **Sources** panel (often labeled "top") — click it and select the iframe's own document to switch context into it.
2. Alternatively, right-click directly inside the iframe's rendered content → **Inspect** — this sometimes opens a scoped DevTools session for just that frame.
3. If cross-origin isolation blocks all of the above, copy the iframe's `src` URL and open it directly in a new browser tab — you now have a full, unrestricted DevTools session for that app alone.
4. For Node/server processes: run the process with `--inspect` (e.g. `NODE_OPTIONS='--inspect' next dev`), then open `chrome://inspect` in Chrome, and click **"inspect"** under Remote Target to attach a full DevTools instance to that process.

### H. Bundle/dependency-level debugging — step by step

1. Open Console and run:
   ```js
   window.__REACT_DEVTOOLS_GLOBAL_HOOK__.renderers.size
   ```
   A result greater than `1` confirms multiple React copies are active.
2. Check `node_modules/react/package.json` versions across each app/remote if you have filesystem access, to spot mismatches.
3. Run a bundle analyzer to see what's actually inside a given build:
   ```bash
   npx webpack-bundle-analyzer stats.json
   ```
4. Look at the visual treemap output — duplicate large blocks labeled `react`/`react-dom` appearing more than once confirms deduplication failed.
5. For Module Federation specifically, open the relevant `webpack.config.js` and check the `shared` section for `singleton: true` on `react`/`react-dom` — this is the actual fix, not just a diagnostic.

### I. Server-side debugging — step by step

1. Start your server with the inspector flag:
   ```bash
   NODE_OPTIONS='--inspect' next dev
   ```
2. Open Chrome and navigate to `chrome://inspect`.
3. Under **Remote Target**, find your Node process and click **inspect** — a dedicated DevTools window opens, attached to the server, not the browser tab.
4. Set breakpoints directly in your server component / API route files exactly like Type A — they'll pause real server execution.
5. Remember: anything logged via `console.log()` inside server-only code prints to your **terminal**, not the browser console — check there first if logs seem to be "missing."

### J. Production/remote debugging — step by step

1. As part of your CI/CD build step, generate source maps but do **not** deploy the `.map` files publicly:
   ```js
   // next.config.js
   productionBrowserSourceMaps: true
   ```
2. Upload the generated `.map` files privately to your error-tracking service (e.g. Sentry) via their CLI/build plugin as a release step.
3. When a real user hits an error, the tracker automatically resolves the minified stack trace back to your original file/line using the privately-stored map — without ever exposing the map publicly on your CDN.
4. Review the resolved stack trace and breadcrumbs (recent user actions, network calls) in the tracker's dashboard to reproduce the issue locally, then switch to Type A/C for the actual fix.

**[⬆ Back to navigation](#navigation)**

---

## 3. Scenario 1: Plain Vite/CRA Dev

**Row reference:** Source maps work, DevTools work — the happy path.

**Step-by-step:**

1. Run `npm run dev`.
2. Open DevTools → **Sources** tab — your real `src/` folder structure is already there.
3. Click any line number to set a real breakpoint directly on your original code (Type A).
4. Open the **Components** tab (React DevTools extension) — the tree renders instantly, since only one renderer registers.
5. No special technique needed. If something feels off here, the fault is almost always local project config, not architecture — check for an accidentally-set `GENERATE_SOURCEMAP=false` in `.env`.

**[⬆ Back to navigation](#navigation)**

---

## 4. Scenario 2: Production Build (No Sourcemap Flag)

**Row reference:** Source maps → minified blob. DevTools → sometimes works but names are minified.

**Step-by-step:**

1. **Don't debug the live minified blob first.** Reproduce locally instead:
   ```bash
   # Vite
   # vite.config.js -> build: { sourcemap: true }
   npm run build && npm run preview
   ```
   ```bash
   # Next.js
   # next.config.js -> productionBrowserSourceMaps: true
   npm run build && npm run start
   ```
2. If you truly must debug the *live* deployed bundle: open DevTools → **Sources** → find the minified file → click the **{}** "pretty print" icon at the bottom of the code pane. This reformats the single-line blob into readable indentation (Type B).
3. Use **Chrome DevTools → Sources → Overrides**: right-click the pretty-printed file → "Save for overrides" → now you can insert a `debugger;` statement directly into the served file, and Chrome will re-inject your edit on every reload, without touching the real deployed code (Type C, made durable).
4. For the long-term fix: senior teams upload source maps **privately** to an error tracker (Sentry, Datadog, Bugsnag) as part of the CI/CD pipeline, so crash reports resolve to real file/line info — without ever exposing the `.map` files publicly on the CDN (Type J).

**[⬆ Back to navigation](#navigation)**

---

## 5. Scenario 3: devtool false / GENERATE_SOURCEMAP=false

**Row reference:** Source maps off. React DevTools tree still works fine (independent systems).

**Step-by-step:**

1. Confirm this is *intentional* — check with the team or search the repo for the setting before assuming it's a bug:
   ```bash
   grep -r "GENERATE_SOURCEMAP" .
   grep -r "devtool" webpack.config.js next.config.js
   ```
2. If you have permission and it's just blocking your own local work, temporarily override it in your own env file or branch — never change a shared/committed config just to debug.
3. Since React DevTools still works here, lean on Type D fully: open **Components** tab, click any component, inspect live props/state/hooks in the right-side panel — you don't need source maps to see current values.
4. Combine with Type C: insert `console.log(state)`/`console.table(props)` directly at the point of interest in your source file, since that survives the build regardless of map settings.
5. Revert any temporary local override before committing.

**[⬆ Back to navigation](#navigation)**

---

## 6. Scenario 4: Cross-Origin Iframe

**Row reference:** Both source maps and DevTools affected — isolated `window`, extension can't reach in.

**Step-by-step:**

1. First, confirm it actually is an iframe and whether it's cross-origin:
   - DevTools → **Elements** tab → look for `<iframe src="...">`.
   - Compare the iframe's `src` domain to the top-level page's domain.
2. Use **Type G (cross-context debugging)**: in Chrome, the **Sources** and **Console** panels both have a **context/frame dropdown** near the top (often labeled "top" by default) — switch it to the iframe's own document to inspect that frame specifically.
3. Alternative: right-click *inside* the iframe's visible content → "Inspect" — Chrome sometimes opens a scoped DevTools session for just that frame.
4. If the iframe simply won't cooperate (common with strict cross-origin isolation), open the iframe's **own URL directly in a new tab**. This sidesteps the isolation entirely — you're now debugging that app standalone, with clean source maps and a working DevTools tree, because there's no longer a parent/child boundary.
5. For network-level issues (a common iframe bug source — auth cookies, CORS failures), the parent page's **Network tab (Type E)** still shows requests made by the iframe, so you don't need to switch context for pure request/response debugging.

**[⬆ Back to navigation](#navigation)**

---

## 7. Scenario 5: Next.js Server Components

**Row reference:** Source maps partially work. DevTools blind to these parts entirely — no client-side React execution for them.

**Step-by-step:**

1. **Mental shift first:** Server Components never run in the browser. Browser DevTools (Sources, Components tab) are the wrong tool for this code entirely — skip straight to server-side debugging.
2. Attach Node's inspector to your dev server:
   ```bash
   NODE_OPTIONS='--inspect' next dev
   ```
3. Open `chrome://inspect` in Chrome → click **"inspect"** under Remote Target for your Node process. This opens a full DevTools instance attached to the *server* process, with real breakpoints on your server component code (Type I).
4. Remember: `console.log()` inside a Server Component prints to your **terminal**, not the browser console — check your terminal output, not DevTools, for these logs.
5. For the Client Component portions of the same page (`'use client'` files), switch back to normal browser DevTools — Sources and Components tab work normally for those, since they do execute in the browser.
6. If you're unsure which parts of a file are server vs client, check for the `'use client'` directive at the top of the file — its absence means server-only by default in the App Router.

**[⬆ Back to navigation](#navigation)**

---

## 8. Scenario 6: App Disables the DevTools Hook

**Row reference:** Source maps unaffected. DevTools tree gone — deliberate enterprise security choice.

**Step-by-step:**

1. Verify this is actually what's happening:
   ```js
   window.__REACT_DEVTOOLS_GLOBAL_HOOK__.isDisabled
   // true -> confirmed
   ```
2. Since source maps still work here, fall back fully to Type A/B/C — set real breakpoints in your original files (Sources tab still shows your structure), and use `console.log(state)` at key points to track values manually, since you can't inspect props/state live via the Components tab.
3. If you have a local dev branch, this disabling code is usually gated by an environment check (`process.env.NODE_ENV === 'production'`) — running locally in dev mode will typically leave the hook enabled, giving you the Components tab back for local investigation, matching production logic otherwise.
4. Don't try to force-enable the hook in a live production/staging environment you don't control — this was disabled intentionally to prevent sensitive prop/state data from being exposed to anyone with the extension installed.

**[⬆ Back to navigation](#navigation)**

---

## 9. Module Federation — What It Is & Why It Matters

**Background needed before Scenario 7 and 8 below.**

Module Federation is a **Webpack 5 feature** (also available for Vite, Rspack) that lets independently built and deployed apps share code with each other **at runtime**, instead of one single build containing everything.

- The **host** (also called **shell** or **container**) — the main app that loads the others.
- One or more **remotes** — independently built micro-apps, each deployed on its own URL, loaded on demand by the host.

Example: a shell loads `header` from `team-a.company.com` and `checkout` from `team-b.company.com`, stitched into one page — but each was built by a different team, possibly with different dependency versions.

**Why it breaks both debugging systems:**

- **Source maps:** each remote's `.map` files live on that remote's own server. If that server doesn't serve them (common), or serves them cross-origin in a way the browser won't fetch, you get a patchwork — some parts of the app map cleanly, others don't.
- **React DevTools:** if the shared React isn't configured as a **singleton**, each remote ships its *own* copy of React, and each registers its own renderer with the hook — this is exactly what produces `renderers.size > 1`. Fragmented tree, and sometimes context/hooks breaking across remote boundaries.

**[⬆ Back to navigation](#navigation)**

---

## 10. Scenario 7: Module Federation (Misconfigured)

**Row reference:** Source maps broken for remotes specifically. DevTools partial — multiple React copies.

**Step-by-step:**

1. Confirm multiple renderers via the console — this is your fastest diagnostic (Type H):
   ```js
   window.__REACT_DEVTOOLS_GLOBAL_HOOK__.renderers.size
   // > 1 confirms duplicate React copies
   ```
2. Open the federation config (usually `webpack.config.js` under `ModuleFederationPlugin`) and check the `shared` section:
   ```js
   shared: {
     react: { singleton: true, requiredVersion: '^18.0.0' },
     'react-dom': { singleton: true, requiredVersion: '^18.0.0' }
   }
   ```
   If `singleton: true` is missing, **this is the root cause, not just a debugging inconvenience** — fixing this config is the actual resolution.
3. For source maps: check each remote's deployed map URL directly, e.g.:
   ```
   https://team-a.company.com/static/js/remoteEntry.js.map
   ```
   A 404 here means that remote simply isn't shipping maps — a build-config fix that belongs to the owning team, not something fixable from the host side.
4. Run a **bundle analyzer** on each remote independently to confirm which shared dependencies actually got deduplicated vs. duplicated:
   ```bash
   npx webpack-bundle-analyzer stats.json
   ```
5. As a workaround while the config fix is pending, debug each remote **standalone** — most Module Federation setups support running a single remote in isolation (its own dev server, its own port) — this gives you one clean React instance to inspect via Components tab, without the shell's noise.

**[⬆ Back to navigation](#navigation)**

---

## 11. Scenario 8: Micro-Frontends (Iframe or MF) Generally

**Row reference:** Both source maps and DevTools often affected — a combination of the iframe and Module Federation rows above.

**Step-by-step:**

1. **Identify which micro-frontend owns the buggy component first**, before choosing a debugging technique:
   - Use React DevTools' search/filter within the Components tab (if any tree shows at all), or
   - Inspect the DOM in Elements tab to see which section/zone of the page the element sits in.
2. Once identified, determine *which* micro-frontend implementation is in play — iframe-based or Module Federation-based — since the fix differs:
   - Iframe → apply the cross-context technique from [Scenario 4](#6-scenario-4-cross-origin-iframe).
   - Module Federation → apply the singleton/renderer check from [Scenario 7](#10-scenario-7-module-federation-misconfigured).
3. **Isolate and run that one micro-frontend standalone**, outside the shell, in its own dev server. This is the single highest-leverage senior technique here — debugging one clean React app is always easier than debugging it wedged inside a shell alongside two or three other active renderers.
4. Once isolated, you're effectively back in [Scenario 1](#3-scenario-1-plain-vitecra-dev) conditions — clean source maps, one renderer, ordinary breakpoints all work again.
5. Only return to the composed/full-shell environment to verify the fix actually resolves the issue in context — don't do your primary investigation there.

**[⬆ Back to navigation](#navigation)**

---

## 12. The Senior Dev's Default Workflow

Regardless of which scenario you land in, this is the general order of operations an experienced developer follows, every time:

1. **Reproduce with the least noise possible** — isolate the component/micro-frontend standalone if you can, rather than debugging inside the full composed app.
2. **Confirm client vs. server** — is this code even running in the browser? (Especially relevant with Next.js Server Components.)
3. **Run the two "free" diagnostics first**, before touching any breakpoints — they take seconds and immediately tell you which category of problem you're in:
   ```js
   window.__REACT_DEVTOOLS_GLOBAL_HOOK__.renderers.size
   ```
   …and a glance at the **Network** tab.
4. **Use `debugger;`/logs before fighting source maps.** Maps are a convenience, not a requirement — don't waste time trying to fix map configuration mid-investigation unless it's clearly the actual root cause.
5. **Fix root cause, not the symptom.** For Module Federation, the fix is the `shared` config — not "how do I read three React trees at once."
6. **Reserve full DevTools/source-map fidelity for local dev only.** For staging/production incidents, rely on privately-uploaded source maps to an error tracker rather than exposing public production maps just to debug.

**[⬆ Back to navigation](#navigation)**

---

## 13. Quick-Reference Command Cheatsheet

```js
// Check if the DevTools hook exists at all
window.__REACT_DEVTOOLS_GLOBAL_HOOK__

// Check if the app deliberately disabled reporting
window.__REACT_DEVTOOLS_GLOBAL_HOOK__.isDisabled

// Check how many separate React copies are registered
window.__REACT_DEVTOOLS_GLOBAL_HOOK__.renderers.size

// Find React's internal Fiber key on a selected DOM node ($0)
Object.keys($0).find(k => k.startsWith('__reactFiber'))
```

```bash
# Vite: build with source maps for local production testing
# vite.config.js -> build: { sourcemap: true }
npm run build && npm run preview

# Next.js: enable production client source maps
# next.config.js -> productionBrowserSourceMaps: true
npm run build && npm run start

# Next.js: attach Node inspector for Server Component debugging
NODE_OPTIONS='--inspect' next dev
# then open chrome://inspect

# Check if a Webpack config disables source maps
grep -r "devtool" webpack.config.js

# Check if CRA disables source maps via env
grep -r "GENERATE_SOURCEMAP" .

# Check if a Module Federation remote is serving its source map
curl -I https://remote-url/static/js/remoteEntry.js.map
```

**[⬆ Back to navigation](#navigation)**
