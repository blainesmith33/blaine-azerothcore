# VAL-0009 — Rootless Docker Functional Runtime Validation

## Record control

- Identifier: `VAL-0009`
- Validation date: 2026-07-14
- Related execution: `RUN-0009`
- Related decision: `ADR-0010`
- Authoritative root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Lifecycle target: `S0 — Proposed`
- Validation execution: 2026-07-14T20:03:24-07:00
- Status: **FAIL — functional milestone not achieved; containment/integrity PASS**

## Validation scope

Independently validate the exact two-record governance boundary; project/staging preservation; pinned installation, user service, context, socket, rootless security, data-root, host-state, test-resource cleanup, storage reliability, and lifecycle. Functional dimensions must be reported separately; an installation/security success may not mask a container-test failure.

## Pre-write comparison anchors

- Existing authoritative files: 69; directories: 52.
- Existing-file content-list SHA-256: `c86295d6de8fe9b61ce48c58a2a7af0ccbf3a816c928a3c2bb30d52c3bb71137`.
- Existing-file metadata-list SHA-256: `9a37f9b9bba70adadbbd2cedc61a49095e3e80670812eef8e46d39dacc0f5dfb`.
- Staging: 147 files, 121 directories; content SHA-256 `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`; metadata SHA-256 `0f05adb46b8e6a57b5ff5375715eea715d2481347dece31ffbb5dec59e3f19e5`.
- Pacman package count: 1,158; local-DB metadata SHA-256 `9c7dd0b0aa3b58d42de9445e6a17f259e90b361f461258ae37cb49fcec5b168c`.
- `/etc/subuid` and `/etc/subgid`: SHA-256 `3dbc9d9593703451d1eed51a1af39f8cf6f3a226fee979ac0f6583cff67dc19c` each.
- Shell profile SHA-256 values: `.bashrc` `ba6cce6e38c79dba3f25ec487a27f9a980ff49cb594b11d11c74bf17707cc965`; `.bash_profile` `a621169446500734421a3a65714da8bed1a6557873d98b50aa17c5d0e694b148`; `.profile` and `.zshrc` absent.
- SteamOS read-only: enabled; `Linger=no`.
- Firewalld: running; default public; runtime/permanent ports `1024-65535/tcp 1024-65535/udp`.
- Host sysctls: `user.max_user_namespaces=58833`, `net.ipv4.ip_unprivileged_port_start=1024`, `net.ipv4.ip_forward=0`.
- `/var/lib/docker`: root-owned mode 0755, size 4096, mtime/ctime epoch `1783911810`, zero entries.
- Storage reliability: 416 historical matching kernel lines, SHA-256 `ca2fde5021ad2d3c4b19bef670f4a1e929509e06aa234b7f340a6349d0fdd4fe`; zero new since operation start.

## Validation method

The closing validator will use only read-only `find`, `sha256sum`, `stat`, `rg`, Docker query commands, `systemctl`, `loginctl`, `steamos-readonly`, package database queries, `firewall-cmd` query forms, `ss`, `ps`, `findmnt`, and `journalctl`. It will exclude only RUN-0009 and VAL-0009 when recomputing the prior governance boundary.

## Independent results

