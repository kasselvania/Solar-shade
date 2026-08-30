# Component Candidates

**Status:** researched candidates, not purchase authority  
**Updated:** 2026-08-29

Retail prices are dated snapshots. Verify current availability, documentation, and exact revision before purchase.

## 1. Selection method

A component moves through:

```text
researched
→ candidate
→ selected for one bounded prototype
→ bench proved
→ fixture proved
→ qualified for a defined use
```

No component in this file is currently fixture proved.

## 2. Controller

### Leading family: ESP32-C6

Why it is attractive:

- Wi-Fi and Bluetooth LE for easy first commissioning and MQTT;
- IEEE 802.15.4 for later Zigbee or Thread evaluation;
- sleep modes;
- official ESP-IDF support;
- sufficient peripherals for a quadrature encoder, PWM/H-bridge, buttons, ADC/sensors, and service interfaces;
- broad development-board availability.

Risks:

- development-board sleep current may be much higher than module sleep current;
- Wi-Fi association and poor-signal retries may dominate energy;
- multiprotocol support does not mean simultaneous unrestricted radio operation;
- OTA, Zigbee/Thread, and secure provisioning can create premature scope.

Decision posture:

- acceptable controller family for first prototype;
- exact development board deferred until S1/S2 needs are known;
- production module and firmware language deferred;
- measure whole-board current before energy claims.

### Firmware framework decision

Candidates:

| Option | Strength | Risk | Current posture |
|---|---|---|---|
| ESP-IDF C/C++ | Best official coverage for power, OTA, Wi-Fi, Zigbee/Thread | More manual memory/safety discipline | Leading support baseline |
| Rust over ESP-IDF | Strong type and ownership tooling; portfolio value | Peripheral/radio support and build complexity must be verified at current versions | Worth bounded evaluation |
| Arduino core | Fast initial demos | Can obscure power, driver, OTA, and production boundaries | Not default for safety core |

S0 requires no firmware decision.

## 3. Solar panel

### Conservative first candidate: Voltaic P126

Published values:

- maximum power: 2.37 W;
- open-circuit voltage: 8.51 V;
- voltage at maximum power: 7.28 V;
- current at maximum power: 330 mA;
- dimensions: 112 × 136 × 2.7 mm;
- mass: 79 g;
- ETFE surface and IP67 published construction;
- one-off price observed at $21 on 2026-08-29.

Why it is attractive:

- large enough to remove panel-size uncertainty from the first energy trial;
- appropriate voltage class for common 1S solar charging experiments;
- documented electrical values;
- thin and relatively light;
- can later be replaced by a narrower form factor.

Risks:

- square footprint may be visually intrusive;
- behind-glass output may be much lower;
- 8.51 V open-circuit requires charger input margin;
- window heat and cable routing;
- one exact retailer/supplier dependence.

Decision posture:

- leading S0 logging and first window candidate;
- not qualified for seasonal autonomy;
- exact mounting and visual acceptance deferred.

### Smaller alternatives

Smaller 6 V-class panels around 0.6–1.2 W may be enough after the daily budget is measured.

Do not optimize downward until:

- actual window watt-hours exist;
- radio budget is measured;
- reserve target is defined;
- worst-week behavior is understood.

### Form-factor direction

A mature design may prefer a long, narrow strip near the top or side of the window. Commercial solar accessories demonstrate this form is plausible.

Electrical equivalence and visual fit must be proven independently.

## 4. Prototype charger and power path

### Adafruit BQ24074 power-path board

Published/official basis:

- BQ24074 one-cell linear charger;
- roughly 5–10 V input range on the board;
- system/load output with power-path behavior;
- input power can support load while battery supplements;
- up to 1.5 A-class system/load handling subject to board and thermal conditions;
- USB-C service input;
- NTC capability;
- observed price $14.95 on 2026-08-29.

Why it is attractive:

- rapid safe-ish prototyping compared with an improvised charger;
- visible test points and documentation;
- lets the project measure the requested split-load behavior;
- supports panel and USB service experiments.

Risks:

- linear conversion loses panel voltage headroom as heat;
- no assumption that it extracts maximum panel power in all conditions;
- board thermal and input current settings must be understood;
- motor transients still require battery supplement and decoupling;
- prototype board size and quiescent behavior may not fit final design.

Decision posture:

- leading prototype power-path candidate after the motor route is measured;
- exact charge current, input limit, NTC, and pack compatibility must be configured and tested;
- not a final PCB decision.

## 5. Future custom charger

### TI BQ25620

Published basis:

- single-cell switch-mode charger;
- 3.5 A charge-current class;
- NVDC power path;
- battery supplement when input capability is exceeded;
- programmable input-voltage/current regulation;
- integrated ADC;
- thermal and protection features;
- autonomous and host-controlled behavior.

Why it is attractive:

- higher input-power recovery than a linear charger;
- richer telemetry and policy;
- suitable current class for motor-adjacent system design;
- strong custom-PCB candidate.

Risks:

- more complex schematic/layout/firmware;
- charger settings and USB/solar source behavior must be correctly bounded;
- one chip does not replace battery protection, thermal design, fuse/protection, or energy measurement;
- package and board assembly complexity;
- unnecessary before panel/load data is stable.

Decision posture:

- research candidate for a later custom PCB;
- not authorized in S0;
- compare against other current parts when the custom-PCB slice begins.

## 6. Battery

### Planning class

```text
chemistry: 1S lithium-ion or lithium-polymer
nominal voltage: ~3.7 V
planning capacity: ~2,500 mAh
required: protection
required: published discharge capability adequate for worst motor current
required: temperature monitoring
required: documented charge limits
```

Why this class is attractive:

- about 9.25 Wh nominal energy;
- strong autonomy reserve at estimated daily loads;
- common charger ecosystem;
- sufficient burst capability is commercially plausible;
- manageable size.

Selection blockers:

- exact motor current not measured;
- exact pack dimensions and placement unknown;
- window temperature unknown;
- charge/discharge rating unknown;
- final chemistry not decided.

Reject any pack that lacks:

- credible manufacturer or distributor documentation;
- protection;
- adequate wire/connector;
- discharge rating;
- physical condition;
- known polarity.

### LiFePO₄ alternative

Potential benefits:

- thermal stability;
- cycle/calendar behavior;
- lower nominal voltage can match low-voltage electronics.

Costs:

- lower energy density;
- different charge voltage and charger;
- reduced motor speed/torque unless converted;
- sourcing and form factor.

Keep as an explicit future comparison, not a hybrid architecture.

## 7. Development motor

### Pololu 99:1 25D low-power 6 V encoder gearmotor, item 4827

Published values:

- price observed: $53.95;
- gearbox ratio: 98.78:1;
- no-load speed at 6 V: 61 RPM;
- no-load current at 6 V: 120 mA;
- extrapolated stall torque: 9.1 kg·cm;
- extrapolated stall current: 2.0 A;
- 48 counts per motor revolution;
- approximately 4,741.44 counts per output revolution;
- 25 mm gearbox diameter;
- 4 mm output shaft.

Why it is attractive:

- high development torque margin;
- encoder resolution;
- straightforward mounting;
- known vendor documentation;
- likely low enough speed for a quiet first route;
- avoids choosing an underpowered miniature motor before S0.

Risks:

- expensive;
- larger than desired final unit;
- published stall values are extrapolated;
- no guarantee at a 1S system rail;
- gearbox noise;
- high available reaction torque raises mount and safety pressure.

Decision posture:

- leading first-proof motor only after S0 confirms fit and a safe current limit;
- use a current-limited supply and current-observable driver;
- do not treat it as final BOM.

### Non-encoder sibling

The similar Pololu 99:1 25D low-power motor without encoder was observed at roughly $29.95.

It is cheaper but sacrifices direct motion evidence. The first system should prefer the encoder version unless a separate sensor provides better truth.

### Compact future motor

A micro metal encoder gearmotor may reduce size and cost.

Required before selection:

- measured reference current and torque;
- acceptable speed/noise;
- shaft/bearing side-load strategy;
- compact gearbox life;
- current and thermal margin;
- encoder signal quality;
- sourcing.

Do not pick it from advertised stall torque alone.

### Rejected default: stepper

A stepper is not the default because:

- lower efficiency at this duty;
- potential holding-current temptation;
- audible tonal behavior;
- open-loop missed steps without added sensing;
- driver complexity.

A stepper can be reconsidered only if a measured mechanical or positioning advantage justifies it.

### Worm gearmotor

Potential benefits:

- self-locking or reduced backdrive;
- compact right-angle packaging.

Risks:

- efficiency;
- noise;
- backlash;
- manual release;
- higher energy.

Keep as a comparative mechanical option.

## 8. Motor driver

### Prototype requirement

The prototype driver must provide:

- bidirectional brushed-DC drive;
- voltage compatibility with 1S/system rail;
- current capability above bounded operating current;
- hardware current limiting or a separate independent current limit;
- useful current observation;
- sleep/enable default-off behavior;
- protection;
- accessible test points.

A generic driver board is not selected merely because its headline current is high.

### Future custom candidate: TI DRV8213

Published values/features:

- 1.65–11 V motor supply;
- 4 A peak class;
- integrated current sensing and regulation;
- stall-detection support;
- low sleep current;
- overcurrent, thermal, and undervoltage protections.

Why it is attractive:

- fits a 1S/low-voltage motor system;
- provides the observations needed for load and jam detection;
- can implement an independent current envelope;
- compact custom PCB path.

Risks:

- PCB thermal performance;
- peak versus continuous interpretation;
- tuning current regulation and stall logic;
- sensor accuracy;
- 4 A capability exceeds likely safe pack/mechanism limits.

Decision posture:

- leading custom-PCB driver candidate;
- prototype board and exact current limit deferred.

## 9. Voltage regulation

Requirements for the 3.3 V rail:

- low quiescent current;
- acceptable efficiency from the power-path system voltage;
- ESP32 peak-current capability;
- stable through motor transient;
- enable/sleep behavior;
- low dropout or buck-boost choice based on rail;
- thermal margin.

Do not select the regulator before the exact system-output voltage and controller board are chosen.

A development board's onboard regulator and USB bridge may be tolerated for function proof but must be measured separately from the final energy architecture.

## 10. Sensing

### Minimum prototype observations

- battery/system voltage;
- motor current;
- encoder;
- battery temperature;
- controller temperature if useful;
- panel/input voltage;
- charger status.

### Useful later observations

- panel/input current;
- integrated energy;
- battery coulomb counting;
- guard/service switch;
- mount vibration;
- absolute reference.

Every sensor adds standby current, PCB area, calibration, and failure modes. Add only when it supports a decision or safety requirement.

## 11. Local controls

Leading first interaction:

- physical `OPEN`;
- physical `STOP`;
- physical `CLOSE`;
- status LED or compact indication;
- guarded commissioning/service action;
- USB-C service input.

Two-button chord schemes can reduce parts but may make stop and commissioning less obvious. Physical stop clarity outranks minimal BOM.

## 12. Mechanical materials

### Early prototypes

- printed fit coupons;
- printed sprocket;
- printed guard and mount;
- metal fasteners and inserts;
- deliberate inspection intervals.

Candidate print materials should be selected for:

- window temperature;
- creep;
- wear;
- layer direction;
- impact;
- dimensional stability.

PLA should not be assumed suitable near a warm window.

### Later form

Possible:

- higher-temperature printed polymer;
- machined/acetal sprocket;
- injection-molded parts;
- metal motor plate;
- molded guard;
- replaceable wear inserts.

Material changes require dimensional and endurance requalification.

## 13. Homelab components

### First stack

- Mosquitto or another local MQTT broker;
- Home Assistant MQTT Cover integration;
- optional small fleet/configuration service;
- SQLite/PostgreSQL for configuration/evidence metadata;
- InfluxDB/Prometheus-compatible or other time-series storage if justified;
- Grafana or Home Assistant dashboards.

Do not require all services for one device. Begin with the broker and an inspectable client.

### Device contract

See [`../contracts/MQTT_DEVICE_CONTRACT_DRAFT.md`](../contracts/MQTT_DEVICE_CONTRACT_DRAFT.md).

## 14. Commercial comparison

### SOMA Smart Shades 3

Useful benchmark characteristics published by the vendor include:

- continuous beaded-chain and cord-loop retrofit;
- battery operation;
- solar accessory;
- Zigbee;
- multiple speed/noise modes;
- advertised multi-month battery life;
- approximately 1.5 m movement in under 10 seconds at fastest mode;
- USB-C charging;
- product pricing in the low hundreds of dollars.

### Aqara Roller Shade Driver E1

Useful benchmark characteristics include:

- 3–6 mm beaded-chain support;
- multiple adapters;
- USB-C rechargeable battery;
- Zigbee-based ecosystem;
- advertised battery duration around months;
- straightforward wall installation.

These are competitive references, not designs to copy and not proof of this project's safety or performance.

## 15. First-purchase discipline

Do not buy the complete BOM before S0.

Reasonable early purchases are only those needed to measure:

- digital caliper if absent;
- suitable force gauge/scale;
- candidate panel plus logger/load if S0 solar logging is authorized;
- temperature logger if absent;
- fit-coupon material.

Motor, driver, battery, and charger purchase should follow the S0 decision packet unless availability makes a reversible early development purchase worthwhile.

## 16. Candidate-change record

When replacing a candidate, record:

- old exact part;
- new exact part;
- reason;
- affected voltage/current/mechanical interfaces;
- changed calculations;
- changed tests;
- evidence invalidated;
- procurement date and price snapshot.

A “drop-in replacement” claim requires exact electrical, mechanical, firmware, and acceptance compatibility.
