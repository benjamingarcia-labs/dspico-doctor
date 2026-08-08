# Requirements: macOS DSpico Setup Workflow

- **Status:** Approved requirements draft
- **Related decision:** Decision 0001
- **Tracking issue:** #15
- **Selected deliverable:** Verified macOS DSpico setup workflow

## 1. Purpose

The deliverable shall provide a safe, beginner-usable, evidence-backed procedure for setting up DSpico from macOS.

The workflow shall cover:

- obtaining the appropriate current public DSpico ecosystem artifacts;
- preparing compatible removable microSD media safely;
- installing the complete relevant default public Pico Launcher and DSpico Pico Loader structure;
- handling macOS-specific filesystem behavior;
- verifying the resulting setup;
- performing a minimum hardware boot validation;
- using Docker for compilation only when compilation is actually necessary;
- keeping restricted, copyrighted, or user-specific content outside the public base installation.

The workflow shall distinguish the source of material requirements instead of labeling every constraint as a DSpico requirement.

## 2. Requirement-Source Classification

Material requirements shall be classified using the following categories.

| Classification | Meaning |
| --- | --- |
| **DSPICO** | Requirement originating directly from DSpico |
| **UPSTREAM-COMPONENT** | Requirement originating from Pico Loader, Pico Launcher, BlocksDS, or another upstream component |
| **PLATFORM** | Nintendo hardware/platform, flashcart, SD/media, or related compatibility constraint |
| **HOST** | macOS, Docker, filesystem, or host-tool behavior |
| **PROJECT-SAFETY** | Safety, recovery, or evidence requirement imposed by DSpico Doctor |
| **VALIDATED-CONFIG** | Configuration independently demonstrated to work by DSpico Doctor but not established as universally required |

A requirement may have multiple classifications where appropriate.

This classification exists to answer: **Why does this requirement exist, and who or what owns it?**

## 3. Target User

The primary user is a DSpico owner using macOS who:

- can follow explicit terminal instructions;
- may have little familiarity with Unix disk identifiers;
- may not understand FAT32 geometry, Docker, firmware build systems, or DSpico component relationships;
- wants a reliable setup process without reconstructing instructions from several upstream repositories and community sources.

Embedded-development expertise shall not be assumed.

## 4. Intended Outcome

A successful installation shall leave the user with:

- correctly prepared removable media;
- the complete relevant public default Pico Launcher structure;
- the complete required DSpico Pico Loader contents for the selected release;
- optional public features left usable as upstream intends;
- restricted or user-specific features clearly separated;
- unnecessary AppleDouble files removed from relevant setup paths;
- an inspectable and verifiable card configuration;
- DSpico reaching a usable Pico Launcher file-browser interface on a documented tested hardware environment.

Firmware compilation is not required for the base public setup unless a specific supported outcome requires it.

## 5. Scope

The first version shall cover:

- Apple Silicon macOS;
- macOS 26.5.2, build 25F84, as the initial validated host;
- official public Pico Launcher artifacts;
- the DSpico-specific Pico Loader release;
- removable-media identification;
- FAT32/partition preparation appropriate to the supported configuration;
- filesystem-property inspection;
- complete default public card assembly;
- `patchlist.bin`;
- bundled Launcher themes and other shipped public support files;
- macOS metadata behavior and cleanup;
- integrity verification;
- minimum DSpico hardware boot validation;
- optional Docker-based compilation where justified;
- legal and restricted-input boundaries;
- destructive-operation recovery guidance.

## 6. Non-Goals

The initial deliverable shall not claim:

- Windows support;
- Linux support;
- Intel Mac support;
- support for every macOS version;
- compatibility with every microSD capacity or manufacturer;
- game compatibility;
- DSiWare compatibility;
- emuNAND configuration as part of the base setup;
- NAND management;
- ROM management;
- BIOS or Nintendo firmware acquisition;
- WRFU Tester acquisition;
- native macOS BlocksDS installation;
- automated microSD validation software;
- firmware modification;
- broad DSpico troubleshooting;
- universal flashcart or Nintendo-storage guidance.

Future evidence may justify expanded requirements or separate deliverables.

## 7. Supported and Validated Environment

