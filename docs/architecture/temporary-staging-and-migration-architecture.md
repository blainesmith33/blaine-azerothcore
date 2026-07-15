# Temporary Staging and Migration Architecture

Status: Authoritative S0 proposal  
Lifecycle: S0 — Proposed  
Decision authority: ADR-0005

## Authority split

The authoritative governance and evidence root remains /run/media/deck/blAIneXPLAT/azerothcore/. The workspace at /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/ is temporary implementation preparation and cannot supersede authority at the governance root.

## Staging role and boundaries

Staging may hold path-portable structure, inactive instance definitions, empty module workspaces, non-secret templates, scripts, manifests, and migration controls. It does not authorize acquired source, builds, live databases, container storage, client assets, credentials, sockets, services, or realms.

The permanent Linux-native target is undecided. Source, build, runtime, database, container-data, client-data, backup, export, and log destinations remain NOT_YET_SELECTED and must not be created from that marker.

## Path indirection

Environment-specific paths are centralized in environment/paths.env.example. Scripts canonicalize explicit roots, reject missing or unresolved paths, and enforce containment. Repeated embedded environment paths are prohibited.

## Migration and integrity

Migration requires a later approved target, recorded identities, quiesced state, exact source locks, exclusions, command evidence, SHA-256 manifests before and after transfer, and VAL acceptance. The staging drive must not hold the only copy of unique data.

Live database files are not copied as migration. Approved, version-aware database export and import procedures are required. Source and modules require exact full commit SHAs; moving labels are context, not validated pins.

## Filesystem and reliability boundary

Staging is ext4, but AUD-0002 records historical USB/UAS disconnect and I/O instability. Staging is not presumed suitable for trusted runtime, database, container, build, socket, or unique-data workloads. Those uses require separate storage and reliability validation.

