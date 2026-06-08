# Spec: DATA-001 — Core Object Model

**Spec ID:** DATA-001
**Type:** Data
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose — Defines primary semantic objects shared across TAKG and UAGL.
1.2 Context — Object model prevents semantic flattening across architecture layers.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) — none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Define entities and relationships.
   2.1.B Define boundaries, surfaces, operations, contracts, flows, views, artifacts.
   2.1.C Support layer-aware validation.
2.2 Non-Goals (out of scope)
   2.2.A Does not define React rendering.
   2.2.B Does not define exporter syntax.
   2.2.C Does not use untyped maps as primary model.

## 3. Requirements
3.1 Functional requirements
   3.1.A Each object must have stable ID.
   3.1.B Objects must support plane and layer.
   3.1.C Relationships must have typed direction/category.
   3.1.D Objects must support lifecycle/provenance where needed.
3.2 Non-functional requirements
   3.2.A Struct fields must be explicit and serializable.
   3.2.B Object model must avoid hidden circular references.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Core structs include Entity, Relationship, Boundary, Surface, Operation, Contract, Flow, View, Artifact, Diagnostic.
   4.1.B Each struct uses typed IDs.

## 5. Behavior
5.1 Happy path
   5.1.A Parse YAML object.
   5.1.B Convert to Rust struct.
   5.1.C Serialize deterministically.
5.2 Edge cases
   5.2.A Missing optional metadata uses defaults.
   5.2.B Unknown kind is unresolved dialect reference.
5.3 Error states
   5.3.A Invalid ID returns typed error.
   5.3.B Missing required field returns validation-compatible error.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] Core objects serialize (verifies §3.1.A) — Verified by: [—]
   6.1.B [ ] Plane/layer fields exist (verifies §3.1.B) — Verified by: [—]
   6.1.C [ ] Relationship category exists (verifies §3.1.C) — Verified by: [—]

## 7. Open Questions & Assumptions
7.1 Open questions — No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A Stable IDs are required for diffs. — Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
