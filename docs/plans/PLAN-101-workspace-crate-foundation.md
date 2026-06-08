# Plan: PLAN-101 - Workspace and Crate Foundation

**Status:** Draft
**Repo Scope:** `UAG-core`

## Goal
Create the Rust workspace foundation for all future core implementation.

## Tasks
1. Create `crates/uag-core` library crate.
2. Create `crates/uag-schema-gen` utility crate.
3. Add workspace lint, format, test, and doc commands.
4. Add initial module tree matching the architecture docs.
5. Add CI-ready commands to README or contributor docs.

## Success Criteria
1. `cargo fmt --check` runs.
2. `cargo test` runs and discovers both crates.
3. `cargo doc` runs for public API skeleton.
4. Empty modules exist for IDs, model, dialect, diagnostic, serialization, schema, graph, compatibility, and errors.
