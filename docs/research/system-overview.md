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

#### ARM9 Startup and Boot Handoff

The Pico Loader ARM9 does not begin in a conventional C++ `main()` function. Execution starts at the `_start` symbol in `arm9/source/crt0.s`.

The startup routine:

- disables interrupts during initialization
- configures CP15, the memory protection unit, caches, and write buffering
- configures ITCM and DTCM
- copies the ITCM and DTCM sections into place
- clears the BSS section
- creates stacks for the SVC, IRQ, and system processor modes
- branches to `loaderMain()`

`loaderMain()` completes C++ runtime initialization, clears graphics memory, synchronizes with ARM7, configures IPC FIFO communication, detects DS or DSi mode, initializes the heap, selects a loader-platform implementation, and waits for commands from ARM7.

When ARM7 sends `IPC_COMMAND_ARM9_BOOT`, the command dispatcher calls `handleBootCommand()`. This handler temporarily maps the Nintendo DS and Game Boy Advance cartridge interfaces to ARM9, performs platform-specific ROM preparation, clears ARM9 hardware state, configures DSi-mode registers when required, and then calls `bootArm9()`.

`bootArm9()` clears loader-related VRAM, finalizes DS or DSi hardware state, clears pending interrupt flags, reads the loaded ROM header from shared memory, and passes `romHeader->arm9EntryAddress` to `jumpToArm9EntryPoint()`.

`jumpToArm9EntryPoint()` preserves the entry address on the stack, clears the general-purpose registers except the stack pointer, and executes `pop {pc}`. Loading the saved address into the program counter directly transfers ARM9 execution to the selected Nintendo DS program. Pico Loader does not expect ARM9 execution to return afterward.

The confirmed ARM9 control path is:

```text
_start
        ↓
loaderMain()
        ↓
handleArm7Command()
        ↓
handleBootCommand()
        ↓
bootArm9()
        ↓
jumpToArm9EntryPoint()
        ↓
selected program's ARM9 entry point
```

This path is confirmed through source inspection. It has not yet been build-verified, emulator-tested, or hardware-tested as part of DSpico Doctor.

#### ARM7 Startup, Loading, and Boot Handoff

Pico Loader ARM7 execution begins at the `_start` symbol in `arm7/source/crt0.s`. The linker selects `_start` as the entry point through `ENTRY(_start)` in `arm7/loader7.ld`.

The ARM7 startup routine:

- disables interrupts during initialization
- clears the BSS section
- sets the stack pointer to `0x0380FD80`
- branches directly to `loaderMain()`

`loaderMain()` initializes the C++ runtime, clears ARM7 sound registers, initializes interrupt and main-thread support, and synchronizes with ARM9. The processors use a four-part handshake through IPC synchronization bits. After the handshake, ARM9 reports whether the console is operating in DS or DSi mode.

ARM7 then initializes its environment, heap, logger, real-time clock state, and selected storage interface. Depending on the loader configuration, it can mount DLDI-backed storage, the DSi SD interface, or AGB semihosting storage.

`NdsLoader::Load()` coordinates the selected program's loading process. During a normal boot, ARM7 opens the selected `.nds` file, reads its ROM header, prepares shared boot memory, and loads both processor binaries:

- the ARM9 binary is read into `arm9LoadAddress`
- the ARM7 binary is read into `arm7LoadAddress`
- DSi-mode programs may also include optional ARM9i and ARM7i binaries

The load addresses identify where executable images are copied. The separate entry addresses identify where each processor begins execution after loading is complete.

ARM7 and ARM9 use two IPC mechanisms:

- IPC synchronization bits perform the startup handshake and communicate DS or DSi mode
- the IPC FIFO carries 32-bit commands, arguments, completion signals, and returned memory addresses

ARM7 owns the high-level loading workflow. ARM9 remains in a polling loop, receives FIFO commands from ARM7, and performs ARM9-specific operations. ARM7 loads the ARM9 executable, sends `IPC_COMMAND_ARM9_APPLY_PATCHES`, and continues loading the ARM7 executable while ARM9 applies its patches. ARM7 waits for ARM9's completion response before continuing with game-specific and ARM7 patch preparation.

Shared memory holds structured boot information, including ROM headers, card identifiers, CRC values, firmware user settings, reset information, ROM offsets, boot type, and DS or DSi configuration data. ARM7 prepares most of this state before either processor enters the selected program.

For some ARM7 patches, ARM9 generates or allocates patch resources and returns their addresses through the IPC FIFO. ARM7 preprocesses cheat data, temporarily adjusts WRAM mappings when required, copies the generated patch code through the ARM7-visible WRAM window, restores the previous mapping, and places the patch data into its final destination.

The final handoff begins in `NdsLoader::StartRom()`. ARM7 waits for a display timing boundary, sends `IPC_COMMAND_ARM9_BOOT` and the soft-reset flag to ARM9, clears pending ARM7 interrupt flags, and calls the function address stored in `_romHeader.arm7EntryAddress`.

The confirmed ARM7 control path is:

```text
_start
        ↓
loaderMain()
        ↓
NdsLoader::Load()
        ↓
load ARM9 and ARM7 executable images
        ↓
coordinate shared memory, IPC, and patching
        ↓
StartRom()
        ↓
send IPC_COMMAND_ARM9_BOOT
        ↓
selected program's ARM7 entry point
```

The combined final handoff is:

```text
ARM7                                      ARM9
-----                                     ----
StartRom()
send IPC_COMMAND_ARM9_BOOT  ------------> handleArm7Command()
clear pending ARM7 interrupts              handleBootCommand()
call arm7EntryAddress                      bootArm9()
        ↓                                      ↓
selected program's ARM7                selected program's ARM9
```

These findings are based on Pico Loader source inspection at commit ad2055669b1d5e115d9261c83dc7be3e09a5f2b6, tagged v1.7.1. They have not yet been build-verified, emulator-tested, or hardware-tested as part of DSpico Doctor.

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

#### Selected Software Launch Path

Pico Launcher prepares the selected file for Pico Loader rather than loading the selected Nintendo DS software itself.

For an ordinary `.nds` file, `NdsFileType::TrySetLaunchParameters()` copies the selected file path into `pload_params_t::romPath`.

For a custom file association, `CustomFileType::TrySetLaunchParameters()`:

- copies the associated application's path into `romPath`
- copies the selected file path into `arguments`
- records the argument-buffer length in `argumentsLength`

Before preparing these values, `RomBrowserController::SetPicoLoaderParams()` clears the save path and argument fields. The inspected Pico Launcher ARM9 source does not assign another save path during this launch flow.

When launch parameters are prepared successfully, Pico Launcher changes to `PicoLoaderProcess`. That process disables both display controllers and calls `pload_start()`.

The ARM9 bootstrap then:

1. reads `/_pico/picoLoader9.bin`
2. reads `/_pico/picoLoader7.bin`
3. copies both binaries into VRAM
4. writes the boot drive and launch parameters into the Pico Loader ARM7 header
5. copies the launcher path when the loader supports API version 2 or newer
6. supplies the cheat-data pointer when the loader supports API version 3 or newer
7. remaps VRAM C and D for ARM7 access
8. sends value `1` on `IPC_CHANNEL_LOADER`
9. transfers ARM9 control to the Pico Loader ARM9 image at `0x06800000`

The Pico Launcher ARM7 IPC handler receives the loader message and sets a start flag. The normal ARM7 state machine then selects `ExitMode::PicoLoader`, disables sound and interrupts, writes the DLDI driver pointer into the Pico Loader ARM7 header, and transfers control through the entry address stored in `header7->entryPoint`.

The confirmed launcher-to-loader path is:

```text
user selects a file
        ↓
RomBrowserController::SetPicoLoaderParams()
        ↓
FileType::TrySetLaunchParameters()
        ├── .nds file: selected path → romPath
        └── custom file: associated app → romPath
                         selected file → arguments
        ↓
PicoLoaderProcess::Run()
        ↓
launcher ARM9 loads Pico Loader ARM9 and ARM7 binaries
        ↓
launcher ARM9 populates the Pico Loader ARM7 header
        ↓
launcher ARM9 sends IPC_CHANNEL_LOADER value 1
        ↓
launcher ARM9 transfers control to Pico Loader ARM9
        ↓
launcher ARM7 state machine selects ExitMode::PicoLoader
        ↓
launcher ARM7 transfers control through header7->entryPoint
```

These findings are based on Pico Launcher source inspection at commit `d31a15c315237bd69ee9b3d5bc1351ae5e38b99c`. They have not yet been build-verified, emulator-tested, or hardware-tested as part of DSpico Doctor.

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
        ↓
Pico Launcher prepares the selected path and launch parameters
        ↓
Pico Launcher loads Pico Loader ARM9 and ARM7 binaries
        ↓
Pico Launcher signals ARM7 and transfers both processors into Pico Loader
        ↓
Pico Loader reads, prepares, patches, and starts the selected software
```

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
```

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

## Build and Validation Coverage

The five core software repositories use different levels of build and validation support.

### DSpico Firmware

The firmware repository provides a local CMake build process through `CMakeLists.txt` and `compile.sh`.

The build script:

1. removes the previous `build/` directory
2. configures the project with CMake
3. builds the firmware
4. produces `DSpico.uf2`

No GitHub Actions workflow was found in the inspected repository.

The firmware includes compile-time checks for required structure offsets and source-level assertion checks for selected SDIO and USB assumptions. Whether those runtime assertions remain enabled in the produced firmware build was not verified. These checks do not constitute an automated behavioral test suite.

### DSpico Bootloader

The bootloader repository has a GitHub Actions workflow that:

1. checks out the repository and submodules
2. runs `make`
3. uploads `BOOTLOADER.nds`

The inspected workflow and Makefiles compile and package the ARM7 and ARM9 programs. No automated unit, integration, emulator, or hardware test command was found.

### DSpico DLDI

The DLDI repository has a GitHub Actions workflow that:

1. checks out the repository and submodules
2. runs `make`
3. uploads `DSpico.dldi`

No automated behavioral test, emulator execution, or hardware-validation command was found in the inspected workflow or Makefile.

### Pico Loader

Pico Loader has GitHub Actions workflows that build and package multiple platform configurations.

The nightly workflow builds 17 platform targets, including `DSPICO`, and uploads the resulting Pico Loader binaries and supporting data files.

Pico Loader also contains compile-time assertions that verify required structure sizes, field offsets, shared-memory layouts, ROM-header layouts, and other binary interfaces.

The source supports a `MELONDS` build configuration. Selecting:

```text
PICO_PLATFORM=MELONDS
```

causes the ARM9 build to define `PICO_LOADER_TARGET_MELONDS`, which selects `MelonDSLoaderPlatform`.

The inspected GitHub Actions matrix does not include `MELONDS`, and no workflow was found that launches melonDS, executes a defined test case, or records a behavioral pass or failure.

### Pico Launcher

Pico Launcher has GitHub Actions workflows that run `make`, upload `LAUNCHER.nds` and the `_pico/` directory, and package release files.

The source includes compile-time checks for selected binary layouts, register offsets, sprite dimensions, and data structures.

No automated unit, integration, emulator, or hardware test command was found.

### Validation Boundary

A successful automated build confirms that the configured source compiled and that the expected artifacts were produced.

It does not prove that:

- the software starts correctly
- ARM7 and ARM9 coordinate correctly at runtime
- microSD reads and writes work on hardware
- flashing succeeds
- the launcher-to-loader transition works
- selected software loads correctly
- existing functionality remains unaffected

The repository inspection found build automation and compile-time validation, but it did not find an automated behavioral, emulator, or hardware-in-the-loop test suite for the five core software repositories.

The exact source revisions inspected were:

- `LNH-team/dspico-firmware` — `472c9d8e9957ad18df367f14b9cc337b9b887e65`
- `LNH-team/dspico-bootloader` — `29671d041fe2e497f8c39bae562e98d955afdbc5`
- `LNH-team/dspico-dldi` — `8ba45f65690bc40d9279e663e1d89ca806451cc1`
- `LNH-team/pico-loader` — `ad2055669b1d5e115d9261c83dc7be3e09a5f2b6`
- `LNH-team/pico-launcher` — `d31a15c315237bd69ee9b3d5bc1351ae5e38b99c`

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
* Pico Loader ARM9 execution begins at `_start` in `arm9/source/crt0.s`
* `_start` prepares the ARM9 processor and memory environment before branching to `loaderMain()`
* `loaderMain()` synchronizes with ARM7 and waits for ARM7 commands through the IPC FIFO
* `IPC_COMMAND_ARM9_BOOT` leads through `handleBootCommand()` and `bootArm9()`
* the final ARM9 handoff loads `romHeader->arm9EntryAddress` into the program counter through `pop {pc}`
* Pico Loader ARM7 execution begins at `_start` in `arm7/source/crt0.s`
* ARM7 synchronizes with ARM9 through IPC synchronization bits before loading begins
* ARM7 opens the selected `.nds` file and loads both the ARM9 and ARM7 executable images
* shared memory stores structured boot data while the IPC FIFO carries commands, arguments, responses, and pointers
* ARM7 coordinates ARM9 patching while continuing ARM7-side loading work
* ARM7 sends `IPC_COMMAND_ARM9_BOOT` before calling `_romHeader.arm7EntryAddress`
* Pico Launcher stores an ordinary selected `.nds` path in `pload_params_t::romPath`
* custom file associations launch their configured application and pass the selected file path through `arguments`
* Pico Launcher loads `picoLoader9.bin` and `picoLoader7.bin` into VRAM before transferring control
* Pico Launcher writes boot, path, argument, launcher-return, and cheat information into the Pico Loader ARM7 header
* launcher ARM9 signals launcher ARM7 with value `1` on `IPC_CHANNEL_LOADER`
* launcher ARM7 enters Pico Loader through the entry address stored in `header7->entryPoint`
* DSpico Bootloader, DSpico DLDI, Pico Loader, and Pico Launcher use GitHub Actions for automated builds and artifact publishing
* the inspected DSpico Firmware repository provides a local CMake build process but no GitHub Actions workflow
* DSpico Firmware, Pico Loader, and Pico Launcher contain compile-time validation for selected structure layouts and interfaces
* Pico Loader supports a `MELONDS` build configuration, but the inspected CI matrix does not build it
* no automated unit, integration, emulator-execution, or hardware-in-the-loop test suite was found in the five inspected core software repositories

## Remaining Questions

The following details still require further source inspection:

* the full NTR and TWL switching sequence
* the exact SDIO transaction implementation
* how maintainers currently validate firmware changes

## Scope of This Document

This document is an architectural overview, not a build guide or user setup guide.

It does not claim that:

* the firmware was built locally
* hardware behavior was tested
* native macOS building is supported
* all DSpico modes are covered
* every cartridge command has been traced
