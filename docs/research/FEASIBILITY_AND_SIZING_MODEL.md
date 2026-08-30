# Feasibility and Sizing Model

**Status:** research and planning basis  
**Updated:** 2026-08-29  
**Authority:** not implementation authority; real fixture evidence supersedes assumptions

## 1. Conclusion

The project appears feasible with currently available components.

The central reason is duty cycle:

- the motor may draw hundreds of milliamps to low amperes;
- it runs for tens of seconds;
- the panel may provide only tens to hundreds of milliamps;
- it can collect energy for hours.

A protected battery handles the burst. A solar-aware power path lets the panel contribute when available and replenish the daily energy afterward.

The dominant uncertainty is not whether a small panel can theoretically replace one movement. It is whether the exact window, radio policy, charger, temperature, and mechanism produce a positive real-world daily energy balance.

## 2. Model classes

Values in this document are:

- `PUBLISHED` when copied from an identified source;
- `ASSUMED` when used for a scenario;
- `DERIVED` when calculated.

No `OBSERVED` reference-fixture values exist yet.

## 3. Mechanical sizing

### 3.1 Sprocket pitch radius

For a bead pitch \(p\) and \(N\) equally spaced pockets:

\[
r = \frac{p}{2\sin(\pi/N)}
\]

Illustrative assumption:

```text
p = 6.35 mm
N = 10 pockets
```

Derived:

\[
r = 10.27\text{ mm}
\]

This is an ideal pitch radius. Pocket shape, bead diameter, connecting cord, wrap, and manufacturing tolerance still require physical coupons.

### 3.2 Required load torque

\[
\tau = F \times r
\]

Illustrative force:

```text
F = 8 N
r = 0.01027 m
```

Derived:

\[
\tau = 0.082\text{ N·m}
\]

Using \(1\text{ kg·cm} \approx 0.0981\text{ N·m}\):

\[
\tau \approx 0.84\text{ kg·cm}
\]

This value is not a motor selection by itself.

A selection must account for:

- breakaway force;
- measurement variability;
- gearbox efficiency;
- sprocket/idler losses;
- low battery voltage;
- temperature;
- acceleration;
- current limit;
- desired life;
- motor vendor's distinction between rated and extrapolated stall values.

### 3.3 Design margin

A preliminary mechanism might target at least 2× the measured peak load torque while keeping representative operation well below stall behavior.

That does **not** mean selecting a motor whose stall torque is exactly 2× load. Stall torque is an endpoint condition, not a sustainable operating rating.

The first proof should log current, speed, and temperature across:

- both directions;
- top/middle/bottom positions;
- low and normal supply voltage;
- repeated starts;
- induced obstruction.

### 3.4 Chain speed and travel time

Linear chain speed:

\[
v = RPM \times N \times p
\]

Illustrative assumptions:

```text
output speed = 41 RPM
pockets = 10
pitch = 6.35 mm
```

Derived:

\[
v = 2.60\text{ m/min}
\]

For 1.5 m chain displacement:

\[
t = \frac{1.5}{2.6035}\times60
  \approx 34.6\text{ seconds}
\]

This sits inside a plausible quiet movement range. Actual chain displacement must be measured.

## 4. Motor-energy scenarios

Motor electrical energy:

\[
E_{motor} = \frac{V \times I \times t}{3600}
\]

For one full cycle, \(t\) includes open and close durations.

### Scenario A — light fixture

```text
V = 4.0 V
I = 0.35 A average
30 s open + 30 s close
```

\[
E = 4.0 \times 0.35 \times 60 / 3600
  = 0.0233\text{ Wh}
\]

### Scenario B — medium fixture

```text
V = 4.0 V
I = 0.70 A average
35 s open + 35 s close
```

\[
E = 4.0 \times 0.70 \times 70 / 3600
  = 0.0544\text{ Wh}
\]

### Scenario C — heavy or inefficient fixture

