---
title: Tahoma
description: Instructions on how to integrate Somfy Tahoma devices with Home Assistant.
logo: tahoma.png
ha_category:
  - Hub
  - Binary Sensor
  - Cover
  - Scene
  - Switch
  - Sensor
ha_release: 0.59
ha_iot_class: Cloud Polling
ha_codeowners:
  - '@philklei'
---

The `Tahoma` integration platform is used as an interface to the [tahomalink.com](https://www.tahomalink.com) website. It adds covers, scenes and a sun sensor from the Tahoma platform.

There is currently support for the following device types within Home Assistant:

- Binary Sensor
- Sensor
- Cover
- Switch
- Scene

## Configuration

To use your Tahoma devices in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
tahoma:
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
  exclude: [BridgeHUEComponent, HueLampHUEComponent, PodComponent]
```

{% configuration %}
username:
  description: Your username for tahomalink.com.
  required: true
  type: string
password:
  description: Your password for tahomalink.com.
  required: true
  type: string
exclude:
  description: List of devices to exclude.
  required: false
  type: list
{% endconfiguration %}

This also works with the Somfy Connexoon. Check [here](https://somfyhouse.freshdesk.com/nl/support/solutions/articles/14000058145-wat-is-het-verschil-tussen-de-tahoma-en-de-connexoon-) for the differences between the bridges.
