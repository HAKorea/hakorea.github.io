---
title: Netdata
description: Instructions on how to integrate Netdata within Home Assistant.
logo: netdata.png
ha_category:
  - System Monitor
ha_release: 0.35
ha_iot_class: Local Polling
ha_codeowners:
  - '@fabaff'
---

The `netdata` sensor platform allows you to display information collected by [Netdata](https://my-netdata.io/).

## Setup

Getting the details to configure the sensors is a bit tricky as Netdata uses different name for the `element:` value that is required. To get the value for the `data_group:` use Netdata's web interface. `1.` marks the name for the `data_group:`. `2.` are the names for the element to show in Home Assistant. The name that is shown can be different than the name under which the metrics are available.

<p class='img'>
  <img src='{{site_root}}/images/integrations/netdata/details.png' />
</p>

To check if the `element:` name matches the name in the Netdata frontend, use `curl` with the IP address of your Netdata instance, its port and the `data_group`:

```bash
$ curl -X GET "http://[Netdata_Instance]:19999/api/v1/data?chart=[data_group]&points=2&options=jsonwrap"
{
   "api": 1,
   "id": "system.ipv4",
   "name": "system.ipv4",
[...]
   "dimension_names": ["received", "sent"],
   "dimension_ids": ["InOctets", "OutOctets"],
[...]
```

- `dimension_names`: Names shown in the frontend.
- `dimension_ids`: Names to use for `element`.

Alternatively you can browse to the built in Netdata API in your browser `http://[Netdata_Instance]:19999/api/v1/allmetrics?format=json` and search for the `data_group` identified in the Netdata frontend. In the example JSON below the data group is "system.load".

```json
	"system.load": {
		"name":"system.load",
		"context":"system.load",
		"units":"load",
		"last_updated": 1558446920,
		"dimensions": {
			"load1": {
				"name": "load1",
				"value": 0.1250000
			},
			"load5": {
				"name": "load5",
				"value": 0.1290000
			},
			"load15": {
				"name": "load15",
				"value": 0.1430000
			}
		}
	},
```

Once the `data_group` "system.load" and the `element` "load15" have been identified from the JSON it can be configured in your configuration.yaml like the example below. 

## Configuration

Add the following to your `configuration.yaml`.

```yaml
# Example configuration.yaml entry
sensor:
  - platform: netdata
    resources:    
      load:
        data_group: system.load
        element: load15
```

{% configuration %}
host:
  description: The IP address or hostname of your Netdata instance.
  required: false
  type: string
  default: localhost
port:
  description: The port that the Netdata instance is running on.
  required: false
  type: integer
  default: 19999
name:
  description: Name of the monitored Netdata instance.
  required: false
  type: string
  default: Netdata
resources:
  description: List of details to monitor.
  required: true
  type: map
  keys:
    name:
      description: Name to use for the sensor in the frontend.
      required: true
      type: string
      keys:
        data_group:
          description: "Name of the data group to monitor, e.g., `system.cpu`."
          required: true
          type: string
        element:
          description: The element of the group to monitor.
          required: true
          type: string
        icon:
          description: Icon to use for the sensor.
          required: false
          type: icon
          default: "mdi:desktop-classic"
        invert:
          description: Invert the sensor values.
          required: false
          type: boolean
          default: false
{% endconfiguration %}

### Full Example

```yaml
# Example configuration.yaml entry
sensor:
  - platform: netdata
    host: '192.168.1.2'
    port: '19999'
    name: SomeHostName
    resources: 
      system_load:
        data_group: system.load 
        element: load15
      core0_freq:
        data_group: 'cpu.cpufreq'
        element: 'cpu0'
        icon: mdi:chip
```
