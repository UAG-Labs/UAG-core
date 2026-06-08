# Plan: PLAN-100 - Long-Term Implementation

**Status:** Draft
**Handoff Target:** Haiku 4.5, Composer, GPT-5.2, or equivalent implementation agent
**Repo Scope:** `UAG-core` only

## End State
`UAG-core` is the production-quality Rust foundation for UAG. It provides typed IDs, graph model structs, TAKG/UAGL serialization, generated schemas, dialect loading, ontology primitives, validation primitives, source-map/runtime/provenance/loss-report types, graph utilities, compatibility handling, and stable crate APIs used by compiler and Studio.

## Non-Negotiable Boundaries
1. Do not implement compiler pipeline orchestration, exporters, CLI commands, or Studio UI here.
2. Do implement reusable model and validation primitives that other repos can depend on.
3. Keep this crate deterministic, testable, and dependency-light.

## Phases
| Phase | Plan | Exit State |
|---|---|---|
| 1 | [PLAN-101](./PLAN-101-workspace-crate-foundation.md) | Workspace, crates, CI commands, and module skeleton exist. |
| 2 | [PLAN-102](./PLAN-102-typed-core-model.md) | TAKG/UAGL model structs and typed refs compile. |
| 3 | [PLAN-103](./PLAN-103-serialization-schemas.md) | Canonical YAML/JSON and schema generation work. |
| 4 | [PLAN-104](./PLAN-104-dialects-ontology.md) | Dialect and ontology registry loads deterministically. |
| 5 | [PLAN-105](./PLAN-105-validation-primitives.md) | Diagnostics, validation results, and core errors are stable. |
| 6 | [PLAN-106](./PLAN-106-graph-utilities-compatibility.md) | Graph indexes, traversal helpers, compatibility, and migration primitives exist. |
| 7 | [PLAN-107](./PLAN-107-release-hardening.md) | Public API, docs, tests, and release artifacts are stable. |

## Final Success Criteria
1. `cargo test` passes across all crates.
2. Generated schemas validate canonical TAKG/UAGL/dialect/diagnostic/loss/package fixtures.
3. Canonical serialization is stable across repeated runs.
4. Public APIs are documented and usable by `UAG-compiler` and `UAG-studio`.
5. Compatibility and migration primitives support current plus supported previous schema versions.
6. No compiler- or Studio-specific behavior leaks into core.

## Very Last Task
After all phases and final success criteria are complete, perform a full `docs/` folder audit as the final task in this repo. Update the documentation folder so it fully reflects the finished system, including specs, inherited procedures, plans, ADRs, skills, generated schema notes, public API decisions, compatibility records, and any repo-specific implementation knowledge. This documentation audit must be the final closeout action and should not be skipped or moved earlier.
