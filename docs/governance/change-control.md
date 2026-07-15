# Change Control

Status: **Authoritative**  
Current lifecycle status: **S0 — Proposed**

## Purpose

Ensure changes are authorized, traceable, reversible where practical, and supported by execution and validation evidence.

## Controlled workflow

1. **Propose:** State objective, scope, affected platform, assumptions, risks, and acceptance criteria.
2. **Classify:** Determine whether governance, architecture, implementation, operations, or public material is affected.
3. **Decide:** Obtain explicit human approval. Create an ADR for an important architecture choice before implementation.
4. **Plan evidence:** Define the RUN and VAL records required and identify authoritative sources.
5. **Execute:** Run only approved commands against identified targets. Capture commands, timestamps, working directory, outputs, exit status, and unexpected behavior.
6. **Validate:** Perform explicit checks and record them under a VAL identifier. Linux and Windows require separate validation.
7. **Reconcile:** Update authoritative documents to match observed results. Preserve failures in RUN or incident records.
8. **Approve stage or release:** Human approval is required for lifecycle advancement and public release.

## Change classes

- **Editorial:** No technical meaning changes; preserve authority and references.
- **Procedural:** Changes commands or operational sequence; requires execution and validation before success claims.
- **Architectural:** Changes boundaries, platforms, storage, runtime, networking, data, or security model; requires an ADR.
- **Release-affecting:** Changes public claims or published artifacts; requires public-release review.
- **Module or instance integration:** Changes module composition, exact baselines, database ownership, realm identity, client requirements, or a combined manifest; requires compatibility review, applicable ADR authority, RUN/VAL evidence, and integration-promotion approval.

## Execution requirements

Every command that modifies the Steam Deck or project implementation must appear in a RUN record. Records include failures, partial results, warnings, and materially unexpected behavior. Silent omission is prohibited.

## Secrets and sanitized examples

Execution evidence destined for publication must remove or replace secrets, passwords, keys, tokens, personal data, and non-public IP addresses. Sanitization must not falsify the technical result. Publishable configuration uses explicit placeholders such as `<DATABASE_PASSWORD>` and `<SERVER_IP>`.

## Emergency or failed change

Stop when target identity, authority, or safety assumptions fail. Preserve the attempted command and result. Create an incident record if impact or investigation warrants it. Do not claim completion until a subsequent execution and validation establish it.

## S0 amendment: compatibility and promotion control

Third-party module work begins from the maintainer-documented reference baseline where reasonably possible, while clearly marking recommendations as unverified. A candidate Blaine baseline is proposed only through compatibility convergence across the entire desired feature set. Changes are added one at a time and promoted only after exact pinning plus combined build, database, runtime, gameplay, upgrade, removal, rollback, security, stability, conflict, and regression evidence. Individual build or runtime success cannot bypass this gate. Every exception requires a decision record.
