# VAL-0014 — Official AzerothCore Source Acquisition Validation

## Record control

- Date: 2026-07-14
- Subject: `RUN-0014`, `SRC-ACORE-WOTLK-001`
- Branch: `main`
- Status: **SPLIT — source-object acquisition PASS; checkout/build readiness DEFERRED; overall NOT FULL PASS**
- Lifecycle: `S0 — Proposed`

Full PASS is prohibited because the authoritative exFAT mount cannot represent the one pinned symbolic-link entry. The verified no-checkout Git object repository is retained, and no inaccurate working tree was created.

## Validation matrix

| Assertion | Result | Evidence |
|---|---|---|
| Correct governance repository/remote | PASS | Canonical authorized root; exact SSH origin. |
| Synchronized clean start | PASS | HEAD, origin/main, and remote main were `38c1e515d47564fe7f04f3d55503785b104ce5a7`; clean main; no active Git operation. |
| Lifecycle at start | PASS | README/GOVERNANCE/VAL-0013 reported S0 — Proposed. |
| Docker/Buildx prerequisites unchanged | PASS | Rootless Docker 29.6.1, Compose 5.3.1, Buildx 0.35.0 official checksum, selected rootless builder, fuse-overlayfs/internal data root. |
| Correct official upstream | PASS | Exact `https://github.com/azerothcore/azerothcore-wotlk.git` origin. |
| Master ref resolved consistently | PASS | Two pre-clone observations and one post-clone observation all returned the pin. |
| Acquisition pin recorded | PASS | `34a8bd6655c02448d3da6195dcd00647f634dde3`; research SHA is its ancestor. |
| Full-history single-branch clone | PASS | Exact clone flags and master-only fetch mapping. |
| Repository not shallow/partial | PASS | Shallow=false; no partial/promisor configuration. |
| Git object integrity | PASS | `git fsck --full --strict`, exit 0; no garbage after completed clone. |
| Pin from official master | PASS | `origin/master` equals pin; commit object present; ancestry check exit 0. |
| Tree identity | PASS | `25ad25f5fb8e119f68fef69b080934f1182ad8d6`. |
| Deterministic archive digest | PASS | Streamed SHA-256 `26375e7744b7b767c628c3bbaba159e919b04947b647e0dfc7ba5bc77d2556b1`; no archive stored. |
| License recorded | PASS | Root GPL version 2 text and blob/content identities recorded. |
| Case-fold/path-name audit | PASS | Zero case collisions; zero exFAT-invalid names. |
| Symlink/executable audit | PASS inventory | One symlink and 42 executables identified. |
| Filesystem representability | FAIL at authorized path | exFAT `ln -s` probe returned `Operation not permitted` for required symlink semantics. |
| Detached pinned checkout | DEFERRED | Checkout deliberately not run after representability failure. |
| Clean upstream working tree | NOT APPLICABLE | No working tree exists; clone remains no-checkout. |
| Gitlinks/submodules | PASS | Zero Gitlinks; `.gitmodules` absent; no submodule command run. |
| LFS | PASS | No LFS filter declaration or pointer detected; no LFS command run. |
| Build definitions inventoried | PASS static | Compose services, Dockerfiles/stages, CMake/wrappers, contexts, images, volumes, networks, publications, health/dependency behavior recorded. |
| External acquisition inventoried | PASS static | Base images, apt operations, client-data download, module/catalog and installer network paths identified without execution. |
| Client-data behavior | PASS static | Runtime client-data initializer and tools extraction path identified; nothing downloaded or extracted. |
| Source modification/module addition | PASS containment | Zero physical source files checked out; no patch, edit, module, or branch. |
| Image build/pull | PASS containment | None. |
| Container/runtime | PASS containment | No container created or started; visible counts remained zero at preflight. |
| Database | PASS containment | No database created, imported, initialized, or started. |
| Client/game assets | PASS containment | No client, DBC, map, VMap, MMap, addon, patch, or Blizzard asset acquired. |
| Port/network exposure | PASS containment | No port, listener, Docker/network setting, firewall, router, DNS, VPN, tunnel, proxy, or ingress change. |
| Parent ignore boundary | PASS | Exact rule applies to clone `.git` and source paths; no upstream path tracked or staged. |
| Manifest accuracy | PASS pre-publication | Split object/checkout state, counts, hashes, license, and boundaries match evidence. |
| README/current sequence alignment | PASS pre-publication | Current state will report object acquisition and checkout deferral without claiming a build. |
| Secret/restricted publication | PASS pre-staging | Records contain no credential values, client assets, source vendoring, binary, or private network detail. |
| Governance commit/push | PASS | Commit `abe2b13fc44e2b10a326de3e397e628419b388ef` pushed normally as a fast-forward; no force push. |
| Governance refs after first push | PASS | Local HEAD, origin/main, and remote main all equaled `abe2b13fc44e2b10a326de3e397e628419b388ef`; starting history remained an ancestor. |
| Lifecycle remains S0 | PASS | No advancement authorized or recorded. |

## Split classification

### Source-object acquisition: PASS

Official full-history objects, exact master pin, ancestry, tree, strict integrity, deterministic digest, license, and static build/acquisition inventory are verified and retained.

### Source-checkout/build readiness: DEFERRED

The pinned Git tree requires a symlink that exFAT cannot create. The operation correctly stopped before checkout. The clone therefore has no source working tree, is not detached at a checked-out pin, and cannot be supplied to the upstream Docker build. A later governed decision must establish a symlink-faithful source location or mechanism and repeat checkout validation.

### Overall functional result: NOT FULL PASS

This result is not a source-integrity failure and does not authorize a workaround, altered tree, archive expansion, or build. It is a physical checkout-fidelity blocker.

## Publication validation

The first publication staged exactly the nine intended text/governance paths. Every staged object was mode 100644; cached whitespace and complete-diff review passed; path-only secret/binary findings were zero; no upstream source path appeared in the index. Commit `abe2b13fc44e2b10a326de3e397e628419b388ef` was pushed normally and all three refs matched it.

Completing RUN-0014 and this validation record requires the one authorized validation-only commit. Its object ID and final local/origin/remote equality must be captured after that commit and normal push in the final operation report. Governance publication success does not change the overall source result from split/not-full-PASS.

## Conclusion

Object acquisition and provenance pass; checkout fidelity and build readiness are deferred on the current exFAT path. No source was modified, no restricted content was published, no build/runtime/client/database action occurred, and lifecycle remains **S0 — Proposed**.
