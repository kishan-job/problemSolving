# Complete IT Career Paths Guide
## Every Role Explained Simply — What They Do, Salaries, Levels, AI Risk

---

# Table of Contents

- [How to Read This Guide](#how-to-read-this-guide)
- [1. Software Development](#1-software-development)
  - [Frontend Developer](#frontend-developer)
  - [Backend Developer](#backend-developer)
  - [Full-Stack Developer](#full-stack-developer)
  - [Mobile Developer](#mobile-developer)
  - [Software Architect](#software-architect)
  - [Principal / Staff Engineer](#principal--staff-engineer)
- [2. AI & Machine Learning](#2-ai--machine-learning)
  - [AI / ML Engineer](#ai--ml-engineer)
  - [AI Developer](#ai-developer)
  - [LLM Engineer](#llm-engineer)
  - [Data Scientist](#data-scientist)
  - [Data Engineer](#data-engineer)
  - [Data Analyst](#data-analyst)
- [3. Cybersecurity](#3-cybersecurity)
  - [Security Analyst](#security-analyst)
  - [Cybersecurity Engineer](#cybersecurity-engineer)
  - [Security Architect](#security-architect)
- [4. Cloud & DevOps](#4-cloud--devops)
  - [Cloud Engineer](#cloud-engineer)
  - [Cloud Architect](#cloud-architect)
  - [DevOps Engineer](#devops-engineer)
  - [Site Reliability Engineer (SRE)](#site-reliability-engineer)
  - [Platform Engineer](#platform-engineer)
- [5. Management & Leadership](#5-management--leadership)
  - [Engineering Manager](#engineering-manager)
  - [Technical Product Manager](#technical-product-manager)
  - [Director → VP → CTO Path](#director--vp--cto-path)
- [6. Specialized Roles](#6-specialized-roles)
  - [Blockchain Developer](#blockchain-developer)
  - [Game Developer](#game-developer)
  - [QA / Test Engineer](#qa--test-engineer)
  - [IoT Engineer](#iot-engineer)
- [The Master Comparison Table](#the-master-comparison-table)
- [Which Path Should YOU Choose?](#which-path-should-you-choose)

---

# How to Read This Guide

```
Each role has:
  🎯 WHAT THEY DO      — explained like you're 15 years old
  🔧 DAILY WORK        — what a typical day looks like
  📚 SKILLS NEEDED      — what you need to learn
  💰 SALARY LEVELS      — Junior → Senior → Lead (US + India)
  📈 CAREER LADDER      — how you grow in this role
  🤖 AI IMPACT          — will AI take this job?
  🏢 WHO HIRES          — types of companies
  ✅ GOOD FIT IF...      — personality match
  ❌ BAD FIT IF...       — when to avoid this path

RATING:
  🟢 GREEN  = Safe, growing, invest here
  🟠 ORANGE = Moderate risk, adapt needed
  🔴 RED    = High risk, avoid as primary career
```

---

# 1. Software Development

---

## Frontend Developer

> 🟢 Senior = Safe | 🔴 Junior = At Risk

**🎯 WHAT THEY DO:**

You build everything the user SEES and TOUCHES on a website or app. When someone opens Instagram, Twitter, or Amazon — every button, every animation, every page layout, every form — that's frontend. You turn designs into living, breathing, interactive web pages.

```
Designer gives you:  A Figma mockup (a picture of what the site should look like)
You turn it into:    A real working website with HTML, CSS, JavaScript, and React
```

**🔧 DAILY WORK:**

```
Morning:
  - Check Slack/email for any urgent bugs
  - Join daily standup (15 min meeting: what I did, what I'll do, any blockers)

Day:
  - Build new features ("Add a search filter to the products page")
  - Fix bugs ("The button doesn't work on mobile Safari")
  - Review teammates' code (Pull Request reviews)
  - Write tests for your components
  - Work with designers to clarify designs
  - Work with backend developers to agree on API format

Meetings:
  - Sprint planning (what to build this 2-week cycle)
  - Design review (look at new mockups)
  - Code review sessions
```

**📚 SKILLS NEEDED:**

```
MUST HAVE:
  HTML + CSS (Flexbox, Grid, responsive design)
  JavaScript (ES6+)
  TypeScript
  React (or Vue/Angular)
  Next.js
  Tailwind CSS
  Git + GitHub
  REST APIs (calling backend APIs)
  Browser DevTools (debugging)

SHOULD HAVE:
  Testing (React Testing Library, Vitest)
  State management (Zustand, Redux)
  Performance optimization (Core Web Vitals)
  Accessibility (ARIA, screen readers)
  CI/CD basics

NICE TO HAVE:
  Design skills (Figma)
  Animation (Framer Motion)
  SEO
  System design
```

**💰 SALARY LEVELS:**

| Level | Experience | US Salary | India ₹ LPA | India (Product Co) |
|---|---|---|---|---|
| Junior | 0-1 years | $65-90K | ₹3-6L | ₹6-12L |
| Mid | 1-3 years | $90-130K | ₹6-12L | ₹12-22L |
| Senior | 3-6 years | $130-170K | ₹12-20L | ₹22-40L |
| Staff/Lead | 6-10 years | $170-230K | ₹20-35L | ₹35-60L |
| Principal | 10+ years | $230-300K+ | ₹35-55L | ₹55-90L |

**📈 CAREER LADDER:**

```
Junior Frontend → Mid Frontend → Senior Frontend
  ↓ then choose:
  PATH A: Staff/Principal Frontend (stay technical, go deep)
  PATH B: Frontend Team Lead → Engineering Manager (manage people)
  PATH C: Full-Stack Developer (add backend skills)
  PATH D: Frontend Architect (design systems, micro-frontends)
```

**🤖 AI IMPACT:**

```
JUNIOR FRONTEND: 🔴 HIGH RISK
  - AI tools (v0, Cursor, Copilot) generate basic components
  - Simple landing pages being created by AI
  - Fewer junior openings as AI handles beginner work

SENIOR FRONTEND: 🟢 SAFE
  - Complex interactions, performance, accessibility — AI can't do this well
  - System design decisions require human judgment
  - AI makes seniors MORE productive, not obsolete
```

**✅ GOOD FIT IF:** You're visual. You like seeing immediate results. You care about user experience. You enjoy pixel-perfect work.

**❌ BAD FIT IF:** You hate CSS. You don't care what things look like. You prefer working with data/logic over UI.

[↑ Back to Table of Contents](#table-of-contents)

---

## Backend Developer

> 🟢 Senior = Safe | 🔴 Junior = At Risk

**🎯 WHAT THEY DO:**

You build everything BEHIND the scenes — the server logic, databases, and APIs that power the application. When a user clicks "Login," your code checks the password. When they click "Buy," your code processes the payment and updates inventory. Users never see your code, but nothing works without it.

```
Frontend sends:    "User wants to buy Product #42"
You handle:        Check inventory → Process payment → Update database
                   → Send confirmation email → Return success response
```

**🔧 DAILY WORK:**

```
  - Design and build API endpoints
  - Write database queries and optimize slow ones
  - Implement authentication and authorization
  - Handle file uploads, email sending, payment processing
  - Set up background jobs (send email after order, process images)
  - Monitor server logs and fix errors
  - Write tests for business logic
  - Work with frontend team on API contracts
  - Review code and database designs
```

**📚 SKILLS NEEDED:**

```
MUST HAVE:
  Node.js + Express/Fastify (or Python/Java/Go)
  TypeScript
  PostgreSQL + SQL
  REST API design
  Git + GitHub
  Authentication (JWT, sessions, OAuth)
  ORM (Prisma or Drizzle)

SHOULD HAVE:
  Docker
  Redis (caching)
  Message queues (BullMQ)
  Testing (unit + integration)
  Cloud basics (AWS or Vercel)
  CI/CD

NICE TO HAVE:
  System design (scaling, microservices)
  GraphQL
  Kubernetes
  Monitoring (Sentry, Datadog)
```

**💰 SALARY LEVELS:**

| Level | Experience | US Salary | India ₹ LPA | India (Product Co) |
|---|---|---|---|---|
| Junior | 0-1 years | $70-100K | ₹4-7L | ₹7-14L |
| Mid | 1-3 years | $100-140K | ₹7-15L | ₹14-25L |
| Senior | 3-6 years | $140-180K | ₹15-25L | ₹25-45L |
| Staff/Lead | 6-10 years | $180-250K | ₹25-40L | ₹40-65L |
| Principal | 10+ years | $250-320K+ | ₹40-60L | ₹60-95L |

**📈 CAREER LADDER:**

```
Junior Backend → Mid Backend → Senior Backend
  ↓ then choose:
  PATH A: Staff/Principal Backend Engineer (deep expertise)
  PATH B: Backend Team Lead → Engineering Manager
  PATH C: Full-Stack Developer (add frontend)
  PATH D: Software Architect (design entire systems)
  PATH E: DevOps/Cloud (infrastructure focus)
```

**🤖 AI IMPACT:**

```
JUNIOR: 🔴 HIGH RISK — AI generates basic CRUD APIs, simple routes
SENIOR: 🟢 SAFE — System design, security decisions, scaling strategies, 
        debugging production issues — AI can't do these well
```

**✅ GOOD FIT IF:** You like logic and problem-solving. You enjoy working with data. You prefer behind-the-scenes work over visual design.

**❌ BAD FIT IF:** You need to see visual results immediately. You want to work closely with designers. You dislike debugging server logs.

[↑ Back to Table of Contents](#table-of-contents)

---

## Full-Stack Developer

> 🟢 Most versatile role. Highest demand.

**🎯 WHAT THEY DO:**

You build BOTH the frontend (what users see) AND the backend (server, database, APIs). You can single-handedly build an entire application from scratch. You're the Swiss Army knife of developers — companies love you because you can work on anything.

```
You build:  The login page (frontend)
            + The login API (backend)
            + The user database table (database)
            + The deployment (DevOps)
            = The entire feature, end to end
```

**🔧 DAILY WORK:**

```
  - Build complete features (frontend + backend together)
  - Design database schemas for new features
  - Create APIs and connect them to the UI
  - Handle deployments
  - Fix bugs anywhere in the stack
  - You switch context a LOT (React in the morning, SQL in the afternoon)
```

**📚 SKILLS NEEDED:**

```
MUST HAVE:
  Everything from Frontend + Backend:
  React + Next.js + TypeScript + Tailwind
  Node.js + Express/Fastify (or Next.js API Routes)
  PostgreSQL + Prisma
  REST API design
  Git + GitHub
  Auth (JWT, NextAuth)
  Deployment (Vercel)

SHOULD HAVE:
  Docker, Redis, Testing, CI/CD, Cloud basics

THE MOST IN-DEMAND FULL-STACK COMBO IN 2026:
  TypeScript + Next.js + Tailwind + PostgreSQL + Prisma + Vercel
```

**💰 SALARY LEVELS:**

| Level | Experience | US Salary | India ₹ LPA | India (Product Co) |
|---|---|---|---|---|
| Junior | 0-1 years | $70-100K | ₹4-8L | ₹8-15L |
| Mid | 1-3 years | $100-150K | ₹8-15L | ₹15-28L |
| Senior | 3-6 years | $150-200K | ₹15-28L | ₹28-50L |
| Staff/Lead | 6-10 years | $200-270K | ₹28-45L | ₹45-70L |

**📈 CAREER LADDER:**

```
Junior Full-Stack → Mid → Senior Full-Stack
  ↓ then choose:
  PATH A: Staff Engineer → Principal (deep technical)
  PATH B: Engineering Manager (lead a team)
  PATH C: Software Architect (design systems at scale)
  PATH D: CTO of a startup (you built everything, now lead tech)
  PATH E: Freelance/Consultant ($100-200/hr)
```

**🤖 AI IMPACT: 🟢 SAFE (senior level)**

```
AI makes full-stack developers MORE powerful, not obsolete.
You can now build in 1 week what used to take 1 month.
The "solo developer building a SaaS" is a real career path now because
AI tools amplify one person's output massively.

HOWEVER: Junior full-stack roles are shrinking.
Companies prefer fewer senior devs with AI tools over many juniors.
```

**✅ GOOD FIT IF:** You like variety. You want to build complete products. You don't want to be limited to just frontend or just backend. Great for startups and freelancing.

**❌ BAD FIT IF:** You prefer going extremely deep in one area. You get stressed by context-switching between different technologies.

[↑ Back to Table of Contents](#table-of-contents)

---

## Mobile Developer

> 🟠 Moderate — cross-platform is safe, native-only is shrinking

**🎯 WHAT THEY DO:**

You build apps that run on phones (iOS and Android). Every app on your phone — Instagram, Uber, WhatsApp, Zomato — was built by mobile developers. You handle touch gestures, offline storage, push notifications, camera access, and making sure the app feels smooth.

**📚 TWO PATHS:**

```
PATH A: Cross-Platform (ONE codebase → both iOS + Android)
  React Native + Expo    ← if you know React (most popular for React devs)
  Flutter + Dart         ← Google's framework (beautiful custom UIs)

PATH B: Native (SEPARATE codebase per platform)
  iOS:     Swift + SwiftUI      ← Apple only
  Android: Kotlin + Jetpack Compose  ← Google only
```

**💰 SALARY LEVELS:**

| Level | Experience | US Salary | India ₹ LPA | India (Product Co) |
|---|---|---|---|---|
| Junior | 0-1 years | $70-100K | ₹4-8L | ₹8-15L |
| Mid | 1-3 years | $100-140K | ₹8-16L | ₹16-28L |
| Senior | 3-6 years | $140-180K | ₹16-28L | ₹28-45L |
| Lead | 6+ years | $180-230K | ₹28-40L | ₹40-60L |

**🤖 AI IMPACT: 🟠**

```
Cross-platform (React Native, Flutter): 🟢 Safe — still needs human judgment
Native-only (Swift/Kotlin): 🟠 Shrinking — companies prefer cross-platform to save money
AI RISK: AI generates basic screens but can't handle device APIs, performance, offline storage
```

**✅ GOOD FIT IF:** You love apps. You want to build something people carry in their pocket. You like React and want to extend to mobile.

[↑ Back to Table of Contents](#table-of-contents)

---

## Software Architect

> 🟢 One of the safest, highest-paid technical roles

**🎯 WHAT THEY DO:**

You design the ENTIRE system before anyone writes a single line of code. You decide: Which technologies to use? How should the database be structured? How will 10 million users be handled? How do the microservices communicate? You're the architect of a building — you draw the blueprint, others construct it.

```
A developer asks: "How should I build this feature?"
You answer:       "Use this database design, this API pattern, this caching strategy,
                   and here's why — because when we scale to 1M users, this approach
                   won't break."
```

**🔧 DAILY WORK:**

```
  - Design system architecture for new features/products
  - Create technical design documents
  - Review critical pull requests
  - Meet with product managers to understand requirements
  - Evaluate new technologies ("Should we switch from REST to GraphQL?")
  - Mentor senior developers
  - Handle cross-team technical decisions
  - Plan for scalability, reliability, and security
  - Present architecture decisions to leadership
```

**📚 SKILLS NEEDED:**

```
  5-10+ years of development experience (you must have BUILT things before designing them)
  System design (load balancing, caching, message queues, microservices)
  Multiple languages/frameworks (breadth, not just depth)
  Database design (SQL + NoSQL, when to use which)
  Cloud architecture (AWS/GCP/Azure)
  Security best practices
  Performance optimization
  Communication (explain complex ideas simply to executives and juniors)
  Trade-off analysis ("This approach is faster but costs more — here's why it's worth it")
```

**💰 SALARY LEVELS:**

| Level | Experience | US Salary | India ₹ LPA | India (Product Co) |
|---|---|---|---|---|
| Junior Architect | 5-8 years | $160-200K | ₹25-40L | ₹35-55L |
| Senior Architect | 8-12 years | $200-280K | ₹35-55L | ₹55-80L |
| Chief Architect | 12+ years | $280-350K+ | ₹50-75L | ₹75L-1Cr+ |

**🤖 AI IMPACT: 🟢 VERY SAFE**

```
AI can generate code, but it CANNOT:
  - Decide whether to use microservices or monolith
  - Design a system that handles 10M concurrent users
  - Make trade-off decisions (cost vs speed vs reliability)
  - Navigate organizational politics ("Team A wants X, Team B wants Y")
  - Take ACCOUNTABILITY for architecture decisions

This role is JUDGMENT-based, not CODE-based. AI assists, never replaces.
```

**✅ GOOD FIT IF:** You love the big picture. You enjoy design more than implementation. You're good at communicating complex ideas. You have years of development experience.

**❌ BAD FIT IF:** You prefer writing code over drawing diagrams. You don't like meetings. You have less than 5 years of experience (you need to build before you can design).

[↑ Back to Table of Contents](#table-of-contents)

---

## Principal / Staff Engineer

> 🟢 Highest individual contributor role. Very safe.

**🎯 WHAT THEY DO:**

The most senior engineer who DOESN'T manage people. You solve the hardest technical problems in the company. You set technical direction across multiple teams. You're called in when nobody else can figure it out.

```
DIFFERENCE FROM ARCHITECT:
  Architect:  Designs systems on paper. More meetings, more documents.
  Principal:  Gets hands dirty. Writes the most critical code. Solves the hardest bugs.
              Also designs, but focuses more on implementation.
```

**💰 SALARY:**

| Level | US Salary | India (Product Co) |
|---|---|---|
| Staff Engineer | $180-250K | ₹35-60L |
| Senior Staff | $250-320K | ₹55-80L |
| Principal | $300-400K+ | ₹75L-1.2Cr |
| Distinguished (rare) | $400-600K+ | ₹1Cr+ |

**🤖 AI IMPACT: 🟢 VERY SAFE** — You solve problems AI can't even understand yet.

[↑ Back to Table of Contents](#table-of-contents)

---

# 2. AI & Machine Learning

---

## AI / ML Engineer

> 🟢 Highest demand role in tech. You BUILD the AI.

**🎯 WHAT THEY DO:**

You build, train, and deploy machine learning models. You teach computers to recognize faces, translate languages, recommend movies, detect fraud, or drive cars. You work with massive datasets and mathematical models to make machines "smart."

```
SIMPLE EXAMPLE:
  Netflix wants to recommend movies.
  You collect data: what users watched, liked, skipped.
  You build a model that learns patterns: "People who liked X also liked Y."
  You train it on millions of data points.
  You deploy it so it runs in real-time for every Netflix user.
  That recommendation you see? Your model made it.
```

**🔧 DAILY WORK:**

```
  - Collect and clean training data (80% of the work!)
  - Build and train ML models (neural networks, decision trees, etc.)
  - Evaluate model accuracy (is it getting better or worse?)
  - Optimize models for speed and size (make it run on a phone)
  - Deploy models to production servers
  - Monitor model performance (is it drifting? getting less accurate?)
  - Read research papers to stay current
  - Collaborate with data engineers (data pipelines) and product team (what to build)
```

**📚 SKILLS NEEDED:**

```
MUST HAVE:
  Python (NumPy, Pandas, Scikit-learn)
  Math (linear algebra, statistics, probability, calculus)
  Machine Learning algorithms (regression, classification, clustering, neural networks)
  Deep Learning frameworks (PyTorch or TensorFlow)
  Data processing (cleaning, feature engineering)
  SQL (for data extraction)

SHOULD HAVE:
  MLOps (deploying models: Docker, MLflow, AWS SageMaker)
  Cloud (AWS, GCP)
  NLP (Natural Language Processing) — for text AI
  Computer Vision — for image/video AI
  LLMs (GPT, Llama — fine-tuning, RAG, prompt engineering)

EDUCATION:
  Most roles want: Master's or PhD in CS/Math/Statistics
  BUT: Self-taught with strong portfolio + Kaggle competitions CAN break in
```

**💰 SALARY LEVELS:**

| Level | Experience | US Salary | India ₹ LPA | India (Product Co) |
|---|---|---|---|---|
| Junior ML Engineer | 0-2 years | $100-140K | ₹8-16L | ₹16-28L |
| Mid ML Engineer | 2-5 years | $140-200K | ₹16-30L | ₹30-50L |
| Senior ML Engineer | 5-8 years | $200-280K | ₹30-50L | ₹50-80L |
| Staff / Lead ML | 8+ years | $280-380K+ | ₹50-75L | ₹75L-1.2Cr |

**🤖 AI IMPACT: 🟢 VERY SAFE**

```
You BUILD the AI. AI can't build itself (yet).
AutoML tools handle basic model selection, but:
  - Custom models for specific business problems = human
  - Debugging why a model fails = human
  - Ethical AI decisions = human
  - Novel research = human

This is like asking "will robots replace the robot factory workers?"
```

**✅ GOOD FIT IF:** You love math. You enjoy experimentation (try, fail, improve). You're comfortable with uncertainty (models don't always work). You like research.

**❌ BAD FIT IF:** You hate math/statistics. You want immediate visible results (models take weeks to train). You prefer building UIs.

[↑ Back to Table of Contents](#table-of-contents)

---

## AI Developer

> 🟢 Fastest-growing entry point into AI

**🎯 WHAT THEY DO:**

Unlike ML Engineers who BUILD models from scratch, AI Developers INTEGRATE existing AI models (OpenAI, Claude, Llama) into products. You add "smart" features to apps — chatbots, text summarization, image generation, smart search, etc.

```
ML Engineer:   Trains the GPT model from scratch (needs PhD-level math)
AI Developer:  Uses the GPT API to add a chatbot to your company's website (needs dev skills)
```

**📚 SKILLS NEEDED:**

```
  JavaScript/TypeScript or Python
  React + Next.js (for building the AI-powered UI)
  OpenAI API / Anthropic API / Hugging Face
  Prompt engineering (getting AI to give good answers)
  RAG (Retrieval-Augmented Generation — AI + your company's documents)
  Vector databases (Pinecone, pgvector)
  Vercel AI SDK (streaming AI responses)
  Basic understanding of how LLMs work (tokens, context windows, temperature)
```

**💰 SALARY LEVELS:**

| Level | Experience | US Salary | India ₹ LPA |
|---|---|---|---|
| Junior | 0-2 years | $90-130K | ₹8-18L |
| Mid | 2-4 years | $130-180K | ₹18-35L |
| Senior | 4+ years | $180-250K | ₹35-60L |

**🤖 AI IMPACT: 🟢 SAFE** — Ironic but true: AI creates more demand for people who can integrate AI.

**✅ GOOD FIT IF:** You're a web developer who wants to add AI skills. Easiest bridge from full-stack to AI. No PhD needed.

[↑ Back to Table of Contents](#table-of-contents)

---

## LLM Engineer

> 🟢 Brand new role. Extremely high demand. Premium pay.

**🎯 WHAT THEY DO:**

Specializes in Large Language Models (ChatGPT, Claude, Llama). Fine-tunes models for specific use cases, builds RAG pipelines, optimizes prompt chains, ensures AI outputs are safe and accurate.

```
AI Developer:  Calls the OpenAI API
LLM Engineer:  Fine-tunes a model on company data, builds evaluation pipelines,
               sets up guardrails, optimizes token costs, deploys custom models
```

**💰 SALARY:** $150-280K (US) | ₹25-70L (India Product Co)

**🤖 AI IMPACT: 🟢 VERY SAFE** — This role didn't exist 2 years ago and is the fastest-growing job in tech.

[↑ Back to Table of Contents](#table-of-contents)

---

## Data Scientist

> 🟢 Safe but evolving. Must learn AI tools.

**🎯 WHAT THEY DO:**

You analyze data to find patterns, make predictions, and help businesses make smarter decisions. You answer questions like: "Which customers are likely to cancel?" "What price should we set?" "Is this transaction fraudulent?"

```
Business asks:  "Why are users leaving after 30 days?"
You do:         Analyze user behavior data → find patterns → build prediction model
You deliver:    "Users who don't complete onboarding in 3 days have 70% churn rate.
                 Here's a model that predicts who's at risk."
```

**💰 SALARY LEVELS:**

| Level | Experience | US Salary | India ₹ LPA |
|---|---|---|---|
| Junior | 0-2 years | $90-120K | ₹6-15L |
| Mid | 2-5 years | $120-170K | ₹15-30L |
| Senior | 5+ years | $170-230K | ₹30-55L |

**🤖 AI IMPACT: 🟢 but evolving**

```
Basic analysis (make a chart, run a query) → 🔴 being automated by AI tools
Advanced modeling + business strategy → 🟢 safe, AI can't replace business judgment
WHAT TO DO: Move from "make charts" to "make decisions." Be the person who
            interprets what the data MEANS, not just what it SHOWS.
```

[↑ Back to Table of Contents](#table-of-contents)

---

## Data Engineer

> 🟢 Safe. Someone must build the data infrastructure.

**🎯 WHAT THEY DO:**

You build the pipes that move data from where it's generated to where it's analyzed. If Data Scientists are chefs, you're the one who builds the kitchen, installs the plumbing, and makes sure ingredients arrive fresh every day.

**💰 SALARY:** Junior $90-120K → Senior $170-220K (US) | ₹6-15L → ₹25-45L (India)

**Skills:** Python, SQL, Spark, Airflow, cloud (AWS/GCP), ETL pipelines, data warehouses (Snowflake, BigQuery)

---

## Data Analyst

> 🟠 Entry-level roles at risk. Evolve or be replaced.

**🎯 WHAT THEY DO:**

You query databases, create dashboards, and answer business questions with data. "How many users signed up last month?" "Which products sell best on weekends?"

**💰 SALARY:** $60-90K (Junior US) → $120-150K (Senior) | ₹4-8L → ₹15-25L (India)

**🤖 AI IMPACT: 🟠 MODERATE TO HIGH**

```
Basic SQL queries + dashboards → AI tools (ChatGPT, Metabase AI) can do this now
SURVIVAL: Learn Python, learn ML basics, become "Analytics Engineer" instead of "Data Analyst"
         Move from "answering questions" to "asking the RIGHT questions"
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 3. Cybersecurity

> 🟢 ALL roles are safe. Breaches don't stop for recessions or AI.

---

## Security Analyst

**🎯 WHAT THEY DO:**

You're the security guard watching the monitors. You monitor systems for threats, investigate suspicious activity, and respond to incidents. When something looks wrong — an unusual login from another country, a spike in traffic — you investigate.

**💰 SALARY:** $80-110K (Junior) → $140-170K (Senior) | ₹5-10L → ₹18-30L (India)

**Entry Path:** CompTIA Security+ certification → SOC Analyst → Senior Analyst

---

## Cybersecurity Engineer

**🎯 WHAT THEY DO:**

You BUILD the security defenses. Firewalls, encryption, intrusion detection systems, vulnerability scanning, penetration testing ("ethical hacking" — you try to break in to find weaknesses before real hackers do).

**💰 SALARY:**

| Level | US Salary | India ₹ LPA |
|---|---|---|
| Junior | $100-130K | ₹8-15L |
| Mid | $130-170K | ₹15-28L |
| Senior | $170-220K | ₹28-45L |

---

## Security Architect

**🎯 WHAT THEY DO:**

You design the ENTIRE security strategy for an organization. Which encryption? Which access control model? How to secure the cloud infrastructure? How to comply with regulations (GDPR, HIPAA, SOC2)?

**💰 SALARY:** $180-280K (US) | ₹35-70L (India Product Co)

**🤖 AI IMPACT: 🟢 VERY SAFE** — Hackers use AI → companies need MORE security, not less.

```
WHY CYBERSECURITY IS RECESSION-PROOF:
  - A data breach costs $4.5 million on average
  - Regulations REQUIRE security spending (can't cut it)
  - Hackers don't take recessions off
  - AI creates NEW attack vectors that need NEW defenses
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 4. Cloud & DevOps

---

## Cloud Engineer

> 🟢 Senior safe | 🟠 Junior basic tasks being automated

**🎯 WHAT THEY DO:**

You build and maintain the cloud infrastructure where applications run. You set up servers, databases, networking, and storage on cloud platforms (AWS, Azure, GCP). You make sure everything is secure, scalable, and cost-effective.

**💰 SALARY:** $100-130K (Junior) → $170-220K (Senior) | ₹8-15L → ₹25-45L (India)

**Skills:** AWS/Azure/GCP, Terraform, Linux, networking, Docker, CI/CD

---

## Cloud Architect

> 🟢 Very safe. High pay. Strategic role.

**🎯 WHAT THEY DO:**

You design the entire cloud strategy. Which cloud provider? Which services? How to migrate from on-premise? How to optimize costs (cloud bills can be $100K+/month)?

**💰 SALARY:** $170-270K (US) | ₹30-60L (India)

**Most valued cert:** AWS Solutions Architect Professional

---

## DevOps Engineer

> 🟢 Safe. Automation creates more DevOps work, not less.

**🎯 WHAT THEY DO:**

You're the bridge between developers and operations. You automate everything: building code, running tests, deploying to production, monitoring, scaling. Your goal: developers push code → it automatically tests, builds, and deploys — no manual steps.

```
WITHOUT DEVOPS:
  Developer finishes code → emails it to ops team → ops manually copies to server
  → prays nothing breaks → takes 2 weeks per deploy

WITH DEVOPS:
  Developer pushes code → CI/CD automatically tests → builds → deploys in 10 minutes
  → monitoring alerts if anything breaks → auto-rollback if needed
```

**💰 SALARY:**

| Level | US Salary | India ₹ LPA |
|---|---|---|
| Junior | $100-130K | ₹8-16L |
| Mid | $130-180K | ₹16-30L |
| Senior | $180-230K | ₹30-50L |

**Skills:** Docker, Kubernetes, CI/CD (GitHub Actions), Terraform, AWS, Linux, monitoring (Grafana, Datadog)

---

## Site Reliability Engineer

> 🟢 Very safe. Google invented this role. Premium pay.

**🎯 WHAT THEY DO:**

You keep systems running 99.99% of the time. When Netflix is up at 3 AM — that's because of SREs. You're the engineer who gets paged when something breaks, figures out why, fixes it, and then builds automation so it never happens again.

**💰 SALARY:** $140-250K (US) | ₹20-55L (India)

**Skills:** Everything from DevOps + deep Linux, networking, performance tuning, incident management

---

## Platform Engineer

> 🟢 Fastest-growing infrastructure role.

**🎯 WHAT THEY DO:**

You build internal tools and platforms that make OTHER developers productive. Developer portals, automated deployment pipelines, internal CLIs, self-service infrastructure.

**💰 SALARY:** $130-220K (US) | ₹18-45L (India)

[↑ Back to Table of Contents](#table-of-contents)

---

# 5. Management & Leadership

---

## Engineering Manager

> 🟢 AI-proof. Can't automate people management.

**🎯 WHAT THEY DO:**

You manage a team of 5-10 developers. You don't write code full-time anymore (maybe 20%). Your job: hire great people, help them grow, remove blockers, ensure projects ship on time, shield team from unnecessary meetings.

```
INDIVIDUAL CONTRIBUTOR: "I built this feature"
ENGINEERING MANAGER:     "I built this TEAM that builds features"
```

**💰 SALARY:**

| Level | US Salary | India ₹ LPA |
|---|---|---|
| EM (first-line) | $170-230K | ₹25-50L |
| Senior EM | $230-290K | ₹40-70L |
| Director | $260-350K | ₹55-90L |
| VP Engineering | $350-500K+ | ₹80L-1.5Cr |
| CTO | $400K-1.2M+ | ₹1-3Cr+ |

**✅ GOOD FIT IF:** You care about people more than code. You enjoy mentoring. You're a good communicator. You have patience.

**❌ BAD FIT IF:** You just want to write code. You avoid confrontation. You don't enjoy meetings.

---

## Technical Product Manager

> 🟢 Safe. Requires human judgment about what to build.

**🎯 WHAT THEY DO:**

You decide WHAT to build and WHY. You don't write code — you talk to customers, analyze data, prioritize features, write requirements, and work with engineers to deliver the right product.

```
Engineers: HOW to build it
Product Manager: WHAT to build and WHY
Designer: How it should LOOK and FEEL
```

**💰 SALARY:** $130-220K (US) | ₹20-50L (India)

---

## Director → VP → CTO Path

```
Director of Engineering ($250-350K / ₹50-90L):
  Manages 3-5 engineering managers. 20-50 engineers total.
  Sets technical strategy for a department.

VP of Engineering ($350-500K+ / ₹80L-1.5Cr):
  Manages directors. 50-200+ engineers.
  Owns engineering budget, hiring, technology decisions.

CTO ($400K-1.2M+ / ₹1-3Cr+):
  The top technology leader. Reports to CEO.
  Sets tech vision for the entire company.
  At startups: often also writes code.
  At large companies: pure strategy + leadership.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# 6. Specialized Roles

---

## Blockchain Developer

> 🟠 Cyclical. Tied to crypto market.

**🎯 WHAT THEY DO:** Build smart contracts, decentralized apps (dApps), and blockchain systems. Solidity, Ethereum, Solana.

**💰:** $120-200K (US) | ₹15-40L (India)

**Risk:** Pay swings wildly with crypto market. Bull market = huge salaries. Bear market = layoffs.

---

## Game Developer

> 🟠 Passion-driven. Lower pay than other dev roles. Competitive.

**🎯 WHAT THEY DO:** Build video games. Unity (C#), Unreal Engine (C++), Godot.

**💰:** $70-140K (US) | ₹5-20L (India) — Lower than other dev roles for same experience level.

**Reality check:** Very competitive. Long hours ("crunch"). Lower pay. But if you love games, nothing else compares.

---

## QA / Test Engineer

> 🔴 Manual testing dying | 🟠 Automation surviving

| Type | AI Risk | Salary US | India |
|---|---|---|---|
| Manual QA Tester | 🔴 Being replaced by AI | $50-80K | ₹3-8L |
| Automation QA Engineer | 🟠 Still needed but shrinking | $90-140K | ₹8-22L |
| SDET (Software Dev in Test) | 🟢 Safe — writes testing frameworks | $120-170K | ₹15-30L |

**If you're in QA:** Move to automation → SDET → or transition to DevOps/SRE.

---

## IoT Engineer

> 🟢 Safe. Physical devices need humans.

**🎯 WHAT THEY DO:** Build software for physical devices — smart home, industrial sensors, wearables, medical devices.

**💰:** $100-170K (US) | ₹10-30L (India)

**Skills:** C/C++, Python, embedded systems, networking protocols, cloud (AWS IoT)

[↑ Back to Table of Contents](#table-of-contents)

---

# The Master Comparison Table

| Role | US Salary Range | India ₹ LPA Range | AI Risk | Recession Risk | Rating | Entry Barrier |
|---|---|---|---|---|---|---|
| AI/ML Engineer | $140-280K | ₹16-80L | 🟢 Very Safe | 🟢 Low | 🟢 | High (Math + CS) |
| LLM Engineer | $150-280K | ₹25-70L | 🟢 Very Safe | 🟢 Low | 🟢 | Medium-High |
| Security Architect | $180-280K | ₹35-70L | 🟢 Very Safe | 🟢 Very Low | 🟢 | High (Years) |
| Software Architect | $160-280K | ₹30-70L | 🟢 Very Safe | 🟢 Low | 🟢 | High (Years) |
| Principal Engineer | $180-400K | ₹40-1.2Cr | 🟢 Very Safe | 🟢 Low | 🟢 | Very High |
| DevOps/SRE | $120-250K | ₹12-55L | 🟢 Safe | 🟢 Low | 🟢 | Medium |
| Cloud Architect | $170-270K | ₹30-60L | 🟢 Safe | 🟢 Low | 🟢 | Medium-High |
| Eng. Manager | $170-290K | ₹25-70L | 🟢 Very Safe | 🟢 Low | 🟢 | Medium (People skills) |
| Full-Stack (Senior) | $150-200K | ₹18-50L | 🟢 Safe | 🟠 Medium | 🟢 | Medium |
| AI Developer | $90-250K | ₹8-60L | 🟢 Safe | 🟢 Low | 🟢 | Low-Medium |
| Data Scientist | $90-230K | ₹6-55L | 🟢 Mostly Safe | 🟠 Medium | 🟢 | Medium-High |
| Data Engineer | $90-220K | ₹6-45L | 🟢 Safe | 🟠 Medium | 🟢 | Medium |
| Cybersecurity Eng. | $100-220K | ₹8-45L | 🟢 Safe | 🟢 Very Low | 🟢 | Medium |
| Platform Engineer | $130-220K | ₹18-45L | 🟢 Safe | 🟢 Low | 🟢 | Medium |
| Product Manager | $130-220K | ₹20-50L | 🟢 Safe | 🟠 Medium | 🟢 | Medium |
| Mobile Developer | $100-180K | ₹10-40L | 🟠 Moderate | 🟠 Medium | 🟠 | Medium |
| Full-Stack (Junior) | $65-100K | ₹4-15L | 🔴 High | 🔴 High | 🔴 | Low |
| Frontend (Junior) | $65-90K | ₹3-12L | 🔴 High | 🔴 High | 🔴 | Low |
| Data Analyst | $60-120K | ₹4-18L | 🟠 Moderate | 🟠 Medium | 🟠 | Low |
| Blockchain Dev | $120-200K | ₹15-40L | 🟠 Moderate | 🔴 High (cyclical) | 🟠 | Medium |
| Game Developer | $70-150K | ₹5-25L | 🟠 Moderate | 🔴 High | 🟠 | Medium |
| QA (Manual) | $50-80K | ₹3-8L | 🔴 Dying | 🔴 High | 🔴 | Low |
| Sys Admin | $70-110K | ₹5-15L | 🔴 Dying | 🔴 High | 🔴 | Low |

[↑ Back to Table of Contents](#table-of-contents)

---

# Which Path Should YOU Choose?

```
"I love building things users can see and touch"
  → Frontend → Full-Stack → Senior/Architect

"I love logic, data, and behind-the-scenes work"
  → Backend → Full-Stack → Software Architect

"I love math and experiments"
  → Data Scientist → ML Engineer → AI Architect

"I want the fastest path to high salary"
  → Full-Stack → AI Developer → LLM Engineer (add AI to your web skills)

"I want maximum job security"
  → Cybersecurity → never unemployed

"I want to manage people someday"
  → Senior Developer → Engineering Manager → Director → VP → CTO

"I want to work for myself"
  → Full-Stack → Freelance → SaaS founder

"I'm in India and want maximum pay"
  → Target product companies (not service)
  → Specialize in AI, Cloud, or Security
  → Or: remote work for US companies (₹50L-1Cr living in India)

"I don't know what I want yet"
  → Start with Full-Stack (TypeScript + Next.js)
  → It's the most versatile starting point
  → You can specialize LATER once you know what you enjoy
```

[↑ Back to Table of Contents](#table-of-contents)
