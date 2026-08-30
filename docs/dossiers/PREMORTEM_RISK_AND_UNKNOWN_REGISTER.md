# Pre-mortem, Risk, and Unknown Register

## 1. Purpose

Assume the project has failed eighteen months from now.

The shades move unreliably, batteries need manual charging, one mechanism has chewed a chain, the homelab shows false positions, the enclosure is ugly and hot, and the public portfolio overstates what was actually proven.

This register asks what likely caused that outcome, what early signal would have appeared, and what proof must exist before the risk is treated as controlled.

A risk is not closed because a mitigation was written. It is reduced only by implementation and evidence.

## 2. Severity and status

- **Critical:** could injure a person, damage property, or create an unsafe battery/chain condition.
- **High:** could invalidate the product or require major redesign.
- **Medium:** could cause recurring failure, maintenance, or portfolio weakness.
- **Low:** limited inconvenience or polish issue.

Statuses:

- `OPEN`
- `MEASURING`
- `MITIGATING`
- `EVIDENCE_REQUIRED`
- `CONTROLLED_FOR_DEFINED_USE`
- `ACCEPTED`
- `REJECTED_DESIGN`

## 3. Primary pre-mortem table

| ID | Failure story | Severity | Early warning | Governing mitigation | Proof gate |
|---|---|---:|---|---|---|
| R-001 | The sprocket was designed from a guessed chain size and skips or jams. | High | Beads sit loosely, connector catches, inconsistent counts | Measure diameter/pitch; print fit coupons; retain connector test; cassette design | Repeated full chain traversal with zero skip and documented fit envelope |
| R-002 | The final compact motor cannot start the shade in a high-load position. | High | Slow start, high current, driver trips, manual force varies widely | Measure breakaway/peak force; use oversized first motor; select from operating margin, not stall headline | Worst-position starts at qualified voltage/temp with current and thermal margin |
| R-003 | The oversized first motor makes the unit noisy, bulky, and expensive, and the project never optimizes. | Medium | Prototype works but cannot fit an acceptable enclosure | Treat development motor as instrument; retain real load/current; authorize a separate optimization slice | Compact candidate reproduces qualified route and fault behavior |
| R-004 | The chain loop becomes looser or less constrained than the original tensioner. | Critical | Guard opens, chain can be pulled away, loop size increases | Actuator replaces tension function continuously; positive anchor; captive guard; service interlock | Installation review and load test show no accessible loose-loop state in ordinary use |
| R-005 | A child or hand reaches the sprocket or idler pinch point. | Critical | Open mechanism needed for ordinary operation; large finger gaps | Fully enclosed drive path; tool or deliberate service access; motor disable on guard/service state | Physical inspection against defined probe/access criteria; no motion with guard open |
| R-006 | Endpoint detection uses hard stall and damages the shade clutch or chain over time. | High | Current spike every cycle; increasing noise; endpoint drift | Calibrated travel; slow supervised calibration; stop before ordinary stall; optional redundant reference | Endurance trace shows endpoint stop without repeated hard-stall signature |
| R-007 | Chain slips occasionally, while software continues to report a precise position. | High | Encoder counts look correct but physical shade drifts | Position confidence; current/speed plausibility; periodic reference; visible `UNKNOWN`; recalibration | Injected skip/manual movement invalidates or corrects position within defined bound |
| R-008 | A mount releases because trim, adhesive, or printed plastic creeps in window heat. | Critical | Movement, cracks, adhesive edge lift, changing alignment/current | Structural fasteners into known substrate; heat/load/cycle tests; conservative printed material | Qualified mount survives defined static multiple, cycles, and temperature without movement |
| R-009 | The battery overheats in sun or charges outside its safe temperature range. | Critical | Cell/enclosure temperature rises near glass; charger lacks NTC | Battery shaded and separated; cell thermistor; charge inhibit; known protected pack | Bright-window thermal test and forced thermal cutoff verify safe behavior |
| R-010 | The selected pack cannot supply motor burst current and trips protection or sags into reset. | High | Voltage collapse, BMS trip, MCU brownout, driver fault | Exact discharge rating; bench sag test; bulk decoupling; low-voltage policy; oversized first pack | Worst movement at low allowed state of charge completes without unsafe sag/reset |
| R-011 | The panel nameplate looked adequate, but glazing, foliage, orientation, and winter reduce harvest below load. | High | Battery slowly trends downward despite daylight | Log watt-hours at exact location; conservative reserve; larger panel; lower radio budget; USB-C fallback | Defined seasonal trial meets reserve target without routine charging |
| R-012 | The linear prototype charger wastes too much panel power or collapses in weak light. | Medium | Panel voltage oscillates/collapses; low stored energy despite irradiance | Characterize charger input behavior; compare switch-mode candidate; tune input limit | Side-by-side or modeled evidence justifies retained topology |
| R-013 | Always-on Wi-Fi consumes more daily energy than shade movement. | High | Daily battery loss with no movement; radio dominates current trace | Local schedule; sleep; batched telemetry; measured reconnect policy; later sleepy 802.15.4 option | Whole-day current integration meets budget under realistic signal/outage conditions |
| R-014 | Motor EMI resets the ESP32 or corrupts encoder readings. | High | Resets only during starts/stops; impossible encoder spikes | Suppression, layout, decoupling, ground control, filtered inputs, reset logging | Repeated worst-current starts with no reset and bounded encoder noise |
| R-015 | A stale retained MQTT command moves a shade unexpectedly after reboot. | Critical | Reconnect triggers motion without fresh user action | Desired-state revision, expiry, reconciliation, local-stop precedence, no raw movement commands | Reboot/outage matrix proves stale command cannot create unexpected motion |
| R-016 | Homelab failure makes the shade unusable. | High | Local buttons or schedules depend on broker acknowledgement | Device-local control, schedules, time policy, calibration, and safety | Broker/network outage tests preserve local operation and stop |
| R-017 | Duplicate or conflicting commands cause repeated or oscillating movement. | High | Back-to-back reversals, repeated starts, command queue growth | Idempotent command IDs; one desired-state owner; bounded queue; conflict policy | Duplicate/conflict tests produce one bounded reconciliation |
| R-018 | An OTA update bricks devices mounted around the home. | High | Single image, no recovery, update starts at low battery | Signed/integrity-checked image; rollback/recovery; idle/energy gate; staged fleet rollout | Interrupted-update tests recover without motor movement or data loss |
| R-019 | Manual chain operation destroys calibration without being detected. | Medium | Reported position disagrees after service or power loss | Manual release state; motion plausibility; absolute/reference option; explicit uncertainty | Manual-movement test causes defined confidence loss and recovery |
| R-020 | The chain connector cannot pass through the compact enclosure. | High | Connector strikes sprocket or guard once per loop | Measure connector; relief path; larger path; reposition connector if lawful; incompatibility label | Connector completes repeated passages in both directions |
| R-021 | The printed sprocket wears, cracks, or deforms at window temperature. | High | Rising debris, noise, backlash, current, visible deformation | Material selection; stress/heat tests; replaceable wear item; inspect current trend | Endurance and thermal aging meet defined revision envelope |
| R-022 | The mechanism is technically functional but aesthetically unacceptable in a living space. | Medium | Large box, visible wires, loud motion, awkward panel | Product-experience review after proof; narrow panel/wire routes; enclosure and acoustic slice | Installed review meets explicit size/noise/appearance targets |
| R-023 | Commissioning is fragile and requires developer tools. | Medium | Endpoint setup fails, direction confusion, no clear fault reason | Local guided flow; safe jog; clear indication; recoverable reset; app only as enhancement | Nondeveloper can commission from documented steps and recover a deliberate error |
| R-024 | The household fleet becomes a maintenance burden. | High | Unique configs, mystery batteries, manual firmware, inconsistent names | Stable IDs; inventory; revision telemetry; replaceable modules; staged updates; common parts | Multi-device trial demonstrates provisioning, fault isolation, update, and replacement |
| R-025 | A component disappears and the design is coupled to its exact breakout board. | Medium | Out-of-stock candidate blocks all progress | Define electrical/mechanical requirements; abstraction at interfaces; second-source review | Substitute part passes same owner contract and acceptance suite |
| R-026 | Telemetry is trusted even though sensors are uncalibrated or drifted. | Medium | Energy balance impossible; impossible battery state | State sensor accuracy/limits; calibration procedure; plausibility checks; raw values retained | Comparison to reference instrument inside defined tolerance |
| R-027 | Battery state of charge is inferred from voltage during motor load and is misleading. | Medium | SOC jumps during/after movement | Load-aware estimation; rest-voltage context; coulomb/energy sensing later if justified | SOC/reserve policy remains conservative across load/recovery test |
| R-028 | The project claims “solar powered” while routine USB charging is still needed. | Medium | Manual interventions omitted from public results | Define solar-neutral acceptance; log every service charge and energy intervention | Seasonal report includes complete intervention count |
| R-029 | The public repo exposes home, family, network, or device-credential information. | High | Full room photos, SSIDs, IPs, serials in logs | Public evidence sanitation checklist; cropped images; secret scanning; private raw archive | Pre-publication review finds no prohibited information |
| R-030 | The portfolio becomes a dossier project and never produces hardware. | High | Increasing prose without measurements or current slice closure | One active slice; evidence requirements; no future scaffolding; operator review | S0 closes with real measurements, followed by executable verticals |
| R-031 | The portfolio becomes a demo project and loses design/evidence discipline. | High | One successful video replaces negative tests and logs | Claim envelopes; retained failures; acceptance matrix; exact revisions | Public case study links reproducible evidence and nonclaims |
| R-032 | Safety language is interpreted as certification. | Critical | “Safe” or “child-safe” used without standard/testing | Explicit prototype disclaimer; defined safety controls; certification gap retained | Publication review removes unsupported certification language |
| R-033 | Temperature, current, or fault thresholds are copied from example code rather than qualified hardware. | High | Round-number thresholds with no source; nuisance trips or unsafe range | Derive from exact components and fixture evidence; record margin and units | Threshold register traces every limit to source and test |
| R-034 | Low energy causes repeated reboot/move/reboot cycling. | High | Motor starts, rail collapses, restarts, stale command replays | Preflight voltage/energy gate; brownout-safe driver; startup lockout; command freshness | Low-battery test produces refusal, not repeated partial movement |
| R-035 | Remote stop is assumed sufficient during a physical jam. | Critical | No reachable local stop or power isolation | Physical stop/service control; hardware current limit; motor timeout | Operator can halt test without network under worst credible fault |
| R-036 | The panel wire or connector becomes the least reliable part of the installation. | Medium | Intermittent charging, snagging, repeated flex, connector heating | Strain relief; keyed connector; protected routing; replaceable cable; input diagnostics | Installed movement/window cycles do not alter connection; fault is detectable |
| R-037 | Solar and battery optimization begins before mechanical behavior is stable. | Medium | Energy data changes every mechanism revision | Stage work: characterize and prove motion first; retain revision identity | Power qualification uses frozen mechanical route |
| R-038 | Fleet or Matter work consumes the project before one local unit is reliable. | High | Large service stack, no endurance-tested actuator | Wi-Fi/MQTT only after local controller proof; later transports separate | One installed local unit passes field gate before fleet expansion |
| R-039 | “Universal” sprocket support multiplies uncontrolled compatibility cases. | Medium | Many untested CAD variants and no fixture matrix | Support exact measured families; version cassettes; explicit unsupported cases | Each published cassette has fit and load evidence |
| R-040 | The shade itself is worn or defective and the actuator is blamed or causes more damage. | High | Manual binding, clutch slip, asymmetric force, damaged chain | Pre-install manual health check; reject out-of-envelope fixtures | Fixture passes manual baseline before actuation |

