# Sales Team Template

> Build an AI-powered outbound sales pipeline.

A team template for **B2B outbound sales** where AI agents handle research, outreach, follow-up, and pipeline management — while a human sales lead focuses on strategy and closing.

## Team Structure

```
                    ┌─────────────────────────────────────┐
                    │        Human Sales Lead              │
                    │   (strategy, qualification, closing)  │
                    └──────────────┬──────────────────────┘
                                   │
                           strategy + targets
                                   │
                    ┌──────────────▼──────────────────────┐
                    │       Pipeline Manager Agent         │
                    │  (coordination, CRM updates,         │
                    │   pipeline health, reporting)         │
                    └──────┬──────────────────┬───────────┘
                           │                  │
                    assign │           assign │
                           │                  │
                    ┌──────▼─────┐     ┌──────▼──────┐
                    │ Research   │     │ Outreach    │
                    │ Agent      │     │ Agent       │
                    │            │     │             │
                    │ - ICP match│     │ - Email     │
                    │ - Company  │     │ - Follow-up │
                    │   intel    │     │ - Sequences │
                    │ - Contact  │     │ - Response  │
                    │   finding  │     │   handling  │
                    └────────────┘     └─────────────┘
```

## Roles

| Role | Type | Count | Responsibilities |
|------|------|-------|------------------|
| **Sales Lead** | Human | 1 | Target market definition, ICP refinement, deal qualification, closing calls |
| **Pipeline Manager** | Agent | 1 | CRM maintenance, pipeline reporting, task coordination, sequence scheduling |
| **Research Agent** | Agent | 1 | Company research, contact finding, ICP scoring, competitive intel |
| **Outreach Agent** | Agent | 1 | Email drafting, follow-up sequences, response categorization, meeting scheduling |

## hxa-connect Integration

| Channel | Type | Purpose |
|---------|------|---------|
| `{company}-sales-pipeline` | Thread | Daily pipeline status, new leads, blockers |
| DM: Pipeline Manager → Research | Direct | Research requests for new targets |
| DM: Pipeline Manager → Outreach | Direct | Outreach assignments with research context |
| DM: Outreach → Sales Lead | Direct | Qualified leads ready for human touch |

**Message patterns:**
- New target batch: Sales Lead posts criteria → Pipeline Manager distributes to Research Agent
- Research complete: Research Agent posts company brief to thread → Pipeline Manager assigns to Outreach
- Response received: Outreach Agent categorizes (interested/not now/no) → routes to Pipeline Manager
- Qualified lead: Pipeline Manager escalates to Sales Lead via DM with full context

## Workflow

1. **Sales Lead defines ICP** — target industries, company size, titles, pain points
2. **Research Agent builds prospect list** — company intel, contact info, ICP scoring
3. **Pipeline Manager reviews and assigns** — qualified prospects go to Outreach Agent
4. **Outreach Agent runs sequences** — personalized first touch + automated follow-ups
5. **Response handling** — Outreach categorizes replies, escalates warm leads
6. **Sales Lead qualifies and closes** — takes over for human-touch conversations

**Quality gates:**
- Research must include ICP score before passing to Outreach
- Outreach must have Sales Lead-approved email templates before sending
- Warm leads must include full conversation context before escalation

## Templates Included

*Coming soon:*
- ICP definition template
- Company research brief template
- Email sequence templates (first touch, follow-up 1-3, breakup)
- Pipeline report template
- Weekly sales review template

## Infrastructure

| Tool | Purpose | Required |
|------|---------|----------|
| hxa-connect | Agent communication | Yes |
| CRM (HubSpot/Pipedrive/etc.) | Pipeline tracking | Recommended |
| Email platform | Outreach sending | Yes |
| LinkedIn/Apollo/similar | Contact finding | Recommended |
| Calendar tool | Meeting scheduling | Optional |

## Status

**Planned** — template structure defined, content in progress. The dev-team template serves as the reference implementation; sales-team follows the same schema with sales-specific content.

Want to contribute? See the [contributing guide](../../docs/contributing.md).
