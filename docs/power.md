# Power

## Scope

Power is responsible for:

- 480 VAC machine power
- 120 VAC support power
- 24 VDC control power
- DC/DC conversion
- motor-control hardware
- electrical protection
- grounding and bonding
- power wiring

## Current Architecture

```text
480 VAC, 3-phase
      |
      v
Protection / Disconnect
      |
      v
Motor Control
      |
      v
Main Motor


120 VAC
   |
   v
24 VDC Power Supply
   |
   v
24 VDC Distribution
   |
   +--> Safety Hardware
   +--> Sensors
   +--> Relays / Contactor Coils
   +--> Interface Electronics
   +--> DC/DC Conversion
   |
   v
5 VDC Power supply
```

Current decisions:

- 480 VAC is used for the main high-power motor path.
- 120 VAC is used primarily for control and support power.
- 24 VDC is the main control and instrumentation distribution voltage.
- Exact motor-control hardware has not yet been selected.

## Electrical Loads
480 VAC, 3-phase motor loads
24 VDC control electronics (Teensy, Raspberry Pi)
24 VDC sensors
5 VDC sensors
24 VDC contactor coils
24 VDC relay coils


## M2 Design Work

- confirm the motor electrical requirements
- determine whether variable speed or reverse operation is required
- select the motor-control method
- create the 24 VDC load budget
- confirm whether the existing 24 VDC supply is sufficient
- define 24 VDC branch distribution
- select branch protection
- define required DC/DC conversion
- define grounding and bonding
- define cable and connector requirements
- develop the preliminary electrical layout

## Open Questions

- What are the final motor voltage, current, and startup requirements?
- Should the motor use a contactor, soft starter, or VFD? Is variable speed required? Is reverse required?
- Is the EDR-120-24 enough?
- What protection is required on each power branch?
- What lower voltages are required by the selected controllers?
- Where will the electrical hardware be physically mounted?
