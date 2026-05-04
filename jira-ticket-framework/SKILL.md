---
name: jira-ticket-framework
description: >
  Draft and refine Jira ticket descriptions and acceptance criteria for the engineering team
  engineering tickets. Use this skill whenever someone asks you to write, draft, review,
  or improve a Jira ticket — including Enhancements (Stories), Epics, Bugs, and Production
  Bugs. Also trigger when the user mentions "acceptance criteria," "AC," "ticket description,"
  "Jira ticket," "story," "user story," "definition of done," "GWT" (Given/When/Then),
  or asks you to help scope, refine, or write up a feature, bug report, or epic. This skill
  covers description templates, AC format guidelines, quality checklists, validation checks,
  anti-patterns, dependency flags, and Definition of Done alignment — all tailored to
  the product's domain and workflows.
---

# Jira – Unified Description & Acceptance Criteria Framework

## How to Use This Document

This is an instruction file for drafting Jira tickets. It is organized into shared guidance sections that apply to all issue types, followed by dedicated sections for each issue type (Enhancement, Epic, Bug / Production Bug) that contain the description template and AC guidance specific to that type. When drafting a ticket, read the shared sections first, then navigate to the relevant issue type section.

## Quick Reference

| Issue Type | Clarifying Questions | Description Template | AC Guidance |
|---|---|---|---|
| Enhancement (Story) | Who is the user? What is the outcome? Edge cases? Integrations? | Summary, Mocks, Technical Notes, Risks, Release Notes | Happy path, edge cases, validation, UI/UX, integrations |
| Epic | What is the business outcome? Constraints? Out-of-scope items? | Problem Statement, Proposed Solution, Use Cases, Release Outliers, Product Outliers, Visuals | High-level end-to-end outcomes; refine as stories are created |
| Bug / Production Bug | What is the current vs. expected behavior? Steps to reproduce? Data impact? | Summary, Regression, Workaround, Screenshots, Technical Notes, Risks | Defect no longer reproduces; adjacent functionality unaffected; prod bugs also confirm environment + data remediation |

Shared sections that apply to all issue types: Core Questions · Drafting with Assumptions · Proactive Visuals · Product Knowledge Base · AC Format Guidelines · Validation Checks · Quality Checklist · Anti-Patterns · Dependency Flags · Definition of Done · AC Iteration Guidance

---

## Core Questions Claude Should Ask

Before drafting any ticket:

> "Do I have enough information to identify the user, the action, and the expected outcome?"
>
> "What conditions, when verified, would confirm this ticket is complete and working as intended?"

If either question can't be answered from the ticket input, see the Clarifying Questions for the relevant issue type before drafting.

---

## Drafting with Assumptions

If a ticket has enough information to attempt a draft but some details are unclear, draft the AC and note assumptions at the top:

> ⚠️ *Assumptions made: [state assumption]. If this is incorrect, the following criteria may need to be revised: [list affected criteria].*

---

## Proactive Visuals

Before drafting any ticket, Claude should ask itself: *"Is there a visual — such as a flowchart, table, wireframe, state diagram, or decision tree — that would make the problem or solution clearer than prose alone?"*

If yes, Claude should produce it and include it in the most relevant section of the description:

- **Mocks (Enhancements)** — for UI concepts, flows, or layout ideas
- **Visuals (Epics)** — for diagrams, use case flows, or solution overviews
- **Summary (any issue type)** — for tables or decision trees that help explain the problem

Claude should not wait to be asked. If a visual would add clarity, include it.

---

## Product Knowledge Base

When drafting descriptions or AC, Claude should consult the Product Knowledge Base wherever it adds specificity and accuracy — using real UI element names, correct terminology, and product-specific language rather than generic descriptions.

| Situation | Why It Matters |
|---|---|
| Ticket references a UI element | Use the actual component name (e.g., correct button label, field name, screen name) |
| Ticket involves a known workflow | Reference the workflow by its correct name |
| Ticket references a document or ID type | Use exact terminology (e.g., "Driver's License" vs. "government-issued ID") |
| Ticket involves a county, state, or recording system | Use the correct system or entity name |
| Validation rules reference known constraints | Use actual limits, formats, or values defined in the product |

---

## AC Format Guidelines

Use the format that best fits the rule being described. Default to GWT for behavioral and conditional rules; avoid forcing it where simpler formats are clearer.

### Given / When / Then (GWT) — Default for Behavioral & Conditional Rules

- **Given** – the precondition or starting state
- **When** – the action or trigger
- **Then** – the expected outcome

