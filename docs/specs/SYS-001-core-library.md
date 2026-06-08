# Spec: SYS-001 — Core Rust Library

**Spec ID:** SYS-001
**Type:** System-overview
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose — Defines UAG-core as canonical shared Rust model layer.
1.2 Context — Compiler and Studio need one source of model truth.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) — none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Define crate boundaries.
   2.1.B Define module responsibilities.
   2.1.C Keep core independent.
2.2 Non-Goals (out of scope)
   2.2.A Does not implement CLI.
   2.2.B Does not render UI.
   2.2.C Does not run full compiler pipeline.

## 3. Requirements
3.1 Functional requirements
   3.1.A Core must expose TAKG and UAGL types.
   3.1.B Core must expose graph primitives.
   3.1.C Core must expose validation diagnostics.
   3.1.D Core must expose dialect and ontology types.
3.2 Non-functional requirements
   3.2.A Public types must serialize deterministically.
   3.2.B Crate must compile on stable Rust.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Inputs are Rust structs and schemas.
   4.1.B Outputs are library APIs and schema artifacts.

## 5. Behavior
5.1 Happy path
   5.1.A Compiler imports core.
   5.1.B Studio imports types/bindings.
   5.1.C Core remains independent.
5.2 Edge cases
   5.2.A Unknown future fields handled through versioning.
   5.2.B Editor metadata stays out of UAGL.
5.3 Error states
   5.3.A Serialization error returns typed error.
   5.3.B Unknown dialect returns diagnostic-compatible error.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] TAKG/UAGL structs exist (verifies §3.1.A) — Verified by: [—]
   6.1.B [ ] Graph primitives exist (verifies §3.1.B) — Verified by: [—]
   6.1.C [ ] Diagnostics exist (verifies §3.1.C) — Verified by: [—]

## 7. Open Questions & Assumptions
7.1 Open questions — No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A Rust is implementation language. — Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
