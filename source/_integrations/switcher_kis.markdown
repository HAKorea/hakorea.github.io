---
title: Switcher
description: Controlling your Switcher V2 Water Heater.
logo: switcher_boiler.png
ha_category:
  - Switch
ha_release: 0.93
ha_iot_class: Local Push
ha_codeowners:
  - '@tomerfi'
---

This `Switcher` integration allows you to control the [Switcher V2 Water Heater](https://www.switcher.co.il/).

To enable it, add an entry to your `configuration.yaml` according to the following configuration instructions.

To retrieve your device's details, please follow the instructions [here](https://github.com/NightRang3r/Switcher-V2-Python).

<div class='note warning'>
  Please note, the Switcher-V2-Python script is written in python 2.7 syntax, it won't run with python 3.x.
</div>

<div class='note warning'>
  Please note, for the Switcher-V2-Python script to run successfully, you need to configure your device to work locally.
</div>

<div class='note warning'>

  Please note, on the original script repository, users recently reported difficulties controlling the device after upgrading the firmware to the new 3.0 version.As this integration is based on that script, please do not upgrade to version 3.0 until this issue is resolved. You can follow the issue [here](https://github.com/NightRang3r/Switcher-V2-Python/issues/3).

</div>

```yaml
switcher_kis:
  phone_id: 'REPLACE_WITH_PHONE_ID'
  device_id: 'REPLACE_WITH_DEVICE_ID'
  device_password: 'REPLACE_WITH_DEVICE_PASSWORD'
```

{% configuration %}
phone_id:
  description: The device's phone id.
  required: true
  type: string
device_id:
  description: The device's id.
  required: true
  type: string
device_password:
  description: The device's password.
  required: true
  type: string
{% endconfiguration %}

## Switch State Attributes

| Attribute          | Type    | Description                                            | Example           |
| ------------------ | ------- | ------------------------------------------------------ | ----------------- |
| `friendly_name`    | string  | Defaults to the device's configured name.              | "Switcher Boiler" |
| `auto_off_set`     | string  | The auto shutdown time limit configured on the device. | "01:30:00"        |
| `remaining_time`   | string  | Time remaining to shutdown (auto or timer).            | "01:29:41"        |
| `electric_current` | float   | The electric current in amps.                          | 12.5              |
| `current_power_w`  | integer | The current power used in watts.                       | 2756              |

<div class='note warning'>

  Please note, the following attributes are not eligible when the device is off and therefore will not appear as state attributes: `remaining_time`, `electric_current`, `current_power_w`.

</div>

## Services

### Service: `switcher_kis.set_auto_off`

You can use the `switcher_kis.set_auto_off` service to set the auto-off configuration setting for the device.

Meaning the device will turn itself off when reaching the auto-off configuration limit.

| Service Field | Mandatory | Description                                                                            | Example                    |
| ------------- | --------- | -------------------------------------------------------------------------------------- | -------------------------- |
| `entity_id`   | Yes       | Name of the entity id associated with the integration, used for permission validation. | switch.switcher_kis_boiler |
| `auto_off`    | Yes       | Time period string containing hours and minutes.                                       | "02:30"                    |
