# Roles

The ops dev team has **4 roles**: one dev & UI lead agent, one developer agent, one security auditor agent, and one human product owner. Each role has distinct responsibilities and required capabilities.

---

## Dev & UI Lead

**Primary function:** Requirement gathering from all sources, UI design, task assignment, code deployment, and service operations.

### Responsibilities

- Gather requirements from multiple sources: investment team needs, product direction from the Product Owner, operational needs
- Convert requirements into actionable development tasks with clear scope and acceptance criteria
- Produce UI designs (Figma output) for user-facing features
- Assign tasks to the Developer with design artifacts and specifications
- Review completed work before deployment
- Handle code deployment and service launch (CI/CD, server configuration, DNS, etc.)
- Monitor deployed services for health and performance
- Coordinate between the dev team and the investment team (the Dev Lead may serve both)

### Required Capabilities

| Capability | Why |
|------------|-----|
| Persistent memory | Maintains project context, requirement history, and deployment state across sessions |
| Scheduler / cron | Self-wakes for deployment monitoring, service health checks, follow-ups |
| External communication | Messages team channels, interacts with deployment platforms |
| UI design tools | Produces Figma-compatible design outputs |
| Code execution | Deployment scripts, service configuration, infrastructure management |
| Always-on availability | 24/7 uptime for service monitoring and incident response |

### Communication Patterns

| Target | Channel | Purpose |
|--------|---------|---------|
| Product Owner | `#general` / external | Requirement clarification, acceptance requests |
| Developer | DM / `#dev` | Task assignment, design handoff, technical questions |
| Security Auditor | `#dev` | Security review coordination, deployment clearance |
| Investment team | Cross-team channels | Gather investment tooling requirements |

### Escalation Path

- If requirements are ambiguous → clarify with the Product Owner
- If deployment fails → rollback, investigate, notify Product Owner and Developer
- If investment team and dev tasks conflict → prioritize based on Product Owner's direction

### Notes

- The Dev Lead may also serve on the investment team as Investment Coordinator
- The dual role creates a natural bridge: the Dev Lead understands investment team needs firsthand and translates them into development tasks
- UI design output is key — the Dev Lead produces visual designs, not just wireframes

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a cloud server (for always-on availability, deployment, and monitoring)
- Any persistent agent platform with design tools, code execution, and communication

---

## Developer

**Primary function:** Full-stack development, data pipelines, and tool building for investment operations.

### Responsibilities

- Accept task assignments from the Dev & UI Lead
- Develop features on isolated feature branches following the Dev Lead's design specifications
- Build data pipelines (market data ingestion, on-chain data processing, analytics feeds)
- Build monitoring dashboards for investment operations
- Build trading tools and utilities as specified
- Maintain and improve the investment platform infrastructure
- Submit code for security review by the Security Auditor
- Iterate on code based on security review feedback
- Write tests and documentation for developed features

### Required Capabilities

| Capability | Why |
|------------|-----|
| Code execution | Full development environment — frontend, backend, data engineering |
| Git workflow | Branch, commit, push, open PRs, respond to review comments |
| Data engineering | Build ETL pipelines, work with APIs, process structured/unstructured data |
| Frontend development | Build dashboards, UIs based on Figma designs |
| Backend development | APIs, services, database operations |
| Communication | Receives tasks via DM/`#dev`, reports progress, delivers code |

### Communication Patterns

| Target | Channel | Purpose |
|--------|---------|---------|
| Dev Lead | DM / `#dev` | Receive tasks, report progress, ask design questions, notify completion |
| Security Auditor | DM | Respond to security findings, discuss fixes |

### Escalation Path

- If task scope is unclear → ask the Dev Lead for clarification via DM
- If blocked by infrastructure or access issues → notify the Dev Lead in `#dev`
- If security review raises architectural concerns → discuss with the Dev Lead before changing approach

### Notes

- The Developer may also serve on the investment team as Investment Engineer (model building, strategy backtesting)
- Development tasks and investment modeling tasks come through different channels (`#dev` vs `#research`)
- Priority conflicts should be resolved by the Dev Lead (who may coordinate both teams)

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a cloud or local machine
- Any agent with full-stack development capabilities, Git workflow, and communication

---

## Security Auditor

**Primary function:** Code security review, systemic risk scanning, and code quality assurance.

### Responsibilities

- Review all code changes for security vulnerabilities before deployment
- Perform systemic risk scanning across the codebase and infrastructure
- Check code quality (error handling, edge cases, performance anti-patterns)
- Scan for dependency vulnerabilities (`npm audit`, `pip audit`, etc.)
- Verify that security-sensitive operations (API key handling, authentication, data access) follow best practices
- Report findings to the Developer (for fixing) and the Dev Lead (for awareness)
- Maintain a security review log

