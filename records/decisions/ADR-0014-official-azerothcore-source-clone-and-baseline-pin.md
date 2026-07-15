# ADR-0014: Official AzerothCore Source Clone and Baseline Pin

- Status: Accepted at S0 — Proposed; checkout implementation deferred by filesystem fidelity gate
- Date: 2026-07-14
- Scope: Clean unmodified AzerothCore WotLK source identity and custody
- Related decisions: `ADR-0010`, `ADR-0011`, `ADR-0013`
- Related evidence: `SRC-ACORE-WOTLK-001`, `RUN-0014`, `VAL-0014`

## Context

The clean baseline requires an exact, reviewable AzerothCore source identity before any image build, database initialization, client-data acquisition, or server startup. The governance repository must publish provenance without vendoring a large independently maintained upstream Git repository or its database.

At acquisition, official `azerothcore/azerothcore-wotlk` `master` resolved twice to `34a8bd6655c02448d3da6195dcd00647f634dde3`. The full-history object repository passed strict verification. Its pinned tree contains one symbolic-link entry, `.claude/skills/generate-pr-description`, targeting `../../.agents/skills/generate-pr-description`. The authoritative exFAT mount rejects symbolic-link creation. A faithful working tree therefore cannot be approved at the present physical path.

## Decision

Acquire the official `azerothcore/azerothcore-wotlk` `master` branch as a full-history, single-branch upstream clone; pin `SRC-ACORE-WOTLK-001` to the exact acquisition-time commit; exclude the physical clone from the blAIne governance repository; and preserve source identity through a tracked manifest, commit/tree identifiers, deterministic archive digest, and validation evidence.

The approved source identity is:

| Property | Decision |
|---|---|
| Source | `https://github.com/azerothcore/azerothcore-wotlk.git` |
| Branch strategy | Full-history, single-branch `master` |
| Pin | `34a8bd6655c02448d3da6195dcd00647f634dde3` |
| Tree | `25ad25f5fb8e119f68fef69b080934f1182ad8d6` |
| Identifier | `SRC-ACORE-WOTLK-001` |
| Physical object clone | `linux/upstream/azerothcore-wotlk/` |
| Parent Git eligibility | Ignored; never vendored or staged |
| Intended checkout state | Detached HEAD at the pin, clean and faithful |
| Current checkout state | No checkout; deferred because exFAT cannot represent the pinned symlink |

The no-checkout object repository is retained as verified provenance. It is not a build-ready source tree. A later ADR or bounded operation must approve a symlink-capable source location or another demonstrably faithful checkout mechanism before detached checkout and build preparation can proceed.

## Rationale and alternatives

- A shallow clone is rejected because it weakens ancestry, update, and provenance review.
- An archive acquisition is rejected because it omits Git history and object relationships.
- Vendoring is rejected because it would mix upstream source with governance history and make accidental publication likely.
- A Git submodule is not introduced because the physical clone is governed independently and no parent-repository source dependency is yet approved.
- Forcing checkout on exFAT is rejected because materializing the symlink as an ordinary file would not faithfully represent the pinned tree.
- Building directly from the no-checkout object repository is rejected because required source paths are absent.

## Clean-baseline and update policy

The pinned baseline must not be edited, patched, formatted, generated into, or supplied with custom modules. An update must resolve official `master` again, review the old-to-new commit range, repeat object, license, LFS, Gitlink, case, path, symlink, build-definition, acquisition-behavior, and deterministic-digest validation, and update the manifest through a separate governed decision. Moving `origin/master` does not silently move the approved pin.

Future custom work must use separately governed branches, modules, forks, or repositories. Upstream contributions require their own fork/branch and contribution review; they must not mutate this evidence clone.

## Build and runtime relationship

ADR-0013 establishes the checksum-verified Buildx v0.35.0 plugin and the existing rootless Docker-driver builder. That prerequisite remains intact, but it does not overcome source-filesystem fidelity. A future build must use a faithful source checkout, review the upstream Docker/Compose acquisition behavior, and govern the exact Buildx/Compose invocation. No Docker image, database, client data, module, container, port, authserver, or worldserver is authorized by this ADR.

## License obligations

The root `LICENSE` blob `ecbc0593737be657aef92e3adcf3bcbd6fdb812e` contains GNU GPL version 2 text. Future distribution, modification, images, and derivative work must preserve applicable GPLv2 and bundled dependency-license obligations. This record is an inventory, not legal advice.

## Rollback and removal

If separately authorized, removal may delete only the ignored clone at `linux/upstream/azerothcore-wotlk/` after confirming its origin, identifier, and absence of unique modifications. Tracked manifest and validation evidence are preserved. No automatic removal is approved, and no source object is pushed upstream.

## Deferred decisions and lifecycle

Deferred: faithful checkout placement, detached-HEAD completion, build invocation, cache placement, image set, database initialization, client-data handling, runtime configuration, and service/network exposure. The lifecycle remains **S0 — Proposed**.
