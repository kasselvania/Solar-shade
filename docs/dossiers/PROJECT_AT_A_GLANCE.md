# Solar Shade — Project at a Glance

> **A safe, locally autonomous, energy-observable retrofit for manual continuous-loop roller shades.**

## Product pressure

Many roller shades already have a usable mechanical interface: a continuous loop of beaded chain.

The obvious automation path is to place a small motor on that loop. The difficult product is not the motor. It is the complete boundary around the motor:

- the exact force and chain geometry vary;
- a chain loop is a safety-critical object;
- windows provide inconsistent light and high heat;
- a motor needs burst power while a panel produces variable trickle power;
- an embedded radio can consume more daily energy than the motor;
- position can drift if the chain skips or is moved manually;
- a homelab can be unavailable;
- a household fleet must remain serviceable;
- a portfolio claim must be supported by evidence rather than a single successful demonstration.

Solar Shade treats those pressures as the project, not as cleanup after a motor spins.

## Intended product

Each shade receives one reversible lower-loop actuator containing or connecting:

- a guarded sprocket matched to the exact bead pitch;
- a tension and mounting system;
- a brushed encoder gearmotor;
- a current-observable motor driver;
- an ESP32-C6-class controller;
- a protected single-cell battery;
- a solar-aware power path;
- local buttons and service charging;
- local-first networking and telemetry.

The actuator should normally charge from a small panel mounted at the same window. The battery supplies motor bursts and carries the device through weak-light periods.

## Five peer concerns

| Concern | Owns | Must not own |
|---|---|---|
| Mechanical interface | Chain fit, tension, guarding, mount, release, transmission | Remote policy or battery safety |
| Power and energy | Panel, charger, battery, rails, temperature, energy balance | Shade position intent |
| Device control and safety | Motor enable, state machine, position confidence, fault stop, local schedule | Homelab UI policy |
| Network and fleet | Desired-state transport, discovery, telemetry, update orchestration | PWM, endpoints, safety bypass |
| Verification | Fixture identity, procedures, raw evidence, acceptance | Runtime control |

The embedded control-and-safety owner is the only component allowed to energize the motor.

## First executable route

```text
one measured shade
        ↓
one measured chain and load envelope
        ↓
one guarded current-limited bench actuator
        ↓
one closed-loop local motion state machine
        ↓
one protected battery and measured power path
        ↓
one window-mounted panel and energy ledger
        ↓
one local-first homelab integration
        ↓
one long-duration installed field trial
```

Each crossing is earned separately.

## Intended user experience

### Installation

1. Identify chain size with a gauge or measurement.
2. Select the matching sprocket cassette.
3. Attach the actuator at the anchored lower-loop location.
4. Thread the chain and close the guard.
5. Connect the panel.
6. Enter commissioning mode.
7. Move slowly to safe endpoints and store travel.
8. Verify local up, stop, and down controls.
9. Assign room and window identity in the local homelab.

Installation should not require opening the shade headrail or cloud registration.

### Daily use

The shade:

- follows local schedules;
- accepts local physical control immediately;
- reports battery, energy, position confidence, and faults;
- rejects motion when energy or temperature is unsafe;
- continues ordinary local operation when the homelab is down;
- reconciles safely when network control returns.

### Service

The user can:

- stop movement physically;
- disengage or service the chain without dismantling the shade;
- use USB-C fallback charging;
- recalibrate after manual movement or service;
- replace the battery, motor, panel, or sprocket as documented modules;
- inspect fault evidence rather than guessing why movement failed.

## Energy thesis

The panel does not need to carry the motor alone.

A correct power-path design lets available panel power support the system while the battery supplies the deficit. The important quantity is daily watt-hours:

```text
daily harvested energy
-
sleep + radio + sensing + movement + conversion losses
=
daily energy balance
```

Motor movement is brief. Poor radio sleep, weak window collection, charger behavior, and hot-battery policy are likely to decide success.

## Mechanical thesis

The first actuator should be selected from measured chain pull, not aesthetics.

A deliberately large encoder gearmotor may be appropriate for the first proof. The final product can become smaller only after current, torque, speed, wear, and slip are known.

A replaceable parametric sprocket is more valuable than guessing one universal chain wheel.

## Safety thesis

The project fails if automation creates a larger or looser chain hazard.

The mechanism must preserve or replace the anchored tension function, guard pinch points, disable the motor on fault, avoid routine hard stalls, and provide local stop and manual recovery.

Battery and charging safety are equally first-class. Window heat is part of the design environment.

## Local-first thesis

Remote control is convenience, not safety authority.

The device owns:

- calibration;
- current and timeout limits;
- position confidence;
- local schedules;
- fault state;
- motor enable.

The homelab owns:

- desired state;
- grouping;
- presentation;
- history;
- automation policy;
- fleet updates.

## Portfolio value

A mature project can demonstrate:

- requirements derived from physical measurement;
- mechanism and parametric CAD;
- motor and power-electronics selection;
- low-power embedded design;
- state-machine and fault engineering;
- MQTT/Home Assistant integration;
- telemetry and time-series analysis;
- custom PCB development;
- repeatable fixtures and test plans;
- field energy and reliability evidence;
- honest treatment of unknowns and failed approaches.

The portfolio artifact should let another engineer audit how every major design choice was earned.

## Current posture

The repository is in S0 characterization.

No motor, panel, battery, chain size, or final communication protocol is yet qualified for the reference shade.

## Hopeful final evidence

The desired final reference-unit packet includes:

- exact installed CAD and BOM;
- schematic and PCB sources;
- signed or reproducible firmware build;
- local homelab deployment;
- commissioning and recovery documentation;
- at least 1,000 retained movements;
- zero uncontrolled stalls or loose-loop events;
- quantified position repeatability;
- quantified acoustic behavior;
- quantified harvested and consumed energy;
- a defined winter no-routine-charge field period;
- brownout, jam, network-loss, and update-recovery results;
- compatibility and noncompatibility envelope;
- public-safe photographs and dashboards;
- a clear list of remaining certification and production gaps.

That is the north star, not current completion.
