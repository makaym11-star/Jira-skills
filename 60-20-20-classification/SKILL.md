---
name: 60-20-20-classification
description: >
  Classify Jira tickets by evaluating three fields together: 60:20:20, Story/Defect Type,
  and A11Y Required. Use this skill whenever someone asks you to classify, categorize, or
  tag a Jira ticket — or when creating/drafting a ticket that needs these fields populated.
  Also trigger when the user mentions "60:20:20," "story type," "defect type," "A11Y required,"
  "accessibility required," "RTB," "Feature," "Technical Refresh," ticket classification,
  or asks which category a ticket falls into. This skill covers keyword-based signal matching,
  type-to-field mapping tables, edge case override rules, A11Y keyword signals, and a
  structured reasoning output format. Works in tandem with the Jira Ticket Framework
  and Compliance Review Framework.
---

# 60:20:20, Story/Defect Type & A11Y – Combined Decision Framework

## Overview

When a user creates a Jira ticket, Claude should evaluate all three fields together using the ticket title, description, and acceptance criteria. Story Type and Defect Type are strong predictors of both the 60:20:20 classification and the A11Y Required value, so reasoning through them in tandem reduces redundant analysis and improves accuracy.

### Fields covered by this framework:

- **60:20:20** — Required for Epic, Story, Defect, Production Defect
- **Story Type** — Required for Story tickets only
- **Defect Type** — Required for Defect and Production Defect tickets only
- **A11Y Required** — Required for Epic, Story, Defect, Production Defect

> When signals are weak or ambiguous, Claude should flag the uncertainty in its reasoning output rather than defaulting to a specific value. See Reasoning Output section for guidance.

---

## How Claude Should Reason Through All Three Fields

Claude should use the following sequence when evaluating a ticket:

1. **Identify the issue type** (Story, Defect, Production Defect, Epic).
2. **Determine Story Type or Defect Type** using the keyword signals and decision logic below.
3. **Use the type-to-field mapping** to derive both the 60:20:20 classification and A11Y Required value — do not evaluate these fields independently if the type is already clear.
4. **Apply edge case rules** to validate or override the mapping when context warrants it.
5. **Surface all field suggestions together** in a single response.

---

## Story Type Framework

### Signal Keywords by Story Type

| Story Type | Keywords / Signals to Look For |
|---|---|
| Accessibility (ADA) | ADA, accessibility, screen reader, WCAG, keyboard navigation, ARIA, color contrast, 508 compliance, assistive technology |
| Environment/Configuration | environment setup, config change, configuration, infrastructure, deployment settings, env variable, server setup, dev/staging/prod |
| New Functionality | new feature, add ability to, implement, build, create, introduce, users should be able to, new workflow, new screen, new endpoint |
| Onboarding | onboarding, new user setup, account creation, first-time user, welcome flow, customer setup, provisioning |
| Performance/Stability | performance, slow, latency, timeout, load time, memory leak, crash, stability, throughput, optimization, response time |
| QA Automation | automate test, automation script, test coverage, regression automation, Selenium, Cypress, automated suite |
| QA Functional | manual test, test case, test plan, verify functionality, QA review, test scenario, acceptance testing |
| QA Regression | regression, regression suite, regression pass, re-test after fix, regression cycle |
| Release Support | release, deployment support, go-live, rollout, release checklist, cut a release, version bump |
| Retrospective | retrospective, retro, process improvement, team feedback, action item from retro |
| Security | security, vulnerability, authentication, authorization, encryption, OAuth, SAML, SSO, penetration test, CVE, access control, data protection |
| Spike | spike, research, investigate, proof of concept, POC, evaluate options, discovery, explore |
| Support | support request, customer issue, escalation, helpdesk, troubleshoot, user-reported issue, assist customer |
| Technical Debt | technical debt, refactor, clean up, legacy code, code quality, modernize, deprecate, replace old implementation |
| Usability/User Experience | usability, user flow, UX improvement, confusing UI, redesign, user research, improve experience, user feedback |
| UX Debt | UX debt, outdated design, inconsistent UI, design system alignment, legacy UI, UI cleanup |

### Story Type Decision Logic

