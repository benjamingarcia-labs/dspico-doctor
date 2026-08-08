# DSpico Doctor

DSpico Doctor is an independent, open-source project focused on making the DSpico ecosystem easier to understand, diagnose, maintain, and recover safely.

The project is currently establishing its technical foundation through user-problem research, upstream repository analysis, system documentation, and disciplined contribution practices. Its first DSpico Doctor-owned deliverable is a verified macOS DSpico setup workflow covering Docker-based compilation where needed, safe FAT32/MBR microSD preparation, public file assembly, macOS metadata cleanup, and explicit validation and restricted-input boundaries. Requirements for that deliverable are defined, the repository-managed firmware builder has completed build-level revalidation, and the beginner-oriented end-user guide has completed the approved workflow and minimum hardware validation boundary on the recorded Apple Silicon macOS environment.

## Project Goals

DSpico Doctor aims to provide useful, safe, and maintainable value for DSpico users by developing:

- clear beginner-oriented guidance;
- documented diagnostic and troubleshooting processes;
- safe and reversible recovery guidance;
- verified setup and platform-specific workflows;
- focused tools or documentation supported by demonstrated user needs;
- transparent records of testing, limitations, and unresolved behavior.

The project favors small, understandable, testable contributions over broad or speculative feature development.

## Current Project Maturity

DSpico Doctor has completed its first owned deliverable at the approved validation boundary. The pinned repository-managed firmware builder completed fresh no-cache build-level revalidation on the recorded Apple Silicon environment, and the beginner-oriented macOS setup guide has completed the documented end-to-end workflow and minimum hardware boot validation needed for the current scope.

Work completed so far includes:

- defining the project vision, scope, constraints, and initial user problem;
- researching the DSpico repository ecosystem and component relationships;
- documenting an initial high-level system overview;
- establishing contribution guidance and structured issue templates;
- contributing a focused documentation correction to the upstream DSpico project;
- selecting the first DSpico Doctor-owned deliverable from reviewed and validated evidence;
- defining requirements, scope, safety boundaries, acceptance criteria, and validation categories for the selected macOS setup workflow;
- implementing and freshly revalidating the pinned repository-managed Docker firmware builder at the build level on the recorded Apple Silicon environment;
- implementing and validating the beginner-oriented macOS setup guide through the approved workflow and minimum hardware boot acceptance boundary.

The current evidence does not establish broad macOS support, all-card compatibility, all-console compatibility, game compatibility, DSiWare compatibility, emuNAND compatibility, or other behavior beyond the recorded validation environment and workflow.

The repository does not currently provide a released diagnostic application, automated recovery tool, or firmware modification.

Planned or in-progress capabilities should not be treated as supported until implementation, testing, review, and applicable release evidence are available.

## Repository Navigation

### Project Foundation

- [Project Vision](docs/vision.md) — Describes the long-term direction and guiding principles of DSpico Doctor.
- [Project Charter](docs/project-charter.md) — Defines the initial user problem, scope, non-goals, constraints, risks, and research questions.
- [Project Roadmap](docs/roadmap.md) — Defines the project's phase gates from foundational research through deliverable selection, implementation, validation, and release preparation.
- [Decision 0001: First Owned Deliverable](docs/decisions/0001-select-first-dspico-doctor-deliverable.md) — Records the evidence-backed selection of the project's first owned deliverable and its validation boundaries.
- [macOS DSpico Setup Workflow Requirements](docs/requirements/macos-dspico-setup-workflow.md) — Defines the approved scope, requirement provenance, safety boundaries, acceptance criteria, validation categories, and current supported environment for the first owned deliverable.

### Guides

- [macOS DSpico Setup Guide](docs/guides/macos-dspico-setup.md) — Beginner-oriented end-to-end setup guide covering local firmware preparation, BOOTSEL flashing, microSD preparation, public Launcher/Loader installation, macOS metadata cleanup, verification, and minimum hardware boot validation on the recorded Apple Silicon macOS environment.

### Implementation

- [DSpico Firmware Builder](tools/firmware-builder/README.md) — Contains the pinned Docker firmware-build support used by the macOS setup workflow. This repository version completed build-level revalidation on the recorded Apple Silicon environment and is incorporated into the validated macOS setup workflow.

### Research

- [Research Overview](docs/research/README.md) — Provides navigation to the project's research records.
- [Initial Research Findings](docs/research/initial-findings.md) — Records the initial investigation of DSpico documentation, repositories, community evidence, and possible contribution paths.
- [DSpico System Overview](docs/research/system-overview.md) — Documents how major DSpico software and firmware components interact based on source and repository inspection.

Research records distinguish confirmed findings from assumptions, inference, recommendations, unresolved questions, and behavior that has not been independently tested.

### Contributing

- [Contribution Guidelines](CONTRIBUTING.md) — Explains project scope, development workflow, testing expectations, evidence standards, and upstream contribution boundaries.
- [Open Issues](https://github.com/benjamingarcia-labs/dspico-doctor/issues) — Tracks active, paused, blocked, deferred, and proposed project work.
- [Pull Requests](https://github.com/benjamingarcia-labs/dspico-doctor/pulls) — Contains proposed repository changes and their review history.

Use the repository's structured issue forms to report a bug or propose a feature.

## Relationship to DSpico

DSpico Doctor is an independent companion project. It is not an official repository or product of the DSpico maintainers.

The official DSpico project and its component repositories remain authoritative for upstream source code, releases, documentation, hardware behavior, firmware behavior, supported workflows, and maintainer decisions.

Research, documentation, experiments, forks, and unmerged changes developed through DSpico Doctor must not be presented as official DSpico functionality unless they are accepted by the appropriate upstream maintainers.

## Evidence and Safety Boundaries

DSpico Doctor follows these evidence boundaries:

- Source inspection does not prove runtime or hardware behavior.
- A successful build does not prove feature correctness.
- One successful hardware test does not prove broad compatibility.
- Community reports are evidence of reported experiences, not independent verification.
- macOS, Windows, firmware, hardware, and storage compatibility must not be claimed without supporting evidence.
- AI-generated material must be reviewed, understood, and validated before it is treated as project evidence.

Firmware flashing, storage modification, reformatting, or other potentially destructive operations require understood backup, recovery, target-identification, and controlled-validation procedures before they are introduced as supported user workflows.

## Legal and Content Restrictions

DSpico Doctor does not distribute copyrighted BIOS files, firmware files, game files, or other restricted content.

Contributors are responsible for following applicable licenses, preserving attribution, and ensuring that submitted material may legally be included in the repository.

## Contributing

Contributions should be narrow, evidence-based, testable, documented, and consistent with the project's current scope.

Before beginning substantial work:

1. Read the [contribution guidelines](CONTRIBUTING.md).
2. Review the project documentation and current issues.
3. Search for related upstream issues and pull requests.
4. Open an issue when the proposed change affects code, firmware, architecture, hardware workflows, or project scope.
5. Clearly record what was tested, what was observed, and what remains unverified.
