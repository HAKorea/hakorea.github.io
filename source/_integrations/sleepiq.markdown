---
title: SleepIQ
description: Instructions for how to integrate SleepIQ beds within Home Assistant.
logo: sleepiq.png
ha_category:
  - Health
  - Sensor
  - Binary Sensor
ha_release: 0.29
ha_iot_class: Local Polling
---

The SleepIQ implementation lets you view sensor data from [SleepIQ by SleepNumber](https://www.sleepnumber.com/sleepiq-sleep-tracker). In particular, it lets you see the occupancy and current SleepNumber (ie current firmness) of each side of a SleepNumber bed.

## Setup

You will need an account on [SleepIQ](https://sleepiq.sleepnumber.com/) to use this component.

## Configuration

To set it up, add the following information to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sleepiq:
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
```

{% configuration %}
username:
  description: Your SleepIQ username (usually an e-mail address).
  required: true
  type: string
password:
  description: Your SleepIQ password.
  required: true
  type: string
{% endconfiguration %}
