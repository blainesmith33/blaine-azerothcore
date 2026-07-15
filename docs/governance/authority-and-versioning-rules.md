# Authority and Versioning Rules

Status: **Authoritative**  
Current lifecycle status: **S0 — Proposed**

## Authoritative format

Markdown (`.md`) is authoritative for project documentation and records. Generated PDF, HTML, office-document, image, or other exports are derivative. If an export conflicts with Markdown, Markdown controls.

## One-current-version rule

Only one authoritative current version may exist for each combination of scope and artifact class. A filename, title, status field, and identifier must make its authority unambiguous. Copies must not be presented as parallel current authorities.

Examples of artifact classes include governance rules, requirements, ADRs, procedures, platform configuration guidance, execution records, validation records, and public guides.

## Supersession and archive rules

When a current authoritative document is replaced:

1. Approve the replacement through change control.
2. Mark the former document superseded and identify its successor.
3. Move the former document out of the current location into an archive location appropriate to its scope.
4. Preserve its original identifier, content, and history.
5. Update inbound references to the current authority.

Archive directories are created only when first needed. Root governance and requirements use `docs/archive/`; root records use `records/archive/`; platform documentation uses the relevant platform's `documentation/archive/`. Archived material is historical evidence, not current authority.

## Identifier rules

- Architecture decisions: `ADR-NNNN-short-title.md`.
- Execution records: `RUN-NNNN-short-title.md`.
- Validation records: `VAL-NNNN-short-title.md`.
- Incident records: `INC-NNNN-short-title.md` when a dedicated incident is warranted.

Identifiers are never reused. A superseded record retains its identifier.

## Status and version metadata

Authoritative documents state their status and applicable lifecycle stage. Material changes must identify the authorizing decision or execution record. Editorial corrections must not obscure prior evidence.

## Platform scope

Root documents may govern shared policy. Linux and Windows implementation artifacts remain within their respective platform trees. Cross-platform similarity does not make an artifact authoritative for both platforms unless root governance explicitly establishes shared scope.

## S0 amendment: module, suite, and instance version authority

- Each module evaluation has an immutable internal identifier and a provenance/compatibility record.
- Each defined instance has one current exact source-and-module manifest for its platform and database state.
- Each Blaine suite candidate or release has one current combined integration manifest with explicit dependency and migration ordering.
- Branch labels such as `latest`, `master`, `main`, or `current` may describe discovery context but cannot serve as authoritative reproducible versions. Full commit SHAs, immutable dependency versions, database revisions, configuration identity, and client requirements are required for validated manifests.
- Maintainer-recommended baselines and project-validated baselines are distinct artifact fields and must not be silently conflated.
- Superseded manifests and module evaluations follow the existing archive rule; provenance and evidence are retained.
