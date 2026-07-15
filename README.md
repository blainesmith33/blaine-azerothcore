# blAIne AzerothCore

Governed multi-realm AzerothCore development, client compatibility, documentation, and blAIne Realm Control.

## Project Status

**Lifecycle:** S0 — Proposed

This repository defines and governs a planned AzerothCore development environment under the broader blAIne umbrella.

The project is intended to support:

- A clean, reproducible AzerothCore baseline
- Multiple independently managed realms
- Realm-specific server and client compatibility
- Server modules, client addons, and client patches
- Governed full-stack feature packages
- A structured World of Warcraft documentation corpus
- **blAIne Realm Control**, a local-first observability and control layer

This README describes the intended architecture and the current verified state. It does not imply that every described component has already been implemented.

## Current Verified State

The project currently includes governance records, architecture decisions, manifests, templates, and staging scaffolding.

Rootless Docker is installed and can run ordinary containers using:

- Rootless Docker Engine
- `fuse-overlayfs`
- Docker Compose
- CPU, memory, and PID limits
- Internal-storage-backed Docker data

The Docker storage remediation has passed.

A strict network-boundary validation remains unresolved because the rootless `pasta` forwarding socket may appear as `0.0.0.0` even when Docker is instructed to publish only to `127.0.0.1`.

Docker Buildx is not yet installed.

No AzerothCore source tree, databases, custom modules, client modifications, ChromieCraft client, or operational Realm Control software is currently included in this repository.

## Project Principles

### Governance before execution

Infrastructure changes, acquisitions, builds, and validations should be performed through bounded, reviewable operations.

### Evidence before claims

A component must not be described as installed, operational, compatible, or validated without supporting evidence.

### Clean baseline first

The first AzerothCore environment must be a clean, unmodified Wrath of the Lich King 3.3.5a baseline built from an exact pinned AzerothCore commit.

### Explicit compatibility

Every realm must identify the exact server, database, module, client, addon, patch, and protocol requirements needed for operation.

### Server authority

The server remains authoritative for protected gameplay state. Client and addon messages must be treated as untrusted requests.

### Local-first control

blAIne Realm Control must not expose the Docker socket directly to a browser or provide unrestricted host-level access.

### Legal and provenance boundaries

Game clients, Blizzard-owned assets, and other restricted materials must not be committed to public Git repositories or treated as redistributable project artifacts.

## Conceptual Architecture

```text
blAIne
└── AzerothCore Development Project
    ├── Clean AzerothCore Baseline
    ├── Multi-Realm Server Environment
    ├── Research and Development Realms
    ├── Server Modules
    ├── Client Addons
    ├── Realm-Specific Client Variants
    ├── Full-Stack Feature Packages
    ├── WoW Documentation Corpus
    └── blAIne Realm Control
```

## Planned Repository Structure

```text
.
├── governance/
├── records/
│   ├── audits/
│   ├── decisions/
│   ├── executions/
│   └── validations/
├── documentation/
│   └── source-corpus/
├── infrastructure/
├── servers/
├── clients/
├── feature-packages/
├── realm-control/
├── scripts/
└── README.md
```

The final structure may evolve through governed architecture decisions.

## Multi-Realm Model

Each worldserver should remain independently identifiable and isolatable.

Every realm should eventually record:

- Realm ID and name
- Network port
- Configuration
- World database
- Characters database
- AzerothCore commit
- Module manifest
- Server build record
- Backup set
- Logs
- Required client identifier
- Required addon profile
- Required protocol version
- Validation evidence

Planned instance classes include:

- Baseline
- Research
- Development
- Integrated
- Disposable

Initial planned server identifiers include:

- `SRV-BASELINE-001`
- `SRV-RESEARCH-001`
- `SRV-INTEGRATED-001`

These identifiers are provisional and do not represent currently running servers.

## Client Compatibility Model

The project does not assume that every realm can use the same client installation.

A realm deployment may require a specific combination of:

- AzerothCore core commit
- Server modules and module commits
- Database revision
- Client baseline
- Client executable modifications
- MPQ or patch files
- Modified DBC or data files
- Addon profile
- Client/server protocol version
- Realm Control definitions
- Validation evidence

Compatibility should be classified as:

- Connection-compatible
- Feature-compatible
- Protocol-compatible
- Asset-compatible
- Fully validated

Planned client identifiers include:

- `CLT-BASE-335A-001`
- `CLT-CHROMIECRAFT-001`
- Realm-specific derived client identifiers

## Clean Baseline Requirements

The first operational environment should be:

- AzerothCore-based
- Wrath of the Lich King 3.3.5a
- Built from an exact pinned commit
- Free of custom modules
- Free of custom core changes
- Free of custom client modifications

