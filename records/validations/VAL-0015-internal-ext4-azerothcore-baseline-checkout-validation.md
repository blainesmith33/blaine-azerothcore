# VAL-0015 — Internal ext4 AzerothCore Baseline Checkout Validation

## Record control

- Date: 2026-07-14
- Subject: `CHK-ACORE-WOTLK-001`, `RUN-0015`
- Branch: `main`
- Status: **PENDING PUBLICATION — technical checkout and structural validation PASS**
- Lifecycle: `S0 — Proposed`

Final PASS requires the intended governance set to be committed, pushed normally, and verified at matching local/origin/remote refs. Image-build validation is explicitly outside this record.

## Validation matrix

| Assertion | Result | Evidence |
|---|---|---|
| Governance synchronized | PASS | Clean main; no active Git operation; all three refs `1e74a39cec174d8372f0cc2e0308417205b17023`. |
| Required source governance present | PASS | ADR/RUN/VAL-0014 and source manifest existed with historical split result. |
| Portable source objects valid | PASS | Official origin, full master history, approved pin/tree, strict fsck exit 0, deterministic digest match. |
| Official upstream observation | PASS | HEAD/master remained the approved pin; no fetch or pin movement. |
| Internal destination absent at start | PASS | Exact destination and new parent chain absent. |
| Internal capacity | PASS | About 1.1 TiB free before clone; more than 5 GiB required. |
| ext4 and read/write mount | PASS | Internal `/dev/nvme0n1p8[/deck]`, ext4, read/write. |
| Symlink semantics | PASS | Owner-only probe and real checkout link succeeded. |
| Executable-mode semantics | PASS | Probe retained 0755; all 42 Git executable entries passed. |
| Case sensitivity/path depth | PASS | Case-distinct files coexisted; 125-byte probe path passed; no residue. |
| Destination path safety | PASS | Canonical internal path; owner deck; non-world-writable parents; no symlink component. |
| Independent clone | PASS | Exact full-history `--no-local`, single-branch, no-checkout clone completed once. |
| No alternates | PASS | Alternates file/config absent; object directory internal. |
| No hardlink dependency | PASS | Internal pack/index/rev files link count 1 on different device/inodes from portable files. |
| Not shallow/partial/promisor | PASS | All absent/false. |
| Internal object integrity | PASS | `git fsck --full --strict`, exit 0. |
| Official origin | PASS | New clone origin changed to exact official HTTPS URL without fetch. |
| Fetch scope | PASS | Master-only mapping retained. |
| Detached HEAD | PASS | Exact approved pin; no branch created. |
| Tree identity | PASS | Exact approved tree. |
| Working-tree cleanliness | PASS | Clean status/index/files; zero missing, modified, untracked, or ignored paths. |
| Symlink fidelity | PASS | Exact link text, Git mode 120000, real lstat link, in-root existing target. |
| Executable fidelity | PASS | 42/42 owner-executable; zero non-executable mismatches. |
| Tracked and physical counts | PASS | 10,011 tracked; 464 directories; 10,010 regular files; one symlink. |
| Case/Gitlink/LFS audits | PASS | Zero collisions, Gitlinks, submodules, or LFS requirements. |
| Archive digest | PASS | `26375e...d2556b1`, unchanged. |
| Self-contained `.git` | PASS | Real internal directory; internal common/object dirs; no pointer, commondir, or external dependency. |
| Structural build context | PASS | Required paths and Git metadata structure present; `.git` included and directory mount-compatible. |
| Actual image build | NOT RUN | Explicitly deferred; no image-build PASS claimed. |
| Docker/Buildx unchanged | PASS | Rootless 29.6.1, Compose 5.3.1, Buildx 0.35.0, rootless builder, fuse-overlayfs; no state created. |
| Source/module changes | PASS containment | None. |
| Image build/pull | PASS containment | None. |
| Container/service | PASS containment | None created or started. |
| Database | PASS containment | None initialized/imported. |
| Client data/assets | PASS containment | None acquired. |
| Port/network | PASS containment | No publication or configuration change. |
| Governance accuracy | PASS pre-publication | ADR, manifests, policies, README, sequence, RUN, and VAL reflect evidence. |
| Secret/restricted publication | PASS pre-staging | No credentials, private network detail, client asset, binary, object pack, or source vendoring. |
| Governance commit/push | PENDING | Requires normal push; no force. |
| Final refs | PENDING | Requires post-push equality. |
| Lifecycle remains S0 | PASS | No advancement authorized or recorded. |

## Technical conclusion

`CHK-ACORE-WOTLK-001` is a faithful, independent, self-contained, clean detached checkout of the approved source pin on internal ext4. It resolves the prior exFAT checkout blocker without modifying the portable object repository or upstream source.

Structural build-context prerequisites pass. This means the pinned definitions can structurally see required source and real Git metadata; it does not mean an image has been built, pulled, compiled, or runtime-validated.

## Publication boundary

Final PASS remains withheld until only the intended governance Markdown/current-document set is audited, committed, pushed normally, and final refs match. No historical RUN-0014 or VAL-0014 record may be changed.

Lifecycle remains **S0 — Proposed**.
