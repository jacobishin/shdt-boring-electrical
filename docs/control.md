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

- What low-level controller should be used?
- What sensors are required?
- What IO count and signal types are required?
- What machine states are needed?
- What conditions should prevent motion?
- What faults should stop the machine?
- What should happen if communication is lost?
- What safety relay or safety hardware should be used?
- How should the safety circuit inhibit the motor?
- How should the system be reset after an E-stop or fault?

## Issue: Motor Control Aprroach maxrobertson

    Research assuming 15 HP AC motor

-Variable Frequency Drive (VFD)
    changes AC supply frequency and voltage sent to the motor to adjust the speed of an electric motor
    different frequencies are sent to the motor, changing how quicly the shaft is spun

    -Contains AC to DC converter comprimised of diodes. Most VFD's will have phase voltages (A,B,C) with a 
    diode on the positive and negative end of each phase. ex: When the A-phase voltage is more positive than B- or C-phase voltage, that diode opens. When B-phase becomes more positive than A-phase, that diode will open and the A-phase diode closes. This same thing happens on the negative side. This means that we get six current “pulses” as each diode opens and closes. This called a “six-pulse VFD.” 
    -VFD utilizes Pulse-Width Modulation (PWM)

    Benefits of VFD:
    -Adjusts speed of motor to digging environment (more rough->higher speeds, smoother digging->slower speeds)
    -Because speed is adjusted appropriately it can extend equipment lifespan by making them not run as hard

    Potential Drawbacks
    -VFD's are subject to Harmonic distortion, if VFD is affected, harmonic filter's and VFD topologies that produce less
    harmonics can be implemented
    -Long cables between VFD's and motors can cause reflective wave phenomena

### Motor control methods within VFD's

    Open Loop Vector Control
        -regulates AC motor speed and torque individually without an encoder or resolver
        -calculates flux slip and speed through real time currents and voltages
        -Method with low wiring complexity
        -Relies on intitial drive autotuning
        -delivers 200% rated torque
        -cannot hold zero speed reliably 
        -cost effective method -> less precision
        -relies on pre-defined settings rather than the feedback from the motor
        -closed loop control allows for more precision continously adjusting the motors parameters


-Across the Line Starting (ATL/DOL)
    -direct connection to full line voltage using contactors and relays
    -Extremely High in rush current to motors
    - Better for small motors and not very large AC motors

    -Likely not a good option for controlling the motor in the TBM

### Motor control interfacing

    



*   What specific signal conditioning and protection circuits are needed for each sensor?
*   What control signals are required by the selected VFD or soft starter?
*   How will relative encoders or depth measurements be restored/homed after a power cycle?
*   How is the safety relay wired to guarantee motor inhibition while keeping <50 V control power active for telemetry?