### ENV-01 — Initial supported host
**Source:** VALIDATED-CONFIG

The first supported host environment shall be:

```text
Apple Silicon Mac
macOS 26.5.2
Build 25F84
```

### ENV-02 — Compatibility language
**Source:** PROJECT-SAFETY

Documentation shall distinguish:

- **validated** — directly tested;
- **supported** — deliberately claimed and maintained;
- **untested** — no compatibility claim.

Other macOS versions and Intel Macs shall initially be treated as untested unless separately validated.

### ENV-03 — Docker
**Source:** HOST / UPSTREAM-COMPONENT

Where compilation is used, the workflow shall record:

- Docker prerequisite;
- host architecture;
- container base;
- relevant toolchain;
- upstream revisions or release versions;
- dependency-pinning policy.

## 8. Public Artifact and Default-Installation Requirements

### ART-01 — Prefer official release artifacts
**Source:** PROJECT-SAFETY / maintainability

Official public release artifacts shall be preferred when they satisfy the installation requirement.

Compilation shall not be required merely because it is technically possible.

### ART-02 — Authoritative ownership
**Source:** UPSTREAM-COMPONENT

The workflow shall identify which upstream project owns each artifact and requirement.

### ART-03 — Default-release completeness
**Source:** UPSTREAM-COMPONENT

The workflow shall install the **complete relevant public structure supplied by the selected Pico Launcher release**, rather than reconstructing only the files required for a minimum boot test.

It shall then add the complete required DSpico-specific Pico Loader release contents.

This prevents DSpico Doctor from accidentally dropping default files or features simply because they were not needed during the initial validation experiment.

### ART-04 — Required Pico Loader data
**Source:** UPSTREAM-COMPONENT

While required by Pico Loader, the installation shall include at minimum:

```text
/_pico/aplist.bin
/_pico/patchlist.bin
/_pico/picoLoader7.bin
/_pico/picoLoader9.bin
/_pico/savelist.bin
```

### ART-05 — `patchlist.bin`
**Source:** UPSTREAM-COMPONENT

`patchlist.bin` shall be treated as a Pico Loader requirement, not as a DSpico-specific requirement.

### ART-06 — Pico Launcher boot file
**Source:** DSPICO / UPSTREAM-COMPONENT

For DSpico, Pico Launcher shall be installed under the upstream-required DSpico boot filename and location.

### ART-07 — Upstream inconsistencies
**Source:** PROJECT-SAFETY

When upstream documentation disagrees, the workflow shall:

1. identify the conflicting sources;
2. identify the component that owns the behavior;
3. document the inconsistency;
4. avoid silently converting inference into fact.

## 9. Feature-Accounting Requirements

### FEATURE-01 — Account for current public features
**Source:** UPSTREAM-COMPONENT

The guide shall account for features available in the selected upstream Pico Launcher/Pico Loader releases rather than documenting only what was necessary for the minimum boot test.

### FEATURE-02 — Feature classification
**Source:** PROJECT documentation requirement

Features shall be classified as:

```text
Default public setup
Optional public configuration
User-supplied / restricted feature
```

### FEATURE-03 — Default public features

Public files and structures shipped as part of the normal release shall be preserved during installation.

### FEATURE-04 — Optional public features

Features such as:

- display modes;
- file associations;
- covers;
- custom icons and banners;
- themes;
- background music;
- cheats;

shall be acknowledged where supported by the selected release.

The base setup does not need to preconfigure every optional feature, but it shall not remove or prevent their normal use.

### FEATURE-05 — Restricted/user-specific features

Features requiring BIOS, NAND-derived files, game content, or other user-specific/restricted inputs shall be documented separately from the public base installation.

They shall not be silently omitted, but they shall also not be bundled.

## 10. Storage Preparation Requirements

### STORAGE-01 — Backup before destructive operations
**Source:** PROJECT-SAFETY

The user shall be instructed to preserve any data they intend to keep before formatting.

### STORAGE-02 — Positive target identification
**Source:** PROJECT-SAFETY / HOST

The target removable device shall be positively identified before destructive commands.

Relevant characteristics should include:

- external/removable status;
- approximate capacity;
- protocol;
- current volume/partition information.

