---
title: ComEd Hourly Pricing
description: Instructions on how to set up the ComEd Hourly Pricing sensor in Home Assistant.
logo: comed.png
ha_category:
  - Energy
ha_release: '0.40'
ha_iot_class: Cloud Polling
---

The ComEd Hourly Pricing program is an optional program available to ComEd electric subscribers which charges customers a variable rate for electricity supply based on current demand rather than a traditional fixed rate. Live prices are published [here](https://hourlypricing.comed.com/live-prices/) and also via an [API](https://hourlypricing.comed.com/hp-api/) which we can integrate as a sensor in Home Assistant.

There are two price feeds available: the 5-minute price and current hour average price.

To use this sensor in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: comed_hourly_pricing
    monitored_feeds:
      - type: five_minute
      - type: current_hour_average
```

{% configuration %}
monitored_feeds:
  description: Feeds to monitor.
  required: true
  type: list
  keys:
    type:
      description: Name of the feed.
      required: true
      type: list
      keys:
        five_minute:
          description: The latest 5-minute price in cents.
        current_hour_average:
          description: The latest current hour average price in cents.
    name:
      description: Custom name for the sensor.
      required: false
      type: string
    offset:
      description: The pricing feeds provide only the *supply* cost of the electricity. The offset parameter allows you to provide a fixed constant that will be added to the pricing data to provide a more accurate representation of the total electricity cost per kWh.
      required: false
      default: 0.0
      type: float
{% endconfiguration %}
