# Roles

The investment team has **5 roles**: one coordinator agent, one research analyst agent, one investment engineer agent, one security committee agent, and one human strategic decision maker. Each role has distinct responsibilities and required capabilities.

> In the reference implementation, the team uses named agents (Roey, Boy, Joey, Zylos_ABCDE) + a human decision maker (BMAN) on `boot.hxa.net`. Throughout this document, we use both generic labels and reference names so you can map them to your own setup.

---

## Investment Coordinator (Roey)

**Primary function:** Signal monitoring, classification, daily reporting, and cross-agent task dispatch.

### Responsibilities

- Monitor external data sources (on-chain data, news feeds, social media, market APIs) for investment-relevant signals
- Classify incoming signals using the red/yellow/green framework
- Escalate red signals immediately to the Strategic Decision Maker (BMAN)
- Produce daily reports at 10:00 and 22:00 with consolidated yellow/green signal summaries
- Dispatch research tasks to the Research Analyst (Boy) with clear scope and deadlines
- Dispatch modeling tasks to the Investment Engineer (Joey)
- Monitor downstream execution of dispatched tasks
- Maintain the signal database and classification history
- Act as the communication hub between all team members

### Required Capabilities

| Capability | Why |
|------------|-----|
| Persistent memory | Maintains signal history, classification decisions, and team state across sessions |
| Scheduler / cron | Self-wakes for daily reports (10:00, 22:00), periodic signal sweeps, and follow-ups |
| External communication | Messages team channels, posts to hxa-connect, interacts with data APIs |
| Data processing | Parses market data feeds, on-chain data, news APIs |
| Always-on availability | 24/7 uptime for real-time signal monitoring — markets never sleep |

### Communication Patterns

| Target | Channel | Purpose |
|--------|---------|---------|
| BMAN | DM / `#general` | Red signal escalation, daily reports |
| Boy | DM / `#research` | Research task dispatch, scope clarification |
| Joey | DM / `#research` | Modeling task assignment, data pipeline requests |
| Zylos_ABCDE | `#intel` | Intelligence sharing, signal context |
| All | `#general` | Daily reports, red signal broadcasts |

### Escalation Path

- If unable to classify a signal → escalate to BMAN with raw data and context
- If a dispatched task is overdue → re-ping the assigned agent, then escalate to BMAN if unresolved

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a cloud server (for always-on availability and scheduler)
- Any persistent agent platform with memory + scheduling + communication + data API access

---

## Research Analyst (Boy)

**Primary function:** Deep-dive investment research, due diligence, and project backtesting.

### Responsibilities

- Accept research tasks from the Investment Coordinator (Roey)
- Conduct due diligence on crypto projects, protocols, and tokens
- Produce structured DD reports covering: team, technology, tokenomics, market position, risks
- Run historical backtests on projects and strategies
- Maintain a research database with standardized report formats
- Track previously researched projects for status changes
- Contribute research findings to `#research` channel

### Required Capabilities

| Capability | Why |
|------------|-----|
| Web research | Access to crypto research platforms, block explorers, documentation |
| Data analysis | Process historical price data, on-chain metrics, token distributions |
| Persistent memory | Maintains research history, previous DD reports, and project tracking |
| Structured output | Produces standardized reports that other agents can parse |
| Communication | Receives tasks via DM, posts reports to `#research` |

### Communication Patterns

| Target | Channel | Purpose |
|--------|---------|---------|
| Roey | DM / `#research` | Receive research tasks, deliver reports, clarify scope |
| Joey | `#research` | Share data for modeling, collaborate on strategy analysis |
| Zylos_ABCDE | DM (on request) | Respond to audit inquiries about research methodology |

### Escalation Path

- If research scope is unclear → ask Roey for clarification via DM
- If unable to complete DD (data unavailable, project unverifiable) → report blockers to Roey with partial findings

### Notes

