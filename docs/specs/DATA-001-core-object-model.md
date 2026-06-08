# Spec: DATA-001 - Core Object Model

**Spec ID:** DATA-001
**Type:** Data
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose - Defines primary semantic objects shared across TAKG and UAGL.
1.2 Context - Object model prevents semantic flattening across architecture layers.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) - none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Define entities, relationships, boundaries, surfaces, operations, contracts, flows, views, artifacts, source maps, runtime observations, dependencies, policies, and diagnostics.
   2.1.B Support layer-aware validation without baking every domain into core.
   2.1.C Define canonical IDs, canonical ordering, and typed references.
2.2 Non-Goals (out of scope)
   2.2.A Does not define React rendering.
   2.2.B Does not define exporter syntax.
   2.2.C Does not use untyped maps as primary model.
   2.2.D Does not execute full semantic validation; the compiler owns rule orchestration.

## 3. Requirements
3.1 Functional requirements
   3.1.A Each object must have a stable typed ID and optional human-readable name.
   3.1.B Objects must support `plane`, `layer`, `kind`, `tags`, `owner`, `lifecycle`, `classification`, and `provenance` where applicable.
   3.1.C Relationships must have typed direction/category plus `source`, `target`, `mode`, `protocol`, `cardinality`, `data`, `auth`, and `failure_behavior`.
   3.1.D Views must be semantic projections with filters; layouts must be separate.
   3.1.E Runtime observations must link to design entities without mutating design entities.
   3.1.F Contracts must represent operations, messages, schemas, bindings, channels, and servers at a protocol-neutral level.
   3.1.G Dependencies and artifacts must allow external provenance references for SBOM, deployments, generated docs, API specs, and AI context.
   3.1.H The model must support source maps from source object paths/spans to compiled objects.
3.2 Non-functional requirements
   3.2.A Struct fields must be explicit and serializable.
   3.2.B Object model must avoid hidden circular references.
   3.2.C Canonical serialization order must be deterministic for stable diffs.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Core structs include Entity, Relationship, Boundary, Surface, Operation, Contract, Flow, View, LayoutRef, Artifact, DependencyRef, SourceMap, RuntimeObservation, Policy, Diagnostic, LossReport, ValidationResult, and PackageRef.
   4.1.B Each struct uses typed IDs and typed references instead of raw strings at Rust API boundaries.
   4.1.C Relationship refs are endpoint IDs plus optional endpoint role/port/operation refs.
   4.1.D Source map refs include package ID, file path, object path, optional line/column span, compiled object ID, and transform stage.
   4.1.E Runtime observations include environment, observed timestamp, telemetry source, entity ref, attributes, status, and confidence.

## 5. Behavior
5.1 Happy path
   5.1.A Parse YAML/JSON object.
   5.1.B Convert to Rust struct.
   5.1.C Serialize deterministically.
5.2 Edge cases
   5.2.A Missing optional metadata uses defaults.
   5.2.B Unknown kind is unresolved dialect reference.
   5.2.C External dependency metadata is preserved by reference when policy allows it.
5.3 Error states
   5.3.A Invalid ID returns typed error.
   5.3.B Missing required field returns validation-compatible error.
   5.3.C Relationship endpoint mismatch returns a structured diagnostic candidate.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] Core objects serialize (verifies 3.1.A) - Verified by: [--]
   6.1.B [ ] Plane/layer fields exist (verifies 3.1.B) - Verified by: [--]
   6.1.C [ ] Relationship contract fields exist (verifies 3.1.C) - Verified by: [--]
   6.1.D [ ] View filter and layout refs are separate (verifies 3.1.D) - Verified by: [--]
   6.1.E [ ] SourceMap and RuntimeObservation objects exist (verifies 3.1.E and 3.1.H) - Verified by: [--]

## 7. Open Questions & Assumptions
7.1 Open questions - No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A Stable IDs are required for diffs. - Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
