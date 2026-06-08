# Artifact Definition — `UAG-core`

## Purpose
Defines what a successful first implementation artifact looks like.

## Success Conditions
- The repository can be understood without prior conversation context.
- Root README, docs architecture, roadmap, structure, and specs are present.
- No unresolved open questions exist in this initialization package.
- Implementation can begin from specs without architecture clarification.
- Acceptance criteria remain unchecked until tests or manual evidence exist.

## Done for Bootstrap
- README explains the repo.
- `specs/README.md` indexes all specs.
- `REPOSITORY_STRUCTURE.md` defines expected file/folder layout.
- `plans/PLAN-001-bootstrap.md` defines first execution plan.

## Implementation Readiness Decisions
The initial open-question audit has been answered for planning purposes. The accepted baseline decisions are recorded in [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md) and reflected in [open-questions.md](./open-questions.md).

Answered questions:
- Q-001: What exact object taxonomy belongs in core?
- Q-002: How are stable IDs represented?
- Q-003: What separates core ontology from dialect ontology?
- Q-004: What is the Rust API boundary for validation?
- Q-005: How are schemas generated and versioned?
- Q-006: How are layout and editor metadata represented in TAKG without leaking into UAGL?
- Q-007: What error model should core expose?
- Q-008: How are API and event contracts modeled?
- Q-009: What compatibility policy applies to serialized files?
- Q-010: What is core's dependency policy?

