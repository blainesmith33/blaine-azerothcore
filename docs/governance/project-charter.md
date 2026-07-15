# Project Charter

Status: **Authoritative**  
Lifecycle status: **S0 — Proposed**

## Purpose

Establish a governed, auditable, and reproducible reference implementation for running AzerothCore in a Steam Deck context, beginning with a Linux implementation and reserving a separate Windows path.

## Scope

- Governance, requirements, architecture, procedures, execution evidence, validation evidence, and release controls.
- A future Steam Deck Linux implementation under `linux/`.
- A separately governed future Windows implementation under `windows/`.
- Reproducible, sanitized public documentation derived from verified project evidence.
- Identification and later validation of storage, container, build, runtime, database, networking, client, backup, and operational requirements.
- Governance of multiple isolated AzerothCore instances intended for baseline, research, Blaine development, integrated gameplay, and disposable experiments.
- Evaluation, provenance, compatibility convergence, adaptation, replacement, porting, or independent implementation of modular gameplay features.
- Governed future internet accessibility for authorized players, with public gameplay separated from authenticated management, private internal, host-local, and prohibited-direct service boundaries.

## Out of scope at S0

- Installing or configuring Docker, AzerothCore, packages, services, containers, databases, firewall rules, or game-client files.
- Building or operating an AzerothCore server or local realm.
- Claiming successful client connectivity or operational readiness.
- Treating Linux validation as Windows validation.
- Publishing secrets, credentials, private keys, tokens, personal data, or non-public network details.

## Intended Steam Deck Linux implementation

Future authorized work will be developed under `/run/media/deck/blAIneXPLAT/azerothcore/linux/`. Architecture and audit work must precede environment preparation. The exFAT project filesystem is an explicit constraint: Linux-native runtime workloads, permissions, links, database storage, and other filesystem-sensitive behavior require validation before placement on exFAT.

## S0 amendment: realm and Blaine integration objective

The intended future topology provides isolated worldserver realms for different purposes and, where technically feasible, one shared client login and realm-selection experience. Shared authentication is a candidate architecture, not an operational claim. The Steam Deck may define more realms than it runs concurrently.

The Blaine-integrated realm targets one coherent player experience delivered as a governed suite or distribution. Separate modules are preserved where appropriate, with shared interfaces, configuration conventions, dependency ordering, exact combined manifests, provenance, licensing, and individual plus combined validation. The integration core is chosen by compatibility convergence rather than by automatically selecting the newest AzerothCore release.

## Reserved Windows implementation

`/run/media/deck/blAIneXPLAT/azerothcore/windows/` reserves a distinct implementation path. It may share root governance and requirements, but it requires its own procedures, execution evidence, and validation. No Windows implementation is started by this charter.

## Temporary staging and ordered server work

The non-authoritative workspace at /run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/ may prepare portable templates and manifests while the permanent Linux-native target remains undecided. It contains no approved runtime, source, database, container, client, or secret state.

After later architecture and environment approval, the clean unmodded baseline is implemented first and preserved as control. Requested research servers follow one at a time in user-approved order. Integrated Blaine evaluation begins only after required research, requirements extraction, and compatibility convergence.

## Reproducibility objective

A technically capable third party should eventually be able to reproduce validated results from authoritative Markdown, versioned sources, sanitized configuration, manifests, recorded commands, and acceptance evidence. Unrecorded assumptions are not reproducibility evidence.

## Human approval and Codex execution model

The human project owner approves scope, architecture, consequential changes, stage transitions, and publication. Codex performs explicitly authorized work, records exact commands and observed results, validates within scope, and reports failures without omission. Execution evidence informs approval but does not replace it.

## Governance hierarchy

1. Charter and governance rules.
2. Requirements and approved ADRs.
3. Platform procedures and controlled configuration.
4. RUN execution records.
5. VAL validation records.
6. Public documentation and derivative exports.

Higher layers authorize lower layers. Public claims must be traceable back through this hierarchy.

## Evidence requirements

Technical claims require at least one appropriate evidence form:

- exact recorded commands and exit status;
- observed system state;
- a validation record with acceptance outcome; or
- an explicitly identified authoritative source.

Important architecture choices require ADRs. Validation activities require VAL records. Failed and materially unexpected outcomes remain visible in RUN or incident records.

## Public-release objective

The project may become a reproducible public reference implementation only after release governance is satisfied. Public material must be sanitized, evidence-backed, internally consistent, and downstream of validated implementation records.

## Current status

The project is **S0 — Proposed**. Governance structure may exist at S0; no implementation component is claimed installed, built, connected, or operational.

## Subordinate control-plane and package scope

blAIne is the broader umbrella orchestration and AI-systems-development identity. This project, blAIne Realm Control, and governed full-stack feature packages are subordinate project components. The project may plan original tooling, modules, addons, control software, automation, integration, documentation, training, and support, but does not provide hosted realms, sell realm access, or distribute Blizzard-owned client or game assets. This project-specific boundary is not legal advice and does not redefine blAIne.
