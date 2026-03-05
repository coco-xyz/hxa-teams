# Git Branching Workflow

Branch strategy for agent teams. Applies to all projects in your hxa-teams setup.

## Branch Structure

| Branch | Purpose | Protection |
|--------|---------|------------|
| `main` | Production-ready code | PR required + 1 reviewer, no force push |
| `develop` | Staging/integration | PR required + 1 reviewer, no force push |
| `feat/*`, `fix/*`, `chore/*` | Feature development and fixes | Unprotected — agent freely operates |

## Workflow

```
feat/fix branch → PR → develop (staging) → PR → main (production)
```

### 1. Development

```bash
# Pull latest from develop
git checkout develop && git pull

# Create a feature branch
git checkout -b feat/my-feature

# Develop and commit
git add . && git commit -m "feat: description"

# Push and open PR targeting develop
git push -u origin feat/my-feature
```

### 2. Staging

- PR target branch: `develop`
- At least 1 reviewer must approve
- After merge to `develop`, deploy to staging automatically (configure in CI)
- Staging validation passes → proceed to release flow

### 3. Release

- Open PR from `develop` → `main`
- At least 1 reviewer must approve
- After merge to `main`, create a git tag and release
- Deploy to production (Human Lead approval required)

## Branch Naming

| Prefix | Purpose | Example |
|--------|---------|---------|
| `feat/` | New feature | `feat/annotation-overlay` |
| `fix/` | Bug fix | `fix/toolbar-position` |
| `chore/` | Maintenance | `chore/update-deps` |
| `docs/` | Documentation | `docs/api-guide` |
| `refactor/` | Refactoring | `refactor/storage-layer` |

## Setting Up Branch Protection

### GitHub

```bash
gh api repos/{owner}/{repo}/branches/{branch}/protection \
  --method PUT \
  -f required_pull_request_reviews.required_approving_review_count=1 \
  -F enforce_admins=true \
  -F allow_force_pushes=false \
  -F allow_deletions=false
```

### GitLab

```bash
curl -X POST -H "PRIVATE-TOKEN: $TOKEN" \
  "$GITLAB_URL/api/v4/projects/$PROJECT_ID/protected_branches" \
  -d "name=$BRANCH&push_access_level=40&merge_access_level=40&allow_force_push=false"
```

## Notes

- **Never push directly to `main` or `develop`** — always go through a PR/MR.
- **Codex review is still required** even after branch protection is in place (see `docs/codex-review-workflow.md`).
- **Hotfixes:** branch from `main`, fix, then open PRs targeting both `main` and `develop` to keep them in sync.
- Agent developers follow the same rules as human developers — no exceptions.
