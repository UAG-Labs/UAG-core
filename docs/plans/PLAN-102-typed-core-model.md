# Plan: PLAN-102 - Typed Core Model

**Status:** Draft
**Repo Scope:** `UAG-core`

## Goal
Implement the typed model shared by TAKG and UAGL.

## Tasks
1. Implement typed IDs and references for packages, entities, relationships, views, operations, contracts, diagnostics, source maps, artifacts, dependencies, and runtime observations.
2. Implement model structs for TAKG root, UAGL root, entity, relationship, boundary, surface, operation, contract, flow, view, layout ref, policy, artifact, dependency ref, source map, runtime observation, diagnostic, loss report, and validation result.
3. Add builders or constructors that enforce minimal invariant checks.
4. Add object-path and normalized-field-path helpers.

## Success Criteria
1. Invalid IDs fail with typed errors.
2. Every model object serializes through serde.
3. Relationship endpoint refs and view refs are typed.
4. Runtime observations and source maps are separate from design objects.
