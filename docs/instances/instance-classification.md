# Server Instance Classification

Status: **Authoritative S0 classification; no instances deployed**  
Current lifecycle status: **S0 — Proposed**

## Common controls

Every defined instance receives an identifier, purpose, owner, platform, lifecycle status, exact core/module manifest, database manifest, configuration identity, client requirements, realm ID/name/ports, resource policy, backup/restore plan, and linked RUN/VAL records. Defined instances may remain stopped.

Worldserver realms remain isolated by configuration, character database, world database, module manifest, backups, and lifecycle evidence. Shared authentication is permitted only under the approved multi-realm topology.

## Baseline

Purpose: a clean AzerothCore comparison realm without Blaine gameplay modules. It establishes reference behavior for a pinned baseline. “Clean” does not mean unversioned or automatically newest.

SRV-BASELINE-001 is first after architecture and environment approval. It has no custom modules, core modifications, or client modifications and remains control rather than being silently repurposed.

Directory: `instances/baseline/`

## Research

Purpose: isolated evaluation of existing modules on their documented or adapted reference baselines. Research realms may use different exact core versions and must not share mutable realm databases with integration candidates.

Research servers are introduced one at a time in user-approved order. Each records exact baselines, provenance, licensing, requirements, and independent build, runtime, and gameplay evidence.

Directory: `instances/research/`

## Development

Purpose: iterative construction and independent testing of original, ported, replacement, or independently implemented Blaine modules and interfaces.

Directory: `instances/development/`

## Integrated

Purpose: combined Blaine suite validation and, after later approval, the unified player-facing Blaine experience. Inclusion requires the integration-promotion procedure and a pinned combined manifest.

SRV-INTEGRATED-001 remains prohibited until required research and compatibility convergence are complete. It is a governed user-owned experience and may use separate attributable modules rather than one monolith.

Directory: `instances/integrated/`

## Disposable

Purpose: risky or short-lived experiments. Disposable means no continuity promise; it does not waive provenance, safety, target identity, command recording, data-isolation, or incident requirements.

Directory: `instances/disposable/`

## Realm menu and resource policy

Where technically feasible, compatible intended realms should appear through one shared client login endpoint and realm-selection menu with gameplay- or mod-identity labels. The Steam Deck may define every realm while running only a resource-appropriate subset. Client-modifying realms may require governed separate client profiles or installations.

## Realm Control target identity

Every future Realm Control request must resolve an explicit instance and realm target, even when that realm is stopped. Instance class does not grant permission. Addon, module, protocol, preset, and feature-package manifests remain instance-specific and NOT_IMPLEMENTED until validated.
