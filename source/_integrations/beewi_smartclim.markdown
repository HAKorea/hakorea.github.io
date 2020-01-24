---
title: BeeWi SmartClim BLE sensor
description: Instructions on how to integrate MBeeWi SmartClim BLE sensor with Home Assistant.
logo: beewi_by_otio.png
ha_category:
  - Sensor
ha_release: 0.99
ha_iot_class: Local Polling
ha_codeowners:
  - '@alemuro'
---

The `beewi_smartclim` sensor platform allows one to monitor room or external temperature and humidity. The [BeeWi SmartClim BLE](http://www.bee-wi.com/produits/capteurs/capteur-de-temperature/) is a Bluetooth Low Energy sensor device that monitors temperature from a room or a garden from your smartphone by using an APP. Use this integration to track these metrics from any location thanks to Home Assistant, as well as to create some automation scripts based on your room's temperature.

## Installation

Depending on the operating system you're running, you have to configure the proper Bluetooth backend on your system:

- On [Hass.io](/hassio/installation/): `beewi_smartclim` will work out of the box as long as the host supports Bluetooth (like the Raspberry Pi does).
- On a [generic Docker installation](/docs/installation/docker/): Works out of the box with `--net=host` and properly configured Bluetooth on the host.
- On other Linux systems:
  - Preferred solution: Install the `bluepy` and `btlewrap` library (via pip). When using a virtual environment, make sure to use install the library in the right one.
  - Fallback solution: Install `btlewrap` library (via pip) and `gatttool` via your package manager. Depending on the distribution, the package name might be: `bluez`, `bluetooth` or    `bluez-deprecated`.
- Windows and MacOS are currently not supported by the `btlewrap` library.

## Configuration

Start a scan to determine the MAC addresses of the sensor:

```bash
$ sudo hcitool lescan
LE Scan ...
D0:5F:B8:51:9B:36 BeeWi SmartClim
[...]
```

Or if your distribution is using bluetoothctl:

```bash
$ bluetoothctl
[bluetooth]# scan on
Discovery started
[CHG] Controller XX:XX:XX:XX:XX:XX Discovering: yes
[NEW] Device D0:5F:B8:51:9B:36 BeeWi SmartClim
```

Check for `BeeWi SmartClim` or similar entries, those are your sensor.

To use your Mi Temperature and Humidity sensor in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: beewi_smartclim
    mac: 'xx:xx:xx:xx:xx:xx'
```

{% configuration %}
mac:
  description: The MAC address of your sensor.
  required: true
  type: string
name:
  description: The name displayed in the frontend.
  required: false
  type: string
{% endconfiguration %}

## Full example

A full configuration example could look like the one below:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: beewi_smartclim
    mac: 'xx:xx:xx:xx:xx:xx'
    name: Garden
```
