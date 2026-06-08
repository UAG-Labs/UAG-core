# Plan: PLAN-103 - Serialization and Schemas

**Status:** Draft
**Repo Scope:** `UAG-core`

## Goal
Provide canonical YAML/JSON serialization and generated schemas.

## Tasks
1. Implement canonical ordering for root sections, object arrays, maps, diagnostics, and loss reports.
2. Implement YAML and JSON round trips.
3. Generate JSON Schema for TAKG, UAGL, dialects, diagnostics, loss reports, and package manifests.
4. Add fixture validation tests using canonical examples.
5. Add schema artifact version metadata.

## Success Criteria
1. Repeated serialization of the same model is byte-stable except allowed formatting differences.
2. Generated schemas validate valid fixtures and reject invalid fixtures.
3. Schema errors can map into diagnostic primitives.
4. Schema artifacts are documented and versioned.
