# Architecture — UAG-core

## Identity
UAG-core is the canonical Rust type layer for the UAG system. It defines the graph — not a view of the graph, not a serialization of the graph, but the graph itself as typed Rust data structures. Every other UAG component speaks the language defined here.

## Boundary
Pure Rust model and utility layer. No compiler pipeline, no policy logic, no UI code. UAG-compiler and UAG-studio depend on UAG-core; UAG-core depends on nothing else in the UAG ecosystem.

## Module Structure

```text
crates/uag-core/
  ids/            typed identifiers — NodeId, EdgeId, EventId, CapabilityId, ...
  takg/           TAKG structs — editable source graph type system
  uagl/           UAGL structs — compiled IR type system
  primitives/     seven graph primitive types
  behavior/       five behavior layer types
  shape/          data shape and type system — typed contracts across edges and holes
  graph/          traversal, query, index primitives
  ontology/       kinds, facets, planes, layers, profiles
  dialects/       dialect registry and official dialect definitions
  validation/     diagnostic types — Error, Warning, Loss; rule primitives
  serialization/  YAML/JSON helpers, round-trip support
  schema/         schema versioning, migration support
  error/          core error types

crates/uag-schema-gen/
  CLI utility — emits canonical JSON Schema from uag-core Rust types
```

## Graph Primitives (`primitives/`)

The seven typed primitives every UAG graph is built from. These are first-class Rust types, not enum variants of a generic `Node`:

| Module | Type | Role |
|---|---|---|
| `node` | `Node` | Systems, services, modules, components, agents, processes |
| `edge` | `Edge` | Calls, dependencies, data flows, event subscriptions |
| `event` | `Event` | Domain events, commands, queries, notifications |
| `capability` | `Capability` | Operations a node exposes or consumes |
| `resource` | `Resource` | Databases, queues, stores, streams, cloud primitives |
| `constraint` | `Constraint` | Security boundaries, SLAs, data retention, access rules |
| `goal` | `Goal` | Business objectives, quality attributes, non-functional requirements |

Edges carry typed annotations: synchronous/asynchronous, call direction, data shape, optional temporal and concurrency attributes.

## Behavior Layer Types (`behavior/`)

Five behavior layers are encoded directly in the graph. UAG-core defines the type for each layer:

### Layer 2 — StateMachine
```
StateMachine {
  capability_id: CapabilityId,
  states: Vec<State>,
  transitions: Vec<StateTransition>,
  initial: StateId,
  terminal: Vec<StateId>,
}
StateTransition { from, to, guard: Option<Predicate>, effect: Option<Effect> }
```
Every state must have defined transitions. Every terminal state must be reachable. The compiler validates exhaustiveness.

### Layer 3 — Predicate
Typed boolean expression tree for guards, access rules, and conditional composition:
```
Predicate = And(Vec<Predicate>)
          | Or(Vec<Predicate>)
          | Not(Predicate)
          | Comparison { lhs: ShapeRef, op: CompareOp, rhs: Value }
          | MemberOf { subject: ShapeRef, set: SetRef }
          | HasRole { subject: ShapeRef, role: RoleRef }
```

### Layer 3 — Effect
Typed resource operation descriptor. Describes what a capability does to resources:
```
Effect = Read  { resource: ResourceId, shape: ShapeRef }
       | Write { resource: ResourceId, shape: ShapeRef }
       | Emit  { event: EventId, payload: ShapeRef }
       | Consume { event: EventId, payload: ShapeRef }
       | Call  { capability: CapabilityId, input: ShapeRef, output: ShapeRef }
```

### Layer 3 — Transformation
Shape-typed data conversion pipeline. Describes how data moves and changes shape across a dataflow edge:
```
Transformation {
  input:  ShapeRef,
  output: ShapeRef,
  steps:  Vec<TransformStep>,
}
```

### Layer 5 — Hole
Typed contract for irreducible business logic. The hole's existence, contract, and adapter binding are tracked in the graph. The implementation lives in a registered adapter:
```
Hole {
  id:         HoleId,
  name:       String,        // derives from graph naming; never AI-generated
  inputs:     Vec<(String, ShapeRef)>,
  outputs:    Vec<(String, ShapeRef)>,
  invariants: Vec<Predicate>,
  adapter:    Option<AdapterRef>,  // None = compilation readiness error
}
```
An unrouted Hole at compile time is a compilation readiness error, not a warning.

## Shape/Type System (`shape/`)

UAG-core defines a typed shape system used across edges, effect descriptors, transformation pipelines, and hole contracts. Shapes are not raw JSON schemas — they are typed graph nodes:

```
Shape = Primitive(PrimitiveType)         // String, Int, Float, Bool, Bytes, DateTime
      | Struct { fields: Vec<Field> }
      | Enum   { variants: Vec<Variant> }
      | List   { item: ShapeRef }
      | Map    { key: ShapeRef, value: ShapeRef }
      | Option { inner: ShapeRef }
      | Ref(ShapeId)                      // named reference to another shape
```

Shapes are the type system of the graph. The codegen module derives language-specific type definitions from them.

## Constraint and Goal as Active Generators

`Constraint` and `Goal` nodes carry enforcement semantics beyond their type fields:

- A `Constraint` has an `enforcement: EnforcementMode` field — `TypeSystem`, `Runtime`, `Audit`, or `Documentation`. The compiler must emit enforcement at the specified level. `Documentation` is the only level that permits a comment rather than code. All other levels are compilation errors if the emitter cannot satisfy them.
- A `Goal` has a `quality_attribute: QualityAttribute` field that influences the policy engine's pattern selection — e.g., a `HighAvailability` goal causes the compiler to select retry/circuit-breaker patterns where applicable.

## TAKG vs UAGL Type Systems

UAG-core defines both type systems as separate module trees. They share shape types but are otherwise independent:

**TAKG (`takg/`)** — editable, human-authored, may have unresolved references, dialect expansions, partial specifications. This is what Studio writes.

**UAGL (`uagl/`)** — fully resolved, canonicalized, all references are typed IDs, all behavior layers are present, no dialect sugar. This is what the compiler emits and what codegen reads.

The lowering boundary between TAKG and UAGL is owned by UAG-compiler. UAG-core only owns the type definitions on both sides.

## Determinism

All collection types in UAG-core use `IndexMap` or `BTreeMap` rather than `HashMap` to guarantee deterministic iteration order. Same graph inputs always produce the same serialized output.
