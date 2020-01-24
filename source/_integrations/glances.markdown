---
title: Glances
description: Instructions on how to integrate Glances sensors into Home Assistant.
logo: glances.png
ha_category:
  - System Monitor
ha_iot_class: Local Polling
ha_release: 0.7.3
ha_config_flow: true
ha_codeowners:
  - '@fabaff'
  - '@engrbm87'
---

The `glances` integration allows you to monitor the system information provided by the [Glances](https://github.com/nicolargo/glances) API. This enables one to track remote host and display their stats in Home Assistant.

## Setup

These sensors needs a running instance of `glances` on the host. The minimal supported version of `glances` is 2.3.
To start a Glances RESTful API server on its default port 61208 then test you can use the following command:

```bash
$ sudo glances -w
Glances web server started on http://0.0.0.0:61208/
```

Check if you are able to access the API located at `http://IP_ADRRESS:61208/api/3`. Don't use `-s` as this will start the XMLRPC server on port 61209. Home Assistant only supports the REST API of GLANCES.

The details about your memory usage is provided as a JSON response. If so, you are good to proceed.

```bash
$ curl -X GET http://IP_ADDRESS:61208/api/3/mem/free
{"free": 203943936}
```

If this doesn't work, try changing the `3` to `2`, if you don't have the latest version of Glances installed.

For details about auto-starting `glances`, please refer to [Start Glances through Systemd](https://github.com/nicolargo/glances/wiki/Start-Glances-through-Systemd).  

## Configuration

Set up the integration through **Configuration -> Integrations -> Glances**. To import the configuration from `configuration.yaml` remove any previously configured sensors with platform type `glances` and add the following lines:

```yaml
# Example configuration.yaml entry
glances:
  - host: IP_ADDRESS
```

{% configuration %}
host:
  description: IP address of the host where Glances is running.
  required: false
  type: string
  default: localhost
port:
  description: The port where Glances is listening.
  required: false
  type: integer
  default: 61208
name:
  description: The prefix for the sensors.
  required: false
  type: string
  default: Glances
username:
  description: Your username for the HTTP connection to Glances.
  required: false
  type: string
password:
  description: Your password for the HTTP connection to Glances.
  required: false
  type: string
ssl:
  description: "If `true`, use SSL/TLS to connect to the Glances system."
  required: false
  type: boolean
  default: false
verify_ssl:
  description: Verify the certification of the system.
  required: false
  type: boolean
  default: true
version:
  description: "The version of the Glances API. Supported version: `2` and `3`."
  required: false
  type: integer
  default: 3
{% endconfiguration %}

## Integration Entities

Glances integration will add the following sensors:

- disk_use_percent: The used disk space in percent.
- disk_use: The used disk space.
- disk_free: The free disk space.
- memory_use_percent: The used memory in percent.
- memory_use: The used memory.
- memory_free: The free memory.
- swap_use_percent: The used swap space in percent.
- swap_use: The used swap space.
- swap_free: The free swap space.
- processor_load: The load.
- process_running: The number of running processes.
- process_total: The total number of processes.
- process_thread: The number of threads.
- process_sleeping: The number of sleeping processes.
- cpu_use_percent: The used CPU in percent.
- cpu_temp: The CPU temperature (may not be available on all platforms).
- docker_active: The count of active Docker containers.
- docker_cpu_use: The total CPU usage in percent of Docker containers.
- docker_memory_use: The total memory used by Docker containers.

Not all platforms are able to provide all metrics. For instance `cpu_temp` is requires installing and configuring `lmsensors` in Ubuntu, and may not be available at all in other platforms.
