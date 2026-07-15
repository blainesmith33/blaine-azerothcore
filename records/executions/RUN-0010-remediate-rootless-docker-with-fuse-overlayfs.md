# RUN-0010 — Remediate Rootless Docker with fuse-overlayfs

## Record control

- Identifier: `RUN-0010`
- Execution date: 2026-07-14
- Operation start: 2026-07-14T20:12:38-07:00
- Configuration installation: 2026-07-14T20:19:26-07:00
- User-service restart: 2026-07-14T20:20:25-07:00
- Authoritative root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Authorized cache: `/home/deck/.cache/blAIne/azerothcore/RUN-0010-fuse-overlayfs-remediation/`
- Governing decisions: `ADR-0010`, `ADR-0011`
- Storage-remediation result: **PASS**
- Rootless containment result: **PASS**
- Functional milestone result: **FAIL — literal loopback listener-shape assertion not satisfied by pasta implicit port forwarding**
- Lifecycle before and after: `S0 — Proposed`

## Authorization

This operation was limited to the rootless daemon storage configuration, one restart of the `deck` user’s `docker.service`, the three approved disposable images, RUN-0010 containers/networks/cache files, and exactly ADR-0011, RUN-0010, and VAL-0010 in governance.

No sudo, root shell, root systemd change, lingering change, SteamOS immutability change, pacman transaction, `/etc`, `/usr`, `/usr/local`, firewall, host nftables/iptables policy, host sysctl, subordinate-ID, shell-profile, Docker TCP API, external data root, destructive rollback, `vfs` fallback, network-driver change, or AzerothCore/client acquisition was authorized or performed.

## Pre-change state

All mandatory gates passed before the first write:

- The authoritative root was `/dev/sda1`, exFAT, mounted read/write; staging was distinct `/dev/sdb1`, ext4, and was only read.
- RUN-0009 and VAL-0009 existed. VAL-0009 reported containment/integrity PASS and functional milestone FAIL at the containerd overlayfs snapshotter.
- Lifecycle records reported `S0 — Proposed`; ADR-0011 was the safe next decision identifier; all three target names were absent.
- Project baseline: 71 files, 52 directories; prior-file content-list SHA-256 `560b88b846d2bc37ee7cf2eaf00b1202c7c6d2f541418091480134a7fea9c915`.
- Staging baseline: 147 files, 121 directories; content SHA-256 `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`; recorded metadata baseline `0f05adb46b8e6a57b5ff5375715eea715d2481347dece31ffbb5dec59e3f19e5`.
- The current-boot relevant-storage-error baseline was captured; the bounded operation window initially contained zero new matching lines.
- Docker client/server 29.6.1, Compose v5.3.1, and RootlessKit 3.0.1 were active as UID 1000 through the rootless context and `unix:///run/user/1000/docker.sock`.
- Security options included rootless; the old driver was overlayfs through `io.containerd.snapshotter.v1`; the data root was `/home/deck/.local/share/docker` on internal `/dev/nvme0n1p8` ext4.
- No root service/socket, `/var/run/docker.sock`, Docker TCP listener, container, volume, RUN-0009 network, or test-port listener existed. `Linger=no`; SteamOS read-only status was enabled.
- Pacman package count was 1,158. Firewall, sysctl, subordinate-ID, and profile baselines matched RUN-0009.
- `/home/deck/.config/docker/` and `daemon.json` did not exist, so no backup was required and no unrelated key needed merging.

## fuse-overlayfs and FUSE evidence

| Item | Finding |
|---|---|
| Binary | `/usr/bin/fuse-overlayfs`; root:root; mode 0755; 109,280 bytes; x86-64 PIE |
| SHA-256 | `e39a5297af619df01a6d76505d594f02277dd6886f257f8b57f5724667a90678` |
| Version | fuse-overlayfs 1.15; FUSE library 3.17.1; kernel interface 7.40 |
| Device | `/dev/fuse`, character device, mode 0666; readable and writable by deck |
| Mount helpers | `/usr/bin/fusermount` 2.9.9 and `/usr/bin/fusermount3` 3.17.1, setuid root as installed |
| Kernel support | `fuse`, `fuseblk`, and `fusectl` visible; `CONFIG_FUSE_FS=y` |

## Prior daemon configuration

`/home/deck/.config/docker/daemon.json` was absent. The directory was also absent. No credentials, unexplained configuration, `hosts`, insecure-registry entry, or conflicting key existed. No backup file was needed.

