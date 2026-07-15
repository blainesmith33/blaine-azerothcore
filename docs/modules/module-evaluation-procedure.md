# Module Evaluation Procedure

Status: **Authoritative procedure; not yet executed**  
Current lifecycle status: **S0 — Proposed**

## Purpose

Evaluate an existing module or candidate feature without confusing an upstream recommendation, an individual build, or a research result with approval for the Blaine-integrated realm.

## Procedure

1. Assign an immutable internal evaluation identifier.
2. Define the intended gameplay purpose and classify the candidate as original, independently inspired, derived, dependency, or reference-only.
3. Record upstream project, repository, author or organization, license, notices, version, branch, tag, and exact commit SHA. Preserve artifact checksums if an immutable source reference is unavailable.
4. Capture the maintainer-recommended AzerothCore version, branch, tag, exact commit when available, dependencies, toolchain, database revisions, configuration, and client requirements. Cite the upstream source and observation date.
5. State explicitly that the recommendation is unverified until reproduced by this project.
6. Freeze the evaluation input to exact core, module, dependency, database, configuration, and client identifiers. Do not use `latest`, `master`, `main`, or `current` as final evidence.
7. When reasonably possible and later authorized, test the module first in an isolated research instance on its documented reference baseline.
8. Record clean-build, startup, shutdown, runtime, gameplay, database, security, stability, and client observations in RUN and VAL records.
9. Identify required core patches, hooks, symbols, SQL, configuration keys, ports, database ownership, client assets, and conflicts.
10. Evaluate compatibility with other desired candidates without implying combined approval.
11. Estimate port-forward effort, independent-reimplementation feasibility, replacement alternatives, maintenance condition, and security exposure.
12. Complete a copy of `module-compatibility-matrix-template.md` and recommend research, adapt, port, replace, independently implement, reject, defer, or reference-only disposition.
13. Obtain evidence-backed review. Record exceptions and architecture consequences through the applicable ADR process.

## Outputs

- provenance and license record;
- completed compatibility matrix entry;
- exact source/module manifest;
- RUN and VAL evidence when technical testing is authorized;
- recommended disposition, limitations, and unresolved questions.

Evaluation alone never promotes a module into the integrated realm.
