# Blaine Module Suite Architecture

Status: **Authoritative S0 proposal; not implemented**  
Current lifecycle status: **S0 — Proposed**  
Decision: **ADR-0004**

## Player-facing objective

The intended integrated realm presents a unified Blaine gameplay experience. Player-facing unity means coherent behavior, configuration, naming, progression, and support; it does not require all implementation source to reside in one module.

## Governed distribution

The Blaine implementation may be released as a governed module suite or distribution composed of independently identifiable modules, dependencies, configuration, patches, database changes, and manifests. A distribution release pins the exact compatible set and preserves each component's provenance and license.

## Component boundaries

Separate implementation modules are preferred where they preserve coherent ownership, testability, replacement, rollback, licensing, or optionality. Shared behavior is coordinated through:

- documented interfaces and AzerothCore hooks;
- shared naming and configuration conventions;
- explicit dependency and load ordering;
- namespaced database and configuration changes where feasible;
- combined integration manifests;
- declared cross-module contracts and conflict ownership.

No architectural rule requires merging all source code into one monolithic module.

## Independent and combined validation

Each component must remain independently buildable and testable where technically appropriate. Individual success is necessary evidence, not integration approval. A suite candidate must also pass combined compile, database, runtime, gameplay, upgrade, rollback, conflict, and regression validation.

## Manifest model

Each suite manifest identifies the exact AzerothCore commit, component names and exact commits, dependency versions, required patches, database revisions and order, configuration state, client requirements, supported platform, known conflicts, and linked RUN/VAL evidence.

## Evolution

Components may be original, independently inspired, derived, dependencies, or reference-only during evaluation. Governed promotion may choose porting, adaptation, replacement, or independent implementation. Removed or superseded components retain provenance and decision history.

## Full-stack packages

A governed module suite may participate in a broader full-stack feature package containing a client addon, blAIne Realm Control definition, explicit protocol, presets, migrations, configuration, documentation, and tests. Components remain independently versioned and testable; the server remains authoritative for protected gameplay state, and addon or control-plane requests are treated as untrusted input.