The baseline should establish:

- Reproducible builds
- Database initialization
- Authserver operation
- Worldserver operation
- Local network behavior
- Matching-client connection
- Performance baseline
- Comparison point for later modifications

Custom modules and specialized realms should be introduced only after the baseline is validated.

## blAIne Realm Control

**blAIne Realm Control** is the provisional name for the planned local-first observability and control layer.

Initial maturity target:

**RC-S1 — Read-only observatory**

Planned capabilities include:

- Realm health
- Authserver and worldserver health
- Database health
- Container health
- Logs
- Player count
- Uptime
- Build state
- Core and module commits
- Client compatibility requirements
- Backup state
- Resource use
- Realm commands
- Governed presets
- Structured database operations
- Later command execution through a broker

Nothing operational has yet been implemented.

## Full-Stack Feature Packages

A future gameplay feature may contain:

```text
Feature Package
├── Server Module
├── Optional Client Addon
├── Optional Client Patch
├── Realm Control Integration
├── Protocol Contract
├── Database Migrations
├── Configuration
├── Presets
├── Documentation
└── Tests
```

This structure is intended to keep server, client, protocol, configuration, and validation requirements bound together.

## Documentation Corpus

The project plans to maintain a governed World of Warcraft and AzerothCore documentation corpus.

Planned source classes include:

- Historical Blizzard manuals
- Blizzard legal and policy material
- WoW addon-development policy
- Blizzard Developer API documentation
- Official support material
- Official patch information
- Relevant Blizzard-authored forum posts
- Exact-client extracted interface sources
- Official AzerothCore documentation
- Official ChromieCraft documentation
- Carefully classified community research

Each acquired item should record:

- Publisher
- Official or nonofficial classification
- Author
- Original URL
- Final retrieval URL
- Retrieval timestamp
- Publication or effective date
- Title
- Document type
- Language
- File size
- SHA-256
- MIME type
- Relevant client or game version
- Current or historical classification
- Copyright or license notice
- Redistribution status
- Git eligibility
- Supersession relationship
- Technical applicability

The corpus is intended to preserve provenance and applicability, not merely collect files.

## Current Docker Boundary

The current rootless Docker environment uses:

- Rootless Docker
- `fuse-overlayfs`
- `pasta` networking
- Internal-storage-backed Docker data
- No rootful Docker daemon
- No Docker TCP API listener

Container execution, DNS, HTTPS transport, resource limits, Compose execution, and cleanup have passed validation.

The unresolved issue is the listener representation used by `pasta` for loopback-requested port publication. AzerothCore ports must not be exposed to the LAN until that boundary is resolved or explicitly accepted through an architecture decision.

## Planned Execution Sequence

1. Resolve or formally defer the rootless Docker port-publication boundary.
2. Install and validate Docker Buildx.
3. Formalize server/client compatibility architecture.
4. Formalize documentation-corpus architecture and provenance rules.
5. Acquire the official documentation corpus.
6. Acquire and inventory the official ChromieCraft-compatible client.
7. Acquire the official AzerothCore source repository.
8. Pin an exact AzerothCore commit.
9. Build the clean unmodified baseline.
10. Initialize databases.
11. Start authserver and worldserver locally.
12. Validate connection with the matching clean client.
13. Begin module research and specialized realm development.

## Repository Boundaries

This repository may contain:

- Governance records
- Architecture decisions
- Validation records
- Infrastructure definitions
- Scripts
- Manifests
- Documentation written for this project
- Metadata and checksums for restricted external assets
- Source code that may legally be stored and distributed

This repository must not contain:

- World of Warcraft client installations
- Blizzard-owned game assets
- Credentials or secrets
- Private recovery data
- Unlicensed redistributed material
- Unreviewed binary patches of uncertain provenance

## Development Method

Project work is performed one bounded operation at a time.

Each significant operation should:

1. Define scope.
2. Identify authorized paths.
3. Preserve the prior state.
4. Execute only the approved action.
5. Capture evidence.
6. Record failures and warnings.
7. Validate the result.
8. Update governance records.
9. Avoid lifecycle advancement without explicit approval.

## Naming

- **Repository:** `blaine-azerothcore`
- **Project:** blAIne AzerothCore Development Project
- **Control layer:** blAIne Realm Control
- **Control-layer abbreviation:** BRC

## License

No project-wide license has been selected yet.

Third-party source code, documentation, client materials, and other external assets remain subject to their respective licenses, terms, and distribution restrictions.

## Disclaimer

This is an independent development project. It is not affiliated with, endorsed by, or sponsored by Blizzard Entertainment, AzerothCore, or ChromieCraft.

World of Warcraft, Warcraft, Blizzard, and related names and assets are trademarks or property of their respective owners.
