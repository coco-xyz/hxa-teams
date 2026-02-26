# Investment Team Template

**Version:** 1.0.0

> Autonomous investment signal monitoring, research, modeling, and decision support for crypto/blockchain investments.

A team template for building **investment operations teams** where AI agents continuously monitor markets, research opportunities, model strategies, and support human decision-making — 24/7, across global time zones.

## Team Structure

```
                    ┌─────────────────────────────────────┐
                    │   Strategic Decision Maker (Human)    │
                    │  (final trades, investment direction)  │
                    └──────────────┬──────────────────────┘
                                   │
                          red signal escalation
                          + daily reports
                                   │
                    ┌──────────────▼──────────────────────┐
                    │      Investment Coordinator           │
                    │  (signal triage, daily reports,       │
                    │   task dispatch, downstream monitor)  │
                    └──────┬───────────────┬──────────────┘
                           │               │
                   research│        modeling│
                   dispatch│          tasks │
                           │               │
                    ┌──────▼─────┐  ┌──────▼──────┐
                    │  Research   │  │  Investment  │
                    │  Analyst    │  │  Engineer    │
                    └──────┬─────┘  └──────┬──────┘
                           │               │
                           └───────┬───────┘
                                   │
                        reports + models
                                   │
                    ┌──────────────▼──────────────────────┐
                    │         Security Auditor              │
                    │  (investment logic audit, compliance,  │
                    │   systemic risk, daily 03:30 UTC scan) │
                    └──────────────┬──────────────────────┘
                                   │
                          dual report
                                   │
                    ┌──────────────▼──────────────────────┐
                    │   Strategic Decision Maker (Human)    │
                    │   + Investment Coordinator             │
                    └─────────────────────────────────────┘
```

## Roles

| Role | Type | Responsibilities |
|------|------|------------------|
| **Investment Coordinator** | agent | Signal triage (red/yellow/green), daily reports at 10:00 & 22:00, task dispatch |
| **Research Analyst** | agent | Due diligence, project backtesting, research reports |
| **Investment Engineer** | agent | Investment model building, strategy modeling, investment platform development |
| **Security Auditor** | agent | Investment logic audit, compliance, systemic risk scanning, daily audit at 03:30 UTC |
| **Strategic Decision Maker** | human | Final trade decisions, investment direction, red signal escalation |

See [docs/roles.md](docs/roles.md) for detailed role definitions.

## hxa-connect Integration

Communication channels this template creates:

| Channel | Type | Members | Purpose |
|---------|------|---------|---------|
| `#intel` | Channel | Coordinator, Security Auditor | Signal triage output, intelligence summaries |
| `#research` | Channel | Research Analyst, Coordinator, Investment Engineer | DD reports, backtest results, investment modeling |
| `#general` | Channel | All | Red signal broadcasts, cross-team announcements |
| DM: Coordinator → Research Analyst | Direct | Coordinator, Research Analyst | Research task dispatch |
| DM: Coordinator → Investment Engineer | Direct | Coordinator, Investment Engineer | Modeling task assignment |
| DM: Security Auditor → anyone | Direct | Security Auditor, * | Audit inquiries |

**Message patterns:**
- Signal detected: Coordinator classifies and posts to `#intel` with severity tag
- Red signal: Coordinator broadcasts to `#general`, escalates to Decision Maker immediately
- Research dispatch: Coordinator assigns task to Research Analyst via DM, Research Analyst posts report to `#research`
- Model update: Investment Engineer posts model results to `#research`
- Audit finding: Security Auditor posts to `#intel`, dual-reports to Decision Maker + Coordinator
- Daily report: Coordinator posts to `#general` at 10:00 and 22:00

## Signal Classification

| Level | Color | Example Triggers | Action |
|-------|-------|-----------------|--------|
| Critical | Red | Black swan events, >10% price moves, security vulnerabilities | Immediate escalation to Decision Maker |
| Important | Yellow | Earnings miss, whale movements, regulatory news | Daily report inclusion |
| Routine | Green | Market data updates, periodic information | Auto-archive |

## Workflow

1. Coordinator continuously monitors data sources, receives and classifies signals
2. Red signals escalate immediately to Decision Maker; Yellow signals go into daily reports
3. Coordinator dispatches research tasks to Research Analyst; Research Analyst produces DD/backtest reports
4. Investment Engineer builds and maintains investment models, strategy backtesting systems, and investment platform
5. Security Auditor audits investment logic, compliance, and systemic risks — dual report to Decision Maker + Coordinator
6. Decision Maker makes final decisions based on aggregated reports

See [docs/workflow.md](docs/workflow.md) for the full investment flow.

## Metrics

| Metric | Description | Target |
|--------|-------------|--------|
| Signal-to-report latency | Time from signal detection to classified report | Red: <5 min, Yellow: <1 hr |
| DD turnaround | Time from research dispatch to completed report | <24 hrs |
| Audit coverage | Percentage of investment decisions audited | 100% |
| Daily report accuracy | Reports delivered on schedule (10:00 & 22:00) | >99% |
| False positive rate | Red signals that turn out to be non-events | <10% |

## Templates Included

Planned — TBD. Future templates may include:
- Signal report template
- Due diligence report template
- Investment model specification template
- Audit report template

## Documentation

| Document | Description |
|----------|-------------|
| [Roles](docs/roles.md) | Detailed role definitions |
| [Workflow](docs/workflow.md) | Complete investment flow with diagrams |
| [Infrastructure](docs/infrastructure.md) | Tools, platforms, data sources, and communication setup |
