# ADR-0002 - Implementation Readiness Decisions for UAG-core

## Status
Accepted

## Context
The first documentation audit identified open implementation-readiness questions for UAG-core. The questions covered core object taxonomy, IDs, ontology, validation primitives, schemas, metadata, errors, contracts, compatibility, and dependencies. Each question received an AI recommendation after reviewing the repository documentation and comparable systems.

## Decision
Adopt the AI recommendations recorded in [open-questions.md](../open-questions.md) as the planning baseline for the first implementation plans. These decisions are not final product law; they are the current accepted defaults that implementation plans should use unless a later ADR supersedes them.

## Decisions
| Question | Topic | Decision |
|---|---|---|
| Q-001 | What exact object taxonomy belongs in core? | Start with Entity, Relationship, Boundary, View, Diagnostic, Artifact, and extension metadata as stable core objects; add Operation, Contract, Flow, Surface, and protocol-specific objects once dialect/export needs prove their exact shape. |
| Q-002 | How are stable IDs represented? | Use typed ID newtypes wrapping validated, human-readable stable strings. Permit generated IDs, but require them to remain stable across save/compile cycles. |
| Q-003 | What separates core ontology from dialect ontology? | Keep core ontology minimal: planes, layers, facets, IDs, relationships, and extension hooks belong in core; domain-specific kinds and aliases belong in dialect files. |
| Q-004 | What is the Rust API boundary for validation? | Core should provide diagnostic structs, severity/location models, and primitive validators, while the compiler owns full semantic rule orchestration. |
| Q-005 | How are schemas generated and versioned? | Generate JSON Schema from Rust types for milestone one, then evaluate a meta-model generator only if Rust/schema drift becomes painful. |
| Q-006 | How are layout and editor metadata represented in TAKG without leaking into UAGL? | Define TAKG-only layout structs in core under explicit layout/editor metadata fields, and prove with tests that UAGL serialization excludes them. |
| Q-007 | What error model should core expose? | Expose typed Rust errors at library boundaries and provide deterministic conversion into user-facing diagnostics for CLI and Studio. |
| Q-008 | How are API and event contracts modeled? | Use a core contract abstraction for operations, messages, schemas, and bindings, while keeping OpenAPI/AsyncAPI protocol-specific details in dialect/export layers. |
| Q-009 | What compatibility policy applies to serialized files? | Before `1.0`, read only the current schema plus one previous minor version; provide explicit compiler migration commands when breaking file changes appear. |
| Q-010 | What is core's dependency policy? | Keep `uag-core` dependency-light: `serde`, YAML/JSON support, error handling, and schema generation only; move heavier graph/query helpers into separate crates if needed. |

## Consequences
- Implementation plans can proceed from a concrete baseline instead of unresolved ambiguity.
- Future disagreement should create a new open question and, if accepted, a superseding ADR.
- Specs and plans should cite this ADR when they rely on these decisions.

## Follow-up
- Update implementation plans to reference this ADR.
- Promote decisions into detailed specs when implementation starts.
