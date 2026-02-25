# Marketing Team Template

> Run a content marketing pipeline with AI agents.

A team template for **content marketing** where AI agents handle research, writing, review, and multi-platform publishing — while a human marketing lead sets strategy and approves final output.

## Team Structure

```
                    ┌─────────────────────────────────────┐
                    │      Human Marketing Lead            │
                    │   (strategy, brand voice, approval)   │
                    └──────────────┬──────────────────────┘
                                   │
                          content brief
                                   │
                    ┌──────────────▼──────────────────────┐
                    │      Content Coordinator Agent       │
                    │  (editorial calendar, assignments,    │
                    │   quality control, publishing)        │
                    └──────┬──────────────┬───────────────┘
                           │              │
                    assign │       assign │
                           │              │
                    ┌──────▼─────┐ ┌──────▼──────┐
                    │ Research   │ │ Writer      │
                    │ Agent      │ │ Agent       │
                    │            │ │             │
                    │ - Topic    │ │ - Drafting  │
                    │   research │ │ - Editing   │
                    │ - Data     │ │ - Platform  │
                    │   gathering│ │   adaptation│
                    │ - Trend    │ │ - SEO       │
                    │   analysis │ │   optimization│
                    └────────────┘ └─────────────┘
```

## Roles

| Role | Type | Count | Responsibilities |
|------|------|-------|------------------|
| **Marketing Lead** | Human | 1 | Content strategy, brand guidelines, topic approval, final review |
| **Content Coordinator** | Agent | 1 | Editorial calendar, task assignment, quality gates, publish scheduling |
| **Research Agent** | Agent | 1 | Topic research, competitive analysis, data/stats gathering, trend monitoring |
| **Writer Agent** | Agent | 1+ | Drafting, editing, platform adaptation (blog/Twitter/WeChat/etc.), SEO |

> **Scaling note:** The Writer Agent handles drafting, editing, platform adaptation, and SEO — a wide scope that works at small scale but can become a bottleneck. For larger teams or high-volume content pipelines, consider splitting into **Writer** (drafting + platform adaptation) and **Editor** (editing + SEO + quality assurance) roles.

## hxa-connect Integration

| Channel | Type | Purpose |
|---------|------|---------|
| `{brand}-content-pipeline` | Thread | Editorial calendar, content status, review cycles |
| DM: Coordinator → Research | Direct | Research briefs and data requests |
| DM: Coordinator → Writer | Direct | Writing assignments with research context |
| DM: Coordinator → Marketing Lead | Direct | Content ready for approval |

**Message patterns:**
- New content brief: Marketing Lead posts topic + angle → Coordinator assigns Research Agent
- Research done: Research Agent posts findings to thread → Coordinator assigns Writer
- Draft ready: Writer posts draft to thread as artifact → Coordinator reviews
- Review cycle: Coordinator sends revision notes → Writer iterates → re-submit
- Approved: Marketing Lead approves → Coordinator schedules publishing across platforms

## Workflow

1. **Marketing Lead defines content brief** — topic, angle, target audience, platforms, deadline
2. **Research Agent gathers material** — competitive landscape, data points, reference articles, trends
3. **Content Coordinator packages brief** — combines strategy + research into writing assignment
4. **Writer Agent drafts content** — long-form article, adapted versions for each platform
5. **Content Coordinator reviews** — checks quality, brand voice, factual accuracy
6. **Marketing Lead approves** — final sign-off on content
7. **Content Coordinator publishes** — scheduled rollout across platforms (blog → social → community)

**Quality gates:**
- Research must include sources and data before writing starts
- Draft must pass Coordinator review before Marketing Lead sees it
- Platform-specific versions must be adapted (not just copy-pasted)
- All content must include standard ending/CTA templates

## Templates Included

*Planned — TBD (see [Status](#status)):*
- Content brief template
- Research report template
- Editorial calendar template
- Platform adaptation checklist (blog, Twitter, WeChat, XHS, etc.)
- Content performance report template

## Infrastructure

| Tool | Purpose | Required |
|------|---------|----------|
| hxa-connect | Agent communication | Yes |
| Blog/CMS | Long-form publishing | Yes |
| Social media accounts | Distribution | Yes |
| SEO tool | Keyword research, optimization | Recommended |
| Analytics | Performance tracking | Recommended |
| Image generation | Visual content | Optional |

## Status

**Planned** — template structure defined, content in progress. Based on real experience running the COCO content pipeline (EP0 article, ClawFeed launch post, multi-platform publishing).

Want to contribute? See the [contributing guide](../../docs/contributing.md).
