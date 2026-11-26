# Rozum
Compact flight computer and software for hobby rocketry.

## About

Rozum is a small, home-built flight computer for hobby rockets. It is based on the [Altus Metrum EasyMini](https://altusmetrum.org/EasyMini/): a simple dual-deploy altimeter that detects apogee and fires the recovery charges at the right moments.

The name *Rozum* means "reason" or "mind": it is the rocket's brain.

## Hardware

- **Barometer**: measures air pressure to work out altitude, and is the main source for detecting apogee.
- **Accelerometer**: detects launch and measures acceleration during powered flight.
- **Onboard flash memory**: stores flight data for download after recovery (no SD card).
- **Screw terminals** for:
  - **Battery**: powers the board.
  - **Kill switch**: an external switch that arms the board, so it stays safe while the rocket is handled on the ground.
  - **Drogue e-match**: fired at apogee to deploy the drogue parachute.
  - **Main e-match**: fired at a set altitude during descent to deploy the main parachute.

## How a flight works

1. **Pad**: the board is switched on with the kill switch and waits for launch.
2. **Boost**: the accelerometer detects launch.
3. **Coast**: the motor burns out and the rocket keeps climbing.
4. **Apogee**: the barometer sees altitude stop rising and the board fires the drogue e-match.
5. **Descent**: when the rocket drops to the set main-deploy altitude, the board fires the main e-match.
6. **Landing**: the board stops logging and the flight data can be read back from memory.

## Status

Early development. Hardware design, firmware, and documentation will be added as the project progresses.

## Acknowledgements

Rozum is inspired by the open hardware and software of [Altus Metrum](https://altusmetrum.org/). Any design files or code reused from their projects keep their original licenses and credit.
