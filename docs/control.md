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

The machine will use a low-level controller for direct machine control.

Expected responsibilities include:

- reading sensors
- controlling machine outputs
- maintaining machine state
- checking local interlocks
- detecting faults
- issuing motor-control commands
- reporting machine status

Exact controller hardware is still TBD.

Possible options include:

- Teensy
- STM32-based controller
- PLC

Emergency shutdown will use a separate hardware safety path and will not rely solely on normal controller software.

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

## Safety Architecture
For safety, there's essentially three separate safety systems:
- Physical E-stop
- Automatic hardware stops
- MCU controlled stop

### Physical E-stop
There's a large and obvious physical button that when used, allows the operator to stop hazardous machine operation. This should physically interupt the connection between the hardware and the power source. 

### Automatic Hardware stops
As well, there should be automatic, independent electrical hardware that detects unsafe operation, and automatically stops the hazardous operation. This will be specific to specific components and connections, but may include:
- safety relays
- contactors
- power isolation
- hardware interlocks
- electrical protection devices (circuit breakers, fuses, PE's, surge protection, etc)


### MCU controlled stop
The MCU adds a third layer of protection, which is software controlled. It involves automatic shutdowns through sensor monitoring, watchdogs, state validation, etc. It can also be stopped by the operator through GUI control


## M2 Design Work

- select the low-level controller
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
