# VAL-0001: Project Structure Validation

Status: **Passed**  
Date and local time: **2026-07-14T16:41:50-07:00**  
Scope: **`/run/media/deck/blAIneXPLAT/azerothcore/`**

## Objective

Verify the governed S0 project structure, required authoritative files, platform separation, path boundary, and non-installation claims for RUN-0001.

## Acceptance checks

- Every required directory exists.
- Every required file exists.
- No required path was created outside the project root.
- Linux and Windows paths are separate.
- The lifecycle remains S0.
- No Docker, AzerothCore, package, service, database, firewall, mount, or game-client changes were made.

## Required paths tested

Directories:

```text
.
docs/
docs/governance/
docs/architecture/
docs/requirements/
docs/public/
records/
records/decisions/
records/audits/
records/executions/
records/validations/
records/incidents/
records/releases/
linux/
linux/source/
linux/config/
linux/scripts/
linux/compose/
linux/documentation/
linux/records/
linux/manifests/
linux/backups/
windows/
windows/source/
windows/config/
windows/scripts/
windows/documentation/
windows/records/
windows/manifests/
windows/backups/
```

Files:

```text
README.md
GOVERNANCE.md
docs/governance/project-charter.md
docs/governance/authority-and-versioning-rules.md
docs/governance/change-control.md
docs/governance/public-release-governance.md
docs/requirements/acceptance-criteria.md
linux/README.md
windows/README.md
records/decisions/ADR-0001-project-directory-and-platform-separation.md
records/executions/RUN-0001-create-governed-project-structure.md
records/validations/VAL-0001-project-structure-validation.md
```

## Validation commands

The validation shell established `root=/run/media/deck/blAIneXPLAT/azerothcore`, populated arrays with every absolute path listed above, and executed these checks:

```bash
for path in "${required_dirs[@]}"; do
    test -d "$path" || fail=1
done

for path in "${required_files[@]}"; do
    test -f "$path" || fail=1
done

canonical_root="$(realpath "$root")"
for path in "${required_dirs[@]}" "${required_files[@]}"; do
    canonical_path="$(realpath "$path")"
    case "$canonical_path" in
        "$canonical_root"|"$canonical_root"/*) ;;
        *) fail=1 ;;
    esac
done

linux_real="$(realpath "$root/linux")"
windows_real="$(realpath "$root/windows")"
test "$linux_real" != "$windows_real" || fail=1
test "$linux_real" = "$canonical_root/linux" || fail=1
test "$windows_real" = "$canonical_root/windows" || fail=1

rg -q 'S0 — Proposed' \
  "$root/README.md" \
  "$root/GOVERNANCE.md" \
  "$root/docs/governance/project-charter.md" \
  "$root/docs/requirements/acceptance-criteria.md" || fail=1

if rg -n 'Current lifecycle status:.*S[1-8]|Lifecycle status:.*S[1-8]' \
  "$root" --glob '*.md'; then
    fail=1
fi

find "$root" -mindepth 1 -printf '%P\n' | sort
find "$root" -mindepth 1 -printf '.' | wc -c

git -C "$root" rev-parse --is-inside-work-tree
```

The final presentation command prints the complete tree and first 240 lines of every Markdown file, then repeats explicit directory and file tests:

```bash
root=/run/media/deck/blAIneXPLAT/azerothcore
find "$root" -print | sort

while IFS= read -r file; do
    printf '\n===== %s =====\n' "$file"
    sed -n '1,240p' "$file"
done < <(find "$root" -type f -name '*.md' -print | sort)

fail=0
for path in "${required_dirs[@]}"; do
    if [ -d "$root/$path" ]; then
        printf 'PASS directory %s\n' "$root/$path"
    else
        printf 'FAIL directory %s\n' "$root/$path"
        fail=1
    fi
done
for path in "${required_files[@]}"; do
    if [ -f "$root/$path" ]; then
        printf 'PASS file %s\n' "$root/$path"
    else
        printf 'FAIL file %s\n' "$root/$path"
        fail=1
    fi
done
printf 'FINAL_LIFECYCLE_STATUS=S0 — Proposed\n'
exit "$fail"
```

## Observed results

- Required directories: `30/30 PASS` (project root plus 29 descendants).
- Required files: `12/12 PASS`.
- Canonical boundary checks: `42/42 PASS`; all required paths resolve within the project root.
- Platform separation: `PASS`; canonical Linux and Windows paths are distinct and correctly rooted.
- Lifecycle authority text: `PASS`; required authorities state S0, and no document claimed S1 through S8 as its current lifecycle status.
- Exact inventory: `41` entries below the project root, consisting of the expected 29 descendant directories and 12 files.
- Git detection: no Git work tree was present, so no Git initialization or remote action occurred.
- First validation exit status: `0`.

## Non-installation and non-modification boundary

RUN-0001 records the complete modifying scope: directory creation and Markdown authoring only inside the project root. Review of those operations found no Docker, AzerothCore, package, service, container, database, firewall, mount, partition, filesystem, game-client, or Git changes. This validation does not infer whether unrelated software already exists on the host; it verifies that RUN-0001 made none of those changes.

## Result

**PASS.** The governed structure and authoritative files satisfy the S0 acceptance scope. This result does not advance the project beyond **S0 — Proposed** and does not validate any implementation or runtime capability.
