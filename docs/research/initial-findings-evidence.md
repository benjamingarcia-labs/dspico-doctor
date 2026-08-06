# Initial DSpico Research Evidence Register

## Purpose

This register records the evidence supporting the source-dependent claims in [Initial DSpico Research Findings](initial-findings.md).

The register is time-bounded. It records what was inspected, where the evidence came from, how it was classified, and what the evidence does not prove. It is not a claim that upstream repositories, releases, issues, pull requests, or documentation remain unchanged after the recorded inspection date.

## Evidence Classifications

- **Official upstream:** material published in an `LNH-team` repository, release, issue, or pull request.
- **Direct archive inspection:** locally observed contents of an official published release asset.
- **Direct user observation:** an experience reported during one beginner setup.
- **Inference:** an interpretation derived from identified evidence rather than a statement made by an authoritative source.
- **Unresolved:** a question or claim for which the inspected evidence is insufficient.

## Register

<a id="e-01"></a>
### E-01 — DSpico Project Index

- **Source:** `LNH-team/dspico` project index
- **Repository or authority:** Official upstream
- **Revision or item:** `develop` branch, `README.md` content blob `51d979ccac53cca75607da0b1984590a796c22ed`
- **Inspection date:** 2026-08-05
- **Evidence use:** Confirms that DSpico is organized across separate repositories for hardware, firmware, bootloader, DLDI, Wrfuxxed, Pico Launcher, Pico Loader, and USB examples.
- **Evidence classification:** Official upstream
- **Current-state limitation:** Confirms the index contents as inspected; repository organization may change later.
- **Source link:** https://github.com/LNH-team/dspico/blob/develop/README.md

<a id="e-02"></a>
### E-02 — Official Guide Storage Workflows

- **Source:** `LNH-team/dspico` setup guide
- **Repository or authority:** Official upstream
- **Revision or item:** `develop` branch, `GUIDE.md` content blob `2c5e04f809235647d66ac9a165e51c0ffc2047ae`
- **Inspection date:** 2026-08-05
- **Evidence use:** Confirms that firmware flashing copies `DSpico.uf2` to the RP2040 USB drive, while microSD preparation copies Pico Launcher, Pico Loader, lists, themes, and user content to the card.
- **Evidence classification:** Official upstream
- **Current-state limitation:** Source inspection confirms the documented distinction; it does not prove how clearly every beginner understands it.
- **Source link:** https://github.com/LNH-team/dspico/blob/develop/GUIDE.md

<a id="e-03"></a>
### E-03 — Official Guide Build Environment and Components

- **Source:** `LNH-team/dspico` setup guide
- **Repository or authority:** Official upstream
- **Revision or item:** `develop` branch, `GUIDE.md` content blob `2c5e04f809235647d66ac9a165e51c0ffc2047ae`
- **Inspection date:** 2026-08-05
- **Evidence use:** Confirms that the guide documents Linux or WSL prerequisites and source-build steps for DSpico DLDI, bootloader, firmware, Pico Loader, Pico Launcher, and optional Wrfuxxed components.
- **Evidence classification:** Official upstream
- **Current-state limitation:** The guide documents Linux or WSL. This evidence does not establish that native macOS is unsupported; it only shows that a complete native macOS workflow is not documented there.
- **Source link:** https://github.com/LNH-team/dspico/blob/develop/GUIDE.md

<a id="e-04"></a>
### E-04 — Pico Launcher Release Asset

- **Source:** `LNH-team/pico-launcher` release
- **Repository or authority:** Official upstream
- **Revision or item:** Release `v1.3.0`, release ID `310735670`, asset `Pico_Launcher.zip`
- **Inspection date:** 2026-08-05
- **Evidence use:** Confirms that an official prebuilt Pico Launcher archive is published.
- **Evidence classification:** Official upstream
- **Current-state limitation:** Release metadata confirms the asset exists; archive contents are recorded separately in E-06.
- **Source link:** https://github.com/LNH-team/pico-launcher/releases/tag/v1.3.0

<a id="e-05"></a>
### E-05 — Pico Loader DSpico Release Asset

- **Source:** `LNH-team/pico-loader` release
- **Repository or authority:** Official upstream
- **Revision or item:** Release `v1.7.1`, release ID `345870919`, asset `Pico_Loader_DSPICO.zip`
- **Inspection date:** 2026-08-05
- **Evidence use:** Confirms that an official prebuilt DSpico-specific Pico Loader archive is published.
- **Evidence classification:** Official upstream
- **Current-state limitation:** Release metadata confirms the asset exists; archive contents are recorded separately in E-07.
- **Source link:** https://github.com/LNH-team/pico-loader/releases/tag/v1.7.1

<a id="e-06"></a>
### E-06 — Pico Launcher Release Archive

