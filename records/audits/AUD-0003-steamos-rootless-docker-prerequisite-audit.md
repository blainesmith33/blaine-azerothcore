# AUD-0003 — SteamOS rootless Docker prerequisite audit

## Record control

- Identifier: `AUD-0003`
- Evidence date: 2026-07-14
- Evidence interval: 2026-07-14T18:47:39-07:00 through 2026-07-14T18:56:34-07:00
- Authoritative project root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Read-only staging root: `/run/media/deck/blAIne-476GB-SAM/blAIne/azerothcore-staging/`
- Current lifecycle status: `S0 — Proposed`
- Related records: `RUN-0007`, `VAL-0007`, `AUD-0002`, `INC-0001`
- Audit classification: **Rootless Docker prerequisites partially satisfied; no architecture approved**

## Scope

This narrowly scoped audit determines whether the current SteamOS host has the prerequisites to support a future persistent, user-managed, rootless Docker Engine installation under `/home/deck` for a clean, pinned AzerothCore WotLK baseline. It gathers evidence only. It does not select or approve an architecture, install or acquire software, run Docker's rootless installation script, create a Docker data directory, start a daemon, change a service, change SteamOS read-only mode, alter subordinate IDs, change a sysctl or firewall rule, acquire AzerothCore content, or advance lifecycle.

## Method and authority boundary

Read-only shell queries collected host, kernel, session, cgroup, storage, network, package, process, and journal evidence. Two immediate, non-persistent `unshare` tests were run; both created no files, started no daemon, and terminated immediately. Version invocations of `curl`, `wget`, and `git` did not contact a network.

The initial restricted execution mount view reported both external filesystems as `ro`. An explicitly authorized host-view query at 2026-07-14T18:47:56-07:00 reported the authoritative mounts as `rw` and `shared`. This difference is treated as a mount-namespace restriction, not host storage state.

```text
restricted: /dev/sda1 exfat ro,... private,slave
restricted: /dev/sdb1 ext4  ro,... private,slave
host:       /dev/sda1 exfat rw,... shared
host:       /dev/sdb1 ext4  rw,... shared
```

The host view also returned `AUTHORITATIVE_ROOT_TEST_W=YES`. The governance root and staging root resolved to different paths, devices, filesystems, and inode identities. The governance root is on external `/dev/sda1`, not internal `/dev/nvme0n1p4` or `/dev/nvme0n1p8`.

## Pre-write integrity evidence

- The authoritative root existed, was mounted, was host-writable, contained `GOVERNANCE.md`, `README.md`, `docs/`, `instances/`, `linux/`, `modules/`, `records/`, and `windows/`, and was not the staging root.
- The project was not a Git repository: `GIT_REPOSITORY=NO`.
- All three proposed filenames were absent through the final pre-write gate at 2026-07-14T18:55:40-07:00.
- Existing authoritative inventory: 63 files and 52 directories.
- SHA-256 over the sorted `sha256sum` list of the 63 existing files: `af95c46abf47da5082affbfa64bf4129cffa19f72fc9de0a38b4e029695147e9`.
- SHA-256 over sorted existing-file path, size, time, mode, owner, and group metadata: `bf4d2b8b4b1f5300efd78610c6ee5ee5a4bf00396e1f877baf1b1d5e4eb265ca`.
- Staging inventory: 147 files and 121 directories; aggregate content-list SHA-256 `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`; metadata-list SHA-256 `0f05adb46b8e6a57b5ff5375715eea715d2481347dece31ffbb5dec59e3f19e5`.

The project uses SHA-256 manifests for governed transfer validation. This audit used the same `sha256sum` method without creating a manifest file.

## A. Host identity and operating system

