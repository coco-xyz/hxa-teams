# Codex Review Workflow

Standard AI-assisted code review loop for hxa-teams projects. Uses OpenAI Codex as an automated reviewer, cycling until the codebase is clean before human review.

## Flow

```
PR opened
    │
    ▼
┌─────────────┐
│ Codex Review │ ──── CLEAN ──→ Human Review → Merge
└─────────────┘
    │
    ISSUES FOUND (P1/P2/P3)
    │
    ▼
┌──────────────┐
│ Developer Fix │ ← Fix P1 + P2 (P3 optional)
└──────────────┘
    │
    ▼
┌───────────────┐
│ Codex Re-review│ ──── CLEAN ──→ Human Review → Merge
└───────────────┘
    │
    ISSUES FOUND
    │
    ▼
    ... (repeat until CLEAN)
```

## Severity Levels

| Level | Description | Action Required |
|-------|-------------|-----------------|
| **P1** | Security vulnerability, data loss risk | Must fix before merge |
| **P2** | Bug, crash, incorrect behavior | Must fix before merge |
| **P3** | Code quality, style, minor improvement | Fix optional |

## Running a Codex Review

### Prerequisites

- `codex-cli` installed (v0.106.0+)
- ChatGPT Plus account authenticated (`codex login`)

### Command

```bash
cd <repo>
git diff origin/main...HEAD | codex --quiet --full-auto "
You are a senior security-focused code reviewer. Review this PR diff.

For each issue found:
1. Classify severity: P1 (security/data loss), P2 (bug/crash), P3 (quality)
2. Cite exact file:line
3. Explain the issue and provide a fix

Output format:
## Codex Review — PR #<N> (<title>)
**Model:** <model>
**Result:** CLEAN | ISSUES FOUND

### P1 — <title>
...
"
```

### Review Output

Post the review as a GitHub/GitLab PR comment. Include:
- Model name and version
- Overall verdict: `CLEAN` or `ISSUES FOUND`
- Per-issue: severity, file:line, description, suggested fix

## Merge Gate

A PR requires a `CLEAN` Codex review before merging. The review-fix-review cycle continues until all P1 and P2 issues are resolved.

## Example Cycle

A typical first-pass cycle for a medium-sized PR:

1. **Codex Review** → issues found (e.g., 2 P1 + 3 P2 + 2 P3)
   - Example P1: path traversal vulnerability, unsafe file access
   - Example P2: null dereference crash, missing input validation, incorrect serialization
2. **Developer Fix** → targeted commits addressing each P1 and P2
3. **Codex Re-review** → all fixes verified, CLEAN
4. **Human Review + Merge** → tests passing, no regressions

Total cycle time: roughly 1–3 hours depending on PR scope.

## Future: Automation via .hxa-teams.yml

Target integration to automate the review trigger:

```yaml
pr_workflow:
  on_opened:
    - codex_review:
        auto_trigger: true
        severity_threshold: P2
  on_fix_pushed:
    - codex_re_review:
        auto_trigger: true
  merge_gate:
    require: codex_clean
```

This will be implemented as part of hxa-teams Phase 2 (registry + automation hooks).