- **Source:** Official `Pico_Launcher.zip` release asset from `v1.3.0`
- **Repository or authority:** Official upstream release asset
- **Revision or item:** SHA-256 `b4ed7f2cf2713e0c74985764536a0bfb7381566c67b0f85dc8b83375bcfc495d`
- **Inspection date:** 2026-08-05
- **Inspection method:** Downloaded the published asset, listed its archive index with `unzip -l`, and compared the local SHA-256 with the digest in GitHub release metadata.
- **Evidence use:** Confirms that the archive contains `LAUNCHER.nds`, `_pico/`, and the included theme structure. It does not contain Pico Loader binaries or list files.
- **Evidence classification:** Direct archive inspection of official upstream asset
- **Current-state limitation:** Confirms archive contents only; it does not prove successful setup, runtime compatibility, or hardware behavior.
- **Source link:** https://github.com/LNH-team/pico-launcher/releases/tag/v1.3.0

<a id="e-07"></a>
### E-07 — Pico Loader DSpico Release Archive

- **Source:** Official `Pico_Loader_DSPICO.zip` release asset from `v1.7.1`
- **Repository or authority:** Official upstream release asset
- **Revision or item:** SHA-256 `a3eaa2ce0e9a461a2baeda719cce1385e8218521b66f1cf892225c0bedff01b2`
- **Inspection date:** 2026-08-05
- **Inspection method:** Downloaded the published asset, listed its archive index with `unzip -l`, and compared the local SHA-256 with the digest in GitHub release metadata.
- **Evidence use:** Confirms that the archive contains exactly `aplist.bin`, `patchlist.bin`, `picoLoader7.bin`, `picoLoader9.bin`, and `savelist.bin`.
- **Evidence classification:** Direct archive inspection of official upstream asset
- **Current-state limitation:** Confirms archive contents only; it does not prove successful setup, runtime compatibility, or hardware behavior.
- **Source link:** https://github.com/LNH-team/pico-loader/releases/tag/v1.7.1

<a id="e-08"></a>
### E-08 — Documented Loader and Launcher Assembly

- **Source:** `LNH-team/dspico` setup guide together with the inspected release archives
- **Repository or authority:** Official upstream and direct archive inspection
- **Revision or item:** `GUIDE.md` content blob `2c5e04f809235647d66ac9a165e51c0ffc2047ae`; Pico Launcher `v1.3.0`; Pico Loader `v1.7.1`
- **Inspection date:** 2026-08-05
- **Evidence use:** Confirms the documented assembly pattern: place Loader files in the Launcher-provided `_pico` directory, rename `LAUNCHER.nds` to `_picoboot.nds`, and copy `_pico` and `_picoboot.nds` to the microSD root.
- **Evidence classification:** Official upstream plus direct archive inspection
- **Current-state limitation:** Confirms the documented file-placement process. The specific release pair was not independently run on hardware during this research.
- **Source link:** https://github.com/LNH-team/dspico/blob/develop/GUIDE.md

<a id="e-09"></a>
### E-09 — Official Guide Omission of `patchlist.bin`

- **Source:** `LNH-team/dspico` setup guide
- **Repository or authority:** Official upstream
- **Revision or item:** `develop` branch, `GUIDE.md` content blob `2c5e04f809235647d66ac9a165e51c0ffc2047ae`
- **Inspection date:** 2026-08-05
- **Evidence use:** Confirms that the microSD copy steps and final directory example list `aplist.bin`, `savelist.bin`, `picoLoader7.bin`, and `picoLoader9.bin` but omit `patchlist.bin`.
- **Evidence classification:** Official upstream
- **Current-state limitation:** Confirms the omission in the inspected guide revision only.
- **Source link:** https://github.com/LNH-team/dspico/blob/develop/GUIDE.md

<a id="e-10"></a>
### E-10 — Pico Loader README Requirement for `patchlist.bin`

- **Source:** `LNH-team/pico-loader` README
- **Repository or authority:** Official upstream
- **Revision or item:** `develop` branch, `README.md` content blob `6d004fe7c15cc3eae76d0395b9cb0e0cd8a065ef`
- **Inspection date:** 2026-08-05
- **Evidence use:** Confirms that the Pico Loader setup instructions list `patchlist.bin` among the files to copy into `/_pico`.
- **Evidence classification:** Official upstream
- **Current-state limitation:** Confirms the README requirement as inspected; it does not independently prove runtime consequences if the file is missing.
- **Source link:** https://github.com/LNH-team/pico-loader/blob/develop/README.md

<a id="e-11"></a>
### E-11 — Upstream `patchlist.bin` Guide Issue

