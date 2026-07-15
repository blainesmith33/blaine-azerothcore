# Server Progression and Promotion Architecture

Status: Authoritative S0 proposal  
Lifecycle: S0 — Proposed  
Decision authority: ADR-0006

## Ordered progression

1. Approve architecture and prepare the environment through later governed stages.
2. Build and validate SRV-BASELINE-001 as a clean, unmodded AzerothCore WotLK 3.3.5a control.
3. Preserve the validated baseline without silent repurposing.
4. Create requested research servers one at a time in a user-approved order.
5. Validate each independently on its appropriate exact core and module baseline.
6. Extract behavior to reproduce, retain, change, and reject into requirements.
7. Develop or adapt independently attributable Blaine systems.
8. Perform compatibility convergence after required research evidence exists.
9. Evaluate integrated Blaine behavior through combined promotion gates.

## Control and research

The baseline has no custom modules, core modifications, or client modifications. Any incompatible change requires a new instance identity and decision rather than mutation of the control.

Research evidence is isolated. Success validates only the exact core, modules, dependencies, client, database, platform, and tests recorded for that server. It neither approves integration nor selects the integrated baseline.

## Blaine integration

The final target is a governed user-owned Blaine gameplay experience, not a pile of installed third-party modules. Systems may be original, adapted, replaced, ported, or independently implemented. They may remain separate modules with explicit interfaces and ordering.

Promotion requires exact manifests and provenance, licensing, compatibility, combined build, database, startup, shutdown, runtime, gameplay, regression, conflict, removal, rollback, upgrade, and approval evidence.

All instances remain disabled and PLANNED_NOT_IMPLEMENTED. No server, commit, module, client, database, port, realm identity, or runtime is approved here.