| Finding | Exact observation |
|---|---|
| Time | `2026-07-14T18:49:12-07:00`; timezone `America/Phoenix (MST, -0700)`; NTP synchronized and active |
| Host | `steamdeck` (`hostname` executable itself was absent; `hostnamectl` supplied the static hostname) |
| Current user | `deck`; UID `1000`; GID `1000`; groups `deck`, `steamos-log-submitter`, `wheel` |
| Architecture | `x86_64` / hostnamectl `x86-64` |
| Hardware | Valve `Galileo`; AMD Custom APU 0932 |
| SteamOS | SteamOS `3.8.14`, build `20260703.1`, codename `holo`, stable branch |
| Kernel | `6.16.12-valve24.4-1-neptune-616-gfe145653a794`; full uname build dated 2026-07-03 |
| Desktop/session | KDE Plasma, Wayland, active local session `3`, class `user`, not remote |
| HOME | `/home/deck` |
| XDG runtime | `/run/user/1000` |
| Shell | environment and passwd login shell both `/bin/bash` |
| Selected display/bus values | `WAYLAND_DISPLAY=wayland-0`, `DISPLAY=:0`, user bus `unix:path=/run/user/1000/bus` |
| Virtualization query | `systemd-detect-virt` returned `none` with exit `1` (normal no-virtualization status) |

Exact PATH:

```text
/home/deck/.codex/packages/standalone/releases/0.144.4-x86_64-unknown-linux-musl/codex-path:/home/deck/.codex/tmp/arg0/codex-arg0jCa8oW:/home/deck/.codex/packages/standalone/releases/0.144.4-x86_64-unknown-linux-musl/codex-path:/home/deck/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/bin:/var/lib/flatpak/exports/bin:/usr/bin/site_perl:/usr/bin/vendor_perl:/usr/bin/core_perl
```

The Codex command context is attached to the real SteamOS user/session and host systemd, but its ordinary external-drive mount view is restricted. Host-authorized read-only commands were therefore authoritative for external mount state; ordinary namespace `ro` results were not.

## B. SteamOS immutability and update persistence

`steamos-readonly status` returned `enabled` with exit `0` at 18:49, 18:54, and 18:55. No command disabled or changed it. `steamos-devmode status` exists but returned `!! steamos-devmode needs to be run as root` with exit `1`; developer-mode state is unresolved.

Host mounts:

```text
/      /dev/nvme0n1p4 btrfs  rw,relatime,ssd,discard=async,space_cache=v2,subvolid=5,subvol=/
/usr   resolves to the same root btrfs mount
/etc   overlay            rw,relatime,lowerdir=/new_root/etc,upperdir=/new_root/var/lib/overlays/etc/upper,...
/home  /dev/nvme0n1p8 ext4   rw,relatime
```

SteamOS's own status command, rather than the underlying btrfs mount's `rw` flag, is the authority for the OS-image read-only mode. It remained enabled. `/home/deck`, `/home/deck/.config`, and `/home/deck/.local` were owned by UID/GID 1000 and writable. `/home` is a separate ext4 filesystem and is the update-persistent location relevant to a future user-level installation.

`/etc/pacman.conf` was readable. It uses `DBPath = /usr/lib/holo/pacmandb/`; the local database existed there with 1,158 package directories and `pacman -Q` returned 1,158 packages. Pre-write local-database metadata digest was `9c7dd0b0aa3b58d42de9445e6a17f259e90b361f461258ae37cb49fcec5b168c`. No database initialization, synchronization, or package mutation occurred. `/var/log/pacman.log` was absent.

There is no evidence from these checks that SteamOS read-only mode was disabled or that the operating-system image was changed during the audit.

## C. Existing container runtime state

| Component | Path/package/version | Runtime state |
|---|---|---|
| `docker`, `dockerd`, `docker compose`, `docker-compose` | absent from PATH; no Docker package matched | no Docker process or unit; context/data-root/driver/cgroup values not applicable |
| `containerd`, `runc`, `rootlesskit`, `nerdctl` | absent from PATH | no matching process; system containerd unit not found |
| `dockerd-rootless.sh`, `dockerd-rootless-setuptool.sh` | absent from PATH and searched user/system bin locations | not applicable |
| Podman | `/usr/bin/podman`; package `podman 5.5.2-1.1`; `podman version 5.5.2` | no Podman process; system and user service/socket inactive and disabled |
| Distrobox | `/usr/bin/distrobox`; package `distrobox 1.8.1.2-1`; version `1.8.1.2` | executable present; no nested runtime was started |
| Buildah | absent from PATH | not running |

