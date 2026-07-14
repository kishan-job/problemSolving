# React Server Components, SSR, SSG & Rendering Scenarios — Complete Notes

## 1. React 18 vs React 19 — Key Differences

**Release timeline:**
- React 19 stable — December 5, 2024
- Next.js 15 stable — October 2024
- Next.js 16 — October 2025 (current major)

| Feature | React 18 | React 19 |
|---|---|---|
| Concurrent rendering | Introduced | Made more stable |
| Compiler | None — manual `useMemo`/`useCallback` | **React Compiler** auto-handles memoization |
| Async form handling | Manual (`useState` + `useEffect`) | **Actions API** — auto pending/error/optimistic |
| Read async data in render | Not supported | New **`use`** hook — can read promises, callable conditionally |
| Document metadata | Needed React Helmet | Built into components natively |
| Server Components | Experimental/canary only | Fully stable |

---

## 2. Server Components

### Core properties
- **Never ship JavaScript** to the browser
- Cannot use `useState`, `useEffect`, event handlers
- Cannot update themselves after page loads
- **Can** fetch data from DBs, differ per request/user, run at build OR request time

### "Static" ≠ Server Component

| | Static HTML | Server Component |
|---|---|---|
| Fetch fresh DB data per request? | No | Yes |
| Differ per user? | No | Yes |
| Ships JS to browser? | No | No |
| Change after page loads? | No | No |

**Key:** Server Components = *dynamic on server, frozen once in browser*.

### Do you need Next.js?
**Yes, for real apps.** React defines the spec; Next.js provides the server runtime, bundler, protocol, and router. Next.js has supported RSC since Next.js 13 (Oct 2022) — ~2 years before React 19 stabilized them.

---

## 3. Client Components
- Marked with `'use client'`
- Ship JS to browser
- Full interactivity (`useState`, `useEffect`, event handlers)

---

## 4. SSG vs SSR vs CSR

### CSR (Client-Side Rendering)
- Server sends empty shell + JS bundle
- Browser builds everything from scratch
- Example: Create React App, plain Vite + React
- **Slow first view** (blank screen until JS runs)

### SSG (Static Site Generation)
- HTML generated **once at build time**
- Served identically to every user
- Fastest first view, cheapest per request
- Data can go stale
- Best for: blogs, docs, marketing

### SSR (Server-Side Rendering)
- HTML generated **fresh on every request**
- Always up-to-date, can be personalized
- Slower + more expensive per request
- Best for: dashboards, live data, personalized pages

### Critical: All three can be interactive
- SSG/SSR only describe *when HTML is built*, NOT whether interactivity exists
- Blog (SSG) can still have like buttons/comments — via Client Components

### Independent axes
```
CSR vs SSR/SSG      → rendering strategy (where/when HTML built)
React 18 vs 19      → feature set
Server vs Client    → does this component ship JS?
SSG vs SSR          → built once, or per-request?
```

---

## 5. How Next.js Determines SSG vs SSR (App Router)

Inspects your data-fetching code:

| Code pattern | Result |
|---|---|
| No fetching / default cache | **SSG** |
| `fetch(url, { cache: 'force-cache' })` | **SSG** |
| `fetch(url, { cache: 'no-store' })` | **SSR** |
| `fetch(url, { next: { revalidate: 60 } })` | **ISR** (hybrid) |
| Uses `cookies()`, `headers()`, search params | **SSR** (forced) |
| Dynamic route without `generateStaticParams` | **SSR** |

**Important:** Classified **per-route** — one dynamic API call forces the whole route to SSR (unless using PPR).

### ISR
- `revalidate: 60` → build as SSG, auto-rebuild every 60s in background
- Middle ground between SSG speed and SSR freshness

---

## 6. Partial Prerendering (PPR)

Solves "header/footer static + body dynamic on same page."

### Status
- **Stable in Next.js 16 (Oct 2025)** via Cache Components (`cacheComponents: true`)
- Was experimental in Next.js 14/15

### How it works
- Outside `<Suspense>` → static shell, CDN-cached
- Inside `<Suspense>` → dynamic hole, streamed at request time
- User sees layout instantly, dynamic content fills in progressively

### Key advantage
- `cookies()`/`headers()` inside a Suspense boundary no longer force whole route to SSR
- Outer shell stays statically cached even if children read cookies

### When to use PPR
- Product pages (static info + dynamic price/cart)
- Blog + personalized recommendations
- Long-form content with 1-2 personalized widgets
- NOT: fully personalized dashboards (< 30% static)
- NOT: pages where user might act on stale content before dynamic loads

---

## 7. The Three Rendering Scenarios

### Scenario 1: Direct URL Visit / Full Page Load

**Trigger:** First visit, refresh, typed URL, new tab.

