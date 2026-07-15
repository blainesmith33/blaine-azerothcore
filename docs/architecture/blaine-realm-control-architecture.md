# blAIne Realm Control Architecture

- Authority: Proposed architecture
- Project lifecycle: S0 — Proposed
- Component maturity: RC-S0 — Proposed architecture
- Implementation status: NOT_IMPLEMENTED

blAIne is the broader umbrella orchestration and AI-systems-development identity. The AzerothCore development project and its provisional blAIne Realm Control component are subordinate to blAIne.

```text
blAIne
└── AzerothCore development project
    ├── Multi-realm server environment
    ├── Server modules
    ├── Client addons
    ├── Full-stack feature packages
    └── blAIne Realm Control
```

## Separation and trust boundary

Realm Control is a separate local-first control plane, not a process embedded in a worldserver container. It must remain capable of reporting when any realm is stopped or unhealthy. A visual client communicates with a governed backend; it receives no unrestricted Docker socket, database-root, AzerothCore-administrator, host-root, or filesystem-write access.

The backend separates realm, container, database, and host contexts. Read-only observability precedes any change operation. Raw command access is disabled by default, and any future non-root host shell and privilege elevation require distinct authorization controls.

## Planned capabilities

Every capability below is NOT_IMPLEMENTED and NOT_YET_VALIDATED:

- Multi-realm status overview
- Authserver, worldserver, database, and container health
- Player count and uptime
- Live logs
- Start, stop, and restart
- Build and compilation status
- Exact core and module commit display
- Module and addon manifest display
- Database revision, size, migration, backup, and restore status
- CPU, memory, disk, temperature, and container usage
- Realm IDs, names, ports, and availability
- Incident and validation records
- Realm command execution
- Container command execution
- Structured database operations
- Optional raw SQL console
- Optional host terminal
- Preprogrammed visual server commands
- Reusable presets and command sequences
- Experiment-to-permanent-feature promotion

## Maturity

RC-S0 through RC-S6 are defined by ADR-0007. They track the component only and never replace S0 through S8 project lifecycle controls. No remote internet exposure is approved.