`pgrep` found no user or system `dockerd`, `containerd`, `podman`, or `rootlesskit` process. No inactive daemon was started. Because Docker is absent, there is no active Docker context and no operational Docker data root, storage driver, cgroup driver, or cgroup version to report.

SteamOS already had an empty, root-owned `/var/lib/docker` mount backed by `/dev/nvme0n1p8[/.steamos/offload/var/lib/docker]`; it predated this audit, contained no entries at depth two, used 4,096 bytes, and had mtime/ctime `2026-07-12T20:03:30.268137770-07:00`. This is not evidence of an operational Docker installation and was not created or modified by this audit.

## D. Rootless Docker binary inventory

Package ownership was determined with read-only `pacman -Qo` queries.

| Binary | Exact path | Package/version or observation |
|---|---|---|
| `newuidmap`, `newgidmap` | `/usr/bin/newuidmap`, `/usr/bin/newgidmap` | `shadow 4.18.0-1` |
| `slirp4netns` | absent | missing candidate network helper |
| `vpnkit` | absent | missing candidate network helper |
| `pasta` | `/usr/bin/pasta` | `passt 2025_06_11.0293c6f-1`; version `2025_06_11.0293c6f` |
| `fuse-overlayfs` | `/usr/bin/fuse-overlayfs` | `fuse-overlayfs 1.15-1`; version 1.15, FUSE 3.17.1 |
| `iptables`, `ip6tables` | `/usr/bin/iptables`, `/usr/bin/ip6tables` | `iptables 1:1.8.11-2`; legacy frontend 1.8.11 |
| `nft` | `/usr/bin/nft` | `nftables 1:1.1.3-1`; version 1.1.3 |
| `modprobe` | `/usr/bin/modprobe` | `kmod 34.2-1.1`; version 34.2 |
| `unshare`, `nsenter`, `mount`, `findmnt` | `/usr/bin/...` | `util-linux 2.41.1-1`; version 2.41.1 |
| `curl` | `/usr/bin/curl` | `curl 8.15.0-1`; version 8.15.0 |
| `wget` | `/usr/bin/wget` | `wget 1.25.0-2`; version 1.25.0 |
| `git` | `/usr/bin/git` | `git 2.50.1-3`; version 2.50.1 |
| `tar` | `/usr/bin/tar` | `tar 1.35-2`; GNU tar 1.35 |
| `xz` | `/usr/bin/xz` | `xz 5.8.1-1`; XZ Utils 5.8.1 |
| `ps` | `/usr/bin/ps` | `procps-ng 4.0.5-3` |
| `systemctl`, `loginctl` | `/usr/bin/systemctl`, `/usr/bin/loginctl` | `systemd 257.7-2.5`; systemd 257 |

## E. Subordinate UID/GID configuration

Only the current user's applicable lines were printed:

```text
/etc/subuid: deck:100000:65536
/etc/subgid: deck:100000:65536
```

Each range starts at 100000, contains 65,536 IDs, and ends at 165535 inclusive. Both satisfy the at-least-65,536 criterion. Each file contained exactly one configured allocation line, so no visible allocation can overlap the current user's range. Pre-write SHA-256 for each 18-byte file was `3dbc9d9593703451d1eed51a1af39f8cf6f3a226fee979ac0f6583cff67dc19c`; both had mtime 2026-07-03 and were not changed.

## F. User-namespace support

