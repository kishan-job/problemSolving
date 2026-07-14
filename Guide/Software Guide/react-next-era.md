# React Development: Before vs After (CRA Era vs React 19 Era)

## The Two Eras of React Development

### Before (React 16-18 era, roughly 2018-2023)
The default way to start a React app was **Create React App (CRA)** — the officially recommended tool by the React team for years.

- Run `npx create-react-app my-app` → get a working React project
- **Pure CSR** (Client-Side Rendering) — server sent empty HTML shell, JS built everything
- No SSR, no SSG, no Server Components out of the box
- Single-page app (SPA) by default
- Slow first paint (blank screen), bad SEO
- If you wanted SSR, you had to reach for Next.js separately (seen as "extra")

### Now (React 19 era, 2024+)
The React team has **officially deprecated Create React App**. Their new guidance: **use a framework from the start.**

- CRA is dead — no longer recommended, no longer maintained
- New recommended paths:
  - **Next.js** — for anything needing SSR/SSG/Server Components (most production apps)
  - **Vite + React** — for pure client-side apps where you don't need SSR (internal tools, dashboards behind a login, learning projects)
  - **Remix / React Router v7** — an alternative to Next.js with similar capabilities

---

## Server Components Timeline — Next.js Led, React Followed

Server Components existed in Next.js **~2 years before React officially released them.** Here's how that happened:

### Phase 1: Next.js 13 introduces App Router with RSC (October 2022)
- Next.js 13 shipped the **App Router** with Server Components support
- At this point, React Server Components were still **experimental/canary** — not officially released
- Next.js was building on React's **canary channel** (unstable pre-release builds)
- Meta and Vercel worked together — the React team specifically designed a "canary for frameworks" channel so Next.js could use unfinished features in production while giving feedback to shape the final API

### Phase 2: Next.js 14 refines RSC (October 2023)
- Server Actions, streaming, and partial rendering improvements
- Still using React canary — no stable React release yet with RSC

### Phase 3: React 19 stable release (December 5, 2024)
- React officially stabilized Server Components as part of React 19
- React 19 includes all of the React Server Components features included from the Canary channel 
- What Next.js had been shipping unofficially for 2 years finally became the standard

### Phase 4: Next.js updates to stable React 19 (Next.js 15.1, December 2024)
- Next.js 15 originally shipped with React 19 RC (October 2024)
- Next.js 15.1 (December 2024) upgraded to **stable React 19** in both App Router and Pages Router
- No breaking changes for existing App Router users — the "canary" APIs Next.js relied on were already what React 19 stabilized

### Phase 5: Next.js 16 stabilizes PPR (October 2025)
- Partial Prerendering graduated from experimental to stable via Cache Components (`cacheComponents: true`)
- Continued the pattern of Next.js pushing features to stable while React itself has already stabilized the underlying primitives

### Why this ordering happened
- React defines the **spec/mechanism** (how Server Components behave, boundaries, serialization)
- Next.js provides the **implementation** (server runtime, bundler config, router, RSC payload transport)
- React can't ship RSC as a usable feature without a framework implementing all the machinery around it
- So Next.js had to build the machinery first, using React's experimental primitives, before React could declare them stable

### One-line summary of the timeline
**Next.js 13 (Oct 2022) → shipped RSC using React canary → refined through Next.js 14/15 → React 19 stable released Dec 2024 finalizing the spec → Next.js 15.1 upgraded to stable React 19 → Next.js 16 (Oct 2025) stabilized PPR.**

---

## Old Developer vs New Developer — Practical Differences

| | Old React developer (pre-2024) | New React developer (2024+) |
|---|---|---|
| Starter tool | `create-react-app` | `create-next-app` or `npm create vite@latest` |
| Default rendering | CSR only | SSR/SSG (Next.js) or CSR (Vite) — chosen deliberately |
| Server Components | Didn't exist / experimental | Standard part of the mental model |
| Data fetching | `useEffect` + `fetch` in components | Server Components with `async/await` in the component itself |
| Routing | React Router (separate library) | Built into Next.js file-system routing |
| SEO/performance | Bolted-on afterthought | Built into the framework's default behavior |
| Mental model | Everything runs in browser | Some things run on server, some on client — need to know the difference |

---

## Why This Shift Matters (For a Classic React Developer)

A skills profile of **React.js + Redux + Appwrite + REST APIs** reads as a classic pre-2024 React developer:

- Redux was the default state solution in the CRA era
- Appwrite as backend + REST APIs is a client-side pattern (all fetching happens in the browser)
- No Next.js → suggests CSR mindset

### What product companies and GCCs now screen for
- Next.js (App Router, Server Components, Server Actions)
- Understanding of the server/client boundary
- Data fetching in Server Components (not just `useEffect`)
- Modern state approaches:
  - **React Query** for server state
  - **Zustand** or **Context** for client state
  - Redux increasingly seen as legacy

---

## The Practical Upgrade Path

React fundamentals still apply — these are the additions:

1. **Learn Next.js 15/16 with App Router** — industry default for real React apps
2. **Understand the server/client component split** — the biggest new mental model
3. **Understand data fetching in Server Components** — replaces most `useEffect + fetch` patterns
4. **Try Vite** at least once — the modern no-framework alternative to CRA

---

## One-Line Summary

**Before 2024:** React = CRA + CSR + Redux + React Router, everything in the browser.

**After React 19 (late 2024+):** React = Next.js (or Vite) + Server Components + framework-native routing/data fetching, with a real server/client split.

**CRA is officially deprecated. "Just React by itself" is no longer the recommended starting point for new apps.**

**Server Components timeline:** Next.js shipped them 2 years before React officially released them (Next.js 13 in Oct 2022 → React 19 stable in Dec 2024) — Next.js built the machinery on React's canary channel, React finalized the spec, then Next.js upgraded to stable React 19.