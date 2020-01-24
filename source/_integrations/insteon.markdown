---
title: Insteon
description: Instructions on how to set up an Insteon Modem (PLM or Hub) locally within Home Assistant.
logo: insteon.png
ha_category:
  - Hub
  - Binary Sensor
  - Cover
  - Fan
  - Light
  - Sensor
  - Switch
ha_iot_class: Local Push
ha_release: 0.39
---

This integration adds "local push" support for INSTEON Modems allowing linked INSTEON devices to be used within Home Assistant.

There is currently support for the following device types within Home Assistant:

- Binary Sensor
- Cover
- Fan
- Light
- Sensor
- Switch

Device support is provided by the underlying [insteonplm] package. It is known to work with the [2413U] USB and [2412S] RS242 flavors of PLM and the [2448A7] USB stick. It has also been tested to work with the [2242] and [2245] Hubs.

[insteonplm]: https://github.com/nugget/python-insteonplm
[2413U]: https://www.insteon.com/powerlinc-modem-usb
[2412S]: https://www.insteon.com/powerlinc-modem-serial
[2448A7]: https://www.smarthome.com/insteon-2448a7-portable-usb-adapter.html
[2245]: https://www.insteon.com/insteon-hub/
[2242]: https://www.insteon.com/support-knowledgebase/2014/9/26/insteon-hub-owners-manual

### INSTEON Modem configuration

To set up an INSTEON Powerline Modem (PLM) device such as the [2413U], use the following configuration:

```yaml
# PLM configuration variables
insteon:
  port: SERIAL_PORT
```

To set up an INSTEON Hub model [2245], use the following configuration:

```yaml
# Hub 2245 configuration variables
insteon:
  host: HOST
  ip_port: IP_PORT
  username: USERNAME
  password: PASSWORD
  hub_version: 2
```

To set up an INSTEON Hub model [2242], use the following configuration:

```yaml
# Hub 2242 configuration variables
insteon:
  host: HOST
  hub_version: 1
```

Additional configuration items are available:

```yaml
insteon:
  <PLM or Hub configuration>
  device_override:
     - address: ADDRESS
       cat: CATEGORY
       subcat: SUBCATEGORY
       firmware: FIRMWARE
       product_key: PRODUCT_KEY
  x10_devices:
     - housecode: HOUSECODE
       unitcode: UNITCODE
       platform: PLATFORM
       steps: STEPS
  x10_all_units_off: HOUSECODE
  x10_all_lights_on: HOUSECODE
  x10_all_lights_off: HOUSECODE
```

{% configuration %}
port:
  description: The serial or USB port for your device, e.g., `/dev/ttyUSB0` or `COM3`. Required for PLM setup.
  required: false
  type: string
host:
  description: The host name or IP address of the Hub. Required with Hub.
  required: false
  type: string
ip_port:
  description: The IP port number of the Hub. For Hub model [2245] (i.e. Hub version 2) the default port is 25105. For the Hub model [2242] (i.e. Hub version 1) the default port is 9761. Use the Insteon app to find the port number for your specific Hub. Optional with Hub.
  required: true
  type: integer
username:
  description: The username to login in to the local Hub. You can find your Hub username on the bottom of the Hub or you can use the Insteon app. Required for Hub version 2 setup.
  required: false
  type: string
password:
  description: The password to login in to the local Hub. You can find your Hub password on the bottom of the Hub or you can use the Insteon app. Required for Hub version 2 setup.
  required: false
  type: string
hub_version:
  description: The Hub version number where model [2242] is Hub version 1 and model [2245] is Hub version 2. Required for Hub version 1 setup.
  required: false
  default: 2
  type: integer
device_override:
  description: Override the default device definition.
  required: false
  type: list
  keys:
    address:
      description: is found on the device itself in the form 1A.2B.3C or 1a2b3c.
      required: true
      type: string
    cat:
      description: is found in the back of the device's User Guide in the form of 0x00 - 0xff.
      required: false
      type: integer
    subcat:
      description: is found in the back of the device's User Guide in the form of 0x00 - 0xff.
      required: false
      type: integer
    firmware:
      description: are more advanced options and will typically not be used.
      required: false
      type: string
    product_key:
      description: are more advanced options and will typically not be used.
      required: false
      type: integer
