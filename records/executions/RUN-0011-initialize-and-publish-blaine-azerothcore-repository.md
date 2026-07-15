# RUN-0011 — Initialize and Publish blAIne AzerothCore Repository

## Record control

- Date: 2026-07-14 (America/Phoenix)
- Lifecycle: `S0 — Proposed`
- Objective: initialize the existing governed local scaffold as the working copy of `git@github.com:blainesmith33/blaine-azerothcore.git`, preserve the remote README history, protect restricted content, audit the publication set, commit, and push normally to `main`.
- Authorized root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Remote: `git@github.com:blainesmith33/blaine-azerothcore.git`
- Remote branch: `main`
- Publication state in this record revision: scaffold commit and first normal push validated; completed evidence is ready for the single validation-only follow-up commit.

## Authorization boundary

Writes are confined to the authoritative project root. Communication is confined to the named GitHub remote and its commit-pinned README endpoint. No sudo, package installation, Docker or system-service change, firewall/mount/storage/SteamOS change, AzerothCore/ChromieCraft/client/documentation acquisition, history rewrite, rebase, force push, lifecycle advancement, content deletion, or content move is authorized or performed.

## Starting physical inventory

Before any write:

- canonical root: `/run/media/deck/blAIneXPLAT/azerothcore`;
- filesystem: `/dev/sda1`, exFAT, mounted read/write at `/run/media/deck/blAIneXPLAT`;
- directories: 52;
- regular files: 74;
- apparent size: 405,537 bytes;
- `.git`: absent;
- `.gitignore`: absent;
- `README.md`: present, 5,322 bytes, SHA-256 `1ea5f5c7cdf3b1aaa0806781c16cd16e185ab9c373ffd48c109fdf9e97a45a66`;
- symbolic links: zero;
- regular files larger than 25 MiB: zero.

## Starting Git and remote state

The root was not a Git repository. No staged changes, unexplained commits, active Git operation, or conflicting remote existed. Existing Git identity was inspected and not changed:

- user name: `Blaine Smith`;
- user email: `blainesmith.33@gmail.com`.

`git ls-remote git@github.com:blainesmith33/blaine-azerothcore.git` exited 0 and returned:

```text
950f6bf09b7048aca7208ed9e2ae974290cdb16f HEAD
950f6bf09b7048aca7208ed9e2ae974290cdb16f refs/heads/main
```

The commit-pinned raw README endpoint returned HTTP 200 with 10,954 bytes. Its SHA-256 was `e61ac8b331463cd13a72de8f6e2178313529e72ce6cacd4ad4530673f7e3d6e7`. After fetch, the remote tree independently showed one tracked `README.md` blob `e7460132e8cddb2b76423258bb84583d516e19aa` at starting commit `950f6bf09b7048aca7208ed9e2ae974290cdb16f`.

## Restricted-content and secret audit method

The preflight inventory did not follow symlinks and checked:

- all symlink paths and targets;
- all files larger than 25 MiB;
- archive, disk-image, installer, executable, shared-library, client, database, log, cache, build, runtime, credential, cookie, browser, wallet, recovery, private, and personal-data name/extension patterns;
- native file types using `file`;
- `documentation/source-corpus` contents;
- binary-excluding text scans with path-only reporting for private-key headers, GitHub tokens, AWS keys, OAuth/Battle.net secrets, password assignments, bearer tokens, credentialed database URIs, Discord/Slack tokens, and generic assigned high-entropy secret patterns.

Results: zero symlinks, zero >25 MiB files, zero restricted/binary/database/client/cache/build candidates, and zero path-only secret findings. No secret value was printed.

## Publication protections established

A root `.gitignore` was created with governed exclusions for credentials and authentication material; WoW clients and payloads; raw documentation corpus with manifest/index exceptions; database runtime state/backups while deliberately not ignoring `*.sql`; build/generated dependencies; runtime/log state; container/service state; caches/editor/OS metadata; and archives/installers/images/binaries.

On this exFAT working tree Git set `core.ignorecase=true`. The initially requested broad `**/Data/` and `**/Cache/` patterns would also have ignored legitimate lowercase `data/sql` paths. Before staging, those two patterns were narrowed to `Data` and `Cache` beneath the already restricted client-root families. Representative client paths remain ignored, and `modules/example/data/sql/updates/db_world/2026_01_01.sql` is eligible. No blanket `*.sql` rule exists.

## README reconciliation

The preexisting local README differed from the remote README. It was copied byte-for-byte, without overwriting another record, to:

`records/source-state/README.preexisting-local-before-git-initialization.md`

The preserved file has SHA-256 `1ea5f5c7cdf3b1aaa0806781c16cd16e185ab9c373ffd48c109fdf9e97a45a66`. The authoritative root README was then restored from remote starting commit `950f6bf09b7048aca7208ed9e2ae974290cdb16f`; its final pre-commit hash is `e61ac8b331463cd13a72de8f6e2178313529e72ce6cacd4ad4530673f7e3d6e7`.

## Git initialization method

The safe in-place attachment sequence was:

```text
git init -b main
git remote add origin git@github.com:blainesmith33/blaine-azerothcore.git
git fetch origin main
git update-ref refs/heads/main refs/remotes/origin/main
git reset --mixed HEAD
git restore --source=HEAD --worktree -- README.md
```

No unrelated-history merge, rebase, force push, history replacement, or working-tree deletion was used. Repository-local config identifies the correct origin; global Git configuration was not modified.

## Pre-commit staging and audit

- Files staged: 77 additions; zero modifications or deletions. With the unchanged remote README, the prospective tree contains 78 tracked files.
- Deliberately excluded: zero restricted files were physically present and zero physical ignored paths were reported. Representative credential, client, raw-corpus, database, backup, build, runtime, cache, archive, and binary paths were independently proven ignored with `git check-ignore -v`.
- `git status --short`, `git diff --cached --stat`, and `git diff --cached --name-status` showed only the intended governed scaffold, `.gitignore`, README preservation record, RUN-0011, and VAL-0011. `git diff --cached --check` returned 2 with 169 diagnostic lines for inherited Markdown trailing two-space line breaks and blank-line-at-EOF formatting. These warnings were reviewed and accepted to preserve prior governed record bytes; they are not secret, restricted-content, binary, or scope findings.
- Every staged path was inspected. All 77 staged entries are mode 100644 and `text/plain`; zero symlinks or non-regular index modes exist. Maximum staged size is 28,572 bytes; zero exceeds 25 MiB.
- Restricted path/extension and credential-name checks returned zero staged paths. Native/archive/database/image/audio/video signature review returned zero candidate.
- Binary-excluding staged-blob scans returned zero paths for every secret category. Email-like scanning was classified by path and count only: all matches were systemd unit names or the named SSH remote except the explicitly required configured Git email recorded in this execution record; residual unexpected personal-information findings were zero.
- Root README has no staged difference from starting commit `950f6bf09b7048aca7208ed9e2ae974290cdb16f`.
- Untracked-file count after staging: zero.

## Warnings and anomalies

- The broad client `Data`/`Cache` ignore patterns required narrowing because the filesystem is case-insensitive; corrected before staging.
- GitHub SSH does not support `git-upload-archive`; the unsuccessful read-only remote archive attempt exited 2 and wrote no project state. README existence was instead proven by the commit-pinned raw endpoint and fetched tree.
- The preexisting README differed and was preserved as required.

## Commit and push result

- Scaffold commit: `eabf64ec2f48bfbf891e94cb6d5e92e6fc871e0a`, message `Publish governed blAIne AzerothCore project scaffold`; parent `950f6bf09b7048aca7208ed9e2ae974290cdb16f`.
- First push: `git push -u origin main`, exit 0, normal fast-forward `950f6bf..eabf64e`; no force option.
- Post-first-push local `HEAD`: `eabf64ec2f48bfbf891e94cb6d5e92e6fc871e0a`.
- Post-first-push local `origin/main`: `eabf64ec2f48bfbf891e94cb6d5e92e6fc871e0a`.
- Post-first-push remote `refs/heads/main`: `eabf64ec2f48bfbf891e94cb6d5e92e6fc871e0a`.
- Original README commit ancestry: PASS; `git merge-base --is-ancestor 950f6bf... eabf64e...` exited 0.
- Post-first-push tree: clean; 78 tracked regular files; zero physical ignored paths and zero untracked files.
- Validation-only commit: required because completing RUN-0011 and VAL-0011 changes them. It must use message `Record blAIne AzerothCore publication validation` and be pushed normally. Its object ID and final three-ref equality are necessarily verified after this record's bytes are committed and are reported in the final operation report; a commit cannot contain its own final object ID.

## Lifecycle statement

This operation publishes governance and scaffold material only. It does not create or acquire AzerothCore runtime/source/client content and does not advance acceptance status. The lifecycle remains **S0 — Proposed**.
