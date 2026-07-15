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
- Internet-accessible gameplay for authorized remote players through a later governed ingress design
- **blAIne Realm Control**, initially host-local, with any future remote access constrained by a separate authenticated management boundary

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

A strict host-local listener-shape validation remains unresolved because the rootless `pasta` forwarding socket may appear broader than the loopback address Docker was instructed to publish. This result applies to validation of host-local bindings; it does not select or disqualify a future public-ingress architecture.

No public ingress is currently implemented or validated.

Docker Buildx v0.35.0 is installed as a checksum-verified, user-scoped CLI plugin on internal storage. Native Linux AMD64 `FROM scratch` builds passed both directly and through Docker Compose using the builder associated with the active `rootless` context. Docker remains rootless with `fuse-overlayfs`; no extra builder, QEMU, privileged helper, registry push, port publication, or multi-platform support was introduced. Buildx updates remain manual governed operations.

Official AzerothCore WotLK source objects have now been acquired as `SRC-ACORE-WOTLK-001` from `azerothcore/azerothcore-wotlk` `master` and pinned to `34a8bd6655c02448d3da6195dcd00647f634dde3` (tree `25ad25f5fb8e119f68fef69b080934f1182ad8d6`). The full-history upstream clone is governed separately beneath `linux/upstream/azerothcore-wotlk/` and is ignored by this parent repository.

Git object integrity and deterministic tree identity passed. Checkout fidelity did not: the pinned tree contains one symbolic link that the authoritative exFAT project filesystem cannot represent. The verified clone therefore remains no-checkout object storage, not a detached build-ready working tree. No source file has been modified, no module is installed, no image has been built or pulled, no database or client data has been acquired, and no server has started. Buildx remains established but has not been used for AzerothCore.

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

### Constrained control

blAIne Realm Control must not expose the Docker socket directly to a browser or provide unrestricted host-level access. Any future remote management must use authenticated, encrypted, brokered, authorized, logged, and auditable mechanisms.

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

**blAIne Realm Control** is the provisional name for the planned observability and control layer. Its initial deployment profile is host-local. Remote access may be considered later only through a separately approved authenticated management architecture.

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

## Network Exposure Architecture

Authorized players are expected eventually to connect from outside the local network. Public internet gameplay access is therefore a required future deployment capability, but it is not currently implemented, configured, or validated.

Every networked service must eventually receive one approved exposure classification per deployment profile. The provisional categories are:

- public gameplay, limited to the minimum explicitly approved player-facing services;
- authenticated remote management, separated from gameplay and protected by encrypted, brokered access;
- private internal services, including databases and container-to-container control traffic;
- host-local services, including development diagnostics and disposable validation listeners; and
- prohibited-direct services, including Docker control sockets, raw database access, backup interfaces, credential stores, and unrestricted command execution.

Exact gameplay ports, bind addresses, firewall policy, and ingress methods remain undecided. They must be derived later from pinned AzerothCore configuration and approved deployment manifests. No current inbound capability, router control, address type, or ISP behavior is assumed.

The future blAIne Realm Control interface may support authorized remote administrators only through an authenticated, encrypted, constrained, and auditable management path. A browser must never receive direct Docker-socket or database-root access. External internet validation is a separate future milestone after a clean host-local baseline and an ingress ADR.

## Current Docker Boundary

The current rootless Docker environment uses:

- Rootless Docker
- `fuse-overlayfs`
- `pasta` networking
- Internal-storage-backed Docker data
- No rootful Docker daemon
- No Docker TCP API listener

Container execution, DNS, HTTPS transport, resource limits, Compose execution, and cleanup have passed validation.

Buildx uses the context-derived `rootless` Docker-driver builder for current native Linux AMD64 builds. The unused builder entry named `default` belongs to the unavailable rootful context and is not required. The plugin is pinned at v0.35.0 under `/home/deck/.docker/cli-plugins/docker-buildx`; its update and rollback policy is recorded in ADR-0013 and `docs/tooling/docker-buildx-user-plugin-management.md`.

The unresolved issue is the listener representation used by `pasta` for loopback-requested publication. It remains relevant to `NET-HOST-LOCAL` validation. It does not prove public reachability and does not determine the final public gameplay ingress path. No AzerothCore port is approved for exposure by the current Docker records.

## Planned Execution Sequence

1. Established: record the internet-access and trust-boundary architecture.
2. Established: install and validate Docker Buildx for native rootless builds.
3. Established with split result: acquire and pin the official AzerothCore source objects for the clean unmodified baseline; checkout fidelity remains deferred on exFAT.
4. Next: approve a symlink-faithful source working-tree location or mechanism and complete detached-checkout validation.
5. Design and execute the clean unmodified baseline image build from the pinned source only after checkout fidelity passes.
6. Initialize databases only after build validation.
7. Start authserver and worldserver only after database and configuration validation.
8. Validate host-local operation.
9. Determine available inbound-connectivity conditions.
10. Select an ingress architecture through a later ADR.
11. Implement only the approved public gameplay exposure.
12. Validate from an independent internet connection.
13. Validate that private and prohibited services remain inaccessible.
14. Introduce authenticated remote management only after its own architecture and threat-model approval.

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