- `/proc/sys/kernel/unprivileged_userns_clone`: absent or unreadable; this host does not expose that optional knob.
- `user.max_user_namespaces=58833`; other visible per-user namespace maxima queried were also 58,833.
- Kernel config: `CONFIG_NAMESPACES=y`, `CONFIG_USER_NS=y`, `CONFIG_SECCOMP=y`, `CONFIG_CGROUPS=y`, `CONFIG_CGROUP_BPF=y`, `CONFIG_FUSE_FS=y`, `CONFIG_OVERLAY_FS=m`.
- `unshare --user --map-root-user true`: exit `0`.
- `unshare --user --map-auto true`: exit `0`.
- A third observation-only mapping command showed namespace UID/GID 0 mapped to host UID/GID 1000 and exited `0`.

Unprivileged user namespace creation and automatic subordinate-range mapping are therefore directly demonstrated. No persistent namespace state was created.

## G. systemd user-manager support

`/run/user/1000` was UID/GID 1000 mode 0700. `/run/user/1000/bus` was a user-owned socket. `systemctl --user is-system-running` returned `running` with exit `0`; user services were queryable. User manager version was 257.7, control group `/user.slice/user-1000.slice/user@1000.service`.

`loginctl show-user deck` returned `State=active`, sessions `3 2`, runtime path `/run/user/1000`, and `Linger=no`. The manager is currently available during the logged-in user session. It is not currently configured to persist after logout or start independently at boot. This audit did not enable lingering or create, enable, or start a user unit.

## H. Cgroup support

```text
mount: /sys/fs/cgroup cgroup2 rw,nosuid,nodev,noexec,relatime,nsdelegate,memory_recursiveprot
root controllers: cpuset cpu io memory hugetlb pids rdma misc dmem
root subtree: cpu memory pids dmem
user@1000.service: Delegate=yes
delegated controllers: cpu memory pids
self cgroup: /user.slice/user-1000.slice/user@1000.service/app.slice/app-org.kde.konsole-5257.scope
```

At the user-manager cgroup, `cgroup.controllers` and `cgroup.subtree_control` both included `cpu memory pids dmem`; the current user could write that delegated cgroup's `cgroup.procs` and `cgroup.subtree_control`. CPU, memory, and PID controllers are present and delegated to the user manager. This makes rootless Docker CPU, memory, and PID limits technically plausible. No limit was set or exercised, so enforcement is not proven usable by this audit. IO is globally available but was not in the observed delegated set.

## I. Rootless storage-driver conditions

`/home/deck` is backed by `/dev/nvme0n1p8`, ext4, `rw,relatime`, with 1,169,563,488,256 bytes available and 20,890,680 free inodes at the audit point. `/home/deck/.local/share/docker` did not exist and was not created.

| Candidate | Evidence-based classification |
|---|---|
| `overlay2` | **Plausible/conditional, not proven.** Kernel 6.16 has user namespaces, the overlay module is loaded, and the intended ext4 backing filesystem is compatible in principle. An actual later rootless Docker mount/test is required. |
| `fuse-overlayfs` | **Plausible/conditional fallback, not proven.** `/usr/bin/fuse-overlayfs` 1.15 and kernel FUSE support are present on ext4. An actual Docker test is still required. |
| `btrfs` | **Not relevant for the intended `/home/deck` data root.** `/home` is ext4, not btrfs. Using btrfs would require a different data-root/filesystem decision that this audit does not approve. |
| `vfs` | **Technically plausible fallback, not proven; operationally undesirable.** It does not require overlay support but has materially worse space/performance characteristics. |

No storage driver is claimed to work. The internal root filesystem has only about 1.406 GB available and is not the intended rootless data location.

## J. Network and port conditions

- Active LAN interface: `wlan0`, IPv4 `192.168.1.65/24`; IPv6 ULA addresses `fdcb:e333:913:8cfe:6929:58c7:939f:65c9/64` and `fdcb:e333:913:8cfe:ea8d:a6ff:fee1:1e48/64`, plus link-local IPv6.
- Default IPv4 route: via `192.168.1.1` on `wlan0`, source `192.168.1.65`, metric 600.
- Ethernet USB interface `enp4s0f3u1u4` was down/no carrier.
- No listening TCP or UDP socket used 3306, 3724, 7878, or 8085.
- All four planned ports are above 1024. `net.ipv4.ip_unprivileged_port_start=1024`.
- Firewalld was active and enabled; active/default zone `public` on `wlan0` allowed `1024-65535/tcp` and `1024-65535/udp`, with forwarding enabled and masquerade disabled.
- `nft list ruleset` was denied to the unprivileged user (exit 1); `iptables -S` and `ip6tables -S` were denied on the lock file (exit 4). Firewalld's read-only D-Bus query succeeded.

