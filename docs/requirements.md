# Electrical Requirements

## Competition Requirements
These requirements are directly imposed by the NABC 2027 rule package

RULE:
- Vertical bore area >= 0.2 m²
- Bore depth >= 1 m
- Remote control from the surface
- Relevant telemetry >= 0.1 Hz
- At least one latching E-stop near the Operator Station
- E-stop must interrupt machine power near the source
- Restart after E-stop requires two actions
- Machine energized indication required
- State diagrams required
- LOTO required
- Maximum machine power draw is 100 kW
- TBC provides 480 VAC, 3-phase, 60 Hz and 120 VAC circuits
- Telemetry GUI required
- IO list required
- Critical IO must be monitored and interlocked
- Power loss must leave the machine in an E-stopped state.
- Remote control, telemetry, state diagrams, critical IO monitoring, and safe behavior after power or network loss are required

## Machine Requirements
These requirements are a byproduct of the high-level machine design

- Must power and control the digging motor
- Must support safe stopping of powered hazardous motion
- Must support remote operator control
- Must acquire and report required telemetry

Still need to determine:

- whether cutter speed control is required
- whether reversing is required
- what sensors are required
- exact motor-control requirements
- exact safety interlocks