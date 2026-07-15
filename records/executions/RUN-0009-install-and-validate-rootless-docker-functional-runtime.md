# RUN-0009 — Install and Validate Rootless Docker Functional Runtime

## Record control

- Identifier: `RUN-0009`
- Execution date: 2026-07-14
- Operation start: 2026-07-14T19:40:25-07:00
- Installer execution: 2026-07-14T19:47:14-07:00 through 2026-07-14T19:47:25-07:00
- First functional test: 2026-07-14T19:52:36-07:00
- Authoritative root: `/run/media/deck/blAIneXPLAT/azerothcore/`
- Authorized cache: `/home/deck/.cache/blAIne/azerothcore/RUN-0009-rootless-docker/`
- Governing decision: `ADR-0010`
- Installation result: **PASS**
- Daemon/rootless security result: **PASS**
- Functional milestone result: **FAIL — container creation blocked by kernel overlayfs/filesystem incompatibility**
- Lifecycle before and after: `S0 — Proposed`

## Authorization boundary

This host-changing operation was limited to the official Docker rootless installer, pinned Docker Engine/rootless-extras `29.6.1`, Docker Compose `v5.3.1`, the three approved disposable Docker Hub images, transient rootless runtime state, the authorized user paths, the RUN-0009 cache, and exactly `RUN-0009` plus `VAL-0009` in governance.

No sudo, root shell, pacman transaction, SteamOS immutability change, lingering change, root system unit, firewall change, host sysctl change, shell-profile change, rootful daemon, TCP Docker API, AzerothCore acquisition, staging write, or prior-governance modification was authorized or performed.

## Pre-install state and gates

| Gate | Evidence | Result |
|---|---|---|
| Authoritative project host view | `/dev/sda1`, exFAT, `rw`, shared; root writable | PASS |
| Restricted view reconciliation | Prior restricted namespace showed external mounts read-only; authoritative host view showed `rw,shared`; no mount was changed | PASS |
| Staging separation | `/dev/sdb1`, ext4, `rw`, shared; distinct device/path | PASS |
| Required prior evidence | `AUD-0003`, `RUN-0007`, `VAL-0007`, `ADR-0010`, `RUN-0008`, `VAL-0008` present | PASS |
| `VAL-0008` | Status PASS; corrected primary validation exit `0` | PASS |
| Lifecycle | README, GOVERNANCE, and ADR-0010 reported `S0 — Proposed` | PASS |
| Target collisions | RUN-0009 and VAL-0009 absent at initial and final pre-write checks | PASS |
| Project boundary | 69 files, 52 directories; content-list SHA-256 `c86295d6de8fe9b61ce48c58a2a7af0ccbf3a816c928a3c2bb30d52c3bb71137`; metadata-list SHA-256 `9a37f9b9bba70adadbbd2cedc61a49095e3e80670812eef8e46d39dacc0f5dfb` | Captured |
| Staging boundary | 147 files, 121 directories; content SHA-256 `3cbf32be8fad7025854b22af89a127cd5443d0b369bd8faa7f5ad1f0d549be9f`; metadata SHA-256 `0f05adb46b8e6a57b5ff5375715eea715d2481347dece31ffbb5dec59e3f19e5` | Captured |
| Storage reliability baseline | 416 matching current-boot kernel lines; SHA-256 `ca2fde5021ad2d3c4b19bef670f4a1e929509e06aa234b7f340a6349d0fdd4fe` | Captured; old warning unresolved |
| New storage errors before install | Zero since operation start | PASS |
| Authorized Docker paths | `~/bin`, `~/.config/docker`, `~/.docker`, `~/.local/share/docker`, docker user unit/link, RUN-0009 cache all absent | PASS; no conflicts |
| Credentials/config conflicts | `~/.docker/config.json`, `~/.docker/key.json`, `~/.config/docker/daemon.json` absent | PASS |
| Docker/runtime absence | Docker, dockerd, Compose, RootlessKit, containerd, runc, rootless scripts, rootless context/socket absent | PASS |
| Rootful absence | No dockerd process, `/var/run/docker.sock` absent, root docker service/socket not found/inactive | PASS |
| Session/persistence | User manager running; user bus available; `Linger=no` | PASS |
| SteamOS | `steamos-readonly status` = `enabled` | PASS |
| Internal storage | `/home/deck` on `/dev/nvme0n1p8`, ext4, `rw`; 1,169,566,715,904 free bytes (>20 GiB) | PASS |
| Subordinate IDs | `/etc/subuid` and `/etc/subgid`: `deck:100000:65536`; both SHA-256 `3dbc9d9593703451d1eed51a1af39f8cf6f3a226fee979ac0f6583cff67dc19c` | PASS |
| User namespaces | `user.max_user_namespaces=58833`; `unshare --user --map-auto true` exit `0` | PASS |
| Cgroup delegation | cgroup v2; `Delegate=yes`; `cpu memory pids dmem` in user-manager controllers/subtree | PASS |
| Ports/socket path | No TCP/UDP listener on 18080/18081; no listener or file at `/run/user/1000/docker.sock` | PASS |
| Package baseline | 1,158 pacman packages; local-DB metadata SHA-256 `9c7dd0b0aa3b58d42de9445e6a17f259e90b361f461258ae37cb49fcec5b168c` | Captured |
| Shell profiles | `.bashrc` SHA-256 `ba6cce6e38c79dba3f25ec487a27f9a980ff49cb594b11d11c74bf17707cc965`; `.bash_profile` SHA-256 `a621169446500734421a3a65714da8bed1a6557873d98b50aa17c5d0e694b148`; `.profile` and `.zshrc` absent | Captured |

