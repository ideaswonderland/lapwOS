# LAPWOS AI MANIFESTO

**Project:** Levent Aydın Personal Work OS (lapwOS)  
**Document:** LAPWOS AI MANIFESTO  
**Version:** 0.1  
**Status:** Working Draft  
**Last Updated:** 2026-09-11

---

## 1. Purpose

This document defines how AI should collaborate in the design,
development, evolution, and maintenance of lapwOS.

The manifesto exists to prevent:

- context drift,
- undocumented assumptions,
- hallucinated requirements,
- accidental architectural complexity,
- contradictory decisions,
- loss of historical decisions,
- and implementation based on outdated conversation context.

This document defines the rules of collaboration.

It is not a replacement for product requirements or architecture
documentation.

---

# 2. AI'nin Rolü

AI is a collaborative design and development partner.

AI must:

- understand the user's actual working environment,
- challenge weak or contradictory ideas,
- identify hidden consequences,
- point out conflicts between requirements,
- propose alternatives when appropriate,
- explain the reasoning behind recommendations,
- distinguish facts from assumptions,
- distinguish accepted decisions from proposals,
- and avoid silently inventing requirements.

AI must not simply agree with every idea.

If a proposed feature creates unnecessary complexity, AI should say so.

If two accepted requirements conflict, AI must explicitly identify
the conflict rather than silently choosing one.

AI should optimize for the quality and coherence of lapwOS,
not for the number of features.

---

# 3. Source of Truth Hierarchy

**STATUS: ACCEPTED**

lapwOS uses a documented Source of Truth hierarchy.

The purpose of this hierarchy is to prevent the conversational
history from becoming an unreliable product specification.

## 3.1 Source of Truth Order

When information conflicts, the following order applies:

1. Accepted Requirements
2. Architecture Decisions / ADRs
3. AI Manifesto
4. Roadmap
5. Proposed Requirements
6. Ideas
7. Conversation History

Higher-level sources take precedence over lower-level sources.

Conversation history is NOT considered a permanent source of truth.

---

## 3.2 Accepted Requirements

`docs/requirements/ACCEPTED.md` is the authoritative source
for accepted product requirements.

If a requirement is not documented there as `ACCEPTED`,
AI must not treat it as an accepted product requirement.

---

## 3.3 Architecture Decisions

Architecture decisions that have been formally documented
in ADRs must be respected during implementation.

If implementation reveals that an architecture decision is
no longer appropriate, it must be explicitly revisited.

It must not be silently changed.

---

## 3.4 Conversation History

Conversation history is a design workspace.

It is useful for:

- exploring ideas,
- discussing alternatives,
- asking questions,
- discovering contradictions,
- and developing requirements.

However, conversation history does not override documented
requirements or decisions.

---

## 3.5 Conflict Resolution

When AI discovers a conflict between documented decisions:

1. Identify the conflicting requirements.
2. Explain the conflict.
3. Do not silently choose a solution.
4. Propose one or more possible resolutions.
5. Wait for a decision when user confirmation is required.
6. Update the appropriate documentation after the decision.

---

# 4. Requirement Lifecycle

**STATUS: ACCEPTED**

Every product requirement must have an explicit lifecycle state.

The lifecycle is:

```text
IDEA
  ↓
PROPOSED
  ↓
ACCEPTED
```

Additional states:

```text
ACCEPTED → DEPRECATED
ACCEPTED → REJECTED
PROPOSED → REJECTED
IDEA → REJECTED
```

---

## 4.1 IDEA

An IDEA is a possibility.

It means:

> "This might be useful someday."

An IDEA must not be implemented as an accepted requirement.

An IDEA may contain:

- a short description,
- motivation,
- possible future value,
- open questions.

It does not require detailed design.

---

## 4.2 PROPOSED

A PROPOSED requirement is an actively discussed solution.

It may contain:

- proposed behavior,
- UX suggestions,
- data implications,
- advantages,
- disadvantages,
- open questions.

A PROPOSED requirement is not yet binding.

AI must not present it as an accepted decision.

---

## 4.3 ACCEPTED

An ACCEPTED requirement is an approved product decision.

Once accepted:

- it becomes part of the product specification,
- implementation must respect it,
- conflicting new proposals must explicitly address it,
- and changes must be documented.

Every accepted requirement must have a unique ID.

Example:

```text
REQ-151
```

---

## 4.4 REJECTED

A REJECTED requirement is explicitly not part of the product.

Rejected requirements should remain documented when they
are useful for understanding why a particular design was chosen.

This prevents the same rejected idea from being unknowingly
reintroduced later.

---

## 4.5 DEPRECATED

A DEPRECATED requirement was previously accepted but is no
longer the current rule.

Deprecated requirements should not be used for new implementation.

They should remain documented when their historical context
is important.

---

# 5. Requirement IDs

Every requirement must have a unique and immutable ID.

Example:

```text
REQ-001
REQ-002
REQ-003
...
```

A requirement ID must never be reused.

If a requirement changes substantially, do not overwrite its
historical meaning without documentation.

Use a new requirement or explicitly document the change.

Requirement IDs provide a stable reference between:

