# Open Questions - `UAG-core`

## Status
All initial audit questions have been answered for planning purposes. Future model/API uncertainty should be added as new open questions.

## Research Basis
- Structurizr separates architecture model, views, identifiers, archetypes, groups, and extensions: https://docs.structurizr.com/dsl/language
- JSON Schema separates core and validation vocabularies, and annotations can support documentation/form-generation tools: https://json-schema.org/specification and https://json-schema.org/understanding-json-schema/reference/annotations
- AsyncAPI uses schemas, messages, channels, operations, servers, and protocol bindings as explicit separable concepts: https://www.asyncapi.com/docs/reference/specification/v3.0.0
- OpenAPI descriptions are machine-readable API contracts with a defined document root: https://learn.openapis.org/specification/structure.html

## Question Format
```markdown
## Q-001 - Title
Status: Open | Resolved
Raised by:
Question:
Why it matters:
Options:
Impacts:
Decision needed before:
Resolution evidence:
```

## Q-001 - What exact object taxonomy belongs in core?
Status: Resolved
Raised by: Audit of `DATA-001-core-object-model.md` and cross-repo examples.
Question: What are the first stable core object types: Entity, Relationship, Boundary, Surface, Operation, Contract, Flow, View, Artifact, Diagnostic, or a smaller subset?
Why it matters: Compiler, Studio, schemas, and examples all depend on these names and fields.
Options:
- Implement every listed object now.
- Start with Entity, Relationship, Boundary, View, Diagnostic, and extend later.
- Keep only generic graph primitives in core and move domain object types into dialects.
AI recommendation: Start with Entity, Relationship, Boundary, View, Diagnostic, Artifact, and extension metadata as stable core objects; add Operation, Contract, Flow, Surface, and protocol-specific objects once dialect/export needs prove their exact shape.
Decision: Start with Entity, Relationship, Boundary, View, Diagnostic, Artifact, and extension metadata as stable core objects; add Operation, Contract, Flow, Surface, and protocol-specific objects once dialect/export needs prove their exact shape.
Impacts: Public Rust API, generated schemas, dialect model, Studio palettes, compiler lowering.
Decision needed before: Writing `crates/uag-core` structs.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-002 - How are stable IDs represented?
Status: Resolved
Raised by: Requirement for deterministic compile, diffs, references, and layout.
Question: Should IDs be user-authored strings, generated UUID/ULID-like values, hierarchical paths, or typed newtypes wrapping strings?
Why it matters: IDs determine merge behavior, diff quality, references, and Studio object lifecycle.
Options:
- Human-readable path IDs.
- Stable opaque IDs plus human labels.
- Typed ID newtypes with validated string formats.
AI recommendation: Use typed ID newtypes wrapping validated, human-readable stable strings. Permit generated IDs, but require them to remain stable across save/compile cycles.
Decision: Use typed ID newtypes wrapping validated, human-readable stable strings. Permit generated IDs, but require them to remain stable across save/compile cycles.
Impacts: All TAKG/UAGL references, schema validation, compiler resolver, Studio editing.
Decision needed before: Core ID module implementation.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-003 - What separates core ontology from dialect ontology?
Status: Resolved
Raised by: `DATA-002-dialect-model.md`.
Question: Which kinds, relationship categories, planes, layers, and facets are built into core, and which must be loaded from dialect files?
Why it matters: Too much in core makes dialects cosmetic; too little makes validation weak.
Options:
- Minimal core ontology with all domain concepts in dialects.
- Rich core ontology with dialects only adding aliases/extensions.
- Core defines planes/facets; dialects define kinds and relationship categories.
AI recommendation: Keep core ontology minimal: planes, layers, facets, IDs, relationships, and extension hooks belong in core; domain-specific kinds and aliases belong in dialect files.
Decision: Keep core ontology minimal: planes, layers, facets, IDs, relationships, and extension hooks belong in core; domain-specific kinds and aliases belong in dialect files.
Impacts: Dialect files, validation, examples, Studio palette grouping.
Decision needed before: Dialect loader and official dialect files.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-004 - What is the Rust API boundary for validation?
Status: Resolved
Raised by: `COMP-002-validation-primitives.md` says full validator lives in compiler but diagnostics live in core.
Question: Should core provide only diagnostic structs, reusable primitive validators, or a validation trait system used by the compiler?
Why it matters: Validation rules may be shared by CLI, Studio, and schema generation without pulling compiler logic into core.
Options:
- Diagnostics only.
- Primitive validators for IDs, schema shape, dialect references, and secret heuristics.
- Trait-based validation framework with compiler-owned rules.
AI recommendation: Core should provide diagnostic structs, severity/location models, and primitive validators, while the compiler owns full semantic rule orchestration.
Decision: Core should provide diagnostic structs, severity/location models, and primitive validators, while the compiler owns full semantic rule orchestration.
Impacts: Dependency boundaries, Studio live validation, compiler architecture.
Decision needed before: `validation` module implementation.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-005 - How are schemas generated and versioned?
Status: Resolved
Raised by: JSON Schema research and `COMP-001-serialization-schema-library.md`.
Question: Should schemas be hand-written, generated from Rust types, or generated from a separate language meta-model?
Why it matters: Hand-written schemas can drift from Rust structs; generated schemas may not capture semantic constraints.
Options:
- Rust types generate JSON Schema.
- Meta-model generates Rust and schemas.
- Hand-written schemas plus tests asserting Rust round trips.
AI recommendation: Generate JSON Schema from Rust types for milestone one, then evaluate a meta-model generator only if Rust/schema drift becomes painful.
Decision: Generate JSON Schema from Rust types for milestone one, then evaluate a meta-model generator only if Rust/schema drift becomes painful.
Impacts: `uag-schema-gen`, CI, docs, compiler input validation, Studio forms.
Decision needed before: Schema generator implementation.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-006 - How are layout and editor metadata represented in TAKG without leaking into UAGL?
Status: Resolved
Raised by: TAKG/UAGL separation and React Flow viewport/layout concepts.
Question: What types in core represent layout, selection-independent view state, and editor metadata, and which are excluded from UAGL?
Why it matters: Studio saves visual state, while compiler output must stay deterministic and semantic.
Options:
- Core has TAKG-only layout structs.
- Studio owns layout structs and serializes them under extension metadata.
- Core defines generic extension metadata slots with namespaced ownership.
AI recommendation: Define TAKG-only layout structs in core under explicit layout/editor metadata fields, and prove with tests that UAGL serialization excludes them.
Decision: Define TAKG-only layout structs in core under explicit layout/editor metadata fields, and prove with tests that UAGL serialization excludes them.
Impacts: TAKG schema, Studio state, compiler lowering, UAGL purity.
Decision needed before: TAKG model implementation.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-007 - What error model should core expose?
Status: Resolved
Raised by: Need shared diagnostics across CLI and Studio.
Question: Should core expose a single `Diagnostic` model only, or also typed Rust errors for IO, parsing, schema, dialect, and validation failures?
Why it matters: CLI exit codes, Studio panels, compiler pipeline, and library ergonomics depend on error shape.
Options:
- All user-facing issues are diagnostics; Rust errors are internal.
- Typed errors convert into diagnostics at boundaries.
- Separate fatal errors, diagnostics, warnings, and loss reports.
AI recommendation: Expose typed Rust errors at library boundaries and provide deterministic conversion into user-facing diagnostics for CLI and Studio.
Decision: Expose typed Rust errors at library boundaries and provide deterministic conversion into user-facing diagnostics for CLI and Studio.
Impacts: API design, compiler results, Studio display, test assertions.
Decision needed before: Error/diagnostic modules.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-008 - How are API and event contracts modeled?
Status: Resolved
Raised by: OpenAPI and AsyncAPI research.
Question: Should HTTP operations, message channels, protocol bindings, schemas, and contracts be first-class core concepts or dialect-specific extensions?
Why it matters: UAG aims to export OpenAPI and AsyncAPI, but those standards have specific concepts that may not fit a generic graph.
Options:
- First-class `Operation`, `Contract`, `Channel`, and `Message` objects in core.
- Generic core relationships with OpenAPI/AsyncAPI dialect extensions.
- Core contract interface plus dialect-specific protocol binding structs.
AI recommendation: Use a core contract abstraction for operations, messages, schemas, and bindings, while keeping OpenAPI/AsyncAPI protocol-specific details in dialect/export layers.
Decision: Use a core contract abstraction for operations, messages, schemas, and bindings, while keeping OpenAPI/AsyncAPI protocol-specific details in dialect/export layers.
Impacts: Exporters, dialect design, examples, schema complexity.
Decision needed before: Object model finalization.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-009 - What compatibility policy applies to serialized files?
Status: Resolved
Raised by: Schema versioning and future migrations module.
Question: How many prior TAKG/UAGL versions must core read, and where do migrations live?
Why it matters: Studio users will keep project files; compiler must provide clear upgrade errors.
Options:
- Read only current version before `1.0`.
- Read current and one previous minor version.
- Provide explicit migration commands in compiler using core migration primitives.
AI recommendation: Before `1.0`, read only the current schema plus one previous minor version; provide explicit compiler migration commands when breaking file changes appear.
Decision: Before `1.0`, read only the current schema plus one previous minor version; provide explicit compiler migration commands when breaking file changes appear.
Impacts: Schema module, compiler CLI, Studio file-open UX, tests.
Decision needed before: Versioned schema release.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-010 - What is core's dependency policy?
Status: Resolved
Raised by: Boundary rule that core depends on no sibling repo.
Question: Which third-party crates are acceptable for serialization, schema generation, graph algorithms, diagnostics, and error handling?
Why it matters: Core should remain stable, lightweight, and reusable by compiler and Studio.
Options:
- Minimal dependencies only: serde, serde_yaml/json, thiserror.
- Use graph/schema helper crates where they reduce risk.
- Keep `uag-core` minimal and move heavy helpers to secondary crates.
AI recommendation: Keep `uag-core` dependency-light: `serde`, YAML/JSON support, error handling, and schema generation only; move heavier graph/query helpers into separate crates if needed.
Decision: Keep `uag-core` dependency-light: `serde`, YAML/JSON support, error handling, and schema generation only; move heavier graph/query helpers into separate crates if needed.
Impacts: Compile times, API stability, workspace layout, downstream dependency load.
Decision needed before: Creating Cargo workspace and crates.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Resolved Initialization Decisions
- R-001: Repos are `UAG`, `UAG-core`, `UAG-compiler`, and `UAG-studio`.
- R-002: Rust is used for system-level implementation.
- R-003: React + TypeScript are used for Studio frontend.
- R-004: TAKG is editable source; UAGL is compiled IR.
- R-005: All specs follow fixed `TYPE-NNN-name.md` naming and seven-section format.
