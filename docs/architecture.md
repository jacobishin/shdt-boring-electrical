# Electrical System Architecture

## 1. Purpose

This document describes the high-level electrical architecture of the boring machine design. It defines the three main electrical subsystems and their areas of responsibility. The purpose is to establish the overall system structure, so detailed design work is located in the specific subsystem files

## 2. System Overview

The electrical system is divided into three subsystems:

- **Control** manages the machine's physical inputs and outputs, processes sensor information, and coordinates machine operation.
- **Power** supplies and distributes electrical energy to the machine's equipment and electrical systems.
- **Software** provides the operator interface, communication, monitoring, and high-level machine functions.

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
|---|---|
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

The Control subsystem manages the machine's physical hardware and coordinates its operation. Essentially, it is responsible for translating commands into machine-level actions. The central processing with be an an MCU, namely, a Teensy 4.1.

Its responsibilities include:

- Interfacing with sensors and electrical hardware.
- Acquiring and processing machine inputs.
- Managing machine states and operating conditions.
- Controlling physical outputs and equipment.
- Implementing local interlocks and fault responses.
- Communicating machine status and information to Software.

More information in [control](control.md)


### 4.2 Power

The Power subsystem supplies and distributes electrical energy throughout the machine.

Its responsibilities include:

- Receiving and distributing incoming electrical power.
- Providing appropriate voltage levels to electrical equipment.
- Supplying power to motors and other high-power devices.
- Supporting motor control and operation.
- Providing electrical protection and power isolation.
- Supplying electrical power to Control and Software hardware.

More information in [power](power.md)

### 4.3 Software

The Software subsystem provides the high-level software functions required to operate and monitor the machine. It will run on a Raspberry Pi, or an equivalent high-level computer. 

Its responsibilities include:

- Providing the graphical user interface (GUI).
- Allowing operators to interact with the machine.
- Sending high-level operating commands to Control.
- Displaying machine measurements and status information.
- Handling communication between system components.
- Logging machine data and reporting faults.
- Providing software-based shutdown requests.

More information in [software](software.md)

## 5. Safety

Safety is a responsibility and consideration of all three subsystems. The safety responsibilities of each subsystem are documented below:

| Subsystem | Safety Responsibilities |
|---|---|
| Control | Monitors machine conditions, processes safety inputs, implements interlocks, and manages fault responses. |
| Power | Provides electrical protection and hardware capable of disconnecting hazardous power. |
| Software | Displays warnings and faults, monitors machine status, and provides GUI-based shutdown requests. |

The machine will have two manual emergency stops: a GUI based E-stop, and a large physical E-stop with a latching mechanism, which interupts the flow of power. 

## 6. Subsystem Interfaces

The table below documents interfaces between subsystems. 

| Interface | Description |
|---|---|

| Control → Software | Sends machine measurements, operating status, and fault information. |
| Control ↔ Machine Equipment | Interfaces with sensors, actuators, and motor-control hardware. |
| Power → Control | Supplies electrical power to control hardware and connected devices. |
| Power → Software | Supplies electrical power to computing and interface hardware. |
| Power → Machine Equipment | Supplies electrical power to motors and other electrical loads. |
| Software → Control | Sends operating commands and shutdown requests. |

Specifics will be established during M3

## 7. Open Design Questions

The following items require further investigation and development:

- Final selection of control hardware and computing hardware.
- Communication interface between Control and Software.
- Detailed power distribution and branch protection.
- Motor-control hardware and implementation.
- Sensor interfacing and signal conditioning.
- Physical E-stop and independent hardware safety architecture.
- Power requirements for each subsystem.
- Verification of subsystem interfaces and system-level operation.

These decisions will be developed and documented as the individual subsystems progress.

## 8. Related Documentation

- [Requirements documentation](requirements.md)
- [Control subsystem documentation](control.md)
- [Power subsystem documentation](power.md)
- [Software subsystem documentation](software.md)
