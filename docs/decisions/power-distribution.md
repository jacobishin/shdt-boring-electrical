# Power Distribution

## Decision

Use the competition-provided power sources as follows:

- **480 VAC, 3-phase** for the main excavation motor and any future high-power loads that require it.
- **120 VAC** for control/support power and low-power equipment.
- **24 VDC** is generated from 120 VAC for the control system.

Current high-level architecture:
480 VAC
  |
  +--> Motor Control --> Main Motor
  +--> Future high-power loads, if required

120 VAC
  |
  +--> EDR-120-24 --> 24 VDC control power
  +--> Other 120 VAC support loads, if required

## Why

This keeps control power simple while avoiding the assumption that 480 VAC is reserved only for the cutter motor. It also leaves room for future high-power loads without forcing them onto the 120 VAC circuits.

## Notes
- Exact motor-control hardware is still TBD.
- Exact 24 V branch loads and protection are still TBD.
- Loss-of-power and E-stop behavior will be defined as the safety/control design develops.