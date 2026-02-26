# Workflow

The complete development workflow from requirement to deployed product. This process is designed for a lean agent dev team building investment operations tooling, with human oversight at requirement confirmation and acceptance.

---

## Overview

```
Requirement Sources → Roey Gathers → UI Design → Development → Security Review → Deployment → Acceptance
```

Each step is detailed below.

---

## 1. Requirement Gathering

**Who:** Dev & UI Lead (Roey)
**Where:** Cross-team channels, `#general`, external inputs

Roey gathers requirements from multiple sources:
- **Investment team:** Tooling needs from the investment workflow (Roey observes these firsthand as Investment Coordinator)
- **BMAN:** Product direction, feature requests, strategic priorities
- **Operations:** Infrastructure needs, monitoring requirements, performance issues

Roey consolidates requirements into actionable development tasks with:
- Clear scope and acceptance criteria
- Priority level
- Dependencies on other tasks or data sources
- Design requirements (if user-facing)

**Example:**
> Requirement: The investment team needs a real-time dashboard showing portfolio positions, signal history, and model outputs. Source: Investment team daily workflow gaps observed by Roey.

---

## 2. Requirement Confirmation

**Who:** Product Owner (BMAN)
**Where:** `#general` or DM

Before development begins, BMAN confirms:
- The requirement is correctly understood
- Priority is correct relative to other work
- Scope is appropriate (not too broad, not too narrow)

**Quality gate:** No development starts until BMAN confirms the requirement.

**Exception:** Bug fixes and urgent operational issues can proceed immediately, with BMAN notified after the fact.

---

## 3. UI Design

**Who:** Dev & UI Lead (Roey)
**Where:** `#dev`

For user-facing features, Roey produces UI designs:
1. Translate requirements into visual designs (Figma output)
2. Include interaction flows, component specifications, and responsive layouts
3. Post designs to `#dev` for Joey's reference
4. Optionally share with BMAN for design review

**Example design handoff:**
```
TO: Joey
TASK: Portfolio Dashboard — Frontend
DESIGN: [Figma link / exported screens]
SPECS:
  - Real-time position table (auto-refresh every 30s)
  - Signal history timeline (last 7 days, filterable by color)
  - Model output cards (VaR, drawdown, concentration)
  - Responsive: desktop-first, tablet-friendly
BACKEND: API endpoints TBD — Roey will provide spec
PRIORITY: High
DEADLINE: 3 days
```

---

## 4. Task Assignment

**Who:** Dev & UI Lead (Roey) → Developer (Joey)
**Where:** DM (Roey → Joey), `#dev`

Roey assigns tasks to Joey with:
- Task description and scope
- Design artifacts (if applicable)
- Technical specifications (APIs, data formats, infrastructure requirements)
- Priority and deadline
- Dependencies and blockers

Joey acknowledges and begins development.

**Communication during development:**
```
Joey in #dev:
  "Portfolio dashboard frontend — started. Using React + Recharts for charts.
   Question: Should signal history timeline use WebSocket for real-time updates
   or polling every 30s?"

Roey in #dev:
  "Polling every 30s is fine for v1. We can add WebSocket in a later iteration."
```

---

## 5. Development

**Who:** Developer (Joey)
**Where:** Feature branches, `#dev`

Joey develops features following standard practices:

1. Create a feature branch from `main`
2. Develop the feature following Roey's design and specifications
3. Write tests (unit, integration as appropriate)
4. Run local checks (lint, test, build)
5. Notify Roey and Zylos_ABCDE that code is ready for review

```
     main
       │
       ├── feat/portfolio-dashboard       ◄── Joey
       │
       ├── feat/signal-data-pipeline      ◄── Joey
       │
       └── feat/model-output-api          ◄── Joey
```

**Quality gate:** Code must pass local lint and test checks before requesting review.

---

## 6. Security Review

**Who:** Security Auditor (Zylos_ABCDE)
**Where:** DM (Zylos_ABCDE → Joey), `#dev`

Every code change goes through security review before deployment:

```
Joey notifies code ready
        │
        ▼
┌──────────────────────┐
│  Zylos_ABCDE Reviews  │
│  - Security vulns     │──── Issues found ──→ Joey fixes ──┐
│  - Code quality       │                                    │
│  - Dependency audit   │                                    │
└────────┬─────────────┘                                     │
         │                                                   │
         │◄──────────────────────────────────────────────────┘
         │
    No issues
         │
         ▼
   CLEARED for deployment
```

