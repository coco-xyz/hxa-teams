# Release Process

Team workflow for shipping versions. Applies to all projects in your hxa-teams setup.

## Release Checklist

Every version release must follow these steps in order.

### 1. Pre-release

- [ ] All PRs for the release are merged to `main`
- [ ] Version bumped in `package.json` (and `manifest.json` for browser extensions)
- [ ] CHANGELOG updated
- [ ] All tests pass (`npm test` or equivalent)

### 2. Tag and Release

- [ ] Create git tag: `git tag v{VERSION}`
- [ ] Push tag: `git push origin v{VERSION}`
- [ ] Create GitHub/GitLab Release with release notes
- [ ] For browser extensions: include the `.zip` asset in the release

### 3. Full Functional Test (MANDATORY)

Every release must include a complete functional test before going to production. A fast release with bugs is not a release.

After the release is created:

- [ ] QA agent deploys to staging using the release tag
- [ ] Run the full automated test suite
- [ ] Run manual verification covering all major modules:
  - Authentication (OAuth, API key, invite flows)
  - Core CRUD operations
  - Routing and delivery rules
  - AI features (if applicable — requires API key configured in staging)
  - Analytics
  - Extension or plugin features (if applicable)
- [ ] Write a test report with: PASS/FAIL/WARN counts, issues found, environment details
- [ ] Post test report to the team thread (hxa-connect + Human Lead sync channel)

### 4. Production Deployment

- [ ] Test report shows no blocking issues
- [ ] Human Lead approves production deployment
- [ ] Deploy to production
- [ ] Smoke test production after deploy

### 5. Post-release

- [ ] Update `docs/roadmap.md` (or equivalent) with release status
- [ ] Close related issues

## Roles

| Role | Responsibility |
|------|---------------|
| Developer (Agent A / Agent C) | Code, PR, version bump, tag, create release |
| QA (Agent B) | Full functional test, write and post test report |
| Product Owner (Human Lead) | Approve production deployment |

## Notes

- If you use a `/health` or `/version` endpoint, read the version from `package.json` automatically — no manual sync needed.
- For browser extensions: the distribution zip must be attached to the GitHub/GitLab Release so the exact build is reproducible.
- Never push to production without Human Lead explicit approval. The test report is a prerequisite, not a formality.
