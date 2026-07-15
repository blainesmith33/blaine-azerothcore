# ADR-0006 — Baseline-First Sequential Research and Integration

Status: Accepted as S0 progression architecture  
Date: 2026-07-14  
Lifecycle: S0 — Proposed

## Context

Multiple gameplay experiences will be studied before a coherent Blaine realm is evaluated. Individual module builds cannot establish combined compatibility or define desired gameplay. A stable control and ordered evidence process are required.

## Decision

- Implement and validate a clean unmodded baseline first after later approvals.
- Preserve it as the control realm.
- Build requested research servers one at a time in user-selected order.
- Validate each independently on exact appropriate baselines.
- Extract desired, changed, and rejected mechanics into requirements.
- Begin integrated evaluation only after required research and compatibility convergence.
- Treat the final Blaine realm as a governed user-owned experience rather than a pile of third-party modules.
- Permit separately attributable, testable modules instead of requiring one monolith.

## Consequences

Research success does not promote a feature automatically. Integration requires provenance, licensing, exact pins, compatibility, combined build, database, runtime, gameplay, conflict, removal, rollback, upgrade, and approval evidence.

No server, commit, module, realm identity, port, database, client, or runtime is approved by this decision.

