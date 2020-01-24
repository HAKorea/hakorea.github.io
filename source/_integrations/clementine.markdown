---
title: Clementine Music Player
description: Instructions on how to integrate Clementine Music Player within Home Assistant.
logo: clementine.png
ha_category:
  - Media Player
ha_release: 0.39
ha_iot_class: Local Polling
---

The `clementine` platform allows you to control a [Clementine Music Player](https://www.clementine-player.org).

To add a Clementine Player to your Home Assistant installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
media_player:
  - platform: clementine
    host: 192.168.0.20
```

{% configuration %}
host:
  description: The IP address of the Clementine Player e.g., 192.168.0.20.
  required: true
  type: string
port:
  description: The remote control port.
  required: false
  default: 5500
  type: integer
access_token:
  description: The authorization code needed to connect.
  required: false
  type: integer
name:
  description: The name you would like to give to the Clementine player.
  required: false
  default: Clementine Remote
  type: string
{% endconfiguration %}

Remember that Clementine must be configured to accept connections through its network remote control protocol.

You can configure this through Clementine  `Tools > Preferences > Network remote control` configuration menu. Enable `Use network remote control` and configure the other options for your use case.

This integration does not implement the `play_media` service so you cannot add tracks to the playlist.
