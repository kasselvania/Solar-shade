# Verification, Field Trial, and Portfolio Governing Design

## 1. Purpose

Solar Shade is simultaneously:

- a useful home project;
- an embedded and electromechanical engineering exercise;
- a public portfolio proof.

Those goals reinforce one another only when the portfolio is built from retained engineering evidence. A polished video without measurements is marketing. A pile of logs without a coherent product experience is not a product.

The governing outcome is:

> A technically literate reviewer can trace the final behavior from user intent through mechanical load, power flow, embedded decisions, network contracts, and field evidence, including where the system abstains or fails.

## 2. Verification philosophy

### 2.1 Claims are envelopes

A claim should name:

- fixture;
- hardware revision;
- firmware revision;
- environmental range;
- command/load range;
- duration or cycle count;
- pass criterion;
- excluded cases.

Example:

> Revision P2 on fixture SS-FX-001 completed 1,000 full-travel movements at 20–27 °C with zero chain skips, zero uncontrolled stalls, and maximum verified endpoint error of 1.5% travel.

This is more useful than “reliable.”

### 2.2 Failure is retained

A failed test can improve the project if it establishes:

- a real limit;
- a reproducible signature;
- a rejected component;
- a safer threshold;
- a changed mechanism;
- an honest compatibility boundary.

Do not erase failed prototypes from the narrative. Distinguish them from the current design.

### 2.3 Bench proof is not field proof

Bench proof may establish:

- electrical correctness;
- state logic;
- motor current;
- encoder behavior;
- a guarded mechanical route.

Field proof adds:

- real mount;
- real window heat;
- real solar exposure;
- repeated household use;
- network interruptions;
- dust, wear, accidental handling;
- visual and acoustic acceptance.

Certification remains a separate future activity.

## 3. Fixture identity

Each shade installation receives an immutable fixture ID.

Recommended fields:

```yaml
fixture_id: SS-FX-001
shade_type: continuous-loop roller shade
shade_manufacturer: unknown
shade_model: unknown
chain_material: unknown
bead_diameter_mm: null
bead_pitch_mm: null
chain_connector: unknown
shade_width_mm: null
shade_travel_mm: null
mount_substrate: unknown
window_orientation: private/not-public
created_at: 2026-08-29
```

Private location information may exist outside the repository. Public evidence uses only the environmental details needed for the test.

## 4. Revision identity

### Mechanical revision

Retain:

- mount revision;
- sprocket revision;
- idler/tension revision;
- guard revision;
- motor/gearbox;
- fastener and material specification.

### Electrical revision

Retain:

- schematic/PCB or wiring revision;
- panel;
- charger;
- battery;
- regulator;
- driver;
- current/voltage/temperature sensing;
- protection and fuse;
- wire/connector details.

### Firmware revision

Retain:

- commit;
- build environment;
- SDK/toolchain;
- configuration profile;
- schema versions;
- update method.

### Homelab revision

Retain:

- broker;
- integration/service version;
- contract version;
- dashboard/config revision;
- database schema where relevant.

## 5. Characterization tests

### 5.1 Chain geometry

Pass when:

- repeated bead diameter readings agree inside instrument and manufacturing variation;
- pitch over multiple beads is unambiguous;
- a fit coupon runs without binding or excessive play;
- connector passage is proven or excluded.

Retain:

- caliper photos where public-safe;
- raw measurements;
- calculation;
- coupon revision;
- fit result.

### 5.2 Force

Measure:

- breakaway;
- steady;
- peak;
- up and down;
- top, middle, bottom;
- multiple repeats.

Retain distributions, not only maxima.

### 5.3 Mount load

The mount test should eventually apply a defined multiple of measured operational load for a defined duration and cycle count.

It must consider:

- pull direction;
- reaction torque;
- heat;
- creep;
- fastener loosening;
- accidental side load.

Do not improvise a destructive test on window glass or trim without understanding the substrate.

### 5.4 Solar and thermal

Retain:

- panel location and orientation;
- sample interval;
- voltage/current/watt-hours;
- weather class without precise geolocation;
- battery/enclosure/glass-adjacent temperatures;
- shade position;
- obstruction notes;
- missing samples.

A panel passes only for a defined energy budget and reserve policy.

## 6. Electrical and power tests

