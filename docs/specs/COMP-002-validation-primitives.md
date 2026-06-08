# Spec: COMP-002 — Validation Primitives

**Spec ID:** COMP-002
**Type:** Component
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose — Defines diagnostics and validation primitive objects.
1.2 Context — Compiler and Studio need one diagnostic model.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) — none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Define severity enum.
   2.1.B Define diagnostic object.
   2.1.C Define validation result.
2.2 Non-Goals (out of scope)
   2.2.A Does not execute full validation pass.
   2.2.B Does not execute arbitrary user code.

## 3. Requirements
3.1 Functional requirements
   3.1.A Diagnostics must include severity, message, affected object, rule ID, remediation.
   3.1.B Severity supports info/warning/error/critical.
   3.1.C Results serialize.
3.2 Non-functional requirements
   3.2.A Diagnostics must be machine-readable.
   3.2.B Diagnostics must be displayable by CLI/Studio.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Inputs are validation findings.
   4.1.B Outputs are structured diagnostics.

## 5. Behavior
5.1 Happy path
   5.1.A Validator creates diagnostic.
   5.1.B CLI prints diagnostic.
   5.1.C Studio displays diagnostic.
5.2 Edge cases
   5.2.A Package-level diagnostics are allowed.
   5.2.B Multiple diagnostics can reference same object.
5.3 Error states
   5.3.A Missing affected object becomes package diagnostic.
   5.3.B Unknown severity fails deserialization.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] Severity exists (verifies §3.1.A) — Verified by: [—]
   6.1.B [ ] Diagnostic object exists (verifies §3.1.B) — Verified by: [—]
   6.1.C [ ] Result serializes (verifies §3.1.C) — Verified by: [—]

## 7. Open Questions & Assumptions
7.1 Open questions — No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A Full validator lives in UAG-compiler. — Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