### STORAGE-03 — No fixed `/dev/diskN`
**Source:** HOST / PROJECT-SAFETY

The workflow shall never assume that a previously observed device identifier remains correct.

### STORAGE-04 — Stop on uncertainty
**Source:** PROJECT-SAFETY

If the intended disk cannot be confidently identified, the user shall stop before any write or format operation.

### STORAGE-05 — Filesystem preparation
**Source:** PLATFORM / upstream guidance / VALIDATED-CONFIG

The workflow shall prepare the card using filesystem characteristics appropriate to the supported Nintendo/DSpico environment.

The currently validated card was:

```text
31.9 GB
MBR/FDisk
FAT32
32 KiB allocation blocks
```

This is a validated configuration, not a universal DSpico requirement.

### STORAGE-06 — Allocation-block provenance
**Source:** PLATFORM / HOST / VALIDATED-CONFIG

The guide shall distinguish between:

- platform/storage guidance;
- host formatting behavior;
- upstream requirements;
- DSpico Doctor validation evidence.

### STORAGE-07 — ≤32 GB media
**Source:** upstream/platform formatting guidance

For the current 31.9 GB validated card, the workflow shall not claim that 32 KiB allocation blocks must be forced unless stronger authoritative evidence establishes that requirement.

### STORAGE-08 — Allocation-block inspection
**Source:** PROJECT-SAFETY

The resulting filesystem characteristics shall be inspected rather than assumed.

### STORAGE-09 — Larger cards
**Source:** PLATFORM / unresolved support boundary

Card capacities not independently validated by DSpico Doctor shall be identified as untested rather than automatically unsupported.

The guide may reference authoritative upstream formatting guidance for larger media without claiming independent DSpico Doctor validation.

## 11. File Assembly Requirements

### FILE-01 — Preserve release structure
**Source:** UPSTREAM-COMPONENT

The Pico Launcher release structure shall be copied as supplied except where DSpico-specific renaming or placement is explicitly required.

### FILE-02 — Deterministic destinations
**Source:** UPSTREAM-COMPONENT

DSpico-specific Loader files shall have explicit documented destination paths.

### FILE-03 — Preserve contents
**Source:** PROJECT-SAFETY

Upstream artifacts shall not be modified unless an authoritative upstream procedure requires transformation.

### FILE-04 — Integrity verification
**Source:** PROJECT-SAFETY / VALIDATED-CONFIG

Critical copied files shall be verifiable against their source using SHA-256 or equivalent deterministic comparison where practical.

## 12. Theme and Configuration Requirements

### CONFIG-01 — Preserve bundled themes
**Source:** UPSTREAM-COMPONENT

Themes shipped in the selected Pico Launcher release shall be preserved as part of the normal release structure.

### CONFIG-02 — Theme installation vs selection
**Source:** UPSTREAM-COMPONENT

The guide shall distinguish between:

- installing theme assets;
- selecting the active theme.

### CONFIG-03 — `settings.json`
**Source:** UPSTREAM-COMPONENT

The guide shall follow normal Pico Launcher behavior for `/_pico/settings.json`.

DSpico Doctor shall not invent a custom default configuration merely to make a theme appear unless upstream behavior or user requirements justify it.

### CONFIG-04 — Configuration completeness
**Source:** PROJECT documentation requirement

The user shall be told which features are:

- already available after installation;
- configurable later;
- dependent on additional public assets;
- dependent on user-supplied/restricted material.

## 13. macOS Metadata Requirements

### META-01 — AppleDouble awareness
**Source:** HOST / VALIDATED-CONFIG

The guide shall document that macOS may create `._*` AppleDouble files on FAT32 media.

### META-02 — Cleanup
**Source:** HOST / VALIDATED-CONFIG

A tested cleanup procedure shall be provided.

Current validated method:

```text
dot_clean -m
```

### META-03 — Cleanup verification
**Source:** PROJECT-SAFETY

The workflow shall inspect whether AppleDouble files remain after cleanup.

### META-04 — macOS-managed metadata
**Source:** HOST

Directories such as:

```text
.fseventsd
.Spotlight-V100
```

shall not automatically be treated as DSpico setup failures.

## 14. Restricted-Content Requirements

### LEGAL-01 — No redistribution

