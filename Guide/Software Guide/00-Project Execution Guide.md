# Complete Project Execution Guide — SAFe Agile Deep Dive

---

## 📌 Navigation

1. [Project Execution Methodologies](#1-project-execution-methodologies)
2. [SAFe Agile Deep Dive](#2-safe-agile-deep-dive)
3. [SAFe Artifacts](#3-safe-artifacts)
4. [All Roles with Certifications](#4-all-roles-with-certifications)
   - [Business Analyst (BA)](#41-business-analyst-ba)
   - [Product Owner (PO)](#42-product-owner-po)
   - [Product Manager (PM)](#43-product-manager-pm)
   - [Scrum Master (SM)](#44-scrum-master-sm)
   - [Release Train Engineer (RTE)](#45-release-train-engineer-rte)
   - [Solution Manager](#46-solution-manager)
   - [System Architect](#47-system-architect)
   - [Developer](#48-developer)
   - [QA Engineer](#49-qa-engineer)
   - [DevOps / Release Manager](#410-devops--release-manager)
   - [Technical Writer](#411-technical-writer)
   - [Knowledge Management Specialist](#412-knowledge-management-specialist)
   - [DevEx Engineer](#413-devex-engineer)
   - [Developer Advocate](#414-developer-advocate)
5. [Team Structure](#5-team-structure)
6. [How All Roles Work Together](#6-how-all-roles-work-together)
7. [SAFe Ceremonies](#7-safe-ceremonies)
8. [PI Planning Deep Dive](#8-pi-planning-deep-dive)
9. [Complete Project Execution Flow](#9-complete-project-execution-flow)
10. [DevOps and CI/CD in SAFe](#10-devops-and-cicd-in-safe)
11. [Metrics and Reporting](#11-metrics-and-reporting)
12. [Stakeholder Management](#12-stakeholder-management)
13. [Jira and Confluence in SAFe](#13-jira-and-confluence-in-safe)
14. [Real World Challenges](#14-real-world-challenges)
15. [Tools Used](#15-tools-used)
16. [New Joiner Guide](#16-new-joiner-guide)
17. [Glossary](#17-glossary)
18. [Summary Table — Roles, Certifications and Salary](#18-summary-table--roles-certifications-and-salary)

---

## 1. Project Execution Methodologies

> This section gives a brief overview of all major project execution methodologies. This document then focuses entirely on SAFe Agile.

---

### 1.1 Waterfall

```
Requirements → Design → Development → Testing → Deployment → Maintenance
```

- Linear and sequential approach
- Each phase must complete before next begins
- Heavy documentation upfront
- Changes are very difficult mid-project
- **Best for:** Construction, manufacturing, government projects
- **Not good for:** Software where requirements change frequently

---

### 1.2 Agile (Scrum)

```
Plan → Sprint (2 weeks) → Review → Retrospective → Repeat
```

- Iterative and incremental delivery
- Work done in short sprints (1-4 weeks)
- Welcomes changing requirements
- Continuous feedback from stakeholders
- **Best for:** Small to medium software teams
- **Not good for:** Very large organizations with 100+ teams

---

### 1.3 Kanban

```
Backlog → To Do → In Progress → Review → Done
```

- Continuous flow, no fixed sprints
- Visual board to track work
- Limit work in progress (WIP)
- Focus on reducing bottlenecks
- **Best for:** Support teams, maintenance work, operations
- **Not good for:** Projects needing fixed deliverables

---

### 1.4 SAFe Agile (Scaled Agile Framework)

```
Portfolio → Large Solution → Program (ART) → Team (Scrum)
```

- Agile scaled for large enterprises
- Multiple teams working together
- Synchronized planning across teams
- **Best for:** Large organizations with 100s of developers
- **Used by:** DirecTV, Boeing, Intel, Cisco
- **This document focuses entirely on SAFe Agile**

---

### 1.5 Hybrid

```
Waterfall (Planning) + Agile (Execution)
```

- Mix of waterfall and agile
- Fixed scope but agile delivery
- **Best for:** Companies transitioning from waterfall to agile

---

### 1.6 Quick Comparison Table

| Factor | Waterfall | Scrum | Kanban | SAFe |
|---|---|---|---|---|
| Team Size | Any | 5-10 | Any | 100+ |
| Planning | Upfront | Per Sprint | Continuous | Per PI (12 weeks) |
| Delivery | End only | Every sprint | Continuous | Every sprint + PI |
| Changes | Very hard | Easy | Easy | Managed |
| Best for | Fixed projects | Small teams | Support/Ops | Large enterprise |

[⬆ Back to Top](#-navigation)

---

## 2. SAFe Agile Deep Dive

---

### 2.1 What is SAFe?

SAFe stands for **Scaled Agile Framework**. It is a set of organization and workflow patterns designed to guide enterprises in scaling agile and lean practices.

```
Created by: Dean Leffingwell
Year: 2011
Latest Version: SAFe 6.0
Used by: 20,000+ companies worldwide
```

---

### 2.2 Why SAFe?

Regular Scrum works for one team. But what happens when:
- You have 50 teams building one product?
- Teams depend on each other?
- You need synchronized releases?

```
Problem:          Solution:
─────────         ─────────
50 teams          SAFe coordinates
working alone  →  all teams together
→ Chaos           → Organized delivery
```

---

### 2.3 SAFe Four Levels

```
LEVEL 4 — PORTFOLIO
─────────────────────────────────────────────
Business strategy, investment, Epic level work
Epics → approved → funded → sent to teams

        ↓

LEVEL 3 — LARGE SOLUTION
─────────────────────────────────────────────
Multiple ARTs working on one large solution
Solution Manager, Solution Architect oversee

        ↓

LEVEL 2 — PROGRAM (ART Level)
─────────────────────────────────────────────
Agile Release Train (ART)
5-12 teams working together
RTE coordinates, PI Planning happens here

        ↓

LEVEL 1 — TEAM
─────────────────────────────────────────────
Individual Scrum teams
5-10 people per team
2-week sprints
Daily standups, sprint ceremonies
```

---

### 2.4 What is an ART?

ART = **Agile Release Train**

```
Think of ART like a train:
→ Multiple teams = train compartments
→ All moving in same direction
→ Same speed (synchronized sprints)
→ Same destination (PI Goals)
→ RTE = Train Driver
```

At DirecTV:
- SCIM ART = one train
- Ariane team = one compartment of that train

---

### 2.5 SAFe vs Regular Scrum

| Feature | Regular Scrum | SAFe |
|---|---|---|
| Team size | 5-10 people | 50-125+ people (ART) |
| Planning | Sprint planning (2 weeks) | PI Planning (12 weeks) |
| Sync event | Daily standup | ART sync + team standup |
| Delivery | Sprint goals | PI Objectives |
| Roles | PO, SM, Dev Team | PO, SM, RTE, Solution Manager, Architect + more |
| Dependencies | Not formal | Formally tracked |
| Budget | Project based | Value stream based |

[⬆ Back to Top](#-navigation)

---

## 3. SAFe Artifacts

---

### 3.1 Work Item Hierarchy

```
EPIC (Largest)
└── FEATURE
        └── USER STORY
                └── TASK / SUB-TASK (Smallest)
```

---

### 3.2 Epic

```
What: Large body of work spanning multiple PIs
Who creates: Product Management / Business
Example: "Implement Mobile Responsive Design for React App"
Size: Takes months to complete
Lives in: Portfolio Backlog
```

---

### 3.3 Feature

```
What: Chunk of Epic, deliverable in one PI
Who creates: Product Owner / Product Manager
Example: "Make Homepage Mobile Responsive"
Size: Takes one PI (12 weeks) or less
Lives in: Program Backlog
```

---

### 3.4 User Story

```
What: Small piece of work done in one sprint
Who creates: BA / Product Owner
Format: "As a [user], I want [action], So that [benefit]"
Example: "As a mobile user, I want the navigation 
          menu to collapse, so that I can use 
          the app on small screens"
Size: Completed in 1 sprint (2 weeks)
Lives in: Team Backlog / Sprint
```

---

### 3.5 Task / Sub-task

```
What: Technical steps to complete a story
Who creates: Developer / QA
Example: 
  → "Write CSS media queries for navbar"
  → "Write test cases for mobile navbar"
  → "Test on iPhone 12"
Size: Hours to 1-2 days
Lives in: Sprint Board
```

---

### 3.6 Definition of Ready (DoR)

Before a story enters a sprint it must be READY:

```
✅ Story is clearly written
✅ Acceptance criteria defined
✅ Dependencies identified
✅ Story is estimated (story points)
✅ Designs/mockups attached (if UI)
✅ Team understands the story
```

---

### 3.7 Definition of Done (DoD)

Before a story is marked DONE it must have:

```
✅ Code written and reviewed
✅ Unit tests written and passing
✅ QA tested and approved
✅ No critical bugs open
✅ Confluence updated (if needed)
✅ PO accepted the story
✅ Deployed to QA environment
```

---

### 3.8 Acceptance Criteria

```
What: Conditions that must be met for story to be accepted
Who writes: BA / Product Owner
Who validates: QA Engineer + Product Owner

Example for mobile navbar story:
✅ Navbar collapses on screens < 768px
✅ Hamburger menu icon appears on mobile
✅ All menu items accessible on mobile
✅ Menu animation is smooth
✅ Works on iOS Safari and Android Chrome
✅ Meets WCAG accessibility standards
```

---

### 3.9 Story Points

```
What: Effort estimate for a story (not hours!)
Scale: 1, 2, 3, 5, 8, 13, 20 (Fibonacci)

1 point  = Very simple (few hours)
2 points = Simple (half day)
3 points = Medium (1 day)
5 points = Complex (2-3 days)
8 points = Very complex (full sprint needed)
13+      = Too big! Break it down further
```

---

### 3.10 Program Increment (PI)

```
What: A timebox of 5 sprints (approx 12 weeks)
Contains: 4 delivery sprints + 1 Innovation/Planning sprint
Goal: Deliver a set of features agreed in PI Planning

PI Timeline:
─────────────────────────────────────────────
Sprint 1 → Sprint 2 → Sprint 3 → Sprint 4 → IP Sprint
(Deliver)  (Deliver)  (Deliver)  (Deliver)  (Plan next PI
                                             + Innovation
                                             + Hardening)
```

[⬆ Back to Top](#-navigation)

---

## 4. All Roles with Certifications

---

### 4.1 Business Analyst (BA)

#### What They Do Daily:
```
→ Gather requirements from stakeholders
→ Write detailed user stories
→ Create process flow diagrams
→ Define acceptance criteria
→ Clarify requirements for dev team
→ Validate solution meets business needs
→ Maintain requirement documents in Confluence
→ Participate in sprint ceremonies
```

#### Responsibilities:
```
→ BRD (Business Requirement Document)
→ FRD (Functional Requirement Document)
→ User Stories with Acceptance Criteria
→ Process Flow Diagrams
→ Use Case Documents
→ Gap Analysis
→ RTM (Requirement Traceability Matrix)
```

#### Who They Work With:
```
Stakeholders → BA → Product Owner → Dev Team → QA
```

#### Certifications:
```
Entry Level:
→ ECBA (Entry Certificate in Business Analysis)
  - No experience needed
  - Best for freshers

Mid Level:
→ CCBA (Certificate of Capability in BA)
  - Needs 2-3 years experience

Senior Level:
→ CBAP (Certified Business Analysis Professional)
  - Most recognized globally
  - Needs 5 years experience

Agile Specific:
→ PMI-PBA (Professional in Business Analysis)
→ SAFe Business Analyst Certification
→ Agile Analysis Certification (AAC)
```

#### How to Become a BA:
```
Path 1 (Most Common):
QA Engineer (2-3 years)
      ↓
Learn requirement writing
      ↓
Junior BA
      ↓
Senior BA

Path 2:
Engineering Graduate
      ↓
ECBA Certification
      ↓
Junior BA

Path 3:
MBA
      ↓
Direct Junior BA entry
```

#### Career Progression:
```
Junior BA → BA → Senior BA → Lead BA → Product Owner / Product Manager
```

---

### 4.2 Product Owner (PO)

#### What They Do Daily:
```
→ Manage and prioritize product backlog
→ Write and refine user stories
→ Participate in sprint planning
→ Accept or reject completed stories
→ Communicate with stakeholders
→ Clarify requirements for team
→ Attend all sprint ceremonies
→ Define sprint goals
```

#### Responsibilities:
```
→ Own the product backlog
→ Prioritize features by business value
→ Define PI Objectives for team
→ Work with BA on detailed requirements
→ Work with Product Manager on vision
→ Final say on accepting work
```

#### Who They Work With:
```
Product Manager → PO → BA → Dev Team → QA → Stakeholders
```

#### Certifications:
```
→ CSPO (Certified Scrum Product Owner) — Scrum Alliance
→ PSPO (Professional Scrum PO) — Scrum.org
→ SAFe Product Owner/Product Manager (POPM)
→ PMI-ACP (Agile Certified Practitioner)
```

#### How to Become a PO:
```
Path 1:
Senior BA (3-5 years)
      ↓
CSPO Certification
      ↓
Product Owner

Path 2:
Developer / QA (3-5 years)
      ↓
Team Lead role
      ↓
Product Owner

Path 3:
MBA + Business background
      ↓
Direct PO entry
```

#### Career Progression:
```
Junior PO → Product Owner → Senior PO → Product Manager → Director of Product
```

---

### 4.3 Product Manager (PM)

#### What They Do Daily:
```
→ Define product vision and strategy
→ Work with business stakeholders
→ Manage program backlog (features level)
→ Prioritize features across teams
→ Work with multiple Product Owners
→ Define PI Objectives at ART level
→ Present at System Demo and Solution Demo
→ Market and competitive analysis
```

#### Responsibilities:
```
→ Product roadmap ownership
→ Feature prioritization across ART
→ Business case for new features
→ Stakeholder alignment
→ Vision for next 2-3 PIs
→ Works at Program level (above PO)
```

#### Who They Work With:
```
Executives → Product Manager → Product Owners → RTE → System Architect
```

#### Certifications:
```
→ SAFe Product Owner/Product Manager (POPM)
→ Pragmatic Marketing Certification
→ Product Management certification (Coursera/Product School)
→ MBA (strong advantage)
```

#### How to Become a Product Manager:
```
Senior Product Owner (3+ years)
      ↓
Product Manager

OR

Senior BA + Business exposure
      ↓
Product Manager
```

#### Career Progression:
```
PO → Product Manager → Senior PM → Director of Product → VP of Product → CPO
```

---

### 4.4 Scrum Master (SM)

#### What They Do Daily:
```
→ Facilitate daily standup
→ Remove blockers for team
→ Protect team from outside interruptions
→ Facilitate sprint planning
→ Facilitate sprint review and retrospective
→ Coach team on agile practices
→ Track sprint progress
→ Escalate impediments to RTE
→ Update Jira board
```

#### Responsibilities:
```
→ Team agile health
→ Sprint velocity tracking
→ Impediment log maintenance
→ Team coaching and mentoring
→ Coordinate with other Scrum Masters
→ Participate in Scrum of Scrums (ART level sync)
```

#### Who They Work With:
```
RTE → Scrum Master → Dev Team + QA + BA + PO
```

#### Certifications:
```
Entry Level:
→ CSM (Certified Scrum Master) — Scrum Alliance
→ PSM I (Professional Scrum Master) — Scrum.org

Advanced:
→ A-CSM (Advanced CSM)
→ PSM II / PSM III

SAFe Specific:
→ SAFe Scrum Master (SSM)
→ SAFe Advanced Scrum Master (SASM)
```

#### How to Become a Scrum Master:
```
Developer / QA / BA (2-3 years experience)
      ↓
CSM or SSM Certification
      ↓
Junior Scrum Master
      ↓
Scrum Master
```

#### Career Progression:
```
Scrum Master → Senior SM → RTE → Agile Coach → Enterprise Agile Coach
```

---

### 4.5 Release Train Engineer (RTE)

#### What They Do Daily:
```
→ Facilitate ART events and ceremonies
→ Coordinate across multiple teams
→ Remove ART level impediments
→ Manage program risks
→ Facilitate PI Planning
→ Run ART Sync meetings
→ Track PI Objectives progress
→ Coach teams and Scrum Masters
→ Communicate with Solution Manager
→ Manage cross-team dependencies
```

#### Responsibilities:
```
→ ART level agile health
→ PI Planning facilitation
→ Cross-team dependency management
→ Program board maintenance
→ Risk management (ROAM)
→ ART metrics and reporting
→ Stakeholder communication
```

#### Who They Work With:
```
Solution Manager → RTE → All Scrum Masters → Product Managers → System Architect
```

#### What is ROAM?
```
R - Resolved   : Risk is resolved, no action needed
O - Owned      : Someone owns it and will resolve
A - Accepted   : Risk accepted, cannot be resolved
M - Mitigated  : Plan in place to reduce impact
```

#### Certifications:
```
→ SAFe Release Train Engineer (RTE) — Most important!
→ SAFe Program Consultant (SPC)
→ Advanced Scrum Master certifications
→ PMP (helpful)
→ Agile Coach certifications
```

#### How to Become an RTE:
```
Senior Scrum Master (3-5 years)
      ↓
SAFe RTE Certification
      ↓
RTE role

OR

Senior Project Manager
      ↓
SAFe certifications
      ↓
RTE role
```

#### Career Progression:
```
Scrum Master → RTE → Senior RTE → Agile Coach → Enterprise Agile Transformation Lead
```

---

### 4.6 Solution Manager

#### What They Do Daily:
```
→ Define solution vision (across multiple ARTs)
→ Manage solution backlog
→ Work with multiple Product Managers
→ Coordinate large solution delivery
→ Manage solution level PI Objectives
→ Present at Solution Demo
→ Work with Solution Architect
→ Manage customer and supplier relationships
```

#### Responsibilities:
```
→ Solution roadmap ownership
→ Capability prioritization
→ Multiple ART coordination
→ Business outcome ownership
→ Large solution vision
```

#### When Does This Role Exist?
```
Only in large organizations where:
→ Multiple ARTs (5+ teams) work on ONE product
→ Solution is very complex
→ Example: Building entire DirecTV platform
  needs Solution Manager to coordinate
  multiple ARTs
```

#### Who They Work With:
```
Business Executives → Solution Manager → Multiple Product Managers → RTEs
```

#### Certifications:
```
→ SAFe for Architects
→ SAFe Program Consultant (SPC)
→ Product Management certifications
→ MBA (strong advantage)
```

#### Career Progression:
```
Senior Product Manager → Solution Manager → VP of Engineering/Product
```

---

### 4.7 System Architect

#### What They Do Daily:
```
→ Define technical architecture of the system
→ Make technology decisions
→ Guide developers on implementation
→ Identify technical debt
→ Ensure non-functional requirements met
→ Review code architecture
→ Participate in PI Planning
→ Define architectural runway
→ Work with System Team
→ Coordinate with Solution Architect
```

#### Responsibilities:
```
→ Architecture decisions
→ Technology stack selection
→ API design standards
→ Security architecture
→ Performance requirements
→ Architectural runway (building foundation for future features)
→ Technical risk management
```

#### Architectural Runway:
```
What: Technical infrastructure built ahead of features
Why: So teams can build features without hitting technical walls

Example for MFR 2.0 (your project):
→ Set up responsive design framework
→ Define breakpoints standard
→ Set up component library
→ Define mobile testing standards
BEFORE teams start making things responsive
```

#### Who They Work With:
```
Solution Architect → System Architect → All Developers → RTE → Product Manager
```

#### Certifications:
```
→ SAFe for Architects
→ AWS/Azure/GCP Architecture certifications
→ TOGAF (The Open Group Architecture Framework)
→ Certified Software Architect (various)
```

#### How to Become a System Architect:
```
Senior Developer (5-8 years)
      ↓
Tech Lead role
      ↓
Solutions/Principal Engineer
      ↓
System Architect
```

#### Career Progression:
```
Senior Dev → Tech Lead → System Architect → Solution Architect → Enterprise Architect → CTO
```

---

### 4.8 Developer

#### What They Do Daily:
```
→ Pick up assigned stories from sprint board
→ Write code to implement features
→ Write unit tests
→ Code review for teammates
→ Update Jira ticket status
→ Attend daily standup
→ Collaborate with QA on bug fixes
→ Update technical documentation
→ Work within definition of done
```

#### Types of Developers:
```
Frontend Developer  → UI, React, CSS, JavaScript
Backend Developer   → APIs, databases, business logic
Full Stack          → Both frontend and backend
Mobile Developer    → iOS, Android apps
DevOps Engineer     → Infrastructure, CI/CD pipelines
```

#### Certifications:
```
Frontend:
→ AWS Certified Developer
→ Google Professional Developer
→ Meta Frontend Developer Certificate

React Specific:
→ Various React certifications (Udemy, Coursera)

General:
→ Oracle Java Certifications
→ Microsoft Azure Developer
```

#### Career Progression:
```
Junior Dev → Developer → Senior Developer → Tech Lead → System Architect → CTO
```

---

### 4.9 QA Engineer

#### What They Do Daily:
```
→ Read and understand user stories
→ Write test cases based on acceptance criteria
→ Execute manual tests
→ Report bugs in Jira
→ Retest fixed bugs
→ Attend daily standup
→ Participate in sprint ceremonies
→ Write automation test scripts
→ Update test status in Jira
→ Sign off on completed stories
→ Regression testing before release
```

#### Types of Testing in SAFe:
```
Unit Testing        → Developer does this
Integration Testing → QA + Developer
System Testing      → QA team
Regression Testing  → QA team (before each release)
UAT                 → Business/Stakeholders
Performance Testing → Specialized QA
Security Testing    → Specialized QA / DevOps
```

#### Certifications:
```
Manual Testing:
→ ISTQB Foundation Level (CTFL) — Most important!
→ ISTQB Advanced Level

Automation:
→ Selenium WebDriver certifications
→ Cypress certifications
→ Appium (mobile testing)

Agile Testing:
→ ISTQB Agile Tester Extension
→ SAFe Practitioner

Performance:
→ JMeter certification
→ LoadRunner certification

API Testing:
→ Postman API certification
```

#### Career Progression:
```
Junior QA → QA Engineer → Senior QA → QA Lead → QA Manager → Director of Quality
                                              ↓
                                    Test Automation Engineer
                                              ↓
                                    SDET (Software Dev Engineer in Test)
```

---

### 4.10 DevOps / Release Manager

#### What They Do Daily:
```
→ Manage CI/CD pipelines
→ Coordinate deployments across environments
→ Monitor system health
→ Manage release calendar
→ Go/No-Go decisions for releases
→ Automate deployment processes
→ Manage infrastructure (cloud/on-premise)
→ Incident management
→ Work with Dev and QA teams
→ Manage environment access
```

#### Environments They Manage:
```
DEV → QA → STAGING → PRODUCTION
(code    (testing  (final     (live
written)  here)    check)     users)
```

#### Certifications:
```
DevOps:
→ AWS DevOps Engineer
→ Azure DevOps Engineer
→ Google Professional DevOps Engineer
→ Docker/Kubernetes certifications

ITIL (Release Management):
→ ITIL 4 Foundation — Most important for Release Managers
→ ITIL Practitioner

SAFe:
→ SAFe DevOps Practitioner (SDP)
```

#### Career Progression:
```
Developer/SysAdmin → DevOps Engineer → Senior DevOps → Release Manager → DevOps Architect → VP Engineering
```

---

### 4.11 Technical Writer

#### What They Do Daily:
```
→ Write user manuals and guides
→ Create API documentation
→ Maintain Confluence pages
→ Write onboarding guides
→ Document release notes
→ Create process documentation
→ Interview developers to understand features
→ Make complex tech simple to read
→ Maintain style guides
```

#### Certifications:
```
→ Google Technical Writing Courses (Free — highly recommended!)
→ CPTC (Certified Professional Technical Communicator)
→ API Documentation (Write the Docs community)
→ DITA XML certification
```

#### Career Progression:
```
Junior Technical Writer → Technical Writer → Senior TW → Lead TW → Documentation Manager → Knowledge Manager
```

---

### 4.12 Knowledge Management Specialist

#### What They Do Daily:
```
→ Structure Confluence spaces
→ Create knowledge base systems
→ Define documentation standards
→ Train teams on documentation practices
→ Audit and clean up outdated docs
→ Ensure information is findable
→ Manage content lifecycle
→ Prevent knowledge loss when people leave
```

#### Certifications:
```
→ Certified Knowledge Manager (CKM)
→ Confluence Administrator certification
→ Information Architecture certifications
→ SharePoint certifications
```

#### Career Progression:
```
Technical Writer → Knowledge Management Specialist → Knowledge Manager → Chief Knowledge Officer (CKO)
```

---

### 4.13 DevEx Engineer (Developer Experience Engineer)

#### What They Do Daily:
```
→ Improve developer tools and workflows
→ Build internal developer platforms
→ Improve CI/CD developer experience
→ Write and maintain developer documentation
→ Create coding standards and guidelines
→ Reduce friction in development process
→ Gather feedback from developers
→ Improve onboarding for new developers
→ Build internal tooling
```

#### Certifications:
```
→ Strong development background required
→ Platform Engineering certifications
→ Cloud certifications (AWS/Azure/GCP)
→ DevOps certifications
```

#### Career Progression:
```
Senior Developer (5+ years) → DevEx Engineer → Senior DevEx → Head of Developer Experience → VP Engineering
```

---

### 4.14 Developer Advocate

#### What They Do Daily:
```
→ Create tutorials, blogs, videos
→ Speak at tech conferences
→ Build developer community
→ Give product feedback to engineering teams
→ Write sample code and demos
→ Engage with developers on social media
→ Run workshops and webinars
→ Represent developer needs internally
```

#### Certifications:
```
→ No specific certification
→ Strong coding portfolio required
→ Public speaking skills
→ Content creation skills
→ Community building experience
```

#### Career Progression:
```
Developer (3-5 years) + Public presence → Developer Advocate → Senior DA → Head of Developer Relations → VP Developer Relations
```

[⬆ Back to Top](#-navigation)

---

## 5. Team Structure

---

### 5.1 Individual Agile Team

```
One Agile Team (5-10 people):
─────────────────────────────
1 Product Owner
1 Scrum Master
2-4 Developers
1-2 QA Engineers
1 BA (sometimes shared)
─────────────────────────────
Total: 6-9 people
```

---

### 5.2 ART Structure (Program Level)

```
One ART (Agile Release Train) = 50-125 people:
────────────────────────────────────────────────
1 RTE (Release Train Engineer)
1 System Architect
1-2 Product Managers
5-12 Agile Teams
  Each team has: PO + SM + Devs + QA + BA
1 System Team (DevOps/Infrastructure)
────────────────────────────────────────────────
```

---

### 5.3 DirecTV Team Structure (Based on What You See)

```
SCIM ART (Your ART)
├── RTE
├── System Architect
├── Product Manager
│
├── Ariane Team (Your Team!)
│     ├── Product Owner
│     ├── Scrum Master
│     ├── Developers (React/Frontend)
│     ├── QA Engineers ← You are here!
│     └── BA
│
├── ART Self Care Team
│     ├── Product Owner
│     ├── Scrum Master
│     └── Dev + QA + BA
│
└── Other SCIM Teams...
```

---

### 5.4 Large Solution Level

```
Solution Manager
Solution Architect
      ↓
Multiple ARTs working together
(SCIM ART + Other ARTs)
```

[⬆ Back to Top](#-navigation)

---

## 6. How All Roles Work Together

---

### 6.1 Full Organization Chart

```
PORTFOLIO LEVEL
──────────────────────────────────────
Business Executives / Stakeholders
        ↓
Epic Owners / Enterprise Architects
        ↓

LARGE SOLUTION LEVEL
──────────────────────────────────────
Solution Manager ←→ Solution Architect
        ↓

PROGRAM LEVEL (ART)
──────────────────────────────────────
Product Manager ←→ System Architect
        ↓
RTE (coordinates everything at ART level)
        ↓

TEAM LEVEL
──────────────────────────────────────
Product Owner ←→ Scrum Master
        ↓
BA → Dev Team + QA Team
        ↓
DevOps / Release Manager
        ↓
Technical Writer / Knowledge Manager
```

---

### 6.2 Communication Flow

```
IDEA FLOW (Top Down):
Business Need → Product Manager → Product Owner → BA → Dev Team

DELIVERY FLOW (Bottom Up):
Dev builds → QA tests → PO accepts → Release Manager deploys → Business sees value

BLOCKER FLOW:
Dev blocked → Scrum Master → RTE → Solution Manager → Resolved
```

---

### 6.3 Who Works With Whom Daily

| Role | Works Daily With |
|---|---|
| BA | PO, Dev Team, QA, Stakeholders |
| Product Owner | BA, Dev Team, Scrum Master, Product Manager |
| Scrum Master | Dev Team, QA, PO, RTE |
| RTE | All Scrum Masters, Product Managers, System Architect |
| System Architect | Developers, RTE, Product Manager |
| Developer | Other Devs, QA, BA, Scrum Master |
| QA Engineer | Developers, BA, PO, Release Manager |
| DevOps/Release Manager | Dev Team, QA, RTE |

[⬆ Back to Top](#-navigation)

---

## 7. SAFe Ceremonies

---

### 7.1 Team Level Ceremonies

#### Daily Standup (15 minutes)
```
When: Every day, same time
Who: Entire team
Format: Each person answers 3 questions:
  1. What did I do yesterday?
  2. What will I do today?
  3. Any blockers?

Rules:
→ Stand up (keeps it short!)
→ Exactly 15 minutes
→ Scrum Master facilitates
→ Detailed discussions AFTER standup
```

#### Sprint Planning (2-4 hours)
```
When: Start of every sprint (every 2 weeks)
Who: PO + Scrum Master + Dev Team + QA + BA
Purpose: Decide what work goes into next sprint

Activities:
→ PO presents top priority stories
→ Team discusses and estimates
→ Team commits to sprint goal
→ Stories moved to sprint board
```

#### Sprint Review / Demo (1-2 hours)
```
When: End of every sprint
Who: Team + Stakeholders + PO
Purpose: Show completed work

Activities:
→ Team demos completed stories
→ PO accepts or rejects work
→ Stakeholders give feedback
→ Only DONE work is demonstrated
```

#### Sprint Retrospective (1 hour)
```
When: End of every sprint (after review)
Who: Team + Scrum Master (no stakeholders)
Purpose: Improve team processes

Format: Team discusses:
→ What went well? (Keep doing)
→ What went wrong? (Stop doing)
→ What to improve? (Start doing)
→ Action items assigned
```

#### Backlog Refinement (1-2 hours)
```
When: Middle of sprint (weekly)
Who: PO + BA + Dev Team + QA
Purpose: Prepare stories for next sprint

Activities:
→ Review upcoming stories
→ Add details and acceptance criteria
→ Estimate story points
→ Ensure stories meet Definition of Ready
```

---

### 7.2 ART Level Ceremonies

#### Scrum of Scrums (30-45 minutes)
```
When: Weekly (or more frequent)
Who: All Scrum Masters + RTE
Purpose: Coordinate across teams

Format: Each SM answers:
→ What did our team complete?
→ What are we working on?
→ Any cross-team dependencies/blockers?
```

#### PO Sync (30-60 minutes)
```
When: Weekly
Who: All Product Owners + Product Manager + RTE
Purpose: Align priorities across teams
Activities:
→ Review cross-team dependencies
→ Align on feature priorities
→ Identify risks
```

#### ART Sync (30-60 minutes)
```
When: Weekly
Who: RTE + All Scrum Masters + Product Managers + System Architect
Purpose: Overall ART health check
Activities:
→ PI progress review
→ Risk identification
→ Dependency management
→ Upcoming milestone planning
```

#### System Demo (1-2 hours)
```
When: End of every sprint
Who: All teams + Stakeholders + Product Management
Purpose: Show integrated work of ALL teams together

Key difference from Sprint Review:
Sprint Review = one team's work
System Demo = ALL teams' work integrated together
```

#### Inspect and Adapt (I&A) (3-4 hours)
```
When: End of PI (every 12 weeks)
Who: Entire ART
Purpose: Big retrospective for entire ART

Three parts:
1. PI System Demo (show all PI work)
2. Quantitative measurement (metrics review)
3. Problem solving workshop (fix big issues)
```

[⬆ Back to Top](#-navigation)

---

## 8. PI Planning Deep Dive

---

### 8.1 What is PI Planning?

```
PI = Program Increment = 12 weeks of work
PI Planning = 2-day event to plan those 12 weeks

Think of it as:
Sprint Planning = plan 2 weeks
PI Planning    = plan 12 weeks for ALL teams together
```

---

### 8.2 Before PI Planning

```
2-4 weeks before PI Planning:
─────────────────────────────
Product Manager prepares:
→ Top 10 features for next PI
→ Business context presentation
→ Product vision update

System Architect prepares:
→ Architectural vision
→ Technical direction
→ Infrastructure needs

RTE prepares:
→ Logistics and facilitation plan
→ Team capacity calculation
→ Previous PI metrics review

Product Owners prepare:
→ Team backlog refinement
→ Story preparation for PI
→ Dependency identification
```

---

### 8.3 During PI Planning (2 Days)

```
DAY 1:
────────────────────────────────
Morning:
→ Business Context (Executives present)
→ Product Manager presents top features
→ System Architect presents tech vision
→ RTE explains PI Planning process

Afternoon:
→ Teams break out into their rooms
→ Teams plan their sprints for next PI
→ Teams identify dependencies with other teams
→ Teams identify risks

DAY 2:
────────────────────────────────
Morning:
→ Teams finalize sprint plans
→ Draft PI Objectives created
→ Program Board updated with dependencies

Afternoon:
→ Teams present their PI plans
→ Risk identification (ROAM session)
→ Confidence vote
→ Final PI Objectives agreed
```

---

### 8.4 Program Board

```
Visual board showing ALL teams' work and dependencies:

         Sprint 1   Sprint 2   Sprint 3   Sprint 4
Team A  [Feature1] [Feature2]            [Feature5]
           ↓ depends on
Team B  [Feature3] [Feature4] [Feature5]
                               ↑ depends on
Team C             [Feature6] [Feature7]

Arrows show dependencies between teams!
RTE manages these dependencies.
```

---

### 8.5 PI Objectives

```
After PI Planning each team has:
Team PI Objectives = What our team will deliver this PI

Example (Ariane Team - MFR 2.0):
1. Make homepage mobile responsive (High value)
2. Make product listing page responsive (High value)
3. Mobile navigation component (High value)
4. Performance optimization for mobile (Medium value)
5. Stretch: Mobile checkout flow (Stretch goal)
```

---

### 8.6 Confidence Vote

```
At end of PI Planning, team votes confidence:

🖐️ Fist of Five Vote:
5 fingers = Very confident, great plan!
4 fingers = Good plan, minor concerns
3 fingers = OK plan, some concerns
2 fingers = Not confident, major concerns
1 finger  = Very low confidence
Fist (0)  = Will not work, need to replan

Goal: Average of 3.5+ to proceed
Below 3 = Replan needed
```

---

### 8.7 After PI Planning

```
Week 1-8 (Sprints 1-4):
→ Teams execute their plans
→ Daily standups
→ Sprint ceremonies
→ ART sync weekly
→ System Demo every sprint

Week 9-10 (Sprint 5 - IP Sprint):
→ Innovation and Planning sprint
→ Fix technical debt
→ Explore new ideas
→ Plan for next PI
→ Inspect and Adapt workshop
```

[⬆ Back to Top](#-navigation)

---

## 9. Complete Project Execution Flow

---

### 9.1 From Idea to Delivery — Full Flow

```
STEP 1: IDEA / BUSINESS NEED
─────────────────────────────
Who: Business Stakeholders / Executives
What: "We need our React app to work on mobile"
Output: Business case

        ↓

STEP 2: EPIC CREATION
─────────────────────────────
Who: Product Management / Epic Owners
What: Create Epic in Portfolio backlog
Output: Approved and funded Epic
Example: "MFR 2.0 - Mobile Responsive React App"

        ↓

STEP 3: FEATURE BREAKDOWN
─────────────────────────────
Who: Product Manager + System Architect
What: Break Epic into Features
Output: Features in Program Backlog
Examples:
→ "Mobile Responsive Homepage"
→ "Mobile Navigation Component"
→ "Mobile Product Listing Page"
→ "Mobile Checkout Flow"

        ↓

STEP 4: PI PLANNING
─────────────────────────────
Who: Entire ART (all teams together)
What: Plan which features go into next PI
Output: Team PI Objectives + Sprint Plans
Duration: 2 days

        ↓

STEP 5: STORY WRITING
─────────────────────────────
Who: BA + Product Owner
What: Break Features into User Stories
Output: Ready stories in team backlog
Example: "As a mobile user, I want hamburger 
          menu so I can navigate on small screens"

        ↓

STEP 6: SPRINT PLANNING
─────────────────────────────
Who: Dev Team + QA + PO + Scrum Master
What: Pick stories for this 2-week sprint
Output: Sprint backlog with committed stories
Duration: 2-4 hours

        ↓

STEP 7: DEVELOPMENT
─────────────────────────────
Who: Developers
What: Build the features/stories
Activities:
→ Pick story from sprint board
→ Move to "In Progress"
→ Write code
→ Write unit tests
→ Code review with teammate
→ Move to "Dev Complete"
Duration: Days to 1 week per story

        ↓

STEP 8: QA TESTING
─────────────────────────────
Who: QA Engineers ← You!
What: Test the completed stories
Activities:
→ Read acceptance criteria
→ Write test cases
→ Execute tests on QA environment
→ Report bugs in Jira if found
→ Developer fixes bugs
→ Retest fixed bugs
→ Move to "Test Complete"
Duration: 1-3 days per story

        ↓

STEP 9: PO ACCEPTANCE
─────────────────────────────
Who: Product Owner
What: Verify story meets requirements
Activities:
→ Review completed work
→ Check acceptance criteria met
→ Accept → move to "Done"
→ Reject → back to developer

        ↓

STEP 10: SYSTEM DEMO
─────────────────────────────
Who: All teams + Stakeholders
What: Demo all completed work
Frequency: Every sprint end
Output: Stakeholder feedback

        ↓

STEP 11: RELEASE / DEPLOYMENT
─────────────────────────────
Who: Release Manager + DevOps
What: Deploy to production
Activities:
→ Release planning
→ Go/No-Go decision
→ Deploy to staging
→ Final verification
→ Deploy to production
→ Monitor for issues

        ↓

STEP 12: INSPECT AND ADAPT
─────────────────────────────
Who: Entire ART
What: Review PI results and improve
Frequency: End of every PI (12 weeks)
Output: Improvement actions for next PI
```

---

### 9.2 Where Your Role (QA) Fits

```
Your main involvement:
Step 5  → Review stories for testability
Step 6  → Estimate QA effort in sprint planning
Step 7  → Collaborate with devs on acceptance criteria
Step 8  → YOUR PRIMARY WORK (testing)
Step 9  → Support PO in acceptance
Step 10 → Demonstrate test coverage in demos
Step 11 → Regression testing before release
```

[⬆ Back to Top](#-navigation)

---

## 10. DevOps and CI/CD in SAFe

---

### 10.1 What is CI/CD?

```
CI = Continuous Integration
CD = Continuous Delivery / Deployment

CI: Developers merge code frequently
    Every merge triggers automatic tests
    Catch bugs early!

CD: After CI passes, code automatically
    moves toward production
    Faster, safer releases!
```

---

### 10.2 Environments

```
DEV Environment
────────────────
→ Developer's local machine or shared dev server
→ Code is written and unit tested here
→ Very unstable, changes constantly

        ↓

QA Environment
────────────────
→ Stable environment for QA testing
→ Code deployed here after dev complete
→ QA Engineers test here ← You work here!
→ Bugs found and fixed here

        ↓

STAGING Environment
────────────────────
→ Mirror of production
→ Final testing before go-live
→ UAT (User Acceptance Testing) happens here
→ Performance testing here

        ↓

PRODUCTION Environment
────────────────────────
→ Live environment (real users!)
→ Only fully tested code goes here
→ Release Manager controls deployments
→ Monitored 24/7
```

---

### 10.3 CI/CD Pipeline

```
Developer pushes code
        ↓
Automated build runs
        ↓
Unit tests run automatically
        ↓
Code quality check (SonarQube etc)
        ↓
Deploy to QA environment automatically
        ↓
QA team tests manually + automation tests run
        ↓
QA approves → Deploy to Staging
        ↓
Final checks on Staging
        ↓
Release Manager gives Go/No-Go
        ↓
Deploy to Production
        ↓
Monitor production
```

---

### 10.4 SAFe DevOps CALMR

```
SAFe's DevOps approach called CALMR:

C - Culture      : Everyone responsible for quality
A - Automation   : Automate everything possible
L - Lean Flow    : Smooth flow from dev to production
M - Measurement  : Measure everything
R - Recovery     : Fast recovery when things go wrong
```

[⬆ Back to Top](#-navigation)

---

## 11. Metrics and Reporting

---

### 11.1 Team Level Metrics

#### Velocity
```
What: How many story points team completes per sprint
Why: Predict how much work team can do in future sprints

Example:
Sprint 1: 24 points
Sprint 2: 28 points
Sprint 3: 22 points
Average Velocity: 24.6 points per sprint

Use: If next sprint has 30 points → team is overcommitted!
```

#### Burndown Chart
```
What: Shows remaining work in sprint over time
X-axis: Days in sprint
Y-axis: Story points remaining

Ideal line: Straight diagonal from top-left to bottom-right
Good sign: Actual line close to ideal
Bad sign: Actual line flat or going up (not completing work!)
```

#### Burnup Chart
```
What: Shows completed work over time
Shows scope changes clearly
Good for tracking PI progress
```

---

### 11.2 ART Level Metrics

#### PI Burndown
```
Shows PI progress across all teams
RTE tracks this weekly
Red = behind schedule
Green = on track
```

#### Feature Progress
```
How many features planned vs completed
Tracked by Product Manager
Reported in ART Sync
```

#### Predictability
```
What percentage of PI Objectives achieved
Target: 80%+ predictability
Formula: (Actual points / Planned points) × 100
```

---

### 11.3 Quality Metrics (Important for QA!)

```
Defect Density      : Bugs per feature
Defect Escape Rate  : Bugs found in production (lower = better!)
Test Coverage       : % of code covered by tests
Automation Rate     : % of tests automated
Mean Time to Detect : How fast bugs are found
Mean Time to Resolve: How fast bugs are fixed
```

[⬆ Back to Top](#-navigation)

---

## 12. Stakeholder Management

---

### 12.1 Who Are Stakeholders?

```
Internal Stakeholders:
→ Business owners
→ Executives / VPs
→ Other teams depending on your work
→ Marketing, Sales, Support teams

External Stakeholders:
→ End customers (in some cases)
→ Vendors and partners
→ Regulatory bodies
```

---

### 12.2 How PO/PM Manages Stakeholders

```
Regular Communication:
→ Sprint Reviews (every 2 weeks)
→ System Demo (every sprint)
→ PI Planning (every 12 weeks)
→ Stakeholder update emails
→ Roadmap reviews

Feedback Loops:
→ Demo → Feedback → Backlog update → Build → Demo again
```

---

### 12.3 Stakeholder Engagement Model

```
HIGH INTEREST + HIGH INFLUENCE = Manage Closely
(Executives, Business Owners)
→ Regular updates, involve in decisions

HIGH INTEREST + LOW INFLUENCE = Keep Informed
(End users, Support teams)
→ Newsletter, demo invites

LOW INTEREST + HIGH INFLUENCE = Keep Satisfied
(Legal, Security teams)
→ Consult when needed, keep happy

LOW INTEREST + LOW INFLUENCE = Monitor
(General public)
→ Minimal effort
```

[⬆ Back to Top](#-navigation)

---

## 13. Jira and Confluence in SAFe

---

### 13.1 Jira in SAFe

```
Hierarchy in Jira mirrors SAFe hierarchy:

Portfolio Level → Epic
Program Level  → Feature (or Epic in some setups)
Team Level     → Story → Sub-task

Your Jira (DirecTV):
Epic   = Large feature (SCIM-2188 type)
Story  = AR-8255 type tickets
Sub-task = Individual tasks inside story
```

---

### 13.2 Jira Board in SAFe

```
Your sprint board columns:
TO DO → IN PROGRESS → DEV COMPLETE → RETEST → TEST COMPLETE → DONE

This maps to workflow:
Story assigned → Dev working → Dev done → QA testing → QA done → PO accepted
```

---

### 13.3 Confluence in SAFe

```
Confluence stores:
Team Level:
→ Sprint meeting notes
→ Team working agreements
→ Test plans and test cases
→ Onboarding guides

ART Level:
→ PI Planning outputs
→ Program board
→ ART retrospective notes
→ Feature documentation

Portfolio Level:
→ Epic hypothesis statements
→ Lean business cases
→ Portfolio roadmap
```

---

### 13.4 How Jira and Confluence Link

```
Jira Ticket (Story AR-8255)
        ↕ linked
Confluence Page (Test Plan / Requirements / Design Doc)

In a ticket you can:
→ Link to Confluence pages
→ Attach design docs
→ Reference test plans
→ Link to related tickets
```

[⬆ Back to Top](#-navigation)

---

## 14. Real World Challenges

---

### 14.1 Common Problems in SAFe

#### Dependencies Between Teams
```
Problem: Team A waits for Team B to finish API before they can build UI
Solution:
→ Identify in PI Planning
→ Mark on Program Board
→ RTE tracks weekly
→ Escalate early if blocked
```

#### Scope Creep
```
Problem: Business keeps adding new requirements mid-sprint
Solution:
→ PO protects team from mid-sprint changes
→ New items go to backlog
→ Discussed in next sprint planning
→ No changes during sprint (sacred rule!)
```

#### Team Velocity Mismatch
```
Problem: One team too fast, another too slow — creating bottlenecks
Solution:
→ RTE balances team sizes
→ Faster team helps slower team
→ Workload redistribution in PI Planning
```

#### Technical Debt
```
Problem: Quick fixes pile up making code harder to maintain
Solution:
→ Allocate 20% of sprint capacity to tech debt
→ System Architect tracks architectural debt
→ IP Sprint used for cleanup
```

---

### 14.2 What Happens When Sprint Fails?

```
Scenario: Team commits to 30 points but only finishes 20

Step 1: Identify in sprint review
        → Be transparent about what's done vs not done

Step 2: Retrospective
        → Why did we not finish?
        → Overcommitted? Blockers? Scope unclear?

Step 3: Unfinished stories
        → Move to next sprint (if still relevant)
        → Return to backlog (if priority changed)

Step 4: PI Objectives impact
        → Update PI confidence if major stories missed
        → Communicate to stakeholders early!

Key rule: Never mark something Done if it is NOT done!
```

---

### 14.3 What Happens When There Are Production Bugs?

```
Critical Bug in Production:
Step 1: DevOps/Release Manager identifies issue
Step 2: Incident created
Step 3: On-call developer investigates
Step 4: Hotfix branch created
Step 5: QA tests the fix in staging
Step 6: Emergency release to production
Step 7: Post-mortem meeting
Step 8: Preventive actions added to backlog
```

[⬆ Back to Top](#-navigation)

---

## 15. Tools Used

---

### 15.1 Tools by Role

| Role | Primary Tools |
|---|---|
| BA | Confluence, Jira, Lucidchart, Visio, Figma (basic) |
| Product Owner | Jira, Confluence, Excel/Sheets |
| Product Manager | Jira Align, Confluence, Roadmap tools |
| Scrum Master | Jira, Confluence, Excel, Miro |
| RTE | Jira Align, Confluence, Excel, Program Board tools |
| System Architect | Draw.io, Lucidchart, Confluence, code repos |
| Developer | VS Code, GitHub/Bitbucket, Jira, Confluence |
| QA Engineer | Jira, Confluence, Postman, Selenium, Cypress, Excel |
| DevOps | Jenkins, GitHub Actions, Docker, Kubernetes, AWS/Azure |
| Technical Writer | Confluence, Word, MadCap Flare, Git |

---

### 15.2 Tools by Purpose

| Purpose | Tools |
|---|---|
| Project Tracking | Jira, Azure DevOps, Jira Align |
| Documentation | Confluence, SharePoint, Notion |
| Communication | Microsoft Teams, Slack, Outlook |
| Diagramming | Lucidchart, Draw.io, Miro, Visio |
| Design/Wireframes | Figma, Adobe XD, Balsamiq |
| Code Repository | GitHub, Bitbucket, GitLab |
| CI/CD | Jenkins, GitHub Actions, Azure DevOps |
| Testing | Selenium, Cypress, Postman, JMeter |
| Monitoring | Splunk, Datadog, New Relic, PagerDuty |
| Cloud | AWS, Azure, Google Cloud |

[⬆ Back to Top](#-navigation)

---

## 16. New Joiner Guide

---

### 16.1 First Day Checklist

```
Access & Setup:
✅ Get Jira access
✅ Get Confluence access
✅ Get email setup (Outlook)
✅ Join Microsoft Teams channels
✅ Get VPN access
✅ Get code repository access (GitHub/Bitbucket)
✅ Get QA environment access (URLs + credentials)

People to Meet:
✅ Your direct manager
✅ Your Scrum Master
✅ Your Product Owner
✅ Your BA
✅ Fellow QA engineers
✅ Developers you'll work with
```

---

### 16.2 First Week Checklist

```
Understanding the Project:
✅ Read Confluence onboarding pages
✅ Understand the product (use the actual app!)
✅ Read recent sprint notes
✅ Look at Done tickets in Jira to understand what was built
✅ Ask for architecture overview from System Architect or Tech Lead
✅ Understand the tech stack (React, APIs, databases)

Understanding the Process:
✅ Attend daily standup (just listen first)
✅ Understand sprint cycle
✅ Understand bug reporting process in Jira
✅ Understand test environments (Dev/QA/Staging/Prod)
✅ Understand release process

QA Specific:
✅ Get existing test cases/test plans
✅ Understand what is already automated
✅ Understand regression suite
✅ Run existing tests on QA environment
✅ Understand bug severity/priority levels
```

---

### 16.3 First Month Goals

```
Week 1: Learn and observe
Week 2: Shadow senior QA, write first test cases
Week 3: Take first story to test independently
Week 4: Contribute to sprint fully, raise bugs independently
```

---

### 16.4 Questions to Ask Your Lead

```
About Project:
→ What is the main product we are building?
→ What is the current PI goal?
→ What did we deliver last PI?
→ What is the biggest challenge right now?

About Process:
→ How do we report bugs? (severity/priority scale)
→ What is our definition of done?
→ How do we handle regression testing?
→ What is our test automation strategy?

About Access:
→ Which environments can I test on?
→ Where are test credentials stored?
→ Where are existing test cases?
→ Which Confluence pages should I read first?
```

[⬆ Back to Top](#-navigation)

---

## 17. Glossary

| Term | Full Form | Meaning |
|---|---|---|
| ART | Agile Release Train | Group of 5-12 agile teams working together |
| BA | Business Analyst | Bridges business needs and technical team |
| BRD | Business Requirement Document | Detailed business requirements |
| CALMR | Culture, Automation, Lean Flow, Measurement, Recovery | SAFe DevOps approach |
| CD | Continuous Delivery/Deployment | Automated code delivery to environments |
| CI | Continuous Integration | Frequent code merges with automated testing |
| CSPO | Certified Scrum Product Owner | PO certification by Scrum Alliance |
| CSM | Certified Scrum Master | SM certification by Scrum Alliance |
| DevEx | Developer Experience | Making developer tools and workflows better |
| DoD | Definition of Done | Criteria for work to be considered complete |
| DoR | Definition of Ready | Criteria for story to enter a sprint |
| Epic | — | Largest unit of work spanning multiple PIs |
| Feature | — | Mid-size work item, deliverable in one PI |
| FRD | Functional Requirement Document | Detailed functional requirements |
| I&A | Inspect and Adapt | End of PI retrospective event |
| IP Sprint | Innovation and Planning Sprint | Last sprint of PI for planning and innovation |
| ISTQB | International Software Testing Qualifications Board | QA certification body |
| Jira | — | Project tracking tool by Atlassian |
| PI | Program Increment | 12-week planning and delivery cycle in SAFe |
| PMP | Project Management Professional | Project management certification |
| PO | Product Owner | Owns team backlog and prioritizes work |
| PM | Product Manager | Owns product vision at ART level |
| POPM | Product Owner/Product Manager | SAFe certification for PO and PM |
| PSM | Professional Scrum Master | SM certification by Scrum.org |
| ROAM | Resolved Owned Accepted Mitigated | Risk management framework in SAFe |
| RTE | Release Train Engineer | Agile coach for entire ART |
| RTM | Requirement Traceability Matrix | Maps requirements to test cases |
| SAFe | Scaled Agile Framework | Framework for scaling agile in large organizations |
| SCIM | Self Care and Identity Management | Team/module name at DirecTV |
| SM | Scrum Master | Facilitates team agile process |
| SPC | SAFe Program Consultant | Advanced SAFe certification |
| Sprint | — | 2-week delivery cycle in Scrum/SAFe |
| Story | User Story | Small unit of work done in one sprint |
| UAT | User Acceptance Testing | Business validates final product |
| WIP | Work In Progress | Work started but not completed |

[⬆ Back to Top](#-navigation)

---

## 18. Summary Table — Roles, Certifications and Salary

### 18.1 Complete Role Summary

| Role | Key Responsibility | Reports To | Works With |
|---|---|---|---|
| Business Analyst | Requirements gathering and documentation | PO / Project Manager | PO, Dev, QA, Stakeholders |
| Product Owner | Backlog ownership and prioritization | Product Manager | BA, Dev Team, SM |
| Product Manager | Product vision and ART level features | Solution Manager | POs, RTE, Architect |
| Scrum Master | Team agile facilitation | RTE | Dev Team, QA, PO |
| RTE | ART coordination and PI facilitation | Solution Manager | All SMs, PMs, Architect |
| Solution Manager | Large solution vision | Executives | RTEs, Product Managers |
| System Architect | Technical architecture decisions | Solution Architect | Developers, RTE |
| Developer | Build features | Tech Lead / SM | QA, BA, Other Devs |
| QA Engineer | Test features and ensure quality | QA Lead / SM | Developers, BA, PO |
| DevOps/Release Manager | Deployments and infrastructure | Engineering Manager | Dev, QA, RTE |
| Technical Writer | Documentation | Manager | All teams |
| Knowledge Mgmt Specialist | Organize company knowledge | Manager | All teams |
| DevEx Engineer | Developer productivity | Engineering Manager | All Developers |
| Developer Advocate | Developer community and relations | VP DevRel | External Developers |

---

### 18.2 Certifications Summary

| Role | Entry Certification | Mid Certification | Advanced Certification |
|---|---|---|---|
| BA | ECBA | CCBA | CBAP |
| Product Owner | CSPO | A-CSPO | SAFe POPM |
| Product Manager | SAFe POPM | Pragmatic Marketing | MBA |
| Scrum Master | CSM / PSM I | A-CSM / PSM II | SAFe RTE |
| RTE | SAFe Scrum Master | SAFe Advanced SM | SAFe RTE / SPC |
| Solution Manager | SAFe for Teams | SAFe POPM | SAFe SPC |
| System Architect | SAFe for Architects | TOGAF | AWS/Azure Architect |
| Developer | Cloud Associate | AWS Developer | Solutions Architect |
| QA Engineer | ISTQB Foundation | ISTQB Advanced | SAFe Practitioner |
| DevOps/Release Manager | ITIL Foundation | AWS DevOps | SAFe DevOps (SDP) |
| Technical Writer | Google Tech Writing | CPTC | API Documentation |
| DevEx Engineer | Cloud certifications | DevOps certifications | Platform Engineering |
| Developer Advocate | No formal cert — portfolio required | Speaking + community | Conference presence |

---

### 18.3 Salary Table — India (LPA = Lakhs Per Annum)

| Role | Entry (0-2 yrs) | Mid (3-6 yrs) | Senior (7+ yrs) |
|---|---|---|---|
| Business Analyst | 4-8 LPA | 8-15 LPA | 15-30 LPA |
| Product Owner | 8-12 LPA | 15-25 LPA | 25-40 LPA |
| Product Manager | 12-18 LPA | 20-35 LPA | 35-60 LPA |
| Scrum Master | 8-12 LPA | 15-25 LPA | 25-40 LPA |
| RTE | 18-25 LPA | 25-40 LPA | 40-65 LPA |
| Solution Manager | 20-30 LPA | 30-50 LPA | 50-80 LPA |
| System Architect | 20-30 LPA | 30-50 LPA | 50-90 LPA |
| Developer (Frontend/React) | 4-8 LPA | 10-20 LPA | 20-40 LPA |
| QA Engineer | 3-6 LPA | 6-15 LPA | 15-30 LPA |
| SDET / Automation QA | 6-10 LPA | 12-22 LPA | 22-40 LPA |
| DevOps Engineer | 6-12 LPA | 15-25 LPA | 25-45 LPA |
| Release Manager | 10-15 LPA | 18-30 LPA | 30-50 LPA |
| Technical Writer | 4-7 LPA | 8-15 LPA | 15-25 LPA |
| Knowledge Mgmt Specialist | 4-8 LPA | 8-15 LPA | 15-25 LPA |
| DevEx Engineer | 15-20 LPA | 25-40 LPA | 40-70 LPA |
| Developer Advocate | 12-18 LPA | 20-35 LPA | 35-60 LPA |

---

### 18.4 Career Path Summary

| Starting Role | Path 1 | Path 2 | Path 3 |
|---|---|---|---|
| QA Engineer | → Senior QA → QA Lead → QA Manager | → SDET → Automation Architect | → BA → Product Owner |
| Developer | → Senior Dev → Tech Lead → Architect → CTO | → DevOps → Release Manager | → Scrum Master → RTE |
| BA | → Senior BA → Product Owner → Product Manager | → Scrum Master → RTE | → Project Manager |
| Scrum Master | → Senior SM → RTE → Agile Coach | → Product Owner | → Project Manager |
| Junior PO | → PO → Senior PO → Product Manager → VP Product | → Solution Manager | → BA Lead |

[⬆ Back to Top](#-navigation)

---

*Document created for DirecTV Ariane Team — New Joiner Reference Guide*
*Last updated: May 2026*