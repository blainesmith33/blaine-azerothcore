# ADR-0004: Modular Blaine Suite over a Required Monolithic Module

Status: **Accepted for S0 planning; suite not implemented**  
Date: **2026-07-14**  
Current lifecycle status: **S0 — Proposed**

## Context

The desired player-facing result is one coherent Blaine gameplay experience. That goal does not inherently require one source module. A forced monolith could weaken independent testing, licensing clarity, dependency management, replacement, optionality, and rollback.

## Decision

Define Blaine as a governed module suite or distribution that may combine multiple independently identifiable and testable modules in one integrated realm. Preserve separate implementation modules where technically appropriate. Coordinate them with shared interfaces, configuration conventions, explicit dependency ordering, combined manifests, and integration tests. Do not require all source code to be merged into one monolithic module.

Existing module ideas or implementations may be studied, tested, modified under their licenses, ported, replaced, or independently reimplemented. Provenance, authorship, licensing, and transformation history remain visible for every component.

## Rationale

Player-facing cohesion and implementation modularity solve different problems. A governed distribution can make the experience coherent while maintaining boundaries that improve testability, provenance, conflict diagnosis, maintenance, and rollback.

## Consequences

- Components require stable contracts, naming, dependency ordering, and configuration conventions.
- Each component needs individual evidence and the suite needs combined evidence.
- Individual build success never approves integrated-realm use.
- Releases require an exact combined manifest and component license inventory.
- A monolithic component remains possible when evidence justifies it, but it is not the default architectural mandate.

## Alternatives not selected

- Require one source module for every feature: rejected as an unnecessary coupling constraint.
- Treat unrelated modules as a suite without combined governance: rejected because player-facing unity requires pinned integration and validation.

## Validation boundary

VAL-0002 validates that the modular-suite decision is governed. It does not build, combine, or validate any module.
