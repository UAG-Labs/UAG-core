# Plan: PLAN-107 - Release Hardening

**Status:** Draft
**Repo Scope:** `UAG-core`

## Goal
Prepare `UAG-core` for stable consumption by sibling repos and external users.

## Tasks
1. Audit public API and remove accidental exports.
2. Add crate-level docs and examples.
3. Add complete fixture tests and snapshot tests.
4. Add release notes and compatibility notes.
5. Verify dependency policy and license metadata.
6. Publish schema artifacts in the expected release location.

## Success Criteria
1. Public API docs build cleanly.
2. All tests pass locally and in CI.
3. Sibling repos can pin a version and consume model/schema contracts.
4. Release artifact contains crate docs, schema artifacts, and compatibility notes.
