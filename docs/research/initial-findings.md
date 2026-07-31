# Initial DSpico Research Findings

## Purpose

This document records the initial research used to evaluate beginner setup problems and possible first contributions for DSpico Doctor.

The findings are preliminary. They should be updated when new repository evidence, community feedback, maintainer guidance, or testing results become available.

## Sources Reviewed

Initial research included:

- `LNH-team/dspico`
- `LNH-team/dspico-firmware`
- `LNH-team/dspico-bootloader`
- `LNH-team/pico-loader`
- `LNH-team/pico-launcher`
- the official DSpico setup guide
- GitHub releases for major DSpico components
- upstream issues and pull requests
- community setup guides
- direct beginner setup experience

## Upstream Project Structure

The DSpico project is organized across multiple repositories.

The `LNH-team/dspico` repository serves as the central project index and links to separate repositories for:

- hardware
- firmware
- bootloader
- DLDI
- Wrfuxxed
- Pico Loader
- Pico Launcher
- USB examples

This means research and contribution planning must consider several repositories rather than treating DSpico as one codebase.

## Confirmed User Evidence

During an initial beginner setup, the following problems occurred:

1. The user did not clearly understand the difference between the RP2040 BOOTSEL drive and direct access to the microSD card.

2. The user copied individual files to the microSD card without realizing that the complete directory structure and launcher components were also required.

3. The available instructions appeared primarily oriented toward Windows and Linux workflows, while the user was working on macOS.

These observations are confirmed for one user. They do not yet prove that the same problems are widespread across the DSpico community.

## Confirmed Documentation Findings

### BOOTSEL and microSD Are Separate Storage Targets

The official guide describes firmware flashing and microSD preparation in separate sections.

The BOOTSEL workflow uses the USB storage volume exposed by the RP2040 and receives `DSpico.uf2`.

The microSD card stores Pico Launcher, Pico Loader, themes, lists, and user content.

The official guide describes both workflows but does not provide a concise beginner-oriented comparison explaining their different purposes.

### The Official Guide Is a Source-Build Workflow

The main DSpico guide instructs the user to compile:

- DSpico DLDI
- DSpico Bootloader
- DSpico Firmware
- Pico Loader
- Pico Launcher
- optional Wrfuxxed components

The documented prerequisites are Linux or Windows Subsystem for Linux.

A complete native macOS workflow has not been verified.

### Prebuilt Pico Loader and Pico Launcher Releases Exist

The Pico Loader and Pico Launcher repositories provide downloadable release packages.

The relevant packages include:

- `Pico_Loader_DSPICO.zip`
- `Pico_Launcher.zip`

These packages can simplify microSD preparation by allowing users to avoid compiling Pico Loader and Pico Launcher from source.

### The Release Packages Must Be Combined

The launcher release includes:

- `_pico/`
- `LAUNCHER.nds`

The loader release includes:

- `picoLoader7.bin`
- `picoLoader9.bin`
- `aplist.bin`
- `savelist.bin`
- `patchlist.bin`

The user must:

1. copy the loader files into the launcher's `_pico` folder
2. rename `LAUNCHER.nds` to `_picoboot.nds`
3. copy `_pico` and `_picoboot.nds` to the microSD root

### `patchlist.bin` Documentation Is Inconsistent

The current Pico Loader README identifies `patchlist.bin` as part of the required `_pico` contents.

The main DSpico guide and Pico Launcher final directory examples omit it.

Upstream issue #7, titled `Add patchlist.bin to guide`, already records this problem.

## Existing Upstream Work

### Pull Request #5 — Create Extended Guide

An open pull request proposes a detailed beginner-oriented guide focused on Windows 11 and WSL.

The proposed guide includes:

- detailed terminal explanations
- step-by-step compilation instructions
- BOOTSEL drive identification
- a prebuilt Pico Loader and Pico Launcher path
- final microSD assembly instructions

The pull request remains open and unmerged.

It does not provide a native macOS workflow.

Its displayed final microSD structure still omits `patchlist.bin`.

## Community Evidence

Community setup guides already provide simpler workflows than the main source-build guide.

Some community documentation:

- distinguishes the `RPI-RP2` BOOTSEL drive from the microSD card
- provides combined or simplified packages
- reduces the need for users to manually assemble multiple releases
- recommends prebuilt files for ordinary setup

This supports the conclusion that the official source-build workflow creates beginner friction.

However, research has not yet shown strong repeated demand for an automated microSD validation tool.

## Contribution Candidates

### Candidate A — Fix `patchlist.bin` Documentation

Value:

- resolves a confirmed documentation inconsistency
- aligns the main guide with the Pico Loader README
- directly relates to upstream issue #7

Concerns:

- the task may be too small to serve as the main DSpico Doctor contribution
- the issue is already known upstream

### Candidate B — Improve BOOTSEL and microSD Explanation

Value:

- addresses a real beginner mental-model problem
- helps users understand which files belong to which storage target
- can reduce setup mistakes

Concerns:

- partially addressed by open pull request #5
- must avoid duplicating active work

### Candidate C — Native macOS Build Guidance

Value:

- addresses a platform gap not covered by the main guide or pull request #5
- directly reflects the initial user's experience

Concerns:

- native macOS compatibility has not been verified
- the complete workflow may require unsupported tools or modifications
- documentation must not be written before the workflow is successfully reproduced

### Candidate D — Read-Only microSD Validator

Possible behavior:

- accept a mounted microSD path
- check for required files and folders
- report missing, optional, and unexpected items
- perform no writes or formatting
- produce a plain-language result

Value:

- provides a programming and testing opportunity
- directly checks whether the card structure is complete
- avoids firmware and hardware-write risk

Concerns:

- repeated community demand has not been demonstrated
- maintainer interest is unknown
- the tool may be unnecessary if improved documentation is sufficient

## Candidates Not Recommended

### Broad Beginner Guide

Not recommended because open pull request #5 already proposes a detailed beginner guide.

### New Combined Pico Package

Not recommended because community guides already distribute simplified combined packages.

### Automatic Flasher

Not recommended because it introduces write operations, hardware risk, recovery requirements, and significantly greater testing burden.

## Current Conclusions

The initial user problems are supported by evidence, but the best first contribution has not yet been selected.

The strongest confirmed documentation issue is the omission of `patchlist.bin`.

The strongest unresolved platform issue is the absence of a verified native macOS workflow.

The read-only validator remains technically plausible but lacks strong evidence of repeated user demand.

The next phase of research should focus on maintainer expectations, contribution guidance, licensing, and whether native macOS support can be safely reproduced.

## Open Questions

- Is a native macOS build workflow officially supported?
- Would maintainers prefer a narrow documentation correction or a separate utility?
- Is issue #7 available for a first-time contributor?
- Does pull request #5 have a likely merge path?
- Are Pico Loader and Pico Launcher release versions expected to remain mutually compatible?
- Would a read-only validator provide enough value to justify ongoing maintenance?
