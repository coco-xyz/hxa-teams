# Contributing to HxA Teams

> How to submit a new team template.

## We Welcome Contributions

If you've built an effective agent team configuration — for any domain — we'd love to include it. The goal is to build a library of proven team templates that any agent can pick up and run.

## What Makes a Good Template

1. **Tested in practice** — you've actually run agents using this configuration
2. **Clear roles** — each role has defined responsibilities and boundaries
3. **hxa-connect integration** — communication patterns are explicitly defined
4. **Quality gates** — the workflow includes review/validation steps, not just "do the work"
5. **Concrete, not theoretical** — real examples beat abstract frameworks

## How to Submit

### 1. Follow the Schema

Read the [team template specification](../schema/team-template-spec.md). Your template must include:

- `README.md` — team definition, structure diagram, roles, hxa-connect integration, workflow
- `docs/roles.md` — detailed role definitions
- `docs/workflow.md` — step-by-step workflow
- `docs/infrastructure.md` — required tools and setup

Optional but encouraged:
- `docs/case-study-*.md` — real-world usage example
- `docs/getting-started.md` — setup guide specific to this team type
- `templates/` — reusable documents, scripts, or artifacts

### 2. Create Your Template

```
teams/your-team-type/
├── README.md
├── docs/
│   ├── roles.md
│   ├── workflow.md
│   └── infrastructure.md
└── templates/          (optional)
    └── ...
```

### 3. Submit a Pull Request

1. Fork the repo
2. Create a branch: `feat/your-team-type`
3. Add your template under `teams/your-team-type/`
4. Update the main `README.md` teams table
5. Open a PR with:
   - What kind of team this is
   - How you've tested it (which agents, what platform, what project)
   - Any notable lessons learned

### 4. Review Process

All PRs follow an iterative agent review loop before human review:

1. **PR submitted** — You open a pull request
2. **Agent review** — A team agent reviews your PR for schema compliance, consistency, and quality
3. **Iterate if needed** — If the reviewer requests changes, address them and return to step 2. This loop repeats until the agent reviewer approves.
4. **Human review** — Once agent review passes, a maintainer reviews and merges

The agent review must result in an explicit approval before advancing to human review.

## Ideas for New Templates

Looking for inspiration? Here are team types we'd love to see:

- **Customer Support Team** — ticket triage, response drafting, escalation
- **Data Analysis Team** — data collection, cleaning, analysis, reporting
- **Design Team** — requirements gathering, mockup generation, iteration
- **DevOps Team** — monitoring, incident response, infrastructure management
- **Legal/Compliance Team** — document review, policy checking, risk flagging
- **Recruiting Team** — sourcing, screening, interview scheduling

## Questions?

Open an issue or reach out via [hxa-connect](https://github.com/coco-xyz/hxa-connect).

---

## Contributing Improvements from Your Fork

If you're using hxa-teams via a fork (see [Fork & Customize Workflow](fork-workflow.md)), you may discover improvements while running a template on a real project. We encourage contributing these back.

### What to Contribute

- Workflow improvements (better quality gates, clearer handoffs)
- New template files (checklists, scripts, additional docs)
- Documentation fixes and clarifications
- Scaling patterns (e.g., one agent handling multiple roles for small teams)

### How to Contribute

1. Branch from `upstream/main` (not your fork's customized branch)
2. Generalize your improvement — replace project-specific names/URLs with template placeholders
3. **Run the desensitization checklist** (see below)
4. Open a PR to `coco-xyz/hxa-teams` with context on how you discovered the improvement

### Desensitization Checklist

Before submitting a PR, verify your content is fully desensitized:

- [ ] No real person names (replace with Agent A / Agent B / Human Lead)
- [ ] No real domain names or URLs (use `example.com` or `your-org` placeholders)
- [ ] No real thread IDs, group IDs, or API tokens
- [ ] No real organization or company names
- [ ] No real project names that could identify the source (use generic names like "content platform")
- [ ] No real GitHub usernames or email addresses
- [ ] Commit author uses a generic or noreply email (not a company domain)

**Quick scan:** `grep -riE "(your-company|real-name|internal-domain)" --include="*.md"` should return nothing.

### What NOT to Contribute

Project-specific customizations stay in your fork's `instances/` directory:
- Agent name/platform mappings
- Infrastructure details (domains, ports, servers)
- Project-specific communication channels
