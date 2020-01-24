---
title: Yamaha MusicCast
description: Instructions on how to integrate Yamaha MusicCast Receivers into Home Assistant.
logo: yamaha.png
ha_category:
  - Media Player
ha_release: 0.53
ha_codeowners:
  - '@jalmeroth'
---

The `yamaha_musiccast` platform allows you to control [Yamaha MusicCast Receivers](https://usa.yamaha.com/products/audio_visual/hifi_components/index.html) from Home Assistant.

Supported devices are listed on their [German site](https://de.yamaha.com/de/products/contents/audio_visual/musiccast/products.html).

To add a Yamaha MusicCast Receiver to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
media_player:
  - platform: yamaha_musiccast
    host: 192.168.xx.xx
```

{% configuration %}
host:
  description: IP address or hostname of the device.
  required: true
  type: string
port:
  description: UDP source port. If multiple devices are present, specify a different port per device.
  required: false
  type: integer
  default: 5005
interval_seconds:
  description: Polling interval in seconds.
  required: false
  type: integer
  default: 480
{% endconfiguration %}

### Supported operations

Currently, this integration supports powering on/off, mute, volume control, and source selection. Playback controls, for instance, play and stop are available for sources that support it.

### Example configuration

A full configuration example will look like the sample below:
```yaml
# Example configuration.yaml entry
media_player:
  - platform: yamaha_musiccast
    host: 192.168.178.97
    port: 5005
```
