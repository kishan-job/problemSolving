# Developer Onboarding — Crash Course
### Join Any Company or Project in 5 Days

---

## 📌 Navigation
- [Before You Start](#before-you-start)
- [Day 1 — Access and Setup](#day-1--access-and-setup)
- [Day 2 — Understand the Project](#day-2--understand-the-project)
- [Day 3 — Understand the Codebase](#day-3--understand-the-codebase)
- [Day 4 — Talk to Team and Setup Local](#day-4--talk-to-team-and-setup-local)
- [Day 5 — Before First Code](#day-5--before-first-code)
- [Week 1 — Start Coding](#week-1--start-coding)
- [Daily Routine](#daily-routine)
- [Key Rules](#key-rules)
- [Quick Reference Tables](#quick-reference-tables)

---

## Before You Start

[⬆ Back to Top](#-navigation)

### Know Your Context First

| Company Type | Expect |
|---|---|
| Startup | Fast, informal, less docs |
| Mid Size | Some process, ask directly |
| Enterprise (DirecTV) | VPN, Jira, Confluence, SAFe, approvals |

| Project Type | Your Focus |
|---|---|
| Greenfield | Setup from scratch |
| Brownfield | Understand existing first! |
| Legacy Migration | Understand old + new |

```
Rule: Brownfield = NEVER change patterns without asking!
```

---

## Day 1 — Access and Setup

[⬆ Back to Top](#-navigation)

### Get These Accesses

```
□ Company email
□ Teams / Slack
□ Jira
□ Confluence
□ GitHub / GitLab (added to org + team + repos)
□ Figma
□ VPN ← critical!
□ Dev/SIT/UAT environment URLs + credentials
□ Storybook URL
□ Swagger/API docs URL
□ Password manager
```

### Install These Tools

```
□ Git
□ Node.js LTS
□ nvm (Node Version Manager) ← must have!
□ VS Code
□ Chrome
□ Postman
□ Docker Desktop (if project uses Docker)
□ pnpm → npm install -g pnpm (if monorepo)
```

### VS Code Extensions

```
□ ESLint
□ Prettier
□ GitLens
□ ES7+ React Snippets
□ Tailwind IntelliSense (if Tailwind)
□ Error Lens
□ Import Cost
```

### Git + SSH Setup

```bash
git config --global user.name "Your Name"
git config --global user.email "work@company.com"
git config --global core.editor "code --wait"

ssh-keygen -t ed25519 -C "work@company.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub | clip   # copy to GitHub SSH keys

ssh -T git@github.com              # test connection!

# Enterprise GitHub (SAML SSO):
# Settings → SSH Keys → Configure SSO → Authorize!
```

### VPN Setup

```
□ Ask IT: what VPN software? (Cisco AnyConnect?)
□ Install VPN
□ Connect before: cloning repos, accessing dev/SIT/UAT URLs, internal APIs

Cannot connect → check MFA code + credentials
Connected but no access → ask tech lead for permissions
```

### Node Version Setup

```bash
# Check required version:
cat .nvmrc                    # or check package.json engines field

# Switch version:
nvm install 18.17.0
nvm use                       # auto reads .nvmrc!
node --version                # verify!
```

### Day 1 Checklist

```
□ Email working
□ Teams/Slack joined
□ Jira accessible
□ Confluence accessible
□ GitHub access confirmed
□ All tools installed
□ nvm installed
□ Git configured + SSH tested
□ VPN installed and tested
```

---

## Day 2 — Understand the Project

[⬆ Back to Top](#-navigation)

### Step 1 — How Many Repos?

```
One URL given   → Single Repo or Monorepo
Multiple URLs   → Multirepo → clone each separately!
```

### Step 2 — Identify Repo Type

| Sign | Type |
|---|---|
| `turbo.json` exists | Turborepo Monorepo |
| `nx.json` exists | Nx Monorepo |
| `apps/` + `packages/` folders | Monorepo |
| `pnpm-workspace.yaml` | Monorepo |
| One `package.json`, simple structure | Single Repo |
| Multiple repo URLs given | Multirepo |

```bash
# Monorepo commands:
pnpm install          # install all apps at once
pnpm dev              # run all apps
pnpm dev --filter web # run only web app

# Single Repo:
npm install && npm run dev
```

### Step 3 — Identify Architecture

```
Ask tech lead first! Then check:

□ shell/ or host/ app exists?      → Micro Frontend!
□ "dev:shell" in scripts?          → Micro Frontend!
□ ModuleFederationPlugin in webpack? → Micro Frontend!
□ Only one app folder?             → Single App

4 Combos:
→ Monorepo + Single App     = One repo, one website
→ Monorepo + Micro Frontend = One repo, ONE website (multiple apps!)
→ Multirepo + Single App    = Separate repos, SEPARATE websites
→ Multirepo + Micro Frontend= Separate repos, ONE website combined
```

### Step 4 — Find Tech Stack

```
CHECK package.json dependencies:

Package manager:  package-lock.json=npm  yarn.lock=yarn  pnpm-lock.yaml=pnpm
Framework:        "next"=Next.js  "react"=React  "@angular/core"=Angular
Build tool:       webpack.config.js  vite.config.ts  turbo.json
Language:         .tsx/.ts files=TypeScript  tsconfig.json=TypeScript
State mgmt:       "@reduxjs/toolkit"=Redux  "zustand"=Zustand  "@tanstack/react-query"=React Query
CSS:              "tailwindcss"=Tailwind  "styled-components"  *.module.css=CSS Modules
Component lib:    "@mui/material"=MUI  "class-variance-authority"=Shadcn  "@stencil/core"=Stencil
API:              "axios"  "@apollo/client"=GraphQL  app/api/ folder=Next.js routes
Auth:             "next-auth"  "@auth0/nextjs-auth0"  middleware.ts exists
Testing:          "jest"  "cypress"  "playwright"  .storybook/ folder
CI/CD:            .github/workflows/  Jenkinsfile  azure-pipelines.yml
Analytics:        "@sentry/nextjs"=errors  "@segment/analytics-next"  "@datadog/browser-rum"

SEARCH codebase (Ctrl+Shift+F):
"useSelector"  → Redux
"useQuery("    → React Query
"create("      → Zustand
"useContext"   → Context API
```

### Step 5 — Find Environments

```
WHERE to find URLs:
□ README.md → environments section
□ Confluence → project space → environments page
□ .github/workflows/deploy.yml → shows branch → env mapping
□ .env.sit / .env.uat files in repo
□ Ask tech lead! (fastest)

BRANCH → ENVIRONMENT mapping:
develop → DEV
release → SIT
(QA approves) → UAT
main → PRODUCTION

ENVIRONMENT types:
Local → localhost:3000 (you only)
Dev   → all devs test here
SIT   → System Integration Testing (QA)
UAT   → User Acceptance Testing (business)
Prod  → real users!
```

### Step 6 — Find API Docs and Code Standards

```
API docs:
□ /api-docs URL → Swagger!
□ swagger.json or openapi.yaml in repo
□ Ask: is there Postman collection?

Code standards:
□ .eslintrc.json → linting rules
□ .prettierrc → formatting
□ CONTRIBUTING.md → contribution guide
□ Ask senior dev for unwritten rules!

Naming conventions (common):
Components → PascalCase (SideSheet.tsx)
Hooks      → camelCase with use (useBilling.ts)
Utils      → camelCase (formatCurrency.ts)
Folders    → feature based (billing/, auth/)
```

---

## Day 3 — Understand the Codebase

[⬆ Back to Top](#-navigation)

### Read Files In This Order

```
1. README.md              → setup, architecture, decisions
2. package.json           → all dependencies + scripts
3. next.config.js         → redirects, rewrites, config
4. .env.example           → all env vars needed
5. tsconfig.json          → TypeScript + path aliases (@/)
6. app/layout.tsx         → ALL providers! (Redux, React Query, Auth)
7. lib/providers.tsx      → provider details
8. middleware.ts          → auth guards
9. store/index.ts         → Redux store (if used)
10. lib/api.ts            → API client base config
11. services/*.ts         → service functions
12. Your feature page     → start here for your work!
```

### Search Strategies (Ctrl+Shift+F)

```
Find your feature:
□ Search button text      → "View Current Charges"
□ Search route            → "current-charges"
□ Search component name   → "ViewCharges"

Find patterns:
□ F12 / Ctrl+Click        → Go to definition
□ Shift+F12               → Find all usages
□ Alt+←                   → Go back
□ Ctrl+P                  → Open file by name
```

### Draw on Paper!

```
Draw these BEFORE coding:
□ Provider tree (outermost → innermost)
□ Component tree (page → components → subcomponents)
□ Data flow (user action → state → re-render)
□ API flow (component → hook → service → API route → external)
```

### Check Tests, Design System, Feature Flags

```
Tests:
□ __tests__/ or *.test.tsx files
□ npm run test → run tests
□ npm run test:coverage → see coverage %
□ Ask: coverage requirement? (80%? 90%?)

Design System:
□ Storybook URL → see ALL components!
□ Check BEFORE building anything!
□ packages/@company/ui → custom library?

Feature Flags:
□ Search "LaunchDarkly" or "featureFlag" in code
□ Ask: do we use feature flags?
□ Ask: how to enable flag locally?

Mock Data:
□ mocks/ folder exists?
□ MSW installed?
□ Ask: how to switch mock ↔ real?

i18n:
□ Search "i18next" or "t(" in code
□ If found: NEVER hardcode text strings!
□ Always add to translation files!
```

### Project Management — Jira and Confluence

```
Jira reading order:
1. Summary tab   → project health
2. Goals tab     → WHY project exists
3. Timeline tab  → epics and schedule
4. Backlog tab   → all stories
5. Your stories  → read acceptance criteria!

Confluence reading order:
1. Onboarding page
2. Project overview
3. Architecture docs
4. Environment URLs + credentials
5. API documentation
6. Team contacts
```

---

## Day 4 — Talk to Team and Setup Local

[⬆ Back to Top](#-navigation)

### Questions for Each Person

```
PO (Product Owner):
□ Figma link?
□ Which story to start first?
□ Mobile breakpoints?
□ Sprint start date?
□ Designer contact?

Tech Lead:
□ Git repo URL?
□ Branching strategy?
□ .env values?
□ Who reviews PRs?
□ Existing component for my feature?
□ State management to use?
□ API route already exists?
□ Mock for external APIs?
□ Node version?
□ Tests required? Coverage %?

Scrum Master:
□ Standup time?
□ Sprint length?
□ How to report blockers?

Senior Dev:
□ Common gotchas?
□ Patterns to follow?
□ What to avoid?
□ Deprecated patterns in code?

QA:
□ Test environment URL?
□ Test credentials?
□ Bug severity levels?
□ Definition of done?
```

### Request KT Sessions

```
Request these from senior dev / tech lead:
□ Architecture overview KT
□ Codebase walkthrough KT
□ Your feature area KT
□ Deployment process KT

During KT:
□ Write EVERYTHING down!
□ Ask "why" after each decision
□ Record if allowed
```

### Clone and Run Project

```bash
# Single Repo:
git clone git@github.com:org/repo.git
cd repo && nvm use && npm install && npm run dev

# Monorepo:
git clone git@github.com:org/repo.git
cd repo && nvm use && pnpm install && pnpm dev

# Multirepo (clone each separately):
git clone git@github.com:org/shell.git
git clone git@github.com:org/billing.git
# Then in each: nvm use && npm install && npm run dev

# See all branches:
git branch -a

# Setup env:
cp .env.example .env.local
code .env.local   # fill values (ask tech lead!)
```

### Understand package.json Scripts

```
"dev"           → start dev server
"build"         → production build
"lint"          → check errors ← run before PR!
"type-check"    → TypeScript check ← run before PR!
"test"          → run tests ← run before PR!
"test:coverage" → see coverage %
"format"        → format code
"e2e"           → E2E tests
```

### Debugging Setup

```
Install Chrome extensions:
□ React Developer Tools → see component props/state
□ Redux DevTools → see actions + state (time travel!)

Network tab (F12):
□ XHR/Fetch filter → see API calls
□ 200=OK  401=Auth problem  404=Wrong URL  500=Server error

VS Code debugger:
□ Create .vscode/launch.json
□ Set breakpoints (click line number)
□ F5 to start debugging
```

---

## Day 5 — Before First Code

[⬆ Back to Top](#-navigation)

### Must Have Before Writing Single Line of Code

```
Design:
□ Figma link received ← CANNOT start without!
□ Mobile + desktop designs
□ Loading, error, empty state designs
□ All breakpoints defined

Technical:
□ Project running locally
□ nvm use done (correct Node version!)
□ VPN connected if needed
□ Know which state management to use
□ Know which CSS approach to use
□ Checked Storybook → component exists?
□ Searched → API route exists?
□ Tech lead approved your approach!

Process:
□ Know branch naming convention
□ Know commit message format
□ Know who reviews PRs
□ Know which scripts to run before PR
```

### Check if Code Exists First!

```
BEFORE building:
□ Search component name → already exists?
□ Check Storybook → in design system?
□ Check packages/ → shared component?
□ Search API endpoint → route exists?

RULE: Reuse > Modify > Build from scratch!
```

### Create Feature Branch

```bash
git checkout develop && git pull origin develop
git checkout -b feature/TICKET-description
# Example: feature/M20B-1-side-sheet-component
```

---

## Week 1 — Start Coding

[⬆ Back to Top](#-navigation)

### Daily Git Workflow

```bash
# Morning:
git pull origin develop

# During work:
git add . && git commit -m "feat(TICKET): description"
git push origin branch-name

# End of day:
git push origin branch-name   ← backup!
```

### Commit Message Format

```
feat(TICKET):     new feature
fix(TICKET):      bug fix
style(TICKET):    CSS/styling
test(TICKET):     adding tests
a11y(TICKET):     accessibility
chore(TICKET):    build/config
refactor(TICKET): code change
```

### Before Creating PR — Run These!

```bash
npm run lint         ← fix all lint errors!
npm run type-check   ← fix all TypeScript errors!
npm run test         ← all tests must pass!
npm run build        ← build must succeed!
```

### PR Checklist

```
□ What changed (description)
□ Jira ticket link
□ Screenshots (desktop + mobile!)
□ Testing done checklist
□ Add reviewer (tech lead)
```

### Code Review (Giving Feedback)

```
Use prefixes:
"blocker:"    → must fix before merge
"suggestion:" → optional improvement
"nit:"        → very minor thing

GOOD: "suggestion: extract to custom hook to match useBilling.ts pattern"
BAD:  "this is wrong"

New joiner: just READ PRs first 2 weeks, then start reviewing week 3+
```

### Security + Performance + Accessibility

```
Security:
❌ Never hardcode API keys or passwords
❌ Never commit .env files
❌ Never console.log sensitive data
✅ Always use environment variables
✅ Run: npm audit

Performance (ask tech lead for targets):
□ Lighthouse > 90
□ LCP < 2.5s, CLS < 0.1
□ Check bundle size: npm run build

Accessibility (WCAG AA):
□ Color contrast 4.5:1
□ Keyboard navigation works
□ ARIA labels on icon buttons
□ axe DevTools extension to check
```

---

## Daily Routine

[⬆ Back to Top](#-navigation)

```
Morning:
□ Check Teams/Slack
□ Connect VPN
□ git pull origin develop
□ Check Jira tickets
□ Attend standup → say: yesterday / today / blockers

During day:
□ Commit every few hours
□ Stuck 30 mins → ask for help!
□ Update Jira ticket status

End of day:
□ Push all code (backup!)
□ Update Jira progress
□ Note blockers for tomorrow

Weekly:
□ Monday: review sprint board
□ Friday: push all, clean branches
□ Always: read merged PRs (learn!)
```

### First Month Goals

| Week | Goal |
|---|---|
| Week 1 | All access, project running, first commit |
| Week 2 | First story done, first PR merged |
| Week 3 | Second story, contributing in standups |
| Week 4 | Independent work, reviewing others PRs |

---

## Key Rules

[⬆ Back to Top](#-navigation)

### Never

```
❌ Start coding without Figma
❌ Wrong package manager (check lock file!)
❌ Commit .env files
❌ Commit to main/develop directly
❌ Build something that already exists
❌ Mix npm and yarn in same project
❌ Hardcode secrets in code
❌ Leave console.log in production
❌ Change architecture without approval
❌ Use different patterns than existing code
❌ Stay stuck more than 30 mins without asking
```

### Always

```
✅ Read README first
✅ Check lock file for package manager
✅ Run nvm use before starting
✅ Connect VPN if needed
✅ Check Storybook before building
✅ Ask tech lead before big decisions
✅ Get Figma before coding UI
✅ Follow existing code patterns
✅ Run lint/test/type-check before PR
✅ Push code end of every day
✅ Update Jira ticket status
✅ Ask when stuck for 30 mins
```

---

## Quick Reference Tables

[⬆ Back to Top](#-navigation)

### Repo Type Identification

| File/Folder | Repo Type |
|---|---|
| `turbo.json` | Turborepo Monorepo |
| `nx.json` | Nx Monorepo |
| `apps/` + `packages/` | Monorepo |
| Multiple repo URLs | Multirepo |
| Simple `src/` or `app/` | Single Repo |

### Tech Stack Quick Lookup

| Package | Technology |
|---|---|
| `next` | Next.js |
| `@reduxjs/toolkit` | Redux |
| `zustand` | Zustand |
| `@tanstack/react-query` | React Query |
| `tailwindcss` | Tailwind CSS |
| `class-variance-authority` | Shadcn/ui |
| `@stencil/core` | Stencil |
| `next-auth` | NextAuth |
| `axios` | Axios HTTP client |
| `@sentry/nextjs` | Sentry (error monitoring) |
| `i18next` | Internationalization |

### Environment Names

| Type | Environments |
|---|---|
| Startup | local → dev → prod |
| Mid Size | local → dev → QA → staging → prod |
| Enterprise | local → dev → SIT → UAT → staging → prod |
| Banks | local → dev → SIT → UAT → DR → pre-prod → prod |

### Git Branch Naming

| Type | Format |
|---|---|
| Feature | `feature/TICKET-description` |
| Bug fix | `bugfix/TICKET-description` |
| Hotfix | `hotfix/TICKET-description` |
| Cleanup | `chore/TICKET-description` |

### Bug Severity

| Level | Meaning | Response |
|---|---|---|
| P1 | Production down | Immediate! |
| P2 | Major feature broken | Same day |
| P3 | Partially broken | This sprint |
| P4 | Minor issue | Next sprint |

### Key Glossary

| Term | Meaning |
|---|---|
| Monorepo | Multiple apps in ONE repo |
| Multirepo | Multiple apps in SEPARATE repos |
| Micro Frontend | App split into independent deployable pieces |
| Brownfield | Existing project you join mid-way |
| KT | Knowledge Transfer session |
| SIT | System Integration Testing |
| UAT | User Acceptance Testing |
| BFF | Backend For Frontend |
| Feature Flag | Switch features ON/OFF without deploying |
| Tech Debt | Code that needs future improvement |
| nvm | Node Version Manager |
| VPN | Secure tunnel to company network |
| CI/CD | Automated build and deploy pipeline |
| PR | Pull Request — request to merge code |
| WCAG | Web accessibility standards |

[⬆ Back to Top](#-navigation)

---

*Developer Onboarding Crash Course — May 2026*