x10_devices:
  description: Define X10 devices to control or respond to.
  required: false
  type: list
  keys:
    housecode:
      description: is the X10 housecode values a - p
      required: true
      type: string
    unitcode:
      description: is the X10 unit code values 1 - 16
      required: true
      type: integer
    platform:
      description: "is the Home Assistant Platform to associate the device with. The following platforms are supported: binary_sensor: Used for on/off devices or keypad buttons that are read-only. light: Used for dimmable X10 devices. switch: Used for On/Off X10 devices."
      required: true
      type: string
    dim_steps:
      description: is the number of dim/bright steps the device supports. Used for dimmable X10 devices only.
      required: false
      default: 22
      type: integer
x10_all_units_off:
  description: Creates a binary_sensor that responds to the X10 standard command for All Units Off.
  required: false
  type: string
x10_all_lights_on:
  description: Creates a binary_sensor that responds to the X10 standard command for All Lights On
  required: false
  type: string
x10_all_lights_off:
  description: Creates a binary_sensor that responds to the X10 standard command for All Lights Off
  required: false
  type: string
{% endconfiguration %}

### Autodiscovery

The first time autodiscovery runs, the duration may require up to 20 seconds per device. Subsequent startups will occur much quicker using cached device information. If a device is not recognized during autodiscovery, you can add the device to the **device_override** configuration.

In order for a device to be discovered, it must be linked to the INSTEON Modem as either a responder or a controller.

### Linking Devices to the INSTEON Modem

In order for any two Insteon devices to talk with one another, they must be linked. For an overview of device linking, please read the Insteon page on [understanding linking]. The Insteon Modem module supports All-Linking through [Development Tools] service calls. The following services are available:

- **insteon.add_all_link**: Puts the Insteon Modem (IM) into All-Linking mode. The IM can be set as a controller or a responder. If the IM is a controller, put the IM into linking mode then press the SET button on the device. If the IM is a responder, press the SET button on the device then put the IM into linking mode.
- **insteon.delete_all_link**: Tells the Insteon Modem (IM) to remove an All-Link record from the All-Link Database of the IM and a device. Once the IM is set to delete the link, press the SET button on the corresponding device to complete the process.
- **insteon.load_all_link_database**: Load the All-Link Database for a device. WARNING - Loading a device All-Link database may take a LONG time and may need to be repeated to obtain all records.
- **insteon.print_all_link_database**: Print the All-Link Database for a device. Requires that the All-Link Database is loaded first.
- **insteon.print_im_all_link_database**: Print the All-Link Database for the INSTEON Modem (IM).

If you are looking for more advanced options, you can use the [insteonplm_interactive] command line tool that is distributed with the [insteonplm] Python module. Please see the documentation on the [insteonplm] GitHub site. Alternatively, you can download [HouseLinc] which runs on any Windows PC, or you can use [Insteon Terminal] which is open source and runs on most platforms. SmartHome no longer supports HouseLinc, but it still works. Insteon Terminal is a very useful tool but please read the disclaimers carefully, they are important.

[understanding linking]: https://www.insteon.com/support-knowledgebase/2015/1/28/understanding-linking
[Development Tools]: /docs/tools/dev-tools/
[HouseLinc]: https://www.smarthome.com/houselinc.html
[Insteon Terminal]: https://github.com/pfrommerd/insteon-terminal
[insteonplm_interactive]: https://github.com/nugget/python-insteonplm#command-line-interface

### Customization

The only configuration item that is necessary is the PLM port or Hub IP address, username and password so that Home Assistant can connect to the INSTEON Modem. This will expose all the supported INSTEON devices which exist in the modem’s ALL-Link database. However, devices will only be shown by their INSTEON hex address (e.g., “1A.2B.3C”) which can be a bit unwieldy. As you link and unlink devices using the ‘Set’ buttons, they’ll be added and removed from Home Assistant automatically.

