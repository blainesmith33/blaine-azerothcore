# VAL-0008 — Rootless Docker Runtime Architecture Validation

## Record control

- Identifier: `VAL-0008`
- Validation date: 2026-07-14
- Related records: `ADR-0010`, `RUN-0008`
- Authoritative root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Lifecycle target: `S0 — Proposed`
- Validation execution: 2026-07-14T19:29:17-07:00
- Status: **PASS**

## Validation scope

Independently validate the exact three-record write boundary; preservation of prior governed files and staging content/metadata; non-installation and non-acquisition constraints; service, loginctl, SteamOS, Docker, firewall, mount, and storage-error state; required ADR content; and lifecycle.

## Pre-write comparison anchors

- Existing authoritative files: 66; directories: 52.
- Existing-file aggregate content-list SHA-256: `e0221216a8d77e8cd02104a12ae625d11e50e80c92ba679d33d2fe71e154e5de`.
- Existing-file metadata-list SHA-256: `d58f4f05818034b692022f754c92b876d9c416be55cd064df7959012231ebd10`.
- Staging files: 147; directories: 121.
- Staging content-list SHA-256: `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`.
- Staging metadata-list SHA-256: `0f05adb46b8e6a57b5ff5375715eea715d2481347dece31ffbb5dec59e3f19e5`.
- Pacman package count: 1,158; local-database metadata digest: `9c7dd0b0aa3b58d42de9445e6a17f259e90b361f461258ae37cb49fcec5b168c`.
- `/etc/subuid` and `/etc/subgid` SHA-256: `3dbc9d9593703451d1eed51a1af39f8cf6f3a226fee979ac0f6583cff67dc19c` each.
- SteamOS read-only status: `enabled`.
- `Linger=no`.
- Docker user service/socket: `not-found/inactive/dead`.
- Docker, dockerd, docker-compose, RootlessKit, containerd, and runc: absent.
- Docker data root, Docker contexts directory, and Compose plugin target: absent.
- Firewalld: running; default zone `public`; public ports `1024-65535/tcp 1024-65535/udp`.
- Current-boot historical storage-error baseline: 513 filtered lines; SHA-256 `3b4b89259f3396b6687f5fd6bdea666ec4dc126c3d038b0dbc863d68464bfdf6`.

## Validation method

The closing validator will use read-only `find`, `sha256sum`, `stat`, `rg`, `pacman`, `systemctl`, `loginctl`, `steamos-readonly`, `firewall-cmd` query forms, `findmnt`, and `journalctl`. It will exclude only the three new targets when recomputing the pre-existing project content and metadata digests. Markdown headings and required decision phrases will be checked with `rg`; no structured project data was touched.

## Independent results

