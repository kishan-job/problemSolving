# Micro-Frontends — Complete Guide Across Eras

A structured reference covering micro-frontend architectures from 2016 through 2026, including Webpack Module Federation, Vite implementations, and Next.js patterns.

---

## 1. Overview — The Three Eras of Micro-Frontends

| Era | Time | Dominant approach | Rendering |
|---|---|---|---|
| Era 1 | 2016-2019 | iframes, Single-SPA, ESI | CSR |
| Era 2 | 2020-2023 | Webpack Module Federation | CSR |
| Era 3 | 2024-2026 | Next.js Multi-Zones + Turborepo | SSR + Server Components |

### Core mindset shift over time

- **Era 1-2:** Micro-frontends meant *"multiple apps sharing components at runtime in the browser"*
- **Era 3:** Micro-frontends mean *"multiple Next.js apps composed via routing/URL paths, each fully independent"*

---

## 2. Era 1 (2016-2019) — Primitive Approaches

### 2.1 iframes
- Each micro-frontend a separate app deployed independently
- Composed via `<iframe>` tags in a container page
- **Problems:** poor UX, no shared state, styling isolation issues, SEO nightmare
- Considered a "last resort" even at the time

### 2.2 Single-SPA
- JavaScript framework letting multiple SPAs coexist on one page
- Container app registered micro-apps and controlled their lifecycle
- Framework-agnostic — React, Angular, Vue could mix
- **Popular for:** enterprises with legacy Angular apps adding React incrementally

### 2.3 Server-side composition (Edge Side Includes)
- CDN/nginx assembled the page by fetching HTML fragments from different services
- Each micro-frontend rendered its own HTML on its own server
- Combined at CDN level via ESI tags
- **Used by:** IKEA, Zalando (pioneers)
- **Problems:** heavy infrastructure, hard to add cross-boundary interactivity

### 2.4 Build-time integration
- Each team published as npm package; container installed and bundled at build
- **Problems:** every update required rebuilding container — defeats the point

---

## 3. Era 2 (2020-2023) — Webpack Module Federation

The dominant approach in the CRA/Webpack world.

### 3.1 The core idea

**A big website split into multiple small React apps, each owned by a different team, each deployed separately, appearing to the user as ONE seamless website.**

Analogy: A shopping mall
- The mall itself (walls, floors, common areas) = **shell/container app**
- Each store (H&M, Zara, Starbucks) = **micro-frontend**
- Different companies own each store, different schedules, different decor
- Customer walks through one connected space

### 3.2 Setup structure

```
Company GitHub:
├── shell-app          ← the container (Platform team)
├── checkout-app       ← micro-frontend (Checkout team)
├── product-list-app   ← micro-frontend (Search team)
├── account-app        ← micro-frontend (Account team)
└── cart-app           ← micro-frontend (Cart team)
```

**5 separate repos, 5 separate teams, 5 separate deploy pipelines.**

### 3.3 How Module Federation worked

- Introduced in Webpack 5 (October 2020) by Zack Jackson
- Each micro-frontend built as a Webpack bundle exposing specific modules
- Container app dynamically loaded remote modules at **runtime**
- Deployed independently — no rebuild of container needed
- Shared dependencies (React, Redux) automatically deduplicated

### 3.4 Config example — remote (exposing) app

```javascript
// checkout webpack.config.js
new ModuleFederationPlugin({
  name: 'checkout',
  exposes: {
    './CheckoutButton': './src/CheckoutButton',
    './CheckoutPage': './src/CheckoutPage',
  },
  shared: {
    react: { singleton: true, requiredVersion: '18.2.0' },
    'react-dom': { singleton: true, requiredVersion: '18.2.0' },
  },
});
```

### 3.5 Config example — host (consuming) app

```javascript
// shell webpack.config.js
new ModuleFederationPlugin({
  name: 'shell',
  remotes: {
    checkout: 'checkout@https://checkout.example.com/remoteEntry.js',
    productList: 'productList@https://products.example.com/remoteEntry.js',
  },
  shared: {
    react: { singleton: true, requiredVersion: '18.2.0' },
    'react-dom': { singleton: true, requiredVersion: '18.2.0' },
  },
});
```

