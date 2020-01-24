---
title: RESTful
description: Instructions on how to integrate REST sensors into Home Assistant.
logo: restful.png
ha_category:
  - Sensor
ha_release: 0.7.4
ha_iot_class: Local Polling
---

The `rest` sensor platform is consuming a given endpoint which is exposed by a [RESTful API](https://en.wikipedia.org/wiki/Representational_state_transfer) of a device, an application, or a web service. The sensor has support for GET and POST requests.

To enable this sensor, add the following lines to your `configuration.yaml` file for a GET request:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: rest
    resource: http://IP_ADDRESS/ENDPOINT
```

or for a POST request:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: rest
    resource: http://IP_ADDRESS/ENDPOINT
    method: POST
    payload: '{ "device" : "heater" }'
```

or a template based request:

{% raw %}

```yaml
# Example configuration.yaml entry
sensor:
  - platform: rest
    resource_template: http://IP_ADDRESS/{{ now().strftime('%Y-%m-%d') }}
```

{% endraw %}

{% configuration %}
resource:
  description: The resource or endpoint that contains the value.
  required: true
  type: string
resource_template:
  description: The resource or endpoint that contains the value with template support.
  required: true
  type: template
method:
  description: The method of the request. Either `POST` or `GET`.
  required: false
  type: string
  default: GET
name:
  description: Name of the REST sensor.
  required: false
  type: string
  default: REST Sensor
device_class:
  description: Sets the [class of the device](/integrations/sensor/), changing the device state and icon that is displayed on the frontend.
  required: false
  type: string
value_template:
  description: "Defines a [template](/docs/configuration/templating/#processing-incoming-data) to extract the value."
  required: false
  type: template
payload:
  description: The payload to send with a POST request. Depends on the service, but usually formed as JSON.
  required: false
  type: string
verify_ssl:
  description: Verify the SSL certificate of the endpoint.
  required: false
  type: boolean
  default: True
timeout:
  description: Defines max time to wait data from the endpoint.
  required: false
  type: integer
  default: 10
unit_of_measurement:
  description: Defines the units of measurement of the sensor, if any.
  required: false
  type: string
authentication:
  description:  Type of the HTTP authentication. `basic` or `digest`.
  required: false
  type: string
username:
  description: The username for accessing the REST endpoint.
  required: false
  type: string
password:
  description: The password for accessing the REST endpoint.
  required: false
  type: string
headers:
  description: The headers for the requests.
  required: false
  type: [string, list]
json_attributes:
  description: A list of keys to extract values from a JSON dictionary result and then set as sensor attributes.
  reqired: false
  type: [string, list]
force_update:
  description: Sends update events even if the value hasn't changed. Useful if you want to have meaningful value graphs in history.
  reqired: false
  type: boolean
  default: false
{% endconfiguration %}

<div class='note warning'>
Make sure that the URL exactly matches your endpoint or resource.
</div>

<div class='note'>

Use either `resource` or `resource_template`.

</div>

`curl` can help you identify the variable you want to display in your Home Assistant frontend. The example below shows the JSON response of a device that is running with [aREST](https://arest.io/).

```bash
$ curl -X GET http://192.168.1.31/temperature/
{"temperature": 77, "id": "sensor02", "name": "livingroom", "connected": true}
```

The response is expected to be a dictionary or a list with a dictionary as its 0th element.

## Examples

In this section you find some real-life examples of how to use this sensor.

### External IP address

You can find your external IP address using the service [JSON Test](https://www.jsontest.com/) at their [http://ip.jsontest.com/](http://ip.jsontest.com/) URL.

```yaml
sensor:
  - platform: rest
    resource: http://ip.jsontest.com
    name: External IP
    value_template: '{% raw %}{{ value_json.ip }}{% endraw %}'
```

### Single value from a local Glances instance

The [glances](/integrations/glances) sensor is doing the exact same thing for all exposed values.

```yaml
sensor:
  - platform: rest
    resource: http://IP_ADRRESS:61208/api/2/mem/used
    name: Used mem
    value_template: '{% raw %}{{ value_json.used| multiply(0.000000954) | round(0) }}{% endraw %}'
    unit_of_measurement: MB
```

### Value from another Home Assistant instance

The Home Assistant [API](/developers/rest_api/) exposes the data from your attached sensors. If you are running multiple Home Assistant instances which are not [connected](/developers/architecture/#multiple-connected-instances) you can still get information from them.

If the Home Assistant instance in the resource variable is protected by an API password, you can append `?api_password=YOUR_PASSWORD` to the resource URL to authenticate or use `headers:`.

```yaml
sensor:
  - platform: rest
    resource: http://IP_ADDRESS:8123/api/states/sensor.weather_temperature
    name: Temperature
    value_template: {% raw %}'{{ value_json.state }}'{% endraw %}
    unit_of_measurement: "°C"
```

### Accessing an HTTP authentication protected endpoint

The REST sensor supports HTTP authentication and customized headers.

```yaml
sensor:
  - platform: rest
    resource: http://IP_ADDRESS:5000/sensor
    username: ha1
    password: test1
    authentication: basic
    headers:
      User-Agent: Home Assistant
      Content-Type: application/json
```

The headers will contain all relevant details. This will also give you the ability to access endpoints that are protected by tokens.

```bash
Content-Length: 1024
Host: IP_ADDRESS1:5000
Authorization: Basic aGExOnRlc3Qx
Accept-Encoding: identity
Content-Type: application/json
User-Agent: Home Assistant
```

If you are accessing a resource protected by a `Bearer` token in an `Authorization` header, you can either put the token in the header field of the sensor configuration (not recommended) or store the token in your [`secrets.yaml`](/docs/configuration/secrets/) file. In that case, be sure to include the word `Bearer` in the `secrets` file.

```yaml
sensor:
  - platform: rest
    resource: http://IP_ADDRESS:5000/sensor
    headers:
      Authorization: !secret my_sensor_secret_token
```

Example entry for the `secrets.yaml` file:

```yaml
my_sensor_secret_token: Bearer gh_DHQIXKVf6Pr4H8Yqz8uhApk_mnV6Zje6Pr4H8Yqz8A8nCxz6SBghQdS51
```

### Use GitHub to get the latest release of Home Assistant

This sample is very similar to the [`updater`](/integrations/updater/) integration but the information is received from GitHub.

```yaml
sensor:
  - platform: rest
    resource: https://api.github.com/repos/home-assistant/home-assistant/releases/latest
    username: YOUR_GITHUB_USERNAME
    password: YOUR_GITHUB_ACCESS_TOKEN
    authentication: basic
    value_template: '{% raw %}{{ value_json.tag_name }}{% endraw %}'
    headers:
      Accept: application/vnd.github.v3+json
      Content-Type: application/json
      User-Agent: Home Assistant REST sensor
```

### Fetch multiple JSON values and present them as attributes

[JSON Test](https://www.jsontest.com/) returns the current time, date and milliseconds since epoch from [http://date.jsontest.com/](http://date.jsontest.com/).

{% raw %}
```yaml
sensor:
  - platform: rest
    name: JSON time
    json_attributes:
      - date
      - milliseconds_since_epoch
    resource: http://date.jsontest.com/
    value_template: '{{ value_json.time }}'
  - platform: template
    sensors:
      date:
        friendly_name: 'Date'
        value_template: '{{ states.sensor.json_time.attributes["date"] }}'
      milliseconds:
        friendly_name: 'milliseconds'
        value_template: '{{ states.sensor.json_time.attributes["milliseconds_since_epoch"] }}'
```
{% endraw %}

This sample fetches a weather report from [OpenWeatherMap](https://openweathermap.org/), maps the resulting data into attributes of the RESTful sensor and then creates a set of [template](/integrations/template) sensors that monitor the attributes and present the values in a usable form.

{% raw %}
```yaml
sensor:
  - platform: rest
    name: OWM_report
    json_attributes:
      - main
      - weather
    value_template: '{{ value_json["weather"][0]["description"].title() }}'
    resource: https://api.openweathermap.org/data/2.5/weather?zip=80302,us&APPID=VERYSECRETAPIKEY
  - platform: template
    sensors:
      owm_weather:
        value_template: '{{ state_attr('sensor.owm_report', 'weather')[0]["description"].title() }}'
        entity_picture_template: '{{ "https://openweathermap.org/img/w/"+state_attr('sensor.owm_report', 'weather')[0]["icon"].lower()+".png" }}'
        entity_id: sensor.owm_report
      owm_temp:
        friendly_name: 'Outside temp'
        value_template: '{{ state_attr('sensor.owm_report', 'main')["temp"]-273.15 }}'
        unit_of_measurement: "°C"
        entity_id: sensor.owm_report
      owm_pressure:
        friendly_name: 'Outside pressure'
        value_template: '{{ state_attr('sensor.owm_report', 'main')["pressure"] }}'
        unit_of_measurement: "hP"
        entity_id: sensor.owm_report
      owm_humidity:
        friendly_name: 'Outside humidity'
        value_template: '{{ state_attr('sensor.owm_report', 'main')["humidity"] }}'
        unit_of_measurement: "%"
        entity_id: sensor.owm_report
```
{% endraw %}

This config shows how to extract multiple values from a dictionary with `json_attributes` and `template`. It helps you to avoid flooding the REST service and only ask once the results and separate them in multiple templates referring to it. (No need for a specific state on the REST sensor and it's default state will be the full JSON value which will be longer than the 255 max length. It's why we'll used a static value)

{% raw %}
```json
{
    "bedroom1": {
        "temperature": 15.79,
        "humidity": 55.78,
        "battery": 5.26,
        "timestamp": "2019-02-27T22:21:37Z"
    },
    "bedroom2": {
        "temperature": 18.99,
        "humidity": 49.81,
        "battery": 5.08,
        "timestamp": "2019-02-27T22:23:44Z"
    },
    "bedroom3": {
        "temperature": 18.58,
        "humidity": 47.95,
        "battery": 5.15,
        "timestamp": "2019-02-27T22:21:22Z"
    }
}
```
{% endraw %}

{% raw %}
```yaml
sensor:
  - platform: rest
    name: room_sensors
    resource: http://<address_to_rest_service>
    json_attributes:
      - bedroom1
      - bedroom2
      - bedroom3
    value_template: 'OK'
  - platform: template
    sensors:
      bedroom1_temperature:
        value_template: '{{ states.sensor.room_sensors.attributes["bedroom1"]["temperature"] }}'
        device_class: temperature
        unit_of_measurement: '°C'
      bedroom1_humidity:
        value_template: '{{ states.sensor.room_sensors.attributes["bedroom1"]["humidity"] }}'
        device_class: humidity
        unit_of_measurement: '%'
      bedroom1_battery:
        value_template: '{{ states.sensor.room_sensors.attributes["bedroom1"]["battery"] }}'
        device_class: battery
        unit_of_measurement: 'V'
      bedroom2_temperature:
        value_template: '{{ states.sensor.room_sensors.attributes["bedroom2"]["temperature"] }}'
        device_class: temperature
        unit_of_measurement: '°C'
```
{% endraw %}