| Required check | Result | Independent evidence |
|---|---|---|
| RUN-0009 and VAL-0009 are the only new governance files | PASS | Project count changed from 69 to exactly 71; directory count remained 52; only the two targets had mtimes after operation start. |
| No prior governance/project file changed | PASS | Excluding the two targets, content-list SHA-256 remained `c86295d6de8fe9b61ce48c58a2a7af0ccbf3a816c928a3c2bb30d52c3bb71137`; metadata-list SHA-256 remained `9a37f9b9bba70adadbbd2cedc61a49095e3e80670812eef8e46d39dacc0f5dfb`. |
| Staging checksums and metadata unchanged | PASS | 147 files, 121 directories; content SHA-256 `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`; metadata SHA-256 `0f05adb46b8e6a57b5ff5375715eea715d2481347dece31ffbb5dec59e3f19e5`. |
| Docker client exactly 29.6.1 | PASS | `docker version --format` returned `29.6.1`. |
| Docker server exactly 29.6.1 | PASS | Rootless endpoint returned server `29.6.1`. |
| Docker Compose exactly v5.3.1 | PASS | `docker compose version --short` returned `5.3.1`; plugin hash matched the official `v5.3.1` asset. |
| Rootless security option present | PASS | Security options were `seccomp` builtin, `rootless`, and `cgroupns`. |
| Active context is rootless | PASS | `docker context show` returned `rootless`. |
| Endpoint is approved Unix socket | PASS | Context endpoint `unix:///run/user/1000/docker.sock`; no TCP API listener. |
| Docker root directory approved | PASS | `/home/deck/.local/share/docker`. |
| Docker root on internal ext4 | PASS | `/dev/nvme0n1p8`, ext4, `rw`; no external project-drive placement. |
| Storage driver | PASS as observation; **FAIL functionally** | Driver reports `overlayfs`, but the basic container exposed a kernel/filesystem incompatibility. Driver is not functional on this backing store. |
| Cgroup version and driver | PASS as observation | Cgroup v2 and systemd driver. Limit enforcement was not tested after the basic failure. |
| No rootful Docker service active | PASS | Root `docker.service` and `docker.socket` remained not-found/inactive/dead. |
| No rootful Docker socket | PASS | `/var/run/docker.sock` absent; `/var/lib/docker` retained its exact prior metadata and zero entries. |
| No UID-0 dockerd | PASS | UID-0 count zero; the only host dockerd UID was 1000. |
| User docker.service runs as UID 1000 | PASS | Unit loaded/active/running/enabled; MainPID owner UID 1000. |
| No Docker TCP listener | PASS | No dockerd, port 2375, or port 2376 TCP listener. |
| Linger remains no | PASS | `loginctl show-user deck` returned `no`. |
| SteamOS read-only remains enabled | PASS | `steamos-readonly status` returned `enabled`. |
| Package database unchanged | PASS | 1,158 packages; local-DB metadata SHA-256 remained `9c7dd0b0aa3b58d42de9445e6a17f259e90b361f461258ae37cb49fcec5b168c`. |
| `/etc/subuid` and `/etc/subgid` unchanged | PASS | Both retained SHA-256 `3dbc9d9593703451d1eed51a1af39f8cf6f3a226fee979ac0f6583cff67dc19c`. |
| No host sysctl changed | PASS | Host values remained `user.max_user_namespaces=58833`, unprivileged port start `1024`, and IPv4 forwarding `0`. The rootless launcher changed forwarding only inside its transient network namespace. |
| No firewall configuration changed | PASS | Firewalld running, default public, runtime and permanent ports both remained `1024-65535/tcp 1024-65535/udp`; daemon log states firewalld management was skipped for rootless mode. |
| No shell startup file changed | PASS | `.bashrc` and `.bash_profile` retained their pre-install hashes; `.profile` and `.zshrc` remained absent. |
| No unauthorized internal user file changed | PASS within operation evidence | Installed binary set was exactly the 11 expected archive members. New systemd-user entries were exactly `docker.service`, `default.target.wants/`, and its Docker symlink. All other created/changed paths were within the explicitly authorized Docker/cache roots; command inventory contains no other write target. |
| Test containers removed | PASS | Container count zero. The failed create left no container. |
| Test images | EXPLAINED RETENTION | Exactly three approved images remain, retained for diagnosis after failure; no AzerothCore image exists. |
| Test networks removed | PASS | Only Docker defaults `bridge`, `host`, and `none`; no RUN-0009 or Compose network. |
| Test volumes removed | PASS | Volume count zero. |
| Ports 18080 and 18081 clear | PASS | Listener count zero; neither port was published. |
| No AzerothCore content/credential acquired | PASS | Exact project/staging boundaries, only approved image digests, and artifact inventory show no AzerothCore source, image, database, module, addon, client file, credential, or configuration. |
| Authoritative project mount remains read/write | PASS | `/dev/sda1`, exFAT, host view `rw`; staging remains separate `/dev/sdb1`. |
| No new relevant storage errors | PASS | Historical filter remained 416 lines and the same SHA-256; zero new matching events since operation start. The overlayfs error is separately classified as a functional compatibility failure. |
| Lifecycle remains S0 | PASS | README, GOVERNANCE, and RUN-0009 retain `S0 — Proposed`; no advancement occurred. |