1. Scan the ticket title, description, and acceptance criteria for keywords above.
2. Match to the most specific type based on keyword signals.
3. When multiple types match, apply these priority rules:
   - Research/discovery task → prefer **Spike** over New Functionality
   - Automated testing → prefer **QA Automation** over QA Functional
   - Code cleanup with no new behavior → prefer **Technical Debt** over New Functionality
   - ADA/accessibility-driven → prefer **Accessibility (ADA)** over Usability/User Experience
4. If no strong signals match → flag uncertainty in the reasoning output and ask the user to clarify.

---

## Defect Type Framework

### Signal Keywords by Defect Type

| Defect Type | Keywords / Signals to Look For |
|---|---|
| Accessibility (ADA) | ADA, accessibility, screen reader, WCAG, keyboard navigation, ARIA, color contrast, 508, assistive technology |
| API Governance | API contract, API versioning, breaking change, API standards, undocumented endpoint, API deprecation, API spec, swagger |
| Compatibility | browser compatibility, cross-browser, mobile device, OS version, platform support, IE, Safari, Android, iOS |
| Compliance | regulatory, law, legal requirement, state requirement, county requirement, compliance, industry-specific regulatory terms |
| Functionality | does not work, broken, incorrect behavior, not functioning, fails to, expected to, bug in, error when, wrong result |
| Grammar | typo, misspelling, grammar, punctuation, incorrect label, wrong text, copy error, wording |
| Performance/Stability | slow, timeout, crash, latency, memory leak, unresponsive, load time, performance regression |
| Security | security vulnerability, unauthorized access, data exposure, CVE, injection, XSS, authentication bypass, encryption |
| Syntax | syntax error, code error, invalid format, parsing error, malformed, JSON error, schema violation |
| Usability/User Experience | confusing, unintuitive, hard to use, poor experience, UX issue, user frustration, unclear |
| Other | Use only when the defect genuinely does not fit any category above and cannot be reasonably inferred |

### Defect Type Decision Logic

1. Scan the ticket title, description, and steps to reproduce for keywords above.
2. Match to the most specific type based on keyword signals.
3. When multiple types match, apply these priority rules:
   - Compliance-driven defect (tied to a law or regulation) → prefer **Compliance** over Functionality
   - Security risk present → prefer **Security** over Functionality
   - Text/copy error only → prefer **Grammar** over Functionality
   - Cross-browser or device issue → prefer **Compatibility** over Functionality
   - ADA/accessibility issue → prefer **Accessibility (ADA)** over Usability/User Experience
4. If no strong signals match → flag uncertainty in the reasoning output and ask the user to clarify.
5. Only suggest **Other** if the defect genuinely does not fit any listed category.

---

## Type-to-Field Mapping

Once Story Type or Defect Type is determined, use this mapping to derive both 60:20:20 and A11Y Required simultaneously.

### Story Type → 60:20:20 & A11Y Required

| Story Type | 60:20:20 | A11Y Required | Notes |
|---|---|---|---|
| New Functionality | Feature | Evaluate* | Depends on whether the feature has UI/user-facing impact |
| Spike | Feature | No | Research/discovery only |
| Technical Debt | Feature | Evaluate* | Depends on whether UI or interactive elements are touched |
| Performance/Stability | Feature | No | Typically no accessibility impact; see edge cases |
| QA Automation | Feature | No | Test automation, no direct accessibility implication |
| QA Functional | Feature | Evaluate* | If testing accessibility scenarios, set Yes |
| Usability/User Experience | Feature | Yes | UX changes affect how users interact with the product |
| QA Regression | RTB | No | Ongoing regression, not accessibility-specific |
| Support | RTB | No | Reactive support work |
| Retrospective | RTB | No | Process activity |
| UX Debt | RTB | Yes | UI/UX changes affect user interaction |
| Accessibility (ADA) | RTB | Yes — always | Hard rule: this type always sets A11Y Required to Yes |
| Environment/Configuration | RTB | No | Infrastructure only, no user-facing impact |
| Onboarding | RTB | Evaluate* | If onboarding has a UI component, evaluate for accessibility |
| Release Support | RTB | No | Release maintenance |
| Security | RTB | No | Security hardening, typically no accessibility impact |

### Defect Type → 60:20:20 & A11Y Required

