---
title: "MQTT Sensor"
description: "Instructions on how to integrate MQTT sensors within Home Assistant."
logo: mqtt.png
ha_category:
  - Sensor
ha_release: 0.7
ha_iot_class: Configurable
---

This `mqtt` sensor platform uses the MQTT message payload as the sensor value. If messages in this `state_topic` are published with *RETAIN* flag, the sensor will receive an instant update with last known value. Otherwise, the initial state will be undefined.

## Configuration

To use your MQTT sensor in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: mqtt
    state_topic: "home/bedroom/temperature"
```

{% configuration %}
state_topic:
  description: The MQTT topic subscribed to receive sensor values.
  required: true
  type: string
name:
  description: The name of the MQTT sensor.
  required: false
  type: string
  default: MQTT Sensor
qos:
  description: The maximum QoS level of the state topic.
  required: false
  type: integer
  default: 0
unit_of_measurement:
  description: Defines the units of measurement of the sensor, if any.
  required: false
  type: string
icon:
  description: The icon for the sensor.
  required: false
  type: icon
expire_after:
  description: Defines the number of seconds after the value expires if it's not updated.
  required: false
  type: integer
  default: 0
value_template:
  description: "Defines a [template](/docs/configuration/templating/#processing-incoming-data) to extract the value."
  required: false
  type: template
force_update:
  description: Sends update events even if the value hasn't changed. Useful if you want to have meaningful value graphs in history.
  reqired: false
  type: boolean
  default: false
availability_topic:
  description: The MQTT topic subscribed to receive availability (online/offline) updates.
  required: false
  type: string
payload_available:
  description: The payload that represents the available state.
  required: false
  type: string
  default: online
payload_not_available:
  description: The payload that represents the unavailable state.
  required: false
  type: string
  default: offline
json_attributes_topic:
  description: The MQTT topic subscribed to receive a JSON dictionary payload and then set as sensor attributes. Implies `force_update` of the current sensor state when a message is received on this topic.
  required: false
  type: string
json_attributes_template:
  description: "Defines a [template](/docs/configuration/templating/#processing-incoming-data) to extract the JSON dictionary from messages received on the `json_attributes_topic`."
  required: false
  type: template
json_attributes:
  description: (Deprecated, replaced by json_attributes_topic) A list of keys to extract values from a JSON dictionary payload and then set as sensor attributes.
  required: false
  type: [string, list]
unique_id:
  description: "An ID that uniquely identifies this sensor. If two sensors have the same unique ID, Home Assistant will raise an exception."
  required: false
  type: string
device_class:
  description: The [type/class](/integrations/sensor/#device-class) of the sensor to set the icon in the frontend.
  required: false
  type: device_class
  default: None
device:
  description: "Information about the device this sensor is a part of to tie it into the [device registry](https://developers.home-assistant.io/docs/en/device_registry_index.html). Only works through [MQTT discovery](/docs/mqtt/discovery/) and when [`unique_id`](#unique_id) is set."
  required: false
  type: map
  keys:
    identifiers:
      description: A list of IDs that uniquely identify the device. For example a serial number.
      required: false
      type: [string, list]
    connections:
      description: 'A list of connections of the device to the outside world as a list of tuples `[connection_type, connection_identifier]`. For example the MAC address of a network interface: `"connections": [["mac", "02:5b:26:a8:dc:12"]]`.'
      required: false
      type: list
    manufacturer:
      description: The manufacturer of the device.
      required: false
      type: string
    model:
      description: The model of the device.
      required: false
      type: string
    name:
      description: The name of the device.
      required: false
      type: string
    sw_version:
      description: The firmware version of the device.
      required: false
      type: string
{% endconfiguration %}

## Examples

In this section you find some real-life examples of how to use this sensor.

### JSON attributes topic configuration

The example sensor below shows a configuration example which uses a JSON dict: `{"ClientName": <string>, "IP": <string>, "MAC": <string>, "RSSI": <string>, "HostName": <string>, "ConnectedSSID": <string>}` in a separate topic `home/sensor1/attributes` to add extra attributes. It also makes use of the `availability` topic. Extra attributes will be displayed in the frontend and can also be extracted in [Templates](/docs/configuration/templating/#attributes). For example, to extract the `ClientName` attribute from the sensor below, use a template similar to: {% raw %}`{{ state_attr('sensor.bs_rssi', 'ClientName') }}`{% endraw %}.

{% raw %}
```yaml
# Example configuration.yaml entry
sensor:
  - platform: mqtt
    name: "RSSI"
    state_topic: "home/sensor1/infojson"
    unit_of_measurement: 'dBm'
    value_template: "{{ value_json.RSSI }}"
    availability_topic: "home/sensor1/status"
    payload_available: "online"
    payload_not_available: "offline"
    json_attributes_topic: "home/sensor1/attributes"
