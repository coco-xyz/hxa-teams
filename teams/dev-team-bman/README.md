# Dev Team (BMAN) Template

> Build tools, platforms, and infrastructure for investment operations with a lean agent dev team.

A team template for building **technical development teams** focused on investment tooling — data pipelines, monitoring dashboards, trading tools, and platform infrastructure. Designed for small, focused teams where a single dev lead handles requirement gathering, UI design, and deployment alongside a full-stack developer.

## Team Structure

```
                    ┌─────────────────────────────────────┐
                    │       Product Owner (BMAN, Human)     │
                    │   (product direction, requirements,   │
                    │    acceptance)                         │
                    └──────────────┬──────────────────────┘
                                   │
                          requirements +
                          acceptance
                                   │
                    ┌──────────────▼──────────────────────┐
                    │       Dev & UI Lead (Roey)            │
                    │  (requirement gathering, UI design,   │
                    │   task assignment, deployment & ops)   │
                    └──────┬──────────────────┬───────────┘
                           │                  │
                  dev tasks│         security │ review
                           │                  │
                    ┌──────▼─────┐     ┌──────▼──────┐
                    │ Developer  │     │  Security    │
                    │ (Joey)     │     │  Auditor     │
                    │            │     │ (Zylos_ABCDE)│
                    └──────┬─────┘     └──────┬──────┘
                           │                  │
                           └────────┬─────────┘
                                    │
                              code + review
                                    │
                    ┌───────────────▼─────────────────────┐
                    │       Dev & UI Lead (Roey)            │
                    │      (deployment + service ops)       │
                    └──────────────┬──────────────────────┘
                                   │
                              deployed
                                   │
                    ┌──────────────▼──────────────────────┐
                    │       Product Owner (BMAN)            │
                    │      (acceptance + validation)        │
                    └─────────────────────────────────────┘
```

## Roles

| Role | Agent | Type | Responsibilities |
|------|-------|------|------------------|
| **Dev & UI Lead** | Roey | agent | Requirement gathering, UI design (Figma output), task assignment, code deployment & service ops |
| **Developer** | Joey | agent | Full-stack development, data pipelines, tool building |
| **Security Auditor** | Zylos_ABCDE | agent | Code security review, systemic risk scanning, code quality checks |
| **Product Owner** | BMAN | human | Product direction, requirement confirmation, acceptance |

See [docs/roles.md](docs/roles.md) for detailed role definitions.

## hxa-connect Integration

Communication channels this template creates:

| Channel | Type | Members | Purpose |
|---------|------|---------|---------|
| `#dev` | Channel | Joey, Roey | Development collaboration, code review, deployment notifications |
| `#general` | Channel | All | Cross-team announcements, product updates |
| DM: Roey → Joey | Direct | Roey, Joey | Task assignment, technical questions |
| DM: Zylos_ABCDE → Joey | Direct | Zylos_ABCDE, Joey | Security findings, code quality feedback |

**Message patterns:**
- Task assignment: Roey posts to `#dev` with task details and design artifacts
- Code ready: Joey notifies Roey via `#dev` when code is ready for deployment
- Security finding: Zylos_ABCDE DMs Joey with findings, posts summary to `#dev`
- Deployment: Roey posts deployment notification to `#dev` and `#general`
- Acceptance: BMAN reviews via `#general` or external platform

## Workflow

1. Roey gathers requirements from all sources (investment team, product, operations), converts to dev tasks, produces UI designs
2. Joey executes development (data pipelines, monitoring dashboards, trading tools, etc.)
3. Zylos_ABCDE reviews code security and systemic risks
4. Roey handles code deployment and service launch
5. BMAN accepts/validates product

See [docs/workflow.md](docs/workflow.md) for the full development flow.

## Metrics

| Metric | Description | Target |
|--------|-------------|--------|
| Task turnaround | Time from requirement to deployed feature | <3 days for standard tasks |
| Security review coverage | Percentage of code changes reviewed by Zylos_ABCDE | 100% |
| Deployment success rate | Deployments without rollback | >95% |
| Acceptance rate | Features accepted by BMAN on first submission | >80% |
| Uptime | Service availability for deployed tools | >99% |

## Templates Included

Planned — TBD. Future templates may include:
- Task specification template
- UI design handoff template
- Deployment checklist template
- Security review report template

## Documentation

| Document | Description |
|----------|-------------|
| [Roles](docs/roles.md) | Detailed role definitions |
| [Workflow](docs/workflow.md) | Complete development flow with diagrams |
| [Infrastructure](docs/infrastructure.md) | Tools, platforms, and communication setup |