## Official-source confirmation

Official text/source metadata was inspected read-only on 2026-07-14; no executable was downloaded.

- `https://docs.docker.com/engine/storage/containerd/`: Engine 29 fresh installations default to the containerd image store; overlayfs is its default snapshotter; switching stores hides prior data without deleting it.
- `https://docs.docker.com/reference/cli/dockerd/`: boolean feature switches, `storage-driver`, and `dockerd --validate` are supported.
- `https://docs.docker.com/engine/storage/drivers/select-storage-driver/`: `fuse-overlayfs` is the preferred rootless compatibility driver when native rootless overlay cannot be used; `vfs` is a poor-performance testing fallback.
- `https://docs.docker.com/engine/daemon/` and `https://docs.docker.com/engine/security/rootless/tips/`: rootless daemon configuration belongs at `/home/deck/.config/docker/daemon.json`; data and socket defaults match ADR-0010.
- `https://github.com/moby/moby/blob/master/daemon/daemon.go`: Moby records whether snapshotter image storage is selected and determines image-store choice at startup.

## Configuration validation and exact file change

The prepared and installed effective configuration was:

```json
{
  "features": {
    "containerd-snapshotter": false
  },
  "storage-driver": "fuse-overlayfs"
}
```

- Prepared/installed SHA-256: `b58ec579adc94cbb0777e459f25257479febd84927a60b6d098c70326adb6193`.
- Independent `jq -e` parse: exit 0.
- `HOME=/home/deck PATH=/home/deck/bin:/usr/bin:/bin /home/deck/bin/dockerd --validate --config-file=...`: `configuration OK`, exit 0.
- Installation used an owner-only temporary file and atomic rename to `/home/deck/.config/docker/daemon.json`.
- Final owner/mode/size: `deck:deck`, 0600, 98 bytes.
- No `hosts`, TCP, insecure-registry, data-root, or network key was added.

One post-write validation attempt omitted the required operation-local PATH. It returned exit 1 (`invalid userland-proxy-path`) because the static daemon could not resolve sibling `docker-proxy`. No service restart had occurred and the file was not changed. Repeating the identical validation with `/home/deck/bin` restored returned `configuration OK`, exit 0.

## Service restart result

`systemctl --user restart docker.service` ran once and exited 0. Docker was ready on bounded probe 1. The unit remained loaded, enabled, active, and running at `/home/deck/.config/systemd/user/docker.service`; main RootlessKit PID after restart was 73883.

Startup evidence included:

```text
Running in rootless mode
Running with RootlessKit integration
[graphdriver] trying configured driver: fuse-overlayfs
Docker daemon ... containerd-snapshotter=false storage-driver=fuse-overlayfs version=29.6.1
skipping firewalld management for rootless mode
API listen on /run/user/1000/docker.sock
```

The managed containerd runtime still loads its own plugins for runtime duties. Docker image storage is nevertheless classic: the daemon explicitly reports `containerd-snapshotter=false`, `Storage Driver: fuse-overlayfs`, empty `DriverStatus`, and no `driver-type io.containerd.snapshotter.v1` in Docker information.

## Rootless security revalidation and Docker information

| Property | Result |
|---|---|
| Docker client/server | 29.6.1 / 29.6.1 |
| Compose | v5.3.1 |
| RootlessKit | 3.0.1 |
| containerd / runc | v2.2.5 / 1.3.6 |
| Context / endpoint | `rootless` / `unix:///run/user/1000/docker.sock` |
| Security options | seccomp builtin, rootless, cgroupns |
| Socket | deck:deck, UID/GID 1000, mode reported 1660 socket bits, no TCP endpoint |
| Docker root | `/home/deck/.local/share/docker` on `/dev/nvme0n1p8` ext4 |
| Storage driver | `fuse-overlayfs` |
| Cgroup | v2, systemd |
| Logging | json-file |
| Network / port driver | RootlessKit `pasta` / `implicit`, unchanged and unforced |
| Root system services | docker.service/socket inactive; `/var/run/docker.sock` absent |
| Daemon UID | only 1000 |
| Linger / SteamOS readonly | no / enabled |
| Docker warnings | no cpuset; no io.weight/io.max delegation; unrelated to required CPU, memory, PID checks |

No rootful daemon, rootful socket, Docker TCP listener, host firewall change, root system unit, unsafe socket ownership, external data root, or non-root dockerd UID appeared.

## Backend transition and hidden prior data

