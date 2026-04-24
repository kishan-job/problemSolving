# React Practice Guide — All Prompts + Study Plans

---

# Table of Contents

- [Quick Decision — Which Prompt to Use](#quick-decision--which-prompt-to-use)
- [Prompt 1 — Interview Prep (Option A)](#prompt-1--interview-prep-option-a)
- [Prompt 2 — Senior / Architecture (Option B)](#prompt-2--senior--architecture-option-b)
- [Prompt 3 — Real Product Builder](#prompt-3--real-product-builder)
- [Master Prompt — All Purposes Combined](#master-prompt--all-purposes-combined)
- [What Each Prompt Includes](#what-each-prompt-includes)
- [How Many Days Each Level Takes](#how-many-days-each-level-takes)
- [Realistic Study Plans](#realistic-study-plans)
- [The Honest Truth](#the-honest-truth)
- [How to Generate Parts](#how-to-generate-parts)

---

# Quick Decision — Which Prompt to Use

```
"I have an interview coming up"
  → Prompt 1 — Interview Prep

"I want to think like a senior dev / architect"
  → Prompt 2 — Senior / Architecture

"I want to build real portfolio features"
  → Prompt 3 — Product Builder

"I want one guide that covers everything"
  → Master Prompt
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Prompt 1 — Interview Prep (Option A)

> **Use when:** Preparing for interviews, learning React,
> need to practice building specific features.
> You know exactly what you're practicing.
> "I need to build a form" → go to Pattern 2.

```
You are building a complete scenario-based React + JavaScript
practice guide for developers preparing for interviews.
Category names are task-based — what you BUILD, not theory.

═══════════════════════════════════════════════
FULL STRUCTURE — ALL 4 LEVELS
═══════════════════════════════════════════════

LEVEL 1 — FOUNDATION (80% of interview questions)
  Pattern 1:  📋 Fetch & Display Data
  Pattern 2:  📝 Form + Validation
  Pattern 3:  🔁 List + Search + Filter
  Pattern 4:  💾 localStorage Usage
  Pattern 5:  🔀 Show/Hide UI
  Pattern 6:  🧭 Page Navigation

LEVEL 2 — INTERMEDIATE (15% of interview questions)
  Pattern 7:  ♾️  Pagination / Infinite Scroll
  Pattern 8:  🔔 Toast / Notifications
  Pattern 9:  🛒 Shopping Cart
  Pattern 10: 🌙 Dark Mode / User Preferences
  Pattern 11: 🔐 Auth Flow

LEVEL 3 — ADVANCED (4% of interview questions)
  Pattern 12: ⚡ Performance Optimization
  Pattern 13: 📡 Real-time / Polling
  Pattern 14: 🔄 Complex Async

LEVEL 4 — SPECIALIST (1% — senior interviews only)
  Pattern 15: 🕳️  Memory Leaks
  Pattern 16: 👻 Stale Closures
  Pattern 17: 🔁 Infinite Render Loops
  Pattern 18: 🔑 Token Expiry + Silent Refresh
  Pattern 19: 📦 Context Performance at Scale

═══════════════════════════════════════════════
FOR EACH PATTERN — HEADER FORMAT
═══════════════════════════════════════════════

## Pattern [N]: [Emoji] [Name]
> Level [1/2/3/4]

🎯 What you will build in this pattern:
[3 bullet points — real features from real job tasks]

📋 Interview asks this as:
[3 bullet points — exact ways interviewers phrase this]

🚨 Most common mistake in interviews:
[The one thing that makes interviewers mark you down]

═══════════════════════════════════════════════
FOR EACH SCENARIO — FORMAT
═══════════════════════════════════════════════

---
### 🔴 Pattern [N] · Scenario [N] — [Problem Title]

🏢 Situation:
[2-3 lines. You are in an interview or at a company.
Something needs to be built or is broken.
Make it feel like a real task you would be given.]

Here is the starter code / broken code:

[Realistic code. Real variable names. Real API shapes.
NOT toy counter examples.]

Your tasks:
  ✅ [specific thing to build or fix]
  ✅ [specific thing to build or fix]
  ✅ [specific thing to build or fix]

<details>
<summary>💡 Hint</summary>

[One key insight that points in the right direction.
Do not give the answer.]

</details>

<details>
<summary>✅ Full Solution</summary>

[Complete working code with inline comments.]

✅ What the interviewer is checking:
[2 lines — what specifically they want to see
in your solution to pass this question]

</details>
---

═══════════════════════════════════════════════
SCENARIOS PER PATTERN
═══════════════════════════════════════════════

LEVEL 1 → 4 scenarios each
LEVEL 2 → 4 scenarios each
LEVEL 3 → 3 scenarios each
LEVEL 4 → 2 scenarios each

═══════════════════════════════════════════════
RULES
═══════════════════════════════════════════════

- Code must look like real interview starter code
- Scenarios go: easy → medium → hard → tricky edge case
- React functional components + hooks only
- async/await not .then() chains
- Clickable TOC at top
- "↑ Back to TOC" after every pattern
- Level dividers between levels

END WITH:
  Interview Pattern Picker:
  "Interviewer asks [X]" → "Go to Pattern [N]"
  At least 20 entries.

═══════════════════════════════════════════════
HOW TO GENERATE
═══════════════════════════════════════════════

Generate one part at a time.
Wait for instruction before generating next part.

PARTS:
  "Generate Part 1 — Level 1, Patterns 1 to 6"
  "Generate Part 2 — Level 2, Patterns 7 to 11"
  "Generate Part 3 — Level 3, Patterns 12 to 14"
  "Generate Part 4 — Level 4, Patterns 15 to 19"

Or generate one pattern only:
  "Generate only Pattern [N] — [Name].
   All scenarios. Complete. No placeholders."

OUTPUT: Complete markdown. No placeholders.
Every scenario fully written. Every solution working.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Prompt 2 — Senior / Architecture (Option B)

> **Use when:** Thinking about system design, preparing for
> senior roles, want to build mental models not just feature memory.
> It builds the WHY behind the code, not just the HOW.

```
You are building a complete scenario-based React + JavaScript
practice guide that teaches developers to THINK in patterns.
Not just what to build — how to THINK about what to build.

═══════════════════════════════════════════════
FULL STRUCTURE — ALL 4 LEVELS
═══════════════════════════════════════════════

LEVEL 1 — FOUNDATION (80% of real work)
  Pattern 1:  📋 Data Display Pattern
  Pattern 2:  📝 Form Flow Pattern
  Pattern 3:  🔀 Conditional UI Pattern
  Pattern 4:  🔁 List Operations Pattern
  Pattern 5:  💾 State Sync Pattern
  Pattern 6:  🧭 Navigation Flow Pattern

LEVEL 2 — INTERMEDIATE (15% of real work)
  Pattern 7:  ⚡ Performance Pattern
  Pattern 8:  🔄 Async Pattern
  Pattern 9:  🧩 Component Architecture Pattern
  Pattern 10: 🌐 Global State Pattern
  Pattern 11: 🔌 Side Effect Pattern

LEVEL 3 — ADVANCED (4% of real work)
  Pattern 12: 🔐 Auth Flow Pattern
  Pattern 13: ♻️  Reusability Pattern
  Pattern 14: 📡 Real-time Pattern

LEVEL 4 — SPECIALIST (1% of real work)
  Pattern 15: 🕳️  Memory Leak Pattern
  Pattern 16: 👻 Stale Closure Pattern
  Pattern 17: 🔁 Infinite Loop Pattern
  Pattern 18: 🔑 Token Management Pattern
  Pattern 19: 📦 Context Scale Pattern

═══════════════════════════════════════════════
FOR EACH PATTERN — HEADER FORMAT
═══════════════════════════════════════════════

## Pattern [N]: [Emoji] [Name]
> Level [1/2/3/4] — [Foundation/Intermediate/Advanced/Specialist]

🧠 The Mental Model:
[How a senior dev THINKS about this pattern.
Not a definition — a thinking approach.
The question they ask themselves when they see this problem.
2-3 lines.]

⚡ The One Rule:
[Single sentence. Solves 80% of problems in this pattern.
This is what separates junior from senior thinking.]

🚨 What junior devs always get wrong:
[The specific wrong mental model, not just the wrong code.
WHY they get it wrong, not just WHAT they get wrong.]

📍 You need this pattern when:
  - [Real situation 1]
  - [Real situation 2]
  - [Real situation 3]

🔗 This pattern connects to:
[Which other patterns this one builds on or leads to]

═══════════════════════════════════════════════
FOR EACH SCENARIO — FORMAT
═══════════════════════════════════════════════

---
### Pattern [N] · Scenario [N] — [Problem Title]

🏢 Situation:
[2-3 lines. Real company. Real person affected.
Real business consequence. Feel like a real decision
a senior dev has to make.]

Here is the current approach:

[Realistic code that works but is architecturally wrong,
OR is incomplete in a way that matters at scale.
NOT just syntax errors — wrong thinking.]

❓ What is wrong with this approach and why does it
   break down in production?

Your tasks:
  ✅ [architectural fix or decision]
  ✅ [architectural fix or decision]
  ✅ [architectural fix or decision]

<details>
<summary>💡 Hint — How to think about this</summary>

[Do NOT give the answer. Give the THINKING DIRECTION.
Ask the question that unlocks the right mental model.
"Ask yourself: who owns this state?"
"Ask yourself: what happens when this scales to 10x?"]

</details>

<details>
<summary>✅ Full Solution + Architecture Explanation</summary>

[Complete working code with inline comments.]

🧠 The principle at work:
[Connect the fix back to the pattern's One Rule.
1-2 lines.]

📐 How this scales:
[What happens when this pattern is applied correctly
as the app grows. 2 lines.]

🔍 What a senior dev spots in 10 seconds:
[The exact signal — not the symptom but the ROOT CAUSE
signal — that makes experienced devs immediately
know what's wrong.]

⚠️  The alternative that looks correct but isn't:
[The tempting wrong solution and why it fails later.]

</details>
---

═══════════════════════════════════════════════
SCENARIOS PER PATTERN
═══════════════════════════════════════════════

LEVEL 1 → 4 scenarios each (naive → scaled thinking)
LEVEL 2 → 4 scenarios each (works now → breaks at scale)
LEVEL 3 → 3 scenarios each (wrong architecture → right one)
LEVEL 4 → 2 scenarios each (invisible bug → production incident)

═══════════════════════════════════════════════
RULES
═══════════════════════════════════════════════

- Problems must be about WRONG THINKING not just wrong syntax
- Solutions must explain the PRINCIPLE not just the fix
- Every scenario must change how the developer thinks
- Companies: e-commerce, SaaS, fintech, social, healthcare
- React functional components + hooks only
- Clickable TOC at top
- "↑ Back to TOC" after every pattern
- Level dividers between levels

END WITH:
  Mental Model Picker:
  "I keep thinking [wrong mental model]"
  → "Read Pattern [N] — here is why that thinking breaks"
  At least 15 entries.

═══════════════════════════════════════════════
HOW TO GENERATE
═══════════════════════════════════════════════

Generate one part at a time.
Wait for instruction before generating next part.

PARTS:
  "Generate Part 1 — Level 1, Patterns 1 to 6"
  "Generate Part 2 — Level 2, Patterns 7 to 11"
  "Generate Part 3 — Level 3, Patterns 12 to 14"
  "Generate Part 4 — Level 4, Patterns 15 to 19"

Or generate one pattern only:
  "Generate only Pattern [N] — [Name].
   All scenarios. Complete. No placeholders."

OUTPUT: Complete markdown. No placeholders.
Every scenario fully written. Every solution working.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Prompt 3 — Real Product Builder

> **Use when:** You want to practice building complete real
> features end to end. Not isolated exercises — full features
> a real company ships to real users. Good for portfolio projects.

```
You are building a complete scenario-based React + JavaScript
practice guide where every scenario is a REAL PRODUCT FEATURE
— not a concept demo, not an isolated exercise.

Every scenario = something a real company ships to real users.

═══════════════════════════════════════════════
FULL STRUCTURE — ALL 4 LEVELS
═══════════════════════════════════════════════

LEVEL 1 — FOUNDATION (80% of real product features)
  Pattern 1:  📋 Data Loading Feature
              → product page, user list, dashboard widget
  Pattern 2:  📝 Form Feature
              → login, signup, contact, settings update
  Pattern 3:  🔁 List Feature
              → search, filter, sort, paginate
  Pattern 4:  💾 Persistence Feature
              → cart, preferences, draft saving
  Pattern 5:  🔀 Interactive UI Feature
              → modal, drawer, tabs, accordion, tooltip
  Pattern 6:  🧭 Navigation Feature
              → protected pages, redirects, breadcrumbs

LEVEL 2 — INTERMEDIATE (15% of product features)
  Pattern 7:  ♾️  Load More Feature
              → pagination, infinite scroll, load more button
  Pattern 8:  🔔 Feedback Feature
              → toasts, banners, inline errors, confirmations
  Pattern 9:  🛒 Cart Feature
              → add, remove, update, persist, calculate
  Pattern 10: 🎨 Theme Feature
              → dark mode, font size, color scheme, persist
  Pattern 11: 🔐 Auth Feature
              → full login → protected → refresh → logout flow

LEVEL 3 — ADVANCED (4% of product features)
  Pattern 12: ⚡ Optimization Feature
              → debounce, virtualize, memo, code split
  Pattern 13: 📡 Live Feature
              → real-time updates, polling, websocket
  Pattern 14: 🔄 Resilient Async Feature
              → retry, race conditions, loading states

LEVEL 4 — SPECIALIST (1% — breaks production)
  Pattern 15: 🕳️  Leak Fix
  Pattern 16: 👻 Closure Fix
  Pattern 17: 🔁 Loop Fix
  Pattern 18: 🔑 Token Fix
  Pattern 19: 📦 Scale Fix

═══════════════════════════════════════════════
FOR EACH PATTERN — HEADER FORMAT
═══════════════════════════════════════════════

## Pattern [N]: [Emoji] [Name]
> Level [1/2/3/4]

🏭 Real products that have this feature:
[3 real apps and how they use this pattern.
Example: "Spotify uses this for their search bar.
Amazon uses this for their product filters."]

📦 What a complete version of this feature includes:
[5-7 bullet points — the full checklist of what
makes this feature production-ready, not demo-ready]

⚠️  What makes the demo version different from the real version:
[The 2-3 things that demos always skip but real
products always need]

═══════════════════════════════════════════════
FOR EACH SCENARIO — FORMAT
═══════════════════════════════════════════════

---
### 🔴 Pattern [N] · Scenario [N] — [Feature Title]

🏢 Product Brief:
[Write this like a real product requirement.
Company type. What the feature does. Who uses it.
What done looks like. 3-4 lines.]

Current state of the code:

[Realistic incomplete or broken feature code.
Real component names. Real data shapes.
Looks like something a dev started but did not finish
OR finished incorrectly.]

🎯 Definition of Done:
  ✅ [specific requirement]
  ✅ [specific requirement]
  ✅ [specific requirement]
  ✅ [edge case that must be handled]

<details>
<summary>💡 Hint</summary>
[Point toward what is missing or wrong without
giving the full solution.]
</details>

<details>
<summary>✅ Complete Feature Implementation</summary>

[Complete production-ready code with comments.
Include ALL the edge cases from Definition of Done.]

🚀 Production checklist for this feature:
[5 checkboxes — things to verify before shipping this
feature to real users]

🔄 How this feature connects to others:
[Which other patterns/features this one typically
works with in a real product]

</details>
---

═══════════════════════════════════════════════
SCENARIOS PER PATTERN
═══════════════════════════════════════════════

LEVEL 1 → 4 scenarios (basic → production-ready)
LEVEL 2 → 4 scenarios (incomplete → complete feature)
LEVEL 3 → 3 scenarios (demo version → real version)
LEVEL 4 → 2 scenarios (silent bug → fixed feature)

═══════════════════════════════════════════════
RULES
═══════════════════════════════════════════════

- Every scenario = a real shippable feature, not an exercise
- Code must handle: loading, error, empty state, edge cases
- No "happy path only" code — real code handles failures
- Companies: e-commerce, SaaS, fintech, social, healthcare,
  food delivery, job portal, HR tool
- React functional components + hooks only
- async/await throughout
- Clickable TOC at top
- "↑ Back to TOC" after every pattern
- Level dividers between levels

END WITH:
  Feature Readiness Checklist:
  For each pattern, 5 questions to ask before shipping.
  Example for Pattern 1 (Data Loading):
    □ Does it show a loading state?
    □ Does it handle API errors?
    □ Does it handle empty data?
    □ Does it clean up on unmount?
    □ Does it handle slow connections?

═══════════════════════════════════════════════
HOW TO GENERATE
═══════════════════════════════════════════════

Generate one part at a time.
Wait for instruction before generating next part.

PARTS:
  "Generate Part 1 — Level 1, Patterns 1 to 6"
  "Generate Part 2 — Level 2, Patterns 7 to 11"
  "Generate Part 3 — Level 3, Patterns 12 to 14"
  "Generate Part 4 — Level 4, Patterns 15 to 19"

Or one pattern only:
  "Generate only Pattern [N] — [Name].
   All scenarios. Complete. No placeholders."

OUTPUT: Complete markdown. No placeholders.
Every scenario fully written. Every feature complete.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Master Prompt — All Purposes Combined

> **Use when:** You want one guide that covers everything —
> interview prep + mental models + real product features
> + production bugs. The most complete version.

```
You are building a complete scenario-based React + JavaScript
practice guide. This is the master version — it combines
interview prep, mental model building, and real product
feature practice in one guide.

═══════════════════════════════════════════════
FULL STRUCTURE — ALL 4 LEVELS
═══════════════════════════════════════════════

LEVEL 1 — FOUNDATION (80% of real work)
  Pattern 1:  📋 Fetch & Display Data
  Pattern 2:  📝 Form + Validation
  Pattern 3:  🔁 List + Search + Filter
  Pattern 4:  💾 localStorage / Persistence
  Pattern 5:  🔀 Show/Hide UI
  Pattern 6:  🧭 Page Navigation

LEVEL 2 — INTERMEDIATE (15% of real work)
  Pattern 7:  ♾️  Pagination / Infinite Scroll
  Pattern 8:  🔔 Toast / Notifications
  Pattern 9:  🛒 Shopping Cart
  Pattern 10: 🌙 Dark Mode / User Preferences
  Pattern 11: 🔐 Auth Flow

LEVEL 3 — ADVANCED (4% of real work)
  Pattern 12: ⚡ Performance Optimization
  Pattern 13: 📡 Real-time / Polling
  Pattern 14: 🔄 Complex Async

LEVEL 4 — SPECIALIST (1% — production incidents)
  Pattern 15: 🕳️  Memory Leaks
  Pattern 16: 👻 Stale Closures
  Pattern 17: 🔁 Infinite Render Loops
  Pattern 18: 🔑 Token Expiry + Silent Refresh
  Pattern 19: 📦 Context Performance at Scale

═══════════════════════════════════════════════
FOR EACH PATTERN — HEADER FORMAT
═══════════════════════════════════════════════

## Pattern [N]: [Emoji] [Name]
> Level [1/2/3/4] — [Foundation/Intermediate/Advanced/Specialist]

🧠 The Mental Model:
[How a senior dev thinks about this.
The question they ask themselves. 2-3 lines.]

⚡ The One Rule:
[Single sentence. Solves 80% of this pattern's problems.]

🚨 What junior devs always get wrong:
[Specific wrong thinking — not just wrong code.]

📍 You need this pattern when:
  - [real situation 1]
  - [real situation 2]
  - [real situation 3]

📋 Interview asks this as:
  - [exact interview phrasing 1]
  - [exact interview phrasing 2]

📦 Production-ready version always includes:
  - [thing demos skip but real apps need 1]
  - [thing demos skip but real apps need 2]
  - [thing demos skip but real apps need 3]

═══════════════════════════════════════════════
FOR EACH SCENARIO — FORMAT
═══════════════════════════════════════════════

---
### 🔴 Pattern [N] · Scenario [N] — [Problem Title]

🏢 Situation:
[2-3 lines. Real company. Real person affected — PM,
manager, user, on-call alert. Real consequence.
Feels like Monday morning at a real job.]

Here is the current code:

[Realistic broken or incomplete code.
Real variable names. Real API shapes.
Real component names. NOT toy examples.
Looks like something a real dev wrote —
not intentionally broken, just incomplete or wrong.]

Your tasks:
  ✅ [specific fix or feature]
  ✅ [specific fix or feature]
  ✅ [specific fix or feature]
  ✅ [edge case that must be handled]

<details>
<summary>💡 Hint — How to think about this</summary>

[THINKING DIRECTION not the answer.
Ask the question that unlocks the right approach.
Connect to the pattern's mental model.
2-3 lines max.]

</details>

<details>
<summary>✅ Full Solution</summary>

[Complete working code with inline comments.
Every important decision explained in comments.
No skipped parts. No "// rest of code here".]

🧠 The principle:
[1 line — connects fix back to the pattern's One Rule.]

⚡ Why this matters in production:
[What actually breaks without this. Be specific. 2 lines.]

🔍 What a senior dev spots in 10 seconds:
[The root cause signal — not the symptom.]

✅ What the interviewer is checking:
[What they specifically want to see to pass this question.]

🚀 Before you ship this, verify:
  □ [production checklist item]
  □ [production checklist item]
  □ [production checklist item]

</details>
---

═══════════════════════════════════════════════
SCENARIOS PER PATTERN
═══════════════════════════════════════════════

LEVEL 1 → 4 scenarios (easy → medium → hard → edge case)
LEVEL 2 → 4 scenarios (incomplete → broken → scaled → edge case)
LEVEL 3 → 3 scenarios (wrong approach → right approach → production)
LEVEL 4 → 2 scenarios (subtle bug → full production fix)

═══════════════════════════════════════════════
COMPANY TYPES — ROTATE THROUGH THESE
═══════════════════════════════════════════════

  - E-commerce (products, cart, orders, reviews)
  - SaaS dashboard (analytics, users, billing, reports)
  - Food delivery (orders, tracking, menu, drivers)
  - Social platform (posts, comments, likes, feed)
  - Fintech / Banking (transactions, accounts, transfers)
  - HR / Recruitment (jobs, candidates, interviews)
  - Job portal (listings, applications, profiles)
  - Healthcare (appointments, records, doctors)

═══════════════════════════════════════════════
RULES
═══════════════════════════════════════════════

REALISM:
  - Code must look like something a real dev wrote
  - Real data shapes: { id, name, email, createdAt, status }
  - Real API patterns: /api/users, /api/products/:id
  - Real error types: network fail, 401, 404, 500, timeout
  - Real component names: ProductCard, UserTable, OrderForm

TEACHING:
  - Every fix teaches a transferable principle
  - Scenarios within a pattern: easy → harder
  - Level 3 and 4: show the silent bug before the fix
  - Connect every solution back to the pattern's One Rule

CODE STYLE:
  - React functional components + hooks only
  - async/await not .then() chains
  - TypeScript-style prop comments where helpful
  - Realistic imports included

FORMAT:
  - Clickable TOC at very top
  - Level dividers between each level
  - "↑ Back to TOC" after every pattern

═══════════════════════════════════════════════
END SECTIONS
═══════════════════════════════════════════════

END WITH TWO SECTIONS:

SECTION 1 — Pattern Picker (Problem → Pattern):
  At least 25 entries covering all 4 levels.
  Format: "My [problem]" → "Pattern [N] — [reason]"

  Examples:
  "My search hits the API on every keystroke"
    → Pattern 12 — debounce in Performance
  "My data disappears on page refresh"
    → Pattern 4 — localStorage Persistence
  "My list rerenders when nothing changed"
    → Pattern 12 — unnecessary renders
  "User gets logged out mid-session"
    → Pattern 18 — Token Expiry
  "My modal stays open after navigating away"
    → Pattern 6 — Navigation Flow cleanup
  "My cart empties on refresh"
    → Pattern 9 — Cart + localStorage
  "I am getting stale data in my useEffect"
    → Pattern 16 — Stale Closures

SECTION 2 — Production Readiness Checklist:
  For every pattern, 4-5 questions.
  Format:
  Pattern [N] — [Name]
    □ [question before shipping]
    □ [question before shipping]
    □ [question before shipping]

═══════════════════════════════════════════════
HOW TO GENERATE — IMPORTANT
═══════════════════════════════════════════════

Generate one part at a time.
Do not generate the next part until instructed.
After finishing a part, say:
"Part [N] complete. Say 'Generate Part [N+1]' to continue."

PARTS:
  "Generate Part 1 — Level 1, Patterns 1 to 6"
  "Generate Part 2 — Level 2, Patterns 7 to 11"
  "Generate Part 3 — Level 3, Patterns 12 to 14"
  "Generate Part 4 — Level 4, Patterns 15 to 19"

To generate one pattern only:
  "Generate only Pattern [N] — [Name].
   All scenarios. Complete. No placeholders."

To regenerate a specific scenario:
  "Regenerate Pattern [N] Scenario [N].
   Make it harder / more realistic / different company."

═══════════════════════════════════════════════
OUTPUT
═══════════════════════════════════════════════

Single complete markdown per part.
No placeholders. No "add your code here".
Every scenario fully written.
Every solution complete and working.
All code tested mentally before writing.
```

[↑ Back to Table of Contents](#table-of-contents)

---

# What Each Prompt Includes

| | Prompt 1 | Prompt 2 | Prompt 3 | Master |
|---|---|---|---|---|
| Build from scratch | ✅ | ❌ | ✅ | ✅ |
| Fix broken code | ✅ | ❌ | ✅ | ✅ |
| Why it breaks | ❌ | ✅ | ❌ | ✅ |
| Mental models | ❌ | ✅ | ❌ | ✅ |
| Architecture fix | ❌ | ✅ | ❌ | ✅ |
| Full feature e2e | ❌ | ❌ | ✅ | ✅ |
| Production checklist | ❌ | ❌ | ✅ | ✅ |
| Interview criteria | ✅ | ❌ | ❌ | ✅ |
| Senior thinking | ❌ | ✅ | ❌ | ✅ |
| All 4 levels | ✅ | ✅ | ✅ | ✅ |
| Pattern picker | ✅ | ✅ | ✅ | ✅ |
| **Best for** | **Interview** | **Senior** | **Portfolio** | **Everything** |

[↑ Back to Table of Contents](#table-of-contents)

---

# How Many Days Each Level Takes

## Time Per Scenario (Realistic Estimate)

```
Prompt 1 — Interview Prep      → 1.0 hour per scenario
Prompt 2 — Senior/Architecture → 1.5 hours per scenario
Prompt 3 — Product Builder     → 2.0 hours per scenario
Master Prompt                  → 2.5 hours per scenario
```

---

## Prompt 1 — Interview Prep

| Level | Patterns | Scenarios | Hours | 2h/day | 3h/day |
|---|---|---|---|---|---|
| Level 1 — Foundation | 1 to 6 | 24 | 24h | 12 days | 8 days |
| Level 2 — Intermediate | 7 to 11 | 20 | 20h | 10 days | 7 days |
| Level 3 — Advanced | 12 to 14 | 12 | 12h | 6 days | 4 days |
| Level 4 — Specialist | 15 to 19 | 10 | 15h | 8 days | 5 days |
| **Total** | | **66** | **71h** | **36 days** | **24 days** |

---

## Prompt 2 — Senior / Architecture

| Level | Patterns | Scenarios | Hours | 2h/day | 3h/day |
|---|---|---|---|---|---|
| Level 1 — Foundation | 1 to 6 | 24 | 36h | 18 days | 12 days |
| Level 2 — Intermediate | 7 to 11 | 20 | 30h | 15 days | 10 days |
| Level 3 — Advanced | 12 to 14 | 12 | 18h | 9 days | 6 days |
| Level 4 — Specialist | 15 to 19 | 10 | 20h | 10 days | 7 days |
| **Total** | | **66** | **104h** | **52 days** | **35 days** |

---

## Prompt 3 — Product Builder

| Level | Patterns | Scenarios | Hours | 2h/day | 3h/day |
|---|---|---|---|---|---|
| Level 1 — Foundation | 1 to 6 | 24 | 48h | 24 days | 16 days |
| Level 2 — Intermediate | 7 to 11 | 20 | 40h | 20 days | 14 days |
| Level 3 — Advanced | 12 to 14 | 12 | 24h | 12 days | 8 days |
| Level 4 — Specialist | 15 to 19 | 10 | 24h | 12 days | 8 days |
| **Total** | | **66** | **136h** | **68 days** | **46 days** |

---

## Master Prompt — All Combined

| Level | Patterns | Scenarios | Hours | 2h/day | 3h/day |
|---|---|---|---|---|---|
| Level 1 — Foundation | 1 to 6 | 24 | 60h | 30 days | 20 days |
| Level 2 — Intermediate | 7 to 11 | 20 | 50h | 25 days | 17 days |
| Level 3 — Advanced | 12 to 14 | 12 | 30h | 15 days | 10 days |
| Level 4 — Specialist | 15 to 19 | 10 | 30h | 15 days | 10 days |
| **Total** | | **66** | **170h** | **85 days** | **57 days** |

---

## All Prompts Side By Side

```
                   Prompt 1   Prompt 2   Prompt 3   Master
                   ────────   ────────   ────────   ──────
Level 1 days
  2 hours/day       12 days    18 days    24 days   30 days
  3 hours/day        8 days    12 days    16 days   20 days

Level 2 days
  2 hours/day       10 days    15 days    20 days   25 days
  3 hours/day        7 days    10 days    14 days   17 days

Level 3 days
  2 hours/day        6 days     9 days    12 days   15 days
  3 hours/day        4 days     6 days     8 days   10 days

Level 4 days
  2 hours/day        8 days    10 days    12 days   15 days
  3 hours/day        5 days     7 days     8 days   10 days

TOTAL
  2 hours/day       36 days    52 days    68 days   85 days
  3 hours/day       24 days    35 days    46 days   57 days
```

[↑ Back to Table of Contents](#table-of-contents)

---

# Realistic Study Plans

## Plan A — Interview in 30 Days

```
Use:       Prompt 1 only
Cover:     Level 1 + Level 2 only
Skip:      Level 3 and Level 4 for now
Time:      3 hours/day
Schedule:  8 days Level 1 + 7 days Level 2 = 15 days
           Use remaining 15 days to redo weak patterns
Result:    Ready for most React interviews
```

## Plan B — Interview in 60 Days

```
Use:       Prompt 1 for Level 1 + Level 2
           Master Prompt for Level 3 + Level 4
Cover:     All 4 levels
Time:      2-3 hours/day
Schedule:  ~45 days total
Result:    Ready for mid to senior React interviews
```

## Plan C — No Interview, Want to Grow

```
Use:       Master Prompt
Cover:     All 4 levels, no rush
Time:      1-2 hours/day
Schedule:  85-170 days
Result:    Solid senior-level React thinking
```

## Plan D — Weekend Warrior

```
Use:       Any prompt
Cover:     1 pattern per weekend (4 scenarios)
Time:      4 hours Saturday
Schedule:  19 weekends = about 5 months
Result:    Steady deep progress without burnout
```

## Plan E — Deepest Learning (No Deadline)

```
Use:       Prompt 2 for Level 1 and Level 2
           Master Prompt for Level 3 and Level 4
Cover:     All 4 levels, architecture focus
Time:      2 hours/day
Schedule:  ~60 days
Result:    Senior-level thinking + interview ready
```

[↑ Back to Table of Contents](#table-of-contents)

---

# The Honest Truth

```
These times assume you actually do this:

  ✅ Try the scenario yourself BEFORE looking at solution
     → Minimum 20-30 minutes of actual trying

  ✅ Read the full explanation after, do not just copy code
     → Understand WHY, not just WHAT

  ✅ Close the solution and write it again yourself
     → This is where real learning happens

  ✅ Take notes on what you got wrong
     → Review these notes before the next session

  ✅ Revisit patterns you struggled with
     → Do not move on until you can do it without hints


If you just read without doing:
  Time:    Half the estimate
  Benefit: 20% of the value


The rule that matters most:

  30 minutes struggling yourself
  >
  2 hours reading solutions


One pattern done properly
>
Five patterns skimmed quickly
```

[↑ Back to Table of Contents](#table-of-contents)

---

# How to Generate Parts

## Starting a Part

```
Copy the full prompt.
Then add one of these at the end:

  "Generate Part 1 — Level 1, Patterns 1 to 6"
  "Generate Part 2 — Level 2, Patterns 7 to 11"
  "Generate Part 3 — Level 3, Patterns 12 to 14"
  "Generate Part 4 — Level 4, Patterns 15 to 19"
```

## One Pattern Only

```
Add this at the end instead:

  "Generate only Pattern 3 — List + Search + Filter.
   All scenarios. Complete. No placeholders."
```

## Regenerate a Specific Scenario

```
  "Regenerate Pattern 2 Scenario 3.
   Make it harder."

  "Regenerate Pattern 5 Scenario 1.
   Use a food delivery company instead."

  "Regenerate Pattern 12 Scenario 2.
   Focus on the race condition bug specifically."
```

## After Each Part

```
After Part 1 is done → say "Generate Part 2"
After Part 2 is done → say "Generate Part 3"
After Part 3 is done → say "Generate Part 4"
```

## Recommended Order

```
Week 1-2:  Part 1 (Level 1 Foundation)
Week 3-4:  Part 2 (Level 2 Intermediate)
Week 5-6:  Part 3 (Level 3 Advanced)
Week 7+:   Part 4 (Level 4 Specialist)
```

[↑ Back to Table of Contents](#table-of-contents)