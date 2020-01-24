---
title: LG Netcast
description: Instructions on how to integrate a LG TV (Netcast 3.0 & 4.0) within Home Assistant.
logo: lg.png
ha_category:
  - Media Player
ha_iot_class: Local Polling
ha_release: '0.20'
---

The `lg_netcast` platform allows you to control a LG Smart TV running NetCast 3.0 (LG Smart TV models released in 2012) and NetCast 4.0 (LG Smart TV models released in 2013). For the new LG WebOS TV's use the [webostv](/integrations/webostv#media-player) platform.

To add a LG TV to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
media_player:
  - platform: lg_netcast
    host: 192.168.0.20
```

{% configuration %}
host:
  description: The IP address of the LG Smart TV, e.g., 192.168.0.20.
  required: true
  type: string
access_token:
  description: The access token needed to connect.
  required: false
  type: string
name:
  description: The name you would like to give to the LG Smart TV.
  required: false
  default: LG TV Remote
  type: string
{% endconfiguration %}

To get the access token for your TV configure the `lg_netcast` platform in Home Assistant without the `access_token`.
After starting Home Assistant the TV will display the access token on screen.
Just add the token to your configuration and restart Home Assistant and the media player integration for your LG TV will show up.

<div class='note'>
The access token will not change until you factory reset your TV.
</div>
