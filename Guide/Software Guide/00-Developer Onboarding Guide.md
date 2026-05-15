# Developer Onboarding Guide
### Complete Step by Step for Any Company or Project

---

## 📋 Document Structure

```
TOP — Before Everything
   → What is this document
   → How to use this guide
   → Company types
   → Project types
   → Team types
   → How onboarding differs

PART 1 — Day 1: Access and Tools Setup
   → Get all accesses list
   → Tools to install
   → Accounts to setup
   → First day checklist

PART 2 — Day 1: Understand Organization
   → Company structure
   → Your team structure
   → Communication tools setup
   → Meeting cadence
   → Who is who (roles)

PART 2B — Day 1: VPN Setup
   → Why VPN needed
   → How to setup
   → When to connect

PART 2C — Day 1: Node Version Management
   → Check required Node version
   → nvm setup
   → Switch versions per project

PART 3 — Day 2: Understand Repository
   → How many repos given?
   → Single repo identification
   → Monorepo identification
   → Multirepo identification
   → What changes based on repo type

PART 4 — Day 2: Understand Architecture
   → Step 1: Ask tech lead
   → Step 2: Read README
   → Step 3: Folder structure
   → Step 4: Package.json scripts
   → Step 5: Config files
   → Single App identification
   → Micro Frontend identification
   → What changes based on architecture

PART 5 — Day 2: Find Tech Stack
   → Package manager
   → Framework
   → Build tool
   → Language
   → State management
   → CSS approach
   → Component library
   → API layer
   → Auth
   → Testing tools
   → CI/CD
   → Code quality tools
   → Analytics/Monitoring

PART 5B — Day 2: Find Environments
   → What environments exist
   → Where to find URLs
   → Branch to environment mapping
   → Environment names by company type
   → What to ask tech lead

PART 5C — Day 2: API Documentation
   → Swagger/OpenAPI
   → Postman collection
   → How to test APIs locally
   → Auth for API testing

PART 5D — Day 2: Code Standards
   → Naming conventions
   → Folder structure conventions
   → TypeScript rules
   → Where to find standards
   → Unwritten rules

PART 6 — Day 3: Understand Codebase
   → Read files in order
   → Root level files
   → App level files
   → Layout and providers
   → Find state management files
   → Find API layer files
   → Find your feature area
   → Search strategies in VS Code
   → Draw diagrams

PART 6B — Day 3: Understanding Existing Tests
   → Where are tests
   → How to run tests
   → Test coverage requirement
   → Test naming conventions
   → Writing tests following patterns

PART 6C — Day 3: Design System and Figma
   → How to read Figma as developer
   → Extract CSS values
   → Design tokens
   → Storybook URL
   → Who is the designer

PART 7 — Day 3: Understand Project Management
   → Jira reading order
   → Confluence reading order
   → Agile ceremonies

PART 8 — Day 4: Talk to Team
   → Talk to PO
   → Talk to tech lead
   → Talk to scrum master
   → Talk to senior developer
   → Talk to QA engineer

PART 8B — Day 4: KT Sessions and Pair Programming
   → Request KT sessions
   → What to cover in KT
   → Pair programming
   → Shadowing

PART 9 — Day 4: Setup Local Environment
   → Clone steps per repo type
   → Install dependencies
   → Setup .env file
   → Run the project
   → Observe current behavior

PART 9B — Day 4: Debugging Setup
   → VS Code debugger
   → React DevTools
   → Redux DevTools
   → Network tab
   → Debug API calls

PART 10 — Day 5: Before First Code
   → Must have checklist
   → Get Figma access
   → Check existing code
   → Get tech lead approval
   → Create feature branch

PART 11 — Week 1: First Code
   → Git workflow
   → Branch naming
   → Commit message format
   → PR process
   → Code review process

PART 11B — Security Standards
   → No secrets in code
   → No console.log in prod
   → Dependency checks
   → Code scanning

PART 11C — Performance and Accessibility
   → Performance budgets
   → Lighthouse scores
   → WCAG standards
   → Core Web Vitals

PART 12 — Daily Routine
   → Morning routine
   → During day
   → End of day
   → Weekly habits

PART 12B — Deployment Process
   → How deployment works
   → Who triggers it
   → How to monitor
   → Rollback process
   → Change management

PART 12C — Incident and Bug Process
   → How bugs reported
   → Severity levels
   → Hotfix process
   → On-call rotation

PART 13 — First Month Goals
   → Week 1 targets
   → Week 2 targets
   → Week 3 targets
   → Week 4 targets

PART 14 — Key Rules
   → Never do list
   → Always do list
   → Ask before assuming

PART 13B — Additional Technical Topics
   → Docker setup
   → Feature flags
   → Database/Backend knowledge
   → Dependency management
   → Browser compatibility
   → Internationalization (i18n)
   → Error handling patterns
   → Mock vs real data
   → Tech debt and TODOs
   → Code review (giving reviews)

PART 15 — Glossary
   → Repo types
   → Architecture types
   → Tech stack terms
   → Git terms
   → Agile/SAFe terms
   → Environment terms
```

---

## 📌 Navigation

