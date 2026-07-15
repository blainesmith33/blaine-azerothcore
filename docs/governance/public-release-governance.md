# Public Release Governance

Status: **Authoritative**  
Current lifecycle status: **S0 — Proposed**

## Release principle

Public documentation is a downstream representation of approved governance, architecture, procedures, execution records, and validation records. It is not independent evidence and cannot expand validated claims.

## Claim requirements

A public document may state that a step works only when:

1. the implementation path and platform are identified;
2. the step was executed and preserved in a RUN record;
3. the expected outcome was checked in a VAL record;
4. failures and limitations are represented accurately; and
5. the evidence supports the exact scope of the claim.

Linux success does not support a Windows claim. A server-operational result does not support client-connectivity or operational-readiness claims without their own validation.

## Sanitization

Public material must exclude:

- secrets, passwords, private keys, and account tokens;
- personal data;
- IP addresses or host details not intended for publication;
- private paths or identifiers that do not contribute reproducible evidence.

Examples use clear placeholders such as `<DATABASE_PASSWORD>`, `<REALM_NAME>`, and `<SERVER_IP>`. Sanitization is reviewed before release.

## Source and derivative control

Authoritative public source is Markdown under `docs/public/` when such material is approved. PDFs, rendered sites, archives, and other exports are derivative. Derivatives must identify the source revision and may not supersede it.

## Release gate

Public release requires S8 acceptance evidence, a release record, an inventory of included artifacts, link and command review, sanitization review, platform-scope review, and explicit human approval. Until then, material is draft and must not present the project as release ready.

## Current restriction

At S0, no public document may claim that Docker, AzerothCore, MySQL or another database, a game client, or a local realm is installed or operational.

## S0 amendment: multi-realm and module release claims

Public module or realm claims must identify the validated platform, instance class, exact AzerothCore and module commits, dependency and database revisions, configuration identity, client requirements, and supporting RUN/VAL records. A maintainer recommendation must be attributed and must not be described as project verification. A shared realm-menu claim requires client login and realm-list validation; an individual module result cannot be presented as integrated-suite success.

Any released Blaine suite must include its exact combined manifest, component provenance, licenses and notices, modification disclosures, dependency ordering, known conflicts, supported client profile, upgrade/rollback evidence, and excluded or unverified candidates.
