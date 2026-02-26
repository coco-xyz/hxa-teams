# Workflow

The complete investment workflow from signal detection to decision execution. This process is designed for autonomous agent operation with human oversight at critical decision points.

---

## Overview

```
Signal Detection → Classification → Research/Modeling → Audit → Decision → Execution → Feedback Loop
```

Each step is detailed below.

---

## 1. Signal Detection

**Who:** Investment Coordinator (Roey)
**Where:** External data sources, `#intel`

Roey continuously monitors data sources for investment-relevant signals:
- On-chain data (whale movements, TVL changes, token flows)
- Market data (price feeds, volume spikes, funding rates)
- News feeds (regulatory announcements, partnerships, incidents)
- Social media (sentiment shifts, influencer activity)
- Security feeds (vulnerability disclosures, exploit reports)

**Example:**
> Signal detected: ETH funding rate inverted on 3 major exchanges. Volume spike 340% in 4 hours.

---

## 2. Signal Classification

**Who:** Investment Coordinator (Roey)
**Where:** `#intel`, `#general`

Roey classifies each signal using the red/yellow/green framework:

| Level | Color | Criteria | Action |
|-------|-------|----------|--------|
| Critical | Red | Black swan events, >10% price moves, security vulnerabilities | Immediate BMAN escalation |
| Important | Yellow | Earnings miss, whale movements, regulatory news | Daily report inclusion |
| Routine | Green | Market data updates, periodic information | Auto-archive |

**Red signal flow:**
```
Signal detected
      │
      ▼
Roey classifies as RED
      │
      ├──→ Broadcast to #general (all agents alerted)
      │
      └──→ Immediate DM to BMAN with:
           - Signal summary
           - Severity assessment
           - Recommended action
           - Time sensitivity
```

**Yellow/Green signal flow:**
```
Signal detected
      │
      ▼
Roey classifies as YELLOW or GREEN
      │
      ├──→ YELLOW: Added to daily report queue
      │
      └──→ GREEN: Auto-archived with timestamp
```

---

## 3. Daily Reporting

**Who:** Investment Coordinator (Roey)
**Where:** `#general`
**Schedule:** 10:00 and 22:00 (local time)

Roey produces two daily reports covering:
- Summary of all signals received since last report
- Yellow signal analysis and context
- Status of ongoing research tasks
- Model output highlights
- Pending decisions for BMAN
- Audit findings from Zylos_ABCDE (if any)

**Example daily report structure:**
```
=== Daily Report (10:00) ===

## Signals (last 12h)
- 🔴 Red: 0
- 🟡 Yellow: 3
- 🟢 Green: 14

## Yellow Signals
1. [YELLOW] Uniswap governance proposal #47 — potential fee switch
   - Impact: Medium (affects UNI token value thesis)
   - Action: Research task dispatched to Boy

2. [YELLOW] Whale wallet 0x3f...moved 12,000 ETH to exchange
   - Impact: Low-Medium (potential sell pressure)
   - Action: Monitoring

3. [YELLOW] SEC commissioner speech on DeFi regulation
   - Impact: Medium (regulatory uncertainty)
   - Action: Full text analysis in progress

## Ongoing Research
- Boy: Uniswap fee switch DD — ETA 6h
- Joey: Updated portfolio risk model — complete, results in #research

## Pending Decisions
- None requiring immediate BMAN input

## Audit
- Zylos_ABCDE 03:30 UTC audit: CLEAN — no findings
```

---

## 4. Research Dispatch

**Who:** Investment Coordinator (Roey) → Research Analyst (Boy)
**Where:** DM (Roey → Boy), `#research`

When a signal warrants deeper investigation, Roey dispatches a research task:

1. Roey sends a structured task to Boy via DM:
   - Subject: What to research
   - Scope: Specific questions to answer
   - Deadline: Expected turnaround
   - Priority: Urgency level
   - Context: Related signals, prior research to reference

2. Boy acknowledges and begins research

3. Boy posts completed DD report to `#research`

**Example dispatch:**
```
TO: Boy
TASK: Due diligence on Uniswap fee switch proposal
SCOPE:
  - Revenue impact modeling (current vs proposed fee structure)
  - Governance vote likelihood (quorum analysis, delegate positions)
  - Historical precedent (other protocol fee switches)
  - Risk assessment
DEADLINE: 12 hours
PRIORITY: Medium
CONTEXT: Yellow signal from governance tracker. UNI is 4% of portfolio.
```

---

## 5. Research Execution

**Who:** Research Analyst (Boy)
**Where:** `#research`

Boy conducts due diligence following a structured process:

1. **Data collection** — Gather on-chain data, documentation, governance forums, market data
2. **Analysis** — Evaluate team, technology, tokenomics, market position, risks
3. **Backtesting** — Where applicable, run historical backtests on similar events/strategies
4. **Report production** — Structured DD report with clear findings and risk assessment
5. **Delivery** — Post to `#research` and notify Roey

**Quality gate:** Every DD report must include:
- [ ] Clear thesis statement (bullish/bearish/neutral with reasoning)
- [ ] Quantified risk assessment (low/medium/high with supporting data)
- [ ] Historical context or backtesting results
- [ ] Identified unknowns and data gaps

---

## 6. Investment Modeling

**Who:** Investment Engineer (Joey)
**Where:** `#research`

Joey builds and maintains investment models in parallel with research:

1. **Model construction** — Build quantitative models (pricing, risk, portfolio optimization)
2. **Strategy backtesting** — Test investment strategies against historical data
3. **Platform development** — Build and maintain data pipelines, dashboards, analytics tools
4. **Model updates** — Incorporate new research data and market conditions
5. **Output delivery** — Post model results to `#research`

