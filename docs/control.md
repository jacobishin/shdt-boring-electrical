# Control

## Scope

Control is responsible for:

*   low-level controller hardware and firmware (Teensy 4.1)
*   sensor interfacing and signal conditioning
*   actuator and motor-control command signals
*   machine state machine, safety interlocks, and fault handling
*   hardwired E-stop safety relay logic and motor-inhibit signals (co-designed with Power)

## Current Architecture

The Control subsystem centers around a Teensy 4.1 microcontroller that interfaces directly with the machine's sensors, actuators, and motor-control hardware, while communicating with the high-level computer (Software).

```text
High-Level Computer (Software)
              |
              v
   Teensy 4.1 (Control Hardware & Firmware)
      |               |               |
      v               v               v
   Sensors      Motor Control     Actuators
```

### Low-Level Controller Hardware & Firmware

Control owns both the physical electrical interfacing and the embedded C/C++ firmware running on the Teensy 4.1. Expected responsibilities include:

*   reading and conditioning sensor inputs (RPM, depth, motor current, motor temperature, inclination, travel limits)
*   maintaining the authoritative machine state machine
*   enforcing critical I/O bounds and safety interlocks
*   executing local abort and safe-stop sequences upon fault or communication loss
*   generating low-level control outputs for motor drives and actuators
*   exchanging telemetry and high-level commands with the Raspberry Pi

### Safety & Motor Inhibit Interface (Joint with Power)

*   Control specifies the safety relay logic, E-stop state monitoring, and two-action reset behavior.
*   Control and Power jointly own the interface between the safety relay outputs, 24 VDC contactor/relay coils, and VFD Safe Torque Off (STO) / motor-inhibit inputs.

## M2 Design Work

*   define the machine state diagrams and Theory of Operations
*   finalize the I/O List (critical flags, warning bounds, alarm/interlock bounds)
*   select sensors and design signal-conditioning circuits
*   implement initial low-level controller firmware structure on the Teensy 4.1
*   co-define the serial/network Interface Control Document (ICD) and heartbeat watchdog with Software
*   define local abort behavior upon communication loss or sensor timeout (>10 seconds)
*   define reboot and encoder/position homing behavior so I/O reflects physical reality after power loss
*   co-design the hardwired E-stop and motor-inhibit schematic with Power

## Open Questions

*   What specific signal conditioning and protection circuits are needed for each sensor?
*   What control signals are required by the selected VFD or soft starter?
*   How will relative encoders or depth measurements be restored/homed after a power cycle?
*   How is the safety relay wired to guarantee motor inhibition while keeping <50 V control power active for telemetry?