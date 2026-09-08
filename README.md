# Machine Interface

Ethernet-based hardware bridge between an autonomy computer and a piece of heavy machinery. It translates network commands into the machine's native CAN, PWM, and discrete I/O signals in real time, reports machine status back over the network, and always falls back to direct manual control if the firmware or autonomy link fails.

OSU senior capstone project, sponsor-specified requirements.

## Status

| Milestone | Covers | Status |
|---|---|---|
| M1 — Initial Firmware & PCB Design | Needs 1–13, 18, 34, 35 | TODO |
| M2 — Prototype Build & Core Firmware | Needs 14–17, 19–33 | TODO |
| M3 — Advanced Control, Status & Telemetry | Needs 36–43 | TODO |

Full task breakdown and week-by-week sequencing: see the team's task tracker (linked in `docs/`).

## Repository layout

```
machine-interface/
├── hardware/       PCB & Power — schematic, layout, fab outputs
├── firmware/       Firmware & RTOS — embedded source code
├── protocol/       Network & Protocol — command/control message schema
├── docs/           Integration & Test — specs, matrices, procedures
├── .github/        issue and pull request templates
├── .gitignore
└── LICENSE
```

### `hardware/` — PCB & Power

| Path | Contents |
|---|---|
| `hardware/schematic/` | KiCad schematic source files (`.kicad_sch`) |
| `hardware/pcb/` | KiCad PCB layout source files (`.kicad_pcb`) |
| `hardware/fab-outputs/rev-A/` | Gerbers, drill file, BOM, and pick-and-place file for board revision A. Each new revision gets its own `rev-B/`, `rev-C/`, etc. — never overwrite an old revision's outputs |
| `hardware/datasheets/` | Reference PDFs for parts used on the board (MCU, CAN transceivers, Ethernet PHY, connectors) |

### `firmware/` — Firmware & RTOS

| Path | Contents |
|---|---|
| `firmware/src/` | Application logic: control state machine, PID loops, command handlers, LED status mapping |
| `firmware/hal/` | Hardware abstraction layer — code tied to the specific MCU pinout and peripherals |
| `firmware/test/` | Unit tests and dev-kit test harnesses used before hardware exists |

Toolchain and build instructions: _TODO — fill in once the dev-kit MCU and IDE/build system are finalized (e.g. STM32CubeIDE project, or CMake + a specific toolchain)._

### `protocol/` — Network & Protocol

| Path | Contents |
|---|---|
| `protocol/schema/` | `.proto` message definitions for the UDP command/control interface (heartbeat, output command, PWM read, CAN pass-through, fault report, controller ID) |
| `protocol/sim/` | PC-side simulator that acts as a stand-in commanding controller, for testing firmware before real hardware or a real autonomy stack exists |
| `protocol/docs/` | Message format spec, port numbers, and framing rules that aren't captured in the `.proto` files themselves |

### `docs/` — Integration & Test

| Path | Contents |
|---|---|
| `docs/requirements-traceability-matrix.md` | Every sponsor need (1–43) mapped to a milestone and a verification method. Keep this current all year — it's also an M3 deliverable |
| `docs/integration-guide.md` | Connector pinouts, power requirements, and the flashing procedure |
| `docs/validation-report.md` | Final verification & validation results |
| `docs/meeting-notes/` | Notes from team and sponsor/advisor meetings |
| `docs/test/bench-rig/` | Scripts/configs for the CAN bus simulator and PWM measurement rig standing in for the real machine |
| `docs/test/procedures/` | Written test procedures for each peripheral (CAN, PWM, Ethernet, LED, I/O) |

### `.github/`

Pull request template and issue templates (bug report / feature request). Use the PR template checklist before merging anything that touches the fail-safe bypass or other safety-critical paths.

## Team ownership

| Folder | Lane |
|---|---|
| `hardware/` | PCB & Power |
| `firmware/` | Firmware & RTOS |
| `protocol/` | Network & Protocol |
| `docs/` (+ `docs/test/`) | Integration & Test |

Ownership doesn't mean exclusive access — it means that person/pair signs off on pull requests touching that folder.

## Contributing

1. Branch off `main` per task, e.g. `firmware/heartbeat-loop` or `hardware/power-rail-rev-a`.
2. Open a pull request into `main` and fill out the PR template.
3. Get a review from at least one other teammate before merging — required for anything under `hardware/` (fab-ready files) or `firmware/src/` changes touching the control state machine or fail-safe logic.

## Confidentiality

None as far as we are aware.
