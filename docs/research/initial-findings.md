# Initial DSpico Research Findings

## Purpose

This document records the initial research used to evaluate beginner setup problems and possible first contributions for DSpico Doctor.

The findings are preliminary and time-bounded. Major source-dependent claims link to the companion [evidence register](initial-findings-evidence.md), which records exact sources, revisions, inspection dates, classifications, and validation limits.

## Sources Reviewed

Initial research included:

- official `LNH-team` repositories and documentation;
- official GitHub releases;
- upstream issues and pull requests;
- direct inspection of published release archives;
- direct beginner setup experience; and
- community documentation consulted during the original investigation.

The exact community sources and revisions were not preserved. Those recollections are not treated as reproducible evidence in the current findings. See the [community-source boundary](initial-findings-evidence.md#community-source-boundary).

## Upstream Project Structure

The DSpico project is organized across multiple repositories. The `LNH-team/dspico` repository serves as the central project index and links to separate repositories for hardware, firmware, bootloader, DLDI, Wrfuxxed, Pico Loader, Pico Launcher, and USB examples. Research and contribution planning must therefore consider several repositories rather than treating DSpico as one codebase. See [E-01](initial-findings-evidence.md#e-01).

## Confirmed User Evidence

During one initial beginner setup on macOS, the following problems occurred:

1. The user did not clearly understand the difference between the RP2040 BOOTSEL drive and direct access to the microSD card.
2. The user copied individual files to the microSD card without realizing that the complete directory structure and launcher components were also required.
3. The available instructions were difficult to apply from the user's environment because the documented workflow was oriented toward Linux or WSL.

These observations are confirmed for one user only. They do not establish prevalence across the wider DSpico community. See [E-13](initial-findings-evidence.md#e-13).

## Confirmed Documentation Findings

### BOOTSEL and microSD Are Separate Storage Targets

The official guide treats firmware flashing and microSD preparation as separate workflows. The BOOTSEL workflow copies `DSpico.uf2` to the RP2040 USB drive, while the microSD workflow stores Pico Launcher, Pico Loader, themes, lists, and user content. See [E-02](initial-findings-evidence.md#e-02).

The guide documents the two workflows, but whether the distinction is sufficiently clear for beginners is an interpretation informed by the direct user observation rather than an upstream conclusion.

### The Official Guide Uses a Source-Build Workflow

The main DSpico guide documents Linux or WSL prerequisites and instructs the user to compile DSpico DLDI, the bootloader, firmware, Pico Loader, Pico Launcher, and optional Wrfuxxed components. See [E-03](initial-findings-evidence.md#e-03).

A complete native macOS workflow has not been verified by this research. This does not establish that native macOS is unsupported or impossible. See [E-14](initial-findings-evidence.md#e-14).

### Prebuilt Pico Loader and Pico Launcher Releases Exist

Official prebuilt release assets exist for Pico Launcher and the DSpico-specific Pico Loader:

- `Pico_Launcher.zip` from Pico Launcher `v1.3.0`; and
- `Pico_Loader_DSPICO.zip` from Pico Loader `v1.7.1`.

See [E-04](initial-findings-evidence.md#e-04) and [E-05](initial-findings-evidence.md#e-05).

### The Release Packages Must Be Combined

Direct archive inspection confirmed that the Launcher archive contains `LAUNCHER.nds`, `_pico/`, and the theme structure, while the Loader archive contains:

- `aplist.bin`;
- `patchlist.bin`;
- `picoLoader7.bin`;
- `picoLoader9.bin`; and
- `savelist.bin`.

The documented assembly process places the Loader files in the Launcher-provided `_pico` directory, renames `LAUNCHER.nds` to `_picoboot.nds`, and copies `_pico` and `_picoboot.nds` to the microSD root. See [E-06](initial-findings-evidence.md#e-06), [E-07](initial-findings-evidence.md#e-07), and [E-08](initial-findings-evidence.md#e-08).

The archives and documented file-placement process were inspected, but this release pair was not independently tested on hardware during this research.

### `patchlist.bin` Documentation Is Inconsistent

The current Pico Loader README lists `patchlist.bin` among the files required in `/_pico`, while the inspected DSpico guide revision omits it from both the copy steps and final directory example. See [E-09](initial-findings-evidence.md#e-09) and [E-10](initial-findings-evidence.md#e-10).

Upstream Issue #7, `Add patchlist.bin to guide`, remained open when inspected on 2026-08-05. Its body is empty, so the title is the available direct description of the issue. See [E-11](initial-findings-evidence.md#e-11).

## Existing Upstream Work

### Pull Request #5 — Create Extended Guide

Pull Request #5 proposes a detailed guide aimed at users unfamiliar with terminal or Linux environments. The contributor reports successfully compiling firmware with the documented steps. The pull request remained open and unmerged when inspected on 2026-08-05. See [E-12](initial-findings-evidence.md#e-12).

The contributor's reported success is not independent validation or maintainer acceptance. The pull request's state and contents must be reverified before relying on it for a future decision.

## Community Evidence Boundary

Community documentation was consulted during the original investigation, but the exact sources, revisions, and inspection dates were not preserved. A later search did not recover a clearly independent source set that reproducibly supported all of the original community-evidence statements.

The earlier broad claims about community guides are therefore not treated as confirmed evidence. They should be reconsidered only after exact sources are recovered or newly inspected and classified.

## Contribution Candidates

### Candidate A — Fix `patchlist.bin` Documentation

**Value**

- resolves a confirmed documentation inconsistency;
- aligns the main guide with the Pico Loader README; and
- directly relates to upstream Issue #7.

**Concerns**

- the task may be too small to serve as the main DSpico Doctor-owned deliverable; and
- the issue is already known upstream.

### Candidate B — Improve BOOTSEL and microSD Explanation

**Value**

- addresses a mental-model problem observed during one beginner setup;
- helps explain which files belong to which storage target; and
- may reduce setup mistakes.

**Concerns**

- related guide work exists in open Pull Request #5; and
- any new work must avoid duplicating active upstream effort.

### Candidate C — Native macOS Build Guidance

**Value**

- addresses a workflow gap encountered during the initial setup; and
- could help users working outside the guide's documented Linux or WSL environment.

**Concerns**

- native macOS compatibility has not been verified;
- the complete workflow may require unsupported tools or modifications; and
- documentation must not be written before the workflow is successfully reproduced.

### Candidate D — Read-Only microSD Validator

Possible behavior:

- accept a mounted microSD path;
- check for required files and folders;
- report missing, optional, and unexpected items;
- perform no writes or formatting; and
- produce a plain-language result.

**Value**

- provides a focused programming and testing opportunity;
- directly checks whether the card structure is complete; and
- avoids firmware and hardware-write risk.

**Concerns**

- repeated community demand has not been demonstrated;
- maintainer interest is unknown; and
- the tool may be unnecessary if improved documentation is sufficient.

## Candidates Not Recommended

### Broad Beginner Guide

Not currently recommended because open Pull Request #5 already proposes a detailed guide. Its state and overlap must be reverified before a later decision.

### New Combined Pico Package

Not currently recommended because this research has not established a maintenance, licensing, or user-need basis for distributing a new combined package.

### Automatic Flasher

Not recommended because it introduces write operations, hardware risk, recovery requirements, and significantly greater testing burden.

## Current Conclusions

The initial user problems are supported as direct observations, but the best first DSpico Doctor-owned deliverable has not yet been selected.

The strongest confirmed documentation issue is the omission of `patchlist.bin` from the inspected DSpico guide revision.

The strongest unresolved platform question is whether a complete native macOS workflow can be safely reproduced and supported.

The documented workflow can reasonably be inferred to create beginner friction in some cases, but the available evidence does not establish community-wide prevalence. The read-only validator remains technically plausible but lacks demonstrated repeated demand. See [E-15](initial-findings-evidence.md#e-15).

Before these findings are used for deliverable selection, affected time-sensitive evidence must be reverified according to the [maintenance and reverification procedure](initial-findings-evidence.md#maintenance-and-reverification).

## Open Questions

- Is a native macOS build workflow officially supported or reproducibly workable?
- Would maintainers prefer a narrow documentation correction or a separate utility?
- Is Issue #7 available for a first-time contributor?
- Does Pull Request #5 have a likely merge path?
- Are current Pico Loader and Pico Launcher release versions expected to remain mutually compatible?
- Would a read-only validator provide enough value to justify ongoing maintenance?

## Validation Boundary

This document summarizes inspected sources, direct observations, inferences, and unresolved questions. It does not prove successful builds, runtime compatibility, hardware behavior, maintainer preference, or community-wide demand.