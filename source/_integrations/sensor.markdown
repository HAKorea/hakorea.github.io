---
title: Sensor
description: Instructions on how to setup your sensors with Home Assistant.
logo: home-assistant.png
ha_category:
  - Sensor
ha_release: 0.7
ha_quality_scale: internal
---

Sensors are gathering information about states and conditions.

Home Assistant currently supports a wide range of sensors. They are able to display information which are provides by Home Assistant directly, are gathered from web services, and, of course, physical devices.

## Device Class

The way these sensors are displayed in the frontend can be modified in the [customize section](/docs/configuration/customizing-devices/). The following device classes are supported for sensors:

- **None**: Generic sensor. This is the default and doesn't need to be set.
- **battery**: Percentage of battery that is left.
- **humidity**: Percentage of humidity in the air.
- **illuminance**: The current light level in lx or lm.
- **signal_strength**: Signal strength in dB or dBm.
- **temperature**: Temperature in °C or °F.
- **power**: Power in W or kW.
- **pressure**: Pressure in hPa or mbar.
- **timestamp**: Datetime object or timestamp string.

<p class='img'>
<img src='/images/screenshots/sensor_device_classes_icons.png' />
Example of various device class icons for sensors.
</p>
