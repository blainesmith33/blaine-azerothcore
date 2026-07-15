# ADR-0003: Module Version Convergence Strategy

Status: **Accepted for S0 planning; no integration baseline selected**  
Date: **2026-07-14**  
Current lifecycle status: **S0 — Proposed**

## Context

Desired third-party modules may target different AzerothCore revisions, dependencies, database schemas, hooks, and clients. Automatically choosing the newest core may break desired features; permanently choosing an old core for one abandoned module may create maintenance or security problems. Moving branch names cannot reproduce a validated result.

## Decision

1. Evaluate each module first on its maintainer-documented reference baseline where reasonably possible.
2. Treat “recommended” as an attributed starting point, not project verification.
3. Pin the exact core, module, dependency, database, configuration, toolchain, and client state actually validated.
4. Select the Blaine integration baseline only through the documented compatibility-convergence procedure across every desired module and feature.
5. Prefer the newest core only when compatibility, maintainability, security, and validation evidence support it.
6. Permit an older evidence-justified convergence point.
7. Prevent unsupported or abandoned modules from silently fixing the permanent baseline; evaluate porting, replacement, removal, or independent implementation.
8. Permit research instances to run different exact core versions with separate manifests and non-interchangeable databases unless migration is validated.
9. Require a decision record for exceptions.

## Rationale

Compatibility is a system property, not the property of a version label or an individual module build. Exact pins and incremental combined testing allow findings to be reproduced and compared.

## Consequences

- No Blaine integration baseline exists at S0.
- Evaluation may require multiple temporary research baselines.
- Candidate selection includes maintenance and security risk, not build success alone.
- Database migration and rollback are part of convergence.
- Linux and Windows conclusions remain separate.

## Alternatives not selected

- Always use newest AzerothCore: rejected because it ignores module and client compatibility evidence.
- Use each module's recommendation as the integrated baseline: rejected because recommendations can conflict and are not project validation.
- Let the least-maintained module permanently pin the core: rejected without a specific evidence-backed exception.

## Validation boundary

VAL-0002 validates the existence and consistency of this decision. It does not perform module research or select a core commit.