## Exact official sources and artifact evidence

All artifacts were downloaded as files into the authorized cache. No remote content was piped to a shell.

| Artifact | Official source | Bytes | SHA-256 | Verification |
|---|---|---:|---|---|
| Rootless installer | `https://get.docker.com/rootless` | 8,645 | `9badc490230817c5c94960782496a95641a72021c8bbea8e8aba9d9f1e23433e` | Syntax PASS; stable `29.6.1`; default channel stable; commit declared |
| Commit source | `https://raw.githubusercontent.com/docker/docker-install/02cb80d6c7d24c85a458ae31d166a6c535c7a37a/rootless-install.sh` | 8,666 | `519165a123f9924c530c64bdba3019124555eb311a671e149e2d1c1f79a6a92d` | Syntax PASS; source comparison PASS |
| Docker Engine archive | `https://download.docker.com/linux/static/stable/x86_64/docker-29.6.1.tgz` | 87,300,816 | `b0df4a43a98d7ecb708acbdb5a34a3416e13b6e39bcbbdf296f51f0f3442b29f` | Stable index/name/architecture, gzip, members, embedded versions, installed bytes PASS |
| Rootless extras archive | `https://download.docker.com/linux/static/stable/x86_64/docker-rootless-extras-29.6.1.tgz` | 11,997,367 | `43e2450df3797e78a8a4e57fef0540f95109a4937c8f1af4294721bda2e3fb67` | Stable index/name/architecture, gzip, members, RootlessKit, installed bytes PASS |
| Compose binary | Official `docker/compose` release `v5.3.1`, asset `docker-compose-linux-x86_64` | 32,564,869 | `f9ebc6ebdb19d769b793c245a736caaeb198c62587f13b25c660c13b4987f959` | Per-asset checksum, checksums.txt, API digest, embedded version, installed bytes PASS |
| Compose per-asset checksum | Official release asset | 94 | `58c3fe267f546870525db7df42e84ba0c898fbf6fb9e75fef90d7d859bf27c07` | `sha256sum -c` PASS |
| Compose checksums.txt | Official release asset | 4,616 | `885a9cc13b7117e55cce76c4dc547e8aa9f203f0a849bf977eaa1b1696ccf282` | Target entry matched |
| Compose release JSON | GitHub API for official `docker/compose` tag `v5.3.1` | 116,463 | `f86212202269e8a3a82fc59ab3eecff63ecb5e5afe7d2481e0061c740b91a745` | Release ID `350238808`; draft=false; prerelease=false; API digest matched |
| Docker stable index | Official stable x86_64 index | 45,143 | `0afe9c723322e33ae80093207d82f2b3dda370c9e597e4f176e3679c79ef7c99` | Both `29.6.1` archive names present |
| Docker v29 release notes | Official Docker documentation | 650,685 | `177bbfbb2099168b579323f7d218c5b7ec8c948a9435e7dfa03236a9173397a3` | `29.6.1`, dated 2026-06-26 |