- requirements,
- architecture,
- ADRs,
- implementation,
- tests,
- and future discussions.

---

# 6. Ambiguity and Contradiction Handling

AI must not silently resolve ambiguous requirements when the
decision could materially affect the product.

When ambiguity is detected, AI should:

1. identify what is unclear,
2. explain why it matters,
3. provide a reasonable proposal,
4. ask for confirmation when necessary.

Minor implementation details may be resolved pragmatically
when they do not affect product behavior.

Major product, data, or architecture decisions require explicit
confirmation.

---

# 7. Context and Memory Discipline

lapwOS is expected to be a long-running project.

The project must therefore not depend on AI remembering
everything from previous conversations.

Important decisions must be written to project documentation.

AI should periodically recognize when the amount of accumulated
context makes documentation more important than continuing
the conversation.

The objective is:

> The project should remain understandable even if the
> original conversation is no longer available.

---

# 8. Hallucination Prevention

AI must distinguish between:

- known facts,
- accepted requirements,
- proposed solutions,
- ideas,
- assumptions,
- and unknown information.

AI must never present an assumption as an accepted requirement.

AI must never invent:

- previously approved requirements,
- architectural decisions,
- user preferences,
- implementation details,
- completed development work,
- test results,
- or project capabilities.

If information is unknown, AI should say that it is unknown.

---

# 9. Product Before Code

Product decisions should be clarified before implementation
whenever practical.

The preferred sequence is:

```text
Problem
  ↓
Idea
  ↓
Discussion
  ↓
Proposal
  ↓
Decision
  ↓
Requirement
  ↓
Architecture
  ↓
Implementation
  ↓
Test
```

Code must not become the place where unresolved product
decisions are accidentally made.

---

# 10. Event-First / Time Engine Principle

**STATUS: ACCEPTED**

lapwOS is fundamentally a **time engine and work history system**,
not merely a timer application.

The visible timer is a user interface layer.

The underlying system must be capable of reconstructing
meaningful work history from recorded events.

Example:

```text
SESSION_STARTED
TASK_STARTED
INTERRUPTION_STARTED
INTERRUPTION_ENDED
TASK_RESUMED
WAIT_STARTED
WAIT_ENDED
TASK_COMPLETED
SESSION_ENDED
```

---

## 10.1 Events Instead of Continuous Logging

The system must not record every second as an individual event.

Meaningful state transitions should be recorded as events.

Durations should be derived from events.

This reduces unnecessary data while preserving meaningful history.

---

## 10.2 Historical Integrity

Historical Time Engine events must be treated as immutable
historical records.

The system should not rewrite the past merely because the
current Task or Project information has changed.

Current state and historical events are separate concepts.

---

## 10.3 Derived Data

Values such as:

- actual Task time,
- Project actual time,
- Session duration,
- waiting duration,
- interruption duration,
- reports,
- statistics,

should be derived from authoritative historical data wherever
practical.

Derived values must not become competing sources of truth.

---

# 11. Data Integrity

Historical data must be protected from accidental corruption.

The following conceptual distinction must remain clear:

```text
Current State
      ≠
Historical Event
```

For example:

- current Priority may change,
- current Project relationships may change,
- current Task information may change,

while historical work events remain historical.

Any operation that modifies historical data must be treated
as a deliberate architectural decision.

---

# 12. Minimize User Input

lapwOS is intended to reduce the amount of administrative
work required from the user.

The user should be able to:

- create Tasks quickly,
- start work quickly,
- record spontaneous work quickly,
- continue working without excessive forms,
- and obtain useful reports without manually maintaining
  extensive records.

The application should collect information when it provides
meaningful value.

It should not collect information merely because it is
technically possible.

---

# 13. Decision Fatigue

lapwOS should reduce decision fatigue.

The system may recommend what the user should work on next.

However, recommendations must remain understandable and
user-controllable unless a future requirement explicitly
defines stronger behavior.

The application should avoid turning every action into a
configuration decision.

---

# 14. Progressive Complexity

lapwOS should follow the principle:

> Simple by default, detailed when needed.

Frequently used information should be immediately accessible.

Rarely used information should not clutter the primary interface.

Detailed information may be available through:

- detail views,
- history views,
- reports,
- or dedicated management sections.

The existence of data does not mean that all data must be
visible simultaneously.

---

# 15. Explainability

When lapwOS makes a recommendation, the user should be able
to understand why.

For example:

```text
Recommended because:

- Same Project
- Deadline is today
- Natural successor to the previous Task
- Waiting for 4 days
```

The exact Decision Engine calculation is defined separately
from this manifesto.

The important principle is that recommendations should not
appear arbitrary.

---

# 16. Data Retention

Not every piece of data must be retained forever.

Retention policy should distinguish between:

- valuable historical records,
- active operational data,
- archived data,
- temporary/deleted data,
- and derived reporting data.

Deletion rules must be explicit.

Deleting operational data must not silently corrupt
historical accounting or reporting where such history is
intended to remain.

---

# 17. Architecture Evolution

lapwOS should be designed for reasonable evolution,
not hypothetical unlimited complexity.

Future possibilities must not unnecessarily complicate
the MVP.

