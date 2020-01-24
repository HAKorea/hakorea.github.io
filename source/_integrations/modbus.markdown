---
title: Modbus
description: Instructions on how to integrate Modbus within Home Assistant.
logo: modbus.png
ha_category:
  - Hub
ha_release: pre 0.7
ha_iot_class: Local Push
ha_codeowners:
  - '@adamchengtkc'
---

[Modbus](http://www.modbus.org/) is a serial communication protocol to control PLCs (Programmable logic controller).
It currently supports sensors and switches which can be controlled over serial, TCP, and UDP connections.

## Configuration

The configuration for adding modbus to your installation depends on the connection type, either a network or serial connection.

### Network connection

For a network connection, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry for a TCP connection
modbus:
  name: hub1
  type: tcp
  host: IP_ADDRESS
  port: 2020
```

{% configuration %}
type:
  description: Type of the connection to Modbus. Possible values are `tcp` (Modbus TCP protocol according to "MODBUS Messaging Implementation Guide version 1.0b" provided by Schneider Automation.), `udp`(Modbus TCP form, but using UDP for transport. It removes the overheads required for TCP.) and `rtuovertcp` (Modbus RTU message transmitted with a TCP/IP wrapper and sent over a network instead of serial lines.).
  required: true
  type: string
host:
  description: The IP address of your Modbus device, e.g., 192.168.1.1.
  required: true
  type: string
port:
  description: The network port for the communication.
  required: true
  type: integer
name:
  description: Name for this hub. Must be unique, so it is required when setting up multiple instances.
  required: false
  default: default
  type: string
timeout:
  description: Timeout for slave response in seconds.
  required: false
  default: 3
  type: integer
{% endconfiguration %}

### Serial connection

For a serial connection, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry for a serial connection
modbus:
  name: hub1
  type: serial
  method: rtu
  port: /dev/ttyUSB0
  baudrate: 9600
  stopbits: 1
  bytesize: 8
  parity: N
```

{% configuration %}
type:
  description: "Type of the connection to Modbus, needs to be `serial` for this setup."
  required: true
  type: string
method:
  description: "Method of the connection to Modbus, either `rtu` or `ascii`."
  required: true
  type: string
port:
  description: The port where your Modbus device is connected to your Home Assistant host.
  required: true
  type: string
baudrate:
  description: The speed for the serial connection.
  required: true
  type: integer
stopbits:
  description: "The stopbits for the serial connection, either `1` or `2`."
  required: true
  type: integer
bytesize:
  description: "The bytesize for the serial connection; can be `5`, `6`, `7` or `8`."
  required: true
  type: integer
parity:
  description: "The parity for the serial connection; can be `E`, `O` or `N`."
  required: true
  type: string
name:
  description: Name for this hub. Must be unique, so it is required when setting up multiple instances.
  required: false
  default: default
  type: string
timeout:
  description: Timeout for slave response in seconds.
  required: false
  default: 3
  type: integer
{% endconfiguration %}

### Multiple connections

Multiple connections are possible, add something like the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry for multiple TCP connections
modbus:
  - type: tcp
    host: IP_ADDRESS_1
    port: 2020
    name: hub1

  - type: tcp
    host: IP_ADDRESS_2
    port: 501
    name: hub2
```

### Services


| Service | Description |
| ------- | ----------- |
| write_register | Write register. Requires `hub`, `unit`, `address` and `value` fields. `value` can be either single value or an array |

#### Service Data Attributes

| Attribute | Description |
| --------- | ----------- |
| hub       | Hub name (defaults to 'default' when omitted) |
| unit      | Slave address (set to 255 you talk to Modbus via TCP) |
| address   | Address of the Register (e.g., 138) |
| value     | A single value or an array of 16-bit values. Single value will call modbus function code 6. Array will call modbus function code 16. Array might need reverse ordering. E.g., to set 0x0004 you might need to set `[4,0]` |

## Building on top of Modbus

 - [Modbus Binary Sensor](/integrations/binary_sensor.modbus/)
 - [Modbus Climate](/integrations/climate.modbus/)
 - [Modbus Sensor](/integrations/sensor.modbus/)
 - [Modbus Switch](/integrations/switch.modbus/)