## 4. Risk clusters

### 4.1 Human and physical safety

Highest-priority risks:

- loose continuous loop;
- reachable pinch point;
- mount release;
- battery thermal event;
- stale unexpected motion;
- lack of physical stop;
- unsupported safety claims.

No schedule or portfolio deadline justifies accepting these by accident.

### 4.2 Product viability

Most likely product-killing risks:

- poor window energy;
- excessive standby radio consumption;
- unattractive/noisy mechanism;
- fragile calibration;
- difficult commissioning;
- fleet maintenance burden.

These are not secondary polish. They determine whether the device remains installed.

### 4.3 Engineering-truth risk

The project can also fail intellectually:

- measurement replaced by assumptions;
- published maxima treated as observed performance;
- successful demonstrations generalized;
- manual charging omitted;
- failures removed;
- private context published;
- documents grow while hardware remains untested.

Governance is intended to counter those failure modes.

## 5. Unknown register

| ID | Unknown | Why it matters | Earliest lawful proof |
|---|---|---|---|
| U-001 | Exact bead diameter/pitch | Determines sprocket | S0 caliper measurements |
| U-002 | Connector geometry/passage | Can jam mechanism | S0 measurement and later coupon |
| U-003 | Force envelope | Determines motor, mount, current | S0 force test |
| U-004 | Total chain travel | Determines calibration and movement time | S0 travel measurement |
| U-005 | Desired noise/speed | Determines gear ratio/PWM/enclosure | Operator decision after bench comparison |
| U-006 | Mount substrate strength | Determines safe installation | Fixture inspection and load test |
| U-007 | Panel daily watt-hours | Determines solar viability | S0+ logger at exact location |
| U-008 | Worst-season energy | Determines autonomy claim | Seasonal field trial |
| U-009 | Window/battery temperature | Determines placement and charge policy | S0 thermal observation and later logged trial |
| U-010 | Real motor energy | Determines battery/panel size | S1 current trace |
| U-011 | Real radio energy | Determines transport/sleep policy | Embedded power measurement |
| U-012 | Best endpoint strategy | Determines position robustness | S1/S2 motion evidence |
| U-013 | Slip detectability | Determines confidence model | Induced slip experiments |
| U-014 | Manual-release architecture | Determines serviceability | Mechanical prototype comparison |
| U-015 | Final battery chemistry/pack | Determines voltage, thermal, life, size | Power-path decision after load/thermal data |
| U-016 | Final radio | Determines latency, energy, fleet integration | Wi-Fi baseline followed by bounded comparison |
| U-017 | Number of target fixtures | Determines fleet pressure | Operator inventory, kept private as needed |
| U-018 | Acceptable BOM | Determines optimization threshold | Operator product decision |
| U-019 | License | Determines public reuse | Explicit operator decision |
| U-020 | Certification path | Determines productization scope | Later regulatory/safety research |

## 6. Risk review cadence

Review this register:

- before authorizing a new slice;
- after a failed test;
- after changing motor, battery, charger, mount, sprocket, or radio;
- before installed unattended operation;
- before public claims;
- before a custom PCB;
- before multi-device rollout.

A new risk may narrow the next slice. It does not automatically widen the current one.

## 7. Closure standard

To mark a risk `CONTROLLED_FOR_DEFINED_USE`, record:

- exact use envelope;
- exact revisions;
- implemented controls;
- test procedure;
- retained evidence;
- residual risk;
- monitoring/maintenance requirement;
- conditions that reopen the risk.

No risk in this initial register is currently controlled by project evidence.
