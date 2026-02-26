# Team Template Specification

> Standard schema for HxA Teams templates. All team templates must follow this structure.
>
> Templates are designed for **2nd-gen persistent agents** (with memory, identity, and autonomous scheduling) following the **HxA (Human × Agent)** philosophy — humans and agents as equal team members. Templates use [SKILL.md](https://github.com/anthropics/skills) format conventions for cross-platform compatibility.

## Required Fields

Every team template must include:

### 1. Team Definition (`README.md`)

| Field | Required | Description |
|-------|----------|-------------|
| **Name** | Yes | Team type name (e.g., "Dev Team", "Sales Team") |
| **Version** | Yes | Semantic version (e.g., "1.0.0"). Increment on template changes. |
| **Description** | Yes | One-line description of what this team does |
| **Minimum Agents** | Yes | Minimum number of agents needed to run this template (e.g., "2" or "2 agents + 1 human") |
| **Team Structure** | Yes | Visual diagram showing roles and relationships |
| **Roles Table** | Yes | Role name, count, responsibilities |
| **hxa-connect Integration** | Yes | Communication channels, message patterns, escalation paths |
| **Workflow** | Yes | Numbered steps from input to output |
| **Metrics** | Yes | How to measure team performance (throughput, quality, cycle time, etc.) |
| **Templates Included** | Yes | List of included templates/artifacts. Use "Planned — TBD" for templates in development. |

### 2. Role Definitions (`docs/roles.md`)

For each role:

| Field | Required | Description |
|-------|----------|-------------|
| **Role name** | Yes | Clear, descriptive name |
| **Type** | Yes | `human` or `agent` |
| **Count** | Yes | How many (exact or range, e.g., "1" or "1-3") |
| **Responsibilities** | Yes | Bulleted list of what this role does |
| **Skills required** | Yes | What capabilities the agent needs |
| **Communication patterns** | Yes | Who this role talks to, via what channel, about what |
| **Escalation path** | Yes | What happens when this role is blocked |

### 3. Workflow (`docs/workflow.md`)

| Field | Required | Description |
|-------|----------|-------------|
| **Steps** | Yes | Numbered sequence from trigger to completion |
| **Inputs** | Yes | What kicks off the workflow |
| **Outputs** | Yes | What the workflow produces |
| **Handoff points** | Yes | Where work passes between roles |
| **Quality gates** | Yes | Checkpoints before work moves forward |
| **hxa-connect channels** | Yes | Which channels are used at each step |

### 4. Infrastructure (`docs/infrastructure.md`)

| Field | Required | Description |
|-------|----------|-------------|
| **Required tools** | Yes | What tools/platforms agents need access to |
| **hxa-connect setup** | Yes | Channels to create, permissions, thread structure |
| **External integrations** | No | Third-party services (GitHub, CRM, etc.) |
| **Cost estimate** | No | Approximate monthly cost per agent |

## Optional Fields

| Field | Description |
|-------|-------------|
| **Case study** | Real-world example of this team in action |
| **Getting started guide** | Step-by-step setup instructions |
| **Templates** | Reusable documents (PRDs, reports, scripts, etc.) |
| **Failure modes** | Common problems and how to handle them |

## Directory Structure

```
teams/{team-type}/
├── README.md              # Team definition (required)
├── docs/
│   ├── roles.md           # Role definitions (required)
│   ├── workflow.md         # Workflow steps (required)
│   ├── infrastructure.md   # Tools and setup (required)
│   ├── case-study-*.md     # Real examples (optional)
│   └── getting-started.md  # Setup guide (optional)
└── templates/              # Reusable documents (optional)
    └── ...
```

## Versioning Rules

Team templates use [Semantic Versioning](https://semver.org/):

- **Major** (e.g., 1.0.0 → 2.0.0): Breaking changes to roles or workflow — removing a role, renaming roles, reordering workflow steps, changing required fields in a way that invalidates existing team plans.
- **Minor** (e.g., 1.0.0 → 1.1.0): Additive, non-breaking changes — new optional fields, new optional templates, adding a recommended (not required) tool, expanding a role's responsibilities without changing its interface.
- **Patch** (e.g., 1.0.0 → 1.0.1): Documentation fixes, typo corrections, clarifications that don't change behavior.

## Quality Checklist

Before submitting a new team template:

- [ ] All required fields present
- [ ] hxa-connect integration explicitly defined (channels, message patterns)
- [ ] Roles have clear boundaries (no overlapping responsibilities)
- [ ] Workflow has quality gates (not just "do the work")
- [ ] At least one concrete example or case study
- [ ] Diagram showing team structure
- [ ] Tested with at least 2 agents running the template
