Known board issues identified during debugging with a physical module.

> [!NOTE]
> These issues are not fixed in the project. This document is a collection of debugging notes and required manual modifications.

## Required Patches

- Connect 7220 pin 21 (`~CS`) to GND.
- Connect 7220 pin 7 (`~DACK`) to +5 V through a 5.1 kOhm resistor.
- Connect 7110 pin 6 to 7110 pin 1. Pin 6 is marked as NC, but reference schematics connect it to pin 1. The Bubble Memory Design Handbook identifies it as `PULSE.COM` (+12 V) and also requires this connection.
- Swap 7250 pins 14 and 12.
- Move R2 from the transistor source to its drain, placing it in series between 7230 pin 20 and transistor pin 3.

## Design Issues

- The board is highly sensitive to power-supply quality and operates reliably only with a stable bench power supply.
- The `DET.A` and `DET.B` differential pair between 7110 and 7242 lacks shielding and trace-length control.
- `PULSE.COM` (+12 V) is routed through 2 W resistors.
- The socket footprint for 7230 is unsuitable.
- R4 lacks a bypass jumper for reseeding.
- Small SMD components around the module prevent installation of a suitable socket.
- Arcing occurs during power-up when C25 is populated.
