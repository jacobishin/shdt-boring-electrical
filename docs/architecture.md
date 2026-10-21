# Electrical Architecture

## Purpose

This document defines the high-level electrical architecture for the boring machine at the end of M1.

Detailed subsystem design will be developed separately during M2.

## System Overview

The electrical system is divided into four main functions:

- Power & Drives
- Machine Control
- Supervision & Operator Interface
- Hardware Safety

High-level system flow:

```text
Operator Laptop
      |
      v
High-Level Computer
      |
      v
Low-Level Controller
   /         \
Sensors    Motor Control
                |
                v
            Main Motor

Hardware Safety
      |
      +------------------> Motor Inhibit
```

Exact controller hardware is still TBD.

## Power Architecture

The current power architecture is:

```text
480 VAC, 3-phase
      |
      v
Protection / Disconnect
      |
      v
Motor Control
      |
      v
Main Motor

120 VAC
   |
   v
24 VDC Power Supply
   |
   v
24 VDC Distribution
   |
   +--> Safety Hardware
   +--> Sensors
   +--> Relays / Contactor Coils
   +--> Interface Electronics
   +--> DC/DC Conversion
             |
             v
      Low-Voltage Electronics
```

Current decisions:

- 480 VAC is used for the main high-power motor path.
- 120 VAC is used primarily for control and support power.
- 24 VDC is the main control and instrumentation distribution voltage.
- Lower-voltage electronics will use DC/DC conversion as required.
- Exact motor-control hardware is still TBD.
- The existing EDR-120-24 is the current candidate 24 VDC supply and will be checked during M2.

## Control Architecture

The machine will use two control layers.

### High-Level Computer

Responsible for:

- operator communication
- networking
- telemetry
- logging
- GUI/backend functions
- sending high-level commands to the low-level controller

Exact hardware is TBD.

### Low-Level Controller

Responsible for:

- sensor and machine IO
- machine state
- local interlocks
- fault handling
- actuator commands
- motor-control commands

Exact hardware is TBD.

The high-level computer sends high-level requests to the low-level controller. The low-level controller determines whether those requests are allowed before commanding machine hardware.

## Hardware Safety

Emergency shutdown will use a hardware safety path independent of normal control software.

The current concept is:

```text
E-Stop / Safety Inputs
        |
        v
Safety Relay / Hardwired Safety Circuit
        |
        +------------------> Motor Inhibit
        |
        +------------------> Safety Status
                                |
                                v
                     Low-Level Controller
```

The controller may monitor safety status, but software will not be the only mechanism capable of stopping hazardous motion.

Exact safety hardware and implementation are still TBD.

## Main Interfaces

| Interface | Purpose |
|---|---|
| Operator Laptop <-> High-Level Computer | Operator commands, GUI, telemetry |
| High-Level Computer <-> Low-Level Controller | Commands, state, faults, telemetry |
| Low-Level Controller <-> Sensors | Machine measurements and status |
| Low-Level Controller <-> Motor Control | Normal motor commands and status |
| Hardware Safety -> Motor Control | Independent hazardous-motion inhibit |
| Hardware Safety -> Low-Level Controller | Safety status monitoring |
| 24 VDC Distribution -> Control Hardware | Control and instrumentation power |

Detailed protocols, wiring, pinouts, and component selections will be developed during M2.

## Open Questions

- What high-level computer should be used?
- What low-level controller should be used?
- How should the two controllers communicate?
- What sensors and IO are required?
- What motor-control method should be used?
- Is variable speed required?
- Is reverse operation required?
- Is the existing 24 VDC supply sufficient?
- What branch protection is required?
- How should grounding and bonding be implemented?
- What safety relay or safety hardware should be used?
- What machine states, interlocks, and fault responses are required?
- What should the operator interface display and control?