# Current Slice: S0 — Reference Shade Characterization and Window-Energy Baseline

## Status

```text
status: READY_TO_START
work_classification: measurement and architecture-basis proof
implementation_authority: observational tooling and disposable test fixtures only
unattended_installed_operation: prohibited
```

## Primary claim

Establish a retained, reproducible characterization of one exact reference shade and one candidate solar-panel location sufficient to select the first motor, sprocket geometry, mount concept, panel class, battery class, charger architecture, and guarded bench-test envelope without relying on visual inference or generic assumptions.

S0 does **not** prove an automated shade. It proves that the first automated-shade prototype can be chosen from measured requirements.

## Reference fixture

Create a fixture ID:

```text
SS-FX-001
```

The fixture is the continuous-loop beaded-chain roller shade shown in the initiating project discussion.

The original room/window photograph is not automatically committed to this public repository. Create cropped evidence that shows only the chain, current tensioner, mounting surfaces, shade travel, and panel exposure needed for the technical claim.

## Exact grounding

Before recording evidence, retain:

- repository commit;
- this current-slice blob;
- `ARCHITECTURE.md` blob;
- instrument list;
- measurement date;
- fixture ID;
- a sanitized fixture overview;
- known shade manufacturer/model only if visible or documented;
- whether the current chain tensioner is manufacturer-installed or later-added, if known.

Do not infer manufacturer, chain size, wall material, fastener strength, or shade mass from the initiating photograph.

## First missing facts

The project currently lacks exact answers to:

```text
bead diameter
bead center-to-center pitch
connector-link geometry
chain-loop and usable mounting geometry
total chain travel for full shade travel
peak and running pull force by direction and position
desired movement speed and acceptable noise
existing endpoint/clutch behavior
candidate mount material and fastener path
candidate panel's harvested energy at the exact window
window-side temperature exposure
```

Until those facts exist, final motor, gearbox, sprocket, battery, and panel choices remain hypotheses.

## In scope

### A. Mechanical identification

Measure and retain:

1. bead diameter at no fewer than five beads;
2. center-to-center pitch over at least ten consecutive bead intervals;
3. chain connector or splice dimensions and how often it traverses the lower loop;
4. current tensioner geometry and attachment;
5. free loop geometry with the tensioner installed;
6. available actuator mounting width, height, depth, and fastener surfaces;
7. clearances to window, trim, blind travel, furnishings, and controls;
8. shade visible width and travel height;
9. total chain displacement from fully open to fully closed;
10. direction convention: which chain leg raises and lowers;
11. manual full-travel time at a comfortable speed;
12. any binding, clutch slip, uneven load, or abrupt endpoint behavior.

Use a caliper for bead geometry where available. A ruler photograph alone is insufficient for final sprocket pitch.

### B. Pull-force characterization

Using a spring scale, force gauge, or suitable luggage scale with a controlled adapter:

1. measure breakaway force;
2. measure slow steady pull force;
3. measure peak force;
4. repeat in both directions;
5. sample near top, middle, and bottom travel;
6. repeat each condition at least three times;
7. record whether force is applied to the active chain leg while the loop remains tensioned;
8. record the pulling speed and scale resolution.

Do not jerk the chain. Do not allow the loop to become unconstrained.

Retain raw readings, not only an average.

### C. Torque and speed envelope

From the measured pitch and candidate sprocket pocket count:

\[
r = \frac{p}{2\sin(\pi/N)}
\]

where:

- \(r\) is sprocket pitch radius;
- \(p\) is measured bead pitch;
- \(N\) is pocket count.

Calculate:

\[
\tau_{load} = F_{peak} \times r
\]

Then define and justify:

- minimum starting torque;
- target operating torque;
- design margin;
- maximum acceptable drive current;
- target output RPM;
- expected movement time;
- candidate motor/gearbox envelope.

A motor datasheet's extrapolated stall torque is not available continuous torque. The target operating point should remain comfortably below stall and must be confirmed by current and temperature testing in a later slice.

### D. Panel-location characterization

For the exact intended panel location, retain:

- panel orientation;
- distance and thermal separation from glass;
- partial shading from frame, foliage, buildings, or the shade itself;
- whether the shade blocks the panel in any position;
- daylight exposure observations over the day;
- window and candidate enclosure temperature during at least one bright period;
- candidate panel open-circuit voltage;
- loaded voltage/current through a characterized load or charger;
- accumulated watt-hours when suitable logging hardware is available.

Preferred baseline:

- at least seven consecutive days;
- multiple weather conditions;
- sample interval and logger accuracy stated;
- panel temperature or nearby temperature recorded when practical.

A one-day result may unblock a bench prototype, but it cannot prove seasonal energy neutrality.

