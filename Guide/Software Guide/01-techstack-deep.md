# Every Tech Stack Type — Deep Dive Guide
## Named Stacks, App Types, Roles, Company Sizes — All Explained

---

# Table of Contents

- [Part 1: Named Stacks](#part-1-named-stacks)
  - [MERN Stack](#mern-stack)
  - [PERN Stack](#pern-stack)
  - [T3 Stack](#t3-stack)
  - [MEAN Stack](#mean-stack)
  - [LAMP / LEMP Stack](#lamp--lemp-stack)
  - [JAMstack](#jamstack)
  - [Django Stack](#django-stack)
  - [Rails Stack](#rails-stack)
  - [Spring Stack](#spring-stack)
  - [.NET Stack](#net-stack)
  - [Go Stack](#go-stack)
  - [Named Stacks Comparison](#named-stacks-comparison)
- [Part 2: Stacks by App Type](#part-2-stacks-by-app-type)
  - [SPA (Single Page App)](#spa--single-page-app)
  - [SSR (Server-Side Rendered)](#ssr--server-side-rendered)
  - [E-Commerce](#e-commerce)
  - [SaaS](#saas)
  - [Content / Blog / Docs](#content--blog--docs)
  - [Real-Time App](#real-time-app)
  - [Mobile App](#mobile-app)
  - [Desktop App](#desktop-app)
  - [API / Microservices](#api--microservices)
  - [AI / ML App](#ai--ml-app)
  - [CMS / WordPress](#cms--wordpress)
- [Part 3: Stacks by Role](#part-3-stacks-by-role)
- [Part 4: Stacks by Company Size](#part-4-stacks-by-company-size)
- [The Ultimate Decision Guide](#the-ultimate-decision-guide)

---

# Part 1: Named Stacks

---

## MERN Stack

```
M = MongoDB     (NoSQL database)
E = Express.js  (backend framework)
R = React       (frontend library)
N = Node.js     (JavaScript runtime)
```

| | Details |
|---|---|
| **What** | Full JavaScript stack. Same language (JS) on frontend, backend, and database queries. The most taught stack in bootcamps. |
| **How It Works** | React renders the UI → sends API requests to Express → Express processes logic and talks to MongoDB → sends data back to React. All in JavaScript. |
| **Best For** | Learning full-stack. Rapid prototyping. Startups. Apps with flexible/changing data schemas. |
| **Real Apps** | Instagram (partial), Netflix UI, Uber Eats, Facebook (partial). |
| **Advantages** | One language everywhere. Huge community. Most tutorials. MongoDB is flexible (no rigid schema). Fast to prototype. |
| **Disadvantages** | MongoDB isn't great for relational data (users→posts→comments). No type safety by default. Express is minimal (you add everything yourself). |
| **When NOT to Use** | Data-heavy apps with complex relationships (use PostgreSQL instead). Apps needing strict data integrity (banking, finance). |

```
TYPICAL PROJECT STRUCTURE:
  /client          → React app (Vite or CRA)
    /src
      /components
      /pages
      App.jsx
  /server          → Express API
    /routes
    /controllers
    /models        → Mongoose schemas
    server.js
  package.json
```

```
EXAMPLE FLOW: User creates a blog post
  1. React form → user types title + content
  2. React sends POST /api/posts to Express
  3. Express validates data
  4. Express saves to MongoDB via Mongoose
  5. MongoDB stores as JSON document: { title, content, author, date }
  6. Express returns { success: true, post: {...} }
  7. React updates UI with new post
```

**2026 Status:** Still popular for learning. Production apps often switch to PostgreSQL (PERN) for better data relationships.

---

## PERN Stack

```
P = PostgreSQL   (SQL relational database)
E = Express.js   (backend framework)
R = React        (frontend library)
N = Node.js      (JavaScript runtime)
```

| | Details |
|---|---|
| **What** | MERN but with PostgreSQL instead of MongoDB. Relational database for structured data with relationships. |
| **How It Works** | Same as MERN, but data is stored in tables with rows and columns (like spreadsheets) instead of JSON documents. |
| **Best For** | Apps with structured, relational data. E-commerce, social media, CRM, any app where data has clear relationships. |
| **Advantages** | PostgreSQL is more powerful than MongoDB for relationships (JOINs, transactions, constraints). Data integrity enforced by the database. Better for complex queries. |
| **Disadvantages** | SQL has a learning curve. Schema must be designed upfront (less flexible than MongoDB). Migrations needed for schema changes. |
| **When NOT to Use** | Quick prototypes where data schema isn't clear yet. Very simple apps. |

```
MERN vs PERN — WHEN TO PICK WHICH:
  Data is flexible / changes often / nested JSON      → MERN (MongoDB)
  Data has relationships / needs integrity / structured → PERN (PostgreSQL)
  
  Example: blog with tags, categories, comments        → PERN
  Example: IoT sensor data with varying fields          → MERN
```

**2026 Status:** Growing. Many teams switching from MongoDB to PostgreSQL. PERN + Prisma is a common modern setup.

---

## T3 Stack

```
T3 = TypeScript + tRPC + Tailwind
(Plus: Next.js + Prisma + NextAuth)
```

| | Details |
|---|---|
| **What** | The modern full-stack TypeScript stack created by Theo Browne. Full end-to-end type safety — types flow from database to frontend automatically. |
| **How It Works** | Prisma defines database schema → generates TypeScript types → tRPC shares those types between backend and frontend → Next.js renders the UI → Tailwind styles it. Change one type → everything updates. |
| **Best For** | TypeScript-first teams. SaaS products. Solo developers wanting maximum type safety. |
| **Real Apps** | Many indie SaaS products. cal.com (open-source scheduling). |
| **Advantages** | End-to-end type safety (catch bugs at compile time, not runtime). No API schema to maintain. Fast development with auto-complete everywhere. Modern tools. |
| **Disadvantages** | Steeper learning curve (TypeScript, tRPC, Prisma all at once). Tightly coupled to Next.js. Not great for teams with non-TypeScript backends. |
| **When NOT to Use** | Team doesn't know TypeScript well. Backend is Python/Java/Go. Need to support non-web clients (mobile app with different tech). |

```
THE T3 MAGIC — END-TO-END TYPE SAFETY:

  Prisma schema:
    model User { id Int, name String, email String }
    → auto-generates TypeScript types

  tRPC router:
    getUser = (id) => prisma.user.findUnique({ where: { id } })
    → return type is automatically User | null

  React component:
    const { data } = trpc.getUser.useQuery({ id: 1 })
    → data is typed as User | null
    → data.name → autocomplete works!
    → data.nonExistent → ❌ TypeScript error instantly

  Change the database schema → TypeScript errors show EVERYWHERE
  that needs updating. No runtime surprises.
```

**2026 Status:** Very popular among solo developers and small teams. The "default modern stack."

---

## MEAN Stack

```
M = MongoDB     (NoSQL database)
E = Express.js  (backend framework)
A = Angular     (frontend framework)
N = Node.js     (JavaScript runtime)
```

| | Details |
|---|---|
| **What** | Same as MERN but with Angular instead of React. Full JavaScript stack with a strict, opinionated frontend framework. |
| **Best For** | Enterprise teams who prefer Angular's structure. Teams coming from Java/C# who like class-based, typed code. |
| **Advantages** | Angular has EVERYTHING built-in (routing, forms, HTTP, testing). TypeScript required. Strict structure = consistency across large teams. |
| **Disadvantages** | Angular has a steeper learning curve. Smaller community than React. Fewer job postings. |

**2026 Status:** Declining. React has overtaken Angular in most markets. Still used in enterprise (banks, government).

---

## LAMP / LEMP Stack

```
LAMP:                          LEMP:
L = Linux (operating system)   L = Linux
A = Apache (web server)        E = Nginx (web server, pronounced "engine-x")
M = MySQL (database)           M = MySQL
P = PHP (programming language) P = PHP
```

| | Details |
|---|---|
| **What** | The original web stack from the 2000s. Powers WordPress, Facebook (originally), Wikipedia. Still runs billions of websites. |
| **Best For** | WordPress sites. Shared hosting. Legacy projects. |
| **Advantages** | Cheapest hosting (shared hosting supports PHP). WordPress ecosystem (themes, plugins). Massive legacy codebase. |
| **Disadvantages** | PHP is less modern than JS/Python. Apache is slower than Nginx for high traffic. Not great for SPAs or real-time apps. |

**2026 Status:** Legacy but MASSIVE. 43% of all websites run WordPress (PHP + MySQL). Not dying, just not growing.

---

## JAMstack

```
J = JavaScript  (dynamic functionality)
A = APIs        (server functions called on demand)
M = Markup      (pre-built HTML at deploy time)
```

| | Details |
|---|---|
| **What** | An architecture, not specific tools. Pre-render as much as possible at build time → serve static HTML → use APIs for dynamic parts. Coined by Netlify. |
| **How It Works** | At build time: generate all HTML pages. At runtime: serve pre-built HTML from CDN (fast). Dynamic features (login, comments) handled by APIs and serverless functions. |
| **Example Tools** | Gatsby, Next.js (SSG mode), Astro, Hugo, 11ty + Netlify/Vercel. |
| **Best For** | Blogs, docs, marketing sites, portfolios — content that doesn't change every second. |
| **Advantages** | Blazing fast (pre-built HTML on CDN). Secure (no server to hack). Cheap (static hosting is nearly free). |
| **Disadvantages** | Not great for highly dynamic apps (dashboards, social media). Build times grow with content size. |

**2026 Status:** The term "JAMstack" is used less, but the IDEA lives on in Astro, Next.js SSG, and Cloudflare Pages.

---

## Django Stack

```
Python + Django + PostgreSQL + (React or Django Templates for frontend)
```

| | Details |
|---|---|
| **What** | Python's "batteries included" web framework. Admin panel, ORM, auth, forms, middleware — ALL built-in from day one. |
| **Best For** | Python developers. Data-heavy apps. Apps needing an admin dashboard. Rapid prototyping. |
| **Real Apps** | Instagram, Pinterest, Spotify (backend), Dropbox, Mozilla. |
| **Advantages** | Fastest to build CRUD apps (admin auto-generated). Python is great for data/ML. Mature and battle-tested. Excellent security defaults. |
| **Disadvantages** | Monolithic (hard to split into microservices). Python is slower than Node.js for I/O. Django templates feel old vs React. |

---

## Rails Stack

```
Ruby + Ruby on Rails + PostgreSQL + (Hotwire/Turbo or React for frontend)
```

| | Details |
|---|---|
| **What** | "Convention over configuration." Rails makes hundreds of decisions for you so you write less code. |
| **Best For** | Startups wanting to ship fast. MVPs. Solo developers. |
| **Real Apps** | GitHub, Shopify, Basecamp, Airbnb (originally), Twitch. |
| **Advantages** | Fastest to build MVPs. Massive amount of built-in magic. Great community. Battle-tested for startups. |
| **Disadvantages** | Ruby is slower than other languages. Harder to hire Ruby developers (smaller pool). Can feel "magical" (hard to debug when things break). |

---

## Spring Stack

```
Java + Spring Boot + PostgreSQL/MySQL + (Angular or React for frontend)
```

| | Details |
|---|---|
| **What** | Enterprise Java framework. The most used backend framework for large corporations worldwide. |
| **Best For** | Banks, insurance, government, healthcare, Fortune 500. |
| **Advantages** | Extremely mature. Strong typing. Massive enterprise ecosystem. Best tooling (IntelliJ). JVM performance. |
| **Disadvantages** | Verbose (lots of boilerplate). Slow development speed compared to Node/Python. Heavy. |

---

## .NET Stack

```
C# + ASP.NET Core + SQL Server + (Angular or React for frontend)
```

| | Details |
|---|---|
| **What** | Microsoft's full-stack framework. Tight Azure integration. |
| **Best For** | Microsoft shops. Azure cloud. Enterprise. Game backends (Unity uses C#). |
| **Advantages** | Excellent IDE (Visual Studio). Azure integration. Strong typing. High performance. |
| **Disadvantages** | Smaller web community than Node/Python. Historically Windows-only (now cross-platform). |

---

## Go Stack

```
Go + Gin/Fiber/Echo + PostgreSQL + (React for frontend)
```

| | Details |
|---|---|
| **What** | Google's compiled language. Built for high-performance concurrent servers. |
| **Best For** | Microservices. DevOps tools. High-traffic APIs. CLI tools. |
| **Real Apps** | Docker, Kubernetes, Terraform — all written in Go. |
| **Advantages** | Compiled (fast). Simple language (small spec). Built-in concurrency (goroutines). Small binaries. |
| **Disadvantages** | No generics until recently. Verbose error handling. Smaller web ecosystem than Node. |

---

## Named Stacks Comparison

| Stack | Language | DB | Frontend | Best For | Job Market |
|---|---|---|---|---|---|
| **MERN** | JavaScript | MongoDB | React | Bootcamps, prototypes | Large |
| **PERN** | JavaScript | PostgreSQL | React | Structured data apps | Growing |
| **T3** | TypeScript | PostgreSQL | Next.js | Modern full-stack, SaaS | Growing fast |
| **MEAN** | TypeScript | MongoDB | Angular | Enterprise Angular shops | Declining |
| **LAMP** | PHP | MySQL | PHP/HTML | WordPress, legacy | Huge (legacy) |
| **Django** | Python | PostgreSQL | Django/React | Data/ML apps, admin | Medium |
| **Rails** | Ruby | PostgreSQL | Rails/React | Startup MVPs | Small but loyal |
| **Spring** | Java | MySQL/Postgres | Angular/React | Enterprise, banking | Very large |
| **.NET** | C# | SQL Server | Angular/React | Microsoft/Azure shops | Large |
| **Go** | Go | PostgreSQL | React | Microservices, DevOps | Growing |

```
DECISION:
  Learning full-stack web?    → MERN or PERN
  Modern type-safe stack?     → T3
  Data science / AI?          → Django (Python)
  Ship MVP fastest?           → Rails or T3
  Enterprise job?             → Spring (Java) or .NET (C#)
  High-performance APIs?      → Go
  WordPress / existing sites? → LAMP
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Part 2: Stacks by App Type

---

## SPA — Single Page App

```
WHAT: Entire app loads once. Navigation happens WITHOUT page reloads.
      Browser downloads HTML + JS + CSS once, then JS handles everything.
EXAMPLES: Gmail, Trello, Figma, Google Maps
```

| Layer | Tool | Why |
|---|---|---|
| Build | Vite | Fastest dev server |
| Frontend | React | Component-based UI |
| Routing | React Router v7 | Client-side navigation |
| Styling | Tailwind + shadcn/ui | Fast, consistent |
| Client State | Zustand | Lightweight global state |
| Server State | TanStack Query | API data caching |
| Forms | React Hook Form + Zod | Validation |
| Deploy | Netlify or Vercel | Static hosting |

```
PROS: Fast after initial load. Rich interactions. App-like feel.
CONS: Bad SEO (content rendered by JS). Slow initial load (big JS bundle).
      Browser back button can be tricky.

USE WHEN: Internal tools, dashboards, admin panels, apps where SEO doesn't matter.
DON'T USE WHEN: Need SEO (blog, e-commerce, marketing). Use SSR instead.
```

---

## SSR — Server-Side Rendered

```
WHAT: Server generates HTML for each page request.
      Browser gets ready-to-display HTML (good for SEO, fast first paint).
EXAMPLES: Most e-commerce sites, news sites, marketing sites
```

| Layer | Tool | Why |
|---|---|---|
| Framework | Next.js | SSR + SSG + Server Components |
| Styling | Tailwind + shadcn/ui | Fast, consistent |
| Client State | Zustand | Lightweight |
| Server State | TanStack Query | Caching |
| Backend | Next.js API Routes | Co-located |
| Database | PostgreSQL + Prisma | Relational data |
| Auth | NextAuth.js | Built for Next.js |
| Deploy | Vercel | Optimized for Next.js |

```
PROS: Great SEO. Fast first paint. Server handles heavy lifting.
CONS: Server cost (each request hits your server). More complex than SPA.

USE WHEN: SEO matters. E-commerce. Marketing. Public-facing content.
```

---

## E-Commerce

```
WHAT: Online store. Products, categories, cart, checkout, payments, orders.
EXAMPLES: Amazon, Shopify stores, independent online stores
```

| Layer | Tool | Why |
|---|---|---|
| Framework | Next.js (SSR for SEO) | Product pages need SEO |
| Styling | Tailwind + shadcn/ui | Consistent product UI |
| State | Zustand (cart) + TanStack Query (products) | Cart in client, products from API |
| Backend | Next.js API Routes | Server-side logic |
| Database | PostgreSQL + Prisma | Products, orders, users, relationships |
| Payments | Stripe | Industry standard. Checkout, subscriptions. |
| Search | Algolia or Meilisearch | Fast product search with filters |
| Images | Cloudinary or S3 | Product image storage + optimization |
| Auth | NextAuth.js | Customer accounts |
| Email | Resend | Order confirmations, receipts |
| Deploy | Vercel | Fast, edge-optimized |

```
OR USE A PLATFORM:
  Shopify         → no-code store builder. Handles everything.
  Medusa.js       → open-source Shopify alternative (Node.js).
  Saleor          → open-source, GraphQL-based.
  WooCommerce     → WordPress plugin for e-commerce.

CUSTOM vs PLATFORM:
  Need full control, custom features?  → Custom stack (Next.js + Stripe)
  Need to launch fast, standard store? → Shopify or Medusa
```

---

## SaaS (Software as a Service)

```
WHAT: Web app users pay to use monthly/yearly.
EXAMPLES: Slack, Notion, Canva, Vercel, Linear, Cal.com
```

| Layer | Tool | Why |
|---|---|---|
| Framework | Next.js + TypeScript | Full-stack, type-safe |
| UI | Tailwind + shadcn/ui | Beautiful, consistent |
| State | Zustand + TanStack Query | Client + server state |
| API | tRPC or Next.js Server Actions | Type-safe end-to-end |
| Database | PostgreSQL + Prisma | Relational data |
| Auth | Clerk or NextAuth.js | Multi-tenant, social login, MFA |
| Payments | Stripe | Subscriptions, invoices, customer portal |
| Email | Resend + React Email | Beautiful transactional emails |
| Analytics | PostHog | Product analytics (user behavior) |
| Error Tracking | Sentry | Catch bugs in production |
| Feature Flags | LaunchDarkly or Vercel Flags | Roll out features gradually |
| Deploy | Vercel | Zero-config, preview deployments |
| Monitoring | Vercel Analytics + Sentry | Performance + errors |

```
KEY SaaS FEATURES TO BUILD:
  ✅ User auth (signup, login, social, MFA)
  ✅ Subscription management (plans, billing, upgrade/downgrade)
  ✅ Dashboard (the core product)
  ✅ Settings (profile, team, billing)
  ✅ Admin panel (manage users, view metrics)
  ✅ Email notifications (welcome, invoice, alerts)
  ✅ Landing page (marketing, pricing page)
```

---

## Content / Blog / Docs

```
WHAT: Content-heavy sites. Mostly static. Great SEO needed.
EXAMPLES: Company blogs, documentation, portfolios, landing pages
```

| Layer | Tool | Why |
|---|---|---|
| Framework | Astro (best for content) or Next.js | Astro ships zero JS by default |
| Content | MDX (Markdown + JSX) | Write content in Markdown, embed React components |
| OR CMS | Sanity / Contentful / Strapi / Payload | Non-developers can edit content |
| Styling | Tailwind | Clean typography, responsive |
| Deploy | Vercel or Cloudflare Pages | Static = free/cheap hosting |

```
STATIC vs HEADLESS CMS:
  Developer writes content in files?    → MDX + Astro (simplest)
  Non-developers need to edit content?  → Headless CMS (Sanity, Contentful)
  WordPress team wants modern frontend? → WordPress (backend) + Next.js (frontend)
```

---

## Real-Time App

```
WHAT: Live data that updates without refreshing the page.
      Server PUSHES data to client (not just client requesting).
EXAMPLES: Chat (WhatsApp web), live dashboards, collaborative editors, gaming
```

| Layer | Tool | Why |
|---|---|---|
| Framework | Next.js or Vite + React | UI framework |
| Real-time | Socket.IO | Most popular. Auto-reconnect, rooms. |
| OR | Supabase Realtime | If using Supabase. Auto-sync DB changes. |
| OR | Liveblocks | Multiplayer: cursors, presence, comments. |
| Backend | Node.js + Express + Socket.IO server | Handles WebSocket connections |
| Database | PostgreSQL + Redis | Postgres for data, Redis for pub/sub |
| Auth | NextAuth.js | Session management |
| Deploy | Railway or Fly.io | Needs persistent server (NOT serverless) |

```
⚠️ IMPORTANT: Serverless (Vercel, Netlify) CAN'T maintain WebSocket connections.
   Real-time apps need a persistent server (Railway, Fly.io, EC2, DigitalOcean).
   
   OR use managed real-time: Supabase Realtime, Ably, Pusher (they handle the server).
```

---

## Mobile App

```
WHAT: Apps on iOS and Android phones.
```

| Approach | Tools | Best For | Trade-off |
|---|---|---|---|
| **Cross-platform (JS)** | React Native + Expo | React developers. One codebase → iOS + Android. | Not 100% native feel. Some native modules need bridging. |
| **Cross-platform (Dart)** | Flutter | Beautiful custom UIs. One codebase. | New language (Dart). Smaller job market than React Native. |
| **Native iOS** | Swift + SwiftUI | Best iOS performance + Apple features | iOS only. Must build Android separately. |
| **Native Android** | Kotlin + Jetpack Compose | Best Android performance | Android only. Must build iOS separately. |
| **Web → Mobile** | PWA or Capacitor/Ionic | Quick mobile version of web app | Limited native access. Not in app stores (PWA). |

```
DECISION:
  Know React? → React Native + Expo
  Want beautiful custom UI? → Flutter
  Only iOS? → Swift
  Only Android? → Kotlin
  Quick & dirty mobile? → PWA
  
  MOST POPULAR FOR WEB DEVELOPERS: React Native + Expo (same language as web)
```

---

## Desktop App

```
WHAT: Apps for Windows, Mac, Linux.
EXAMPLES: VS Code, Slack, Discord, Figma desktop, Notion desktop
```

| Tool | What It Is | Best For |
|---|---|---|
| **Electron** | Wraps web app (HTML/CSS/JS) in a desktop window with Node.js access. | Most popular. VS Code, Slack, Discord. |
| **Tauri** | Like Electron but uses Rust backend. Much smaller bundles. | Performance-conscious. 10x smaller than Electron. Rising 2026. |

```
Electron bundle: ~150 MB (ships entire Chromium browser)
Tauri bundle:    ~10 MB (uses system webview)

PICK: Need all web features? → Electron. Need small size + performance? → Tauri.
```

---

## API / Microservices

```
WHAT: Backend-only services. No frontend. Other apps call your API.
EXAMPLES: Stripe API, SendGrid, Twilio, your internal services
```

| Layer | Tool | Why |
|---|---|---|
| Runtime | Node.js (or Go/Rust for perf) | JS ecosystem or high-performance |
| Framework | Express / Fastify / Hono | Handle HTTP requests |
| Database | PostgreSQL + Prisma | Store data |
| Cache | Redis | Speed up frequent queries |
| Queue | BullMQ + Redis | Background jobs (email, image processing) |
| Docs | Swagger / OpenAPI | Auto-generated API documentation |
| Auth | JWT or API keys | Authenticate API callers |
| Deploy | Docker + Railway or AWS Lambda | Containerized or serverless |
| Monitoring | Sentry + Datadog | Errors + performance |

---

## AI / ML App

```
WHAT: Apps using AI/ML models for features.
EXAMPLES: ChatGPT, AI writing tools, image generators, AI search
```

| Layer | Tool | Why |
|---|---|---|
| Frontend | Next.js + React | UI with streaming support |
| AI SDK | Vercel AI SDK | Stream AI responses to UI |
| LLM | OpenAI API / Anthropic API | GPT-4, Claude for text generation |
| OR | Open-source (Llama, Mistral) | Self-hosted, no API costs |
| Vector DB | Pinecone / Weaviate / pgvector | Store embeddings for semantic search (RAG) |
| Backend | Python (ML training) + Node.js (web) | Python for ML, Node for API |
| Deploy | Vercel (frontend) + Modal/Railway (AI) | Separate frontend and AI infra |

```
RAG (Retrieval Augmented Generation):
  Your data → convert to embeddings → store in vector DB
  User asks question → search vector DB for relevant context
  → send context + question to LLM → get accurate answer about YOUR data
```

---

## CMS / WordPress

```
WHAT: Content managed by non-developers through a dashboard.
```

| Approach | Stack | Best For |
|---|---|---|
| **Traditional WordPress** | PHP + MySQL + WordPress themes | Simple sites, non-technical users |
| **Headless WordPress** | WordPress (backend CMS) + Next.js (frontend) | WordPress content + modern React frontend |
| **Sanity** | Sanity (CMS) + Next.js | Developer-friendly, real-time, flexible schemas |
| **Contentful** | Contentful (CMS) + Next.js | Enterprise CMS, content teams |
| **Strapi** | Strapi (CMS) + Next.js | Open-source, self-hosted, REST/GraphQL |
| **Payload** | Payload (CMS) + Next.js | TypeScript-first, lives INSIDE Next.js |

```
PICK:
  Non-technical client manages content?  → WordPress or Sanity
  Enterprise content team?               → Contentful
  Developer wants full control?          → Payload (lives inside your Next.js app)
  Need open-source, self-hosted?         → Strapi
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Part 3: Stacks by Role

---

### Frontend Developer — Must Know

| Category | Tools |
|---|---|
| Languages | HTML, CSS, JavaScript, TypeScript |
| Framework | React (+ Next.js) |
| Styling | Tailwind CSS + shadcn/ui |
| State | useState, Context, Zustand, TanStack Query |
| Routing | Next.js App Router or React Router |
| Forms | React Hook Form + Zod |
| Testing | RTL + Vitest + Playwright |
| Tools | VS Code, Git, Chrome DevTools, Figma |
| Extras | Accessibility, performance, responsive design |

---

### Backend Developer — Must Know

| Category | Tools |
|---|---|
| Languages | JavaScript/TypeScript (or Python/Java/Go) |
| Runtime | Node.js |
| Framework | Express or Fastify |
| Database | PostgreSQL + SQL skills |
| ORM | Prisma or Drizzle |
| API | REST design, status codes, validation |
| Auth | JWT, sessions, OAuth concepts |
| Caching | Redis |
| Containers | Docker basics |
| Testing | Vitest + Supertest |
| Tools | VS Code, Git, Postman |
| Extras | System design, rate limiting, logging |

---

### Full-Stack Developer — Must Know

```
Everything from Frontend + Backend, PLUS:
  - Connecting frontend to backend (API integration)
  - Database design (schema, relationships)
  - Auth end-to-end (login flow: frontend → backend → DB)
  - Deployment (Vercel, Docker, CI/CD)
  - Environment management (dev, staging, production)

THE MOST IN-DEMAND FULL-STACK IN 2026:
  TypeScript + Next.js + Tailwind + PostgreSQL + Prisma + Vercel
```

---

### DevOps Engineer — Must Know

| Category | Tools |
|---|---|
| OS | Linux (Ubuntu, commands, file system, permissions) |
| Containers | Docker, Docker Compose, Kubernetes |
| CI/CD | GitHub Actions, Jenkins, GitLab CI |
| Cloud | AWS (EC2, S3, Lambda, RDS, CloudFront) or GCP/Azure |
| IaC | Terraform, Pulumi |
| Monitoring | Grafana, Datadog, Prometheus, PagerDuty |
| Networking | DNS, SSL, load balancers, CDN, reverse proxy |
| Scripting | Bash, Python |
| Security | Firewalls, IAM, secrets management |

---

### Mobile Developer — Must Know

| Path | Tools |
|---|---|
| React Native | React Native + Expo + TypeScript |
| Flutter | Flutter + Dart |
| iOS Native | Swift + SwiftUI + Xcode |
| Android Native | Kotlin + Jetpack Compose + Android Studio |
| All paths need | App store deployment, push notifications, offline storage, device APIs |

[↑ Back to Table of Contents](#table-of-contents)

---

# Part 4: Stacks by Company Size

---

### Solo Developer / Side Project

| Choice | Tool | Why |
|---|---|---|
| Framework | Next.js | Frontend + backend in one |
| Backend/DB/Auth | Supabase | All-in-one: Postgres + Auth + Storage + Realtime |
| Styling | Tailwind + shadcn/ui | Beautiful, fast |
| Payments | Stripe | If monetizing |
| Email | Resend | Transactional emails |
| Deploy | Vercel | Free tier, zero config |

```
TOTAL COST: $0/month on free tiers until you have real users.
ONE PERSON manages everything. Minimize tools. Ship fast.
```

---

### Startup (2-10 developers)

| Choice | Tool | Why |
|---|---|---|
| Framework | Next.js + TypeScript | Full-stack, type-safe |
| UI | Tailwind + shadcn/ui | Consistent, fast to build |
| State | Zustand + TanStack Query | Simple, effective |
| Database | PostgreSQL + Prisma | Or Supabase |
| Auth | Clerk or NextAuth | Fast setup |
| Testing | Vitest + Playwright | Catch bugs before users |
| Deploy | Vercel + GitHub Actions | Auto-deploy on push |
| Monitoring | Sentry | Catch errors in production |
| Analytics | PostHog | Understand user behavior |

```
KEY PRINCIPLE: Monolith first. Don't do microservices. Ship features fast.
               Add complexity ONLY when you hit scaling problems.
```

---

### Mid-Size (10-100 developers)

| Choice | Tool | Why |
|---|---|---|
| Framework | Next.js or separate FE/BE | May split as team grows |
| TypeScript | Strict mode | Consistency across teams |
| UI | Design system (shadcn/ui or custom) | Consistency |
| Database | PostgreSQL + Prisma + Redis | Cache for performance |
| State | Redux Toolkit (if complex) or Zustand | Team standardization |
| Testing | Vitest + RTL + Playwright + Storybook | Full coverage |
| Docker | Yes | Consistent dev environments |
| CI/CD | GitHub Actions | Automated pipeline |
| Monitoring | Sentry + Datadog | Errors + performance |
| Deploy | Vercel or AWS | Depends on scale needs |

---

### Enterprise (100+ developers)

| Choice | Tool | Why |
|---|---|---|
| Architecture | Micro-frontends or monorepo (Turborepo/Nx) | Multiple teams, independent deployment |
| TypeScript | Strict, shared configs | Absolute consistency |
| UI | MUI or Ant Design | Enterprise-grade, WCAG compliance |
| State | Redux Toolkit | Standardized, DevTools |
| Database | PostgreSQL + Redis + message queues | Scale + async processing |
| Containers | Docker + Kubernetes | Orchestration at scale |
| Cloud | AWS or Azure (full suite) | Enterprise features, compliance |
| CI/CD | Jenkins or GitHub Actions | Complex pipelines |
| Testing | Jest + RTL + Playwright + Storybook | Mandatory coverage thresholds |
| Monitoring | Datadog + PagerDuty + Sentry | 24/7 alerting |
| IaC | Terraform | Infrastructure as code |
| Auth | SSO + RBAC + custom | Enterprise security requirements |
| Compliance | SOC2, HIPAA, GDPR | Legal requirements |

[↑ Back to Table of Contents](#table-of-contents)

---

# The Ultimate Decision Guide

```
"I want to learn full-stack" → PERN or T3 Stack
"I want a job fast"          → React + Next.js + Tailwind + PostgreSQL
"I'm building a SaaS"        → T3 Stack + Stripe + Clerk
"I'm building e-commerce"    → Next.js + Stripe (or Shopify if simple)
"I'm building a blog"        → Astro + MDX (or WordPress if non-technical)
"I need real-time"           → Next.js + Socket.IO + Redis
"I want mobile app"          → React Native + Expo (if you know React)
"I'm at enterprise"          → Spring (Java) or .NET (C#) or Next.js + AWS
"I want AI features"         → Next.js + Vercel AI SDK + OpenAI API
"I'm a solo founder"         → Next.js + Supabase + Vercel + Stripe
```

[↑ Back to Table of Contents](#table-of-contents)
