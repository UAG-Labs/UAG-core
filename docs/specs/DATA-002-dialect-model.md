# Spec: DATA-002 — Dialect Model

**Spec ID:** DATA-002
**Type:** Data
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose — Defines official and future dialect representation.
1.2 Context — Dialects allow AI, low-level, enterprise, and data modeling without bloating core.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) — none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Define dialect metadata.
   2.1.B Define extension rules.
   2.1.C Prevent redefinition of core concepts.
2.2 Non-Goals (out of scope)
   2.2.A Does not implement remote registry.
   2.2.B Does not allow executable rule code in dialect files.

## 3. Requirements
3.1 Functional requirements
   3.1.A Dialect must declare ID, version, kinds, facets, relationships, compatibility.
   3.1.B Dialect types must map to core concepts.
   3.1.C Aliases must be supported.
3.2 Non-functional requirements
   3.2.A Dialect loading must be deterministic.
   3.2.B Invalid dialects produce diagnostics.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Dialect fields include id, version, extends, entity_kinds, relationship_kinds, facets, aliases.
   4.1.B Registry resolves by ID/version.

## 5. Behavior
5.1 Happy path
   5.1.A Core loads dialect.
   5.1.B Compiler resolves graph kind.
   5.1.C Unknown kind emits diagnostic.
5.2 Edge cases
   5.2.A Alias collision emits diagnostic.
   5.2.B Version mismatch emits diagnostic.
5.3 Error states
   5.3.A Invalid dialect syntax returns error.
   5.3.B Unknown dialect returns diagnostic.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] Official dialects load (verifies §3.1.A) — Verified by: [—]
   6.1.B [ ] Types map to core (verifies §3.1.B) — Verified by: [—]
   6.1.C [ ] Alias conflict detection exists (verifies §3.1.C) — Verified by: [—]

## 7. Open Questions & Assumptions
7.1 Open questions — No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A Official dialects begin as YAML files. — Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
