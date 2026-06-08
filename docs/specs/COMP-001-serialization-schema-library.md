# Spec: COMP-001 - Serialization and Schema Library

**Spec ID:** COMP-001
**Type:** Component
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose - Specifies YAML/JSON serialization and schema generation responsibilities.
1.2 Context - Compiler and Studio need stable IO and schema validation.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) - none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Support TAKG/UAGL YAML round trips.
   2.1.B Support JSON round trips.
   2.1.C Generate versioned JSON Schema artifacts for TAKG, UAGL, dialects, diagnostics, loss reports, and package manifests.
   2.1.D Define canonical serialization and migration compatibility behavior.
2.2 Non-Goals (out of scope)
   2.2.A Does not implement CLI file commands.
   2.2.B Does not implement Studio file picker.

## 3. Requirements
3.1 Functional requirements
   3.1.A Serialization must preserve semantic fields.
   3.1.B Serialization must be stable and canonical.
   3.1.C Schemas must match model structs and be generated from the Rust model.
   3.1.D Generated schemas must include `$id`, schema version, model version, and compatibility metadata.
   3.1.E Schema validation errors must map into validation primitives with object path and source path when available.
   3.1.F The library must preserve unknown fields only when the version policy allows them.
3.2 Non-functional requirements
   3.2.A Round trips must be tested.
   3.2.B Errors must include context where practical.
   3.2.C Canonical output must order root sections, objects, maps, diagnostics, and loss entries deterministically.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Inputs are Rust structs.
   4.1.B Outputs are YAML, JSON, and schema artifacts.
   4.1.C Schema artifacts are grouped by model family and version.
   4.1.D Canonical serialization sorts objects by typed ID, relationships by source/target/id, and diagnostics by severity/code/source path/id.

## 5. Behavior
5.1 Happy path
   5.1.A Serialize object.
   5.1.B Deserialize object.
   5.1.C Compare semantic equality.
   5.1.D Generate schema and validate canonical examples.
5.2 Edge cases
   5.2.A Unknown future fields follow version policy.
   5.2.B Empty optional sections default safely.
   5.2.C Older compatible minor versions can be read and normalized before `1.0`.
5.3 Error states
   5.3.A Invalid YAML returns typed error.
   5.3.B Schema mismatch returns validation result.
   5.3.C Non-canonical output fails fixture tests.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] YAML round trips pass (verifies 3.1.A) - Verified by: [--]
   6.1.B [ ] JSON round trips pass (verifies 3.1.B) - Verified by: [--]
   6.1.C [ ] TAKG/UAGL/dialect/diagnostic schemas exist (verifies 3.1.C) - Verified by: [--]
   6.1.D [ ] Canonical fixture output is stable (verifies 3.2.C) - Verified by: [--]
   6.1.E [ ] Schema errors map to diagnostics (verifies 3.1.E) - Verified by: [--]

## 7. Open Questions & Assumptions
7.1 Open questions - No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A Initial format is YAML plus JSON Schema. - Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
