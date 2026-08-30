# Architecture

This is the short governing architecture for active implementation.

Long-form dossiers describe the intended product and the pressure that future work must survive. They do not authorize all future modules at once. `CURRENT_SLICE.md` authorizes one bounded proof.

## 1. Product boundary

Solar Shade is a retrofit system for an existing continuous-loop beaded-chain roller shade.

The product may own:

- chain engagement through a reversible guarded actuator;
- local motion control and position estimation;
- panel, charger, battery, and device power behavior;
- local buttons, local schedules, and fault handling;
- local-network desired-state control and telemetry;
- fixture evidence, energy accounting, and maintenance observations.

The product does not own:

- the internal shade clutch, tube, fabric, or manufacturer warranty;
- household electrical wiring;
- a cloud account as a prerequisite for ordinary use;
- a claim that every shade or window is compatible;
- safety certification before certification work exists;
- exact room comfort, lighting, or HVAC policy unless a later application layer provides it.

## 2. Peer ownership domains

### Mechanical and shade interface

Owns:

- bead diameter and pitch compatibility;
- sprocket geometry and chain engagement;
- idlers, tension, guard, mount, shaft coupling, and manual release;
- physical travel conversion;
- service access and enclosure fit;
- evidence of slip, wear, retention, and mounting strength.

Must not own:

- remote command semantics;
- battery charge permission;
- position truth derived without controller evidence;
- firmware safety thresholds.

### Power and energy

Owns:

- solar input;
- charging and power-path behavior;
- battery qualification and protection;
- regulated rails;
- voltage, current, temperature, and harvested/consumed energy observations;
- permitted operation under low energy or unsafe temperature;
- service charging.

Must not own:

- desired shade position;
- network schedules;
- encoder position;
- mechanical endpoint meaning.

### Device control and safety

Owns:

- the sole motor-enable authority;
- bounded motor drive requests;
- motion state;
- command freshness and idempotency;
- calibration;
- encoder-derived position estimate and confidence;
- current, timeout, temperature, and implausible-motion interlocks;
- local buttons and local schedules;
- fault latching and recovery policy;
- nonvolatile device state.

Must not own:

- homelab user-interface policy;
- panel nameplate interpretation;
- sprocket geometry as an unmeasured constant;
- fleet identity outside its own exact device identity.

### Network and fleet adapter

Owns:

- commissioning transport;
- local-network discovery;
- versioned desired-state messages;
- Home Assistant or MQTT adaptation;
- telemetry and event publication;
- fleet inventory;
- update distribution and orchestration;
- presentation of device-reported state and confidence.

Must not own:

- PWM or driver pins;
- endpoint determination;
- safety-limit overrides;
- reconstruction of position from command history;
- assumption that network availability equals device availability.

### Evidence and verification

Owns:

- fixture identity;
- measurement procedures;
- raw and derived evidence;
- compatibility matrices;
- acceptance summaries;
- retained failure observations.

It has no runtime authority.

## 3. Controlled crossings

### Desired-state crossing

```text
user or schedule intent
        ↓
network adapter validates schema, identity, freshness
        ↓
device receives one desired-state command
        ↓
device control validates local state and safety permission
        ↓
bounded motion or explicit rejection
```

A desired-state command may request `0–100%`, `OPEN`, `CLOSE`, or `STOP`. It never requests raw direction, voltage, duty cycle, current limit, or endpoint bypass.

### Power crossing

```text
solar panel + service input + battery
        ↓
power-path and protection
        ↓
system rail and measured energy state
        ↓
power owner publishes permitted operating envelope
        ↓
device control may request bounded motor operation
```

The battery may supplement the panel during motion. “Solar powered” does not imply that instantaneous motor current comes only from the panel.

### Motion-observation crossing

```text
driver current + encoder motion + elapsed time + voltage + temperature
        ↓
device control state machine
        ↓
position estimate, confidence, completion, or fault
        ↓
network telemetry and retained evidence
```

The homelab displays device truth. It does not recreate it from expected movement.

## 4. Device states

The first complete controller should model at least:

```text
UNCOMMISSIONED
IDLE
MOVING_OPEN
MOVING_CLOSED
CALIBRATING
STOPPED_LOCAL
LOW_ENERGY
SERVICE
FAULT_LATCHED
```

Boot is a transition procedure, not permission to move.

A transition into motion requires:

- valid commissioned geometry;
- valid and fresh command or local schedule;
- no fault latch;
- acceptable battery/rail state;
- acceptable battery and controller temperature;
- driver available;
- motor timeout configured;
- position estimate or explicitly authorized calibration behavior;
- guard and service interlock state where implemented.

Any contradictory observation leads to stop or abstention, not optimistic continuation.

## 5. Command precedence

Highest to lowest:

1. hardware protection and motor-driver shutdown;
2. local physical stop or service interlock;
3. embedded safety state and fault latch;
4. local explicit command;
5. locally stored schedule;
6. fresh remote desired state;
7. fleet automation policy.