### List-Style Statements — For Static or Visual Rules

Use when the rule has no trigger or condition (e.g., a visual property, label, or static UI element).

### Scenario Table — For Multi-Variable Combinations

Use when 2+ variables each have multiple values and writing individual GWT blocks would be repetitive.

### Choosing the Right Format

| Situation | Recommended Format |
|---|---|
| Behavior depends on a condition or user action | GWT |
| Rule is static, visual, or unconditional | List-style statement |
| Multiple variables create several combinations | Scenario table |
| Mix of behavioral and static rules | Hybrid — GWT and list-style together |

---

## Validation Checks

Validation checks should be included in AC when they are user-facing, testable, and specific to the behavior defined in the ticket. Claude should evaluate each ticket individually — validation criteria should never be copied generically from ticket to ticket.

### When to Include Validation in AC

| Validation Type | Include in AC? | Notes |
|---|---|---|
| Required field rules | ✅ Yes | Include the specific fields and the error messaging behavior |
| Format or pattern rules | ✅ Yes | e.g., date format, ID number structure, file type |
| Character or file size limits | ✅ Yes | Include the limit and what happens when exceeded |
| User-facing error messages | ✅ Yes | Describe when they appear, what they say, and where |
| Conditional field rules | ✅ Yes | e.g., a field becomes required only when another field is set |
| Backend/database constraints | ❌ No | Belongs in tech notes, not AC |
| API schema validation | ❌ No | Belongs in tech notes unless the user sees the result |
| Existing system-enforced rules | ❌ No | Only include if this ticket changes or extends that behavior |
| Security or access validation | ⚠️ Conditional | Include user-facing outcomes; flag for compliance review if regulatory |

### Writing Validation AC

**GWT example (conditional validation):**

> Given a user submits the form without completing a required field,
> When the submission is attempted,
> Then an inline error message appears below the field reading "This field is required."

**List-style example (static rule):**

- Accepted file types are PDF and JPEG only
- Maximum file size is 10MB
- File name must not exceed 100 characters

**Scenario table example (multi-variable validation):**

| Field | Condition | Behavior |
|---|---|---|
| Document ID | Left blank | Inline error: "Document ID is required" |
| Document ID | Invalid format | Inline error: "Enter a valid Document ID" |
| File Upload | Wrong file type | Inline error: "Only PDF and JPEG files are accepted" |
| File Upload | Exceeds size limit | Inline error: "File must be under 10MB" |

If validation rules are not specified but the ticket involves user input or data submission, flag the gap:

> ⚠️ *Validation gap noted: This ticket involves user input but does not specify validation rules (e.g., required fields, format constraints, error messaging). Confirm expected behavior before finalizing AC.*

---

## Anti-Patterns to Avoid

| Anti-Pattern | Example | Why It's a Problem |
|---|---|---|
| Implementation detail | "The developer updates the status column in the DB" | Describes how, not what — belongs in tech notes |
| Vague outcome | "The page works correctly" | Not testable — pass/fail is undefined |
| Duplicate of description | "User can submit the form" | Restates the ticket, adds no verification value |
| Compound criterion | "User can log in and see their dashboard and receive an email" | Multiple conditions in one — should be split |
| Assumed knowledge | "Follows the existing pattern" | Unclear to anyone unfamiliar with the pattern |
| Untestable negative | "The system should never fail" | Too absolute — replace with specific failure handling criteria |

---

## Dependency & Integration Flags

When drafting any ticket, scan for signals that another system, team, or ticket may affect the outcome. If any of the following are present, flag them alongside the suggested AC:

- References to third-party systems or APIs (e.g., external partner systems, third-party service providers)
- Mentions of another Jira ticket, Epic, or release dependency
- Workflows that span multiple teams or roles
- Data that originates from or is consumed by another service

> ⚠️ *Dependency noted: This ticket references [system/ticket]. AC may need to be validated against the behavior of that system. Confirm with the owning team before finalizing.*

---

## Definition of Done Alignment

AC defines what the ticket must do — but it is one part of a broader Definition of Done. Flag if any of the following appear to be missing or unaddressed:

| DoD Item | What to Watch For |
|---|---|
| Code reviewed | No mention of review requirement on complex or high-risk changes |
| Tests written | No reference to unit, integration, or regression test coverage |
| Accessibility (A11Y) | UI changes with no A11Y criteria — refer to the A11Y Framework |
| Compliance | Changes touching legal or regulatory requirements — refer to the Compliance Framework |
| Documentation | User-facing changes with no mention of help text, release notes, or doc updates |
| Regression Testing | Covered by QA's full regression cycle at release — do not add regression criteria to individual ticket AC |

