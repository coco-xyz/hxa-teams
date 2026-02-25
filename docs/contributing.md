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

- A maintainer will review against the schema spec
- We may suggest changes to improve consistency with other templates
- Once approved, your template joins the library

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