| Defect Type | 60:20:20 | A11Y Required | Notes |
|---|---|---|---|
| Functionality | Feature or RTB† | Evaluate* | Depends on whether the broken behavior affects user interaction |
| Grammar | Feature or RTB† | Evaluate* | If the text error affects accessible labels or descriptions, set Yes |
| Usability/User Experience | Feature or RTB† | Yes | UX issues affect how users interact with the product |
| Performance/Stability | Feature or RTB† | No | Typically no direct accessibility implication |
| Syntax | Feature or RTB† | No | Code-level error, no direct accessibility implication |
| Accessibility (ADA) | RTB | Yes — always | Hard rule: this type always sets A11Y Required to Yes |
| Compliance | RTB | Evaluate* | If the compliance requirement has a UI component, evaluate |
| Compatibility | RTB | Evaluate* | If the compatibility issue affects assistive technology, set Yes |
| Security | RTB | No | Security defect, no direct accessibility implication |
| API Governance | RTB | No | API-level defect, no direct accessibility implication |
| Other | RTB | Evaluate* | Evaluate based on ticket description |

> **† Pre/post production rule:** For defect types marked Feature or RTB, the deciding factor is deployment status — if the defect is from a feature not yet deployed to Production, it is Feature. If it is present in the Production codebase (even if released "Dark"), it is RTB.

> **\* Evaluate** means Claude should scan the ticket description for A11Y keyword signals (see below) to determine the correct value. If signals are present → Yes. If no signals and no UI impact → No. If unclear → flag as uncertain and ask.

---

## A11Y Required: Additional Keyword Signals

Use these when the Story/Defect Type does not resolve A11Y Required directly (i.e., wherever "Evaluate*" appears in the mapping above).

### Regulatory / Standards

- ADA, Americans with Disabilities Act, WCAG (any version), Section 508, WAI-ARIA, ARIA roles, ARIA labels, EN 301 549, accessibility compliance, accessibility audit, accessibility review

### Assistive Technology / User Experience

- Screen reader (NVDA, JAWS, VoiceOver, TalkBack), keyboard navigation, keyboard-only, tab order, focus management, focus trap, focus indicator, skip navigation, high contrast, color contrast ratio, text resize, zoom support, captions, subtitles, audio description, transcripts, alt text, alternative text, accessible name, touch target, tap target size

### UI / Component Accessibility

- Semantic HTML, heading hierarchy, landmark regions, form labels, input labels, error messages accessible, modal accessibility, dialog accessibility, tooltip accessibility, accessible table, live region, aria-live, aria-alert

### Testing / Tooling

- axe, Lighthouse accessibility, Deque, accessibility scan, accessibility test, manual accessibility review, remediation (in an accessibility context)

### General

- Accessible, accessibility, disability, disabled users, inclusive design, universal design, assistive technology, perceivable, operable, understandable, robust (POUR principles)

---

## A11Y Hard Rules

1. **Story/Defect Type "Accessibility (ADA)" = always Yes.** No exceptions.
2. **Any WCAG, Section 508, or ADA mention = Yes.** Regulatory references are always sufficient.
3. **Assistive technology or screen reader references = Yes.**
4. **Backend-only changes with no UI impact = No.**
5. **When in doubt, default to Yes** — accessibility oversights are harder to remediate post-release. If Claude sets Yes due to uncertainty rather than a clear signal, it must explicitly state this in its reasoning output and identify what aspect of the ticket created the ambiguity.

---

## 60:20:20 Edge Case Rules

Apply these rules to validate or override the type mapping when context warrants it:

1. **Performance improvements — context matters:**
   - New feature meeting SLAs → Feature
   - Optimizing existing queries or processes → RTB
   - Migrating to a new technology → Technical Refresh

2. **Technology migration or replacement = Technical Refresh.** When the solution involves moving to a new technology, framework, or platform, it's Technical Refresh — even if triggered by a compliance or IPT issue.

3. **Compliance/IPT that leads to a tech upgrade:** The initial compliance or IPT ticket is RTB; the Epic/Stories for the technology migration itself are Technical Refresh.

4. **QA Regression = RTB.** Regardless of any other signals, ongoing regression efforts are maintenance activities.

