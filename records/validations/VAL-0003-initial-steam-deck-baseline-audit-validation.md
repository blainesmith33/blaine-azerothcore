# VAL-0003: Initial Steam Deck Baseline Audit Validation

Status: **Passed**  
Date and local time: **2026-07-14T17:13:48-07:00**  
Lifecycle target: **S0 — Proposed**  
Scope: **AUD-0001 and RUN-0003 under the authoritative project root**

## Acceptance checks

- AUD-0001, RUN-0003, and VAL-0003 exist.
- All three paths resolve inside the authoritative project root.
- AUD-0001 contains sections A through J.
- Commands and raw observations are distinguishable from interpretation and limitations.
- Publication-sensitive values are marked for sanitization.
- No installation, package, service, container, database, firewall, mount, source, client, permission, ownership, or Git changes occurred.
- AUD-0001 does not claim Docker, AzerothCore, a database, a game client, authserver, worldserver, or any realm is operational.
- AUD-0001 does not select a final installation architecture.
- The lifecycle remains S0 — Proposed pending human review.
- Validation exits successfully.

## Method

The validation shell set:

```bash
root=/run/media/deck/blAIneXPLAT/azerothcore
aud="$root/records/audits/AUD-0001-initial-steam-deck-baseline.md"
run="$root/records/executions/RUN-0003-perform-initial-steam-deck-baseline-audit.md"
val="$root/records/validations/VAL-0003-initial-steam-deck-baseline-audit-validation.md"
```

It then executed explicit tests equivalent to:

```bash
for path in "$aud" "$run" "$val"; do
    test -f "$path" || fail=1
    canonical="$(realpath "$path")"
    case "$canonical" in "$root"/*) ;; *) fail=1 ;; esac
done

for letter in A B C D E F G H I J; do
    rg -q "^## $letter\\." "$aud" || fail=1
done

for heading in '### Commands' '### Raw observations' \
               '### Interpretation' '### Limitations'; do
    rg -q "^${heading}" "$aud" || fail=1
done

rg -q 'PUBLICATION-SENSITIVE: SANITIZE|require sanitization' "$aud" || fail=1
rg -q 'does \*\*not\*\* establish that Docker, AzerothCore' "$aud" || fail=1
rg -q 'does \*\*not\*\* select or approve a final installation architecture' "$aud" || fail=1
rg -q 'No final runtime location was selected or created' "$aud" || fail=1
rg -q 'The project remains \*\*S0 — Proposed\*\* pending human review' "$aud" || fail=1
rg -q 'Only read-only system inspection and creation/amendment of the three authorized Markdown records occurred' "$run" || fail=1
rg -q 'No package, service, container, image, repository, source tree, database, firewall rule, network configuration, mount, permission, ownership, runtime directory outside the project' "$run" || fail=1

if rg -n '^(Lifecycle status|Lifecycle before and after|Current lifecycle status):.*S[1-8]' \
  "$aud" "$run" "$val"; then
    fail=1
fi

find "$root" -mindepth 1 -printf '.' | wc -c

if git -C "$root" rev-parse --is-inside-work-tree >/dev/null 2>&1; then
    git -C "$root" status --short
fi
```

## Observed results

- Required record files: `3/3 PASS`.
- Canonical containment: `3/3 PASS`; all resolve inside the authoritative root.
- Required audit sections A–J: `10/10 PASS`.
- Evidence distinction headings and method statement: `5/5 PASS`.
- Sensitive-value sanitization marking: `PASS`.
- Explicit non-operational statement: `PASS`.
- No final installation architecture: `PASS`.
- No final runtime location: `PASS`.
- AUD and RUN S0 statements: `PASS`.
- No later lifecycle status in AUD-0001, RUN-0003, or VAL-0003: `PASS`.
- Read-only modification boundary and prohibited-change denial: `PASS`.
- AUD-0002/later implementation skip: `PASS`.
- Prior RUN-0001/RUN-0002 and VAL-0001/VAL-0002 evidence: present.
- Project inventory: `79` entries, matching the previous 76 plus these three records.
- Git repository: absent; Git was not initialized and status was not run.
- Primary validation exit status: `0`.

## Non-installation validation basis

RUN-0003 is the command ledger. It contains only inspection commands, bounded searches, version/package queries, external source consultation, record authoring, and validation. It records all nonzero results, permission failures, warnings, and intentionally skipped stateful commands. Review found no package installation, service change, container/image operation, database action, firewall/network change, mount/remount, permission/ownership change, source/client acquisition, Git initialization, or later audit command.

## Result

**PASS.** AUD-0001 satisfies the authorized read-only baseline-evidence scope subject to its recorded namespace limitations. This validation does not approve the audit conclusions for S1, approve an installation architecture, or advance the project. Final lifecycle status remains **S0 — Proposed** pending human review.
