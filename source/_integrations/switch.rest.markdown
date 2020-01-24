---
title: "RESTful Switch"
description: "Instructions on how to integrate REST switches into Home Assistant."
logo: restful.png
ha_category:
  - Switch
ha_release: 0.7.6
ha_iot_class: Local Polling
---

The `rest` switch platform allows you to control a given endpoint that supports a [RESTful API](https://en.wikipedia.org/wiki/Representational_state_transfer). The switch can get the state via GET and set the state via POST on a given REST resource.

## Configuration

To enable this switch, add the following lines to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
switch:
  - platform: rest
    resource: http://IP_ADDRESS/ENDPOINT
```

{% configuration %}
resource:
  description: The resource or endpoint that contains the value.
  required: true
  type: string
method:
  description: "The method of the request. Supported `post` or `put`."
  required: false
  type: string
  default: post
name:
  description: Name of the REST Switch.
  required: false
  type: string
  default: REST Switch
timeout:
  description: Timeout for the request.
  required: false
  type: integer
  default: 10
body_on:
  description: "The body of the POST request that commands the switch to become enabled. This value can be a [template](/topics/templating/)."
  required: false
  type: string
  default: ON
body_off:
  description: "The body of the POST request that commands the switch to become disabled. This value can also be a [template](/topics/templating/)."
  required: false
  type: string
  default: OFF
is_on_template:
  description: "A [template](/docs/configuration/templating/#processing-incoming-data) that determines the state of the switch from the value returned by the GET request on the resource URL. This template should compute to a boolean (True or False). If the value is valid JSON, it will be available in the template as the variable `value_json`. Default is equivalent to `'{% raw %}{{ value_json == body_on }}{% endraw %}'`. This means that by default, the state of the switch is on if and only if the response to the GET request matches."
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
  description: The headers for the request.
  required: false
  type: [string, list]
verify_ssl:
  description: Verify the SSL certificate of the endpoint.
  required: false
  type: boolean
  default: true
{% endconfiguration %}

<div class='note warning'>
Make sure that the URL matches exactly your endpoint or resource.
</div>

## Example

### Switch with templated value

This example shows a switch that uses a [template](/topics/templating/) to allow Home Assistant to determine its state. In this example, the REST endpoint returns this JSON response with true indicating the switch is on.

```json
{"is_active": "true"}
```

{% raw %}
```yaml
switch:
  - platform: rest
    resource: http://IP_ADDRESS/led_endpoint
    body_on: '{"active": "true"}'
    body_off: '{"active": "false"}'
    is_on_template: '{{ value_json.is_active }}'
    headers:
      Content-Type: application/json
    verify_ssl: true
```
{% endraw %}

`body_on` and `body_off` can also depend on the state of the system. For example, to enable a remote temperature sensor tracking on a radio thermostat, one has to send the current value of the remote temperature sensor. This can be achieved by using the template `{% raw %}'{"rem_temp":{{states('sensor.bedroom_temp')}}}'{% endraw %}`.
