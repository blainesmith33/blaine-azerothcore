# Linux Implementation Path

Status: **Reserved for governed implementation**  
Lifecycle status: **S0 — Proposed**

This is the active path for future Steam Deck Linux implementation work. No Docker engine, package, AzerothCore source, build artifact, configuration, container, database, service, game-client file, or local realm is installed or operational here.

## Directory roles

- `source/`: future controlled source checkouts or source manifests.
- `config/`: future platform configuration with secrets excluded.
- `scripts/`: future reviewed automation.
- `compose/`: future container composition definitions if approved.
- `documentation/`: Linux-specific procedures and architecture details.
- `records/`: Linux-specific supporting records where root record classes do not apply.
- `manifests/`: future versions, checksums, and artifact inventories.
- `backups/`: future governed backup staging only after architecture approval.

## Constraints

The path resides on exFAT. Before Linux-native runtime workloads are placed here, the project must validate requirements involving permissions, ownership, executable behavior, symbolic links, case handling, locking, durability, database storage, container bind mounts, and backup semantics. ADR-0001 records this constraint.

Linux execution and validation evidence applies only to Linux. Modifying work requires authorization, a RUN record, and a VAL record appropriate to its acceptance criteria.
