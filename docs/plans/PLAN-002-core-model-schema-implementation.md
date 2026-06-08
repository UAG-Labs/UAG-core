# Plan: PLAN-002 - Core Model and Schema Implementation

**Status:** Draft
**Derived From:** ../specs/DATA-001-core-object-model.md, ../specs/DATA-002-dialect-model.md, ../specs/COMP-001-serialization-schema-library.md, ../specs/COMP-002-validation-primitives.md
**Derivation Status:** Current

## Objective
Implement the first executable graph contract: Rust model types, typed IDs, canonical serialization, generated schemas, dialect loading, and validation primitives.

## Preconditions
1. `UAG` canonical examples exist and remain parseable.
2. TAKG/UAGL root fields are stable enough for first schema generation.
3. No unresolved core object-model question blocks implementation.

## Work Packages
| Work Package | Scope | Primary Specs | Exit Criteria |
|---|---|---|---|
| WP1 Workspace skeleton | Create `crates/uag-core` and `crates/uag-schema-gen` with tests and CI-ready commands. | SYS-001 | `cargo test` discovers both crates. |
| WP2 Typed IDs and refs | Implement package, object, entity, relationship, view, source-map, diagnostic, artifact, and dependency IDs. | DATA-001 | Invalid IDs return typed errors. |
| WP3 Core graph structs | Implement TAKG/UAGL shared structs for entities, relationships, boundaries, surfaces, operations, contracts, flows, views, policies, source maps, runtime observations, dependencies, artifacts, diagnostics, and loss reports. | DATA-001 | Structs serialize and deserialize. |
| WP4 Serialization | Implement YAML/JSON read/write with canonical ordering. | COMP-001 | Round-trip and canonical snapshot tests pass. |
| WP5 Schema generation | Generate JSON Schema for TAKG, UAGL, dialects, diagnostics, loss reports, and package manifests. | COMP-001 | Schema artifacts validate canonical fixtures. |
| WP6 Dialect registry | Load deterministic namespaced dialect definitions without executable rule code. | DATA-002 | Unknown dialect/kind produces diagnostics-compatible result. |
| WP7 Validation primitives | Implement severity, diagnostic categories, source locations, validation result, and conversion from core errors. | COMP-002 | Diagnostics serialize and sort deterministically. |

## Suggested Module Layout
```text
crates/uag-core/src/
  ids.rs
  model/
    mod.rs
    entity.rs
    relationship.rs
    contract.rs
    view.rs
    source_map.rs
    runtime.rs
    policy.rs
    artifact.rs
  dialect/
  diagnostic/
  serialization/
  schema/
  error.rs
```

## Test Plan
1. Unit tests for ID parsing and formatting.
2. YAML/JSON round-trip tests for small objects.
3. Fixture tests against canonical examples copied or referenced from `UAG`.
4. Snapshot tests for canonical ordering.
5. Schema generation tests that validate TAKG/UAGL fixtures.

## Dependencies
1. `serde` and YAML/JSON support.
2. Schema generation crate selected during implementation.
3. No dependency on `UAG-compiler` or `UAG-studio`.

## Risks
1. Model surface may grow too large for the first crate pass.
2. JSON Schema generation may not represent all semantic validation.
3. Canonical ordering must be designed early or diff/query behavior will drift.

## Exit Criteria
1. `cargo test` passes.
2. Generated schemas exist for all first contract artifacts.
3. Canonical examples can be parsed or schema-validated.
4. `UAG-compiler` can depend on a stable crate API for compile planning.
