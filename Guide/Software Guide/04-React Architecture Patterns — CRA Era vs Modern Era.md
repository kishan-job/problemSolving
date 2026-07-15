# React Architecture Patterns — CRA Era vs Modern Era

A structured reference covering how React apps are architected in companies, from the CRA era (2016-2023) through the modern React 19 / Next.js App Router era (2024-2026).

---

## 1. Overview — The Two Eras

| | CRA Era (2016-2023) | Modern Era (2024-2026) |
|---|---|---|
| Default starter | Create React App (CRA) | Next.js or Vite |
| Rendering | CSR by default | SSR/SSG + Server Components |
| Backend integration | Separate API always | Server Components can talk to DB directly |
| Data transformation | Browser (client-side) | Server (Server Components) |
| State management | Redux dominant | React Query + Zustand + Server Components |

---

## 2. Architectures in the CRA/Vite Era (2016-2023)

Before Next.js App Router and Server Components, React apps had these main architectural patterns:

### 2.1 SPA + Separate REST API (most common, ~70% of apps)
- **Frontend:** CRA (later Vite) → CSR only → deployed to Netlify/S3/CloudFront
- **Backend:** Separate Node.js/Java/Python/Go server exposing REST endpoints
- **Communication:** Browser makes fetch/axios calls to backend after page loads
- **State:** Redux (later Redux Toolkit) for everything — server data + client state mixed
- **Routing:** React Router (client-side only)
- **Auth:** JWT tokens in localStorage/cookies
- **Example:** Most SaaS dashboards, internal tools, admin panels

### 2.2 SPA + GraphQL Backend (2018+)
- **Frontend:** CRA + Apollo Client (or Relay)
- **Backend:** GraphQL server (Apollo Server, Hasura, or custom)
- **Why:** Better than REST for complex UIs with many related data pieces
- **State:** Apollo Client cache doubled as state manager (reduced Redux need)
- **Example:** Facebook (Relay), GitHub, Airbnb, Shopify

### 2.3 Next.js Pages Router with SSR/SSG (2018-2023)
- **Frontend + Backend combined** in one Next.js project
- Used `getServerSideProps` for SSR, `getStaticProps` for SSG, `getStaticPaths` for dynamic SSG
- API routes in `/pages/api/` for backend logic
- **State:** Redux + SWR/React Query for server data
- **Example:** Marketing sites needing SEO, e-commerce, content-heavy apps

### 2.4 JAMstack (Gatsby + Headless CMS) (~2018-2022)
- **Frontend:** Gatsby (SSG at build time) + GraphQL data layer
- **Content:** Headless CMS (Contentful, Sanity, WordPress-as-API)
- **Deploy:** Netlify or S3, purely static files
- **Example:** Company blogs, docs sites, marketing landing pages

### 2.5 Micro-Frontends (large enterprises, ~2019+)
- Multiple CRA/Webpack apps composed together as one product
- Used Webpack Module Federation (2020+) or single-spa (earlier)
- **Example:** IKEA, Zalando, DAZN, large enterprise portals

### 2.6 BFF (Backend for Frontend) (large enterprises, ~2017+)
- Frontend team owned a Node.js BFF layer between UI and downstream microservices
- BFF aggregated calls, shaped data, handled auth
- CRA SPA talked to BFF, BFF talked to backend microservices
- **Example:** Netflix, SoundCloud, most large orgs with microservices

### 2.7 PWA (Progressive Web App) (~2016-2019)
- CRA had `service-worker.js` template built in
- Add-to-homescreen, offline support, push notifications
- **Example:** Twitter Lite, Uber, Pinterest, Flipkart

### 2.8 Static SPA + Firebase/Serverless (~2017-2022)
- CRA frontend + Firebase (auth, Firestore, functions) as full backend
- No server to maintain — Google handled everything
- **Example:** Countless startup MVPs, indie SaaS

### CRA Era State Management Evolution