Before the switch, the containerd backend held the three RUN-0009 images and measured 19,357,601 bytes across 531 files. With classic storage active, Docker initially showed zero visible images, proving store separation rather than deletion.

After all tests and cleanup, `/home/deck/.local/share/docker/containerd` still measured exactly 19,357,601 bytes and 531 files. The content blobs named by all three prior RepoDigests remained present. The directory is hidden from the classic visible inventory and was not migrated or deleted. Managed-containerd startup can update its own metadata; no claim of byte-for-byte metadata immutability is made. Deferred cleanup remains required for deletion.

The first corrected post-restart aggregate content hash was `3a80314388db8635c60cd550e00cbc5b1309d398066d679e41adf1dcc441548c`. An earlier read-only hash command used relative names from the wrong working directory and emitted file-not-found diagnostics; it altered nothing.

## Image digests

The same three approved tags were pulled into the classic backend. Each immutable RepoDigest exactly matched RUN-0009:

| Tag | RepoDigest |
|---|---|
| `hello-world:latest` | `hello-world@sha256:96498ffd522e70807ab6384a5c0485a79b9c7c08ca79ba08623edcad1054e62d` |
| `busybox:1.37.0` | `busybox@sha256:9532d8c39891ca2ecde4d30d7710e01fb739c87a8b9299685c63704296b16028` |
| `alpine:3.23` | `alpine@sha256:fd791d74b68913cbb027c6546007b3f0d3bc45125f797758156952bc2d6daf40` |

There was no conflicting tag resolution or unexplained platform distinction.

## Functional tests

### Basic engine — PASS

`blaine-run0010-basic` was created on `fuse-overlayfs`, started, printed `Hello from Docker!`, exited 0 with no OOM/error, and was removed. This directly proves the RUN-0009 storage blocker was corrected.

### DNS and HTTPS — PASS

`blaine-run0010-dns-https` used unmodified `alpine:3.23`. `nslookup registry-1.docker.io` returned IPv4 and IPv6 answers. BusyBox `wget` connected over HTTPS with normal certificate validation and received expected `HTTP/1.1 401 Unauthorized`; the assertion container exited 0 and was removed. No package was installed and no certificate bypass or insecure registry was used.

### Loopback-only publication — EFFECTIVE REACHABILITY PASS; LITERAL LISTENER-SHAPE FAIL

Docker recorded `127.0.0.1:18080:8080`; host loopback returned the exact body `blaine-run0010-loopback-ok`; the host LAN address `192.168.1.65:18080` was not reachable; runtime firewall state was unchanged; the container was unprivileged, used bridge networking, and was removed. The listener disappeared after removal.

However, both the test and a focused repeat probe showed the UID-1000 pasta implicit forwarding socket as:

```text
0.0.0.0:18080 uid:1000 cgroup:/user.slice/user-1000.slice/user@1000.service/app.slice/docker.service
```

Docker's binding metadata and effective LAN-address test are loopback-only, but the kernel socket is not literally bound only to `127.0.0.1`. No remote LAN peer test was available. The requested literal assertion therefore fails and LAN non-exposure is not promoted beyond the tested same-host result. The network/port driver was not changed or forced.

### Resource limits — PASS

`blaine-run0010-limits` remained running with no OOM and was removed. Docker recorded 0.50 CPU, 64 MiB memory, and PID limit 64. Its delegated cgroup v2 scope showed:

```text
cpu.max=50000 100000
memory.max=67108864
pids.max=64
memory.events: oom=0, oom_kill=0, oom_group_kill=0
```

These are observed effective values, not merely accepted Docker configuration.

### Docker Compose — ORCHESTRATION PASS; NETWORK ASSERTION INHERITS LISTENER-SHAPE FAIL

The cache-only one-service `compose.yaml` used `busybox:1.37.0`, no mounts/volumes/secrets/privilege/host networking, and requested `127.0.0.1:18081:8080` under project `blaine-run0010-compose`.

`docker compose config`, `up -d`, `ps`, loopback body `blaine-run0010-compose-ok`, and `down --remove-orphans` all passed. The LAN-address request failed and firewall state did not change. The project container and network were removed and no volume existed. While active, pasta again represented the forwarder socket as `0.0.0.0:18081`, so the Compose network-boundary assertion inherits the same literal failure.

## Build-path prerequisite probe

- `docker buildx version`: exit 1, `docker: unknown command: docker buildx`.
- Installed CLI plugins: only `docker-compose`.
- `docker build --help`: exit 0 but warns that the legacy builder is deprecated and Buildx should be installed.

