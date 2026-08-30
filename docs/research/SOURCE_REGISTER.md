# Source Register

**Updated:** 2026-08-29

This register retains the primary published basis used for the initial feasibility model. It does not replace datasheets, fixture measurements, or component qualification.

Prices are snapshots observed on the access date.

## Source classes

- `PRIMARY_DATASHEET`: manufacturer technical document.
- `PRIMARY_PRODUCT`: manufacturer/distributor official product page.
- `OFFICIAL_FRAMEWORK`: official software documentation.
- `GOVERNMENT_SAFETY`: government safety guidance or rule.
- `COMMERCIAL_BENCHMARK`: vendor claim about an existing product.
- `PROJECT_DERIVATION`: calculation in this repository.

## Embedded controller

### Espressif ESP32-C6 documentation

- Publisher: Espressif Systems
- Class: `PRIMARY_DATASHEET` / `OFFICIAL_FRAMEWORK`
- Accessed: 2026-08-29
- Links:
  - [ESP32-C6 series documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c6/)
  - [ESP32-C6 sleep modes](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c6/api-reference/system/sleep_modes.html)
  - [ESP32-C6 datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-c6_datasheet_en.pdf)
  - [ESP Zigbee SDK guide](https://docs.espressif.com/projects/esp-zigbee-sdk/en/latest/esp32c6/)
  - [ESP Thread documentation](https://docs.espressif.com/projects/esp-thread-br/en/latest/esp32c6/)
- Facts used:
  - ESP32-C6 provides Wi-Fi, Bluetooth LE, and IEEE 802.15.4 capability.
  - ESP-IDF provides sleep-mode support.
  - Zigbee and Thread endpoint development are supported by official stacks.
- Limitations:
  - Module/chip sleep figures do not establish development-board or complete-device average current.
  - Multiprotocol capability does not remove shared-radio scheduling constraints.
  - Current software support must be rechecked when a firmware slice begins.

## Prototype charger and power path

### Texas Instruments BQ24074

- Publisher: Texas Instruments
- Class: `PRIMARY_DATASHEET`
- Accessed: 2026-08-29
- Link: [BQ24074 product page and datasheet](https://www.ti.com/product/BQ24074)
- Facts used:
  - one-cell linear lithium charger;
  - power-path/system-output behavior;
  - battery supplement;
  - 1.5 A-class output/charge family;
  - input overvoltage and thermal/protection features.
- Limitations:
  - Exact board configuration, thermal performance, input current, and weak-panel behavior remain implementation-specific.

### Adafruit BQ24074 Universal USB/DC/Solar Charger board

- Publisher: Adafruit
- Class: `PRIMARY_PRODUCT`
- Accessed: 2026-08-29
- Link: [Adafruit product 4755](https://www.adafruit.com/product/4755)
- Snapshot:
  - observed price: $14.95;
  - published 5–10 V input class;
  - USB-C/DC/solar use;
  - load sharing/power path;
  - NTC support.
- Facts used:
  - practical prototype board exists for input-plus-battery system behavior;
  - board documentation recommends a panel voltage with useful charger headroom.
- Limitations:
  - Vendor description is not a fixture energy qualification.
  - The board is linear and may not recover all panel nameplate power.

## Future custom charger

### Texas Instruments BQ25620

- Publisher: Texas Instruments
- Class: `PRIMARY_DATASHEET`
- Accessed: 2026-08-29
- Link: [BQ25620 product page and datasheet](https://www.ti.com/product/BQ25620)
- Facts used:
  - single-cell switch-mode charger;
  - NVDC power path;
  - battery supplement;
  - input-voltage/current regulation;
  - integrated ADC;
  - thermal and protection features;
  - autonomous and host-controlled behavior.
- Limitations:
  - It is a research candidate, not a selected design.
  - A later design must recheck errata, availability, layout, battery, and source requirements.

## Motor driver

### Texas Instruments DRV8213

- Publisher: Texas Instruments
- Class: `PRIMARY_DATASHEET`
- Accessed: 2026-08-29
- Link: [DRV8213 product page and datasheet](https://www.ti.com/product/DRV8213)
- Facts used:
  - low-voltage brushed-DC H-bridge;
  - 1.65–11 V supply class;
  - 4 A peak class;
  - current sensing/regulation;
  - stall detection support;
  - low-power sleep and protections.
- Limitations:
  - Peak current is not an authorized operating current.
  - Continuous current depends on PCB and thermal design.
  - Final limits must respect battery, wiring, motor, mount, and shade.

## Solar panel

### Voltaic Systems P126

- Publisher: Voltaic Systems
- Class: `PRIMARY_PRODUCT`
- Accessed: 2026-08-29
- Link: [P126 2 Watt 6 Volt ETFE solar panel](https://voltaicsystems.com/p126-2-watt-6-volt-solar-panel-etfe/)
- Snapshot:
  - observed one-off price: $21;
  - maximum power: 2.37 W;
  - open-circuit voltage: 8.51 V;
  - maximum-power voltage: 7.28 V;
  - maximum-power current: 330 mA;
  - dimensions: 112 × 136 × 2.7 mm;
  - mass: 79 g.
- Facts used:
  - a compact documented panel can provide a conservative first energy source;
  - panel voltage/current class is compatible with a suitably chosen one-cell solar charger.
- Limitations:
  - Rated conditions are not behind-window conditions.
  - Exact window, angle, heat, glazing, and obstruction must be logged.

## Development motor

### Pololu 99:1 Metal Gearmotor 25Dx54L mm LP 6V with 48 CPR Encoder, item 4827

- Publisher: Pololu
- Class: `PRIMARY_PRODUCT`
- Accessed: 2026-08-29
- Link: [Pololu item 4827](https://www.pololu.com/product/4827)
- Snapshot:
  - observed price: $53.95;
  - ratio: 98.78:1;
  - no-load speed at 6 V: 61 RPM;
  - no-load current: 120 mA;
  - extrapolated stall torque: 9.1 kg·cm;
  - extrapolated stall current: 2.0 A;
  - approximately 4,741.44 encoder counts/output revolution.
- Facts used:
  - an over-capable, high-resolution development motor is available;
  - its speed is consistent with a tens-of-seconds movement after voltage and sprocket assumptions.
- Limitations:
  - Stall values are extrapolated and not continuous ratings.
  - Behavior at a 1S/system rail is a project derivation until measured.
  - The motor is not a final size/cost choice.

### Pololu non-encoder sibling, item 1587

- Publisher: Pololu
- Class: `PRIMARY_PRODUCT`
- Accessed: 2026-08-29
- Link: [Pololu item 1587](https://www.pololu.com/product/1587)
- Snapshot:
  - observed price: $29.95;
  - similar 99:1 low-power 6 V gearmotor without encoder.
- Fact used:
  - encoder accounts for meaningful development cost but provides essential motion evidence.
- Limitation:
  - Not currently preferred for first closed-loop proof.

## Home Assistant integration

### MQTT Cover

- Publisher: Home Assistant
- Class: `OFFICIAL_FRAMEWORK`
- Accessed: 2026-08-29
- Link: [Home Assistant MQTT Cover](https://www.home-assistant.io/integrations/cover.mqtt/)
- Facts used:
  - Home Assistant has an official MQTT cover integration suitable for blinds/shades;
  - position, open/close/stop, and state topics can be represented.
- Limitations:
  - Home Assistant's UI model must not force the device to claim a known percentage when confidence is lost.
  - The repository's exact schema remains a project decision.

### MQTT discovery

- Publisher: Home Assistant
- Class: `OFFICIAL_FRAMEWORK`
- Accessed: 2026-08-29
- Link: [Home Assistant MQTT discovery](https://www.home-assistant.io/integrations/mqtt/#mqtt-discovery)
- Fact used:
  - local devices can publish discovery configuration.
- Limitation:
  - Discovery is convenience; device safety and identity must not depend on it.

## Commercial feasibility benchmarks

### SOMA Smart Shades 3

- Publisher: SOMA Smart Home
- Class: `COMMERCIAL_BENCHMARK`
- Accessed: 2026-08-29
- Links:
  - [SOMA Smart Shades 3](https://www.somasmarthome.com/products/soma-smart-shades-3)
  - [SOMA solar panel](https://www.somasmarthome.com/products/solar-panel-for-smart-shades)
- Snapshot/claims used:
  - beaded-chain and cord-loop retrofit;
  - rechargeable battery and USB-C;
  - optional solar panel;
  - Zigbee;
  - published speed/noise tradeoffs;
  - multi-month advertised battery life;
  - approximately 1.5 m movement in under 10 seconds in fast mode;
  - published support for common bead-chain families;
  - solar accessory observed around $39;
  - motor observed around $159 sale / $199 ordinary listing.
- Limitations:
  - Vendor claims are not independently verified here.
  - Product design, firmware, energy budget, and safety evidence are not available to this project.
  - It is a benchmark, not a source to copy.

### Aqara Roller Shade Driver E1

- Publisher: Aqara
- Class: `COMMERCIAL_BENCHMARK`
- Accessed: 2026-08-29
- Link: [Aqara Roller Shade Driver E1](https://www.aqara.com/en/product/roller-shade-driver-e1/)
- Claims used:
  - 3–6 mm beaded-chain support;
  - multiple chain adapters;
  - Zigbee;
  - USB-C rechargeable battery;
  - multi-month advertised battery duration.
- Limitations:
  - Vendor claims are not fixture evidence.
  - Compatibility range does not establish the initiating shade's dimensions.

## Continuous-loop safety

### U.S. Consumer Product Safety Commission window-covering guidance

- Publisher: U.S. Consumer Product Safety Commission
- Class: `GOVERNMENT_SAFETY`
- Accessed: 2026-08-29
- Links:
  - [Window blind cord safety](https://www.cpsc.gov/Safety-Education/Safety-Guides/Kids-and-Babies/Window-Blind-Cords)
  - [Childproofing your home](https://www.cpsc.gov/safety-education/safety-guides/kids-and-babies/Childproofing-Your-Home)
- Facts used:
  - continuous loops and bead chains are recognized strangulation hazards;
  - loops should be anchored with appropriate tension/restraining devices;
  - corded-window-covering safety is a first-class product constraint.
- Limitations:
  - General guidance does not certify a custom mechanism.
  - Applicable standards and legal requirements must be researched before productization.

### Federal rule and supporting CPSC material

- Publisher: U.S. Consumer Product Safety Commission / U.S. government
- Class: `GOVERNMENT_SAFETY`
- Accessed: 2026-08-29
- Link: [CPSC window-covering rule information](https://www.cpsc.gov/Newsroom/News-Releases/2022/CPSC-Approves-New-Federal-Safety-Standard-for-Custom-Window-Coverings-to-Prevent-Deaths-and-Serious-Injuries-from-Strangulation-Products-With-Hazardous-Operating-Cords-Will-Need-to-Meet-Same-Safety-Requirements-as-Stock-Window-Coverings)
- Facts used:
  - operating cords are subject to serious safety scrutiny;
  - restraining devices and durability are relevant to product design.
- Limitations:
  - This repository has not yet completed a regulatory applicability analysis.
  - No compliance claim is made.

## Project derivations

### Feasibility and Sizing Model

- Publisher: this repository
- Class: `PROJECT_DERIVATION`
- Link: [`FEASIBILITY_AND_SIZING_MODEL.md`](FEASIBILITY_AND_SIZING_MODEL.md)
- Calculations:
  - sprocket pitch radius;
  - force-to-torque;
  - chain speed;
  - motor energy;
  - battery nominal and usable planning energy;
  - solar equivalent-full-power minutes;
  - supercapacitor energy;
  - cost planning.
- Limitation:
  - Inputs labeled as assumed remain unproved until S0/S1.

## Update rule

When a source changes or a candidate is selected:

1. retain the exact new source;
2. record access date and version/date where published;
3. identify the old fact;
4. identify the new fact;
5. state which calculations or decisions are invalidated;
6. do not silently rewrite an evidence package that used the old source.
