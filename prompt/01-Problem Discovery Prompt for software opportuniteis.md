# AI Problem Discovery Prompt
### Find Real Unmet Software Opportunities Across 18 Sectors in India
#### Use With Claude, ChatGPT, or Perplexity

---

## Table of Contents

- [What This Prompt Does](#what-this-prompt-does)
- [What This Prompt Does NOT Do](#what-this-prompt-does-not-do)
- [The Prompt](#the-prompt)
  - [The 18 Sectors](#the-18-sectors)
  - [Signal 1 — Regulation That Recently Changed](#signal-1--regulation-that-recently-changed)
  - [Signal 2 — Evidence of Manual Workarounds](#signal-2--evidence-of-manual-workarounds)
  - [Signal 3 — Funded Competitor Check](#signal-3--funded-competitor-check)
  - [Signal 4 — Market Size Evidence](#signal-4--market-size-evidence)
  - [Signal 5 — Red Flags](#signal-5--red-flags)
  - [Output Format](#output-format)
  - [Important Rules](#important-rules)
- [How To Use This Prompt](#how-to-use-this-prompt)
- [What The Prompt Will Not Tell You](#what-the-prompt-will-not-tell-you)
- [After The Prompt — What To Do Next](#after-the-prompt--what-to-do-next)

---

## What This Prompt Does

```
DESIGNED TO:
→ Find where money is currently being lost
→ Find where people are paying consultants
  or doing things manually right now
→ Find where regulation recently changed
  but software has not caught up yet
→ Find where a well-funded competitor does NOT exist
→ Flag every assumption so you know what
  needs field verification before you build anything
```

---

## What This Prompt Does NOT Do

```
NOT designed to:
→ Give you a business idea
→ Tell you what to build
→ Confirm an assumption you already have
→ Replace talking to real people in the industry
→ Guarantee the opportunity is real

The EPR recycling blueprint was built without
this prompt. It assumed a problem from desk research.
It missed Recykal — a ₹1,000Cr company already
solving the same problem since 2019.

This prompt is designed to catch that mistake
before you spend months building the wrong thing.
```

---

## The Prompt

> Copy everything from this section and paste it into Claude, ChatGPT, or Perplexity.

---

### Opening Instruction

```
You are a business researcher helping me find real,
unmet software opportunities in India across 18 sectors.

Your job is NOT to suggest business ideas.
Your job is to find WHERE PAIN CURRENTLY EXISTS
based only on verifiable, publicly available evidence.
```

---

### The 18 Sectors

```
THE 18 SECTORS TO ANALYZE:

1.  Technology and Digital
2.  Services — Professional and Personal
3.  Trading, Import and Export
4.  Media, Content and Entertainment
5.  Education and Training
6.  Hospitality, Food and Tourism
7.  Agriculture and Allied
8.  Manufacturing and Production
9.  Recycling, Waste and Circular Economy
10. Real Estate and Construction
11. Healthcare and Wellness
12. Finance and Investment
13. Transport and Logistics
14. Energy and Utilities
15. Mining and Natural Resources
16. Government and Defense Contracts
17. Intellectual Property and Licensing
18. Space and Frontier Tech
```

---

### Signal 1 — Regulation That Recently Changed

```
FOR EACH SECTOR, FIND THE FOLLOWING
USING ONLY PUBLICLY AVAILABLE EVIDENCE:

SIGNAL 1 — REGULATION THAT RECENTLY CHANGED

Find any Indian regulation, rule, or government
mandate that:
- Was introduced or significantly amended
  in the last 1-3 years
- Affects businesses in this sector
- Requires documentation, reporting, tracking,
  or certification that was not required before
- Has a compliance deadline that has passed
  or is approaching

For each regulation found, state:
- Exact name of the rule or act
- Which businesses it applies to
- What they must now do differently
- What the penalty for non-compliance is
- Whether a digital portal or manual submission
  is required
```

---

### Signal 2 — Evidence of Manual Workarounds

```
SIGNAL 2 — EVIDENCE OF MANUAL WORKAROUNDS

Find public evidence that businesses in this sector
are currently solving a problem manually.
This includes:

- Job postings for roles like "compliance executive"
  "data entry operator" "documentation manager"
  that suggest manual work is happening

- Consultant or CA firm websites offering
  manual services (filing, reporting, tracking)
  in this sector — and what they charge

- Forum posts, Reddit, IndiaMART, or LinkedIn
  discussions where business owners describe
  doing something manually

- News articles describing paperwork burdens
  or compliance chaos in this sector

For each manual workaround found, state:
- What is being done manually
- Who is doing it (owner, employee, consultant)
- What it costs approximately
- How often it needs to be done
- What goes wrong when it is not done properly
```

---

### Signal 3 — Funded Competitor Check

```
SIGNAL 3 — FUNDED COMPETITOR CHECK

For each problem identified above, search for:
- Indian startups that raised funding to solve
  this specific problem in the last 5 years
- Large software companies (SAP, Oracle, Tally,
  Zoho) that have a module covering this
- Government portals that already digitize this

If a funded competitor or government portal exists:
- Name them
- State their funding amount and date
- State what they cover and what they do NOT cover
- State whether an MSME in India could afford
  and actually use their solution

If NO funded competitor exists:
- Explicitly state "No funded competitor found"
- State what the closest alternative is and
  why it is inadequate
```

---

### Signal 4 — Market Size Evidence

```
SIGNAL 4 — MARKET SIZE EVIDENCE

Find publicly available data on:
- How many businesses in India are affected
  by this problem (from MSME data, MCA filings,
  government registration databases,
  industry association reports)
- What the total annual spend on manual
  workarounds might be (consultants,
  compliance staff, penalties paid)

Only use numbers from named sources.
Do not estimate or extrapolate.
If the number is not publicly available, say so.
```

---

### Signal 5 — Red Flags

```
SIGNAL 5 — RED FLAGS

For each candidate problem, actively look for
reasons it might NOT be a real opportunity:

- Is the market too small to build a business on?
- Is the regulation so new that businesses
  have not yet started feeling the pain?
- Is the regulation so old that businesses
  have already found their own solution?
- Is the customer too poor or too cash-based
  to pay for software?
- Is the problem being solved by the government
  itself through a mandatory portal?
- Would a large player (Recykal, Razorpay, Zoho)
  naturally expand into this once it grows?
- Is there a global equivalent already entering India?
```

---

### Output Format

```
OUTPUT FORMAT FOR EACH SECTOR:

Sector name
├── Regulation that changed : [name, date, what changed]
├── Who is affected         : [number of businesses, type]
├── What they do manually   : [specific task description]
├── What it costs them      : [₹ per year + source]
├── Funded competitor       : [name + funding OR "none found"]
├── Government portal       : [yes / no / partial coverage]
├── MSME affordability      : [can they pay ₹5-20K/month?]
├── Red flags               : [list every reason this might fail]
└── Field verification      : [what you must confirm by talking
                               to actual businesses before building]
```

---

### Important Rules

```
IMPORTANT RULES FOR YOUR RESPONSE:

RULE 1
Never suggest a solution or product name.
Only describe the problem and its evidence.

RULE 2
Every claim must have a source or be marked as
"unverified — needs field confirmation."

RULE 3
If you cannot find public evidence for a sector,
say "insufficient public data" rather than guessing.

RULE 4
Actively try to KILL each opportunity with red flags
before presenting it as viable.
The goal is to find what survives the killing —
not to make everything look promising.

RULE 5
At the end, rank the top 5 candidates by:
- Strength of evidence (most verified first)
- Absence of funded competitor
- Ability of target customer to pay
- Proximity to a physical location in Hyderabad
  where field verification is possible

RULE 6
Do not present more than 5 final candidates.
If fewer than 5 survive the red flag filter,
present only those that survived.
Do not pad the list to reach 5.
```

---

## How To Use This Prompt

### Option 1 — Use With Claude (Recommended)

```
Best for   : Deep research with web search enabled
How        : Paste the full prompt above in a new conversation
             Make sure web search is turned ON in settings
What to expect : Claude searches each sector individually
                 Takes 1-2 hours of back-and-forth
Strength   : Cites sources, flags uncertainty clearly
```

### Option 2 — Use With ChatGPT With Browsing

```
Best for   : Cross-checking what Claude found
How        : Paste in ChatGPT-4o with browsing enabled
What to expect : Similar output, different sources sometimes
Strength   : Good at structured output format
```

### Option 3 — Use With Perplexity

```
Best for   : Verifying regulation claims specifically
How        : Paste in Perplexity with Focus set to "All"
What to expect : Shorter answers but stronger source citations
Strength   : Every claim has a URL — easy to verify manually
```

### Option 4 — Run All Three and Compare

```
Where all three agree    → High confidence, worth investigating
Where two agree, one differs → Investigate the disagreement
Where all three disagree → Needs field verification, not desk research
```

---

## What The Prompt Will Not Tell You

```
After running this prompt perfectly across all 18 sectors,
you will have a shortlist of 3-5 candidates where
public evidence suggests a gap exists.

It will NOT tell you:

1. Whether the business owner will actually pay
   Only the person sitting in front of you can tell you that

2. Whether a stealth startup is already building this
   Unannounced companies do not appear in any search

3. Whether the regulation will actually be enforced
   Government enforcement speed is unpredictable

4. Whether the customer's real pain matches
   what the regulation describes
   Reality inside a factory is always different
   from what is written in official documents

THE PROMPT:     Gets you from 600 opportunities → 5 candidates
FIELD RESEARCH: Gets you from 5 candidates → 1 real one

Both steps are necessary.
Neither replaces the other.
```

---

## After The Prompt — What To Do Next

Once you have your shortlist of 3-5 candidates from the prompt:

### Step 1 — Pick the Top 2 That Survived All Red Flags

```
Criteria:
→ Regulation is real and recently enforced
→ No funded Indian competitor found
→ Target customer is a business (not individual)
  and has money to pay for software
→ You can physically reach this customer
  in Hyderabad this weekend
```

### Step 2 — Go Talk To 10 People in Each Candidate Sector

```
Where to go in Hyderabad:

Manufacturing / Pharma  → Patancheru, IDA Bollaram, Jeedimetla
Food Processing         → Jeedimetla, Nacharam industrial areas
Export businesses       → TSIIC parks, airport cargo area
Jewellery               → Secunderabad, Abids
Construction            → L.B. Nagar, Uppal material markets
Recycling / Scrap       → Afzalgunj market

What to say:
"I am a software developer researching operational
 problems in [sector] businesses. Not selling anything.
 Can I talk to someone for 20 minutes?"

What to ask:
"Walk me through what you did yesterday —
 from morning to evening."

What to listen for:
→ Any sentence starting with "I had to manually..."
→ Any sentence starting with "We called someone to find out..."
→ Any sentence starting with "We didn't know until..."
→ Any mention of a consultant they pay regularly
→ Any visible frustration when describing a task
```

### Step 3 — Apply The 5 Validation Signals

```
SIGNAL 1: Multiple people describe the SAME broken thing
          independently — without you suggesting it

SIGNAL 2: They are already paying someone to manage
          this pain (consultant, part-time person, workaround)

SIGNAL 3: The pain has a rupee value they can tell you
          "This cost me ₹3 lakh last year"

SIGNAL 4: They tried to fix it and failed
          "We tried Excel but it got too complicated"

SIGNAL 5: They ask YOU "can you solve this?"
          before you have pitched anything
```

### Step 4 — Confirm Willingness To Pay Before Building

```
Go back to the person who described the problem.

Say exactly this:
"If I built something that fixed this,
 would you pay ₹X per month?"

If they say yes, ask:
"Would you pay now for early access
 before it is fully built?"

If they pull out their phone to pay —
you have found a real problem.
Start building.

If they say "maybe" or "let me think" —
the pain is not acute enough.
Keep looking.
```

---

*Prompt designed to find real problems, not confirm assumptions.*
*Every output from this prompt is a hypothesis — not a conclusion.*
*The market is the only thing that can confirm a conclusion.*