You can use the normal Home Assistant [device customization] section of your configuration to assign friendly names and special icons to your devices. This is especially useful for setting device_class on your binary_sensor INSTEON devices.

[device customization]: /getting-started/customizing-devices/

### Device Overrides

INSTEON devices are added to Home Assistant using the platform(s) that make the most sense given the model and features of the hardware. The features of the INSTEON devices are built into the Home Assistant platform. Changing the platform is not recommended.

There are two primary uses for the **device_override** feature:

- Devices that do not respond during autodiscovery. This is common for battery operated devices.
- Devices that have not been fully developed. This allows an unknown device to be mapped to a device that operates similarly to another device.

### Example Configuration with Options

```yaml
# Full example of Insteon configuration with customizations and overrides

homeassistant:
  customize:
    light.a1b2c3:
      friendly_name: Bedside Lamp
    binary_sensor.a2b3c4:
      friendly_name: Garage Door
      device_class: opening

insteon:
  port: /dev/ttyUSB0
  device_override:
    - address: a1b2c3  # Hidden Door Sensor [2845-222]
      cat: 0x10
      subcat: 0x11
```

### What NOT to do

Insteon Modem is a top-level integration and device discovery will identify the Home Assistant platform the device belongs in. As such, do not declare Insteon devices in other platforms. For example, this configuration will NOT work:

```yaml
light:
  - platform: insteon
    address: 1a2b3c
```

### INSTEON Scenes

Trigger an INSTEON scene on or off is done via automations. Two services are provided to support this feature:

- **insteon.scene_on**
  - **group**: (required) The INSTEON scene number to trigger.
- **insteon.scene_off**
  - **group**: (required) The INSTEON scene to turn off

```yaml
automation:
  # Trigger an INSTEON scene 25
  - id: trigger_scene_25_on
    alias: Turn on scene 25
    action:
      - service: insteon.scene_on
        group: 25
```

### Events and Mini-Remotes

Mini-Remote devices do not appear as Home Assistant entities, they generate events. The following events are available:

- **insteon.button_on**
  - **address**: (required) The Insteon device address in lower case without dots (e.g., 1a2b3c)
  - **button**: (Optional) The button id in lower case. For a 4-button remote the values are `a` to `d`. For an 8 button remote the values are `a` to `h`. For a one-button remote this field is not used.
- **insteon.button_off**
  - **address**: (required) The Insteon device address in lower case without dots (e.g., 1a2b3c)
  - **button**: (Optional) The button id in lower case. For a 4-button remote the values are a to d. For an 8 button remote the values are `a` to `h`. For a one-button remote this field is not used.

This allows the mini-remotes to be configured as triggers for automations. Here is an example of how to use these events for automations:

```yaml
automation:
  # 4 or 8 button remote with button c pressed
  - id: light_on
    alias: Turn a light on
    trigger:
      - platform: event
        event_type: insteon.button_on
    event_data:
      address: 1a2b3c
      button: c
    condition:
      - condition: state
        entity_id: light.some_light
        state: 'off'
    action:
      - service: light.turn_on
        entity_id: light.some_light

  # single button remote
  - id: light_off
    alias: Turn a light off
    trigger:
      - platform: event
        event_type: insteon.button_on
    event_data:
      address: 1a2b3c
    condition:
      - condition: state
        entity_id: light.some_light
        state: 'off'
    action:
      - service: light.turn_on
        entity_id: light.some_light
```

### Known Issues with the INSTEON Hub

The INSTEON Hub has three known issues that are inherent to the design of the Hub:

1. If you see multiple error messages in the log file stating the Hub connection is closed, and reconnection has failed, this generally requires the Hub to be restarted to reconnect.

2. You cannot use both Home Assistant and the INSTEON app. If you do, the changes made in the app will not appear in Home Assistant. Changes made in Home Assistant will appear in the app after a period of time, however.

3. The Hub response times can be very slow. This is due to the Hub polling devices frequently. Since only one INSTEON message can be broadcast at a time, messages to and from Home Assistant can be delayed.
