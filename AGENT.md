# AGENT.md — Context for `UAG-core`

## Repository Identity
Repository: `UAG-core`
Organization: `UAG-Labs`
Role: Canonical Rust type layer — graph primitives, behavior layer types, TAKG/UAGL structs, schema versioning.

## Non-Negotiable Rules
- The architecture graph is the source of truth.
- The diagram is a view.
- The export is a projection.
- The code is a compilation target.
- All seven graph primitives (nodes, edges, events, capabilities, resources, constraints, goals) must be typed with full fidelity — no lossy encoding.
- All five behavior layer types (StateMachine, Predicate, Effect, Transformation, Hole) must be first-class graph types, not annotations.
- Constraints and Goals are active generators, not passive labels. Unenforced constraints are compilation errors.
- Holes are typed contracts with named inputs, outputs, and invariants. The absence of an implementation is not a type error here — it is a compilation error in UAG-compiler.
- TAKG is the editable source graph. UAGL is the compiled IR. UAG-core owns both type systems.
- Rust only. No UI code belongs here.

## Technology
Rust workspace. No React UI. No compiler pipeline. No CLI.

## Dependency Boundary
Depends on no UAG sibling repo. UAG-compiler and UAG-studio both depend on UAG-core. Nothing in this repo may depend on either of them.

## Expected Output
- `crates/uag-core`: all seven graph primitives, all five behavior layer types, typed identifiers, shape/type system, ontology, dialect registry, validation diagnostics, YAML/JSON serialization, schema versioning
- `crates/uag-schema-gen`: CLI utility that emits canonical JSON Schema from uag-core Rust types

## Working Instructions
1. Read `README.md`, `docs/architecture.md`, `docs/artifact.md`, `docs/REPOSITORY_STRUCTURE.md`, and `docs/specs/README.md` before any work.
2. Add or update specs using `docs/procedures/add-specification-file.md`.
3. Do not implement undocumented behavior.
4. Do not create unresolved questions. Record blockers in `docs/open-questions.md` and stop.
5. Preserve TAKG as editable source graph and UAGL as compiled IR. They are separate type systems with a defined lowering boundary.
6. Keep generated output deterministic wherever possible.
7. Keep repo responsibilities inside this repo's boundary. Policy resolution, adapter binding, and codegen are compiler concerns — not core concerns.
