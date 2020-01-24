---
title: Abode
description: Instructions on integrating Abode home security with Home Assistant.
logo: abode.jpg
ha_category:
  - Hub
  - Alarm
  - Binary Sensor
  - Camera
  - Cover
  - Light
  - Lock
  - Sensor
  - Switch
ha_release: 0.52
ha_iot_class: Cloud Push
ha_config_flow: true
ha_codeowners:
  - '@shred86'
---

The `abode` integration will allow users to integrate their Abode Home Security systems into Home Assistant and use its alarm system and sensors to automate their homes.

Please visit the [Abode website](https://goabode.com/) for further information about Abode Security.

There is currently support for the following device types within Home Assistant:

- **Alarm Control Panel**: Reports on the current alarm status and can be used to arm and disarm the system.
- [**Binary Sensor**](/integrations/abode/#binary-sensor): Reports on `Quick Actions`, `Door Contacts`, `Connectivity` sensors (remotes, keypads, and status indicators), `Moisture` sensors, and `Motion` or `Occupancy` sensors. Also lists all Abode `Quick Actions` that are set up. You can trigger these quick actions by passing the `entity_id` of your quick action binary sensor to the [trigger_quick_action service](/integrations/abode/#trigger_quick_action).
- **Camera**: Reports on `Camera` devices and will download and show the latest captured still image.
- **Cover**: Reports on `Secure Barriers` and can be used to open and close the cover.
- **Lock**: Reports on `Door Locks` and can be used to lock and unlock the door.
- [**Light**](/integrations/abode/#light): Reports on `Dimmer` lights and can be used to dim or turn the light on and off.
- [**Switch**](/integrations/abode/#switch): Reports on `Power Switch` devices and can be used to turn the power switch on and off. Also reports on `Automations` set up in the Abode system and allows you to activate or deactivate them (does not work with Abode's CUE automations).
- **Sensor**: Reports on `Temperature`, `Humidity`, and `Light` sensors.

## Configuration

To use Abode devices in your installation, add your Abode account from the integrations page. Two-factor authentication must be disabled on your Abode account. Alternatively, Abode can be configured by adding the following `abode` section to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
abode:
  username: abode_username
  password: abode_password
```

{% configuration %}
username:
  description: Username for your Abode account.
  required: true
  type: string
password:
  description: Password for your Abode account.
  required: true
  type: string
polling:
  description: >
    Enable polling if cloud push updating is less reliable.
    Will update the devices once every 30 seconds.
  required: false
  type: boolean
  default: false
{% endconfiguration %}

## Events

There are a number of events that can be triggered from Abode.
They are grouped into the below events:

- **abode_alarm**: Fired when an alarm event is triggered from Abode. This includes Smoke, CO, Panic, and Burglar alarms.
- **abode_alarm_end**: Fired when an alarm end event is triggered from Abode.
- **abode_automation**: Fired when an Automation is triggered from Abode.
- **abode_panel_fault**: Fired when there is a fault with the Abode hub. This includes events like loss of power, low battery, tamper switches, polling failures, and signal interference.
- **abode_panel_restore**: Fired when the panel fault is restored.
- **abode_disarm**: Fired when the alarm is disarmed.
- **abode_arm**: Fired when the alarm is armed (home or away).
- **abode_test**: Fired when a sensor is in test mode.
- **abode_capture**: Fired when an image is captured.
- **abode_device**: Fired for device changes/additions/deletions.
- **abode_automation_edit**: Fired for changes to automations.

All events have the fields:

Field | Description
----- | -----------
`device_id` | The Abode device ID of the event.
`device_name` | The Abode device name of the event.
`device_type` | The Abode device type of the event.
`event_code` | The event code of the event.
`event_name` | The name of the event.
`event_type` | The type of the event.
`event_utc` | The UTC timestamp of the event.
`user_name` | The Abode user that triggered the event, if applicable.
`app_type` | The Abode app that triggered the event (e.g. web app, iOS app, etc.).
`event_by` | The keypad user that triggered the event.
`date` | The date of the event in the format `MM/DD/YYYY`.
`time` | The time of the event in the format `HH:MM AM`.

There is a unique list of known event_codes that can be found
[here](https://github.com/MisterWil/abodepy/files/1262019/timeline_events.txt).

## Services

### Service `change_setting`

Change settings on your Abode system.
For a full list of settings and valid values, consult the
[AbodePy settings section](https://github.com/MisterWil/abodepy/blob/master/README.rst#settings).

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `setting` | No | The setting you wish to change.
| `value` | No | The value you wish to change the setting to.

### Service `capture_image`

Request a new still image from your Abode IR camera.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | No | String or list of strings that point at `entity_id`s of Abode cameras.

### Service `trigger_quick_action`

Trigger a quick action automation on your Abode system.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | No | String or list of strings that point at `entity_id`s of binary_sensors that represent your Abode quick actions.
