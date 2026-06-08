# ADR-0001 — Repository Purpose and Boundary

**Status:** Accepted  
**Date:** 2026-06-08

## Context
UAG-Labs is a four-repo organization. Each repo needs a strict boundary so implementation does not collapse into an unmaintainable monolith.

## Decision
`UAG-core` has this role:

Rust shared library for graph/language types, TAKG/UAGL models, ontology, dialects, schemas, validation primitives, serialization, and graph utilities.

## Consequences
- This repo must not absorb sibling responsibilities.
- Specs define expected implementation behavior.
- New behavior requires specs before code.
- Cross-repo dependency direction must remain explicit.
