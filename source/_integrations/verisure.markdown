---
title: Verisure
description: Instructions on how to setup Verisure devices within Home Assistant.
logo: verisure.png
ha_category:
  - Hub
  - Alarm
  - Binary Sensor
  - Camera
  - Lock
  - Sensor
  - Switch
ha_release: pre 0.7
ha_iot_class: Cloud Polling
---

Home Assistant has support to integrate your [Verisure](https://www.verisure.com/) devices.

There is currently support for the following device types within Home Assistant:

- Alarm
- Camera
- Switch (Smartplug)
- Sensor (Thermometers, Hygrometers and Mouse detectors)
- Lock
- Binary Sensor (Door & Window)

## Configuration

To integrate Verisure with Home Assistant, add the following section to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
verisure:
  username: USERNAME
  password: PASSWORD
```

{% configuration %}
username:
  description: The username to Verisure mypages.
  required: true
  type: string
password:
  description: The password to Verisure mypages.
  required: true
  type: string
alarm:
  description: Set to `true` to show alarm, `false` to disable.
  required: false
  type: boolean
  default: true
hygrometers:
  description: Set to `true` to show hygrometers, `false` to disable.
  required: false
  type: boolean
  default: true
smartplugs:
  description: Set to `true` to show smartplugs, `false` to disable.
  required: false
  type: boolean
  default: true
locks:
  description: Set to `true` to show locks, `false` to disable.
  required: false
  type: boolean
  default: true
default_lock_code:
  description: Code that will be used to lock or unlock, if none is supplied.
  required: false
  type: string
thermometers:
  description: Set to `true` to show thermometers, `false` to disable.
  required: false
  type: boolean
  default: true
mouse:
  description: Set to `true` to show mouse detectors, `false` to disable.
  required: false
  type: boolean
  default: true
door_window:
  description: Set to `true` to show mouse detectors, `false` to disable.
  required: false
  type: boolean
  default: true
code_digits:
  description: Number of digits in PIN code.
  required: false
  type: integer
  default: 4
giid:
  description: The GIID of your installation (If you have more then one alarm system). To find the GIID for your systems run 'python verisure.py EMAIL PASSWORD installations'.
  required: false
  type: string
{% endconfiguration %}

## Alarm Control Panel

The Verisure alarm control panel platform allows you to control your [Verisure](https://www.verisure.com/) Alarms.

The requirement is that you have setup your Verisure hub first, with the instruction above.

The `changed_by` attribute enables one to be able to take different actions depending on who armed/disarmed the alarm in [automation](/getting-started/automation/).

```yaml
automation:
  - alias: Alarm status changed
    trigger:
      - platform: state
        entity_id: alarm_control_panel.alarm_1
    action:
      - service: notify.notify
        data_template:
          message: >
            {% raw %}Alarm changed from {{ trigger.from_state.state }}
            to {{ trigger.to_state.state }}
            by {{ trigger.to_state.attributes.changed_by }}{% endraw %}
```

## Services

| Service | Description |
| ------- | ----------- |
| disable_autolock | Disables autolock function for a specific lock. |
| enable_autolock | Enables autolock function for a specific lock. |
| smartcam_capture | Capture a new image from a specific smartcam. |