| Period | Dominant state pattern |
|---|---|
| 2016-2018 | Redux (verbose, boilerplate-heavy) |
| 2018-2020 | Redux + Redux Saga/Thunk for async |
| 2019-2021 | Context API + useReducer (Redux alternative for smaller apps) |
| 2020-2023 | Redux Toolkit (simplified Redux) + React Query/SWR (server state) |
| 2022-2023 | Zustand, Jotai (lighter alternatives to Redux) |

---

## 3. Architectures in the Modern Era (2024-2026)

The React 19 + Next.js App Router + Server Components era changed the landscape:

### 3.1 Full-Stack Next.js Monolith with RSC (dominant new-app pattern)
- One Next.js project handles everything — UI, backend logic, DB queries
- Server Components fetch data directly (`async/await` in components)
- Server Actions handle mutations (form submissions, updates, deletes)
- No separate REST API layer needed for most operations
- **Stack:** Next.js 15/16 App Router + Prisma/Drizzle ORM + Postgres/Neon/Supabase + Tailwind
- **Deploy:** Vercel, Railway, Fly.io, or self-hosted Docker
- **Example:** Most new startups, YC companies, indie SaaS, modern e-commerce

### 3.2 Next.js Frontend + Existing Backend API (dominant enterprise pattern)
- Next.js handles UI + rendering
- Server Components call an existing backend API (Java/Spring, .NET, Node, Python)
- Server Actions used for UI-triggered actions, delegate to backend
- **Stack:** Next.js + Spring Boot/Django/Node microservices + Kafka/gRPC for internal comms
- **Deploy:** AWS/Azure/GCP, often multiple services + CDN
- **Example:** Most GCCs, banks, insurance, healthcare, enterprise SaaS

### 3.3 Vite SPA + Backend API (still common for internal tools)
- Vite + React (CSR) for frontend, no SSR needed
- Separate backend service handles data
- React Query for server state, Zustand/Context for client state
- **Stack:** Vite + React + React Query + Zustand + backend (Node/Java/Python)
- **Best for:** admin panels, internal dashboards, apps behind login where SEO doesn't matter
- **Example:** Many B2B SaaS admin panels, enterprise dashboards

### 3.4 Headless Commerce/CMS + Next.js Storefront
- Next.js frontend + headless commerce (Shopify Hydrogen, Medusa) or CMS (Sanity, Contentful)
- ISR/SSG for product pages, Server Components for personalized content, Server Actions for cart
- **Stack:** Next.js + Sanity/Contentful + Shopify Storefront API + Stripe
- **Example:** Modern D2C brands, publishers, media companies

### 3.5 Edge-First Serverless with Next.js
- Next.js deployed on Vercel Edge or Cloudflare Workers
- Server Components run at edge locations near users → global low latency
- Data in edge-compatible DBs (Turso, Neon serverless, DynamoDB, Cloudflare D1)
- **Best for:** global consumer apps, high-traffic sites

### 3.6 Micro-Frontends with Next.js Multi-Zones or Turborepo
- Multiple Next.js apps composed as one product using Multi-Zones or Module Federation
- Each team owns one domain (checkout, product listing, account)
- Shared design system in a monorepo
- **Stack:** Turborepo + multiple Next.js apps + shared UI library
- **Example:** Very large e-commerce, big consumer products

### 3.7 BFF Pattern with Server Components as the BFF
- Next.js Server Components + Route Handlers act as the BFF layer
- Aggregate calls to multiple downstream microservices (gRPC, GraphQL, REST)
- UI-friendly data shaping happens in Server Components
- **Best for:** enterprises with many microservices needing UI-tailored aggregation
- **Example:** Netflix-scale, Uber-style, Airbnb-style companies

### 3.8 Astro + React Islands (rising for content-heavy sites)
- Astro for static shell, React Components as "islands" for interactivity
- Almost zero JS shipped for non-interactive parts
- **Best for:** blogs, docs, marketing sites, portfolios

### Modern Era State Management

