---
title: MH-Z19 CO2 Sensor
description: Instructions on how to integrate the MH-Z19 CO2 sensor with Home Assistant.
logo: mhz19_logo.png
ha_category:
  - DIY
ha_release: 0.27
ha_iot_class: Local Polling
---

The MH-Z19 is a small non-dispersive infrared sensor that can measure CO2 level. High CO2 levels can lead to drowsiness, poor concentration, loss of attention or increased heart rate. The CO2 level outside is around 400ppm, but inside levels can reach between 1000 and 5000 ppm. High CO2 levels indicate that you should increase ventilation.

Check out the [Open Home Automation web site](https://www.open-homeautomation.com/2016/08/24/monitor-co2-levels-in-your-house/) for a quick guide how to connect the sensor to your PC or Raspberry Pi.

**Note:** the new version MH-Z19B requires the VIN to be connected to a 5V pin, rather than 3.3V.

## Configuration

To use this sensor in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: mhz19
    serial_device: /dev/tty.SLAB_USBtoUART
```

- **serial_device** (*Required*): The serial port to use. On *nix systems, it can often be identified by `$ ls /dev/tty*`
- **name** (*Optional*): The name displayed in the frontend.
- **monitored_conditions** (*Optional*, starting from version 0.40): Conditions to monitor. Supported conditions:
  - **co2** (*default*)
  - **temperature**

Full example:
```yaml
sensor:
  - platform: mhz19
    serial_device: /dev/tty.SLAB_USBtoUART
    name: My MHZ19
    monitored_conditions:
      - co2
      - temperature
```

## Raspberry Pi 3(+) GPIO UART and Hass.io

To directly connect the sensor on the GPIO pins of a RPi, first append the following to `config.txt` in the boot directory:

```text
enable_uart=1
```

Then (after a reboot): you can setup the sensor using:

```yaml
  serial_device: /dev/ttyS0
```

## Calibration

The MH-Z19B version of the sensor has Automatic Baseline Calibration enabled by default, which will calibrate the 400PPM level to the lowest measured PPM in the last 24h cycle. Currently the integration does not allow turning this functionality off.
