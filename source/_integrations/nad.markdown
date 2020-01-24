---
title: NAD
description: Instructions on how to integrate NAD receivers into Home Assistant.
logo: nad.png
ha_category:
  - Media Player
ha_release: 0.36
ha_iot_class: Local Polling
---

The `nad` platform allows you to control a [NAD receiver](https://nadelectronics.com/) through RS232, TCP and Telnet from Home Assistant.

To add an NAD receiver to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry for RS232 configuration
media_player:
  - platform: nad
    serial_port: /dev/ttyUSB0
```
```yaml
# Example configuration.yaml entry for TCP configuration
media_player:
  - platform: nad
    type: TCP
    host: IP_ADDRESS
```

{% configuration %}
type:
  description: Type of communication. Valid types are `RS232`, `Telnet` or `TCP`
  required: false
  default: RS232
  type: string
serial_port:
  description: The serial port. (for `RS232` type only)
  required: false
  default: /dev/ttyUSB0
  type: string
host:
  description: The IP address of your amplifier. (for `TCP` and `Telnet` types)
  required: false
  type: string
port:
  description: The port number of the device. (for `Telnet` type only)
  required: false
  default: 53
  type: integer
name:
  description: Name of the device.
  required: false
  default: NAD Receiver
  type: string
min_volume:
  description: Minimum volume in dB to use with the slider.
  required: false
  default: -92
  type: integer
max_volume:
  description: Maximum volume in dB to use with the slider.
  required: false
  default: -20
  type: integer
sources:
  description: A list of mappings from source to source name. Valid sources are `1 to 10`. (for `RS232` and `Telnet` types)
  required: false
  type: [list, string]
volume_step:
  description: The amount in dB you want to increase the volume with when pressing volume up/down. (for `TCP` type only)
  required: false
  default: 4
  type: integer
{% endconfiguration %}

The min_volume and max_volume are there to protect you against misclicks on the slider so you will not blow up your speakers when you go from -92dB to +20dB. You can still force it to go higher or lower than the values set with the plus and minus buttons.

<div class='note warning'>

On linux the user running home-assistant needs `dialout` permissions to access the serial port.
This can be added to the user by doing `sudo usermod -a -G dialout <username>`.
Be aware that the user might need to logout and logon again to activate these permissions.

</div>

A full configuration example could look like this:

```yaml
# Example configuration.yaml entry
media_player:
  - platform: nad
    serial_port: /dev/ttyUSB0
    name: NAD Receiver
    min_volume: -60
    max_volume: -20
    sources:
      1: 'Kodi'
      2: 'TV'
```
