# Official AzerothCore Upstream Source Management

Status: Authoritative policy at **S0 — Proposed**

Source identifier: `SRC-ACORE-WOTLK-001`

## Source boundary

- Organization/repository: `azerothcore/azerothcore-wotlk`
- Official clone URL: `https://github.com/azerothcore/azerothcore-wotlk.git`
- Official branch: `master`
- Object-clone path: `/run/media/deck/blAIneXPLAT/azerothcore/linux/upstream/azerothcore-wotlk/`
- Parent boundary: the exact checkout directory is ignored by `blaine-azerothcore`; neither its source nor nested `.git` database may be staged.
- Identity record: `manifests/sources/SRC-ACORE-WOTLK-001.md`

The physical upstream repository and the public governance repository have separate Git histories. Source identity is published as commit, tree, origin, archive digest, key-file hashes, and validation evidence rather than by vendoring source.

## Pin and fidelity policy

Approved baseline work uses the exact full SHA in the manifest. A complete working checkout must be detached at that SHA, clean, and byte/mode/symlink faithful to the Git tree. The exFAT location continues to hold a verified no-checkout object repository only: its single pinned symlink cannot be represented on that mount. `CHK-ACORE-WOTLK-001` now provides a separately governed, independent, faithful checkout on internal ext4.

Do not edit the pinned baseline, create a development branch in the evidence clone, initialize submodules, pull LFS content, apply patches, add modules, or run generated-code/database/update scripts. Customizations belong in separately governed branches, modules, forks, or repositories after the clean baseline is established.

## Verification commands

Read-only verification should include:

```text
git ls-remote https://github.com/azerothcore/azerothcore-wotlk.git HEAD refs/heads/master
git -C <clone> remote -v
git -C <clone> rev-parse --is-shallow-repository
git -C <clone> cat-file -t <pin>
git -C <clone> merge-base --is-ancestor <pin> refs/remotes/origin/master
git -C <clone> fsck --full --strict
git -C <clone> rev-parse <pin>^{tree}
git -C <governance-root> check-ignore -v linux/upstream/azerothcore-wotlk/.git/HEAD
```

After a faithful checkout becomes possible, additionally require detached `HEAD == <pin>`, a matching tree, clean status, no filters changing content, no untracked source, and another strict object check.

The validated internal checkout model and maintenance rules are recorded in `docs/source/azerothcore-internal-baseline-checkout-management.md`. The portable object repository and internal working checkout have distinct roles and must not be silently substituted for one another.

## Manifest and update requirements

Every source update must retain full history and record official ref observations, timestamps, prior and new pins, ancestry, commit metadata/signature state, tree, deterministic `git archive` SHA-256, key-file hashes, counts, sizes, case/path/symlink/Gitlink/LFS audits, license, and build-definition/acquisition-behavior changes. Review must occur before moving the approved pin. No moving tag or branch name substitutes for the full SHA.

Rollback selects a previously validated manifest and object commit; it does not rewrite upstream history. Removal of a clone is separately authorized and must preserve governance evidence.

## Build-readiness distinction

Source-object integrity proves that the official Git objects and pin are present. Checkout fidelity proves that the mounted filesystem faithfully represents the pinned tree; this now passes for `CHK-ACORE-WOTLK-001`, while the exFAT path remains unsuitable. Build readiness additionally requires approved cache and output placement, compatible rootless Buildx/Compose invocation, external-image/package acquisition authorization, and static secret/network review. None of those broader claims follows from object or checkout validation alone.

## License, attribution, and restricted assets

The root source license is GNU GPL version 2; bundled dependencies carry their own notices. Preserve source attribution and applicable license material in future distributions. No WoW client, Blizzard asset, client-derived map/DBC/VMap/MMap data, credential, or proprietary game payload belongs in this upstream checkout or the governance repository.

Upstream contributions require separate fork, branch, provenance, testing, and contribution approval. This policy does not authorize a fork or an upstream push.
