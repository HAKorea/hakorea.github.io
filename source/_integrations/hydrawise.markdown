---
title: Hunter Hydrawise
description: Instructions on how to integrate your Hunter Hydrawise Wi-Fi irrigation control system within Home Assistant.
logo: hydrawise_logo.png
ha_category:
  - Irrigation
  - Binary Sensor
  - Sensor
  - Switch
ha_release: 0.71
ha_iot_class: Cloud Polling
---

The `hydrawise` integration allows you to integrate your [Hunter Hydrawise](https://hydrawise.com) Wi-Fi irrigation controller system in Home Assistant.

There is currently support for the following device types within Home Assistant:

- [Binary Sensor](#binary-sensor)
- [Sensor](#sensor)
- [Switch](#switch)

## Configuration

To enable it, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
hydrawise:
  access_token: YOUR_API_KEY
```

{% configuration %}
access_token:
  description: The API KEY assigned to your Hydrawise account.
  required: true
  type: string
scan_interval:
  description: The time interval, in seconds, to poll the Hydrawise cloud.
  required: false
  type: integer
  default: 30
{% endconfiguration %}

To get your API access token log into your [Hydrawise account](https://app.hydrawise.com/config/login) and in the 'My Account Details' section under Account Settings click 'Generate API Key'. Enter that key in your configuration file as the `API_KEY`.

## Binary Sensor

Once you have enabled the `hydrawise` component, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
binary_sensor:
  - platform: hydrawise
```

{% configuration %}
monitored_conditions:
  description: The binary sensors that should be displayed on the frontend.
  required: false
  type: list
  default: All binary sensors are enabled.
  keys:
    is_watering:
      description: The binary sensor is `on` when the zone is actively watering.
    rain_sensor:
      description: Is `on` when the rain_sensor (if installed on the controller) is active (wet).
    status:
      description: This will indicate `on` when there is a connection to the Hydrawise cloud. It is not an indication of whether the irrigation controller hardware is online.
{% endconfiguration %}

## Sensor

Once you have enabled the `hydrawise` component, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: hydrawise
```

{% configuration %}
monitored_conditions:
  description: The sensors that should be displayed on the frontend.
  required: false
  type: list
  default: All sensors are enabled.
  keys:
    watering_time:
      description: The amount of time left if the zone is actively watering. Otherwise the time is 0.
    next_cycle:
      description: The day and time when the next scheduled automatic watering cycle will start. If the zone is suspended then the value will be `NS` to indicate Not Scheduled.
  {% endconfiguration %}

## Switch

Once you have enabled the `hydrawise` component, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
switch:
  - platform: hydrawise
```

{% configuration %}
watering_minutes:
  description: When manual watering is enabled this will determine the length of time in minutes that the irrigation zone will run. The allowed values are 5, 10, 15, 30, 45, or 60.
  required: false
  type: integer
  default: 15
monitored_conditions:
  description: Selects the set of switches that should be enabled on the frontend. Also sets the length of time a zone will run under manual control.
  required: false
  type: list
  default: All switches are enabled.
  keys:
    auto_watering:
      description: Enables the Smart Watering features for this zone.
    manual_watering:
      description: Enables the manual watering control for this zone.
{% endconfiguration %}

### Switch Operation

When `auto_watering` is `on` the irrigation zone will follow the Smart Watering schedule set through the Hydrawise [mobile or web app](https://www.hydrawise.com). When the `auto_watering` switch is `off` the zone's Smart Watering schedule is suspended for 1 year.

When `manual_watering` is `on` the zone will run for the amount of time set by `watering_minutes`.

```yaml
# An example that enables all the switches, and sets the manual watering time to 20 minutes.
switch:
  - platform: hydrawise
    watering_minutes: 20
```

```yaml
# An example that enables only the manual control switches.
switch:
  - platform: hydrawise
    monitored_conditions: manual_watering
```