> 💡 Do not duplicate AC from other frameworks. Cross-reference them and flag when they apply.

---

## AC Iteration Guidance

When refining existing AC:

1. Identify which existing criteria are still valid and retain them
2. Flag criteria that need to be updated or removed and explain why
3. Add new criteria without duplicating existing ones
4. Note any assumptions that have changed

---

## Issue Types

### Enhancement (Story)

#### Clarifying Questions

Ask these before drafting if the ticket is missing key information:

- Who is the intended user performing this action?
- What is the expected outcome when everything works correctly?
- Are there any error states or edge cases to account for?
- Are there any downstream systems or integrations affected?

#### Description Template

**Summary**

- **History & Context:** What led to this request? Is it tied to a previous ticket, customer feedback, a compliance requirement, or a product initiative?
- **Assumptions:** Any assumptions made when defining the problem or solution that could affect implementation if incorrect.
- **Problem:** What is the current gap or pain point? Why does it matter?
- **Definitions:** Any domain-specific terms, acronyms, or concepts relevant to the ticket.
- **User Stories:** One or more statements capturing who the users are, what they want, and why. Format: *"As a [user], I want [goal], so that [reason]."* Claude should consider all user types that may be affected by this ticket and draft a statement for each. Flag any that are assumed for team confirmation.
- **Proposed Solution:** The intended approach. If multiple solutions were considered, briefly summarize the alternatives and why this approach was chosen.

**Mocks** Include wireframes, early-stage flows, or UI concepts produced during the conversation. Final approved UX/UI mocks should be added here once available.

**Technical Notes**

- **Implementation guidance:** Technical approaches, patterns, or architectural decisions the developer should know.
- **Code references:** Relevant files, classes, methods, APIs, or services likely involved.
- **Data considerations:** Database tables, fields, or migrations that may be affected.
- **Integration points:** Third-party services, internal APIs, or system dependencies.
- **Known constraints:** Technical limitations or platform-specific considerations.

**Risks** Claude should reference the Product Knowledge Base to identify risks specific to the system's architecture and business rules before generating this section.

- **Technical Risk:** Could this change break existing functionality, introduce performance issues, or create unintended side effects?
- **Business Rule Risk:** Does this change conflict with or affect any existing business rules?
- **Compliance Risk:** Does this touch any regulated workflows, PII, or legally sensitive areas?
- **Dependency Risk:** Does this rely on other tickets, teams, or third-party systems?
- **Scope Risk:** Are there areas of ambiguity that could cause the ticket to expand beyond its original intent?

**Release Notes** A short, plain-language summary of what is changing and why, written for end users. Avoid technical jargon. Answer: "What did we change, and how does it benefit the user?" Aim for 2–3 sentences.

**Completed Screenshots** To be provided by the working developer upon completion. Leave blank.

#### Acceptance Criteria

AC for a Story should be specific, testable, and user-focused. Each criterion represents a single verifiable condition.

Claude should suggest criteria covering:

1. The primary happy path
2. At least one edge case or boundary condition
3. Error and validation states
4. UI/UX conditions if applicable (field visibility, messaging, state changes)
5. Any downstream or integration impacts

**Example (document upload story):**

> Given a user is on the identity verification screen,
> When they select a valid file to upload,
> Then the file is uploaded and a preview is displayed confirming the document was received.

> Given a user has successfully uploaded their ID,
> When they proceed to the next step,
> Then the system marks identity verification as complete and advances the user in the workflow.

> Given a user is on a mobile device,
> When they reach the upload step,
> Then no upload buttons are shown and "Adding..." text is displayed instead.

Refer to the Validation Checks shared section to determine whether validation criteria apply to this ticket.

---

### Epic

#### Clarifying Questions

Ask these before drafting if the ticket is missing key information:

- What is the business outcome this Epic is meant to achieve?
- Are there known constraints, dependencies, or out-of-scope items?

#### Description Template

**Problem Statement** A clear, concise summary of the problem this Epic is intended to solve. Include who is affected, what the impact is, and why solving it matters. Avoid solution language — focus solely on the problem.

**Proposed Solution** High-level approach to solving the problem. Should give stakeholders enough context to understand the direction. Note any major assumptions or constraints that shaped the approach.

