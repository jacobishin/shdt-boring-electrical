# Software

## Scope

Software is responsible for:

*   high-level computer software (on Raspberry Pi)
*   communication link with the low-level controller (co-defined with Control)
*   operator interface (Operator GUI and Telemetry GUI)
*   competition server telemetry reporting (0.1 Hz HTTP POST JSON payload)
*   external datastore logging
*   networking and link monitoring

## Current Architecture

The machine will use two control layers.

```text
Operator Laptop (GUI)
      |
      v
High-Level Computer (Software - Raspberry Pi)
      |
      v
Low-Level Controller (Control - Teensy 4.1)
      |
      v
   Machine
```

### High-Level Computer

Expected responsibilities include:

*   operator communication and GUI hosting/backend services
*   networking and active monitoring of all communication links
*   telemetry handling and 0.1 Hz JSON POST requests to the TBC server
*   data logging of all I/O List parameters to an external datastore
*   sending high-level operating commands and interlock-override requests (with TBC approval) to the low-level controller

Exact hardware is still TBD (baseline: Raspberry Pi). 

### Interface with Low-Level Controller

Control implements the firmware/software on the low-level controller (Teensy 4.1). Software and Control jointly define the communication protocol (ICD), command set, telemetry packets, and heartbeat watchdog between the Raspberry Pi and Teensy 4.1.

## M2 Design Work

*   co-define the communication protocol (ICD) and command/telemetry data exchanged between the Raspberry Pi and Teensy 4.1 with Control
*   define GUI requirements (Critical I/O displays, alarm/warn indicators, interlock status, TBC-approved interlock disable mechanism, manual fallback controls)
*   define telemetry requirements (including the 0.1 Hz TBC JSON schema)
*   define external datastore logging requirements
*   define networking and reconnect behavior
*   define high-level communication-loss handling, stale-data indicators, and global abort triggers
*   create the first high-level software structure and GUI prototypes

## Open Questions

*   How should the high-level computer and low-level controller communicate (e.g., Ethernet, CAN, USB/UART)?
*   What messages and heartbeat intervals need to be exchanged in the ICD?
*   What information should the operator see on the primary tab vs. specialized tabs?
*   What commands should the operator be allowed to send in closed-loop vs. manual fallback modes?
*   How should the external datastore be structured for live viewing and debugging?
*   How should the GUI indicate stale data, network loss, or machine power loss?

## Extra Note

Control owns the low-level controller hardware and firmware, enforcing real-time state transitions and hardware interlocks. Software owns the high-level computer, operator GUI, data logging, and external networking.

So for example:

*   Control detects a sensor failure on the Teensy 4.1, triggers a local interlock/safe state in firmware, and reports the fault status over the serial/network link.
*   Software receives the fault report on the Raspberry Pi, logs the event to the external datastore, highlights the alarm/interlock on the Operator GUI, and provides the interface for operator recovery or TBC-approved override.