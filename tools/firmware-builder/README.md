# DSpico Firmware Builder

This directory contains the DSpico Doctor Docker build recipe used by the macOS setup workflow to produce `DSpico.uf2` when no suitable official prebuilt firmware asset is available.

## Status

This repository version is an **implementation candidate pending revalidation**. It is derived from the controlled Apple Silicon reproduction recorded in Issue #14, but its private-input paths were intentionally reorganized for a safer public layout. Do not treat this version as validated until Issue #23 records a successful rebuild and artifact comparison.

## Supported validation target

Initial DSpico Doctor validation targets:

```text
Apple Silicon Mac
macOS 26.5.2
Build 25F84
Docker Desktop / ARM64 container execution
```

Other hosts are currently untested by DSpico Doctor.

## Local-only private inputs

Create a local directory named:

```text
private-inputs/
```

and place these user-supplied files inside it:

```text
private-inputs/
├── biosnds7.rom
├── biosdsi7.rom
└── wrfu.srl
```

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

DSpico Doctor does not distribute, mirror, embed, upload, or provide unofficial acquisition instructions for these inputs. The local `private-inputs/` directory is ignored by Git and must remain outside the public repository and distribution path.

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

## Build

From this directory, after Docker is running and the three private inputs have been placed and verified:

```bash
docker build \
  --progress=plain \
  --file Dockerfile \
  --tag dspico-doctor-builder .
```

A successful Docker build proves compilation only. It does not prove hardware behavior.

## Extract the firmware artifact

Create the ignored local output directory:

```bash
mkdir -p output
```

Create a temporary container, copy only the firmware artifact, then remove the container:

```bash
container_id="$(docker create dspico-doctor-builder)"

docker cp \
  "$container_id:/opt/dspico-firmware/build/DSpico.uf2" \
  ./output/DSpico.uf2

docker rm "$container_id"
```

Verify the artifact exists and record its identity:

```bash
ls -lh output/DSpico.uf2
shasum -a 256 output/DSpico.uf2
```

The earlier controlled reproduction produced a `DSpico.uf2` exactly `3,277,824` bytes in size with SHA-256:

```text
c29aceb95478300fdeb00423420e164697e326e4832a4e7e3ce52a1f0373b789
```

That hash is evidence from one recorded build, not a universal required hash. Comparison with the historical known-working artifact showed that only the embedded build-date bytes differed.

## Firmware destination

`DSpico.uf2` is firmware for the DSpico RP2040 internal flash. It is flashed through the RP2040 BOOTSEL USB device.

It does **not** belong on the removable DSpico microSD card.

## Technical references

- Issue #14 — selection evidence and controlled reproduction history
- Issue #23 — current implementation and validation tracking
- `docs/requirements/macos-dspico-setup-workflow.md` — approved requirements and acceptance criteria
- `docs/decisions/0001-select-first-dspico-doctor-deliverable.md` — deliverable selection and validation boundaries

The upstream DSpico repositories remain authoritative for DSpico source code and behavior. This builder is an independent DSpico Doctor support artifact.
