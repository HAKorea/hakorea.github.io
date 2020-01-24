---
title: Openweathermap
description: Instructions on how to integrate OpenWeatherMap within Home Assistant.
logo: openweathermap.png
ha_category:
  - Weather
  - Sensor
ha_release: 0.32
ha_iot_class: Cloud Polling
ha_codeowners:
  - '@fabaff'
---

The `openweathermap` weather platform uses [OpenWeatherMap](https://openweathermap.org/) as a source for current meteorological data for your location.

There is currently support for the following device types within Home Assistant:

- [Sensor](#sensor)
- [Weather](#weather)

You need an API key which is free but requires a [registration](https://home.openweathermap.org/users/sign_up).

## Weather

To add OpenWeatherMap to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
weather:
  - platform: openweathermap
    api_key: YOUR_API_KEY
```

{% configuration %}
api_key:
  description: Your API key for [OpenWeatherMap](https://openweathermap.org/).
  required: true
  type: string
name:
  description: Name to use in the frontend.
  required: false
  type: string
  default: OpenWeatherMap
mode:
  description: "Can specify `hourly`, `daily` of `freedaily`. Select `hourly` for a three-hour forecast, `daily` for daily forecast or `freedaily` for a five days forecast with the free tier."
  required: false
  type: string
  default: "`hourly`"
latitude:
  description: Latitude of the location to display the weather.
  required: false
  type: float
  default: "The latitude in your `configuration.yaml` file."
longitude:
  description: Longitude of the location to display the weather.
  required: false
  type: float
  default: "The longitude in your `configuration.yaml` file."
{% endconfiguration %}

<div class='note'>

This platform is an alternative to the [`openweathermap`](/integrations/openweathermap#sensor) sensor.

</div>

## Sensor

The `openweathermap` platform uses [OpenWeatherMap](https://openweathermap.org/) as a source for current meteorological data for your location. The `forecast` will show you the condition in 3h.

To add OpenWeatherMap sensor to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: openweathermap
    api_key: YOUR_API_KEY
    monitored_conditions:
      - weather
```

{% configuration %}
api_key:
  description: Your API key for OpenWeatherMap.
  required: true
  type: string
name:
  description: Additional name for the sensors. Default to platform name.
  required: false
  default: OWM
  type: string
forecast:
  description: Enables the forecast. The default is to display the current conditions.
  required: false
  default: false
  type: string
language:
  description: The language in which you want text results to be returned. It's a two-characters string, e.g., `en`, `es`, `ru`, `it`, etc.
  required: false
  default: en
  type: string
monitored_conditions:
  description: Conditions to display in the frontend.
  required: true
  type: list
  keys:
    weather:
      description: A human-readable text summary.
    temperature:
      description: The current temperature.
    wind_speed:
      description: The wind speed.
    wind_bearing:
      description: The wind bearing.
    humidity:
      description: The relative humidity.
    pressure:
      description: The sea-level air pressure in millibars.
    clouds:
      description: Description about cloud coverage.
    rain:
      description: The rain volume.
    snow:
      description: The snow volume.
    weather_code:
      description: The current weather condition code.
{% endconfiguration %}

Details about the API are available in the [OpenWeatherMap documentation](https://openweathermap.org/api).
