# [Feature Name] — Technical Design Document

<!--
  TEMPLATE: Replace [Feature Name] with your feature name.

  When to use:
  - Complex features that need architectural decisions documented
  - Changes that affect multiple system components
  - New integrations or infrastructure changes

  This complements the PRD. The PRD defines WHAT to build;
  the TDD defines HOW to build it.
-->

## Overview

<!-- 1-2 sentence summary of what this document covers -->

**Related PRD:** [Link to PRD]

## Current State

<!--
  How does the system work today?
  Include relevant architecture, data flow, or component diagrams.
-->

## Proposed Design

### Architecture

<!--
  High-level architecture of the proposed solution.
  Use ASCII diagrams where helpful.

  Example:
  ```
  Client → API Gateway → Service A → Database
                       → Service B → External API
  ```
-->

### Data Model

<!--
  New or modified database tables, schemas, or data structures.

  Example:
  ```sql
  CREATE TABLE subscriptions (
    id         UUID PRIMARY KEY,
    user_id    UUID NOT NULL REFERENCES users(id),
    source_id  UUID NOT NULL REFERENCES sources(id),
    frequency  TEXT NOT NULL DEFAULT 'weekly',
    created_at TIMESTAMPTZ DEFAULT NOW()
  );
  ```
-->

### API Design

<!--
  New or modified API endpoints.

  Example:
  ```
  POST   /api/subscriptions          - Create subscription
  GET    /api/subscriptions          - List user's subscriptions
  DELETE /api/subscriptions/:id      - Remove subscription
  ```
-->

### Component Changes

<!--
  Which existing components/modules are affected?
  What new components need to be created?
-->

| Component | Change Type | Description |
|-----------|------------|-------------|
| ... | New / Modified / Deleted | ... |

## Alternatives Considered

<!--
  What other approaches did you evaluate? Why were they rejected?
  This helps future readers understand the reasoning.
-->

| Alternative | Pros | Cons | Why Rejected |
|-------------|------|------|-------------|
| ... | ... | ... | ... |

## Migration Plan

<!--
  If this involves data migration or breaking changes:
  - Step-by-step migration procedure
  - Rollback procedure
  - Estimated downtime (if any)
-->

## Performance Considerations

<!--
  - Expected load/traffic for new endpoints
  - Database query performance (indexes needed?)
  - Caching strategy
  - Rate limiting
-->

## Security Considerations

<!--
  - Authentication/authorization for new endpoints
  - Data validation and sanitization
  - Sensitive data handling
  - New attack surfaces
-->

## Dependencies

<!--
  - New libraries or services required
  - External API dependencies
  - Infrastructure requirements
-->

| Dependency | Version | Purpose |
|-----------|---------|---------|
| ... | ... | ... |

## Implementation Plan

<!--
  How will this be implemented? Reference the batch split from the PRD
  or define a more detailed step-by-step plan.
-->

| Phase | Tasks | Assignee | Estimated Effort |
|-------|-------|----------|-----------------|
| 1 | ... | ... | ... |
| 2 | ... | ... | ... |

## Open Questions

<!--
  Unresolved decisions that need input from the PO or team.
  Track these and update the document as decisions are made.
-->

1. [ ] Question 1
2. [ ] Question 2
