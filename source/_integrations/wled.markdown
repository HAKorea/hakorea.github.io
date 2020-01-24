---
title: WLED
description: Instructions on how to integrate WLED with Home Assistant.
logo: wled.png
ha_category:
  - Light
  - Sensor
  - Switch
ha_release: 0.102
ha_iot_class: Local Polling
ha_config_flow: true
ha_quality_scale: platinum
ha_codeowners:
  - '@frenck'
---

[WLED](https://github.com/Aircoookie/WLED) is a fast and feature-rich
implementation of an ESP8266/ESP32 webserver to control
NeoPixel (WS2812B, WS2811, SK6812, APA102, and similar) LED's.

## Configuration

This integration can be configured using the integrations in the
Home Assistant frontend.

Menu: **Configuration** -> **Integrations**.

In most cases, the WLED devices will be automatically discovered by
Home Assistant. Those automatically discovered WLED devices are listed
on the integrations page.

If for some reason (e.g., due to lack of mDNS support on your network),
the WLED device isn't discovered, it can be added manually.

Click on the `+` sign to add an integration and click on **WLED**.
After completing the configuration flow, the WLED
integration will be available.

## Lights

This integration adds the WLED device as a light in Home Assistant.
Home Assistant treats every segment of the LED strip as a separate light
entity.

Only native supported features of a light in Home Assistant are supported
(which includes effects).

## Sensors	

This integration provides sensors for the following information from WLED:	

- Estimated current.	
- Uptime.	
- Free memory.

## Switches

The integration will create a number of switches:

- Nightlight.
- Sync Receive.
- Sync Send.

## Services

This integration currently does not offer any additional services.
