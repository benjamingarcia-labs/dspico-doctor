# macOS DSpico Setup Guide

## Introduction

This guide walks through setting up DSpico from an Apple Silicon Mac, from preparing the firmware to building the microSD card and confirming that Pico Launcher starts successfully.

It covers building and flashing `DSpico.uf2`, preparing a FAT32/MBR microSD card, downloading and installing Pico Launcher and Pico Loader, cleaning unnecessary macOS metadata, verifying the installation, and performing the initial hardware test.

Some firmware-build inputs must be supplied locally by the user and are not distributed by DSpico Doctor. Where required, this guide identifies those files and provides verification information without providing unofficial download sources.

The initial DSpico Doctor validation was performed on an Apple Silicon Mac running macOS 26.5.2, build 25F84. Other host configurations are currently untested unless noted otherwise.

## Before you begin

You will need:

- an assembled DSpico;
- an Apple Silicon Mac;
- a USB data cable for the DSpico;
- a microSD card and card reader;
- Docker Desktop;
- the user-supplied private firmware inputs described below.

DSpico uses two separate storage targets:

```text
DSpico RP2040 internal flash
→ receives DSpico.uf2 through USB BOOTSEL mode

removable microSD card
→ receives Pico Launcher, Pico Loader, and related files
```

`DSpico.uf2` does **not** belong on the microSD card.

