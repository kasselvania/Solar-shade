# MQTT Device Contract — Draft

**Status:** design draft  
**Authority:** not implementation authority  
**Purpose:** make the intended local-first boundary concrete enough to review before firmware exists

The transport delivers desired state and observations. It does not expose motor PWM, raw direction, current-limit overrides, endpoint bypass, or fault-latch bypass.

## 1. Namespace

Provisional base:

```text
solar-shade/v1/<device_id>/
```

Topics:

```text
availability
command
state
telemetry
event
config
```

A Home Assistant discovery adapter may publish under Home Assistant's configured discovery prefix, but that is an adapter concern.

## 2. Device identity

`device_id` is a stable provisioned identity.

It must not be:

- current IP address;
- human room name;
- mutable MQTT client name alone;
- an exposed secret;
- a hardware serial that unnecessarily leaks supplier/customer data.

Human labels are separate metadata.

## 3. Availability

Topic:

```text
solar-shade/v1/<device_id>/availability
```

Payload:

```json
{
  "schema_version": 1,
  "device_id": "ss-0001",
  "state": "online",
  "boot_id": "opaque-boot-identity",
  "firmware_version": "0.1.0",
  "state_revision": 42,
  "reported_at": "2026-08-29T18:00:00Z"
}
```

Allowed state:

```text
online
offline
service
```

MQTT Last Will may publish `offline`, but local safety does not depend on delivery.

## 4. Desired-state command

Topic:

```text
solar-shade/v1/<device_id>/command
```

Payload:

```json
{
  "schema_version": 1,
  "device_id": "ss-0001",
  "command_id": "018f-example",
  "issued_at": "2026-08-29T18:00:00Z",
  "expires_at": "2026-08-29T18:00:30Z",
  "source": {
    "kind": "home_assistant",
    "id": "local-ha"
  },
  "expected_state_revision": 42,
  "desired": {
    "kind": "position",
    "position_percent": 65
  }
}
```

Desired kinds:

```json
{"kind":"position","position_percent":65}
{"kind":"open"}
{"kind":"close"}
{"kind":"stop"}
```

No other movement fields are accepted in version 1.

### Admission

The device validates:

- schema version;
- exact device ID;
- command ID;
- freshness;
- expiry;
- duplicate status;
- optional expected state revision;
- commissioned state;
- local stop/service/fault state;
- position confidence;
- power and temperature permission.

The device may reject a validly formatted command.

### Retention warning

A broker-retained command can be useful as a desired-state record, but it can also become stale movement pressure.

Before using retained commands, the implementation must define:

- expiry;
- state revision;
- boot reconciliation;
- local-stop precedence;
- what happens when the device was manually moved;
- what happens after a long outage;
- whether `STOP` is retained;
- whether commands or only a separate desired-state topic are retained.

The first implementation may choose non-retained commands plus retained device state.

## 5. Device state

Topic:

```text
solar-shade/v1/<device_id>/state
```

Retained payload:

```json
{
  "schema_version": 1,
  "device_id": "ss-0001",
  "boot_id": "opaque-boot-identity",
  "state_revision": 43,
  "reported_at": "2026-08-29T18:00:03Z",
  "mode": "idle",
  "motion": "stopped",
  "position": {
    "percent": 65.2,
    "confidence": "calibrated",
    "calibration_revision": 3
  },
  "last_command": {
    "command_id": "018f-example",
    "disposition": "completed"
  },
  "energy_mode": "normal",
  "fault": null
}
```

Modes may include:

```text
uncommissioned
idle
moving_open
moving_closed
calibrating
stopped_local
low_energy
service
fault_latched
```

Motion:

```text
opening
closing
stopped
unknown
```

Position confidence:

```text
calibrated
estimated
degraded
unknown
```

When confidence is `unknown`, omit `percent` or set it to `null`.

Do not publish an invented percentage to satisfy a UI.

## 6. Command disposition

State or event reports:

```text
accepted
already_satisfied
completed
stopped_local
deferred_low_energy
rejected_stale
rejected_duplicate
rejected_revision_conflict
rejected_uncommissioned
rejected_faulted
rejected_position_unknown
rejected_thermal
rejected_low_energy
rejected_service
failed_motion
```

A duplicate of a completed command returns the existing disposition and does not repeat movement.

## 7. Telemetry

Topic:

```text
solar-shade/v1/<device_id>/telemetry
```