A future rootless published port bound to a non-loopback host address would be expected to pass the currently reported firewalld high-port policy and be potentially LAN reachable. Actual reachability depends on the future rootless port driver, bind address, daemon configuration, Wi-Fi/LAN policy, and a real connectivity test; it is not proven here. No port was opened and no Docker API TCP socket was exposed.

## K. Resource baseline

| Resource | Observation |
|---|---|
| CPU | 8 logical CPUs, 4 physical cores, 1 socket, 2 threads/core; AMD Custom APU 0932; AMD-V present |
| RAM | 15,524,212,736 bytes total; 11,271,843,840 bytes available at 18:52:56 |
| Swap/ZRAM | 7,762,079,744-byte `/dev/zram0`, zstd, priority 100; about 1.43 GB used |
| Load | `1.24, 1.64, 1.92`; uptime 7:08 |
| Internal root | 5,368,709,120 bytes total; 1,406,775,296 available |
| Internal `/home` | 1,982,522,322,944 bytes total; 1,169,563,488,256 available |
| Temperatures | ACPI 74°C; Wi-Fi 54°C; NVMe 48.9°C; GPU edge 61°C; battery 34°C; fan 3485 RPM |
| Power profile | `powerprofilesctl`, ACPI platform-profile files, `tuned-adm`, and `ryzenadj` unavailable; current profile indeterminate |

No stress test was run.

## L. Project storage

| Path | Device/filesystem/transport | Host mount and capacity | Identity |
|---|---|---|---|
| Governance root | `/dev/sda1`, exFAT, USB | `rw,nosuid,nodev,relatime,...,errors=remount-ro`; 1,024,175,898,624 bytes total; 1,024,159,514,624 available | Sabrent bridge; model reported by lsblk as `Micron_2400_MTFDKBK1T0QFM`; serial `234544AAB642`; block RO flag 0; running |
| Staging root | `/dev/sdb1`, ext4, USB | `rw,nosuid,nodev,noatime,errors=remount-ro`; 502,921,060,352 bytes total; 477,177,643,008 available; 31,255,344 free inodes | Samsung SSD 830 Series; serial `S0Y0NEAC400931`; block RO flag 0; running |

Both are USB storage. The staging root was inspected read-only even though its authoritative host mount was writable. The restricted namespace's `ro` results do not override the verified host evidence.

## M. Storage reliability

The current-boot kernel journal still contains the warning documented by AUD-0002/INC-0001: many earlier USB disconnect, UAS recovery/reset, device-offline, buffer I/O, JBD2 journal, ext4 shutdown, and cache-sync failure messages. The filtered current-boot set contained 513 lines at 18:48 and hash `3b4b89259f3396b6687f5fd6bdea666ec4dc126c3d038b0dbc863d68464bfdf6`.

For the staging filesystem UUID `70913570-fb08-4b22-a534-a8e658cb86d4`, a 16:26 attachment under dynamic name `/dev/sda1` suffered USB disconnect, device-offline writes, ext4 journal shutdown, JBD2 I/O error, and cache-sync failure. The device later attached as `/dev/sdb1` and was mounted r/w at 17:25:57. At the final pre-write gate, both project devices were running, block RO 0, and host-mounted r/w. There were zero matching new storage-error lines from audit start 18:47:39 through 18:55:40.

This is positive evidence only for the bounded interval. It does not resolve the existing intermittent USB/UAS reliability problem. No claim of resolution is made.

## Rootless prerequisite matrix

