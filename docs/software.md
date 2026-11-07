# Software

## Scope

Software is responsible for:

- high-level computer software (On pi)
- low-level controller firmware/software
- communication between controllers
- operator interface
- telemetry
- logging
- networking

## Current Architecture

The machine will use two control layers.

```text
Operator Laptop
      |
      v
High-Level Computer
      |
      v
Low-Level Controller
      |
      v
Machine
```

### High-Level Computer

Expected responsibilities include:

- operator communication
- networking
- telemetry handling
- data logging
- GUI or backend services
- sending high-level commands to the low-level controller

Exact hardware is still TBD.

### Low-Level Controller Software

Software will implement the firmware/software required by the low-level controller.

The required machine behavior, including states, interlocks, and fault responses, will be defined jointly with Control.

## M2 Design Work

- define communication between the two control layers
- define the command and telemetry data exchanged
- define GUI requirements
- define telemetry requirements
- define logging requirements
- define networking behavior
- define reconnect behavior
- define communication-loss handling
- define watchdog behavior
- create the first software structure and prototypes

## Open Questions

- How should the high-level computer and low-level controller communicate?
- What messages need to be exchanged?
- What information should the operator see?
- What commands should the operator be allowed to send?
- What data should be logged?
- How should the GUI indicate stale or lost data?
- What should happen when either controller restarts?
- What watchdog or heartbeat behavior is required?

## Extra Note

Control defines required machine behavior. Software implements that behavior in firmware and software.

So For example:
- Control defines what should happen when a sensor fails.
- Software implements that response in the controller code.
