# lapwOS Roadmap

**Status:** PROPOSED

## Purpose

This roadmap defines the major delivery milestones for lapwOS, their expected outputs, and the conditions for moving between phases.

It is a planning document, not a substitute for accepted requirements or architecture decisions.

## M0 — Governance and Working Foundations

**Current state:** Largely established; remaining items require verification.

### Deliverables

- AI Manifesto and Source of Truth hierarchy.
- Explicit requirement lifecycle.
- Accepted Requirements and ADR documentation structure.
- Project instructions aligned with the documented working rules.
- A maintained roadmap with explicit milestone exit criteria.

### Exit criteria

- The authoritative documents and their roles are clear.
- Proposals are not treated as accepted decisions.
- Document changes can be reviewed and verified.
- Project instructions support consistent collaboration across conversations.

## M1 — MVP Product Requirements

**Current state:** In progress.

### Deliverables

- Review accepted requirements and related ADRs for consistency.
- Identify unresolved decisions that materially affect MVP behavior or architecture.
- Define verifiable acceptance criteria for critical MVP requirements.
- Keep MVP scope separate from future ideas.
- Document accepted decisions and maintain related requirements consistently.

### Planning principle

Resolve decisions that materially affect MVP scope, technical architecture, or the first implementation slice. Do not require every product detail to be finalized before moving forward.

### Exit criteria

- MVP scope and exclusions are explicit.
- Product decisions that could materially change architecture or the first implementation slice are resolved or explicitly bounded.
- Remaining uncertainties are documented and do not block the next phase.
- Critical requirements have verifiable acceptance criteria.
- Implementation tasks can be derived without inventing product rules.

## M2 — Technical Architecture

**Current state:** Not started as a formal architecture phase.

### Deliverables

- Select the desktop technology stack.
- Define persistence and Event Store boundaries.
- Specify event transaction, projection, and Work Interval derivation responsibilities.
- Define recovery behavior and relevant failure boundaries.
- Establish the test architecture and data migration approach.
- Record accepted architectural decisions as ADRs.

### Planning principle

Define and accept the core architecture before implementation. Low-risk implementation details may be resolved during development, provided they do not contradict accepted requirements or architecture decisions.

### Exit criteria

- Core architecture boundaries and critical decisions are documented and accepted.
- Architecture is consistent with accepted product requirements.
- Data integrity, event transaction, timing, and recovery behavior have explicit answers where required for the first implementation slice.
- Remaining implementation details and their boundaries are documented where necessary.
- The first implementation slice can be defined without inventing product rules or making unapproved architectural decisions.

## M3 — Implementation Readiness

**Current state:** Not started.

### Deliverables

- Select the first implementation slice.
- Define agent task boundaries and review responsibilities.
- Define test and acceptance criteria for the first slice.
- Confirm repository structure and development workflow.
- Identify dependencies and prerequisites.

### Exit criteria

- The first slice has a clear scope and measurable completion criteria.
- Required architecture decisions are accepted.
- Testing and review responsibilities are clear.
- No critical prerequisite is left implicit.

## M4 — MVP Implementation and Validation

**Current state:** Not started.

### Deliverables

- Implement in small, verifiable increments.
- Validate Event Store and Time Engine behavior.
- Test interval derivation, aggregation, and recovery.
- Validate user-facing behavior against accepted requirements.
- Record defects, deviations, and follow-up decisions.

### Exit criteria

- Required MVP acceptance criteria are verified.
- Critical tests pass.
- Known release-blocking issues are resolved.
- Implementation and documentation are consistent.

## Roadmap Governance

- Milestones remain proposed until explicitly accepted.
- Progress is reported through verified deliverables and exit criteria, not invented percentages.
- Accepted requirements and ADRs take precedence over this roadmap.
- Unresolved details must not be silently invented during implementation.
- Scope changes must follow the documented requirement lifecycle.
- A milestone may be considered complete only when its exit criteria are verified.

## Next Step

Continue M1 by identifying the unresolved product decisions that genuinely block technical architecture or the first implementation slice. Resolve those decisions before starting M2.
