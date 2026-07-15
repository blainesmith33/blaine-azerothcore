# Module Version and Integration Policy

Status: **Authoritative**  
Current lifecycle status: **S0 — Proposed**  
Authority: **RUN-0002; ADR-0003**

## Purpose

Govern how AzerothCore versions and third-party module versions are evaluated, converged, pinned, and promoted for multiple server instances and the eventual Blaine-integrated realm.

## Baseline terms

- **Reference baseline:** the AzerothCore version, branch, tag, exact commit when available, dependencies, database state, and client requirements documented by a module maintainer.
- **Validated reference baseline:** a reference baseline that this project has actually reproduced and validated; “recommended” alone never means “verified.”
- **Candidate integration baseline:** an exact AzerothCore commit and dependency set under combined evaluation for the Blaine module suite.
- **Pinned integration baseline:** the exact evidence-approved core, modules, dependencies, database revisions, configuration state, and client requirements for a defined integration manifest.

## Policy

1. Each third-party module is first tested on its documented recommended baseline where reasonably possible.
2. Maintainer recommendations are recorded as attributed upstream claims. **Recommended does not mean verified.**
3. The project records the actual AzerothCore commit, module commit, dependency versions, database revisions, configuration state, and client requirements used in every validation.
4. Moving references such as `latest`, `master`, `main`, or `current` are discovery aids, not reproducible version records. A record is not pinned until it contains immutable identifiers, including full commit SHAs for source-controlled inputs.
5. The Blaine integration baseline is selected only after the compatibility-convergence procedure reviews every desired module and feature.
6. The newest AzerothCore version is preferred only when it satisfies compatibility, maintainability, security, and validation requirements. It is not selected automatically.
7. An older baseline may be selected when evidence documents why it provides the best defensible convergence point and how its security and maintenance risks will be managed.
8. An unsupported or abandoned module must not silently force the permanent integrated baseline. Porting, replacement, removal, or independent reimplementation must be considered.
9. Every exception to this policy requires an ADR or a more specific decision record identified by the governing ADR.
10. Separate server instances may temporarily run different exact AzerothCore versions for research or transition.
11. Every instance exposes a source-and-module manifest containing exact pins, dependency versions, database revisions, configuration identity, client requirements, and evidence references.
12. Databases from different AzerothCore or module versions are not interchangeable unless migration, compatibility, backup, and rollback have been validated.

## Integration approval boundary

An individual clean build or successful individual runtime does not approve a module for the integrated realm. Promotion additionally requires combined build, database, startup, shutdown, runtime, gameplay, upgrade, rollback, regression, and conflict evidence under the candidate integration manifest.

## Platform scope

Linux and Windows use separate manifests and validation records. A pin validated on one platform remains unverified on the other until platform-specific execution and validation exist.

## Coordinated component pins

Where a module participates in a full-stack feature package, the addon, Realm Control definition, protocol, preset, and migration versions are recorded independently from the module and core commits. A compatible module build does not validate the client addon, control-plane integration, protocol, or combined package.
