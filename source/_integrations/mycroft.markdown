---
title: Mycroft
description: Instructions on how to setup Mycroft AI within Home Assistant.
logo: mycroft.png
ha_category:
  - Voice
  - Notifications
ha_release: 0.53
---

[Mycroft](https://mycroft.ai) is an open source voice assistant that allows you to send notifications and more to Mycroft from Home Assistant.

There is currently support for the following device types within Home Assistant:

- **Notifications** - Allows to deliver notifications from Home Assistant to [Mycroft AI](https://mycroft.ai/).

## Configuration

To use Mycroft in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
mycroft:
  host: 0.0.0.0
```

{% configuration %}
host:
  description: The IP address of your Mycroft instance.
  required: true
  type: string
{% endconfiguration %}
