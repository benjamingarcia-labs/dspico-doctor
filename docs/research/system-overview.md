# DSpico System Overview

## Purpose

This document provides a beginner-oriented overview of how the main DSpico software components work together.

It is based on direct inspection of the current upstream repositories and source code. It focuses on the normal boot and microSD-read paths.

Some lower-level details remain under investigation and are listed separately.

## High-Level Architecture

DSpico combines software running on two different systems:

- the RP2040 microcontroller inside the DSpico cartridge
- the Nintendo DS, DSi, or 3DS console using the cartridge

The RP2040 runs the DSpico firmware.

The Nintendo DS runs the embedded bootloader, Pico Loader support code, Pico Launcher, and selected Nintendo DS software.

These parts cooperate through the Nintendo DS cartridge bus.

## Main Components

### DSpico Firmware

Repository:

`LNH-team/dspico-firmware`

Runs on:

RP2040 microcontroller

Primary responsibilities:

- emulate a Nintendo DS cartridge
- expose an embedded Nintendo DS ROM to the console
- communicate with the Nintendo DS cartridge bus
- access the microSD card through the RP2040 SD subsystem
- provide custom cartridge commands for microSD and USB access
- enter BOOTSEL mode when the microSD card cannot be mounted

Build output:

`DSpico.uf2`

The firmware is flashed to the RP2040 through the `RPI-RP2` BOOTSEL drive.

### DSpico Bootloader

Repository:

`LNH-team/dspico-bootloader`

Runs on:

Nintendo DS ARM9, with ARM7 coordination

Primary responsibilities:

- initialize the Nintendo DS-side environment
- access the DSpico microSD card through DLDI
- request that Pico Loader start `fat:/_picoboot.nds`

Build output:

`BOOTLOADER.nds`

After patching and preparation, it is renamed to:

`default.nds`

That file becomes a build input to the DSpico firmware.

### DSpico DLDI

Repository:

`LNH-team/dspico-dldi`

Runs on:

Nintendo DS software side

Primary responsibilities:

- provide standard DLDI storage functions
- translate sector reads and writes into DSpico-specific cartridge commands
- move data between Nintendo DS memory and the RP2040 firmware

Important functions include:

- `dldi_readSectors`
- `dldi_writeSectors`

### Pico Loader

Repository:

`LNH-team/pico-loader`

Runs on:

Nintendo DS

Primary responsibility:

- load Nintendo DS software requested by the bootloader or launcher

The DSpico bootloader uses Pico Loader functions to start:

`fat:/_picoboot.nds`

### Pico Launcher

Repository:

`LNH-team/pico-launcher`

Runs on:

Nintendo DS

Primary responsibilities:

- provide the user interface
- browse content on the microSD card
- start selected Nintendo DS software

Release filename:

`LAUNCHER.nds`

Required DSpico microSD filename:

`_picoboot.nds`

The file must be renamed because the bootloader searches specifically for:

`fat:/_picoboot.nds`

## Build-Time Flow

The DSpico bootloader is first compiled as:

`BOOTLOADER.nds`

It is then patched and prepared as:

`default.nds`

The firmware build includes `src/romData.S`, which uses the assembler directive:

`.incbin "../roms/default.nds"`

This embeds the complete contents of `default.nds` into the RP2040 firmware image.

The embedded ROM is exposed through symbols including:

- `gDefaultRom`
- `gDefaultRomSize`

The firmware build then produces:

`DSpico.uf2`

This means `default.nds` is not loaded from the microSD card during normal cartridge startup. Its bytes are already embedded in the flashed RP2040 firmware.

## Runtime Boot Flow

The normal boot sequence is:

```text
Nintendo DS powers on
        ↓
RP2040 runs DSpico firmware
        ↓
firmware emulates a Nintendo DS cartridge
        ↓
firmware presents embedded default.nds
        ↓
Nintendo DS runs the DSpico bootloader
        ↓
bootloader accesses microSD storage through DLDI
        ↓
bootloader asks Pico Loader to start fat:/_picoboot.nds
        ↓
Pico Launcher starts
        ↓
user selects Nintendo DS software
````

## Firmware ROM Emulation

During firmware startup, the embedded ROM data is assigned to the cartridge-emulation state:

```text
gNtrRomEmu.romData = gDefaultRom
gNtrRomEmu.romSize = gDefaultRomSize
```

The firmware configures RP2040 PIO hardware for Nintendo DS cartridge-bus communication.

PIO receives timed cartridge-bus data and raises interrupts when command words arrive.

The interrupt routine then dispatches the incoming data to command handlers based on:

* the current cartridge mode
* command-word position
* command identifier

## ROM Read Path

A normal game-mode ROM page read uses command:

`0xB7`

The firmware:

1. receives the command through the cartridge bus
2. declares a 512-byte response
3. calculates the requested ROM address
4. checks that the requested page is within `romSize`
5. selects bytes from `romData`
6. prepares the outgoing data
7. sends 512 bytes through DMA and PIO

The verified data path is:

```text
Nintendo DS command
        ↓
PIO receive FIFO
        ↓
interrupt handler
        ↓
command handler
        ↓
