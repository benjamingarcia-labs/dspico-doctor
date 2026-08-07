# Decision 0001: Select the First DSpico Doctor-Owned Deliverable

- **Status:** Accepted
- **Date:** 2026-08-07
- **Decision owner:** DSpico Doctor project
- **Related issue:** #14
- **Follow-on requirements:** #15

## Context

DSpico Doctor needed to select its first durable deliverable: something the project itself will own, maintain, validate, and document rather than an upstream-only contribution or an unverified experiment.

The initial research identified several possible paths, including a BOOTSEL-versus-microSD explanation, a macOS setup workflow, and a read-only microSD structure validator. Issue #14 required current upstream and project evidence to be re-inspected before making a choice, with particular attention to user value, duplication risk, scope, safety, legal constraints, testing feasibility, maintenance burden, and current capability.

During that review, the macOS candidate was refined from a vague "native macOS build workflow" into a narrower and evidence-backed setup workflow. The verified user friction was not principally native compilation. The observed difficulty was completing a reliable DSpico setup on macOS, especially preparing the microSD correctly and knowing the host-side commands required to reproduce a known-working configuration.

## Decision

DSpico Doctor selects the following as its first owned deliverable:

> **A verified macOS DSpico setup workflow using Docker for compilation where needed, plus safe FAT32/MBR microSD preparation, public file assembly, macOS metadata cleanup, and explicit validation and restricted-input boundaries.**

This decision selects the deliverable only. It does not mean the deliverable is implemented, documented for release, broadly compatible, or released.

## Evidence Reviewed

### Historical macOS setup evidence

A prior successful Apple Silicon macOS setup was reconstructed from local shell history, the original Docker workspace, the historical Docker image, and the surviving firmware artifact.

Recovered evidence included:

- historical Dockerfile SHA-256: `3937255e27f97ab530e8e92a59a8b715309c1cbe5f724dc7f030fb2868522106`;
- historical Docker image ID: `sha256:9baec065d392cb5bbafdeb2bfbd4f906e0a9659796a86644ee50869501960915`;
- historical `DSpico.uf2` SHA-256: `31377788360d05ca4ccb13f6b1d92d58d0a7b369039de6592a38e32136ea160e`;
- exact historical source revisions for DSRomEncryptor, DSpico Bootloader, DLDI, Firmware, and Wrfuxxed;
- historical macOS storage commands using `diskutil`, `newfs_msdos`, and `dot_clean`.

### Controlled Docker reproduction

A separate disposable reproduction workspace was created so the historical workspace remained unchanged.

The reproduction Dockerfile pinned:

- BlocksDS `slim-v1.22.1` for `linux/arm64` by digest;
- all five recovered repository revisions.

The controlled build completed successfully with exit status `0` and produced a new `DSpico.uf2`.

The historical and reproduction UF2 files were both exactly `3,277,824` bytes. A byte-level comparison found only four differing bytes, all within the embedded compilation-date string:

- historical: `Jul 24 2026`;
- reproduction: `Aug  7 2026`.

No other binary differences were found. This satisfied build reproducibility at the workflow level for the tested environment.

### macOS microSD formatting reproduction

The known-working 31.9 GB microSD used:

- MBR/FDisk partitioning;
- FAT32;
- 32 KiB allocation blocks.

On the tested macOS environment, `diskutil eraseDisk FAT32 DSPICO MBRFormat` recreated FAT32/MBR but produced 16 KiB clusters.

Running `newfs_msdos -F 32 -c 64 -v DSPICO` on the verified FAT32 partition reproduced 32 KiB clusters, matching the known-working card geometry.

This result is specific to the tested card and environment and must not be generalized to all media or macOS versions without additional evidence.

### Public file-layout validation

The test card was assembled using only public Pico Launcher and Pico Loader files. The layout included:

- `/_picoboot.nds`;
- `/_pico/picoLoader7.bin`;
- `/_pico/picoLoader9.bin`;
- `/_pico/aplist.bin`;
- `/_pico/savelist.bin`;
- `/_pico/patchlist.bin`;
- the public Launcher theme directories.

Each required copied file was verified byte-for-byte against its source using SHA-256.

Current upstream Pico Loader documentation requires `patchlist.bin`; the current DSpico guide omits it from its example card-preparation steps. The selected workflow should follow the authoritative component requirement while clearly documenting that upstream inconsistency rather than silently reproducing it.

### macOS metadata behavior

Copying files to FAT32 created AppleDouble `._*` metadata sidecars even when `.DS_Store` was excluded from the copy operation.

`dot_clean -m /Volumes/DSPICO` removed the AppleDouble sidecars from the accessible DSpico setup tree. A `.Spotlight-V100` access warning did not prevent the required cleanup. macOS also created `.fseventsd`, which is host-generated filesystem metadata and not part of the required DSpico file layout.

### Minimal hardware validation

The public-only card was tested in DSpico hardware. DSpico successfully read the card, Pico Launcher started, and the file-browser interface was usable.