- **Source:** `LNH-team/dspico` Issue #7
- **Repository or authority:** Official upstream issue tracker
- **Revision or item:** Issue #7, `Add patchlist.bin to guide`
- **Inspection date:** 2026-08-05
- **Evidence use:** Confirms that an upstream issue exists for adding `patchlist.bin` to the guide and that it remained open when inspected.
- **Evidence classification:** Official upstream repository state
- **Current-state limitation:** The issue body is empty, so the title is the available direct description. Issue state may change after inspection.
- **Source link:** https://github.com/LNH-team/dspico/issues/7

<a id="e-12"></a>
### E-12 — Extended Guide Pull Request

- **Source:** `LNH-team/dspico` Pull Request #5
- **Repository or authority:** Upstream contribution under review
- **Revision or item:** PR #5, `Create Extended Guide`; head commit `76d07701e50b2eca17bbd1007ad8ab999ed0b23c`
- **Inspection date:** 2026-08-05
- **Evidence use:** Confirms that a detailed guide contribution aimed at users unfamiliar with terminal or Linux environments exists and remained open and unmerged when inspected. The contributor reports successfully compiling firmware using the documented steps.
- **Evidence classification:** Official upstream pull-request state with contributor-reported validation
- **Current-state limitation:** The contributor's report is not independent validation or maintainer acceptance. PR contents and status may change after inspection.
- **Source link:** https://github.com/LNH-team/dspico/pull/5

<a id="e-13"></a>
### E-13 — Direct Beginner Setup Observation

- **Source:** Initial DSpico Doctor beginner setup experience
- **Repository or authority:** Direct user observation recorded by the project
- **Revision or item:** One setup attempt by a macOS user; no independent observation artifact was preserved
- **Inspection date:** Initial research period; documented before Issue #13
- **Evidence use:** Records confusion between the RP2040 BOOTSEL drive and microSD storage, incomplete file copying, and difficulty applying the available platform-oriented instructions.
- **Evidence classification:** Direct user observation
- **Current-state limitation:** Applies to one user and does not establish prevalence across the wider DSpico community.
- **Source link:** [Initial findings user-evidence section](initial-findings.md#confirmed-user-evidence)

<a id="e-14"></a>
### E-14 — Native macOS Workflow Status

- **Source:** Official guide review and current research boundary
- **Repository or authority:** Official upstream documentation plus project research limitation
- **Revision or item:** `GUIDE.md` content blob `2c5e04f809235647d66ac9a165e51c0ffc2047ae`
- **Inspection date:** 2026-08-05
- **Evidence use:** Supports the statement that the official guide documents Linux or WSL and that this research has not verified a complete native macOS workflow.
- **Evidence classification:** Official upstream documentation plus unresolved research question
- **Current-state limitation:** Does not establish that native macOS is unsupported or impossible.
- **Source link:** https://github.com/LNH-team/dspico/blob/develop/GUIDE.md

<a id="e-15"></a>
### E-15 — Beginner-Friction and Validator-Demand Boundary

- **Source:** E-02, E-03, E-12, and E-13
- **Repository or authority:** Derived project interpretation
- **Revision or item:** Evidence records current through their listed inspection dates
- **Inspection date:** 2026-08-05
- **Evidence use:** Supports a limited inference that the documented workflow can create beginner friction. It does not establish widespread demand for an automated microSD validator.
- **Evidence classification:** Inference and unresolved demand evidence
- **Current-state limitation:** No reproducible independent community-source set or demand study was preserved. The inference must not be presented as an official upstream conclusion.
- **Source link:** [Initial findings conclusions](initial-findings.md#current-conclusions)

## Community-Source Boundary

Community documentation was consulted during the initial investigation, but the exact sources, revisions, and inspection dates were not preserved. A later search did not recover a clearly independent source set that reproducibly supported all of the original community-evidence statements.

Those recollections are therefore not used as confirmed evidence in this register. Community claims should be added only after the exact source is recovered or newly inspected and classified.

## Maintenance and Reverification

Reverify an affected evidence record when:

- the related upstream repository, branch, release, issue, or pull request will be relied upon for a new consequential decision;
- an upstream source has materially changed;
- a finding is used to select, design, implement, test, document, or release a deliverable;
- a recorded open, closed, merged, supported, required, or current-state claim may have changed;
- a contributor identifies conflicting evidence; or
- the related research synthesis receives a material update.

When reverifying:

1. inspect the exact authoritative source;
2. preserve the prior inspection boundary when it remains historically relevant;
3. record the new inspection date and revision or item;
4. update only the affected evidence records and linked findings;
5. distinguish changed evidence from changed interpretation;
6. keep unresolved questions open unless evidence resolves them;
7. review `docs/research/README.md` and the root README for impact; and
8. link the change to the applicable Issue and pull request.

Do not update an inspection date merely to make the register appear current. A document does not require modification solely because it is old.

## Validation Boundary

This register improves reproducibility and source traceability. It does not prove that upstream state remains unchanged after inspection, that release files work together at runtime, that hardware behavior was reproduced, or that community-wide user demand exists.