```text
V = 4.0 V
I = 1.10 A average
50 s open + 50 s close
```

\[
E = 4.0 \times 1.10 \times 100 / 3600
  = 0.1222\text{ Wh}
\]

These scenarios omit driver, wiring, conversion, and startup losses. They are useful for order of magnitude only.

## 5. Standby and radio energy

Daily electronics energy can exceed motor energy.

Illustrative examples:

| Average whole-device standby | Daily energy |
|---:|---:|
| 0.5 mW | 0.012 Wh |
| 1 mW | 0.024 Wh |
| 5 mW | 0.120 Wh |
| 20 mW | 0.480 Wh |
| 50 mW | 1.200 Wh |

A system that averages 20–50 mW because Wi-Fi remains active can turn a comfortable solar problem into a large panel/battery problem.

The relevant average includes:

- regulator quiescent current;
- charger/system leakage;
- battery monitor;
- ESP32 sleep;
- wake and association;
- MQTT connection;
- telemetry;
- signal-strength retries;
- local timekeeping;
- sensors;
- LEDs.

The power budget must be measured at the battery/system boundary, not estimated from the ESP32 deep-sleep line alone.

## 6. Daily-load scenarios

Planning totals:

| Case | Motor cycle | Electronics + losses | Total |
|---|---:|---:|---:|
| Efficient | 0.023 Wh | 0.027–0.057 Wh | 0.05–0.08 Wh/day |
| Moderate | 0.054 Wh | 0.026–0.096 Wh | 0.08–0.15 Wh/day |
| Heavy/poorly optimized | 0.122 Wh | 0.028–0.128 Wh | 0.15–0.25 Wh/day |
| Always-on radio failure case | 0.054 Wh | 0.48–1.20 Wh | 0.53–1.25 Wh/day |

The first three cases make a small panel credible. The failure case shows why radio behavior is a first-order design concern.

## 7. Battery model

A 1S 2,500 mAh battery at nominal 3.7 V contains:

\[
E_{nominal} = 2.5 \times 3.7 = 9.25\text{ Wh}
\]

Not all nominal energy should be treated as usable.

Illustrative usable fractions:

| Usable fraction | Usable energy |
|---:|---:|
| 70% | 6.48 Wh |
| 75% | 6.94 Wh |
| 80% | 7.40 Wh |

The actual fraction depends on:

- pack cutoff;
- load voltage;
- motor sag;
- regulator dropout;
- temperature;
- aging;
- reserve policy.

### Battery-only autonomy

Using 6.94 Wh as a planning value:

| Daily load | Approximate days |
|---:|---:|
| 0.05 Wh | 139 days |
| 0.08 Wh | 87 days |
| 0.10 Wh | 69 days |
| 0.15 Wh | 46 days |
| 0.25 Wh | 28 days |
| 0.60 Wh | 12 days |
| 1.20 Wh | 6 days |

These are ideal energy divisions, not runtime guarantees.

### Motor-only equivalent cycles

At 6.94 Wh usable:

| Motor scenario | Energy/cycle | Equivalent cycles |
|---|---:|---:|
| Light | 0.0233 Wh | 298 |
| Moderate | 0.0544 Wh | 128 |
| Heavy | 0.1222 Wh | 57 |

Standby and reserve reduce real counts.

### Battery cycling pressure

If a solar-equipped device uses and replenishes 0.10–0.25 Wh per day from a 9.25 Wh nominal pack, daily depth is roughly 1–3%.

In that regime, calendar age and window heat may dominate cycle wear. That reinforces battery placement and temperature policy.

## 8. Solar model

### 8.1 Candidate panel

Published Voltaic P126 values:

```text
maximum power: 2.37 W
voltage at maximum power: 7.28 V
current at maximum power: 330 mA
open-circuit voltage: 8.51 V
dimensions: 112 × 136 × 2.7 mm
```

These are rated conditions, not behind-window results.

### 8.2 Equivalent full-power minutes