### 3.6 Using in the shell

```jsx
const CheckoutPage = React.lazy(() => import('checkout/CheckoutPage'));

function App() {
  return (
    <div>
      <Header />
      <CheckoutPage />  {/* Loaded live from checkout.example.com */}
    </div>
  );
}
```

**User browses `example.com`, but checkout code fetched at runtime from `checkout.example.com`.** They don't see the difference.

### 3.7 Communication between micro-frontends

| Pattern | How it worked |
|---|---|
| Custom events | `window.dispatchEvent(new CustomEvent('cart:updated'))` |
| Shared Redux store | Container held Redux, all micro-apps read/wrote |
| URL as state | Filters/routing in URL, all apps read from it |
| PubSub library | Simple message bus, apps subscribe to topics |
| Shared context via container | Container exposed context all remotes consumed |

### 3.8 Companies famous for this pattern
- IKEA (pioneered, wrote influential blog posts)
- Zalando (Project Mosaic)
- DAZN (streaming, team-per-domain)
- Spotify (internal tools)
- American Express (Membership Rewards platform)
- DHL (logistics dashboards)

### 3.9 Downsides of Module Federation
- Complex Webpack configs, hard to debug
- Version-mismatch issues between shared dependencies
- Bundle bloat (duplicate small utilities)
- No SSR support — CSR only
- Team coordination overhead

---

## 4. React Sharing in Module Federation — How It Actually Works

### 4.1 The build-time reality

Each micro-frontend is built independently and **does include React in its bundle** (marked as shared, version 18.2.0):

```
shell/dist/       → contains React 18.2 (marked as shared)
checkout/dist/    → contains React 18.2 (marked as shared)
cart/dist/        → contains React 18.2 (marked as shared)
```

Every team's build has its own copy on disk. Each build is standalone and can run alone in isolation.

### 4.2 The runtime negotiation

When the user visits the site:

1. **Browser loads shell app** → shell downloads its `remoteEntry.js`
2. `remoteEntry.js` **registers** what shell offers + what version of React it carries
3. **User navigates to checkout** → browser fetches `checkout.example.com/remoteEntry.js`
4. Checkout's `remoteEntry.js` also registers: *"I have React 18.2 available"*
5. Module Federation's runtime sees: *"Shell already loaded React 18.2. Checkout needs 18.2. Skip loading checkout's copy — reuse shell's"*
6. Checkout's actual code runs using shell's React instance

### 4.3 What actually gets downloaded

User visits shell, then clicks checkout, then clicks cart:

