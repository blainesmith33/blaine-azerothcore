# VAL-0010 — Rootless Docker fuse-overlayfs Remediation Validation

## Record control

- Identifier: `VAL-0010`
- Validation date: 2026-07-14
- Subject: `ADR-0011` and `RUN-0010`
- Storage-remediation status: **PASS**
- Containment/integrity status: **PASS with stated staging-metadata-method limitation**
- Overall functional milestone: **FAIL — pasta implicit socket is represented as wildcard while published**
- Lifecycle: `S0 — Proposed`
- Final corrected validation: 2026-07-14T20:35:21-07:00, exit 0

## Independent validation method

Validation re-parsed and daemon-validated the installed JSON; queried Docker, systemd, context, socket, processes, mounts, firewalld, package inventory, sysctls, subordinate IDs, profiles, resources, listeners, and current-boot storage logs; re-counted visible/hidden Docker data; and recomputed authoritative prior-file and staging content digests. It did not rely solely on RUN-0010 narrative assertions.

## Required integrity and configuration assertions

| Assertion | Result | Independent evidence |
|---|---|---|
| Exactly three governance files created | PASS | ADR-0011, RUN-0010, and VAL-0010 are the only target additions; final project count is expected to be 74 from baseline 71. |
| No prior project/governance file changed | PASS | Excluding the three targets, content-list SHA-256 remained `560b88b846d2bc37ee7cf2eaf00b1202c7c6d2f541418091480134a7fea9c915`. |
| Staging remained unchanged | PASS for content/count/write boundary | 147 files, 121 directories, content SHA-256 remained `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`; no command targeted staging for writing. The recorded pre-change metadata digest was `0f05adb46b8e6a57b5ff5375715eea715d2481347dece31ffbb5dec59e3f19e5`; the exact earlier metadata-manifest serialization was not recovered for a like-for-like final recomputation, so metadata equivalence is not overstated beyond the write boundary, counts, and content. |
| daemon.json approved keys only | PASS | Exactly `features.containerd-snapshotter=false` and `storage-driver=fuse-overlayfs`; no other key. |
| JSON valid and accepted | PASS | `jq -e` exit 0; corrected operation-local `dockerd --validate` reports `configuration OK`, exit 0. |
| Config ownership/mode | PASS | deck:deck, mode 0600, 98 bytes, SHA-256 `b58ec579adc94cbb0777e459f25257479febd84927a60b6d098c70326adb6193`. |

## Runtime and containment assertions

| Assertion | Result | Independent evidence |
|---|---|---|
| Engine remains 29.6.1 | PASS | Client 29.6.1; server 29.6.1. |
| Compose remains v5.3.1 | PASS | `docker compose version` reports v5.3.1. |
| RootlessKit remains 3.0.1 | PASS | Version and Docker server component output. |
| Docker remains rootless | PASS | Security options include rootless; dockerd/RootlessKit UID only 1000. |
| Context/endpoint approved | PASS | `rootless`; `unix:///run/user/1000/docker.sock`. |
| No Docker TCP listener | PASS | Zero listeners on 2375/2376; no daemon TCP endpoint. |
| No rootful daemon/socket | PASS | Root docker.service/socket inactive; `/var/run/docker.sock` absent. |
| Data root/internal ext4 | PASS | `/home/deck/.local/share/docker`; backing `/dev/nvme0n1p8`, ext4, read/write. |
| Storage driver fuse-overlayfs | PASS | Docker reports `fuse-overlayfs` 1.15. |
| Containerd snapshotter image storage disabled | PASS | Startup reports `containerd-snapshotter=false`; `DriverStatus=[]`; no Docker `driver-type io.containerd.snapshotter.v1`. Managed containerd remains for runtime duties as expected. |
| Hidden old backend preserved | PASS | 19,357,601 bytes, 531 files, and all three RUN-0009 digest blobs present; no migration/deletion. |
| Rootless network unchanged | PASS | pasta with implicit port driver; no forced driver/config key. |
| Linger/SteamOS state | PASS | `Linger=no`; `steamos-readonly status` enabled. |

## Functional test results

