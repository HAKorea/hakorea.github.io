---
title: Serial
description: Instructions on how to integrate data from serial connected sensors into Home Assistant.
logo: home-assistant.png
ha_category:
  - Sensor
ha_release: 0.56
ha_iot_class: Local Polling
ha_codeowners:
  - '@fabaff'
---

The `serial` sensor platform is using the data provided by a device connected to the serial port of the system where Home Assistant is running. With [`ser2net`](http://ser2net.sourceforge.net/) and [`socat`](http://www.dest-unreach.org/socat/) would it also work for sensors connected to a remote system.

To check what kind of data is arriving at your serial port, use a command-line tool like `minicom` or `picocom` on Linux, on a macOS you can use `screen` or on Windows `putty`.

```bash
sudo minicom -D /dev/ttyACM0
```

## Configuration

To setup a serial sensor to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: serial
    serial_port: /dev/ttyACM0
```

{% configuration %}
serial_port:
  description: Local serial port where the sensor is connected and access is granted.
  required: true
  type: string
name:
  description: Friendly name to use for the frontend. Default to "Serial sensor".
  required: false
  type: string
baudrate:
  description: Baudrate of the serial port.
  required: false
  default: 9600 Bps
  type: integer
value_template:
  description: "Defines a [template](/docs/configuration/templating/#processing-incoming-data) to extract a value from the serial line."
  required: false
  type: template
{% endconfiguration %}

## `value_template` for Template sensor

### TMP36

{% raw %}
```yaml
"{{ (((states('sensor.serial_sensor') | float * 5 / 1024 ) - 0.5) * 100) | round(1) }}"
```
{% endraw %}

## Examples

### Arduino

For controllers of the Arduino family, a possible sketch to read the temperature and the humidity could look like the sample below.The returned data is in JSON format and can be split into the individual sensor values using a [template](/docs/configuration/templating/#processing-incoming-data).

```c
#include <ArduinoJson.h>

void setup() {
  Serial.begin(115200);
}

void loop() {
  StaticJsonDocument<100> jsonBuffer;

  jsonBuffer["temperature"] = analogRead(A0);
  jsonBuffer["humidity"] = analogRead(A1);

  serializeJson(jsonBuffer, Serial);
  Serial.println();
  
  delay(1000);
}
```

### Devices returning multiple sensors as a text string

For devices that return multiple sensors as a concatenated string of values with a delimiter, (i.e., the returned string is not JSON formatted) you can make several template sensors, all using the same serial response. For example, a stream from the [Sparkfun USB Weather Board](https://www.sparkfun.com/products/retired/9800) includes temperature, humidity and barometric pressure within it returned text string. Sample returned data:

```c
$,24.1,50,12.9,1029.83,0.0,0.00,*
$,24.3,51,12.8,1029.76,0.0,0.00,*
```

To parse this into individual sensors, split using the comma delimiter and then create a template sensor for each item of interest.

{% raw %}
```yaml
# Example configuration.yaml entry
sensor:
  - platform: serial
    serial_port: /dev/ttyUSB0
    baudrate: 9600

  - platform: template
    sensors:
      my_temperature_sensor:
        friendly_name: Temperature
        unit_of_measurement: "°C"
        value_template: "{{ states('sensor.serial_sensor').split(',')[1] | float }}"
      my_humidity_sensor:
        friendly_name: Humidity
        unit_of_measurement: "%"
        value_template: "{{ states('sensor.serial_sensor').split(',')[2] | float }}"
      my_barometer:
        friendly_name: Barometer
        unit_of_measurement: "mbar"
        value_template: "{{ states('sensor.serial_sensor').split(',')[4] | float }}"
```
{% endraw %}

### Digispark USB Development Board

This [blog post](/blog/2017/10/23/simple-analog-sensor/) describes the setup with a Digispark USB Development Board.