### Required Capabilities

| Capability | Why |
|------------|-----|
| Code analysis | Read and analyze code for security vulnerabilities, logic errors, quality issues |
| Security knowledge | Identify OWASP top 10, dependency vulnerabilities, crypto-specific risks |
| Persistent memory | Maintains security review history and known vulnerability patterns |
| Communication | Posts findings via DM to Developer, summaries to `#dev` |

### Communication Patterns

| Target | Channel | Purpose |
|--------|---------|---------|
| Developer | DM | Detailed security findings, code quality feedback, fix requests |
| Dev Lead | `#dev` | Security review summaries, deployment clearance |
| Product Owner | `#general` (critical only) | Critical security vulnerabilities that affect product decisions |

### Escalation Path

- Routine findings → DM to Developer for fixing
- Critical vulnerabilities → DM to Developer + immediate notification to Dev Lead in `#dev`
- Systemic risks (architectural flaws, data exposure risks) → escalate to Product Owner via Dev Lead

### Why a Separate Security Role?

- **Independence:** The auditor did not write the code, providing objective review
- **Systemic view:** Scans for cross-cutting risks that developers may miss
- **Gate function:** No code deploys without security clearance from the Security Auditor
- **Dual expertise:** The same agent may audit investment logic on the investment team, creating holistic security coverage across code and business logic

### Notes

- The Security Auditor may also serve on the investment team as the Security Committee member
- Code security review and investment logic audit are complementary — vulnerabilities in code can directly affect investment safety
- Systemic risk scanning covers both code-level and platform-level concerns (e.g., vulnerabilities in external platforms, DeFi integrations)

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a cloud server (shared with investment team role)
- Any agent with code analysis capabilities and communication

---

## Product Owner (Human)

**Primary function:** Product direction, requirement confirmation, and acceptance.

### Responsibilities

- Define product direction and priorities for investment tooling
- Confirm requirements before development begins
- Accept or reject delivered features
- Make decisions on scope changes and priority conflicts
- Set quality standards and non-functional requirements (performance, availability, etc.)

### Interaction Model

The Product Owner interacts with the team through:

1. **hxa-connect channels** — Post requirements, review updates, accept deliveries
2. **External platforms** (Telegram, etc.) — Quick decisions, urgent confirmations
3. **Design reviews** — Review the Dev Lead's UI designs before development starts
4. **Product acceptance** — Validate deployed features against requirements

### How Much Human Involvement?

The goal is to minimize PO involvement to high-value decisions:

| Activity | PO Involvement |
|----------|---------------|
| Requirement gathering | Medium (Product Owner sets direction; Dev Lead gathers details from all sources) |
| UI design | Low (Dev Lead produces designs; Product Owner reviews) |
| Task assignment | None (Dev Lead handles) |
| Development | None (Developer handles) |
| Security review | None (Security Auditor handles) |
| Deployment | None (Dev Lead handles) |
| Product acceptance | High (Product Owner validates) |
| Priority conflicts | Medium (Product Owner resolves when Dev Lead escalates) |

---

## Role Interaction Map

```
                  Requirements + acceptance
    Product  ◄──────────────────────────────► Dev Lead
    Owner                                        │
                                                 │ task assignment
                                                 │ + design handoff
                                                 │
                                          ┌──────▼──────┐
                                          │  Developer   │
                                          └──────┬──────┘
                                                 │
                                          code for review
                                                 │
                                          ┌──────▼──────┐
                                          │  Security    │
                                          │  Auditor     │
                                          └──────┬──────┘
                                                 │
                                          findings → Developer fixes
                                          clearance → Dev Lead deploys
                                                 │
                                          ┌──────▼──────┐
                                          │  Dev Lead    │
                                          │ (Deploy+Ops) │
                                          └──────┬──────┘
                                                 │
                                             deployed
                                                 │
                                          ┌──────▼──────┐
                                          │  Product     │
                                          │  Owner       │
                                          │ (Acceptance) │
                                          └─────────────┘
```

## Scaling the Team

| Team Size | Configuration | Best For |
|-----------|--------------|----------|
| Minimum (3) | 1 Dev Lead + 1 Developer + 1 PO | Simple tooling, early-stage |
| Standard (4) | 1 Dev Lead + 1 Developer + 1 Security + 1 PO | Full development with security review |
| Scaled (5+) | 1 Dev Lead + 2+ Developers + 1 Security + 1 PO | Larger projects with parallel workstreams |

> This team is intentionally lean. The Dev Lead handles UI design and deployment, which in larger teams would be separate roles. If workload increases, consider adding a dedicated developer agent to handle parallel tasks.
