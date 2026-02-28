# Fork & Customize Workflow

> How to use hxa-teams templates in your own projects — fork, customize, and contribute back.

## Overview

```
upstream (coco-xyz/hxa-teams)          your fork (your-org/hxa-teams)
┌──────────────────────────┐          ┌──────────────────────────────┐
│ teams/dev-team/           │ ──fork─▶ │ teams/dev-team/               │
│   README.md (template)    │          │   README.md (template)        │
│   docs/ (template)        │          │   docs/ (template)            │
│   templates/ (template)   │          │   instances/                  │
│                           │          │     my-project/               │
│                           │          │       team-plan.md            │
│                           │          │       overrides/              │
│                           │ ◀──PR─── │                               │
│ (general improvements)    │          │ (project-specific stays here) │
└──────────────────────────┘          └──────────────────────────────┘
```

## Step 1: Fork the Repo

```bash
gh repo fork coco-xyz/hxa-teams --clone
cd hxa-teams
git remote add upstream https://github.com/coco-xyz/hxa-teams.git
```

## Step 2: Select a Template

Browse `teams/` and pick the template that fits your project. For software development, start with `teams/dev-team/`.

## Step 3: Create Your Project Instance

```bash
mkdir -p teams/dev-team/instances/my-project
```

Each instance directory holds your project-specific customization:

```
teams/dev-team/instances/my-project/
├── team-plan.md       # Role assignments, infra, channels (planner output)
├── README.md          # Project-specific notes and context
└── overrides/         # Customized versions of template files
    ├── PROCESS.md     # Your adapted process
    ├── TEAM.md        # Your team structure
    └── ...
```

### Using the Planner

Feed the [planner system prompt](../planner/system-prompt.md) + the template to your agent. Save the output as `team-plan.md` in your instance directory.

### Manual Setup

1. Copy template files you want to customize into `overrides/`
2. Edit them for your project (map generic roles to real agents, update URLs, etc.)
3. Commit to your fork

## Step 4: Apply Templates to Your Project

Copy the relevant template files into your actual project repo:

```bash
# Copy base templates
cp teams/dev-team/templates/CONTRIBUTING.md ~/my-project/
cp teams/dev-team/templates/PROCESS.md ~/my-project/docs/
cp -r teams/dev-team/templates/.github ~/my-project/

# Apply your overrides on top
cp teams/dev-team/instances/my-project/overrides/* ~/my-project/docs/
```

Or reference them directly — your project's AGENTS.md can point to the fork for team context.

## Step 5: Keep in Sync with Upstream

```bash
git fetch upstream
git merge upstream/main
```

Since your customizations live in `instances/` (which is `.gitignore`-d upstream), merge conflicts should be rare.

## Step 6: Contribute Back

### What to PR Back

✅ **General improvements:**
- Better workflow steps or quality gates
- New template files (checklists, scripts)
- Documentation fixes
- Scaling patterns (e.g., one agent handling multiple roles)

❌ **Keep in your fork:**
- Agent name mappings (your agents → template role names)
- Project-specific URLs, ports, domains
- Infrastructure details (your servers, your cloud setup)
- Team-specific communication channels

### How to PR Back

1. Branch from `upstream/main`:
   ```bash
   git checkout -b fix/improve-workflow upstream/main
   ```

2. Generalize your improvement — replace project-specific details with template placeholders

3. Push and open a PR:
   ```bash
   git push origin fix/improve-workflow
   gh pr create --repo coco-xyz/hxa-teams
   ```

## Summary

```
1. Fork  ──▶  your-org/hxa-teams
2. Select template  ──▶  teams/dev-team/
3. Create instance  ──▶  teams/dev-team/instances/my-project/
4. Customize  ──▶  team-plan.md + overrides/
5. Apply to project repo  ──▶  cp templates + overrides → project/
6. Sync upstream  ──▶  git merge upstream/main
7. Contribute back  ──▶  gh pr create --repo coco-xyz/hxa-teams
```