A stale retained MQTT message must not outrank a recent local stop. Command IDs, revisions, expiry, and acknowledged device state are required before remote control can be considered reliable.

## 6. Position law

Position is an estimated physical fact owned by the device controller.

The controller may use:

- encoder counts;
- calibrated travel;
- known direction;
- current and speed signatures;
- endpoint sensors if later selected;
- slip or missed-motion detection.

It must retain position confidence and invalidate or degrade confidence after:

- manual chain movement;
- encoder inconsistency;
- unexpected current without motion;
- motion without expected counts;
- power loss during movement;
- chain skip;
- mechanical service;
- calibration change.

A device with lost confidence may expose `UNKNOWN` and request recalibration. It must not silently report a fabricated percentage.

## 7. Failure behavior

All ordinary failures converge on motor disable.

| Failure | Required immediate behavior | Retained result |
|---|---|---|
| Overcurrent or stall signature | Disable drive | Fault cause, peak current, position, elapsed time |
| Encoder not moving | Disable drive | No-motion fault |
| Encoder implausibly fast/reversed | Disable drive | Encoder/plausibility fault |
| Movement timeout | Disable drive | Timeout fault |
| Battery undervoltage | Disable or refuse start | Low-energy state |
| Charge/battery overtemperature | Stop charging; block motion if required | Thermal event |
| MCU watchdog/brownout | Hardware-safe driver disable | Reset cause and uncertain position |
| Network loss | Continue local safety and schedules | Offline availability |
| Homelab restart | No unsolicited stale movement | Reconciliation event |
| OTA failure | Roll back or remain serviceable | Update fault |
| Position uncertainty | Stop and report unknown | Recalibration required |

A retry loop may not repeatedly hit a jam. Fault recovery must require a bounded local or explicitly authorized action.

## 8. Mechanical safety boundary

The actuator must preserve the safety function of the existing anchored loop or replace it with a demonstrably safe anchored and guarded assembly.

The installed mechanism must eventually provide:

- positive wall/frame attachment sized from measured chain load;
- controlled chain tension;
- adequate sprocket wrap;
- connector-link passage or an explicit incompatibility;
- no reachable sprocket pinch point in ordinary use;
- no enlarged free loop;
- no sharp edge against chain or user;
- manual release or service procedure;
- bounded reaction torque;
- evidence that fasteners, printed parts, and adhesive choices tolerate heat and repeated load.

An open bench mechanism is not an installed product state.

## 9. Power architecture

The intended first architecture is:

```text
nominal 6–7 V solar panel
        ↓
solar-tolerant power-path charger
        ↔ protected 1S lithium battery
        ↓
system rail
        ├── current-observable H-bridge → brushed encoder gearmotor
        └── low-quiescent 3.3 V rail → ESP32-C6 and sensors
```

Prototype power-path and motor-driver breakout boards are allowed. A custom PCB is not authorized until the measured route is stable.

The design should include:

- battery protection;
- battery thermistor;
- charger temperature qualification;
- input and battery voltage measurement;
- useful current or energy measurement;
- bulk and high-frequency motor decoupling;
- motor EMI suppression;
- driver current limit independent of normal firmware control;
- reverse and transient protection appropriate to the selected parts;
- a USB-C service input with an unambiguous power path.

## 10. Communication architecture

The first useful remote route is expected to be local Wi-Fi and MQTT because it is inspectable and integrates with common homelab software.

The controller must sleep or otherwise meet a measured energy budget. Always-connected Wi-Fi is not assumed acceptable.

ESP32-C6 keeps later Zigbee or Thread evaluation possible. Matter is not a prerequisite for first proof.

The canonical device contract is versioned separately under `docs/contracts/`. Transport adapters may evolve without moving motor safety out of the device.

## 11. Dependency direction

The intended logical direction is:

```text
shared device identities and bounded value types
        ↓
power observations + mechanical calibration
        ↓
device control and safety
        ↓
network adapters and application integrations
        ↓
homelab presentation and fleet policy
```

Network or UI code may not be imported into the motor-safety core.

Mechanical and power owners provide exact values; the control owner consumes them through small interfaces. Circular authority is prohibited.

## 12. Engineering method

Architecture is hardened through executable verticals:

1. measure one fixture;
2. prove one guarded bench motion route;
3. prove closed-loop repeatability and fault stop;
4. add battery and measured power-path behavior;
5. add installed solar collection and energy accounting;
6. add local-first network control;
7. field-test one window;
8. generalize through additional fixtures;
9. only then reduce size, cost, and commissioning friction.

Future capabilities do not receive production-sized abstractions before their first real proof.

## 13. Active governance

Active implementation is governed by:

- this file;
- `CURRENT_SLICE.md`;
- accepted ADRs;
- ordinary code, schematic, CAD, tests, and retained evidence.

Long-form dossiers inform human decisions. Research candidates do not become dependencies merely because they appear in a table.