| Type of state | Tool of choice |
|---|---|
| Server data (users, products) | **React Query / SWR** |
| Global client state | **Zustand** or **Jotai** |
| Form state | **React Hook Form** |
| URL state (filters, search) | **nuqs** or Next.js `searchParams` |
| Complex client state (rare) | **Redux Toolkit** (declining) |

---

## 4. BFF Pattern Deep Dive — Does Next.js Also Handle UI?

**Yes — in the BFF pattern with Next.js, one codebase handles both jobs simultaneously.**

### The two roles Next.js plays in BFF

| Role | What it does |
|---|---|
| **UI layer** | Renders React pages the user sees (Server + Client Components) |
| **BFF layer** | Aggregates and shapes data from multiple backend services before rendering UI |

Both jobs happen in the **same Next.js codebase**.

### Example: A Netflix-style dashboard

```jsx
// This is a Server Component acting as the BFF
async function DashboardPage() {
  // BFF role: aggregate multiple backend service calls in parallel
  const [profile, history, recommendations, continueWatching] = await Promise.all([
    fetch('http://user-service.internal/profile'),
    fetch('http://history-service.internal/watch-history'),
    fetch('http://recs-service.internal/recommended'),
    fetch('http://playback-service.internal/continue'),
  ]);
  
  // UI role: shape the data and render the UI
  return (
    <div>
      <UserHeader user={profile} />
      <ContinueWatching items={continueWatching} />
      <Recommendations shows={recommendations} />
      <WatchHistory history={history} />
    </div>
  );
}
```

**One file, one Server Component, doing both:** calling 4 microservices + rendering the UI.

### Key characteristics of BFF pattern

1. **Aggregation** — combines multiple microservice calls into one page
2. **Data shaping** — transforms backend data into UI-friendly shape
3. **Auth handling** — attaches user tokens when calling internal services
4. **UI-specific logic** — knows what UI needs, not what backend generically exposes
5. **Trusted internal network** — calls internal microservices directly, browser never sees these endpoints

### Two variations of BFF

**Variation A: Server Components as the BFF** (modern, Next.js 13+)
- No separate BFF service
- Server Components directly call microservices

**Variation B: Route Handlers as the BFF** (more traditional)
- Next.js API routes (`/app/api/*`) act as BFF endpoints
- Called by Client Components using fetch/React Query
- Used when endpoints also need to be exposed to mobile apps

### Companies using BFF pattern
- Netflix, Uber, Airbnb, SoundCloud, most large orgs with many microservices

---

## 5. If BFF Is Not There — What Architecture Is That?

**If Next.js renders UI but calls a single already-shaped API endpoint (instead of aggregating multiple raw microservices), it's Architecture #2 — Next.js Frontend + Existing Backend API.**

### The core distinction

| | Architecture #2 (Next.js + Backend) | Architecture #7 (BFF) |
|---|---|---|
| Next.js is | Thin UI layer | Thick UI + aggregation layer |
| Calls per page | 1 pre-shaped API endpoint | Multiple raw microservices |
| Who shapes data | Backend team | Frontend team via Next.js |
| Are backend endpoints designed for this UI? | Yes — pre-shaped | No — raw generic microservices |

### Architecture #2 example

```jsx
async function DashboardPage() {
  // ONE call to a pre-shaped endpoint the backend team built
  const dashboardData = await fetch('http://api.internal/dashboard');
  return <Dashboard data={dashboardData} />;
}
```

Behind that single endpoint, the backend team wrote code aggregating users, orders, recommendations, etc. Next.js just receives ready-to-render data.

### Simple rule to remember

