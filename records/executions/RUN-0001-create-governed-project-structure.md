# RUN-0001: Create Governed Project Structure

Status: **Executed**  
Date and local time: **2026-07-14T16:38:35-07:00**  
Working directory: **`/home/deck`**  
Authorized project root: **`/run/media/deck/blAIneXPLAT/azerothcore/`**

## Objective

Create only the governed directory structure and initial authoritative Markdown documents while keeping the project at S0.

## Pre-write command

```bash
pwd
if [ -d /run/media/deck/blAIneXPLAT/ ]; then
    printf 'MOUNT_PATH_EXISTS=yes\n'
else
    printf 'MOUNT_PATH_EXISTS=no\n'
fi
if [ -e /run/media/deck/blAIneXPLAT/azerothcore/ ]; then
    printf 'PROJECT_ROOT_EXISTS=yes\n'
    find /run/media/deck/blAIneXPLAT/azerothcore/ -print | sort
else
    printf 'PROJECT_ROOT_EXISTS=no\n'
fi
```

Observed output:

```text
/home/deck
MOUNT_PATH_EXISTS=yes
PROJECT_ROOT_EXISTS=no
```

Exit status: `0`.

## Directory-creation command

```bash
mkdir -p \
  /run/media/deck/blAIneXPLAT/azerothcore/docs/governance \
  /run/media/deck/blAIneXPLAT/azerothcore/docs/architecture \
  /run/media/deck/blAIneXPLAT/azerothcore/docs/requirements \
  /run/media/deck/blAIneXPLAT/azerothcore/docs/public \
  /run/media/deck/blAIneXPLAT/azerothcore/records/decisions \
  /run/media/deck/blAIneXPLAT/azerothcore/records/audits \
  /run/media/deck/blAIneXPLAT/azerothcore/records/executions \
  /run/media/deck/blAIneXPLAT/azerothcore/records/validations \
  /run/media/deck/blAIneXPLAT/azerothcore/records/incidents \
  /run/media/deck/blAIneXPLAT/azerothcore/records/releases \
  /run/media/deck/blAIneXPLAT/azerothcore/linux/source \
  /run/media/deck/blAIneXPLAT/azerothcore/linux/config \
  /run/media/deck/blAIneXPLAT/azerothcore/linux/scripts \
  /run/media/deck/blAIneXPLAT/azerothcore/linux/compose \
  /run/media/deck/blAIneXPLAT/azerothcore/linux/documentation \
  /run/media/deck/blAIneXPLAT/azerothcore/linux/records \
  /run/media/deck/blAIneXPLAT/azerothcore/linux/manifests \
  /run/media/deck/blAIneXPLAT/azerothcore/linux/backups \
  /run/media/deck/blAIneXPLAT/azerothcore/windows/source \
  /run/media/deck/blAIneXPLAT/azerothcore/windows/config \
  /run/media/deck/blAIneXPLAT/azerothcore/windows/scripts \
  /run/media/deck/blAIneXPLAT/azerothcore/windows/documentation \
  /run/media/deck/blAIneXPLAT/azerothcore/windows/records \
  /run/media/deck/blAIneXPLAT/azerothcore/windows/manifests \
  /run/media/deck/blAIneXPLAT/azerothcore/windows/backups
```

Exit status: `0`.

## Directories created

```text
azerothcore/
docs/{governance,architecture,requirements,public}/
records/{decisions,audits,executions,validations,incidents,releases}/
linux/{source,config,scripts,compose,documentation,records,manifests,backups}/
windows/{source,config,scripts,documentation,records,manifests,backups}/
```

## Files created

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

Files were authored through the workspace patch operation; the operation completed successfully. Validation commands and outcomes are recorded in `VAL-0001`.

## Timestamp and validation commands

The local timestamps were observed with:

```bash
date --iso-8601=seconds
```

Observed timestamps were `2026-07-14T16:38:35-07:00` and `2026-07-14T16:41:50-07:00`; both invocations exited `0`.

The exact required-path arrays, existence loops, canonical-boundary checks, platform comparison, lifecycle searches, entry inventory, Git detection, final tree, Markdown display, and their results are recorded in `VAL-0001`. The first validation command exited `0`. The final presentation-and-existence command is the post-record validation defined there.

## Warnings and unexpected behavior

The privileged directory-creation operation returned asynchronously after an extended authorization/tool wait, then completed with exit status `0` and no stderr. No partial failure was observed. This behavior is retained here rather than omitted.

## Modification boundary

Only directories and Markdown files under the authorized project root were created. No software was installed. No package, service, container, database, firewall, mount, partition, filesystem, game-client, or Git configuration was changed. No Git repository or remote was created.