| Area | Result | Evidence |
|---|---|---|
| Configuration | PASS | Valid JSON, exact approved effective keys, daemon accepts. |
| Service startup | PASS | User service restart exit 0, ready probe 1, loaded/enabled/active/running. |
| Rootless containment | PASS | UID 1000, rootless option/context/socket, no root/TCP daemon. |
| Storage backend | PASS | fuse-overlayfs active; first container starts; old store hidden/preserved. |
| Basic container | PASS | Expected hello-world text, container exit 0, removed. |
| DNS/HTTPS | PASS | DNS answers; verified TLS/HTTPS; expected registry HTTP 401; removed. |
| Loopback publication | **FAIL on literal listener shape** | Docker requests/reports 127.0.0.1; loopback body succeeds; same-host LAN-address request fails; but `ss`/`/proc/net/tcp` show UID-1000 `0.0.0.0:18080` while active. |
| Resource limits | PASS | Effective cgroup v2 `cpu.max=50000 100000`, `memory.max=67108864`, `pids.max=64`; no OOM. |
| Compose | **PASS orchestration; FAIL inherited network assertion** | config/up/ps/body/down/cleanup pass, LAN-address request fails, but active pasta socket appears `0.0.0.0:18081`. |
| Build path | INFORMATIONAL BLOCKER | Buildx absent; legacy build help only; no build/download performed. |
| Cleanup | PASS | Zero containers, visible images, volumes, RUN-0010/non-default networks, and listeners on 18080/18081. |
| Storage reliability | PASS for bounded window | Zero new relevant current-boot log matches; historical staging warning remains unresolved. |
| Overall functional milestone | **FAIL** | All storage/runtime tests pass, but literal loopback listener binding and independent LAN non-exposure proof are not complete. |

## Host-state assertions

| Assertion | Result | Evidence |
|---|---|---|
| Package database unchanged | PASS | Package count remains 1,158; no pacman modifying command or package installation occurred. |
| Firewall unchanged | PASS | firewalld running, default public, runtime/permanent ports remain `1024-65535/tcp 1024-65535/udp`; daemon log says rootless firewalld management skipped. |
| Sysctls unchanged | PASS | `user.max_user_namespaces=58833`, unprivileged port start 1024, host ip_forward 0, matching pre-change. Rootless namespace forwarding is transient and not a host sysctl change. |
| Subordinate IDs unchanged | PASS | `/etc/subuid` and `/etc/subgid` SHA-256 both remain `3dbc9d9593703451d1eed51a1af39f8cf6f3a226fee979ac0f6583cff67dc19c`. |
| Shell profiles unchanged | PASS | `.bashrc` and `.bash_profile` hashes match pre-change; `.profile` and `.zshrc` remain absent. |
| No unauthorized host path changed | PASS within captured boundary | Permanent writes are confined to authorized `daemon.json`, RUN-0010 cache, active Docker data, transient `/run/user/1000`, and three governance records. |
| No AzerothCore/client content | PASS | No acquisition command or AzerothCore image; final visible Docker images zero; governance/cache inventory contains only remediation evidence. |
| Lifecycle remains S0 | PASS | README and GOVERNANCE still state `S0 — Proposed`. |

## Image and cleanup validation

The three visible classic-backend images resolved to the same immutable RUN-0009 RepoDigests and were removed after testing. No visible shared test layer remains. Hidden RUN-0009 containerd content was deliberately not deleted.

Final Docker counts: containers 0, images 0, volumes 0, RUN-0010 networks 0, non-default networks 0. Final listener counts: 18080 = 0; 18081 = 0.

## Governance integrity conclusion

Configuration, service startup, rootless containment, storage backend, basic execution, DNS/HTTPS, resource enforcement, orchestration mechanics, cleanup, and bounded storage reliability pass. The storage remediation is successful.

The milestone cannot receive an overall PASS because the installed, unforced pasta/implicit port-driver path creates a wildcard kernel socket for a Docker publication requested and reported as 127.0.0.1. Same-host LAN-address connection attempts failed, but the literal binding requirement and independent remote-LAN non-exposure proof remain unresolved. No unauthorized driver change was attempted.

No AzerothCore source, database, module, addon, client, client modification, credential, proprietary asset, or AzerothCore image was acquired. The lifecycle remains **S0 — Proposed**.
