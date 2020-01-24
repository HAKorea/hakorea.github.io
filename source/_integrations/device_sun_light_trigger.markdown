---
title: Presence-based Lights
description: Instructions on how to automate your lights with Home Assistant.
logo: home-assistant.png
ha_category:
  - Automation
ha_release: pre 0.7
ha_quality_scale: internal
---

Home Assistant has a built-in integration called `device_sun_light_trigger` to help you automate your lights. The integration will:

 * Fade in the lights when the sun is setting and there are people home
 * Turn on the lights when people get home after the sun has set
 * Turn off the lights when all people leave the house

This integration requires the integrations [sun](/integrations/sun/), [device_tracker](/integrations/device_tracker/), [person](/integrations/person/) and [light](/integrations/light/) to be enabled.

To enable this integration, add the following lines to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
device_sun_light_trigger:
```

{% configuration %}
light_group:
  description: Specify a specific light/group of lights that has to be turned on.
  required: false
  type: string
light_profile:
  description: Specify which light profile to use when turning lights on.
  required: false
  default: relax
  type: string
device_group:
  description: Specify which group to track. The group can contain device_trackers or persons.
  required: false
  type: string
disable_turn_off:
  description: Disable lights being turned off when everybody leaves the house.
  required: false
  default: false
  type: boolean
{% endconfiguration %}

A full configuration example could look like this:

```yaml
# Example configuration.yaml entry
device_sun_light_trigger:
  light_group: group.living_room
  light_profile: relax
  device_group: group.my_devices
  disable_turn_off: 1
```
