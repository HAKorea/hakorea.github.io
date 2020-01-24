---
title: Etekcity VeSync
description: Instructions on how to set up Etekcity VeSync switches and outlets within Home Assistant.
logo: vesync.png
ha_category:
  - Switch
ha_release: 0.66
ha_config_flow: true
ha_codeowners:
  - '@markperdue'
  - '@webdjoe'
---

The `vesync` integration enables you to control Etekcity smart switches and outlets connected to the VeSync App.

The devices must be added to the VeSync App before this integration can discover them.

The following platforms are supported:

- **switch**

## Supported Devices

This integration supports devices controllable by the VeSync App.  The following devices are supported by this integration:

### Plugs

- Etekcity 7 Amp US outlet - ESW01-USA (Round)
- Etekcity 10 Amp US outlet - ESW10-USA (Round)
- Etekcity 10 Amp EU outlet - ESW10-EU (Round)
- Etekcity 15 Amp US outlet - ESW15-USA (Rectangular)
- Etekcity 2 Plug Outdoor Outlet - ESO15-TB

### Switches

- Etekcity In Wall Smart Switch (EWSL01-USA)

## Configuration

To use this integration, all devices must be registered with the VeSync App. Once registration is complete, you can add the VeSync integration by adding the VeSync integration in the configuration section of the frontend and entering your username and password.  You can also use the traditional configuration method by adding the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
vesync:
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
```

{% configuration %}
username:
  description: Username needed to log in to VeSync.
  required: true
  type: string
password:
  description: Password needed to log in to VeSync.
  required: true
  type: string
{% endconfiguration %}

## Services

| Service | Description |
|---------|-------------|
| `update_devices` | Poll Vesync server to find and add any new devices |

## Outlet Exposed Attributes

VeSync outlets will expose the following details for only the smart outlets. Energy monitoring not available for in-wall switches.

| Attribute               | Description                                                             | Example         |
| ----------------------- | ----------------------------------------------------------------------- | --------------- |
| `current_power_w`       | The present power consumption of the switch in watts.                   | 100             |
| `today_energy_kwh`      | The kilowatt hours used by the switch during the previous 24 hours.     | 0.12            |
| `voltage`               | Current voltage of the device                                           | 120.32          |
| `weekly_energy_total`   | Total energy usage for week starting from Monday 12:01AM in kWh         | 14.74           |
| `monthly_energy_total`  | Total energy usage for month starting from 12:01AM on the first in kWh  | 52.30           |
| `yearly_energy_total`   | Total energy usage for year start from 12:01AM on Jan 1 in kWh          | 105.25          |

## Extracting Attribute data

In order to get the attributes readings from supported devices, such as energy from outlets or fan attributes, you'll have to create a [template sensor](/integrations/switch.template/).

In the example below, change all of the `vesync_switch`'s to match your device's entity ID.

Adapted from the [TP-Link integration](https://www.home-assistant.io/integrations/tplink/#plugs).

{% raw %}

```yaml
sensor:
  - platform: template
      vesync_switch_watts:
        friendly_name_template: "{{ states.switch.vesync_switch.name}} Current Consumption"
        value_template: '{{ states.switch.vesync_switch.attributes["current_power_w"] | float }}'
        unit_of_measurement: 'W'
      vesync_switch_total_kwh:
        friendly_name_template: "{{ states.switch.vesync_switch.name}} Total Consumption"
        value_template: '{{ states.switch.vesync_switch.attributes["today_energy_kwh"] | float }}'
        unit_of_measurement: 'kWh'
      vesync_switch_volts:
        friendly_name_template: "{{ states.switch.vesync_switch.name}} Voltage"
        value_template: '{{ states.switch.vesync_switch.attributes["voltage"] | float }}'
        unit_of_measurement: 'V'
```

{% endraw %}
