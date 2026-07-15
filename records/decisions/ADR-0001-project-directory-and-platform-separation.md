# ADR-0001: Project Directory and Platform Separation

Status: **Accepted for S0 structure**  
Date: **2026-07-14**

## Context

The project needs a stable authoritative root on the cross-platform `blAIneXPLAT` drive, a current Steam Deck Linux implementation path, a reserved Windows path, and common governance suitable for a future public reference implementation. The drive uses exFAT, whose behavior differs from Linux-native filesystems in areas potentially relevant to source trees, builds, databases, containers, permissions, and operations.

## Decision

1. Use `/run/media/deck/blAIneXPLAT/azerothcore/` as the authoritative project root.
2. Use `linux/` and `windows/` as separate implementation paths.
3. Perform current and near-term implementation work under `linux/`, subject to later approval and lifecycle gates.
4. Keep shared governance, requirements, architecture, root records, and public-release material at the project root.
5. Treat exFAT as an architectural constraint. Before placing Linux-native runtime workloads on it, explicitly validate filesystem-sensitive requirements and record any placement decision in a later ADR.

## Rationale

Platform separation prevents evidence from one operating system from being misapplied to another. Root-level governance avoids duplicated authority. Recording exFAT as a constraint prevents the convenient project location from being mistaken for a validated runtime location.

## Consequences

- Linux and Windows require independent procedures and validation.
- Shared policy may be referenced by both paths, but platform evidence remains scoped.
- Future architecture must decide which workloads can safely reside on exFAT and which require Linux-native storage.
- Public material must identify the validated platform.

## Alternatives not selected

- A single mixed implementation directory: rejected because it obscures platform authority and evidence.
- Treating the project drive as automatically suitable for all runtime data: rejected pending validation.
- Duplicating governance inside both platform trees: rejected because it risks competing current authorities.

## Validation

`VAL-0001` verifies directory separation and the initial governed structure only. It does not validate exFAT for runtime use.