Docker's static index did not publish a separate SHA-256 file for the Engine or rootless archive. Their locally calculated hashes were therefore supported by exact official HTTPS source identity, archive safety/integrity, embedded versions, and byte-for-byte equality between every installed file and the separately preserved candidate extraction. This limitation is explicit and is not represented as an official Docker checksum.

The Compose release redirected to the official GitHub release-asset infrastructure. Three expiring signed redirect query strings were redacted from retained headers; source host/path identity and artifact hashes were preserved.

## Installer source commit comparison

The installer declared:

```text
SCRIPT_COMMIT_SHA="02cb80d6c7d24c85a458ae31d166a6c535c7a37a"
STABLE_LATEST="29.6.1"
TEST_LATEST="29.6.1"
DEFAULT_CHANNEL_VALUE="stable"
```

The saved `get.docker.com` script and the official `rootless-install.sh` at that commit differed only in the documented upload-job substitutions:

```diff
-SCRIPT_COMMIT_SHA="$LOAD_SCRIPT_COMMIT_SHA"
+SCRIPT_COMMIT_SHA="02cb80d6c7d24c85a458ae31d166a6c535c7a37a"
-STABLE_LATEST="$LOAD_SCRIPT_STABLE_LATEST"
+STABLE_LATEST="29.6.1"
-TEST_LATEST="$LOAD_SCRIPT_TEST_LATEST"
+TEST_LATEST="29.6.1"
```

No unexplained local modification was present. Stable was explicitly selected at execution; the test channel was not selected.

## Archive member inspection

`docker-29.6.1.tgz` contained only `docker/` and these regular mode-0755 executables:

```text
containerd
containerd-shim-runc-v2
runc
ctr
docker
docker-proxy
dockerd
docker-init
```

`docker-rootless-extras-29.6.1.tgz` contained only `docker-rootless-extras/` and:

```text
dockerd-rootless.sh
rootlesskit
dockerd-rootless-setuptool.sh
```

Both archives passed `gzip -t`. Each had zero absolute paths, parent traversals, special/device files, and set-ID modes. Numeric archive ownership was 1001/1001; unprivileged extraction did not restore that owner and installed files are owned by `deck`.

Embedded versions were Docker client `29.6.1` build `8900f1d`, daemon `29.6.1` build `8ec5ab3`, containerd `2.2.5`, runc `1.3.6`, and RootlessKit `3.0.1`.

## Commands executed and exit statuses

The exact command families and significant statuses were:

| Command/action | Status |
|---|---:|
| Preflight `findmnt`, `find`, `sha256sum`, `stat`, `rg`, `systemctl` queries, `loginctl`, `ss`, `firewall-cmd` queries, package DB query | 0 |
| First combined preflight script | 127 after earlier gates, due only to an incorrectly quoted subordinate-ID `awk`; no change occurred |
| Corrected subordinate-ID and remaining preflight checks | 0 |
| `unshare --user --map-auto true` | 0 |
| Official text/metadata and artifact `curl --fail --location ... --output FILE` downloads | 0 |
| `sh -n` on installer and commit source | 0 |
| Installer/source `diff -u` | Differences limited to upload-job variables; accepted after inspection |
| Compose release JSON `jq` identity/digest parsing | 0 |
| Compose per-asset `sha256sum -c` | 0 |
| First aggregate checksums.txt grep | No value because it assumed a two-space separator; corrected parser matched the `*filename` form |
| Corrected checksums.txt/API/local hash comparison | 0 |
| `gzip -t` and archive member safety checks | 0 |
| Cache-only embedded version commands | 0 |
| Official installer command | 0 |
| Byte comparisons of all 11 installed Engine/rootless files | 0 each |
| Compose plugin copy/hash/version | 0 |
| Pull each of the three approved images | 0 |
| `docker run --name blaine-run0009-hello hello-world:latest` | **125** |
| Failed-container inspection | Container not created |
| Header-token sanitization | Redaction succeeded; wrapper returned 1 because `pipefail` treated the expected zero-match verification as nonzero |
| Post-failure diagnostics and zero-container confirmation | 0 |

No DNS/HTTPS, loopback, resource-limit, or Compose test command was executed after the non-security functional failure.

## Installer execution and output

The verified installer was executed unmodified as UID 1000 with:

```text
CHANNEL=stable
HOME=/home/deck
DOCKER_BIN=/home/deck/bin
XDG_RUNTIME_DIR=/run/user/1000
TMPDIR=/home/deck/.cache/blAIne/azerothcore/RUN-0009-rootless-docker/installer-tmp
PATH=/home/deck/bin:<operation-local prior PATH>
FORCE_ROOTLESS_INSTALL unset
SKIP_IPTABLES unset
```

Redirecting `TMPDIR` into the authorized cache kept the installer's duplicate archive downloads inside the authorized write boundary. No remote output was piped to a shell.

Material installer output was:

```text
# Installing stable version 29.6.1
# Executing docker rootless install script, commit: 02cb80d6c7d24c85a458ae31d166a6c535c7a37a
[INFO] Creating /home/deck/.config/systemd/user/docker.service
[INFO] starting systemd service docker.service
Active: active (running)
API listen on /run/user/1000/docker.sock
Client Version: 29.6.1
Server Engine Version: 29.6.1
rootlesskit Version: 3.0.1
NetworkDriver: pasta
Created symlink .../default.target.wants/docker.service -> .../docker.service
[INFO] Creating CLI context "rootless"
Successfully created context "rootless"
[INFO] Using CLI context "rootless"
Current context is now "rootless"
```

The installer printed optional shell-profile instructions but did not modify a shell profile. It also printed the command that could enable lingering; that command was not run.

## Files and directories created or modified

Pre-existing unrelated files inside `/home/deck/.config/systemd/user/` were preserved. Only the following authorized host roots were created or changed by the operation:

- `/home/deck/bin/` — 11 deck-owned mode-0755 Engine/rootless files listed above.
- `/home/deck/.docker/` — installer-created rootless context and `config.json`, plus the verified Compose plugin.
- `/home/deck/.docker/cli-plugins/docker-compose` — deck:deck, mode 0755, SHA-256 `f9ebc6ebdb19d769b793c245a736caaeb198c62587f13b25c660c13b4987f959`.
- `/home/deck/.local/share/docker/` — deck-owned mode 0710 active data root.
- `/home/deck/.config/systemd/user/docker.service` — deck-owned mode 0644.
- `/home/deck/.config/systemd/user/default.target.wants/docker.service` — symlink to the user unit.
- `/home/deck/.cache/blAIne/azerothcore/RUN-0009-rootless-docker/` — artifacts, sanitized headers, inspection extraction, manifest, and diagnostics; retained after failure.
- Transient rootless state under `/run/user/1000/`, including the Docker socket and RootlessKit/containerd state.
- `RUN-0009` and `VAL-0009` in governance.

`/home/deck/.config/docker/` remains absent; no daemon JSON was created. `/var/lib/docker` remained the pre-existing empty root-owned offload mount with the same mtime/ctime epoch `1783911810` and zero entries.

