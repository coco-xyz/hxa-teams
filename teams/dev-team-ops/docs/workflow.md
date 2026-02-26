# Workflow

The complete development workflow from requirement to deployed product. This process is designed for a lean agent dev team building investment operations tooling, with human oversight at requirement confirmation and acceptance.

---

## Overview

```
Requirement Sources → Dev Lead Gathers → UI Design → Development → Security Review → Deployment → Acceptance
```

Each step is detailed below.

---

## 1. Requirement Gathering

**Who:** Dev & UI Lead
**Where:** Cross-team channels, `#general`, external inputs

The Dev Lead gathers requirements from multiple sources:
- **Investment team:** Tooling needs from the investment workflow (the Dev Lead observes these firsthand if also serving as Investment Coordinator)
- **Product Owner:** Product direction, feature requests, strategic priorities
- **Operations:** Infrastructure needs, monitoring requirements, performance issues

The Dev Lead consolidates requirements into actionable development tasks with:
- Clear scope and acceptance criteria
- Priority level
- Dependencies on other tasks or data sources
- Design requirements (if user-facing)

**Example:**
> Requirement: The investment team needs a real-time dashboard showing portfolio positions, signal history, and model outputs. Source: Investment team daily workflow gaps observed by the Dev Lead.

---

## 2. Requirement Confirmation

**Who:** Product Owner (Human)
**Where:** `#general` or DM

Before development begins, the Product Owner confirms:
- The requirement is correctly understood
- Priority is correct relative to other work
- Scope is appropriate (not too broad, not too narrow)

**Quality gate:** No development starts until the Product Owner confirms the requirement.

**Exception:** Bug fixes and urgent operational issues can proceed immediately, with the Product Owner notified after the fact.

---

## 3. UI Design

**Who:** Dev & UI Lead
**Where:** `#dev`

For user-facing features, the Dev Lead produces UI designs:
1. Translate requirements into visual designs (Figma output)
2. Include interaction flows, component specifications, and responsive layouts
3. Post designs to `#dev` for the Developer's reference
4. Optionally share with the Product Owner for design review

**Example design handoff:**
```
TO: Developer
TASK: Portfolio Dashboard — Frontend
DESIGN: [Figma link / exported screens]
SPECS:
  - Real-time position table (auto-refresh every 30s)
  - Signal history timeline (last 7 days, filterable by color)
  - Model output cards (VaR, drawdown, concentration)
  - Responsive: desktop-first, tablet-friendly
BACKEND: API endpoints TBD — Dev Lead will provide spec
PRIORITY: High
DEADLINE: 3 days
```

---

## 4. Task Assignment

**Who:** Dev & UI Lead → Developer
**Where:** DM (Dev Lead → Developer), `#dev`

The Dev Lead assigns tasks to the Developer with:
- Task description and scope
- Design artifacts (if applicable)
- Technical specifications (APIs, data formats, infrastructure requirements)
- Priority and deadline
- Dependencies and blockers

The Developer acknowledges and begins development.

**Communication during development:**
```
Developer in #dev:
  "Portfolio dashboard frontend — started. Using React + Recharts for charts.
   Question: Should signal history timeline use WebSocket for real-time updates
   or polling every 30s?"

Dev Lead in #dev:
  "Polling every 30s is fine for v1. We can add WebSocket in a later iteration."
```

---

## 5. Development

**Who:** Developer
**Where:** Feature branches, `#dev`

The Developer develops features following standard practices:

1. Create a feature branch from `main`
2. Develop the feature following the Dev Lead's design and specifications
3. Write tests (unit, integration as appropriate)
4. Run local checks (lint, test, build)
5. Notify Dev Lead and Security Auditor that code is ready for review

```
     main
       │
       ├── feat/portfolio-dashboard       ◄── Developer
       │
       ├── feat/signal-data-pipeline      ◄── Developer
       │
       └── feat/model-output-api          ◄── Developer
```

**Quality gate:** Code must pass local lint and test checks before requesting review.

---

## 6. Security Review

**Who:** Security Auditor
**Where:** DM (Security Auditor → Developer), `#dev`

Every code change goes through security review before deployment:

```
Developer notifies code ready
        │
        ▼
┌───────────────────────────┐
│  Security Auditor Reviews  │
│  - Security vulns          │──── Issues found ──→ Developer fixes ──┐
│  - Code quality            │                                        │
│  - Dependency audit        │                                        │
└────────┬──────────────────┘                                         │
         │                                                            │
         │◄───────────────────────────────────────────────────────────┘
         │
    No issues
         │
         ▼
   CLEARED for deployment
```

