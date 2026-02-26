# Investment Team Template

> Autonomous investment signal monitoring, research, modeling, and decision support for crypto/blockchain investments.

A team template for building **investment operations teams** where AI agents continuously monitor markets, research opportunities, model strategies, and support human decision-making — 24/7, across global time zones.

## Team Structure

```
                    ┌─────────────────────────────────────┐
                    │    Strategic Decision Maker (Human)   │
                    │   (final trades, investment direction) │
                    └──────────────┬──────────────────────┘
                                   │
                          red signal escalation
                          + daily reports
                                   │
                    ┌──────────────▼──────────────────────┐
                    │     Investment Coordinator (Roey)     │
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
                    │  (Boy)      │  │  (Joey)      │
                    └──────┬─────┘  └──────┬──────┘
                           │               │
                           └───────┬───────┘
                                   │
                        reports + models
                                   │
                    ┌──────────────▼──────────────────────┐
                    │     Security Committee (Zylos_ABCDE)  │
                    │  (investment logic audit, compliance,  │
                    │   systemic risk, daily 03:30 UTC scan) │
                    └──────────────┬──────────────────────┘
                                   │
                          dual report
                                   │
                    ┌──────────────▼──────────────────────┐
                    │    Strategic Decision Maker (BMAN)    │
                    │    + Investment Coordinator (Roey)     │
                    └─────────────────────────────────────┘
```

## Roles

| Role | Agent | Type | Responsibilities |
|------|-------|------|------------------|
| **Investment Coordinator** | Roey | agent | Signal triage (red/yellow/green), daily reports at 10:00 & 22:00, task dispatch |
| **Research Analyst** | Boy | agent | Due diligence, project backtesting, research reports |
| **Investment Engineer** | Joey | agent | Investment model building, strategy modeling, investment platform development |
| **Security Committee** | Zylos_ABCDE | agent | Investment logic audit, compliance, systemic risk scanning, daily audit at 03:30 UTC |
| **Strategic Decision Maker** | BMAN | human | Final trade decisions, investment direction, red signal escalation |

See [docs/roles.md](docs/roles.md) for detailed role definitions.

## hxa-connect Integration

Communication channels this template creates:

| Channel | Type | Members | Purpose |
|---------|------|---------|---------|
| `#intel` | Channel | Roey, Zylos_ABCDE | Signal triage output, intelligence summaries |
| `#research` | Channel | Boy, Roey, Joey | DD reports, backtest results, investment modeling |
| `#general` | Channel | All | Red signal broadcasts, cross-team announcements |
| DM: Roey → Boy | Direct | Roey, Boy | Research task dispatch |
| DM: Roey → Joey | Direct | Roey, Joey | Modeling task assignment |
| DM: Zylos_ABCDE → anyone | Direct | Zylos_ABCDE, * | Audit inquiries |

**Message patterns:**
- Signal detected: Roey classifies and posts to `#intel` with severity tag
- Red signal: Roey broadcasts to `#general`, escalates to BMAN immediately
- Research dispatch: Roey assigns task to Boy via DM, Boy posts report to `#research`
- Model update: Joey posts model results to `#research`
- Audit finding: Zylos_ABCDE posts to `#intel`, dual-reports to BMAN + Roey
- Daily report: Roey posts to `#general` at 10:00 and 22:00

## Signal Classification

| Level | Color | Example Triggers | Action |
|-------|-------|-----------------|--------|
| Critical | Red | Black swan events, >10% price moves, security vulnerabilities | Immediate BMAN escalation |
| Important | Yellow | Earnings miss, whale movements, regulatory news | Daily report inclusion |
| Routine | Green | Market data updates, periodic information | Auto-archive |

## Workflow

1. Roey continuously monitors data sources, receives and classifies signals
2. Red signals escalate immediately to BMAN; Yellow signals go into daily reports
3. Roey dispatches research tasks to Boy; Boy produces DD/backtest reports
4. Joey builds and maintains investment models, strategy backtesting systems, and investment platform
5. Zylos_ABCDE audits investment logic, compliance, and systemic risks — dual report to BMAN + Roey
6. BMAN makes final decisions based on aggregated reports

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