## User service and Docker context changes

- User `docker.service`: loaded, enabled, active/running.
- Main PID: 65284, RootlessKit, UID/EUID 1000.
- Cgroup: `/user.slice/user-1000.slice/user@1000.service/app.slice/docker.service`.
- Enabled link: `default.target.wants/docker.service`.
- Root system `docker.service` and `docker.socket`: not found/inactive/dead.
- Context created and selected: `rootless`.
- Context endpoint: `unix:///run/user/1000/docker.sock`.
- Socket: deck:deck, mode reported as `1660` (sticky plus owner/group read-write, no other access); group `deck` has no supplementary member list.
- `/var/run/docker.sock`: absent.
- Docker TCP listeners: none.
- `Linger=no` throughout.

## Installed component versions

| Component | Observed version |
|---|---|
| Docker client | 29.6.1, build `8900f1d` |
| Docker server | 29.6.1, build `8ec5ab3` |
| Docker API | 1.55; minimum 1.40 |
| Docker Compose | v5.3.1 |
| containerd | v2.2.5, commit `e53c7c1516c3b2bff98eb76f1f4117477e6f4e66` |
| runc | 1.3.6, commit `v1.3.6-0-g491b69b` |
| docker-init | 0.19.0, commit `de40ad0` |
| RootlessKit | 3.0.1, API 1.1.1 |
| `dockerd-rootless-setuptool.sh` | Exact rootless-extras 29.6.1 member; SHA-256 `1c9f0dc93ebb3d75255254ec760d26a912affba7f329ab8abffe8e25eb0b3f94` |

## Rootless and security evidence

`docker info` reported:

```text
Context=rootless
ServerVersion=29.6.1
SecurityOptions=[seccomp builtin, rootless, cgroupns]
DockerRootDir=/home/deck/.local/share/docker
OperatingSystem=SteamOS
Architecture=x86_64
KernelVersion=6.16.12-valve24.4-1-neptune-616-gfe145653a794
LoggingDriver=json-file
LiveRestoreEnabled=false
Swarm=inactive
```

RootlessKit, its child namespace process, pasta, dockerd, and containerd all ran with host UID/EUID 1000. The daemon was parented through the RootlessKit user-namespace stack. No UID-0 dockerd, root system unit, rootful socket, Docker TCP listener, privileged container, host network, unauthorized mount, or Docker API exposure occurred.

The service journal explicitly stated `Running in rootless mode`, `Running with RootlessKit integration`, and `skipping firewalld management for rootless mode`. Firewalld state, default zone, runtime/permanent ports, and services remained unchanged.

The rootless launcher ran `sysctl -w` only through `nsenter` into its transient private network namespace for IPv4/IPv6 forwarding. Host `net.ipv4.ip_forward` remained `0`; this was not a host sysctl modification.

## Storage-driver and data-root evidence

- Docker root directory: `/home/deck/.local/share/docker`.
- Backing mount: `/dev/nvme0n1p8`, ext4, `rw,relatime`, internal.
- Docker storage driver: `overlayfs`.
- Driver type: `io.containerd.snapshotter.v1`.
- Data root was never placed on either external project drive.

The daemon initialized successfully, and image pulls succeeded, but container creation exposed an unvalidated kernel/filesystem limitation:

```text
failed to mount /run/user/1000/containerd-mount66497146:
mount source: "overlay" ... userxattr,index=off ... err: invalid argument
```

The kernel supplied the exact cause:

```text
overlay: case-insensitive capable filesystem on
/home/deck/.local/share/docker/containerd/daemon/io.containerd.snapshotter.v1.overlayfs/snapshots/5/work
not supported
```

Thus internal ext4 placement is correct, but the selected rootless containerd overlayfs snapshotter is not functional on this SteamOS `/home` filesystem/kernel combination. `fuse-overlayfs` 1.15 is present and is a possible later governed remediation candidate; it was not configured or tested here.

