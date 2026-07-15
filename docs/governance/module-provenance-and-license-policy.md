# Module Provenance and License Policy

Status: **Authoritative**  
Current lifecycle status: **S0 — Proposed**  
Authority: **RUN-0002; ADR-0004**

## Purpose

Preserve the origin, authorship, licensing, transformation history, and permitted use of third-party, derived, inspired, replacement, and original module work.

## Required classification

Every evaluated feature or module is classified as one of:

- **Original:** created for this project without copied third-party implementation.
- **Independently inspired:** implements a general idea after a documented clean analysis boundary, without incorporating protected source expression.
- **Derived:** modifies, ports, incorporates, or translates third-party implementation material.
- **Dependency:** used as a required external component without being represented as project-authored.
- **Reference-only:** studied for requirements or comparison but not incorporated.

Classification is an evidence statement, not a substitute for license review.

## Required provenance record

Before build promotion, record the internal evaluation identifier, upstream project and repository, author or organization, retrieved version/tag/branch, exact commit SHA, retrieval date, license name and license text location, copyright notices, dependency provenance, modifications, patch history, and evidence references under `records/modules/provenance/`.

Moving references do not satisfy provenance. If an upstream lacks an immutable version, preserve the retrieved artifact checksum and document the limitation.

## License controls

- Preserve required copyright, attribution, notice, source-offer, and license text.
- Record compatibility obligations before combining or distributing code or assets.
- Do not remove upstream provenance when a module is ported or modified.
- Keep original, derived, and independently reimplemented material distinguishable in source history and manifests.
- Do not describe independently inspired work as upstream code, or derived work as wholly original.
- Escalate unclear licensing, missing license terms, incompatible obligations, or asset rights for human decision before incorporation or release.

## Permitted strategy space

Subject to applicable licenses and recorded approval, existing modules may be studied, tested, modified, ported, replaced, or independently reimplemented. Technical convenience does not override license obligations.

## Public release

Release evidence must include an attribution and license inventory, source pins, modification disclosures, included notices, dependency licenses, and exclusions. Private credentials, personal data, and non-public network details remain prohibited.

## Records and retention

Provenance and license evidence is retained even when a candidate is rejected or archived. Replacement does not erase the decision trail or prior evaluation evidence.

## Addons and full-stack feature packages

The same provenance controls apply to client addons, Realm Control definitions, protocols, presets, migrations, and coordinated feature packages. Third-party modules and addons remain upstream assets; renaming them does not create original blAIne authorship. Future acquisition and inventory require human approval and exact source, maintainer, license, version, commit, timestamp, hash where applicable, dependencies, conflicts, platform requirements, and disposition.