**What the Security Auditor checks:**
- Security vulnerabilities (injection, XSS, authentication flaws, API key exposure)
- Dependency vulnerabilities (`npm audit`, `pip audit`)
- Error handling and edge cases
- Data access patterns (is sensitive data properly protected?)
- Crypto-specific risks (if interacting with wallets, exchanges, or smart contracts)
- Code quality and maintainability

**Findings flow:**
1. Security Auditor DMs Developer with detailed findings
2. Developer fixes and notifies Security Auditor
3. Security Auditor re-reviews
4. Repeat until CLEARED
5. Security Auditor posts clearance summary to `#dev`

---

## 7. Deployment

**Who:** Dev & UI Lead
**Where:** `#dev`, `#general`

After security clearance, the Dev Lead handles deployment:

1. Merge code to `main` (or deployment branch)
2. Run deployment pipeline (build, deploy to server/platform)
3. Verify deployment succeeded (health checks, smoke tests)
4. Post deployment notification to `#dev` and `#general`

**Deployment notification:**
```
Dev Lead in #dev:
  "Deployed: Portfolio Dashboard v1.0
   - Real-time position table
   - Signal history timeline
   - Model output cards
   URL: https://dashboard.example.com
   Status: HEALTHY — all checks passing"
```

**If deployment fails:**
1. Rollback immediately to previous version
2. Notify Developer in `#dev`
3. Investigate root cause
4. Fix and re-deploy

---

## 8. Product Acceptance

**Who:** Product Owner (Human)
**Where:** `#general` or external platform

The Product Owner validates the deployed feature:
- Does it match the original requirement?
- Is the UI consistent with expectations?
- Does it work correctly in real usage?

**Outcomes:**
- **Accepted:** Feature ships. Dev Lead records completion.
- **Changes requested:** Dev Lead converts feedback into follow-up tasks for the Developer.
- **Rejected:** Dev Lead analyzes root cause (misunderstood requirement, design issue, etc.) and restarts from the appropriate step.

---

## Communication Patterns

### Channels

| Channel | Purpose | Participants |
|---------|---------|-------------|
| **#dev** | Development collaboration, code review, deployment notifications | Developer, Dev Lead |
| **#general** | Cross-team announcements, product updates, acceptance | All |
| **DM: Dev Lead ↔ Developer** | Task assignment, technical questions, design clarification | Dev Lead, Developer |
| **DM: Security Auditor ↔ Developer** | Security findings, code quality feedback | Security Auditor, Developer |

### Notification Flow

```
Requirement confirmed ──→ Dev Lead posts task to #dev
                              │
Development complete ──→ Developer notifies in #dev
                              │
Security review ──→ Security Auditor posts clearance to #dev
                              │
Deployed ──→ Dev Lead notifies #dev + #general
                              │
Acceptance ──→ Product Owner responds in #general
```

---

## Cross-Team Coordination

This team operates alongside the investment team. Key coordination points:

| Scenario | How It Works |
|----------|-------------|
| Investment team needs a new tool | Dev Lead observes the need as Investment Coordinator, converts to dev task |
| Model code needs updating | Developer works on it via `#research` (investment team) or `#dev` (dev team) depending on scope |
| Security issue spans both teams | Security Auditor coordinates across `#intel` (investment) and `#dev` (development) |
| Priority conflict | Dev Lead identifies the conflict, escalates to Product Owner for resolution |

---

## Automation Layer

| Automation | Owner | Mechanism |
|-----------|-------|-----------|
| Service health monitoring | Dev Lead | Scheduler polls health endpoints periodically |
| Deployment pipeline | Dev Lead | Triggered on code merge to main |
| Security dependency scan | Security Auditor | Periodic `npm audit` / `pip audit` |
| Task progress tracking | Dev Lead | Monitors `#dev` for updates, follows up on overdue tasks |
| Cross-team requirement sync | Dev Lead | Observes investment team needs, converts to dev tasks |

---

## Complete Flow Diagram

```
Requirement sources (investment team, Product Owner, operations)
         │
         ▼
Dev Lead gathers + consolidates requirements
         │
         ▼
Product Owner confirms requirement
         │
         ▼
Dev Lead produces UI design (if user-facing)
         │
         ▼
Dev Lead assigns task to Developer
         │
         ▼
Developer develops on feature branch
         │
         ▼
┌──────────────────────────────────┐
│  Security Auditor Security Review │    ◄── Iterate until CLEARED
│  Fix → Re-review → Fix → ...     │
└──────────────┬───────────────────┘
               │
               ▼
Dev Lead deploys to production
               │
               ▼
Dev Lead posts deployment notification
               │
               ▼
Product Owner accepts / requests changes
               │
               ▼
Feature complete → record + feedback loop
```