## Network-driver and port-driver evidence

The unmodified installer selected:

- RootlessKit network driver: `pasta`.
- RootlessKit port driver: `implicit`.
- RootlessKit flag: `--disable-host-loopback` for container-to-host protection.
- Docker management endpoint: local Unix socket only.

This was detected automatically because `pasta` exists and `slirp4netns` does not; neither driver was forced. No test port was published because the basic container failed first. No LAN reachability claim is made.

## Cgroup and resource-limit evidence

- Cgroup version: 2.
- Docker cgroup driver: systemd.
- User service delegation: enabled.
- Host audit showed CPU, memory, and PID controllers delegated.
- Docker warned that cpuset and several IO controls were unavailable.

The bounded resource-limit container was **not run** after the basic failure. CPU/memory/PID acceptance and enforcement therefore remain unvalidated.

## Test images and immutable digests

Only the approved Docker Hub images were pulled:

| Tag | Resolved immutable RepoDigest | Pull result | Final state |
|---|---|---|---|
| `hello-world:latest` | `hello-world@sha256:96498ffd522e70807ab6384a5c0485a79b9c7c08ca79ba08623edcad1054e62d` | PASS | Retained for diagnosis |
| `busybox:1.37.0` | `busybox@sha256:9532d8c39891ca2ecde4d30d7710e01fb739c87a8b9299685c63704296b16028` | PASS | Retained for diagnosis |
| `alpine:3.23` | `alpine@sha256:fd791d74b68913cbb027c6546007b3f0d3bc45125f797758156952bc2d6daf40` | PASS | Retained for diagnosis |

No AzerothCore image was pulled.

## Functional test results

### Basic container test — FAIL

Command:

```text
docker run --name blaine-run0009-hello hello-world:latest
```

Exit status: `125`. The failure occurred during container creation before the image entrypoint could run. No hello-world output was produced and no container record remained. The exact overlayfs/kernel evidence is recorded above.

### DNS and HTTPS test — NOT RUN

Stopped by the earlier basic functional failure. No Alpine container ran, so DNS/TLS/HTTP transport is unvalidated.

### Loopback-only port test — NOT RUN

Stopped by the earlier basic functional failure. Port 18080 was never published; loopback success and LAN rejection are unvalidated.

### Resource-limit test — NOT RUN

Stopped by the earlier basic functional failure. CPU, memory, PID configuration and enforcement are unvalidated.

### Docker Compose test — NOT RUN

The plugin version test passed, but no Compose project was created after the basic failure. Port 18081 was never published; Compose config/up/HTTP/ps/down behavior is unvalidated.

## Cleanup and retained diagnostic state

- Failed hello-world attempt created no container; final container count is zero.
- No RUN-0009 Compose resource exists.
- Only default `bridge`, `host`, and `none` networks exist; no test network exists.
- Volume count is zero.
- Ports 18080 and 18081 have no listeners.
- No test process remains.
- The three approved images are retained because failure handling requires diagnostic preservation; they consume approximately 19.83 MB in Docker's accounting.
- The acquisition cache is retained at 394,307,473 bytes with metadata digest `1ff31c54c7ff2ce684f1afc4173c934f0b2629372fc12bbb575c10e1ca88ee45` before governance record creation.
- Engine, rootless extras, Compose, rootless context, user service, and internal data root remain installed.
- No rollback, uninstall, storage-driver change, image deletion, data deletion, or service stop was performed.

## Failed attempts and corrections