| Step | What downloads to the browser |
|---|---|
| Visit shell | shell's `remoteEntry.js` + shell's React + shell's code |
| Click checkout | checkout's `remoteEntry.js` + checkout's code ONLY (no React — reuses shell's) |
| Click cart | cart's `remoteEntry.js` + cart's code ONLY (no React — reuses shell's) |

**React downloaded ONCE.** Everything else is per-app code.

### 4.4 Version-based negotiation

If shell has React 18.2 but checkout was built with React 17:
- Module Federation sees: *"React 18.2 loaded. Checkout requires 17. Incompatible."*
- **Fallback:** checkout's own React 17 gets downloaded and used just for checkout
- Now TWO React instances running simultaneously → likely breaks

**Teams had strict rules: every micro-frontend MUST use the same React version.**

### 4.5 Why apps still bundle React locally

Each micro-frontend needs to run **standalone** for:
- Local development (`npm start` on port 3001)
- Testing in isolation
- Being used as a standalone site if needed

Without local React, the app wouldn't work outside the shell. Module Federation's magic: *"bundle React locally for standalone use, but skip loading it at runtime if shell already provided it."*

---

## 5. React as a Separate File — Bundling Difference

### 5.1 Normal (non-Module-Federation) build

Webpack bundles everything into one/few big JS files:

```
app/dist/
├── main.js       (~500KB — React + ReactDOM + app code + all libraries)
├── vendor.js     (sometimes split — React and 3rd party libs)
└── index.html
```

React is **baked into these files** — inseparable. Transpiled, minified, mixed with everything else.

### 5.2 Module Federation build — React is a separate chunk

```
checkout/dist/
├── remoteEntry.js         (~5KB — the manifest)
├── main.js                (~100KB — app code, WITHOUT React)
├── react.[hash].js        (~40KB — React, as its OWN separate chunk)
├── react-dom.[hash].js    (~130KB — ReactDOM, as its OWN separate chunk)
├── redux.[hash].js        (~20KB — Redux, if shared)
└── other component chunks
```

**React and shared libraries are now their OWN separate files**, not baked into main bundle.

### 5.3 Why the separation is required

Webpack must be able to make this runtime decision:
- *"Should I load `react.[hash].js` from this app?"*
- *"Or reuse React already loaded from another app?"*

For that to be **skippable**, React must be a **separate file** — you can't skip loading something baked into a file you're already downloading.

Analogy:
- **Normal bundling** = sealed lunchbox with sandwich, drink, dessert all packed together — can't share the drink separately
- **Module Federation bundling** = lunchbox with everything in **separate containers** — grab only what you need, offer your drink to someone else

### 5.4 What triggers the separation

The `shared` config tells Webpack: *"treat this dependency as separately loadable, not baked in."*

```javascript
shared: {
  react: { singleton: true, requiredVersion: '18.2.0' },
  'react-dom': { singleton: true, requiredVersion: '18.2.0' },
}
```

Webpack automatically:
1. Extracts React from main bundle into its own chunk file
2. Adds runtime logic checking *"is React already loaded?"* before downloading
3. Only downloads `react.[hash].js` if no compatible version is active

---

## 6. Module Federation in Vite

**Yes, Module Federation works in Vite too — but through a community plugin, not built-in.**

### 6.1 The plugin

`@originjs/vite-plugin-federation`
- Community-maintained, not from Vite core team
- Emerged around 2021-2022 as Vite adoption grew
- API very similar to Webpack's `ModuleFederationPlugin`

### 6.2 Remote config in Vite

```javascript
// checkout-app/vite.config.js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import federation from '@originjs/vite-plugin-federation';

export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'checkout',
      filename: 'remoteEntry.js',
      exposes: {
        './CheckoutPage': './src/CheckoutPage.jsx',
        './CheckoutButton': './src/CheckoutButton.jsx',
      },
      shared: ['react', 'react-dom'],
    }),
  ],
  build: {
    target: 'esnext',
    minify: false,
    cssCodeSplit: false,
  },
});
```

### 6.3 Host config in Vite

```javascript
// shell-app/vite.config.js
export default defineConfig({
  plugins: [
    react(),
    federation({
      name: 'shell',
      remotes: {
        checkout: 'https://checkout.example.com/assets/remoteEntry.js',
        products: 'https://products.example.com/assets/remoteEntry.js',
      },
      shared: ['react', 'react-dom'],
    }),
  ],
});
```

### 6.4 Differences vs Webpack

| | Webpack Module Federation | Vite `vite-plugin-federation` |
|---|---|---|
| Bundler | Webpack (mature) | Rollup (prod) + esbuild (dev) |
| Support | Official Webpack feature | Community plugin |
| Runtime negotiation | Robust, battle-tested | Works, less mature |
| Dev experience | HMR works across federated apps | HMR quirks — sometimes needs restart |
| Version handling | Sophisticated (`singleton`, `strictVersion`) | Simpler, less flexible |
| Build modes | Full dev + prod support | **Production build required to test federation** |
| SSR support | Complex but possible | Limited |

### 6.5 The biggest catch with Vite

**Development mode doesn't work fully with Module Federation:**
- Vite's dev server uses native ES modules and unbundled files (that's what makes it fast)
- Module Federation needs bundled artifacts
- You must run `vite build && vite preview` to test federation locally

Workarounds:
- Build remote apps first, then preview them
- Run remote apps in preview mode during dev
- Use `@module-federation/vite` (newer alternative)

### 6.6 Alternatives to `@originjs/vite-plugin-federation`

- **`@module-federation/vite`** — newer, from official Module Federation team, better DX
- **Native ES modules + import maps** — skip Module Federation entirely, use dynamic imports directly
- **Nx monorepo** — has built-in federated Vite support

### 6.7 2026 verdict

Vite + Module Federation works but is less battle-tested than with Webpack. Most teams doing modern micro-frontends have moved to:
1. **Next.js Multi-Zones** (if they need SSR)
2. **Turborepo monorepos** (if they can accept coordinated deploys)
3. **Webpack Module Federation** (if they need true runtime independence + willing to stay on Webpack)

Vite + Module Federation is niche — for teams specifically wanting Vite's DX + true runtime micro-frontends.

---

## 7. Micro-Frontends in Next.js

The philosophy shifted significantly in the Next.js era. Three main approaches:

### 7.1 Approach 1: Next.js Multi-Zones (most common)

**Core idea:** Multiple completely separate Next.js apps, each owning a URL path prefix, composed at routing layer via `rewrites`. Appear as one site.

**Setup:**
```
├── main-app       → owns example.com/*        (Platform team)
├── checkout-app   → owns example.com/checkout/*  (Checkout team)
├── docs-app       → owns example.com/docs/*   (Docs team)
└── blog-app       → owns example.com/blog/*   (Marketing team)
```

Each is a full standalone Next.js project — own repo, own team, own deploy, own DB if needed.

**Rewrite config:**
```javascript
// main-app/next.config.js
module.exports = {
  async rewrites() {
    return [
      {
        source: '/checkout/:path*',
        destination: 'https://checkout.example.com/checkout/:path*',
      },
      {
        source: '/docs/:path*',
        destination: 'https://docs.example.com/docs/:path*',
      },
    ];
  },
};
```

**Key characteristics:**
- Each app has own Server Components, own Server Actions, own data-fetching
- No shared React across zones — each zone fully independent
- No runtime component sharing
- Shared UI/design system via shared npm package or monorepo

**Zone boundary = full page reload happens when crossing zones.**

**Real-world users:** Vercel (vercel.com main + /docs + /blog), Notion, various e-commerce platforms

### 7.2 Approach 2: Turborepo Monorepo (dominant now)

**Core idea:** One monorepo, multiple Next.js apps, shared packages. Each app deploys independently but shares code via internal packages.

**Setup:**
```
company-monorepo/
├── apps/
│   ├── web/           → main site (example.com)
│   ├── admin/         → admin panel (admin.example.com)
│   ├── docs/          → docs site (docs.example.com)
│   └── mobile-web/    → mobile-optimized site (m.example.com)
├── packages/
│   ├── ui/            → shared React components
│   ├── config/        → shared configs (ESLint, Tailwind, tsconfig)
│   ├── db/            → shared database client (Prisma)
│   ├── auth/          → shared auth logic
│   └── utils/         → shared helpers
├── turbo.json
└── package.json
```

**Example imports:**
```jsx
// apps/web/app/page.tsx
import { Button } from '@company/ui';
import { db } from '@company/db';
import { getSession } from '@company/auth';

export default async function HomePage() {
  const session = await getSession();
  const products = await db.product.findMany();
  return <div>...</div>;
}
```

**Key characteristics:**
- Independent deploys per app
- Shared code at build time via workspace packages (not runtime)
- Design system in `packages/ui` — used by all apps
- Type safety across apps
- Turborepo caches builds — only rebuilds what changed

**Real-world users:** Vercel (owns Turborepo), Shopify, Datadog, most modern SaaS

### 7.3 Approach 3: Module Federation with Next.js (rare)

Plugin: `@module-federation/nextjs-mf`

**Why rare:**
- App Router + Server Components don't fit Module Federation's runtime model well
- Server Components can't be federated at runtime (they run on server)
- Only Client Components can be shared this way
- Significant complexity for marginal benefit

**When still used:**
- Migrating existing Webpack Module Federation app to Next.js incrementally
- Very large orgs needing true runtime independence for Client Components

### 7.4 Comparison of the three

| | Multi-Zones | Turborepo Monorepo | Module Federation with Next.js |
|---|---|---|---|
| Composition | Routing layer (URL paths) | Build time (shared packages) | Runtime (dynamic imports) |
| Repo structure | Separate repos | One monorepo | Separate repos |
| Independent deploys | Yes | Yes | Yes |
| Shared code | Published npm packages | Internal workspace packages | Runtime chunks |
| Server Components | Yes, per zone | Yes, per app | Limited (Client only) |
| Cross-section nav | Full page reload | Full page reload | Client-side possible |
| Complexity | Low-medium | Low-medium | High |
| Popularity 2026 | Growing | **Dominant** | Declining |

### 7.5 What job descriptions actually mean by "micro-frontends"

If a job posting mentions "micro-frontends" and it's a Next.js role:
- **~90%** → Turborepo monorepo
- **~8%** → Multi-Zones
- **~2%** → actual Module Federation

---

## 8. Multi-Zones — URL Behavior and Authentication

### 8.1 How the URL stays under the parent app

**Mechanism:** rewrites act as a reverse proxy at the Next.js/edge level.

**Full request flow** — user on `example.com/products` clicks link to `/checkout/cart`:

1. Browser sends request to `example.com/checkout/cart`
2. Parent app's server (or Vercel's edge) receives it
3. Parent's `next.config.js` matches `/checkout/:path*` → triggers rewrite
4. Parent's server makes server-to-server HTTP request to `https://checkout.example.com/checkout/cart`
5. Checkout app on its own server does full Next.js rendering:
   - Runs its Server Components
   - Fetches its own data
   - Produces full HTML + embedded RSC payload + JS bundle references
