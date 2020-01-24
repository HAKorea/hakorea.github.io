---
title: Ness Alarm
description: Instructions on how to integrate a Ness D8x/D16x alarm system with Home Assistant.
logo: ness.png
ha_category:
  - Alarm
  - Binary Sensor
ha_release: 0.85
ha_iot_class: Local Push
ha_codeowners:
  - '@nickw444'
---

The `ness_alarm` integration will allow Home Assistant users who own a Ness D8x/D16x alarm system to leverage their alarm system and its sensors to provide Home Assistant with information about their homes. Connectivity between Home Assistant and the alarm is accomplished through a IP232 module that must be connected to the alarm.

There is currently support for the following device types within Home Assistant:

- Binary Sensor: Reports on zone statuses
- Alarm Control Panel: Reports on alarm status, and can be used to arm/disarm the system

The module communicates via the [Ness D8x/D16x ASCII protocol](http://www.nesscorporation.com/Software/Ness_D8-D16_ASCII_protocol.pdf).

## Configuration

A `ness_alarm` section must be present in the `configuration.yaml` file and contain the following options as required:

```yaml
# Example configuration.yaml entry
ness_alarm:
  host: alarm.local
  port: 2401
  zones:
    - name: Garage
      id: 1
    - name: Storeroom
      id: 2
    - name: Kitchen
      id: 3
    - name: Front Entrance
      id: 4
    - name: Front Door
      id: 5
      type: door
```

{% configuration %}
host:
  description: The hostname of the IP232 module on your home network.
  required: true
  type: string
port:
  description: The port on which the IP232 module listens for clients.
  required: true
  type: integer
scan_interval:
  description: "Time interval between updates. Supported formats: `scan_interval: 'HH:MM:SS'`, `scan_interval: 'HH:MM'` and Time period dictionary (see example below)."
  required: false
  default: '00:01:00'
  type: time
infer_arming_state:
  description: Infer the disarmed arming state only via system status events. This works around a bug with some panels (`<v5.8`) which emit `update.status = []` when they are armed.
  required: false
  default: false
  type: boolean
zones:
  description: List of zones to add
  required: false
  type: list
  keys:
    zone_id:
      description: ID of the zone on the alarm system (i.e Zone 1 -> Zone 16).
      required: true
      type: integer
    name:
      description: Name of the zone.
      required: true
      type: string
    type:
      description: The zone type. Can be any [binary_sensor device class](/integrations/binary_sensor/#device-class).
      required: false
      default: motion
      type: string
{% endconfiguration %}

#### Time period dictionary example

```yaml
scan_interval:
  # At least one of these must be specified:
  days: 0
  hours: 0
  minutes: 0
  seconds: 10
  milliseconds: 0
```

## Services

### Service `aux`

Trigger an aux output.  This requires PCB version 7.8 or higher.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `output_id` | No | The aux output you wish to change.  A number from 1-4.
| `state` | Yes | The On/Off State, represented as true/false. Default is true.  If P14xE 8E is enabled then a value of true will pulse output x for the time specified in P14(x+4)E.

### Service `panic`

Trigger a panic

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `code` | No | The user code to use to trigger the panic.