```
{% endraw %}

### JSON attributes template configuration

The example sensor below shows a configuration example which uses a JSON dict: `{"Timer1":{"Arm": <status>, "Time": <time>}, "Timer2":{"Arm": <status>, "Time": <time>}}` on topic `tele/sonoff/sensor` with a template to add `Timer1.Arm` and `Timer1.Time` as extra attributes.  Extra attributes will be displayed in the frontend and can also be extracted in [Templates](/docs/configuration/templating/#attributes). For example, to extract the `Arm` attribute from the sensor below, use a template similar to: {% raw %}`{{ state_attr('sensor.timer1', 'Arm') }}`{% endraw %}.

{% raw %}
```yaml
# Example configuration.yaml entry
sensor:
  - platform: mqtt
    name: "Timer 1"
    state_topic: "tele/sonoff/sensor"
    value_template: "{{ value_json.Timer1.Arm }}"
    json_attributes_topic: "tele/sonoff/sensor"
    json_attributes_template: "{{ value_json.Timer1 | tojson }}"
  - platform: mqtt
    name: "Timer 2"
    state_topic: "tele/sonoff/sensor"
    value_template: "{{ value_json.Timer2.Arm }}"
    json_attributes_topic: "tele/sonoff/sensor"
    json_attributes_template: "{{ value_json.Timer2 | tojson }}"
```
{% endraw %}

The state and the attributes of the sensor by design do not update in a synchronous manner if they share the same MQTT topic. Temporal mismatches between the state and the attribute data may occur if both the state and the attributes are changed simultaneously by the same MQTT message. An automation that triggers on any state change of the sensor will also trigger both on the change of the state or a change of the attributes. Such automations will be triggered twice if both the state and the attributes change. Please use a [MQTT trigger](/docs/automation/trigger/#mqtt-trigger) and process the JSON in the automation directly via the {% raw %}`{{ trigger.payload_json }}`{% endraw %} [trigger data](/docs/automation/templating/#mqtt) for automations that must synchronously handle multiple JSON values within the same MQTT message.

### Get battery level

If you are using the [OwnTracks](/integrations/owntracks) and enable the reporting of the battery level then you can use a MQTT sensor to keep track of your battery. A regular MQTT message from OwnTracks looks like this:

```bash
owntracks/tablet/tablet {"_type":"location","lon":7.21,"t":"u","batt":92,"tst":144995643,"tid":"ta","acc":27,"lat":46.12}
```

Thus the trick is extracting the battery level from the payload.

{% raw %}
```yaml
# Example configuration.yaml entry
sensor:
  - platform: mqtt
    name: "Battery Tablet"
    state_topic: "owntracks/tablet/tablet"
    unit_of_measurement: '%'
    value_template: "{{ value_json.batt }}"
```
{% endraw %}

### Get temperature and humidity

If you are using a DHT sensor and a NodeMCU board (esp8266), you can retrieve temperature and humidity with a MQTT sensor. A code example can be found [here](https://github.com/mertenats/open-home-automation/tree/master/ha_mqtt_sensor_dht22). A regular MQTT message from this example looks like this:

```json
office/sensor1
  {
    "temperature": 23.20,
    "humidity": 43.70
  }
```

Then use this configuration example to extract the data from the payload:

{% raw %}
```yaml
# Example configuration.yaml entry
sensor:
  - platform: mqtt
    name: "Temperature"
    state_topic: "office/sensor1"
    unit_of_measurement: '°C'
    value_template: "{{ value_json.temperature }}"
  - platform: mqtt
    name: "Humidity"
    state_topic: "office/sensor1"
    unit_of_measurement: '%'
    value_template: "{{ value_json.humidity }}"
```
{% endraw %}

### Get sensor value from a device with ESPEasy

Assuming that you have flashed your ESP8266 unit with [ESPEasy](https://github.com/letscontrolit/ESPEasy). Under "Config" set a name ("Unit Name:") for your device (here it's "bathroom"). A "Controller" for MQTT with the protocol "OpenHAB MQTT" is present and the entries ("Controller Subscribe:" and "Controller Publish:") are adjusted to match your needs. In this example the topics are prefixed with "home". Please keep in mind that the ESPEasy default topics start with a `/` and only contain the name when writing your entry for the `configuration.yaml` file.

- **Controller Subscribe**: `home/%sysname%/#` (instead of `/%sysname%/#`)
- **Controller Publish**: `home/%sysname%/%tskname%/%valname%` (instead of `/%sysname%/%tskname%/%valname%`)

Also, add a sensor in the "Devices" tap with the name "analog" and "brightness" as value.

As soon as the unit is online, you will get the state of the sensor.

```bash
home/bathroom/status Connected
...
home/bathroom/analog/brightness 290.00
```

The configuration will look like the example below:

{% raw %}
```yaml
# Example configuration.yaml entry
sensor:
  - platform: mqtt
    name: "Brightness"
    state_topic: "home/bathroom/analog/brightness"
```
{% endraw %}
