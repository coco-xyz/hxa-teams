# hxa-events — Agent-Native Event Mesh

## Overview

hxa-events is the perception nervous system for HxA agents. It normalizes events from external platforms (GitHub, GitLab, Notion) into a unified schema and routes them to agent sinks (HxA-Connect, Lark, Slack) — with routing logic that is dynamic and agent-driven, not hardcoded.

**Key distinction:** The consumer is an agent, not a human. Events are not notifications — they are signals that trigger agent reasoning and action.

```
sources/                  event schema              router              sinks/
  github/    ──────────►                                            hxa-connect/
  gitlab/    ──────────►  HxAEvent (unified)  ──►  agent router  ►  lark/
  notion/    ──────────►                                            slack/
```

## Documents

| File | Description |
|------|-------------|
| [event-schema.md](./event-schema.md) | Unified event format — all sources produce this |
| [adapter-interface.md](./adapter-interface.md) | Source adapter + sink adapter interface contracts |

## Architecture Principles

1. **Agent-first routing** — routing decisions are made by an agent at runtime, not by static config
2. **Schema normalization** — every source emits the same `HxAEvent` shape; adapters handle translation
3. **Loose coupling** — sources and sinks know nothing about each other; the router owns the wiring
4. **Extensibility** — adding a new source or sink only requires implementing the relevant interface

## MVP Scope (~1 week)

**Sources:** GitHub, GitLab
**Sinks:** HxA-Connect
**Router:** rule-based (agent-readable config), upgradeable to LLM-driven later

## Project Status

- [ ] Event schema v1
- [ ] Source adapter interface
- [ ] Sink adapter interface
- [ ] GitHub adapter (source)
- [ ] GitLab adapter (source)
- [ ] HxA-Connect adapter (sink)
- [ ] Router (rule-based MVP)
