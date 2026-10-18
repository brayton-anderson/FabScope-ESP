# FabScope-KE (working title)

A through-hole, ESP32-based variant of the **FabScope** open-source oscilloscope, redesigned so it can be built in Kenya from locally available parts.

> **This project is a derivative work of [FabScope](https://github.com/satshas/FabScope) by Daniele Ingrassia (Fablab Kamp-Lintfort, Hochschule Rhein-Waal), licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).** Full credit for the original concept, schematic and firmware goes to the original author. See [Attribution](#attribution) and [Changes from the original](#changes-from-the-original).

![Board render or photo](media/board.png)
<!-- Replace with a real image of your board / assembled unit -->

## Why this variant exists

The original FabScope relies on SMD parts and an ATmega1284P-AU, which are hard to source and hand-solder in many local markets. This variant aims to be buildable with:

- Through-hole components found in Kenyan electronics shops (e.g. Nairobi's Luthuli Avenue and River Road) or easily shipped locally
- An off-the-shelf **LOLIN32 V1 D32 (ESP-32S)** module instead of a bare microcontroller
- Basic fabrication: home-etched, CNC-milled, or low-cost PCB fab

## Changes from the original

| Area | Original FabScope | This variant |
|---|---|---|
| Microcontroller | ATmega1284P-AU (SMD) | ESP-32S on LOLIN32 V1 D32 module |
| Components | Mostly SMD | Through-hole |
| Programming | FTDI adapter | USB via the LOLIN32 board <!-- verify against your design --> |
| Sourcing | Fab lab / EU suppliers | Locally available parts in Kenya |
| Firmware | AVR / Arduino C++ | ESP32 / Arduino-ESP32 <!-- adjust --> |
| Logic / ADC level | 5 V | 3.3 V (see [Design notes](#design-notes)) |

## Features

<!-- Keep only what your version actually does. Original features, for reference: -->
- Voltage ranges: 0–5 V and 0–50 V (DC only) <!-- verify -->
- Adjustable sampling frequency (potentiometer) <!-- verify -->
- 128 × 64 graphic display <!-- verify -->
- Min / max / average voltage measurement <!-- verify -->
- On/off switch, plug-and-play probes, battery powered <!-- verify -->

## Repository structure

```
.
├── hardware/
│   ├── schematic/        # Schematic source + PDF export
│   ├── pcb/              # Board files (KiCad/Eagle), Gerbers, fab-ready zips
│   └── enclosure/        # Case files (MDF/acrylic, laser-cut etc.)
├── firmware/             # ESP32 firmware (Arduino / PlatformIO)
├── docs/
│   ├── bom.md            # Bill of materials with local sourcing notes
│   ├── assembly.md       # Build guide
│   └── calibration.md    # Calibration procedure
├── media/                # Photos, renders, screenshots
├── LICENSE
├── NOTICE.md             # Attribution details
└── README.md
```

## Bill of materials

See [`docs/bom.md`](docs/bom.md) for the full list. Include for each part: designator, value, package (through-hole), quantity, and where to buy it locally.

## Getting started

### 1. Fabricate the board
Gerber files are in `hardware/pcb/gerbers/`. [Add notes on board size, layers, copper weight and minimum track width.]

### 2. Assemble
Follow [`docs/assembly.md`](docs/assembly.md). Solder from the lowest-profile parts up (resistors, diodes, sockets, headers, then taller parts). Use female headers for the LOLIN32 module so it can be replaced.

### 3. Flash the firmware
1. Install the Arduino IDE (or PlatformIO) with ESP32 board support.
2. Select board: **WEMOS LOLIN32** (or **LOLIN D32**).
3. Open `firmware/<sketch-name>/`, install any required libraries, then upload over USB.

### 4. Calibrate
See [`docs/calibration.md`](docs/calibration.md).

### 5. Features
--

- 2 voltage range supported: 0-5V, 0-50V (DC only)
- button to change the voltage ranges and fit using interrupts
- supported sampling frequency: from 100HZ to 10KHZ
- adjustable sampling frequency using a potentiometer
- 128 x 64 graphic display
- embedded high voltage relays
- on/off switch
- plug and play probes
- portable (9V battery last for more than 8 hours)
- mdf and acrylic cases
- programmable using an FTDI
- voltage measurements: max, min, average
- fit function that will zoom exactly into the current signal range

Board
--

## Design notes

- **Logic and ADC are 3.3 V.** The original design is built around 5 V. Input dividers/protection for the 0–5 V and 0–50 V ranges must be sized so the ESP32 ADC pin never exceeds 3.3 V. ESP32 GPIOs are **not** 5 V tolerant.
- **ESP32 ADC is nonlinear** and noisier than the AVR's. Calibration (and possibly a lookup table) is recommended. ADC2 pins are unavailable when Wi-Fi is in use.
- <!-- Add notes on relay drive, display interface, battery voltage and regulation as built. -->

## Status

- [x] Schematic complete
- [x] PCB tested
- [ ] Firmware ported and tested
- [ ] Enclosure files added
- [ ] Calibration documented

## Attribution

This work is based on **FabScope** by **Daniele Ingrassia** at **Fablab Kamp-Lintfort, Hochschule Rhein-Waal**:
<https://github.com/satshas/FabScope>

Original license: [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/).

Changes were made to the original design; they are listed in [Changes from the original](#changes-from-the-original). This is an independent adaptation and is not endorsed by the original author or institution.

## License

This derivative work is released under the same license as the original, **CC BY-SA 4.0**, as required by its ShareAlike term. See [`LICENSE`](LICENSE).

<!-- If any firmware you wrote is entirely original and not derived from FabScope's code, you may license it separately (e.g. MIT) and say so here. Check the original firmware's license first. -->

## Author

**Brayton Anderson** — Nairobi, Kenya
**WhatsApp** - +254 7068 529 23
**Email** - brayton@proton.me
**LinkedIn**[Brayton Anderson)](https://linkedin.com/in/brayton-anderson-512557142).


## Contributing

Issues and pull requests are welcome, especially: alternative local part substitutions, assembly photos, calibration data, and firmware improvements.

## Acknowledgements

- Daniele Ingrassia, creator of FabScope
- Fablab Kamp-Lintfort, Hochschule Rhein-Waal