6. Checkout app returns HTML response to parent's server
7. Parent's server forwards HTML to user's browser as-is
8. Browser receives HTML that looks like it came from `example.com/checkout/cart`
9. Browser paints, downloads referenced JS bundles from `checkout.example.com/_next/static/...`
10. Hydration happens — with checkout app's JS bundles

**Key insight:** parent's server acts as middleman. Browser unaware a second Next.js app produced the HTML.

### 8.2 Where JS bundles come from

Static assets served directly from child app's domain:
```javascript
// checkout-app/next.config.js
module.exports = {
  assetPrefix: 'https://checkout.example.com',
  basePath: '/checkout',
};
```

Browser:
- Downloads HTML from `example.com/checkout/cart` (proxied)
- Downloads JS/CSS from `checkout.example.com/_next/static/...` (direct)

### 8.3 Navigation behavior

**Within same zone (`/checkout/cart` → `/checkout/payment`):**
- Both URLs match same rewrite
- Next.js `<Link>` handles as client-side navigation
- Fetches RSC payload from checkout app (via parent's proxy)
- No full page reload

**Crossing zones (`/checkout/cart` → `/dashboard`):**
- URLs match different apps
- **Full page reload happens** — browser reloads because JS bundle boundary changes
- Brief blank/flash before new zone renders
- Known trade-off of Multi-Zones

### 8.4 Authentication across zones — three solutions

#### Solution 1: Shared cookies on parent domain (most common)

Both apps set/read cookies scoped to parent domain (`example.com`), not their own subdomains:

```javascript
res.setCookie('session_token', token, {
  domain: '.example.com',    // ← leading dot, or just 'example.com'
  path: '/',
  httpOnly: true,
  secure: true,
  sameSite: 'lax',
});
```

**Why this works:**
- User logs in on parent → cookie set with domain `.example.com`
- User navigates to `/checkout/cart` → browser sends cookie
- Parent's rewrite forwards request to `checkout.example.com` **with cookie**
- Checkout app reads cookie, knows user is authenticated

**Requirements:**
- All apps on same root domain (subdomains work)
- All apps know shared cookie format
- All apps access shared secret to verify tokens

#### Solution 2: Central auth service (enterprise pattern)

Both apps delegate auth to shared service:
```
User request → Parent/Checkout app → verify token with auth service → get user info
```

Popular options 2026:
- **NextAuth.js / Auth.js** with shared database
- **Clerk** — SaaS auth with cross-app SSO built in
- **Auth0** — enterprise standard
- **Supabase Auth** — for Supabase-backed apps
- **Custom OAuth2/OIDC server** — internal at enterprises

#### Solution 3: Shared session middleware in monorepo

If apps live in Turborepo, share auth logic as a package:
```jsx
// packages/auth/index.ts
export async function getSession() {
  const cookie = cookies().get('session_token')?.value;
  if (!cookie) return null;
  return await verifyJWT(cookie);
}

// apps/checkout/app/cart/page.tsx
import { getSession } from '@company/auth';

export default async function CartPage() {
  const user = await getSession();
  if (!user) redirect('/login');
}
```

All apps import same `getSession`, read same cookie, verify same JWT. Cleanest pattern when you own all apps.

### 8.5 Full authenticated cross-zone flow

**Setup:** User logged in via parent app. Auth cookie set on `.example.com`.

1. User is on `example.com/dashboard` (parent app)
2. User clicks link to `/checkout/cart`
3. Browser navigates → full page load (crosses zones)
4. Request goes to `example.com/checkout/cart` with auth cookie
5. Parent's server matches rewrite → forwards to `checkout.example.com/checkout/cart` **including cookie**
6. Checkout app on server:
   - Receives request with cookie
   - Extracts session token from cookie
   - Verifies JWT OR calls central auth service
   - Renders page with user context
7. HTML returned → parent forwards to browser
8. User sees cart page, already authenticated

### 8.6 Common gotchas

| Gotcha | Fix |
|---|---|
| Cookie domain mismatch (set on subdomain vs root) | Enforce `.example.com` domain in all auth cookie setters |
| CSRF protection across zones | Use `sameSite: 'lax'` + shared CSRF secret across apps |
| Session expiry differences | Standardize refresh logic via shared auth package |
| Middleware conflicts | Push auth to edge or delegate to shared auth service |

### 8.7 What most companies use in 2026

**Clerk (or Auth.js) + shared cookies on root domain + JWT verification via shared secret.**

- Clerk manages login UI and session storage
- Cookies live on root domain so all zones see them
- Each Next.js app has minimal auth code — verifies JWT, gets user info
- Shared logic in workspace package (`@company/auth`) if using monorepo

### 8.8 UX implications of zone crossings

Because zone crossings trigger full reloads, teams design user flows to minimize crossings:
- **Checkout stays in checkout zone** — all checkout steps in one zone, no reloads mid-flow
- **Marketing pages separate** — different zone from product, but users rarely bounce back and forth
- **Docs in own zone** — users go, read, follow one link back — infrequent crossings

Multi-Zones works best when zone boundaries **align with user journey phases**, not arbitrary code boundaries.

---

## 9. Module Federation vs Turborepo — Deploy Independence

**Short answer:** Turborepo does NOT give the same "unlimited independent deploys" freedom as Webpack Module Federation.

### 9.1 The core difference in one sentence

- **Module Federation:** teams deploy independently at **runtime** — changes to one app appear live in others without any redeploy of the consumer
- **Turborepo:** teams deploy independently at **build time** — changes to shared packages require consumer apps to rebuild and redeploy

### 9.2 Module Federation independence (CRA era)

Scenario: Team A owns `checkout-app`. Team B owns `shell-app` using checkout components.

1. Team A updates a button in checkout-app
2. Team A deploys to `checkout.example.com/remoteEntry.js`
3. Next user visits shell-app → browser fetches latest `remoteEntry.js` → gets Team A's new button
4. Team B did nothing — no rebuild, no redeploy, didn't even know

**True runtime independence.** Team A can deploy 50 times a day; Team B never lifts a finger.

### 9.3 Turborepo behavior

Scenario: `apps/web` and `apps/admin` both use `packages/ui`. Team A owns `packages/ui`.

1. Team A updates a button in `packages/ui`
2. Team A commits and pushes to monorepo
3. **Nothing changes in production yet** — `apps/web` and `apps/admin` already deployed with OLD button
4. To ship new button:
   - Team B rebuilds/redeploys `apps/web`
   - Or CI auto-rebuilds+redeploys all affected apps

**Build-time coupling.** Team A's change only "live" once every consumer app rebuilds.

### 9.4 Same scenario, both systems

**Team A changes shape of `<Button>` used by 3 other teams:**

Module Federation:
```
Team A updates Button → deploys new bundle at ui.example.com/remoteEntry.js
                     → all consumer apps see new Button live on next page load
Total deploys: 1 (Team A only)
Coordination needed: 0
Time to production: ~5 minutes
```

Turborepo:
```
Team A updates Button in packages/ui → commits to monorepo
                     → CI detects apps/web, apps/admin, apps/mobile all depend on packages/ui
                     → CI rebuilds all 3 apps + rebuilds packages/ui
                     → CI deploys all 3 apps + packages/ui
Total deploys: 3 apps + 1 package
Coordination needed: coordinated deploy pipeline (usually automated)
Time to production: ~15-30 minutes
```

### 9.5 What's same, what's different

**Same in both:**
- Each app deploys as its own artifact
- Each app can go down/scale independently
- Each app can have own release cadence for isolated changes
- Teams don't step on each other's code during development

**Different:**

| | Module Federation | Turborepo |
|---|---|---|
| Shared update visible without consumer redeploy? | Yes | No |
| Consumer apps rebuild when shared code changes? | No | Yes |
| Runtime component version conflicts possible? | Yes (real pain) | No |
| Instant rollback via reverting shared bundle? | Yes | Requires all-app redeploy |
| Coordination for shared code changes? | Minimal | Yes — cross-app testing |

### 9.6 Why industry chose Turborepo despite Module Federation's advantages

1. **Version coordination still needed** — everyone had to agree on React 18.2 vs 18.3
2. **Runtime failures were scary** — bad shared bundle deploy could break every consumer instantly
3. **Debugging was miserable** — errors could come from any of N deployed versions
4. **Type safety broken** — TypeScript couldn't check across runtime module boundaries
5. **SSR didn't work well** — Server Components can't be runtime-federated
6. **Testing was hard** — couldn't easily test the combination of all deployed versions

Turborepo trades runtime independence for **build-time simplicity, type safety, predictability**. Teams decided coordination cost was worth it.

### 9.7 When Module Federation's freedom is still needed in 2026

- Widget/plugin ecosystems where third parties inject components (help widgets, chat SDKs)
- Massive scale (100+ engineers) where rebuild-all-consumers is genuinely painful
- Legacy migrations where existing Module Federation is too big to change
- Multi-tenant products where customer-specific customizations need runtime loading

Small minority of teams in 2026.

### 9.8 The nuance — Turborepo IS independent for app-scoped changes

If Team A owns `apps/checkout` and Team B owns `apps/dashboard`, and they don't share packages:
- Team A pushes changes to `apps/checkout` only
- Turborepo cache detects: *"only checkout changed"*
- CI/CD deploys only `apps/checkout`
- Dashboard untouched

**This IS independent deployment** — but only for changes isolated to one app. Shared package touch requires coordinated deploys.

---

## 10. Key Mental Models

1. **Era 1 (2016-2019):** iframes and Single-SPA — early, painful, framework-agnostic focus
2. **Era 2 (2020-2023):** Webpack Module Federation — mature, runtime composition, CSR-only, industry standard
3. **Era 3 (2024-2026):** Next.js Multi-Zones + Turborepo — routing-based composition fitting Server Components
4. **Module Federation separates React into its own chunk file** — that's what enables runtime sharing
5. **Multi-Zones works via reverse proxy** — parent app forwards requests to child apps behind the scenes
6. **Auth across zones = shared cookies on root domain** — simplest and most common solution
7. **Module Federation = runtime independence; Turborepo = build-time coordination** — different trade-offs
8. **Most modern "micro-frontends" jobs actually mean Turborepo monorepo**, not true runtime federation

---

## 11. Quick Decision Reference

| Need | Choice |
|---|---|
| True runtime independence, willing to accept complexity | Webpack Module Federation |
| Same but with Vite | `@originjs/vite-plugin-federation` (limited dev experience) |
| Modern React with SSR + team autonomy | Next.js Multi-Zones |
| Multiple apps sharing code, coordinated deploys OK | Turborepo Monorepo |
| Widget/plugin ecosystem, third-party components | Module Federation |
| Small team, single product | **Don't do micro-frontends** — single app is fine |
| Enterprise scale (100+ engineers) | Multi-Zones + Turborepo hybrid (like Vercel's own setup) |