The test did not establish broad game compatibility, DSiWare compatibility, compatibility across multiple consoles, or general compatibility across card models and sizes.

## Restricted-Input Boundary

The historical firmware build used private user-supplied BIOS and WRFU inputs. Their hashes were recorded only to identify the historical/reproduction inputs.

DSpico Doctor will not distribute, upload, embed, or provide unofficial download instructions for copyrighted BIOS files, Nintendo firmware, game files, WRFU Tester ROMs, or other restricted content.

Where restricted inputs are necessary for optional workflows, project documentation must keep them external and describe only lawful user-supplied prerequisites and validation boundaries.

## Alternatives Considered

### BOOTSEL-versus-microSD explanation

**Deferred as the primary owned deliverable.**

The need is real and beginner confusion was directly observed, but current upstream work already covers BOOTSEL, flashing, and microSD preparation in significant detail. A standalone DSpico Doctor deliverable would have relatively high duplication risk and would be too narrow to justify being the project's first owned artifact.

This topic may still appear as a supporting section or focused upstream documentation contribution where appropriate.

### Read-only microSD structure validator

**Deferred.**

A validator would be safe, testable, and technically suitable for a small owned software artifact. However, current evidence does not establish repeated user demand or a sufficiently strong verified need for a structure validator. Building it now would risk choosing a programming project because it is attractive to implement rather than because it solves the strongest demonstrated problem.

### Native macOS toolchain workflow

**Not selected as currently framed.**

The direct user evidence showed that native macOS compilation was not the primary setup problem. Docker had already provided a working compilation path. Installing or documenting a native toolchain would therefore expand scope beyond the verified need without resolving the principal friction.

## Why This Deliverable Was Selected

The selected workflow has the strongest current evidence because it:

- addresses a directly experienced macOS setup problem;
- has been reproduced from recorded inputs on Apple Silicon macOS;
- can be tested end-to-end without distributing restricted content;
- exposes concrete macOS-specific behavior that upstream Linux/WSL-oriented documentation does not fully cover;
- is narrow enough to understand, document, validate, and maintain;
- provides immediate beginner value while remaining compatible with future diagnostic or validation tooling;
- has clear stop, recovery, and safety boundaries for destructive storage operations.

## Consequences and Maintenance Ownership

DSpico Doctor owns the accuracy and maintenance of the macOS workflow it publishes.

That includes responsibility for:

- clearly separating confirmed behavior from assumptions and untested compatibility;
- recording the macOS, architecture, Docker, BlocksDS, upstream revision, and microSD environment used for validation;
- keeping destructive storage commands behind explicit target-identification and backup steps;
- tracking upstream DSpico, Pico Loader, Pico Launcher, BlocksDS, Docker, and macOS changes that materially affect the workflow;
- revalidating affected steps when those dependencies change materially;
- keeping restricted inputs external to the repository and public distribution path;
- avoiding claims that a single hardware success proves broad compatibility.

The workflow should prefer public release artifacts when compilation is unnecessary. Docker-based compilation belongs only where it materially supports the setup being documented.

## Required Validation for the Deliverable

Issue #15 must define the actual requirements and acceptance criteria. At minimum, requirements work should address:

- supported macOS versions and Apple Silicon assumptions;
- Docker prerequisites and pinned-versus-moving dependency policy;
- when compilation is required versus when official release artifacts should be used;
- safe removable-disk identification and recovery guidance;
- FAT32/MBR and allocation-block validation boundaries;
- required public card layout, including `patchlist.bin`;
- macOS metadata behavior and cleanup guidance;
- restricted-input handling;
- minimum hardware boot validation;
- evidence and compatibility claims permitted by the tested matrix;
- maintenance and reverification triggers.

## Conditions for Reconsideration

Reconsider this decision if:

- upstream publishes and maintains an equivalent macOS workflow that materially eliminates the unmet need;
- macOS, Docker, BlocksDS, or DSpico changes make the selected approach unsafe or disproportionately difficult to maintain;
- evidence shows the 32 KiB cluster requirement is unnecessary, harmful, or different across supported media in a way that changes the workflow design;
- a different verified user problem becomes materially more important before implementation begins;
- responsible validation would require distributing restricted content;
- the project cannot maintain the workflow within its stated evidence and safety standards.

## Related Records

- DSpico Doctor Issue #14 — deliverable selection and reproduction evidence
- DSpico Doctor Issue #15 — follow-on requirements work
- DSpico Doctor Issue #9 — paused firmware-validation research; not required for this selection unless firmware-specific validation becomes relevant
- DSpico Doctor Issue #19 — documentation lifecycle work; not blocking this decision
- Upstream DSpico setup guide and current Pico Loader/Pico Launcher documentation

## Validation Boundary

This decision proves that DSpico Doctor deliberately selected its first owned deliverable from reviewed evidence.

It does **not** prove that the final deliverable has been implemented, reviewed, released, or broadly compatible. Those claims require the follow-on requirements, implementation, documentation, testing, and release evidence defined by later project work.