| Prerequisite | Result | Meaning |
|---|---|---|
| Persistent writable user location | Satisfied | `/home/deck` is writable ext4 on a separate internal partition |
| SteamOS OS-image immutability retained | Satisfied | `steamos-readonly status` remained `enabled` |
| `newuidmap`/`newgidmap` | Satisfied | present and package-owned |
| subordinate UID/GID ranges | Satisfied | 65,536 each, non-overlapping visibly |
| unprivileged user namespaces | Satisfied | kernel config and two successful immediate tests |
| systemd user manager and bus | Satisfied while logged in | running and queryable |
| unattended persistence after logout/boot | **Blocked in current state** | `Linger=no`; no change authorized |
| cgroup v2 | Satisfied | cgroup2 mounted with relevant controllers |
| CPU/memory/PID delegation | Satisfied as delegation evidence | `user@1000.service` delegates all three; actual limit enforcement untested |
| storage-driver host conditions | Partially satisfied | overlay2 and fuse-overlayfs plausible; neither tested with Docker |
| rootless network helper | Partially satisfied | `pasta` exists; `slirp4netns` and `vpnkit` absent; eventual Docker/RootlessKit compatibility untested |
| Docker Engine/rootless extras/Compose | Not present by design | acquisition was prohibited; immediate runtime validation impossible |
| planned port availability | Satisfied at observation point | no conflicts; high ports allowed by reported firewalld zone |
| persistent storage reliability | Conditional | internal `/home` is suitable in principle; external USB warning remains unresolved for project/staging evidence paths |

## N. Installation-path feasibility classifications

These are audit classifications, not architecture approval.

### 1. Official user-level rootless Docker under `/home/deck` — **remain conditional**

- Required later changes: acquire/install Docker's user-level Engine/rootless extras and Compose under the user profile; create user configuration/service and data root; decide whether to enable lingering for boot/logout persistence.
- Update persistence: strong for files under `/home/deck`; independent of SteamOS image replacement, subject to user-level compatibility maintenance.
- Security: least host privilege among Docker candidates; daemon and containers remain in user namespaces, but the user-controlled daemon still grants substantial authority over that user's data.
- Complexity: moderate; needs PATH/environment, rootless networking, storage-driver, cgroup, and systemd-user validation.
- Compose/AzerothCore: expected native Docker CLI/Compose compatibility once exact versions are installed and pinned; not tested.
- Automatic start: user service can operate in the logged-in manager now; unattended boot/logout persistence is blocked by `Linger=no` unless separately approved later.
- Data root: intended default `/home/deck/.local/share/docker`, currently absent; ample internal ext4 capacity.
- Audit blockers: Docker/rootless extras/Compose absent; supported network-driver behavior, storage driver, cgroup limits, and LAN publication untested; lingering disabled.

### 2. Rootful Docker in the SteamOS image — **rejected for this host plan**

- Required changes: alter the SteamOS image/package state, install privileged daemon/client components, create/enable system units, and manage privileged firewall/storage integration.
- Update persistence: poor; OS-image updates can replace or invalidate image-level changes.
- Security: root daemon materially expands host privilege and attack surface.
- Complexity: high on immutable SteamOS despite conventional Docker behavior.
- Compose/AzerothCore: normally compatible, but no implementation is present or tested.
- Automatic start: possible via a system unit only after prohibited root/system changes.
- Data root: pre-existing empty `/var/lib/docker` offload mount exists, but using it would be a rootful architecture decision.
- Blockers: conflicts with the persistent user-managed `/home/deck` objective and requires changes expressly outside this audit; read-only mode is enabled.

### 3. Docker Engine static binaries — **remain conditional**

- Required changes: acquire and pin Engine/CLI/rootless support binaries and Compose separately; construct user service/environment and update procedure manually.
- Update persistence: good if entirely under `/home/deck`, but manual lifecycle/security updates are the user's responsibility.
- Security: can be rootless if correctly assembled; static placement alone does not make it rootless.
- Complexity: higher than the official user-level mechanism; dependency and version coherence must be governed manually.
- Compose/AzerothCore: conditional on separately installed compatible Compose plugin and exact workflow testing.
- Automatic start/data root: same user-manager/lingering constraint and intended internal ext4 data root as candidate 1.
- Blockers: all Docker static payloads and rootless scripts absent; no versions or compatibility can be validated.

