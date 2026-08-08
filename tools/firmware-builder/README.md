# DSpico Firmware Builder

This directory contains the DSpico Doctor Docker build recipe used by the macOS setup workflow to produce `DSpico.uf2` when no suitable official prebuilt firmware asset is available.

This README documents the builder's implementation contract, pinned dependencies, restricted-input boundary, and validation evidence. It is not the end-user setup procedure. The complete beginner-oriented macOS procedure is being implemented under [Issue #23](https://github.com/benjamingarcia-labs/dspico-doctor/issues/23) and will be linked here once the guide exists.

## Status

This repository version was freshly revalidated on the recorded Apple Silicon macOS environment for [Issue #23](https://github.com/benjamingarcia-labs/dspico-doctor/issues/23). Validation used the reorganized local `private-inputs/` layout, disabled Docker build cache, completed the full pinned build, extracted only `DSpico.uf2`, and confirmed that local private inputs and output artifacts remained ignored by Git.

The fresh artifact was exactly `3,277,824` bytes. Its SHA-256 was `bab09ec584e7ba207885478ddb064ca23ae1b33965c9bfc2c643f8fe342c85d8`. A byte comparison against the earlier controlled reproduction found exactly one differing byte, corresponding to the embedded build date changing from `Aug  7 2026` to `Aug  8 2026`.

This is build-level validation only. It does not prove BOOTSEL flashing, runtime behavior, hardware compatibility, or the complete macOS setup workflow.

## Supported validation target

Initial DSpico Doctor validation target:

```text
Apple Silicon Mac
macOS 26.5.2
Build 25F84
Docker Desktop / ARM64 container execution
```

Other hosts are currently untested by DSpico Doctor.

## Builder interface

The builder expects three local, user-supplied inputs under `private-inputs/`:

```text
private-inputs/
├── biosnds7.rom
├── biosdsi7.rom
└── wrfu.srl
```

The build produces the firmware artifact that the setup workflow extracts as:

```text
output/DSpico.uf2
```

`DSpico.uf2` is firmware for the DSpico RP2040 internal flash. It is flashed through the RP2040 BOOTSEL USB device. It does **not** belong on the removable DSpico microSD card.

For the complete user procedure—prerequisite checks, private-input placement and verification, Docker execution, artifact extraction, BOOTSEL flashing, microSD preparation, and hardware validation—follow [Issue #23](https://github.com/benjamingarcia-labs/dspico-doctor/issues/23) until the dedicated macOS setup guide is published.

## Restricted-input boundary

The exact inputs used during the controlled reproduction had these SHA-256 values:

```text
biosnds7.rom
ba65f690eb04ec92db67c0e299e21ad71de087d6d5de8a9cb17a62eaab563c17

biosdsi7.rom
2946281e730e71f7cafdb125f5cb60fed944ca5d610ee1e082c441b602b5f4e2

wrfu.srl
0d98a480a075106aa1ad62b84fafb14d9a283fd186a67a20e786b17cb3ca5958
```

These hashes are file-identity evidence for the validated configuration. They do not establish ownership or permission to possess or use the files.

DSpico Doctor does not distribute, mirror, embed, upload, or provide unofficial acquisition instructions for these inputs. The local `private-inputs/` directory is ignored by Git and must remain outside the public repository and distribution path. Filename-level ignore rules also protect these restricted inputs and generated `DSpico.uf2` against accidental tracking elsewhere under this builder directory.

## Pinned build inputs

The Dockerfile pins the build environment and upstream source revisions used by the controlled reproduction:

```text
BlocksDS ARM64 image
skylyrac/blocksds:slim-v1.22.1
sha256:55ca69d4f5685fb11af53bd4118485f118eee8d4cda6715dcd3c32006362ede4

Gericom/DSRomEncryptor
f44682e79a5657dec62682ef80649506f561f00e

LNH-team/dspico-bootloader
29671d041fe2e497f8c39bae562e98d955afdbc5

LNH-team/dspico-dldi
8ba45f65690bc40d9279e663e1d89ca806451cc1

LNH-team/dspico-firmware
472c9d8e9957ad18df367f14b9cc337b9b887e65

LNH-team/dspico-wrfuxxed
25e7d47f1854f367a902af6a15ed17edb262339f
```

The controlled reproduction also resolved:

```text
dspico-bootloader/libtwl
55ffbc9e45b27d44cfee4404caac5fda19d4b8cc

dspico-firmware/pico-sdk
6a7db34ff63345a7badec79ebea3aaef1712f374
```

These revisions are maintenance and reproducibility evidence. The upstream repositories remain authoritative for their own source code and behavior.

## Validation evidence

The earlier controlled reproduction produced a `DSpico.uf2` exactly `3,277,824` bytes in size with SHA-256:

```text
c29aceb95478300fdeb00423420e164697e326e4832a4e7e3ce52a1f0373b789
```

The fresh no-cache repository validation produced the same file size with SHA-256:

```text
bab09ec584e7ba207885478ddb064ca23ae1b33965c9bfc2c643f8fe342c85d8
```

A byte comparison found exactly one differing byte, explained by the embedded build date changing from `Aug  7 2026` to `Aug  8 2026`. These hashes are validation evidence, not universal required hashes for future builds.

A successful build proves compilation only. It does not prove BOOTSEL flashing, runtime behavior, hardware compatibility, microSD preparation, or broad compatibility.

## Technical references

- [Issue #14 — deliverable selection evidence and controlled reproduction history](https://github.com/benjamingarcia-labs/dspico-doctor/issues/14)
- [Issue #23 — macOS setup workflow implementation and validation](https://github.com/benjamingarcia-labs/dspico-doctor/issues/23)
- [macOS DSpico Setup Workflow Requirements](../../docs/requirements/macos-dspico-setup-workflow.md)
- [Decision 0001: Select First DSpico Doctor Deliverable](../../docs/decisions/0001-select-first-dspico-doctor-deliverable.md)
- [Upstream DSpico firmware repository](https://github.com/LNH-team/dspico-firmware)
- [Upstream DSpico bootloader repository](https://github.com/LNH-team/dspico-bootloader)
- [Upstream DSpico DLDI repository](https://github.com/LNH-team/dspico-dldi)
- [Upstream DSpico Wrfuxxed repository](https://github.com/LNH-team/dspico-wrfuxxed)
- [Upstream DSRomEncryptor repository](https://github.com/Gericom/DSRomEncryptor)

The upstream DSpico repositories remain authoritative for DSpico source code and behavior. This builder is an independent DSpico Doctor support artifact.
