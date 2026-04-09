# React Ecosystem — Complete Library & Tools Guide
## Every Tool, Library & Package You Need to Know (2026)

---

# Table of Contents

- [1. Frameworks (React Meta-Frameworks)](#1-frameworks)
- [2. Build Tools & Bundlers](#2-build-tools--bundlers)
- [3. Language & Type Safety](#3-language--type-safety)
- [4. UI Component Libraries (Styled)](#4-ui-component-libraries-styled)
- [5. Headless / Unstyled Component Libraries](#5-headless--unstyled-component-libraries)
- [6. CSS & Styling Solutions](#6-css--styling-solutions)
- [7. State Management — Client State](#7-state-management--client-state)
- [8. State Management — Server State (Data Fetching)](#8-state-management--server-state)
- [9. Routing](#9-routing)
- [10. Forms & Validation](#10-forms--validation)
- [11. Animation & Motion](#11-animation--motion)
- [12. Tables & Data Grids](#12-tables--data-grids)
- [13. Charts & Data Visualization](#13-charts--data-visualization)
- [14. Authentication](#14-authentication)
- [15. Internationalization (i18n)](#15-internationalization-i18n)
- [16. Date & Time](#16-date--time)
- [17. Icons](#17-icons)
- [18. Notifications & Toasts](#18-notifications--toasts)
- [19. Drag & Drop](#19-drag--drop)
- [20. File Upload](#20-file-upload)
- [21. Rich Text Editors](#21-rich-text-editors)
- [22. PDF & Documents](#22-pdf--documents)
- [23. Real-Time / WebSockets](#23-real-time--websockets)
- [24. Backend / Database / BaaS](#24-backend--database--baas)
- [25. Testing](#25-testing)
- [26. Utility Libraries](#26-utility-libraries)
- [27. Developer Tools](#27-developer-tools)
- [28. Deployment & Hosting](#28-deployment--hosting)
- [Quick Picker — "What Should I Use?"](#quick-picker)

---

# 1. Frameworks

> React alone is just a UI library. Frameworks add routing, SSR, API routes, and more.

| Framework | What It Is | Best For | Key Advantage |
|---|---|---|---|
| **Next.js** | Full-stack React framework by Vercel | Most projects, SEO, full-stack | SSR, SSG, Server Components, API routes, file-based routing. Industry standard. |
| **Remix** | Full-stack framework (React Router team) | Web-standard focused apps | Progressive enhancement, nested routes, built on Web APIs (fetch, FormData). |
| **Astro** | Content-focused framework | Blogs, docs, marketing sites | Ships zero JS by default. Supports React components as "islands" of interactivity. |
| **Gatsby** | Static site generator | Content sites, blogs (declining) | GraphQL data layer, huge plugin ecosystem. Being replaced by Next.js/Astro. |
| **Vite + React** | Not a framework — SPA setup | Internal tools, dashboards | Fastest dev server. No SSR. Pure client-side SPA. |

```
WHEN TO PICK WHAT:
  Need SEO + full-stack?          → Next.js
  Need web standards + forms?     → Remix
  Need fast content site?         → Astro
  Need pure SPA (no SEO)?        → Vite + React
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 2. Build Tools & Bundlers

| Tool | What It Does | Why Use It |
|---|---|---|
| **Vite** | Dev server + bundler | Instant hot reload. 10-100x faster than Webpack. Uses native ES modules. Default for new React projects. |
| **Webpack** | Legacy bundler | Still used in many existing projects. Complex config. Being replaced by Vite. |
| **Turbopack** | Next.js bundler (by Vercel) | Built for Next.js. Written in Rust. Faster than Webpack for Next.js projects. |
| **esbuild** | Ultra-fast JS bundler | Written in Go. Powers Vite under the hood. Used for library builds. |
| **Bun** | Runtime + bundler + package manager | All-in-one. 3x faster than Node.js. Drop-in Node replacement. Rising fast. |
| **SWC** | Fast JS/TS compiler | Written in Rust. Replaces Babel. Used by Next.js and Vite for compilation. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 3. Language & Type Safety

| Tool | What It Does | Why Use It |
|---|---|---|
| **TypeScript** | Typed superset of JavaScript | Industry standard. Catches bugs at compile time. Better IDE autocomplete. Every major library supports it. Non-negotiable in 2026. |
| **Zod** | Schema validation | Define a schema once → validate data + infer TypeScript types. Works with forms, APIs, environment variables. |
| **tRPC** | End-to-end type-safe APIs | Automatically shares types between your Next.js frontend and backend. No API schema to maintain. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 4. UI Component Libraries (Styled)

> Pre-styled, ready-to-use components. Fast to ship but harder to customize.

| Library | Style | Components | Best For | Stars |
|---|---|---|---|---|
| **Material UI (MUI)** | Material Design | 50+ with MUI X for data grids | Enterprise apps, Google-style UI | 97k |
| **Ant Design** | Enterprise clean | 60+ including tables, date pickers | Data-heavy enterprise dashboards | 93k |
| **Mantine** | Modern clean | 100+ with hooks, forms, notifications | All-in-one solution, rapid prototyping | 27k |
| **Chakra UI** | Clean accessible | 40+ with style props system | Accessible-first apps, quick builds | 38k |
| **React Bootstrap** | Bootstrap style | Bootstrap components as React | Teams migrating from Bootstrap | 22k |
| **Semantic UI React** | Human-friendly | 50+ with clear naming | Quick readable UIs | 13k |
| **Blueprint** | Data-dense desktop | Tables, trees, complex inputs | Desktop-style data apps (by Palantir) | 21k |
| **NextUI** | Modern glossy | 30+ with Tailwind support | Modern, sleek landing pages | 23k |

```
PICKING GUIDE:
  Need everything + enterprise?     → MUI or Ant Design
  Need fastest prototyping?         → Mantine (100+ components in one package)
  Need accessibility first?         → Chakra UI
  Need data-heavy desktop app?      → Blueprint
  Need modern sleek look?           → NextUI
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 5. Headless / Unstyled Component Libraries

> Give you behavior + accessibility. YOU control the styling. Perfect with Tailwind.

| Library | What It Provides | Best For |
|---|---|---|
| **shadcn/ui** | Copy-paste components built on Radix + Tailwind. You OWN the code (not a dependency). | Custom design systems. Most popular in 2026 (104k stars). Not installed via npm — copied into your project. |
| **Radix UI** | Unstyled accessible primitives (Dialog, Dropdown, Tooltip, etc.) | Building custom design systems. Powers shadcn/ui under the hood. |
| **Headless UI** | Unstyled components by Tailwind team | Tailwind-first projects. Dropdown, Menu, Dialog, Tabs, etc. |
| **React Aria (Adobe)** | Accessibility-focused hooks & components | Maximum accessibility compliance. Adobe's a11y expertise. |
| **Base UI** | Unstyled primitives (by MUI + Radix creators) | Next-gen headless components. New in 2025/2026. |
| **Ark UI** | Headless components with state machines | Complex interactions needing predictable behavior. |
| **DaisyUI** | Tailwind plugin with semantic classes (btn, card) | Tailwind users wanting `class="btn btn-primary"` instead of utility classes. |

```
shadcn/ui vs traditional libraries:
  Traditional: npm install → import → fight customization
  shadcn/ui:   npx shadcn add button → code in YOUR project → modify freely
  
  This "copy-paste" approach is the BIGGEST shift in React ecosystem in 2025-2026.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 6. CSS & Styling Solutions

| Solution | Approach | Best For | Trade-off |
|---|---|---|---|
| **Tailwind CSS** | Utility classes in JSX | Most projects in 2026 | Learning curve for utility approach. Most popular by far. |
| **CSS Modules** | Scoped CSS files | Teams who prefer traditional CSS | No runtime cost. Built into Next.js. |
| **styled-components** | CSS-in-JS (template literals) | Dynamic styles based on props | Runtime overhead. Less popular in 2026 but still widely used. |
| **Emotion** | CSS-in-JS (similar to styled-components) | Same as styled-components | Slightly better performance. Used by MUI and Chakra. |
| **Vanilla Extract** | Zero-runtime CSS-in-TS | Type-safe styles with no runtime cost | Requires build step. Great DX with TypeScript. |
| **Panda CSS** | Build-time CSS-in-JS | Best of both worlds (CSS-in-JS DX + zero runtime) | Newer, growing community. By Chakra team. |
| **StyleX** | Meta's CSS library | Performance-critical apps | Used at Meta scale. Atomic CSS at build time. |

```
2026 TREND: Tailwind CSS is dominant. CSS-in-JS is declining (runtime cost).
            Zero-runtime solutions (Vanilla Extract, Panda CSS) are rising.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 7. State Management — Client State

| Library | Approach | Bundle Size | Best For |
|---|---|---|---|
| **useState / useReducer** | Built-in React | 0 KB | Local component state. Always start here. |
| **Context API** | Built-in React | 0 KB | Theme, auth, locale. Infrequent updates. |
| **Zustand** | Minimal hooks store | ~1 KB | Simple global state. Replacing Redux for many teams. Minimal boilerplate. |
| **Redux Toolkit (RTK)** | Flux pattern + actions/reducers | ~11 KB | Large complex apps. DevTools time-travel. Enterprise standard. |
| **Jotai** | Atomic (bottom-up) | ~2 KB | Fine-grained reactivity. Each atom is independent. Minimal re-renders. |
| **Recoil** | Atomic (by Meta) | ~20 KB | Facebook-scale apps. Similar to Jotai but heavier. Development slowed. |
| **Valtio** | Proxy-based (mutable style) | ~3 KB | Developers who prefer mutable syntax. Proxy handles reactivity. |
| **XState** | State machines + statecharts | ~15 KB | Complex flows: wizards, auth, media players. Visual editor. |
| **Legend State** | Proxy-based, fast | ~4 KB | Performance-critical apps. Fine-grained updates. |

```
DECISION TREE:
  Simple app, few shared states?          → Zustand
  Large app, many developers, need DevTools? → Redux Toolkit
  Want atomic approach (fine-grained)?    → Jotai
  Complex state flows (step-by-step)?     → XState
  Just component-level state?             → useState + Context
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 8. State Management — Server State

> Server state = data from APIs. Different from UI state. Needs caching, refetching, syncing.

| Library | What It Does | Best For |
|---|---|---|
| **TanStack Query (React Query)** | Cache, refetch, sync server data. Loading/error states. Background updates. Pagination. | Any app that fetches data. THE standard in 2026. |
| **SWR** | Stale-while-revalidate. Lighter than TanStack Query. By Vercel. | Simpler projects. Good Next.js integration. |
| **RTK Query** | Server state built into Redux Toolkit | If already using Redux. Avoids adding another library. |
| **Apollo Client** | GraphQL client with cache | GraphQL APIs. Full-featured with local state too. |
| **URQL** | Lightweight GraphQL client | GraphQL with simpler API than Apollo. |

```
REST API?    → TanStack Query (most popular) or SWR (simpler)
GraphQL?     → Apollo Client (full-featured) or URQL (lighter)
Using Redux? → RTK Query (built-in)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 9. Routing

| Library | Best For | Key Feature |
|---|---|---|
| **Next.js App Router** | Next.js projects | File-based routing. Layouts, loading/error states, parallel routes. |
| **React Router v7** | Vite SPA or Remix | Most popular client-side router. Loaders, actions, nested routes. |
| **TanStack Router** | Type-safe routing | Full TypeScript inference for routes, params, search. New and growing. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 10. Forms & Validation

| Library | What It Does | Best For |
|---|---|---|
| **React Hook Form** | Performant form handling | Most forms. Uncontrolled by default (fast). Minimal re-renders. |
| **Zod** | Schema validation (TypeScript-first) | Pair with React Hook Form. Define schema → validate + infer types. |
| **Yup** | Schema validation | Older alternative to Zod. Less TypeScript-native. |
| **TanStack Form** | Type-safe form management | Newer alternative to React Hook Form. Growing in 2026. |
| **Formik** | Form handling | Legacy. Being replaced by React Hook Form. |
| **Native HTML forms** | No library | Simple forms. Server Actions in Next.js. |

```
2026 STANDARD: React Hook Form + Zod (for complex forms)
               Native HTML forms + Zod (for simple forms with Server Actions)
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 11. Animation & Motion

| Library | What It Does | Best For |
|---|---|---|
| **Framer Motion** | Declarative animations for React | Most animation needs. Layout animations, gestures, exit animations. The most popular. |
| **React Spring** | Physics-based animations | Natural-feeling animations. Configurable spring physics. |
| **GSAP** | Professional animation toolkit | Complex timeline animations. Used by agencies and creative sites. Not React-specific. |
| **AutoAnimate** | Automatic animations with 1 line | Add smooth transitions without config. `<div ref={autoAnimate}>`. |
| **Motion** | Framer Motion successor (renamed) | Same as Framer Motion. Being rebranded in 2026. |
| **Lottie (lottie-react)** | After Effects animations in React | Complex SVG/vector animations exported from After Effects. |
| **CSS transitions** | No library | Simple hover/mount animations. No library needed. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 12. Tables & Data Grids

| Library | What It Does | Best For |
|---|---|---|
| **TanStack Table** | Headless table (you style it). Sort, filter, pagination, grouping. | Custom-styled tables. Full control over UI. Free. |
| **AG Grid** | Enterprise data grid | 100k+ rows. Excel-like features. Enterprise license. |
| **MUI DataGrid** | Material UI data grid (MUI X) | MUI projects needing advanced grids. |
| **Mantine DataTable** | Mantine-based data table | Mantine projects. Clean defaults. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 13. Charts & Data Visualization

| Library | What It Does | Best For |
|---|---|---|
| **Recharts** | Composable chart components | Simple charts (bar, line, pie). React-native feel. |
| **Chart.js + react-chartjs-2** | Wrapper for Chart.js | Familiar Canvas-based charts. Good docs. |
| **Nivo** | Rich data viz built on D3 | Beautiful charts with many types. Server-side rendering support. |
| **Visx** | Low-level D3 + React (by Airbnb) | Custom visualizations. Full control over SVG. |
| **Tremor** | Dashboard charts with Tailwind | Quick dashboards with Tailwind styling. |
| **D3.js** | The low-level king | Any custom visualization. Steep learning curve. Not React-specific. |
| **Plotly (react-plotly.js)** | Scientific charts | 3D charts, statistical plots, scientific data. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 14. Authentication

| Library | What It Does | Best For |
|---|---|---|
| **NextAuth.js / Auth.js** | Authentication for Next.js. OAuth, credentials, magic links. | Next.js projects. Free. Open-source. |
| **Clerk** | Drop-in auth UI + backend. Social login, MFA, user management. | Fastest setup. Beautiful pre-built UI. Paid (free tier). |
| **Supabase Auth** | Auth bundled with Supabase (Postgres backend). | If using Supabase for database. Row-level security. |
| **Firebase Auth** | Google's auth service. Social, email, phone auth. | If using Firebase ecosystem. |
| **Lucia** | Lightweight auth library. DIY approach. | Developers wanting full control over auth. |
| **Kinde** | Auth + user management as a service. | B2B SaaS with org management. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 15. Internationalization (i18n)

| Library | What It Does | Best For |
|---|---|---|
| **react-i18next** | Most popular i18n solution. Based on i18next. | Most projects. Huge ecosystem. |
| **next-intl** | i18n specifically for Next.js App Router. | Next.js projects. Server Component support. |
| **react-intl (FormatJS)** | Uses browser Intl API. Dates, numbers, plurals. | When you need precise formatting (currency, dates). |
| **Lingui** | Lightweight, macro-based i18n. | Performance-sensitive projects. Small bundle. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 16. Date & Time

| Library | Size | Best For |
|---|---|---|
| **date-fns** | ~6 KB (tree-shakeable) | Modern, functional. Import only what you need. |
| **dayjs** | ~2 KB | Moment.js replacement. Same API, tiny size. |
| **Temporal API** | 0 KB (browser built-in) | Coming native to browsers. Not fully available yet (2026). |
| ~~Moment.js~~ | ~~67 KB~~ | ~~DEPRECATED. Don't use in new projects.~~ |

[↑ Back to Table of Contents](#table-of-contents)

---

# 17. Icons

| Library | Icons | Best For |
|---|---|---|
| **Lucide React** | 1500+ clean icons | Default with shadcn/ui. Clean, consistent. |
| **React Icons** | 40,000+ from 20+ sets (FA, Material, etc.) | When you need icons from specific sets (Font Awesome, etc.). |
| **Heroicons** | 300+ by Tailwind team | Tailwind projects. Two styles: outline + solid. |
| **Phosphor Icons** | 9000+ flexible icons | 6 weights per icon. Very customizable. |
| **Tabler Icons** | 5000+ open source | Free, clean, well-maintained. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 18. Notifications & Toasts

| Library | What It Does | Best For |
|---|---|---|
| **Sonner** | Beautiful toast notifications | Modern, animated. Best-looking toasts in 2026. Works with shadcn/ui. |
| **react-hot-toast** | Lightweight toasts | Simple, minimal, customizable. ~5 KB. |
| **React Toastify** | Feature-rich toasts | Most features (progress bar, drag to dismiss). Larger bundle. |
| **Notistack** | Stackable snackbars (MUI) | MUI projects. Material Design style. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 19. Drag & Drop

| Library | What It Does | Best For |
|---|---|---|
| **dnd-kit** | Modern, modular drag and drop | Most projects. Accessible, performant, small bundle. Recommended in 2026. |
| **react-beautiful-dnd** | List reordering (by Atlassian) | Kanban boards, list sorting. DEPRECATED (no updates since 2023). |
| **@hello-pangea/dnd** | Fork of react-beautiful-dnd (maintained) | If you need the react-beautiful-dnd API with ongoing maintenance. |
| **react-dnd** | Flexible drag and drop | Complex custom drag-and-drop interactions. Steeper learning curve. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 20. File Upload

| Library | What It Does | Best For |
|---|---|---|
| **react-dropzone** | Drag-and-drop file zone | Most file upload needs. Lightweight. Handles file types, sizes. |
| **UploadThing** | Full upload solution (React + backend) | Next.js projects. Handles upload, storage, and URLs. |
| **Filepond** | Rich file upload UI | Beautiful progress, image preview, transform. |
| **Uppy** | Full upload dashboard (by Transloadit) | Multiple upload sources (camera, drive, dropbox, URL). |

[↑ Back to Table of Contents](#table-of-contents)

---

# 21. Rich Text Editors

| Library | What It Does | Best For |
|---|---|---|
| **Tiptap** | Headless rich text editor (based on ProseMirror) | Custom editor UI. Collaborative editing support. Most popular in 2026. |
| **Plate** | Tiptap-like for React (based on Slate) | React-first editor. Plugin system. |
| **Lexical** | Meta's rich text framework | High-performance. Used by Facebook. |
| **React Quill** | Quill editor for React | Quick setup. Less customizable. Older. |
| **Editor.js** | Block-style editor (like Notion) | Notion-like editing experience. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 22. PDF & Documents

| Library | What It Does | Best For |
|---|---|---|
| **react-pdf** | Display PDFs in React | Viewing PDFs in the browser. |
| **@react-pdf/renderer** | Generate PDFs with React components | Create invoices, reports, tickets as PDFs. JSX to PDF. |
| **jsPDF** | Generate PDFs from JS | Simple PDF generation without React components. |
| **docx** | Generate Word documents | Creating .docx files from data. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 23. Real-Time / WebSockets

| Library | What It Does | Best For |
|---|---|---|
| **Socket.IO (+ socket.io-react-hook)** | WebSocket with auto-reconnect, rooms, namespaces | Chat, notifications, live updates. Most popular. |
| **Ably** | Real-time messaging service | Managed real-time without running your own WebSocket server. |
| **Pusher** | Real-time channels as a service | Simple pub/sub without WebSocket infrastructure. |
| **Supabase Realtime** | Real-time database changes | If using Supabase. Auto-sync database changes to UI. |
| **Liveblocks** | Real-time collaboration | Multiplayer features: cursors, presence, comments. |
| **PartyKit** | Edge-based real-time | Deploy real-time logic at the edge. Growing in 2026. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 24. Backend / Database / BaaS

| Tool | Type | Best For |
|---|---|---|
| **Next.js API Routes** | Server endpoints inside Next.js | Simple APIs co-located with frontend. |
| **Next.js Server Actions** | Call server functions from components | Form submissions, mutations without API routes. |
| **Prisma** | Database ORM (TypeScript-first) | Any SQL database. Auto-generates types. Most popular ORM. |
| **Drizzle ORM** | Lightweight TypeScript ORM | Faster than Prisma. SQL-like syntax. Rising fast in 2026. |
| **PostgreSQL** | Relational database | Most apps. The default SQL database choice. |
| **MongoDB** | NoSQL document database | Flexible schema. MERN stack. |
| **Supabase** | Postgres + Auth + Storage + Realtime | Open-source Firebase alternative. Best BaaS in 2026. |
| **Firebase** | Google's BaaS | Quick prototyping. Auth + Firestore + Storage. |
| **Convex** | Reactive backend | Auto-syncing real-time data. No manual cache invalidation. |
| **Appwrite** | Self-hosted BaaS | When you need to host your own backend. |
| **PlanetScale** | Serverless MySQL | Branching, scaling. Serverless-friendly. |
| **Neon** | Serverless Postgres | Branching, auto-scaling Postgres. Free tier. |
| **Turso** | Edge SQLite (libSQL) | Edge-deployed database. Very fast reads. |
| **Upstash** | Serverless Redis + Kafka | Rate limiting, caching, queues. Edge-compatible. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 25. Testing

| Tool | Type | Best For |
|---|---|---|
| **Vitest** | Unit test runner | Fastest. Vite-native. Replacing Jest for Vite projects. |
| **Jest** | Unit test runner | Still widely used. Default for Create React App / older projects. |
| **React Testing Library** | Component testing | Test by user behavior (click, type). Industry standard. |
| **Playwright** | E2E browser testing | Cross-browser (Chrome, Firefox, Safari). Auto-wait. Parallel. Best E2E in 2026. |
| **Cypress** | E2E browser testing | Great DX. Interactive test runner. Chrome-focused. |
| **MSW (Mock Service Worker)** | API mocking | Mock network requests in tests. Realistic. Works with any test framework. |
| **Storybook** | Component documentation | Visual component library. Develop components in isolation. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 26. Utility Libraries

| Library | What It Does | Why Use It |
|---|---|---|
| **clsx / classnames** | Conditional className joining | `clsx('base', isActive && 'active')`. Essential with Tailwind. |
| **usehooks-ts** | Collection of TypeScript React hooks | useLocalStorage, useMediaQuery, useDarkMode, etc. Don't reinvent hooks. |
| **lodash** | Utility functions (debounce, groupBy, etc.) | General-purpose utils. Tree-shake to import only what you need. |
| **uuid / nanoid** | Generate unique IDs | nanoid is smaller and faster. For keys, tokens, etc. |
| **axios** | HTTP client | Features over fetch: interceptors, timeout, cancel. Still popular but native fetch is gaining. |
| **nuqs** | Typed URL search params | Type-safe search/sort/pagination in URL state. Great with Next.js. |
| **next-safe-action** | Type-safe Server Actions | Validated, authenticated Server Actions in Next.js. |
| **sharp** | Image processing | Resize, convert, optimize images on the server. Used by Next.js Image. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 27. Developer Tools

| Tool | What It Does | Why Use It |
|---|---|---|
| **VS Code** | Code editor | 90%+ of React developers use this. Extensions: ESLint, Prettier, Tailwind IntelliSense. |
| **Chrome DevTools** | Browser debugging | Network, console, performance, elements inspection. |
| **React DevTools** | React-specific Chrome extension | Inspect components, props, state, hooks. Profile renders. |
| **ESLint** | Linter (catch code errors) | Enforce code quality rules. Catch bugs before runtime. |
| **Prettier** | Code formatter | Auto-format on save. Consistent style across team. |
| **Biome** | ESLint + Prettier replacement | Faster (written in Rust). Single tool for lint + format. Rising in 2026. |
| **Storybook** | Component development environment | Build/test components in isolation. Visual documentation. |
| **React Query DevTools** | TanStack Query debugging | Visualize cache, queries, mutations. Built-in floating panel. |
| **Redux DevTools** | Redux debugging | Time-travel debugging. Inspect every action and state change. |
| **Postman / Thunder Client** | API testing | Test endpoints before building UI. Thunder Client is a VS Code extension. |
| **Figma** | Design collaboration | Read designs from designers. Inspect spacing, colors, fonts. |

[↑ Back to Table of Contents](#table-of-contents)

---

# 28. Deployment & Hosting

| Platform | Best For | Key Feature |
|---|---|---|
| **Vercel** | Next.js projects | Made by Next.js creators. Zero-config deploy. Edge functions. Free tier. |
| **Netlify** | Static sites + serverless | Easy deploy. Form handling. Edge functions. Free tier. |
| **Cloudflare Pages** | Edge deployment | Global edge network. Very fast. Workers for server logic. Free tier. |
| **AWS Amplify** | AWS ecosystem | Full AWS integration. For teams already on AWS. |
| **Railway** | Full-stack apps + databases | Easy deploy with database. Good for backends. |
| **Fly.io** | Edge-deployed containers | Deploy containers globally. Good for custom backends. |
| **Docker** | Containerization | Package your app to run anywhere. Required for custom infra. |
| **GitHub Actions** | CI/CD | Auto-test and deploy on git push. Free for public repos. |

[↑ Back to Table of Contents](#table-of-contents)

---

# Quick Picker

> "I'm overwhelmed. Just tell me what to use."

## The 2026 Starter Stack (Get a Job)

```
Framework:    Next.js
Language:     TypeScript
Styling:      Tailwind CSS + shadcn/ui
State:        Zustand (client) + TanStack Query (server)
Forms:        React Hook Form + Zod
Auth:         NextAuth.js or Clerk
Database:     Prisma + PostgreSQL (or Supabase)
Testing:      Vitest + RTL + Playwright
Deploy:       Vercel
Icons:        Lucide React
Toasts:       Sonner
```

## The "I Just Need a Quick SPA" Stack

```
Build:        Vite
Routing:      React Router v7
Styling:      Tailwind CSS
State:        Zustand
Fetching:     TanStack Query
Forms:        React Hook Form + Zod
Deploy:       Netlify or Vercel
```

## The Enterprise Stack

```
Framework:    Next.js
Language:     TypeScript (strict)
UI:           MUI or Ant Design
State:        Redux Toolkit + RTK Query
Forms:        React Hook Form + Zod
Auth:         Auth.js + custom backend
Database:     PostgreSQL + Prisma
Testing:      Jest + RTL + Playwright + Storybook
CI/CD:        GitHub Actions + Docker
Deploy:       AWS or Vercel Enterprise
Monitoring:   Sentry (errors) + Datadog/Vercel Analytics
```

[↑ Back to Table of Contents](#table-of-contents)