Buildx is the next AzerothCore build prerequisite. The probe is informative and does not itself fail the rootless runtime. No Buildx download, Dockerfile, build, or temporary build image was created.

## Cleanup

All newly visible classic-backend test images were untagged and deleted. Final state:

- containers: 0;
- visible images: 0;
- volumes: 0;
- RUN-0010 networks: 0;
- non-default networks: 0 (only bridge, host, none);
- listeners on 18080 and 18081: 0.

The rootless installation, Compose plugin, context, user service, approved data root, `daemon.json`, and diagnostic cache remain. Hidden RUN-0009 containerd data remains intact by count/size and blob presence.

## Storage-error checks

The current-boot baseline was captured before change. The exact bounded filter for USB disconnect, UAS error/reset/offline, device offline/rejection, I/O error, ext4/journal error, remount, cache-sync failure, and block reset returned zero new lines through final validation. This does not resolve the historical staging-drive warning.

## Files and directories created or changed

Authorized host changes:

- created `/home/deck/.config/docker/daemon.json` (no predecessor; no backup needed);
- created the owner-only RUN-0010 cache and its prepared config, official-source evidence, command/evidence manifests, Compose YAML, and record drafts;
- changed transient/classic Docker data only through the approved images/workloads and cleanup;
- restarted only `docker.service` in the deck user manager;
- created exactly ADR-0011, RUN-0010, and VAL-0010 in governance.

No prior governance file or staging content changed. Excluding the three targets, the authoritative prior-file content-list digest remained `560b88b846d2bc37ee7cf2eaf00b1202c7c6d2f541418091480134a7fea9c915`. Staging retained 147 files, 121 directories, and content digest `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`.

## Failed attempts and corrections

1. Post-write daemon validation without the operation-local PATH returned exit 1 because `docker-proxy` was not resolvable. Corrected PATH validation returned `configuration OK`; the configuration was unchanged and the service had not yet restarted.
2. The first read-only hidden-backend aggregate hash used relative paths from the wrong working directory. It emitted file-not-found messages. Re-running from the backend root produced the recorded digest; neither command wrote data.
3. The requested loopback binding produced a pasta implicit wildcard kernel socket. This was reproduced and is recorded as an unresolved validation failure, not corrected by an unauthorized network-driver change.
4. The first post-record validation wrapper had a shell-quoting syntax error at its `jq` expression and exited 2 before that assertion; it changed nothing.
5. The corrected wrapper passed every assertion through the final storage-log check, where `pipefail` converted the expected zero-match `grep` into exit 1. The final wrapper handled zero matches explicitly and completed at 2026-07-14T20:35:21-07:00 with `FINAL_VALIDATION_EXIT=0`.

## Remaining limitations

- Literal 127.0.0.1-only kernel socket binding is not satisfied with the existing pasta/implicit implementation, even though Docker metadata and the same-host LAN test enforce the requested behavior.
- LAN non-exposure lacks an independent remote-peer test.
- Buildx is absent.
- Logout/reboot persistence remains unvalidated because lingering remains disabled.
- Hidden containerd-backend cleanup/migration is deferred.
- Historical external staging-drive reliability remains unresolved.
- cpuset and I/O controller limits are unavailable; required CPU quota, memory, and PID limits passed.

## Exact rollback procedure — not executed

Because `daemon.json` did not previously exist:

1. Preserve RUN-0010 logs, records, config hash, and backend inventories.
2. Confirm no containers are running.
3. `systemctl --user stop docker.service`.
4. Atomically move the RUN-0010-created `/home/deck/.config/docker/daemon.json` into the retained RUN-0010 cache; remove no other file or directory.
5. `systemctl --user start docker.service`.
6. Revalidate UID-1000 rootless operation, rootless context/socket, no root/TCP endpoint, approved data root, and restored prior containerd-backed image visibility.
7. Do not delete classic or containerd data. Any deletion or migration requires separate authorization.

Rollback was not required and was not executed.

## Acquisition and lifecycle statements

No AzerothCore source, image, database, module, addon, client, client modification, credential, proprietary game asset, or WoW content was acquired. The only images acquired were the three approved disposable Docker validation images, and their visible classic-backend copies were removed.

The storage remediation succeeded, but the Rootless Docker Functional Runtime milestone remains **FAIL** on the literal loopback listener-shape requirement. The project lifecycle remains **S0 — Proposed**.
