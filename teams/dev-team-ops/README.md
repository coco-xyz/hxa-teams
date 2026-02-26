# Dev Team (Ops) Template

**Version:** 1.0.0

> Build tools, platforms, and infrastructure for investment operations with a lean agent dev team.

A team template for building **technical development teams** focused on investment tooling — data pipelines, monitoring dashboards, trading tools, and platform infrastructure. Designed for small, focused teams where a single dev lead handles requirement gathering, UI design, and deployment alongside a full-stack developer.

## Team Structure

```
                    ┌─────────────────────────────────────┐
                    │       Product Owner (Human)           │
                    │   (product direction, requirements,   │
                    │    acceptance)                         │
                    └──────────────┬──────────────────────┘
                                   │
                          requirements +
                          acceptance
                                   │
                    ┌──────────────▼──────────────────────┐
                    │       Dev & UI Lead                   │
                    │  (requirement gathering, UI design,   │
                    │   task assignment, deployment & ops)   │
                    └──────┬──────────────────┬───────────┘
                           │                  │
                  dev tasks│         security │ review
                           │                  │
                    ┌──────▼─────┐     ┌──────▼──────┐
                    │ Developer  │     │  Security    │
                    │            │     │  Auditor     │
                    └──────┬─────┘     └──────┬──────┘
                           │                  │
                           └────────┬─────────┘
                                    │
                              code + review
                                    │
                    ┌───────────────▼─────────────────────┐
                    │       Dev & UI Lead                   │
                    │      (deployment + service ops)       │
                    └──────────────┬──────────────────────┘
                                   │
                              deployed
                                   │
                    ┌──────────────▼──────────────────────┐
                    │       Product Owner (Human)           │
                    │      (acceptance + validation)        │
                    └─────────────────────────────────────┘
```

## Roles

| Role | Type | Responsibilities |
|------|------|------------------|
| **Dev & UI Lead** | agent | Requirement gathering, UI design (Figma output), task assignment, code deployment & service ops |
| **Developer** | agent | Full-stack development, data pipelines, tool building |
| **Security Auditor** | agent | Code security review, systemic risk scanning, code quality checks |
| **Product Owner** | human | Product direction, requirement confirmation, acceptance |

See [docs/roles.md](docs/roles.md) for detailed role definitions.

## hxa-connect Integration

Communication channels this template creates:

| Channel | Type | Members | Purpose |
|---------|------|---------|---------|
| `#dev` | Channel | Developer, Dev Lead | Development collaboration, code review, deployment notifications |
| `#general` | Channel | All | Cross-team announcements, product updates |
| DM: Dev Lead → Developer | Direct | Dev Lead, Developer | Task assignment, technical questions |
| DM: Security Auditor → Developer | Direct | Security Auditor, Developer | Security findings, code quality feedback |

**Message patterns:**
- Task assignment: Dev Lead posts to `#dev` with task details and design artifacts
- Code ready: Developer notifies Dev Lead via `#dev` when code is ready for deployment
- Security finding: Security Auditor DMs Developer with findings, posts summary to `#dev`
- Deployment: Dev Lead posts deployment notification to `#dev` and `#general`
- Acceptance: Product Owner reviews via `#general` or external platform

## Workflow

1. Dev Lead gathers requirements from all sources (investment team, product, operations), converts to dev tasks, produces UI designs
2. Developer executes development (data pipelines, monitoring dashboards, trading tools, etc.)
3. Security Auditor reviews code security and systemic risks
4. Dev Lead handles code deployment and service launch
5. Product Owner accepts/validates product

See [docs/workflow.md](docs/workflow.md) for the full development flow.

## Metrics

| Metric | Description | Target |
|--------|-------------|--------|
| Task turnaround | Time from requirement to deployed feature | <3 days for standard tasks |
| Security review coverage | Percentage of code changes reviewed by Security Auditor | 100% |
| Deployment success rate | Deployments without rollback | >95% |
| Acceptance rate | Features accepted by Product Owner on first submission | >80% |
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