- Next.js calls **1-2 pre-shaped APIs per page** → Architecture #2
- Next.js **stitches 4+ raw microservices per page** → BFF (Architecture #7)

Same file layout, same Server Components, same rendering — only difference is **how much aggregation work Next.js does** and **which team owns that work.**

---

## 6. Data Transformation in the CRA Era

In CRA + Backend API architecture (2016-2023), there was **no server layer to do transformation** — everything happened in the browser.

### The 5 common places transformation happened in CRA apps

#### 6.1 Inside `useEffect` right after fetch (most common early approach)
```jsx
function Dashboard() {
  const [data, setData] = useState(null);
  useEffect(() => {
    fetch('/api/user')
      .then(r => r.json())
      .then(rawData => {
        // Transformation happened HERE, in the browser
        const shaped = {
          fullName: `${rawData.first_name} ${rawData.last_name}`,
          isPremium: rawData.subscription_tier > 2,
        };
        setData(shaped);
      });
  }, []);
}
```
Transformation logic scattered across components — messy but simple.

#### 6.2 In Redux reducers/selectors (mid-era, more organized)
```jsx
// Redux action fetches raw data
const fetchUser = () => async (dispatch) => {
  const raw = await fetch('/api/user').then(r => r.json());
  dispatch({ type: 'USER_LOADED', payload: raw });
};

// Selectors do transformation lazily
const selectUserFullName = (state) => 
  `${state.user.first_name} ${state.user.last_name}`;
```
Transformation centralized in selectors — cleaner but adds boilerplate.

#### 6.3 In a dedicated service/API layer (enterprise pattern)
```jsx
// services/userService.js
export async function getUser() {
  const raw = await fetch('/api/user').then(r => r.json());
  return {
    fullName: `${raw.first_name} ${raw.last_name}`,
    isPremium: raw.subscription_tier > 2,
  };
}
```
Component gets clean data; service file owns transformation. Cleaner enterprise pattern.

#### 6.4 In Context providers (smaller apps, mid-era)
```jsx
function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  useEffect(() => {
    fetch('/api/user').then(r => r.json()).then(raw => {
      setUser({ fullName: `${raw.first_name} ${raw.last_name}` });
    });
  }, []);
  return <UserContext.Provider value={user}>{children}</UserContext.Provider>;
}
```

#### 6.5 Inside React Query / SWR select functions (late era, 2020+)
```jsx
const { data } = useQuery({
  queryKey: ['user'],
  queryFn: () => fetch('/api/user').then(r => r.json()),
  select: (raw) => ({
    fullName: `${raw.first_name} ${raw.last_name}`,
    isPremium: raw.subscription_tier > 2,
  }),
});
```
React Query cached raw response; `select` transformed on-demand per component.

### Where transformation was stored

| Storage location | When popular |
|---|---|
| Local component state (`useState`) | 2016-2019, small apps |
| Redux store | 2016-2021 (peak) |
| Context API | 2018-2022 |
| React Query / SWR cache | 2020-2023+ |
| Zustand / Jotai | 2021-2023+ |

### The fundamental problem in CRA era

**Every user's browser had to do the transformation work.**

| Problem | Impact |
|---|---|
| Duplicate work | 1M users = 1M times the same transformation ran |
| Slower first paint | Data arrived raw, then had to be shaped before rendering |
| Bigger JS bundles | Transformation logic shipped to every browser |
| Business logic exposed | Field-mapping visible in DevTools |
| No caching across users | Each user re-did the work independently |

---

## 7. Data Transformation in Next.js (Without BFF)

In Architecture #2 (Next.js + Existing Backend), **transformation happens on the server** — inside Server Components or their helper files.

### Three sub-cases within Architecture #2

#### Case A: Backend gives perfectly UI-ready data (rare)
```jsx
async function DashboardPage() {
  const data = await fetch('http://api.internal/dashboard-page');
  return <Dashboard data={data} />;
}
```
- **Transformation happens:** on the backend before Next.js receives it
- **In Next.js:** none needed
- Requires close backend-frontend collaboration + per-page endpoints

#### Case B: Backend gives generic but clean data (most common)
```jsx
async function DashboardPage({ userId }) {
  const user = await fetch(`http://api.internal/users/${userId}`);
  
  // LIGHT transformation in the Server Component
  const displayData = {
    fullName: `${user.first_name} ${user.last_name}`,
    isPremium: user.subscription_tier > 2,
    joinedAgo: calculateDaysAgo(user.created_at),
  };
  
  return <Dashboard data={displayData} />;
}
```
- **Transformation happens:** in the Server Component itself
- **Where it lives:** inline in component, or extracted into helper functions
- Most common real-world pattern

#### Case C: Backend gives raw messy data needing heavy shaping
```jsx
// utils/transformers.ts
export function transformUserForDashboard(raw) {
  return {
    fullName: `${raw.FIRST_NM} ${raw.LAST_NM}`,
    status: raw.STATUS_CD === 'A' ? 'Active' : 'Inactive',
    accountAge: computeAge(raw.CREATED_DTS),
  };
}