- Boy is a dedicated research role — does not cross into the dev team
- This focus ensures research depth is not compromised by multitasking
- More research analysts can be added for parallel coverage of multiple sectors

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a cloud or local machine
- Any agent with web access, data processing, and communication capabilities

---

## Investment Engineer (Joey)

**Primary function:** Investment model construction, strategy modeling, and investment platform development.

### Responsibilities

- Build and maintain quantitative investment models (pricing models, risk models, portfolio optimization)
- Develop strategy backtesting systems for historical performance analysis
- Build and maintain the investment platform (data pipelines, dashboards, analytics tooling)
- Integrate data sources (on-chain data, market feeds, research outputs) into unified models
- Produce model outputs and strategy performance reports for the team
- Collaborate with Boy on translating research findings into quantifiable models
- Maintain model documentation and version history

### Required Capabilities

| Capability | Why |
|------------|-----|
| Code execution | Full development environment for model building and data pipelines |
| Data engineering | Build ETL pipelines for market data, on-chain data, and research inputs |
| Quantitative modeling | Statistical analysis, backtesting frameworks, portfolio math |
| Persistent memory | Maintains model versions, pipeline configurations, and project context |
| Communication | Receives tasks from Roey, collaborates with Boy, posts results to `#research` |

### Communication Patterns

| Target | Channel | Purpose |
|--------|---------|---------|
| Roey | DM / `#research` | Receive modeling tasks, report progress, deliver outputs |
| Boy | `#research` | Receive research data, collaborate on strategy parameters |
| Zylos_ABCDE | DM (on request) | Respond to audit inquiries about model logic |

### Escalation Path

- If model requirements are ambiguous → clarify with Roey
- If data sources are unavailable or unreliable → report to Roey, suggest alternatives
- If model produces unexpected results → flag to Roey and Zylos_ABCDE for audit

### Notes

- Joey also serves on the dev team (dev-team-bman) as a full-stack developer
- Investment modeling tasks and dev tasks are dispatched through different channels (`#research` vs `#dev`)
- Priority conflicts between teams should be resolved by Roey (who serves as coordinator in both teams)

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a cloud or local machine
- Any agent with code execution, data pipeline capabilities, and communication

---

## Security Committee (Zylos_ABCDE)

**Primary function:** Investment logic audit, compliance review, systemic risk scanning, and security assessment.

### Responsibilities

- Audit all investment decisions for logical soundness before execution
- Review compliance with investment policies and risk limits
- Perform systemic risk scanning across the portfolio and external environment
- Scan for security vulnerabilities in tools and platforms used by the team (including OpenClaw vulnerabilities)
- Run a daily comprehensive audit at 03:30 UTC
- Dual-report all findings to both BMAN (Strategic Decision Maker) and Roey (Investment Coordinator)
- Maintain an audit trail of all reviewed decisions and findings
- Flag conflicts of interest or process violations

### Required Capabilities

| Capability | Why |
|------------|-----|
| Analytical reasoning | Deep analysis of investment logic, risk assessment, compliance checking |
| Security scanning | Identify vulnerabilities in platforms, smart contracts, and operational processes |
| Scheduler / cron | Self-wakes for daily 03:30 UTC audit cycle |
| Persistent memory | Maintains audit history, compliance records, and risk register |
| Communication | Posts findings to `#intel`, dual-reports to BMAN + Roey |
| Independence | Operates independently from the investment workflow to maintain audit objectivity |

### Communication Patterns

| Target | Channel | Purpose |
|--------|---------|---------|
| Roey | `#intel` / DM | Share audit findings, request clarification on investment decisions |
| BMAN | DM / `#general` | Escalate critical audit findings, compliance violations |
| Boy | DM | Audit inquiries about research methodology or data sources |
| Joey | DM | Audit inquiries about model logic or platform security |
| All | `#general` | Critical risk alerts that affect the entire team |

### Escalation Path

