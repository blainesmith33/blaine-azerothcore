# Compatibility Convergence Procedure

Status: **Authoritative procedure; not yet executed**  
Current lifecycle status: **S0 — Proposed**  
Decision: **ADR-0003**

## Purpose

Select a defensible AzerothCore integration baseline by converging the requirements and verified compatibility of every desired Blaine feature and module, instead of automatically selecting the newest core or allowing one abandoned module to dictate the baseline.

## Inputs

- desired gameplay requirements and priority;
- completed module evaluations and provenance records;
- maintainer-recommended reference baselines, clearly marked as recommendations;
- project-validated reference-baseline evidence;
- compatibility matrices, security findings, maintenance status, client requirements, and platform constraints.

## Procedure

1. Freeze the desired feature set and distinguish required, optional, experimental, and replacement-eligible capabilities.
2. List each candidate's exact validated and recommended core ranges, module pins, dependencies, database revisions, patches, configuration, and client constraints.
3. Build a compatibility graph showing overlaps, conflicts, unknowns, and candidate convergence points.
4. Define exact candidate integration baselines. A moving branch name is insufficient.
5. Compare candidates for feature coverage, combined build feasibility, database compatibility, runtime behavior, client compatibility, maintainability, upstream support, security, upgrade cost, rollback, and Linux/Windows scope.
6. Prefer the newest candidate only when evidence shows it satisfies compatibility, maintainability, security, and validation requirements.
7. Permit an older candidate when evidence documents superior convergence and an acceptable security and maintenance plan.
8. For unsupported or abandoned blockers, compare porting, adaptation, replacement, removal, or independent reimplementation rather than silently freezing the permanent core baseline.
9. Test candidate combinations incrementally, adding one feature or module at a time and recording exact manifests and results.
10. Validate combined compile, database migration, startup, shutdown, gameplay, conflict, regression, upgrade, removal, and rollback behavior.
11. Select a baseline only through an ADR that cites the full evidence set, rejected alternatives, risks, exceptions, and exact pins.
12. Maintain separate conclusions for Linux and Windows.

## Output

The output is a proposed pinned integration manifest plus decision evidence. It becomes an approved integration baseline only after the required combined validation and human approval. No such baseline is selected at S0.