Example:

```json
{
  "schema_version": 1,
  "device_id": "ss-0001",
  "reported_at": "2026-08-29T18:05:00Z",
  "sample_window_seconds": 300,
  "battery": {
    "voltage_v": 3.91,
    "state_of_charge_percent": null,
    "temperature_c": 24.8
  },
  "solar": {
    "input_voltage_v": 6.82,
    "input_current_ma": 112.0,
    "harvested_wh_today": 0.31
  },
  "system": {
    "energy_used_wh_today": 0.08,
    "controller_temperature_c": 29.1,
    "radio_rssi_dbm": -61,
    "wake_count_today": 19
  },
  "motor_last": {
    "direction": "open",
    "duration_ms": 32120,
    "average_current_ma": 514,
    "peak_current_ma": 882,
    "encoder_counts": 24110,
    "energy_wh": 0.019
  }
}
```

Fields remain optional until sensors are implemented.

Sensor accuracy and calibration metadata should be available through config/device metadata rather than implied.

## 8. Events

Topic:

```text
solar-shade/v1/<device_id>/event
```

Events are non-retained and may include:

```text
movement_started
movement_completed
movement_stopped
fault_latched
fault_cleared
position_confidence_changed
calibration_completed
low_energy_entered
low_energy_exited
thermal_inhibit_entered
thermal_inhibit_exited
reboot
update_result
service_entered
service_exited
```

Example:

```json
{
  "schema_version": 1,
  "device_id": "ss-0001",
  "event_id": "opaque-event-id",
  "event": "fault_latched",
  "occurred_at": "2026-08-29T18:00:02Z",
  "state_revision": 44,
  "details": {
    "fault_code": "motor_no_motion",
    "command_id": "018f-example",
    "elapsed_ms": 820,
    "peak_current_ma": 1040,
    "position_percent": 65.2
  }
}
```

## 9. Fault model

Candidate codes:

```text
motor_overcurrent
motor_no_motion
motor_encoder_implausible
motor_timeout
driver_fault
battery_undervoltage
battery_overtemperature
charge_overtemperature
sensor_fault
guard_or_service_open
position_unknown
calibration_invalid
storage_fault
watchdog_reset_during_motion
```

The exact fault set belongs to the embedded controller.

The network cannot clear a safety fault unless an accepted decision explicitly allows that exact fault class. Critical mechanical and thermal faults should normally require local inspection/action.

## 10. Configuration

Configuration is distinct from movement.

Potential topic:

```text
solar-shade/v1/<device_id>/config
```

Versioned config may eventually include:

- human labels;
- timezone;
- local schedule;
- telemetry cadence;
- non-safety movement preference such as quiet mode;
- Home Assistant discovery enablement.

Remote configuration must not increase current, temperature, timeout, or mechanical limits beyond the firmware-qualified envelope.

Safety-critical calibration may require a local commissioning state.

## 11. Home Assistant mapping

Provisional mapping:

| Home Assistant concept | Device field |
|---|---|
| open command | desired `open` |
| close command | desired `close` |
| stop command | desired `stop` |
| set position | desired `position` |
| position state | state position when confidence is not unknown |
| opening/closing | state motion |
| availability | availability topic |
| diagnostics | telemetry and fault sensors |

An adapter should expose a diagnostic entity when position is unknown or the device is faulted.

## 12. Security

Minimum later requirements:

- broker authentication;
- per-device credentials or equivalent;
- encrypted transport when crossing an untrusted link;
- no embedded shared household password in source;
- credential rotation;
- command authorization;
- firmware integrity;
- no raw debug control on production topics.

The first bench network may be isolated, but insecure bench shortcuts must not silently become installed defaults.

## 13. Bounds

The implementation must define and test:

- maximum payload;
- maximum command rate;
- maximum queued commands;
- telemetry cadence;
- reconnect backoff;
- offline buffer;
- command expiry window;
- clock uncertainty behavior;
- retained-state size.

One desired state is normally more useful than a queue of historical movement commands.

## 14. Open decisions

- commands versus separate retained desired-state topic;
- required remote latency;
- periodic wake versus maintained connection;
- clock source and behavior without time;
- exact authentication;
- Home Assistant discovery ownership;
- telemetry storage;
- Zigbee/Thread mapping;
- remote fault-clear classes.

No open decision may move motor safety out of the device.