- [Before Everything](#before-everything)
- [Part 1 — Day 1: Access and Tools Setup](#part-1--day-1-access-and-tools-setup)
- [Part 2 — Day 1: Understand Organization](#part-2--day-1-understand-organization)
- [Part 2B — Day 1: VPN Setup](#part-2b--day-1-vpn-setup)
- [Part 2C — Day 1: Node Version Management](#part-2c--day-1-node-version-management)
- [Part 3 — Day 2: Understand Repository](#part-3--day-2-understand-repository)
- [Part 4 — Day 2: Understand Architecture](#part-4--day-2-understand-architecture)
- [Part 5 — Day 2: Find Tech Stack](#part-5--day-2-find-tech-stack)
- [Part 5B — Day 2: Find Environments](#part-5b--day-2-find-environments)
- [Part 5C — Day 2: API Documentation](#part-5c--day-2-api-documentation)
- [Part 5D — Day 2: Code Standards](#part-5d--day-2-code-standards)
- [Part 6 — Day 3: Understand Codebase](#part-6--day-3-understand-codebase)
- [Part 6B — Day 3: Understanding Existing Tests](#part-6b--day-3-understanding-existing-tests)
- [Part 6C — Day 3: Design System and Figma](#part-6c--day-3-design-system-and-figma)
- [Part 7 — Day 3: Understand Project Management](#part-7--day-3-understand-project-management)
- [Part 8 — Day 4: Talk to Team](#part-8--day-4-talk-to-team)
- [Part 8B — Day 4: KT Sessions and Pair Programming](#part-8b--day-4-kt-sessions-and-pair-programming)
- [Part 9 — Day 4: Setup Local Environment](#part-9--day-4-setup-local-environment)
- [Part 9B — Day 4: Debugging Setup](#part-9b--day-4-debugging-setup)
- [Part 10 — Day 5: Before First Code](#part-10--day-5-before-first-code)
- [Part 11 — Week 1: First Code](#part-11--week-1-first-code)
- [Part 11B — Security Standards](#part-11b--security-standards)
- [Part 11C — Performance and Accessibility](#part-11c--performance-and-accessibility)
- [Part 12 — Daily Routine](#part-12--daily-routine)
- [Part 12B — Deployment Process](#part-12b--deployment-process)
- [Part 12C — Incident and Bug Process](#part-12c--incident-and-bug-process)
- [Part 13 — First Month Goals](#part-13--first-month-goals)
- [Part 13B — Additional Technical Topics](#part-13b--additional-technical-topics)
- [Part 14 — Key Rules](#part-14--key-rules)
- [Part 15 — Glossary](#part-15--glossary)

---

## Before Everything

[⬆ Back to Top](#-navigation)

### What is This Document?
```
→ Step by step guide for joining any company
→ Works for any project type
→ Works for any tech stack
→ Follow day by day
→ Never skip steps!
```

### How to Use This Guide
```
→ Read TOP section first
→ Follow Day 1 → Day 2 → Day 3 order
→ Check boxes as you complete
→ Never start coding before Day 5
→ Ask questions early saves days later!
```

### Company Types — How Onboarding Differs

| Type | Examples | Onboarding Style |
|---|---|---|
| **Startup** | Early stage, <50 people | Fast, informal, figure it out |
| **Mid Size** | 50-500 people | Some process, some informal |
| **Large Enterprise** | DirecTV, Banks, 500+ | Heavy process, formal, many tools |

```
Startup:
□ Less documentation
□ Fewer tools
□ Ask directly
□ Move fast

Large Enterprise (like DirecTV):
□ Heavy documentation (Confluence!)
□ Many tools (Jira, GitHub Enterprise)
□ Formal processes (SAFe, PI Planning)
□ Multiple approvals needed
□ More people to meet
□ VPN required!
□ Change management process!
```

### Project Types — How Onboarding Differs

| Type | What It Means | Your Focus |
|---|---|---|
| **Greenfield** | Brand new project | Setup everything from scratch |
| **Brownfield** | Existing project, join mid-way | Understand existing code first! |
| **Legacy Migration** | Old system being rewritten | Understand old + new both |

```
Brownfield (most common!):
→ Code already exists
→ Patterns already established
→ NEVER change patterns without asking!
→ Understand before touching!
→ Ask "why is it done this way?"
```

### Team Types — How Onboarding Differs

| Type | People | What to Expect |
|---|---|---|
| **Solo** | Just you | Full ownership, no review |
| **Small** | 2-5 devs | Informal, quick decisions |
| **Large** | 10+ devs | Formal PRs, code reviews, processes |

---

## Part 1 — Day 1: Access and Tools Setup

[⬆ Back to Top](#-navigation)

### Accesses to Get on Day 1

```
Communication:
□ Company email (Outlook/Gmail)
□ Microsoft Teams or Slack
□ Zoom or Google Meet

Project Management:
□ Jira access
□ Confluence access

Code:
□ GitHub / GitLab / Bitbucket access
□ Added to organization
□ Added to your team
□ Added to your repositories

Design:
□ Figma access

Cloud/Infrastructure:
□ VPN access ← Very important!
□ Dev environment URL + credentials
□ QA/SIT environment URL + credentials
□ UAT environment URL + credentials
□ AWS / Azure / GCP console (if needed)

Other:
□ Password manager (1Password, LastPass)
□ Any company specific tools
□ Storybook URL (component library docs)
□ Swagger/API docs URL
```

### Tools to Install on Laptop

```
Must Have:
□ Git → git-scm.com
□ Node.js (LTS) → nodejs.org
□ VS Code → code.visualstudio.com
□ Chrome browser
□ Postman → API testing

Package Managers:
□ npm → comes with Node.js
□ yarn → npm install -g yarn
□ pnpm → npm install -g pnpm

Version Manager (MUST HAVE!):
□ nvm (Node Version Manager)
   Windows: nvm-windows
   Mac/Linux: nvm
   → Manages multiple Node versions!
   → nvm use switches per project!

Terminal:
□ Git Bash (Windows)
□ iTerm2 (Mac)
□ Windows Terminal (Windows)

Other:
□ Docker Desktop (if project uses Docker)
```

### VS Code Extensions to Install

```
Essential:
□ ESLint
□ Prettier
□ GitLens
□ ES7+ React Snippets
□ Auto Import

Framework Specific:
□ Tailwind CSS IntelliSense (if Tailwind)
□ Vetur or Volar (if Vue)
□ Angular Language Service (if Angular)

Debugging:
□ Debugger for Chrome
□ JavaScript Debugger

Helpful:
□ Import Cost
□ Better Comments
□ Path IntelliSense
□ Bracket Pair Colorizer
□ Error Lens (shows errors inline!)
```

### Git Setup on Day 1

```bash
# Configure global identity:
git config --global user.name "Your Name"
git config --global user.email "work@company.com"

# Set VS Code as editor:
git config --global core.editor "code --wait"

# Generate SSH key:
ssh-keygen -t ed25519 -C "work@company.com"

# Add to SSH agent:
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Copy public key to clipboard:
cat ~/.ssh/id_ed25519.pub | clip

# Add to GitHub/GitLab → Settings → SSH Keys
# Test connection:
ssh -T git@github.com

# If company uses SAML SSO (Enterprise GitHub):
# Settings → SSH Keys → Configure SSO → Authorize!
```

### Day 1 Checklist

```
Morning:
□ Company email working
□ Teams/Slack joined
□ Jira accessible
□ Confluence accessible
□ GitHub org access confirmed

Afternoon:
□ All tools installed
□ nvm installed
□ Git configured
□ SSH key setup and tested
□ VPN installed and tested
□ VS Code setup with extensions
□ Joined relevant Teams/Slack channels
```

---

## Part 2 — Day 1: Understand Organization

[⬆ Back to Top](#-navigation)

### Understand Company Structure

```
Ask HR or manager:
□ What does this company do?
□ What product/service do I work on?
□ Which department am I in?
□ Who is my direct manager?
□ What is the org chart?
```

### Understand Your Team Structure

```
Find out:
□ Who is the Product Owner (PO)?
□ Who is the Scrum Master (SM)?
□ Who is the Tech Lead?
□ Who are the senior developers?
□ Who are the QA engineers?
□ Who is the Business Analyst (BA)?
□ Who is the DevOps/Release Manager?
□ How many people in team?
□ Are there offshore members?
   → Different time zones?
   → Different standup times?
```

### Communication Tools Setup

```
Microsoft Teams / Slack:
□ Join team channel
□ Join project channel
□ Join general/announcements
□ Turn on notifications
□ Save important contacts

Email:
□ Setup signature
□ Join relevant email groups

Calendar:
□ Accept recurring standup invite
□ Accept sprint ceremony invites
□ Block focus time for yourself
```

### Meeting Cadence

| Meeting | Frequency | Duration | Who Attends |
|---|---|---|---|
| Daily Standup | Every day | 15 mins | Whole team |
| Sprint Planning | Every 2 weeks | 2-4 hours | Whole team |
| Sprint Review | Every 2 weeks | 1-2 hours | Team + stakeholders |
| Retrospective | Every 2 weeks | 1 hour | Team only |
| PI Planning | Every 12 weeks | 2 days | Entire ART |
| Backlog Refinement | Weekly | 1-2 hours | Team |

```
First week rule:
→ Just LISTEN in all meetings!
→ Don't speak unless asked
→ Take notes
→ Ask questions AFTER meeting
```

### Who is Who

| Role | What They Do | When to Talk |
|---|---|---|
| PO | Owns backlog, accepts work | Requirements, design |
| Scrum Master | Facilitates process | Blockers, process |
| Tech Lead | Technical decisions | Architecture, approach |
| Senior Dev | Code expertise | Patterns, technical help |
| BA | Requirements details | Story clarification |
| QA | Testing | Test environment, bugs |
| DevOps | Deployments | Environment issues |

---

## Part 2B — Day 1: VPN Setup

[⬆ Back to Top](#-navigation)

### Why VPN is Needed

```
Company resources are on PRIVATE network:
→ Git repositories (private org)
→ Dev/SIT/UAT environment URLs
→ Internal APIs
→ Confluence and Jira (sometimes)
→ Database access

Without VPN:
→ Cannot clone private repos
→ Cannot access test environments
→ Nothing works from home!
```

### How to Setup VPN

```
Ask IT team / tech lead:
□ What VPN software to use?
   → Cisco AnyConnect (most common)
   → Palo Alto GlobalProtect
   → Pulse Secure
   → Zscaler

□ Where to download it?
□ What are the VPN credentials?
□ What is the VPN server address?

Setup steps (Cisco AnyConnect example):
1. Download and install AnyConnect
2. Open AnyConnect
3. Enter VPN server address
4. Enter username + password
5. Enter MFA code (if required)
6. Connected!
```

### When to Connect VPN

```
Always connect VPN before:
□ Cloning company repositories
□ Accessing dev/SIT/UAT URLs
□ Calling internal APIs
□ Accessing Confluence (sometimes)
□ Accessing Jira (sometimes)
□ Any company internal resource

VPN not needed for:
□ Public websites
□ npm install (public packages)
□ Google, Stack Overflow etc
```

### VPN Troubleshooting

```
Cannot connect:
□ Check credentials are correct
□ Check MFA app has latest code
□ Check VPN server address
□ Ask IT team for help

Connected but cannot access:
□ Some resources need extra permissions
□ Ask tech lead for access
□ Check if specific network needed
```

---

## Part 2C — Day 1: Node Version Management

[⬆ Back to Top](#-navigation)

### Why Node Version Matters

```
Wrong Node version = project won't run!
Very common issue for new joiners!

Example error:
"The engine node is incompatible"
"Expected version >=18 but got 16"

Solution: Use correct Node version!
```

### Find Required Node Version

```
Check these files:
□ .nvmrc file in root
   → Contains exact version: "18.17.0"

□ package.json engines field:
   "engines": {
     "node": ">=18.0.0"
   }

□ README.md
   → "Requires Node 18+"

□ .node-version file
   → Same as .nvmrc
```

### nvm Setup and Usage

```bash
# Install nvm (Windows):
# Download from: github.com/coreybutler/nvm-windows

# Install nvm (Mac/Linux):
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Verify nvm installed:
nvm --version

# List available Node versions:
nvm list available

# Install specific version:
nvm install 18.17.0

# Install latest LTS:
nvm install --lts

# Use specific version:
nvm use 18.17.0

# Use version from .nvmrc (automatic!):
nvm use

# Set default version:
nvm alias default 18.17.0

# Check current version:
node --version
```

### .nvmrc File

```bash
# If .nvmrc exists in project root:
cd your-project
nvm use
# Automatically switches to correct version!

# If .nvmrc doesn't exist, create one:
echo "18.17.0" > .nvmrc
```

---

## Part 3 — Day 2: Understand Repository

[⬆ Back to Top](#-navigation)

### Step 1 — How Many Repos?

```
Ask tech lead:
"How many repositories do I need access to?"

One URL given:
→ Could be Single Repo OR Monorepo
→ Investigate further

Multiple URLs given:
→ Multirepo!
→ Clone each separately
→ Each is independent app/service
```

### Step 2 — Identify Repo Type

#### Single Repo
```
Signs:
□ One package.json at root
□ One app folder (src/ or app/)
□ No apps/ or packages/ folders
□ No turbo.json, nx.json, lerna.json
□ Simple flat structure

Commands:
npm install
npm run dev
```

#### Monorepo
```
Signs:
□ turbo.json exists      → Turborepo
□ nx.json exists         → Nx
□ lerna.json exists      → Lerna
□ pnpm-workspace.yaml    → pnpm workspaces
□ apps/ folder exists
□ packages/ folder exists
□ Multiple package.json files

Structure:
repo/
├── apps/
│     ├── web/        ← deployable app
│     └── docs/       ← documentation
└── packages/
      ├── ui/         ← shared components
      └── config/     ← shared config

Commands:
pnpm install
pnpm dev
pnpm dev --filter web
```

#### Multirepo
```
Signs:
□ Multiple separate repo URLs given
□ Each repo is independent
□ Each has its own package.json
□ Each deployed separately

Commands (in each repo separately):
git clone each repo
npm install
npm run dev
```

### What Changes Based on Repo Type

| Step | Single Repo | Monorepo | Multirepo |
|---|---|---|---|
| Clone | Once | Once | Multiple times |
| Install | `npm install` | `pnpm install` root | Per repo |
| Run | `npm run dev` | `pnpm dev` | Per repo |
| Find feature | One place | Check apps/ | Check each repo |
| Shared code | components/ | packages/ | Separate repo |

---

## Part 4 — Day 2: Understand Architecture

[⬆ Back to Top](#-navigation)

### Step 1 — Ask Tech Lead First!

```
□ "Is this a micro frontend?"
□ "Is this a single app?"
□ "How many apps are deployed?"
□ "What is the overall architecture?"

Always ask before investigating!
```

### Step 2 — Read README.md

```
□ Architecture description?
□ Mentions "micro frontend"?
□ Mentions "shell" or "host" app?
□ How many apps listed?
□ What URLs for each?
```

### Step 3 — Look at Folder Structure

```
Single App:
□ One app folder only
□ pages/ or app/ at root
□ Simple flat structure

Micro Frontend:
□ shell/ or host/ app exists
□ remote/ apps exist
□ Multiple apps/ folders
□ Different port per app
```

### Step 4 — Check package.json Scripts

```
Single App:
"dev": "next dev"

Micro Frontend:
"dev:shell": "..."
"dev:billing": "..."
"start:remote": "..."
```

### Step 5 — Check Config Files (Last!)

```
webpack.config.js:
□ ModuleFederationPlugin? → Micro Frontend!
□ "remotes:" key?         → Micro Frontend!
□ "exposes:" key?         → Micro Frontend!

vite.config.ts:
□ "@originjs/vite-plugin-federation"? → Micro Frontend!
```

### Architecture Types

#### Single App (Combo 1)
```
One repo → One app → One deployment
Example: Small company website
```

#### Monorepo + Micro Frontend (Combo 2)
```
One repo → Multiple apps → ONE website
www.company.com/billing  ← Billing micro app
www.company.com/profile  ← Profile micro app
Used by: Goldman Sachs, DirecTV, Airbnb
```

#### Multirepo + Single App (Combo 3)
```
Multiple repos → SEPARATE websites
www.bank.com      ← internet banking repo
admin.bank.com    ← admin portal repo
Each completely independent!
```

#### Multirepo + Micro Frontend (Combo 4)
```
Multiple repos → ONE website
Each micro app in own repo
All combined at runtime
Used by: JPMorgan Chase, Netflix, Amazon
```

### What Changes Based on Architecture

| Factor | Single App | Micro Frontend |
|---|---|---|
| Run locally | One command | One per app |
| Shared components | components/ | packages/ or separate repo |
| Deployment | One | One per micro app |
| Find feature | One codebase | Find which app owns it |

---

## Part 5 — Day 2: Find Tech Stack

[⬆ Back to Top](#-navigation)

### 1. Find Package Manager
```
Check lock files in root:
□ package-lock.json   → npm
□ yarn.lock           → yarn
□ pnpm-lock.yaml      → pnpm
□ bun.lockb           → bun

Check package.json:
□ "packageManager": "pnpm@8.x" → pnpm

Check README.md for install command!
```

### 2. Find Framework
```
Check package.json dependencies:
□ "next"              → Next.js
□ "react"             → React
□ "vue"               → Vue
□ "nuxt"              → Nuxt
□ "@angular/core"     → Angular
□ "svelte"            → Svelte
□ "@remix-run/react"  → Remix

Check folder structure:
□ app/ folder         → Next.js App Router
□ pages/ folder       → Next.js Pages Router
□ src/App.tsx         → React (CRA/Vite)
```

### 3. Find Build Tool
```
Check config files:
□ webpack.config.js   → Webpack
□ vite.config.ts      → Vite
□ rollup.config.js    → Rollup
□ .swcrc              → SWC
□ turbo.json          → Turborepo

Check devDependencies:
□ "webpack"           → Webpack
□ "vite"              → Vite
□ "esbuild"           → ESBuild
□ "@swc/core"         → SWC
```

### 4. Find Language
```
Check file extensions:
□ .tsx .ts files      → TypeScript
□ .jsx .js files      → JavaScript

Check root:
□ tsconfig.json       → TypeScript
□ jsconfig.json       → JavaScript
```

### 5. Find State Management
```
Check package.json:
□ "@reduxjs/toolkit"      → Redux Toolkit
□ "zustand"               → Zustand
□ "jotai"                 → Jotai
□ "@tanstack/react-query" → React Query
□ "swr"                   → SWR

Search codebase (Ctrl+Shift+F):
□ "useSelector"           → Redux
□ "useDispatch"           → Redux
□ "create("               → Zustand
□ "useQuery("             → React Query
□ "useContext"            → Context API

Check folders:
□ store/ folder           → Redux or Zustand
□ context/ folder         → Context API
```

### 6. Find CSS Approach
```
Check package.json:
□ "tailwindcss"           → Tailwind
□ "styled-components"     → Styled Components
□ "@emotion/react"        → Emotion
□ "sass"                  → SASS/SCSS
□ "@mui/material"         → MUI

Check component files:
□ className="flex p-4"    → Tailwind
□ *.module.css files      → CSS Modules
□ styled.div``            → Styled Components
□ sx={{ }}                → MUI
```

### 7. Find Component Library
```
Check package.json:
□ "@mui/material"             → Material UI
□ "antd"                      → Ant Design
□ "@chakra-ui/react"          → Chakra UI
□ "class-variance-authority"  → Shadcn/ui!
□ "@stencil/core"             → Stencil
□ "@fluentui/react"           → Microsoft Fluent

Check folders:
□ components/ui/ folder       → Shadcn/ui
□ packages/@company/ui        → Custom library!

Search imports:
□ from '@mui/material'        → MUI
□ from '@/components/ui/'     → Shadcn
□ from '@company/ui'          → Custom (Stencil?)
```

### 8. Find API Layer
```
Check package.json:
□ "axios"                 → Axios
□ "@tanstack/react-query" → React Query
□ "graphql"               → GraphQL
□ "@apollo/client"        → Apollo

Check folders:
□ services/ folder        → Service layer
□ lib/api.ts              → API base config
□ app/api/ folder         → Next.js API routes
□ graphql/ folder         → GraphQL queries

Search:
□ "axios.create"          → Axios instance
□ "interceptors"          → Axios interceptors
□ "gql`"                  → GraphQL
```

### 9. Find Auth
```
Check package.json:
□ "next-auth"             → NextAuth
□ "@auth0/nextjs-auth0"   → Auth0
□ "firebase"              → Firebase Auth
□ "@okta/okta-react"      → Okta

Check folders:
□ app/api/auth/           → NextAuth routes
□ middleware.ts           → Auth middleware
```

### 10. Find Testing Tools
```
Check devDependencies:
□ "jest"                      → Jest
□ "vitest"                    → Vitest
□ "@testing-library/react"    → React Testing Library
□ "cypress"                   → Cypress E2E
□ "playwright"                → Playwright E2E
□ "@storybook/react"          → Storybook

Check config files:
□ jest.config.ts
□ vitest.config.ts
□ cypress.config.ts
□ .storybook/ folder
```

### 11. Find CI/CD
```
Check hidden folders:
□ .github/workflows/      → GitHub Actions
□ .gitlab-ci.yml          → GitLab CI
□ Jenkinsfile             → Jenkins
□ azure-pipelines.yml     → Azure DevOps

Check deployment config:
□ vercel.json             → Vercel
□ Dockerfile              → Docker
□ docker-compose.yml      → Docker Compose
□ k8s/ or kubernetes/     → Kubernetes
```

### 12. Find Code Quality Tools
```
Check devDependencies:
□ "eslint"                → ESLint
□ "prettier"              → Prettier
□ "husky"                 → Git hooks
□ "lint-staged"           → Lint on commit
□ "commitlint"            → Commit message rules

Check config files:
□ .eslintrc.json
□ .prettierrc
□ .husky/ folder
□ .commitlintrc
```

### 13. Find Analytics/Monitoring
```
Check package.json:
□ "@segment/analytics-next"       → Segment
□ "@amplitude/analytics-browser"  → Amplitude
□ "mixpanel-browser"              → Mixpanel
□ "@datadog/browser-rum"          → Datadog
□ "@sentry/nextjs"                → Sentry (errors!)
□ "newrelic"                      → New Relic
□ "web-vitals"                    → Core Web Vitals
```

### Tech Stack Summary Table

| What | Where to Look |
|---|---|
| Package manager | Lock files in root |
| Framework | package.json dependencies |
| Build tool | Config files in root |
| Language | File extensions + tsconfig |
| State management | package.json + search |
| CSS | package.json + component files |
| Component library | package.json + imports |
| API layer | package.json + services/ |
| Auth | package.json + middleware.ts |
| Testing | devDependencies + config files |
| CI/CD | Hidden folders |
| Code quality | Config files in root |
| Analytics | package.json |

---

## Part 5B — Day 2: Find Environments

[⬆ Back to Top](#-navigation)

### What Environments Are

```
Environments = Different deployed versions
               of your app on different servers

Each environment has:
→ A URL to access it
→ Its own data (separate database)
→ Its own API connections
→ Different access permissions
```

### Where to Find Environment URLs

```
PLACE 1 — README.md:
→ Environments section
→ Lists all URLs!

PLACE 2 — Confluence:
→ Project space → Environment page
→ All URLs + credentials listed!

PLACE 3 — CI/CD Config files:
.github/workflows/deploy.yml:
→ Shows which branch → which environment
→ Shows environment URLs

PLACE 4 — .env files in repo:
.env.development:
NEXT_PUBLIC_APP_URL=https://dev.company.com

.env.sit:
NEXT_PUBLIC_APP_URL=https://sit.company.com

.env.uat:
NEXT_PUBLIC_APP_URL=https://uat.company.com

PLACE 5 — Ask tech lead!
→ Fastest way!
```

### Branch to Environment Mapping

```
How CI/CD works at most companies:

Your laptop (local → localhost:3000)
    ↓ push feature branch
    ↓ PR merged to develop
develop → AUTO DEPLOYS → DEV environment
    ↓ PR merged to release
release → AUTO DEPLOYS → SIT environment
    ↓ QA approves
                → AUTO DEPLOYS → UAT
    ↓ Business approves
main    → AUTO DEPLOYS → PRODUCTION

Push code → wait few minutes → URL updated!
```

### Environment Names by Company Type

| Type | Environments |
|---|---|
| **Startup** | local → dev → staging → prod |
| **Mid Size** | local → dev → QA → staging → prod |
| **Enterprise** | local → dev → SIT → UAT → staging → prod |
| **Banks** | local → dev → SIT → UAT → DR → pre-prod → prod |

### What Each Environment Means

| Environment | Full Name | Used By | Purpose |
|---|---|---|---|
| **Local** | Your laptop | You only | Development |
| **Dev** | Development | All devs | Integration |
| **SIT** | System Integration Testing | QA team | Test all systems |
| **UAT** | User Acceptance Testing | Business team | Business validates |
| **Staging** | Pre-production | Release team | Final check |
| **DR** | Disaster Recovery | Ops team | Backup |
| **Prod** | Production | Real users | Live! |

### What to Ask Tech Lead

```
□ What environments do we have?
□ What is the URL for each?
□ Which branch deploys to which env?
□ Do I need VPN to access them?
□ What are test credentials for SIT/UAT?
□ Who approves deployment to UAT?
□ Who approves deployment to production?
□ How long does deployment take?
□ How do I know deployment succeeded?
```

---

## Part 5C — Day 2: API Documentation

[⬆ Back to Top](#-navigation)

### Find API Documentation

```
Ask tech lead:
□ Is there Swagger/OpenAPI docs?
□ Is there a Postman collection?
□ Is there an internal API wiki page?
□ Are there API docs in Confluence?

Where to check in repo:
□ /docs folder             → API docs?
□ swagger.json exists?     → Swagger!
□ openapi.yaml exists?     → OpenAPI!
□ README mentions API docs URL?
```

### Types of API Documentation

| Type | What It Shows | URL Format |
|---|---|---|
| **Swagger/OpenAPI** | All endpoints, request/response | /api-docs or /swagger |
| **Postman Collection** | Importable API requests | .json file to import |
| **Confluence page** | Internal API guide | Confluence URL |
| **README.md** | Basic API overview | In repo |

### How to Test APIs Locally

```
Using Postman:
□ Import collection (if exists)
□ Set environment variables
   → Base URL: http://localhost:3000
   → Auth token: get from login API

Testing with auth:
□ Login first to get token
□ Add token to Authorization header:
   Bearer your-token-here
□ OR ask tech lead for test token

Testing Next.js API routes:
□ http://localhost:3000/api/billing/charges
□ Open in Postman or browser
□ Check Network tab in browser too!
```

### Questions to Ask About APIs

```
□ Is there Swagger UI available?
□ Is there a Postman collection to import?
□ How do I authenticate API calls locally?
□ Are there mock APIs for development?
□ Which APIs call external services?
□ Which external services exist?
   (like Evergent for billing)
□ Is there API versioning?
```

---

## Part 5D — Day 2: Code Standards

[⬆ Back to Top](#-navigation)

### Find Written Standards

```
Check these files:
□ .eslintrc.json        → Linting rules
□ .prettierrc           → Formatting rules
□ .commitlintrc         → Commit message rules
□ CONTRIBUTING.md       → Contribution guide
□ CODE_STYLE.md         → Style guide
□ README.md             → May have standards section

Check Confluence:
□ Search "coding standards"
□ Search "development guidelines"
□ Search "best practices"
```

### Common Naming Conventions

```
Files:
□ Components: PascalCase → SideSheet.tsx
□ Hooks: camelCase with use → useBilling.ts
□ Utils: camelCase → formatCurrency.ts
□ Types: PascalCase → BillingTypes.ts
□ Styles: same as component → SideSheet.module.css
□ Tests: same name + .test → SideSheet.test.tsx

Variables/Functions:
□ Variables: camelCase → isOpen, chargesList
□ Constants: UPPER_SNAKE → MAX_RETRIES
□ Components: PascalCase → SideSheet
□ Interfaces: PascalCase → ChargesResponse
□ Types: PascalCase → ButtonVariant

Folders:
□ Feature based → billing/, auth/, profile/
□ OR type based → components/, hooks/, services/
□ Check existing structure and MATCH IT!
```

### Unwritten Rules — Always Ask Senior Dev!

```
Ask senior developer:
□ What patterns should I follow?
□ What should I avoid doing?
□ Any hidden complexity I should know?
□ Are there any deprecated patterns still in code?
□ Any specific way to structure components?
□ How are props typed in this project?
□ Do we use barrel exports (index.ts)?
□ How are errors handled consistently?
□ Any team-specific coding preferences?
```

### TypeScript Standards

```
Check tsconfig.json:
□ "strict": true     → Strict mode (no any!)
□ "noImplicitAny"    → Must type everything
□ Path aliases (@/)  → Use these for imports!

Common strict mode rules:
□ No any type allowed
□ All function params must be typed
□ All return types explicit
□ Null checks required
□ Interfaces for all objects
```

---

## Part 6 — Day 3: Understand Codebase

[⬆ Back to Top](#-navigation)

### Files to Read — In Order

```
Level 1 — Project Overview:
□ README.md               → setup, architecture
□ package.json            → all dependencies + scripts
□ next.config.js          → redirects, rewrites
□ .env.example            → all env variables
□ tsconfig.json           → TypeScript + path aliases

Level 2 — App Shell:
□ app/layout.tsx          → ALL providers!
□ lib/providers.tsx       → provider details
□ middleware.ts           → auth guards
□ app/page.tsx            → home page

Level 3 — State Management:
□ store/index.ts          → store setup
□ store/*Slice.ts         → state per feature
□ context/*Context.tsx    → context files

Level 4 — API Layer:
□ lib/api.ts              → base API config
□ services/*.ts           → service functions
□ app/api/**/*.ts         → API routes

Level 5 — Your Feature:
□ Find your page file
□ Find your components
□ Search for related code
```

### VS Code Search Strategies

```
Ctrl+Shift+F → Search all files

Search by feature:
□ Feature name            → Find related files
□ Button text             → Find exact component
□ Route path              → Find navigation

Search by pattern:
□ "useSelector"           → Redux components
□ "useQuery("             → React Query
□ "axios.create"          → API client

Useful shortcuts:
□ F12 / Ctrl+Click        → Go to definition
□ Shift+F12               → Find all usages
□ Alt+←                   → Go back
□ Ctrl+P                  → Open file by name
```

### Draw These on Paper!

```
DIAGRAM 1 — Provider Tree:
Provider 1 (outermost)
  └── Provider 2
        └── Provider 3
              └── App pages

DIAGRAM 2 — Component Tree:
Page (Server/Client?)
  ├── ComponentA
  └── ComponentB

DIAGRAM 3 — Data Flow:
User Action → dispatch → State updates → Re-render

DIAGRAM 4 — API Flow:
Component → Hook → Service → API Client → API Route → External API
```

---

## Part 6B — Day 3: Understanding Existing Tests

[⬆ Back to Top](#-navigation)

### Find Existing Tests

```
Check folder structure:
□ __tests__/ folder       → Jest tests
□ *.test.tsx files        → Unit tests
□ *.spec.tsx files        → Unit tests
□ cypress/ folder         → E2E tests
□ playwright/ folder      → E2E tests
□ *.stories.tsx files     → Storybook stories

Run existing tests:
□ npm run test            → Run all tests
□ npm run test:watch      → Watch mode
□ npm run test:coverage   → Coverage report
□ npm run e2e             → E2E tests
```

### Understand Test Structure

```
Look at existing test files:

What to note:
□ How are tests organized?
□ What testing library is used?
□ How are components rendered in tests?
□ How are mocks set up?
□ What is tested (unit vs integration)?
□ What is NOT tested (understand gaps!)

Check coverage report:
□ npm run test:coverage
□ See which files have low coverage
□ Understand what needs testing
```

### Test Requirements

```
Ask tech lead:
□ Do I need to write tests for my story?
□ What is minimum coverage requirement?
   → 80%? 90%? No requirement?
□ Are unit tests or E2E required?
□ Do tests run in CI before merge?
□ What happens if tests fail in CI?
□ Is there a test naming convention?
```

### Test Naming Convention

```
Common patterns:
□ describe("ComponentName", () => {
     it("should render correctly", () => {})
     it("should open on button click", () => {})
     it("should close on ESC key", () => {})
  })

Match existing test style exactly!
Look at neighboring test files and copy pattern!
```

---

## Part 6C — Day 3: Design System and Figma

[⬆ Back to Top](#-navigation)

### Find Design Resources

```
Ask PO/Designer:
□ Figma link for your feature
□ Figma Dev Mode access (shows CSS values!)
□ Design system documentation URL
□ Storybook URL (component explorer)
□ Who is the designer to contact?

Check Confluence:
□ Design space/page
□ Component library docs
□ Brand guidelines
□ Design tokens
```

### How to Read Figma as Developer

```
In Figma, click any element:

Right panel shows:
□ Width/Height          → CSS width/height
□ X/Y position         → For positioning
□ Fill color            → CSS background/color
□ Border radius         → CSS border-radius
□ Font size/weight      → CSS font properties
□ Padding/Margin        → CSS spacing
□ Shadow                → CSS box-shadow
□ Opacity               → CSS opacity

In Dev Mode (if you have access):
□ Shows CSS code directly!
□ Copy paste values!
□ Shows Tailwind equivalents!
```

### What to Extract from Figma

```
Before coding any UI:
□ Component dimensions (width/height)
□ Spacing values (padding/margin/gap)
□ Color values (hex/rgb)
□ Font sizes and weights
□ Border radius values
□ Shadow values
□ Animation duration and easing
□ Breakpoints for responsive
□ All states:
   → Default
   → Hover
   → Active/Focused
   → Loading
   → Error
   → Empty
   → Disabled
```

### Storybook — Component Explorer

```
If Storybook URL exists:
□ See ALL available components
□ See all props each component accepts
□ See all variants (primary, secondary etc)
□ Copy usage examples directly!
□ Check if your needed component exists!

This saves HOURS of work!
Always check Storybook BEFORE building!
```

### Check Design Tokens

```
Design tokens = Named design values

Examples:
--color-primary: #0066CC
--spacing-md: 16px
--border-radius-sm: 4px
--font-size-body: 16px

Where to find:
□ Figma → Design tokens plugin
□ packages/tokens/ in monorepo
□ styles/variables.css
□ tailwind.config.ts (custom values!)

Always use tokens instead of hardcoded values!
```

---

## Part 7 — Day 3: Understand Project Management

[⬆ Back to Top](#-navigation)

### Jira — Reading Order

```
Step 1: Summary tab
□ How many tickets total?
□ Who is active?
□ Epic progress

Step 2: Goals tab
□ WHY does this project exist?
□ What is business objective?

Step 3: Timeline tab
□ What are the epics?
□ What is the schedule?

Step 4: Backlog tab
□ All stories listed
□ Priorities

Step 5: Your Stories
□ Read each story fully
□ Read acceptance criteria
□ Note Figma links attached
□ Note dependencies
```

### Confluence — Reading Order

```
Step 1: Find your space
□ Spaces → search project name

Step 2: Read in this order:
□ Onboarding/Getting Started page
□ Project Overview
□ Architecture docs
□ Design/Figma links
□ Test strategy
□ Environment details (URLs, credentials!)
□ API documentation
□ Team contacts

Step 3: Bookmark important pages!
```

### Agile Ceremonies

| Ceremony | Week 1 | Week 2+ |
|---|---|---|
| Daily Standup | Listen only | Give updates |
| Sprint Planning | Listen, ask 1 question | Estimate + commit |
| Sprint Review | Watch demos | Demo your work |
| Retrospective | Listen | Contribute ideas |
| PI Planning | Observe | Participate |

---

## Part 8 — Day 4: Talk to Team

[⬆ Back to Top](#-navigation)

### Talk to Product Owner (PO)

```
□ What is the main goal of this project?
□ Can you share the Figma link?
□ What are the mobile breakpoints?
□ What is the sprint start date?
□ What is the release date target?
□ Which story should I start first?
□ Are there design files for all states?
   (loading, error, empty, success)
□ Who is the designer to contact?
```

### Talk to Tech Lead

```
□ Where is the Git repository?
□ What is the branching strategy?
□ What are the .env variable values?
□ Who reviews my PRs?
□ What is the PR process?
□ Does a component exist for my feature?
□ What state management pattern to use?
□ Does API route exist for my feature?
□ Is there a mock for external APIs?
□ How do I deploy to DEV environment?
□ What Node version does project use?
□ What analytics platform is configured?
□ Any specific coding standards to follow?
□ Do I need to write tests for my story?
□ What is the test coverage requirement?
```

### Talk to Scrum Master

```
□ When is daily standup?
□ What is the sprint length?
□ When is next sprint planning?
□ How do I report blockers?
□ Is there a team working agreement?
□ How are story points estimated?
```

### Talk to Senior Developer

```
□ What are the common gotchas in this codebase?
□ What patterns should I follow?
□ What should I avoid doing?
□ Is there any code I should study first?
□ Any hidden complexities I should know?
□ Any deprecated patterns still in code?
□ How does deployment work?
□ What is the hotfix process?
```

### Talk to QA Engineer

```
□ How do I report bugs?
□ What is the test environment URL?
□ What are the test credentials?
□ How does the QA process work?
□ What is the definition of done?
□ Where are existing test cases?
□ What testing tools are used?
□ What severity levels are used for bugs?
```

---

## Part 8B — Day 4: KT Sessions and Pair Programming

[⬆ Back to Top](#-navigation)

### Request KT Sessions

```
KT = Knowledge Transfer

Request these sessions:
□ Architecture overview KT
   → Ask tech lead / senior dev
   → Understand overall system design

□ Codebase walkthrough KT
   → Walk through key files together
   → Understand data flow

□ Feature area KT
   → Focus on your specific feature area
   → Understand existing code you will modify

□ Deployment process KT
   → How to deploy to each environment
   → What happens in CI/CD pipeline

□ Testing process KT
   → How tests are written
   → How to run tests
   → What needs testing
```

### What to Cover in Each KT

```
Architecture KT:
□ What is the overall system?
□ How do all parts connect?
□ What external services exist?
□ What are the main data flows?

Codebase KT:
□ Walk through key files
□ Explain naming conventions
□ Show how feature is structured
□ Point out tricky/complex parts
□ Show how state management works

Feature KT:
□ What is the current behavior?
□ Where is the code for this feature?
□ What needs to change?
□ What should NOT change?
```

### Pair Programming and Shadowing

```
Pair Programming:
□ Ask senior dev: "Can we pair program
  on my first story?"
□ You write code
→ They guide and review live
→ Fastest way to learn patterns!

Shadowing:
□ Watch senior dev fix a bug
□ Watch senior dev do a PR review
□ Watch a deployment happen
□ Watch a standup (first few days)
□ Learn by observing first!

Notes during shadowing:
□ Write down everything!
□ Note commands run
□ Note decisions made
□ Note tools used
□ Ask "why" after each step
```

---

## Part 9 — Day 4: Setup Local Environment

[⬆ Back to Top](#-navigation)

### Clone Steps by Repo Type

```bash
# SINGLE REPO:
git clone git@github.com:org/repo.git
cd repo
nvm use                    ← switch Node version!
npm install
npm run dev

# MONOREPO:
git clone git@github.com:org/repo.git
cd repo
nvm use                    ← switch Node version!
pnpm install               ← installs ALL at once!
pnpm dev                   ← runs ALL apps!
pnpm dev --filter web      ← runs ONLY web app

# MULTIREPO:
git clone git@github.com:org/shell.git
git clone git@github.com:org/billing.git
# In each repo:
cd shell
nvm use
npm install
npm run dev
```

### See All Existing Branches

```bash
# See all local branches:
git branch

# See all remote branches:
git branch -r

# See all branches (local + remote):
git branch -a

# What to look for:
□ What is the main branch? (main/master/develop?)
□ What is the integration branch?
□ Are there environment branches?
   (develop/release/main)
□ What branches are active?
   (recent feature branches)

# Check branch protection (ask tech lead):
□ Which branches are protected?
□ Who can push to main?
□ Is PR required to merge?
□ How many approvals needed?
```

### Setup Environment Variables

```bash
# Copy example file:
cp .env.example .env.local

# Open and fill values:
code .env.local

# RULE: Never commit .env.local!
# It is in .gitignore already!
```

### Understand package.json Scripts

```
Before running anything, understand scripts:

"scripts": {
  "dev": "next dev"           → Start development server
  "build": "next build"       → Production build
  "start": "next start"       → Run production build
  "lint": "next lint"         → Check code errors ← run before PR!
  "format": "prettier --write" → Format code ← run before PR!
  "test": "jest"              → Run all tests ← run before PR!
  "test:watch": "jest --watch" → Watch mode for TDD
  "test:coverage": "jest --coverage" → See coverage %
  "type-check": "tsc --noEmit" → TypeScript check ← run before PR!
  "e2e": "cypress run"        → E2E tests
}

BEFORE CREATING PR RUN:
□ npm run lint
□ npm run type-check
□ npm run test
□ Fix all errors!
```

### Verify Project Running

```
□ Dev server runs without error
□ Browser opens at localhost:3000
□ App loads without errors
□ Connect VPN if needed
□ Navigate to your feature area
□ Click relevant buttons
□ Observe CURRENT behavior
□ Open Network tab → see API calls
□ Open React DevTools → see components
□ Open Redux DevTools → see state
```

---

## Part 9B — Day 4: Debugging Setup

[⬆ Back to Top](#-navigation)

### React Developer Tools

```
Install: Chrome Web Store → "React Developer Tools"

How to use:
□ Open Chrome DevTools (F12)
□ Components tab → see React tree
□ Click any component → see props + state
□ Profiler tab → find performance issues

Useful for:
□ Why is component not rendering?
□ What props is component receiving?
□ What is current state value?
□ Which components re-render on action?
```

### Redux DevTools

```
Install: Chrome Web Store → "Redux DevTools"

How to use:
□ Open Chrome DevTools (F12)
□ Redux tab appears
□ See all actions dispatched
□ See state before/after each action
□ Time travel: click any action → see that state!
□ Diff view: what changed?

Useful for:
□ Did my action dispatch?
□ Did state update correctly?
□ What is current Redux state?
□ Why is component not getting new state?
```

### Network Tab for API Debugging

```
F12 → Network tab → XHR/Fetch filter

Click any API request to see:
□ URL: correct endpoint?
□ Method: GET/POST/PUT correct?
□ Status: 200 (OK) / 401 (Auth) / 500 (Error)
□ Headers: auth token present?
□ Request body: correct data sent?
□ Response: what came back?
□ Timing: how slow is it?

Common status codes:
□ 200 → Success!
□ 401 → Authentication problem!
□ 403 → Authorization problem!
□ 404 → Wrong URL!
□ 500 → Server error! Check terminal!
```

### VS Code Debugger

```
Setup launch.json for Next.js:
Create .vscode/launch.json:

{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Next.js: debug server-side",
      "type": "node-terminal",
      "request": "launch",
      "command": "npm run dev"
    },
    {
      "name": "Next.js: debug client-side",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000"
    }
  ]
}

How to use:
□ Click line number → red dot (breakpoint)
□ Press F5 to start debugging
□ App pauses at breakpoint
□ See variable values in side panel
□ Step through code line by line
```

### Common Debug Scenarios

```
Problem: State not updating
→ Open Redux DevTools
→ Did action dispatch?
→ Did reducer handle it?

Problem: API call failing
→ Open Network tab
→ Check status code
→ Check request headers
→ Check response body

Problem: Component not rendering
→ Open React DevTools
→ Is component in tree?
→ What props is it receiving?
→ Any console errors?

Problem: Wrong data displaying
→ console.log(data) temporarily
→ Check Network response
→ Check Redux state
```

---

## Part 10 — Day 5: Before First Code

[⬆ Back to Top](#-navigation)

### Must Have Before Writing Single Line of Code

```
Design:
□ Figma link received ← CANNOT start without this!
□ Mobile design exists
□ Desktop design exists
□ Loading state design
□ Error state design
□ Empty state design
□ All breakpoints defined
□ Design tokens identified

Technical:
□ Project running locally
□ Correct Node version (nvm use!)
□ VPN connected (if needed)
□ Understand folder structure
□ Know which state management to use
□ Know which CSS approach to use
□ Know which component library to use
□ Checked if component exists! (Storybook!)
□ Checked if API route exists!
□ Understood existing code patterns
□ Understood test patterns
□ Tech lead approved approach

Access:
□ All environments accessible
□ .env variables filled and working
□ Can push to remote (git push works)

Process:
□ Know branch naming convention
□ Know commit message format
□ Know who reviews PRs
□ Know scripts to run before PR
□ Know test requirements
```

### Check if Code Exists

```
BEFORE building anything:

□ Search component name → exists?
□ Check Storybook → in design system?
□ Check packages/ → shared component?
□ Search API endpoint → route exists?
□ Search service function → already written?

RULE:
Reuse > Modify > Build from scratch!
```

### Create Feature Branch

```bash
git checkout develop
git pull origin develop
git checkout -b feature/TICKET-description

# Examples:
git checkout -b feature/M20B-1-side-sheet
git checkout -b bugfix/M20B-1-fix-animation
git checkout -b chore/update-dependencies
```

---

## Part 11 — Week 1: First Code

[⬆ Back to Top](#-navigation)

### Git Workflow Daily

```bash
# Start of day:
git pull origin develop

# During work:
git status
git add .
git commit -m "feat(TICKET): description"
git push origin branch-name

# End of day:
git push origin branch-name  ← backup!
```

### Branch Naming Convention

```
feature/TICKET-description   → new feature
bugfix/TICKET-description    → bug fix
hotfix/TICKET-description    → urgent fix
chore/TICKET-description     → cleanup
docs/TICKET-description      → documentation
```

### Commit Message Format

```
type(ticket): short description

Types:
feat     → new feature
fix      → bug fix
style    → CSS/styling
refactor → code change
test     → adding tests
docs     → documentation
chore    → build/config
a11y     → accessibility

Examples:
feat(M20B-1): add SideSheet component
fix(M20B-1): fix Safari animation
style(M20B-1): match Figma spacing
test(M20B-1): add unit tests
a11y(M20B-3): add focus trap
```

### Before Creating PR — Run These!

```bash
# Always run before PR:
npm run lint          ← fix all lint errors!
npm run type-check    ← fix all TS errors!
npm run test          ← all tests must pass!
npm run build         ← build must succeed!
```

### PR Process

```
Step 1: Push branch
Step 2: Go to GitHub → Create Pull Request
Step 3: Base = develop | Compare = your branch
Step 4: Fill description:
   → What changed
   → Jira ticket link
   → Screenshots desktop + mobile
   → Testing done checklist
Step 5: Add reviewer
Step 6: Submit!
Step 7: Fix review comments
Step 8: Get approval
Step 9: Merge!
```

### Handle Code Review Feedback

```
□ Read all comments carefully
□ Don't take it personally!
□ Ask if you don't understand
□ Fix all requested changes
□ Reply to each comment when fixed
□ Request re-review when done
```

---

## Part 11B — Security Standards

[⬆ Back to Top](#-navigation)

### Code Security Rules

```
NEVER do this:
❌ Hardcode API keys in code
❌ Hardcode passwords in code
❌ Commit .env files
❌ console.log sensitive data
❌ console.log in production code
❌ Hardcode environment URLs
❌ Use eval() function
❌ Disable TypeScript checks with @ts-ignore
   (unless absolutely necessary!)

ALWAYS do this:
✅ Use environment variables for secrets
✅ Use .env.local for local secrets
✅ Add sensitive files to .gitignore
✅ Use HTTPS for all API calls
✅ Validate user inputs
✅ Handle errors gracefully (don't expose errors)
```

### Check for Vulnerabilities

```bash
# Check for known vulnerabilities in packages:
npm audit

# Fix automatically:
npm audit fix

# See details:
npm audit --json

# Ask tech lead:
□ Is there automated vulnerability scanning?
□ Is SonarQube or similar tool used?
□ What is the process for security issues?
```

### Secrets Management

```
Ask tech lead:
□ Where are production secrets stored?
   → AWS Secrets Manager?
   → Azure Key Vault?
   → HashiCorp Vault?
   → Environment variables in CI/CD?

□ How do I access secrets for local dev?
□ Who manages production secrets?
□ What is the process to rotate keys?
```

---

## Part 11C — Performance and Accessibility

[⬆ Back to Top](#-navigation)

### Performance Standards

```
Ask tech lead:
□ Is there a performance budget?
□ What Lighthouse score is required?
□ What are Core Web Vitals targets?
□ Is there bundle size limit?
□ Are there page load time requirements?

Common targets:
□ Lighthouse Performance: > 90
□ LCP (Largest Contentful Paint): < 2.5s
□ FID (First Input Delay): < 100ms
□ CLS (Cumulative Layout Shift): < 0.1
□ Bundle size: varies per project
```

### How to Check Performance

```bash
# Run Lighthouse in Chrome:
□ F12 → Lighthouse tab
□ Click "Analyze page load"
□ See score and recommendations

# Check bundle size:
npm run build
# Look at build output sizes

# Check Core Web Vitals:
□ Chrome DevTools → Performance tab
□ Record page load
□ See LCP, FID, CLS values
```

### Accessibility Standards

```
Ask tech lead:
□ What WCAG level required? (A, AA, AAA)
□ Is there automated accessibility testing?
□ Are there specific screen reader tests?

Common WCAG AA requirements:
□ Color contrast ratio 4.5:1 for text
□ All interactive elements keyboard accessible
□ Focus visible on all elements
□ Alt text on all images
□ Proper heading hierarchy (h1, h2, h3)
□ ARIA labels on icon buttons
□ Error messages programmatically associated

Tools to check:
□ axe DevTools Chrome extension
□ WAVE accessibility checker
□ Keyboard-only navigation test
□ Screen reader test (NVDA/VoiceOver)
```

---

## Part 12 — Daily Routine

[⬆ Back to Top](#-navigation)

### Morning Routine

```
□ Check Teams/Slack messages
□ Check email
□ Connect VPN (if needed)
□ git pull origin develop
□ Check Jira for ticket updates
□ Attend daily standup (on time!)
□ Update standup:
   → What I did yesterday
   → What I will do today
   → Any blockers?
```

### During the Day

```
□ Work on assigned story
□ Commit code every few hours
□ If stuck 30 mins → ask for help!
□ Update Jira ticket status
□ Ask questions in Teams/Slack
□ Attend scheduled meetings
□ Review PRs if asked
```

### End of Day

```
□ Push all code to remote (backup!)
□ Update Jira ticket with progress
□ Note blockers for tomorrow
□ Log hours (if required)
□ Plan tomorrow's work
```

### Weekly Habits

```
□ Monday: Plan week, review sprint board
□ Wednesday: Mid-week sprint goal check
□ Friday: Push all code, clean branches
□ Weekly: Review own PR before submitting
□ Weekly: Read team's merged PRs (learn!)
□ Weekly: Check if tests are passing
```

---

## Part 12B — Deployment Process

[⬆ Back to Top](#-navigation)

### How Deployment Works

```
TYPICAL FLOW:
──────────────────────────────────────
Your feature branch
    ↓ PR approved + merged
develop branch
    ↓ CI/CD runs automatically
    ↓ Tests run
    ↓ Build succeeds
DEV environment updated!
    ↓ Release branch cut
SIT environment updated!
    ↓ QA approves
UAT environment updated!
    ↓ Business approves
    ↓ Release Manager approves
PRODUCTION updated!
```

### How to Monitor Deployment

```
Ask tech lead where to monitor:
□ GitHub Actions logs
□ Jenkins build logs
□ Azure DevOps pipelines
□ Vercel deployment logs

Signs deployment succeeded:
□ CI/CD shows green checkmark
□ Environment URL shows new changes
□ No error alerts in monitoring tools

Signs deployment failed:
□ CI/CD shows red X
□ Error in build logs
□ Rollback may be needed
```

### Change Management (Enterprise!)

```
Large enterprises need approvals:
□ Change ticket raised (ITSM tool)
□ Change Review Board approval
□ Release Manager sign-off
□ QA sign-off
□ Business sign-off

Ask tech lead:
□ Do we need a change ticket?
□ Who approves production deployments?
□ What is the release schedule?
□ Are there deployment freeze periods?
   (no deployments during holidays etc!)
```

### Rollback Process

```
If production deployment breaks:
□ Alert goes out immediately
□ Team notified in Teams/Slack
□ On-call engineer investigates
□ Decision: fix forward or rollback?

Rollback:
□ Revert to previous deployment
□ Previous version restored
□ Usually takes 5-15 minutes
□ Post-mortem done after

As new joiner:
□ Don't panic!
□ Inform tech lead immediately
□ Follow their instructions
□ Document what happened
```

---

## Part 12C — Incident and Bug Process

[⬆ Back to Top](#-navigation)

### Bug Severity Levels

| Severity | What It Means | Response Time |
|---|---|---|
| **P1 Critical** | Production down, all users affected | Immediate! |
| **P2 High** | Major feature broken, many users | Same day |
| **P3 Medium** | Feature partially broken | This sprint |
| **P4 Low** | Minor issue, workaround exists | Next sprint |

### How Bugs are Reported

```
From QA team:
□ Bug raised in Jira
□ Assigned to developer
□ Severity/priority set
□ Steps to reproduce included
□ Environment where found noted
□ Screenshots/videos attached

From Production:
□ Monitoring alert fires
□ Customer support reports
□ Automatic error tracking (Sentry)
□ On-call engineer notified
```

### Hotfix Process

```
For P1/P2 production bugs:

Step 1: Create hotfix branch
git checkout main
git checkout -b hotfix/TICKET-description

Step 2: Fix the bug quickly
Step 3: PR against main (not develop!)
Step 4: Emergency review (fast!)
Step 5: Merge + deploy immediately
Step 6: Cherry-pick to develop too!
git checkout develop
git cherry-pick <commit-hash>

Ask tech lead:
□ What is hotfix process here?
□ Who approves hotfix PRs?
□ How quickly deployed?
```

### As New Joiner — Bug Handling

```
If you find a bug:
□ Note exact steps to reproduce
□ Note which environment
□ Take screenshot/video
□ Raise Jira ticket
□ Assign to yourself if you will fix
□ Inform team in standup

If you introduce a bug:
□ Don't hide it!
□ Tell tech lead immediately
□ Assess severity
□ Fix ASAP
□ Learn from it
→ Everyone introduces bugs!
→ How you handle it matters!
```

---

## Part 13 — First Month Goals

[⬆ Back to Top](#-navigation)

### Week 1 Goals

```
□ All access setup done
□ VPN working
□ Node version correct
□ Project running locally
□ Understand tech stack
□ Understand team and roles
□ Attend all ceremonies (just listen)
□ First feature branch created
□ First commit pushed
□ KT sessions requested and started
```

### Week 2 Goals

```
□ First story completed
□ First PR created and reviewed
□ Understand state management deeply
□ Understand API layer deeply
□ Ask questions without hesitation
□ First PR merged!
□ Understand test patterns
□ Understand deployment process
```

### Week 3 Goals

```
□ Second story in progress
□ Know codebase well enough
□ Contribute in standup actively
□ Give estimates in sprint planning
□ No major re-writes needed
□ Code follows existing patterns
□ Writing tests with story
```

### Week 4 Goals

```
□ All assigned stories done or in progress
□ Know team members well
□ Comfortable with git workflow
□ Can review others PRs
□ Understand full deployment process
□ Feel confident in daily work!
□ Can work independently on tasks
```

---

## Part 13B — Additional Technical Topics

[⬆ Back to Top](#-navigation)

### 1. Docker

```
Check if Docker is used:
□ Dockerfile exists in root?
□ docker-compose.yml exists?
□ "docker" mentioned in README?

If Docker used:
□ Install Docker Desktop
□ Common commands:

docker-compose up        → Start all services
docker-compose down      → Stop all services
docker-compose up --build → Rebuild and start
docker ps                → See running containers
docker logs container-name → See logs

Ask tech lead:
□ Do I need Docker for local dev?
□ What services run in Docker?
   (database? redis? mock APIs?)
□ Is Docker needed or optional locally?
```

---

### 2. Feature Flags

```
What are feature flags:
→ Switches to turn features ON/OFF
→ Without redeploying code!
→ Enable for specific users
→ Enable for specific environments
→ Very common in enterprise!

Check if used:
□ Search "featureFlag" in codebase
□ Search "LaunchDarkly" in package.json
□ Search "Split" in package.json
□ Search "Unleash" in package.json
□ Search "isEnabled" in codebase
□ Check .env.example for FF variables

Common tools:
□ LaunchDarkly    → most popular enterprise
□ Split.io        → A/B testing + flags
□ Unleash         → open source option
□ Custom          → simple boolean in config

What to ask tech lead:
□ Do we use feature flags?
□ How do I enable a flag locally?
□ What flags control my feature?
□ How do I create a new flag?
```

---

### 3. Database and Backend Knowledge

```
As frontend developer you don't need deep DB knowledge
BUT you need to understand:

□ What database does this project use?
   → PostgreSQL? MySQL? MongoDB? DynamoDB?

□ Who owns the backend?
   → Same team? Different team?
   → Who to contact for API changes?

□ Is there a BFF (Backend For Frontend)?
   → Next.js API routes acting as BFF?
   → Separate BFF service?

□ How does data flow to your UI?
   → External API → Next.js API route → Your component
   → Direct API → Your component (avoid!)

□ What is the data refresh strategy?
   → Real-time (WebSockets)?
   → Polling (every X seconds)?
   → On demand (user action)?

Ask tech lead:
□ Who owns the APIs I consume?
□ Who do I contact if API has a bug?
□ Is there API versioning?
□ How are breaking API changes handled?
```

---

### 4. Dependency Management

```
Adding new packages:
□ Check if package already exists!
   → Search package name in package.json first!
□ Ask tech lead before adding new package
   → Some companies restrict new dependencies!
□ Check package license compatibility
   → MIT, Apache = usually fine
   → GPL = check with legal team!
□ Check package health:
   → Last updated recently?
   → Many weekly downloads?
   → Active maintenance?

Commands:
npm install package-name       → add to dependencies
npm install -D package-name    → add to devDependencies
npm uninstall package-name     → remove package
npm outdated                   → see outdated packages
npm update                     → update packages

Updating packages:
□ Don't update all packages at once!
□ Update one at a time
□ Run tests after each update
□ Check changelog for breaking changes!

Ask tech lead:
□ Do I need approval to add packages?
□ Is there an approved package list?
□ How are package updates handled?
□ Who manages security vulnerabilities?
```

---

### 5. Browser Compatibility

```
Ask tech lead:
□ Which browsers must we support?
□ What is minimum browser version?

Common support matrix:
□ Chrome (last 2 versions)
□ Firefox (last 2 versions)
□ Safari (last 2 versions)
□ Edge (last 2 versions)
□ Mobile Safari (iOS 14+)
□ Chrome Android (last 2 versions)
□ IE 11? (hopefully not! very old!)

How to test:
□ Test manually in each browser
□ Use BrowserStack for cross-browser
□ Check CSS compatibility:
   → caniuse.com → search CSS property
   → See which browsers support it!

Common browser issues:
□ Safari: CSS gap in flexbox (older versions)
□ Safari: date parsing differences
□ Firefox: scrollbar width differences
□ IE11: no support for modern CSS/JS

Check in package.json:
□ "browserslist" field
   → Defines supported browsers
   → Used by Babel/PostCSS automatically!
```

---

### 6. Internationalization (i18n)

```
Check if i18n is used:
□ Search "i18n" in package.json
□ Search "i18next" in package.json
□ Search "react-intl" in package.json
□ Search "t(" in codebase (translation function!)
□ locales/ or translations/ folder exists?
□ en.json or messages.json files?

If i18n exists:
□ NEVER hardcode text strings directly!
   WRONG: <button>Submit</button>
   RIGHT: <button>{t('common.submit')}</button>

□ Always add new strings to translation files
□ Ask: what is the key naming convention?
□ Ask: which languages are supported?
□ Ask: how are new translations added?

Common patterns:
// Using i18next:
const { t } = useTranslation();
return <p>{t('billing.viewCharges')}</p>

// Translation file (en.json):
{
  "billing": {
    "viewCharges": "View Current Charges"
  }
}

Ask tech lead:
□ Do we use i18n?
□ What languages are supported?
□ What is the key naming convention?
□ How do I add new text strings?
```

---

### 7. Error Handling Patterns

```
Find existing error handling:
□ Search "ErrorBoundary" in codebase
□ Search "catch(error" in codebase
□ Search "toast" or "notification" in codebase
□ Check how API errors are shown to user

Types of error handling:
□ Global error boundary (catch React errors)
□ API error handling (show error message)
□ Form validation errors
□ Network error handling (offline)
□ 404/401/403 page handling

Common patterns to look for:
// Toast notifications:
toast.error('Something went wrong');

// Error boundary:
<ErrorBoundary fallback={<ErrorPage />}>
  <YourComponent />
</ErrorBoundary>

// API error in component:
if (isError) return <ErrorState message={error.message} />;

Rule:
→ Never show raw error messages to users!
→ Always show user-friendly messages!
→ Always log errors to monitoring (Sentry)
→ Follow EXISTING error handling pattern!
```

---

### 8. Mock Data vs Real Data

```
Why mock data is used:
→ Backend API not ready yet
→ Avoid hitting real API in development
→ Avoid using real user data in tests
→ Work offline without VPN

How to identify:
□ Search "mock" in codebase
□ mocks/ folder exists?
□ __mocks__/ folder exists?
□ MSW (Mock Service Worker) installed?
□ .env.local has USE_MOCK=true?

Common approaches:
// MSW (intercepts API calls):
handlers.ts → defines mock responses
browser.ts  → sets up in browser

// Static JSON files:
data/mockCharges.json → imported in dev

// Feature flag:
if (process.env.NEXT_PUBLIC_USE_MOCK === 'true') {
  return mockData;
}

Ask tech lead:
□ Is there a mock setup for local dev?
□ How do I switch between mock and real?
□ Is there a feature flag to toggle?
□ Which APIs have mocks available?
```

---

### 9. Tech Debt and TODO Comments

```
Find existing tech debt:
□ Search "TODO" in codebase
□ Search "FIXME" in codebase
□ Search "HACK" in codebase
□ Search "TEMP" in codebase

What they mean:
TODO:  → Something to do later
FIXME: → Known bug or issue
HACK:  → Temporary workaround
NOTE:  → Important note to remember

How to handle:
□ Read all TODOs in your feature area
□ Understand WHY they exist
□ Ask before removing or fixing them!
□ Don't add new TODOs without context:
   BAD:  // TODO: fix this
   GOOD: // TODO(M20B-1): fix animation after Figma update

Raising tech debt tickets:
□ Found significant tech debt?
→ Raise a Jira tech debt ticket
→ Label as "tech-debt"
→ Discuss with tech lead
→ Don't fix silently without ticket!

Ask tech lead:
□ How do we handle tech debt here?
□ Is there a tech debt backlog?
□ Do we allocate sprint capacity for it?
```

---

### 10. Code Review — How to Review Others

```
When asked to review someone's PR:

What to check:
□ Does code work as described?
□ Does it follow existing patterns?
□ Are there obvious bugs?
□ Is there missing error handling?
□ Are there security issues?
□ Is code readable and clear?
□ Are tests included?
□ Are there unnecessary console.logs?
□ Does it follow naming conventions?

How to give feedback:
□ Be constructive, not critical
□ Explain WHY you suggest change
□ Use "suggestion:" for optional changes
□ Use "nit:" for very minor things
□ Use "blocker:" for must-fix issues

Examples:
GOOD: "suggestion: we could extract this
       logic into a custom hook to match
       our pattern in useBilling.ts"

BAD:  "this is wrong"

GOOD: "nit: missing semicolon"

GOOD: "blocker: this API key should be in
       env variable not hardcoded"

As new joiner:
□ First few weeks → just read PRs
□ Week 3+ → start giving small reviews
□ Always learn from senior dev reviews
□ Ask if unsure before requesting changes
```

[⬆ Back to Top](#-navigation)

---

## Part 14 — Key Rules

[⬆ Back to Top](#-navigation)

### Never Do List

```
❌ Never start coding without Figma
❌ Never use wrong package manager (check lock file!)
❌ Never commit .env files
❌ Never commit directly to main/develop
❌ Never build something that already exists
❌ Never mix npm and yarn in same project
❌ Never assume — always verify
❌ Never stay stuck for more than 30 mins
❌ Never skip code review feedback
❌ Never push broken code to shared branch
❌ Never change architecture without approval
❌ Never use different patterns than existing code
❌ Never hardcode secrets in code
❌ Never leave console.log in production code
❌ Never skip tests if required
❌ Never deploy without running lint/tests
❌ Never ignore security vulnerabilities
```

### Always Do List

```
✅ Always read README first
✅ Always check lock file for package manager
✅ Always run nvm use before starting
✅ Always connect VPN if needed
✅ Always check if component exists first
✅ Always ask tech lead before big decisions
✅ Always get Figma before coding UI
✅ Always follow existing code patterns
✅ Always commit small and often
✅ Always push code at end of day
✅ Always update Jira ticket status
✅ Always add screenshots to PRs
✅ Always run lint/test/type-check before PR
✅ Always test before creating PR
✅ Always pull latest before starting work
✅ Always ask when stuck for 30 mins
```

### Ask Before Assuming

```
5 minutes asking = saves hours of rewriting!

Ask about:
□ Architecture approach
□ Which pattern to follow
□ Which component to use
□ PR process specifics
□ Any unclear requirement
□ Security requirements
□ Performance requirements
□ Test requirements
```

---

## Part 15 — Glossary

[⬆ Back to Top](#-navigation)

### Repository Terms

| Term | Meaning |
|---|---|
| **Single Repo** | One app, one repository |
| **Monorepo** | Multiple apps, ONE repository |
| **Multirepo** | Multiple apps, multiple repositories |
| **Turborepo** | Monorepo management tool (fast builds) |
| **Nx** | Another monorepo management tool |
| **Workspace** | A package/app inside a monorepo |

### Architecture Terms

| Term | Meaning |
|---|---|
| **Micro Frontend** | App split into independent deployable pieces |
| **Shell/Host** | Container app that loads micro apps |
| **Remote** | Micro app loaded by shell |
| **Module Federation** | Webpack feature enabling micro frontends |
| **Greenfield** | Brand new project from scratch |
| **Brownfield** | Existing project you join mid-way |
| **Monolith** | One large single app |

### Tech Stack Terms

| Term | Meaning |
|---|---|
| **Next.js** | React framework with SSR, file routing |
| **App Router** | New Next.js routing using /app folder |
| **Server Component** | Runs on server, no JS to browser |
| **Client Component** | Runs in browser, needs 'use client' |
| **Redux Toolkit** | State management library |
| **Zustand** | Simpler state management |
| **React Query** | Server state / API data caching |
| **Tailwind** | Utility-first CSS framework |
| **Shadcn/ui** | Component library built on Radix UI |
| **Stencil** | Compiler for Web Components |
| **Web Component** | Custom HTML element works anywhere |
| **Axios** | HTTP client for API calls |
| **pnpm** | Fast package manager for monorepos |
| **Vite** | Fast build tool / dev server |
| **Webpack** | Build tool / bundler |
| **TypeScript** | JavaScript with static types |
| **ESLint** | Code linting tool |
| **Prettier** | Code formatting tool |
| **Husky** | Git hooks manager |
| **nvm** | Node Version Manager |

### Git Terms

| Term | Meaning |
|---|---|
| **Clone** | Download repo to local machine |
| **Branch** | Separate line of development |
| **Commit** | Snapshot of changes |
| **Push** | Upload commits to remote |
| **Pull** | Download latest from remote |
| **PR/MR** | Request to merge your branch |
| **Merge conflict** | Two branches changed same code |
| **develop** | Integration branch |
| **main/master** | Production branch |
| **feature branch** | Branch for one feature |
| **hotfix branch** | Branch for urgent production fix |
| **SSH** | Secure Git authentication |
| **SAML SSO** | Company single sign-on auth |
| **Cherry-pick** | Apply specific commit to another branch |

### Environment Terms

| Term | Meaning |
|---|---|
| **Local** | Your laptop (localhost:3000) |
| **Dev** | Shared development environment |
| **SIT** | System Integration Testing |
| **UAT** | User Acceptance Testing |
| **Staging** | Production mirror for final testing |
| **DR** | Disaster Recovery environment |
| **Production/Prod** | Live environment (real users!) |
| **VPN** | Secure tunnel to company network |
| **.env.local** | Local environment variables (never commit!) |
| **CI/CD** | Automated build and deployment pipeline |
| **Rollback** | Revert to previous working deployment |

### Agile/SAFe Terms

| Term | Meaning |
|---|---|
| **Sprint** | 2-week work cycle |
| **PI** | Program Increment (12 weeks) |
| **Epic** | Large feature spanning multiple sprints |
| **Story** | Small feature done in one sprint |
| **Acceptance Criteria** | Conditions for story to be complete |
| **Story Points** | Effort estimate |
| **Velocity** | Story points completed per sprint |
| **PO** | Product Owner |
| **SM** | Scrum Master |
| **RTE** | Release Train Engineer |
| **ART** | Agile Release Train |
| **PI Planning** | 2-day planning event for 12 weeks |
| **Standup** | Daily 15-min sync |
| **Retrospective** | End of sprint improvement meeting |
| **KT** | Knowledge Transfer session |
| **Jira** | Project tracking tool |
| **Confluence** | Documentation/wiki tool |
| **SAFe** | Scaled Agile Framework |

### Bug/Incident Terms

| Term | Meaning |
|---|---|
| **P1** | Critical — production down |
| **P2** | High — major feature broken |
| **P3** | Medium — partially broken |
| **P4** | Low — minor issue |
| **Hotfix** | Urgent fix deployed directly to production |
| **Incident** | Unplanned production issue |
| **Post-mortem** | Analysis after incident |
| **SLA** | Service Level Agreement (response time) |
| **On-call** | Developer on duty for production issues |

### Additional Terms

| Term | Meaning |
|---|---|
| **Feature Flag** | Switch to enable/disable features without deploying |
| **LaunchDarkly** | Popular feature flag management tool |
| **Docker** | Container platform for running apps consistently |
| **docker-compose** | Tool to run multiple Docker containers together |
| **BFF** | Backend For Frontend — server layer owned by frontend team |
| **i18n** | Internationalization — supporting multiple languages |
| **i18next** | Popular i18n library for React |
| **MSW** | Mock Service Worker — intercepts API calls for testing |
| **Tech Debt** | Code shortcuts that need future improvement |
| **TODO** | Code comment marking something to fix later |
| **FIXME** | Code comment marking known bug |
| **browserslist** | Config defining which browsers to support |
| **caniuse.com** | Website to check browser support for CSS/JS |
| **Error Boundary** | React component catching errors in child components |
| **Sentry** | Error monitoring and tracking tool |
| **npm audit** | Command to check for security vulnerabilities |
| **Cherry-pick** | Apply specific commit to another branch |
| **nvm** | Node Version Manager — switch Node versions per project |

[⬆ Back to Top](#-navigation)

---

*Developer Onboarding Guide — For Any Company or Project*
*Updated: May 2026*