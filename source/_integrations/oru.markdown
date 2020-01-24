---
title: Orange and Rockland Utility (ORU)
description: Instructions on how to integrate the Orange and Rockland Utility real-time energy usage sensor within Home Assistant.
logo: oru.png
ha_release: 0.101
ha_category:
  - Sensor
ha_iot_class: Cloud Polling
ha_codeowners:
  - '@bvlaicu'
---

[Orange and Rockland Utility](https://oru.com) is an energy provider in NY and NJ, USA.
The `oru` sensor platform fetches your current energy usage from your ORU smart meter.

## Configuration

To add the `oru` sensor to your installation, add your `meter_number` to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: oru
    meter_number: YOUR_METER_NUMBER
```

{% configuration %}
meter_number:
  description: The meter number of your smart meter with Orange and Rockland Utility. 
  required: true
  type: string
{% endconfiguration %}

`meter_number` is the smart meter number. It can be found on your energy bill, on the top left corner, alongside your account number.
