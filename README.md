<p align="center">
  <img src="./assets/banner/uag-labs-readme-banner.svg" alt="UAG-core banner" />
</p>

# UAG-core

**The canonical Rust type layer for the UAG graph-native architecture compiler platform.**

UAG-core defines what the graph is. Everything else in the UAG system — the compiler, the policy engine, the Studio — speaks the language defined here.

## What It Owns

- **Typed identifiers** — `NodeId`, `EdgeId`, `EventId`, `CapabilityId`, `ResourceId`, `ConstraintId`, `GoalId`, `HoleId`
- **TAKG structs** — editable source graph types, full primitive set
- **UAGL structs** — compiled IR types, all five behavior layers
- **Graph primitives** — nodes, edges, events, capabilities, resources, constraints, goals
- **Behavior layer types** — `StateMachine`, `Predicate`, `Effect`, `Transformation`, `Hole`
- **Shape/type system** — typed data contracts across graph edges and hole boundaries
- **Ontology and dialect model** — kinds, facets, planes, layers, profiles
- **Validation diagnostics** — error/warning/loss types, rule primitives
- **Serialization helpers** — YAML/JSON round-trip support
- **Schema versioning** — graph schema versions, migration support
- **Schema generation utility** — emit canonical JSON Schema from Rust types

## What It Does Not Own

- CLI commands
- Full compiler pipeline or policy engine logic
- Codegen emitters
- Studio UI
- Platform adapters

## Graph Primitives

The seven typed primitives every UAG graph is built from:

| Primitive | Rust Type | Description |
|---|---|---|
| **Node** | `Node` | Systems, services, modules, components, agents, processes |
| **Edge** | `Edge` | Calls, dependencies, data flows, event subscriptions |
| **Event** | `Event` | Domain events, commands, queries, notifications |
| **Capability** | `Capability` | Operations a node exposes or consumes |
| **Resource** | `Resource` | Databases, queues, stores, streams, cloud primitives |
| **Constraint** | `Constraint` | Security boundaries, SLAs, data retention, access rules |
| **Goal** | `Goal` | Business objectives, quality attributes, non-functional requirements |

## Behavior Layer Types

Five behavior layers are encoded directly in the graph. UAG-core owns the type definitions for all of them:

```text
Layer 1 — STRUCTURE     Node, Edge                         what exists, what connects
Layer 2 — STATE         StateMachine, StateTransition       per-capability control flow
Layer 3 — COMPOSITION   Predicate, Effect, Transformation   typed logic from composable primitives
Layer 4 — POLICY        (resolved by UAG-compiler)          adapter bindings, naming conventions
Layer 5 — HOLES         Hole                                typed contracts for irreducible logic
```

- `StateMachine` — states, transitions, guards, terminal states per capability
- `Predicate` — typed boolean expression trees (And, Or, Not, Comparison, MemberOf)
- `Effect` — typed resource operation descriptor (read, write, emit, consume)
- `Transformation` — shape-typed data conversion pipeline with input/output schemas
- `Hole` — named typed contract: inputs, outputs, invariants; no implementation, routed by policy

## Constraint and Goal as Active Generators

Constraints and Goals are not annotations. They are graph nodes that drive compilation:

- A `Constraint` node carries enforcement semantics — the compiler must emit enforcement code, not a comment
- A `Goal` node carries quality attribute semantics that influence pattern selection in the policy engine
- An unenforced constraint is a compilation error

## Workspace

```text
crates/uag-core        canonical graph model, all primitives, behavior layer types
crates/uag-schema-gen  emits JSON Schema from uag-core Rust types
```

## First Milestone

Implement public Rust structs and serialization for the full canonical object model: all seven primitives, all five behavior layer types, shape/type system, typed identifiers, validation diagnostics.