### 4. Docker inside Distrobox/another container — **rejected as the primary permanent path**

- Required changes: create/configure a Distrobox or other parent environment and then install/nest Docker inside it, potentially with extra namespace/privilege/socket arrangements.
- Update persistence: parent container/home data can persist, but host/parent/inner runtime layers add coupled upgrade state.
- Security: nested privilege and socket choices are harder to reason about; rootless-inside-rootless does not automatically improve isolation.
- Complexity: high; startup, storage, network publication, cgroups, and troubleshooting span layers.
- Compose/AzerothCore: potentially runnable but not equivalent until nested behavior is proven against the main-repository workflow.
- Automatic start: awkward and still constrained by the parent environment and user manager/lingering.
- Data root: nested container storage adds duplication and indirection under home.
- Blockers: Distrobox exists, but no environment or inner Docker exists; no nested validation supports this path.

### 5. Permanent Podman substitution — **rejected as a permanent substitution**

- Required changes: configure Podman/Compose-compatibility tooling, user units, networking, and storage.
- Update persistence: user state can persist under home; SteamOS-packaged binary update behavior follows the OS image.
- Security: rootless Podman can reduce daemon privilege, but it is a different runtime/compatibility surface.
- Complexity: moderate, with Docker-Compose compatibility exceptions and translation risk.
- Compose/AzerothCore: not assumed equivalent; Podman is installed but inactive, and no AzerothCore workflow was tested.
- Automatic start: user units are possible but current `Linger=no` still matters.
- Data root: separate Podman storage; it must not be silently conflated with Docker data.
- Blocker/decision constraint: the user expressly intends Docker and forbids silent permanent Podman substitution.

## Confirmed blockers

1. A persistent user manager across logout/unattended boot is not currently configured: `Linger=no`.
2. Docker Engine, Docker CLI, Compose, RootlessKit, rootless setup scripts, and Docker runtime payloads are absent, so no end-to-end rootless Docker, Compose, driver, or cgroup-limit test is possible in this no-acquisition audit.
3. `slirp4netns` and `vpnkit` are absent; `pasta` exists, but compatibility with the future pinned Docker/RootlessKit version is not established.
4. The existing external USB/UAS reliability warning is unresolved and constrains reliance on the project and staging USB devices even though no new audit-interval errors occurred.

## Unresolved questions

- Whether future explicit approval will permit lingering, or whether login-only manager lifetime is acceptable.
- Which exact pinned Docker Engine, rootless extras, RootlessKit, Compose, and network-helper versions would be selected.
- Whether overlay2, fuse-overlayfs, cgroup CPU/memory/PID limits, and LAN port publication work in an actual later rootless Docker test.
- Whether the future bind policy should expose all game/database ports to the LAN; current firewalld policy allows high ports, but no runtime connectivity test occurred.
- SteamOS developer-mode state; the installed status command required root.
- Raw nftables/iptables rules beyond the successful firewalld zone view; unprivileged reads were denied.
- Current power profile; no readable supported interface was found.
- Long-term resolution of the documented USB/UAS disconnect and I/O failure history.

## Audit conclusion

The current host has the core kernel, subordinate-ID, user-namespace, systemd-user, cgroup-v2, writable persistent-home, capacity, and plausible storage-driver foundations for a future rootless Docker evaluation. Readiness is **partially satisfied**, not proven viable end to end. Missing runtime payloads, disabled lingering, untested rootless networking/storage/cgroup behavior, and unresolved USB reliability prevent a satisfied classification.

**No installation, download, software acquisition, service change, configuration change, Docker context change, data-directory creation, AzerothCore acquisition, module acquisition, addon acquisition, or architecture approval occurred. The project lifecycle remains `S0 — Proposed`.**
