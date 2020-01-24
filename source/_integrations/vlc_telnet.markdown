---
title: VLC media player Telnet
description: Instructions on how to integrate VLC media player into Home Assistant using the telnet interface.
logo: videolan.png
ha_category:
  - Media Player
ha_release: 0.95
ha_iot_class: Local Polling
ha_codeowners:
  - '@rodripf'
---

The `vlc_telnet` platform allows you to control a [VLC media player](https://www.videolan.org/vlc/index.html) using the built in telnet interface.

To add a VLC media player to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
media_player:
  - platform: vlc_telnet
    host: IP_ADDRESS
    password: PASSWORD
```

{% configuration %}
name:
  default: VLC-TELNET
  description: The name to use in the frontend.
  required: false
  type: string
pasword:
  description: The password to control the VLC through the telnet interface.
  required: true
  type: string
host:
  description: The hostname or IP address where the VLC Player is running.
  required: true
  type: string
port:
  default: 4212
  description: The port number where the VLC Player is running.
  required: false
  type: integer
{% endconfiguration %}

Only the "music" media type is supported for now.

This service will control any instance of VLC player on the network with the telnet interface activated. 
To activate the telnet interface on your VLC Player please read the [official VLC documentation](https://wiki.videolan.org/Documentation:Modules/telnet/). Also remember to add a firewall rule allowing inbound connections for the port used in the device running VLC.

## Full configuration

A full configuration for VLC could look like the one below:

```yaml
# Example configuration.yaml entry
media_player:
  - platform: vlc_telnet
    name: Remote Player
    host: 192.168.1.48
    port: 4212
    password: your-secure-password
```

##### Additional configuration for Rasperry Pi

You can run a VLC Media Player inside your Hass.io installation using the [hassio-local-vlc add-on](https://github.com/rodripf/hassio-local-vlc). Using it you can play files on the local network, Internet or files and playlist locally saved to the /share folder of your Hass.io installation.