1. A combined preflight script exited 127 because `$1` in a nested `awk` was expanded by the outer shell. All earlier read-only gates had completed; no change occurred. The subordinate-ID and remaining gates were rerun with `rg`, exit 0.
2. The first `checksums.txt` parser assumed a two-space separator and returned an empty value. The official per-asset checksum and API digest already matched; the corrected parser accepted the actual `hash *filename` form and all three values matched.
3. Header sanitization replaced three expiring signed redirect queries successfully. Its wrapper exited 1 because `set -o pipefail` treated the expected post-redaction zero-match `rg` status as failure. A direct verification confirmed zero sensitive header matches.
4. A final UID-0 dockerd count query reused the same nested-`awk` quoting mistake. Independent process evidence before and after showed dockerd UID/EUID 1000 only; the corrected closing validator uses a non-awk fixed form.
5. The first and only container run failed with exit 125 because overlayfs rejects this case-insensitive-capable ext4 backing filesystem. Per failure handling, it was not retried and no subsequent functional tests ran.

## Storage-error checks

The historical current-boot USB/UAS/ext4 warning is not resolved. The operation's exact filtered baseline remained 416 kernel lines with SHA-256 `ca2fde5021ad2d3c4b19bef670f4a1e929509e06aa234b7f340a6349d0fdd4fe` at the final pre-record check.

There were zero new matching USB disconnect, UAS failure, device-offline, I/O, ext4/journal, cache-sync, reset, rejection, or filesystem-remount events from 2026-07-14T19:40:25-07:00 through the final pre-record check. The overlayfs compatibility error is a functional configuration/kernel limitation, not evidence that the historic USB storage fault was resolved.

## Remaining limitations

- Rootless containers cannot currently be created with the automatically selected containerd overlayfs snapshotter on the internal case-insensitive-capable ext4 filesystem.
- The Rootless Docker Functional Runtime milestone is not achieved.
- DNS/HTTPS, loopback publication, LAN rejection, resource limits, and Compose operation remain untested.
- Actual CPU/memory/PID limit enforcement is unproven.
- Logout/reboot persistence remains unvalidated because `Linger=no`.
- The approved diagnostic images and full cache remain until a later governed remediation/cleanup operation.
- A later decision is required before configuring/testing a different storage driver or snapshotter. No automatic switch to `fuse-overlayfs` was made.

## Exact rollback procedure — not executed

Rollback is not authorized by this record and was not run. If separately authorized, it must use an operation-local PATH and preserve the RUN/VAL/cache evidence:

1. Capture final inventories and hashes of every path below.
2. Stop and disable only the user unit: `systemctl --user disable --now docker.service`.
3. Confirm all UID-1000 RootlessKit, dockerd, containerd, and pasta processes and `/run/user/1000/docker.sock` are gone.
4. With Docker CLI still present, select a non-rootless context only for CLI metadata, then remove only `rootless`: `docker context use default`; `docker context rm rootless`. This must not contact a rootful daemon.
5. Remove only the installed files proven created by RUN-0009 from `/home/deck/bin/`: `containerd`, `containerd-shim-runc-v2`, `ctr`, `docker`, `docker-init`, `docker-proxy`, `dockerd`, `runc`, `dockerd-rootless.sh`, `dockerd-rootless-setuptool.sh`, and `rootlesskit`.
6. Remove only `/home/deck/.docker/cli-plugins/docker-compose`, the RUN-0009-created rootless context metadata, and installer-created current-context entry; preserve any later unrelated Docker CLI content.
7. Remove only the RUN-0009 service symlink and `docker.service`, then run `systemctl --user daemon-reload`.
8. Treat `/home/deck/.local/share/docker/` separately: preserve by default. Delete it only under explicit data-destruction authorization after another inventory.
9. Preserve the RUN-0009 cache and governance records by default.
10. Remove a created parent directory only if an exact inventory proves it is empty and was created by RUN-0009. Never delete a pre-existing user directory recursively.
11. Revalidate package DB, shell profiles, SteamOS read-only state, lingering, root system units/socket, firewall, mounts, and storage-error window.

## Acquisition and lifecycle statement

No AzerothCore source, database, module, addon, client file, credential, configuration, or container image was acquired. The Docker-only installation did not advance the project. Lifecycle remains **S0 — Proposed**.

