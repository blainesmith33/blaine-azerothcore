# Lifecycle Acceptance Criteria

Status: **Authoritative**  
Current lifecycle status: **S0 — Proposed**

Lifecycle stages are cumulative governance gates, not informal progress labels. Advancement requires completed stage evidence and explicit human approval. Linux and Windows are evaluated independently wherever implementation evidence is platform-specific.

## S0 — Proposed

Acceptance criteria:

- Project purpose, scope, authority, evidence model, and platform separation are documented.
- Governed root structure and initial records exist.
- No implementation or operational capability is claimed.

Current result: **Active stage.** This structure satisfies only the proposal/governance baseline; it does not authorize advancement.

## S1 — Baseline Audited

Acceptance criteria:

- Steam Deck hardware, operating system, storage, filesystem, network, package, container, security, and resource baseline observations are recorded.
- Audit commands and outputs are preserved under RUN and audit records.
- Findings and constraints are validated in one or more VAL records.
- No unresolved identity or target ambiguity remains for planned changes.

## S2 — Architecture Approved

Acceptance criteria:

- Runtime, storage, database, networking, build, configuration, backup, recovery, security, and client-connectivity architecture is documented.
- Important choices, including treatment of exFAT constraints, have approved ADRs.
- Platform-specific boundaries and acceptance tests are defined.
- Explicit human architecture approval is recorded.

## S3 — Environment Prepared

Acceptance criteria:

- Approved prerequisites are installed and configured for the applicable platform.
- All modifying commands and versions are recorded.
- Environment, permissions, storage placement, and required services pass VAL checks.
- Preparation does not imply AzerothCore has been built.

## S4 — AzerothCore Built

Acceptance criteria:

- Identified source revision is acquired and built through the approved process.
- Build inputs, commands, versions, outputs, and failures are recorded.
- Build artifacts and manifests pass validation.
- Build success does not imply a server is operational.

## S5 — Server Operational

Acceptance criteria:

- Approved database and server components start with controlled configuration.
- Health, process, port, database, log, restart, and shutdown behavior is validated.
- Operational claims are limited to the validated platform and configuration.

## S6 — Client Connected

Acceptance criteria:

- An authorized game client is configured without distributing unauthorized client files.
- Authentication, realm selection, and in-world connection are executed and validated.
- Network and client evidence is sanitized for publication.

## S7 — Operations Validated

Acceptance criteria:

- Backup, restore, update, restart, failure handling, observability, data persistence, and routine operations are tested.
- Recovery evidence demonstrates the approved objectives.
- Known limitations and unresolved incidents are documented.

## S8 — Public Release Ready

Acceptance criteria:

- Public documentation is traceable to approved architecture, RUN, and VAL evidence.
- Commands and links are reviewed; examples are sanitized.
- Platform claims are separated and accurately scoped.
- Release manifest and release record are complete.
- Explicit human public-release approval is recorded.

## S0 amendment: multi-realm and module stage controls

The following controls refine later-stage evidence without advancing the current stage:

- **S0:** Multi-realm intent, instance classes, module provenance, exact version pinning, compatibility convergence, and modular-suite governance are documented as proposals and requirements only.
- **S1:** The baseline audit must gather resource, storage, network, database, client, toolchain, and platform facts needed to assess multiple realms and module research.
- **S2:** Approval must cover realm isolation, shared-login feasibility, instance manifests, database boundaries, client profiles, module interfaces, provenance/licensing, and the compatibility-convergence method. It need not select an integration commit before evidence exists.
- **S3:** Prepared environments must remain instance- and platform-specific, with exact toolchain and dependency evidence.
- **S4:** Build acceptance requires exact core/module commits and both individual and applicable combined clean-build evidence; individual success does not approve integrated use.
- **S5:** Operational acceptance must validate isolated realm identities, ports, configurations, databases, manifests, backups, and resource-appropriate concurrency.
- **S6:** Client acceptance must validate the intended shared realm menu where feasible and govern unified versus separate client assets.
- **S7:** Operations acceptance must include combined upgrade, migration, removal, rollback, conflict, regression, backup, and restore testing.
- **S8:** Release readiness requires exact manifests, provenance and license inventories, platform-scoped claims, and evidence for every advertised module and realm behavior.

No module, feature, realm, shared authentication design, client profile, or integration baseline is approved by this S0 amendment.

## S0 amendment: staging and promotion sequence controls

- Temporary staging structure may exist at S0 without constituting environment preparation.
- Staging is non-authoritative, contains no live runtime state, and does not satisfy S3.
- S2 must resolve permanent runtime placement before trust-bearing use.
- S4 evidence begins with the clean unmodded baseline; research builds remain independent scopes.
- Research order requires explicit user approval and sequential execution.
- Integrated claims require research evidence, extracted requirements, compatibility convergence, and combined validation.
- No scaffold, manifest, or placeholder advances the project beyond S0.

## Advancement rule

This project remains **S0 — Proposed**. No criterion in S1 through S8 is claimed complete by creation of these documents.