| Check | Result | Evidence |
|---|---|---|
| Exactly the three authorized records were created | PASS | All three target paths are regular files; project file count changed from 66 to exactly 69 and directory count remained 52. |
| No prior project file changed | PASS | Excluding only the three targets, content-list SHA-256 remained `e0221216a8d77e8cd02104a12ae625d11e50e80c92ba679d33d2fe71e154e5de` and metadata-list SHA-256 remained `d58f4f05818034b692022f754c92b876d9c416be55cd064df7959012231ebd10`. |
| Staging content and metadata remained unchanged | PASS | 147 files and 121 directories; content-list SHA-256 remained `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`; metadata-list SHA-256 remained `0f05adb46b8e6a57b5ff5375715eea715d2481347dece31ffbb5dec59e3f19e5`. |
| No software was installed | PASS | Pacman package count remained 1,158 and local-database metadata digest remained `9c7dd0b0aa3b58d42de9445e6a17f259e90b361f461258ae37cb49fcec5b168c`. No package/install command occurred. |
| No executable artifact was downloaded | PASS | The only network retrieval was read-only official text/release metadata inspection through the web research facility. The command inventory contains no installer, archive, binary, clone, pull, package transaction, or executable retrieval. All three new files are Markdown. |
| No service changed state | PASS | Docker user service/socket remained `not-found/inactive/dead`; no enable, disable, start, stop, restart, daemon-reload, or service-write command occurred. Firewalld remained running. |
| No `loginctl` setting changed | PASS | `Linger=no` before and after; only `loginctl show-user` was used. |
| SteamOS read-only mode remained enabled | PASS | `steamos-readonly status` remained `enabled`; no mutating SteamOS command occurred. |
| Docker remained absent | PASS | `docker`, `dockerd`, RootlessKit, containerd, and runc remained absent from PATH; the Docker user unit/socket remained not found. |
| Docker Compose remained absent | PASS | `docker-compose` remained absent and `/home/deck/.docker/cli-plugins/docker-compose` remained absent. |
| No Docker data directory was created | PASS | `/home/deck/.local/share/docker` remained absent. |
| No Docker context was created | PASS | `/home/deck/.docker/contexts` remained absent and Docker CLI remained absent. |
| No container image was pulled | PASS | Docker remained absent, no image command occurred, and the Docker data root remained absent. |
| No AzerothCore source or assets were acquired | PASS | Every pre-existing project file retained its content/metadata boundary; staging was identical; only the three Markdown records were added. No source, module, addon, client, image, or database acquisition command occurred. |
| No firewall rule changed | PASS | Firewalld remained running, default zone remained `public`, and public ports remained `1024-65535/tcp 1024-65535/udp`. No mutating firewall command occurred. |
| Protected identity/namespace configuration remained unchanged | PASS | `/etc/subuid` and `/etc/subgid` each retained SHA-256 `3dbc9d9593703451d1eed51a1af39f8cf6f3a226fee979ac0f6583cff67dc19c`; `user.max_user_namespaces` remained 58,833; no sysctl command occurred. |
| Authoritative mount remained writable | PASS | `/dev/sda1`, exFAT, host view `rw`; staging remained distinct `/dev/sdb1`, ext4. No mount operation occurred. |
| No new storage errors appeared | PASS | Zero new matching USB/UAS/I/O/ext4/journal/offline/reset/cache-sync/remount events were found from operation start 2026-07-14T19:19:21-07:00 through validation. The historical warning remains unresolved. |
| ADR required content | PASS | Required context, evidence, decision, alternative, consequence, security, storage, persistence, network, milestone, rollback, deferred-decision, and lifecycle sections were present; stable candidates and approved paths were found. |
| No structured project data changed | PASS | No JSON, YAML, TOML, Compose, shell, manifest, configuration, or source file changed; additions are Markdown records only. |
| Lifecycle remains S0 | PASS | `README.md`, `GOVERNANCE.md`, `ADR-0010`, and `RUN-0008` contain the authoritative `S0 — Proposed` state; no lifecycle advancement was recorded. |

## Corrected validation event

The first closing invocation at 2026-07-14T19:28:10-07:00 returned exit `1` for two validation-script assertions, not for a project or host-state failure:

1. It attempted to compare the inherited `AUD-0003` storage baseline against a newly expressed, narrower full-journal filter. The expressions were not equivalent. The authoritative stop-condition check is the bounded query since this operation began; that check returned zero new events in both invocations.
2. It searched case-sensitively for `Rootless Docker Engine operated by the deck user`; the ADR's exact phrase starts with lowercase `rootless`. The corrected fixed-string check found the decision at line 54.

The corrected full validation at 2026-07-14T19:29:17-07:00 retained the inherited historical warning as reference, used the bounded new-event check for the stop condition, corrected the phrase case, repeated every comparison, and returned exit `0`.

For transparency, the current kernel-only narrower filter produced 392 historical lines and SHA-256 `6c3b38e75a9ce689f6a6cd70eea207a58e1959d77dcbb0237da4a8ac4b0330c5`; it does not replace the broader 513-line `AUD-0003` baseline or imply resolution.

## Record checksums

The corrected validation observed these hashes before this pending record was finalized:

```text
ADR-0010  d0638299771035f5d23aad0ea9d3f8fcf0644ba1e5573c8df198e0ca5abe8821
RUN-0008  43509e369223a30978fed3211b0defc42a24ee5ebd0a0e80ffd9efd20ecc39ef
VAL-0008  de5d420c4e9b99a873ea1a49d0dcf246efd1dfe38ba04e72a0e263c038ae6a5d (provisional content)
```

`VAL-0008` necessarily changes when the observed results replace its pending section. The final read-only closing pass recalculates the finished hash and repeats the exact write-boundary, mount, lifecycle, package, service, Docker-path, firewall, and bounded storage-error checks.

## Final result

- Corrected primary validation exit status: `0`.
- Result: **PASS**.
- Architecture record boundary: **PASS; exactly three authorized Markdown records created**.
- Installation/acquisition boundary: **PASS; no installation or executable/AzerothCore acquisition occurred**.
- Service, persistence, firewall, Docker, and SteamOS boundary: **PASS; unchanged**.
- Storage reliability: **no new relevant events; historical USB/UAS warning remains unresolved**.
- Lifecycle: **`S0 — Proposed`**.

This validation approves the records-only architecture operation. It does not validate a Docker runtime, Docker Compose, a storage driver, a network driver, a container, an AzerothCore deployment, or unattended persistence.