romData address
        ↓
512-byte output buffer
        ↓
DMA
        ↓
Nintendo DS cartridge bus
```

## DLDI microSD Read Path

Nintendo DS software reads storage through the standard DLDI interface.

For a sector read, the DSpico DLDI driver:

1. sends command `0xE3` with the requested sector
2. polls command `0xE4` until the firmware reports ready
3. sends command `0xE5` to retrieve 512 bytes
4. stores the returned sector in the caller's buffer
5. repeats for additional sectors

On the RP2040 side, the firmware:

1. begins reading the requested microSD sector
2. stores the sector in an RP2040 RAM buffer
3. reports when the operation is ready
4. sends the sector back through DMA and PIO
5. begins preloading the next sector when appropriate

The full path is:

```text
Nintendo DS software
        ↓
filesystem
        ↓
DSpico DLDI
        ↓
custom cartridge command
        ↓
RP2040 firmware
        ↓
SdCard subsystem
        ↓
SDIO
        ↓
microSD card
```

The return path is:

```text
microSD card
        ↓
RP2040 RAM buffer
        ↓
DMA and PIO
        ↓
Nintendo DS memory
```

## DLDI microSD Write Path

Nintendo DS software writes storage through the standard DLDI interface.

For a sector write, the DSpico DLDI driver:

1. divides the request into 512-byte sectors
2. sends command `0xF6` with the target sector and transfer flags
3. sends a 512-byte payload from Nintendo DS memory to the DSpico
4. polls until the firmware is ready for the next block
5. repeats until all sectors are written

The transfer flags identify whether a block is:

- the first block
- a middle block
- the final block
- both first and final for a single-sector write

On the RP2040 side, the firmware:

1. validates the write command
2. prepares to receive a 512-byte payload
3. stores the incoming data in an RP2040 RAM buffer
4. invokes a completion callback when the payload is complete
5. starts or queues the physical microSD write
6. reports readiness before the next block proceeds

The full path is:

```text
Nintendo DS memory
        ↓
DSpico DLDI
        ↓
command `0xF6`
        ↓
RP2040 cartridge-command handler
        ↓
RP2040 RAM buffer
        ↓
SdCard subsystem
        ↓
SDIO
        ↓
microSD card

The firmware alternates between two 512-byte buffers during multi-sector writes. This allows one sector to be written to the microSD card while the next sector is received from the Nintendo DS.

Writes require more coordination than reads because interrupted or incorrectly sequenced writes can affect stored data. The source shows readiness checks, transfer-boundary flags, callbacks, and buffer sequencing, but this project has not verified write behavior on hardware.

## Buffering

The firmware uses two 512-byte buffers for sequential SD reads.

This is called double buffering.

While one buffer is being sent to the Nintendo DS, the other can begin receiving the next microSD sector.

This reduces idle time during multi-sector reads.

## BOOTSEL Behavior

During startup, the firmware attempts to mount the microSD card.

If the card cannot be mounted, the RP2040 calls its USB boot routine and enters BOOTSEL mode.

This explains the documented recovery behavior:

```text
microSD card present and mountable
→ normal firmware operation

microSD card absent or not mountable
→ RPI-RP2 BOOTSEL drive appears
```

## Important File Distinctions

### `DSpico.uf2`

Purpose:

RP2040 firmware image

Location:

copied to the `RPI-RP2` BOOTSEL drive

Runs on:

RP2040

### `default.nds`

Purpose:

Nintendo DS ROM embedded into the RP2040 firmware

Location during build:

`dspico-firmware/roms/default.nds`

Runs on:

Nintendo DS

### `_picoboot.nds`

Purpose:

runtime Nintendo DS program loaded from microSD by the bootloader

Normal content:

renamed `LAUNCHER.nds`

Location:

microSD root

Runs on:

Nintendo DS

## Confirmed Findings

The following have been confirmed through upstream source inspection:

* `default.nds` is embedded into the firmware with `.incbin`
* the firmware assigns the embedded ROM to `gNtrRomEmu.romData`
* the RP2040 firmware emulates the cartridge interface
* the DSpico bootloader loads `fat:/_picoboot.nds`
* the bootloader selects DLDI-backed storage
* command `0xB7` reads 512-byte ROM pages
* DLDI sector reads use commands `0xE3`, `0xE4`, and `0xE5`
* SD sectors are transferred in 512-byte units
* the firmware uses double buffering for sequential SD reads
* a failed microSD mount causes the firmware to enter BOOTSEL mode

## Remaining Questions

The following details still require further source inspection:

* the complete ARM7 responsibilities in the bootloader and Pico Loader
* the full NTR and TWL switching sequence
* the exact SDIO transaction implementation
* the complete DLDI write path
* the internal Pico Loader program-loading sequence
* how Pico Launcher requests and starts selected software
* which portions of the system are covered by automated tests
* how maintainers currently validate firmware changes

## Scope of This Document

This document is an architectural overview, not a build guide or user setup guide.

It does not claim that:

* the firmware was built locally
* hardware behavior was tested
* native macOS building is supported
* all DSpico modes are covered
* every cartridge command has been traced
