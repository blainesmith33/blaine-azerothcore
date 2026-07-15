# Checkout Manifest: CHK-ACORE-WOTLK-001

## Identity

| Field | Value |
|---|---|
| Checkout identifier | `CHK-ACORE-WOTLK-001` |
| Source identifier | `SRC-ACORE-WOTLK-001` |
| Materialization source | `/run/media/deck/blAIneXPLAT/azerothcore/linux/upstream/azerothcore-wotlk/` |
| Authoritative upstream | `https://github.com/azerothcore/azerothcore-wotlk.git` |
| Checkout path | `/home/deck/.local/share/blAIne/azerothcore/checkouts/SRC-ACORE-WOTLK-001/` |
| Materialization date | `2026-07-14` (America/Phoenix) |
| Pin | `34a8bd6655c02448d3da6195dcd00647f634dde3` |
| Tree | `25ad25f5fb8e119f68fef69b080934f1182ad8d6` |
| Archive SHA-256 | `26375e7744b7b767c628c3bbaba159e919b04947b647e0dfc7ba5bc77d2556b1` |

## Clone and independence

| Property | Result |
|---|---|
| Clone strategy | `git clone --no-local --branch master --single-branch --no-checkout <verified-local-source> <checkout>` |
| `--no-local` | Used; objects copied |
| Filesystem | `/dev/nvme0n1p8[/deck]`, internal ext4, read/write |
| Owner | `deck:deck` |
| Origin | Official AzerothCore HTTPS URL |
| Fetch mapping | `+refs/heads/master:refs/remotes/origin/master` |
| Shallow | No |
| Partial/promisor | No |
| Alternates | Absent |
| Hardlinks | Absent; internal pack/index/rev files link count 1 on device 66312; portable files on device 2049 |
| `.git` | Real directory inside checkout; not a pointer file |
| Common directory | Internal checkout `.git` |
| Object directory | Internal checkout `.git/objects` |
| `commondir` | Absent |
| Object integrity | PASS: `git fsck --full --strict`, exit 0 |

## Checkout fidelity

| Property | Result |
|---|---|
| Detached HEAD | PASS; exact approved pin |
| Tree identity | PASS; exact approved tree |
| Working tree | Clean; zero modified, missing, untracked, or ignored paths |
| Tracked paths | 10,011 |
| Physical directories excluding `.git` | 464 |
| Physical regular files excluding `.git` | 10,010 |
| Symbolic links | 1; exact link text and in-root resolved target |
| Executable entries | 42; all retain owner-executable mode |
| Non-executable mode mismatches | 0 |
| Gitlinks/submodules | 0 |
| LFS requirements | 0 |
| Case-fold collisions | 0 |
| Longest path | 104 bytes |
| Working-tree apparent bytes | 897,467,746 (approximately 880 MiB allocated/displayed by `du -sh`) |
| `.git` size | Approximately 1.3 GiB |
| Total checkout size | 2,229,925,046 bytes (approximately 2.2 GiB) |

The required symlink is `.claude/skills/generate-pr-description` -> `../../.agents/skills/generate-pr-description`, resolving to `.agents/skills/generate-pr-description` within the source root. Git mode is `120000`, filesystem type is symbolic link, and it was not materialized as a regular file.

## Git configuration and structural build context

- `core.filemode=true`.
- `core.symlinks` is unset with positive ext4 and checkout evidence; effective symlink behavior passed.
- `core.ignorecase` is unset on demonstrated case-sensitive ext4; zero case collisions.
- No clean/smudge filters, sparse checkout, custom active hooks, submodules, LFS, partial clone, promisor remote, alternates, linked worktree, or external common/object directory.
- `.dockerignore` does not exclude `.git`.
- The pinned Dockerfile uses `--mount=type=bind,target=/azerothcore/.git,source=.git`; the source path is a real directory.
- All root build-context COPY and Compose context paths exist.
- Git revision commands can access the pin and objects. Local materialization paths occur only in reflog provenance and are not configuration/object dependencies.

Structural result: **STRUCTURAL BUILD-CONTEXT PREREQUISITES PASS**. Actual image build and Compose compatibility remain unvalidated.

## State boundary

- Faithful checkout: **ESTABLISHED**.
- Source modification: none.
- Module status: none added.
- Image build/pull: not run.
- Database: not initialized or imported.
- Client data: not acquired.
- Containers/services: none created or started.
- Port publication: none.

The next validation boundary is a separately authorized clean unmodified baseline image-build operation using this checkout. Lifecycle remains **S0 — Proposed**.
