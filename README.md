# Claude Skills — Jira Ticket Intelligence

A suite of AI skills built for [Claude](https://claude.ai) that standardize how engineering tickets are created, classified, and evaluated for compliance. Designed for product and engineering teams that use Jira and want consistent, high-quality tickets without slowing down their workflow.

## What This Does

These skills teach Claude how to draft Jira tickets, classify them across multiple field taxonomies, and flag compliance concerns — either all at once or independently. The result is tickets that consistently have the information needed for triage and grooming, reducing rework cycles and back-and-forth.

**Before:** Tickets created with varying quality → triage surfaces gaps → pushed back for questions → re-discussed next session.

**After:** Tickets created with structured descriptions, testable AC, correct field classifications, and compliance flags from the start.

## Skills

### [Ticket Skill Orchestrator](./ticket-skill-orchestrator/SKILL.md)
Coordinates the three skills below into a single end-to-end workflow. Use this when creating a ticket from scratch to get a complete draft with description, acceptance criteria, field classifications, and compliance determination in one pass. Each skill also works independently — the orchestrator just sequences them.

### [Jira Ticket Framework](./jira-ticket-framework/SKILL.md)
Drafts ticket descriptions and acceptance criteria for Stories, Epics, Bugs, and Production Bugs. Includes description templates, AC format guidelines (GWT, list-style, scenario tables), a quality checklist, anti-pattern detection, dependency flags, validation checks, and Definition of Done alignment.

### [60:20:20 Classification](./60-20-20-classification/SKILL.md)
Classifies tickets by evaluating three fields together: **60:20:20** (Feature / RTB / Technical Refresh), **Story/Defect Type**, and **A11Y Required**. Uses keyword signal matching, type-to-field mapping tables, edge case override rules, and a structured reasoning output with confidence levels.

### [Compliance Review Flag](./compliance-review-flag/SKILL.md)
Determines whether a ticket requires compliance review by evaluating it against a decision framework. Outputs **YES**, **NO**, or **I DON'T KNOW** with reasoning and detected keywords. Covers regulatory, privacy, and legal signal detection.

## How to Use

### As a Claude Skill
1. Clone or download this repo
2. Add the skill folder(s) to your Claude project's skill directory
3. Claude will automatically trigger the relevant skill based on your request

### Standalone Reference
Each `SKILL.md` file is a self-contained markdown document. You can read them as reference material for how to structure tickets, classify work, or evaluate compliance — even without Claude.

### Quick Examples

| What You Say | What Runs |
|---|---|
| "Create a ticket for this feature" | Orchestrator → all three skills |
| "Draft acceptance criteria for this bug" | Jira Ticket Framework only |
| "Is this ticket Feature or RTB?" | 60:20:20 Classification only |
| "Does this need compliance review?" | Compliance Review Flag only |
| "Evaluate this ticket" | Orchestrator → all three skills |

## Repo Structure

```
claude-skills/
├── README.md
├── ticket-skill-orchestrator/
│   └── SKILL.md
├── jira-ticket-framework/
│   └── SKILL.md
├── 60-20-20-classification/
│   └── SKILL.md
└── compliance-review-flag/
    └── SKILL.md
```

## Impact

Adopted team-wide for both creating new tickets and evaluating tickets from other teams. Results observed:

- **Standardized ticket quality** across the team regardless of who authors the ticket
- **Reduced triage/grooming cycles** — tickets consistently arrive with the information needed, eliminating the push-back-and-revisit loop
- **Faster classification** — field values (60:20:20, Story/Defect Type, A11Y, compliance flag) are suggested with reasoning rather than debated in meetings
- **Cross-team consistency** — applied to tickets created outside the team to bring them up to the same standard

## Contributing

These skills are living documents. To contribute:

1. Create a branch for your changes
2. Edit the relevant `SKILL.md` file(s)
3. Submit a PR with a description of what changed and why
4. Tag a reviewer from the team

When updating a skill, check whether the change affects cross-references in other skills — especially between the compliance and classification frameworks.

## License

Internal use. See your organization's policies for sharing and distribution.