// Server Component uses the helper
async function DashboardPage() {
  const raw = await fetch('http://legacy-api.internal/user');
  const shaped = transformUserForDashboard(raw);
  return <Dashboard data={shaped} />;
}
```
- **Transformation happens:** in Server Component, delegated to helper files
- **Where it lives:** `lib/`, `utils/`, or `services/` folders
- Common at GCCs, banks, IT services companies with legacy backends

### Where transformation code lives in Next.js

| Location | Best for |
|---|---|
| Inline in Server Component | Small, one-off transformations (formatting a date) |
| `lib/` or `utils/` folder | Reusable transformers across multiple pages |
| `services/` folder | Business logic + API calls grouped by domain |
| Feature-based `features/` folder | Larger apps organizing by feature |
| Route Handlers (`/app/api/`) | When transformed data exposed as URL for Client Components |

### Where transformation does NOT happen anymore

Unlike the CRA era:
- ❌ Not in `useEffect` — Server Components have no `useEffect`
- ❌ Not in Redux/Zustand — those are for client state
- ❌ Not in React Query `select` — most data comes from Server Components now
- ❌ Not in Context providers — Context holds already-transformed data

### The critical shift

In CRA era: transformation happened **in the browser** (nowhere else to put it).

In Next.js era: transformation happens **on the server**:
- Server has CPU to do it
- Happens once per request (or once per build for SSG) rather than once per user
- Transformed result sent as HTML — browser doesn't repeat the work
- Sensitive shaping logic never reaches the client

### Same task, three eras

**Task:** Show "Full Name" from a backend returning `first_name` and `last_name`.

| Era | Where transformation happens |
|---|---|
| CRA (2018) | Browser, in `useEffect` after fetch → stored in `useState` or Redux |
| Next.js Pages Router (2021) | `getServerSideProps` on the server → passed as props |
| Next.js App Router (2024+) | Server Component itself, inline or in helper → passed as JSX props |

---

## 8. Quick Company Type Reference

| Company type | Most likely architecture (2026) |
|---|---|
| Startups / YC / new SaaS | Full-Stack Next.js Monolith (#3.1) |
| GCCs (Amazon/Microsoft Hyderabad) | Next.js + existing backend (#3.2) or BFF (#3.7) |
| Banks / finance / insurance | Next.js + separate legacy backend (#3.2) |
| Large e-commerce | Micro-frontends (#3.6) or BFF (#3.7) |
| Content-heavy sites | Headless CMS + Next.js (#3.4) or Astro (#3.8) |
| Global consumer apps | Edge Serverless + Next.js (#3.5) |
| IT services companies | Usually #3.2 or #3.3 (depends on client project) |
| B2B SaaS admin panels | Vite SPA + backend (#3.3) — SEO doesn't matter |

---

## 9. Key Takeaways

1. **CRA era** = everything ran in the browser; transformation, state, rendering all happened client-side
2. **Modern era** = default flipped; components run on the server unless marked `'use client'`
3. **BFF pattern** = one Next.js codebase does both UI rendering AND multi-service aggregation
4. **Not-BFF (Architecture #2)** = Next.js is a thin UI layer calling pre-shaped backend APIs
5. **Data transformation moved from browser to server** — this single shift eliminated massive amounts of duplicate work and improved security, speed, and cost
6. **State management shifted** — Redux dominance replaced by React Query (server state) + Zustand (client state) + Server Components (transformed data as JSX props)