# RUN-0008 — Formalize Rootless Docker Runtime Architecture

## Record control

- Identifier: `RUN-0008`
- Execution date: 2026-07-14
- Operation start: 2026-07-14T19:19:21-07:00
- Authoritative root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Staging root: `/run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/`
- Related decision: `ADR-0010`
- Related host evidence: `AUD-0003`, `RUN-0007`, `VAL-0007`
- Lifecycle before and after: `S0 — Proposed`

## Authorization

The operation was authorized to inspect the authoritative project and staging tree read-only, inspect limited text and metadata from official Docker and AzerothCore sources, and create exactly one ADR, one execution record, and one validation record. It did not authorize software installation, executable retrieval, source cloning, image pulls, service transitions, lingering changes, SteamOS read-only changes, package-manager changes, firewall changes, Docker contexts/data, mounts, staging writes, AzerothCore content, or lifecycle advancement.

## Pre-write gates

| Gate | Evidence | Result |
|---|---|---|
| Authoritative root exists and is host-writable | Host-view `findmnt -T` and `test -w` | PASS: `/dev/sda1`, exFAT, `rw`, writable |
| Restricted view reconciled | Restricted namespace showed `ro,private,slave`; host view showed `rw,shared` | PASS; no mount command run |
| Staging is distinct | `/dev/sdb1`, ext4, separate path/device from `/dev/sda1` | PASS |
| Prior evidence exists | Located `AUD-0003`, `RUN-0007`, and `VAL-0007` | PASS |
| Prior validation | `VAL-0007` status PASS; corrected primary exit status 0 | PASS |
| Lifecycle | `README.md` and `GOVERNANCE.md` | PASS: `S0 — Proposed` |
| ADR sequence and format | Inventoried ADR-0001 through ADR-0009; inspected ADR-0009 and naming rules | PASS: next is `ADR-0010` under `records/decisions/` |
| Target collisions | `test -e` at discovery and 2026-07-14T19:22:57-07:00 | PASS: all three absent |
| Git status | `git -C ROOT rev-parse --is-inside-work-tree` | Not a Git repository |
| Authoritative content boundary | 66 files, 52 directories; content-list SHA-256 `e0221216a8d77e8cd02104a12ae625d11e50e80c92ba679d33d2fe71e154e5de`; metadata-list SHA-256 `d58f4f05818034b692022f754c92b876d9c416be55cd064df7959012231ebd10` | Captured |
| Staging boundary | 147 files, 121 directories; content-list SHA-256 `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`; metadata-list SHA-256 `0f05adb46b8e6a57b5ff5375715eea715d2481347dece31ffbb5dec59e3f19e5` | Captured |
| Storage-error baseline | Current-boot filtered historical set: 513 lines, SHA-256 `3b4b89259f3396b6687f5fd6bdea666ec4dc126c3d038b0dbc863d68464bfdf6` | Warning retained |
| New storage errors | Filtered current-boot journal since operation start | PASS: zero through final pre-write check |

The initial lifecycle helper also queried a nonexistent top-level `acceptance-criteria.md` and returned an `rg` path error. This optional filename was not used as evidence. The authoritative `README.md` and `GOVERNANCE.md` checks succeeded.

## Sources inspected

All source retrieval was text/metadata inspection on 2026-07-14 through the read-only web research facility. No response was saved as an executable or project artifact.

- Docker rootless mode: `https://docs.docker.com/engine/security/rootless/`
- Docker rootless tips: `https://docs.docker.com/engine/security/rootless/tips/`
- Docker rootless troubleshooting/network drivers: `https://docs.docker.com/engine/security/rootless/troubleshoot/`
- Docker Engine v29 release notes: `https://docs.docker.com/engine/release-notes/29/`; release `29.6.1`, 2026-06-26
- Docker stable x86_64 text index: `https://download.docker.com/linux/static/stable/x86_64/`; metadata entries `docker-29.6.1.tgz` and `docker-rootless-extras-29.6.1.tgz`
- Official Compose latest/release metadata: `https://github.com/docker/compose/releases/latest` and `https://github.com/docker/compose/releases/tag/v5.3.1`; Latest release `v5.3.1`, 2026-07-07, verified tag/commit `f32009d`
- Official Compose repository documentation: `https://github.com/docker/compose`
- AzerothCore Docker guide: `https://www.azerothcore.org/wiki/install-with-docker`
- AzerothCore FAQ: `https://www.azerothcore.org/wiki/faq`
- AzerothCore getting started: `https://www.azerothcore.org/wiki/getting-started`

Official sources materially agreed. Engine `29.6.1` and Compose `v5.3.1` were stable candidates, not prerelease/RC/beta/test versions. AzerothCore currently classifies Docker as supported and separately warns that the workflow can be non-idiomatic and that production operators should understand Docker.

## Commands executed