### E. Existing safety function

Document:

- how the current tensioner anchors the loop;
- what free-loop hazard would exist if it were removed;
- whether a prototype can replace its mounting location;
- the guard volume required around a driven sprocket;
- a reversible manual-release concept;
- the likely mount reaction direction;
- whether the mount can use structural fasteners rather than adhesive alone.

The S1 bench mechanism must either preserve the existing tensioner or replace its function continuously. There is no authorized state in which the chain is left as a loose loop.

### F. Candidate selection packet

At S0 closeout, produce a decision packet identifying:

- first sprocket geometry and manufacturing method;
- first motor/gearbox;
- first current-observable H-bridge;
- first bench power supply limits;
- first panel and charger architecture;
- first battery class, if battery testing is authorized next;
- first mounting/guard concept;
- rejected alternatives and reasons;
- exact nonclaims.

The packet may select deliberately oversized development components. It must distinguish first-proof components from final-form candidates.

## Allowed tooling

Examples include:

- digital caliper;
- tape measure;
- spring scale or force gauge;
- stopwatch;
- multimeter;
- current-limited bench supply;
- thermometer or temperature logger;
- solar voltage/current/energy logger;
- camera with sanitized crop;
- spreadsheet or script for derived calculations;
- disposable non-motorized chain gauges or sprocket-fit coupons.

No custom PCB, autonomous firmware, or unattended actuator is authorized in S0.

## Required evidence layout

Retain under:

```text
evidence/s0-reference-shade/
  README.md
  fixture/
    dimensions.csv
    bead-measurements.csv
    sanitized-fixture-overview.*
    tensioner-and-mount.*
  force/
    raw-force-readings.csv
    procedure.md
    summary.md
  travel/
    chain-travel.csv
    timing.md
  solar/
    logger-config.md
    raw-samples.*
    summary.md
  thermal/
    observations.csv
  calculations/
    torque-and-speed.md
    energy-baseline.md
  decision/
    s0-closeout.md
```

Binary evidence may be stored in efficient formats, but every package needs a short human-readable description and nonclaims.

## Acceptance criteria

S0 is complete only when all of the following are true:

1. The exact bead diameter and pitch are measured with enough repeatability to create a fit coupon or sprocket.
2. Connector-link passage is characterized or explicitly excluded from the first sprocket route.
3. Total chain displacement and desired full-travel time are known.
4. Breakaway, running, and peak pull-force ranges are retained for both directions and multiple shade positions.
5. A torque and speed calculation uses the actual measured fixture inputs and units.
6. At least one motor/gearbox candidate fits the calculated envelope with explicit current, torque, speed, and noncontinuous-stall caveats.
7. The actuator mounting envelope and reaction direction are documented.
8. The existing loop-safety function is documented and the next mechanism has a credible anchored and guarded concept.
9. The candidate panel location has at least a bounded measured input result; weak-light and seasonal unknowns remain explicit.
10. Window-side temperature risk is observed sufficiently to place the battery away from direct sun and hot glass in the next design.
11. A first-proof component and test-safety packet is ready for S1.
12. No claim of installed automation, battery autonomy, winter neutrality, child safety, or universal compatibility is made.

## Failure and blocker handling

Close S0 as `BLOCKED`, not “mostly complete,” if:

- chain geometry cannot be measured reliably;
- force measurement would require an unsafe loose loop;
- the mount has no credible anchored load path;
- the chain or shade is already damaged or binding;
- the panel location cannot be measured;
- battery or motor testing is proposed before safe current and thermal limits are known.

A blocked result is useful. It should identify the exact missing instrument, fixture access, replacement part, or product decision.

## Explicit non-goals

- no motorized installed device;
- no unattended movement;
- no final enclosure;
- no custom PCB;
- no lithium pack integration;
- no Wi-Fi, MQTT, Zigbee, Thread, or Matter implementation;
- no Home Assistant dashboard;
- no OTA updates;
- no production BOM;
- no fleet architecture implementation;
- no universal sprocket library;
- no winter solar-neutral claim;
- no child-safe or certified-product claim;
- no permanent alteration of the shade or window.

## Expected S1 handoff

S1 should be able to authorize a route equivalent to:

```text
current-limited bench supply
→ hardware-limited current-observable driver
→ selected encoder gearmotor
→ guarded test sprocket and tension fixture
→ reference shade
→ retained current, encoder, time, slip, and fault evidence
```

S1 should prove bounded, supervised, reversible motion on the reference fixture. Battery, solar charging, homelab control, and unattended field operation remain later work.

## Review standard

Do not close S0 from internet research or the initiating photograph.

The claim is about the exact reference shade and window. Real measurements and sanitized retained evidence are mandatory.
