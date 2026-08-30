# AGENTS.md — Repository Execution Rules

## Mission

Build a safe, local-first, solar-assisted retrofit actuator for continuous-loop beaded-chain roller shades, beginning with one measured reference fixture and expanding only through retained evidence.

This repository is allowed to contain long-range design pressure. Implementation work remains bounded by one current slice.

## Authority order

When instructions conflict, use this order:

1. applicable safety requirements and explicit operator direction;
2. `AGENTS.md`;
3. `CURRENT_SLICE.md`;
4. `ARCHITECTURE.md`;
5. accepted records under `docs/decisions/`;
6. the exact relevant governing dossier;
7. `PROJECT_STATUS.md`;
8. research notes and component candidates;
9. issue or pull-request scope;
10. implementation convenience.

Stop and surface a conflict. Do not quietly reconcile two incompatible authorities.

## Required reading before editing

Read exactly:

1. `AGENTS.md`;
2. `CURRENT_SLICE.md`;
3. `ARCHITECTURE.md`.

Then read only the dossier, decision record, research source, or evidence record explicitly required by the work.

Do not hand a coding agent the whole design corpus and ask it to “build the project.” The dossiers are technical-lead grounding, not an unbounded implementation prompt.

## Fact discipline

Every important technical statement belongs to one of these classes:

- **Observed:** directly measured or photographed on an identified fixture.
- **Published:** stated by an identified manufacturer, standards body, or government source.
- **Derived:** calculated from explicit inputs and equations.
- **Decided:** an operator-approved product or architecture choice.
- **Hypothesized:** plausible but unproved.
- **Unknown:** not yet determined.

Do not promote a published maximum into an observed operating value. Do not promote a calculation into field evidence. Do not convert an inference from a photograph into fixture geometry.

When reality disagrees with design, record:

```text
governing intent
observed implementation or fixture fact
difference
decision required
```

Do not rewrite evidence to preserve the design.

## Core invariants

### Safety

- A network command never directly energizes the motor.
- Exactly one embedded owner may authorize motor drive.
- Hardware or firmware safety limits outrank schedules, remote commands, and convenience.
- The motor must never remain energized indefinitely.
- A fault, watchdog expiry, brownout, boot ambiguity, or corrupted command must result in motor disable.
- Abnormal current, implausible encoder motion, overtemperature, undervoltage, or travel timeout must stop motion and retain a fault.
- Endpoint stall must not be the ordinary positioning method.
- A local physical stop action must interrupt motion without dependence on Wi-Fi, MQTT, the homelab, or a cloud service.
- The beaded loop must remain constrained by an anchored tension/guard mechanism. Do not create a loose loop.
- Sprockets, idlers, and chain pinch points must be enclosed during unattended operation.
- Manual recovery must not require defeating battery protection or exposing an energized mechanism.
- A lithium cell must be protected, thermally observed, kept out of direct sun, and isolated from unsafe charging temperatures.
- No public claim of child safety, product certification, winter autonomy, universal compatibility, or unattended readiness may be made without its exact evidence.

### Ownership

- Mechanical geometry owns transmission facts: bead pitch, sprocket fit, tension, mount, and release.
- Power and energy owns panel, charger, battery, rail, thermal, and energy-permission facts.
- Device control and safety owns motion state, motor enable, position estimate, calibration, interlocks, and fault latch.
- Network and fleet code owns transport, discovery, desired-state delivery, telemetry publication, and update orchestration. It does not own motor PWM, endpoint truth, or safety limits.
- One fact has one committing owner. Other components consume it; they do not reconstruct it from convenience data.
- Homelab availability is not a prerequisite for local buttons, local schedules, safe stop, or retained calibration.
- The device may expose uncertainty. It must not report a precise position when position confidence has been lost.

### Power

- Solar sizing is based on harvested watt-hours at the exact installation, not panel nameplate wattage alone.
- A power-path charger may let the panel carry part of a motor burst; the battery remains the burst reservoir unless measurements prove otherwise.
- Always-on radio consumption must be included in the energy budget.
- Charging and motion policies must define behavior at low energy, high temperature, and weak input.
- A panel, battery, charger, and motor combination is not qualified from datasheets alone.

### Networking and software

- The first useful system is local-first.
- Commands are desired-state intents with identity, freshness, and bounded lifetime—not unversioned fire-and-forget motor instructions.
- Duplicate delivery must not produce duplicate motion.
- Retained MQTT state must not cause stale movement after reboot.
- OTA updates require integrity checking, rollback or recoverability, adequate energy, and a motion-idle state before they can be called safe.
- Credentials, Wi-Fi secrets, home addresses, hostnames, serial numbers, and private network details do not belong in the repository.

