---
title: StarLine
description: Instructions on how to setup your StarLine account with Home Assistant.
logo: starline.png
ha_category:
  - Car
  - Binary Sensor
  - Presence Detection
  - Lock
  - Sensor
  - Switch
ha_release: 0.103
ha_iot_class: Cloud Polling
ha_config_flow: true
ha_codeowners:
  - '@anonym-tsk'
---

The `starline` integration lets you retrieve data of your StarLine security system from the StarLine portal. You will need a working StarLine account.

This integration provides the following platforms:

- Binary Sensors: Hand brake, hood, trunk, alarm status and doors lock state.
- Device tracker: The location of your car.
- Lock: Control the lock of your car.
- Sensors: Battery level, SIM card balance, GSM signal level, interior temperature and engine temperature.
- Switches: Start/stop engine, heater (webasto), additional channel and sound the horn.
- Services: Update the state, set update frequency. More details can be found [here](#services).

## Configuration

1. Create a new application in the [StarLine developer profile](https://my.starline.ru/developer).
2. Open the integrations page and add the StarLine integration.
3. Fill in your application ID, secret, login and password.

After adding the integration, you can configure the API update interval using [`starline.set_scan_interval`](#set-scan-interval) service.

<div class='note'>

You can make up to 1000 API calls per day, which means you could make one approximately every 86 seconds.
By default, the state of integration will be updated every 3 minutes, making 480 calls per day.
It is not recommended to set an update interval of less than 90 seconds.

</div>

## Services

### Update the state

The service `starline.update_state` fetches the last state of the device from the StarLine server.

This service does not require any attributes.

### Set scan interval

The service `starline.set_scan_interval` sets update frequency for entities.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `scan_interval` | no | Update frequency in seconds.

## Disclaimer

This software is not affiliated with or endorsed by StarLine Company.
