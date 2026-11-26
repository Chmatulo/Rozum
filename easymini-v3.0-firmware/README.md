# EasyMini v3.0 firmware (AltOS)

This folder holds the source files from Altus Metrum's **AltOS** that are built into the
firmware for the **EasyMini v3.0** dual-deploy altimeter. Nothing else from AltOS is here.

- **Source:** `git://git.gag.com/fw/altos`, which is the official AltOS repository. AltOS has no
  GitHub mirror. Keith Packard's GitHub repos `altusmetrum` and `web-altusmetrum` hold PCB parts
  and the website, not firmware.
- **Revision:** `f3b64b33a07bc2925e2cb842c19d4e1f944e52d9` (2026-11-17)
- **License:** GPL v2 or later (see `COPYING`; authors are listed in `AUTHORS`)

## Hardware (from `src/easymini-v3.0/ao_pins.h`)

| Item | Detail |
|---|---|
| MCU | NXP LPC11U14, ARM Cortex-M0, 12 MHz crystal, 48 MHz PLL, 24 MHz system clock |
| Barometer | MS5607 on SPI0 (CS P1.5) |
| Log flash | M25-series SPI flash on SPI1 (CS P0.23) |
| Pyro outputs | Drogue on P1.25, main on P0.2, with continuity sensing through the ADC |
| Beeper | Timer 0 channel 3 on P1.27 |
| USB | Device with pull-up, product ID 0x0026 |
| Not fitted | Radio, GPS, accelerometer, serial |

## What is included

The file list comes from the two Makefiles (`src/easymini-v3.0/Makefile` and
`src/easymini-v3.0/flash-loader/Makefile`). Each listed `.c` file was run through
`gcc -MM` with `-DEASYMINI_V_3_0` (or `-DALTOS_FLASH` for the loader) to find every header it
includes. The original directory layout is kept, so the Makefiles' relative paths still work.

| Path | Contents |
|---|---|
| `src/easymini-v3.0/` | Board code: `main()` in `ao_easymini.c`, the pin map, and the Makefile |
| `src/easymini-v3.0/flash-loader/` | Pin config and Makefile for the USB bootloader |
| `src/kernel/` | Scheduler, flight state machine, Kalman filter, logging, command shell, config |
| `src/drivers/` | MS5607 barometer and M25 flash drivers |
| `src/lpc/` | LPC11U drivers (USB, SPI, ADC, timer, beeper, EXTI, flash), startup code, linker scripts |
| `src/product/` | Flash-loader task |
| `src/util/`, `src/kalman/` | Nickle/bash scripts that generate headers at build time |
| `src/Makefile.defs`, `src/Makedefs.in` | Shared build settings |

The device runs two images:

1. **Application** (`easymini-v3.0-*.elf`): the altimeter itself.
2. **Flash loader** (`easymini-v3.0-altos-flash-*.elf`): the USB bootloader used to update the firmware.

## Generated headers (not included)

These three headers are created during the build and are not in the repository:

| Header | Generator |
|---|---|
| `ao_kalman.h` | `bash src/util/make-kalman src/kalman` (needs `nickle`) |
| `altitude-pa.h` | `nickle src/util/make-altitude-pa` |
| `ao_product.h` | `src/util/ao-make-product.5c`, run by the Makefile rule in `src/Makefile.defs` |

## Building

These files are a trimmed copy for reading and study. To build, the easiest route is the full AltOS
tree (`./autogen.sh && ./configure && make` in `src/easymini-v3.0`). That needs:

- `arm-none-eabi-gcc` with **picolibc**
- `nickle`
- `ao-tools/ao-elftohex`, used to combine the loader and application `.ihx` files (not included here)