- Routine findings → include in daily 03:30 UTC audit report
- Critical findings (compliance violations, security vulnerabilities, logical flaws) → immediate dual-report to BMAN + Roey
- Unresolvable disagreements → escalate to BMAN for final ruling

### Why a Separate Security Role?

- **Independence:** The auditor did not make the investment decisions, providing objective review
- **Systemic view:** Scans for risks that individual agents may miss due to narrow focus
- **Dual reporting:** Reports to both the coordinator (Roey) and the human decision maker (BMAN), preventing any single point of filtering
- **Continuous vigilance:** Daily 03:30 UTC audit ensures nothing slips through during off-hours

### Notes

- Zylos_ABCDE also serves on the dev team (dev-team-bman) as a security auditor for code
- The investment logic audit and code security review are complementary but distinct functions
- Systemic risk scanning covers cross-cutting concerns: platform vulnerabilities, smart contract risks, operational security

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a cloud server (for always-on availability and scheduling)
- Any persistent agent platform with analytical capabilities, scheduling, and communication

---

## Strategic Decision Maker (BMAN — Human)

**Primary function:** Final trade decisions, investment direction, and red signal escalation handling.

### Responsibilities

- Make final go/no-go decisions on investment opportunities
- Set overall investment direction, thesis, and risk appetite
- Handle red signal escalations (black swan events, major price moves, security vulnerabilities)
- Review and approve changes to investment models and strategies
- Set portfolio-level risk limits and compliance policies
- Resolve priority conflicts between team members

### Interaction Model

The Strategic Decision Maker interacts with the team through:

1. **hxa-connect channels** — Receive daily reports, red signal escalations, audit findings
2. **Direct messages** — Strategic discussions with Roey, audit escalations from Zylos_ABCDE
3. **External platforms** (Telegram, etc.) — Quick decisions, urgent responses
4. **Scheduled reviews** — Optional; most coordination is async

### How Much Human Involvement?

The goal is to minimize human involvement to high-value decisions:

| Activity | Human Involvement |
|----------|------------------|
| Signal monitoring | None (Roey handles) |
| Signal classification | None (Roey handles; red signals escalated) |
| Research dispatch | None (Roey dispatches to Boy) |
| Due diligence | None (Boy handles) |
| Investment modeling | None (Joey handles) |
| Security audit | None (Zylos_ABCDE handles) |
| Red signal response | High (BMAN decides) |
| Final trade decisions | High (BMAN decides) |
| Investment direction | High (BMAN sets) |
| Daily report review | Medium (BMAN reviews at own pace) |

---

## Role Interaction Map

```
                  Red signals + daily reports
    BMAN ◄──────────────────────────────────── Roey (Coordinator)
    ▲  ▲                                        │  ▲
    │  │                                        │  │
    │  │  audit findings          research      │  │  reports
    │  │                          dispatch      │  │
    │  │                                │       │  │
    │  │                         ┌──────▼────┐  │  │
    │  │                         │    Boy     │──┘  │
    │  │                         │ (Research) │     │
    │  │                         └───────────┘     │
    │  │                                           │
    │  │                         ┌───────────┐     │
    │  │                         │   Joey     │─────┘
    │  │                         │ (Engineer) │
    │  │                         └───────────┘
    │  │                                ▲
    │  │         audit inquiries        │
    │  └──────── Zylos_ABCDE ───────────┘
    │            (Security)
    └──────────── dual report
```

## Scaling the Team

| Team Size | Configuration | Best For |
|-----------|--------------|----------|
| Minimum (3) | 1 Coordinator + 1 Researcher + 1 Human | Simple signal monitoring + research |
| Standard (5) | 1 Coordinator + 1 Researcher + 1 Engineer + 1 Security + 1 Human | Full investment operations |
| Scaled (6+) | + Additional researchers for sector coverage | Multi-sector, high-volume monitoring |

> Adding more research analysts enables parallel sector coverage (DeFi, L1/L2, gaming, etc.) but increases coordination overhead for Roey. Scale based on portfolio complexity.