**Flow:**
1. Server pre-renders whole page (Server + Client Components initial markup) → complete HTML
2. Server embeds **serialized tree data** in `<script>` tags in that same HTML
3. Browser receives HTML → paints visible content immediately
4. Browser downloads JS bundle referenced in the HTML
5. React reads embedded serialized data → builds VDOM in memory
6. React matches VDOM to already-painted DOM → attaches event handlers (**hydration**)
7. Page fully interactive

**Key:** React builds VDOM from embedded script data, NOT by parsing visible HTML.

### Scenario 2: In-App Link Navigation

**Trigger:** Clicking a `<Link>` inside the app.

**Flow:**
1. React (already running) intercepts click — no full reload
2. Browser fetches **RSC payload only** (data, not HTML)
3. Payload = Server Component output + Client Component placeholders (name + props)
4. React reads payload → builds new VDOM (runs Client Component code where needed)
5. React reconciles new tree against current DOM
6. React patches real DOM directly — only what changed
7. New Client Component JS downloaded only if not cached

**Key:** No HTML sent, no re-hydration, no full re-paint.

### Scenario 3: Plain React CSR

**Trigger:** Any React app without SSR/SSG framework.

**Flow:**
1. Server sends empty HTML shell + script tag
2. Browser paints blank page
3. Browser downloads JS bundle
4. React runs `ReactDOM.createRoot().render(<App />)`
5. React builds VDOM from scratch
6. React creates all DOM nodes from scratch
7. Browser paints newly-created DOM
8. Visible + interactive simultaneously

### Comparison

| | Full Load (S1) | Link Nav (S2) | Plain CSR (S3) |
|---|---|---|---|
| Server sends | HTML + embedded data + JS | RSC payload only | Empty shell + JS |
| First paint | Instant (real content) | Fast (patched in place) | Slow (blank first) |
| React creates DOM? | No — reuses existing | Only what changed | Yes — everything |
| VDOM built from | Embedded script data | RSC payload | Executing code |
| Full page reload? | Yes | No | Yes (first visit) |

---

## 8. RSC Payload Deep Dive

Serialized React element tree sent for Server Component content.

| Content | Server Component | Client Component |
|---|---|---|
| Rendered output/tree | Fully expanded | Not included |
| Component reference | N/A | e.g. `CartButton@chunk_cart.js` |
| Props | Baked in output | Serializable only |
| Event handlers, hooks | Never — no JS | In JS bundle, not payload |

### Example structure

```json
[
  { "type": "div", "props": { "className": "product-page", "children": [
      { "type": "h1", "props": { "children": "iPhone 15" } },
      { "type": "p", "props": { "children": "$999" } },
      { "$ref": "CartButton@chunk_cart.js", "props": {
          "productId": 123,
          "price": 999
      }}
  ]}}
]
```

**Both entries describe what to render** — Server ones directly renderable; Client ones need JS bundle to expand.

### Can't serialize
- Function references (inline event handlers)
- Class instances, Dates, DOM nodes
- Non-JSON-safe values

### Exception: Server Actions
- Server functions passed as props to Client Components (`<Form action={updateUser}>`)
- Serialized as ID reference
- Client can "call" it → triggers network request to server

---

## 9. Serialized Data in HTML (Scenario 1)

The `<script>` tag holds the **RSC payload embedded inline** — not JS code.

### Two separate pieces
- **Serialized data (in `<script>`)** — component tree, props, Server Component output, Client Component placeholders — pure JSON
- **JS bundle (separate `.js` file)** — actual Client Component code, hooks, event handlers

Both required. Data alone → no interactivity. Code alone → doesn't know what to render.

### Analogy
- Server Component output = photo of finished dish, plate directly
- Client Component reference = order slip ("make CartButton with productId=123")
- JS bundle = the kitchen (recipes to cook orders on demand)

---

## 10. Key Mental Models

1. **Server Components** = dynamic on server, frozen in browser, no JS
2. **SSG/SSR** = *when HTML is built*, NOT whether interactivity exists
3. **Interactivity** comes from Client Components — independent from SSG/SSR
4. **RSC payload** = React element tree; Server parts expanded, Client parts as ref+props
5. **Hydration ≠ re-rendering** — attaches handlers to existing DOM, doesn't recreate
6. **Full URL load**: HTML + embedded payload + JS delivered together
7. **Link nav**: RSC payload only, DOM patched in place
8. **PPR**: static shell + dynamic Suspense holes on same page (Next.js 16+)

---

## 11. Quick Reference

| Use Case | Choice |
|---|---|
| Marketing/docs/blog | SSG |
| Personalized dashboard | SSR |
| E-commerce product page | PPR |
| Content updates hourly | ISR |
| Interactive widget | Client Component |
| Data-fetching layout, non-interactive | Server Component |
| Fast first paint + interactivity | Next.js (SSR/SSG + Client Components) |
| Simple internal tool, no SEO | Plain React CSR (Vite) |