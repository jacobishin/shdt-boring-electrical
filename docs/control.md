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

## Sensor Requirements

Competition Requirements:
+ Telemetry >= 0.1 Hz
+ Critical I/O must be monitored and interlocked

Measured values:
- 	Drill RPM: Monitors cutterhead’s rotational speed
- 	Drill Depth: Measure digging progress and current depth
- 	Motor Current: Monitors current drawn from the motor
- 	Motor Temperature: Detects temperature and overheating
- 	Machine Inclination: Measure deviation from initial inclination
- 	Travel Limits: Detects end of travel position

Feed speed can be derived from drill depth rather than directly measured.

Preliminary Decisions

Drill RPM:
+ Rotary encoder: Tracking RPM using an encoder. Can have high resolution, better for greater accuracy, can be more expensive.
+ Hall effect sensor: Tracks a permanent magnet to track RPM. Requires protection, cannot be exposed. Won't be able to tell exact degree of rotation, though that might not be required. less expensive
+ note: Hall effect sensor can also measure angular position.

Drill Depth Sensor: 
+ Magnetic linear encoder: Provides direct non contact measurements. Needs to be aligned and mounted.

Motor Current:
+ Hall-effect current transducer: Measures current through a magnetic field, can measure AC/DC current.
+ Motor-drive current telemetry: if cutterhead used a VFD, separate transducer may not be required at all.

Motor Temperature:
+ PT100/PT1000 RTD: High accuracy, stable. Nearly linear responsive curve. More expensive option (relative to NTC thermistors).
+ NTC thermistors: Fits temperature range, Non linear responsive curve. More affordable.

Machine Inclination:
+ Industrial 2-axis inclinometer: Preferred. High accuracy, however less reliable when exposed to vibration. (though dynamic options slightly mitigate that by using acceleration and angular-rate measurements for movement.) More expensive.
+ 6-axis IMU: measures 3-axis acceleration and 3-axis angular velocity. Less appropriate for a directionally static machine, therefore would require more calibration, but less expensive.
  
Travel Limit:
+ Inductive proximity sensor: Non contact, detects a metal target, resistant to debris and vibration. Short sensing range, however.
+ Mechanical limit switch: Activates a switch once a particular position is reached. Could potentially be implemented for e-stop safety instead.


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
