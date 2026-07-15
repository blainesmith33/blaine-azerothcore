# ADR-0002: Multi-Realm Server Architecture

Status: **Accepted for S0 planning; implementation not approved**  
Date: **2026-07-14**  
Current lifecycle status: **S0 — Proposed**

## Context

The project anticipates multiple AzerothCore server instances for clean comparison, third-party module research, Blaine development, combined integration, and risky experiments. The intended player experience should make compatible realms visible through one realm-selection menu where feasible, while preventing one realm's mutable world state, configuration, modules, or failures from contaminating another.

## Decision

Adopt the intended logical topology documented in `docs/architecture/multi-realm-architecture.md`:

1. Use one shared client login endpoint and realm-selection menu where technically feasible and validated.
2. Evaluate one shared authentication service and authentication database where technically appropriate.
3. Run multiple isolated worldserver realms.
4. Give every realm unique IDs, names, ports, configuration, character and world databases, module manifests, backups, and lifecycle evidence.
5. Define baseline, research, development, integrated, and disposable instance classes.
6. Allow all realms to be defined while only a resource-appropriate subset runs concurrently.
7. Govern client-modifying modules separately because they may require unified assets or separate client installations.

## Rationale

This topology supports a unified realm-selection experience without sacrificing research isolation, reproducibility, rollback, or gameplay identity. Instance classes make experimental risk and evidence scope explicit.

## Consequences

- Shared authentication is a future candidate, not an S0 implementation claim.
- Realm IDs, ports, data stores, manifests, backups, and records require coordinated allocation.
- Research realms may temporarily use different exact core versions.
- Client compatibility may limit which realms can safely appear for a particular client profile.
- Linux and Windows require separate topology and operational validation.

## Alternatives not selected

- One worldserver for every experiment: rejected because it mixes incompatible baselines and failure domains.
- Completely unrelated login environments by default: rejected as the target experience, though separate clients or endpoints may be required by evidence.
- Run every defined realm simultaneously: rejected pending resource validation.

## Validation boundary

VAL-0002 validates that this architecture is governed and internally represented. It does not validate networking, authentication, databases, realm-list behavior, client connectivity, or performance.
