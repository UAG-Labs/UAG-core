# Plan: PLAN-106 - Graph Utilities and Compatibility

**Status:** Draft
**Repo Scope:** `UAG-core`

## Goal
Provide graph helpers and compatibility primitives without taking over compiler responsibilities.

## Tasks
1. Implement graph indexes for ID lookup, inbound/outbound relationship lookup, view membership lookup, and source-map lookup.
2. Implement traversal helpers used by validation, query, and Studio display.
3. Implement semantic equality helpers for diff consumers.
4. Implement compatibility metadata and current-plus-supported-previous version handling.
5. Add migration primitives that can be called by compiler migration commands.

## Success Criteria
1. Graph indexes are deterministic and tested.
2. Traversal helpers do not mutate model state.
3. Compatibility checks return diagnostics-compatible results.
4. Migration primitives exist without implementing CLI migration commands.
