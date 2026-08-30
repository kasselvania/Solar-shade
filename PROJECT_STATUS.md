# Project Status

**Updated:** 2026-08-29  
**Repository posture:** pre-prototype  
**Active slice:** [S0 — Reference Shade Characterization and Window-Energy Baseline](CURRENT_SLICE.md)

## Current product statement

Solar Shade is intended to be a reversible, local-first, solar-assisted actuator for existing continuous-loop beaded-chain roller shades.

The project is not yet an installed automation system. Its first job is to replace guesswork with exact fixture measurements.

## Directly observed starting point

From the initiating reference photograph:

- the shade is operated by a continuous beaded loop;
- the chain runs down the right side of the window;
- a lower white component appears to constrain or tension the loop;
- there is a narrow wall/window-return region near the chain that may support an actuator;
- the window receives daylight but also has nearby building and foliage obstruction;
- the battery and electronics should not be placed directly against hot glass or in direct panel sunlight.

These are visual observations only. The photograph does not prove bead pitch, chain material, force, mount strength, sun yield, temperature, or compatibility with any commercial sprocket.

The full photograph is not committed to this public repository because it includes unnecessary home and exterior context.

## Published feasibility basis

Current official and commercial sources establish that the concept is technically plausible:

- ESP32-C6 supports low-power operation and Wi-Fi/Bluetooth/802.15.4 capabilities suitable for a local embedded endpoint.
- Power-path lithium chargers exist that can let available input support the system while the battery supplies load deficits.
- Small nominal 6 V panels in the roughly 1–2.5 W class are commercially available.
- Small brushed encoder gearmotors and current-observable H-bridges exist in the needed development range.
- Commercial retrofit products already move beaded-chain shades from rechargeable batteries and offer solar accessories.
- Government safety guidance treats continuous-loop cords and bead chains as a strangulation hazard unless constrained by an appropriate tensioning or restraining device.

See [`docs/research/SOURCE_REGISTER.md`](docs/research/SOURCE_REGISTER.md).

Published feasibility is not fixture proof.

## Derived planning basis

The current sizing model suggests:

- motor energy per complete open-and-close cycle may be on the order of hundredths to low tenths of a watt-hour;
- a protected 1S 2,500 mAh lithium pack has roughly 9.25 Wh nominal energy before reserve and conversion losses;
- a nominal 2.37 W panel needs only minutes of full-power-equivalent daily collection to replace a 0.1–0.25 Wh daily load in an idealized calculation;
- the true energy risk is likely to be weak-window collection plus radio and regulator idle consumption, not motor energy alone;
- a power-path system can reduce battery burst current when sunlight is available but does not eliminate the battery as the main transient reservoir;
- a 10 F supercapacitor is too small to replace the battery for an ordinary full movement.

These values are planning calculations, not measured performance. See [`docs/research/FEASIBILITY_AND_SIZING_MODEL.md`](docs/research/FEASIBILITY_AND_SIZING_MODEL.md).

## Current component posture

### First-proof candidates

| Function | Candidate posture |
|---|---|
| Controller | ESP32-C6 development board; exact board deferred |
| Panel | Voltaic P126 2.37 W / 6 V-class panel as a conservative first window panel |
| Prototype charger | BQ24074-based power-path board |
| Future custom charger | TI BQ25620-class switch-mode charger |
| First motor | Pololu 99:1 25D low-power 6 V encoder gearmotor to remove torque uncertainty |
| Future compact motor | Smaller encoder gearmotor only after force/current proof |
| Future custom driver | TI DRV8213-class current-sensing H-bridge |
| Battery | Protected 1S lithium pack around 2,500 mAh, exact pack and discharge rating unresolved |
| Network | Wi-Fi/MQTT first; Zigbee/Thread later if energy and responsiveness justify it |

No candidate is yet selected for installation.

See [`docs/research/COMPONENT_CANDIDATES.md`](docs/research/COMPONENT_CANDIDATES.md).

## Known unknowns

### Mechanical

- exact bead diameter and pitch;
- chain connector geometry;
- pull force by direction and shade position;
- total chain displacement;
- chain wear and slip tolerance;
- existing clutch behavior;
- acceptable noise and movement time;
- mount material and structural fastener options;
- long-term printed-part creep near a window;
- manual-release design.

### Power and thermal

- real panel watt-hours at the exact window;
- worst-season and worst-week collection;
- battery temperature near the window;
- motor current and energy on the reference shade;
- sleep, wake, Wi-Fi, MQTT, and telemetry energy;
- charger weak-light behavior behind the glazing;
- service-charging and play-while-charging transitions;
- exact battery pack discharge, protection, and temperature limits.

### Control

- endpoint strategy;
- position-confidence model after power loss or manual movement;
- best slip/jam signature;
- commissioning interaction;
- command latency target;
- local schedule semantics;
- fault reset policy;
- OTA recovery strategy.

### Product and fleet

- number and diversity of target shades;
- desired household grouping and automation policy;
- acceptable per-window BOM;
- preferred aesthetics;
- intended openness and license;
- whether the final radio is Wi-Fi, Zigbee, Thread, or a combination;
- long-term maintenance expectations.

## Current risks

The highest risks are:

1. designing around a guessed chain pitch;
2. selecting a compact motor before measuring force;
3. treating panel nameplate power as window-harvested energy;
4. allowing always-on Wi-Fi to dominate the budget;
5. removing the existing tensioner without replacing its safety function;
6. using endpoint stall as normal operation;
7. mounting a battery where window heat accelerates aging or creates unsafe charge conditions;
8. building remote control before local safety and manual recovery;
9. creating a beautiful enclosure before the mechanical load path is proven;
10. publishing stronger safety or autonomy claims than the evidence supports.

The full register is in [`docs/dossiers/PREMORTEM_RISK_AND_UNKNOWN_REGISTER.md`](docs/dossiers/PREMORTEM_RISK_AND_UNKNOWN_REGISTER.md).

## Current cost model

A deliberately over-capable one-off proving unit is estimated at roughly **$140–190**, including:

- panel;
- power-path charger;
- protected battery;
- encoder gearmotor;
- development controller;
- motor driver;
- sensing, regulation, connectors, and controls;
- printed mechanism, guard, and mounting hardware.

The expensive development motor and breakout boards are expected to dominate the first unit.

A later custom low-volume target around **$40–85 per window** is plausible only after motor, panel, PCB, battery, mechanics, and production sources are qualified. It is not a quote or current BOM.

## Current evidence posture

No retained S0 fixture evidence exists yet.

The repository contains:

- governing design;
- a research and source basis;
- sizing equations;
- candidate components;
- a measurement worksheet;
- a current slice that defines the first legitimate proof.

## First actionable work

Execute S0:

1. assign fixture ID `SS-FX-001`;
2. measure bead geometry;
3. measure force and travel;
4. document tensioner and mount;
5. gather initial panel-location and thermal evidence;
6. calculate torque, speed, and energy from the real values;
7. select the guarded, current-limited S1 bench route.

## Success horizon

The hopeful final state is a reference unit that:

- completes at least 1,000 commanded movements without chain skip or uncontrolled stall;
- retains position within a defined error envelope;
- survives homelab and network outages without losing local schedule or safety;
- demonstrates a defined winter field period without routine manual charging;
- retains energy, current, temperature, motion, and fault evidence;
- remains manually serviceable;
- keeps the continuous loop anchored and guarded;
- provides reusable CAD, electronics, firmware, homelab integration, and validation documentation.

None of those horizon claims is current status.
