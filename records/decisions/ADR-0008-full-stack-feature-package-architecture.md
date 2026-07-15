# ADR-0008: Full-Stack Feature Package Architecture

- Status: Accepted at S0 — Proposed
- Date: 2026-07-14
- Scope: AzerothCore development project

## Context

Original blAIne gameplay capabilities may span server, client, control-plane, data, configuration, and validation surfaces. Treating only one surface as the feature obscures compatibility and provenance.

## Decision

A governed full-stack feature package may contain a server module, client addon, blAIne Realm Control integration, shared communication contract, presets, database migrations, configuration, documentation, and tests.

A package may be module-only, addon-only, paired module/addon, module/Realm-Control, addon/Realm-Control, or full-stack. Separate components remain independently attributable, versioned, testable, and replaceable.

The server remains authoritative for protected gameplay state. Client addons may request operations but are not trusted authorities. A server module must validate identity, permissions, targets, ranges, compatibility, and current state. blAIne Realm Control may invoke the same governed server operation from outside the game.

Protocol versions must be explicit. Module, addon, control-definition, protocol, preset, and migration versions must be independently recorded. Successful experiments may later be converted into reproducible module code, configuration, SQL migrations, feature-package presets, or documented administrative procedures, but only after validation and promotion.

Existing third-party modules and addons remain upstream assets and may not be represented as original blAIne work. Original work is not created merely by renaming third-party code. Provenance and license obligations remain attached to every acquired or adapted component.

## Consequences

Package-level validation must cover component compatibility and coordinated behavior. This decision authorizes only schemas and documentation; it validates no implementation, protocol, hook, command, or gameplay behavior.