5. **Child/related tickets are evaluated independently.** A child ticket does not inherit the parent's 60:20:20 value unless it independently meets the same classification criteria.

---

## Combined Examples

| Issue Type | Ticket Summary | Story/Defect Type | 60:20:20 | A11Y Required | Key Signal |
|---|---|---|---|---|---|
| Story | "Add ability for users to upload a document from the dashboard" | New Functionality | Feature | Evaluate | UI change — evaluate for interactive elements |
| Story | "Ensure all form inputs meet WCAG 2.1 AA contrast requirements" | Accessibility (ADA) | RTB | Yes | Hard rule: ADA type always = Yes |
| Story | "Investigate switching to GraphQL for API performance" | Spike | Feature | No | Research only, no UI impact |
| Story | "Refactor legacy authentication module to use updated OAuth library" | Technical Debt | Feature | No | Backend refactor, no user-facing impact |
| Story | "Improve button placement on document review screen based on user feedback" | Usability/User Experience | Feature | Yes | UX change affects user interaction |
| Story | "Regression pass for Current Release" | QA Regression | RTB | No | Maintenance activity, not accessibility-specific |
| Defect | "Submit button does not respond on document upload screen" | Functionality | Feature or RTB† | Evaluate | UI interaction — check if focus/keyboard affected |
| Defect | "Users cannot tab through form fields using keyboard navigation" | Accessibility (ADA) | RTB | Yes | Hard rule: ADA type + keyboard navigation signal |
| Defect | "Form does not render correctly in Safari on iOS 16" | Compatibility | RTB | Evaluate | Check if assistive technology on iOS is affected |
| Defect | "document processing submission rejected due to missing field required by state law" | Compliance | RTB | No | Regulatory/backend, no accessibility implication |
| Production Defect | "Application times out processing large document batches" | Performance/Stability | RTB | No | Backend performance, no UI accessibility impact |

---

## Reasoning Output (POC Phase)

During the proof of concept, Claude should explain its suggestions for all three fields together. The reasoning should cover:

1. **Story/Defect Type suggested** — and the keyword or signal that drove it
2. **60:20:20 suggested** — and whether it came from the type mapping or an edge case rule
3. **A11Y Required suggested** — and whether it was resolved by the type mapping, keyword signals, or uncertainty
4. **Confidence level** — High, Medium, or Low (one overall rating, or per-field if they differ)
5. **Alternatives to consider** — top 1–2 alternatives if the ticket could reasonably fit multiple types
6. **Notes** — any ambiguity, missing context, or assumptions made

### Example Reasoning Output Format

> **Story Type:** Accessibility (ADA)
> **Signal:** Ticket references WCAG 2.1 AA contrast requirements and form inputs.
>
> **60:20:20:** RTB
> **Mapping:** Accessibility (ADA) → RTB (remediation of existing accessibility gaps).
>
> **A11Y Required:** Yes
> **Mapping:** Accessibility (ADA) type is a hard rule — always Yes, no further evaluation needed.
>
> **Confidence:** High
> **Alternatives:** None — signals are unambiguous.
> **Notes:** All three fields resolved from a single type determination. No edge case rules applied.

---

## Do's and Don'ts

| Issue Summary | Do ✅ | Don't ❌ |
|---|---|---|
| Previous Release Defects | RTB | Feature |
| Tech Debt 26.1 | RTB | Feature |
| Defects 26.1 | RTB | Feature |
| Performance fix for existing API query | RTB | Feature |
| Angular migration to React | Technical Refresh | RTB |
| Compliance issue requiring a DB migration | RTB (initial ticket), Technical Refresh (migration Epic) | Feature |
| Story with WCAG reference | A11Y = Yes | A11Y = No |
| Backend-only data processing change | A11Y = No | A11Y = Yes |

---

## Source & Sync Notice

| Source | Link |
|---|---|
| Source — 60:20:20 | Internal documentation |
| Source — Story/Defect Type | Internal documentation |
| Source — A11Y Required | Internal documentation |
| Last Reviewed | March 2026 |

> ⚠️ **Maintenance Note:** This file was generated from internal documentation. If any source documentation is updated, this file should be reviewed and updated to stay in sync.