Examples of possible future directions include:

- multi-user support,
- management features,
- web application,
- team task assignment,
- advanced workflow,
- more sophisticated dependency systems.

These possibilities must not automatically become current
requirements.

---

# 18. MVP Discipline

The MVP should solve the actual core problem well.

AI should actively challenge:

- premature abstraction,
- unnecessary configuration,
- speculative features,
- complex workflow engines,
- excessive customization,
- and architecture built only for hypothetical future needs.

The question should be:

> "Does this help the user solve the problem now?"

rather than:

> "Could this theoretically be useful someday?"

---

# 19. No Configuration for Configuration's Sake

lapwOS should not become a system where every behavior has
a user-configurable switch.

Configuration should exist when it provides meaningful value.

Too many options create:

- decision fatigue,
- maintenance cost,
- UI complexity,
- documentation burden,
- and inconsistent user experiences.

The application should have sensible defaults.

The system may intentionally make some decisions on behalf
of the user.

---

# 20. User's Working Reality

lapwOS must be designed around real work rather than idealized
productivity environments.

The system should account for realities such as:

- interruptions,
- waiting,
- spontaneous work,
- task switching,
- unexpected requests,
- meetings,
- breaks,
- administrative work,
- and incomplete concentration periods.

The goal is not to pretend that uninterrupted productivity
is always possible.

The goal is to make actual work visible, understandable,
and manageable.

---

# 21. Reports Are Different From Operational Screens

Operational screens should help the user work.

Reports should help the user understand and analyze work.

The two should not be unnecessarily combined.

For example:

```text
Task Screen
→ Work

Dashboard
→ Current situation

Task Detailed History
→ History of one Task

Reports
→ Analysis across Tasks / Projects / Time
```

A screen should not become complicated merely because the
underlying data exists.

---

# 22. AI Communication Style

AI should communicate decisions clearly.

When discussing a requirement, AI should make its state explicit:

```text
IDEA
PROPOSED
ACCEPTED
REJECTED
DEPRECATED
```

AI should clearly distinguish:

- recommendation,
- decision,
- open question,
- assumption,
- and implementation detail.

When the user approves a proposal, AI should record it as
accepted rather than continuing to treat it as hypothetical.

When the user rejects an idea, AI should not repeatedly
reintroduce it without a reason.

---

# 23. Change Management

Changing an accepted requirement is allowed.

However, it must be explicit.

Preferred process:

```text
Existing Requirement
        ↓
Problem Identified
        ↓
Discussion
        ↓
New Proposal
        ↓
User Decision
        ↓
Documentation Update
```

AI must not silently rewrite accepted requirements.

When a change affects other requirements, AI should identify
the affected requirements before implementation.

---

# 24. Documentation Is Part of the Product

Documentation is not an afterthought.

At minimum, lapwOS should maintain:

- AI Manifesto
- Accepted Requirements
- Proposed Requirements
- Ideas
- Rejected Requirements
- Deprecated Requirements
- Architecture documentation
- Architecture Decision Records
- Roadmap
- User Manual

Documentation must evolve together with the product.

---

# 25. Development Workflow

The preferred development workflow is:

```text
Discuss
  ↓
Decide
  ↓
Document
  ↓
Design
  ↓
Implement
  ↓
Test
  ↓
Review
```

Implementation should follow documented requirements.

If implementation reveals a contradiction or missing requirement,
the issue should return to the design/documentation stage.

Do not solve major product ambiguity by silently encoding
a decision in code.

---

# 26. Testing Philosophy

Tests should validate accepted behavior.

When a requirement changes, affected tests should be reviewed.

Important business rules should be tested independently
from the UI whenever practical.

Historical data integrity, Time Engine behavior, Task lifecycle,
and other core domain rules should receive particular attention.

---

# 27. AI Must Not Pretend Work Was Done

AI must never claim that something has been:

- implemented,
- tested,
- committed,
- deployed,
- migrated,
- reviewed,
- or verified

unless that action actually occurred.

Planning is not implementation.

Code generation is not successful implementation.

A proposed architecture is not an implemented architecture.

---

# 28. Current Project Status

At the time of this manifesto version:

- lapwOS is in the product design / requirements phase.
- The project is currently single-user focused.
- The Time Engine / event-first principle is accepted.
- Requirements are being developed incrementally.
- MVP scope is intentionally being kept simple.
- Future multi-user / management functionality remains an IDEA.
- Product requirements are being documented separately from
  this manifesto.

The current accepted requirements are maintained in:

```text
docs/requirements/ACCEPTED.md
```

---

# 29. Golden Rule

The most important rule of this manifesto is:

> **Do not silently assume.**
>
> **Do not silently change.**
>
> **Do not silently complicate.**
>
> **Discuss, decide, document, then implement.**

lapwOS should evolve deliberately.

The system should become more capable over time without becoming
more confusing than necessary.

---

# 30. Status of This Manifesto

This document is a living document.

Sections explicitly marked:

```text
STATUS: ACCEPTED
```

represent established project principles.

Other sections may represent working principles or proposals
and may be revised as the project evolves.

Changes to the manifesto itself must be explicit and documented.

---
