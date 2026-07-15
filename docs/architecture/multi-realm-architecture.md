# Multi-Realm Architecture

Status: **Authoritative S0 proposal; not implemented or operational**  
Current lifecycle status: **S0 — Proposed**  
Decision: **ADR-0002**

## Intended topology

Where technically feasible and validated, one shared game-client login endpoint presents all intended compatible realms through one realm-selection menu. One shared authentication service and authentication database may serve those realms when technically appropriate, supportable, and validated. Multiple isolated `worldserver` realms provide distinct gameplay experiences.

This is an intended architecture, not evidence that a shared login, authentication service, database, realm list, worldserver, or client is installed or operational.

## Realm isolation

Every realm must have a unique and recorded:

- realm ID and player-facing realm name;
- network port allocation;
- worldserver configuration identity;
- character database;
- world database, unless a later ADR and migration validation explicitly authorize sharing;
- source and module manifest with exact commits;
- dependency and database-revision manifest;
- backup and restore set;
- lifecycle, execution, validation, incident, and release records.

Authentication data may be shared only through the approved topology. Realm-specific character, world, module, and configuration state remains isolated so research, migration, rollback, and failure do not silently affect another realm.

## Realm classes and labels

Realm names must communicate gameplay experience or mod identity rather than relying on an internal environment label alone. Intended classes are:

- a clean baseline realm;
- research realms for existing modules on their reference baselines;
- a development realm for Blaine modules;
- an integrated Blaine realm;
- disposable realms for risky, short-lived experiments.

The authoritative class controls are in `docs/instances/instance-classification.md`.

## Resource scheduling

The Steam Deck may define all governed realms while running only a resource-appropriate subset simultaneously. Defined does not mean running. Capacity, contention, startup ordering, and concurrent-realm limits require later audit and validation.

## Version coexistence

Research and transition realms may temporarily use different exact AzerothCore versions. Each instance manifest identifies its core, modules, database state, configuration, and client requirements. Cross-version databases are not assumed interchangeable.

## Client compatibility boundary

Modules that modify the client may require one governed unified client-asset set or separate client installations. Client assets, licenses, version requirements, patch conflicts, and realm compatibility are evaluated separately. A realm must not be advertised in a shared menu as usable by a given client profile until connection and gameplay compatibility are validated.

## Platform boundary

Linux and Windows implementations may share this intended logical model, but topology, ports, paths, services, performance, and client behavior require separate platform validation.

## Proposed control-plane relationship

blAIne Realm Control is planned as a separate local-first control plane, not a process embedded in a worldserver. It must be able to report a realm as stopped or unhealthy without sharing that realm's failure boundary. Realm, container, database, and host command contexts remain distinct, and the first control-plane implementation stage is read-only. These capabilities are NOT_IMPLEMENTED.