If the DSpico already has compatible firmware installed and is working normally, skip the firmware-build and BOOTSEL sections and continue with [Prepare the microSD card](#prepare-the-microsd-card).

Technical references: [official DSpico setup guide](https://github.com/LNH-team/dspico/blob/develop/GUIDE.md), [DSpico Doctor requirements](../requirements/macos-dspico-setup-workflow.md), and [Issue #23](https://github.com/benjamingarcia-labs/dspico-doctor/issues/23).

---

## Prepare the Mac

### 1. Open Terminal

Open **Applications → Utilities → Terminal**.

### 2. Confirm Apple Silicon and macOS version

Run:

```bash
uname -m
sw_vers
```

The validated architecture is:

```text
arm64
```

The initial validated host is macOS 26.5.2, build 25F84. If `uname -m` does not return `arm64`, stop; Intel Macs are currently untested by DSpico Doctor.

### 3. Confirm the required macOS tools

Run:

```bash
git --version
curl --version
shasum --help >/dev/null && echo "shasum available"
diskutil list >/dev/null && echo "diskutil available"
```

Continue when the commands complete successfully.

### 4. Install and start Docker Desktop

Check whether Docker is installed:

```bash
docker --version
```

If Docker is not installed, follow the [official Docker Desktop for Mac installation instructions](https://docs.docker.com/desktop/setup/install/mac-install/).

Start Docker Desktop, then verify that the Docker engine is running:

```bash
docker info >/dev/null && echo "Docker is running"
```

Do not continue until the command prints:

```text
Docker is running
```

---

## Build DSpico firmware

The current DSpico firmware release does not provide a downloadable `DSpico.uf2` asset, so a from-scratch setup uses the repository-managed Docker builder. See the [DSpico firmware releases](https://github.com/LNH-team/dspico-firmware/releases) and the [DSpico Doctor firmware-builder reference](../../tools/firmware-builder/README.md).

### 5. Create a setup workspace

Run:

```bash
mkdir -p "$HOME/Documents/DSpico-Setup"
cd "$HOME/Documents/DSpico-Setup"
```

### 6. Download DSpico Doctor

Clone the public repository to the Mac:

```bash
git clone https://github.com/benjamingarcia-labs/dspico-doctor.git
cd dspico-doctor/tools/firmware-builder
```

This downloads a local copy of the public project. The setup procedure does not upload files to GitHub.

### 7. Add the private firmware inputs

Create the local input directory:

```bash
mkdir -p private-inputs
```

For the DSpico Doctor validated Wrfuxxed-enabled build profile, place these user-supplied files inside `private-inputs/`:

```text
biosnds7.rom
biosdsi7.rom
wrfu.srl
```

These files remain local to the Mac and are used as inputs to the Docker build. DSpico Doctor does not distribute them or provide unofficial download sources.

Verify the files:

```bash
shasum -a 256 \
  private-inputs/biosnds7.rom \
  private-inputs/biosdsi7.rom \
  private-inputs/wrfu.srl
```

The exact inputs used during DSpico Doctor validation had these SHA-256 values:

```text
biosnds7.rom
ba65f690eb04ec92db67c0e299e21ad71de087d6d5de8a9cb17a62eaab563c17

biosdsi7.rom
2946281e730e71f7cafdb125f5cb60fed944ca5d610ee1e082c441b602b5f4e2

wrfu.srl
0d98a480a075106aa1ad62b84fafb14d9a283fd186a67a20e786b17cb3ca5958
```

If a hash differs, stop. A different hash does not prove that the file is invalid; it means the file is not identical to the input DSpico Doctor validated.

### 8. Build the validated firmware profile

Confirm Docker is running:

```bash
docker info >/dev/null && echo "Docker is running"
```

Build the firmware:

```bash
docker build \
  --progress=plain \
  --file Dockerfile \
  --tag dspico-doctor-builder .
```

A successful build proves compilation only. It does not prove hardware behavior.

### 9. Extract `DSpico.uf2`

Run:

```bash
mkdir -p output
container_id="$(docker create dspico-doctor-builder)"
docker cp \
  "$container_id:/opt/dspico-firmware/build/DSpico.uf2" \
  ./output/DSpico.uf2
docker rm "$container_id"
```

Verify the resulting file:

```bash
stat -f '%z bytes' output/DSpico.uf2
shasum -a 256 output/DSpico.uf2
```

DSpico Doctor validation builds produced a file size of:

```text
3277824 bytes
```

Record the SHA-256 value. The firmware embeds its compilation date, so otherwise equivalent builds can have different hashes.

### Optional: build without Wrfuxxed

Upstream DSpico treats Wrfuxxed as optional. A build without Wrfuxxed does not require `wrfu.srl` and omits the Wrfuxxed build, DLDI patch, `roms/dsimode.nds` copy, payload copy, and `DSPICO_ENABLE_WRFUXXED` enablement steps.

The current DSpico Doctor Dockerfile represents the Wrfuxxed-enabled profile that was independently validated. A no-Wrfuxxed macOS Docker build has **not** been independently validated by DSpico Doctor. Users who need that configuration should follow the corresponding non-Wrfuxxed steps in the [official DSpico build guide](https://github.com/LNH-team/dspico/blob/develop/GUIDE.md) and treat the result as untested by DSpico Doctor.

---

## Flash `DSpico.uf2` through BOOTSEL

### 10. Enter BOOTSEL mode

Disconnect the DSpico from any Nintendo system and remove the microSD card.

Upstream DSpico documents these BOOTSEL entry methods:

- an RP2040 that has never been flashed may enter BOOTSEL automatically when connected by USB;
- DSpico firmware may enter BOOTSEL automatically when connected by USB with the microSD removed;
- otherwise, hold the DSpico **BOOTSEL** button while connecting the USB cable.

See the [official DSpico flashing instructions](https://github.com/LNH-team/dspico/blob/develop/GUIDE.md#6-flashing-the-dspico).

### 11. Identify the BOOTSEL USB volume

After connecting the DSpico in BOOTSEL mode, run:

```bash
ls /Volumes
```

An RP2040 BOOTSEL device normally mounts as `RPI-RP2`. Confirm that the volume appeared only after connecting the DSpico.

If it appears as `RPI-RP2`, inspect it with:

```bash
diskutil info /Volumes/RPI-RP2
```

Do not continue if the BOOTSEL volume cannot be identified confidently.

For background on RP2040 BOOTSEL behavior, see the [Raspberry Pi microcontroller documentation](https://www.raspberrypi.com/documentation/microcontrollers/).

### 12. Flash the firmware

Return to the firmware-builder directory if necessary:

```bash
cd "$HOME/Documents/DSpico-Setup/dspico-doctor/tools/firmware-builder"
```

Confirm that the firmware exists:

```bash
ls -lh output/DSpico.uf2
```

Copy it to the BOOTSEL volume:

```bash
cp output/DSpico.uf2 /Volumes/RPI-RP2/
```

Do not disconnect the USB cable while the file is being copied.

After the UF2 is accepted, the RP2040 normally exits BOOTSEL and the `RPI-RP2` volume disappears. Disconnect the DSpico from USB after the copy completes.

### 13. BOOTSEL recovery

If the DSpico does not behave as expected after flashing:

1. disconnect the DSpico;
2. remove the microSD card;
3. hold BOOTSEL while reconnecting USB;
4. confirm that the BOOTSEL volume appears;
5. reflash a verified `DSpico.uf2`.

Stop if the BOOTSEL device cannot be identified or restored using the documented method.

---

## Prepare the microSD card

> **Warning:** Formatting erases the selected disk. Back up any files that must be kept and identify the microSD card positively before running the erase command.

### 14. Insert and identify the microSD card

Insert the microSD card and run:

```bash
diskutil list
```

Identify the card by its external/removable status and approximate capacity. If necessary, compare `diskutil list` before and after inserting the card.

Inspect the candidate disk:

```bash
diskutil info /dev/diskN
```

Replace `/dev/diskN` with the identifier actually assigned to the microSD card. Never reuse an old disk number without checking it again.

Stop if the card cannot be identified confidently.

### 15. Format the card as FAT32 with MBR

After confirming the correct target, run:

```bash
diskutil eraseDisk FAT32 DSPICO MBRFormat /dev/diskN
```

Replace `/dev/diskN` with the confirmed microSD identifier.

After the command succeeds, verify the result:

```bash
diskutil list /dev/diskN
diskutil info /Volumes/DSPICO
```

Confirm that the card uses FAT32 and an MBR/FDisk partition map.

DSpico Doctor has validated a 31.9 GB FAT32/MBR card. Other capacities are currently untested rather than automatically unsupported. The guide does not require forcing a 32 KiB allocation size for the normal ≤32 GB path; see the [requirements record](../requirements/macos-dspico-setup-workflow.md#10-storage-preparation-requirements) for the validation background.

---

## Download Pico Launcher and Pico Loader

The current versions selected for this guide are:

```text
Pico Launcher v1.3.0
Pico Loader v1.7.1 — DSpico package
```

Use the official release artifacts rather than compiling these components unnecessarily.

### 16. Create a downloads directory

Run:

```bash
mkdir -p "$HOME/Documents/DSpico-Setup/downloads"
cd "$HOME/Documents/DSpico-Setup/downloads"
```

### 17. Download Pico Launcher

Run:

```bash
curl -L \
  -o Pico_Launcher.zip \
  "https://github.com/LNH-team/pico-launcher/releases/download/v1.3.0/Pico_Launcher.zip"
```

Verify the archive:

```bash
shasum -a 256 Pico_Launcher.zip
```

Expected SHA-256:

```text
b4ed7f2cf2713e0c74985764536a0bfb7381566c67b0f85dc8b83375bcfc495d
```

Extract it:

```bash
mkdir -p Pico_Launcher
unzip Pico_Launcher.zip -d Pico_Launcher
```

The extracted release should contain `LAUNCHER.nds` and the complete `_pico` directory. See the [official Pico Launcher release](https://github.com/LNH-team/pico-launcher/releases/tag/v1.3.0).

### 18. Download Pico Loader for DSpico

Run:

```bash
curl -L \
  -o Pico_Loader_DSPICO.zip \
  "https://github.com/LNH-team/pico-loader/releases/download/v1.7.1/Pico_Loader_DSPICO.zip"
```

Verify the archive:

```bash
shasum -a 256 Pico_Loader_DSPICO.zip
```

Expected SHA-256:

```text
a3eaa2ce0e9a461a2baeda719cce1385e8218521b66f1cf892225c0bedff01b2
```

Extract it:

```bash
mkdir -p Pico_Loader_DSPICO
unzip Pico_Loader_DSPICO.zip -d Pico_Loader_DSPICO
```

The DSpico installation uses:

```text
picoLoader7.bin
picoLoader9_DSPICO.bin
aplist.bin
savelist.bin
patchlist.bin
```

`patchlist.bin` is required by Pico Loader even though the current DSpico guide's example card layout omits it. See [DSpico Issue #7](https://github.com/LNH-team/dspico/issues/7) and the [Pico Loader repository](https://github.com/LNH-team/pico-loader).

---

## Install the public files on the microSD card

### 19. Confirm the card is mounted

Run:

```bash
ls /Volumes/DSPICO
```

Stop if `/Volumes/DSPICO` is not mounted.

### 20. Install Pico Launcher

Copy the complete Launcher `_pico` directory:

```bash
cp -R \
  "$HOME/Documents/DSpico-Setup/downloads/Pico_Launcher/_pico" \
  /Volumes/DSPICO/
```

Copy and rename the Launcher boot file:

```bash
cp \
  "$HOME/Documents/DSpico-Setup/downloads/Pico_Launcher/LAUNCHER.nds" \
  /Volumes/DSPICO/_picoboot.nds
```

### 21. Install Pico Loader

Run:

```bash
cp "$HOME/Documents/DSpico-Setup/downloads/Pico_Loader_DSPICO/picoLoader7.bin" \
  /Volumes/DSPICO/_pico/picoLoader7.bin

cp "$HOME/Documents/DSpico-Setup/downloads/Pico_Loader_DSPICO/picoLoader9_DSPICO.bin" \
  /Volumes/DSPICO/_pico/picoLoader9.bin

cp "$HOME/Documents/DSpico-Setup/downloads/Pico_Loader_DSPICO/aplist.bin" \
  /Volumes/DSPICO/_pico/aplist.bin

cp "$HOME/Documents/DSpico-Setup/downloads/Pico_Loader_DSPICO/savelist.bin" \
  /Volumes/DSPICO/_pico/savelist.bin

cp "$HOME/Documents/DSpico-Setup/downloads/Pico_Loader_DSPICO/patchlist.bin" \
  /Volumes/DSPICO/_pico/patchlist.bin
```

### 22. Verify the required card files

Run:

```bash
for file in \
  /Volumes/DSPICO/_picoboot.nds \
  /Volumes/DSPICO/_pico/picoLoader7.bin \
  /Volumes/DSPICO/_pico/picoLoader9.bin \
  /Volumes/DSPICO/_pico/aplist.bin \
  /Volumes/DSPICO/_pico/savelist.bin \
  /Volumes/DSPICO/_pico/patchlist.bin; do
  test -f "$file" || echo "MISSING: $file"
done
```

No `MISSING:` lines should appear.

For the selected releases, DSpico Doctor validation recorded these installed-file SHA-256 values:

```text
_picoboot.nds
17599f4fcdd9cd10e516b804dccbbe88e36d63beb24e5038247c1f820092d5c1

picoLoader7.bin
18def39ea14824be8c1c5a8749ea759d7e53cc2e0f3a33627ef9d6d4e618b13f

picoLoader9.bin
de101abad93d79257bdebbba1213277660871ceaabd6f04571ad11e78578d7aa

aplist.bin
13e27e0bca8a8a45af8b22aa09cdd73cbac6f8cdeb33678f0842607f077a36d2

savelist.bin
438685d7a6f783809f43ec1efc39355cec9a1c7b176c804ef7b2951300f8eea8

patchlist.bin
9936b0b324894df9e324a70e05683daeb2b3b2b83c2b22514313fa50aa4a5023
```

Verify them with:

```bash
shasum -a 256 \
  /Volumes/DSPICO/_picoboot.nds \
  /Volumes/DSPICO/_pico/picoLoader7.bin \
  /Volumes/DSPICO/_pico/picoLoader9.bin \
  /Volumes/DSPICO/_pico/aplist.bin \
  /Volumes/DSPICO/_pico/savelist.bin \
  /Volumes/DSPICO/_pico/patchlist.bin
```

If a hash differs, re-copy that file from the verified release archive before continuing.

---

## Clean macOS metadata

### 23. Remove AppleDouble files

macOS may create `._*` AppleDouble files on FAT32 media. Clean them with:

```bash
dot_clean -m /Volumes/DSPICO
```

Verify that none remain:

```bash
find /Volumes/DSPICO -name '._*' -print
```

No output is expected. A warning involving `.Spotlight-V100` does not by itself mean the cleanup failed. macOS-managed directories such as `.fseventsd` or `.Spotlight-V100` are not automatically DSpico setup errors.

---

## Eject and test the DSpico

### 24. Safely eject the microSD card

Run:

```bash
diskutil eject /Volumes/DSPICO
```

Remove the card only after macOS reports that it was ejected successfully.

### 25. Perform the minimum hardware test

1. Insert the prepared microSD card into the DSpico.
2. Insert the DSpico into the Nintendo system.
3. Power on the system.
4. Launch DSpico if the system does not start it automatically.

The minimum successful result is:

```text
DSpico recognized
→ microSD readable
→ Pico Launcher starts
→ usable file-browser interface reached
```

If Pico Launcher starts and the file browser is usable, the base DSpico setup is complete.

If it does not start, first confirm that `_picoboot.nds`, the `_pico` directory, and all five required Pico Loader files are present. For deeper troubleshooting, see the [official DSpico guide](https://github.com/LNH-team/dspico/blob/develop/GUIDE.md), [Pico Launcher](https://github.com/LNH-team/pico-launcher), and [Pico Loader](https://github.com/LNH-team/pico-loader).

---

## Optional features

Pico Launcher supports additional features such as display modes, themes, covers, custom icons and banners, background music, file associations, and cheats. The complete Launcher release structure installed by this guide preserves the files needed for normal supported use.

Some Pico Loader features, including DSiWare and emuNAND-related functionality, require additional BIOS or NAND-derived files from the user's own hardware. Those files are outside the public base installation and are not distributed by DSpico Doctor. See the [Pico Loader documentation](https://github.com/LNH-team/pico-loader) before configuring those features.

---

## Completion checklist

The base installation is complete when:

- [ ] compatible DSpico firmware is installed;
- [ ] the microSD card is FAT32 with an MBR/FDisk partition map;
- [ ] the complete Pico Launcher release structure is installed;
- [ ] `_picoboot.nds` exists at the card root;
- [ ] `picoLoader7.bin`, `picoLoader9.bin`, `aplist.bin`, `savelist.bin`, and `patchlist.bin` exist under `/_pico/`;
- [ ] unwanted AppleDouble files have been removed;
- [ ] the microSD card was safely ejected;
- [ ] DSpico is recognized by the Nintendo system;
- [ ] Pico Launcher reaches a usable file-browser interface.

## References

- [Official DSpico setup guide](https://github.com/LNH-team/dspico/blob/develop/GUIDE.md)
- [DSpico firmware releases](https://github.com/LNH-team/dspico-firmware/releases)
- [DSpico hardware](https://github.com/LNH-team/dspico-hardware)
- [Pico Launcher](https://github.com/LNH-team/pico-launcher)
- [Pico Launcher releases](https://github.com/LNH-team/pico-launcher/releases)
- [Pico Loader](https://github.com/LNH-team/pico-loader)
- [Pico Loader releases](https://github.com/LNH-team/pico-loader/releases)
- [Raspberry Pi microcontroller documentation](https://www.raspberrypi.com/documentation/microcontrollers/)
- [DSpico Doctor firmware builder](../../tools/firmware-builder/README.md)
- [DSpico Doctor macOS setup requirements](../requirements/macos-dspico-setup-workflow.md)
- [DSpico Doctor Issue #23](https://github.com/benjamingarcia-labs/dspico-doctor/issues/23)
