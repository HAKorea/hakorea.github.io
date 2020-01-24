---
title: LaCrosse
description: Instructions on how to integrate LaCrosse sensor data received from Jeelink into Home Assistant.
logo: home-assistant.png
ha_category:
  - DIY
ha_release: 0.58
ha_iot_class: Local Polling
---

The `lacrosse` sensor platform is using the data provided by a [Jeelink](https://www.digitalsmarties.net/products/jeelink) USB dongle or this [Arduino sketch](https://svn.fhem.de/trac/browser/trunk/fhem/contrib/arduino/36_LaCrosse-LaCrosseITPlusReader.zip).

## Tested Devices

- Technoline TX 29 IT (temperature only)
- Technoline TX 29 DTH-IT (including humidity)

## Setup

Since the sensor change their ID after each powercycle/battery change you can check what sensor IDs are available by using the command-line tool `pylacrosse` from the pylacrosse package.

```bash
sudo pylacrosse -d /dev/ttyUSB0 scan
```

Or, when using Docker containers:

```bash
docker exec -it <containername> pylacrosse -d /dev/ttyUSB0 scan
```

## Configuration

To use your `lacrosse` compatible sensor in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: lacrosse
    sensors:
      sensor_identifier:
        type: SENSOR_TYPE
        id: SENSOR_ID
```

{% configuration %}
  device:
    description: The serial device.
    required: true
    type: string
    default: /dev/ttyUSB0
  baud:
    description: The serial baudrate.
    required: true
    type: integer
    default: 57600
  led:
    description: Activate or deactivate the Jeelink LED.
    required: false
    type: boolean
    default: false
  frequency:
    description: Initial frequency in 5kHz steps.
    required: false
    type: integer
  datarate:
    description: "Set the data rate in kbps. Special values for well-known settings are: `0`: 17.241 kbps, `1`: 9.579 kbps, `2`: 8.842 kbps."
    required: false
    type: integer
  toggle_mask:
    description: "The following values can be combined bitwise: `1` = 17.241 kbps, `2` = 9.579 kbps, `4` = 8.842 kbps"
    required: false
    type: integer
  toggle_interval:
    description: Enable the toggle mode and set the interval in seconds.
    required: false
    type: integer
  sensors:
    description: A list of your sensors.
    required: true
    type: map
    keys:
      name:
        description: The name of the sensor.
        required: false
        type: string
      type:
        description: "The type of the sensor. Options: `battery`, `humidity`, `temperature`"
        required: true
        type: string
      id:
        description: The LaCrosse Id of the sensor.
        required: true
        type: integer
{% endconfiguration %}

## Examples

To setup a lacrosse sensor with multiple sensors, add the following to your `configuration.yaml` file:

{% raw %}
```yaml
# Example configuration.yaml entry
sensor:
  - platform: lacrosse
    device: /dev/ttyUSB0
    baud: 57600
    sensors:
      kitchen_humidity:
        name: Kitchen Humidity
        type: humidity
        id: 72
      kitchen_temperature:
        name: Kitchen Temperature
        type: temperature
        id: 72
      kitchen_lacrosse_battery:
        name: Kitchen Sensor Battery
        type: battery
        id: 72
```
{% endraw %}