**Example model outputs:**
```
=== Portfolio Risk Model Update ===
Date: 2026-02-26
Model version: v2.3

Portfolio VaR (95%, 24h): -4.2%
Max drawdown (30d): -18.7%
Correlation matrix: Updated (BTC-ETH: 0.82, BTC-SOL: 0.71)
Concentration risk: HIGH — top 3 positions = 67% of portfolio

RECOMMENDATION: Reduce concentration. Consider rebalancing top 3 to <55%.
```

---

## 7. Security Audit

**Who:** Security Committee (Zylos_ABCDE)
**Where:** `#intel`, DM to BMAN + Roey

Zylos_ABCDE performs continuous and scheduled security audits:

### Scheduled Audit (Daily 03:30 UTC)

Comprehensive review covering:
- Investment logic audit: Are current positions consistent with stated thesis?
- Compliance check: Are risk limits being respected?
- Systemic risk scan: Cross-portfolio correlation, concentration, liquidity risks
- Platform security: Scan for vulnerabilities in tools, platforms, and integrations (including OpenClaw)
- Process audit: Are team workflows being followed?

### Continuous Monitoring

- Flag investment decisions that violate risk policies
- Alert on security vulnerabilities in used platforms
- Identify process deviations (missed reports, overdue research)

### Dual Reporting

All findings are reported to both BMAN and Roey:
```
Audit finding
      │
      ├──→ DM to BMAN (for strategic awareness)
      │
      └──→ DM to Roey / #intel (for operational action)
```

**Quality gate:** Every audit must produce:
- [ ] CLEAN or FINDING status
- [ ] If FINDING: severity, affected area, recommended action
- [ ] Audit trail entry (timestamped, immutable)

---

## 8. Decision

**Who:** Strategic Decision Maker (BMAN)
**Where:** DM with Roey, `#general`

BMAN makes final decisions based on aggregated inputs:
- Daily reports from Roey
- DD reports from Boy
- Model outputs from Joey
- Audit findings from Zylos_ABCDE

**Decision types:**
- **Go/No-go:** Execute or pass on an investment opportunity
- **Rebalance:** Adjust portfolio positions based on model recommendations
- **Escalation response:** Immediate action on red signal events
- **Direction change:** Shift investment thesis or strategy

---

## 9. Feedback Loop

**Who:** All
**Where:** `#general`, knowledge base

After each decision cycle:
1. Record decision and reasoning in the knowledge base
2. Track outcome over time
3. Update models with realized results
4. Conduct periodic retrospectives
5. Refine signal classification thresholds based on false positive/negative rates

```
Decision → Execution → Tracking → Retrospective → Knowledge Update → Decision
  ↑                                                                      |
  +----------------------------------------------------------------------+
```

---

## Communication Patterns

### Channels

| Channel | Purpose | Participants |
|---------|---------|-------------|
| **#intel** | Signal triage, intelligence summaries, audit findings | Roey, Zylos_ABCDE |
| **#research** | DD reports, backtest results, model outputs, investment modeling | Boy, Roey, Joey |
| **#general** | Red signal broadcasts, daily reports, cross-team announcements | All |
| **DM: Roey ↔ Boy** | Research task dispatch, scope clarification | Roey, Boy |
| **DM: Roey ↔ Joey** | Modeling task assignment, data pipeline requests | Roey, Joey |
| **DM: Zylos_ABCDE ↔ anyone** | Audit inquiries, compliance questions | Zylos_ABCDE, * |

### Notification Flow

```
Signal detected ──→ Roey classifies in #intel
                         │
                    ┌────┴────┐
                    ▼         ▼
               RED signal  YELLOW signal
                    │         │
                    ▼         ▼
            #general +    Daily report
            DM to BMAN    (10:00/22:00)
                              │
                              ▼
                      Research dispatch ──→ Boy
                              │
                              ▼
                    DD report in #research
                              │
                              ▼
                    Roey consolidates for BMAN
```

---

## Automation Layer

| Automation | Owner | Mechanism |
|-----------|-------|-----------|
| Signal monitoring | Roey | Continuous data source polling (market APIs, on-chain, news) |
| Signal classification | Roey | Automated triage with severity framework |
| Daily reports | Roey | Scheduler triggers at 10:00 and 22:00 |
| Security audit | Zylos_ABCDE | Scheduler triggers at 03:30 UTC |
| Research tracking | Roey | Monitor dispatched task status, follow up on overdue items |
| Model updates | Joey | Triggered by new data availability or schedule |
| Knowledge base update | All | Post-decision recording to Qdrant/SQLite |

---

## Complete Flow Diagram

```
Data sources (on-chain, market, news, social)
         │
         ▼
Roey monitors + classifies signal
         │
    ┌────┴──────────────────┐
    ▼                       ▼
RED signal              YELLOW/GREEN signal
    │                       │
    ▼                       ▼
Broadcast #general      Daily report queue
+ DM to BMAN                │
    │                       ▼
    ▼               Roey dispatches research
BMAN decides              │
immediate action    ┌─────┴──────┐
                    ▼            ▼
                  Boy          Joey
               (DD report)  (model update)
                    │            │
                    ▼            ▼
              Post to #research
                    │
                    ▼
         Zylos_ABCDE audits
         (logic, compliance, risk)
                    │
                    ▼
         Dual report: BMAN + Roey
                    │
                    ▼
         Roey consolidates daily report
                    │
                    ▼
         BMAN reviews + decides
                    │
                    ▼
         Decision recorded → feedback loop
```
