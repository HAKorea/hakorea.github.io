---
title: Updater
description: Detecting when Home Assistant updates are available.
logo: home-assistant.png
ha_category:
  - Binary Sensor
ha_release: 0.8
ha_quality_scale: internal
ha_codeowners:
  - '@home-assistant/core'
---

The `updater` binary sensor will check daily for new releases. The state will be "on" when an update is available. Otherwise, the state will be "off". The newer version, as well as the link to the release notes, are attributes of the updater. As [Hass.io](/hassio/) has its own schedule for release it doesn't make sense to use this binary sensor on Hass.io.

The updater integration will also collect basic information about the running Home Assistant instance and its environment. The information includes the current Home Assistant version, the time zone, Python version and operating system information. No identifiable information (i.e., IP address, GPS coordinates, etc.) will ever be collected. If you are concerned about your privacy, you are welcome to scrutinize the Python [source code](https://github.com/home-assistant/home-assistant/tree/dev/homeassistant/components/updater).

<div class='note'>

The `updater` binary sensor will wait one hour after startup until it performs the first update. For this period it will be in the state `unavailable`. After that it will check once a day for new releases.

</div>

## Configuration

This integration is by default enabled, unless you've disabled or removed the [`default_config:`](https://www.home-assistant.io/integrations/default_config/) line from your configuration. If that is the case, the following example shows you how to enable this integration manually:

```yaml
updater:
```

{% configuration %}
reporting:
  description: Whether or not to share system information when checking for updates.
  required: false
  type: boolean
  default: true
include_used_components:
  description: Whether or not to report the integrations that you are using in Home Assistant.
  required: false
  type: boolean
  default: false
{% endconfiguration %}

For further information about the Updater's data, please check the [detailed overview](/docs/backend/updater/). If you choose not to share any information when checking for updates, you can set `reporting: false`.

It is possible to report the integrations that you are using to the Home Assistant developers. This will help them focus on improving the popular ones. To enable this option, you have to add `include_used_components: true`.

```json
"components": [
    "apcupsd",
    "api",
    "automation",
    "binary_sensor",
    "binary_sensor.zwave",
    "camera",
    "camera.uvc",
    "config",
    "config.core",
    ...
]
```

## Notification

For an added bonus, an automation integration can be created to send a message with a notifier when that state of this component's entity changes.

{% raw %}
```yaml
# Example configuration.yaml entry
automation:
  alias: Update Available Notification
  trigger:
    - platform: state
      entity_id: binary_sensor.updater
      from: 'off'
      to: 'on'
  action:
    - service: notify.notify
      data_template:
        message: "Home Assistant {{ state_attr('binary_sensor.updater', 'newest_version') }} is available."
```
{% endraw %}