**Use Cases** Example scenarios illustrating how users or systems will interact with the solution. Each use case should describe a specific actor, their goal, and the expected outcome.

**Release Outliers** Items related to this Epic that are explicitly out of scope for the current release but may need to be addressed in a future release.

**Product Outliers** Product-level considerations or related features that fall outside the scope of this Epic but should be tracked for future planning.

**Visuals** Any visuals, diagrams, or reference materials that help illustrate the problem, proposed solution, or use cases.

#### Acceptance Criteria

AC at the Epic level describes the high-level outcomes that define when the entire body of work is complete. Focus on business goals, not implementation steps. Should be broad enough to encompass all child stories but specific enough to be testable.

Claude should suggest:

> "All associated stories are complete and the following end-to-end outcomes are verified: [list outcomes]."

If no child stories exist yet, draft high-level outcome-based AC and note:

> ⚠️ *These criteria should be refined as child stories are created.*

---

### Bug / Production Bug

#### Clarifying Questions

Ask these before drafting if the ticket is missing key information:

- What is the exact behavior currently occurring?
- What is the expected behavior after the fix?
- What are the steps to reproduce the issue?
- Is there any data or user impact that needs to be addressed as part of the fix?

> ⚠️ If no steps to reproduce are provided, note this gap and draft AC based on available information.

#### Description Template

**Summary**

- **History & Context:** When was the issue discovered? How was it reported — by a customer, QA, or internal team? Is this a known recurring issue or newly identified?
- **Assumptions:** Any assumptions made when defining the bug or proposed fix that could affect implementation if incorrect.
- **Problem:** The unintended behavior — what is happening versus what should be happening.
- **Definitions:** Any domain-specific terms, acronyms, or concepts relevant to the issue.
- **Proposed Solution:** If a root cause has been identified, describe the proposed fix. If unknown, note that investigation is needed.
- **Steps to Reproduce:** If known, include the steps required to reproduce the bug consistently.

**Regression** Confirm whether the bug currently exists in production. If yes, note when it was first observed and whether it was introduced by a recent release. If no, describe the environment(s) where it has been reproduced (e.g., QA, staging).

**Potential Workaround** Any known workarounds that allow users to navigate around the issue until a fix is delivered. Include step-by-step guidance if applicable. If no workaround exists, state that explicitly so support teams are aware.

**Screenshots** Include any screenshots, screen recordings, or error logs provided during troubleshooting.

**Technical Notes**

- **Root cause analysis:** Technical explanation of what caused the bug and why the proposed fix addresses it.
- **Code references:** Relevant files, classes, methods, APIs, or services involved in the fix.
- **Data considerations:** Database tables, fields, or data integrity concerns related to the bug or fix.
- **Integration points:** Third-party services, internal APIs, or system dependencies affected by the fix.
- **Known constraints:** Technical limitations or platform-specific considerations.

**Risks** Claude should reference the Product Knowledge Base to identify risks specific to the system's architecture and business rules before generating this section.

- **Regression Risk:** Could the fix inadvertently break related or dependent functionality?
- **Business Rule Risk:** Does this fix conflict with or affect any existing business rules?
- **Compliance Risk:** Does this bug or its fix touch any regulated workflows or sensitive data?
- **Dependency Risk:** Does resolving this depend on changes in other tickets, services, or third-party systems?

#### Acceptance Criteria

AC for a Bug should confirm the defect no longer reproduces and that the fix does not introduce regressions.

Claude should suggest criteria covering:

1. The defect no longer occurs under the original conditions
2. Related workflows or adjacent functionality still work as expected
3. Any edge cases that may have been impacted by the fix

Always include a fix verification statement:

> Given the defect conditions,
> When the fix is applied,
> Then the defect no longer reproduces and existing functionality is not impacted.

**For Production Bugs**, also confirm:

1. The fix is validated in a production-equivalent environment
2. Impacted users or data are accounted for (e.g., backfill, notification, correction)
3. No additional production impact was introduced

---

## Quality Checklist

Before finalizing any AC, verify each criterion meets the following:

| Check | Description |
|---|---|
| ✅ Testable | Can a QA engineer verify this with a clear pass/fail? |
| ✅ Specific | Is the condition precise, not vague or open to interpretation? |
| ✅ User-focused | Is it written from the perspective of the user or system outcome? |
| ✅ Independent | Does each criterion stand alone without depending on another? |
| ✅ Complete | Does the full set define "done" without gaps? |
| ✅ Edge Case | Does the AC include at least one error state, boundary condition, or non-happy-path scenario? |
