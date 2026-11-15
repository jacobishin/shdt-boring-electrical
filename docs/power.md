# Power

## Scope

Power is responsible for:

*   480 VAC 3-phase main power intake and distribution
*   120 VAC support power distribution
*   24 VDC and lower-voltage DC conversion and distribution
*   branch circuit protection, grounding, and residual current monitoring
*   motor-control power hardware (VFDs, soft starters, contactors)
*   hardwired E-stop power interruption, LOTO disconnects, and 480 VAC energized status indication (stacklight)

## Current Architecture

Power receives site power and distributes protected supply rails to all motors, actuators, control electronics, and computing hardware.

```text
480 VAC, 3-Phase Supply
   |
   +---- Energized Indicator (Stacklight)
   |
   +---- Main Disconnect (LOTO) & RCM
            |
     E-Stop Contactor / Motor Control (VFD / Soft Starter)
            |
       Main Motors


120 VAC Supply
   |
24 VDC Power Supply
   |
   +---- Unswitched 24 VDC Bus (Control & Telemetry Electronics)
   |        |
   |     5 VDC Conversion (Low-Voltage Logic)
   |
   +---- Switched 24 VDC Bus (E-Stop Gated Contactor Coils & Actuators)
```

### High-Power & Motor Control Hardware

*   480 VAC 3-phase distribution, cabling, and locking connectors
*   Lock Out Tag Out (LOTO) main disconnect and residual current monitoring (e.g., Bender RCM420)
*   480 VAC stacklight wired directly to indicate when the machine is energized
*   VFD, soft starter, or main contactor power-stage wiring and thermal management

### Low-Voltage Distribution

*   120 VAC to 24 VDC power supply and distribution
*   separation of low-voltage control/telemetry bus (remains active during E-stop) and actuator/contactor coil bus (inhibited during E-stop)
*   DC/DC conversion (e.g., 5 VDC) for low-level and high-level logic hardware

### Safety & E-Stop Power Interruption (Joint with Control)

*   Power and Control co-design the physical E-stop circuit, safety relay wiring, and motor-inhibit/contactor coil connections so hazardous power is interrupted near the source while maintaining safe low-voltage telemetry.

## M2 Design Work

*   complete the machine power budget across all subsystems
*   size circuit breakers, fuses, contactors, and cabling
*   establish electrical wire color-coding standards
*   select motor-control hardware (VFD vs. soft starter vs. contactors)
*   design the 480 VAC, 120 VAC, and 24 VDC electrical schematics
*   co-design the E-stop safety relay and motor-inhibit wiring with Control
*   select power supply telemetry sensors (voltage, current, supply temperature) to feed into Control

## Open Questions

*   What is the peak and continuous current draw of the main drive motor under load?
*   Will the main motor use a VFD, soft starter, or across-the-line contactor?
*   What enclosure cooling is required to keep power electronics below maximum operating temperatures?
*   How are the separate 24 VDC control and peripheral buses isolated during an E-stop event?