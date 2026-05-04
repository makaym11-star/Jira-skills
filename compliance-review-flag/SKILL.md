---
name: compliance-review-flag
description: >
  Evaluate Jira/project tickets to determine whether they require a compliance review.
  Use this skill whenever someone asks you to triage, classify, or flag tickets for
  compliance review — or when evaluating whether a change, feature, or bug ticket has
  regulatory, legal, or privacy implications. Also trigger when the user mentions
  "compliance flag," "compliance review," "is this a compliance issue," regulatory
  triage, or asks whether a ticket needs legal/compliance sign-off. Works for
  [Company] and similar document-recording or government-services platforms, but the
  decision logic applies broadly to any product that must meet federal, state, or
  county legal/regulatory requirements.
---

# Compliance Review Flag – Decision Framework

This skill helps you decide whether a ticket should be flagged for compliance review.
Apply it by reasoning through the ticket's content and answering the core question below.

## Core Question

> "Does this ticket have a direct impact on meeting a federal, state, or county legal or regulatory requirement?"

Based on the answer, assign one of three flags: **YES**, **NO**, or **I DON'T KNOW**.

---

## YES – Flag for Compliance Review

Flag a ticket YES when any of the following signals are present:

| Signal | Reasoning |
|--------|-----------|
| Ticket is driven by a new or existing law, regulation, or government mandate | System must be verified to meet legal requirements |
| Ticket adds a new service to a specific state or county for the first time | Must comply with jurisdiction-specific laws |
| Ticket integrates with a 3rd party platform | Privacy, federal, or state laws may apply |
| Ticket involves document delivery or recording failures (system-side) | Compliance monitors all rejections for reporting purposes |
| Ticket involves numerous rejections that may require a system change | Even if not obviously compliance-related, compliance verifies and monitors |
| Ticket involves new fields added to forms due to a law or county requirement | Must meet the underlying state/county legal requirement |

---

## NO – Do Not Flag for Compliance Review

Do **not** flag a ticket when these signals are present:

| Signal | Reasoning |
|--------|-----------|
| Customer-requested custom change to their own state-promulgated form | Customer assumes responsibility for their own modifications |
| Pure UI/visual change with no bearing on a legal requirement | No regulatory impact |
| Administrative county change (e.g., hours of operation, contact info) | Not tied to a federal, state, or county legal requirement |
| Related/child ticket to a parent compliance ticket, where the child only handles technical implementation (e.g., database update) | Child ticket does not independently affect compliance acceptance criteria |

---

## I DON'T KNOW – Flag for Human Review

Use this when:

- The ticket might involve a regulatory requirement but it is not clearly stated
- The ticket is a system issue where the root cause is unknown and could potentially relate to a legal requirement
- The ticket references compliance indirectly or mentions laws/regulations without clear context
- The ticket involves a county or state change but it's unclear if it's administrative or legally driven

---

## Key Rules to Apply

1. **Direct legal/regulatory driver = YES.** If the ticket exists because of a law, regulation, or government requirement, flag it.
2. **Customer ownership = NO.** If the customer is making a choice about their own forms or configurations, they own compliance for that change.
3. **Indirect or unclear system impact = YES.** When in doubt about whether a system issue touches a legal requirement, compliance reviews it.
4. **Child/related tickets = evaluate independently.** A child ticket does not inherit the parent's compliance flag unless it independently affects the compliance acceptance criteria.
5. **Administrative changes = NO.** Operational or scheduling changes without a legal basis do not require review.

---

## Reasoning Steps

When evaluating a ticket, reason through these questions in order:

1. Is there a law, regulation, or government requirement mentioned or implied?
2. Is the platform (not the customer) responsible for meeting that requirement?
3. Could this change affect how a document is recorded, delivered, or validated under law?
4. Is this a system-side issue that compliance needs to monitor for reporting?

**If yes to any → YES**  
**If clearly none of the above → NO**  
**If uncertain → I DON'T KNOW**

---

## Keyword & Phrase Reference

Use these keyword lists as supplementary signals. Keywords alone don't determine the flag — always apply the reasoning steps above — but they help identify which category to investigate.

### Strong YES – Keywords & Phrases

These should almost always result in a YES flag regardless of surrounding context.

1. Compliance
2. Regulation / Regulations / Regulatory
3. New law
4. New state law
5. PII
6. FPI
7. Privacy
8. Data breach
9. Data loss
10. Customer data
11. SSN
12. Social Security Number
13. Audit
14. SOC2

### Strong I'M NOT SURE – Keywords & Phrases

These should trigger an I Don't Know flag — needs human review.

1. Law / Laws
2. Legal
3. Legally
4. Breach ⚠️ *intentionally separate from "data breach" above*

> **Developer Notes on Redundancy:**
> - "Regulation," "Regulations," and "Regulatory" share the root "regulat-" — consider matching on the root to cover all three with one rule.
> - "Law" and "Laws" share the root "law" — one rule likely covers both.
> - "data breach" (Strong YES) vs. "breach" alone (I'm Not Sure) — this distinction is intentional and well-reasoned; keep as-is.

### Override Phrases – Strong NO Signal

These suggest NO even when a Strong YES keyword is present. *(Placeholder — to be populated based on your examples, e.g., "customer-requested")*

---

## Source & Sync Notice

This framework is based on internal documentation for the compliance review field. If the source documentation is updated, this framework should be reviewed and updated to stay in sync.

**Source:** Internal documentation

> ⚠️ **Last reviewed: March 2026.** Verify this framework reflects the latest guidance before deploying to production.

---

## Output Format

When evaluating a ticket, respond with:

```
**Compliance Flag:** YES | NO | I DON'T KNOW

**Reasoning:** <1-3 sentence explanation citing which signals and rules apply>

**Keywords Detected:** <list any matching keywords from the reference lists, or "None">
```