**What Zylos_ABCDE checks:**
- Security vulnerabilities (injection, XSS, authentication flaws, API key exposure)
- Dependency vulnerabilities (`npm audit`, `pip audit`)
- Error handling and edge cases
- Data access patterns (is sensitive data properly protected?)
- Crypto-specific risks (if interacting with wallets, exchanges, or smart contracts)
- Code quality and maintainability

**Findings flow:**
1. Zylos_ABCDE DMs Joey with detailed findings
2. Joey fixes and notifies Zylos_ABCDE
3. Zylos_ABCDE re-reviews
4. Repeat until CLEARED
5. Zylos_ABCDE posts clearance summary to `#dev`

---

## 7. Deployment

**Who:** Dev & UI Lead (Roey)
**Where:** `#dev`, `#general`

After security clearance, Roey handles deployment:

1. Merge code to `main` (or deployment branch)
2. Run deployment pipeline (build, deploy to server/platform)
3. Verify deployment succeeded (health checks, smoke tests)
4. Post deployment notification to `#dev` and `#general`

**Deployment notification:**
```
Roey in #dev:
  "Deployed: Portfolio Dashboard v1.0
   - Real-time position table
   - Signal history timeline
   - Model output cards
   URL: https://dashboard.example.com
   Status: HEALTHY — all checks passing"
```

**If deployment fails:**
1. Rollback immediately to previous version
2. Notify Joey in `#dev`
3. Investigate root cause
4. Fix and re-deploy

---

## 8. Product Acceptance

**Who:** Product Owner (BMAN)
**Where:** `#general` or external platform

BMAN validates the deployed feature:
- Does it match the original requirement?
- Is the UI consistent with expectations?
- Does it work correctly in real usage?

**Outcomes:**
- **Accepted:** Feature ships. Roey records completion.
- **Changes requested:** Roey converts feedback into follow-up tasks for Joey.
- **Rejected:** Roey analyzes root cause (misunderstood requirement, design issue, etc.) and restarts from the appropriate step.

---

## Communication Patterns

### Channels

| Channel | Purpose | Participants |
|---------|---------|-------------|
| **#dev** | Development collaboration, code review, deployment notifications | Joey, Roey |
| **#general** | Cross-team announcements, product updates, acceptance | All |
| **DM: Roey ↔ Joey** | Task assignment, technical questions, design clarification | Roey, Joey |
| **DM: Zylos_ABCDE ↔ Joey** | Security findings, code quality feedback | Zylos_ABCDE, Joey |

### Notification Flow

```
Requirement confirmed ──→ Roey posts task to #dev
                              │
Development complete ──→ Joey notifies in #dev
                              │
Security review ──→ Zylos_ABCDE posts clearance to #dev
                              │
Deployed ──→ Roey notifies #dev + #general
                              │
Acceptance ──→ BMAN responds in #general
```

---

## Cross-Team Coordination

This team operates alongside the investment team. Key coordination points:

| Scenario | How It Works |
|----------|-------------|
| Investment team needs a new tool | Roey observes the need as Investment Coordinator, converts to dev task |
| Model code needs updating | Joey works on it via `#research` (investment team) or `#dev` (dev team) depending on scope |
| Security issue spans both teams | Zylos_ABCDE coordinates across `#intel` (investment) and `#dev` (development) |
| Priority conflict | Roey identifies the conflict, escalates to BMAN for resolution |

---

## Automation Layer

| Automation | Owner | Mechanism |
|-----------|-------|-----------|
| Service health monitoring | Roey | Scheduler polls health endpoints periodically |
| Deployment pipeline | Roey | Triggered on code merge to main |
| Security dependency scan | Zylos_ABCDE | Periodic `npm audit` / `pip audit` |
| Task progress tracking | Roey | Monitors `#dev` for updates, follows up on overdue tasks |
| Cross-team requirement sync | Roey | Observes investment team needs, converts to dev tasks |

---

## Complete Flow Diagram

```
Requirement sources (investment team, BMAN, operations)
         │
         ▼
Roey gathers + consolidates requirements
         │
         ▼
BMAN confirms requirement
         │
         ▼
Roey produces UI design (if user-facing)
         │
         ▼
Roey assigns task to Joey
         │
         ▼
Joey develops on feature branch
         │
         ▼
┌─────────────────────────────┐
│  Zylos_ABCDE Security Review │    ◄── Iterate until CLEARED
│  Fix → Re-review → Fix → …   │
└──────────────┬──────────────┘
               │
               ▼
Roey deploys to production
               │
               ▼
Roey posts deployment notification
               │
               ▼
BMAN accepts / requests changes
               │
               ▼
Feature complete → record + feedback loop
```
