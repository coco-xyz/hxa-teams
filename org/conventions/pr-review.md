# PR Review Convention

PR review rules for all projects in an hxa-teams setup.

## Principle: Review-Before-Merge, Always

All PRs must go through the team review cycle before merging. No exceptions — not for small changes, not for agents, not under time pressure.

## Review Matrix

Customize this for your team. Example with 3 agents and 1 human lead:

| Author | Reviewer | Merger |
|--------|----------|--------|
| Agent C | Agent A reviews + approves | Human Lead merges |
| Agent B | Agent A reviews + approves | Human Lead merges |
| Agent A | Agent B or Agent C reviews + approves | Human Lead merges |

Key invariants:
- No one reviews their own PR.
- Only the Human Lead (or explicitly delegated authority) merges to `main`.
- Every PR gets at least one non-author review.

## Rules

- **Review happens on the PR** — no offline verbal review followed by a PR. Code must be in a PR before review starts. All review comments must appear on the PR so the record is complete and traceable.
- **No skipping review** — even one-line changes go through the process. The habit matters more than the size.
- **No self-merge** — the author never merges their own PR.
- **Squash and merge** — keeps the `main` commit history clean. One logical change = one commit.

## PR Format

- **Title:** `<type>: <short description>` (e.g., `feat: add export to CSV`, `fix: null crash on empty input`)
- **Description:** what changed, how to test it, linked issue (`Closes #42`)
- **One PR, one concern** — do not mix unrelated changes

## Flow

```
Code complete → open PR → request reviewer → reviewer leaves comments on PR → approve
                    ↑                                        │
                    └──────── push fixes ←───────────────────┘
                            (if changes requested)

Approved → notify Human Lead → Human Lead merges
```

**Anti-pattern — do not do this:**
```
Code complete → offline discussion → fix → open PR  ← review record is lost
```

## Response SLA

The product owner should never have to chase progress. When you receive a review request, act on it.

| Situation | Response Time |
|-----------|--------------|
| Received a review request | Begin review within 30 minutes |
| Review returned changes requested | Fix and re-request review within 30 minutes |
| PR approved | Notify Human Lead immediately — do not wait |

If you are blocked (don't understand the code, need a design discussion), leave a comment on the PR explaining the blocker. Silence is not acceptable.

## Notification Flow

1. PR opened → request reviewer on GitHub/GitLab + post in team hxa-connect thread
2. Reviewer acts on request proactively (no chasing)
3. After approval → notify Human Lead via sync channel
4. After merge → automated webhook notifies team thread
