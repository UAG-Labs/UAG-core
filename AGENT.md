# AGENT.md — Codex Context for `UAG-core`

## Repository Identity
Repository: `UAG-core`  
Organization: `UAG-Labs`  
GitHub description: Shared Rust graph model, schemas, ontology, dialects, and validation primitives for TAKG and UAGL.

## Non-Negotiable Rule
The graph is the source of truth. The diagram is a view. The export is a projection.

## Role
Rust shared library for graph/language types, TAKG/UAGL models, ontology, dialects, schemas, validation primitives, serialization, and graph utilities.

## Technology
Rust workspace. No React UI. No full compiler pipeline.

## Dependency Boundary
Depends on no UAG sibling repo. UAG-compiler and UAG-studio depend on it.

## Expected Output
Reusable Rust crates defining canonical model and schema generation support.

## Working Instructions
1. Read `README.md`, `docs/architecture.md`, `docs/artifact.md`, `docs/REPOSITORY_STRUCTURE.md`, and `docs/specs/README.md` before implementation.
2. Add or update specs using `docs/procedures/add-specification-file.md`.
3. Do not implement undocumented behavior.
4. Do not create unresolved implementation questions. Record blockers in `docs/open-questions.md` and stop.
5. Preserve TAKG as editable source graph and UAGL as compiled IR.
6. Keep generated output deterministic wherever possible.
7. Keep repo responsibilities inside this repo's boundary.
