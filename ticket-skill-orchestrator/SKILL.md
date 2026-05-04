---
name: ticket-skill-orchestrator
description: >
  Coordinates the three ticket skills (Jira Ticket Framework, 60:20:20 Classification,
  and Compliance Review Flag) to produce a complete, fully classified ticket in a single
  pass. Use this skill whenever someone asks you to create, draft, or fully evaluate a
  Jira ticket from scratch — or when a task clearly requires all three skills working
  together. Also trigger when the user says "create a ticket," "draft a full ticket,"
  "write up this feature/bug/epic," or provides raw requirements and expects a
  ready-to-file ticket with all fields populated. Each skill can also be used
  independently — see the standalone usage section below.
---

# Ticket Skill Orchestrator

## Purpose

This skill coordinates three independent skills into a single end-to-end workflow for creating or evaluating Jira tickets. When all three are used together, the result is a fully drafted ticket with a complete description, acceptance criteria, field classifications, and a compliance determination — all in one pass.

## The Three Skills

| Skill | What It Does | Key Output |
|---|---|---|
| **Jira Ticket Framework** | Drafts the ticket description and acceptance criteria based on issue type (Story, Epic, Bug, Production Bug) | Structured description, AC, risks, technical notes, quality checklist |
| **60:20:20 Classification** | Classifies the ticket by Story/Defect Type, 60:20:20 value, and A11Y Required | Field values with reasoning and confidence level |
| **Compliance Review Flag** | Determines whether the ticket needs compliance review | YES, NO, or I DON'T KNOW with reasoning and detected keywords |

---

## When to Use All Three Together

Use the full orchestrated workflow when:

- Creating a new ticket from scratch (feature request, bug report, epic)
- A stakeholder provides raw requirements and expects a ready-to-file ticket
- Reviewing a draft ticket that hasn't been classified or evaluated yet
- The user says "create a ticket," "draft this up," or "write a full ticket for this"

## When to Use Skills Independently

Each skill works on its own. Use a single skill when the task only requires that skill's output:

- **Jira Ticket Framework only** — "Help me write AC for this ticket" or "Draft a description for this bug"
- **60:20:20 Classification only** — "What Story Type is this?" or "Is this Feature or RTB?"
- **Compliance Review Flag only** — "Does this ticket need compliance review?" or "Flag this for compliance"

If uncertain whether one or all three apply, default to the full workflow. It's better to surface all the information than to miss a field.

---

## Orchestrated Workflow

When running all three skills together, follow this sequence:

### Step 1 — Identify the Issue Type

Determine whether the ticket is a Story (Enhancement), Epic, Bug, or Production Bug. This drives which templates and logic apply in all three skills.

### Step 2 — Draft Description & Acceptance Criteria

Use the **Jira Ticket Framework** to:

- Ask or infer the relevant clarifying questions
- Draft the full description using the appropriate template
- Generate acceptance criteria following the format guidelines and quality checklist
- Flag any assumptions, dependency notes, or Definition of Done gaps
- Include proactive visuals where they add clarity

### Step 3 — Classify the Ticket

Use the **60:20:20 Classification** to:

- Determine Story Type (for Stories) or Defect Type (for Bugs/Production Bugs)
- Derive the 60:20:20 value from the type-to-field mapping
- Derive the A11Y Required value, evaluating keyword signals where needed
- Apply edge case rules if context warrants an override
- Output the reasoning with confidence level

### Step 4 — Evaluate for Compliance

Use the **Compliance Review Flag** to:

- Scan the ticket for compliance signals and keywords
- Apply the decision logic (YES / NO / I DON'T KNOW)
- Surface the reasoning and any detected keywords

### Step 5 — Present the Combined Output

Surface everything together in a single, structured response:

```
## Ticket Draft

**Issue Type:** [Story / Epic / Bug / Production Bug]

### Description
[Full description from Jira Ticket Framework]

### Acceptance Criteria
[AC from Jira Ticket Framework]

---

## Classification

**Story/Defect Type:** [Type] — [signal/reasoning]
**60:20:20:** [Feature / RTB / Technical Refresh] — [mapping or edge case applied]
**A11Y Required:** [Yes / No / Evaluate] — [reasoning]
**Confidence:** [High / Medium / Low]

---

## Compliance Review

**Compliance Flag:** [YES / NO / I DON'T KNOW]
**Reasoning:** [1-3 sentences]
**Keywords Detected:** [list or "None"]
```

---

## Cross-Skill References

The three skills reference each other in several places. The orchestrator ensures these connections are handled automatically:

- The Jira Ticket Framework's **Definition of Done** section flags compliance and A11Y considerations → the other two skills resolve them
- The Jira Ticket Framework's **Risks** section includes Compliance Risk → the Compliance Review Flag provides the answer
- The 60:20:20 Classification's **Defect Type "Compliance"** → aligns with a YES flag from the Compliance Review
- The 60:20:20 Classification's **A11Y Required "Evaluate"** → resolved by scanning for A11Y keyword signals in the drafted description

---

## Notes

- The orchestrator does not add new logic. It sequences the three existing skills and combines their outputs.
- If any skill produces an uncertain or ambiguous result, surface that uncertainty clearly rather than forcing a default.
- When refining an existing ticket (not creating from scratch), skip steps that are already complete and focus on what's missing.
