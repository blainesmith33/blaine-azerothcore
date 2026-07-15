# VAL-0007 — SteamOS rootless Docker prerequisite audit validation

## Record control

- Identifier: `VAL-0007`
- Validation date: 2026-07-14
- Related records: `AUD-0003`, `RUN-0007`
- Authoritative root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Lifecycle target: `S0 — Proposed`
- Validation execution: 2026-07-14T19:08:41-07:00
- Status: **PASS**

## Validation scope

Independently validate the three-record write boundary, preservation of governed content and staging, non-installation/non-acquisition boundary, host mount and storage-error stop gates, and lifecycle.

## Pre-write comparison anchors

- Existing authoritative files: 63; directories: 52.
- Existing-file aggregate content-list SHA-256: `af95c46abf47da5082affbfa64bf4129cffa19f72fc9de0a38b4e029695147e9`.
- Existing-file metadata-list SHA-256: `bf4d2b8b4b1f5300efd78610c6ee5ee5a4bf00396e1f877baf1b1d5e4eb265ca`.
- Staging files: 147; directories: 121.
- Staging aggregate content-list SHA-256: `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`.
- Staging metadata-list SHA-256: `0f05adb46b8e6a57b5ff5375715eea715d2481347dece31ffbb5dec59e3f19e5`.
- Pacman local DB entries: 1,158; metadata digest `9c7dd0b0aa3b58d42de9445e6a17f259e90b361f461258ae37cb49fcec5b168c`.
- `/etc/subuid` and `/etc/subgid` SHA-256: `3dbc9d9593703451d1eed51a1af39f8cf6f3a226fee979ac0f6583cff67dc19c` each.
- SteamOS read-only status: `enabled`.
- `/home/deck/.local/share/docker`: absent.
- `/var/lib/docker`: pre-existing empty mount, 4,096 bytes, mtime/ctime 2026-07-12T20:03:30.268137770-07:00.

## Validation method

The post-write shell used only read-only `find`, `sha256sum`, `stat`, `pacman-conf`, `pacman -Q`, `systemctl show`, `systemctl --user show`, `loginctl show-user`, `steamos-readonly status`, `firewall-cmd` query forms, `findmnt`, `rg`, `cat`, and `journalctl` commands. It compared exact pre-write anchors and accumulated a nonzero status for any mismatch.

Core comparison forms were:

```text
find AUTH_ROOT -type f ! -path AUD ! -path RUN ! -path VAL -print0 |
  sort -z | xargs -0 sha256sum | sha256sum
find AUTH_ROOT -type f ! -path AUD ! -path RUN ! -path VAL \
  -printf '%P|%s|%T@|%m|%u|%g\n' | sort | sha256sum
find STAGING_ROOT -type f -print0 | sort -z | xargs -0 sha256sum | sha256sum
find STAGING_ROOT -type f -printf '%P|%s|%T@|%m|%u|%g\n' | sort | sha256sum
find PACMAN_DB/local -type f -printf '%P|%s|%T@\n' | sort | sha256sum
sha256sum /etc/subuid /etc/subgid
steamos-readonly status
systemctl --user show docker.service docker.socket podman.service podman.socket ...
firewall-cmd --state
firewall-cmd --get-default-zone
firewall-cmd --zone=public --list-ports
findmnt -T AUTH_ROOT ...
journalctl -k -b --since '2026-07-14 18:47:39' --no-pager | rg -i STORAGE_PATTERN
```

## Independent results

