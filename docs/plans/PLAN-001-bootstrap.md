# Plan: PLAN-001 - Bootstrap `UAG-core`

**Status:** Draft
**Derived From:** ../specs/README.md
**Derivation Status:** Current

## Objective
Create the initial implementation skeleton exactly as defined in `../REPOSITORY_STRUCTURE.md`.

## Required Reading
- ../../README.md
- ../../AGENT.md
- ../artifact.md
- ../architecture.md
- ../REPOSITORY_STRUCTURE.md
- ../specs/README.md

## Architecture Graph Hardening
Before downstream repos depend on `UAG-core`, core must make the graph contract executable:

1. Implement typed IDs and typed refs for all core objects.
2. Implement Entity, Relationship, Boundary, Surface, Operation, Contract, Flow, View, LayoutRef, Artifact, DependencyRef, SourceMap, RuntimeObservation, Policy, Diagnostic, LossReport, ValidationResult, and PackageRef structs.
3. Generate versioned JSON Schema artifacts for TAKG, UAGL, dialects, diagnostics, loss reports, and package manifests.
4. Enforce canonical serialization order for stable diffs and repeatable compiler tests.
5. Keep dialect loading deterministic, namespaced, versioned, and free of executable rule code.
6. Keep validation primitives shared while full semantic validation remains in `UAG-compiler`.

## Steps
1. Create the root files and folders defined in `../REPOSITORY_STRUCTURE.md`.
2. Implement only the first milestone in `ROADMAP.md`.
3. Add tests corresponding to acceptance criteria.
4. Do not mark criteria verified until evidence exists.
5. Stop if an unresolved design question appears.
