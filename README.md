# Queen's Hyperloop Boring Electrical

Electrical system development for the Queen's Hyperloop Boring Team.

## Current Status

Current milestone: **M1 - System Definition**

The current focus is defining the high-level electrical architecture and identifying the main design work for M2.

See GitHub Issues and Milestones for active work.

## Work Areas

### Power

Responsible for:

- 480 VAC, 120 VAC, and 24 VDC power
- motor-control hardware
- DC/DC conversion
- electrical protection
- grounding and bonding
- power wiring

See [Power](docs/power.md).

### Control

Responsible for:

- sensors and machine IO
- low-level control requirements
- machine states
- interlocks
- faults
- E-stop and hardware safety
- motor enable and inhibit behavior

See [Control](docs/control.md).

### Software

Responsible for:

- high-level computer software
- low-level controller firmware/software
- controller communication
- GUI
- telemetry
- logging
- networking

See [Software](docs/software.md).

## System Architecture

The current design uses:

- 480 VAC for the main high-power motor path
- 120 VAC for control and support power
- a 24 VDC control and instrumentation bus
- separate high-level and low-level control layers
- a hardware safety path independent of normal control software

Exact controller hardware and motor-control hardware are still TBD.

See [Electrical Architecture](docs/architecture.md).

## Repository Structure

```text
boring-electrical/
├── README.md
├── CONTRIBUTING.md
│
├── docs/
│   ├── requirements.md
│   ├── architecture.md
│   ├── power.md
│   ├── control.md
│   ├── software.md
│   └── decisions/
│       ├── power-distribution.md
│       └── ...
│
├── hardware/
│   └── kicad/
│
├── firmware/
│   └── controller/
│
├── software/
│   └── high-level-computer/
│
└── .github/
    ├── ISSUE_TEMPLATE/
    │   └── task.md
    └── pull_request_template.md
```

## Key Documents

- [Electrical Requirements](docs/requirements.md)
- [Electrical Architecture](docs/architecture.md)
- [Power](docs/power.md)
- [Control](docs/control.md)
- [Software](docs/software.md)


## Workflow

1. Pick an assigned GitHub Issue.
2. Create a branch from `main`.
3. Make your changes.
4. Commit and push your branch.
5. Open a Pull Request.
6. Ask another electrical member/manager for review.
7. Merge after review.

## Questions

If you're not sure about something, feel free to ask. 
