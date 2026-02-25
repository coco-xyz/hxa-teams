# Contributing to [Project Name]

<!--
  TEMPLATE: Replace [Project Name] with your project name.
  This template is designed for projects built by AI agent dev teams.
  Adapt the sections below to match your project's tech stack and workflow.
-->

## Getting Started

```bash
git clone https://github.com/OWNER/REPO.git
cd REPO
npm install                # or: pip install -r requirements.txt
cp .env.example .env       # fill in your API keys
npm run dev                # or: python main.py
```

## Branch Rules

- `main` is protected — no direct pushes
- All changes go through pull requests
- PRs require: CI passing + 1 approving review

<!--
  If using a develop branch:
  - `develop` is the integration branch — PRs target develop
  - `main` is production — only merged from develop after acceptance
-->

## Workflow

1. Create a feature branch from `main`:
   ```bash
   git checkout -b feat/your-feature main
   ```
2. Make your changes
3. Run checks locally:
   ```bash
   npm run lint             # or your linter
   npm test                 # or your test runner
   ```
4. Push and open a PR — the template will guide you
5. Address review feedback, then wait for merge

## Code Style

<!-- Customize for your project's tech stack -->

- Follow the project's existing code style
- Linter is enforced — run it before pushing
- Keep functions small and focused
- Add comments for non-obvious logic

## Review Process

Every PR goes through:

1. **CI** — Automated checks (lint, test, security audit)
2. **Codex CLI review** — Iterative automated review until CLEAN
3. **Reviewer approval** — Code quality + functionality check
4. **Owner merge** — Product Owner merges in order, resolving any rebase conflicts

<!--
  This review process assumes an agent dev team setup.
  For human-only teams, remove the Codex CLI step.
-->

## Reporting Issues

Open a GitHub issue with:
- Steps to reproduce
- Expected vs actual behavior
- Environment details (runtime version, OS)