| Check | Result | Evidence |
|---|---|---|
| The three records exist | PASS | Target-record count `3`; all are regular files at the authorized names. |
| No other project file was created | PASS | Project file count changed from 63 to exactly 66; directory count remained 52. |
| No existing governed file changed | PASS | Excluding the three records, aggregate content-list SHA-256 remained `af95c46abf47da5082affbfa64bf4129cffa19f72fc9de0a38b4e029695147e9`; metadata-list SHA-256 remained `bf4d2b8b4b1f5300efd78610c6ee5ee5a4bf00396e1f877baf1b1d5e4eb265ca`. |
| Staging tree unchanged | PASS | 147 files/121 directories; content hash remained `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`; metadata hash remained `0f05adb46b8e6a57b5ff5375715eea715d2481347dece31ffbb5dec59e3f19e5`. |
| No software installed | PASS | Pacman package count and local-DB directory count remained 1,158; local-DB metadata digest remained `9c7dd0b0aa3b58d42de9445e6a17f259e90b361f461258ae37cb49fcec5b168c`. |
| No network download occurred within this operation | PASS | RUN-0007's command inventory contains no URL fetch, package transaction, clone, pull, installer, image, or acquisition command. `curl`, `wget`, and `git` were version-only. This statement is scoped to this audit and does not claim control over unrelated desktop processes. |
| No service was enabled, disabled, started, stopped, or restarted | PASS | Docker user service/socket remained not-found/inactive/dead; Podman user service/socket remained loaded/inactive/dead/disabled; firewalld remained running. RUN-0007 contains query-only systemctl commands and no service-transition verb. |
| SteamOS read-only state unchanged | PASS | `steamos-readonly status` remained `enabled`. |
| `/etc/subuid` and `/etc/subgid` unchanged | PASS | Each retained SHA-256 `3dbc9d9593703451d1eed51a1af39f8cf6f3a226fee979ac0f6583cff67dc19c`. |
| No sysctl changed | PASS | No `sysctl -w` or `/proc/sys` write appears; `user.max_user_namespaces` remained 58,833 and `net.ipv4.ip_unprivileged_port_start` remained 1024. |
| No firewall rule changed | PASS | No mutating firewall command appears; firewalld stayed running, default zone stayed `public`, and public-zone ports stayed `1024-65535/tcp 1024-65535/udp`. Raw nft/iptables reads remained outside unprivileged visibility and are not used to overclaim. |
| No Docker context changed | PASS | Docker CLI remained absent and no Docker/context command was executed. |
| No Docker data directory created | PASS | `/home/deck/.local/share/docker` remained absent. The OS-managed `/var/lib/docker` mount remained empty, 4,096 bytes, root-owned mode 0755, with pre-existing mtime/ctime epoch `1783911810`. |
| No AzerothCore content acquired | PASS | Existing project content hash was unchanged; staging content and metadata hashes were unchanged; only the three Markdown records were added. |
| No module or addon content acquired | PASS | Same exact project/staging inventory boundary; no source, module, addon, client, database, image, or payload command occurred. |
| No structured project data changed | PASS | No JSON, YAML, TOML, Compose, shell, manifest, or project configuration changed; the three added files are Markdown records. |
| Authoritative mount state | PASS | `/dev/sda1`, exFAT, host `rw`, path writable; staging remained distinct `/dev/sdb1`. |
| Storage-error stop gate | PASS | Zero new matching current-boot USB/UAS/I/O/ext4/journal/reset/cache-sync/offline/remount lines since 18:47:39. The older warning remains unresolved. |
| Lifecycle remains S0 | PASS | README and GOVERNANCE retain their authoritative `S0 — Proposed` statements; no current-lifecycle statement claims S1–S8. |

## Checksum recalculation

The first post-write checksum pass at 19:07:34 produced:

```text
AUD-0003  3e51a315a0131b6be356f2cc8250b6fd48b083903d1dc274d5e9c5b95692f615
RUN-0007  7f6f13c599d5c61016feb64c66901282bb881991e75f67d68c26a123057d7999
VAL-0007  233d55cd00262c855b21c7c387a905dc8dff96770c7395860c5bc4efa5418b8e (provisional validation content)
```

VAL-0007's checksum necessarily changes when provisional content is replaced by these final results. A final read-only closing pass recalculates the finished record hashes and repeats the write-boundary, mount, lifecycle, package, subordinate-ID, data-directory, and storage-error gates.

## Corrected validation event

The 19:07:34 validation invocation returned exit `1` solely because its expected epoch for the pre-existing `/var/lib/docker` timestamp was manually encoded as `1783908210`. `date -d` showed that value represented 19:03:30 MST; the observed and pre-recorded wall-clock value `2026-07-12 20:03:30.268137770 -0700` correctly corresponds to epoch `1783911810`. A direct `stat`, empty inventory, `du`, and `findmnt` recheck confirmed the directory remained the same empty pre-existing mount. The corrected full validation at 19:08:41 used `1783911810`, and every check passed.

## Structured-data boundary

Only Markdown records were touched. The established shell/SHA-256 validation method parsed paths, counts, hashes, lifecycle assertions, required record sections, and host state. No structured project data required a parser because none was changed.

## Final result

- Corrected primary validation exit status: `0`.
- Result: **PASS**.
- Rootless prerequisite audit classification: **partially satisfied**, not implementation approval.
- Installation/acquisition boundary: **PASS; none occurred**.
- Lifecycle: **`S0 — Proposed`**.

This validation confirms the governed audit boundary. It does not validate a Docker runtime, storage driver, network publication, Compose workflow, AzerothCore implementation, or architecture choice.