The local/host command inventory was read-only except for the final `apply_patch` that created the three authorized Markdown records. Loops expanded only named paths, units, or executables.

```text
date --iso-8601=seconds
pwd
realpath ROOT STAGING
findmnt -T ROOT -o TARGET,SOURCE,FSTYPE,OPTIONS,PROPAGATION
findmnt -T STAGING -o TARGET,SOURCE,FSTYPE,OPTIONS,PROPAGATION
test -d PATH; test -w PATH; test -e TARGET
find ROOT ...; stat ...; ls ...
rg --files; rg -n ...; sed -n ...
git -C ROOT rev-parse --is-inside-work-tree
find ROOT -type f -print0 | sort -z | xargs -0 sha256sum | sha256sum
find ROOT -type f -printf ... | sort | sha256sum
find STAGING -type f -print0 | sort -z | xargs -0 sha256sum | sha256sum
find STAGING -type f -printf ... | sort | sha256sum
journalctl -b --since TIME --no-pager | rg -i STORAGE_PATTERN
pacman -Qq | wc -l
find /usr/lib/holo/pacmandb/local -type f -printf ... | sort | sha256sum
sha256sum /etc/subuid /etc/subgid
steamos-readonly status
loginctl show-user deck -p Linger --value
systemctl --user show docker.service docker.socket -p LoadState -p ActiveState -p SubState -p UnitFileState
command -v docker dockerd docker-compose rootlesskit containerd runc
test -e /home/deck/.local/share/docker
test -e /home/deck/.docker/contexts
test -e /home/deck/.docker/cli-plugins/docker-compose
firewall-cmd --state
firewall-cmd --get-default-zone
firewall-cmd --zone=public --list-ports
apply_patch (three authorized Markdown additions only)
```

Web inspection requests opened or searched only the official URLs listed under Sources inspected. No shell `curl`, `wget`, `git clone`, package transaction, installer, archive extraction, image pull, or AzerothCore command was run.

## No-installation evidence

Immediately before writing:

- pacman package count was 1,158 and local-database metadata digest was `9c7dd0b0aa3b58d42de9445e6a17f259e90b361f461258ae37cb49fcec5b168c`;
- `docker`, `dockerd`, `docker-compose`, `rootlesskit`, `containerd`, and `runc` were absent;
- Docker user service and socket were `not-found/inactive/dead`;
- `/home/deck/.local/share/docker`, `/home/deck/.docker/contexts`, and the Compose plugin target were absent;
- SteamOS read-only status was `enabled`;
- `Linger=no`;
- `/etc/subuid` and `/etc/subgid` each retained SHA-256 `3dbc9d9593703451d1eed51a1af39f8cf6f3a226fee979ac0f6583cff67dc19c`; and
- firewalld was running with default zone `public` and the pre-existing public ports `1024-65535/tcp 1024-65535/udp`.

The official-source operation retrieved text and metadata only. It did not retrieve executable artifacts, installer scripts, archives, Git repositories, images, modules, addons, client files, or databases.

## Storage-error checks

The historical warning remains unresolved: the current boot contains earlier USB disconnect, UAS recovery/reset, device-offline, I/O, ext4/JBD2, and cache-sync failures associated with the historically unreliable staging device. The pre-operation filtered baseline was 513 lines with SHA-256 `3b4b89259f3396b6687f5fd6bdea666ec4dc126c3d038b0dbc863d68464bfdf6`.

The bounded journal query found zero new matching lines from 2026-07-14T19:19:21-07:00 through the final pre-write check. Post-write validation repeats the bounded check. This record does not state that the USB/UAS problem is resolved.

## Failed or unavailable checks

- The authoritative project is not a Git repository, so no Git status existed to capture. The established sorted SHA-256 content/metadata method was used.
- The ordinary restricted mount namespace reported both external filesystems read-only. The authorized host view reconciled this as `/dev/sda1` and `/dev/sdb1` mounted read/write; no mount was changed.
- The optional top-level `acceptance-criteria.md` lookup used the wrong location and returned an error. Lifecycle was independently established from the authoritative README and GOVERNANCE files.
- No source inspection redirected to or retrieved executable content. No official-source disagreement triggered a stop.

## Files created

Exactly these files were created:

1. `records/decisions/ADR-0010-rootless-docker-runtime-persistence-storage-and-network-architecture.md`
2. `records/executions/RUN-0008-formalize-rootless-docker-runtime-architecture.md`
3. `records/validations/VAL-0008-rootless-docker-runtime-architecture-validation.md`

## Final write boundary

Only the three named Markdown records are authorized to differ from the pre-write project inventory. No prior governed file or directory, staging content or metadata, system configuration, package state, service state, Docker state, firewall state, mount, or lifecycle marker is authorized to change. `VAL-0008` records the independent post-write comparison and final status.

