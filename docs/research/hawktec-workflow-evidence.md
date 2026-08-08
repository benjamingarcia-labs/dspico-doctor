# HawkTec DSpico Workflow Evidence

## Purpose

This record preserves a newly inspected vendor workflow and one direct user setup observation that are relevant to DSpico Doctor's research into beginner setup friction, packaged microSD workflows, and legal/content boundaries.

It is a time-bounded evidence record. It does not redefine official DSpico requirements or change the validated DSpico Doctor macOS setup guide.

## Evidence Sources

### HawkTec DSpico setup page

- **Source:** HawkTec DSpico setup page
- **Authority:** Third-party vendor documentation; not official `LNH-team` documentation
- **URL:** https://www.hawktec.shop/dspico/
- **Inspection date:** 2026-08-08
- **Evidence use:** Documents a simplified vendor setup model in which a user formats a microSD card, downloads a prepared HawkTec filesystem bundle, extracts it to the card root, and boots the DSpico.
- **Limitation:** The vendor page documents HawkTec's supported workflow for its product. It does not establish official DSpico requirements, compatibility for non-HawkTec units, or redistribution rights for every bundled file.

### Direct user setup observation

- **Source:** Project-owner report during DSpico Doctor work
- **Authority:** Direct user observation
- **Observation date:** Reported 2026-08-08
- **Evidence use:** The project owner reported that the HawkTec workflow was the process used to prepare the microSD card that successfully reached a working DSpico/Pico Launcher setup.
- **Limitation:** This is one successful setup observation. It does not prove that every file in the HawkTec bundle is required, that the same bundle works on all hardware, or that the result can be generalized to other DSpico configurations.

### Local HawkTec bundle listing

- **Source:** Terminal output from locally extracting and re-zipping `HWKTC_DSPICO_V1.0.0`
- **Authority:** Direct local file-list observation supplied by the project owner
- **Inspection date:** 2026-08-08
- **Evidence use:** The supplied ZIP listing shows a prepared filesystem that includes `_picoboot.nds`, Pico Loader files under `/_pico/`, settings and themes, emulator-launch helper `.nds` files, BIOS files, and DSi-style `shared1/`, `shared2/`, and `sys/` content.
- **Limitation:** DSpico Doctor did not independently ingest or redistribute this package. The observation is based on the complete terminal listing supplied during the project conversation rather than a repository-controlled archive inspection.

## Confirmed Observations

The supplied HawkTec bundle listing included at least:

```text
_picoboot.nds
_pico/picoLoader7.bin
_pico/picoLoader9.bin
_pico/aplist.bin
_pico/savelist.bin
_pico/settings.json
_pico/biosnds7.rom
_pico/biosdsi7.rom
shared1/
shared2/
sys/
GBA ROMS/
GBC ROMS/
NES ROMS/
NDS ROMS/
```

The listing also showed multiple HawkTec themes and convenience launcher files intended to reduce setup friction.

`patchlist.bin` was not present in the supplied ZIP listing. This is notable because the current Pico Loader documentation inspected by DSpico Doctor lists `patchlist.bin` among the files to install under `/_pico/`.

The successful HawkTec setup therefore provides direct evidence that one observed configuration reached a usable state without `patchlist.bin` appearing in the supplied package listing. It does **not** establish that `patchlist.bin` is unnecessary in general, that all loader behaviors work without it, or that current upstream guidance is incorrect.

## Legal and Redistribution Boundary

The HawkTec package must not be copied into the DSpico Doctor repository or redistributed by DSpico Doctor based on this inspection.

The supplied listing contains files that appear to fall outside a simple public Pico Launcher/Pico Loader distribution, including BIOS ROM files and DSi-style `shared1`, `shared2`, and `sys` content. DSpico Doctor has not established redistribution rights, provenance, or licensing for those files.

This record therefore treats the HawkTec package as **external evidence only**.

## Research Implications

The HawkTec workflow demonstrates a materially simpler user experience than manually assembling each public component:

```text
format card
→ download prepared filesystem
→ copy/extract to card root
→ boot
```

This supports a limited inference that packaging and automation can reduce beginner setup friction.

It does not justify copying the HawkTec bundle or introducing a new DSpico Doctor deliverable automatically. A future DSpico Doctor decision could separately evaluate whether a legal, reproducible setup tool could download verified public components, organize the card structure, accept user-supplied private inputs where lawful and necessary, and validate the result without redistributing restricted content.

Any such work would require its own user-need review, scope decision, requirements, licensing review, safety analysis, and validation plan.

## Relationship to Existing Research

This record is newly inspected evidence and does not retroactively convert the earlier broad community-source recollections in `initial-findings.md` or `initial-findings-evidence.md` into confirmed evidence.

The original research boundary remains valid: previously unpreserved community sources stay unverified unless individually recovered or newly inspected and classified.

## Validation Boundary

This record establishes what the vendor page documented, what the project owner reported using successfully, and what appeared in the supplied local ZIP listing.

It does not prove:

- official upstream support for the HawkTec workflow;
- compatibility across DSpico hardware variants;
- necessity or legality of every bundled file;
- redistribution rights for the package;
- complete game, DSiWare, emuNAND, or emulator compatibility;
- that `patchlist.bin` is generally optional; or
- that a future DSpico Doctor packaged setup tool is required or approved.