### 6.1 Bench supply bring-up

Before battery use:

- set a conservative voltage;
- set a hardware current limit;
- verify driver-disabled boot;
- verify local stop;
- verify current measurement;
- verify encoder direction;
- verify motor suppression and MCU stability;
- exercise a bounded movement.

### 6.2 Motor characterization

For each representative movement retain:

- rail voltage;
- current trace;
- peak current;
- average current;
- encoder counts;
- movement time;
- direction;
- target;
- completion state;
- motor/driver temperature;
- chain observation.

Build a normal signature envelope before setting jam thresholds.

### 6.3 Battery qualification

Before installed use:

- verify pack provenance and protection;
- verify discharge capability;
- measure voltage sag on worst movement;
- verify protection does not nuisance-trip;
- test charger termination;
- test thermal cutoff;
- test low-energy refusal;
- test service charging;
- test reconnect transitions;
- inspect physical temperature and swelling clearance.

Do not intentionally defeat a protection board to test beyond pack ratings.

### 6.4 Power-path test

Exercise:

- panel/input only where permitted;
- battery only;
- input plus idle load;
- input plus motor;
- weak input collapse;
- input insertion/removal;
- service USB insertion/removal;
- low battery;
- charge inhibit by temperature.

Retain how much current the input supplies and how much the battery supplements.

### 6.5 Energy accounting

Compare:

- integrated input watt-hours;
- battery state/charge estimates;
- system-load estimates;
- motor energy;
- standby energy.

The purpose is not laboratory-grade coulomb accuracy on revision one. The purpose is to detect whether the model is directionally honest and whether unaccounted standby dominates.

## 7. Embedded control tests

### 7.1 State transition suite

Cover every allowed and rejected transition.

Examples:

- uncommissioned + remote open → rejected;
- idle + valid local open → moving;
- moving + local stop → stopped;
- moving + duplicate command → nonduplicating;
- moving + stale opposite command → rejected;
- moving + overcurrent → fault latched;
- boot after interrupted movement → position uncertain;
- faulted + remote retry → rejected;
- low energy + schedule → deferred or rejected by defined policy.

### 7.2 Reset safety

Inject:

- MCU reset;
- watchdog;
- brownout;
- power removal;
- driver communication failure.

Verify:

- motor disables;
- no boot movement;
- reset cause retained;
- position confidence updated;
- stale command not replayed.

### 7.3 Sensor fault

Disconnect or corrupt:

- encoder channel;
- current sensor;
- temperature sensor;
- battery voltage sense.

The system should prefer safe refusal or bounded degraded mode, never invented confidence.

### 7.4 Timing and bounds

Measure:

- local stop latency;
- fault-stop latency;
- maximum motor-on time;
- command queue bound;
- network retry bound;
- nonvolatile-write rate;
- boot-to-safe-idle time.

## 8. Network and homelab tests

### 8.1 Contract conformance

Validate:

- schema version;
- device ID;
- command ID;
- expiry;
- duplicate handling;
- invalid position;
- malformed payload;
- unauthorized source;
- state revision;
- fault representation.

### 8.2 Outage matrix

Test:

- access point unavailable;
- broker unavailable;
- DNS unavailable if used;
- homelab restart;
- retained stale desired state;
- device reconnect;
- two controllers issue conflicting state;
- time unavailable;
- time jumps.

Local stop and local schedules must remain independent.

### 8.3 Home Assistant

A cover entity should correctly represent:

- opening;
- closing;
- stopped;
- open/closed;
- intermediate position;
- unavailable;
- faulted or diagnostic state;
- unknown position.

Home Assistant presentation must not hide uncertainty merely because the cover UI expects a percentage.

### 8.4 Fleet tests

Later fleet qualification should include:

- provisioning multiple devices;
- duplicate names;
- credential rotation;
- firmware cohorts;
- bounded update concurrency;
- one offline device;
- one low-energy device;
- one faulted device;
- configuration rollback.

## 9. Mechanical negative tests

Safely induce or simulate:

- chain connector entering sprocket;
- too-low tension;
- too-high tension;
- bead mismatch;
- partial guard opening;
- blocked shade;
- manual movement;
- mount loosening;
- idler misalignment;
- chain contamination;
- worn printed sprocket.