## Retained image evidence

Exactly these approved images remain for reproduction/diagnosis:

```text
hello-world@sha256:96498ffd522e70807ab6384a5c0485a79b9c7c08ca79ba08623edcad1054e62d
busybox@sha256:9532d8c39891ca2ecde4d30d7710e01fb739c87a8b9299685c63704296b16028
alpine@sha256:fd791d74b68913cbb027c6546007b3f0d3bc45125f797758156952bc2d6daf40
```

Retention is consistent with failure handling: the cache and approved images preserve diagnosis. No container, test network, volume, or listener remains.

## Corrected validation event

The first complete validation invocation at 2026-07-14T20:02:13-07:00 returned integrity exit `1` solely because the systemd-user inventory included the queried root directory as an empty relative path, producing a leading comma. The actual non-empty changed entries were already visible and were exactly authorized.

The corrected complete validation used `find -mindepth 1`, repeated every containment, host, runtime, cleanup, storage, and lifecycle check, and returned integrity exit `0` at 2026-07-14T20:03:24-07:00. This correction did not alter the functional result.

## Dimension results

| Dimension | Result | Evidence |
|---|---|---|
| Installation success | **PASS** | Verified official installer exit 0; exact Engine/rootless files installed; exact Compose plugin installed. |
| Daemon startup success | **PASS** | User service loaded, enabled, and active/running. |
| Rootless security success | **PASS** | Rootless security option, UID-1000 stack, approved Unix socket/data root, no rootful daemon/TCP API/firewall change. |
| Functional container success | **FAIL** | First hello-world create returned exit 125; overlay mount EINVAL; kernel rejected case-insensitive-capable backing filesystem. |
| Network success | **NOT RUN** | Stopped after basic failure; image pulls show registry transport only, not the required container DNS/HTTPS or published-port tests. |
| Compose success | **NOT RUN** | Plugin version PASS, but no Compose project was created. |
| Resource-limit success | **NOT RUN** | CPU/memory/PID acceptance and enforcement remain unvalidated. |
| Cleanup success | **PARTIAL PASS** | Zero containers, test networks, volumes, and port listeners; three approved images and cache intentionally retained for diagnosis. |

## Functional failure evidence

Daemon evidence:

```text
failed to mount ... fstype: overlay ... userxattr,index=off ... err: invalid argument
```

Kernel evidence:

```text
overlay: case-insensitive capable filesystem on
/home/deck/.local/share/docker/containerd/daemon/io.containerd.snapshotter.v1.overlayfs/snapshots/5/work
not supported
```

This is the precise blocker. It is not a rootless security failure and not a new USB/UAS storage-reliability event. It prevents the milestone from passing.

## Record checksums

Before this pending validation record was finalized, the independent validator observed:

```text
RUN-0009  365ef9f9db0f5415ca040bd4148adf006eb4cc1e5a4cc46adb409d6d2ebf8809
VAL-0009  f9c3e01934b24c911866ea3234baa86909dc06570af2f0d58a73410c9d993305 (provisional content)
```

VAL-0009 necessarily changes when observed results replace the pending section. A final read-only closing pass recalculates the finished record hash and repeats the critical containment, runtime-security, cleanup, mount, lifecycle, and storage-error gates.

## Final result

- Integrity/containment validation exit: `0` — **PASS**.
- Functional milestone assertion exit: `1` — **FAIL**.
- Installation: **successful and retained**.
- Security boundary: **PASS**.
- Rootless Docker Functional Runtime milestone: **NOT ACHIEVED**.
- Overall validation result: **FAIL due to functional storage-driver incompatibility**.
- AzerothCore acquisition boundary: **PASS; none acquired**.
- Lifecycle: **`S0 — Proposed`**.

This validation does not authorize a storage-driver change, service restart, image cleanup, rollback, or AzerothCore acquisition.
