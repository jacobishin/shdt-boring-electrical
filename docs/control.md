# Control

## Scope

Control is responsible for:

- sensors
- machine IO
- low-level control requirements
- machine states
- interlocks
- fault handling
- E-stop architecture
- hardware safety
- motor enable and inhibit behavior



## Current Architecture

The machine will use a low-level controller (Teensy 4.1) for direct machine control.

Expected responsibilities include:

- reading sensors
- controlling machine outputs
- maintaining machine state
- checking local interlocks
- detecting faults
- issuing motor-control commands
- reporting machine status

## M2 Design Work

- create the machine IO list
- define required sensors
- define sensor signal types and interfaces
- define machine states
- define interlocks
- define fault conditions and responses
- define communication-loss behavior
- define the E-stop implementation
- define safety reset behavior
- define the motor-control interface
- define how safety status is monitored by the controller

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

    



