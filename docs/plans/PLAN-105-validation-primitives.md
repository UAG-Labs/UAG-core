# Plan: PLAN-105 - Validation Primitives

**Status:** Draft
**Repo Scope:** `UAG-core`

## Goal
Implement reusable diagnostics and validation primitives.

## Tasks
1. Implement severity enum, diagnostic categories, source locations, affected refs, remediation, and related diagnostics.
2. Implement validation result aggregation and deterministic sorting.
3. Implement conversions from core errors to diagnostics.
4. Implement primitive validators for ID shape, references, enum values, schema shape, secret-like values, and policy fields.
5. Document validation ownership boundaries with compiler and Studio.

## Success Criteria
1. Diagnostics serialize and deserialize.
2. Diagnostic sorting is deterministic.
3. Primitive validators are reusable by compiler and Studio bindings.
4. No full compiler semantic rule orchestration exists in core.