Let:

- \(E_d\) be daily load;
- \(P_p\) be panel nameplate power;
- \(\eta\) be combined collection and conversion factor.

\[
t_{hours} = \frac{E_d}{P_p \times \eta}
\]

At 2.37 W:

| Daily load | 60% useful factor | 35% useful factor |
|---:|---:|---:|
| 0.05 Wh | 2.1 min | 3.6 min |
| 0.10 Wh | 4.2 min | 7.2 min |
| 0.15 Wh | 6.3 min | 10.9 min |
| 0.25 Wh | 10.5 min | 18.1 min |
| 0.60 Wh | 25.3 min | 43.4 min |
| 1.20 Wh | 50.6 min | 86.8 min |

“Equivalent full-power minutes” is an energy integral. It is not clock time in direct sun.

The exact window may produce a small fraction of rated power for many hours.

### 8.3 Why field evidence is still mandatory

The calculation does not include:

- glass spectral/transmission loss;
- vertical angle;
- seasonal solar elevation;
- nearby building;
- foliage;
- frame shadow;
- shade position;
- panel temperature;
- weak-light charger behavior;
- cable loss;
- battery charge efficiency;
- days with little useful collection;
- soiling;
- household schedule changes.

A panel can have enough annual average energy and still fail after a poor winter week. Reserve policy must be based on time series, not average alone.

## 9. Power-path behavior

### 9.1 Desired behavior

When input is available:

1. input supports the system load up to the charger's input/system capability;
2. excess charges the battery;
3. if system demand exceeds input capability, the battery supplies the deficit;
4. after movement, input replenishes removed energy.

Example, simplified:

```text
motor/system demand at battery-equivalent rail: 700 mA
available panel contribution after conversion: 250 mA
battery contribution: roughly 450 mA plus losses
```

This is the requested “split” behavior.

### 9.2 Linear versus switch-mode

A BQ24074-class linear power path is convenient, but it does not transform high panel voltage into proportionally more system current. Voltage difference becomes heat.

A BQ25620-class buck charger can recover more input power, subject to its regulation and the panel's operating point.

The correct choice depends on measured weak-light and system behavior.

### 9.3 Motion while charging

Motion while input is present must be tested for:

- input collapse;
- charger loop oscillation;
- battery supplement;
- voltage sag;
- charger and board temperature;
- MCU reset;
- current-sensor interpretation;
- charge-state transitions.

Do not assume a “load sharing” label proves the exact motor transient.

## 10. Supercapacitor model

Energy in a capacitor:

\[
E = \frac{1}{2} C(V_1^2 - V_2^2)
\]

For 10 F falling from 4.2 V to 3.5 V:

\[
E = 26.95\text{ J} = 0.0075\text{ Wh}
\]

At a 3 W load, ideal duration is:

\[
t = E/P = 26.95/3 \approx 9.0\text{ s}
\]

Real duration is lower after ESR and voltage requirements.

A large supercapacitor is therefore not a useful replacement for the lithium battery in an ordinary movement. Use:

- the battery for energy and burst;
- the power path for input contribution;
- hundreds to low thousands of microfarads near the driver for short transients;
- motor suppression and sound layout.

## 11. Motor candidate interpretation

Published Pololu 99:1 25D low-power 6 V encoder motor values include:

```text
gear ratio: 98.78:1
no-load speed at 6 V: 61 RPM
no-load current: 120 mA
extrapolated stall torque: 9.1 kg·cm
extrapolated stall current: 2.0 A
encoder: 48 counts/motor revolution
output counts: about 4741.44/revolution
```

A rough voltage-proportional estimate at 4 V might suggest:

```text
no-load speed: roughly 41 RPM
stall torque: roughly 6.1 kg·cm
stall current: roughly 1.3 A
```

This is a `DERIVED` planning approximation, not a vendor rating. Gear friction, driver loss, and motor behavior require measurement.

