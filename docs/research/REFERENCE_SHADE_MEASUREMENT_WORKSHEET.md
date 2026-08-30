# Reference-Shade Measurement Worksheet

Use this worksheet for `SS-FX-001`. Preserve raw measurements in CSV or another machine-readable form under `evidence/s0-reference-shade/`.

Do not fill unknown fields from the initiating photograph.

## 1. Evidence header

```yaml
fixture_id: SS-FX-001
date:
operator:
repository_commit:
current_slice_blob:
location_class: indoor residential window
exact_location_public: false
shade_manufacturer:
shade_model:
instrument_caliper:
instrument_force:
instrument_voltage_current:
instrument_temperature:
notes:
```

## 2. Privacy check

Before committing images:

- [ ] Crop exterior building, street, and identifying surroundings.
- [ ] Remove family photographs and reflections.
- [ ] Remove serial numbers and purchase labels not needed for the claim.
- [ ] Remove Wi-Fi/network information.
- [ ] Preserve a private original only outside the public repository if needed.

## 3. Bead geometry

### Caliper zero and repeatability

```text
caliper resolution:
zero check:
reference check, if available:
```

### Bead diameter

Measure at least five beads and rotate the caliper orientation.

| Sample | Bead ID/location | Diameter A (mm) | Diameter B (mm) | Notes |
|---:|---|---:|---:|---|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |
| 5 | | | | |

Derived:

```text
mean:
minimum:
maximum:
range:
```

### Pitch over multiple intervals

Measure from the same edge of bead 0 to the same edge of bead 10 or use center estimates. Divide by the number of intervals, not the number of beads.

| Trial | Number of intervals | Total distance (mm) | Derived pitch (mm) | Method/notes |
|---:|---:|---:|---:|---|
| 1 | 10 | | | |
| 2 | 10 | | | |
| 3 | 20 if possible | | | |

```text
selected pitch:
uncertainty/tolerance:
```

### Cord/link geometry

```text
connecting cord or link material:
cord diameter/width:
bead-to-cord clearance concern:
visible damage:
```

## 4. Chain connector

Record:

```text
connector present:
connector type:
maximum width:
maximum depth:
length:
position in loop:
passes current tensioner:
expected to pass driven sprocket:
```

Photograph with scale.

If the connector cannot traverse the drive path, record whether:

- it can lawfully remain outside the traversed region;
- it can be replaced with a compatible connector;
- the fixture is incompatible with the current concept.

Do not cut or alter the chain in S0.

## 5. Existing tensioner and mounting

```text
tensioner manufacturer/model:
attachment type:
fastener count/type:
mounting substrate:
distance from chain centerline to wall:
available width:
available height:
available depth:
window/trim clearances:
furniture/curtain clearances:
```

### Safety-function observation

```text
how the tensioner prevents a loose loop:
loop movement allowed:
guarding present:
what changes if removed:
candidate actuator location:
candidate structural fastener path:
```

## 6. Shade dimensions and travel

```text
visible shade width_mm:
shade height/travel_mm:
chain loop total length_mm:
distance between chain legs_mm:
headrail to tensioner_mm:
```

### Chain displacement

Mark one bead or use a removable reference without damaging the chain.

| Trial | Direction | Shade start | Shade end | Chain displacement (mm) | Notes |
|---:|---|---|---|---:|---|
| 1 | open | closed | open | | |
| 2 | close | open | closed | | |
| 3 | open | closed | open | | |
| 4 | close | open | closed | | |

```text
selected full-travel displacement:
variation:
```

### Comfortable manual timing

| Trial | Direction | Full-travel time (s) | Subjective speed/noise notes |
|---:|---|---:|---|
| 1 | open | | |
| 2 | close | | |
| 3 | open | | |
| 4 | close | | |

```text
initial target motor movement time:
reason:
```

## 7. Force-measurement procedure

### Setup rules

- Keep the loop constrained by the existing tensioner.
- Attach the gauge to the active leg without crushing or slipping off the beads.
- Pull in the chain's direction, not sideways.
- Use slow, repeatable motion.
- Do not jerk.
- Stop if the chain, clutch, tensioner, or mount appears damaged.
- Record gauge resolution and peak-hold behavior.

### Raw force table

Use newtons where possible. If the gauge reads kilograms-force or pounds-force, preserve the raw unit and convert separately.

| Trial | Direction | Shade region | Reading type | Raw value | Raw unit | Converted N | Notes |
|---:|---|---|---|---|---:|---:|---|
| 1 | open | bottom | breakaway | | | | |
| 2 | open | bottom | running | | | | |
| 3 | open | bottom | peak | | | | |
| 4 | open | middle | breakaway | | | | |
| 5 | open | middle | running | | | | |
| 6 | open | middle | peak | | | | |
| 7 | open | top | breakaway | | | | |
| 8 | open | top | running | | | | |
| 9 | open | top | peak | | | | |
| 10 | close | top | breakaway | | | | |
| 11 | close | top | running | | | | |
| 12 | close | top | peak | | | | |
| 13 | close | middle | breakaway | | | | |
| 14 | close | middle | running | | | | |
| 15 | close | middle | peak | | | | |
| 16 | close | bottom | breakaway | | | | |
| 17 | close | bottom | running | | | | |
| 18 | close | bottom | peak | | | | |