DSpico Doctor shall not redistribute material for which it lacks redistribution rights, including:

- Nintendo BIOS files;
- Nintendo firmware;
- game ROMs;
- WRFU Tester ROMs;
- NAND-derived restricted files;
- other restricted content.

### LEGAL-02 — No unofficial acquisition dependency

The guide shall not rely on unofficial archives or piracy sources.

### LEGAL-03 — User-supplied inputs

Where optional functionality requires restricted input, documentation may describe lawful user-supplied prerequisites without redistributing them.

### LEGAL-04 — Base public setup

The complete default **public** installation shall remain possible without BIOS, NAND, game ROM, or other restricted content.

## 15. Compilation Requirements

### BUILD-01 — Compilation is conditional
**Source:** PROJECT requirement

Compilation shall not be part of the required beginner path unless necessary for a supported result.

### BUILD-02 — Reproducibility
**Source:** PROJECT-SAFETY / UPSTREAM-COMPONENT

A documented build path shall record:

- container image/version/digest;
- source revisions;
- architecture;
- relevant inputs;
- build command;
- intended artifact.

### BUILD-03 — Avoid moving dependency ambiguity
**Source:** maintainability

Moving tags such as `latest` shall not be sufficient reproducibility evidence without recording the resolved version/digest.

### BUILD-04 — Build-success boundary
**Source:** PROJECT-SAFETY

A successful build proves compilation only.

### BUILD-05 — Output isolation
**Source:** maintainability

The workflow should expose the intended firmware artifact, not unrelated build-directory remnants.

## 16. Verification Requirements

### VERIFY-01 — Storage state

The workflow shall inspect relevant partition/filesystem properties after preparation.

### VERIFY-02 — Default installation completeness

The installed card shall be checked against the selected release structure and required DSpico-specific additions.

### VERIFY-03 — Integrity

Critical copied files shall be checked for unintended modification where practical.

### VERIFY-04 — Metadata

AppleDouble cleanup shall be verified.

### VERIFY-05 — Host verification before hardware test
**Source:** PROJECT-SAFETY

A hardware boot shall not substitute for host-side inspection.

## 17. Minimum Hardware Acceptance

### HW-01 — Acceptance test

The minimum functional acceptance test is:

```text
DSpico recognized
      ↓
microSD readable
      ↓
Pico Launcher starts
      ↓
usable file-browser interface reached
```

### HW-02 — Test-environment recording
**Source:** VALIDATED-CONFIG

The hardware environment used shall be recorded sufficiently to identify what was actually tested.

### HW-03 — Claim boundary
**Source:** PROJECT-SAFETY

A successful boot shall not be presented as proof of:

- universal console compatibility;
- universal card compatibility;
- game compatibility;
- DSiWare compatibility;
- long-term stability.

The hardware test proves only the defined acceptance criterion.

## 18. Failure and Recovery Requirements

### FAIL-01 — Ambiguous storage target

Stop before destructive operations.

### FAIL-02 — Formatting failure

Re-inspect the card before proceeding.

### FAIL-03 — Unexpected filesystem state

Do not declare preparation successful merely because formatting completed.

### FAIL-04 — File mismatch

Correct integrity failures before hardware testing.

### FAIL-05 — Incomplete default structure

Missing files or directories expected from the selected default public release structure shall be investigated before declaring installation complete.

### FAIL-06 — Hardware boot failure

Failure to reach Pico Launcher's expected interface means the minimum acceptance criterion failed.

Do not add unrelated restricted files simply to force success.

### FAIL-07 — Recovery information

Consequential steps shall identify:

- expected change;
- meaningful risk;
- stop condition;
- reasonable recovery path.

## 19. Documentation Requirements

The eventual user-facing guide shall:

- be organized around completing the task;
- keep required actions distinguishable from explanation;
- clearly identify destructive steps;
- explain consequential commands;
- show expected verification outcomes;
- include explicit stop conditions;
- distinguish required, optional, and restricted content;
- distinguish upstream requirements from DSpico Doctor safety requirements;
- distinguish validated configuration from universal requirement;
- distinguish tested, supported, and untested environments;
- preserve the complete default public release structure;
- account for available upstream features;
- identify authoritative upstream sources;
- clearly state DSpico Doctor's independent status;
- avoid requiring users to understand the project's internal research history.

