# Plan: PLAN-104 - Dialects and Ontology

**Status:** Draft
**Repo Scope:** `UAG-core`

## Goal
Implement deterministic dialect and ontology support.

## Tasks
1. Implement dialect metadata, namespace, version, extends, compatibility, aliases, facets, entity kinds, relationship kinds, and export hints.
2. Implement deterministic registry loading.
3. Add official `core` dialect.
4. Add draft fixture dialects for AI-agent, enterprise, low-level, data, runtime, and deployment systems.
5. Validate alias collisions, unknown kinds, version mismatches, and relationship-kind constraints.

## Success Criteria
1. Dialects load without network access.
2. Alias and namespace collisions produce diagnostics-compatible errors.
3. Relationship kinds declare allowed endpoints and required fields.
4. Draft dialects can load in permissive fixture mode.