## Slice discipline

Every implementation or experimental slice must state:

- exact basis commit;
- fixture identity and assumptions;
- one primary claim;
- owner and changed-path envelope;
- in-scope work;
- explicit non-goals;
- measurements or tests required;
- retained evidence;
- acceptance criteria;
- failure and rollback behavior;
- what successor work remains unauthorized.

A slice is complete only when its claim is demonstrably true.

Do not merge uncertainty domains merely because they interact. In particular:

- shade-force characterization is separate from final motor selection;
- chain engagement is separate from endpoint calibration;
- a bench motor test is separate from an installed unattended device;
- battery burst capability is separate from seasonal solar neutrality;
- Wi-Fi/MQTT control is separate from Zigbee, Thread, or Matter;
- one-window proof is separate from fleet commissioning;
- a development-board prototype is separate from a custom PCB;
- position repeatability is separate from child-safety or certification claims;
- a visually acceptable enclosure is separate from thermal qualification;
- successful opening is separate from long-duration cycle reliability.

## Evidence requirements

Retain useful, sanitized evidence under `evidence/`.

Each evidence package must identify:

- date;
- repository commit;
- fixture ID;
- hardware revisions and component part numbers;
- firmware/software revision where applicable;
- instruments and their relevant limitations;
- environmental state;
- procedure;
- raw observations;
- derived calculations;
- pass, fail, or blocked result;
- what the evidence does **not** prove.

Preferred evidence includes:

- dimension and bead-pitch measurements;
- force-versus-position observations;
- motor voltage/current/encoder traces;
- solar voltage/current/energy logs;
- battery voltage, temperature, and recovery curves;
- movement-time and position-repeatability runs;
- jam, slip, brownout, reboot, and homelab-outage tests;
- photographs of guarded mechanics and mounting;
- sanitized MQTT and device logs;
- BOM snapshots and substitution records;
- long-duration trial summaries.

Do not commit:

- uncropped photographs that expose identifiable home or neighborhood details;
- credentials or device certificates;
- account identifiers;
- personal network topology;
- exact geolocation;
- unsafe wiring shown without a prominent test-state warning;
- proprietary third-party documents or binaries without redistribution rights.

## Engineering preferences

- Characterize before optimizing.
- Prefer a deliberately over-capable, current-observable first motor over a beautiful underpowered mechanism.
- Prefer a brushed DC gearmotor with encoder for the first proof unless fixture measurements establish a better choice.
- Prefer hardware current limiting plus firmware supervision.
- Prefer replaceable, parametric sprocket cassettes over one guessed chain geometry.
- Prefer reversible mounts and serviceable fasteners over permanent adhesive-only installation.
- Prefer low-quiescent-current power design and sleep-capable communications.
- Prefer local schedules in nonvolatile device state.
- Prefer explicit state machines and typed fault causes over scattered booleans.
- Prefer versioned contracts and bounded queues.
- Prefer measurements from the exact window over generic solar calculators.
- Prefer a USB-C service path and physical controls during development.
- Prefer one real vertical route over speculative fleet infrastructure.

## Required review questions

A technical review must ask:

1. What exact claim became true?
2. Which fixture and revisions prove it?
3. Which owner commits each new fact?
4. Can any remote or stale state bypass safety?
5. Is resource use bounded?
6. Does failure disable motion?
7. Are derived values traceable to raw measurements?
8. Were negative and fault paths exercised?
9. Did the slice accidentally make a prototype fact universal?
10. Is any public-facing claim stronger than the retained evidence?

## Stop conditions

Stop for operator or technical-lead input when:

- the current slice conflicts with `ARCHITECTURE.md`;
- a product decision is missing;
- one fact would have two committing owners;
- safe chain tension or guarding cannot be preserved;
- battery discharge, charge, or thermal limits are unknown for the intended test;
- the proposed mount could release under chain load;
- the motor/driver combination can exceed known safe current without an independent limit;
- a required source or fixture cannot be inspected;
- a requested claim exceeds available evidence;
- work would require secrets or personal location data in the public repository;
- three repair attempts fail against the same acceptance requirement.

## Pull-request posture

Use ordinary branches, commits, pull requests, review, and CI.

A useful adjacent idea does not widen the active slice. Record it in an issue, risk register, or decision proposal.

Leave a pull request open unless the operator explicitly requests or approves merge. Initial repository bootstrap is the exception because it establishes the governance surface itself.
