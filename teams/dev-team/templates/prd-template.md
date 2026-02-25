# [Feature Name] — Product Requirements Document

<!--
  TEMPLATE: Replace [Feature Name] with your feature name.

  How to use:
  1. Coordinator Agent fills this out based on PO's requirement
  2. Submit as a PR for PO review
  3. PO approves → development begins
  4. Development and QA reference this document throughout
-->

## Background

<!-- Why are we building this? What problem does it solve? -->

## Solution

### Design

<!--
  How will we build it? Include:
  - Data model changes (new tables, fields)
  - API endpoints (new or modified)
  - UI changes (new pages, modified components)
  - Architecture decisions
-->

### Impact Analysis

<!--
  What existing functionality is affected?
  - Which modules/files will change?
  - Are there breaking changes?
  - Performance implications?
-->

## Acceptance Criteria

<!--
  Define what "done" looks like. Be specific.
  The QA agent and PO will test against these.
-->

1. [ ] Criterion 1
2. [ ] Criterion 2
3. [ ] Criterion 3

## Test Cases

<!--
  Detailed test scenarios. QA Agent uses these for testing.
  Cover: happy path, edge cases, error handling, regression.
-->

| # | Scenario | Steps | Expected Result |
|---|----------|-------|-----------------|
| 1 | ... | ... | ... |
| 2 | ... | ... | ... |
| 3 | ... | ... | ... |

## Rollback Plan

<!--
  Required if there are destructive changes (DB migrations, data transformations).
  Optional for additive-only changes.
-->

## Task Breakdown

<!--
  How will this be split into batches for parallel development?
  The Coordinator fills this out after PRD approval.
-->

| Batch | Assignee | Scope |
|-------|----------|-------|
| 1 | [Agent A] | ... |
| 2 | [Agent B] | ... |

## Ownership

- **PRD Author:** [Coordinator Agent]
- **Development:** [Agent A, Agent B]
- **QA:** [QA Agent]
- **Approval:** [Product Owner]