The candidate is attractive for first proof because it likely has substantial margin and excellent position resolution. It is unattractive for final form because of price and size.

## 12. Driver candidate interpretation

A DRV8213-class H-bridge publishes:

- 1.65–11 V motor supply;
- up to 4 A peak drive class;
- integrated current sensing/regulation;
- stall detection support;
- low-power sleep;
- protection features.

The final current limit must be below the safe limits of:

- battery;
- wiring;
- connector;
- PCB;
- driver thermal design;
- motor;
- mechanism;
- shade.

A 4 A-capable driver does not authorize 4 A operation.

## 13. Controller and radio feasibility

ESP32-C6 is a useful controller family because it combines:

- Wi-Fi;
- Bluetooth LE;
- IEEE 802.15.4 for Zigbee/Thread-class protocols;
- sleep modes;
- mature ESP-IDF support;
- sufficient GPIO/peripherals for encoder, driver, buttons, and sensors.

The first firmware language and framework remain a later bounded decision.

The energy design must measure the exact board. Development boards often consume far more in sleep than the module because of regulators, LEDs, USB bridges, and pull networks.

## 14. First-unit cost model

Planning estimate:

| Item | One-off planning range |
|---|---:|
| 2.37 W panel | ~$21 |
| BQ24074 power-path breakout | ~$15 |
| 99:1 25D LP encoder motor | ~$54 |
| ESP32-C6 development board | $7–15 |
| Protected 1S ~2,500 mAh pack | $12–20 |
| Prototype current-observable driver | $6–15 |
| Regulation, sensing, controls, connectors | $10–20 |
| Printed mechanism, guard, fasteners | $15–30 |
| **Estimated first unit** | **$140–190** |

This excludes instruments, shipping, tax, failed prototypes, and custom fabrication services.

### Later low-volume target

A custom, qualified design might target:

| Item group | Planning target |
|---|---:|
| Panel | $8–20 |
| Motor/encoder | $8–20 |
| Battery | $6–12 |
| PCB/assembly | $8–20 |
| Mechanics/enclosure | $6–15 |
| Wiring/fasteners | $3–8 |
| **Target** | **$39–95** |

A narrower $40–85 goal may be possible, but no supplier quote or design supports it yet.

## 15. Commercial benchmark

Commercial retrofit devices establish useful comparison points:

- beaded-chain support;
- USB rechargeable batteries;
- solar accessories;
- app/schedule control;
- multi-month advertised battery life;
- movement-speed/noise tradeoffs;
- product prices around the low hundreds of dollars.

They prove market feasibility, not the performance of this design.

The project should compete first on:

- openness and inspectability;
- local-first ownership;
- energy evidence;
- serviceability;
- parametric compatibility;
- portfolio depth.

Price optimization comes after a reliable route.

## 16. Decision gates

### Motor gate

Requires S0 force, pitch, travel, mount, and desired speed.

### Battery gate

Requires measured worst motion current, voltage sag, daily load, placement temperature, and known pack limits.

### Panel gate

Requires exact-window logging and defined reserve policy.

### Charger gate

Requires panel/load characterization and comparison of linear versus switch-mode benefit.

### Radio gate

Requires measured Wi-Fi/MQTT energy and desired latency.

### Custom PCB gate

Requires stable mechanical load, selected motor/driver, qualified power path, stable sensor set, and proven local controller.

## 17. What would disprove feasibility

The project should be willing to reject the concept for a fixture if:

- safe anchored mounting is not available;
- chain geometry or connector cannot be engaged reliably;
- required force exceeds acceptable compact motor/mount limits;
- window heat prevents safe battery placement;
- available panel exposure is far below the accepted energy budget and USB charging is unacceptable;
- the continuous loop cannot remain safely constrained;
- the shade clutch or chain is already unsuitable for automation;
- the resulting enclosure is unacceptable for the intended room.

A negative fixture result is not necessarily a project failure. It defines compatibility.