Repeat the matrix at least three times in the machine-readable evidence.

### Summary

```text
open breakaway range_N:
open running range_N:
open peak range_N:
close breakaway range_N:
close running range_N:
close peak range_N:
overall selected design force_N:
reason:
```

## 8. Shade health

Before motorization, inspect manually:

- [ ] No visibly cracked beads.
- [ ] No frayed connecting cord.
- [ ] Connector is secure.
- [ ] Tensioner is anchored.
- [ ] Shade moves through full range.
- [ ] No unexplained binding.
- [ ] Clutch holds shade position.
- [ ] Load asymmetry is recorded.
- [ ] Endpoints are understood.
- [ ] Existing damage is photographed.

```text
fixture health result:
accepted / repair required / rejected:
```

## 9. Sprocket calculation

Inputs:

```text
selected bead pitch p_mm:
candidate pocket count N:
measured bead diameter d_mm:
```

Equation:

\[
r = \frac{p}{2\sin(\pi/N)}
\]

```text
pitch radius r_mm:
pitch diameter_mm:
candidate pocket diameter_mm:
candidate groove/cord relief:
candidate wrap_angle_deg:
engaged_beads:
connector relief:
```

## 10. Torque calculation

Inputs:

```text
selected design force F_N:
pitch radius r_m:
mechanical loss/margin rationale:
```

\[
\tau_{load} = F \times r
\]

```text
load torque_Nm:
load torque_kgcm:
selected design multiplier:
target operating torque_Nm:
candidate motor:
published speed/current:
published rated torque, if any:
published extrapolated stall torque:
published stall current:
explicit caveat:
```

## 11. Speed calculation

\[
v = RPM \times N \times p
\]

```text
candidate output RPM:
chain speed_m_per_min:
measured chain travel_m:
calculated travel time_s:
target ramp time:
```

## 12. Mount envelope

Record a dimensioned sketch:

```text
chain centerline:
wall/window planes:
minimum guard clearance:
maximum enclosure width:
maximum enclosure depth:
maximum enclosure height:
fastener locations:
battery location:
panel wire route:
manual service access:
local button reach:
```

### Reaction load

```text
expected chain force direction:
expected motor reaction:
candidate fastener:
candidate substrate:
load-test method proposed:
adhesive role, if any:
```

## 13. Panel-location record

```yaml
panel_candidate:
panel_dimensions:
window_side: private
orientation_description:
distance_from_glass_mm:
frame_shadow:
foliage_obstruction:
building_obstruction:
shade_blocks_panel: unknown
wire_route:
```

### Point measurements

| Timestamp | Shade position | Panel Voc (V) | Loaded V (V) | Loaded I (mA) | Power (W) | Nearby temp (°C) | Conditions |
|---|---|---:|---:|---:|---:|---:|---|
| | | | | | | | |

### Logged energy

```text
logger:
sample_interval:
start:
end:
missing_data:
input_watt_hours:
maximum_power:
daily_minimum:
daily_median:
daily_maximum:
```

Do not publish exact orientation/location detail if it creates a privacy risk. Use sufficient engineering descriptors.

## 14. Thermal observations

| Timestamp | Conditions | Glass-adjacent °C | Candidate battery location °C | Room °C | Panel °C | Notes |
|---|---|---:|---:|---:|---:|---|
| | | | | | | |

```text
observed maximum:
battery placement implication:
charger temperature policy implication:
seasonal unknown:
```

## 15. Energy calculation

```text
assumed movement voltage_V:
assumed/measured current_A:
movement open_s:
movement close_s:
motor Wh/cycle:
standby assumption Wh/day:
conversion/loss allowance:
total planning Wh/day:
```

Solar replacement:

\[
t = E_d/(P_p\eta)
\]

```text
panel nameplate W:
assumed useful factor:
equivalent full-power minutes:
field evidence status:
```

## 16. S0 selection packet

### Selected first-proof route

```yaml
sprocket_revision:
motor:
driver:
bench_supply_voltage:
bench_supply_current_limit:
maximum_motion_time:
guard_concept:
mount_concept:
panel:
charger:
battery_class:
```

### Rejected alternatives

| Alternative | Rejected/deferred reason | Revisit trigger |
|---|---|---|
| | | |

### Explicit nonclaims

- [ ] No installed automation claim.
- [ ] No seasonal solar-neutral claim.
- [ ] No battery runtime claim.
- [ ] No child-safe/certified claim.
- [ ] No universal chain compatibility claim.
- [ ] No final BOM claim.

## 17. Sign-off

```text
operator:
technical review:
result: PASS / BLOCKED / REJECTED
date:
next-slice recommendation:
remaining first-order unknowns:
```
