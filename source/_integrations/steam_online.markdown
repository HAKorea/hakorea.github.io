---
title: Steam
description: Instructions on how to set up Steam sensors in Home Assistant.
logo: steam.png
ha_category:
  - Social
ha_iot_class: Cloud Polling
ha_release: 0.14
---

The `steam` sensor platform will allow you to track the online status of public [Steam](https://steamcommunity.com) accounts.

## Setup

You need a [free API key](https://steamcommunity.com/dev/apikey) to use the platform.

To find an account's 64-bit SteamID on profiles without a custom URL you can check the URL of the profile page, the long string of numbers at the end is the 64-bit SteamID. If the profile has a custom URL you will have to copy the URL into [STEAMID I/O](https://steamid.io/) to find the 64-bit SteamID.

## Configuration

To use Steam in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: steam_online
    api_key: YOUR_API_KEY
    accounts:
      - account1
      - account2
```

{% configuration %}
api_key:
  required: true
  description: Your API key from [https://steamcommunity.com/dev/apikey](https://steamcommunity.com/dev/apikey).
  type: string
accounts:
  required: true
  description: List of accounts.
  type: map
  keys:
    account_id:
      required: true
      description: The 64-bit SteamID.
      type: string
{% endconfiguration %}

## Examples

If you want to add the accounts to a group for example you will have to use:

```yaml
# Example configuration.yaml entry
group:
  steam:
    name: Steam
    entities:
      - sensor.steam_account1
      - sensor.steam_account2
```
