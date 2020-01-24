---
title: Twente Milieu
description: Instructions on how to integrate Twente Milieu with Home Assistant.
logo: twentemilieu.png
ha_category:
  - Sensor
  - Environment
ha_release: 0.97
ha_iot_class: Cloud Polling
ha_config_flow: true
ha_codeowners:
  - '@frenck'
---

The Twente Milieu integration allows you to track the next scheduled waste
pickups by Twente Milieu for each of the different waste types.

## Configuration via the frontend

Menu: **Configuration** -> **Integrations**.

Click on the `+` sign to add an integration and click on **Twente Milieu**.
Follow the configuration flow, after finishing, the Twente Milieu
integration will be available.

## Sensors

This integration provides sensors for the following waste pickup dates from Twente Milieu:

- Next plastic waste pickup date.
- Next organic waste pickup date.
- Next paper waste pickup date.
- Next non-recyclable waste pickup date.

## Services

The Twente Milieu integration exposes a service that allows you to manually update
the pickup date from Twente Milieu.

### Service `update`

Update pickup dates from Twente Milieu

| Service data attribute | Optional | Description                                                  |
| ---------------------- | -------- | ------------------------------------------------------------ |
| `id`                   | Yes      | The unique address ID to update.                             |