## 20. Acceptance Criteria

The deliverable shall not be considered to satisfy these requirements until evidence shows that:

1. the supported host environment is explicit;
2. prerequisites are documented;
3. authoritative upstream artifacts are identified;
4. material requirement provenance is recorded;
5. removable media can be safely identified;
6. the documented storage configuration can be created and inspected;
7. storage constraints are not incorrectly attributed to DSpico;
8. the complete relevant public Launcher release structure is preserved;
9. the required DSpico Pico Loader release contents are installed;
10. `patchlist.bin` is handled according to current Pico Loader requirements;
11. available upstream features are accounted for;
12. optional and restricted features are clearly differentiated;
13. macOS AppleDouble behavior is addressed;
14. critical copied files can be verified;
15. no restricted material is required for the complete default public installation;
16. the prepared setup passes the minimum hardware boot test;
17. the validation environment is recorded;
18. unsupported and untested environments are explicit;
19. stop and recovery conditions exist for consequential operations;
20. maintenance/revalidation triggers are defined.

## 21. Required Validation Categories

Phase 4 validation shall plan for:

- prerequisite verification;
- release-artifact verification;
- default-release structure comparison;
- Docker build reproduction where applicable;
- dependency recording;
- removable-disk identification;
- destructive-operation safeguards;
- filesystem formatting;
- filesystem-property inspection;
- complete public file assembly;
- integrity verification;
- AppleDouble behavior and cleanup;
- incomplete/default-structure failure cases;
- optional feature readiness;
- restricted-input boundary review;
- minimum hardware boot validation;
- beginner-oriented documentation walkthrough.

## 22. Maintenance and Revalidation Triggers

Affected requirements shall be reviewed when material changes occur in:

- DSpico;
- Pico Loader;
- Pico Launcher;
- release archive contents;
- default Launcher file structure;
- BlocksDS;
- Docker/container dependencies;
- macOS storage utilities;
- supported macOS versions;
- relevant Nintendo/platform storage guidance;
- filesystem recommendations;
- supported card capacities;
- restricted-input requirements;
- hardware behavior relevant to the acceptance test.

A change triggers **review**, not an automatic assumption of incompatibility.

## 23. Current Validated Configuration

This records current evidence rather than universal requirements.

```text
HOST
  Apple Silicon Mac
  macOS 26.5.2
  build 25F84

BUILD
  controlled ARM64 Docker reproduction completed

STORAGE
  one 31.9 GB microSD
  MBR/FDisk
  FAT32
  known-working test configuration used 32 KiB allocation blocks

PUBLIC FILES
  Pico Launcher release structure
  DSpico Pico Loader files
  patchlist.bin included
  critical source/destination files SHA-256 matched

macOS BEHAVIOR
  AppleDouble creation observed
  dot_clean cleanup verified
  .fseventsd / .Spotlight-V100 behavior observed

HARDWARE
  DSpico successfully loaded Pico Launcher
  usable file-browser interface reached
```

The 32 KiB allocation-block result is recorded as a working configuration, not a universal DSpico requirement.

## 24. Remaining Unresolved Questions

### Q1 — Broader macOS support

Initial support is macOS 26.5.2 on Apple Silicon.

Other versions can be added later through evidence rather than assumption.

This is not blocking.

### Q2 — Card-capacity support expansion

DSpico Doctor has independently validated one approximately 32 GB card.

Larger or smaller capacities should remain **untested** rather than being declared unsupported.

This is not blocking the first release.

### Q3 — Compilation placement

We still need to decide whether the Docker compilation workflow belongs:

- in the primary setup guide;
- in an optional advanced section;
- or in a separately linked guide.

Current recommendation: keep compilation out of the normal beginner path unless it is required for a feature the base public release cannot provide.

## Traceability and Validation Boundary

These requirements are governed by Decision 0001 and tracked through Issue #15. The detailed selection and reproduction evidence remains in Issue #14 and the decision record.

Approved requirements define what the deliverable must do and how success will be evaluated. They do **not** prove that the final guide has been implemented, reviewed, tested against every acceptance criterion, or released.
