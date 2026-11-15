# Electrical System Architecture

## 1. Purpose

This document describes the high-level electrical architecture of the boring machine design. It defines the three main electrical subsystems and their areas of responsibility. Detailed work is in subsystem documents.

## 2. System Overview

The electrical system is divided into three subsystems:

* **Control** manages the machine's physical inputs and outputs, processes sensor information, implements low-level controller firmware, and coordinates machine operation.
* **Power** supplies and distributes electrical energy to the machine's equipment and electrical systems, and provides hardwired power-disconnect and motor-inhibit switching.
* **Software** provides the high-level computer software, operator interface, communication, monitoring, data logging, and external telemetry functions.

```text
             Operator Interface
                     |
                 Software
                     |
              High-Level Commands
                     |
                  Control
                     |
          Machine Inputs and Outputs
                     |
                   Power
                     |
             Motors and Equipment
```

## 3. Power Architecture

The machine uses multiple voltage levels to supply its electrical equipment. The current plan for power architecture is:

| Voltage | Primary Purpose |
| ------ | ------ |
| 480 VAC, 3-phase | Main high-power motor supply |
| 120 VAC | Supporting electrical and control power |
| 24 VDC | Main control and instrumentation supply |
| Lower-voltage DC | Electronic components requiring lower operating voltages |

The general power distribution structure is:

```text
480 VAC, 3-Phase
       |
Protection / Disconnect
       |
Motor Control
       |
Main Motors


120 VAC
   |
24 VDC Power Supply
   |
24 VDC Distribution
   |
   +---- Control Hardware
   |
   +---- Sensors and Instrumentation
   |
   +---- Supporting Electrical Devices
   |
   +---- 5 VDC/DC Conversion
              |
       Lower-Voltage Electronics
```

## 4. Subsystem Responsibilities

### 4.1 Control

The Control subsystem manages the machine's physical hardware and coordinates its operation. Essentially, it is responsible for translating high-level commands into machine-level actions. The central processing will be an MCU, namely, a Teensy 4.1.

Its responsibilities include:

*   Developing and running the low-level controller firmware on the Teensy 4.1.
*   Defining and executing machine state logic, safety interlocks, and local fault responses.
*   Interfacing with sensors, actuators, and motor-control signals.
*   Co-designing the hardwired E-stop safety relay and motor-inhibit interface with Power.

More information in [control](control.md)

### 4.2 Power

The Power subsystem supplies, converts, and protects electrical power across all machine voltage levels.

Its responsibilities include:

*   Managing 480 VAC 3-phase, 120 VAC, 24 VDC, and lower-voltage DC distribution.
*   Providing branch protection, grounding, and lock-out/tag-out (LOTO) disconnect hardware.
*   Housing and wiring high-power motor-control hardware (VFDs, soft starters, or contactors) and energized status indication (480 VAC stacklight).
*   Co-designing the physical E-stop power-interruption and motor-inhibit circuit with Control.

More information in [power](power.md)

### 4.3 Software

The Software subsystem provides the high-level software functions required to operate and monitor the machine. It will run on a Raspberry Pi, or an equivalent high-level computer.

Its responsibilities include:

*   Providing the graphical user interface (GUI).
*   Allowing operators to interact with the machine.
*   Sending high-level operating commands to Control.
*   Displaying machine measurements, alarm/warn bounds, and interlock status information.
*   Handling communication between the high-level computer, low-level controller, and competition telemetry server.
*   Logging machine data to an external datastore and reporting faults.
*   Providing software-based shutdown requests.

More information in [software](software.md)

## 5. Safety

Safety is a responsibility and consideration of all three subsystems. The safety responsibilities of each subsystem are documented below:

| Subsystem | Safety Responsibilities |
| ------ | ------ |
| Control | Monitors machine conditions, processes safety inputs, implements low-level firmware interlocks and local abort states, and manages fault responses. |
| Power | Provides electrical protection, energized status indication, and hardware capable of disconnecting hazardous power via the E-stop safety circuit. |
| Software | Displays warnings, stale data, and faults, monitors communication links, manages global abort states, and provides GUI-based shutdown and interlock-override requests. |

The machine will have two manual emergency stops: a GUI-based E-stop, and a large physical E-stop with a latching mechanism, which interrupts the flow of hazardous power.

## 6. Subsystem Interfaces

The table below documents interfaces between subsystems.

| Interface | Description |
| ------ | ------ |
| Control → Software | Sends machine measurements, operating status, and fault information. |
| Control ↔ Machine Equipment | Interfaces with sensors, actuators, and motor-control signals. |
| Control ↔ Power | Co-manages the hardwired E-stop safety relay loop, contactor coils, and motor-inhibit signals. |
| Power → Control | Supplies electrical power to control hardware and connected devices. |
| Power → Software | Supplies electrical power to computing and interface hardware. |
| Power → Machine Equipment | Supplies electrical power to motors and other electrical loads. |
| Software → Control | Sends high-level operating commands, heartbeat signals, and shutdown requests. |

Specifics will be established during M3.

## 7. Open Design Questions

The following items require further investigation and development:

*   Final selection of control hardware and computing hardware.
*   Communication interface and protocol (ICD) between Control and Software.
*   Detailed power distribution and branch protection.
*   Motor-control hardware and implementation.
*   Sensor interfacing and signal conditioning.
*   Physical E-stop and independent hardware safety architecture (joint Power/Control interface).
*   Power requirements for each subsystem.
*   Verification of subsystem interfaces and system-level operation.

These decisions will be developed and documented as the individual subsystems progress.

## 8. Related Documentation

*   [Requirements documentation](requirements.md)
*   [Control subsystem documentation](control.md)
*   [Power subsystem documentation](power.md)
*   [Software subsystem documentation](software.md)