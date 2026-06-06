# ALPHA Quad Timer

## Overview

The **ALPHA Quad Timer** is a legacy Honda OBD1 ECU flash replacement and live map switching project based around PLCC32 flash memory devices.

This design expanded on the earlier ALPHA SST Replacement and ALPHA 2Timer Live Mod hardware by doubling the synchronized bank-switching method. Instead of supporting only two live-selectable tunes, the ALPHA Quad Timer allows up to **four separate tunes on one flash chip**.

The board was designed for larger PLCC32 flash devices adapted into the traditional 28-pin DIP footprint used by common 27SF512-style Honda OBD1 ECU chip installs.

The ALPHA Quad Timer also includes onboard LED indicators to show which map or tune bank is currently selected.

This project is now considered legacy hardware and is preserved for educational, historical, and reference purposes.

## Purpose

The purpose of the ALPHA Quad Timer was to push legacy flash-based Honda ECU tuning beyond a single fixed calibration or simple two-map switching setup.

Traditional chip tuning usually required removing, erasing, programming, and reinstalling a chip every time a calibration needed to be changed. Dual-bank designs improved that by allowing two tunes on one chip, but the ALPHA Quad Timer expanded the idea further by supporting four selectable tune banks from a single PLCC32 flash device.

This allowed multiple calibrations to exist on one chip, such as street, race, valet, economy, test, or alternate fuel maps, depending on how the ROM was arranged.

## Supported Flash Device Style

The ALPHA Quad Timer was designed around larger **PLCC32 flash memory devices** commonly used in ALPHA SST replacement hardware.

Supported device families include chips similar to:

```text
SST39SF040
SST39SF010
ST M29F040B
AMD AM29F010
```

These devices provide more addressable memory than the original 27SF512-style chip setup, making it possible to store multiple 32KB Honda OBD1 ROM images on a single physical flash chip.

## Four-Tune Bank Switching

The ALPHA Quad Timer uses the same core idea as the ALPHA 2Timer Live Mod, but doubled up to support four selectable tune banks.

A standard 2Timer-style setup uses synchronized switching logic to safely select between two banks. The Quad Timer expands this concept by controlling additional high address lines, allowing four separate ROM sections to be selected from one larger flash chip.

Instead of allowing the active bank to change asynchronously while the ECU is reading from ROM, the Quad Timer uses flip-flop based timing logic to latch requested bank changes in a more controlled way.

This helps reduce unstable address transitions during live switching.

## 2Timer Method Doubled Up

The original 2Timer method was created to reduce the chance of triggering the **MIL / Check Engine Light** during live map switching.

On some ECU setups, changing the active ROM bank outside of the approximate **180 ns OE high window** could cause the ECU to briefly read invalid or unstable data. That small timing glitch could be enough to trigger a fault.

The ALPHA Quad Timer doubles this method so that multiple bank select lines can be controlled using synchronized logic.

This allows four maps to be selected while still improving switching reliability compared to direct mechanical switching or unsynchronized address line changes.

This does not guarantee every possible live-switching condition is safe, but it provides a much cleaner and more controlled method than directly toggling high address lines during ECU operation.

## LED Map Indicators

The ALPHA Quad Timer includes LED indicators to show which tune bank is currently loaded.

These indicators make it easier to confirm the selected map without needing to guess switch position or track bank state externally.

The LED indicators are intended to provide a quick visual reference for the active calibration bank.

Example map states:

```text
Map 1 selected
Map 2 selected
Map 3 selected
Map 4 selected
```

The exact map naming and ROM layout depend on how the flash chip is programmed.

## Address Line Handling

Like the earlier ALPHA SST replacement hardware, address lines above A14 are used for bank selection.

High address lines must be handled correctly to avoid floating inputs, random bank selection, unstable reads, or unintended map switching.

The Quad Timer design is intended to control the required upper address lines through synchronized logic instead of leaving them floating or switching them directly during ECU operation.

Where applicable, pull-up and pull-down resistors are used to define stable logic states and prevent unwanted behavior.

## ALPHAburner Compatibility

The ALPHA Quad Timer was designed for the same general legacy flash workflow as ALPHA SST replacement hardware.

These boards were intended to work with ALPHAburner-style programming workflows for PLCC32 flash devices used in Honda OBD1 ECU applications.

The original ALPHAburner platform was STM32-based and is now deprecated.

Later ALPHA burner development moved toward RP2350B-based hardware before the project direction shifted away from chip burning and toward real-time emulation.

## Legacy Status

This project is considered **legacy hardware**.

At the time the ALPHA Quad Timer was developed, physical flash-based tuning and multi-map chip switching were still useful ways to expand Honda OBD1 ECU functionality.

Since then, open-source real-time emulation solutions such as **oneROM** have made live ROM emulation extremely affordable and accessible.

With oneROM making real-time emulation possible at roughly a $10 hardware cost, traditional chip tuning, multi-map flash switching, and repeated chip burning have largely been put to bed for development, testing, calibration, and live tuning workflows.

The ALPHA Quad Timer remains available as part of the ALPHA hardware timeline and as a reference for studying legacy ROM bank switching, synchronized map selection, and multi-tune flash hardware.

## Educational Use

The files, designs, documentation, notes, and related information in this repository are provided for **educational and reference use only**.

This project is shared to help others learn about Honda OBD1 ECU memory hardware, PLCC32 flash replacement, expanded ROM layouts, high address line control, synchronized bank switching, flip-flop based edge capture, four-map selection, and legacy chip tuning workflows.

You are responsible for verifying electrical compatibility, timing behavior, ROM layout, flash device support, ECU compatibility, and safe operation before applying these designs to any vehicle, ECU, programmer, or hardware setup.

## Attribution Requirement

Any use, modification, redistribution, derivative hardware, documentation, video content, product listing, public project, or repository based on this design must provide clear attribution to:

```text
ALPHA / ALPHA Tuning
https://github.com/alpha-tuning
```

Attribution must remain visible in any public repository, documentation, schematic, PCB layout, board file, firmware reference, video description, product page, or derivative version that uses or references this work.

You may not remove ALPHA attribution, claim this design as your own original work, or present modified versions in a way that hides the original source.

If this project is used as the basis for another design, that design must clearly state that it is derived from the original ALPHA Quad Timer hardware.

## Disclaimer

This repository is provided as-is, with no warranty, guarantee, or promise of fitness for any particular use.

Working with ECU memory hardware can affect vehicle operation. Incorrect wiring, incorrect flash pinouts, unstable bank switching, invalid ROM layout, unsupported memory devices, improper installation, or faulty calibration data can cause ECU faults, drivability issues, hardware damage, or unsafe vehicle behavior.

Use these files and designs at your own risk.

## Project Status

Development on this hardware has ended.

The ALPHA Quad Timer is considered legacy hardware.

Future ALPHA work is focused on modern real-time emulation platforms, including **ALPHAemu**, **AlphaLink**, and open-source related workflows such as **oneROM**.
