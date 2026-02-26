# Roles

The BMAN dev team has **4 roles**: one dev & UI lead agent, one developer agent, one security auditor agent, and one human product owner. Each role has distinct responsibilities and required capabilities.

> In the reference implementation, the team uses named agents (Roey, Joey, Zylos_ABCDE) + a human product owner (BMAN) on `boot.hxa.net`. Several agents also serve on the investment team — Roey as Investment Coordinator, Joey as Investment Engineer, and Zylos_ABCDE as Security Committee.

---

## Dev & UI Lead (Roey)

**Primary function:** Requirement gathering from all sources, UI design, task assignment, code deployment, and service operations.

### Responsibilities

- Gather requirements from multiple sources: investment team needs, product direction from BMAN, operational needs
- Convert requirements into actionable development tasks with clear scope and acceptance criteria
- Produce UI designs (Figma output) for user-facing features
- Assign tasks to the Developer (Joey) with design artifacts and specifications
- Review completed work before deployment
- Handle code deployment and service launch (CI/CD, server configuration, DNS, etc.)
- Monitor deployed services for health and performance
- Coordinate between the dev team and the investment team (Roey serves both)

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
| BMAN | `#general` / external | Requirement clarification, acceptance requests |
| Joey | DM / `#dev` | Task assignment, design handoff, technical questions |
| Zylos_ABCDE | `#dev` | Security review coordination, deployment clearance |
| Investment team | Cross-team channels | Gather investment tooling requirements |

### Escalation Path

- If requirements are ambiguous → clarify with BMAN
- If deployment fails → rollback, investigate, notify BMAN and Joey
- If investment team and dev tasks conflict → prioritize based on BMAN's direction

### Notes

- Roey also serves on the investment team as Investment Coordinator
- The dual role creates a natural bridge: Roey understands investment team needs firsthand and translates them into development tasks
- UI design output is key — Roey produces visual designs, not just wireframes

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a cloud server (for always-on availability, deployment, and monitoring)
- Any persistent agent platform with design tools, code execution, and communication

---

## Developer (Joey)

**Primary function:** Full-stack development, data pipelines, and tool building for investment operations.

### Responsibilities

- Accept task assignments from the Dev & UI Lead (Roey)
- Develop features on isolated feature branches following Roey's design specifications
- Build data pipelines (market data ingestion, on-chain data processing, analytics feeds)
- Build monitoring dashboards for investment operations
- Build trading tools and utilities as specified
- Maintain and improve the investment platform infrastructure
- Submit code for security review by Zylos_ABCDE
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
| Roey | DM / `#dev` | Receive tasks, report progress, ask design questions, notify completion |
| Zylos_ABCDE | DM | Respond to security findings, discuss fixes |

### Escalation Path

- If task scope is unclear → ask Roey for clarification via DM
- If blocked by infrastructure or access issues → notify Roey in `#dev`
- If security review raises architectural concerns → discuss with Roey before changing approach

### Notes

- Joey also serves on the investment team as Investment Engineer (model building, strategy backtesting)
- Development tasks and investment modeling tasks come through different channels (`#dev` vs `#research`)
- Priority conflicts should be resolved by Roey (who coordinates both teams)

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a cloud or local machine
- Any agent with full-stack development capabilities, Git workflow, and communication

---

## Security Auditor (Zylos_ABCDE)

**Primary function:** Code security review, systemic risk scanning, and code quality assurance.

### Responsibilities

- Review all code changes for security vulnerabilities before deployment
- Perform systemic risk scanning across the codebase and infrastructure
- Check code quality (error handling, edge cases, performance anti-patterns)
- Scan for dependency vulnerabilities (`npm audit`, `pip audit`, etc.)
- Verify that security-sensitive operations (API key handling, authentication, data access) follow best practices
- Report findings to Joey (for fixing) and Roey (for awareness)
- Maintain a security review log

### Required Capabilities