The objective is to establish early-warning signatures and maintenance rules, not to damage the shade.

## 10. Field-trial levels

### F0 — supervised installed checkout

- one fixture;
- operator present;
- local control;
- limited travel;
- no autonomous schedules;
- immediate physical stop.

### F1 — supervised daily operation

- full travel;
- local schedules;
- remote desired state;
- daily inspection;
- battery/thermal logging;
- guard and mount verified.

### F2 — normal household operation

- defined unattended periods;
- fault notifications;
- weekly inspection;
- no routine manual charging during a defined interval;
- homelab outages allowed.

### F3 — seasonal energy trial

- includes weak-light period;
- daily energy balance;
- reserve minima;
- intervention count;
- panel obstruction events;
- battery temperature.

### F4 — compatibility trial

- multiple shade/chain/window fixtures;
- explicit supported envelope;
- failed or unsupported cases retained.

No level implies certification.

## 11. Proposed acceptance targets

Targets guide design but do not become claims until approved and proven.

### Motion

- position command range: 0–100%;
- full-travel time: operator-selected after noise testing, likely 20–60 seconds;
- endpoint error: target ≤2% travel after ordinary cycles;
- local stop latency: target <250 ms from button recognition to driver disable;
- no indefinite drive;
- no chain skip in qualified operation.

### Reliability

- initial endurance target: 1,000 full or equivalent movements;
- no uncontrolled stall;
- no mount release;
- no guard release;
- no corrupted calibration;
- every aborted movement has a retained cause.

### Energy

- field target: no routine manual charge over a defined seasonal trial;
- reserve target: operator-approved minimum after worst measured week;
- movement and standby energy separately observable;
- thermal charging limits obeyed.

### Local-first behavior

- local schedule persists through homelab outage;
- physical stop works offline;
- stale retained command cannot initiate unexpected motion after reboot;
- network restoration reconciles to one current desired state.

### Maintainability

- sprocket and battery are replaceable without removing the shade;
- service does not require cloud access;
- exact revision and fault are visible;
- calibration can be repeated;
- common failure parts have documented replacement.

## 12. Portfolio structure

A strong public project page should eventually show:

### Problem

- manual beaded shade;
- no convenient power wiring;
- desire for local automation;
- chain and battery safety pressure.

### Engineering method

- source research;
- fixture measurement;
- calculations;
- candidate tradeoffs;
- bounded slices;
- negative tests;
- retained evidence.

### System

- mechanical cutaway;
- power-path diagram;
- controller state diagram;
- MQTT contract;
- homelab dashboard;
- component and ownership boundaries.

### Results

- movement trace;
- energy graph;
- current signature;
- position-repeatability plot;
- thermal plot;
- field-trial summary;
- fault-injection demonstration.

### Honesty

- failed designs;
- remaining unknowns;
- unsupported shade types;
- certification gap;
- current BOM and sourcing volatility;
- exact claims and nonclaims.

## 13. Artifacts expected by maturity

- parametric CAD and drawings;
- sprocket fit gauges;
- assembly and safety checklist;
- schematic and PCB;
- firmware source and reproducible build;
- unit and hardware-in-loop tests;
- MQTT or alternate protocol schema;
- Home Assistant integration;
- deployment files;
- test fixtures and scripts;
- component register;
- BOM with dated pricing;
- evidence index;
- field-trial report;
- portfolio case study.

## 14. Pre-publication review

Before publishing a result, ask:

1. Does the claim name the fixture and revision?
2. Are measurements and derived values distinguishable?
3. Is the safety boundary visible?
4. Are failed cases included?
5. Does any image expose private home context?
6. Are credentials, serials, or network details present?
7. Are third-party assets redistributable?
8. Is a prototype being described as a product?
9. Is an energy-neutral statement supported by an appropriate season?
10. Can another engineer reproduce the procedure?

## 15. Final hopeful proof

The desired portfolio culmination is a public, auditable packet showing one installed unit through:

```text
measured shade
→ selected mechanism
→ bounded motor and power path
→ local safe controller
→ local-first homelab route
→ solar energy ledger
→ fault and outage behavior
→ long-duration field evidence
```

The most convincing moment should not be the shade moving. It should be that every part of the movement is explainable, bounded, and supported by evidence.