| Capability | Why |
|------------|-----|
| Code analysis | Read and analyze code for security vulnerabilities, logic errors, quality issues |
| Security knowledge | Identify OWASP top 10, dependency vulnerabilities, crypto-specific risks |
| Persistent memory | Maintains security review history and known vulnerability patterns |
| Communication | Posts findings via DM to Joey, summaries to `#dev` |

### Communication Patterns

| Target | Channel | Purpose |
|--------|---------|---------|
| Joey | DM | Detailed security findings, code quality feedback, fix requests |
| Roey | `#dev` | Security review summaries, deployment clearance |
| BMAN | `#general` (critical only) | Critical security vulnerabilities that affect product decisions |

### Escalation Path

- Routine findings → DM to Joey for fixing
- Critical vulnerabilities → DM to Joey + immediate notification to Roey in `#dev`
- Systemic risks (architectural flaws, data exposure risks) → escalate to BMAN via Roey

### Why a Separate Security Role?

- **Independence:** The auditor did not write the code, providing objective review
- **Systemic view:** Scans for cross-cutting risks that developers may miss
- **Gate function:** No code deploys without security clearance from Zylos_ABCDE
- **Dual expertise:** The same agent audits investment logic on the investment team, creating holistic security coverage across code and business logic

### Notes

- Zylos_ABCDE also serves on the investment team as Security Committee
- Code security review and investment logic audit are complementary — vulnerabilities in code can directly affect investment safety
- Systemic risk scanning covers both code-level and platform-level concerns (including OpenClaw vulnerabilities)

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a cloud server (shared with investment team role)
- Any agent with code analysis capabilities and communication

---

## Product Owner (BMAN — Human)

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
3. **Design reviews** — Review Roey's UI designs before development starts
4. **Product acceptance** — Validate deployed features against requirements

### How Much Human Involvement?

The goal is to minimize PO involvement to high-value decisions:

| Activity | PO Involvement |
|----------|---------------|
| Requirement gathering | Medium (BMAN sets direction; Roey gathers details from all sources) |
| UI design | Low (Roey produces designs; BMAN reviews) |
| Task assignment | None (Roey handles) |
| Development | None (Joey handles) |
| Security review | None (Zylos_ABCDE handles) |
| Deployment | None (Roey handles) |
| Product acceptance | High (BMAN validates) |
| Priority conflicts | Medium (BMAN resolves when Roey escalates) |

---

## Role Interaction Map

```
                  Requirements + acceptance
    BMAN ◄──────────────────────────────────► Roey (Dev & UI Lead)
                                                  │
                                                  │ task assignment
                                                  │ + design handoff
                                                  │
                                           ┌──────▼──────┐
                                           │    Joey      │
                                           │ (Developer)  │
                                           └──────┬──────┘
                                                  │
                                           code for review
                                                  │
                                           ┌──────▼──────┐
                                           │ Zylos_ABCDE  │
                                           │ (Security)   │
                                           └──────┬──────┘
                                                  │
                                           findings → Joey fixes
                                           clearance → Roey deploys
                                                  │
                                           ┌──────▼──────┐
                                           │    Roey      │
                                           │ (Deploy+Ops) │
                                           └──────┬──────┘
                                                  │
                                              deployed
                                                  │
                                           ┌──────▼──────┐
                                           │    BMAN      │
                                           │ (Acceptance) │
                                           └─────────────┘
```

## Scaling the Team

| Team Size | Configuration | Best For |
|-----------|--------------|----------|
| Minimum (3) | 1 Dev Lead + 1 Developer + 1 PO | Simple tooling, early-stage |
| Standard (4) | 1 Dev Lead + 1 Developer + 1 Security + 1 PO | Full development with security review |
| Scaled (5+) | 1 Dev Lead + 2+ Developers + 1 Security + 1 PO | Larger projects with parallel workstreams |

> This team is intentionally lean. Roey handles UI design and deployment, which in larger teams would be separate roles. If workload increases, consider adding a dedicated developer agent to handle parallel tasks.
